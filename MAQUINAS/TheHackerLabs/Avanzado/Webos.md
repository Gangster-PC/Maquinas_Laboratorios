Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240808160507.png)

Veamos lo que hay en el puerto 80:

![](../../../Images/Pasted%20image%2020240808160539.png)

Tenemos un panel de Login, probamos metiendo admin en ambos campos:

![](../../../Images/Pasted%20image%2020240808160823.png)

Y al parecer era una plantilla de login falso, ahora si accedimos a un panel de login verdadero de la página Grav

Usamos Rpcclient para ver los usuarios q tiene esta máquina:

![](../../../Images/Pasted%20image%2020240808162038.png)

Y encontramos q tiene el usuario "webos"

Con Smbclient encontramos una carpeta compartida la cual se llama webos

![](../../../Images/Pasted%20image%2020240808162118.png)

Intentamos acceder a ella con el usuario webos, anteriormente encontrado:

![](../../../Images/Pasted%20image%2020240808162146.png)

Pero nos pide la contraseña de este usuario la cual no tenemos todavia

Usamos fuerza bruta con Medusa para encontrar la contraseña de este usuario:

![](../../../Images/Pasted%20image%2020240808164121.png)

```
medusa -h 192.168.1.206 -u webos -P /usr/share/wordlists/rockyou.txt -M smbnt
```
Y nos encontró que la contraseña es:

![](../../../Images/Pasted%20image%2020240808164303.png)

Geraldine

Accedamos a la máquina por el puerto 445 con e stas credenciales para ver que nos encontramos:

![](../../../Images/Pasted%20image%2020240808164430.png)

Nos encontramos con un .txt

Lo pasamos a nuestro kali para leerlo:

![](../../../Images/Pasted%20image%2020240808164553.png)

Y tenemos un mensaje codificado en Brainfuck 

Lo decodificamos:

![](../../../Images/Pasted%20image%2020240808164647.png)

Y nos encontramos con unas credenciales, admin:Perico69*****

Las probamos en la página web:

![](../../../Images/Pasted%20image%2020240808164746.png)

![](../../../Images/Pasted%20image%2020240808164755.png)

Y, efectivamente logramos acceder a la página web

Como no logramos ver nada importante o un boton, haremos fuzzing web con Gobuster en busca de directorios:

![](../../../Images/Pasted%20image%2020240808165202.png)

Nos encuentra bastantes, pero el que nos interesa se llama /admin

Accedamos al directorio /admin:

![](../../../Images/Pasted%20image%2020240808165249.png)

Tenemos un panel de login nuevamente, probemos con las credenciales ya encontradas anteriormente:

![](../../../Images/Pasted%20image%2020240808165324.png)

![](../../../Images/Pasted%20image%2020240808165336.png)

Y ahora si logramos acceder satisfactoriamente a la pagina web de Grav

Obsevamos que tenemos la versión 1.7.44 de Grav

![](../../../Images/Pasted%20image%2020240808165645.png)

Buscaré un exploit en internet para poder ejecutarlo:

![](../../../Images/Pasted%20image%2020240808170117.png)

Encontré ese

Así que procedo a descargarlo en mi kali:

![](../../../Images/Pasted%20image%2020240808170156.png)

Pero antes de ejecutar el graver.py primero lo edito con mis credenciales de acceso:

![](../../../Images/Pasted%20image%2020240808170506.png)

Ahora si ejecuto el graver.py:

![](../../../Images/Pasted%20image%2020240808170544.png)

Vamos a la dirección que nos dice el exploit:

![](../../../Images/Pasted%20image%2020240808170612.png)

Y lo que hace el exploit es crearnos un LFI 

Vamos a la pestaña Páginas y despues a hacked:

![](../../../Images/Pasted%20image%2020240808171042.png)

![](../../../Images/Pasted%20image%2020240808171106.png)

Acá podemos poner un comando y si le damos al ojito al lado derecho de la página nos muestra el resultado

![](../../../Images/Pasted%20image%2020240808171134.png)

Ejemplo con el comando whoami:

![](../../../Images/Pasted%20image%2020240808171156.png)

Entonces le coloco una revshel, le doy al boton del ojito y me pongo a la escucha con Netcat:

![](../../../Images/Pasted%20image%2020240808171344.png)

![](../../../Images/Pasted%20image%2020240808171357.png)

Y recibo la conexión, ahora estoy dentro de la máquina

Tratamiento de la tty:

![](../../../Images/Pasted%20image%2020240808171442.png)

![](../../../Images/Pasted%20image%2020240808171509.png)

## ESCALADA DE PRIVILEGIOS

Revisando el directorio /opt tenemos una id_rsa

![](../../../Images/Pasted%20image%2020240808171839.png)

Me la paso a mi kali y le extraigo la hash con Ssh2john y la crackeo con John

![](../../../Images/Pasted%20image%2020240808171947.png)

![](../../../Images/Pasted%20image%2020240808172418.png)

Encontramos la contraseña del id_rsa

Buscamos que usuarios tenemos:

![](../../../Images/Pasted%20image%2020240808172510.png)

Así que usaremos esta id_rsa para entrar por el puerto SSH con el usuario webos y la contraseña freestyle

![](../../../Images/Pasted%20image%2020240808172642.png)

Y estamos dentro de la máquina nuevamente pero ahora desde el puerto 22

Flag de user:

![](../../../Images/Pasted%20image%2020240808172746.png)

Para ser usuarios root, observemos las capabilities que tiene la máquina

![](../../../Images/Pasted%20image%2020240808173331.png)

Y encontramos que tiene la capabilitie de Python3 descargada el la dirección /home/webos

Miramos en GTFObins como escalar con python

![](../../../Images/Pasted%20image%2020240808173105.png)

Lo ejecutamos:

![](../../../Images/Pasted%20image%2020240808173130.png)

Y listo, ya somos ROOT

Flag de root:

![](../../../Images/Pasted%20image%2020240808173156.png)

