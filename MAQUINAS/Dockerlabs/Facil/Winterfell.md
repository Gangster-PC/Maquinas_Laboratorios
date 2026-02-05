Escaneo de puertos con Nmap:

![](../../../Images/Pasted%20image%2020240722183638.png)

Veo que es una máquina Windows debido a que tiene los puertos 139 y 445 abiertos, Samba

Observo lo que hay en el puerto 80:

![](../../../Images/Pasted%20image%2020240722183802.png)

Tengo unos posibles usuarios o contraseñas

Haré fuzzing web con Gobuster para encontrar posibles directorios:

![](../../../Images/Pasted%20image%2020240722183921.png)

Tengo un /dragon, veo lo que hay en el:

![](../../../Images/Pasted%20image%2020240722183957.png)

Un archivo "EpisodiosT1", que contiene:

![](../../../Images/Pasted%20image%2020240722184033.png)

Más posibles usuarios o contraseñas

Meteré todos los posibles usuarios o contraseñas encontrados en las anteriores 2 paginás en un .txt:

![](../../../Images/Pasted%20image%2020240722184143.png)

Ahora con este archivo de texto haré fuerza bruta con la herramienta Crackmapexec para entrar por el puerto 445 y leer posibles carpetas:

![](../../../Images/Pasted%20image%2020240722184309.png)

Encuentro el usuario jon con su contraseña seacercaelinvierno

Buscaré carpetas accesibles dentro de este recurso compartido, con Smbclient:

![](../../../Images/Pasted%20image%2020240722184429.png)

Y encuentro la carpeta shared

Entraré a la maquina con las credenciales encontradas anteriormente y veré lo que hay en este directorio:

![](../../../Images/Pasted%20image%2020240722184533.png)

Tengo un "proteccion_del_reino"

Me lo descargo para ver que contiene:

![](../../../Images/Pasted%20image%2020240722184612.png)

Y lo leo desde mi máquina:

![](../../../Images/Pasted%20image%2020240722184638.png)

Tengo un mensaje de Jon hacia Aria dándole una contraseña pero en base 64

![](../../../Images/Pasted%20image%2020240722184819.png)

Y al decodificarlo tengo "hijodelanister"

Haré la intrusión con el usuario "jon" y la contraseña "hijodelanister", por el puerto 22:

![](../../../Images/Pasted%20image%2020240722184920.png)

Y estoy dentro de la máquina ahora si

## ESCALADA DE PRIVILEGIOS

Ejecuto sudo -l para ver el binario que podré usar

![](../../../Images/Pasted%20image%2020240722184957.png)

Veo que puedo ser el usuario "Aria" ejecutando un script en python llamado .mensaje.py ubicado en el directorio de jon

Observo lo que contiene el .mensaje.py

![](../../../Images/Pasted%20image%2020240722185139.png)

Es un script que encripta un mensaje introducido por el usuario, pero también se puede observar que usa una librería llamada Hashlib, la cual tiene una vulnerabilidad

Creo un archivo llamado hashlib.py con una bash:

![](../../../Images/Pasted%20image%2020240722185402.png)

Ahora hago que el script de .mensaje.py lea mi archivo malicioso para así poder escalar al usuario "Aria":

![](../../../Images/Pasted%20image%2020240722185519.png)

Y listo ya soy "aria"

Ejecuto sudo -l para ver el binario que podré usar

![](../../../Images/Pasted%20image%2020240722185613.png)

Observo que con el usuario daenerys puedo leer y listar archivos

Pero también observo que en el directorio de daenerys tengo un archivo:

![](../../../Images/Pasted%20image%2020240722185737.png)

Procedo a leerlo para ver que dice:

![](../../../Images/Pasted%20image%2020240722185814.png)

Tengo una posible contraseña que sería "drakaris"

Ahora intentaré ser el usuario daenerys con la contraseña "drakaris"

![](../../../Images/Pasted%20image%2020240722185913.png)

Listo ya soy "daenerys"

Ejecuto sudo -l para ver el binario que podré usar

![](../../../Images/Pasted%20image%2020240722190017.png)

Puedo ser usuario root con el binario bash leyendo un archivo .shell.sh

Así que lo ejecutaré:

![](../../../Images/Pasted%20image%2020240722190145.png)

Leeré ahora los binarios de la máquina y me encuentro con el binario /bin/bash

![](../../../Images/Pasted%20image%2020240722190222.png)

Ejecutaré el comando bash -p:

![](../../../Images/Pasted%20image%2020240722190249.png)

Y listo, ya soy ROOT
