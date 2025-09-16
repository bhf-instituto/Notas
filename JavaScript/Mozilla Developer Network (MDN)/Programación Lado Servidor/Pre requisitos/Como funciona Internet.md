## Una simple red

Cuando dos ordenadores necesitan comunicarse, tienes que vincularlos, ya sea físicamente (por lo general con un [cable de Ethernet](http://en.wikipedia.org/wiki/Ethernet_crossover_cable)) o de forma inalámbrica (por ejemplo por [WiFi](http://en.wikipedia.org/wiki/WiFi) o sistema de [Bluetooth](http://en.wikipedia.org/wiki/Bluetooth)). Todos los ordenadores modernos pueden soportar cualquiera de este tipo de conexiones.
![[internet-schema-1-1.png]]
Una red no se limita a dos ordenadores, se pueden conectar tantos como se desee; sin embargo, rápidamente se volverá más compleja. Por ejemplo, para conectar diez ordenadores, se necesitarían ¡45 cables y unos nueve conectores por ordenador!
![[internet-schema-2.png]]
Para resolver este problema, cada ordenador en una red está conectado a una pequeña computadora especial llamada enrutador o router (en inglés). Este enrutador cumple una única función: tal como hace un señalizador en una estación de tren, el router se encarga de asegurar que el mensaje enviado desde un ordenador emisor llegue al destino correcto. Para que el ordenador B reciba un mensaje desde el ordenador A, este último debe enviarlo primero al router, que a su vez lo remite al ordenador B asegurándose que dicho mensaje no sea entregado a otro ordenador C.

Una vez que agregamos un enrutador al sistema, nuestra red de 10 ordenadores solo requiere 10 cables: un enchufe para cada ordenador y un enrutador con 10 enchufes.

![Diez ordenadores con un router](https://developer.mozilla.org/es/docs/Learn_web_development/Howto/Web_mechanics/How_does_the_Internet_work/internet-schema-3.png)

## Una red de redes

Hasta aquí todo es más o menos simple, pero ¿qué sucede al conectar cientos, miles, millones de ordenadores entre sí?. Por supuesto un solo _enrutador_ no puede escalar tanto, pero, si lees cuidadosamente, dijimos que un _enrutador_ es como un pequeño ordenador, entonces ¿qué nos impide conectar dos _enrutadores_ a la vez?. Nada: hagámoslo.

![Dos routers conectados entre sí](https://developer.mozilla.org/es/docs/Learn_web_development/Howto/Web_mechanics/How_does_the_Internet_work/internet-schema-4.png)

Conectando ordenadores a enrutadores y luego enrutadores entre sí, podemos escalar infinitamente.

![Routers interconectados](https://developer.mozilla.org/es/docs/Learn_web_development/Howto/Web_mechanics/How_does_the_Internet_work/internet-schema-5.png)

Una red así se acerca mucho a lo que llamamos Internet, pero hay algo que nos falta. Construimos esa red para nuestros propios propósitos. Hay otras redes ahí fuera: tus amigos, tus vecinos, cualquiera puede tener su propia red de ordenadores. Pero no es posible instalar cables entre tu casa y el resto del mundo, así que ¿cómo puedes manejar esto? Bueno, ya hay cables conectados a tu casa, por ejemplo, la energía eléctrica y el teléfono. La infraestructura telefónica ya conecta tu casa con cualquier persona en el mundo, así que es el cable perfecto que necesitamos. Para conectar nuestra red a la infraestructura telefónica, necesitamos un equipo especial llamado _modem_. Este _modem_ convierte la información de nuestra red en información manejable por la infraestructura telefónica y viceversa.

![Un router conectado a un modem](https://developer.mozilla.org/es/docs/Learn_web_development/Howto/Web_mechanics/How_does_the_Internet_work/internet-schema-6.png)

Entonces estamos conectados a la infraestructura telefónica. El siguiente paso es enviar el mensaje desde nuestra red a la red que queremos llegar. Para lograr eso, conectaremos nuestra red a un proveedor de servicios de internet (ISP de sus siglas en inglés Internet Service Provider). Un ISP es una empresa que gestiona algunos enrutadores especiales interconectados, que también pueden acceder a enrutadores de otros ISP. Así, el mensaje de nuestra red es llevada a través de la red de redes de ISP, hasta la red de destino. Internet consiste en toda esta infraestructura de redes.

![stack de Internet al completo](https://developer.mozilla.org/es/docs/Learn_web_development/Howto/Web_mechanics/How_does_the_Internet_work/internet-schema-7.png)

## Encontrando ordenadores
Si deseas enviar un mensaje a una computadora, debes especificar a cuál. Es por ello que todo ordenador conectado a una red cuenta con una dirección única que lo identifica, llamada "dirección IP" (siendo "IP" las siglas en inglés de _Internet Protocol_, o _Protocolo de Internet_). Esta dirección está compuesta por una serie de cuatro números separados por puntos, por ejemplo: `192.168.2.10`.

Para los ordenadores es un identificador simple, pero los humanos tenemos mayor dificultad para recordar este tipo de direcciones. Con el propósito de convertir esta serie numérica en algo que podamos asociar con mayor facilidad a la dirección IP se utiliza lo que conocemos como _nombre de dominio_. Por ejemplo, `google.com` es el nombre de dominio utilizado para la dirección IP `173.194.121.32`. Así, usar un nombre de dominio es la manera más fácil para nosotros de identificar un ordenador a través de Internet.

![Mostrar cómo un nombre de dominio sirve como alias a una dirección IP](https://developer.mozilla.org/es/docs/Learn_web_development/Howto/Web_mechanics/How_does_the_Internet_work/dns-ip.png)

## Internet y la web

Como puedes notar, cuando navegamos por la web con un navegador, normalmente utilizamos el nombre de dominio para llegar a un sitio web. ¿Significa eso que Internet y la Web son la misma cosa? No es tan simple. Como vimos, Internet es una infraestructura técnica que permite que miles de millones de ordenadores estén conectadas entre sí. Algunos de estos ordenadores, llamados _servidores web_ son capaces de enviar mensajes inteligibles a los navegadores. Por tanto _Internet_ es una infraestructura, mientras que la _Web_ es un servicio construido sobre dicha infraestructura. Cabe señalar que existen otros servicios soportados por Internet, como es el correo electrónico e [IRC](https://developer.mozilla.org/es/docs/Glossary/IRC).


