Escaneo de puertos:  
![](../../../Images/Pasted%20image%2020240830095444.png)

Veo lo que corre en el puerto 80:  
![](../../../Images/Pasted%20image%2020240830095509.png)

En el botón Login tengo:  
![](../../../Images/Pasted%20image%2020240830095532.png)

Colocaré una comilla como username a ver si es vulnerable a SQL injection:  
![](../../../Images/Pasted%20image%2020240830180624.png)  
![](../../../Images/Pasted%20image%2020240830180633.png)  
Y efectivamente es vulnerable.

Así que usaré SQLmap para explotar esta vulnerabilidad:  
1.  Enumeraremos posibles bases de datos

```
sqlmap --url http://172.17.0.4/login.html --dbs --batch --forms 
```
![](../../../Images/Pasted%20image%2020240830180837.png)  
![](../../../Images/Pasted%20image%2020240830180846.png)

Usaremos la base de datos Users.

2. Enumeraremos posibles tablas, de esta base de datos:  
![](../../../Images/Pasted%20image%2020240830180925.png)  
![](../../../Images/Pasted%20image%2020240830180932.png)

Tenemos la tabla llamada Usuarios.

3. Enumeraremos posibles columnas de esta tabla:  
![](../../../Images/Pasted%20image%2020240830181005.png)  
![](../../../Images/Pasted%20image%2020240830181011.png)

Tenemos 3 columnas id, password y username.

4. Y por último, enumeraremos usuarios de estas 3 columnas:  
![](../../../Images/Pasted%20image%2020240830181052.png)  
![](../../../Images/Pasted%20image%2020240830181101.png)

Recordando tengo el puerto 22 SSH abierto probaré con estas credenciales a ver cual me deja acceder por allí:  
![](../../../Images/Pasted%20image%2020240830181329.png)

Y las credenciales q me sirven son las de pepe, ya estoy dentro de la máquina.

## ESCALADA DE PRIVILEGIOS

Miro los binarios que contiene la máquina:  
```
find / -perm -4000 -ls 2>/dev/null
```

![](../../../Images/Pasted%20image%2020240830181531.png)

Y me llama la atención el binario Grep y Ls.

Usaré el binario ls para ver las posibles carpetas que contiene /root:  
![](../../../Images/Pasted%20image%2020240901155617.png)

Tengo un .hash

Ahora miraré en GTFOBins como puedo escalar con el binario Grep:  
![](../../../Images/Pasted%20image%2020240830181608.png)

Lo ejecuto apuntando hacia este archivo .hash q acabo de encontrar:  
![](../../../Images/Pasted%20image%2020240901155719.png)

Obtengo el hash de root.

Lo crackeo con la herramienta crackstation:  
![](../../../Images/Pasted%20image%2020240901160117.png)

Y tengo una contraseña llamada spongebob34.

Escalo a root con esta contraseña:  
![](../../../Images/Pasted%20image%2020240901160145.png)

Y listo, ya soy ROOT

