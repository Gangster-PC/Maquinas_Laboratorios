Escaneo de puertos con Nmap
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020260212194353.png)

Revisaré el puerto HTTP 80:

![](../../../Images/Pasted%20image%2020260212194337.png)

Es una página web de "noticias", no encuentro nada relevante
Por ende procederé a revisar su código fuente para ver si encuentro allí alguna pista:

![](../../../Images/Pasted%20image%2020260212194518.png)

En la línea 14 me encuentro con un posible usuario llamado "pipe", así que procederé a hacer fuerza bruta con Hydra usando este nombre; apuntando hacia el puerto 22 SSH:
```
hydra -l pipe -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2 -t 64
```

![](../../../Images/Pasted%20image%2020260212194828.png)

En efecto, este usuario si existe en la máquina y su contraseña correspondiente es "kisses"

Intrusión con estas credenciales por el puerto 22 SSH:

![](../../../Images/Pasted%20image%2020260212194955.png)

Y listo, ya estoy dentro de la máquina que es un windows 

Flag de Pipe:

![](../../../Images/Pasted%20image%2020260212202505.png)

## ESCALADA DE PRIVILEGIOS

Después de estar investigando por toda la máquina me di de cuenta que esta escalada es algo inusual, ya que consta de agrupar todas las iniciales de las frases de la página web y forma una sola palabra que es la contraseña a root, tal y como se muestra en la siguiente imagen:

![](../../../Images/Pasted%20image%2020260212202057.png)

Uniendo todas las iniciales forma la palabra  ***WinServerRootFakeNews***

Escalo a root con esta contraseña:

![](../../../Images/Pasted%20image%2020260212202452.png)

Flag de root:

![](../../../Images/Pasted%20image%2020260212202544.png)

