Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240813104748.png)

Veamos lo que hay en el puerto 80:

![](../../../Images/Pasted%20image%2020240813104849.png)

En toda la página web nos recalca el nombre de NAMARI, miraremos si es que es un directorio o algo asi:

![](../../../Images/Pasted%20image%2020240813105333.png)

Efectivamente es un directorio de la página

Subiremos una RevShell php:

![](../../../Images/Pasted%20image%2020240814104435.png)

![](../../../Images/Pasted%20image%2020240814103140.png)

Pero una vez subido no sabemos exactamente donde se subió para poder ejecutarlo, entonces  descargaremos el código fuente de la web, la cual nos la da en base64.
```
php://filter/convert.base64-encode/resource=index.php
```

![](../../../Images/Pasted%20image%2020240814103644.png)

![](../../../Images/Pasted%20image%2020240814103656.png)

Pasamos este código base 64 a nuestro kali y no decodificamos:

![](../../../Images/Pasted%20image%2020240814104012.png)

Y nos fijamos exactamente en esta parte:

![](../../../Images/Pasted%20image%2020240814104055.png)

La web encondea el nombre de nuestro archivo en Rot13

Así que buscaremos como quedaría el nombre de nuestro archivo encodedado a Rot13:

![](../../../Images/Pasted%20image%2020240814104510.png)

Accedemos a él:

![](../../../Images/Pasted%20image%2020240814104813.png)

Y nos ponemos a la escucha con NetCat:

![](../../../Images/Pasted%20image%2020240814104833.png)

Y recibimos la conexión, estamos dentro de la máquina

### Tratamiento de la TTY
```
script /dev/null -c bash 
Ctrl z
stty raw -echo; fg 
reset xterm 
export SHELL=bash
export TERM=xterm
```

![](../../../Images/Pasted%20image%2020240814105025.png)

![](../../../Images/Pasted%20image%2020240814105053.png)

## ESCALADA DE PRIVILEGIOS

Encontramos en la el directorio /opt una carpeta oculta que contiene un backup.zip:

![](../../../Images/Pasted%20image%2020240814105245.png)

Levantamos un servidor con python por el puerto 5000 y nos lo pasamos a nuestro kali

![](../../../Images/Pasted%20image%2020240814105613.png)

![](../../../Images/Pasted%20image%2020240814105655.png)

Le extraemos la hash con Zip2john y la crackeamos con John:

![](../../../Images/Pasted%20image%2020240814105805.png)

Y la contraseña para descomprimir el .zip es batman

Lo descomprimimos:

![](../../../Images/Pasted%20image%2020240814105839.png)

Nos deja un .txt

Lo leemos:

![](../../../Images/Pasted%20image%2020240814105913.png)

Es la contraseña de el usuario rodgar, escalamos a este usuario con su contraseña:

![](../../../Images/Pasted%20image%2020240814110120.png)

Ahora somos el usuario rodgar

Flag de user:

![](../../../Images/Pasted%20image%2020240814110148.png)

Ejecutamos id, para ver a los grupos que pertenecemos siendo rodgar:

![](../../../Images/Pasted%20image%2020240814110729.png)

Nos llama la atención el LXD

Buscamos en searchsploit algun posible exploit y lo descargamos:

![](../../../Images/Pasted%20image%2020240814110826.png)

Lo leemos:

![](../../../Images/Pasted%20image%2020240814110908.png)

Y seguimos los pasos

1. Nos descargamos el build-alpine

![](../../../Images/Pasted%20image%2020240814152506.png)

2. Extraemos la imagen de él:

![](../../../Images/Pasted%20image%2020240814152533.png)

![](../../../Images/Pasted%20image%2020240814152559.png)

Nos generó un archivo .tar.gz
3. Nos levantamos un servidor en python y nos pasamos el .sh y el .tar.gz a la máquina:

![](../../../Images/Pasted%20image%2020240814152708.png)

![](../../../Images/Pasted%20image%2020240814152840.png)

Ya tenemos ambos archivos en la máquina
4. Ejecutamos el .tar.gz con el .sh:

![](../../../Images/Pasted%20image%2020240814153225.png)

Y somos "root" pero en el contenedor LXD, no como tal en la máquina

Entraremos a /mnt/root/bin y le daremos permisos a la bash

![](../../../Images/Pasted%20image%2020240814153849.png)

Intrusión por el puerto 22 SSH con usuario rodgar y la contraseña extraída anteriormente de un backup.zip que es 6rK5£6iqF;o|8dmla859/_:

![](../../../Images/Pasted%20image%2020240814153707.png)

![](../../../Images/Pasted%20image%2020240814153715.png)

Y ejecutaremos bash -p:

![](../../../Images/Pasted%20image%2020240814153922.png)

Y listo, ahora si somos ROOT directamente de la máquina

Flag de root:

![](../../../Images/Pasted%20image%2020240814153342.png)

