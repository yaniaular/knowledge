=====
NGINX
=====

Configuracion servidor OpenERP
------------------------------

vim /etc/nginx/sites-enabled/openerp-yani
usar puerto ubicado arriba


Nginx
-----

sudo apt-get install nginx nginx-common nginx-full

editar /etc/nginx/sites-enabled/default
y establecer el

.. code-block ::

    server {

        listen 3333;
        server_name openerp_log;


        location / {
            root /home/yanina/branches/logs/html;
            index index.html index.htm;
        }
    }

    server {
        listen 80 default_server;
        listen [::]:80 default_server ipv6only=on;

        root /usr/share/nginx/html;
        index index.html index.htm;

        # Make site accessible from http://localhost/
        server_name localhost;

        location / {
            # First attempt to serve request as file, then
            # as directory, then fall back to displaying a 404.
            try_files $uri $uri/ =404;
            # Uncomment to enable naxsi on this location
            # include /etc/nginx/naxsi.rules
        }

        # Only for nginx-naxsi used with nginx-naxsi-ui : process denied requests
        #location /RequestDenied {
        #	proxy_pass http://127.0.0.1:8080;    
        #}

        #error_page 404 /404.html;

        # redirect server error pages to the static page /50x.html
        #
        #error_page 500 502 503 504 /50x.html;
        #location = /50x.html {
        #	root /usr/share/nginx/html;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #	fastcgi_split_path_info ^(.+\.php)(/.+)$;
        #	# NOTE: You should have "cgi.fix_pathinfo = 0;" in php.ini
        #
        #	# With php5-cgi alone:
        #	fastcgi_pass 127.0.0.1:9000;
        #	# With php5-fpm:
        #	fastcgi_pass unix:/var/run/php5-fpm.sock;
        #	fastcgi_index index.php;
        #	include fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #	deny all;
        #}
    }


Se pueden crear tantos servers se quiera, solo hay que estar pendiente de que el host exista en
el archivo de /etc/hosts

.. code-block ::

    127.0.0.1	openerp_log
    127.0.0.1	localhost
    127.0.1.1	yani-kde

    # The following lines are desirable for IPv6 capable hosts
    ::1     ip6-localhost ip6-loopback
    fe00::0 ip6-localnet
    ff00::0 ip6-mcastprefix
    ff02::1 ip6-allnodes
    ff02::2 ip6-allrouters

Luego basta con hacer http://nombre_de_server

Al hacer cambios en el archivo de nginx se debe reiniciar el servidor 
sudo /etc/init.d/nginx restart

#. Generate your key
ssh-keygen

#. Configure ssh to use the key
vim ~/.ssh/config

#. Copy your key to your server
ssh-copy-id -i /path/to/key.pub SERVERNAME


