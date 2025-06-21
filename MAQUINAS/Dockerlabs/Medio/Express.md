Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020250203170306.png)

Observo lo que corre por el puerto 80:

![](../../../Images/Pasted%20image%2020250203170346.png)

Investigando mas a fondo en esta página web, encuentro que está ligada al dominio express.dl, procedo a hacerle fuzzing web con Gobuster en busqueda de directorios:

![](../../../Images/Pasted%20image%2020250203171320.png)

Accedo al directorio /binary:

![](../../../Images/Pasted%20image%2020250203171350.png)

Me encuentro con un archivo, lo descargo y lo ejecuto:

![](../../../Images/Pasted%20image%2020250203171453.png)

Efectivamente es un juego

Usaré ingeniería inversa con Strings para leer solo palabras dentro del binario a ver si encuentro una pista:

![](../../../Images/Pasted%20image%2020250203171718.png)

Y me encuentro con una posible contraseña que es "P@ssw0rd!#--025163fhusGNFE"

Usaré Hydra para hacer un ataque de fuerza bruta al puerto SSH con esta contraseña para encontrar el usuario:

![](../../../Images/Pasted%20image%2020250203171825.png)

El usuario de la contraseña es admin

Intrusión por el puerto 22 SSH:

![](../../../Images/Pasted%20image%2020250203171909.png)

Y listo, estoy dentro de la máquina

## ESCALADA DE PRIVILEGIOS

Ejecuto sudo -l para ver el binario que puedo usar

![](../../../Images/Pasted%20image%2020250203171935.png)

Puedo ser root ejecutando un archivo .py ubicado en /opt con el binario Python

Para poder escalar con este script.py, explotaré un Python Library Hijacking, buscando un archivo .py con el nombre de la librería que se está importando, en este caso es "pytest"

![](../../../Images/Pasted%20image%2020250203172316.png)

Busco donde está ubicado el pytest.py para editarlo y le escribo una bash para que la ejecute con privilegios:

![](../../../Images/Pasted%20image%2020250203172705.png)

![](../../../Images/Pasted%20image%2020250203172934.png)

Ejecuto el archivo nuevamente con python:

![](../../../Images/Pasted%20image%2020250203172953.png)

Y listo, ya soy ROOT

Flag de user:

![](../../../Images/Pasted%20image%2020250203173045.png)

Flag de root:
<<<<<<< HEAD

![](../../../Images/Pasted%20image%2020250203173018.png)
=======
>>>>>>> 5ba57b1 (Subida de archivos editada con Python para corregir formato de imagenes)

![](../../../Images/Pasted%20image%2020250203173018.png)

