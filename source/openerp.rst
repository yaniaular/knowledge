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

You can use the delete tag in xml. Dont remove the xml data records you have created in the xml
file. In the new version just add the delete tag at the end of the file.

.. code-block :: xml

    <delete id="module_name.xml_record_id" model="hr.employee"/>

If we do not want to display a menu, we can delete its information in the database with the
following two xml commands, 1 to remove its id is stored in table "ir_model_data", 1 to clear its
name stored in table "ir_ui_menu".

.. code-block :: xml

<delete model="ir.model.data" search="[('name','=','menu_hr_attendance_sigh_in_out')]" />
<delete model="ir.ui.menu" search="[('name','=','Sign in / Sign out')]" />

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

Hacer un readonly con el campo de una vista padre
-------------------------------------------------

.. code-block :: xml

    <xpath expr="//field[@name='order_line']/tree//field[@name='product_id']" position="attributes">
        <attribute name="readonly">[('order_state' ,'=', 'modify')]</attribute>
    </xpath>

Debes tener un campo que se relacione al modelo con el cual quieres comparar el valor.
http://stackoverflow.com/questions/19682040/in-openerp-how-to-show-or-hide-a-field-based-on-domain-from-its-parent-many2on

Campo funcional en openerp
--------------------------


.. code-block :: python

    def _set_order_state(self, cr, uid, ids, field_name, arg, context=None):
        """
        Este método es llamado luego de llamar a _get_order_line o luego de que
        algún campo dentro del modelo purchase order line cambie.
        """
        res = {}
        po_obj = self.pool.get('purchase.order')
        for pol_brw in self.browse(cr, uid, ids, context=context):
            res[pol_brw.id] = pol_brw.order_id.state
        return res

    def _get_order_line(self, cr, uid, ids, context=None):
        """
        Cada vez que el campo state del modelo purchase.order cambie
        entonces llamara al metodo _get_order_line que se encargara de
        filtrar las purchase.order.line como yo desee, este caso, retornara
        la lista de ids de las purchase order line que contiene la
        purchase order que cambio. Este método siempre retorna ids
        del modelo original donde se encuentra el campo funcional.
        """
        res = []
        po_obj = self.pool.get('purchase.order')
        for po_brw in po_obj.browse(cr, uid, ids, context=context):
            res += [ol.id for ol in po_brw.order_line]
        return res

    _columns = {
        'order_state': fields.function(_set_order_state, type='char', string='State',
                                        store={
                                        'purchase.order': (_get_order_line, ['state'], 10),
                                        }, 
                                        method = True,
                                        ),
        }

llamada a funciones desde xml
-----------------------------

https://bazaar.launchpad.net/~vauxoo-private/vauxoo-private/8.0-devilsoul-change-pos/view/head%3A/bingo_raffle/demo/stock_move_demo.xml#L20046


.. code-block :: xml

    <record id="stock_move_raffle_demo2004" model="stock.move">
        <field name="product_id" ref="bingo_master.product_template_ticket"/>
        <field name="product_qty">1</field>
        <field name="name">651</field>
        <field name="type">internal</field>
        <field name="product_uom" ref="product.product_uom_unit"/>
        <field name="location_id" ref="stock.stock_location_stock"/>
        <field name="location_dest_id" ref="stock_location_raffle"/>
        <field name="prodlot_id" ref="stock_production_lot_example_651"/>
    </record>

    <function
        eval='[[ ref("stock_move_raffle_demo2004"),]]'
        id="validate_raffle_move3"                                          
        model="stock.move"                                                                       
        name="action_done"/> 



https://bazaar.launchpad.net/~vauxoo-private/vauxoo-private/8.0-devilsoul-change-pos/view/head%3A/bingo_raffle/demo/stock_move_demo.xml#L20046

Modificar crear y editar de campos many2one
-------------------------------------------

.. code-block :: xml

    options="{'create': false, 'create_edit': false}"

Prueba Unitarias en OpenERP
---------------------------

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

Configurar oe conrrectamente
----------------------------

Configuramos nuestro archivo 

yanina@yani-kde:~$ sudo vim /etc/postgresql/9.3/main/pg_hba.conf 
yanina@yani-kde:~$ sudo /etc/init.d/postgresql restart

.. code-block : shell

    local   all             postgres                                peer

    # TYPE  DATABASE        USER            ADDRESS                 METHOD

    # "local" is for Unix domain socket connections only
    **local   all             openerp                                 md5**
    local   all             all                                     peer
    # IPv4 local connections:

Colocamos el usuario a usar con conexion md5

Luego creamos nuestro usuario en el sistema (linux) openerp:

yanina@yani-kde:~$ adduser openerp
yanina@yani-kde:~$ sudo su openerp
openerp@yani-kde:~$

Le damos permisos al usuario openerp para que pueda ejecutar todos los script del user yanina

yanina@yani-kde:~$chmod 755 carpeta_con_branches/ -R


Luego, al instalar oe, descargandolo de los repositorios:

lp:~openerp/openerp-command/7.0/

editamos el archivo vim openerpcommand/run_tests.py, y colocamos 

config['admin_passwd'] = 'openerp'
config['db_password'] = 'openerp'

en las lineas 105 y 106.

Instalamos el script sudo python setup.py install

Luego ejecutamos: 

openerp@yani-kde:~$ oe run-tests -m cicsa_purchase_requisition_workflow -d cicsa_server -p 8069
--addons=/home/yanina/branches/instancias/7.0/addons,
/home/yanina/branches/instancias/7.0/web/addons/,
/home/yanina/branches/instancias/7.0/addons-vauxoo-cicsa,
/home/yanina/branches/instancias/7.0/vx_account-financial-report,
/home/yanina/branches/instancias/7.0/vx_ovl70_trunk,
/home/yanina/branches/instancias/7.0/0companies/cicsa/del_amouth_bdp_purchase_requisition-dev-yani

por ejemplo.

Debemos tener en cuenta que el usuario de postgres usado en config['admin_passwd'] debe ser el
owner de la base de datos especificada en los parametros del script.

doctest
-------

http://sphinx-doc.org/ext/doctest.html

Actualizar datos o data XML en openerp al acutalizar módulo
-----------------------------------------------------------

Hi,

simply add the id parameter to the record. Example from one of my modules (with anonymized data)

.. code-block :: xml

    <data noupdate="0">
    <record id="testpartner" model="res.partner">
    <field name="name">Test</field>
    </record>
    </data>

This searches the partner with the id "testpartner". if there is no record, it is created. if there is, the provided fields are updated.

If you then - for example - change the file to this:

.. code-block :: xml

    <data noupdate="0">
    <record id="testpartner" model="res.partner">
    <field name="name">Test 2</field>
    </record>
    </data>

and then select the module, click "Update Module" and then run the update action (or start the server with the update-parameter to do it outside of the GUI), the record will be updated to "Test 2" in the name field.

This works perfectly for me.

best regards

tuo

Mensaje de confirmacion en un boton de Openerp
----------------------------------------------

    
.. code-block :: xml

    <button name="signal_disapprove_cc" states="approve_cc" string="Disapprove by CC" type="workflow"
        confirm="Do you confirm DISAPPROVE this document?"/>

Arreglar o desinstalar libreria de reportlab
--------------------------------------------

dpkg --get-selections | grep reportlab
pip freeze | grep reportlab
sudo apt-get purge python-reportlab
sudo apt-get purge python-reportlab-accel

Descargar reportlab 2.7
sudo python setup.py install

pip freeze | grep reportlab
reportlab==2.7

Esto ocasiona que se desinstale rst2pdf


Reportes Webkit
---------------

El modulo donde está el reporte debe depender de report_webkit
se debe instalar el paquete 
sudo apt-get install wkhtmltopdf

Crear un botón que se use para mandar a imprimir el reporte.

view/module_name_view.xml

.. code-block :: xml

    <?xml version='1.0' encoding='UTF-8'?>
    <openerp>
        <data>
            <record model="ir.ui.view" id="module_name_report_form_inherit">
                <field name="name">module.name.report.form</field>
                <field name="model">module.name</field>
                <field name="inherit_id" ref="module_name.view_module_name_form"/>
                <field name="arch" type="xml">
                    <xpath expr="//header" position="inside">
                        <button name="print_report" string="Print Webkit" type="object" icon="gtk-print"/>
                    </xpath>
                </field>
            </record>
        </data>
    </openerp>

Luego debes crear un xml, para crear el reporte, indicando sus caracaterísticas y su ubicacion

report/report.xml

.. code-block :: xml

        <?xml version='1.0' encoding='UTF-8'?>
        <openerp>
            <data>

               <record id="module_name_report_webkit" model="ir.actions.report.xml">
                    <field name="report_type">webkit</field>
                    <field name="report_name">module_name_report_webkit</field>
                    <field eval="[(6,0,[])]" name="groups_id"/>
                    <field eval="0" name="multi"/>
                    <field eval="0" name="auto"/>
                    <field eval="0" name="header"/>
                    <field name="model">module.name</field>
                    <field name="type">ir.actions.report.xml</field>
                    <field name="name">Purchase requisition Webkit</field>
                    <field name="report_rml">cicsa_module_name_report/report/module_name_report.mako</field>
                    <field name="report_file">cicsa_module_name_report/report/module_name_report.mako</field>
                </record> 
                <record id="property_module_name_report_webkit" model="ir.property">
                    <field name="name">module_name_report_webkit_property</field>
                    <field name="fields_id" ref="report_webkit.field_ir_act_report_xml_webkit_header"/>
                    <field eval="'ir.header_webkit,'+str(ref('cicsa_module_name_report.requisition_landscape_header'))" model="ir.header_webkit" name="value"/>
                    <field eval="'ir.actions.report.xml,'+str(ref('cicsa_module_name_report.module_name_report_webkit'))" model="ir.actions.report.xml" name="res_id"/>
                </record>
            </data>
        </openerp>

    Adicional a eso, debes tener un header creado si lo deseas. Esto es en caso de que se tenga
    un registro ir.property como se muestra arriba, si no se desea el ir.property, se puede eliminar
    y solo dejar la accion del reporte, si se coloca el property, se debe agregar el codigo a
    continuacion

    data/requisition_webkit_header.xml

    .. code-block :: xml


    <?xml version="1.0" ?>
    <openerp>
        <data noupdate="1">
            <record id="requisition_landscape_header" model="ir.header_webkit">
                <field name="footer_html"><![CDATA[
    <html>
        <head>
            <meta content="text/html; charset=UTF-8" http-equiv="content-type"/>
            <script>
                function subst() {
                var vars={};
                var x=document.location.search.substring(1).split('&');
                for(var i in x) {var z=x[i].split('=',2);vars[z[0]] = unescape(z[1]);}
                var x=['frompage','topage','page','webpage','section','subsection','subsubsection'];
                for(var i in x) {
                var y = document.getElementsByClassName(x[i]);
                for(var j=0; j<y.length; ++j) y[j].textContent = vars[x[i]];
                    }
                }
            </script>
        </head>
        <% import datetime %>
        <body style="border:0; margin: 0;" onload="subst()">
            <table style="border-top: 1px solid black; width: 1080px">
                <tr style="border-collapse:collapse;">
                    <td style="text-align:left;font-size:10;width:350px;">${formatLang( str(datetime.datetime.today()), date_time=True)}</td>
                    <td style="text-align:center;font-size:10;width:350px;">${user.name}</td>
                    <td style="text-align:right;font-size:10;width:350px;">Page&nbsp;<span class="page"/></td>
                    <td style="text-align:left;font-size:10;width:30px">&nbsp;of&nbsp;<span class="topage"/></td>
                </tr>
            </table>
        </body>
    </html>]]></field>
                <field name="orientation">Landscape</field>
                <field name="format">A4</field>
                <field name="html"><![CDATA[
    <html>
        <head>
            <meta content="text/html; charset=UTF-8" http-equiv="content-type"/>
            <script>
                function subst() {
                var vars={};
                var x=document.location.search.substring(1).split('&');
                for(var i in x) {var z=x[i].split('=',2);vars[z[0]] = unescape(z[1]);}
                var x=['frompage','topage','page','webpage','section','subsection','subsubsection'];
                for(var i in x) {
                var y = document.getElementsByClassName(x[i]);
                for(var j=0; j<y.length; ++j) y[j].textContent = vars[x[i]];
                    }
                }
            </script>
            <style type="text/css">
                ${css}
            </style>
        </head>
        <body style="border:0; margin: 0;" onload="subst()">
            <table class="header" style="border-bottom: 0px solid black; width: 100%">
                <tr>
                    <td style="text-align:left; font-size:11px; font-weight: bold;"><span style="text-transform:uppercase; font-size:12px;">${report_name}</span> - ${company.partner_id.name | entity} - ${company.currency_id.name | entity}</td>
                </tr>
            </table> ${_debug or ''|n} </body>
    </html>]]>
                </field>
                <field eval="0.0" name="margin_top"/>
                <field name="css"><![CDATA[

    body, table, td, span, div {
        font-family: Helvetica, Arial;
    }

    .act_as_table {
        display: table;
    }
    .act_as_row  {
        display: table-row;
    }
    .act_as_cell {
        display: table-cell;
    }
    .act_as_thead {
        display: table-header-group;
    }
    .act_as_tbody {
        display: table-row-group;
    }
    .act_as_tfoot {
        display: table-footer-group;
    }
    .act_as_caption {
        display: table-caption;
    }
    act_as_colgroup {
        display: table-column-group;
    }

    .list_table, .data_table {
        width: 1080px;
        table-layout: fixed
    }

    .bg, .act_as_row.labels {
        background-color:#F0F0F0;
    }

    .list_table, .data_table, .list_table .act_as_row {
        border-left:0px;
        border-right:0px;
        text-align:left;
        font-size:9px;
        padding-right:3px;
        padding-left:3px;
        padding-top:2px;
        padding-bottom:2px;
        border-collapse:collapse;
    }

    .list_table .act_as_row.labels, .list_table .act_as_row.initial_balance, .list_table .act_as_row.lines {
        border-color:gray;
        border-bottom:1px solid lightGrey;
    }

    .data_table .act_as_cell {
        border: 1px solid lightGrey;
        text-align: center;
    }

    .data_table .act_as_cell, .list_table .act_as_cell {
        word-wrap: break-word;
    }

    .data_table .act_as_row.labels {
        font-weight: bold;
    }

    .initial_balance .act_as_cell {
        font-style:italic;
    }

    .account_title {
        font-size:10px;
        font-weight:bold;
        page-break-after: avoid;
    }

    .act_as_cell.amount {
        word-wrap:normal;
        text-align:right;
    }

    .list_table .act_as_cell{
        padding-left: 5px;
    }
    .list_table .act_as_cell.first_column {
        padding-left: 0px;
    }

    .sep_left {
        border-left: 1px solid lightGrey;
    }

    .overflow_ellipsis {
        text-overflow: ellipsis;
        overflow: hidden;
        white-space: nowrap;
    }

    .open_invoice_previous_line {
        font-style: italic;
    }

    .clearance_line {
        font-style: italic;
    }

    ]]>
                </field>
                <field name="name">Requisition Landscape Header</field>
            </record>

            <record id="requisition_portrait_header" model="ir.header_webkit">
                <field name="footer_html"><![CDATA[
    <html>
        <head>
            <meta content="text/html; charset=UTF-8" http-equiv="content-type"/>
            <script>
                function subst() {
                var vars={};
                var x=document.location.search.substring(1).split('&');
                for(var i in x) {var z=x[i].split('=',2);vars[z[0]] = unescape(z[1]);}
                var x=['frompage','topage','page','webpage','section','subsection','subsubsection'];
                for(var i in x) {
                var y = document.getElementsByClassName(x[i]);
                for(var j=0; j<y.length; ++j) y[j].textContent = vars[x[i]];
                    }
                }
            </script>
        </head>
        <% import datetime %>
        <body style="border:0; margin: 0;" onload="subst()">
            <table style="border-top: 1px solid black; width: 1080px">
                <tr style="border-collapse:collapse;">
                    <td style="text-align:left;font-size:10;width:350px;">${formatLang( str(datetime.datetime.today()), date_time=True)}</td>
                    <td style="text-align:center;font-size:10;width:350px;">${user.name}</td>
                    <td style="text-align:right;font-size:10;width:350px;">Page&nbsp;<span class="page"/></td>
                    <td style="text-align:left;font-size:10;width:30px">&nbsp;of&nbsp;<span class="topage"/></td>
                </tr>
            </table>
        </body>
    </html>]]></field>
                <field name="orientation">Portrait</field>
                <field name="format">A4</field>
                <field name="html"><![CDATA[
    <html>
        <head>
            <meta content="text/html; charset=UTF-8" http-equiv="content-type"/>
            <script>
                function subst() {
                var vars={};
                var x=document.location.search.substring(1).split('&');
                for(var i in x) {var z=x[i].split('=',2);vars[z[0]] = unescape(z[1]);}
                var x=['frompage','topage','page','webpage','section','subsection','subsubsection'];
                for(var i in x) {
                var y = document.getElementsByClassName(x[i]);
                for(var j=0; j<y.length; ++j) y[j].textContent = vars[x[i]];
                    }
                }
            </script>
            <style type="text/css">
                ${css}
            </style>
        </head>
        <body style="border:0; margin: 0;" onload="subst()">
            <table class="header" style="border-bottom: 0px solid black; width: 100%">
                <tr>
                    <td style="text-align:left; font-size:11px; font-weight: bold;"><span style="text-transform:uppercase; font-size:12px;">${report_name}</span> - ${company.partner_id.name | entity} - ${company.currency_id.name | entity}</td>
                </tr>
            </table> ${_debug or ''|n} </body>
    </html>]]>
                </field>
                <field eval="17.0" name="margin_top"/>
                <field eval="15.0" name="margin_bottom"/>
                <field name="css"><![CDATA[

    body, table, td, span, div {
        font-family: Helvetica, Arial;
    }

    .act_as_table {
        display: table;
    }
    .act_as_row  {
        display: table-row;
    }
    .act_as_cell {
        display: table-cell;
    }
    .act_as_thead {
        display: table-header-group;
    }
    .act_as_tbody {
        display: table-row-group;
    }
    .act_as_tfoot {
        display: table-footer-group;
    }
    .act_as_caption {
        display: table-caption;
    }
    act_as_colgroup {
        display: table-column-group;
    }

    .list_table, .data_table {
        width: 690px;
        table-layout: fixed
    }

    .bg, .act_as_row.labels {
        background-color:#F0F0F0;
    }

    .list_table, .data_table, .list_table .act_as_row {
        border-left:0px;
        border-right:0px;
        text-align:left;
        font-size:9px;
        padding-right:3px;
        padding-left:3px;
        padding-top:2px;
        padding-bottom:2px;
        border-collapse:collapse;
    }

    .list_table .act_as_row.labels, .list_table .act_as_row.initial_balance, .list_table .act_as_row.lines {
        border-color:gray;
        border-bottom:1px solid lightGrey;
    }

    .data_table .act_as_cell {
        border: 1px solid lightGrey;
        text-align: center;
    }

    .data_table .act_as_cell, .list_table .act_as_cell {
        word-wrap: break-word;
    }

    .data_table .act_as_row.labels {
        font-weight: bold;
    }

    .initial_balance .act_as_cell {
        font-style:italic;
    }

    .account_title {
        font-size:10px;
        font-weight:bold;
        page-break-after: avoid;
    }

    .act_as_cell.amount {
        word-wrap:normal;
        text-align:right;
    }

    .list_table .act_as_cell{
        padding-left: 5px;
    }
    .list_table .act_as_cell.first_column {
        padding-left: 0px;
    }

    .sep_left {
        border-left: 1px solid lightGrey;
    }

    .account_level_1 {
        text-transform: uppercase;
        font-size: 15px;
        background-color:#F0F0F0;
    }

    .account_level_2 {
        font-size: 12px;
        background-color:#F0F0F0;
    }

    .account_level_5 {

    }

    .regular_account_type {
        font-weight: normal;
    }

    .view_account_type {
        font-weight: bold;

    .account_level_consol {
        font-weight: normal;
        font-style: italic;
    }

    .overflow_ellipsis {
        text-overflow: ellipsis;
        overflow: hidden;
        white-space: nowrap;
    }

    ]]>
                </field>
                <field name="name">requisition Portrait Header</field>
            </record>
        </data>
    </openerp>

Sobreescribir el create de account_invoice_line
-----------------------------------------------

Cuando se sobreescribe el metodo create de account_invoice_line
y se esta creando la linea desde la linea de reclamo a cliente
se toma el id de la linea del reclamo y se le asigna al reclado 
la linea que se esta creando.

class account_invoice_line(orm.Model):

    _inherit = "account.invoice.line"

    def create(self, cr, uid, vals, context=None):
        claim_line_id = False
        if vals.get('claim_line_id'):
            claim_line_id = vals['claim_line_id']
            del vals['claim_line_id']
        line_id = super(account_invoice_line, self).create(
            cr, uid, vals, context=context)
        if claim_line_id:
            claim_line_obj = self.pool.get('claim.line')
            claim_line_obj.write(cr, uid, claim_line_id,
                                 {'refund_line_id': line_id},
                                 context=context)
        return line_id


Test pylint and flake
---------------------

git clone git@github.com:OCA/maintainer-quality-tools.git

pylint --rcfile=~/Roots/instancias/odoo/maintainer-quality-tools/travis/cfg/travis_run_pylint.cfg .
flake8 --config=~/Roots/instancias/odoo/maintainer-quality-tools/travis/cfg/travis_run_flake8 .

Decorador para ejecutar tests o codigo python cuando la data demo sea False
---------------------------------------------------------------------------

@openerp.tests.common.at_install(False)
@openerp.tests.common.post_install(True)


Hacer referencia a un xml id desde vistas, data y python
========================================================

action domain windows
---------------------

    <record model="ir.actions.act_window" id="crm_claim_rma.act_crm_case_claim_lines">
        <field name="domain" eval="[('claim_id.stage_id.id', '!=', ref('crm_claim.stage_claim1'))]"/>
    </record>

attrs para un campo
-------------------

    <xpath expr="//field[@name='date']" position="attributes">
        <attribute name="attrs">{'readonly':[('stage_id','!=',%(crm_claim.stage_claim1)d)]}</attribute>
    </xpath>

    <xpath expr="//button[@name='render_metasearch_view']" position="attributes">
        <attribute name="attrs">{'invisible': [('stage_id', '!=', %(crm_claim.stage_claim1)d )]}</attribute>
    </xpath>
    
    <div class="oe_form_box_info oe_text_center"
            attrs="{'invisible':[('stage_id','!=',%(crm_claim.stage_claim1)d)]}">
        <b><field name='message' readonly="1"/></b>
    </div>
    
    <record id="filter_customer_new" model="ir.filters">
        <field name="name">New</field>
        <field name="model_id">crm.claim</field>
        <field name="domain" eval="[('stage_id', '=', ref('crm_claim.stage_claim1'))]"/>
        <field name="context">{}</field>
        <field name="user_id"></field>
    </record>


atributo options en field
-------------------------

<field name="category_id" widget="many2many_tags" options="{'limit':
1,'no_create_edit': True,'no_quick_create': True}"/>


<field name="attribute_line_ids" widget="one2many_list" context="{'show_attribute': False}">
    <tree string="Variants" editable="bottom">
        <field name="attribute_id"/>
        <field name="value_ids" widget="many2many_tags" options="{'no_create_edit': True}" domain="[('attribute_id', '=', attribute_id)]" context="{'default_attribute_id': attribute_id}"/>
    </tree>
</field>

funciones workflow por xml assert test transformar referencias a objetos
------------------------------------------------------------------------

<assert id="test_order_1" model="sale.order" string="the sales order is now in 'Manual in progress' state">
    <test expr="state">manual</test>
</assert>

<workflow action="manual_invoice" model="sale.order" ref="test_order_1" uid="base.user_root"/>



<function model="stock.transfer_details"
name="do_detailed_transfer" eval="[ref('transfer_sale_2'), True, False]"/>

<workflow action="manual_invoice" model="sale.order" ref="sale_order_rma_1" uid="base.user_root"/>

<workflow action="invoice_open" model="account.invoice">
    <value eval="obj(ref('sale_order_rma_1')).invoice_ids[0].id" model="sale.order"/>
</workflow>

<record id="transfer_sale_2" model="stock.transfer_details">
    <field name="picking_id" model="stock.picking" search="[('origin', '=', 'Order sale RMA 1')]"/>
    <field name="picking_source_location_id" ref="stock.stock_location_stock"/>
    <field name="picking_destination_location_id" ref="stock.stock_location_customers"/>
</record>

<function
    model="sale.order"
    name="action_button_confirm" eval="[[ref('sale_order_rma_1')]]"/>

<record id="stock_move_rma" model="stock.move">
    <function eval="[[('purchase_line_id.id', '=', ref('purchase_order_rma_1_line_1'))]]" model="stock.move" name="search"/>
</record>

<function model="event.event" name="button_confirm" eval="[ref('event_0')]"/>

<record id="stock_picking_rma" model="stock.picking">
    <function eval="[[('move_lines', '=', ref('stock_move_rma'))]]" model="stock.move" name="search"/>
</record>

<function
    eval="('default',False,'warehouse_id', [('purchase.requisition', False)], ref('stock.warehouse0'), True, False, False, False, True)"
    id="purchase_default_set"
    model="ir.values"
    name="set"/>

<function model="account.invoice" name="pay_and_reconcile">
    <value eval="[obj(ref('test_order_1')).invoice_ids[0].id]" model="sale.order"/>
    <value eval="obj(ref('test_order_1')).amount_total" model="sale.order"/>
    <value model="account.account" search="[('type', '=', 'cash')]"/>
    <value eval="ref('account.period_' + str(int(time.strftime('%m'))))"/>
    <value eval="ref('account.bank_journal')"/>
    <value model="account.account" search="[('type', '=', 'cash')]"/>
    <value eval="ref('account.period_' + str(int(time.strftime('%m'))))"/>
    <value eval="ref('account.bank_journal')"/>
</function>

<function model="stock.picking" name="action_assign">
    <value eval="[obj(ref('test_order_1')).picking_ids[0].id]" model="sale.order"/>
</function>

<!-- Run all schedulers -->
<function model="procurement.order" name="run_scheduler"/>

<record id="test_order_1" model="sale.order">
    <field model="product.pricelist" name="pricelist_id" search="[]"/>
    <field name="user_id" ref="base.user_root"/>
    <field model="res.partner" name="partner_id" search="[]"/>
    <field model="res.partner" name="partner_invoice_id" search="[]"/>
    <field model="res.partner" name="partner_shipping_id" search="[]"/>
</record>



warning en el init
------------------

import logging
import os


_logger = logging.getLogger(__name__ + ".deprecated")

# Skip warnings on runbots
_method = _logger.info if "OCA_RUNBOT" in os.environ else _logger.warning
_method("This module is DEPRECATED. See %s/README.rst.",
        os.path.dirname(__file__))


Mute logger para warnings
-------------------------

 from openerp import exceptions
+from openerp.tools import mute_logger
 
 
 class TestSalesManPayments(TransactionCase):
 @@ -29,6 +30,7 @@ def setUp(self):
         self.account_id = self.env.ref("account.a_recv")
         self.product_id = self.env.ref("product.product_product_6")
 
+    @mute_logger('openerp.addons.base.ir.ir_model', 'openerp.osv.orm')
     def test_create_payment(self):
         """
             Sales Man can not Validate account invoice

Library for panama ruc
----------------------

pip install git+https://github.com/vauxoo/panama-dv.git

Campos agrupados mostrar string de un campo al agruparlo
--------------------------------------------------------

Para poder ver el string real de un campo cuando se agrupa
y no el nombre técnico, debe agregarse el campo
a la vista tree del modelo


Problema con las vistas
-----------------------

En odoo vista list y tree son dos vistas distintas. Y no están
bien definidas en odoo, debido a que se les asignó el mismo nombre
técnico "tree".

list
     ------ "tree"
tree

Entonces al definir view_type=tree me va a mostrar la vista tree
como se ve en chart of accounts. Entonces si le digo
view_type=form, me va a mostrar la vista form en caso de que
esté de primero en view_mode=tree,form. pero como en este
caso, no está de primero, mostrará la vista tree, que
se muestra por pura suerte porque en el view_id= esta definido
el link a la vista form.

Entonces, en view_id se debe especificar la vista tree para que
quede consistente.

Mustrame la vista form en caso de que sea la primera
en la lista, sino, muestra la primera en la lista.

view_type>form<
view_mode>tree,form<
view_id="......_tree"

Como en este caso, se va a mostrar tree de primero,
entonces en el view_id debe estar linkeada al tree.


Cablear vista en un campo many2one
----------------------------------

<field name="wave_id" context="{'default_mrp': True, 'form_view_ref':'lodi_customized_picking_transfer.view_mrp_wave_form'}"/>

o también y mejor..

+    @api.multi
 +    def get_formview_id(self):
 +        """ Return an view id to open the document with. This method is meant to be
 +            overridden in addons that want to give
 +            specific view ids for example.
 +
 +            :param int id: id of the document to open
 +        """
 +        customer_type = self.env.ref("crm_claim_type.crm_claim_type_customer")
 +        my_type = self.claim_type
 +        customer_view = self.env.ref(
 +            "yoytec_customer_rma_workflow.crm_claim_customer_view_form")
 +        supplier_view = self.env.ref(
 +            "yoytec_supplier_rma_workflow.crm_claim_supplier_view_form")
 +        view = my_type == customer_type and customer_view or supplier_view
 +        return view.id




Actions
-------

si se agrega un menuitem y no tiene action no se verá el menu
