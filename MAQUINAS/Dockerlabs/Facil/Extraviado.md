Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020250116185040.png)

Reviso lo que corre por el puerto 80:

![](../../../Images/Pasted%20image%2020250116185109.png)

Tengo una plantilla de Apache

Revisando el código fuente de esta página web, me encuentro en la línea 403 con:

![](../../../Images/Pasted%20image%2020250116185405.png)

Paso esos 2 posibles códigos cifrados a Cyberchef a ver que me encuentra:

![](../../../Images/Pasted%20image%2020250116185545.png)

Son al parecer unas posibles credenciales de usuario "daniela" y de contraseña "focaroja"

Las pruebo ingresando por el puerto 22 SSH:

![](../../../Images/Pasted%20image%2020250116185635.png)

Y efectivamente, ahora estoy dentro de la máquina

## ESCALADA DE PRIVILEGIOS

En el directorio /home/daniela/.secret, me encuentro con la contraseña del usuario diego

![](../../../Images/Pasted%20image%2020250116185807.png)

Pero al parecer también está cifrada así que nuevamente haré uso de la herramienta Cyberchef:

![](../../../Images/Pasted%20image%2020250116190133.png)

Escalo a diego con la contraseña descifrada:

![](../../../Images/Pasted%20image%2020250116190200.png)

Y ahora soy el usuario diego

En la ruta /home/diego/.local/share, me encuentro con una nota que es un acertijo que dice:

![](../../../Images/Pasted%20image%2020250116193256.png)

La respuesta al acertijo es "osoazul", por ende también la contraseña del usuario root

Escalo a root:

![](../../../Images/Pasted%20image%2020250116193431.png)

Y listo, soy ROOT
