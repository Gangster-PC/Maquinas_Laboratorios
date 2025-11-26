Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240816173205.png)

Veamos lo que hay en el puerto 80:

![](../../../Images/Pasted%20image%2020240816173245.png)

Damos en el botón admin:

![](../../../Images/Pasted%20image%2020240816173843.png)

Nos pide una contraseña la cual no tenemos

Entonces haremos fuzzing web con Gobuster para encontrar directorios:

![](../../../Images/Pasted%20image%2020240816174045.png)

Encontramos el directorio /api, accedamos a el:

![](../../../Images/Pasted%20image%2020240816174111.png)

Tenemos un .zip, lo descargamos y lo extraemos:

![](../../../Images/Pasted%20image%2020240816174148.png)

Nos pide una contraseña para poder extraerlo

Entonces extraeremos el hash del .zip con Zip2john y la crackearemos con John:

![](../../../Images/Pasted%20image%2020240816174416.png)

Lo extraemos ahora si con su contraseña babybaby:

![](../../../Images/Pasted%20image%2020240816174451.png)

Tenemos una contraseña, probemos si sirve en la pagina web:

![](../../../Images/Pasted%20image%2020240816174525.png)

Y listo, estamos dentro

Mirando la versión de la pagina web encontramos un exploit:

![](../../../Images/Pasted%20image%2020240816182342.png)

![](../../../Images/Pasted%20image%2020240816174613.png)

Y lo descargamos 

Ejecutamos el exploit:

![](../../../Images/Pasted%20image%2020240816175334.png)

Accedamos a la Webshell que nos creó el exploit:

![](../../../Images/Pasted%20image%2020240816175411.png)

Nos enviamos una Reverse Shell a nuestro kali:

![](../../../Images/Pasted%20image%2020240816175536.png)

![](../../../Images/Pasted%20image%2020240816175545.png)

Recibimos la conexión y ahora estamos dentro de la máquina

### Tratamiento de la TTY
```
script /dev/null -c bash 
Ctrl z
stty raw -echo; fg 
reset xterm 
export SHELL=bash
export TERM=xterm
```

![](../../../Images/Pasted%20image%2020240816175611.png)

![](../../../Images/Pasted%20image%2020240816175628.png)

## ESCALADA DE PRIVILEGIOS

En el directorio /opt nos encontramos con un .zip

![](../../../Images/Pasted%20image%2020240816175709.png)

Levantamos un servidor con python por el puerto 8000 y nos pasamos este .zip a nuestro kali para descomprimirlo:

![](../../../Images/Pasted%20image%2020240816175815.png)

![](../../../Images/Pasted%20image%2020240816175918.png)

Entonces extraeremos el hash del .zip con Zip2john y la crackearemos con John:

![](../../../Images/Pasted%20image%2020240816180011.png)

Lo descomprimimos con la contraseña cassandra:

![](../../../Images/Pasted%20image%2020240816180041.png)

Y tenemos un texto posiblemente codificado en base 58

Lo decodificamos:

![](../../../Images/Pasted%20image%2020240816180320.png)

Tenemos una posible contraseña

Intentamos ser el usuario sarxixa con esta contraseña pero nos dice que la contraseña es incorrecta

![](../../../Images/Pasted%20image%2020240816180631.png)

Pero fijándonos en el nombre del .zip el cuál es edropedropedrooo vemos que le falta al inicio una letra (P) por ende podemos intuir que lo mismo pasa con la contraseña, toca quitarle la primera letra "uepasaolvidona". Intentemos nuevamente cambiar a el usuario sarxixa:

![](../../../Images/Pasted%20image%2020240816180742.png)

Y listo, ahora somos el usuario sarxixa

Miramos a los grupos a los que pertenece este usuario:

![](../../../Images/Pasted%20image%2020240816180902.png)

Nos llama la atención que pertenece al grupo docker

Miramos en GTFObins como escalar con este grupo:

![](../../../Images/Pasted%20image%2020240816181001.png)

Lo ejecutamos:

![](../../../Images/Pasted%20image%2020240816181110.png)

Somos root, pero dentro del contenedor de docker

Entonces editamos el archivo /etc/passwd y borramos la X de root:

![](../../../Images/Pasted%20image%2020240816181158.png)

![](../../../Images/Pasted%20image%2020240816181243.png)

Y listo, ahora si somos usuarios ROOT directamente desde la máquina

Flag de user:

![](../../../Images/Pasted%20image%2020240816181331.png)

Flag de root:

![](../../../Images/Pasted%20image%2020240816181344.png)

