Escaneo de puertos con Nmap

![](../../../Images/Pasted%20image%2020240712102003.png)

Veamos lo que hay en el puerto 80

![](../../../Images/Pasted%20image%2020240712102046.png)

Nos pide que ingresemos el nombre de alguna fruta

Probamos por ejemplo con Mango y nos sale:

![](../../../Images/Pasted%20image%2020240712102141.png)

Pagina no encontrada o caida, pero tenemos un buscar.php

Haremos fuzzing para encontrar dominios ocultos con Gobuster

![](../../../Images/Pasted%20image%2020240712102519.png)

Encontramos un fruits.php veamos lo q contiene 

![](../../../Images/Pasted%20image%2020240712102656.png)

Pagina en blanco

Haremos fuzzing web para hacer un local file inclusion con Wfuzz

![](../../../Images/Pasted%20image%2020240712104113.png)

Encontramos "file"

Veamos la carpeta /etc/passwd para ver posibles usuarios por medio del subdominio file con el metodo de local file inclusion

![](../../../Images/Pasted%20image%2020240712104255.png)

Tenemos el usuario bananaman

Haremos un ataque de fuerza bruta con Hydra a este usuario bananaman por el puerto SSH

![](../../../Images/Pasted%20image%2020240712104803.png)

Y vemos que la contraseña para bananaman es celtic

Inclusión por el puerto 22:

![](../../../Images/Pasted%20image%2020240712104901.png)

Estamos dentro

Encontramos la flag de user:

![](../../../Images/Pasted%20image%2020240712104943.png)

## ESCALADA DE PRIVILEGIOS 

Ejecutamos sudo -l para ver el binario que podremos usar 

![](../../../Images/Pasted%20image%2020240712105210.png)

Podemos usar el binario Find para hacer la escalada a root

Miramos en GTFOBins como podemos escalar con este binario:

![](../../../Images/Pasted%20image%2020240712105243.png)

Lo ejecutamos:

![](../../../Images/Pasted%20image%2020240712105317.png)

Y listo, ya somos root

Encontramos la flag de root

![](../../../Images/Pasted%20image%2020240712105358.png)

