# Practica 2
## 3. Instalar la herramienta rsync
### Paso 1
+ Instalar la herramienta RSYNC 
**sudo apt-get install rsync**
+ Creamos un usuario nuevo (mac) y luego añadir los privilegios diciento como si fuese dueño de la carpeta var/www/

![img1](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica2/img1.png)


+ Clonamos por ejemplo la carpeta de la maquina1 a la maquina2
**Todo esto solo se puede hacer si estoy en la maquina2 con usuario mac ya que he indicado el directorio /var/www/**
En las siguiente imagenes se puede observar el procedimiento.
IMPORTANTE NO HACERLO EN USUARIO ROOT

![img2](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica2/img2.png)

+ Con cual con **curl** accedo al los archivos hola.html de la otra maquina
y veo que los dos tienen lo mismo.
Quiere decir que he clonado, correctamente la carpera de la maquina1 a la maquina2

![img3](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica2/img3.png)

## 4. Acceso sin contraseña para ssh
+ Estando como usuario mac y no root, aplico el siguiente comando
**ssh-keygen -b 4096 -t rsa**
y tres intros seguidos, en el cual quiero que se cren por defecto los ficheros en:
el directorio ~/.ssh/  (id_rsa) para contraseñas privadas y (id_rsa.pub) para las publicas 
*si fuesemos root*.
Pero al ser usuario **mac** se guardan en: */home/mac/.ssh/*
En la imagen siguiente se puede ver.
Todo esto lo realizamos en la maquina2.

![img4](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica2/img4.png)

+ Copiamos la clave publica del fichero (id_rsa.pub) a la maquina1 de la siguiente manera.
Importante saber en que ruta estamos para indicar bien donde esta el archivo publico
**ssh-copy-id -i ./mac/.ssh/id_rsa.pub ubuntu1@192.168.56.102**
Si estamos en la ruta, con poner solo .ssh/ seria suficiente.
Nos pedira la contraseña de la maquina1 (usuario--> ubuntu1) y listo.
El usuario he tenido que poner ubuntu1 que es el de la maquina1.
+ Ya podemos hacer una conexion desde la maquina2 a maquina 1 sin que nos pida la 
contraseña.
En la imagen se ve como hago: **ssh ubuntu1@192.168.56.102** y no me pide contraseña

![img5](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica2/img5.png)

Para cerrar la conexion, simplemente exit ssh.

## 5. Programas tareas con crontab
Edito el fichero /etc/crontab y añado la nueva regla
**47 20 * * * mac  rsyc -avz -e ssh ubuntu1@192.168.56.102:/var/www/ /var/www/**
y efectivamente se actualiza de marena automatica.
IMPORTANTE DESTACAR, que si no hubiesemos echo el paso 4, esto seria imposible porque 
nos pediria contraseña del usuario maquina1, cosa que no podemos hacerlo desde crontab.

![img6](https://github.com/miguelUGR/swap1718/blob/master/Practicas/practica2/img6.png)

