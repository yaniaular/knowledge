======
Python
======

if ac[2] and ac[2].get('accepted', False):
Es igual que decir
if ac[2].get('accepted', False):
subject = 'Accepted '+ ' Criteria '+ ' - '+ criteria[1]
Esto es más legible si lo ponemos por un lado traducible y por otro lado si usamos el método .format() de los strings ese creo que no lo conoces pero es mucho más manejable
'hola {name} tu apellido es {surname}, no es cierto {name}'.format(name='Juan',surname='Perez')
tambien se puede usar con posiciones
'hola {0} tu apellido es {1}, no es cierto {0}'.format('Juan','Perez')
http://ebeab.com/2012/10/10/python-string-format/

-------------------------------
Activar autocompletar en python
-------------------------------

Se debe ejecutar el comando en consola::

    sudo apt-get install python-argparse
    sudo python-argcomplete-check-easy-install-script

This script is part of the Python argcomplete package (https://github.com/kislyuk/argcomplete).
It is used to check if an EASY-INSTALL-SCRIPT wrapper redirects to a script that contains the
string
"PYTHON_ARGCOMPLETE_OK". If you have enabled global completion in argcomplete, the completion hook
will run it every
time you press <TAB> in your shell.


Y luego en cada script de python en el cual se use
el argparse se debe colocar el encabezado...

.. code-block :: python

    #!/usr/bin/python
    # PYTHON_ARGCOMPLETE_OK

    import os
    import argparse
    import argcomplete
    import json

    def argument_parser():
        parser = argparse.ArgumentParser(

------
py2exe
------

Convierte script a un ejecutable.
