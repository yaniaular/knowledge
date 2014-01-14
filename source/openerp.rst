---
ORM
---

server/openerp/osv/orm.py
Linea 4006, 

Se explica como agregar valores
a campos one2many many2many, etc
con el metodo write

-----------
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

------
id xml
------

 grep -rn --include=*.xml search . | grep field | grep model
<field name="fields_id" search="[('model','=','product.category'),('name','=','property_account_income_categ')]"/>

