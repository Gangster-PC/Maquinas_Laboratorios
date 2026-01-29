Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020241004133545.png)

Observo lo que corre por el puerto 80:

![](../../../Images/Pasted%20image%2020241004133619.png)

Quizas "a" sea un posible usuario

Haré fuerza bruta al puerto 22 SSH apuntando hacia este posible usuario con la herramienta llamada Hydra:

![](../../../Images/Pasted%20image%2020241006110924.png)

La contraseña de "a" es "secret"

Intrusión por el puerto 22 SSH:

![](../../../Images/Pasted%20image%2020241006110958.png)

Estoy dentro de la máquina

## ESCALADA DE PRIVILEGIOS

Observo qué otros usuarios contiene la máquina:

![](../../../Images/Pasted%20image%2020241006111424.png)

Tengo el usuario Spencer

Por ende le haré un ataque de fuerza bruta nuevamente pero a este usuario

![](../../../Images/Pasted%20image%2020241006111648.png)

Y la contraseña es password1

Intrusión nuevamente:

![](../../../Images/Pasted%20image%2020241006111752.png)

Pero ahora entré a la máquina siendo el usuario spencer

Ejecuto sudo -l para ver el binario que puedo usar

![](../../../Images/Pasted%20image%2020241006111819.png)

Puedo ser root con el binario Python

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020241006111844.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020241006111929.png)

Y listo, ya soy ROOT

