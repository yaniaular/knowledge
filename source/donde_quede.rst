====================
Historial de trabajo
====================

27 de Agosto de 2013 - 9:31 a.m
-------------------------------

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Problemas solventados en el día
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Para la locación de un stock.move que se guardaba mal, se debía hacer un condicional 
  indicando si se estaba consumiendo o produciendo, ya que, dependiendo de ellos, las
  locaciones que se obtienen de la orden de manufactura se invertian. :)
- Ya se reducen los materiales de inventario

~~~~~~~~~~~~~~~
Resumen del día
~~~~~~~~~~~~~~~

Se crea un SQL mrp_cluster

Data:
- Productos
- BOM
- Routing

Módulos instalados:
- mrp_operations
- mrp
- warehouse
- mrp_byproduct
- mrp_consume_produce (Addons-vauxoo)

Permisos:

- Manage Multiple Units of Measure
- Manage Secondary Unit of Measure
- Manage Multiple Locations and Warehouses
- Manage Routings
- MRP / Button Consume-Produce


~~~~~~~~~~~~~~~~~~
Servers ejecutados
~~~~~~~~~~~~~~~~~~

~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Problemas que se presentaron
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Al agregar una materia prima nueva, se vuelven a agregar las estimadas automaticamente a los
  procurement exceptions

26 de Agosto de 2013 - 5:28 p.m
-------------------------------

~~~~~~~~~~~~~~~
Resumen del día
~~~~~~~~~~~~~~~

Ya se crean los consumibles en el move_lines2, el poblemas es que en los stock.moves
no se están creando bien las localizaciones, es decir, el shipment_move_id que corresponde al
sotck.move en rojo no se esta colocando en state DOne, y el consume_move_id tiene
la localizacion de origen mala, debería ser Stock, y está recibiendo Production.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Problemas que se presentaron
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- El state de un stock.move debe quedar en Done, y queda en Waiting Another Move
- La locacion de un stock.move está mal (Resuelto `27 de Agosto de 2013 - 9:31 a.m`_) 
- Nunca se reduce los materiales nuevos de inventario (Resuelto `27 de Agosto de 2013 - 9:31 a.m`_)

24 de Agosto de 2013 - 8:51 p.m
-------------------------------

~~~~~~~~~~~~~~~
Resumen del día
~~~~~~~~~~~~~~~

Haciendo tarea de cluster 106, haciendo el manual de manufactura para explicar esta tarea 106,
para el manual se explica el modulo mrp_consume_produce de Julio, en el cual
hay ciertos errores al consumir y al producir nuevos items con respecto
al movimiento de inventario o stock.move quedé, arreglando el módulo. Se
debe agregar el stock.move al consumir, revisar código de addons/mrp/mrp.py
linea 962.

./openerp-server -r openerp -w openerp --addons-path=../addons/,../web/addons/,../web_example/,../mrp_consume_produce -u mrp_consume_produce -d mrp_cluster

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Problemas solventados en el día
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Ya logra crear los stock.moves para lo que se produce y lo que se consume







~~~~~
¿Qué?
~~~~~

~~~~~~
¿Cómo?
~~~~~~

~~~~~~~~
¿Cuándo?
~~~~~~~~

~~~~~~~
¿Dónde?
~~~~~~~

