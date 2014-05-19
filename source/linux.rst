============
Repositorios
============

``vim /etc/apt/source.list``

apt-get update

Si nos advierte de que alguna llave pública no está disponible, copiamos el código y procedemos a su descarga.

``gpg --keyserver subkeys.pgp.net --recv-keys XXXXXXXXXXXX``

Una vez obtenida procedemos a exportarla y añadirla.

``gpg --export --armor XXXXXXXXXXXX | apt-key add -``

Y para finalizar, ya solo nos queda actualizar la nueva base de datos.

``aptitude update && aptitude safe-upgrade``



