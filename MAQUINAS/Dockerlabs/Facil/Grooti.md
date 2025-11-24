Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020251119205813.png)

Observaré qué está corriendo en el puerto 80 HTTP:

![](../../../Images/Pasted%20image%2020251119205941.png)

Observando el código fuente de esta página me encuentro con un comentario:

![](../../../Images/Pasted%20image%2020251119210207.png)

En la línea 47 me dan un posible usuario llamado "rocket", probaré hacer fuerza bruta hacia el puerto 3306 Mysql para ver si es de allí y si es así, encontrar su contraseña:
```
hydra -l rocket -P /usr/share/wordlists/rockyou.txt mysql://172.17.0.2 -t 64 
```

![](../../../Images/Pasted%20image%2020251119210323.png)

Efectivamente el usuario pertenece a allí y su contraseña es "password1"

Entraré a la base de datos con estas credenciales:
```
mysql -h 172.17.0.2 -u rocket -ppassword1 --skip-ssl
```

Miro las bases de datos existentes:

![](../../../Images/Pasted%20image%2020251119210741.png)

Uso la base de datos "files_secret" y busco sus tablas:

![](../../../Images/Pasted%20image%2020251119211218.png)

Y selecciono todo lo que haya en esta tabla:

![](../../../Images/Pasted%20image%2020251119211243.png)

Me llama la atención el id #4, es como una dirección a un directorio, accederé a él:

![](../../../Images/Pasted%20image%2020251119211320.png)

Me encuentro con un panel algo extraño e interesante

Escribo cualquier cosa y doy click en el botón verde:

![](../../../Images/Pasted%20image%2020251119211453.png)

Lo que hace la página es devolverme un archivo .txt con lo que yo escribí

Como me dice la propia página, puedo escribir un número entre 1 y 100, comprobaré todos los números a ver si con alguno encuentro algo diferente, para ello abriré c para usar una herramienta que me puede automatizar esta búsqueda de números:

![](../../../Images/Pasted%20image%2020251120172252.png)

Activo la interceptación y activo también FoxyProxy para que se encuentren:

![](../../../Images/Pasted%20image%2020251120172151.png)

Escribo cualquier cosa en los campos, doy click en el botón verde y automáticamente se me abre Burpsuite con la petición interceptada, procedo a seleccionarla toda y mandarla al intruder:

![](../../../Images/Pasted%20image%2020251120172536.png)

En el Intruder, selecciono el número (en mi caso el 13) y doy click en el botón 13, esto con el fin de que el Intruder sepa en donde tiene que hacer la automatización de los 100 números

![](../../../Images/Pasted%20image%2020251120172611.png)

Y a mano derecha, en el payload lo configuro así:

![](../../../Images/Pasted%20image%2020251120172914.png)

Y doy click en el botón naranja "Start attack":

![](../../../Images/Pasted%20image%2020251120173804.png)

Observo, que el número 16, frente a los demás tiene una respuesta diferente un número de respuesta mas elevado, entonces procederé a escribir 16 en la página a ver qué me descarga:

![](../../../Images/Pasted%20image%2020251120173958.png)

Efectivamente, ahora ya no me descargó un .txt más sino un .zip, al momento de tratar de descomprimirlo me pide una contraseña la cuál no tengo, así que procedo a extraerle el hash con Zip2john y seguidamente procedo a crackearlo:
```
zip2john password16.zip > hash  
```

```
john --wordlist=/usr/share/wordlists/rockyou.txt hashjohn --wordlist=/usr/share/wordlists/rockyou.txt hash
```

![](../../../Images/Pasted%20image%2020251120174531.png)

Descomprimo el .zip con la contraseña "password1":

![](../../../Images/Pasted%20image%2020251120174704.png)

Me extrae un .txt con muchas posibles contraseñas, usaré Hydra poniendo por ejemplo de usuario "grooti" para ver  cuál de estas contraseñas es la correcta, apuntando hacia el puerto 22 SSH:

![](../../../Images/Pasted%20image%2020251120174840.png)

Intrusión por el puerto 22 SSH con estas credenciales:

![](../../../Images/Pasted%20image%2020251120175040.png)

Y listo, estoy dentro de la máquina

## ESCALADA DE PRIVILEGIOS

Revisando el directorio /opt, me encuentro con un "cleanup.sh" que hace una tarea que consta de ejecutar un .sh del directorio /tmp

![](../../../Images/Pasted%20image%2020251120175415.png)

Voy a ver si me deja editar ese archivo malicious.sh:

![](../../../Images/Pasted%20image%2020251124141948.png)

Efectivamente, entonces procederé a darle permisos a la SUID 
```
chmod u+s /bin/bash
```

![](../../../Images/Pasted%20image%2020251124142058.png)

Espero un minuto, escribo el comando "bash -p":

![](../../../Images/Pasted%20image%2020251124142259.png)

Y listo, ya soy ROOT 
