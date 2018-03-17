## Ejercicio T2.2:
### Buscar frameworks y librerías para diferentes lenguajes que permitan hacer aplicaciones altamente disponibles con relativa facilidad.

JAVASCRIPT:
+ jQuery: es una librería de JavaScript que facilita el desarrollo con éste lenguaje. Sólo sirve para hacer más fácil algunas expresiones.

+ Bootstrap: Fue creado por un empleado de twitter, Su popularidad se le debe a la facilidad de maquetación que supone usar sus clases CSS, así como el diseño visual de los principales elementos HTML de cualquier página web.

+ Founation: Similar a bootstrap. Uno de los frameworks JavaScript, tiene un apartado específico para WebApps,

+ Backbone JS: Es extremadamente ligero comparado con otros, y te permite utilizar el sistema de plantillas que quieras, otras librerías, etc.. es bastante flexible. Se apoya en Underscore y jQuery a la hora de abordar soluciones JavaScript y no tiene una curva de aprendizaje tan elevada

**RUBY**: creación de  aplicaciones web de forma rápida y con un mínimo esfuerzo.

+ Ruby on Rails: Es una de las frameworks más populares para Ruby. El objetivo principal de Rails es favorecer la convención antes de la configuración. Como muchas otras frameworks, se especializa en disminuir la repetición de código. Requiere una configuración mínima,

+ Sinatra: Con Sinatra puedes estructurar tu aplicación según el problema al que quieras darle solución. Básicamente, posees bastante control al trabajar con Sinatra.

+ Hobbit: Esta micro-framework ha sido desarrollada teniendo como inspiración el DSL de Sinatra. El objetivo por el cual se ha creado esta framework es para evitar que el usuario cree líneas de código innecesarias. Asimismo, al usar Hobbit podrás entender el uso de Rack y sus extensiones.
+ HANAMI, NYNY,CUBA etc etc.

**PHP:**

+ Aura: Muy usado por aquellos desarrolladores que prefieren ir haciéndose sus propias librerías.
+ Slim:Nos encontramos ante uno de los mejores micro-frameworks de código abierto, sofisticado e intuitivo , se ha desarrollado desde cero, sin reutilizar “partes” de otros frameworks de este listado.


+ Phalcon: Un framework diseñado para la velocidad . Está escrito en C desde sus cimientos y es con seguridad el framework más rápido existente hoy día. Ofrece la mayoría de los recursos actuales necesarios para desarrollar un proyecto ágil y fresco ( routing, controladores, vista de plantillas, ORM, caché , etc…).

**PYTHON:**
+ Django: es un framework para aplicaciones web gratuito y open source, escrito en Python, te ayudan a desarrollar sitios web más fácil y rápidamente.
+ Web2py, TurboGears, Grok, Giotto.

***

## Ejercicio T2.3:

### ¿Cómo analizar el nivel de carga de cada uno de los subsistemas en el servidor?

+ Top permite conocer en tiempo real la actividad del procesador, así como la lista con los procesos que hacen un mayor uso de la CPU.

+ Google Page Speed. es una herramienta de análisis que ofrece Google que sirve para aumentar el rendimiento de tu sitio web. Basta introducir la URL del sitio para que genere un reporte personalizado con un listado de sugerencias para mejorar el tiempo de carga en navegadores de escritorio y en dispositivos móviles. Dispone de extensiones para Firefox y Chrome.

+ Apache Benchmark. Indicamos la URL a testear, el número de peticiones que queremos realizar y el número de peticiones concurrentes .

+ Nagios es un sistema de monitorización de redes ampliamente utilizado, de código abierto, que vigila los equipos (hardware) y servicios (software) que se especifiquen, alertando cuando el comportamiento de los mismos no sea el deseado. Entre sus características principales figuran la monitorización de servicios de red (SMTP, POP3, HTTP, SNMP...), la monitorización de los recursos de sistemas hardware (carga del procesador, uso de los discos, memoria, estado de los puertos...), independencia de sistemas operativos, posibilidad de monitorización remota mediante túneles SSL cifrados o SSH, y la posibilidad de programar plugins específicos para nuevos sistemas.”

## Ejercicio T2.4:
### Buscar ejemplos de balanceadores software y hardware (productos comerciales).  
Entre los balanceadores de carga software más conocidos están HAProxy y Nginx, También están  
+ Zen Load Balancer
+ Octopus Load Balancer
+ Apache: mod_proxy_balancer
+ Varnish
+ Servitux  

### Buscar productos comerciales para servidores de aplicaciones.  
Servidores de aplicaciones Java EE:

+ WebLogic de Oracle.
+ WebSphere de IBM.
+ JBoss AS de JBoss (RedHat).
+ Geronimo y TomEE de Apache.

### Buscar productos comerciales para servidores de almacenamiento.

+ Servidor almacenamiento DELL
+ Dell Compellent FS8600
+ HP ConvergedSystem 200-HC StoreVirtual System
+ IBM EXP2500 Storage Enclosure

# Ejercicio TEMA 3


## Ejercicio T3.1:
**Buscar con qué órdenes de terminal o herramientas gráficas podemos configurar bajo Windows y bajo Linux el enrutamiento del tráfico de un servidor para pasar el tráfico desde una subred a otra.**  

En Windows Server 2008 y 2012 se incluye un "Servicio de enrutamiento y acceso remoto" que se puede agregar al servidor mediante un simple asistente.  

En Linux, la configuración de red está en /etc/network/interfaces, archivo donde podemos establecer las configuraciones de red. Para crear una ruta, podemos utilizar el comando route y añadirla o borrarla con add o del respectivamente. Si lo que queremos es mantener estas rutas tras reiniciar el servidor, añadimos la ruta al archivo /etc/network/interfaces mediante la línea up route add -net ....

## Ejercicio T3.2:  
**Buscar con qué órdenes de terminal o herramientas gráficas podemos configurar bajo Windows y bajo Linux el filtrado y bloqueo de paquetes.**

Cisco CSS 11501	Brocade ServerIron ADX 1000  
Precio	£4.019.99	£6,479.99  
Peso	8,16Kg	17Kg  
RAM	-	8GB  
Conexiones	Ethernet, Fast Ethernet	Ethernet, Fast Ethernet, Gigabit Ethernet  
Metetodo. Autent.	DES, T.DES, RSA, RC4, TLS 1.0, SSL 3.0/2.0	SSL  
