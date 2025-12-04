Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020240708205229.png)

Como no tengo usuario ni contraseña ni nada, probaré hacer un ataque de fuerza bruta con Hydra pasándole como usuario "root" y tengo:

![](../../../Images/Pasted%20image%2020240708205905.png)

Accederé por el puerto 22 con estas credenciales:

![](../../../Images/Pasted%20image%2020240708205948.png)

Logré entrar y de una vez accedí siendo ROOT 
