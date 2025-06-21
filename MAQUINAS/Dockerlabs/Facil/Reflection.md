Escaneo de puertos con Nmap:
```
 nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020250115140522.png)

Reviso lo que corre por el puerto 80:

![](../../../Images/Pasted%20image%2020250115140556.png)

Dando click en el botón naranja me encuentro con:

![](../../../Images/Pasted%20image%2020250116184024.png)

Son las credenciales del puerto SSH

Intento acceder por el puerto 22 SSH con estas credenciales:

![](../../../Images/Pasted%20image%2020250116184101.png)

Y listo, estoy dentro de la máquina

## ESCALADA DE PRIVILEGIOS

Busco los binarios que contiene esta máquina:
```
find / -perm -4000 2>/dev/null
```

![](../../../Images/Pasted%20image%2020250116184419.png)

Y me llama la atención el binario Env

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020250116184317.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020250116184451.png)

Y listo, ya soy ROOT

