Escaneo de puertos con Nmap:

![](../../../Images/Pasted%20image%2020240826090455.png)

Veo lo que corre en el puerto 80:

![](../../../Images/Pasted%20image%2020240826090522.png)

Coloco cualquier cosa por probar a ver que pasa:

![](../../../Images/Pasted%20image%2020240827085751.png)

Me responde con lo mismo que coloqué y genera un archivo .txt con lo que escribí

Voy a colocar un comando para ver que archivos hay, por ejemplo "ls -ña" para ver si la máquina me responde:

![](../../../Images/Pasted%20image%2020240827090123.png)

![](../../../Images/Pasted%20image%2020240827090135.png)

Y efectivamente me responde al comando que le coloqué

Así que leeré los usuarios que contiene esta máquina:

![](../../../Images/Pasted%20image%2020240827090311.png)

![](../../../Images/Pasted%20image%2020240827090415.png)

Tengo el usuario samara

Entonces veré si este usuario tiene id_rsa:

![](../../../Images/Pasted%20image%2020240827090822.png)

![](../../../Images/Pasted%20image%2020240827090903.png)

Y efectivamente si contiene

Me paso esta id_rsa a mi kali y hago la intrusión por el puerto 22 SSH con el usuario samara:

![](../../../Images/Pasted%20image%2020240827091258.png)

Flag de user:

![](../../../Images/Pasted%20image%2020240827091312.png)

## ESCALADA DE PRIVILEGIOS

Me levantaré un servidor con python para pasarme desde mi kali la herramienta Pspy64:

![](../../../Images/Pasted%20image%2020240827091547.png)

Le doy permisos de ejecución y la ejecuto para ver los procesos que están corriendo en segundo plano:

![](../../../Images/Pasted%20image%2020240827091704.png)

Observo que todo el tiempo se está ejecutando un script echo.sh

![](../../../Images/Pasted%20image%2020240827091812.png)

Lo edito y le coloco al final el permiso de ejecución a la /bin/bash:

![](../../../Images/Pasted%20image%2020240827091953.png)

Lo guardo y ejecuto bash -p:

![](../../../Images/Pasted%20image%2020240827092016.png)

Y listo, ya soy ROOT

Flag de root

![](../../../Images/Pasted%20image%2020240827092044.png)

