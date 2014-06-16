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

postgresql-9.3            postgresql-client-9.1     postgresql-client-common  
postgresql-client         postgresql-client-9.3     postgresql-common

createuser openerp -d -r -l -P
dropuser openerp
psql --list
> alter database NAME owner to NEW_OWNER;

--------------------
Borrar base de datos
--------------------

dropdb nombre_base_de_datos

-------------------------------------------------
Exportar datos a csv desde base de datos postgres
-------------------------------------------------

COPY stock_warehouse TO '/tmp/warehouse_199.csv' DELIMITER ',' CSV HEADER;
pg_dump --table=nombre_tabla Base_de_datos > /tmp/nombre_tabla.sql
-----------------------------
Ejecutar script desde el psql
-----------------------------

bd=#\i limpiar.sql

---------
Backup DB
---------

pg_dump dbname > outfile

