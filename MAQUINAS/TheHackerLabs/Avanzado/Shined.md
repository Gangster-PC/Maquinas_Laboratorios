Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240824214040.png)

Es interesante esta máquina ya que contiene 2 puertos SSH

Veamos lo que contiene el puerto 80:

![](../../../Images/Pasted%20image%2020240824214156.png)

Tenemos una página de venta de gafas

Haciendo fuzzing web con Gobuster encontramos un directorio llamado /access.php

![](../../../Images/Pasted%20image%2020240824214348.png)

Accedamos a él:

![](../../../Images/Pasted%20image%2020240824214410.png)

Tenemos un panel de Login

Pero me llama la atención que termina en .php por ende intentare hacer un LFI pero usaré la herramienta Wfuzz para encontrar el parámetro para acceder a carpetas internas de la máquina:

![](../../../Images/Pasted%20image%2020240824214908.png)

Y el parámetro es inet

Explotación de la vulnerabilidad Local File Inclusion para leer usuarios en la carpeta /etc/passwd:

![](../../../Images/Pasted%20image%2020240824215023.png)

Tenemos un usuario llamado cifra

Tratamos de ver si hay id_rsa de este usuario:

![](../../../Images/Pasted%20image%2020240824215153.png)

Y efectivamente si la contiene

La pasamos a nuestro kali y hacemos la intrusión por el puerto SSH 2222 con el usuario cifra y la id_rsa:

![](../../../Images/Pasted%20image%2020240824215632.png)

Y listo, estamos dentro de la máquina

Pero nos damos cuenta que estamos dentro de un contenedor

![](../../../Images/Pasted%20image%2020240824215842.png)

Y tenemos un archivo .xlsm

![](../../../Images/Pasted%20image%2020240824215907.png)

Para pasar este archivo a mi kali lo convertiré en base 64:

![](../../../Images/Pasted%20image%2020240824220454.png)

Ahora pegaré este codigo en un archivo y lo decodificare de la base64:

![](../../../Images/Pasted%20image%2020240824220642.png)

Ya tengo ese archivo en mi kali, ahora lo abriré con la herramienta de Libreoffice:

![](../../../Images/Pasted%20image%2020240824221013.png)

Revisando las macros de este Excel encontré unas credenciales, las probaremos en el puerto SSH 22:

![](../../../Images/Pasted%20image%2020240824221115.png)

![](../../../Images/Pasted%20image%2020240824221132.png)

Y efectivamente son unas credenciales válidas estamos nuevamente dentro de la máquina pero ahora ya no en un contenedor

Flag de user:

![](../../../Images/Pasted%20image%2020240824221209.png)

## ESCALADA DE PRIVILEGIOS

Revisando en la carpeta /tmp encontramos 2 archivos .sh:

![](../../../Images/Pasted%20image%2020240825123312.png)

El backup.sh comprime todos los directorios se Script y los guarda en un archivo .tgz
El clean.sh ejecuta un comando que elimina el directorio Scripts

Estos 2 archivos se están ejecutando simultáneamente en segundo plano pero como el backup.sh ejecuta todo lo que contenga el directorio scripts con * , explotaremos una vulnerabilidad llamada Wildcard, nos ayudaremos de la página Hacktricks:

![](../../../Images/Pasted%20image%2020240825124640.png)

Entonces primeramente le daremos permisos /bin/bash en un archivo llamado shell.sh:

![](../../../Images/Pasted%20image%2020240825125213.png)

 Ahora ejecutaremos los comandos que encontramos en la página web:
 

![](../../../Images/Pasted%20image%2020240825130416.png)

  Ahora ejecutamos bash -p
  

![](../../../Images/Pasted%20image%2020240825130627.png)

  Y listo, ya somos ROOT

Flag de root:

![](../../../Images/Pasted%20image%2020240825130646.png)

 