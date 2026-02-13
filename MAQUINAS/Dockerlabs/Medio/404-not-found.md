Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020240827092418.png)

Observo lo que corre en el puerto 80:

![](../../../Images/Pasted%20image%2020240827093427.png)

Tengo un botón, doy click en él:

![](../../../Images/Pasted%20image%2020240827094141.png)

La clave secreta me dice:

![](../../../Images/Pasted%20image%2020240827095122.png)

Revisando la url pues deduzco que toca buscar un subdominio, usaré Wfuzz para encontrarlo:

![](../../../Images/Pasted%20image%2020240827095214.png)
El subdominio es "info", accedo a él:

![](../../../Images/Pasted%20image%2020240827095303.png)

Tengo un panel de login

Revisando el código de fuente de esta página me encuentro con:

![](../../../Images/Pasted%20image%2020240827095344.png)

Así que usaré LDAP injection, buscando en internet encuentro este repositorio con unas posibles credenciales: https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/LDAP%20Injection/README.md

![](../../../Images/Pasted%20image%2020240827095748.png)

Las prueebo en el panel de login:

![](../../../Images/Pasted%20image%2020240827095818.png)

Accedo:

![](../../../Images/Pasted%20image%2020240827095858.png)

Y tengo unas credenciales, pruebo a ver si son del puerto 22 SSH:

![](../../../Images/Pasted%20image%2020240827095959.png)

Y listo, estoy dentro de la máquina

## ESCALADA DE PRIVILEGIOS

Ejecuto sudo -l para ver el binario que podré usar

![](../../../Images/Pasted%20image%2020240827100115.png)

Puedo ser el usuario "200-ok" con un script hecho en python llamado calculator.py

Ejecuto este script:

![](../../../Images/Pasted%20image%2020240827100502.png)

Y literalmente tengo una calculadora, pero pruebo a lanzame una !/bin/bash y escalo de usuario:

![](../../../Images/Pasted%20image%2020240827100558.png)

Ahora soy el usuario "200-ok"

Flag de user:

![](../../../Images/Pasted%20image%2020240827100634.png)

En la carpeta de 200-ok tengo un boss.txt, lo leo:

![](../../../Images/Pasted%20image%2020240827100851.png)

Pruebo ser root con esta posible contraseña:

![](../../../Images/Pasted%20image%2020240827100919.png)

Y listo, ya soy ROOT

Flag de root:

![](../../../Images/Pasted%20image%2020240827100944.png)
