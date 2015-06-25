**Cuando los repo son privados hay que colocar el link ssh**

Crear docker para pruebas con travis
------------------------------------

1.  python travisfile2dockerfile.py https://github.com/vauxoo-dev/rma 8.0-rma_metsearch_field-dev-yani

1.1 python travisfile2dockerfile.py git@github.com:Vauxoo/yoytec.git pull/112

2. cd /tmp/...

.. #. FROM vauxoo/odoo-80-image-shippable-auto
.. #. cambiar el branch de OCA a Vauxoo

3. sudo docker build -t rma .

4. sudo docker images

.. note::

    cada vez que se edite el Dockerfile 
    ejecutar de nuevo sudo docker build -t rma .


.. #. sudo docker run -it --entrypoint=bash rma 

5. sudo docker run -it rma 

8. root@75a63c640b2c:~# 

9. LINT_CHECK=0 PATH=${HOME}/maintainer-quality-tools/travis:${PATH} travis_run_tests

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
