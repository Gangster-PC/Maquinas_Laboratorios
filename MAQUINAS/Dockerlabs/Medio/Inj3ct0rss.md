Escaneo de puertos con Nmap:

![](../../../Images/Pasted%20image%2020240821204229.png)

Observo lo que hay en el puerto 80:

![](../../../Images/Pasted%20image%2020240821204229.png)

Veo lo que hay en el puerto 80:
(Subida de archivos editada con Python para corregir formato de imagenes)

![](../../../Images/Pasted%20image%2020240821204337.png)

Me llama la atención lo de inyecciones SQL, así que probaré hacerle una al panel de login

![](../../../Images/Pasted%20image%2020240821205017.png)

':admin123

![](../../../Images/Pasted%20image%2020240821205033.png)

Y efectivamente la web es vulnerable a inyecciones SQL

Voy a hacer una inyección SQL con ayuda de la herramienta Sqlmap:

1. Enumeraré posibles bases de datos

```
sqlmap --url http://dev.budasec.thl --dbs --batch --forms  
```

![](../../../Images/Pasted%20image%2020240821205609.png)

![](../../../Images/Pasted%20image%2020240821205617.png)

Usaré la base de datos injectors_db

2. Enumero posibles tablas, de esta base de datos:

![](../../../Images/Pasted%20image%2020240821205801.png)

![](../../../Images/Pasted%20image%2020240821205750.png)

Tengo la tabla llamada Users

2. Enumeraré posibles columnas de esta tabla:

![](../../../Images/Pasted%20image%2020240821205833.png)

![](../../../Images/Pasted%20image%2020240821210112.png)

Tengo 3 columnas id, password y username

3. Y por último, enumeraré usuarios de estas 4 columnas:

![](../../../Images/Pasted%20image%2020240821210209.png)

![](../../../Images/Pasted%20image%2020240821210750.png)

Observo que el tal "no_mirar_en_este_directorio" en realidad puede ser un directorio, accedo a él:

![](../../../Images/Pasted%20image%2020240821210702.png)

Me descargamos el .zip:

![](../../../Images/Pasted%20image%2020240821210821.png)

Le extraigo el hash al .zip con Zip2john y lo crackeo con John:

![](../../../Images/Pasted%20image%2020240821210939.png)

La contraseña para descomprimir el secret.zip es computer

Lo descomprimo y leo el .txt q contiene:

![](../../../Images/Pasted%20image%2020240821211020.png)

Intrusión por el puerto 22 SSH, con estas credenciales que me dejó el .txt:

![](../../../Images/Pasted%20image%2020240821211119.png)

Y ahora estoy dentro de la máquina

Flag de user:

![](../../../Images/Pasted%20image%2020240821211155.png)

## ESCALADA DE PRIVILEGIOS

Ejecuto sudo -l para ver el binario que podré usar:

![](../../../Images/Pasted%20image%2020240821211637.png)

Puedo ser el usuario capa con el binario Busybox

Miro en GTFOBins como podemos escalar con este binario:

![](../../../Images/Pasted%20image%2020240821211621.png)

Lo ejecutamos pero de una forma distinta:

![](../../../Images/Pasted%20image%2020240821211929.png)

Encontramos la contraseña del usuario capa

Escalamos al usuario capa:

![](../../../Images/Pasted%20image%2020240821212001.png)

Ejecutamos sudo -l para ver el binario que podremos usar 

![](../../../Images/Pasted%20image%2020240821212019.png)

Podemos ser root con el binario Cat

Miramos en GTFOBins como podemos escalar con este binario:

![](../../../Images/Pasted%20image%2020240821212053.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020240821212144.png)

Obtengo la id_rsa de root, la paso a mi kali:

![](../../../Images/Pasted%20image%2020240821212235.png)

Intrusión por el puerto 22 SSH con el usuario root usando la id_rsa:

![](../../../Images/Pasted%20image%2020240821212316.png)

Y listo, ya soy ROOT

Flag de root:

![](../../../Images/Pasted%20image%2020240821212348.png)
