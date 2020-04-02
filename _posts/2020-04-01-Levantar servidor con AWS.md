---
published: true
---
## Levantar servicio mediante redireccionar trafico con AWS

Hace poco cambio la configuracion de mi router lo cual ahora me impide abrir los puertos para levantar algun servicio o servidor. Investigando llegue a una solucion no tan completa que puede ser util para levantar aplicaciones, alojar paginas web, levantar servidores de juegos o lo que sea necesario.

La solucion consiste en tener el servidor en una maquina local, levantar una maquina virtual en EC2 de aws (en realidad cualquier maquina virtual sobre la cual se tenga un ip publica, acceso a abrir puertos sirve) y rediriguir un puerto local al puerto en la maquina remota mediane ssh.

Una ventaja de este metodo es que como la maquina remota solo redirigue el trafico no requiere mayor capacidad lo cual ahorra en gastos de la maquina virtual, en este caso yo use el free tier de AWS (t2 micro).

# Levantar instancia EC2

Respecto a levantar la instancia no hay mucho que decir, solo es importante que se tenga acceso a ssh y configurar la red mediante un grupo de seguridad.

En el grupo de seguridad se debe agregar el puerto publico donde el servicio se mantendra y que rango de ips puede accederlo.

![ejemplo security group]({{site.baseurl}}/_posts/screenshot.png)

En la imagen se muestra un servicio en el puerto 8080 que podra ser accedido por cualquier ip (0.0.0.0/0 es un rango de todas las ips posibles).

Despues de crear el grupo de seguridad asignarlo a la instancia. Recomiendo crear una template de la instancia con el grupo de seguridad asi se puede reutilizar en un futuro.


# Configurar ssh en instancia


Para habilitar la redireccion por ssh es necesario cambiar la configuracion del servicio ssh.

	ssh -i key_here ec2-user@ec2_url_here -t 'sudo vim "/etc/ssh/sshd_config" && sudo reboot'


Este comando entra a la instancia edita sshd_config, en el cual es necesario cambiar

```bash
#GatewayPorts no
por 
GatewayPorts yes
```

  
Al salir se reiniciara la maquina automaticamente.

# Refiriguir trafico mediante port forwarding

Si tenemos una aplicacion web ejecutandose en localhost:8080 y quiero tener esta aplicacion en el puerto 9000 en la instancia de aws se ejecuta el siguiente comando.

```
ssh -i key_here  -N -R 9000:localhost:8000 -o ServerAliveInterval=10 -o ExitOnForwardFailure=yes  ec2-user@ec2_url_here 
```
Este comando toma localhost:8000 y lo conecta con ec2_url_here:9000 de forma que toda coneccion que entre por este llega al localhost y viceversa. El '-N' es para evitar abrir una consola. El ServerAliveInterval permite que la coneccion no se corte en periodos de inactividad. El ExitOnForwardFailure se encarga de cerrar la conexion en caso de error de forma correcta.

Para probar ir a http://(ec2_url_here):9000 suponiendo que sea una aplicacion web.
