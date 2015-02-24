GIT
===


Configurar mi usuario en git
----------------------------

git config --global user.email "you@example.com"
git config --global user.name "Your Name"

Guardar tu sesion en cache
--------------------------

git config --global credential.helper cache

o

git config --global credential.helper 'cache --timeout=3600'


Iniciar un repositorio
----------------------

git init

Agregar archivos
----------------

git add <file>

Crear commit
------------

git commit -m '''Add file'''

Log de revisiones
-----------------

git log

Crear branch en repositorio
---------------------------

curl -u 'USER' https://api.github.com/user/repos -d '{"name":"REPO"}'
git remote remove origin
git remote add origin https://github.com/USER/REPO
git push -u origin master
git pull -u origin master

# El -u le dice a git que recuerde el parametro para que la proxima vez, solo se haga git push
#Cambiar USER por usuario y REPO por el nombre del branch

https://developer.github.com/v3/repos

Diferencias en los branches
---------------------------

git difftool
git difftool --tool=gvimdiff2
git difftool --tool-help

Revertir cambios en archivos
----------------------------

git reset <file>

Quitar cambios del commit
-------------------------

git checkout <file>

Log y Diff grafico
------------------

gitk

http://git-scm.com/book/en/Git-Basics-Viewing-the-Commit-History#Using-a-GUI-to-Visualize-History

Migrar de launchpad to git
--------------------------

sudo apt-get install bzr-git
bzr branch lp:~yanina-aular/+junk/curriculum
cd curriculum 
curl -u 'yaniaular' https://api.github.com/user/repos -d '{"name":"curriculum"}'
bzr dpush github:yaniaular/curriculum,branch=master --no-rebase

--no-rebase es para que no sincronize bzr con git

Git y Bazaar
------------

Git y Bazaar te permiten mantener en un mismo repo ambos control de versiones

GitEye-1.6.1-linux.x86_64.zip
-----------------------------

Entorno gráfico de git

Eliminar una rama remota con git
--------------------------------

Supongamos que hemos creado una rama en nuestro repositorio para corregir
algunos errores y evitar que el cambio de código pueda afectar a nuestra rama
estable. Una vez corregidos los errores y pasados todos los test de nuevo la
rama debe mezclarse (merge) con nuestra rama estable y debemos eliminar la rama
de cambios del repositorio.

Eliminarla de nuestro repositorio local es fácil, símplemente debemos ejecutar
(suponemos que nuestra rama se llama bugfixes)

**git branch -d bugfixes** #si la rama ha sido mezclada correctamente con
nuestra rama de upstream

**git branch -D bugfixes** #si queremos borrarla aunque la mezcla no se haya
producido con el upstream

Pero borrarla del servidor… Eso ya es otra cosa. Si después de hacer lo
anterior hacemos un git pull la rama local volverá a crearse, ya que seguía
estando en el servidor. Esto podemos arreglarlo de la siguiente manera

**git push origin :bugfixes** #suponiendo que la rama en el servidor se llama
igual que nuestra ex-rama local

**git push origin --delete bugfixes**

Ambos comandos hacen lo mismo, pero quizás por lo del ahorro de pulsaciones la
primera nos guste más :p
 
**git stash save 'mensaje'** 

Se usa para guardar cambios que no se quieren hacer commit aún. Engavetar.

**git stash list**

Ver lista de cambios guardados

