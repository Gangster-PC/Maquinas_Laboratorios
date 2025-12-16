Escaneo de puertos con Nmap
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020250115101105.png)

Tengo un único puerto abierto el puerto HTTP

Reviso lo que corre por el puerto 80:

![](../../../Images/Pasted%20image%2020250115101210.png)

Estoy ante un Drupal

Veré la versión a la cual me estoy enfrentando:

![](../../../Images/Pasted%20image%2020250115102542.png)

Es un Drupal versión 8

Buscaré en Metasploit un exploit a esta versión:

![](../../../Images/Pasted%20image%2020250115102716.png)

Y existe ese

Lo selecciono y leo los requerimientos que me pide:

![](../../../Images/Pasted%20image%2020250115102815.png)

Y relleno el espacio de RHOSTS

![](../../../Images/Pasted%20image%2020250115102855.png)

Y ejecuto el exploit:

![](../../../Images/Pasted%20image%2020250115102950.png)

Y listo, estoy dentro de la máquina

## ESCALADA DE PRIVILEGIOS

Observo los usuarios que contiene esta máquina:

![](../../../Images/Pasted%20image%2020250115103133.png)

Son 2 "ballenita" y root

Buscaré donde está ubicado el archivo settings.php para ver si en él tendré alguna pista:
```
find / -name settings.php  2>/dev/null 
```

![](../../../Images/Pasted%20image%2020250115103334.png)

Lo leo:

![](../../../Images/Pasted%20image%2020250115103451.png)

Tengo la contraseña del usuario "ballenita"

Escalo al usuario "ballenita":

![](../../../Images/Pasted%20image%2020250115103651.png)

Ahora soy el usuario "ballenita"

Ejecuto sudo -l para ver el binario que puedo usar 

![](../../../Images/Pasted%20image%2020250115103733.png)

Puedo ser root con el binario Grep y Ls

Con el binario Ls, veré lo que contiene la carpeta /root:

![](../../../Images/Pasted%20image%2020250115104020.png)

Miro en GTFOBins como puedo escalar con el binario Grep:

![](../../../Images/Pasted%20image%2020250115103803.png)

Lo ejecuto para leer ese .txt:

![](../../../Images/Pasted%20image%2020250115104148.png)

Y puede que sea la contraseña de root

Intento escalar a root con esta contraseña encontrada:

![](../../../Images/Pasted%20image%2020250115104221.png)

Y listo, ya soy ROOT

