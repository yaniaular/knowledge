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

--------
Deepcopy
--------

Assignment statements in Python do not copy objects, they create bindings
between a target and an object. For collections that are mutable or contain
mutable items, a copy is sometimes needed so one can change one copy without
changing the other. This module provides generic shallow and deep copy
operations (explained below).

Interface summary:

copy.copy(x)
Return a shallow copy of x.

copy.deepcopy(x) Return a deep copy of x.

exception copy.error Raised for module specific errors.

The difference between shallow and deep copying is only relevant for compound
objects (objects that contain other objects, like lists or class instances):

A shallow copy constructs a new compound object and then (to the extent
possible) inserts references into it to the objects found in the original.  A
deep copy constructs a new compound object and then, recursively, inserts
copies into it of the objects found in the original.  Two problems often exist
with deep copy operations that don’t exist with shallow copy operations:

Recursive objects (compound objects that, directly or indirectly, contain a
reference to themselves) may cause a recursive loop.  Because deep copy copies
everything it may copy too much, e.g., administrative data structures that
should be shared even between copies.  The deepcopy() function avoids these
problems by:

keeping a “memo” dictionary of objects already copied during the current
copying pass; and letting user-defined classes override the copying operation
or the set of components copied.  This module does not copy types like module,
method, stack trace, stack frame, file, socket, window, array, or any similar
types. It does “copy” functions and classes (shallow and deeply), by returning
the original object unchanged; this is compatible with the way these are
treated by the pickle module.

Shallow copies of dictionaries can be made using dict.copy(), and of lists by
assigning a slice of the entire list, for example, copied_list =
original_list[:].

Changed in version 2.5: Added copying functions.

Classes can use the same interfaces to control copying that they use to control
pickling. See the description of module pickle for information on these
methods. The copy module does not use the copy_reg registration module.

In order for a class to define its own copy implementation, it can define
special methods __copy__() and __deepcopy__(). The former is called to
implement the shallow copy operation; no additional arguments are passed. The
latter is called to implement the deep copy operation; it is passed one
argument, the memo dictionary. If the __deepcopy__() implementation needs to
make a deep copy of a component, it should call the deepcopy() function with
the component as first argument and the memo dictionary as second argument.

https://docs.python.org/2/library/copy.html
