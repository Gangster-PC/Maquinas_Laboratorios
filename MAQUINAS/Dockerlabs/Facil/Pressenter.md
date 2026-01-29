Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020240829095038.png)

Veo lo que corre en el puerto 80:

![](../../../Images/Pasted%20image%2020240829095407.png)

Haré fuzzing web usando Gobuster para encontrar directorios:

![](../../../Images/Pasted%20image%2020240829095501.png)
Inmediatamente me doy de cuenta que estoy ante un Wordpress

Usaré la herramienta de Wpscan para ver posibles usuarios:

![](../../../Images/Pasted%20image%2020240829095623.png)

Tengo 2 usuarios "pressi" y "hacker"

Haré fuerza bruta a el usuario pressi con esta misma herramienta para encontrar su contraseña:

![](../../../Images/Pasted%20image%2020240829095842.png)

Y la contraseña del usuario "pressi" es "dumbass", coloco estas crendenciales en el panel de Login web del Wordpress

![](../../../Images/Pasted%20image%2020240829095808.png)

Y accedí:

![](../../../Images/Pasted%20image%2020240829095904.png)

Ahora una vez dentro de este Wordpress procederé a hacer la intrusión como tal a la máquina.

Para ello, voy a la ruta Plugins-Añadir nuevo plugin, e instalo el Bit File Manager

![](../../../Images/Pasted%20image%2020240829101111.png)

Ahora voy a la pestaña Bit File Manager

![](../../../Images/Pasted%20image%2020240829101217.png)
Luego a wp-content

![](../../../Images/Pasted%20image%2020240829101241.png)

Ahora Uploads

![](../../../Images/Pasted%20image%2020240829101258.png)

Ahora me crearé una RevShell con Msfvenom en un archivo .php

![](../../../Images/Pasted%20image%2020240829101318.png)

Y lo subo a la pagina web en la carpeta Uploads por medio del botón azul Upload

![](../../../Images/Pasted%20image%2020240829101353.png)

Voy a la dirección donde subí este archivo:

![](../../../Images/Pasted%20image%2020240829101429.png)

Me pongo a la escucha con netcat y doy click en mi archivo

![](../../../Images/Pasted%20image%2020240829101448.png)

Recibo la conexión y ahora estoy dentro de la máquina

### Tratamiento de la TTY
```
script /dev/null -c bash 
Ctrl z
stty raw -echo; fg 
reset xterm 
export SHELL=bash
export TERM=xterm
```

![](../../../Images/Pasted%20image%2020240829101653.png)

![](../../../Images/Pasted%20image%2020240829101720.png)

## ESCALADA DE PRIVILEGIOS

Luego de buscar un rato, se me ocurre ver la base de datos Mysql, pero como tiene una contraseña, puedo intentar leer el archivo "wp-config.php" donde está instalado el wordpress y ver si en alguna linea  está el usuario y contraseña:

![](../../../Images/Pasted%20image%2020240901152517.png)

Accedo a la base de datos con estas credenciales:

![](../../../Images/Pasted%20image%2020240901153143.png)

Busco bases de datos:

![](../../../Images/Pasted%20image%2020240901153201.png)

Entro a la base de datos Wordpress

![](../../../Images/Pasted%20image%2020240901153328.png)

Ahora veo sus tablas:

![](../../../Images/Pasted%20image%2020240901153340.png)

Y veo todo lo q contiene la tabla wp_usernames:

![](../../../Images/Pasted%20image%2020240901153506.png)

Y finalmente conseguí una contraseña con su respectivo usuario que es "enter"

Escalo a él con esta credencial de contraseña:

![](../../../Images/Pasted%20image%2020240901153556.png)

Flag de user:

![](../../../Images/Pasted%20image%2020240901153612.png)

Ahora con esta misma contraseña intentaré escalar a root:

![](../../../Images/Pasted%20image%2020240901153648.png)

Y listo, ya soy ROOT

Flag de root:

![](../../../Images/Pasted%20image%2020240901153715.png)
