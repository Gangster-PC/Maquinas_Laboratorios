Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020251112161038.png)

Revisaré el puerto HTTP 80, para ver qué está corriendo por allí:

![](../../../Images/Pasted%20image%2020251112161127.png)

Me encuentro con un panel de Login web

Trataré de ver si es vulnerable este panel de Login a una inyección SQL, pasando el siguiente parámetro como usuario y como contraseña:
```
'1'='1 -- -
```

![](../../../Images/Pasted%20image%2020251112161946.png)

![](../../../Images/Pasted%20image%2020251112161958.png)

Y efectivamente si es vulnerable y me dejó acceder

Leyendo el código fuente de esta página me encontré con:

![](../../../Images/Pasted%20image%2020251112162052.png)

En la línea 2 me dice una pista de un posible archivo "logs.txt"

Lo buscaré, y para ello empezaré realizando fuzzing web con ayuda de Gobuster para encontrar directorios:
```
gobuster dir -u http://172.17.0.2/ -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -x html,php,txt
```

![](../../../Images/Pasted%20image%2020251112162221.png)

Existe un directorio llamado /logs, pero al momento de acceder a él me lanza un código 403 la página es decir está restringido

Entonces le pasaré como parámetro "page" al directorio /logs/logs.txt de la página donde si me dejó acceder:

![](../../../Images/Pasted%20image%2020251112162527.png)

Y ahora si pude leer el archivo logs.txt

Leyendo el archivo me encuentro que existe un usuario llamado "albert" con su respectiva contraseña "NGxiM3J0MTIz"; Probaré estas credenciales en el puerto 22 SSH:

![](../../../Images/Pasted%20image%2020251112162739.png)

Y efectivamente son credenciales correctas y ahora estoy dentro de la máquina

## ESCALADA DE PRIVILEGIOS

Reviso los procesos que se están ejecutando con el comando:
```
ps aux
```

![](../../../Images/Pasted%20image%2020251118205746.png)

Me llama la atención el proceso que está ejecutando el usuario "conx" trata de un archivo socket UNIX en escucha que, al conectarse, ejecuta el binario `/bin/bash` con este usuario.
Se puede realizar una conexión a la shell /bin/bash a través del socket, ejecutando:
```
socat - UNIX-CONNECT:/home/conx/.cache/.sockv
```

![](../../../Images/Pasted%20image%2020251118210558.png)

Y ahora me convertí en el usuario "conx"

Si listo los `crontabs` que hay en la máquina me encontraré con:
```
ls -la /etc/cron.d
```

![](../../../Images/Pasted%20image%2020251118211503.png)

Observo que hay uno distinto de lo normal llamado "backup-cron" y no es creado por defecto, lo leeré:

![](../../../Images/Pasted%20image%2020251118211627.png)

Se evidencia que ejecuta un .sh 

Por ende trataré de modificar ese archivo backup.sh para darle permisos SUID a la bash:
```
echo "chmod u+s /bin/bash" >>  /var/backups/backup.sh
```

![](../../../Images/Pasted%20image%2020251118212011.png)

Esperaré unos segundos y ahora ejecutaré el comando:
```
bash -p
```

![](../../../Images/Pasted%20image%2020251118212109.png)

Y listo, ya soy ROOT

