




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

**jQuery("#myDiv").addClass("highlight");**: Consultar por ID
**jQuery(".someClass");**: Consultar clases
**jQuery("p");**: Consultar elementos

**jQuery("#myDiv").addClass("highlight");**:
                **.removeClass("highlight");**:
                **.toggleClass("highlight");**:
$ == jQuery

Ejemplos de eventos:

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

