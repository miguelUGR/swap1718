# Practica 3
## 3.1 Instalar nginx en Ubuntu Server

Concretamente, para hacer la instalación, se recomienda ejecutar las siguientes órdenes:
+ sudo apt-get update 
+ sudo apt-get dist-upgrade 
+ sudo apt-get autoremove

**Importate tener el puerto 80 libre.**

Osea parar apache: service apache2 stop 
Para no tener problemas en un futuro, desistalo apache.
+ sudo apt-get install nginx 
* sudo systemctl start nginx

## 3.2. Balanceo de carga usando nginx

Para configuar nginx tendremos que modificar el contenido de **/etc/nginx/conf.d/default.conf** eliminandolo y poner lo que citaré a continuación. Puede que el archivo default.conf no exista, por lo que deberemos crearlo.
