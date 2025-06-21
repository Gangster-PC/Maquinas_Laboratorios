Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240804172737.png)

Veamos lo que hay en el puerto 1000

![](../../../Images/Pasted%20image%2020240804173023.png)

Un panel de login de Webmin el cual no tenemos ni usuario ni contraseña

Veamos que exploit encontramos en Metasploit para este Webmin

![](../../../Images/Pasted%20image%2020240804173307.png)

Usaremos el 10 y veremos los datos que nos pide el exploit:

![](../../../Images/Pasted%20image%2020240804173448.png)

Nos pide RHOST y LHOST:

![](../../../Images/Pasted%20image%2020240804173616.png)

Y escribimos run

![](../../../Images/Pasted%20image%2020240804173703.png)

Y ya estamos dentro de la máquina

La flag de user: 

![](../../../Images/Pasted%20image%2020240804173829.png)

## ESCALADA DE PRIVILEGIOS

Ejecutamos getcap -r / para ver los binarios que podemos usar 

![](../../../Images/Pasted%20image%2020240804174933.png)

Tenemos el binario Gdb 

Miramos en GTFOBins como podemos escalar con este binario:

![](../../../Images/Pasted%20image%2020240804180202.png)

Y lo ejecutamos:

![](../../../Images/Pasted%20image%2020240804180146.png)

Y listo, ya somos ROOT

La flag de root:

![](../../../Images/Pasted%20image%2020240804180250.png)

