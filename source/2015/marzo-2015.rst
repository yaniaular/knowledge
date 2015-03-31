4 de marzo del 2015
-------------------

Llegada a México
~~~~~~~~~~~~~~~~

Me puse al día, revise los branches que tenia pendiente, y solo debo hacer
merge del fix cuando se pasa al estado taller, que dio error cuando se lo
mostre a Friedmann.


Correo de kathy
~~~~~~~~~~~~~~~

Excelente Kathy, de hecho, estaba pensando lo mismo ese día.

Pero creo que hay algo confuso aquí.

*1. yoytec puede recibir un producto para reparar que no haya sido vendido por
   ellos.*

Esto significa que es un producto que se recibe por servicio de reparación que
ofrece Yoytec. Es decir, no hay linea de factura y no hay serial number que
pueda llenar la información de los campos producto, precio unitario, proveedor,
factura de proveedor, factura ni prioridad. Se deben llenar a mano.

*La idea es coloca un nuevo campo boolean en la line de reclamo para saber si el
cliente tiene o no factura. (aquí sería otro escenario que te hablaré en el
segundo siguiente párrafo)
 
Este nuevo campo se puede tomar en cuenta en los métodos utilizados en el
onchange para asi saber como llenar los campos del reclamo. (Perfecto)*

Entonces depués me confundo con lo que dices en lo que está en rojo y el
siguiente párrafo, porque ésto sería otro escenario. Que el producto SI fué
vendido por yoytec, pero la persona no trajo la factura. 

*Si esta como no hay factura, sabemos que la data se llenará a través del campo
serial number y que a partir de este se debe conseguir la info de la factura.
(Perfecto, pero esto es SOLO cuando el cliente compro con yoytec y NO trajo la
factura)*

 *El caso contrario no aplica (con el numero de la factura y linea de factura
 tener el serial number). Esto debido a que como sabemos pueden haber varios
 serial numbers asociados a esa factura, para ello propongo que el onchange de
 la linea de la factura no solo actualice los campos sino que devuelva un
 warning diciendo existen varios serial numbers asociados a esta factura, por
 favor seleccione manualmente el serial number correcto. En el caso que exista
 un solo serial number para esa linea de factura entonces se puede escribir
 directamente el serial number/supplier. (Bien)

 Este campo boolean también sería funcional cuando se le entrega a yoytec un
 producto por RMA pero que no fue vendido por ellos. No entendi*

 
*Creo que otro punto importante para que se pueda ver la cuestión de la dinámica
de los campos es re ordenarlos en la vista. Colocando primero los campos que
"calculan" y luego los campos "calculados" Colocaríamos el campo checkbox de la
factura que habilite para escribir los campos factura y linea de factura, y el
serial number. Luego agruparíamos los campos calculados product, supplier y
unit sale La sección de linked documents puede venir con la info del supplier
invoice + los moves + related claim y etc. y los campos de priority y claim
date pueden ser agregados al comienzo o al final de la vistas. (Me gustaría
verlo mas adelante, por ahora creo que lo podemos dejar así)*

Tomaré en cuenta tus comentarios para las mejoras de esa vista.

Análisis de minuta
~~~~~~~~~~~~~~~~~~

**Minuta reunión con friedmann**

Almacen RMA Client: Crear una locacion para rma cliente, recibir reclamos. DONE.
Almacen RMa proveedor: Crear un locacion de tipo transito, cuando se
envía prouctos al proveedor. DONE **Kathy se esta encargando de esto**

Numero de caso por linea en los reclamos de proveedor. **KATHY LO HARA**
numero de secuencia. **KATHY LO HARA**

Recibir PoM. Productos fabricados
por yoytec.
Tomar en cuenta cuando se recibe una computadora 
con varias piezas. **Estudiar caso**

Preparar una base de datos con la acumulacion
de productos en un rma de proveedor...

Acumular los reclamos por direccion de proveedor.
El acumulador debe ser por servicio autorizado.

5 de marzo del 2015
-------------------

Correo de Moi
~~~~~~~~~~~~~

Discusion en HU, revisar:

https://www.vauxoo.com/?db=vauxoo#id=313&view_type=form&model=user.story

**Correo moi**

Esta H.U. afecta directamente a RMA, como lo tenemos ahora, ya que, se quiere
por medio del número serial, poder verificar cuántas veces ha pasado por el
almacén de RMA de cliente. El documento que actualmente tiene esta trazabilidad
completa, es el stock.production.lot (número serial) con los registros y
sub-registros generados desde stock.picking.

Hay que analizar, como podemos lograr esto. Opción 1) Tal vez, generar un
picking de entrada de los rma-customer o un picking de salida de los
rma-vendor.


**Correo yani**

Si Moi, si se puede, de hecho no se si recuerdas que ayer te comenté lo de
tener un Almacen para RMA Client y otro para RMa SUpplier. Este campo se puede
asignar en el reclamo.
https://docs.google.com/a/vauxoo.com/file/d/0B9dj_hHbN6p3OW9QTXpiWGVOb2s/edit?usp=drivesdk
Creo que con esto se puede solventar, ya que la opción para ver la trazabilidad
ya la tiene Odoo como comentas
https://docs.google.com/a/vauxoo.com/file/d/0B9dj_hHbN6p3a255OVVyZGZaanc/edit?usp=drivesdk

**Reunion con Kathy**

Son locaciones en vez de almacenes y se logrará con Picking Type.

9 de marzo del 2015
-------------------

Se agrega campo message_warranty

message_warranty = fields.Char('Message', default='')

<div class="oe_form_box_info oe_text_center"
    attrs="{'invisible':[('message_warranty','in',('', False))]}">
    <field name="message_warranty" readonly="True"/>
</div>


17 de marzo del 2015
--------------------

**situacion 1**

Cuando el servidor se queda en un loop infinito.
Es porque existe un error en el ultimo modulo que
cargo, por ejemplo

2015-03-17 17:24:09,901 4459 INFO rma2 openerp.modules.loading: loading yoytec_crm_claim_rma_number/view/crm_claim_rma.xml
2015-03-17 17:24:10,459 4459 INFO rma2 openerp.modules.module: module yoytec_crm_claim_rma_custom: creating or updating database tables
2015-03-17 17:24:10,596 4459 INFO None werkzeug: 127.0.0.1 - - [17/Mar/2015 17:24:10] "GET /web HTTP/1.1" 302 -
2015-03-17 17:24:10,607 4459 INFO rma2 openerp.modules.loading: loading 1 modules...
2015-03-17 17:24:10,724 4459 INFO rma2 openerp.modules.loading: 1 modules loaded in 0.12s, 0 queries
2015-03-17 17:24:10,778 4459 INFO rma2 openerp.modules.loading: loading 64 modules..

En este caso, es **yoytec_crm_claim_rma_custom**

Y en el servidor se volvera a cargar una y otra vez.

Se puede tumbar y levantar el servidor para comprobar
si se puede mostrar el error real.

**situacion 2**

Cuando se tiene un campo requerido por el modelo
y ya se ha definido en otro lado puede dar
un error parecido a este

2015-03-17 17:27:39,042 7689 CRITICAL rma2 openerp.service.server: Failed to initialize database `rma2`.
Traceback (most recent call last):
  File "/home/yanina/Roots/instancias/odoo/odoo/openerp/service/server.py", line 909, in preload_registries
    registry = RegistryManager.new(dbname, update_module=update_module)
  File "/home/yanina/Roots/instancias/odoo/odoo/openerp/modules/registry.py", line 366, in new
    openerp.modules.load_modules(registry._db, force_demo, status, update_module)
  File "/home/yanina/Roots/instancias/odoo/odoo/openerp/modules/loading.py", line 351, in load_modules
    force, status, report, loaded_modules, update_module)
  File "/home/yanina/Roots/instancias/odoo/odoo/openerp/modules/loading.py", line 255, in load_marked_modules
    loaded, processed = load_module_graph(cr, graph, progressdict, report=report, skip_modules=loaded_modules, perform_checks=perform_checks)
  File "/home/yanina/Roots/instancias/odoo/odoo/openerp/modules/loading.py", line 157, in load_module_graph
    init_module_models(cr, package.name, models)
  File "/home/yanina/Roots/instancias/odoo/odoo/openerp/modules/module.py", line 285, in init_module_models
    result = obj._auto_init(cr, {'module': module_name})
  File "/home/yanina/Roots/instancias/odoo/odoo/openerp/api.py", line 241, in wrapper
    return old_api(self, args, kwargs)
  File "/home/yanina/Roots/instancias/odoo/odoo/openerp/models.py", line 2558, in _auto_init
    self._set_default_value_on_column(cr, k, context=context)
  File "/home/yanina/Roots/instancias/odoo/odoo/openerp/api.py", line 241, in wrapper
    return old_api(self, args, kwargs)
  File "/home/yanina/Roots/instancias/odoo/odoo/openerp/models.py", line 2385, in _set_default_value_on_column
    default = default(self, cr, SUPERUSER_ID, context)
  File "/home/yanina/Roots/instancias/odoo/odoo/openerp/api.py", line 241, in wrapper
    return old_api(self, args, kwargs)
  File "/home/yanina/Roots/instancias/odoo/odoo/openerp/api.py", line 336, in old_api
    result = method(recs, args, kwargs)
  File "/home/yanina/Roots/instancias/odoo/odoo/openerp/fields.py", line 361, in <lambda>
    lambda recs: self.convert_to_write(value(recs))
  File "/home/yanina/Roots/instancias/odoo/odoo/openerp/fields.py", line 1479, in convert_to_write
    return value.id
AttributeError: 'int' object has no attribute 'id'

**situacion 3**


    claim_line_id = fields.Many2one(
        'claim.line', string='Claim Line',
        ondelete='cascade',
        help='')

el ondelete cascade se usa en el campo many2one que hace referencia
a un campo one2many, para que cuando se borre el registro
donde se encuentra el campo one2many, se borren tmbien las lineas.

**situacion 4**

Cuando se quiera reemplazar un branch local con una de arriba
porque dievergen. Hacer --force:

git push vauxoo-dev 8.0-add_o2m_for_bom_of_product-dev-yani --force

**TODO 1**

Un producto puede tener mas de un bom DONE

cambiar none por draft y draft_customer y draft_supplier
por claim_customer o claim_supplier.

transformar en un metodo todo los filtros que se hacen en fiel_view_gets

19 de marzo del 2015
--------------------

TO DOS
~~~~~~

Cotizacion se hace solo si esta fuera de garantia con
yoytec.

Se puede atender al cliente cuando el producto esta fuera
de garantia. es decir, se crea la cotizacion.

A veces reciben productos que no son de ellos
y se debe recibir para reparacion.

Preparar una base de datos con la acumulacion
de productos en un rma de proveedor...

Acumular los reclamos por direccion de proveedor.
El acumulador debe ser por servicio autorizado.

Mostrar cotizacion en el reclamo.


o2m de cliente en serial number.

Cuando se crean los reclamos a proveedor, y en otro reclamo
a cliente tambien se hace lo mismo, entonces debe llenarse
el reclamo a proveedor que ya este abierto.. hasta que
se llene... usando los campos definidos en la compania

La garantia con el proveedor se calcula tomando la fecha de la
factura de compra al proveedor?

agregar campo supplier_id en el modelo stock.production.lot,
busqueda de método que se encarga de asignar el producto
cuando el serial number es creado desde la transferencia de 
productos al almacen de la compañia...

Editar metodo en la orden de venta cuando la factura es creada
antes que el delivery, colocar un condicional que pregunte
si ya existe un picking asociado con la orden de venta
y asignarlo a la facutra que se creó primero

Cambiar los nombres
none que se llamara draft
y draft_customer, claim_customer, y draft_supplier claim_supplier

20 de marzo 2015
================

reunion con hbto kathy y yani. 1:30 m


Se cambia el nombre del campo move_id ya que se neesita el modulo de anglo saxon
instalado en yoytec por la razon XXX, y en anglo saxon de define un campo
con el mismo nombre move_id.

La razon por la cual no se usa el campo de move_id es porque, este campo
es usado en yoytec para tomar el numero de lotes de alguna factura
que está entrando en reclamo, entonces es posible que se mezcle
informacion 


Usar locaciones virtuales para movimientos de recibo de productos en reclamo
y devoluciones al proveedor


Hacer como en early payment, agregar un metodo en Ask refund para agregar una nueva
opcion para hacer la devolucion del producto. Ya que es ninguna opcion
se incluye lo dos asientos de costo promedio y perdida
https://docs.google.com/a/vauxoo.com/document/d/1JVSVOjH2tQnpN5KA1vUeJVu26TNG1I1G9Y1JKHvwdzo/edit#


Cuando se hace la nota de credito va a ser una nota de credito financiera o logistico.

Documentos:
===========

  a) Se debe de registrar la nota de crédito y escanearla adjunta. b) Se debe
  de registrar contablemente esta nota de crédito: (CONTABLEMENTE SE DEBE DE
  REGISTRAR, PÉRDIDA VS COSTO PROMEDIO)


Lenovo > Laptop > Yoytec (Location Stock)
Yoytec > Laptop  > Yanina (Location Customer)
Yanina > Laptop > Yoytec (Location RMA) ----- no hay contabilidad-- tipo iventory
Yoytec > Laptop > Lenovo (Location Supplier)  ---- no hay contabilidad -- tipo supplier
Lenovo > Nota de Credito > Yoytec . -- Registrar contablemente

I*D: NOTAS DE CREDITO
Logistica: es cuando se generan asiento contables para disminuir el valor del
inventario, cuando se hacen devoluciones al proveedor. 

Financiera: no hay producto involucrado y 2 lineas de asiento.

Factura de Lenovo
Expenses (dr) 700
Payable (cr) 700 (A1)

Lenovo > Yoytec (asiento contable financiero)
PURCHASE REFUND
Allowance (dr)   700   0,00
payable (cr)     0,00   700 (A1)

1) compra a proveedor 1. Fecha 01/01/2015
expense(dr) 100
cxp (cr) 100

2) Compra a proveedor 1 en Fecha 03/03/2015
expense (dr) 120
cxp (cr) 120

precio promedio es 110

Nota de crédito de la primera compra
Cxp (dr) 100
Expense (cr) 110 @ precio promedio (Estos asientos no se crean en odoo)
G & L diff (dr) 10

Banco (dr) 100
CxP (cr) 100

Resumen:

Se debe preguntar como YOYTEC registra los asientos contables cuando el
proveedor les hace una nota de crédito. Dependiendo de lo que nos informen,
estudiaremos el caso y veremos si la propuesta aquí discutida es válida

Se debe preguntar como manejan la devolucion YOYTEC - CLIENTE. Si realizan una
nota de crédito financiera.

Propuesta:

Hacer como en early payment (opcion en selection), agregar un metodo llamado
(Allowance: Devolution of product to supplier) en Ask refund para agregar una
nueva opción para hacer la devolución del producto. Ya que en ninguna opcion se
incluye lo dos asientos de costo promedio y perdida

Tambien, crear un atributo en la compañia que permita guarda la cuenta de
ganancias y perdidas con diferencia en precio (promedio) de devoluciones de
compra.


22 de marzo 2015
================

self = self.with_context(variable=valor)
self._context

Manera de editar la variable privada context
en el enviroment actual.


Se cambia creacion de locaciones y products en los tests de crm_rma_stock_location
desde el yml a data demo. Porque cuando se corren los tests por segunda
vez, el id de lo que se creo primero y se le asigno a stock.warehouse0
queda guardado y no se actualiza con el nuevo registro creado.

Cuando se coloco el yml en demo: ['quantity.yml']
tambien daba error, 
ValueError: stock_inventory_socket not found when processing /home/yanina/Roots/instancias/odoo/rma/crm_rma_stock_location/test/quantity.yml.
    This Yaml file appears to depend on missing data. This often happens for
    tests that belong to a module's test suite and depend on each other.

La solucion fue pasar la creacion de las locaciones, asignacion de una de las locaciones
en el warehouse de YOurCompany y creacion del producto que se usa

31 de marzo 2015
================

Documentos a tomar en cuenta
============================

Dudas HU 814

https://docs.google.com/a/vauxoo.com/document/d/1UiIEPncl4MSEucCWjRO4eeM2XJuayrZLc3hAZ26XP9M/edit#heading=h.b75uyd8a8qvi
https://docs.google.com/a/vauxoo.com/document/d/1JVSVOjH2tQnpN5KA1vUeJVu26TNG1I1G9Y1JKHvwdzo/edit#

https://www.odoo.com/forum/help-1/question/what-documentation-is-available-for-odoo-60312
http://www.diferenciahoraria.info/panama-mexico.htm

http://es.wikipedia.org/wiki/Scrum




Buscar sistemas RMA en el mercado

Tenemos que separar las reparaciones del recibimiento de productos
para reclamo. Las reparaciones de que producto se reparó, eso incluye las
BOM, ya que no vamos a avanzar en el Workflow.

Por ahora no se va a incluir el proceso de recibir reclamos
de NO clientes

Hacer las traducciones.

código en git

```python
return company_picking_type and company_picking_type.id 
````

La configuracion de rma_internacional debe ir en yoytec/data/data.xml



