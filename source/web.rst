Instalación de SASS
-------------------


Comando de instalación de GEM

sudo apt-get install gem

Comandos de instalación de Ruby
sudo apt-get install ruby1.9.1	
sudo pacman -S ruby

Comando de instalación de SASS

sudo gem install sass

Comando por si los usuarios pueden ver un error no crítico de 'polling'. Para solucionar este problema, ejecute:
sudo gem install listen rb-fsevent



¿Qué es SASS?
-------------

http://es.wikipedia.org/wiki/Sass_(lenguaje_de_hojas_de_estilo)




Indice:
1.- Como construir un módulo de reporte de facturación electrónica.
	1.1 TODO: Creación del módulo con el nombre que le proporcione el cliente. (Script)
	1.2 Preview Invoice.
	1.3 Modificación de css por medio de SASS

1 Como construir un módulo de reporte 
de facturación electrónica 

1.1  TODO: Creación del módulo con el nombre que le proporcione el cliente. (Script)

	Por el momento esta opción aún no se encuentra disponible. Por lo pronto se trabaja realizando un copy paste de algún reporte, en el cual se debe realizar los cambios con el nuevo nombre del módulo. Estos archivos que se deben modificar son:

__openerp__.py: En este archivo se cambia el nombre del modulo (‘name’) y la referencia hacia el nombre del archivo de este modulo (‘data’).
l10n_mx_facturae_report_webkit.xml: En este archivo se debe cambiar dentro de <report/>  los valores de id, el name, file (ruta teniendo en cuenta el nuevo nombre del módulo) y string. Mientras que del property la referencia con el nuevo nombre del módulo y el id.
report/invoice_facturae_html.py: En este archivo se modifica la función de webkit_report.WebKitParser, en este parte hay que tener cuidado con el primer parámetro de esta función ya que esta compuesta por report.id_report, donde id_report tiene que ser el id que se haya asignado en el archivo de l10n_mx_facturae_report_webkit.xml al reporte, el segundo es el modelo este no se debe cambiar, el tercero es la ruta hacia nuestro archivo mako en el cual debemos cambiar el nombre del módulo nuevo de nuestro reporte y el cuarto parámetro se deja igual.
data/ : dentro de esta carpeta hay que cambiar el nombre de nuestro xml (Esto es opcional) tal cual lo hayamos declarado en nuestro __openerp__.py
Si se realizó el cambio del nombre del archivo .xml o no ingresamos en él y modificamos el id del header y el nombre del header.

	Una vez realizados estos pasos ya podemos instalar nuestro módulo en OpenERP.

1.2 Preview Invoice

	Dentro de nuestro módulo de reporte y la carpeta data estara preview_invoice dentro de esta carpeta encontraras un archivo html el cual te apoyará en la creación de tus reportes de una forma ágil. Recomendable realizar las modificaciones en este archivo html hasta lograr tu diseño deseado.


Para ver un preliminar de cómo se visualiza en reporte pdf basta con presionar crtl+p y ver el acomodo que lleva nuestra factura.



1.3 Modificación de css por medio de SASS


	Esta imagen es una representación de branch de los reportes y 2 modulos scss_template que es “módulo base” y módulo de mi reporte en los cuales encontraremos esos archivos que son los que le darán vida a nuestro css.

Los archivos que se deberán modificar son los que se encuentren dentro de del módulo que estemos creando de reporte, ya que, los archivos de panels, tables y variables_generales contienen estilos muy particulares definidos para estos componentes y los archivos de data son los que contienen la configuración que hace diferente un reporte de otro.

El archivo que se encuentra en data/sass/variables_reporte.scss contiene como su nombre lo dice las variables que alimentan a los scss_template y modificando alguna de ellas nuestro reporte cambiará totalmente. Un claro ejemplo es:

Tenemos en el ejemplo el reporte orange, si lo deseamos cambiar a color verde en cuanto a títulos de tablas, colores de los titulos en las cadenas del cfdi y demás, en el archivo de variables_reporte modificamos la variable de nombre $background_color_titles_table_invoice cambiando el valor que tiene ya sea por un hexadecimal o alguno ya predefinido en las variables generales (se encuentran como comentario los colores disponibles en variables_generales en caso de no encontrar el color se debe colocar el hexadecimal). Ejemplo:

$background_color_titles_table_invoice:$color_pink;-->$background_color_titles_table_invoice: $color_green;
ó
$background_color_titles_table_invoice:$color_pink;---->$background_color_titles_table_invoice: green;
ó
$background_color_titles_table_invoice:$color_pink;---->$background_color_titles_table_invoice: #144505;


Realizando ese mínimo cambio nuestro reporte luciria de esta forma.

Entre otros cambios que se pueden realizar desde el archivo de variables_reporte.scss, tales como tamaño de fuente, tipo de fuente, color de márgenes, colores de fuentes, etc.


El archivo l10n_mx_facturae_css.scss se encontraran las class definidas por nosotros de las cuales te puedes apoyar de las variables del reporte y las generales para otorgar estilos o bien pueden tu asignar valores. Si existiera alguna variable no contemplada se puede declarar en este archivo para su uso ( esto depende de cada desarrollador). Como mencione se crean aquí las class particulares de cada módulo definiendo estilos muy propios de nuestro reporte.

	Cada ocasión que exista un cambio tanto en variables_reporte.scss y l10n_mx_facturae_css.scss se deberá actualizar o crear (en caso de no existir) el archivo l10n_mx_facturae_css.css y esto se realiza con el siguiente comando. Para ello ya debemos tener instalado lo comentado al principio de este documento. ( Instalación de SASS ). 
$ sass --update nombre_archivo.scss

$ sass --update l10n_mx_facturae_css.scss 
	Una vez realizado este paso si tenemos abierto nuestro preview_invoice.html en el navegador solo bastará dar F5 para refrescar nuestro reporte y ctrl+p para ver nuestro reporte como se afecto con los cambios realizados.

	En caso de querer visualizar ya este reporte en OpenERP, necesitamos copiar el contenido del archivo l10n_mx_facturae_css.css que se generó o actualizo después del paso anterior dentro del tag <css>, OJO si el html tuvo cambios de diseño en el html se deben realizar también en el archivo .mako para poder ver nuestro diseño como lo tenemos en el preview_inovice.html. Una vez realizado el copiado del css en el xml y actualizado nuestro .mako (en caso de cambios de diseño) reiniciamos nuestro servidor con un -u nombre_del_modulo para que se actualice nuestro css y generamos la factura para ver nuestro reporte en OpenERP.



Compass
-------

Es un framework para creación de CSS open-source. Simplifica, administra y automatiza el manejo
de archivos css y scss usando sass. Esta desarrollada en ruby.

Crear un proyecto compass:

$ compass create

Con esto, se creara un archivo de configuracion ``config.rb``
en el cual se puede determinar la carpeta de salida ``css`` la carpeta de los archivos de 
entrada ``scss``, el directio principal ``/``, la carpeta con las imágenes y código javascript.

Se pueden borrar las carpetas predeterminadas y crear otras nuevas con el nombre a gusto.
Si se han modificado los archivos scss, se puede actualizar lo css con el siguiente comando.

$ compass compile

Para más información consultar el help de compass.

$compass --help

Compila con Sass
----------------

Para compilar con sass solo necesitamos el archivo scss y el destino del css por ejemplo:

$ sass scss/file.scss css/file.css
