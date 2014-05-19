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

Migrar de launchpad to git
--------------------------

sudo apt-get install bzr-git
bzr branch lp:~yanina-aular/+junk/curriculum
cd curriculum 
curl -u 'yaniaular' https://api.github.com/user/repos -d '{"name":"curriculum"}'
bzr dpush github:yaniaular/curriculum,branch=master
