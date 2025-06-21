Escaneo de puertos con Nmap:

![](../../../Images/Pasted%20image%2020240906192847.png)

Observo lo que corre en el único puerto que tiene esta máquina, el puerto 80:

![](../../../Images/Pasted%20image%2020240906192932.png)

Tengo una supuesta tienda de "Apple"

Haré fuzzing web con Gobuster en busqueda de directorios:

![](../../../Images/Pasted%20image%2020240906193653.png)

Tengo el directorio /register.php, accedo a el:

![](../../../Images/Pasted%20image%2020240906193740.png)

![](../../../Images/Pasted%20image%2020240906193746.png)

Y me registré con mario:admin123

Ahora accederé con estas nuevas credenciales al /login.php:

![](../../../Images/Pasted%20image%2020240906193840.png)

![](../../../Images/Pasted%20image%2020240906193854.png)

Y estoy dentro de la página web ahora

Ahora pruebo buscar un producto, como por ejemplo Iphone:

![](../../../Images/Pasted%20image%2020240912093811.png)

Y efectivamente la página me muestra el producto que busqué

Ahora le pasaré la típica inyección SQL en la url despues del "=" para ver si es vulnerable ```or 1=1-- -```:

![](../../../Images/Pasted%20image%2020240912093913.png)

Y si, efectivamente es vulnerable a SQL injection porque me muestra todos los productos de la pagína sin darme un error

Ahora capturaré esta peticion con Burpsuite:

![](../../../Images/Pasted%20image%2020240912094033.png)

Copiaré y pegaré toda la petición en un archivo creado con nano y le pondré de nombre Injection:

![](../../../Images/Pasted%20image%2020240912094132.png)

Ahora si realizaré la explotación de la vulnerabilidad con SQLmap:
```
sqlmap -r injection --batch --dump
```

![](../../../Images/Pasted%20image%2020240912094215.png)

![](../../../Images/Pasted%20image%2020240912094227.png)

Y obtengo 4 hashes, copio y pego la hash de admin y la crackeo con John:

![](../../../Images/Pasted%20image%2020240912094336.png)

Y la contraseña para el usuario admin es 0844575632

Accedo con estas credenciales al panel de login web:

![](../../../Images/Pasted%20image%2020240912094408.png)

![](../../../Images/Pasted%20image%2020240912094418.png)

Estoy dentro

Ahora doy click en el botón rojo y despues a configuración y obtengo un panel de subida de archivos:

![](../../../Images/Pasted%20image%2020240912094512.png)

Intentaré subir una Reverse Shell en formato .phtml:

![](../../../Images/Pasted%20image%2020240912094622.png)

![](../../../Images/Pasted%20image%2020240912094629.png)

Se subió satisfactoriamente

Ahora, recordando en la busqueda de directorios con Gobuster, yo tengo un /uploads accederé a el:

![](../../../Images/Pasted%20image%2020240912094714.png)

Y acá esta mi archivo

Ahora me pongo a la escucha con Netcat y doy click en mi archivo:

![](../../../Images/Pasted%20image%2020240912094749.png)

Recibo la conexión y ahora estoy dentro de la máquina

Tratamiento de la TTY:

![](../../../Images/Pasted%20image%2020240912094842.png)

![](../../../Images/Pasted%20image%2020240912094900.png)

## ESCALADA DE PRIVILEGIOS

Observo los usuarios que contiene esta máquina:

![](../../../Images/Pasted%20image%2020240912094956.png)

Tiene el usuario Luisillo

Por ende me pasaré una herramienta de mi amigo Malfer para hacer fuerza bruta a este usuario luisillo usando el diccionario rockyou:

![](../../../Images/Pasted%20image%2020240912095317.png)

Le doy permisos de ejecución a la herramienta:

![](../../../Images/Pasted%20image%2020240912095340.png)

Y ejecuto la herramienta:

![](../../../Images/Pasted%20image%2020240912095833.png)

![](../../../Images/Pasted%20image%2020240912101107.png)

Y la contraseña del usuario luisillo_o es 19831983

Escalo a este usuario:

![](../../../Images/Pasted%20image%2020240912101151.png)

Ahora reviso los grupos a los que pertenezco siendo este usuario:

![](../../../Images/Pasted%20image%2020240912101230.png)

Pertenezco al grupo shadow

Por ende veré la contraseña de root en la dirección /etc/shadow:

![](../../../Images/Pasted%20image%2020240912101316.png)

Y obtengo la hash de root

Me la pasaré a mi kali y la crackearé con John:
```
john --format=crypt --wordlist=/usr/share/wordlists/rockyou.txt hash
```

![](../../../Images/Pasted%20image%2020240912102054.png)

Y la contraseña de root es rainbow2

Escalo a root:

![](../../../Images/Pasted%20image%2020240912102129.png)

Y listo, ya soy ROOT
