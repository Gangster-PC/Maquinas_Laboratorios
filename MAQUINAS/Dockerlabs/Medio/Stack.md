Escaneo de puertos con Nmap:
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```
![](../../../Images/Pasted%20image%2020250130200322.png)

Observo lo que corre por el puerto 80:
![](../../../Images/Pasted%20image%2020250130200358.png)

Leyendo el código fuente de esta web:
![](../../../Images/Pasted%20image%2020250130201820.png)
Así que buscaré donde puedo explotar la vulnerabilidad Local File Inlusion para encontrar la contraseña

Haré fuzzing web usando Gobuster en busqueda de directorios:
![](../../../Images/Pasted%20image%2020250130201925.png)
Lo encontré es /file.php

Ahora con ayuda de Wfuzz encontraré el Path Traversal al cuál es vulnerable esta web:
```
wfuzz -c -w /usr/share/wordlists/seclists/Fuzzing/LFI/LFI-Jhaddix.txt -u '172.17.0.2/file.php?file=FUZZ' --hl 
```
![](../../../Images/Pasted%20image%2020250130202401.png)
Lo encontré, ahora apuntaré hacia el directorio de la contraseña para encontrarla:
![](../../../Images/Pasted%20image%2020250130202435.png)
Entonces esa sería la contraseña del usuario bob

Intrusión por el puerto 22 SSH con esas credenciales:
![](../../../Images/Pasted%20image%2020250130202525.png)
Estoy dentro de la máquina

