Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020250130183431.png)

Observo lo que corre por el puerto 80:

![](../../../Images/Pasted%20image%2020250130191620.png)

Revisando el código fuente de la página en la línea 3 me encuentro con un backupp.js:

![](../../../Images/Pasted%20image%2020250130191607.png)

Lo leo:

![](../../../Images/Pasted%20image%2020250130191643.png)

Tengo una contraseña

Haré fuerza bruta con Hydra hacia el puerto SSH para encontrar el posible usuario de esta contraseña:

![](../../../Images/Pasted%20image%2020250130191721.png)

Y el usuario es carlos y su contraseña es baluleroh

Intrusión por el puerto 22 SSH:

![](../../../Images/Pasted%20image%2020250130191811.png)

Y estoy dentro de la máquina

## ESCALADA DE PRIVILEGIOS

Tengo una nota en el directorio de carlos que dice:

![](../../../Images/Pasted%20image%2020250130192131.png)

Me da a entender que tengo una copia de seguridad, veré si puedo leerla

Ejecuto sudo -l para ver el binario que puedo usar

![](../../../Images/Pasted%20image%2020250130191910.png)

Puedo ser root con el binario Ping y el binario Xxd

Miro en GTFOBins como puedo escalar con el binario Xxd:

![](../../../Images/Pasted%20image%2020250130192107.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020250130192215.png)

Y encontré las credenciales de root, las uso para escalar de usuario:

![](../../../Images/Pasted%20image%2020250130192246.png)

Y listo, ya soy ROOT
