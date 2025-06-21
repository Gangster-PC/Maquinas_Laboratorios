Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240805074823.png)

Veamos lo que hay en el puerto 80:

![](../../../Images/Pasted%20image%2020240805074844.png)

Tenemos una plantilla de apache

Revisando su código fuente encontré:

![](../../../Images/Pasted%20image%2020240805074924.png)

En la línea 213 tenemos un código cifrado en Brainfuck

Lo deciframos:

![](../../../Images/Pasted%20image%2020240805075117.png)

Y tenemos una posible contraseña llamada "abuelacalientalasopa"

Así que probamos acceder al puerto 22 con el usuario abuela y la contraseña ya descifrada 

![](../../../Images/Pasted%20image%2020240805075828.png)

Y estamos dentro de la máquina

## ESCALADA DE PRIVILEGIOS

Ejecutamos sudo -l para ver el binario que podremos usar 

![](../../../Images/Pasted%20image%2020240805075959.png)

Podemos ser root con el binario Node

Miramos en GTFOBins como podemos escalar con este binario:

![](../../../Images/Pasted%20image%2020240805080040.png)

Lo ejecutamos:

![](../../../Images/Pasted%20image%2020240805080149.png)

Y listo, ya somos ROOT

Flag de user:

![](../../../Images/Pasted%20image%2020240805080215.png)

Flag de root:

![](../../../Images/Pasted%20image%2020240805080308.png)

