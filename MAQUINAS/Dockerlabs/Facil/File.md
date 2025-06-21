Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020250114175649.png)

Reviso el puerto 80

![](../../../Images/Pasted%20image%2020250114180150.png)

Y tengo una plantilla de Apache común y corriente

Haré fuzzing web con Gobuster para encontrar posibles directorios:
```
gobuster dir -u [http://172.17.0.2](http://172.17.0.2) -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -x html,php,txt
```

![](../../../Images/Pasted%20image%2020250114180606.png)

Me encuentro con 2 directorios importantes, uno llamado /file_upload.php y el otro /uploads

Accedo al /file_upload.php:

![](../../../Images/Pasted%20image%2020250114180640.png)

Tengo una subida de archivos

Procederé a subir una Reverse Shell creada con Msfvenom con extensión .phar:

![](../../../Images/Pasted%20image%2020250114181308.png)

![](../../../Images/Pasted%20image%2020250114181320.png)

![](../../../Images/Pasted%20image%2020250114181327.png)

Ahora, recordando tengo el directorio /uploads:

![](../../../Images/Pasted%20image%2020250114181349.png)

Y aquí está mi archivo

Me pongo a la escucha con Netcat y doy click en mi archivo:

![](../../../Images/Pasted%20image%2020250114181428.png)

Y listo, recibo la conexión y ahora estoy dentro de la máquina

## ESCALADA DE PRIVILEGIOS

Tengo 4 usuarios:

![](../../../Images/Pasted%20image%2020250114182454.png)

Pero no tengo ninguna posible contraseña

Así que usaré la herramienta de mi amigo Malfer para hacer fuerza bruta hacia estos usuarios usando también el diccionario rockyou.txt:

![](../../../Images/Pasted%20image%2020250114182818.png)

Le doy permiso de ejecución a la herramienta:

![](../../../Images/Pasted%20image%2020250114182915.png)

Y la ejecuto:

![](../../../Images/Pasted%20image%2020250114183028.png)

![](../../../Images/Pasted%20image%2020250114183035.png)

Y la contraseña de fernando es chocolate

Escalo al usuario fernando con su contraseña:

![](../../../Images/Pasted%20image%2020250114183457.png)

Reviso en su directorio:

![](../../../Images/Pasted%20image%2020250114183513.png)

Y me encuentro con una imagen, me la paso a mi Kali levantando un servidor con Python:

![](../../../Images/Pasted%20image%2020250114183644.png)

![](../../../Images/Pasted%20image%2020250114183700.png)

Ahora le realizo esteganografía a la imagen con ayuda de la herramienta Stegcracker:

![](../../../Images/Pasted%20image%2020250114183856.png)

Y encuentro una contraseña "secret" y junto a ella un archivo llamado como la imagen pero adicional la extensión .out, la leo y tengo una hash:

![](../../../Images/Pasted%20image%2020250114184152.png)

La paso por la página web Crackstation y descrackeada es password123:

![](../../../Images/Pasted%20image%2020250114184235.png)

La contraseña de un posible usuario

Intento escalar a mario con esta contraseña:

![](../../../Images/Pasted%20image%2020250114184310.png)

Y efectivamente la contraseña es de mario, ahora soy Mario

Ejecuto sudo -l para ver el binario que puedo usar

![](../../../Images/Pasted%20image%2020250114184427.png)

Puedo ser el usuario julen con el binario Awl

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020250114184516.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020250114184552.png)

Y ahora soy julen

Ejecuto sudo -l para ver el binario que puedo usar

![](../../../Images/Pasted%20image%2020250114184615.png)

Puedo ser el usuario iker con el binario Env

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020250114184647.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020250114184731.png)

Y ahora soy iker

Ejecuto sudo -l para ver como puedo ser usuario root

![](../../../Images/Pasted%20image%2020250114184810.png)

Puedo escalar con el binario python, así que borro ese archivo .py y creo el mío propio con una bash:

![](../../../Images/Pasted%20image%2020250114185217.png)

Y listo, ya soy ROOT

Flag de root:

![](../../../Images/Pasted%20image%2020250114185243.png)
