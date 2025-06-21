Escaneo de puertos con Nmap: 

![](../../../Images/Pasted%20image%2020240823080915.png)

Observo lo que corre en el puerto 80: 

![](../../../Images/Pasted%20image%2020240823081120.png)

Existe un mensaje de un supeusto "virus", pero observo que tengo un botón en la página web.
Este botón me lleva a: 

![](../../../Images/Pasted%20image%2020240823081154.png)

Pero tengo un ejemplos.php probaré si existe un Local File Inclusion (LFI), usaré la herramienta Wfuzz para encontrar el parámetro que me permite explotar esta vulnerabilidad: 

![](../../../Images/Pasted%20image%2020240823081451.png)

Y el parámetro es images, pruebo apuntando hacia la carpeta /etc/passwd para ver los usuarios que posee esta máquina: 

![](../../../Images/Pasted%20image%2020240823081524.png)

Tengo de usuario a nico

Haré fuerza bruta a este usuario para encontrar la contraseña del puerto 22 SSH: 

![](../../../Images/Pasted%20image%2020240823082131.png)

Y la contraseña es lovely

Intrusión por el puerto SSH con el usuario nico y la contraseña lovely: 

![](../../../Images/Pasted%20image%2020240823082227.png)

Estoy dentro de la máquina

### ESCALADA DE PRIVILEGIOS

Ejecuto sudo -l para ver el binario que podré usar 

![](../../../Images/Pasted%20image%2020240823082319.png)

Puedo ser root con el binario Env

Miro en GTFOBins como puedo escalar con este binario: 

![](../../../Images/Pasted%20image%2020240823082345.png)

Lo ejecuto: 

![](../../../Images/Pasted%20image%2020240823082423.png)

Y listo, ya soy ROOT
