=======
Errores
=======

Mensaje ibus en la consola
--------------------------


Bus::open: Can not get ibus-daemon's address.
IBusInputContext::createInputContext: no connection to ibus-daemon

Solucion
~~~~~~~~

sudo apt-get install ibus
ibus-daemon -d
