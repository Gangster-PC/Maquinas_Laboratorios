Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020240707195702.png)

Observo lo que corre en el puerto 80 HTTP:

![](../../../Images/Pasted%20image%2020240707195737.png)

Tengo un panel de login de Login Web

Haré una facil inyección SQL para ver si tengo acceso:

![](../../../Images/Pasted%20image%2020240707200000.png)

Logré acceder y obtengo:

![](../../../Images/Pasted%20image%2020240707200026.png)

Un usuario llamado "dylan" con su respectiva contraseña

Accedo a la maquina por medio del puerto 22 SSH con esas credenciales:

![](../../../Images/Pasted%20image%2020240707200153.png)

Estoy dentro

## ESCALADA DE PRIVILEGIOS

Observo los binarios de la maquina:

![](../../../Images/Pasted%20image%2020240707200319.png)

Me llama la atención el binario Env

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020240707200418.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020240707200517.png)

Y listo, ya soy ROOT
