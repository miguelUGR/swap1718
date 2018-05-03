# Configuración hosting web:

##  Instalación de Sofware de control de hosting

### Configuracion del servidor

Instalamos una maquina vistual con el ubuntu server completamente vacio.
+ apt-get update && apt-get -y upgrade  
+ apt-get install -y unzip  
+ cd /tmp
+ wget --no-check-certificate -O installer.tgz "https://github.com/servisys/ispconfig_setup/tarball/master"  
+ tar zxvf installer.tgz  
+ cd *ispconfig*  
+ bash install.sh  

Nos preguntara:  
+ Si ha detectato correctamente el sistema. [YES/NO]  
+ Versiones de mysql  
+ Servidor de ficheros[apache,nginx]  
+ Instalacion de Xcahce,instalar phpmyadmin,escoger servidor de correo[dovecot o, courier] etc.  
(hemos seleccionado siempre la opcion por defecto).  

Hay dos opciones de isntalacion[Estandar o experto], la unica diferencia que hay es que el experto hayq ue hacerlo todo manualmente   
y el estandar esta guiado con el script.

Hemos cogido el estandar y debemos instalar [jailkit] y los certificados SSL(autofirmados).  
Una finalizada la instalacion procedemos a la apertura del sofware.



## Acceso al sofware de gestion de hosting (ISP_CONFIG)
Para acceder, debemos clikear desde el navegador:
+ https://ip_mauina:8080/login
Nos pedira usuario y contraseña. 

Desde ese programa podemos gestionar, CRM(Gestior de clientes)






