Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240729191148.png)

Veo lo que hay en el puerto 80:

![](../../../Images/Pasted%20image%2020240729191501.png)

Intenté hacer fuzzing web y no encontré nada; los botones de la pagina no me llevan a ningún lado, simplemente se recarga la pagina

Así que abriré Burpsuite para ver como se tramita la petición de los botones

![](../../../Images/Pasted%20image%2020240729191647.png)

![](../../../Images/Pasted%20image%2020240729192552.png)

Doy click en Submit Reservation

![](../../../Images/Pasted%20image%2020240729192626.png)

Y me abre Burpsuite con la petición interceptada

Mando todo al Repeater

![](../../../Images/Pasted%20image%2020240729192727.png)

Pruebo metiendo otra palabra en userInput y veo que me dice que acepta base64

![](../../../Images/Pasted%20image%2020240729192824.png)

Leeré el archivo /etc/passwd pero con un comando en base 64

![](../../../Images/Pasted%20image%2020240729192931.png)

![](../../../Images/Pasted%20image%2020240729193040.png)

Tengo el usuario cachopin

Haré un ataque de fuerza bruta al puerto SSH con este usuario para saber su contraseña:

![](../../../Images/Pasted%20image%2020240729193242.png)

Y en encuentro que es "simple" su contraseña

 Intrusión por el puerto 22 SSH:

![](../../../Images/Pasted%20image%2020240729193331.png)

Y listo, estoy dentro

## ESCALADA DE PRIVILEGIOS

En el directorio /home/cachopin/app/com/personal tengo un archivo llamado hash.lst con diferentes hashes

![](../../../Images/Pasted%20image%2020240729193624.png)

Me descargo la herramienta de PatxaSec llamada SHA_Decrypt para decifrar la contraseña de estas hashes

![](../../../Images/Pasted%20image%2020240729195942.png)

![](../../../Images/Pasted%20image%2020240729200009.png)

Pruebo con la primera hash:

![](../../../Images/Pasted%20image%2020240729200357.png)

Y con la primera no me encontró nada

Probaré con la segunda:

![](../../../Images/Pasted%20image%2020240729200453.png)

Encuentro "cecina", que es la contraseña de root

Intengo ser root con esta contraseña:

![](../../../Images/Pasted%20image%2020240729200540.png)

Y listo, ya soy ROOT
