Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240820115753.png)

Veamos lo que hay en el puerto 80:

![](../../../Images/Pasted%20image%2020240820115948.png)

Tenemos una especie de blog

Haremos fuzzing web usando Gobuster en busca de directorios:

![](../../../Images/Pasted%20image%2020240820120242.png)

Y nos damos de cuenta que estamos ante un Wordpress

![](../../../Images/Pasted%20image%2020240820120308.png)

Usaremos la herramienta Wpscan en busca de usuarios y plugins vulnerables:

![](../../../Images/Pasted%20image%2020240820120418.png)

![](../../../Images/Pasted%20image%2020240820120451.png)

Intenté hacer fuerza brura pero no me dió resultado

Así que revisando el código fuente de la página o directorio http://aceituno.thl/2024/04/23/hola-mundo/

![](../../../Images/Pasted%20image%2020240820122355.png)

Encotramos un plugin desacutalizado llamado wpDiscuz

Miramos que exploit existe para este plugin:

![](../../../Images/Pasted%20image%2020240820122749.png)

Y nos descargamos un RCE

Y ejecutamos el exploit .py:

![](../../../Images/Pasted%20image%2020240820122907.png)

Ahora vamos a la webshell que nos generó el exploit:

![](../../../Images/Pasted%20image%2020240820123020.png)

Y comprobamos que si sirve el RCE 

Escribimos una reverse shell y nos ponemos a la escucha con netcat:

![](../../../Images/Pasted%20image%2020240820123152.png)

![](../../../Images/Pasted%20image%2020240820123214.png)

Recibimos la conexión y ahora estamos dentro de la máquina

Sabiendo que es un WordPress vamos a acceder al archivo "wp-config.php" para ver la contraseña de la base de datos:

![](../../../Images/Pasted%20image%2020240820123507.png)

Accedamos a la base de datos:

![](../../../Images/Pasted%20image%2020240820124530.png)

Miramos las bases de datos:

![](../../../Images/Pasted%20image%2020240820124811.png)

Usamos la base de datos "wordpress", miramos sus tablas:

![](../../../Images/Pasted%20image%2020240820124854.png)

![](../../../Images/Pasted%20image%2020240820125006.png)

Tenemos un usuario y su respectiva contraseña

Intrusión por el puerto SSH 22 con estas credenciales:

![](../../../Images/Pasted%20image%2020240820125116.png)

Y estamos dentro de la máquina por el puerto 22 siendo el usuario aceituno

Flag de user:

![](../../../Images/Pasted%20image%2020240820125151.png)

## ESCALADA DE PRIVILEGIOS

Ejecutamos sudo -l para ver el binario que podremos usar 

![](../../../Images/Pasted%20image%2020240820125207.png)

Podemos ser root con el binario Most, este binario nos permite leer archivos

Entonces leeremos la id_rsa de el usuario root:

![](../../../Images/Pasted%20image%2020240820125716.png)

Pero hay un truquito y es si oprimimos la combinación de teclas SHIFT + W +E, y escribimos !/bin/bash:

![](../../../Images/Pasted%20image%2020240820125911.png)

Seremos ROOT automaticamente:

![](../../../Images/Pasted%20image%2020240820130031.png)

Flag de root:

![](../../../Images/Pasted%20image%2020240820130045.png)

