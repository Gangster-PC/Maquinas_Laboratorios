Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020240707200817.png)

Obsero lo que hay en el puerto 80:

![](../../../Images/Pasted%20image%2020240707200934.png)

Pagina en blanco

Miraré su codigo fuente:

![](../../../Images/Pasted%20image%2020240707201006.png)

Tengo 2 posibles usuarios "Juan" y "Camilo"

Creo un archivo de texto con estos 2 usuarios

![](../../../Images/Pasted%20image%2020240707201151.png)

Haré un ataque de fuerza bruta con Hydra a estos 2 posibles usuarios por medio del puerto 22 SSH para encontrar su posible contraseña del usuario existente:

![](../../../Images/Pasted%20image%2020240707201248.png)

Encuentro que el usuario existente dentro de la máquina es "camilo" y su contraseña es "password1"

Entraré a la máquina por el puerto SSH con esas credenciales:

![](../../../Images/Pasted%20image%2020240707201449.png)

## ESCALADA DE PRIVILEGIOS

Encuentro un correo.txt en la carpeta /var/mail/camilo que dice:

![](../../../Images/Pasted%20image%2020240707201808.png)

Haré la escalada al usuario juan con esa contraseña:

![](../../../Images/Pasted%20image%2020240707201908.png)

Ahora soy juan

Ejecuto sudo -l para ver el binario que podré usar 

![](../../../Images/Pasted%20image%2020240707201945.png)

Puedo usar el binario ruby para ser root

Miro en GTFOBins cómo puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020240707202020.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020240707202109.png)

Y listo, ya soy ROOT 
