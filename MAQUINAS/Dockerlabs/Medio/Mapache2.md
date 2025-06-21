Escaneo de puertos:
![](../../../Images/Pasted%20image%2020240830182501.png)

Veo lo que corre en el puerto 80:
![](../../../Images/Pasted%20image%2020240830182543.png)

Dando en el botón Login, tenemos un panel del cual no tenemos credenciales:
![](../../../Images/Pasted%20image%2020240830182652.png)

Revisando el puerto 3306 encuentro:
![](../../../Images/Pasted%20image%2020240830182943.png)
Un usuario llamado medusa

Así que haré un ataque de fuerza bruta con Hydra hacía el panel de login web para encontrar la contraseña del usuario medusa, pero primero crearé un diccionario con palabras que contengan esta página con la herramienta Cewl:
![](../../../Images/Pasted%20image%2020240902110032.png)

Ahora si haré el ataque de fuerza bruta con estas posibles contraseñas:
```
hydra -l medusa -P wordlist "http-post-form://172.17.0.5/login.php:username=^USER^&password=^PASS^:Invalid credentials" -t 64 -I
```
![](../../../Images/Pasted%20image%2020240902110152.png)
Y la contraseña del usuario medusa es enthusiasts

Accedo al panel de login:
![](../../../Images/Pasted%20image%2020240902110236.png)
![](../../../Images/Pasted%20image%2020240902110255.png)

Reviso el código fuente de esta nueva página:
![](../../../Images/Pasted%20image%2020240902110326.png)
Y en la línea 102 obtengo un usuario llamado Kinder 

Haré nuevamente un ataque de fuerza bruta con Hydra pero esta vez apuntando hacia el puerto 22 SSH con este usuario kinder:
![](../../../Images/Pasted%20image%2020240904192148.png)
Y no me dió resultado

Por ende probaré si "medusa" puede ser la posible contraseña de este usuario Kinder:
![](../../../Images/Pasted%20image%2020240904193120.png)
Y efectivamente esa era la contraseña, ahora estoy dentro de la máquina

Flag de user:
![](../../../Images/Pasted%20image%2020240904193153.png)
## ESCALADA DE PRIVILEGIOS

Ejecuto sudo -l para ver el binario que puedo usar 
![](../../../Images/Pasted%20image%2020240904193212.png)
Puedo ser root con el servicio Apache2

Edito el archivo /etc/init.d/apache2:
![](../../../Images/Pasted%20image%2020240904193540.png)
Le coloco una /bin/bash y le doy permisos a esta misma:
![](../../../Images/Pasted%20image%2020240904193746.png)

Lo ejecuto:
![](../../../Images/Pasted%20image%2020240904193845.png)
Y listo, ya soy ROOT

Flag de root:
![](../../../Images/Pasted%20image%2020240904193906.png)
