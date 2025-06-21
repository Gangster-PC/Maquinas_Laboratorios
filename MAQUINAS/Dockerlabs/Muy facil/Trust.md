Escaneo de puertos:
![](../../../Images/Pasted%20image%2020240705194332%201.png)

En el puerto 80 tenemos:  

![](../../../Images/Pasted%20image%2020240705194435%201.png)

Haciendo fuzzing web  con Gobuster nos encontramos un Secret.php:
![](../../../Images/Pasted%20image%2020240705194549%201.png)

En el Secret.php hay:

![](../../../Images/Pasted%20image%2020240705194720%201.png)
Dándonos una pista de un usuario llamado Mario

Anteriormente en el escaneo de puertos vimos que estaba abierto el puerto 22 (SSH) entonces le haremos un ataque de fuerza bruta con Hydra con este usuario para encontrar la posible contraseña:
![](../../../Images/Pasted%20image%2020240705194915%201.png)
Nos da como usuario mario y contraseña chocolate 

Procedemos a entrar:
![](../../../Images/Pasted%20image%2020240705195012%201.png)

## ESCALADA DE PRIVILEGIOS

Ejecutamos sudo -l para ver el binario que podremos usar 
![](../../../Images/Pasted%20image%2020240705195059%201.png)
Podemos usar el binario Vim para hacer la escalada a root

Miramos en GTFOBins como podemos escalar con este binario:
![](../../../Images/Pasted%20image%2020240705203729.png)

Lo ejecutamos:
![](../../../Images/Pasted%20image%2020240705195243%201.png)

Y listo ya somos ROOT
