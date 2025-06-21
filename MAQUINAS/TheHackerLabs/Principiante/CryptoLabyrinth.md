Escaneo de puertos con Nmap
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 192.168.1.52 -oN escaneo
```

![](../../../Images/Pasted%20image%2020250216145233.png)

Observo lo que corre por el puerto 80:

![](../../../Images/Pasted%20image%2020250216145318.png)

Tengo una plantilla de Apache

Revisando el código fuente de esta página me encuentro esto en la línea 371:

![](../../../Images/Pasted%20image%2020250216145603.png)

Hace referencia a una posible contraseña, pero que toca crear un diccionario con posibles caracteres en mayúsculas reemplazando los 2 ** asteriscos, usaré python para crearlo:
```
python3 -c "import itertools; [print(f'2LWxmDsW0{c1}{c2}') for c1, c2 in itertools.product('ABCDEFGHIJKLMNOPQRSTUVWXYZ', repeat=2)]" > diccionario.txt
```

Haré fuzzing web usando Gobuster para encontrar directorios:

![](../../../Images/Pasted%20image%2020250216145626.png)

Existe un directorio llamado /hidden

Accedo al directorio

![](../../../Images/Pasted%20image%2020250216145657.png)

Tengo muchos archivos para leer pero en ellos encuentro 2 usuarios, bob y alice

Haré fuerza bruta con Hydra apuntando hacia el puerto SSH con estos usuarios y usando el diccionario que creé:

![](../../../Images/Pasted%20image%2020250216152352.png)

La contraseña del usuario bob es 2LWxmDsW0AE

Intrusión por el puerto 22 SSH con estas credenciales:

![](../../../Images/Pasted%20image%2020250216172831.png)

Ahora estoy dentro de la máquina

Flag de user

![](../../../Images/Pasted%20image%2020250216172907.png)

## ESCALADA DE PRIVILEGIOS

Ejecuto sudo -l para ver el binario que puedo usar 

![](../../../Images/Pasted%20image%2020250216172935.png)

Puedo ser el usuario alice con el binario Env

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020250216173006.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020250216173046.png)

Y ahora soy el usuario alice

Flag de user #2:

![](../../../Images/Pasted%20image%2020250216173121.png)

En el directorio /mnt, me encuentro con un .txt:

![](../../../Images/Pasted%20image%2020250216173301.png)

Lo leo y observo que debo crear otro diccionario reemplazando nuevamente los 2 asteriscos, usaré la herramienta Crunch para crearlo:
```
crunch 11 11 -t 2LWx%DsW0A% -o diccionario2.txt 
```

![](../../../Images/Pasted%20image%2020250216173954.png)

Haré un ataque de fuerza bruta nuevamente con Hydra apuntando hacia el puerto SSH con el usuario root y usando el diccionario anteriormente creado:

![](../../../Images/Pasted%20image%2020250216174024.png)

La contraseña de root es 2LWx9DsW0A3

Intrusión nuevamente por el puerto 22 SSH con estas credenciales:

![](../../../Images/Pasted%20image%2020250216174216.png)

Y listo, ya soy ROOT

Flag de root

![](../../../Images/Pasted%20image%2020250216174244.png)

