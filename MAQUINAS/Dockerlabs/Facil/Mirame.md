Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020240813092116.png)

Observo lo que corre por el puerto 80:

![](../../../Images/Pasted%20image%2020240813092133.png)

Tengo un panel de login web, probaré una inyección Sql insertando una comilla " ' " a ver si me da resultado:

![](../../../Images/Pasted%20image%2020240813092241.png)

![](../../../Images/Pasted%20image%2020240813092220.png)

Efectivamente este panel de login es vulnerable a inyecciones Sql, entonces me aprovecharé de ello y usaré la herramienta Sqlmap para explotar esta vulnerabilidad:

1. Enumero posibles bases de datos
```
sqlmap --url http://172.17.0.2/index.php --dbs --batch --forms
```

![](../../../Images/Pasted%20image%2020240813093627.png)

![](../../../Images/Pasted%20image%2020240813093707.png)

Usaré la base de datos "Users"

2. Enumero posibles tablas, de esta base de datos:
```
sqlmap --url http://172.17.0.2/index.php -D users --tables --batch --forms
```

![](../../../Images/Pasted%20image%2020240813093813.png)

![](../../../Images/Pasted%20image%2020240813093822.png)

Obtengo la tabla llamada "Usuarios"

3. Enumeré posibles columnas de esta tabla:
```
sqlmap --url http://172.17.0.2/index.php -D users -T usuarios --columns --batch --forms
```

![](../../../Images/Pasted%20image%2020240813093851.png)

![](../../../Images/Pasted%20image%2020240813093906.png)

Tengo 3 columnas "id", "password" y "username"

4. Y por último, enumeraré usuarios de estas 3 columnas:
```
sqlmap --url http://172.17.0.2/index.php -D users -T usuarios -C id,password,username --batch --forms
```

![](../../../Images/Pasted%20image%2020240813093942.png)

![](../../../Images/Pasted%20image%2020240813093951.png)

Tengo un posible nombre de directorio llamado "directoriotravieso", así que intentaré acceder a él a ver si existe:

![](../../../Images/Pasted%20image%2020240813094049.png)

Efectivamente existe y contiene una imagen

Me la descargo y le haré esteganografía con Stegcracker:

![](../../../Images/Pasted%20image%2020240813094230.png)

Y la contraseña de la imagen para poder extraer sus archivos ocultos es "chocolate"

La extraigo con Steghide:

![](../../../Images/Pasted%20image%2020240813094325.png)

Me extrae un .zip

Le extraigo el hash con Zip2john y lo crackeo con la herramienta John:

![](../../../Images/Pasted%20image%2020240813094413.png)

Extraigo el .zip con la contraseña ya encontrada:

![](../../../Images/Pasted%20image%2020240813094441.png)

Me deja un secret.txt

Lo leo:

![](../../../Images/Pasted%20image%2020240813094459.png)

Tengo 2 credenciales

Intrusión por el puerto 22 SSH con estas credenciales:

![](../../../Images/Pasted%20image%2020240813094547.png)

Y listo, estoy dentro de la máquina

## ESCALADA DE PRIVILEGIOS

Observo todos los binarios que contiene la máquina:

![](../../../Images/Pasted%20image%2020240813094802.png)

Y me doy cuenta que uno de ellos no es comun, es el binario Find

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020240813094836.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020240813094850.png)

Y listo, ya soy el usuario ROOT
