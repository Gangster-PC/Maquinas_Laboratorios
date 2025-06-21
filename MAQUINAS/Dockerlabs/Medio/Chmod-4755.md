Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240913125346.png)

Observando los puertos que tengo abiertos tengo el puerto 139 y 445 abierto, por ende usaré la herramienta de Enum4linux para ver los usuarios y recursos compartidos que maneja esta máquina:

![](../../../Images/Pasted%20image%2020240913130212.png)

![](../../../Images/Pasted%20image%2020240913130304.png)

![](../../../Images/Pasted%20image%2020240913130346.png)

Tengo de usuarios smbuser y rabol y de recursos compartidos share_secret_only

Así que les haré fuerza bruta a estos usuarios usando Msfconsole:

![](../../../Images/Pasted%20image%2020240913130628.png)

Y escribo run:

![](../../../Images/Pasted%20image%2020240913131107.png)

Y la contraseña del usuario smbuser es fuckit

Ahora si accedo al recurso compartido con Smbclient:

![](../../../Images/Pasted%20image%2020240913131230.png)

Leo qué contiene:

![](../../../Images/Pasted%20image%2020240913131304.png)

Y descargo el note.txt

Lo leo:

![](../../../Images/Pasted%20image%2020240913131325.png)

Dice "leer mejor"

Recordando esta máquina contiene otro usuario llamado Rabol, entonces haciendo caso a la nota que encontré de "read better", usando lógica intuyo que la contraseña es el nombre del recurso compartido es decir, share_secret_only

Intrusión por el puerto SSH:

![](../../../Images/Pasted%20image%2020240918204625.png)

Y listo ahora estoy dentro de la máquina por el puerto 22

Pero no puedo ejecutar ningun comando:

![](../../../Images/Pasted%20image%2020240918205005.png)

Porque debo escapar de rbash lo cual es un bash que está restringido y solo me permite ejecutar ciertos comandos que están permitidos, ya que me cambia el PATH a uno que tenga esos comandos permitidos y hace que no se pueda cambiar. Hay varias maneras de escapar dependiendo de los binarios que tenga permitidos, en este caso, al ejecutar `ls` observo que si funciona y que hay una carpeta bin, la cual es la que contiene los binarios, y si me fijo que tiene veo esto:

![](../../../Images/Pasted%20image%2020240918205416.png)

Tengo python instalado, por ende me lanzaré una bash:

![](../../../Images/Pasted%20image%2020240918205532.png)

Y ejecuto ese último comando para ya poder habilitar todos los comandos de la máquina

Flag de user:

![](../../../Images/Pasted%20image%2020240918205631.png)

## ESCALADA DE PRIVILEGIOS

Observo los binarios que contiene esta máquina:

![](../../../Images/Pasted%20image%2020240918205711.png)

Y me llama la atención el binario Curl

Entonces desde mi kali crearé un archivo txt con todo lo que cotiene el /etc/passwd pero con una condición que será borrar la "X" de root:

![](../../../Images/Pasted%20image%2020240922175919.png)

Ahora me levanto un servidor con python por el puerto 5555 y me paso este archivo que creé llamado passwd a la máquina, usando curl obviamente y lo ubico en la dirección reemplazando el /etc/passwd:

![](../../../Images/Pasted%20image%2020240922180306.png)

![](../../../Images/Pasted%20image%2020240922180317.png)

Recibió la petición

Ahora observo el cambio que tuvo root desde la máquina:

![](../../../Images/Pasted%20image%2020240922180359.png)

Ya no tiene la "X" root

Simplemente basta con escribir el comando "su root":

![](../../../Images/Pasted%20image%2020240922180429.png)

Y listo, ya soy ROOT

Flag de root:

![](../../../Images/Pasted%20image%2020240922180450.png)
