Escaneo de puertos con Nmap:

![](../../../Images/Pasted%20image%2020240904194324.png)

Observo lo que hay en el puerto 80:

![](../../../Images/Pasted%20image%2020240904194355.png)

Me pide una supuesta clave la cual no tengo

Así que revisaré el puerto 7777:

![](../../../Images/Pasted%20image%2020240904194657.png)

Miro el secret:

![](../../../Images/Pasted%20image%2020240904202403.png)

Tengo una historia muy larga, pero me llama la atención la frase "super_secure_password", la intetaré usar como palabra clave a ver que sucede:

![](../../../Images/Pasted%20image%2020240904202809.png)

![](../../../Images/Pasted%20image%2020240904202832.png)

Y efectivamente logré acceder dentro de la página web

Pruebo colocar un comando en la parte de abajo en el campo de texto de "Configuration Identifier":

![](../../../Images/Pasted%20image%2020240904203035.png)

![](../../../Images/Pasted%20image%2020240904203044.png)

Y la web me responde exitosamente a mi comando

Por ende intentaré subir una Reverse Shell:

![](../../../Images/Pasted%20image%2020240904203412.png)

Ahora me pongo a la escucha con Netcat y ejecuto mi shell:

![](../../../Images/Pasted%20image%2020240905202055.png)

Y estoy dentro de la máquina

### Tratamiento de la TTY
```
script /dev/null -c bash 
Ctrl z
stty raw -echo; fg 
reset xterm 
export SHELL=bash
export TERM=xterm
```

![](../../../Images/Pasted%20image%2020240905202146.png)

![](../../../Images/Pasted%20image%2020240905202213.png)

## ESCALADA DE PRIVILEGIOS

En la carpeta  /home/codebad/secret tengo una adivinanza que dice:

![](../../../Images/Pasted%20image%2020240905202501.png)

La respuesta seria "malware"

Puede ser una contraseña esta pista, por ende veo los usuarios que contiene la máquina e intento acceder a uno de ellos con esta posible contraseña:

![](../../../Images/Pasted%20image%2020240905202811.png)

Y efectivamente es una contraseña y ahora soy el usuario codebad

Ejecuto sudo -l para ver el binario que puedo usar

![](../../../Images/Pasted%20image%2020240905202841.png)

Puedo ser el usuario metadata con un script llamado "code" ubicado en /home/codebad:

![](../../../Images/Pasted%20image%2020240905203325.png)

Es un script que realiza cualquier comando, por ende me lanzaré una Reverse Shell para escalar al usuario metadata:

![](../../../Images/Pasted%20image%2020240905203845.png)

![](../../../Images/Pasted%20image%2020240905203851.png)

Y listo, ahora soy el usuario metadata

Flag de user:

![](../../../Images/Pasted%20image%2020240905203949.png)

Realizo una búsqueda de archivos para metadata y encuentro varias carpetas, entre ellas:

![](../../../Images/Pasted%20image%2020240905204201.png)

![](../../../Images/Pasted%20image%2020240905204211.png)

Asi que accedo a esa carpeta:

![](../../../Images/Pasted%20image%2020240905204513.png)

Me da una pequeña pista la cual nos dice que la contraseña de metadata puede ser metadatosmalos

Ejecuto sudo -l para ver el binario que puedo usar (Colocando la contraseña de metada anteriormente decifrada):

![](../../../Images/Pasted%20image%2020240905204653.png)

Puedo ser root con el binario C89

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020240905204742.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020240905204804.png)

Y listo, ya soy ROOT

Flag de root:
<<<<<<< HEAD

![](../../../Images/Pasted%20image%2020240905204826.png)

=======
>>>>>>> 5ba57b1 (Subida de archivos editada con Python para corregir formato de imagenes)

![](../../../Images/Pasted%20image%2020240905204826.png)

