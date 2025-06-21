Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240909202853.png)

Intentaré acceder al puerto 21 FTP ya que me deja acceder sin contraseña con el usuario anonymous:

![](../../../Images/Pasted%20image%2020240909203806.png)

Y me descargo ambos archivos a mi kali:

![](../../../Images/Pasted%20image%2020240909203858.png)

Observaré el puerto 80:

![](../../../Images/Pasted%20image%2020240909203110.png)

Haré fuzzing web con Gobuster:

![](../../../Images/Pasted%20image%2020240909203709.png)

Accedo a el directorio /mysql:

![](../../../Images/Pasted%20image%2020240909203725.png)

Accedo a database.html:

![](../../../Images/Pasted%20image%2020240909204437.png)

Y leo su código fuente:

![](../../../Images/Pasted%20image%2020240909204457.png)

Y encuentro una contraseña pero codificada, la decodificaré:

![](../../../Images/Pasted%20image%2020240909204548.png)

Decodificada dice fuerzabruta

Recordando que cuando usé Gobuster encontré el directorio /webs, accedo a él y meto esta "contraseña" que descifré:

![](../../../Images/Pasted%20image%2020240909204804.png)

Y obtuve un usuario hulk

Intrusión por el puerto SSH 22 con el usuario hulk y la contraseña fuerzabruta:

![](../../../Images/Pasted%20image%2020240909204938.png)

![](../../../Images/Pasted%20image%2020240909204945.png)

Y listo, estoy dentro de la máquina

Revisando la carpeta /mysql/hint/zip tengo un .txt, lo leo:

![](../../../Images/Pasted%20image%2020240909210203.png)

E intuyo que el nombre del archivo .txt puede ser la contraseña que descomprime el .zip encontrado en el puerto FTP, así qué probaré:

![](../../../Images/Pasted%20image%2020240909210321.png)

Y si me dio resultado positivo

Leeré el archivo .txt que me dejó:

![](../../../Images/Pasted%20image%2020240909210353.png)

Me dice que la contraseña del usuario Hulk para acceder a Mysql contiene la frase "fuerzabruta" pero que adicionalmente tiene 4 números aleatorios, así que crearé un diccionario con estas posibles combinaciones usando la herramienta Crunch: 
```
crunch 15 15 -t fuerzabruta%%%% -o diccionario.txt -d 4@
```

![](../../../Images/Pasted%20image%2020240909211142.png)

Ahora si haré el ataque de fuerza bruta apuntando hacia el puerto 3306 Mysql:

![](../../../Images/Pasted%20image%2020240909211603.png)

Y la contraseña del usuario hulk es fuerzabruta2024

Accedo a la base de datos Mysql con estas credenciales:

![](../../../Images/Pasted%20image%2020240909211807.png)

Ahora miro lo que contienen las tablas del usuario Stif:

![](../../../Images/Pasted%20image%2020240909212008.png)

![](../../../Images/Pasted%20image%2020240909212020.png)

Y obtengo la contraseña del usuario Stif que es escudoamerica

Escalo al usuario Stif:

![](../../../Images/Pasted%20image%2020240909212058.png)

Y ahora soy stif

Ejecuto sudo -l para ver el binario que puedo usar 

![](../../../Images/Pasted%20image%2020240909212122.png)

Puedo ser root con el binario Bash

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020240708210732.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020240909212136.png)

Y listo, ya soy ROOT

Flag de User:

![](../../../Images/Pasted%20image%2020240909212239.png)

Flag de Root:

![](../../../Images/Pasted%20image%2020240909212329.png)

