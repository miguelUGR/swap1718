# Práctica 5. Replicación de bases de datos MySQL  
## 3. Crear una BD e insertar datos  
Importante quitar la configuracion del IPTABLES de la practica 4 y dejarlo todo abierto para no tener problemas o configurarlo adecuadamente.
(ejecuto el script.sh y quito la linea del crontab (vi /etc/crontab) ).  

En primer lugar ponemos en el terminal: mysql -uroot -p  
Nos pedira la contrasela : (admin) y luego es sencillamente seguir los pasos.

+ create database contactos;
+ use contactos;
+ show tables; (para comprobar que esta vacia pero creada)
+ create table datos(nombre varchar(100),tlf int);
+ show tables;
+ insert into datos(nombre,tlf) values ("pepe",95834987);
+ select * from datos; (seleccionamos todo de la tabla datos)  

Se puede observar en la siguiente imagen los siguientes pasos a realizar  

![img1](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica5/img1.png)  

## 4. Replicar una BD MySQL con mysqldump

En el servidor de BD principal (maquina1) hacemos:  
+ mysql -u root –p  
+ FLUSH TABLES WITH READ LOCK;
+ quit

Luego aplicamos el siguiente comando para hacer la copia de seguridad en el directorio por ejemplo /tmp/ (es el directorio que luego puedo enviar a otras maquinas). En la siguiente imagen podemos ver el proceso.

![img2](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica5/img2.png)  

Como habíamos bloqueado las tablas, debemos desbloquearlas (quitar el “LOCK”):
+ mysql -u root –p
+ UNLOCK TABLES;
+ quit

Ya podemos ir a la máquina esclavo (maquina2, secundaria) para copiar el archivo .SQL con todos los datos salvados desde la máquina principal (maquina1):  

![img3](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica5/img3.png)  

### 4.1 Restaurar dicha copia de seguridad en la segunda máquina (clonado manual de la BD), de forma que en ambas máquinas esté esa BD de forma idéntica

![img4](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica5/img4.png)  

Para verificar que se ha copia todo correctamente realizamos los siguiente:
+ mysql -uroot -p  
+ use contactosCopia; (base de datos en M2 (ojo pk no seria contactos, eso es en M1))
+ select * from datos;


## 5. Replicación de BD mediante una configuración maestro-esclavo

Lo  que queremos es que la máquina de respaldo actualice la base de datos ante cualquier modificación que ocurra en el servido de producción. 
Para ello, configuraremos el servidor de respaldo para ser "esclavo", y al servidor de producción como maestro. Para esto seguimos los siguientes pasos:  

Modificamos el siguiente archivo: **vi /etc/mysql/mysql.conf.d/mysqld.cnf**  
+ Comentamos el parámetro bind-address que sirve para que escuche a un servidor: #bind-address 127.0.0.1

+ Indicamos el archivo donde almacenar el log de errores: log_error = /var/log/mysql/error.log.
De esta forma, si por ejemplo al reiniciar el servicio cometemos algún error en el archivo de configuración, en el archivo de log nos mostrará con detalle lo sucedido:  

+ Descomentamos la linea y establecemos el identificador del servidor: server-id = x (donde x será: "1" en el caso del maestro, y "2" en el caso del esclavo).

+ Indicamos la ruta al registro binario: log_bin = /var/log/mysql/bin.log.  (o simplemente el que hay lo descomentamos)  
+ Tras guardar el archivo, reiniciamos el servicio: /etc/init.d/mysql restart  

### Configuro la M2. 

La configuración es similar a la del maestro maquina M1, con la diferencia de que el server-id en esta ocasión será 2(esclavo)

## 5.1 Configuracion de usuario.  
Una vez más, si no da ningún error, habremos tenido éxito.  
Podemos volver al maestro para crear un usuario y darle permisos de acceso para la replicación.  
Entramos en mysql y ejecutamos las siguientes sentencias:  
+ create user  esclavo identified by 'esclavo';
+ grant replication slave on  *.* to 'esclavo'@'%' identified by 'esclavo';
+ FLUSH PRIVILEGES;
+ FLUSH TABLES;
+ FLUSH TABLES WITH READ LOCK;
+ SHOW MASTER STATUS;

En la imagen se puede observar los pasos realizados.  

![img5](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica5/img5.png)  


Nos vamos a la maquina esclavo (M2) y le damos los datos de la maquina maestro (M1), procedemos a  ejecutar lo siguiente:
+ mysql -uroot -p  

En la imagen se ve todo el procedimiento.


![img6](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica5/img6.png)  

Iniciamos el esclavo con :
+ start slave;


Por último, volvemos al maestro y volvemos a activar las tablas para que puedan meterse nuevos datos en el maestro:
+ UNLOCK TABLES;

Ahora, si queremos asegurarnos de que todo funciona perfectamente y que el esclavo no tiene ningún problema para replicar la información, nos vamos al esclavo y con la siguiente orden:
+ show slave status\g 

Me muestra un mensaje de error:(se ve en la imagen siguiente)

![img7](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica5/img7.png)  

En mi caso sin embargo, el valor era null y mostraba un mensaje de error: "The slave I/O thread stops because master and slave have equal MySQL server UUIDs" (Ambos programas tenían el mismo identificador de hebra de MySQL, y tenía que ser diferente). Para solucionar este problema basta con ir a: /var/lib/mysql y borrar el fichero auto.cnf Basta con realizarlo en una de las máquinas (esclavo o maestro, indistintamente), mejor esclavo, 
+ quit y reiniciamos **/etc/init.d/mysql restart**.
+ mysql -uroot -p  
+ start slave;  

Todo funciona correctamente.  
OJO!! que el position siempre cambia y debemos modificarlo y tambien debemos poner el FIle correspondiente.

![img8](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica5/img8.png)  


Metemos nuevos datos en la base de datos del MAESTRO y comprobamos si se ha actualizado en el esclavo.