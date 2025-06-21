Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240905205257.png)

Observo lo que hay en el puerto 80:

![](../../../Images/Pasted%20image%2020240905205314.png)

Revisaré su código fuente para ver si encuentro alguna pista:

![](../../../Images/Pasted%20image%2020240909192829.png)

Y efectivamente, en la línea 34 y 40 obtengo al parecer unas credenciales jenkins-admin:cassandra

En el puerto 8080 tengo un panel de login hacia un Jenkins:

![](../../../Images/Pasted%20image%2020240905205448.png)

Probé credenciales por defecto pero ninguna me dio resultado, así que usaré las credenciales anteriormente encontradas del código fuente de la página base

![](../../../Images/Pasted%20image%2020240909193024.png)

![](../../../Images/Pasted%20image%2020240909193036.png)

Y ya accedí dentro de la página de Jenkins

Ahora voy a la ruta Manage Jenkins > Script Console y tengo una consola donde puedo escribir comandos, así que me lanzaré una RevShell en formato Groovy hacia mi kali escuchando con Netcat:

![](../../../Images/Pasted%20image%2020240909193349.png)

![](../../../Images/Pasted%20image%2020240909193355.png)

Y recibí la conexión, ahora estoy dentro de la máquina

Tratamiento de la TTY:

![](../../../Images/Pasted%20image%2020240909193506.png)

![](../../../Images/Pasted%20image%2020240909193528.png)

## ESCALADA DE PRIVILEGIOS

Leo los usuarios que contiene esta máquina

![](../../../Images/Pasted%20image%2020240909193739.png)

Y me doy cuenta de que existe el usuario Jenkhack, así que buscaré si depronto existe una carpeta con este nombre:

![](../../../Images/Pasted%20image%2020240909193931.png)

Y existen 2, accederé a la de /var/www:

![](../../../Images/Pasted%20image%2020240909193957.png)

Y contiene una nota esta carpeta, la procedo a leer:

![](../../../Images/Pasted%20image%2020240909194013.png)

Es la contraseña de este usuario, pero está cifrada por ende procederé a descifrarla:

![](../../../Images/Pasted%20image%2020240909194111.png)

Y obtengo la contraseña ya descifrada

Escalo a el usuario jenkhack con estas credenciales:

![](../../../Images/Pasted%20image%2020240909194146.png)

Y listo, ya cambié de usuario

Flag de user:

![](../../../Images/Pasted%20image%2020240909194221.png)

Ejecuto sudo -l para ver el binario que puedo usar

![](../../../Images/Pasted%20image%2020240909194238.png)

Y tengo un script

Lo leo:

![](../../../Images/Pasted%20image%2020240909194507.png)

Y me da una pista de que está usando un archivo llamado bash.sh ubicado en /opt, así que procedo a modificarlo permitiéndome editar el archivo /etc/passwd para borrar la "x" de el usuario root:

![](../../../Images/Pasted%20image%2020240909194651.png)

![](../../../Images/Pasted%20image%2020240909194814.png)

Y ahora ejecuto el script anterior:

![](../../../Images/Pasted%20image%2020240909195111.png)

![](../../../Images/Pasted%20image%2020240909194915.png)

![](../../../Images/Pasted%20image%2020240909195012.png)

Flag de root:

![](../../../Images/Pasted%20image%2020240909195131.png)
