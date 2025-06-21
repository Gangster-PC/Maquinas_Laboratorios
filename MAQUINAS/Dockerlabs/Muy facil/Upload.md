Escaneo de puertos:
![](../../../Images/Pasted%20image%2020240705201619%201.png)

En el puerto 80 tenemos:
![](../../../Images/Pasted%20image%2020240705201708%201.png)
Una subida de archivos

Procedemos a subir un archivo .php con una reverse shell hacia nuestra maquina Linux
![](../../../Images/Pasted%20image%2020240705201856%201.png)

Ahora hacemos un fuzzing web con Gobuster para encontrar el archivo que acabamos de subir
![](../../../Images/Pasted%20image%2020240705202001.png)

Lo encontramos está ubicado en /uploads
![](../../../Images/Pasted%20image%2020240705202042.png)

Nos ponemos a la escucha con netcat por el puerto 443 y damos click en el archivo shell.php y recibimos la shell reversa
![](../../../Images/Pasted%20image%2020240705202216.png)

Hacemos tratamiento de la TTY
![](../../../Images/Pasted%20image%2020240705202527.png)
![](../../../Images/Pasted%20image%2020240705202604.png)

## ESCALADA DE PRIVILEGIOS

Ejecutamos sudo -l para ver el binario que podremos usar 
![](../../../Images/Pasted%20image%2020240705202648.png)
Podemos ser root con el binario env

Miramos en GTFOBins como podemos escalar con este binario:
![](../../../Images/Pasted%20image%2020240705203427.png)

Lo ejecutamos:
![](../../../Images/Pasted%20image%2020240705202830.png)

Y listo, ya somos usuario ROOT 




