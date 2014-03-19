==========
PostgreSQL
==========

---------------------------
crear base de datos openerp
---------------------------

createdb -O usuario_base_de_datos -T template0 -E UTF8 nombre_base_de_datos
psql -d nombre_base_de_datos -f archivo_sql.sql -U usuario_base_de_datos

-----------------------------
Entrar con usuario postgreSQL
-----------------------------

$sudo su postgresql

--------------------
Borrar base de datos
--------------------

dropdb nombre_base_de_datos

-------------------------------------------------
Exportar datos a csv desde base de datos postgres
-------------------------------------------------

COPY stock_warehouse TO '/tmp/warehouse_199.csv' DELIMITER ',' CSV HEADER;

-----------------------------
Ejecutar script desde el psql
-----------------------------

bd=#\i limpiar.sql

