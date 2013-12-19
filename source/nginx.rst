=====
NGINX
=====

sudo apt-get install nginx

editar /etc/nginx/sites-enabled/default
y establecer el

server {

listen 80;
root /home/user/.../; #Ruta de la pagina, o el localhost que se quiera
server_name localhost;

}

Se pueden crear tantos servers se quiera, solo hay que estar pendiente de que el host exista en
el archivo de /etc/hosts

Luego basta con hacer http://nombre_de_server

Al hacer cambios en el archivo de nginx se debe reiniciar el servidor 
sudo /etc/init.d/nginx restart

#. Generate your key
ssh-keygen

#. Configure ssh to use the key
vim ~/.ssh/config

#. Copy your key to your server
ssh-copy-id -i /path/to/key.pub SERVERNAME


