
-----------------------
Instalacion de colo-ify
-----------------------

$cd ~/.bazaar/plugins
$bzr branch lp:bzr-colo colo

----------
How to Use
----------

- Verificar que no esté colificado
  $bzr branches
  debe decir ``* default``

- Colificar la carpeta
  $bzr colo-ify

- Generar branch nuevo
  $bzr colo-branch 7.0-modulo-dev-desarrollador

- Hacer switch
  $bzr switch trunk
  $bzr switch 7.0-modulo-dev-desarrollador

- Hacer merge
  $bzr switch trunk
  $bzr merge colo:7.0-modulo-dev-desarrollador
  $bzr st
  $bzr ci -m '''[MERGE]'''
.. note:: 
    Siempre hace bzr add, porque sino el archivo quedará flotando. Siempre hacer commit.



