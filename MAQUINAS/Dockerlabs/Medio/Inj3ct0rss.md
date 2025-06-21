Escaneo de puertos:
<<<<<<< HEAD

![](../../../Images/Pasted%20image%2020240821204229.png)

Observo lo que hay en el puerto 80:
=======

![](../../../Images/Pasted%20image%2020240821204229.png)

Veamos lo que hay en el puerto 80:
>>>>>>> 5ba57b1 (Subida de archivos editada con Python para corregir formato de imagenes)

![](../../../Images/Pasted%20image%2020240821204337.png)

Nos llama la atención lo de inyecciones SQL, así que probaremos hacerle una al panel de login

![](../../../Images/Pasted%20image%2020240821205017.png)

':admin123

![](../../../Images/Pasted%20image%2020240821205033.png)

Y efectivamente la web es vulnerable a inyecciones SQL

Vamos a hacer una inyección SQL con ayuda de la herramienta Sqlmap:
1. Enumeraremos posibles bases de datos

```
sqlmap --url http://dev.budasec.thl --dbs --batch --forms  
```

![](../../../Images/Pasted%20image%2020240821205609.png)
![](../../../Images/Pasted%20image%2020240821205617.png)

Usaré la base de datos injectors_db

2. Enumeraremos posibles tablas, de esta base de datos:
![](../../../Images/Pasted%20image%2020240821205801.png)
![](../../../Images/Pasted%20image%2020240821205750.png)

Tenemos la tabla llamada Users

2. Enumeraremos posibles columnas de esta tabla:

![](../../../Images/Pasted%20image%2020240821205833.png)
![](../../../Images/Pasted%20image%2020240821210112.png)

Tenemos 3 columnas id, password y username

3. Y por último, enumeraremos usuarios de estas 4 columnas:

![](../../../Images/Pasted%20image%2020240821210209.png)
![](../../../Images/Pasted%20image%2020240821210750.png)

Vemos que el tal "no_mirar_en_este_directorio" en realidad puede ser un directorio, accedamos a el

![](../../../Images/Pasted%20image%2020240821210702.png)

Nos descargamos el .zip:

![](../../../Images/Pasted%20image%2020240821210821.png)

Le extraemos el hash al .zip con Zip2john y lo crackeamos con John:

![](../../../Images/Pasted%20image%2020240821210939.png)

La contraseña para descomprimir el secret.zip es computer

Lo descomprimimos y leemos el .txt q contiene:

![](../../../Images/Pasted%20image%2020240821211020.png)

Intrusión por el puerto 22 SSH, con estas credenciales que nos dejó el .txt:

![](../../../Images/Pasted%20image%2020240821211119.png)

Y estamos dentro de la máquina

Flag de user:

![](../../../Images/Pasted%20image%2020240821211155.png)
## ESCALADA DE PRIVILEGIOS

Ejecutamos sudo -l para ver el binario que podremos usar:

![](../../../Images/Pasted%20image%2020240821211637.png)

Podemos ser el usuario capa con el binario Busybox

Miramos en GTFOBins como podemos escalar con este binario:

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

Lo ejecutamos:

![](../../../Images/Pasted%20image%2020240821212144.png)

Obtenemos la id_rsa de root, la pasamos a nuestro kali:

![](../../../Images/Pasted%20image%2020240821212235.png)

Intrusión por el puerto 22 SSH con el usuario root usando la id_rsa:

![](../../../Images/Pasted%20image%2020240821212316.png)

Y listo, ya somos ROOT

Flag de root:

![](../../../Images/Pasted%20image%2020240821212348.png)
