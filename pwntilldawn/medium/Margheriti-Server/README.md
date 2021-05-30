# WRITEUP Margheriti-Server

### ESCANEO
#### 	10.150.150.145	Linux	Medium

```
PORT     STATE SERVICE REASON
22/tcp   open  ssh     syn-ack ttl 63
80/tcp   open  http    syn-ack ttl 63
3306/tcp open  mysql   syn-ack ttl 63
```
Al escanear los puertos podemos observar que solamente hay 3 puertos abiertos.
El puerto 80 tiene un CMS (wordpress) y tiene el puerto mysql abierto, por lo tanto ya me doy una idea de por donde van las cosas.

![image](https://user-images.githubusercontent.com/78449089/120096773-f762d480-c0f2-11eb-91af-01cdc10f3638.png)


### ENUMERACIÓN

Ahora pasaremos a realizar un fuzzeo de directorios y archivos para ver si hay algo interesante.

![image](https://user-images.githubusercontent.com/78449089/120097637-3a26ab80-c0f7-11eb-8e1d-3c57de15b6ff.png)

y en el fuzzeo vemos que hay un archivo muy interesante llamado "Backup.zip".
Dentro del zip, encontramos la primera FLAG14 y el respaldo de la información del cms donde descargamos ese zip.

![image](https://user-images.githubusercontent.com/78449089/120097842-47906580-c0f8-11eb-8b8e-14bdc4fa5e3a.png)

y como era de esperar, dentro del archivo wp-config.php encontramos las variables de conexión hacia la DB que se aloja en el mysql.

![image](https://user-images.githubusercontent.com/78449089/120097929-abb32980-c0f8-11eb-88c4-642b501229ab.png)


### EXPLOTACIÓN 

Ahora que tenemos credenciales validas para la conexión hacía el servicio mysql, las usaremos para subir una webshell y también para cambiar el user del cms wordpress porque posiblemente exista información valiosa dentro de ahí (nunca saltarse nada ya que las flags en PTD están en todos los servicios usualmente)

![image](https://user-images.githubusercontent.com/78449089/120098060-66432c00-c0f9-11eb-9ebb-62ce73b176a5.png)

Una vez estando dentro de la base de datos llamada "wordpress", podemos encontrar otra de las flags, que en este caso sería la FLAG32. También podemos observar que existe un usuario llamado eAdmin, al cual le cambiaré la contraseña para acceder al cms y ver si podemos encontrar algo que valga la pena.


![image](https://user-images.githubusercontent.com/78449089/120098463-bcb16a00-c0fb-11eb-80de-8ae67abd68c5.png)

Matando 2 pajaros de un tiro, subí la webshell y estando ahi mismo también cambié la contraseña del admin por "12345" para entrar al wordpress y al entrar podemos ver otra FLAG23 (como dije, siempre hay flags en casi todos los servicios)

![image](https://user-images.githubusercontent.com/78449089/120098524-fbdfbb00-c0fb-11eb-8900-42b3e4ad3bcf.png)

`user: eAdmin password: 9r*WNyK7GUJ($bwi8G `

ahora que tenemos una webshell, sólo nos queda ver si podemos ver alguna llave id_rsa y algo que nos haga acceder al servicio ssh, sino, nos queda lanzarnos una reverse shell para poder ejecutar comandos en esa maquina.

![image](https://user-images.githubusercontent.com/78449089/120101124-1b7de000-c10a-11eb-8e43-c9b7d2ece096.png)

Una vez dentro de la box, accedimos como www-data (dentro de ahi estaba la FLAG6) y pudimos encontrar la ultima flag(FLAG22). Lo raro de esta box es que no es necesario rootearla para tener todas las banderas... 



Contacto: [Linkedin](https://www.linkedin.com/in/jair-rodriguezz/) [Twitter](https://twitter.com/_niggurath_)


Write-ups have been authorized for this machine by the PwnTillDawn Crew! We are just asking you to give us credit by adding a backlink to [wizlynxgroup](https://www.wizlynxgroup.com/) and [Pwntilldawn](https://online.pwntilldawn.com/) in your write-up.
