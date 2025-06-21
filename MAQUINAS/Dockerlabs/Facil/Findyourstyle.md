Escaneo de puertos con Nmap
```
nmap -p- -sC -sV -sS --min-rate 5000 -n -vvv -Pn 172.17.0.2 -oN escaneo
```
![[Pasted image 20250115101105.png]]
Tengo un único puerto abierto

Reviso lo que corre por el puerto 80:
![[Pasted image 20250115101210.png]]
Tengo un Drupal

Veré la versión a la cual me estoy enfrentando:
![[Pasted image 20250115102542.png]]
Es un Drupal versión 8

Buscaré en Metasploit un exploit a esta versión:
![[Pasted image 20250115102716.png]]Y existe ese

Lo selecciono y leo los requerimientos que me pide:
![[Pasted image 20250115102815.png]]
Y relleno el espacio de RHOSTS
![[Pasted image 20250115102855.png]]

Y ejecuto el exploit:
![[Pasted image 20250115102950.png]]
 Y listo, estoy dentro de la máquina

## ESCALADA DE PRIVILEGIOS

Observo los usuarios que contiene esta máquina:
![[Pasted image 20250115103133.png]]
Son 2 ballenita y root

Buscaré donde está ubicado el archivo settings.php para ver si en él tendré alguna pista:
```
find / -name settings.php  2>/dev/null 
```
![[Pasted image 20250115103334.png]]
Lo leo:
![[Pasted image 20250115103451.png]]
Tengo la contraseña del usuario ballenita

Escalo al usuario ballenita:
![[Pasted image 20250115103651.png]]
Ahora soy el usuario ballenita

Ejecuto sudo -l para ver el binario que puedo usar 
![[Pasted image 20250115103733.png]]
Puedo ser root con el binario Grep y Ls

Con el binario Ls, veré lo que contiene la carpeta /root:
![[Pasted image 20250115104020.png]]

Miro en GTFOBins como puedo escalar con el binario Grep:
![[Pasted image 20250115103803.png]]

Lo ejecuto para leer ese .txt:
![[Pasted image 20250115104148.png]]
Y puede que sea la contraseña de root

Intento escalar a root con esta contraseña encontrada:
![[Pasted image 20250115104221.png]]
Y listo, ya soy ROOT

