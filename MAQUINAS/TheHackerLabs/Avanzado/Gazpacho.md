Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240820105106.png)

Veamos lo que hay en el puerto 80:

![](../../../Images/Pasted%20image%2020240820105324.png)

Tenemos un panel de Login

Y en el puerto 8080 tenemos:

![](../../../Images/Pasted%20image%2020240820105353.png)

Un panel de Login de Jenkins

Si probamos credenciales típicas como admin:12345, logramos acceder:

![](../../../Images/Pasted%20image%2020240820111108.png)

![](../../../Images/Pasted%20image%2020240820111220.png)

Vamos a Manage Jenkins>Script Console:

![](../../../Images/Pasted%20image%2020240820111304.png)

Y aquí vamos a pegarnos una RevShell en lenguaje Groovy, nos podemos a la escucha con NetCat:

![](../../../Images/Pasted%20image%2020240820111451.png)

![](../../../Images/Pasted%20image%2020240820111513.png)

Y listo, recibimos la conexión y estamos dentro de la máquina

### Tratamiento de la TTY
```
script /dev/null -c bash 
Ctrl z
stty raw -echo; fg 
reset xterm 
export SHELL=bash
export TERM=xterm
```

![](../../../Images/Pasted%20image%2020240820111553.png)

![](../../../Images/Pasted%20image%2020240820111612.png)

## ESCALADA DE PRIVILEGIOS

Ejecutamos sudo -l para ver el binario que podremos usar 

![](../../../Images/Pasted%20image%2020240820111721.png)

Podemos ser el usuario ajo con el binario Find

Miramos en GTFOBins como podemos escalar con este binario:

![](../../../Images/Pasted%20image%2020240820111807.png)

Lo ejecutamos:

![](../../../Images/Pasted%20image%2020240820111844.png)

Y ahora somos el usuario ajo

Ejecutamos sudo -l para ver el binario que podremos usar 

![](../../../Images/Pasted%20image%2020240820111918.png)

Podemos ser el usuario cebolla con el binario Aws

Miramos en GTFOBins como podemos escalar con este binario:

![](../../../Images/Pasted%20image%2020240820111953.png)

Lo ejecutamos:

![](../../../Images/Pasted%20image%2020240820112030.png)

![](../../../Images/Pasted%20image%2020240820112048.png)

![](../../../Images/Pasted%20image%2020240820112057.png)

Y ahora somos el usuario cebolla

Ejecutamos sudo -l para ver el binario que podremos usar 

![](../../../Images/Pasted%20image%2020240820112241.png)

Podemos ser el usuario pimiento con el binario Crash

Miramos en GTFOBins como podemos escalar con este binario:

![](../../../Images/Pasted%20image%2020240820112326.png)

Lo ejecutamos:

![](../../../Images/Pasted%20image%2020240820112359.png)

![](../../../Images/Pasted%20image%2020240820112413.png)

![](../../../Images/Pasted%20image%2020240820112424.png)

Ahora somos el usuario pimiento

Ejecutamos sudo -l para ver el binario que podremos usar 

![](../../../Images/Pasted%20image%2020240820112506.png)

Podemos ser el usuario pepino con el binario Cat

Miramos en GTFOBins como podemos escalar con este binario:

![](../../../Images/Pasted%20image%2020240820112532.png)

Lo ejecutamos:

![](../../../Images/Pasted%20image%2020240820112703.png)

Y obtenemos la id_rsa del usuario pepino

Me la paso a mi kali y le extraemos la hash con Ssh2John y la crackeo con John:

![](../../../Images/Pasted%20image%2020240820113105.png)

Y la contraseña es mittens

Intrusión por el puerto 22 SSH con el usuario pepino y la id_rsa:

![](../../../Images/Pasted%20image%2020240820113200.png)

Y volvemos a estar dentro de la máquina pero ahora desde otro puerto

Ejecutamos sudo -l para ver el binario que podremos usar 

![](../../../Images/Pasted%20image%2020240820113226.png)

Podemos ser el usuario tomate con el binario Mail

Miramos en GTFOBins como podemos escalar con este binario:

![](../../../Images/Pasted%20image%2020240820113310.png)

Lo ejetucamos:

![](../../../Images/Pasted%20image%2020240820113343.png)

Ahora somos el usuario tomate 

Ejecutamos sudo -l para ver el binario que podremos usar 

![](../../../Images/Pasted%20image%2020240820114112.png)

Podemos ser root con el binario Bettercap

Con este binario le daremos permisos a la /bin/bash para ser ejecutada:

![](../../../Images/Pasted%20image%2020240820114748.png)

Y listo, ya somos ROOT haciendo un pivoting de usuarios 

Flag de user:

![](../../../Images/Pasted%20image%2020240820114832.png)

Flag de root:

![](../../../Images/Pasted%20image%2020240820114846.png)

