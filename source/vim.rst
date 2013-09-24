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

-------------------------
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



