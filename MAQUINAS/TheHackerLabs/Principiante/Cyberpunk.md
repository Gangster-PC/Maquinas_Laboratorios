Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240802192828.png)

Veamos lo que hay en el puerto FTP, ya que está abierto y podemos entrar con el usuario anonymous

![](../../../Images/Pasted%20image%2020240802192908.png)

Tenemos un secret.txt así que lo descargamos

Leemos lo que contiene:

![](../../../Images/Pasted%20image%2020240802193004.png)

Nada relevante

Como vimos que podemos acceder al puerto FTP crearé un archivo .php con una RevShell
y lo subiré

![](../../../Images/Pasted%20image%2020240802193211.png)

Ahora accederemos a este archivo shell.php desde la pagina web y nos pondremos a la escucha con Netcat para recibir la conexión

![](../../../Images/Pasted%20image%2020240802193329.png)

![](../../../Images/Pasted%20image%2020240802193344.png)

Y recibimos la conexión

### Tratamiento de la TTY
```
script /dev/null -c bash 
Ctrl z
stty raw -echo; fg 
reset xterm 
export SHELL=bash
export TERM=xterm
```

![](../../../Images/Pasted%20image%2020240802193543.png)

![](../../../Images/Pasted%20image%2020240802193604.png)

## ESCALADA DE PRIVILEGIOS

En el directorio /opt encontramos un archivo .txt con algo escrito en lenguaje Brainfuck

![](../../../Images/Pasted%20image%2020240802193803.png)

Lo desencodeamos:

![](../../../Images/Pasted%20image%2020240802193916.png)

Y tenemos una posible contraseña cyberpunk2077

Vemos que hay un usuario llamado arasaka

![](../../../Images/Pasted%20image%2020240802194000.png)

Intentamos ser el usuario arasaka con la contraseña anteriormente descifrada:

![](../../../Images/Pasted%20image%2020240802194052.png)

Y listo, ahora somos arasaka

La flag de user:

![](../../../Images/Pasted%20image%2020240802194123.png)

Ejecutamos sudo -l

![](../../../Images/Pasted%20image%2020240802194154.png)

Y evidenciamos que para poder ser el usuario root podemos ejecutar el archivo randombase64.py con python

Veamos lo que contiene el archivo randombase64.py:

![](../../../Images/Pasted%20image%2020240802194304.png)

Lo que hace es pedir que ingrese un texto y él lo transforma a base 64

![](../../../Images/Pasted%20image%2020240802194409.png)

Para ser root haremos lo siguiente con este archivo:

Vemos que el archivo está importando una librería llamada base 64, así que nos crearemos un archivo .py con el mismo nombre pero con un código en python para recibir una /bin/bash:

![](../../../Images/Pasted%20image%2020240802195004.png)

Ejecutamos el archivo  randombase64.py con python:

![](../../../Images/Pasted%20image%2020240802195059.png)

Y listo, ya somos ROOT

La flag de root:

![](../../../Images/Pasted%20image%2020240802195140.png)

