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

-----------
runsnakerun
-----------

------
PyLint
------

--------
Pyflakes
--------


lxml
----

.. code-block :: python

    doc = etree.XML(res['arch'])
    mess = doc.xpath("//field[@name='message_follower_ids']")
    mess[0].append(etree.Element('field'))
    etree.tostring(mess[0])
    res['arch'] = etree.tostring(mess[0])


    doc = etree.XML(res['arch'])
    xml_original = res['arch']
    nodos = doc.xpath("//field[@name='message_follower_ids']")
    nodos
    apl = etree.Element('field')
    apl.set('name','apl_ids')
    apl
    nodos[0].append(apl)
    etree.tostring(doc)
    res['arch'] = etree.tostring(doc)




    # Este metodo debe ir en el modulo bdp_purchase_requisition y se encarga de agregar la vista de
    # schedule power, para que asi, cualquier modelo que se cree de la forma en que se hizo con
    # purchase requisiton bidders, tenga la vista de schedule power.
        def fields_view_get(self, cr, uid, view_id=None, view_type='form',
                            context=None, toolbar=False, submenu=False):
            if context is None:
                context={}
            fields_view = self.pool[self._inherit].fields_view_get(cr, uid, view_id=view_id, view_type=view_type, context=context, toolbar=toolbar,submenu=False)



            res = super(purchase_requisition, self).fields_view_get(cr, uid, view_id=view_id, view_type=view_type, context=context, toolbar=toolbar,submenu=False)

            import pdb
            #pdb.set_trace()

            #doc_bidders = etree.XML(res['arch'])
            #doc = etree.XML(fields_view['arch'])

           # node2 = etree.XML(xml)
            #for node in doc.xpath("//field[@name='state']"):
            #    node.set('statusbar_visible', "long_list")
            #    import pdb
            #    #pdb.set_trace()
            #res['arch'] = etree.tostring(doc)
            #res.update(fields_view)
            doc = etree.XML(res['arch'])
            #xml_original = res['arch']
            nodos = doc.xpath("//div[@class='oe_chatter']")
            apl = etree.Element('field')
            apl.set('name','apl_ids')
            if nodos:
                nodos[0].insert(0, apl)
                #nodos[0].append(apl)
                res['arch'] = etree.tostring(doc)
                print res['arch']
            #etree.tostring(doc)
            return res
