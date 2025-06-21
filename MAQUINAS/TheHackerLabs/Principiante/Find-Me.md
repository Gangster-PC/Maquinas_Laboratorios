Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240801181031.png)

![](../../../Images/Pasted%20image%2020240801181054.png)

Tenemos el puerto 21 FTP abierto, veamos lo que contiene

![](../../../Images/Pasted%20image%2020240801181357.png)

Nos descargamos el ayuda.txt 

![](../../../Images/Pasted%20image%2020240801181447.png)

Y lo leemos:

![](../../../Images/Pasted%20image%2020240801181511.png)

Tenemos un usuario llamado Geralt, pero nos dice que la contraseña la encontraremos con fuerza bruta y que contiene 5 carácteres empieza por P y termina por A

Crearemos este diccionario con la herramienta Crunch tal y como nos lo piden:

![](../../../Images/Pasted%20image%2020240802185011.png)

![](../../../Images/Pasted%20image%2020240802185034.png)

Veamos lo que hay en el puerto 8080

![](../../../Images/Pasted%20image%2020240801181635.png)

Tenemos un panel de login de la plataforma Jenkins

Haremos un ataque de fuerza bruta con la herramienta Patator con el usuario de geralt a este panel de login web de Jenkins usando el diccionario.txt 

![](../../../Images/Pasted%20image%2020240802184826.png)

Y nos encuentra que la contraseña para el usuario geralt es panda

Accedemos a Jenkins:

![](../../../Images/Pasted%20image%2020240802185205.png)

Estamos dentro de la plataforma

Vamos a Manage Jenkins

![](../../../Images/Pasted%20image%2020240802185314.png)

Y bajamos y damos click en Script Console

![](../../../Images/Pasted%20image%2020240802185425.png)

Y le pasamos una reverse shell en el lenguaje de Groovy

![](../../../Images/Pasted%20image%2020240802185519.png)

Nos podemos a la escucha con Netcat y damos click en Run

![](../../../Images/Pasted%20image%2020240802185908.png)

Y tenemos conexión, estamos dentro de la maquina

Intento ser geralt con la misma contraseña del panel de login de Jenkins

![](../../../Images/Pasted%20image%2020240802190042.png)

Y ahora soy geralt

La flag de user:

![](../../../Images/Pasted%20image%2020240802190119.png)

## ESCALADA DE PRIVILEGIOS

Observamos los binarios de la máquina:

![](../../../Images/Pasted%20image%2020240802190221.png)

Y evidenciamos que podemos escalara a root con el binario Php

Miramos en GTFOBins como podemos escalar con este binario:

![](../../../Images/Pasted%20image%2020240802190406.png)

Lo ejecutamos:

![](../../../Images/Pasted%20image%2020240802190724.png)

Y listo ya somos ROOT

La flag de root:

![](../../../Images/Pasted%20image%2020240802190909.png)

