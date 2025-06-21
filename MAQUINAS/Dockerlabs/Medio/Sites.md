Escaneo de puertos:
![](../../../Images/Pasted%20image%2020240806085707.png)

Veamos lo que hay en el puerto 80:
![](../../../Images/Pasted%20image%2020240806090023.png)
Tenemos una especie de manual para poder proteger un Apache y su configuración, pero también la página nos da una pista de usar LFI (Local File Inclusion) entonces usaremos esta pista para poder leer directorios internos de la máquina sin acceder directamente a ella

Haremos fuzzing web en busca de directorios con Gobuster
![](../../../Images/Pasted%20image%2020240806090252.png)
Encontramos /vulnerable.php

Accedamos a el:
![](../../../Images/Pasted%20image%2020240806090323.png)

Hagamos el Local File Inclusion apuntando hacia el directorio /etc/passwd para ver los usuarios que contiene la máquina:
![](../../../Images/Pasted%20image%2020240806090427.png)
Usaremos la herramienta Wfuzz para encontrar la palabra clave que va en FUZZ para poder acceder a la carpeta de passwd
![](../../../Images/Pasted%20image%2020240806090513.png)
Y la palabra clave es Page

Accedemos ahora si a leer el contenido de esta carpeta:
![](../../../Images/Pasted%20image%2020240806091011.png)
Y encontramos un usuario llamado chocolate

En la pagina web se menciona un archivo llamado sitio.conf el cual contiene información sensible, vamos a encontrarlo:
![](../../../Images/Pasted%20image%2020240806092456.png)
Nos encontramos en él que existe un archivo llamado archivitotraviesito

Vamos a encontrarlo:
![](../../../Images/Pasted%20image%2020240806092613.png)
Lo encontramos, y contiene la contraseña del usuario chocolate

Intrusión al puerto 22:
![](../../../Images/Pasted%20image%2020240806092705.png)
Y listo, estamos dentro de la máquina

## ESCALADA DE PRIVILEGIOS

Ejecutamos sudo -l para ver el binario que podremos usar
![](../../../Images/Pasted%20image%2020240806092742.png)
Podemos ser root con el binario Sed

Miramos en GTFOBins como podemos escalar con este binario:
![](../../../Images/Pasted%20image%2020240806092832.png)

Lo ejecutamos:
![](../../../Images/Pasted%20image%2020240806092908.png)

Y listo, ya somos ROOT