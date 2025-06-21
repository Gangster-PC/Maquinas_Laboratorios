Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020250114190110.png)

Reviso el único puerto abierto, lo que corre por el puerto 80:

![](../../../Images/Pasted%20image%2020250114190411.png)

Interesante página con un botón

Haré fuzzing web usando Gobuster, para encontrar posibles directorios:

![](../../../Images/Pasted%20image%2020250114191513.png)

Encontré el directorio /themes

Accedo a el:

![](../../../Images/Pasted%20image%2020250114191539.png)

Pero no tengo permisos para acceder a este directorio

Volveré a usar Gobuster pero ahora apuntando hacia este directorio:

![](../../../Images/Pasted%20image%2020250114191707.png)

Y obtengo 2 directorios interesantes /uploads y /archivo.html

En el /archivo.html hay:

![](../../../Images/Pasted%20image%2020250114191741.png)

Tengo una posible subida de archivos en formato imagen .jpg:

Así que me crearé una Reverse Shell pero con un bypass de doble extensión es decir, un shell.php.jpg:

![](../../../Images/Pasted%20image%2020250114192636.png)

![](../../../Images/Pasted%20image%2020250114192642.png)

Ahora reviso el directorio /themes/uploads/

![](../../../Images/Pasted%20image%2020250114192726.png)

Doy click en mi archivo y me pongo a la escucha con Netcat:

![](../../../Images/Pasted%20image%2020250114192753.png)

Recibo la conexión y listo, estoy dentro de la máquina

Tratamiento de la TTY:

![](../../../Images/Pasted%20image%2020250114192826.png)

![](../../../Images/Pasted%20image%2020250114192859.png)

## ESCALADA DE PRIVILEGIOS

Ejecuto sudo -l para ver el binario que puedo usar

![](../../../Images/Pasted%20image%2020250114192936.png)

Puedo ser el usuario daphne con el binario Env

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020250114193009.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020250114193045.png)

Ahora soy daphne

Ejecuto sudo -l para ver el binario que puedo usar

![](../../../Images/Pasted%20image%2020250114193109.png)

Puedo ser el usuario vilma con el binario Ash

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020250114193142.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020250114193209.png)

Ahora soy el usuario vilma

Ejecuto sudo -l para ver el binario que puedo usar

![](../../../Images/Pasted%20image%2020250114193231.png)

Puedo ser el usuario shaggy con el binario Ruby

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020250114193300.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020250114193336.png)

Ahora soy el usuario shaggy

Ejecuto sudo -l para ver el binario que puedo usar

![](../../../Images/Pasted%20image%2020250114193402.png)

Puedo ser el usuario fred con el binario Lua

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020250114193430.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020250114193459.png)

Ahora soy el usuario fred

Ejecuto sudo -l para ver el binario que puedo usar

![](../../../Images/Pasted%20image%2020250114193511.png)

Puedo ser el usuario scooby con el binario Gcc

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020250114193545.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020250114193617.png)

Ahora soy el usuario scooby

Ejecuto sudo -l para ver el binario que puedo usar

![](../../../Images/Pasted%20image%2020250114193639.png)

Puedo ser el root con el binario Sudo

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020250114193713.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020250114193755.png)

Y listo, soy ROOT
