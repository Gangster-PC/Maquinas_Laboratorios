Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240815185855.png)

Veamos lo que hay en el puerto 80:

![](../../../Images/Pasted%20image%2020240815190000.png)

Tenemos una plantilla de Apache

Haremos fuzzing web con Gobuster en busca de directorios:

![](../../../Images/Pasted%20image%2020240815190931.png)

Accedamos a /web

![](../../../Images/Pasted%20image%2020240815190951.png)

Tenemos un posible usuario llamado Bob

Haremos fuerza bruta con este usuario para encontrar su contraseña a los puertos 22 y 21:

![](../../../Images/Pasted%20image%2020240815191220.png)

Es la misma contraseña para ambos puertos, chocolate

Intrusión por el puerto 21 FTP:

![](../../../Images/Pasted%20image%2020240815191406.png)

Encontramos un .sh y un .txt, nos descargamos el .sh a nuestro kali y lo leemos:

![](../../../Images/Pasted%20image%2020240815191532.png)

Es un script para borrar archivos temporales del sistema

Intrusión por el puerto SSH 22:

![](../../../Images/Pasted%20image%2020240815191634.png)

Y listo, estamos dentro de la máquina

## ESCALADA DE PRIVILEGIOS

Miramos los usuarios que hay dentro de la máquina

![](../../../Images/Pasted%20image%2020240815192030.png)

Nos encontramos el usuario secretote

Le haremos fuerza bruta con hydra a este usuario por el puerto SSH:

![](../../../Images/Pasted%20image%2020240815193154.png)

Encontramos que la contraseña es chocolate1

Intrusión con el usuario secretote y su contraseña por el puerto 22:

![](../../../Images/Pasted%20image%2020240815193300.png)

Estamos dentro de la máquina nuevamente

Ejecutamos sudo -l para ver el binario que podremos usar 

![](../../../Images/Pasted%20image%2020240815193329.png)

Podemos ser root con el binario Man

Miramos en GTFOBins como podemos escalar con este binario:

![](../../../Images/Pasted%20image%2020240815193408.png)

Lo ejecutamos:

![](../../../Images/Pasted%20image%2020240815193430.png)

![](../../../Images/Pasted%20image%2020240815193449.png)

![](../../../Images/Pasted%20image%2020240815193501.png)

Y listo, ya somos ROOT

Flag de root:

![](../../../Images/Pasted%20image%2020240815193614.png)

