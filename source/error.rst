=======
Errores
=======

Bus::open: Can not get ibus-daemon's address.
---------------------------------------------
IBusInputContext::createInputContext: no connection to ibus-daemon

**Solucion**

sudo apt-get install ibus
ibus-daemon -d

could not reliably determine the server's fully qualified domain name using 127.0.1.1 for servername
----------------------------------------------------------------------------------------------------

**Solucion**

sudo vim etc/hostname
#arreglar nombre de la maquina
#si no te da permisos de escritura
mount -o remount,rw /
