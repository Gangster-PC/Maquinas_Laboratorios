Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.3 -oN escaneo
```

![](../../../Images/Pasted%20image%2020250212073301.png)

Observo lo que corre por el puerto 80:

![](../../../Images/Pasted%20image%2020250212073333.png)

Hay una plantilla de Apache, nada relevante

Por ende, buscaré un exploit en Metasploit para la versión del puerto ftp "vsftpd 2.3.4" y ver si existe:

![](../../../Images/Pasted%20image%2020250212073747.png)

Efectivamente existe uno

Lo selecciono y lo configuro:

![](../../../Images/Pasted%20image%2020250212073854.png)

Debo colocar el Rhosts que es la ip de la máquina:

![](../../../Images/Pasted%20image%2020250212073952.png)

Y lo ejecuto:

![](../../../Images/Pasted%20image%2020250212074335.png)

Estoy dentro de la máquina y de una vez entré siendo ROOT

Flag de root:

![](../../../Images/Pasted%20image%2020250212074412.png)
