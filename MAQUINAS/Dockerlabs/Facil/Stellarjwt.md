Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020250106184249.png)

Observo lo que corre por la página web:

![](../../../Images/Pasted%20image%2020250106184322.png)

Y respondiendo a la pregunta:

![](../../../Images/Pasted%20image%2020250106193305.png)

Realizaré una búsqueda de directorios usando Gobuster para hacer fuzzing web:

![](../../../Images/Pasted%20image%2020250106185349.png)

Me encuentro con el directorio /universe

Accedo a él:

![](../../../Images/Pasted%20image%2020250106192629.png)

Revisando el código fuente de esta página me encuentro con, en la línea 106:

![](../../../Images/Pasted%20image%2020250106192704.png)

Lo paso a CyberChef y me dice que decodificado es:

![](../../../Images/Pasted%20image%2020250106192747.png)

Son al parecer unas posibles credenciales

Intento acceder con ellas por el puerto 22 SSH recordando que el apellido del astrónomo es Gottfried, posiblemente esa sea la contraseña:

![](../../../Images/Pasted%20image%2020250106193503.png)

Y listo, estoy dentro de la máquina

## ESCALADA DE PRIVILEGIOS

Revisando en mi actual directorio, tengo un .txt lo leo y contiene:

![](../../../Images/Pasted%20image%2020250106193731.png)

Una posible contraseña

Observo los usuarios que pertenecen a esta máquina y me encuentro con el usuario nasa

![](../../../Images/Pasted%20image%2020250106193813.png)

Así que trato de escalar a él con la contraseña ya encontrada:

![](../../../Images/Pasted%20image%2020250106193845.png)

Ahora soy el usuario nasa

Ejecuto sudo -l para ver el binario que puedo usar para seguir escalando

![](../../../Images/Pasted%20image%2020250106193928.png)

Puedo ser Elite con el binario Socat

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020250106194024.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020250106194127.png)

Y listo, ahora soy el usuario Elite

Ejecuto sudo -l para ver el binario que puedo usar

![](../../../Images/Pasted%20image%2020250106194508.png)

Puedo ser root con el binario Chown
```
sudo chown elite:elite /etc/passwd
```
```
sed -i 's/^root:x:root::/' /etc/passwd
```

![](../../../Images/Pasted%20image%2020250106200617.png)

![](../../../Images/Pasted%20image%2020250106200625.png)

Y listo, ya soy ROOT
