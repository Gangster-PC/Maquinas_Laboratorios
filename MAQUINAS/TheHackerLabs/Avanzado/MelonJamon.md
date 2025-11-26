Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240807165317.png)

Veamos lo que hay en el puerto 80:

![](../../../Images/Pasted%20image%2020240807165330.png)

Haremos fuzzing web en busca de dierctorios con Gobuster:

![](../../../Images/Pasted%20image%2020240807165516.png)

Encotramos un directorio llamado /gettingstarted

Accedemos a él:

![](../../../Images/Pasted%20image%2020240807165543.png)

Tenemos un panel para subir archivos

Revisando su código fuente tenemos:

![](../../../Images/Pasted%20image%2020240807165652.png)

Un texto codificado en base 64

Decodificación del texto:

![](../../../Images/Pasted%20image%2020240807165736.png)

Encontramos con yaml que funciona con Python

Buscamos en internet en busca de ayuda y nos encontramos con esta página:

![](../../../Images/Pasted%20image%2020240807170756.png)

![](../../../Images/Pasted%20image%2020240807170825.png)

Nos creamos un archivo con este diseño pero le cambiamos para que nos lance una RevShell:

![](../../../Images/Pasted%20image%2020240807171428.png)

Subo este archivo a la página web y me pongo a la escucha con Netcat:

![](../../../Images/Pasted%20image%2020240807171156.png)

![](../../../Images/Pasted%20image%2020240807171353.png)

Y listo, estamos dentro de la máquina

### Tratamiento de la TTY
```
script /dev/null -c bash 
Ctrl z
stty raw -echo; fg 
reset xterm 
export SHELL=bash
export TERM=xterm
```

![](../../../Images/Pasted%20image%2020240807171453.png)

![](../../../Images/Pasted%20image%2020240807171500.png)

![](../../../Images/Pasted%20image%2020240807171522.png)

## ESCALADA DE PRIVILEGIOS

Ejecutamos sudo -l para ver el binario que podremos usar 

![](../../../Images/Pasted%20image%2020240807171607.png)

Podemos ser el usuario melon con el binario Go

Entonces copiamos una reverse shell llamada Golang y nos ponemos a la escucha para recibir la conexión

![](../../../Images/Pasted%20image%2020240807172721.png)

![](../../../Images/Pasted%20image%2020240807172732.png)

Nos sale un pequeño error pero no pasa nada

![](../../../Images/Pasted%20image%2020240807172748.png)

![](../../../Images/Pasted%20image%2020240807172822.png)

![](../../../Images/Pasted%20image%2020240807172835.png)

Recibimos la conexión y ahora estamos dentro de la máquina

Flag de user:

![](../../../Images/Pasted%20image%2020240807173000.png)

Me levanto un servidor con python en el puerto 80 para poder pasarme el Pspy64 a la máquina y ver sus procesos en segundo plano:

![](../../../Images/Pasted%20image%2020240807173310.png)

![](../../../Images/Pasted%20image%2020240807173337.png)

Y listo, ya lo tengo en la máquina el programa

Le damos permisos de ejecución al programa y lo ejecutamos 

![](../../../Images/Pasted%20image%2020240807173417.png)

![](../../../Images/Pasted%20image%2020240807173950.png)

Evidenciamos que está ejecutando el apt update cada cierto tiempo por lo tanto crearé un archivo en esta ruta /etc/apt/apt.conf.d para que le de permisos a la bash y podamos ser root

![](../../../Images/Pasted%20image%2020240807174424.png)

Y ahora esperamos a que el programa se ejecute

![](../../../Images/Pasted%20image%2020240807174528.png)

Como ya nos aparece la S en los permisos es que ya está el cambio hecho

Ejecutamos bash -p

![](../../../Images/Pasted%20image%2020240807174607.png)

Y listo, ya somos ROOT

Flag de root:

![](../../../Images/Pasted%20image%2020240807174629.png)

