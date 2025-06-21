Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240822181118.png)

Veamos lo que corre en el puerto 80:

![](../../../Images/Pasted%20image%2020240822181138.png)

Tenemos una plantilla de Apache

Usaremos Gobuster para encontrar directorios:

![](../../../Images/Pasted%20image%2020240822181245.png)

Encontramos un /penguin.php

Accedamos a este directorio:

![](../../../Images/Pasted%20image%2020240822181300.png)

Tenemos un posible usuario llamado "pinguinito"

Haremos fuerza bruta con hydra para encontrar la contraseña de este usuario al puerto 22 SSH:

![](../../../Images/Pasted%20image%2020240822181616.png)

La contraseña del usuario pinguinito es love

Intrusión por el puerto 22 con estas credenciales:

![](../../../Images/Pasted%20image%2020240822181707.png)

![](../../../Images/Pasted%20image%2020240822181714.png)

## ESCALADA DE PRIVILEGIOS

En la carpeta de /home/pinguinito encontramos una base de datos:

![](../../../Images/Pasted%20image%2020240822181936.png)

Levantamos un servidor con python y la pasamos a nuestro kali:

![](../../../Images/Pasted%20image%2020240822182038.png)

![](../../../Images/Pasted%20image%2020240822182046.png)

Pero recordando la pista del directorio de penguin.php corre un servidor Grafana y en el directorio /tmp del servidor existe un .txt, así que accedamos a este archivo .txt por medio de un Path Traversal 
```
curl --path-as-is http://172.17.0.2:3000/public/plugins/alertlist/../../../../../../../../tmp/database_pass.txt
```

![](../../../Images/Pasted%20image%2020240822184117.png)

![](../../../Images/Pasted%20image%2020240822184318.png)

Ahora si pondemos acceder a la base de datos

![](../../../Images/Pasted%20image%2020240822184737.png)

![](../../../Images/Pasted%20image%2020240822184748.png)

Y encontramos la contraseña del usuario ballenasio

Escalemos al usuario ballenasio con su contraseña:

![](../../../Images/Pasted%20image%2020240822184825.png)

Ahora somos el usuario ballenasio

Ejecutamos sudo -l para ver el binario que podremos usar 

![](../../../Images/Pasted%20image%2020240822184915.png)

Y seremos root:

![](../../../Images/Pasted%20image%2020240822184934.png)

Y listo, ya somos ROOT

Flag de user:

![](../../../Images/Pasted%20image%2020240822184959.png)

Flag de root:

![](../../../Images/Pasted%20image%2020240822185012.png)

