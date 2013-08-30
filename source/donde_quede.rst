====================
Historial de trabajo
====================

Fecha - Hora
------------

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Problemas solventados en el día
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

~~~~~~~~~~~~~~~
Resumen del día
~~~~~~~~~~~~~~~

Asunto del dia1
^^^^^^^^^^^^^^^

Asunto del dia2
^^^^^^^^^^^^^^^

~~~~~~~~~~~~~~~~~~
Servers ejecutados
~~~~~~~~~~~~~~~~~~

~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Problemas que se presentaron
~~~~~~~~~~~~~~~~~~~~~~~~~~~~



29 de Agosto de 2013 - 9:05 a.m
-------------------------------

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Problemas solventados en el día
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Cambiar la forma de obtener el producto principal para la referencia, ya que no siempre
  estará en la posición 0, o la posición -1.

De la materia prima que se carga por defecto desde el BOM, ya se tiene creado los asientos
con la referencia al stock.move principal, que se refiere al producto final principal
definido en la orden de manufactura. A partir del stock.picking de la orden de manufactura y
consultados la lista de stock.move se pudo llevar al stock.move principal que se utilizará,
sin embargo, se debe tener en cuenta que si no existen componentes en el BOM que se define en
la orden de producción, no seŕa posible realizar ésta operación.

~~~~~~~~~~~~~~~
Resumen del día
~~~~~~~~~~~~~~~

Se continúa realizando el manual, ya que el módulo mrp_consume_produce aparentemente está listo.

Al definir un BOM sin componentes en una orden de manufactura no se crean el stock.move del
producto final, se debe reportar un bug a openERP no sin antes probar mi teoría en el runbot.

Trabajo repertido con respecto al módulo mrp_request_return
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Hable con Julio sobre la propuesta de merge del módulo mrp_consume_produce, y resulta que ya
existe un modulo llamada mrp_request_return el cual agrega un boton a la orden de manufactura
el cual se encarga de añadir productos nueva al page product to consume, y luego allí se
puede ir nuevamente al wizard de Consumed para finalmente consumirlos en la orden de produccion.

Sin embargo no encontre un modulo que hiciera la misma tarea para los productos producidos.
Quede con Julio en que él iba a revisar lo que se tenía, y de mandarme una lista de modulos de mrp
que el conozca para saber que tenemos en Vauxoo. Y además de eso debemos complementar
lo que se hizo en mrp_consume_produce con lo que ha hecho Julio de MRP.

Se debe revisar los modulos correspondientes a mrp_variation, mrp_account_variation.

~~~~~~~~~~~~~~~~~~
Servers ejecutados
~~~~~~~~~~~~~~~~~~

~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Problemas que se presentaron
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Cambiar la forma de obtener el producto principal para la referencia, ya que no siempre
  estará en la posición 0, o la posición -1. (Resuelto `29 de Agosto de 2013 - 9:05 a.m`_ )
- Se debe reportar bug con respecto a que no se crean el stock.move del producto principal
  cuando en su BOM no tiene componentes.
- Existe un módulo de Julio que ya agregaba nuevos items y además los cancelaba, mrp_request_return
  y mrp_request_return_cancel.

28 de Agosto de 2013 - 2:48 p.m
-------------------------------

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Problemas solventados en el día
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


- La orden de abastecimiento pasa a Done pero sin el mensaje, se debe buscar el workflow
  correcto para pasar la orden a Done (El state de un stock.move debe quedar en Done, 
  y queda en Waiting Another Move)

Se agregaron las siguientes líneas de código:

    wf_service = netsvc.LocalService("workflow")

    wf_service.trg_validate(uid, 'procurement.order', procurement_id, 'button_check', cr)
    
    procurement_order.action_done(cr, uid, [procurement_id])  

ésto permitió que la excepción de abastecimiento se procesara de manera correcta.

- EL stock.move de la materia prima nueva no pasa a Done

Con la siguiente línea se resuelve el problema:

    self.pool.get('stock.move').action_done(cr, uid, [shipment_move_id], context=context) 

- Al agregar una materia prima nueva, se vuelven a agregar las estimadas automáticamente a los
  procurement exceptions.

Al resolver lo anterior, ésto ya no se manifestó.

- Al producir un elemento adicional, las locaciones del stock.move de los que se produjo es de
  stock a stock y deberia ser de production a stock.

Esto se resuelve colocando los campos de las localizaciones en la vista y con invisible con True
para que no moleste al usuario.

- Al producir todo lo que se tenía y luego se consume algo más, estaba dando un error ya que
  la referencia para consumir un producto se utilizaba el campo move_created_ids que ya
  se encontraban vacío.

if mrp_obj.move_created_ids:
    reference = mrp_obj.move_created_ids[0].id
else:
    reference = mrp_obj.move_created_ids2[-1].id

~~~~~~~~~~~~~~~
Resumen del día
~~~~~~~~~~~~~~~

Con ayuda del pdb de python, y del comando w, se pudo revisar el flujo de procesos por los
cuales se paseaban el openerp al forzar la reservación de materiales y así se pudo
deducir cual era el método que se debía llamar para procesa las excepciones de abastecimiento
de manera correcta. Resumen del pdb:

    /home/yanina/branches/instancias/7.0/addons/mrp/mrp.py(1021)force_production()
    
    -> pick_obj.force_assign(cr, uid, [prod.picking_id.id for prod in self.browse(cr, uid, ids)])

    /home/yanina/branches/instancias/7.0/addons/stock/stock.py(778)force_assign()
    
    -> self.pool.get('stock.move').force_assign(cr, uid, move_ids)

    /home/yanina/branches/instancias/7.0/addons/stock/stock.py(2126)force_assign()
    
    -> wf_service.trg_write(uid, 'stock.picking', move.picking_id.id, cr)

    /home/yanina/branches/instancias/7.0/addons/procurement/procurement.py(482)test_finished()
    
    -> procurement.id, 'button_check', cursor)

se llama al método production_obj.force_production(cr, uid, [mrp_obj.id])

Se pasa la excepción de abastecimiento por un proceso y luego se pasa a Done:

wf_service = netsvc.LocalService("workflow")                                    
wf_service.trg_validate(uid, 'procurement.order', procurement_id, 'button_check', cr)
procurement_order.action_done(cr, uid, [procurement_id])  

Se hizo una propuesta de **merge** a los addons-vauxoo-7.0:

https://code.launchpad.net/~vauxoo/addons-vauxoo/7.0-rev-mrp_consume_produce-yani
https://code.launchpad.net/~vauxoo/addons-vauxoo/7.0-rev-mrp_consume_produce-yani/+merge/182765

~~~~~~~~~~~~~~~~~~
Servers ejecutados
~~~~~~~~~~~~~~~~~~

./openerp-server -r openerp -w openerp --addons-path=../addons/,../web/addons/,../web_example/
,../mrp_consume_produce -u mrp_consume_produce,procurement,mrp -d mrp_cluster

~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Problemas que se presentaron
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Al producir un elemento adicional, las locaciones del stock.move de los que se produjo es de
  stock a stock y deberia ser de production a stock. (Resuelto `28 de Agosto de 2013 - 2:48 p.m`_ )
- Al producir todo lo que se tenía y luego se consume algo más, estaba dando un error ya que
  la referencia para consumir un producto se utilizaba el campo move_created_ids que ya
  se encontraban vacío. (Resuelto `28 de Agosto de 2013 - 2:48 p.m`_ )

27 de Agosto de 2013 - 4:33 p.m
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
^^^^^^^^^^^^^^^^^^^^^^^^^^

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


Proceso de Force Reservation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Modelo: mrp.production
Método: force_production
>
Modelo: stock.picking
Método: force_assing
>
Modelo: stock.move
Metodo: force_assing

No encontre nada que tuviera que ver con pasar el orden de abastecimientos a done
sin embargo se llamo a un metodo  procurement_order.action_done(cr, uid, [procurement_id])
que permitio colocar la orden de abastecimiento en Done, pero el campo de message se queda 
vacío cuando debería decir Products reserved from stock. el único método que edita
ese mensaje es action_move_assigned() en procurement/procurement.py, pero no consigo
donde se llama ese método.

Necesito saber el workflow que se genera al forzar la resevación para poder llevar a Done
la orden de abastecimiento del producto adicional y ademñas de eso necesito pasar el stock.move
a Done.

~~~~~~~~~~~~~~~~~~
Servers ejecutados
~~~~~~~~~~~~~~~~~~

./openerp-server -r openerp -w openerp 
--addons-path=../addons/,../web/addons/,../web_example/,../mrp_consume_produce -u
mrp_consume_produce,procurement,mrp -d mrp_cluster

~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Problemas que se presentaron
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Al agregar una materia prima nueva, se vuelven a agregar las estimadas automáticamente a los
  procurement exceptions (Resuelto `28 de Agosto de 2013 - 2:48 p.m`_) 
- La orden de abastecimiento pasa a Done pero sin el mensaje, se debe buscar el workflow
  correcto para pasar la orden a Done (El state de un stock.move debe quedar en Done, 
  y queda en Waiting Another Move) (Resuelto `28 de Agosto de 2013 - 2:48 p.m`_)
- EL stock.move de la materia prima nueva no pasa a Done (Resuelto `28 de Agosto de 2013 - 2:48 p.m`_ )

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
- La locacion de un stock.move está mal (Resuelto `27 de Agosto de 2013 - 4:33 p.m`_) 
- Nunca se reduce los materiales nuevos de inventario (Resuelto `27 de Agosto de 2013 - 4:33 p.m`_)

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

