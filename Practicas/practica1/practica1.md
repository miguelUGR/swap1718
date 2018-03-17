# Practica 1

## 1.Configuración:

Las maquinas virtuales, tenemos que tener dos adaptadores  de red.
Una tipo nat y otra anfitriona.

![img1](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica1/img1.png)

Para que salga NOMBRE: vboxnet0 y poder aceptar, debo de ir a PREFERENCIAS de virtual vox,
En Red y pestaña Redes—> solo anfitrión, añadimos una mas.
De esta manera, ya tendríamos la posibilidad de darle aceptar como se ve en la imagen anterior.

![img2](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica1/img2.png)

## 2. Después de la instalación
Una vez instalado el Ubuntu server debemos de instalar vi.
Luego modificar el archivo interfaces estando como root.
**vi /etc/network/interfaces**
Como en la configuración de Ubuntu server pusimos que la red principal fuese la enp0s3 entonces:
Tenemos la ret NAT==enp0s3 y la anfitrión==enp0s8. Debemos poner primero en el fichero la
Nat , que actua como principal y luego la anfitrion en estática y lo demás.

IMPORTANTE , quitar el GATEWAY porque da problemas para que la maquina se conecte a internet.

![img3](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica1/img3.png)

El rango de la ip debemos de mirar en :
-Preferencias—Red—Red solo-anfitrion—Boton derecho sobre la red que hay y en EDITAR—Servidor DHCP.

![img4](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica1/img4.png)

Observamos que en Direccion del servidor, tiene marcado 192.168.56.100
Eso quiere decir que las direcciones  que pongamos a las maquinas (adaptador red-anfitrion) deben de estar por encima de 100 y por debajo de 254.

### Cosas a tener en cuenta.

Tener instalado el ufw para iniciar el cortafuegos y hábilitar los puertos ssh y apache
**apt-get install ufw  
Ufw enable— ufw allow 22(o tambien http) — ufo allow 80—  
ufw status para comprobarlo.**

![img5](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica1/img5.png)

### Para finalizar:

Lo que nos pide es hacer ping entre ellas y ha google.
Tambien una conexión ssh y la utilización de curl.
Por supuesto debemos instalar **apt-get install curl.**

Debemos crear un archivo html llamado hora en /var/www/html/hola.html
Que dentro contenga :

<HTML> 
<BODY> 
Esto FUNCIONA
</BODY> </HTML>

+ Para la comunicacion mediante curl:
  curl http:// direccionIp maquina a la que quieres acceder
  y se mostrara el archivo html que hemos creado con anterioridad.

  ![img6](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica1/img6.png)

+ Para la comunicación entre las maquinas:
	ping “direccion ip de la tarjeta red enp0s8” de la otra maquina.

  ![img7](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica1/img7.png)

+ Para la comunicación con el exterior(osea internet)
	ping www.google.com
	si tenemos problemas, modificamos las ips, de las dos maquinas.

![img8](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica1/img8.png)

+ Para comunicación entre ellas mediante ssh
   No estando en modo root ponemos:
	- ssh NombreUsuarioOtraMaquina@direccionIpOtraMaquina
	- yes
  - contraseña del usuario de la otra maquina.
  - **Si Las dos maquinas tubiesen mismo nombre de usuario** bastaria con poner:
    **ssh direcionIPOtraMaquina** 

    Este tipo de conexion, sirve :
    Yo estoy en maquina2 conectado mediante ssh a maquina1 y hago desde maquina2
    por ejemplo una creacion de un archivo, realmente se crea en la maquina1.

  ![img9](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica1/img9.png)
