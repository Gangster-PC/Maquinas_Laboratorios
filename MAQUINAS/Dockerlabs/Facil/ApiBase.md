Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020251101213928.png)

Reviso el puerto http 5000 a ver qué está corriendo por allí:

![](../../../Images/Pasted%20image%2020251102115532.png)

Estoy ante una API Flask muy simple sin endpoint seleccionado, que me sugiere que en el directorio /users puedo realizar querys:

![](../../../Images/Pasted%20image%2020251102115942.png)

No encontré nada mas relevante lo único que se me ocurre es probar en esta página si existe una RFI (Remote File Inclusion), pero me falta el parámetro principal para ver si esta parte de la web es vulnerable a una inyección SQL, la encontraré con Wfuzz

Lo intenté pero no encontré nada, por ende usaré el sentido común y pasaré como parámetro "username", a ver qué me arroja la página

![](../../../Images/Pasted%20image%2020251102121328.png)

Ya me dice "user not found", entonces ese es el parámetro correcto, ahora si podré explotar la inyección SQL; abriré Burpsuite para poder tramitarla:

Activo el proxy para poder conectarme

![](../../../Images/Pasted%20image%2020251102122119.png)

Abro Burpsuite y envío la petición al Repeater:

![](../../../Images/Pasted%20image%2020251102122243.png)

Ahora en la primera línea le paso de parametro una ' (comilla), para ver si es vulnerable a la inyección SQL:

![](../../../Images/Pasted%20image%2020251102123008.png)

Efectivamente si es vulnerable, me doy cuenta por el mensaje que responde la página "HTTP 500 INTERNAL SERVER  ERROR"

Entonces ejecutaré la típica inyección SQL (pero como la estoy tramitando mediante el navegador web, los espacios los reemplazo por un "+"):
```
'or+1=1--+-
```

![](../../../Images/Pasted%20image%2020251102123240.png)

Ahora la página me responde con 2 credenciales, usuario "pingu" con contraseña "pinguinasio", las probaré en el puerto 22 SSH para ver si corresponden allí:

![](../../../Images/Pasted%20image%2020251102123509.png)

Efectivamente esas son las credenciales, ahora ya estoy dentro de la máquina
## ESCALADA DE PRIVILEGIOS

Buscando en el directorio /home, me encontré con un archivo .pcap, lo leo y en él encuentro:

![](../../../Images/Pasted%20image%2020251102123839.png)

Una contraseña llamada "balulero" y al parecer es del usuario root, procedo a escalar a root con esta contraseña:

![](../../../Images/Pasted%20image%2020251102123926.png)

Y listo, ya soy ROOT

