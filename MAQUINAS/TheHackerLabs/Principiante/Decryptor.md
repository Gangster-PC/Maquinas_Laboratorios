Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240729183625.png)

Veamos lo que hay en la página web:

![](../../../Images/Pasted%20image%2020240729184007.png)

Tenemos una plantilla de Apache 2

Veamos el codigo fuente de esta página web 

![](../../../Images/Pasted%20image%2020240729184108.png)

Tenemos un código en cifrado Brainfuck

Al descifrarlo quiere decir:

![](../../../Images/Pasted%20image%2020240729184207.png)

marioeatslettuce, parece ser una posible contraseña

Anteriormente vimos el puerto 2121 ftp abierto, procederemos a hacerle un ataque de fuerza bruta con Hydra con esta posible contraseña para saber el usuario

![](../../../Images/Pasted%20image%2020240729184457.png)

El usuario es mario

Intrusión por el puerto 2121 ftp con estas credenciales

![](../../../Images/Pasted%20image%2020240729184548.png)

Estamos dentro

Nos descargaremos en nuestra maquina la base de datos user.kdbx

![](../../../Images/Pasted%20image%2020240729184643.png)

Tratamos de leer esta base de datos con Keepassxc pero nos pide una contraseña que no tenemos por el momento

![](../../../Images/Pasted%20image%2020240729184741.png)

Así que sacaremos la hash de esta base de datos y la crackearemos con john:

![](../../../Images/Pasted%20image%2020240729185556.png)

Entonces la contraseña de la base de datos es moonshine1

Veamos lo que hay dentro de esta base de datos:

![](../../../Images/Pasted%20image%2020240729185644.png)

![](../../../Images/Pasted%20image%2020240729185656.png)

Haremos la intrusión por el puerto 22 con estas credenciales:

![](../../../Images/Pasted%20image%2020240729185759.png)

Y listo, estamos dentro de la maquina

## ESCALADA DE PRIVILEGIOS 

Ejecutamos sudo -l para ver el binario que podremos usar 

![](../../../Images/Pasted%20image%2020240729185915.png)

Podemos usar el binario Chown para hacer la escalada a root

Miramos en GTFOBins como podemos escalar con este binario:

![](../../../Images/Pasted%20image%2020240729190135.png)

Vemos que podemos cambiar o editar una carpeta o archivo con este binario

Lo ejecutamos:

![](../../../Images/Pasted%20image%2020240729190532.png)

La contraseña de test es test

Y listo ya somos ROOT

La flag del usuario:

![](../../../Images/Pasted%20image%2020240729190616.png)

La flag de root:

![](../../../Images/Pasted%20image%2020240729190628.png)

