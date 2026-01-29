Escaneo  de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```

![](../../../Images/Pasted%20image%2020251124154144.png)

Observo qué corre en el puerto 80 HTTP:

![](../../../Images/Pasted%20image%2020251124154331.png)

Es una imagen con una plantilla sin nada más y al final hay un texto, nada relevante encontré en esta página web

Como no encontré nada más, procederé a realizar fuzzing web usando Gobuster en búsqueda de directorios:
```
gobuster dir -u http://172.17.0.2/ -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -x html,php,txt,log
```

![](../../../Images/Pasted%20image%2020251125081105.png)

Me encuentro con un directorio llamado /notes, accederé a él:

![](../../../Images/Pasted%20image%2020251125081530.png)

Leeré el note.txt que existe aquí:

![](../../../Images/Pasted%20image%2020251125081604.png)

Es un texto el cuál me da unas credenciales, las probaré en el puerto 22 SSH, para ver si pertenecen allí:

![](../../../Images/Pasted%20image%2020251125081816.png)

Y no, en este caso estas no son las credenciales correctas

Posiblemente ese usuario "dev" si exista, pero esa contraseña no pertenece a él, por ende realizaré un ataque de fuerza bruta con Hydra para encontrar la contraseña apuntando hacia el puerto SSH:
```
hydra -l dev -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2 -t 64 
```

![](../../../Images/Pasted%20image%2020251125082354.png)

Y la contraseña correcta del usuario "dev" es "computer".

Intrusión por el puerto 22 SSH con estas credenciales:

![](../../../Images/Pasted%20image%2020251125082449.png)

Y listo, ahora estoy dentro de la máquina

## ESCALADA DE PRIVILEGIOS

Dentro del directorio /opt/scripts/pycache_ _  ,me encuentro con un .pyc pero al tratar de leerlo observo que está codificado, por esa razón leeré sus "strings", a ver qué palabras clave me extrae:

![](../../../Images/Pasted%20image%2020251125085047.png)

Observo que me da un usuario "admin" y su contraseña; Procederé a escalar de usuario con estas credenciales:

![](../../../Images/Pasted%20image%2020251125085148.png)

Y ahora me convertí en el usuario "admin"

Ejecuto sudo -l para ver el binario que puedo usar para seguir escalando:

![](../../../Images/Pasted%20image%2020251125085559.png)

Puedo ser root con el binario Pip3

Miro en GTFOBins como puedo escalar con este binario:

![](../../../Images/Pasted%20image%2020251125085623.png)

Lo ejecuto:

![](../../../Images/Pasted%20image%2020251125085517.png)

Y listo, ya soy ROOT

