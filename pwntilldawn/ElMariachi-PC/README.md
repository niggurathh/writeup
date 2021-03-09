# WRITEUP ELMARIACHI-PC 

### ENUMERACIÓN
#### 10.150.150.69	Windows	Easy
Al hacer un escaneo de puertos encontramos lo siguiente:
```
PORT      STATE SERVICE       REASON
135/tcp   open  msrpc         syn-ack ttl 127
139/tcp   open  netbios-ssn   syn-ack ttl 127
445/tcp   open  microsoft-ds  syn-ack ttl 127
3389/tcp  open  ms-wbt-server syn-ack ttl 127
5040/tcp  open  unknown       syn-ack ttl 127
49664/tcp open  unknown       syn-ack ttl 127
49665/tcp open  unknown       syn-ack ttl 127
49666/tcp open  unknown       syn-ack ttl 127
49667/tcp open  unknown       syn-ack ttl 127
49668/tcp open  unknown       syn-ack ttl 127
49669/tcp open  unknown       syn-ack ttl 127
49670/tcp open  unknown       syn-ack ttl 127
50417/tcp open  unknown       syn-ack ttl 127
60000/tcp open  unknown       syn-ack ttl 127
```

Lo primero que me llama la atención es el puerto 3389 y el puerto 445, por lo tanto empezaré por el servicio SMB y después al servicio RDP

```
┌─[✗]─[niggurath@parrot]─[~/pwntilldawn]
└──╼ #smbmap -H 10.150.150.69
[!] Authentication error on 10.150.150.69
```

Como podemos observar, el servicio no está permitiendo sesiones nulas a recursos compartidos, por lo tanto descartemos este puerto por el momento.
Al intentar acceder al servicio RDP, podemos observar que sí nos da conexión pero como no tenemos usuarios en este momento no podemos hacer mucho. En otra ocasión lo intentaremos ya que podríamos intentar fuerza bruta por RDP.


![imagen](img/e.PNG)


Ahora, intentaré enumerar las versiones y sus servicios para ver si hay algo interesante.

```
PORT      STATE SERVICE       VERSION
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds?
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  msrpc         Microsoft Windows RPC
49670/tcp open  msrpc         Microsoft Windows RPC
50417/tcp open  msrpc         Microsoft Windows RPC
60000/tcp open  unknown
| fingerprint-strings: 
|   FourOhFourRequest: 
|     HTTP/1.1 404 Not Found
|     Content-Type: text/html
|     Content-Length: 177
|     Connection: Keep-Alive
|     <HTML><HEAD><TITLE>404 Not Found</TITLE></HEAD><BODY><H1>404 Not Found</H1>The requested URL nice%20ports%2C/Tri%6Eity.txt%2ebak was not found on this server.<P></BODY></HTML>
|   GetRequest: 
|     HTTP/1.1 401 Access Denied
|     Content-Type: text/html
|     Content-Length: 144
|     Connection: Keep-Alive
|     WWW-Authenticate: Digest realm="ThinVNC", qop="auth", nonce="msikv/6c5UDI4UcC/pzlQA==", opaque="m2yqFi2usv3AY2yatYSTRmyNPAplB8C1oC"
|_    <HTML><HEAD><TITLE>401 Access Denied</TITLE></HEAD><BODY><H1>401 Access Denied</H1>The requested URL requires authorization.<P></BODY></HTML>
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port60000-TCP:V=7.91%I=7%D=3/9%Time=60471155%P=x86_64-pc-linux-gnu%r(Ge
SF:tRequest,179,"HTTP/1\.1\x20401\x20Access\x20Denied\r\nContent-Type:\x20
SF:text/html\r\nContent-Length:\x20144\r\nConnection:\x20Keep-Alive\r\nWWW
SF:-Authenticate:\x20Digest\x20realm=\"ThinVNC\",\x20qop=\"auth\",\x20nonc
SF:e=\"msikv/6c5UDI4UcC/pzlQA==\",\x20opaque=\"m2yqFi2usv3AY2yatYSTRmyNPAp
SF:lB8C1oC\"\r\n\r\n<HTML><HEAD><TITLE>401\x20Access\x20Denied</TITLE></HE
SF:AD><BODY><H1>401\x20Access\x20Denied</H1>The\x20requested\x20URL\x20\x2
SF:0requires\x20authorization\.<P></BODY></HTML>\r\n")%r(FourOhFourRequest
SF:,111,"HTTP/1\.1\x20404\x20Not\x20Found\r\nContent-Type:\x20text/html\r\
SF:nContent-Length:\x20177\r\nConnection:\x20Keep-Alive\r\n\r\n<HTML><HEAD
SF:><TITLE>404\x20Not\x20Found</TITLE></HEAD><BODY><H1>404\x20Not\x20Found
SF:</H1>The\x20requested\x20URL\x20nice%20ports%2C/Tri%6Eity\.txt%2ebak\x2
SF:0was\x20not\x20found\x20on\x20this\x20server\.<P></BODY></HTML>\r\n");
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
```

Vemos varios RPC pero el servicio más llamativo es el del puerto 60000 ya que al parecer podemos acceder a el por un navegador web.
Al intentar entrar a ese servicio por un navegador, nos lanza el siguiente mensaje:


![imagen2](img/imagen_2021-03-09_002010.png)


`ThinVNC crea una vista de tu Escritorio y la sirve a través del navegador web.`
Eso es lo que nos dice google al hacer una busqueda de ese servicio. Por lo tanto lo primero que debemos hacer al encontrar un servicio, es buscar si tiene alguna vulnerabilidad pública. Al ejecutar "Searchsploit thinVNC" lo que encuentra es una vulnerabilidad tipo "Authentication Bypass" (CVE-2019-17662)


![imagen3](img/imagen_2021-03-09_002702.png)


Al leer sobre esa vulnerabilidad, encontramos [este script](https://www.exploit-db.com/exploits/47519) para automatizar el proceso pero lo que haremos es simplemente acceder por el navegador hacia esa ruta y ver qué sucede.


![imagen2](img/bb.PNG)


Lo que hemos encontrado son unas credenciales para acceder directamente al servicio.
```
User=desperado
Password=TooComplicatedToGuessMeAhahahahahahahh
```

entrando nos encontramos con la siguiente pantalla. 


![imagen2](img/c.PNG)


y como anteriormente vimos que el servicio RDP estaba abierto, simplemente ingresamos la dirección ip y vualá, tenemos acceso remoto.
Al entrar podemos ver que la flag está a la vista y ya tendriamos completado nuestro mariachi.


![imagen2](img/d.PNG)


Contacto: [Linkedin](https://www.linkedin.com/in/jair-rodriguezz/) [Twitter](https://twitter.com/_niggurath_)


Write-ups have been authorized for this machine by the PwnTillDawn Crew! We are just asking you to give us credit by adding a backlink to [wizlynxgroup](https://www.wizlynxgroup.com/) and [Pwntilldawn](https://online.pwntilldawn.com/) in your write-up.
