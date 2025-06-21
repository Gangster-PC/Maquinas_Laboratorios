Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240822120626.png)

Veamos lo que hay en el puerto 80:

![](../../../Images/Pasted%20image%2020240822120740.png)

Usaremos Gobuster para encontrar posibles directorios:

![](../../../Images/Pasted%20image%2020240822125139.png)

Y nos encontramos el directorio /agua.php, accedamos a él:

![](../../../Images/Pasted%20image%2020240822125241.png)

Nos devuelve la misma página, entonces podemos probar quizas un LFI (Local File Inclusion), usaremos la herramienta de Wfuzz para ver el parámetro q va para explotar esta vulnerabilidad:

![](../../../Images/Pasted%20image%2020240822130822.png)

Accedemos a la carpeta /etc/passwd para leer los posibles usuarios:

![](../../../Images/Pasted%20image%2020240822130914.png)

Encontramos los usuarios sincebolla y concebolla 

Buscando en el directorio /opt encontré una id_rsa:

![](../../../Images/Pasted%20image%2020240822131407.png)

Me la paso a mi pc, le extraigo el hash con Ssh2john y lo crackeo con John:

![](../../../Images/Pasted%20image%2020240822131911.png)

La contraseña es honda1

Intrusión por el puerto 22 SSH con el usuario sincebolla  y la id_rsa:

![](../../../Images/Pasted%20image%2020240822132009.png)

Y listo, estamos dentro de la máquina

Flag de user:

![](../../../Images/Pasted%20image%2020240822132037.png)

## ESCALADA DE PRIVILEGIOS

Ejecutamos sudo -l para ver el binario que podremos usar 

![](../../../Images/Pasted%20image%2020240822132053.png)

Podemos ser el usuario concebolla con el binario Smokeping

Lo ejecutamos:

![](../../../Images/Pasted%20image%2020240822132805.png)

Y nos lanzamos una /bin/bash:

![](../../../Images/Pasted%20image%2020240822132838.png)

![](../../../Images/Pasted%20image%2020240822132850.png)

Ahora soy el usuario concebolla

Miro los grupos a los que pertenezco:

![](../../../Images/Pasted%20image%2020240822133339.png)

Y me llama la atención el grupo LXD

Buscamos en searchsploit algun posible exploit y lo descargamos:

![](../../../Images/Pasted%20image%2020240822133455.png)

Lo leemos:

![](../../../Images/Pasted%20image%2020240814110908.png)

Y seguimos los pasos

1. Nos descargamos el build-alpine

![](../../../Images/Pasted%20image%2020240822133718.png)

2. Extraemos la imagen de él:

![](../../../Images/Pasted%20image%2020240822133739.png)

![](../../../Images/Pasted%20image%2020240822133849.png)

Nos generó un archivo .tar.gz
3. Nos levantamos un servidor en python y nos pasamos el .sh y el .tar.gz a la máquina:

![](../../../Images/Pasted%20image%2020240822133946.png)

Ya tenemos ambos archivos en la máquina
4. Ejecutamos el .tar.gz con el .sh:

![](../../../Images/Pasted%20image%2020240822134038.png)

Y somos "root" pero en el contenedor LXD, no como tal en la máquina

Entraremos a /root/bin y le daremos permisos a la bash:

![](../../../Images/Pasted%20image%2020240822134145.png)

Nos salimos del contenedor, ejecutamos bash -p:

![](../../../Images/Pasted%20image%2020240822134233.png)

Y listo, ahora si somos ROOT en la máquina

Flag de root:

![](../../../Images/Pasted%20image%2020240822134316.png)

