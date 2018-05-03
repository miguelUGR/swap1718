# Practica 4 Asegurar la granja web
## 2 Instalar un certificado SSL autofirmado para configurar el acceso por HTTPS
Esto se debe hacer en las dos maquinas que tenemos como servidoras.
1º ACTIVAMOS SSL.  
+ a2enmod ssl 
2º REINICIAMOS APACHE.  
+ service apache2 restart  
3º CREAMOS DIRECTORIO LA CERTIFICACION. 
+ mkdir /etc/apache2/ssl  
4º CREAMOS LOS FICHEROS (esta regla SOLO EN UNA MAQUINA)  
+ openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout
/etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt  
Nos pedira datos como el continente,provincia,ciudad,correo etc.
![img1](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica4/img1.png)  

5º Activamos el sitio default--ssl y reiniciamos apache:
+ a2ensite default-ssl  y   service apache2 reload
### IMPORTANTE que la maquina M2,M1 ,BALANCEADOR Y CLIENTE tengan IP DISTINTAS  

EN LA MAQUINA M2 hago lo mismo menos la creacion de los certificados porque ya lo hemos hecho en la M1.  
1º ACTIVAMOS SSL.  
+ a2enmod ssl 
2º REINICIAMOS APACHE.  
+ service apache2 restart  
3º CREAMOS DIRECTORIO LA CERTIFICACION. 
+ mkdir /etc/apache2/ssl  
4º Activamos el sitio default--ssl y reiniciamos apache:
+ a2ensite default-ssl   y   service apache2 reload


Editamos el archivo de configuración del sitio default-ssl: en las dos maquinas.  
+ vi /etc/apache2/sites-available/default-ssl
Y agregamos estas lineas debajo de donde pone SSLEngine on:
+ SSLCertificateFile /etc/apache2/ssl/apache.crt SSLCertificateKeyFile /etc/apache2/ssl/apache.key  

![img2](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica4/img2.png)  

Comprovamos que funciona realizando en la misma maquina  M1 **curl -k https://Ip:maquina1**  y  en M2 **curl -k https://Ip:maquina2** 
![img3](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica4/img3.png) 

+ Copiamos los certificados a la M2 y al balanceador (TODO ESTO COMO USUARIO no ROOT).  
**scp /etc/apache2/ssl/* balanceador@192.168.56.110:/tmp/** porque es un lugar donde puede escribir todo el mundo  
y luego en la maquina balanceadora lo paso a un sitio privado como **mv /tmp/apache.crt  /etc/ssl/**  para que nadie pueda tocar.  
Lo mismo con M2:  
**scp /etc/apache2/ssl/* ubuntu1@192.168.56.108:/tmp/**
**mv /tmp/apache.crt  /etc/apache2/ssl/**   
**mv /tmp/apache.key  /etc/apache2/ssl/**  

Tenemos que tener encuenta que el comando scp son peticiones SSH.

Si me da problemas como en la imagen:
![img6](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica4/img6.png)  
borramos el archivo  que esta en la ruta **.ssh/known_hosts**   o  
me voy a la direcion **/root** y ejecuto: **rm -rf .ssh**  es una eliminacion recursiva incluyendo la carpeta


+ Configuramos el balanceador Nginx para que desde un cliente acepte peticiones https  

Configuración nginx /etc/nginx/conf.d/default.conf  

![img4](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica4/img4.png)  

Para hacer una comprobacion desde un cliente a traves del balanceador debemos abrir el puerto 443.  
Como se ve en la imagen no esta operativo y debemos abrirlo.
![img5](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica4/img5.png)  

Añadimos la regla:
+ **iptables -A INPUT -m state --state NEW -p tcp --dport 443 -j ACCEPT** pero tiene un defecto que no se guarda.

Lo mejor es: me creo el script.sh con los contenidos que expongo a continuacion  y envio el script a la maquina balanceador y M2 y cliente
Contendido del script:  
### (1) Eliminar todas las reglas (configuración limpia) iptables -F
iptables -X
iptables -Z
iptables -t nat -F
### política por defecto: aceptar todo
iptables −P  
iptables −P  
iptables −P
iptables -L
INPUT ACCEPT  
OUTPUT ACCEPT  
FORWARD ACCEPT  
iptables -n -v

Ejecuto el script para tener todo abierto y no tener problemas.
Ya si puedo hacer desde el cliente un curl -k https://Ip_balanceador y me redirtije a las maquinas.




## EJERCICIO 2: Configurar las reglas del cortafuegos con IPTABLES para asegurar el acceso a uno de los servidores web, permitiendo el acceso por los puertos de HTTP y HTTPS a dicho servidor

Esta configuracion lo que hago es que desde la maquina cliente(ip...105) pueda hacer solo ella peticiones https, conexiones SHH a la maquina M1.  
Importante que suele dar problemas, es que si capo para que la salida  M1 vaya a la maquina CLIENTE(ip.....105) me da problemas.
Ha veces es mejor no capar tanto la maquina M1.  
Las peticiones http hacia M1 las tengo bloqueadas. En el script se puede ver la configuracion.

Cosas a tener en cuenta. Yo quise hacer que la M1 solo recibiese y mandase peticiones al balanceador (ip..110) pero las maquinas finales que el balanceador
maneja, no deben de tener iptables instaladas porque da problemas IPTABLES con nginx.

![img9](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica4/img9.png)

## Opción 1

Añadimos nuestro script a /etc/init.d/

mv miScript.sh /etc/init.d/ 

chmod +x /etc/init.d/miScript.sh
Si esta opción, que es la mas "clásica", no funcionase podriamos probar la siguiente opción:

## Opción 2

Hacemos los pasos de la Opción 1 y además lo siguiente:

ln -s /etc/init.d/miScript.sh /etc/rc.d/  

O mejor:
+ vi /etc/rc.local  y al final del archivo, pongo:  
+ sh /rutaDondEEstaMiScript/Nombre_script.sh  


![img7](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica4/img7.png)  


Lo que se ve en la imagen es que la ultima regla que haga al iniciar es ejecutar el script que esta en esa ruta.  
Despues hacemos un: **iptables -L -n -v** y se puede ver la configuracion en la siguiente imagen.  
Y

![img8](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica4/img8.png)  

 
## Opción 3

Como tercera opción, podemos hacer que el demonio cron se encargue de que se ejecute el script añadiendo al crontab la siguiente linea:

### vi /etc/crontab 
**@reboot /home/usuario/miScript.sh**
Para comprobar que las reglas firewall funcionan correctamente, en la defensa de la práctica, se mostrará cómo es imposible acceder mediante ssh a dicha máquina, a no ser que se haga desde la que tiene la dirección que está exenta de dicha regla.


