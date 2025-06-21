Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240825130922.png)

Veamos lo que corre en el puerto 80:

![](../../../Images/Pasted%20image%2020240825131012.png)

Tenemos una especie de web de una universidad

Dando click en el botón "Admisiones" tenemos un panel de registro:

![](../../../Images/Pasted%20image%2020240825131514.png)

Donde podemos subir una imagen

Intentamos con una imagen que tenia guardada en mi kali y nos sale:

![](../../../Images/Pasted%20image%2020240825131924.png)

![](../../../Images/Pasted%20image%2020240825131911.png)

Es curioso ya que efectivamente subimos una imagen .jpg pero igual nos sale error

Entonces subiremos una Reverse Shell en formato .phtml

![](../../../Images/Pasted%20image%2020240826083814.png)

![](../../../Images/Pasted%20image%2020240826083827.png)

Y se subió satisfactoriamente 

Ahora buscaremos el directorio donde se subió nuestra RevShell, que comúnmente es en el directorio /uploads:

![](../../../Images/Pasted%20image%2020240826083921.png)

Y aquí está

Me pondré a la escucha con Netcat y daré click en el archivo:

![](../../../Images/Pasted%20image%2020240826084007.png)

Y ahora estoy dentro de la máquina

Tratamiento de la TTY:

![](../../../Images/Pasted%20image%2020240826084129.png)

![](../../../Images/Pasted%20image%2020240826084148.png)

## ESCALADA DE PRIVILEGIOS

Enumeraremos los binarios que contiene esta máquina:

![](../../../Images/Pasted%20image%2020240826084407.png)

Tenemos el binario base64, el cual usaremos para leer la id_rsa de el usuario james:

![](../../../Images/Pasted%20image%2020240826084543.png)

Me paso esta id_rsa a mi kali, le extraigo el hash con Ssh2john y lo crackeo con John:

![](../../../Images/Pasted%20image%2020240826084644.png)

Y la contraseña es bowwow

Intrusión por el puerto 22 SSH con el usuario james y su respectiva id_rsa:

![](../../../Images/Pasted%20image%2020240826084814.png)

Ya estamos dentro de la máquina nuevamente pero ahora desde el puerto 22

Flag de user:

![](../../../Images/Pasted%20image%2020240826084846.png)

Ejecutamos sudo -l para ver el binario que podremos usar 

![](../../../Images/Pasted%20image%2020240826084917.png)

Podemos ser root con el binario Python ejecutando un script ubicado en la carpeta /opt:

![](../../../Images/Pasted%20image%2020240826085026.png)

Para poder escalar con este example.py, explotaremos un Python Library Hijacking, creando un archivo .py con el nombre de la librería que se está importando, en este caso es "hashlib" y le escribiremos una bash para que la ejecute con privilegios:

![](../../../Images/Pasted%20image%2020240826085322.png)

Ahora ejecutamos el script con python:

![](../../../Images/Pasted%20image%2020240826085358.png)

Y ya somos ROOT

Flag de root:

![](../../../Images/Pasted%20image%2020240826085424.png)

