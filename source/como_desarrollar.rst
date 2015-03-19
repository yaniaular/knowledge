Manejo de instancias:
=====================

El siguiente correo tiene como intención aclarar el uso que se le da en Vauxoo
a las distintas instancias de los clientes y los nombres asociados.

En términos generales contamos con las siguientes instancias:

- **Producción**: su función es bastante obvia, debe existir una única base de
  datos asociada al usuario de producción y es en la que el cliente realiza sus
  actividades diarias asociadas a Odoo.

- **Test o pruebas**: es una copia exacta de producción (con las mismas versiones
  de los repos y ramas) y es la instancia en la que se realizan PoC[1], réplica
  de errores, configuraciones antes de aplicarlas en prod, etc (todo aquello
  que NO implique cambios de código o actualizaciones del mismo)

- **Desarrollo**: Este tipo de instancias es en el que se prueban las ramas antes
  determinar ser mezcladas en la rama estable según corresponda (pueden ser instancias
  de runbot, locales o en el servidor destinado para ese fin).

- **Updates o preproducción**: es la instancia en la que se prueban los cambios
  antes de ser pasados a producción y luego de ser mezclados en las ramas
  estables (actualizaciones, etc)

Es sumamente importante que tomen esto en cuenta para su flujo de trabajo y la
estimación de los tiempos.

Descripción de PR
=================

1. Se debe ser específico en el PR, explicar el (por qué y/o para que) se 
   está haciendo dicho desarrollo. Ejemplos:
    1. Se hace establece la condición de days < 7 para poder determinar
       si la prioridad del reclamo es alta o muy alta, y asi, los técnicos
       tendrán una lista ordenada de reclamos que deben atender.
    2. Se sobreescribe el metodo fields_view_get y se asigna un domain
       en el campo facturas, ya que, solo deben mostrarse las facturas
       que pertenezcan al cliente que está haciendo el reclamo, y así
       evitar desplegar una lista larga de facturas que no tienen que
       ver con el cliente.

2. Documentar todo muy bien en la HU y la Tarea linkearlo en el PR.

3. Colocar la tarea en la descripcion del PR la tarea que se trabajo en ese
branch (De la manera #vx5678), en el caso de que resuelvan varias tareas, 
se deben colocar el id de la tarea en el commit y especificar bien el 
commit (Ver punto 1).

4. Cuando se hagan correcciones de pylint, se debe realizar amend en el ultimo commit.
   Evitar hacer commits del tipo "[IMP] travis"






