Escaneo de puertos:
![](../../../Images/Pasted%20image%2020240904102540.png)

Observo lo que corre en el puerto 80:
![](../../../Images/Pasted%20image%2020240904102600.png)
Es una tienda de cubos de rubik

Haciendo fuzzing web con Gobuster encontré el directorio /administration
![](../../../Images/Pasted%20image%2020240904103636.png)
Accedo a él:
![](../../../Images/Pasted%20image%2020240904103656.png)
Ingresé al panel de administrador sin proporcionar ninguna contraseña ni dato 

Revisando por los diferentes botones encontré una consola de comandos:
![](../../../Images/Pasted%20image%2020240904103843.png)

Intentaré listar carpetas de la máquina de forma remota:
![](../../../Images/Pasted%20image%2020240904103919.png)
![](../../../Images/Pasted%20image%2020240904103929.png)
Pero al momento de darle al botón azul, la pagina se recarga y no me muestra nada interesante

Intentaré convertir este mismo comando pero a base32 a ver si así da resultado:
![](../../../Images/Pasted%20image%2020240904104217.png)
![](../../../Images/Pasted%20image%2020240904104229.png)
![](../../../Images/Pasted%20image%2020240904104241.png)
Y efectivamente puedo leer lo que contiene la carpeta; Me llama la atención que tengo una id_rsa, la leo:
![](../../../Images/Pasted%20image%2020240904104644.png)

Entonces observo cuales usuarios contiene esta máquina:
![](../../../Images/Pasted%20image%2020240904104544.png)
Tengo el usuario luisillo

Intentaré acceder a este usuario usando la id_rsa proporcionada anteriormente por el puerto 22 SSH:
![](../../../Images/Pasted%20image%2020240904183540.png)
Y ahora estoy dentro de la máquina

## ESCALADA DE PRIVILEGIOS

Ejecuto sudo -l para ver el binario que puedo usar 
![](../../../Images/Pasted%20image%2020240904183704.png)
Puedo ser root con el binario Cube

Miro de que trata este binario:
![](../../../Images/Pasted%20image%2020240904184059.png)
Y observo que para escalar puedo ayudarme de una vulnerabilidad llamada Código Arbitrario:
```
a[$(bash >&2)]+42
```
![](../../../Images/Pasted%20image%2020240904184440.png)
Y listo, ya soy ROOT
