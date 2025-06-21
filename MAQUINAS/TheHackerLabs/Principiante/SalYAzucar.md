Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240805111838.png)

Veamos lo que hay en el puerto 80:

![](../../../Images/Pasted%20image%2020240805111818.png)

Tenemos una plantilla de apache

Haremos fuzzing web con Gobuster en busca de directorios:

![](../../../Images/Pasted%20image%2020240805112004.png)

Encontramos Summary

Veamos lo que hay en el directorio /summary

![](../../../Images/Pasted%20image%2020240805112232.png)

Y dentro de summary.html:

![](../../../Images/Pasted%20image%2020240805112251.png)

Como no encontramos nada de información importante haremos ataque de fuerza bruta con Hydra al puerto SSH al usuario y contraseña:

![](../../../Images/Pasted%20image%2020240805112415.png)

Encontramos usuario info y contraseña qwerty

Intrusión al puerto 22:

![](../../../Images/Pasted%20image%2020240805112513.png)

Estamos dentro de la máquina

Flag de user:

![](../../../Images/Pasted%20image%2020240805112605.png)

## ESCALADA DE PRIVILEGIOS

Ejecutamos sudo -l para ver el binario que podremos usar 

![](../../../Images/Pasted%20image%2020240805112630.png)

Podemos escalar con el binario Base64 para ser root

Miramos en GTFOBins como podemos escalar con este binario:

![](../../../Images/Pasted%20image%2020240805112721.png)

Lo ejecutamos:

![](../../../Images/Pasted%20image%2020240805112940.png)

Nos pasamos el id_rsa de root a nuestro pc 

![](../../../Images/Pasted%20image%2020240805113038.png)

Le extraemos la hash:

![](../../../Images/Pasted%20image%2020240805113124.png)

Y la desencriptamos con john de ripper

![](../../../Images/Pasted%20image%2020240805113523.png)

Y tenemos que es honda1 la contraseña

Entraremos a la maquina nuevamente por el puerto 22 SSH pero ahora siendo root y con ayuda de el archivo id_rsa:

![](../../../Images/Pasted%20image%2020240805113716.png)

Y listo, accedimos siendo usuarios ROOT

Flag de root:

![](../../../Images/Pasted%20image%2020240805113745.png)

