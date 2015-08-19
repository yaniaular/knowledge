Como desarrollar correctamente
==============================

Crear un modulo
===============

- El description en el __openerp__.py ya no se usa. 
  Usar un README.md o README.rst por modulo y por repo.

- Todo repo debe tener un APPs

    Reglas básicas de apps

    #.- Cuidado con las referencias cruzadas
    #.- Todo debe poderse instalar independiente
        Es decir, si se tiene módulo A B y C.
        A es un módulo por que no necesitará NADA de B y C
        igual con B, y así con C, son features aislados. 
        Si necesitas algún módulo en común para todos
        debes pensarlo bastante bien, generalmente no es tan 
        necesario si haces bien el trabajo en diseño


- Linkear los PR con las historias de usuario y tareas
  involucradas

- Crear demo data siempre en los modulos, se puede usar
  yml o xml. Crear tests unitarios en yml o python.
  demo: [], test: []. Nunca hacer pruebas en el demo
  (por ejemplo llamadas a funciones con el tag function)
  y no crear demo data historica desde los test (por ejemplo
  stock moves)

- Recordar colocar los atributos help a cada campo

- Documentar los metodos creados en los modulos a excepción
  de los metodos de la orm sobreescritos, como search, create, etc

- Hacer campos nuevos buscables

- La prioridad esta basada en los desarrollos que agreguen
  funcionalidad

- Por repositorio se debe agregar un archivo **setup.py** que instale
  las librerias necesarias para el funcionamiento de los modulos
  involucrados en el repositorio

    .. code-block :: python

        #!/usr/bin/env python
        # -*- coding: utf-8 -*-

        import os
        import sys


        try:
            from setuptools import setup, find_packages
        except ImportError:
            from distutils.core import setup, find_packages

        if sys.argv[-1] == 'publish':
            os.system('python setup.py sdist upload')
            sys.exit()

        readme = open('README.md').read()
        requirements = open('requirements.txt').readlines()

        setup(
            name='vxtools_base',
            version='0.1.0',
            description='Base for all clases and scripts in VauxooTools',
            long_description=readme,
            author='Tulio Ruiz',
            author_email='tulio@vauxoo.com',
            url='https://github.com/vauxoo-dev/vxtools-base',
            download_url='https://github.com/vauxoo-dev/vxtools-base',
            packages=find_packages(),
            include_package_data=True,
            install_requires=requirements,
            license="BSD",
            zip_safe=False,
            keywords='vauxootools vxbase',
            classifiers=[
                'Development Status :: 2 - Pre-Alpha',
                'Intended Audience :: Developers',
                'License :: OSI Approved :: BSD License',
                'Natural Language :: English',
                "Programming Language :: Python :: 2",
                'Programming Language :: Python :: 2.6',
                'Programming Language :: Python :: 2.7',
            ],
            test_suite='tests',
        )

    El archivo **requirements.txt** va de la siguiente manera

    .. code-block :: python

        psycopg2==2.5.3
        wsgiref==0.1.2

    Y asi nos evitamos cosas de este tipo en el archivo
    **travis.yml**

    .. code-block :: python

        install:
          - export PYTHONPATH=${HOME}/panama-dv-lib:${PYTHONPATH}

- Probar pylint y flake8 localmente

    .. code-block :: python

        git clone https://github.com/OCA/maintainer-quality-tools

    Ir a la carpeta donde se quiera hacer la prueba de flake8 y pylint

    .. code-block :: python

        flake8 --config=PATH-ABSOLUTO/maintainer-quality-tools/travis/cfg/travis_run_flake8 .
        pylint --rcfile=PATH-ABSOLUTO/maintainer-quality-tools/travis/cfg/travis_run_pylint.cfg .

- Linkear Tareas con notas mentales sobre la tarea, videos sobre la tarea, branch y PR

- Linkear HU con notas mentales sobre la HU, videos entregables y tareas

- Likear PR con HU y Tarea.

- Daily meeting
    • ¿Qué has hecho desde ayer?
    • ¿Qué vas a hacer hoy?
    • ¿Qué problemas tienes?

Descripción de PR
=================

#. Se debe ser específico en el PR, explicar el (por qué y/o para que) se 
   está haciendo dicho desarrollo. Ejemplos:
    #) Se hace establece la condición de days < 7 para poder determinar
       si la prioridad del reclamo es alta o muy alta, y asi, los técnicos
       tendrán una lista ordenada de reclamos que deben atender.
    #) Se sobreescribe el metodo fields_view_get y se asigna un domain
       en el campo facturas, ya que, solo deben mostrarse las facturas
       que pertenezcan al cliente que está haciendo el reclamo, y así
       evitar desplegar una lista larga de facturas que no tienen que
       ver con el cliente.
#. Documentar todo muy bien en la HU y la Tarea linkearlo en el PR.

#. Colocar la tarea en la descripcion del PR la tarea que se trabajo en ese
   branch (De la manera #vx5678), en el caso de que resuelvan varias tareas, 
   se deben colocar el id de la tarea en el commit y especificar bien el 
   commit (Ver punto 1).

#. Cuando se hagan correcciones de pylint, se debe realizar amend en el ultimo commit.
   Evitar hacer commits del tipo "[IMP] travis"

#. Evitar hacer varios features en un mismo branch, tratar en lo posible de hacer
   cosas muy puntuales en cada PR.

#. Hablar todo lo que se tenga que hablar sobre un PR, en el PR. Importante.



Captura de Requerimientos
=========================

#. Revisión de la HU

#. Reunirse con la persona que escribió la HU inicialmente y aclarar
   dudas con él, se debe llenar las conclusiones tecnicas de la HU en
   ese momento
#. Reunirse con el cliente final para afinar los detalles de las conclusiones
   tecnicas

#. Crear la HU muy especifica de la mano con el cliente

#. Videos ubicarlos en las HUs

#. Documentos ubicarlos en las HUs

#. Notas mentales en las conclusiones técnicas

#. Titulos de HU *Como <tipo de usuario> quiero <poder haer algo>*

Manejo de instancias
====================

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

http://en.wikipedia.org/wiki/Proof_of_concept

Export para yoytec
==================

export PYTHONPATH=/home/yanina/Rutas/panama-dv:${PYTHONPATH}
