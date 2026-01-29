Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020250116194048.png)

Reviso lo que corre por el puerto 80:

![](../../../Images/Pasted%20image%2020250116194121.png)

Tengo una plantilla de Apache

Haré fuzzing web en busqueda de directorios con Gobuster:
```
gobuster dir -u http://172.17.0.2/ -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -x html,php,txt
```

![](../../../Images/Pasted%20image%2020250116194942.png)

Observo que hay en el directorio /index.php:

![](../../../Images/Pasted%20image%2020250116195143.png)

Tengo una pista, así que procedo a hacer fuzzing web para encontrar el parámetro que me permita leer el contenido de /var/www/html/.hidden_pass usando la herramienta de Wfuzz:
```
wfuzz -c -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -u '172.17.0.2/index.php?FUZZ=/var/www/html/.hidden_pass' --hl 0
```

![](../../../Images/Pasted%20image%2020250116194907.png)

El parámetro es page

Accedo y lo leo:

![](../../../Images/Pasted%20image%2020250116195818.png)

Es una posible contraseña, ahora voy a buscar el posible usuario.

Pero para poder hacer eso, ya que pude explotar un Local File Inclusion (LFI), veré que usuarios contiene la máquina en la carpeta /etc/passwd para saber a cuál pertenece la contraseña anteriormente encontrada:

![](../../../Images/Pasted%20image%2020250116200413.png)

Tengo 2 usuarios, pinguino y mario

Pruebo la contraseña con el usuario pinguino por el puerto 22 SSH:

![](../../../Images/Pasted%20image%2020250116200535.png)

Y listo, estoy dentro de la máquina

## ESCALADA DE PRIVILEGIOS

En la carpeta /home/pinguino, me encuentro con un .txt, lo veo y es la contraseña del usuario mario:

![](../../../Images/Pasted%20image%2020250116200636.png)

Escalo al usuario mario:

![](../../../Images/Pasted%20image%2020250116200655.png)

Ahora soy el usuario mario

Busco los binarios que contiene esta máquina:
```
find / -perm -4000 2>/dev/null
```

![](../../../Images/Pasted%20image%2020250116201102.png)

Y me llama la atención el binario Python

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020250116201202.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020250116201215.png)

Y listo, ya soy ROOT

