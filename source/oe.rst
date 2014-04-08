Prueba Unitarias en OpenERP
===========================

Test framework
--------------

Esto es la documentación de cómo se deben hacer las pruebas en openerp,

creo que desde el punto de vista de TDD.

Esto en lugar de usar YAML.

De este website, https://doc.openerp.com/trunk/server/05_test_framework/
leer estas dos secciones "Writing tests" y "Running the tests"


Bajar el branch openerp-command para la versión 7 que lo podemos conseguir
en la siguiente direccion: https://code.launchpad.net/~openerp/openerp-command/7.0
asegurarse que corresponde a la version 7 pues el de la version trunk se ha borrado,
solo ha quedado para históricos.

$ bzr branch lp:~openerp/openerp-command/7.0 openerp-command

$ cd openerp-command
$ sudo python setup.py install

entrar en la direccion del branch del server y aplicar el mismo comando de arriba

$ cd openerp-server/7.0
$ sudo python setup.py install

Configurar a Postgres para que funcione con un usuario de peer de tal forma
que sea igual al usuario del sistema

en pg_hba.conf 

host          all          all          peer

luego de esto levantar una instancia con los módulos que se van a probar

posteriormente desarrolle sus pruebas siguiente los lineamientos de https://doc.openerp.com/trunk/server/05_test_framework/

y luego instale su módulo de la manera usual 

ahora si su módulo entonces incluye una carpeta de "tests" nótese la (s) al final de tests

entonces al ejecutar el siguiente comando su módulo instalado será sometido a las 
pruebas que usted haya descrito en ellas.

oe run-tests -m MODULE -d DB_NAME -p PORT_NUMBER --addons=ADDONS_PATH,WEB_PATH/addons

como comentaba anteriormente, creo que de esta forma es mucho mas
sencilla y simple de poder generar codigo siguiendo el paradigma (A)TDD,
pues nuestras pruebas sería sencillamente código python puro y no mezclas
de xml y etc. Espero que podamos mantenerlo así, con esto no quiero decir
que en algún momento no tengamos que incluir los famosos YAML.

lo puede probar por ejemplo con account de esta forma:

oe run-tests -m account -d bdp_02 -p 8069 --addons=/home/hbtosuse/instancias/7.0/addons/,/home/hbtosuse/instancias/7.0/web/addons


doctest
-------

http://sphinx-doc.org/ext/doctest.html
