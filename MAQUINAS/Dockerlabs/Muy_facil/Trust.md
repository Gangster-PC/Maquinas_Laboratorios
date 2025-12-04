Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020240705194332%201.png)

En el puerto 80 tengo:  

![](../../../Images/Pasted%20image%2020240705194435%201.png)

Haciendo fuzzing web con Gobuster me encuentro un Secret.php:
```
gobuster dir -u http://172.17.0.2 -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -x html,php,txt
```

![](../../../Images/Pasted%20image%2020240705194549%201.png)

En el Secret.php hay:

![](../../../Images/Pasted%20image%2020240705194720%201.png)

Dándome una pista de un usuario llamado Mario

Anteriormente en el escaneo de puertos ví que estaba abierto el puerto 22 (SSH) entonces le haré un ataque de fuerza bruta con Hydra con este usuario para encontrar la posible contraseña:

![](../../../Images/Pasted%20image%2020240705194915%201.png)

Me da como usuario "mario" y su contraseña "chocolate" 

Procedo a entrar:

![](../../../Images/Pasted%20image%2020240705195012%201.png)

## ESCALADA DE PRIVILEGIOS

Ejecuto sudo -l para ver el binario que podremos usar:

![](../../../Images/Pasted%20image%2020240705195059%201.png)

Puedo usar el binario Vim para hacer la escalada a root

Miro en GTFOBins como podemos escalar con este binario:

![](../../../Images/Pasted%20image%2020240705203729.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020240705195243%201.png)

Y listo ya soy ROOT
