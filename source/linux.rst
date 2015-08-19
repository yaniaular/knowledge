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

Establecer IP
-------------

Paso 1. Configurar la IP 
ifconfig eth0 172.16.113.41 netmask 255.255.0.0 

Paso 2. Configurar GateWay 
route add default gw 172.16.113.1 

Paso 3. Configurar DNS 
echo nameserver 172.16.112.200 > /etc/resolv.conf 


zsh
===

nueva shell para yakuake..

https://github.com/robbyrussell/oh-my-zsh/wiki/Installing-ZSH
