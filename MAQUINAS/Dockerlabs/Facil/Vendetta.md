Escaneo de puertos con Nmap:

![](../../../Images/Pasted%20image%2020250106175007.png)

Reviso el puerto 80

![](../../../Images/Pasted%20image%2020250106175225.png)

Y observo que se resalta mucho la V, tal vez puede que sea un posible usuario

Accedo a la base de datos Mysql por el puerto 3306 con este usuario y su contraseña ie168:
```
mysql -h 172.17.0.2 -u V -pie168
```

![](../../../Images/Pasted%20image%2020250106182753.png)

Y me salta el error SSL es requerido

Por ende modifico el comando:
```
mysql -h 172.17.0.2 -u V -pie168 --skip-ssl
```

**![](../../../Images/Pasted%20image%2020250106182911.png)**

Y listo, estoy dentro de la base de datos Mysql

Observo cuales bases de datos existen:

![](../../../Images/Pasted%20image%2020250106183029.png)

Me llama la atención la base de datos llamada BTN, accedo a ella:

![](../../../Images/Pasted%20image%2020250106183124.png)

Observo sus tablas:

![](../../../Images/Pasted%20image%2020250106183149.png)

Entro a la tabla Users:

![](../../../Images/Pasted%20image%2020250106183223.png)

Y obtengo unas credenciales

Pruebo estas credenciales por el puerto 22 SSH:

![](../../../Images/Pasted%20image%2020250106183314.png)

Y listo, accedí a la máquina

## ESCALADA DE PRIVILEGIOS

Ejecuto sudo -l para ver el binario que puedo usar

![](../../../Images/Pasted%20image%2020250106183411.png)

Puedo ser root con el binario Nano

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020250106183442.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020250106183635.png)

![](../../../Images/Pasted%20image%2020250106183700.png)

![](../../../Images/Pasted%20image%2020250106183738.png)

Y listo, ya soy ROOT
