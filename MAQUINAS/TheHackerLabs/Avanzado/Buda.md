Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240812104438.png)

Veamos lo que hay en el puerto 80:

![](../../../Images/Pasted%20image%2020240812104455.png)

Tenemos una plantilla de Apache

Haremos fuzzing web con Gobuster para encontrar direcotorios:

![](../../../Images/Pasted%20image%2020240812104656.png)

Encontramos el directorio /robots.txt

Accedamos a el:

![](../../../Images/Pasted%20image%2020240812104746.png)

Metamos este dominio a nuestro /etc/hosts:

![](../../../Images/Pasted%20image%2020240812104856.png)

Y accedemos a esa página:

![](../../../Images/Pasted%20image%2020240812105018.png)

Tenemos una especie de cuenta regresiva

Haremos fuzzing web con Wfuzz para encontrar sub-dominios:

![](../../../Images/Pasted%20image%2020240812110310.png)

Y encontramos el subdominio dev

Accedamos a esta nueva dirección web:

![](../../../Images/Pasted%20image%2020240812110413.png)

Tenemos un panel de login

Intentemos hacer una inyeccón SQL:

![](../../../Images/Pasted%20image%2020240812110540.png)

![](../../../Images/Pasted%20image%2020240812110605.png)

Y si, si es vulnerable a inyección SQL

Vamos a hacer una inyección SQL con ayuda de la herramienta Sqlmap:
1. Enumeraremos posibles bases de datos
```
sqlmap --url http://dev.budasec.thl --dbs --batch --forms  
```

![](../../../Images/Pasted%20image%2020240812111103.png)

![](../../../Images/Pasted%20image%2020240812110910.png)

Usaré la base de datos buda
2. Enumeraremos posibles tablas, de esta base de datos:

![](../../../Images/Pasted%20image%2020240812111130.png)

![](../../../Images/Pasted%20image%2020240812111002.png)

Tenemos la tabla llamada Users
3. Enumeraremos posibles columnas de esta tabla:

![](../../../Images/Pasted%20image%2020240812111224.png)

![](../../../Images/Pasted%20image%2020240812111238.png)

Tenemos 4 columnas hashed_password, id, password y username

4. Y por último, enumeraremos usuarios de estas 4 columnas:

![](../../../Images/Pasted%20image%2020240812111553.png)

![](../../../Images/Pasted%20image%2020240812111616.png)

Intentamos acceder al puerto FTP 21 con la primera credencial de username y password:

![](../../../Images/Pasted%20image%2020240812111913.png)

Y listo, estamos dentro de el puerto 21

Tratamos de ver el contenido del puerto:

![](../../../Images/Pasted%20image%2020240812112020.png)

Pero hay un Firewall que nos impide leer los archivos 

La solución es: 

![](../../../Images/Pasted%20image%2020240812112204.png)

Tenemos una carpeta llamada ftp

Accedemos a esta carpeta:

![](../../../Images/Pasted%20image%2020240812112308.png)

Nos descargamos todo de la carpeta documents con el comando mget *:

![](../../../Images/Pasted%20image%2020240812112900.png)

Revisemos el knock

![](../../../Images/Pasted%20image%2020240812113118.png)

Tenemos varios numeros de puertos

Haremos golpeo de puertos:

![](../../../Images/Pasted%20image%2020240812113436.png)

Y hacemos nuevamente escaneo de puertos con Nmap:

![](../../../Images/Pasted%20image%2020240812113551.png)

Y evidenciamos que ahora se abrió el puerto 22 SSH

Uno de los documentos que descargamos del puerto 21 fue un .zip, le extraeremos el hash con Zip2john y lo crackeamos con John:

![](../../../Images/Pasted%20image%2020240812113820.png)

Y la contraseña del .zip es manuelito. Lo extraemos el .zip y nos saca un archivo llamado audit2024

Lo leemos este archivo extraido:

![](../../../Images/Pasted%20image%2020240812113916.png)

Es una auditoria

Pero lo que más me llama la atención de este documento son estas credenciales:

![](../../../Images/Pasted%20image%2020240812114724.png)

Probamos con estas credenciales entrar por el puerto SSH:

![](../../../Images/Pasted%20image%2020240812114247.png)

![](../../../Images/Pasted%20image%2020240812114306.png)

Y listo, estamos dentro de la máquina

Flag de user:

![](../../../Images/Pasted%20image%2020240812114322.png)

## ESCALADA DE PRIVILEGIOS

Escribimos id para ver los grupos en los cuales pertenecemos siendo este usuario:

![](../../../Images/Pasted%20image%2020240812115045.png)

Pertenecemos a el grupo docker tambien

Miramos en GTFObins como podemos ser root con esta Shell de docker:

![](../../../Images/Pasted%20image%2020240812115132.png)

Lo ejecutamos:

![](../../../Images/Pasted%20image%2020240812115155.png)

Pero revisando, estamos dentro del contenedor de docker:

![](../../../Images/Pasted%20image%2020240812115449.png)

Entonces editamos el archivo /etc/passwd y borramos la X de root:

![](../../../Images/Pasted%20image%2020240812115524.png)

![](../../../Images/Pasted%20image%2020240812115535.png)

![](../../../Images/Pasted%20image%2020240812115624.png)

Y listo, ahora si somos usuarios ROOT

Flag de root:

![](../../../Images/Pasted%20image%2020240812115220.png)

