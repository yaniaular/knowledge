===
VIM
===

Configurar VIM
--------------

$mkdir ~/.vim

$bzr branch lp:~vauxoo/+junk/vim-tuning ~/

$mv ~/vim-tuning/* ~/.vim/

$mv ~/vim-tuning/.vimrc ~/.vim/

$ln -s ~/.vim/.vimrc ~/.vimrc

Agregar instalador vundle
-------------------------

https://github.com/gmarik/vundle
http://kennedysgarage.com/articles/nerdtree
https://github.com/gmarik/vundle/wiki

Instalar vundle:

``$git clone https://github.com/gmarik/vundle.git ~/.vim/bundle/vundle``

Configurar bundles:

set nocompatible               " be iMproved
filetype off                   " required!

set rtp+=~/.vim/bundle/vundle/
call vundle#rc()

" let Vundle manage Vundle
" required! 
Bundle 'gmarik/vundle'

" My Bundles here:
"
" original repos on github
Bundle 'tpope/vim-fugitive'
Bundle 'Lokaltog/vim-easymotion'
Bundle 'rstacruz/sparkup', {'rtp': 'vim/'}
Bundle 'tpope/vim-rails.git'
" vim-scripts repos
Bundle 'L9'
Bundle 'FuzzyFinder'
" non github repos
Bundle 'git://git.wincent.com/command-t.git'
" git repos on your local machine (ie. when working on your own plugin)
Bundle 'file:///Users/gmarik/path/to/plugin'
" ...

filetype plugin indent on     " required!
"
" Brief help
" :BundleList          - list configured bundles
" :BundleInstall(!)    - install(update) bundles
" :BundleSearch(!) foo - search(or refresh cache first) for foo
" :BundleClean(!)      - confirm(or auto-approve) removal of unused bundles
"
" see :h vundle for more details or wiki for FAQ
" NOTE: comments after Bundle command are not allowed..


Expresiones regulares
---------------------

- %s/\s\+$//e quitar espacios en blanco de las últimas líneas
- g/^$/d eliminar líneas en blanco
- g/pdb/d eliminar cualquier línea que contenga la palabra pdb
- %s/$/hola/e agregar hola al final de la línea

Copiar al Clipboard
-------------------

set clipboard=unnamedplus

Una de las cosas más frustrantes con las que me encontré hace mucho tiempo, al conocer Vim, es el dolor de cabeza que implicaba el copiar/cortar y pegar texto desde Vim a cualquier otra aplicación bajo X Window en Linux, y viceversa. Recuerdo haber instalado varios plugins, haber configurado de diversas maneras URxvt para lograr el objetivo, pero ninguna de estas “soluciones” me dejaba satisfecho.

Hace apenas pocas semanas, por fin me decidí a usar exclusivamente Vim para toda edición de texto (luego de varios intentos frustrados en el pasado), y todos los días Vim me sorprende con su poder, cada día descubro algo nuevo… pero había dejado pendiente el detalle arriba mencionado. Hoy por fin me puse a investigar más a fondo, y no paro de reirme de lo sencillo que es solucionar el bendito problema, ¡sin plugins ni trucos raros!


Primer problema: ¡Vim no tiene soporte para xterm_clipboard!
------------------------------------------------------------

Dentro de Vim, ejecuta

:version

En la salida se muestran todas las opciones con las que Vim ha sido compilado, algunas precedidas con “+” (opción habilitada) y otras con “-” (opción no habilitada). La opción que nos interesa en este caso, es xterm_clipboard, ¿tienes “+” o “-” en ella? Si tienes “+”, ¡no tienes nada de qué preocuparte!. ¿Tienes “-”? Entonces lo lógico es que debes compilar Vim con dicha opción.

¿Odias compilar a mano? La mayoría lo hacemos, no te apures, ¿y entonces? ¡Hay una solución sencilla! En vez de tener instalado el paquete vim, ¡instala el paquete gvim! Con Gvim instalado, aún puedes ejecutar Vim en la terminal, ¡y ya incluye el soporte a xterm_clipboard!

Segundo problema: ¡Nunca recuerdo usar el registro +!
-----------------------------------------------------

Vim usa registros para almacenar texto, y el registro + es el usado para compartir texto desde y
para X Window (sea cual sea el entorno de escritorio que estés usando). O sea, para copiar texto
deberíamos usar "+y y para pegar texto el respectivo "+p, ¡lo cual es muy común de olvidar!, ya que
estamos muy acostumbrados a sólo usar y y p para ello.

¿La solución? Si usas una versión reciente de Vim (7.3.74 en adelante), ¡estás de suerte!, basta
con agregar lo siguiente a tu ~/.vimrc para crear un alias entre ambos registros:

set clipboard=unnamedplus

¡Eso es todo! ¡No más dolores de cabeza al copiar, cortar y pegar texto entre Vim y las
aplicaciones de tu entorno de escritorio favorito!

http://gespadas.com/vim-clipboard



Comandos
========

i + <Enter>: Separar el resto de la línea hacia la siguiente línea.
<Shift> + j: Unir la línea de abajo con la de arriba
<Ctrl> + v: Modo visual por caracter
<Shift> + v: Modo visual por línea
t + <Caracter>: Mover el cursor hasta el <Caracter>    
o: Nueva línea hacia abajo
<Shift> + o: Nueva línea hacia arriba
[command]set clipboard=unnamedplus: permitir copiar y pegar desde otros entornos.
u: Deshacer
<Ctrl> + r: Rehacer
d: Entrar en modo borrar, luego de escribir d debes decirle que borrará.
    - d + w: Borrar palabra
    - d + t + <Caracter>: Borrar hasta caracter
    - d + d: Borrar línea

q + <Cualquier Letra>: Empieza a grabar las acciones que hagas en VIM.
q: Terminas de grabar.
@ + <La misma letra>: Repite todas las acciones grabadas en dicha letra.
