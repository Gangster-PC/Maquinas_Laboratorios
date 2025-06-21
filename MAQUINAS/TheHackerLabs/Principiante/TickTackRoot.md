Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 192.168.1.47 -oN escaneo
```

![](../../../Images/Pasted%20image%2020250212124123.png)

Observo lo que corre por el puerto 80:

![](../../../Images/Pasted%20image%2020250212124216.png)

Me encuentro con una plantilla de Apache, nada relevante

Observando el escaneo me doy cuenta que tengo acceso por el puerto FTP con el usuario y cotnraseña anonymous

![](../../../Images/Pasted%20image%2020250212124538.png)

Veo que contiene este puerto:

![](../../../Images/Pasted%20image%2020250212124743.png)

Me descargaré ambos archivos, pero al momento de hacerlo me doy cuenta que hay un firewall impidiendo la descarga "Passive mode"

![](../../../Images/Pasted%20image%2020250212124830.png)

Lo desconecto con "passive off":

![](../../../Images/Pasted%20image%2020250212124953.png)

Ya descargué ambos archivos en mi kali ahora los leeré:

![](../../../Images/Pasted%20image%2020250212125040.png)

Tengo 2 posibles usuarios

Pero me llamó la atención un tercer usuario que aparece en el momento cuando ingreso por el puerto 21 llamado Robin:

![](../../../Images/Pasted%20image%2020250212125726.png)

Lanzaré un ataque de fuerza bruta por el puerto SSH para encontrar la posible contraseña de este usuario con ayuda de la herramienta Hydra:

![](../../../Images/Pasted%20image%2020250212125753.png)

La contraseña es babyblue

Intrusión por el puerto 22 SSH con estas credenciales:

![](../../../Images/Pasted%20image%2020250212125850.png)

Y estoy dentro de la máquina

Flag de user

![](../../../Images/Pasted%20image%2020250212125912.png)

## ESCALADA DE PRIVILEGIOS

Ejecuto sudo -l para ver el binario que puedo usar 

![](../../../Images/Pasted%20image%2020250212130116.png)

Puedo ser root con el binario Timeout

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020250212130319.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020250212130347.png)

Y listo, soy ROOT

Flag de root:

![](../../../Images/Pasted%20image%2020250212130403.png)

