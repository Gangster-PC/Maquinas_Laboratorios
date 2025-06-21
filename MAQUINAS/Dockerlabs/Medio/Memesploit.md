Escaneo de puertos:
![](../../../Images/Pasted%20image%2020240908190419.png)

Observo lo que hay en el puerto 80:
**![](../../../Images/Pasted%20image%2020240908190604.png)

Observando los puertos que tengo abiertos tengo el puerto 139 y 445 abierto, por ende usaré la herramienta de Enum4linux para ver los usuarios y recursos compartidos que maneja esta máquina:
![](../../../Images/Pasted%20image%2020240908192130.png)
![](../../../Images/Pasted%20image%2020240908192207.png)
![](../../../Images/Pasted%20image%2020240908192344.png)
Tengo de usuarios memehydra y memesploit y de recursos compartidos share_memehydra

Así que les haré fuerza bruta a estos usuarios usando un diccionario creado con la herramienta Cewl de palabras clave de la página web:
![](../../../Images/Pasted%20image%2020240908192802.png)
![](../../../Images/Pasted%20image%2020240908193019.png)
![](../../../Images/Pasted%20image%2020240908193101.png)
Y las credenciales serían:
![](../../../Images/Pasted%20image%2020240908193114.png)

Ahora si accedo al recurso compartido con Smbclient:
![](../../../Images/Pasted%20image%2020240908193456.png)
Y obtengo un .zip, me lo descargo a mi kali:
![](../../../Images/Pasted%20image%2020240908193536.png)

Y para descomprimirlo usé diferentes diccionarios pero no me dio ninguno, así que revisando el código fuente de la página web encontré 3 palabras que estaban escondidas o transparentes y una de ellas es la contraseña del .zip:
![](../../../Images/Pasted%20image%2020240908194018.png)
![](../../../Images/Pasted%20image%2020240908194239.png)
La contraseña para descomprimir el .zip es memesploit_ctf

Leo el .txt que me dejó:
![](../../../Images/Pasted%20image%2020240908194309.png)
Al parecer son las credenciales del puerto 22

Intrusión por el puerto SSH:
![](../../../Images/Pasted%20image%2020240908194406.png)
Y ahora estoy dentro de la máquina

Flag de user:
![](../../../Images/Pasted%20image%2020240908194426.png)

## ESCALADA DE PRIVILEGIOS

Ejecuto sudo -l para ver el binario que puedo usar 
![](../../../Images/Pasted%20image%2020240908194504.png)

Buscaré un archivo llamado login_monitor a ver de que trata:
![](../../../Images/Pasted%20image%2020240909185226.png)

Accedo y leo la carpeta /etc/login_monitor:
![](../../../Images/Pasted%20image%2020240909185244.png)

Miro a ver que permisos tengo en estos archivos:
![](../../../Images/Pasted%20image%2020240909190141.png)
Y no tengo permisos de escritura en ningún archivo, pero si existe un grupo que se llama security el cual tiene permisos sobre el actual directorio, por ende revisaré a que grupos pertenezco:
![](../../../Images/Pasted%20image%2020240909190241.png)
Y efectivamente pertenezco a el grupo security

Entonces lo que haré para escalar privilegios a root es renombrar el archivo actionban.sh y le pondré otro nombre para luego crear una copia de este archivo renombrado para poder editarlo y darle permisos a la /bin/bash:
![](../../../Images/Pasted%20image%2020240909191616.png)
![](../../../Images/Pasted%20image%2020240909191630.png)

Ahora después de hacer este cambio en el archivo actionban.sh haré la intrusión nuevamente a la máquina por el puerto SSH y seguidamente escribiré el comando bash -p:
![](../../../Images/Pasted%20image%2020240909191842.png)
Y listo, ya soy ROOT

Flag de root:
![](../../../Images/Pasted%20image%2020240909191901.png)
