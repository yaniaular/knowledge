OpenERP
=======

ORM
---

server/openerp/osv/orm.py
Linea 4006, 

Se explica como agregar valores
a campos one2many many2many, etc
con el metodo write

Openerp Web
-----------
 
Para un módulo web básico solo necesitas hacer que dependa del modulo ``web`` en vez de ``base`` y
crear la carpeta static.

lp:~openerp-community/+junk/web_test

crear carpeta ``src`` dentro de ``static``. Y dentro de ``src`` creamos ``js``, ``xml`` y ``css``.

https://doc.openerp.com/trunk/web/module/

En el archivo ``__openerp__.py`` vamos a agregar un archivo javascript que se creo en la carpeta
``js`` con un console.log('hola mundo');


``qweb`` son templates html. Son los archivos xml de la carpeta ``xml``


el <field name="tag">example.action</field> es para saber que acción en javascript deberá llamar.


en javascritp siempre debes inicializar las variables en el init, el init es para inicializar los
atributos de los objetos.

init (super_apply: ) -> start 

id xml
------

 grep -rn --include=*.xml search . | grep field | grep model

.. code-block :: xml

    <field name="fields_id" search="[('model','=','product.category'),('name','=','property_account_income_categ')]"/>

WorkFlows
---------

Crear el workflow
~~~~~~~~~~~~~~~~~

**name**: Nombre del Workflow
**osv**: Modelo al cual pertence el workflow
**on_create**: 

.. code-block :: xml

    <record id="purchase_requisiton_wkf" model="workflow">
        <field name="name">Purchase Requisition Workflow</field>
        <field name="osv">purchase.requisition</field>
        <field name="on_create">True</field>
    </record>

Crear los nodos del workflow
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**name**: Nombre del estado
**kind** : Como se va a comportar el estado cuando lleguen a él
 - *dummy*: No hace nada
 - *function*: Ejecuta un método python (se define en campo *action*)
 - *subflow*: Dispara el flujo de otro objecto
 - *stopall*: Se acabo el workflow en ese estado.
**split mode**: Cuales y en que orden se quiere que se ejecuten las flechas que salen del estado.
 - *and*: Si todas son verdad, se ejecutan todas
 - *xor*: Se ejecutan todas las que sean verdad, al mismo tiempo.
 - *or*: Se ejecutan todas las que sean verdad, en forma secuencial.
**join mode**: Cuales y en que orden se quiere que se ejecuten las flechas que entran en el estado.
**action**: Es la acción que se ejecuta en caso de que el **kind** sea tipo *function* y de que
la condición (**condition**) de la transición sea *True* para llegar a este estado.

.. note:
   El valor de verdad de las flechas (transiciones) se obtiene del campo **condition** de las 
   transiciones.

.. code-block :: xml

    <record id="act_draft" model="workflow.activity">
        <field name="wkf_id" ref="purchase_requisiton_wkf"/>
        <field name="name">draft</field>
        <field name="flow_start">True</field>
        <field name="kind">dummy</field>
    </record>

Crear las transiciones
~~~~~~~~~~~~~~~~~~~~~~

**condition**: Expresion python, si la condición es *True*, se pasa al siguiente nodo o estado.
**signal**: Es el disparador de la transicion. Nombre del boton tipo workflow.
**group_id**: Quien tiene permiso de ejecutar la transicion.
**trigger_model**: En que modelo se encuentra el metodo que dispara esta transicion. Para que no
se necesite de ningun botón, sino de la ejecucion de un metodo.
**trigger_expr_id**: 
**act_from**: De donde vengo.
**act_to**: Hacia donde voy.

.. code-block :: xml

    <record id="pr_t1" model="workflow.transition">
        <field name="act_from" ref="act_draft"/>
        <field name="act_to" ref="act_budget"/>
        <field name="signal">signal_budget</field>
        <field name="condition">action_budget()</field>
    </record>


fields_view_get
---------------

.. code-block :: python

    def fields_view_get(self, cr, uid, view_id=None, view_type='form', context=None, toolbar=False, submenu=False):
        if context is None:
            context = {}
       
        workflow= False
        if context.get('new_workflow',False):
            workflow = context['new_workflow']
            del context['new_workflow']

        res = super(purchase_requisition,self).fields_view_get(cr, uid, view_id=view_id, view_type=view_type, context=context, toolbar=toolbar, submenu=submenu)
        if workflow:
            doc = etree.XML(res['arch'])
            for node in doc.xpath("//header/field[@name='state']"):
                import pdb
                pdb.set_trace()
                node.set('statusbar_visible', "draft_bidding")
            res['arch'] = etree.tostring(doc)
        else:
            doc = etree.XML(res['arch'])
            for node in doc.xpath("//header/field[@name='state']"):
                import pdb
                pdb.set_trace()
                node.set('statusbar_visible', "draft")
            res['arch'] = etree.tostring(doc)

        return res

