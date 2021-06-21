# WRITEUP Morty

### ESCANEO
#### 	10.150.150.57	Linux	Medium

Empezando a ver los puertos que la maquina tiene abiertos:
![image](https://user-images.githubusercontent.com/78449089/122804781-ecccd280-d28d-11eb-9e1d-8f0e5097400e.png)

Como podemos observar, hay 3 puertos abiertos. Uno de ellos me causa curiosidad al verlo, y el par de ellos parecen también interesantes.
Con el puerto 53 podemos ver si tiene algun dominio activo y ligarlo a nuestros hosts para poder observar su contenido.

### ENUMERACIÓN

Al entrar al sitio podemos observar esta nota:
![image](https://user-images.githubusercontent.com/78449089/122805116-4b924c00-d28e-11eb-9e05-0a2028ae9509.png)

Una vez añadido a nuestro /etc/hosts, pasamos a observar el contenido del sitio mortys
![image](https://user-images.githubusercontent.com/78449089/122805242-767ca000-d28e-11eb-826e-2b9264b489d0.png)

estando dentro podemos ver la siguiente imagen
![image](https://user-images.githubusercontent.com/78449089/122805326-94e29b80-d28e-11eb-80ac-7155d85499c3.png)

en este paso tuve muchos problemas logrando decifrar qué daba a entender esa foto.

El realizar zone transfer con el dominio encontrado podemos ver el resto de dominios interesantes pero eso lo dejaremos para después.

![image](https://user-images.githubusercontent.com/78449089/122807352-189d8780-d291-11eb-997f-6b74a5970f91.png)


### EXPLOTACIÓN 

Después de horas y horas de dolor de cabeza logré dar con algo y fue usando la herramienta "steghide". La contraseña que se nos daba en la imagen era la clave para poder extraer el archivo que venia oculto dentro de la imagen la cual sirve para poder loguearnos dentro del phpmyadmin

![image](https://user-images.githubusercontent.com/78449089/122806233-b3956200-d28f-11eb-8a10-9bd0a4042061.png)

Una vez agregado el dominio al /etc/hosts del panel de morty, podemos encontrar la primera flag.

![image](https://user-images.githubusercontent.com/78449089/122807670-7fbb3c00-d291-11eb-8d01-1f9b0d41b123.png)
Y con las credenciales que recién logramos sacar de la imagen con steghide podremos loguearnos al phpmyadmin. `keytotheuniverse.txt`

Una vez dentro podemos encontrar la segunda flag.

![image](https://user-images.githubusercontent.com/78449089/122807943-d0cb3000-d291-11eb-83de-1022823c2764.png)

después de estar intentando subir una shell, o de intentar crackear la contraseña de root, pude identificar que la versión de phpmyadmin es vulnerable a [LFI](https://www.exploit-db.com/exploits/44924) 

![image](https://user-images.githubusercontent.com/78449089/122809362-a37f8180-d293-11eb-974d-43b415fcaa07.png)

y siguiendo el directorio hacia morty, pude encontrar la FLAG3.txt. En este caso no es necesario rootear la box ya que con el LFI es posible capturar esa ultima bandera.


Contacto: [Linkedin](https://www.linkedin.com/in/jairr/) [Twitter](https://twitter.com/_niggurath_)


Write-ups have been authorized for this machine by the PwnTillDawn Crew! We are just asking you to give us credit by adding a backlink to [wizlynxgroup](https://www.wizlynxgroup.com/) and [Pwntilldawn](https://online.pwntilldawn.com/) in your write-up.
