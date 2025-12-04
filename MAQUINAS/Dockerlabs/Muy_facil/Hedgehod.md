Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.3 -oN escaneo
```

![](../../../Images/Pasted%20image%2020250212070609.png)

Observo lo que corre por el puerto 80:

![](../../../Images/Pasted%20image%2020250212070631.png)

Tengo un posible nombre de usuario llamado "tails"

Procedo a hacer un ataque de fuerza bruta para encontrar la contraseña de este usuario usando Hydra apuntando hacia el puerto 22 SSH:

![](../../../Images/Pasted%20image%2020250212072117.png)

Y encontré la contraseña

Accedo por el puerto 22 SSH con estas credenciales:

![](../../../Images/Pasted%20image%2020250212072301.png)

Y ahora estoy dentro de la máquina

## ESCALADA DE PRIVILEGIOS

Ejecuto sudo -l para ver el binario que puedo usar 

![](../../../Images/Pasted%20image%2020250212072337.png)

Puedo escalar al super usuario usando "sudo su"
![](../../../Images/Pasted%20image%2020250212072513.png)

Y listo, soy ROOT

