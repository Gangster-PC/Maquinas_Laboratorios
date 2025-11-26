Escaneo  de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020251125172303.png)

Reviso el puerto 80 HTTP:

![](../../../Images/Pasted%20image%2020251125172417.png)

Al momento de tratar de cargar la página de una me salta un panel de Login Web, intento ver si es vulnerable a una posible inyección SQL, pero no lo es

Entonces me resta lanzar un ataque de fuerza bruta hacía este panel de login con Hydra, de la forma/tipo admin:admin (usuario y contraseña seguidos separados por 2 puntos ":")
```
hydra -C /usr/share/seclists/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt http-get://172.17.0.2 -t 64
```

![](../../../Images/Pasted%20image%2020251125204745.png)

Y las credenciales usuario y contraseña de este panel son "httpadmin" y "fhttpadmin"

Me loguearé con estas credenciales:

![](../../../Images/Pasted%20image%2020251125204939.png)

!

Y una vez accedido, me encuentro ante una plantilla de Apache

Pero no encuentro nada relevante, así que procederé a realizar fuzzing web en busca de posibles directorios usando Gobuster pasándole las credenciales anteriormente encontradas, con -U para el usuario y con -P para la contraseña:
```
gobuster dir -u http://172.17.0.2 -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -x html,php,txt,log -U httpadmin -P fhttpadmin
```

![](../../../Images/Pasted%20image%2020251125205547.png)

Revisaré el directorio /login.php:

![](../../../Images/Pasted%20image%2020251125205721.png)

Nuevamente estoy ante otro panel de Login Web, usaré un script que creó [Dise0](https://dise0.gitbook.io/h4cker_b00k/ctf/dockerlabs/influencerhate-dockerlabs-easy#:~:text=Vamos%20a%20crearnos,exit%201), para poder realizar nuevamente un ataque de fuerza bruta:

![](../../../Images/Pasted%20image%2020251125210401.png)

Le pasaré por ejemplo de usuario "admin" para ver cuál es la posible contraseña:

![](../../../Images/Pasted%20image%2020251125210615.png)

Y la contraseña de "admin" es "chocolate"

Login con esas credenciales:

![](../../../Images/Pasted%20image%2020251125210651.png)

Una vez accedí, me dan una pista de un posible usuario llamado "balutin"

Procederé a realizar un ataque de fuerza bruta nuevamente con Hydra, pero esta vez apuntando hacia el puerto 22 SSH para ver si este usuario corresponde allí y de ser así, encontrar su contraseña:
```
hydra -l balutin -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2 -t 64  
```

![](../../../Images/Pasted%20image%2020251125210851.png)

Efectivamente este usuario pertenece aquí y su respectiva contraseña es "estrella"

Intrusión por el puerto 22 SSH con estas credenciales:

![](../../../Images/Pasted%20image%2020251125211003.png)

Y listo, estoy dentro de la máquina

## ESCALADA DE PRIVILEGIOS

Para poder escalar a root, la única forma es realizando un ataque de fuerza bruta para eso me descargaré la herramienta de mi amigo Malfer de su repositorio de GitHub, la enviaré ubicándome en mi Kali y usaré la herramienta SCP:
```
scp /media/kali/root/home/kali/Desktop/Linux-Su-Force.sh balutin@172.17.0.2:/tmp/
```

![](../../../Images/Pasted%20image%2020251125212812.png)

Y también me pasaré el diccionario rockyou.txt:
```
scp /media/kali/root/home/kali/Desktop/rockypu.txt balutin@172.17.0.2:/tmp/
```

![](../../../Images/Pasted%20image%2020251125212932.png)

Le doy permisos de ejecución al archivo en la máquina y ejecuto la herramienta pasándole como usuario "root" y como diccionario el "rockyou.txt":

![](../../../Images/Pasted%20image%2020251125213033.png)

![](../../../Images/Pasted%20image%2020251125213040.png)

Escalo a root con la contraseña "rockyou":

![](../../../Images/Pasted%20image%2020251125213122.png)

Y listo, ya soy ROOT
