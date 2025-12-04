Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020241021132004.png)

Usando la herramienta Enum4linux enumeraré usuarios del puerto 445:

![](../../../Images/Pasted%20image%2020241022092442.png)

Tengo 5 posibles usuarios

Empezaré por hacerle fuerza bruta hacia el puerto SMB 445 al usuario "satriani17" para encontrar su contraseña, con la herramienta Netexec:
```
netexec smb 172.17.0.2 -u satriani7 -p /usr/share/wordlists/rockyou.txt --ignore-pw-decoding
```

![](../../../Images/Pasted%20image%2020241022092648.png)

![](../../../Images/Pasted%20image%2020241022092659.png)

Y la contraseña del usuario "satriani7" es "50cent"

Con ayuda de la herramienta Smbclient enumeraré carpetas compartidas:

![](../../../Images/Pasted%20image%2020241022090007.png)

Y me llama la atención la carpeta "backup24", accederé a esta:

![](../../../Images/Pasted%20image%2020241022092838.png)

Y estoy dentro de la carpeta compartida por medio del protocolo Smb

En la carpeta /Documents/Personal tengo 2 .txt los descargo a mi Kali y los leo:

![](../../../Images/Pasted%20image%2020241022093039.png)

Leo el credentials.txt:

![](../../../Images/Pasted%20image%2020241022093147.png)

Y obtengo las credenciales del usuario Administrador

Ahora intento acceder a el otro recurso compartido llamado home con estas credenciales:

![](../../../Images/Pasted%20image%2020241022094110.png)

Estoy dentro, y al parecer son los archivos que corren por el puerto 80 en la página web, así que procederé a crearme una Reverse Shell con Msfvenom para acceder a la máquina y la subo a la carpeta compartida con el comando "put (nombre del archivo)":

![](../../../Images/Pasted%20image%2020241022094454.png)

Ahora me pongo a la escucha con Netcat por el puerto 443 y accedo a mi .php desde la página web:

![](../../../Images/Pasted%20image%2020241022094621.png)

![](../../../Images/Pasted%20image%2020241022094629.png)

Y listo recibo la conexión, estoy dentro de la máquina

### Tratamiento de la TTY
```
script /dev/null -c bash 
Ctrl z
stty raw -echo; fg 
reset xterm 
export SHELL=bash
export TERM=xterm
```

![](../../../Images/Pasted%20image%2020241022095059.png)

![](../../../Images/Pasted%20image%2020241022095122.png)

## ESCALADA DE PRIVILEGIOS

Ejecuto sudo -l para ver el binario que puedo usar

![](../../../Images/Pasted%20image%2020241022095224.png)

Puedo usar el binario service para ser root

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020241022095517.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020241022095503.png)

Y listo, ya soy ROOT
