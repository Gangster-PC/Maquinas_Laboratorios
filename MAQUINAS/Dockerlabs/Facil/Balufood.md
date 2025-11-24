Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020251105153520.png)

Revisaré el puerto 5000 HTTP:

![](../../../Images/Pasted%20image%2020251105153652.png)

Existe una página de un restaurante "Balulero"

Observo que hay un panel de Login web:

![](../../../Images/Pasted%20image%2020251105153734.png)

Probaré las típicas credenciales, colocando como usuario y como contraseña "admin":

![](../../../Images/Pasted%20image%2020251106151539.png)

Y accedí a través del panel de login:

![](../../../Images/Pasted%20image%2020251106151602.png)

No encontré nada relevante, pero leyendo el código fuente de esta web me encontré con algo interesante:

![](../../../Images/Pasted%20image%2020251106151637.png)

En la línea 37 tengo unas credenciales

Así que recordando, tengo acceso al puerto 22 SSH probaré estas credenciales allí

![](../../../Images/Pasted%20image%2020251106151910.png)

Y si, ahora estoy dentro de la máquina

## ESCALADA DE PRIVILEGIOS

Revisando mi actual directorio me encuentro con un app.py, leyendo el comienzo de dicho archivo observo un la clave algo interesante, parece ser una posible contraseña

![](../../../Images/Pasted%20image%2020251106153237.png)

Listaré los usuarios que contiene la máquina:

![](../../../Images/Pasted%20image%2020251106153414.png)

Tengo el usuario "balulero", probaré si esta contraseña ("cuidaditocuidadin") pertenece a este usuario:

![](../../../Images/Pasted%20image%2020251106153501.png)

Efectivamente si es, ya escalé de usuario, ahora soy "balulero"

Revisaré los alias que existen:

![](../../../Images/Pasted%20image%2020251106153722.png)

Me encuentro con un mensaje que dice "chocolate2" junto a un parámetro que dice root, quizás esta sea la contraseña del super usuario, probaré:

![](../../../Images/Pasted%20image%2020251106153838.png)

Y listo, ya soy ROOT

