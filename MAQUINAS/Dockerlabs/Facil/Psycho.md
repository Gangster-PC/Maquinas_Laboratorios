Escaneo de puertos con Nmap:

![](../../../Images/Pasted%20image%2020240810141916.png)

Veo lo que hay en el puerto 80:

![](../../../Images/Pasted%20image%2020240810141949.png)

Haré fuzzing web con Wfuzz para encontrar la palabra clave para poder encontrar la carpeta /etc/passd y así mismo poder explotar un Local File Inclusion (LFI)  

![](../../../Images/Pasted%20image%2020240810142849.png)

La palabra clave es Secret

Accedo a esta dirección:

![](../../../Images/Pasted%20image%2020240810142927.png)

![](../../../Images/Pasted%20image%2020240810142938.png)

Tengo el usuario luisillo y vaxei

Buscaré una id_rsa de alguno de los 2 usuarios:

![](../../../Images/Pasted%20image%2020240811112051.png)

Encuentro el id_rsa de el usuario vaxei

Creo un archivo con esta id_rsa

![](../../../Images/Pasted%20image%2020240811112322.png)

Le doy permisos y accedo por el puerto 22 con esta id_rsa siendo el usuario vaxei

![](../../../Images/Pasted%20image%2020240811112552.png)

Y listo, estoy dentro de la máquina

## ESCALADA DE PRIVILEGIOS

Ejecuto sudo -l para ver el binario que podré usar

![](../../../Images/Pasted%20image%2020240811112902.png)

Para poder ser el usuario luisillo puedo ejecutar el binario Perl

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020240811112946.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020240811113022.png)

Y listo, ya soy el usuario luisillo de la máquina

Ejecuto sudo -l para ver el binario que podré usar para seguir escalando privilegios:

![](../../../Images/Pasted%20image%2020240811113116.png)

Puedo ser root ejecutando con python un archivo ubicado en /opt llamado paw.py

Observo lo que hace este archivo .py:

![](../../../Images/Pasted%20image%2020240811113310.png)

![](../../../Images/Pasted%20image%2020240811113543.png)

Este script realiza varias tareas sencillas y ejecuta algunos comandos del sistema que solo muestran texto en la terminal. A pesar de incluir funciones que manipulan datos y realizan cálculos, estas no influyen de manera considerable en el resultado final.

Logro observar que el script importa la librería subprocess, entonces me crearé un archivo subprocess.py con una /bin/bash para que al momento de ejecutar el paw.py me lea mi archivo malicioso y automáticamente escale a root:

![](../../../Images/Pasted%20image%2020240811114023.png)

![](../../../Images/Pasted%20image%2020240811114035.png)

Y listo, ya soy ROOT
