Observo lo que corre en el puerto 80: 

![](../../../Images/Pasted%20image%2020240731191705.png)

Tengo una pagina con un boton que en realidad no hace nada
Haré fuzzing web con la herramienta de Gobuster para encontrar directorios: 

![](../../../Images/Pasted%20image%2020240731191817.png)

Miro lo que hay en el directorio /backend 

![](../../../Images/Pasted%20image%2020240731191856.png)

Tengo unos archivos

En el archivo server.js hay: 

![](../../../Images/Pasted%20image%2020240731191930.png)

Encontré una posible contraseña "lapassworddebackupmaschingonadetodas"

Haré un ataque de fuerza bruta con Hydra a el puerto 5000 SSH con esta contraseña, para encontrar el usuario: 

![](../../../Images/Pasted%20image%2020240731192349.png)

El usuario es lovely

Intrusión por el puerto 5000 SSH: 

![](../../../Images/Pasted%20image%2020240731192446.png)

Y listo, estoy dentro de la máquina

### ESCALADA DE PRIVILEGIOS

Ejecuto sudo -l para ver el binario que puedo usar 

![](../../../Images/Pasted%20image%2020240731192607.png)

Puedo ser root con el binario Nano

Miro en GTFOBins como puedo escalar con este binario: 

![](../../../Images/Pasted%20image%2020240731192652.png)

Lo ejecuto: 

![](../../../Images/Pasted%20image%2020240731192711.png)

![](../../../Images/Pasted%20image%2020240731192734.png)

![](../../../Images/Pasted%20image%2020240731192818.png)

Y listo, ya soy ROOT
