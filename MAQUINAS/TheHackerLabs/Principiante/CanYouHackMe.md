Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240911185301.png)

Observo lo que corre en el puerto 80:

![](../../../Images/Pasted%20image%2020240911185358.png)

Revisando el código fuente de esta página web me encuentro con un usuario en la línea 67:

![](../../../Images/Pasted%20image%2020240912083117.png)

El cual se llama Juan

Haré fuerza bruta a este usuario con Hydra al puerto 22:

![](../../../Images/Pasted%20image%2020240912083617.png)

La contraseña de juan es matrix

Intrusión por el puerto 22 SSH:

![](../../../Images/Pasted%20image%2020240912083818.png)

Ahora estoy dentro de la máquina

Flag de user:

![](../../../Images/Pasted%20image%2020240912083722.png)

Revisando a los grupos a los que pertenezco siendo juan:

![](../../../Images/Pasted%20image%2020240912084218.png)

Observo que pertenezco al grupo docker

Miro en GTFObins como puedo ser root con esta Shell de docker:

![](../../../Images/Pasted%20image%2020240812115132.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020240912084349.png)

Y listo soy root, pero en el contenedor de docker no directamente en mi máquina 

Por ende editaré el archivo /etc/passwd con la herramienta Vi, para borrar la "x" de root:

![](../../../Images/Pasted%20image%2020240912084907.png)

![](../../../Images/Pasted%20image%2020240912084936.png)

![](../../../Images/Pasted%20image%2020240912085016.png)

Y listo ya soy ROOT en la máquina ahora si

Flag de root:

![](../../../Images/Pasted%20image%2020240912084712.png)

