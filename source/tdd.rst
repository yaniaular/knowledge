TDD
===

Autores
-------

Python TDD web

KEnt Bock TDD By example. Java

Teoría
------

En primer lugar se debe definir una lista de requisitos y después se ejecuta el siguiente ciclo:

#. Elegir un requisito: Se elige de una lista el requerimiento que se cree que nos dará mayor conocimiento del problema y que a la vez sea fácilmente implementable.

#. Escribir una prueba: Se comienza escribiendo una prueba para el requisito. Para ello el programador debe entender claramente las especificaciones y los requisitos de la funcionalidad que está por implementar. Este paso fuerza al programador a tomar la perspectiva de un cliente considerando el código a través de sus interfaces.

#. Verificar que la prueba falla: Si la prueba no falla es porque el requerimiento ya estaba implementado o porque la prueba es errónea.

#. Escribir la implementación: Escribir el código más sencillo que haga que la prueba funcione. Se usa la metáfora "Déjelo simple" ("Keep It Simple, Stupid" (KISS)).

#. Ejecutar las pruebas automatizadas: Verificar si todo el conjunto de pruebas funciona correctamente.

#. Eliminación de duplicación: El paso final es la refactorización, que se utilizará principalmente para eliminar código duplicado. Se hacen de a una vez un pequeño cambio y luego se corren las pruebas hasta que funcionen.

#. Actualización de la lista de requisitos: Se actualiza la lista de requisitos tachando el requisito implementado. Asimismo se agregan requisitos que se hayan visto como necesarios durante este ciclo y se agregan requerimientos de diseño (P. ej que una funcionalidad esté desacoplada de otra).

PyUnit
======



