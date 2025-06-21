Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240812091457.png)

Veamos lo que hay en el puerto 80:

![](../../../Images/Pasted%20image%2020240812091533.png)

Tenemos una platilla de Apache

Haremos fuzzing web para encontrar directorios con Gobuster:

![](../../../Images/Pasted%20image%2020240812091706.png)

Encontramos el directorio /squirting

Accedamos a él:

![](../../../Images/Pasted%20image%2020240812091735.png)

Tenemos un panel de login y una parte para subir archivos

Intentamos acceder con usuario admin y la contraseña admin y subir una reverse shell en PHTML, por ejemplo

![](../../../Images/Pasted%20image%2020240812092341.png)

![](../../../Images/Pasted%20image%2020240812092833.png)

Y nos sale:

![](../../../Images/Pasted%20image%2020240812092848.png)

Que el tipo de archivo no es permitido

Abriré Burpsuite para interceptar la petición y lograr decifrar que archivos acepta este panel de subida de archivos:

![](../../../Images/Pasted%20image%2020240812093108.png)

Mandamos todo al Intruder:

![](../../../Images/Pasted%20image%2020240812093223.png)

Señalamos la parte en la que vamos a probar varios tipos de extensiones para ver cual nos acepta la web:

![](../../../Images/Pasted%20image%2020240812093335.png)

Creamos una lista con posibles extensiones .PHP:

![](../../../Images/Pasted%20image%2020240812093810.png)

Y damos al botón naranja de Start Attack:

![](../../../Images/Pasted%20image%2020240812094004.png)

Y obsevamos que unicamente acepta las extensiones .phar

Cambiamos nuestro archivo de la reverse shell a .phar y lo intentamos subir nuevamente:

![](../../../Images/Pasted%20image%2020240812094129.png)

![](../../../Images/Pasted%20image%2020240812094143.png)

Listo, ahora si se subió nuestro archivo satisfactoriamente

Veamos el código fuente de esta página:

![](../../../Images/Pasted%20image%2020240812092121.png)

Tenemos un posible directorio

Intentemos acceder a él:

![](../../../Images/Pasted%20image%2020240812094233.png)

Encontramos nuestro archivo

Nos ponemos a la escucha con NetCat damos click en nuestro archivo:

![](../../../Images/Pasted%20image%2020240812094313.png)

Y estamos dentro de la máquina:

Hacemos tratamiento de la TTY:

![](../../../Images/Pasted%20image%2020240812094458.png)

![](../../../Images/Pasted%20image%2020240812094519.png)

## ESCALADA DE PRIVILEGIOS

En el directorio /var/backup nos encontramos con un archivo escondido:

![](../../../Images/Pasted%20image%2020240812094801.png)

Llamado .cositas

Lo leemos:

![](../../../Images/Pasted%20image%2020240812094823.png)

Tenemos una id_rsa

Veamos los usuarios que contiene esta máquina:

![](../../../Images/Pasted%20image%2020240812094850.png)

Tenemos el usuario huevos fritos

Nos pasamos esta id_rsa a nuestro kali

![](../../../Images/Pasted%20image%2020240812094940.png)

Extraemos el hash de este id_rsa con Ssh2john:

![](../../../Images/Pasted%20image%2020240812095152.png)

Y lo crackeo con John, para saber su contraseña:

![](../../../Images/Pasted%20image%2020240812095437.png)

Y es honda1:

Le damos permisos a el id_rsa y accedemos por el puerto 22 SSH:

![](../../../Images/Pasted%20image%2020240812095518.png)

Y listo, estamos nuevamente dentro de la máquina pero ahora siendo otro usuario

Flag de user:

![](../../../Images/Pasted%20image%2020240812095559.png)

Ejecutamos sudo -l para ver el binario que podremos usar 

![](../../../Images/Pasted%20image%2020240812095638.png)

Podemos ser root con el binario Python

Miramos en GTFOBins como podemos escalar con este binario:

![](../../../Images/Pasted%20image%2020240812095759.png)

Crearemos un archivo .py:

![](../../../Images/Pasted%20image%2020240812100154.png)

Ahora ejecutaremos este archivo root.py con el binario python3:

![](../../../Images/Pasted%20image%2020240812100236.png)

Y listo, ya somos  ROOT

Flag de root:

![](../../../Images/Pasted%20image%2020240812100315.png)

