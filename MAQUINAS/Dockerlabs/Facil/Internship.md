Escaneo de puertos con Nmap
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020251030185132.png)

Observo lo que corre por el puerto 80:

![](../../../Images/Pasted%20image%2020251030190442.png)

Tengo esa página web

Daré click en el botón "Iniciar Sesión"

![](../../../Images/Pasted%20image%2020251030190522.png)

Y efectivamente tengo un panel de inicio de sesión, pero no tengo posibles usuarios ni contraseñas

Por ende por el momento dejaré ahí y realizaré un ataque de fuzzing web en busca de directorios con la herramienta Gobuster
```
gobuster dir -u http://gatekeeperhr.com/ -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -x html,php,txt
```

![](../../../Images/Pasted%20image%2020251030190847.png)

Me llama la atención el directorio /spam

Accedo a él:

![](../../../Images/Pasted%20image%2020251030191036.png)

Se ve literalmente nada 

Revisando el código fuente de esta página me encuentro con:

![](../../../Images/Pasted%20image%2020251030191114.png)

En la línea 13 tengo un código aparentemente cifrado

Lo pasaré por Cyberchef a ver que resultado me da:

![](../../../Images/Pasted%20image%2020251030191316.png)

Estaba codificado en ROT13, decodificado este mensaje me da una contraseña "purpl3", ahora me falta encontrar el usuario y a qué o a donde pertenecerían estas credenciales

Con estas posibles contraseñas procederé a hacer un ataque de fuerza bruta con Hydra hacia el puerto 22 SSH para ver si pertenece esa contraseña a algún posible usuario
```
hydra -L /usr/share/wordlists/seclists/Usernames/xato-net-10-million-usernames-dup.txt -p purpl3 ssh://172.17.0.2 -t 64 
```

![](../../../Images/Pasted%20image%2020251030191742.png)

Efectivamente la contraseña "purpl3" pertenece al usuario "pedro"

Intrusión por el puerto SSH con estas credenciales:

![](../../../Images/Pasted%20image%2020251030191945.png)

Y listo, ya estoy dentro de la máquina

## ESCALADA DE PRIVILEGIOS

Revisando el directorio /opt, me encuentro un archivo .sh que ejecuta una acción me imagino que ejecuta este codigo cada X tiempo, pero como pertenece al usuario "valentina" no puedo cambiarlo

![](../../../Images/Pasted%20image%2020251030192316.png)

Intentaré editarlo para crear un código que me permita lanzarme una Reverse Shell a mi máquina y así mismo escalar de usuario:

![](../../../Images/Pasted%20image%2020251030192744.png)

Me pongo  a la escucha con Netcat:

![](../../../Images/Pasted%20image%2020251030192906.png)

Después de esperar unos segundos me llega la conexión y ya entré nuevamente a la máquina pero ahora como el usuario "valentina" 

Tratamiento de la TTY:
```
script /dev/null -c bash 
Ctrl z
stty raw -echo; fg 
reset xterm 
export SHELL=bash
export TERM=xterm
```

![](../../../Images/Pasted%20image%2020251030193149.png)

![](../../../Images/Pasted%20image%2020251030193221.png)

En la carpeta de /home/valentina tengo una imagen .jpeg:

![](../../../Images/Pasted%20image%2020251030193520.png)

La moveré a la carpeta /tmp para poder inspeccionarla mejor:

![](../../../Images/Pasted%20image%2020251030193758.png)

Listo, ahora con la herramienta Scp procederé a pasarme la imagen a mi kali:
```
scp pedro@172.17.0.2:/tmp/profile_picture.jpeg .
```

![](../../../Images/Pasted%20image%2020251030194036.png)

Le haré esteganografía con Steghide para ver si tiene algun archivo oculto
```
steghide extract -sf profile_picture.jpeg
```

![](../../../Images/Pasted%20image%2020251030194310.png)

(De passphrase no le paso nada, le doy Enter)

Me extrajo un .txt lo leeré:

![](../../../Images/Pasted%20image%2020251030194407.png)

Al parecer es una contraseña, veré si es del usuario actual (valentina), con el comando sudo -l:

![](../../../Images/Pasted%20image%2020251030194536.png)

Efectivamente si es de este usuario esa contraseña así que procederé a ver en GTFObins como puedo escalar de usuario usando el binario Vim 

![](../../../Images/Pasted%20image%2020251030194724.png)

Intentaré usando el primer comando (A):

![](../../../Images/Pasted%20image%2020251030194943.png)

Y listo, ya soy ROOT 
