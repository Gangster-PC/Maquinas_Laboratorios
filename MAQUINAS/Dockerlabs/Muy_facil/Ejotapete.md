Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020251020215009.png)

Revisaré lo que corre por el único puerto abierto, que es el puerto 80:

![](../../../Images/Pasted%20image%2020251020215159.png)

Dice que no tengo permiso para acceder a este directorio

Sin tener mas pistas que me pueda ofrecer la página web, procederé a usar Gobuster para realizar una búsqueda de directorios:
```
gobuster dir -u http://172.17.0.2 -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -x html,php,txt
```

![](../../../Images/Pasted%20image%2020251021172455.png)

Encontré el directorio /drupal, procederé a acceder a él:

![](../../../Images/Pasted%20image%2020251022103153.png)

Y efectivamente estoy ante un Drupal 

Procederé a abrir Msfconsole para encontrar un posible exploit que me permita acceder a este Drupal aún sabiendo que no tengo la versión de él

![](../../../Images/Pasted%20image%2020251022103500.png)

Como no tengo una versión para buscar específicamente, procederé a probar el exploit numero 1

![](../../../Images/Pasted%20image%2020251022103605.png)

Con el comando "info", veré los requisitos que me pide el exploit para saber donde atacar y así mismo completarlos:

![](../../../Images/Pasted%20image%2020251022103732.png)

Me pide el RHOSTS que es la dirección ip donde se encuentra el Drupal, también me solicita el RPORT que en su defecto se encuentra la página web subida en el puerto 80 y por último me pide el TARGETURI que quiere decir en qué directorio se encuentra el panel de login; procedo a llenar todos estos datos solicitados para que funcione el exploit:

![](../../../Images/Pasted%20image%2020251022103953.png)

Rectifico que todos los campos hayan sido llenados correctamente:

![](../../../Images/Pasted%20image%2020251022104012.png)

Y escribo el comando "run" para que se ejecute el exploit solo:

![](../../../Images/Pasted%20image%2020251022104209.png)

Ya estoy dentro de la máquina y lo compruebo escribiendo un comando como por ejemplo "ls" y efectivamente la máquina me responde listando los elementos correspondientes de mi actual directorio

## ESCALADA DE PRIVILEGIOS

Ejecuto el comando "shell", para que me permita colocar comandos que me permitan comunicarme con la máquina ya que una sesión de "meterpreter" no me deja

![](../../../Images/Pasted%20image%2020251022110021.png)

Miro los binarios que contiene la máquina:  
```
find / -perm -4000 -ls 2>/dev/null
```

![](../../../Images/Pasted%20image%2020251022110514.png)

Me llama la atención el binario Find

Ahora procederé a mirar en GTFOBins como puedo escalar con este binario en una SUID:

![](../../../Images/Pasted%20image%2020251022110859.png)

Lo ejecuto tal cual:

```
/usr/bin/find . -exec /bin/bash -p \; -quit
```

![](../../../Images/Pasted%20image%2020251022111835.png)

Y listo, ya soy ROOT

