Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020240827103012.png)

Tengo abierto el puerto 139 y el puerto 445, por ende veré los recursos compartidos que tiene esta máquina:
```
smbclient -L //172.17.0.2 -N
```

![](../../../Images/Pasted%20image%2020240827103132.png)

Tengo el recurso compartido de "macarena"

Trateré de acceder a este recurso compartido:

![](../../../Images/Pasted%20image%2020240827103251.png)

Pero me pide una contraseña la cual no tengo.

Entonces haré fuerza bruta con Msfconsole al usuario macarena para encontrar su contraseña:

![](../../../Images/Pasted%20image%2020240827103753.png)

Y le colocaré los datos que hacen falta:

![](../../../Images/Pasted%20image%2020240827104107.png)

Y escribo run

![](../../../Images/Pasted%20image%2020240827104230.png)

Y la contraseña para el usuario macarena es donald

Leeré los permisos de los recursos compartidos a los que pertenece macarena:
```
smbmap -H 172.17.0.2 -u macarena -p donald
```

![](../../../Images/Pasted%20image%2020240827104308.png)

Puedo leer y escribir en el recurso compartido macarena

Accedo a este recurso:
```
smbclient //172.17.0.2/macarena -U macarena
```

![](../../../Images/Pasted%20image%2020240827104652.png)

Y estoy ahora dentro del recurso compartido

Entonces ahora crearé unas claves publicas y privadas ssh para poder acceder por el puerto 22:
1. Creo un directorio llamado .ssh:

![](../../../Images/Pasted%20image%2020240827105307.png)

2. Creo claves publicas y privadas:
```
ssh-keygen -t rsa -b 4096
```

![](../../../Images/Pasted%20image%2020240827105748.png)

Y las pego en la carpeta actual:

![](../../../Images/Pasted%20image%2020240827105942.png)

3. Ahora subo el id_rsa.pub y el authorized_keys al recurso compartido:

![](../../../Images/Pasted%20image%2020240827110040.png)

Intrusión por el puerto 22 SSH con el usuario macarena y la id_rsa:

![](../../../Images/Pasted%20image%2020240827110226.png)

Estoy dentro de la máquina

Flag de user:

![](../../../Images/Pasted%20image%2020240827110248.png)

## ESCALADA DE PRIVILEGIOS

Tengo una hash en el directorio /home/secret/hash:

![](../../../Images/Pasted%20image%2020240827110439.png)

![](../../../Images/Pasted%20image%2020240827110505.png)

Y la decodifico y al parecer es una contraseña

Para ejecutar el comando -l me pide una contraseña, pruebo a ver si es esta la contraseña la que decodifiqué del hash

![](../../../Images/Pasted%20image%2020240827110810.png)

Y efectivamente es.

Para poder ser root puedo usar el binario File

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020240827110924.png)

En el directorio /opt me encuentro un password.txt así que leeré este archivo con este binario:

![](../../../Images/Pasted%20image%2020240827111015.png)

Y listo, ya soy ROOT

Flag de root:

![](../../../Images/Pasted%20image%2020240827111149.png)
