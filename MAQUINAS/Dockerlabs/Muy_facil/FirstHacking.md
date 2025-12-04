Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020240707202327.png)

Observo que la versión del puerto FTP es antigua entonces buscaré un exploit para entrar por ahí:

![](../../../Images/Pasted%20image%2020240707202814.png)

Y lo descargaré

Ahora lo ejecuto:

![](../../../Images/Pasted%20image%2020240707203025.png)

Y accedí a la máquina siendo ROOT
