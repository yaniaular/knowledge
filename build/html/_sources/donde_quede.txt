====================
Historial de trabajo
====================

-------------------------------
24 de Agosto de 2013 - 8:51 p.m
-------------------------------

Haciendo tarea de cluster 106, haciendo el manual de manufactura para explicar esta tarea 106,
para el manual se explica el modulo mrp_consume_produce de Julio, en el cual
hay ciertos errores al consumir y al producir nuevos items con respecto
al movimiento de inventario o stock.move quedé, arreglando el módulo. Se
debe agregar el stock.move al consumir, revisar código de addons/mrp/mrp.py
linea 962.

./openerp-server -r openerp -w openerp --addons-path=../addons/,../web/addons/,../web_example/,../mrp_consume_produce -u mrp_consume_produce -d mrp_cluster


-------------------------------
26 de Agosto de 2013 - 5:28 p.m
-------------------------------

Ya se crean los consumibles en el move_lines2, el poblemas es que en los stock.moves
no se están creando bien las localizaciones, es decir, el shipment_move_id que corresponde al
sotck.move en rojo no se esta colocando en state DOne, y el consume_move_id tiene
la localizacion de origen mala, debería ser Stock, y está recibiendo Production.

~~~~~
¿Qué?
~~~~~

Arreglar módulo de Julio 

~~~~~~
¿Cómo?
~~~~~~

~~~~~~~~
¿Cuándo?
~~~~~~~~

~~~~~~~
¿Dónde?
~~~~~~~


===========
Aprendizaje
===========

--------------
Configurar VIM
--------------

$mkdir ~/.vim
$bzr branch lp:~vauxoo/+junk/vim-tuning ~/
$mv ~/vim-tuning/* ~/.vim/
$mv ~/vim-tuning/.vimrc ~/.vim/
$ln -s ~/.vim/.vimrc ~/.vimrc

---
VIM
---

-------------------------------
23 de Agosto de 2013 - 8:51 p.m
-------------------------------

server/openerp/osv/orm.py
Linea 4006, 

Se explica como agregar valores
a campos one2many many2many, etc
con el metodo write


