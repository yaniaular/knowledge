Instalando wordpress
====================

Instalacion de XAMPP
--------------------

#. sudo chmod 755 xampp-linux-1.8.3-4-installer.run
#. sudo ./xampp-linux-1.8.3-4-installer.run

Correr servidor Xampp
---------------------

#. Detener nginx a cualquier servidor activo que se tenga
    sudo /etc/init.d/nginx stop

#. Iniciar Lampp
    sudo /opt/lampp/lampp start

Descomprimir wordpress (directorio raiz de wordpress)
-----------------------------------------------------

#. unzip wordpress-3.9.1-es_ES.zip
    Esto me descomprime una carpeta llamada wordpress

Linkear directorio web raiz a otra carpeta
------------------------------------------

#. mkdir ~/public_html
#. cd /opt/lampp/htdocs/
#. sudo ln -s ~/public_html 
   O la dirección de la carpeta wordpress

#. En el navegador colocar, http://localhost/public_html/

Guardar wordpress en carpeta raiz
---------------------------------

#. cd /opt/lampp/htdocs/
#. sudo ln -s ~/<Path-wordpress> 

Crear mi base de datos
----------------------

http://localhost/phpmyadmin

#. Crear Base de datos con cotejamiento utf8_bin
#. Hacer click en la base de datos -> privilegios -> Agregar usuario
   #. Asignar nombre de usuario
   #. Servidor Localhost
   #. Otorgar todos los privilegios para la base de datos "base de datos".

Configurar seguridad en wordpress
---------------------------------

sudo /opt/lampp/lampp security

Referencias
-----------

http://ubuntuforums.org/showthread.php?t=223410

http://www.desarrolloweb.com/articulos/2408.php



Documentacion sobre wordpress
=============================

mkdir wp-content/uploads/2014



Referencias
-----------

https://codex.wordpress.org/es:WordPress_Lessons

Problemas con wordpress
-----------------------

Al entrar al wp-admin la página se queda en blanco.

En primer lugar, se tuvo que renombrar la carpeta wp-content/plugins

por plugis-old, y crear una carpeta plugins solo con los archivos hello.php 

e index.php de manera que se pudiera reconocer el error al entrar en wp-admin.

Al hacer esto, se descubrio que el error era  
Fatal error: Allowed memory size of 33554432 bytes exhausted

http://www.hostdime.com.co/blog/10-errores-mas-comunes-de-wordpress-con-soluciones/

Y se escogio la Solucion de 

Solución 4: Crear un archivo php.ini en la carpeta wp-admin

Abra el Bloc de notas.
Inserte el siguiente código en el Bloc de notas.
memory_limit = 64M;
Guardar como “php.ini“.
Cargar este archivo en el directorio “wp-admin“.
