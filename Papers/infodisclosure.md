#  INFORMATION DISCLOSURE
### Resumen
La divulgación de información es una tema un tanto "peligroso" ya que en la etapa de reconocimiento se necesita saber versiones del software que algún servidor está siendo ejecutado, así como técnologias entre otras cosas. Sí un servidor no está configurado como debería, este empezará a mostrar información de errores, de la versión que tiene ejecutando dicho servicio, de la linea en donde nuestro software tiene errores entre otras cosas.


### Impacto
Imaginemos que un atacante tiene como objetivo un sitio web pero el no sabe absolutamente nada sobre las técnologias que ejecuta el servidor. Si por alguna razón el servidor llegase a dar un error en algún input que el usuario ingrese, este le mostrará algún mensaje mostrando DESCRIPTIVAMENTE el error y es aquí cuando las cosas se ponen tensas.

Si el servicio que está mostrando el error muestra la versión, una simple googleada de la versión y podríamos obtener muchas cosas interesantes ("SERVICIO 1.x.x exploit")
Un ejemplo sencillo sería que algún sitio muestra su "phpinfo" y este tiene como versión alguna que es vulnerable a race condition. Si tenemos algún lfi anteriormente encontrado podríamos generar un [RCE](http://dann.com.br/php-winning-the-race-condition-vs-temporary-file-upload-alternative-way-to-easy_php-n1ctf2018/). 

Esto es simplemente uno de los tantos ejemplos que podría numerar pero ya depende de cuanta imaginación tenga cada atacante para saber utilizar la información divulgada que un sitio este dandonos.


Contacto: [Linkedin](https://www.linkedin.com/in/jairr/) [Twitter](https://twitter.com/_niggurath_)
