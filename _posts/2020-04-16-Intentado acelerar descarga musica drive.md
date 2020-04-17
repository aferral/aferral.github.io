---
published: true
---
## Intentando descargar archivos de musica de drive 

Este último tiempo he estado revisando algunos proyectos pequeños que quería terminar. 
Uno es un programa que permite mantener musica en google drive y genera un sistema de archivos virtual (FUSE), la idea es guardar tu propia mi música en múltiples dispositivos y evitar usar youtube o spotify para escuchar música.

De momento el problema que tenia era que la obtención de archivos es muy ineficiente. Se realiza una descarga del archivo desde google drive, luego se desencripta y luego se envía al cliente. Lo que produce 20 - 30 segundos de retardo entre apretar el botón y la reproducción.

Estoy analizando de qué forma es posible entregar el archivo en trozos para evitar tanto tiempo de retardo. Para esto necesito que tanto el frontend como el backend puedan trozar el archivo.

Investigando por la parte del front el elemento audio HTML5 con solamente especificar la url puede realizar consultas parciales. Por lo tanto no fue necesario cambiar nada de ese lado.

```
<audio controls src="/media/examples/t-rex-roar.mp3"> </audio>
```

Por el lado del backend al inicio tenia un servidor con la librería por defecto de python, donde era necesario especificar todos los header a mano. Acá es necesario primero especificar que los rangos están habilitados, esto se logra con el header "Accept-Ranges', 'bytes'" que le dice al cliente que el servidor puede entregar trozos.

Luego el cliente envía los rangos que necesita mediante el header "Content-Range".
```
Content-Range: bytes 21010-47021/47022
```

La cual entrega start-end en bytes seguido por el largo también en bytes. Las consultas deben también retornar código 206.

Si bien aprendí varias cosas termine usando flask, el tiempo pasó de 20 segundos a 5 segundos. Aun es bastante, el siguiente paso sería que realizara un stream desde la api de google al cliente, ya que actualmente espera que se descargue completamente el archivo antes de enviarlo por trozos.

Por la parte de flask afortunadamente se puede dar un generador y enviará los trozos a medida que se generan. Un extracto del codigo que use
```
from flask import Response

@app.route('/download/<id_to_play>')
def download_parcial(id_to_play):
    return Response(iter_file(id_to_play),mimetype='audio')

```

Por lo tanto necesito transforma mi función que descarga de drive en un iterador, lo cual logre pero ocurrió otro problema. Un extracto del codigo que use.

```

lock = threading.Lock()

def iter_file(file_id):

    request = drive_service.files().get_media(fileId=file_id)
    temp_io = io.BytesIO()
    downloader = MediaIoBaseDownload(temp_io, request,chunksize=600*1000)
    readed=0
    done = False
    while done is False:

        with lock:
            status, done = downloader.next_chunk()

        temp_io.seek(readed)
        data_out = temp_io.read()
        readed+=len(data_out)
        print('Readed {0}'.format(readed))
        
        yield data_out


```

Ocurre que la google drive api no es thread safe lo cual implica que o recreo el servicio en cada llamada o encuentro una forma de coordinarlos. La documentación prácticamente no existe y por otros problemas varios corte por lo sano y lo resolvi con un lock en la descarga. Esto no es para nada ideal ya que evita la concurrencia con otras request, pero ahorra problemas.

Si bien no logré que llegara a ser fluido ahora es un tiempo aguantable.

# Cosas utiles que podrian servir

1. [Funciones de descarga](https://github.com/aferral/FUSE-virtual-music-library/commit/2b29d525cf3514bdde5edc9f4c6b6190aaa7a50e)
2. [Backend flask](https://github.com/aferral/webmplayer/blob/e77d52f6971eedc4d6080247b2d8f69542abcb93/run_server.py)

3. [Documentacion sobre objeto de audio HTML5](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/audio)
4. [Sobre streaming files en flask](https://flask.palletsprojects.com/en/1.1.x/patterns/streaming/)
5. [Sobre request parciales](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/206)
6. [Ejemplo de streaming de video en flask](https://blog.miguelgrinberg.com/post/video-streaming-with-flask)
