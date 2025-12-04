Escaneo  de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020240705201619%201.png)

En el puerto 80 tengo:

![](../../../Images/Pasted%20image%2020240705201708%201.png)

Una subida de archivos

Procedo a subir un archivo .php con una reverse shell hacia mi maquina Linux

![](../../../Images/Pasted%20image%2020240705201856%201.png)

Ahora hago fuzzing web con Gobuster para encontrar el archivo que acabo de subir:
```
gobuster dir -u http://172.17.0.2/ -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -x html,php,txt
```

![](../../../Images/Pasted%20image%2020240705202001.png)

Lo encuentro y está ubicado en /uploads

![](../../../Images/Pasted%20image%2020240705202042.png)

Me pongo a la escucha con netcat por el puerto 443 y doy click en el archivo shell.php y recibo la shell reversa

![](../../../Images/Pasted%20image%2020240705202216.png)

### Tratamiento de la TTY
```
script /dev/null -c bash 
Ctrl z
stty raw -echo; fg 
reset xterm 
export SHELL=bash
export TERM=xterm
```

![](../../../Images/Pasted%20image%2020240705202527.png)
![](../../../Images/Pasted%20image%2020240705202604.png)

## ESCALADA DE PRIVILEGIOS

Ejecuto sudo -l para ver el binario que podré usar 

![](../../../Images/Pasted%20image%2020240705202648.png)

Puedo ser root con el binario env

Miro en GTFOBins cómo puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020240705203427.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020240705202830.png)

Y listo, ya soy usuario ROOT 

