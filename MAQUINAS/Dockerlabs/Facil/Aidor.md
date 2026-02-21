Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020260221134617.png)

Empezaré revisando el puerto HTTP 5000:

![](../../../Images/Pasted%20image%2020260221134651.png)

Estoy ante un panel de login web, como no tengo credenciales ni posibles usuarios daré click en "Regístrate aquí" y me registraré con unos datos sencillos; posteriormente daré click en el boton de Crear Cuenta:

![](../../../Images/Pasted%20image%2020260221135441.png)

![](../../../Images/Pasted%20image%2020260221135617.png)

Ya estoy registrado en la página, me doy cuenta que leyendo en la web tengo el id de usuario #55 y revisando el link también aparece este indicador. Junto a su respectiva contraseña pero en formato Hash

![](../../../Images/Pasted%20image%2020260221170738.png)

Revisaré si puedo leer por ejemplo el del usuario anterior, por ejemplo el #50:

![](../../../Images/Pasted%20image%2020260221170839.png)

Efectivamente puede hacer cambio de usuario con solamente cambiar este número

Por ende, me copiaré un archivo realizado en Python de mi compañero [Dise0](https://dise0.gitbook.io/h4cker_b00k/ctf/dockerlabs/aidor-dockerlabs-easy#:~:text=a%20crackearlas.-,Extraer%20informaci%C3%B3n%20por%20IDOR,main(),-Vamos%20a%20utilizarla)
que ayuda a extraer los hashes de las contraseñas de los usuarios desde el #54 al #0 para seguidamente crackearlas:

![](../../../Images/Pasted%20image%2020260221171344.png)

Después de ejecutar este .py, me deja un .txt con los usuarios junto a sus respectivas hashes, le pasaré este archivo Jhon para crackearlas:
```
john --format=Raw-SHA256 --wordlist=/usr/share/wordlists/rockyou.txt users_hashes.txt
```

![](../../../Images/Pasted%20image%2020260221172400.png)

Obtengo 4 contraseñas (columna izquierda) y 4 usuarios (columna derecha), los separo en 2 archivos .txt:

![](../../../Images/Pasted%20image%2020260221172652.png)

Seguidamente procederé a relizar un ataque de fuerza bruta con Hydra apuntando hacia el puerto 22 SSH para encontrar el usuario y su respectiva contraseña usando estos 2 "diccionarios":
```
hydra -L users.txt -P contraseñas.txt ssh://172.17.0.2 -t 64
```

![](../../../Images/Pasted%20image%2020260221173525.png)

Y el usuario es "aidor" con su contraseña "chocolate"

Intrusión a la máquina con estas credenciales:

![](../../../Images/Pasted%20image%2020260221173625.png)

Ahora estoy dentro de la máquina

## ESCALADA DE PRIVILEGIOS 

Revisando en el directorio /home, me encuentro con un "app.py" que es el archivo donde se guarda el backend de la página web:

![](../../../Images/Pasted%20image%2020260221173936.png)

Al leerlo, en el inicio me encuentro con:

![](../../../Images/Pasted%20image%2020260221174052.png)

Es el hash del usuario root, que está comentado dentro de este .py

Lo crackeo con la web de Crackstation

![](../../../Images/Pasted%20image%2020260221174130.png)

Está hasheado en formato MD5 y al crackearlo la contraseña de root es "estrella"

Escalo a root:

![](../../../Images/Pasted%20image%2020260221174236.png)

Y listo ya soy ROOT
