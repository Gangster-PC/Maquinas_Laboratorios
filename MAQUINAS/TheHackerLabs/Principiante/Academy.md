Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240804190630.png)

Veamos lo que hay en el puerto 80:

![](../../../Images/Pasted%20image%2020240804190649.png)

Tenemos una plantilla de Apache

Haremos fuzzing web para encontrar directorios con la herramienta de Gobuster:

![](../../../Images/Pasted%20image%2020240804190833.png)

Tenemos un /wordpress

![](../../../Images/Pasted%20image%2020240804191648.png)

Tenemos una página al parecer hecha en Wordpress

Sigamos haciendo fuzzing pero ahora a este directorio:

![](../../../Images/Pasted%20image%2020240804191020.png)

Efectivamente estamos frente a un wordpress

Accederemos a /wp-login.php

![](../../../Images/Pasted%20image%2020240804191512.png)

Tenemos un panel de login 

Usaremos la herramienta Wpscan para ver usuarios:

![](../../../Images/Pasted%20image%2020240804191735.png)

![](../../../Images/Pasted%20image%2020240804191808.png)

Tenemos el usuario Dylan

Haremos fuerza bruta para encontrar la contraseña a este usuario con la misma herramienta

![](../../../Images/Pasted%20image%2020240804192043.png)

![](../../../Images/Pasted%20image%2020240804192112.png)

La contraseña de dylan es password1

Accedamos a la página con estas credenciales:

![](../../../Images/Pasted%20image%2020240804192201.png)

![](../../../Images/Pasted%20image%2020240804192313.png)

Entramos

Ahora vamos a la pestaña Bit File Manager

![](../../../Images/Pasted%20image%2020240804192615.png)

Luego a wp-content 

![](../../../Images/Pasted%20image%2020240804192648.png)

Ahora Uploads

![](../../../Images/Pasted%20image%2020240804192706.png)

Ahora nos creamos una RevShell con Msfvenom en un archivo .php

![](../../../Images/Pasted%20image%2020240804192809.png)

Y lo subimos a la pagina web en la carpeta Uploads

![](../../../Images/Pasted%20image%2020240804192856.png)

Vamos a la dirección donde subimos este archivo:

![](../../../Images/Pasted%20image%2020240804192931.png)

Nos ponemos a la escucha con netcat y damos click en nuestro archivo

![](../../../Images/Pasted%20image%2020240804193006.png)

Y recibimos conexión

### Tratamiento de la TTY
```
script /dev/null -c bash 
Ctrl z
stty raw -echo; fg 
reset xterm 
export SHELL=bash
export TERM=xterm
```

![](../../../Images/Pasted%20image%2020240804193158.png)

![](../../../Images/Pasted%20image%2020240804193220.png)

## ESCALADA DE PRIVILEGIOS

Nos descargamos Pspy64 que es una herramienta la cual nos deja ver las acciones de la máquina que se están realizando en segundo plano, y le damos permisos de ejecución

![](../../../Images/Pasted%20image%2020240805073446.png)

Lo ejecutamos y encontramos:

![](../../../Images/Pasted%20image%2020240805073629.png)

![](../../../Images/Pasted%20image%2020240805073741.png)

Observamos que se ejecuta un backup.sh en la carpeta /opt en segundo plano

Asi que crearemos este archivo con este nombre y le daremos permisos a la /bin/bash para poder ser usuarios root

![](../../../Images/Pasted%20image%2020240805074135.png)

Y listo, ya somos ROOT

La flag de user:

![](../../../Images/Pasted%20image%2020240805074253.png)

La flag de root:

![](../../../Images/Pasted%20image%2020240805074217.png)

