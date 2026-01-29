Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020250115104940.png)

Reviso lo que corre por el puerto 80:

![](../../../Images/Pasted%20image%2020250115105305.png)

Dando click en el botón naranja me encuentro con:

![](../../../Images/Pasted%20image%2020250115105554.png)

Son las credenciales del puerto SSH

Intento acceder por el puerto 22 SSH con estas credenciales:

![](../../../Images/Pasted%20image%2020250115105628.png)

Y listo, estoy dentro de la máquina

## ESCALADA DE PRIVILEGIOS

Buscaré copias de seguridad antiguas en toda la máquina
```
find  / *.bak  2>/dev/null
```

![](../../../Images/Pasted%20image%2020250115110401.png)

Y me llama la atención una de ellas, la "/secret.bak"

Lo leo:

![](../../../Images/Pasted%20image%2020250115110433.png)

Y son las credenciales del usuario balulito

Escalo a este usuario con su contraseña:

![](../../../Images/Pasted%20image%2020250115110505.png)

Y ahora soy el usuario balulito

Ejecuto sudo -l para ver el binario que puedo usar

![](../../../Images/Pasted%20image%2020250115110534.png)

Puedo ser root con el binario Cp

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020250115110645.png)

Hay 3 diferentes formas de escalar, pero yo usaré la ultima la C

Lo ejecuto:

![](../../../Images/Pasted%20image%2020250115110710.png)

Y listo, ya soy ROOT
