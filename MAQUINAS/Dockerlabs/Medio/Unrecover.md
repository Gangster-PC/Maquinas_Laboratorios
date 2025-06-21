Escaneo de puertos con Nmap
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020250209190327.png)

Observo lo que corre por el puerto 80:

![](../../../Images/Pasted%20image%2020250206190043.png)

Me dice la página que al parecer hay un usuario llamado "capybara"

Haré fuerza bruta con este usuario por el puerto 3306 Mysql con Hydra para encontrar su posible contraseña:

![](../../../Images/Pasted%20image%2020250209190522.png)

La contraseña del usuario capybara es password1

Intrusión por el puerto 3306 Mysql con estas credenciales:
```
mysql -h 172.17.0.3 -u capybara -ppassword1 --skip-ssl
```

![](../../../Images/Pasted%20image%2020250209190752.png)

Miro las bases de datos:

![](../../../Images/Pasted%20image%2020250209190830.png)

Usaré la base de datos beta

![](../../../Images/Pasted%20image%2020250209190927.png)

Veré las tablas

![](../../../Images/Pasted%20image%2020250209190950.png)

Selecciono todo de esta tabla de registration

![](../../../Images/Pasted%20image%2020250209191119.png)

Y tengo un usuario y una contraseña

Crackeo la contraseña con hash usando Crackstation:

![](../../../Images/Pasted%20image%2020250209191227.png)

Intentaré acceder por el puerto 21 FTP con estas credenciales:

![](../../../Images/Pasted%20image%2020250209191438.png)

Accedí efectivamente

Leo y descargo los archivos que hay en este puerto

![](../../../Images/Pasted%20image%2020250209191519.png)

Tengo un .pdf

Lo leo:

![](../../../Images/Pasted%20image%2020250209191716.png)

Y logro entender que la contraseña de root es "passwordpepinaca"

Intrusión por el puerto 22 SSH con las credenciales encontradas:

![](../../../Images/Pasted%20image%2020250209191320.png)

Y estoy dentro de la máquina

## ESCALADA DE PRIVILEGIOS

Escalo a root con la contraseña encontrada en el .pdf ("passwordpepinaca")

![](../../../Images/Pasted%20image%2020250209191806.png)

Y listo, soy ROOT
