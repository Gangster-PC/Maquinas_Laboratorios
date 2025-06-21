Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240722192138.png)

Veamos lo que hay en el puerto 21, por defecto no tiene contraseña solo usuario anoymous:

![](../../../Images/Pasted%20image%2020240722192250.png)

Tenemos un secret.txt

Procedemos a descargarlo y leerlo:

![](../../../Images/Pasted%20image%2020240722192329.png)

Lo dejaremos ahí por el momento

Veamos lo que hay en el puerto 80:

![](../../../Images/Pasted%20image%2020240722192510.png)

Tenemos una pagina como de una especie de Blog

Haremos fuzzing web con Dirb para encontrar dominios:

![](../../../Images/Pasted%20image%2020240722194106.png)

Tenemos bastante información pero nos fijaremos en esta dirección en concreto:

![](../../../Images/Pasted%20image%2020240722194138.png)

Que contiene:

![](../../../Images/Pasted%20image%2020240722194153.png)

Y dentro de este prueba.php, hay:

![](../../../Images/Pasted%20image%2020240722194213.png)

Una LFI, una ejeccución remota de comandos

Ejecutaremos un ejemplo como dice en la misma página para ver como funciona:

![](../../../Images/Pasted%20image%2020240722194317.png)

Y logramos ver que si responde correctamente

Asi q procederemos a mandarnos una reverse sheell a nuestro pc:

![](../../../Images/Pasted%20image%2020240722194440.png)

Y al dar enter, automáticamente recibimos conexión desde la maquina Papaya hacia mi maquina

![](../../../Images/Pasted%20image%2020240722194542.png)

Haremos tratamiento de la TTY:

![](../../../Images/Pasted%20image%2020240722194645.png)

![](../../../Images/Pasted%20image%2020240722194711.png)

## ESCALADA DE PRIVILEGIOS 

Observamos que en el directorio /opt, tenemos un archivo .zip, así que me lo pasare a mi maquina para lograr descifrarlo y ver lo que contiene:

![](../../../Images/Pasted%20image%2020240722194855.png)

Procedemos a descomprimirlo

![](../../../Images/Pasted%20image%2020240722194927.png)

Pero nos pide una contraseña la cual no tenemos

Procedemos a crackearla con Zip2john para saber su contraseña:

![](../../../Images/Pasted%20image%2020240722195040.png)

Y vemos q la contraseña es jesica

Ahora descomprimiremos el archivo pass.zip:

![](../../../Images/Pasted%20image%2020240722195606.png)

Tenemos un pass.txt

Veamos lo q contiene:

![](../../../Images/Pasted%20image%2020240722195626.png)

Una posible contraseña 

Así que usaremos esta contraseña para ser el usuario papaya:

![](../../../Images/Pasted%20image%2020240722195705.png)

Y listo, ahora somos  el usuario papaya

Tenemos la flag del user:

![](../../../Images/Pasted%20image%2020240722195753.png)

Ejecutamos sudo -l para ver el binario que podremos usar para ser root:

![](../../../Images/Pasted%20image%2020240722195836.png)

Podemos usar el binario Scp para hacer la escalada a root

Miramos en GTFOBins como podemos escalar con este binario:

![](../../../Images/Pasted%20image%2020240722195933.png)

Lo ejecutamos:

![](../../../Images/Pasted%20image%2020240722200010.png)

Y listo, ya somos ROOT

Y la flag de root:

![](../../../Images/Pasted%20image%2020240722200037.png)

