# Practica 4 Asegurar la granja web
## 2 Instalar un certificado SSL autofirmado para configurar el acceso por HTTPS
Esto se debe hacer en las dos maquinas que tenemos como servidoras.
1º ACTIVAMOS SSL.  
+ a2enmod ssl 
2º REINICIAMOS APACHE.  
+ service apache2 restart  
3º CREAMOS DIRECTORIO LA CERTIFICACION. 
+ mkdir /etc/apache2/ssl  
4º CREAMOS LOS FICHEROS  
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

Comporvamos que funciona realizando en la misma maquina  M1 **curl -k https://Ip:maquina1**  y  en M2 **curl -k https://Ip:maquina2** 


+ Copiamos los certificados a la M2 y al balanceador (TODO ESTO COMO USUARIO ROOT).  
**scp /etc/apache2/ssl/* balanceador@192.162.56.110:/tmp/** porque es un lugar donde puede escribir todo el mundo  
y luego en la maquina balanceadora lo paso a un sitio privado como **mv /tmp/apache.crt  /etc/ssl/**  para que nadie pueda tocar.  
Lo mismo con M2:  
**scp /etc/apache2/ssl/* ubuntu1@192.162.56.108:/tmp/**
**mv /tmp/apache.crt  /etc/apache2/ssl/**   
**mv /tmp/apache.key  /etc/apache2/ssl/**  

Si me da problemas como en la imagen:
![img6](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica4/img6.png)  
borramos el archivo  que esta en la ruta **.ssh/known_hosts**   o  
me voy a la direcion **/root** y ejecuto: **rm -rf .ssh**  es una eliminacion recursiva incluyendo la carpeta




+ Configuramos el balanceador Nginxpara que desde un cliente acepte peticiones https  

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


