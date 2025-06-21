Escaneo de puertos con Nmap:

![](../../../Images/Pasted%20image%2020240901160648.png)

Observo lo que hay en el puerto 80:

![](../../../Images/Pasted%20image%2020240901160725.png)

Y obtengo un posible usuario llamado andy

Daré click en el botón "Go to the paradise":

![](../../../Images/Pasted%20image%2020240904185221.png)

Revisando su código fuente obtengo un texto codificado en base64:

![](../../../Images/Pasted%20image%2020240904185259.png)

![](../../../Images/Pasted%20image%2020240904185313.png)

Miraré si es una pista de un posible directorio web:

![](../../../Images/Pasted%20image%2020240904185349.png)

Y efectivamente, tengo un .txt

Lo leo:

![](../../../Images/Pasted%20image%2020240904185404.png)

Y tengo un usuario llamado Lucas y como dice la nota, puedo ejecutar Fuerza Bruta hacia el puerto 22 SSH para encontrar su contraseña así que decido usar Hydra:

![](../../../Images/Pasted%20image%2020240904185531.png)

La contraseña de lucas seria chocolate

Intrusión por el puerto SSH con estas credenciales:

![](../../../Images/Pasted%20image%2020240904185609.png)

Y listo, estoy dentro de la máquina

## ESCALADA DE PRIVILEGIOS

Ejecuto sudo -l para ver el binario que puedo usar

![](../../../Images/Pasted%20image%2020240904185632.png)

Puedo ser el usuario Andy con el binario Sed

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020240904185944.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020240904190824.png)

Y ahora soy el usuario Andy

Observo los binarios que contiene la máquina

![](../../../Images/Pasted%20image%2020240904191052.png)

Y me llama la atención el /usr/local/bin/privileged_exec

Lo ejecuto:

![](../../../Images/Pasted%20image%2020240904191122.png)

Y listo, ya soy ROOT
