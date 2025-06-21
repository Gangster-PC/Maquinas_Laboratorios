Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240805162657.png)

Veamos lo que hay en el puerto 80:

![](../../../Images/Pasted%20image%2020240805162720.png)

Tenemos una plantilla de Apache

Haciendo fuzzing web con  Gobuster  en busca de directorios, nos encontramos con un Wordpress:

![](../../../Images/Pasted%20image%2020240805163123.png)

Que contiene:

![](../../../Images/Pasted%20image%2020240805163227.png)

Pero investigando no encontramos nada importante en esta pagina web por el puerto 80

Así que haremos fuerza bruta al puerto 3306 Mysql con el usuario root para encontrar su contraseña:

![](../../../Images/Pasted%20image%2020240805164954.png)

Encontramos que la contraseña del usuario root de la base de datos es cassandra

Accederemos a la base de datos MYSQL con estas credenciales:

![](../../../Images/Pasted%20image%2020240805165112.png)

Miraremos las bases de datos:

![](../../../Images/Pasted%20image%2020240805165141.png)

Usaremos la base de datos Confidencial

![](../../../Images/Pasted%20image%2020240805165337.png)

Veremos sus tablas:

![](../../../Images/Pasted%20image%2020240805165357.png)

Seleccionamos todo de esta tabla de usuarios

![](../../../Images/Pasted%20image%2020240805165551.png)

Y tenemos un usuario y contraseña

Intrusión al puerto 22 con estos datos:

![](../../../Images/Pasted%20image%2020240805165659.png)

Y listo, estamos dentro de la máquina

Flag de user:

![](../../../Images/Pasted%20image%2020240805165743.png)

## ESCALADA DE PRIVILEGIOS

En el directorio /opt encontramos un archivo .zip

![](../../../Images/Pasted%20image%2020240805165917.png)

Me lo paso a mi kali con la herramienta de scp

![](../../../Images/Pasted%20image%2020240805170249.png)

```
scp -r mortadela@192.168.1.201:/opt/confidencial.zip .
```

Le extraemos la hash a este archivo:

![](../../../Images/Pasted%20image%2020240805170435.png)

Y la crackeamos con John:

![](../../../Images/Pasted%20image%2020240805170500.png)

Observamos que la contraseña para extraer el .zip es pinkgirl

Lo extraemos y vemos que contiene:

![](../../../Images/Pasted%20image%2020240805170555.png)

Tenemos una database pero para poder abrirla nos pide una contraseña la cual no tenemos, así que usaremos una  herramienta para encontrar la herramienta del archivo .DMP
https://github.com/z-jxy/keepass_dump.git
Nos pasamos la herramienta a el kali:

![](../../../Images/Pasted%20image%2020240805171134.png)

Y la ejecutamos para encontrar la contraseña de KeePass.DMP:

![](../../../Images/Pasted%20image%2020240805171400.png)

Y la contraseña de la base de datos es Maritrini12345

Accedemos a la base de datos con esta contraseña:

![](../../../Images/Pasted%20image%2020240805172222.png)

Y encontramos la contraseña de root:

![](../../../Images/Pasted%20image%2020240805172247.png)

Accederemos al puerto 22 con estas credenciales:

![](../../../Images/Pasted%20image%2020240805172417.png)

Y listo, estamos dentro de la máquina siendo ROOT

Flag de root:

![](../../../Images/Pasted%20image%2020240805172449.png)

