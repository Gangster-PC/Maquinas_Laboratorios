Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020241006173840.png)

Observo lo que corre por el puerto 80:

![](../../../Images/Pasted%20image%2020241006173903.png)

Tengo una web de un perrito llamado Balú, pero no encuentro nada así importante directamente en la página web

Veré con F12 la consola de la página web y encuentro:

![](../../../Images/Pasted%20image%2020241006174307.png)

Tengo 2 archivos, uno llamado en .env y otro llamado .env_de_baluchingon

Accederé al segundo ya que dice que está visible:

![](../../../Images/Pasted%20image%2020241006174411.png)

Y obtengo unas credenciales

Intrusión por el puerto 22 SSH con estas credenciales:

![](../../../Images/Pasted%20image%2020241006174533.png)

Y listo, estoy dentro de la máquina

## ESCALADA DE PRIVILEGIOS

Ejecuto sudo -l para ver el binario que puedo usar

![](../../../Images/Pasted%20image%2020241006174605.png)

Puedo ser el usuario chocolate con el binario Php

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020241006174644.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020241006174720.png)

Y listo, ahora soy el usuario chocolate

Ahora me levantaré un servidor con python desde mi kali para pasar el Pspy64 que es una herramienta usada para listar procesos/servicios que corren en segundo plano, y la pasaré a la máquina

![](../../../Images/Pasted%20image%2020241006174921.png)

![](../../../Images/Pasted%20image%2020241006175010.png)

Le doy permisos de ejecución y lo ejecuto:

![](../../../Images/Pasted%20image%2020241006175048.png)

Y observo que cada 5 segundos se está ejecutando un script en el directorio /opt:

![](../../../Images/Pasted%20image%2020241006175148.png)

Y el script hace:

![](../../../Images/Pasted%20image%2020241006175231.png)

Entonces lo editaré y le colocaré permisos SUID a la /bin/bash y después con solo ejecutar bash -p ya sería root:

```
echo -e "<?php\n\tsystem('chmod u+s /bin/bash');\n?>" > /opt/script.php
```

![](../../../Images/Pasted%20image%2020241006180054.png)

Y listo, ya soy ROOT
