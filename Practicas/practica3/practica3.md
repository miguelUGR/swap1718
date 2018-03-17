# Practica 3
## 3.1 Instalar nginx en Ubuntu Server
### PRIMERO LUGAR:  
Creo una nueva maquina, balanceadora, solo instalando servidor ssh.  
Instalo:  
+ curl
+ ufw--> habilito y abro los puerto 22 y 80  
Si hago curl a otras maquinas, funciona, pero de otras maquinas a mi no.
Debo hacer la  instalacion nginx.  

Concretamente, para hacer la instalación, se recomienda ejecutar las siguientes órdenes:
+ sudo apt-get update 
+ sudo apt-get dist-upgrade 
+ sudo apt-get autoremove

**Importate tener el puerto 80 libre.**  
Osea parar apache: service apache2 stop  
Para no tener problemas en un futuro, desistalo apache.  
O lo mejor es lo que he echo, una maquina sin apache.  
###Continuamos con la instalacion  
+ sudo apt-get install nginx 
* sudo systemctl start nginx

## 3.2. Balanceo de carga usando nginx

Para configuar nginx tendremos que modificar el contenido de **/etc/nginx/conf.d/default.conf** eliminandolo y poner lo que citaré a continuación.  
Puede que el archivo default.conf no exista, por lo que deberemos crearlo.

![img1](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica3/img1.png)

Una vez que lo tenemos configurado, podemos lanzar el servicio nginx como sigue:
+ sudo systemctl start nginx o service nginx start

Para probar que esto funciona deberemos de usar el comando de cURL hacia nuestras máquinas para ver si estas se están viendo.  
En la imagen siguinte hago un curl a la pagina hola.html que tengo en cada una de las maquinas  
### IMPORTANTE desactivar que la maquina 2 actualice el var/www/html de la maquina 1  

![img2](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica3/img2.png)

**NOTA:** Importante descomentar la siguiente linea del archivo (/etc/nginx/nginx.conf) que se ve en la siguiente imagen.  
Es debido para que no tome como defecto las pagina de nginx del valanceador.
![img3a](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica3/img3a.png)
### Otras opciones para nginx

Podremos añadir diferentes características a nginx. Si queremos que las máquinas soporte más carga de trabajo usaremos una configuración como esta:  

**Cada 3 peticiones la máquina 1 soporta una y la 2, dos.**  
upstream apaches  
{  
  server 172.16.168.130 weight=1;  
  server 172.16.168.131 weight=2;  
}  
Para hacer que la misma máquina soporte las peticiones provenientes de otra y no cambie de servidor, deberemos ajustarlo ya que sino podrían darse errores.  

upstream apaches  
{  
  ip_hash;  
  server 172.16.168.130;  
  server 172.16.168.131;  
}  
Para realizar una conexion con persistencia de múltiples peticiones HTTP en lugar de abrir una conexión nueva cada vez deberemos de usar:  

upstream apaches  
{  
  server 172.16.168.130;  
  server 172.16.168.131;  
  keepalive 3;  
}  
Entre otras muchas opciones que podemos encontrar en el guión del pdf.  
A continuacion lo que voy a hacer es añadir (WEIGHT) a la configuracion del archivo y  que la maquina con ip=...108 le lleguen el doble de solicitudes que a la maquina con ip=...103.  

Cojo un 4 maquina(modifico la ip para que sea distinta) y envio solicitudes al balanceador en las siguientes imagenes se puede ver todo
perfectamente.  
La maquina que actua como cuarta es la del abajo derecha, balanceador= abajo izq.   
**TAMBIEN:** puedo hacer peticiones al balanceador a si mismo


![img4](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica3/img4.png)


![img5](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica3/img5.png)

