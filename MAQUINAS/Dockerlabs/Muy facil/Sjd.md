Escaneo de puertos con Nmap
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```
![](../../../Images/Pasted%20image%2020250128183640.png)

Observo lo que corre por el puerto 80:
![](../../../Images/Pasted%20image%2020250128183703.png)
No encontré nada relevante en la página web

Así que procedo a hacer fuzzing web con Gobuster en búsqueda de directorios:
```
gobuster dir -u http://172.17.0.2 -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -x html,php,txt
```
![](../../../Images/Pasted%20image%2020250128184448.png)
Accedo al directorio pass.txt:
![](../../../Images/Pasted%20image%2020250128184536.png)
Son unos posibles usuarios con unas contraseñas codificadas, las decodifico:
![](../../../Images/Pasted%20image%2020250128184616.png)
![](../../../Images/Pasted%20image%2020250128184626.png)
![](../../../Images/Pasted%20image%2020250128184636.png)
Todas están hechas en base64

Intento acceder por el puerto 22 SSH con las primeras credenciales encontradas:
![](../../../Images/Pasted%20image%2020250128184718.png)
Y listo, estoy dentro de la máquina

## ESCALADA DE PRIVILEGIOS

Intento escalar al usuario admin con la contraseña ya decodificada
![](../../../Images/Pasted%20image%2020250128184755.png)
Ahora soy el usuario admin

Intento escalar al root con la contraseña ya decodificada
![](../../../Images/Pasted%20image%2020250128184840.png)
Y listo, soy ROOT
