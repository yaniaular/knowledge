**Cuando los repo son privados hay que colocar el link ssh**

Crear docker para pruebas con travis
------------------------------------

**Para repo publico**

1. travisfile2dockerfile https://github.com/vauxoo-dev/rma 8.0-rma_metsearch_field-dev-yani

**Para repo privado**

1.1 travisfile2dockerfile git@github.com:Vauxoo/yoytec.git pull/112

**Al link generado**

2. cd /tmp/.../1

.. #. FROM vauxoo/odoo-80-image-shippable-auto
.. #. cambiar el branch de OCA a Vauxoo

**Construir**

3. ./10-build.sh (sudo docker build -t rma .)

**Correr la imagen**

4. ./20-run.sh (sudo docker images)

.. note::

    cada vez que se edite el Dockerfile 
    ejecutar de nuevo sudo docker build -t rma .

5. sudo docker run -itP --entrypoint=bash IMAGE --La P mayusuculas es para conectar a la instancia

6. sudo docker run -it IMAGE

7. root@75a63c640b2c:~#

8. LINT_CHECK=0 PATH=${HOME}/maintainer-quality-tools/travis:${PATH} travis_run_tests

9. Dentro de la imagen puedo hacer /entrypoint

sudo docker ps -a (Ver que docker todos los docker)
sudo docker ps (Ver que docker estan activos)
sudo docker start CONTAINER_ID (si no está activo, entonces tengo que levantarlo)
sudo docker exec -it CONTAINER_ID bash (si el docker esta activo, entro)

docker build -> Créate una nueva imagen con esta receta (Dockerfile)
docker run -it --entrypoint=bash IMAGE -> Create una instancia de la imagen creada anteriormente
docker start CONTAINER_ID -> Despiértate 
docker exec -it CONTAINER_ID bash -> Me meto en la maquinita y hago lo que yo quiera
/entrypoint.sh -> es un script que te levanta base de datos, que te permite correr con root el odoo, y que te genera una base de datos openerp_template con todas las dependencias
y openerp_test con todos los módulos de tu proyecto probándose
Tú puedes parar el script /entrypoint.sh en el paso que tú quieras, para trabajar o levantar los servidores, como tú quieras...
Solo te copias los "--addons-path" de lo que te dé el entrypoint.sh
¿me explico?

dropdb openerp_template
dropdb openerp_test

psql -l
docker exec -it CONTAINER_ID bash

Si haces source ~/.profile
obtienes una shell más chingona
con lo que mandé en el correo de features docker
De hecho está de color fucsia


La ejecucion de la instancia se puede copiar y pegar
desde el archivo Dockefile al final de la linea


.. note::

    Correr el que construye.

    from vauxoo-doo-images
    colocar imagen desde odoo-mexico-v2 moi lo paso por chat.

    python travisfile2dockerfile.py git@github.com:Vauxoo/rma.git pull/21
    ejecuta así, please

    wget -qO- https://get.docker.com/ | sh

    Y luego, ejecutas esto: docker pull vauxoo/odoo-80-image-shippable-auto
    vauxoo/odoo-80-image-shippable-auto

    FROM vauxoo/odoo-80-image-shippable-auto

    docker build -t rma .

    docker images | grep rma
    docker run -it --entrypoint=bash rma
