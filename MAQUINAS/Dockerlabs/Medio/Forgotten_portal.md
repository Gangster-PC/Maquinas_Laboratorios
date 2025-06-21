Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020250129180545.png)

Observo lo que corre por el puerto 80:

![](../../../Images/Pasted%20image%2020250129181050.png)

Revisando el código fuente  de esta página, en la línea 48 me encuentro con:

![](../../../Images/Pasted%20image%2020250129181227.png)

Un posible nombre de un directorio

Accedo a él:

![](../../../Images/Pasted%20image%2020250129181256.png)

Tengo una subida de archivos:

Me crearé una Reverse Shell con Msfvenom y la subiré:

![](../../../Images/Pasted%20image%2020250129181428.png)

![](../../../Images/Pasted%20image%2020250129181417.png)

Me pongo a la escucha con Netcat y doy click en el botón azul "Haz clic aquí para ver el archivo"

![](../../../Images/Pasted%20image%2020250129182503.png)

Recibo la conexión y ahora estoy dentro de la máquina

Tratamiento de la TTY:

![](../../../Images/Pasted%20image%2020250129182701.png)

![](../../../Images/Pasted%20image%2020250129182727.png)

## ESCALADA DE PRIVILEGIOS

Leyendo el archivo access_log ubicado en /var/www/html, me encuentro con una pista:

![](../../../Images/Pasted%20image%2020250129183559.png)

Decodificaré ese código:

![](../../../Images/Pasted%20image%2020250129183640.png)

Encontré las credenciales de un usuario

Escalo a alice:

![](../../../Images/Pasted%20image%2020250129183715.png)

Ahora soy el usuario alice

Leyendo el archivo report en la ubicación /home/alice/incidents, dice que un script generó la misma clave id_rsa para todos los usuarios de la máquina y que la clave de este id_rsa es cyber_security:

![](../../../Images/Pasted%20image%2020250129184711.png)

Así que procedo a pasarme el id_rsa de /home/alice/.ssh/id_rsa a mi kali:

![](../../../Images/Pasted%20image%2020250129185159.png)

Le doy permisos y accedo con el usuario bob y esta id_rsa

![](../../../Images/Pasted%20image%2020250129185250.png)

Accedí nuevamente a la máquina pero ahora soy el usuario bob

Ejecuto sudo -l para ver el binario que puedo usar

![](../../../Images/Pasted%20image%2020250129190931.png)

Puedo ser root con el binario Bash

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020250129191003.png)

Lo ejecuto tal cual:

![](../../../Images/Pasted%20image%2020250129191022.png)

Y listo, ya soy ROOT

Flag de user:

![](../../../Images/Pasted%20image%2020250129191117.png)

Flag de root:

![](../../../Images/Pasted%20image%2020250129191149.png)
