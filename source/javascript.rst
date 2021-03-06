
==========
JavaScript
==========

----------------------------------
Herramientas para comprimir código
----------------------------------

- **JSMin**
- **YUI Compresor**
- **Google Closure Compiler**

--------------------------------------
Herramienta para la calidad del código
--------------------------------------

- **JSLint**: http://jslint.com

-----------------------
Librerías en JavaScript
-----------------------

- **Google's Closure Library**: http://code.google.com/closure/library
- **Mootools**: http://mootools.net
- **YUI Library**: http://developer.yahoo.com/yui
- **dojo**: http://dojotoolkit.org
- **jQuery**: http://jquery.com Ayuda a manejar el DOM, eventos, animaciones, etc.
- **Impress**: Presentaciones
- **Lightbox2**: Imágenes 
- **Script.aculo.us**:
- **moo.fx**
- **CurvyCorners**

------
jQuery
------

Consultar por ID:

.. code-block :: javascript                

    jQuery("#myDiv").addClass("highlight");
    
Consultar clases:

.. code-block :: javascript                

    jQuery(".someClass");

Consultar elementos:

.. code-block :: javascript                

    jQuery("p");

Llamada multiple a elementos:

.. code-block :: javascript                

    jQuery("#myDiv").addClass("highlight");
                    .removeClass("highlight");
                    .toggleClass("highlight");

.. note::

    $ es lo mismo que jQuery

Ejemplos de eventos:

.. code-block :: javascript                


    $("#pageID").click(function() {
        $("pageID").text("You clicked me!");
    });

    $("h2").click(function() {
        $(this).text("You clicked me!");
    });
    //this se refiere a h2

    $("p").click(function() {
        $(this).fadeOut(2000);
    });
    //Desaparece el elemento en 2 segundos

    $(document).ready(function() {
        $("#pageID").text("The DOM is fully loaded.");
    });
    // instancia de window.onload, cuando se cargue el DOM de window entonces se ejecutará la función

    $(document).ready(function() {
        $("h1").css("color","red");
    });

---------------------------------------------
Librerías javascript disponibles desde google
---------------------------------------------

code.google.com/apis/libraries

------------------
http://caniuse.com
------------------

Provee tablas de compatibilidad para características de HTML5 y CSS3.

--------------------
http://modernizr.com
--------------------

Permite detectar características de HTML5 y CSS3 facilmente.

-------------------
http://jqueryui.com
-------------------

Se puede crear páginas web interactivas fácilmente.

-------------
Documentación
-------------

https://developer.mozilla.org/en-US
http://dev.opera.com
http://jquery.com
http://developer.yahoo.com/javascript
http://developer.yahoo.com/performance
http://stackoverflow.com




