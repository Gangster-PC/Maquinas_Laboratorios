Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020250202181030.png)

Observo lo que corre por el puerto 80:

![](../../../Images/Pasted%20image%2020250202181816.png)

Haré fuzzing web con Wfuzz para encontrar un posible subdominio:

![](../../../Images/Pasted%20image%2020250202182537.png)
Y el subdominio es japan

Accedo a la nueva página:

![](../../../Images/Pasted%20image%2020250202182810.png)

Dando click en el botón de Download se me descarga un programa, pero al momento de ejecutarlo me pide el Serial:

![](../../../Images/Pasted%20image%2020250202183907.png)

Entonces usaré ingeniería inversa y con la ayuda de la herramienta "strings" leeré solo palabras dentro del binario a ver si encuentro el serial dentro del mismo programa:

![](../../../Images/Pasted%20image%2020250202184056.png)

![](../../../Images/Pasted%20image%2020250202184107.png)

Y acá encuentro una serie de números, probaré si estos son el serial usando guiones para separar los números entre sí:

![](../../../Images/Pasted%20image%2020250202184258.png)

![](../../../Images/Pasted%20image%2020250202184318.png)

Doy click en el bóton de "Mostrar Contraseña Secreta":

![](../../../Images/Pasted%20image%2020250202184456.png)

Listo, tengo la contraseña ahora me falta encontrar el usuario; probaré por ejemplo de usuario el nombre de la máquina

Intrusión por el puerto 22 SSH con el usuario cracker y su respectiva contraseña:

![](../../../Images/Pasted%20image%2020250202185553.png)

Estoy dentro de la máquina

## ESCALADA DE PRIVILEGIOS

Escalo a root usando el mismo número Serial:

![](../../../Images/Pasted%20image%2020250202185750.png)

Y listo, soy ROOT
