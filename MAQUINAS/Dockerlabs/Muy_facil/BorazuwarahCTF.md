Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020240708210134.png)

Veamos lo que hay en el puerto 80:

![](../../../Images/Pasted%20image%2020240708210234.png)

Tenemos una simple imagen

Nos la descargaremos y le haremos escenografía para ver si depronto tiene algo oculto

![](../../../Images/Pasted%20image%2020240708210329.png)
Y si, tenemos un usuario llamado borazuwarah

Haremos un ataque de fuerza bruta con Hydra al puerto SSH con este usuario

![](../../../Images/Pasted%20image%2020240708210432.png)
Su contraseña es 123456

Haremos la intrusión 

![](../../../Images/Pasted%20image%2020240708210522.png)
Estamos dentro

## ESCALADA DE PRIVILEGIOS

Ejecuto sudo -l para ver el binario que puedo usar 

![](../../../Images/Pasted%20image%2020240708210551.png)
Puedo ser root con el binario Bash

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020240708210732.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020240708210807.png)

Y listo ya soy ROOT

