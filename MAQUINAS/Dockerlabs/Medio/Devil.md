Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240919210021.png)

Observo lo que corre en el único puerto que tiene abierto la máquina, el puerto 80:

![](../../../Images/Pasted%20image%2020240919210112.png)

Es una web como de ciberseguridad

Haciendo fuzzing web con Dirb encuentro unos directorios algo sospechosos:

![](../../../Images/Pasted%20image%2020240922180823.png)

![](../../../Images/Pasted%20image%2020240922180834.png)

Accederé a el /plugins/backdoor:

![](../../../Images/Pasted%20image%2020240922180907.png)

Tengo una subida de archivos

Intentaré subir una Reverse Shell:

![](../../../Images/Pasted%20image%2020240922180959.png)

Se subió satisfactoriamente

Recordando que el resultado del fuzzing encontré también en este mismo directorio un /plugins/backdoor/uploads, accedo a él:

![](../../../Images/Pasted%20image%2020240922181052.png)

Y aquí está mi shell.php

Me pongo a la escucha con Netcat y doy click en mi archivo:

![](../../../Images/Pasted%20image%2020240922181134.png)

Recibo la conexión y ahora estoy dentro de la máquina

Tratamiento de la TTY:

![](../../../Images/Pasted%20image%2020240922181333.png)

![](../../../Images/Pasted%20image%2020240922181435.png)
