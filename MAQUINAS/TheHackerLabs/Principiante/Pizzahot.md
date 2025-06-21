Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240712172937.png)

Veamos lo que hay en el puerto 80:

![](../../../Images/Pasted%20image%2020240712173222.png)

Tenemos una pagina de venta de pizza común y corriente 

Veamos lo que hay en su codigo fuente (Control+U)

![](../../../Images/Pasted%20image%2020240712174147.png)

Tenemos un posible usuario llamado pizzapiña

Haremos un ataque de fuerza bruta con Hydra a este usuario por el puerto 22, para saber su contraseña

![](../../../Images/Pasted%20image%2020240712174512.png)

Y efectivamente nos arroja que es steven su contraseña

Haremos la intrusión por medio del puerto SSH

![](../../../Images/Pasted%20image%2020240712174551.png)

Estamos dentro

La flag del user es:

![](../../../Images/Pasted%20image%2020240712174625.png)

## ESCALADA DE PRIVILEGIOS

Ejecutamos sudo -l para ver el binario que podremos usar 

![](../../../Images/Pasted%20image%2020240712174721.png)

Y vemos que para ser el usuario Pizzasinpiña podemos ejecutar el binario Gcc

Miramos en GTFOBins como podemos escalar con este binario:

![](../../../Images/Pasted%20image%2020240712174810.png)

Lo ejecutamos:

![](../../../Images/Pasted%20image%2020240712174840.png)

Y listo ahora somos el usuario Pizzasinpiña

Ejecutamos sudo -l para ver el binario que podremos usar para ser root

![](../../../Images/Pasted%20image%2020240712174915.png)

Podemos usar el binario Man para hacer la escalada a root

Miramos en GTFOBins como podemos escalar con este binario:

![](../../../Images/Pasted%20image%2020240712175006.png)

Lo ejecutamos:

![](../../../Images/Pasted%20image%2020240712175103.png)

![](../../../Images/Pasted%20image%2020240712175255.png)

![](../../../Images/Pasted%20image%2020240712175327.png)

Y listo, ya somos root

Y la flag de root:

![](../../../Images/Pasted%20image%2020240712175353.png)

