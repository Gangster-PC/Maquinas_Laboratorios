Escaneo de puertos:
![](../../../Images/Pasted%20image%2020240707195702.png)

Tenemos en el puerto 80:
![](../../../Images/Pasted%20image%2020240707195737.png)
Un panel de login

Haremos una facil inyección SQL para ver si tenemos acceso
![](../../../Images/Pasted%20image%2020240707200000.png)

Logramos entrar y tenemos:
![](../../../Images/Pasted%20image%2020240707200026.png)
Un usuario llamado dylan con su respectiva contraseña

Accedemos a la maquina por medio del puerto SSH con esas credenciales
![](../../../Images/Pasted%20image%2020240707200153.png)
Estamos dentro

## ESCALADA DE PRIVILEGIOS

Observamos los binarios de la maquina:
![](../../../Images/Pasted%20image%2020240707200319.png)
Y logramos evidenciar que podemos escalar por medio del binario Env

Miramos en GTFOBins como podemos escalar con este binario:
![](../../../Images/Pasted%20image%2020240707200418.png)

Lo ejecutamos:
![](../../../Images/Pasted%20image%2020240707200517.png)

Y listo, ya somos ROOT


