Comandos
========

- **fcrackzip y rarcrack**: Para poder descubrir contraseñas de archivos comprimidos. 

- **TOR**, hoy descargue tor para poder cambiar la ip de mi maquina.
https://www.torproject.org/download/download-easy.html.en
lo descargue de alli, 64 bits y funciona de maravilla

- **find, xargs y sed**: find . -name "index*" | xargs sed -i "s/jquery\-ui\.min\.js/info\.js/g"

- **wget**: wget --wait=20 --limit-rate=20K -r -p -U jmarin http://ajmarin.alwaysdata.net/
  usar -c para renaudar la descarga **wget -c http://dl.google.com/android/adt/adt-bundle-linux-x86-20140702.zip**

Este truco viene muy bien si hemos tratado de descargar algún fichero y más
tarde nos damos cuenta de que por alguna razón, la descarga no se completó.
Muchas veces, si se corta una descarga de un navegador, se puede continuar
donde se perdió si le damos inmediatamente a volver a descargar (porque aún
esté el fichero a descargar en la caché). Sin embargo, otras veces esto no
sirve y en vez de volver a descargar el fichero desde el principio podemos
aprovechar el trozo que hemos descargado y descargar sólo lo que nos resta,
gracias a esa fantástica herramienta que es wget. Sólo tienes que abrir una
terminal, situarte en el directorio donde está el fichero a medio descargar y
ejecutar:

$ wget -c URL_del_archivo Aunque antes de hacer esto puede que tengas que
modificar el nombre del archivo que está a medio descargar, para que coincida
con el del archivo en el servidor. Esto puede ser por ejemplo eliminar la
extensión .part o algún (1)  o [1] que introducen ciertos navegadores…

Wget es un programita sencillo pero potente ya que admite muchas opciones
adicionales que aumentan sus posibilidades. Por poner un ejemplo, si el
servidor exige que para descargarse un archivo haya que estar registrado, se le
pueden indicar muy fácilmente los datos de registro con Wget:

$ wget -c --http-user="nombre" --http-password="clave"
"http://www.servidor.com/fichero" Nota: para que esto funcione la URL tiene que
apuntar directamente al archivo. Si en vez de eso, por ejemplo, apunta a una
página PHP con algún parámetro para seleccionar el archivo, es poco probable
que funcione (aunque a veces sí, todo depende de cómo esté configurado el
servidor).

- **youtube-dl**:

Actualizar: sudo youtube-dl --update
Restart: sudo youtube-dl

youtube-dl -tc http://www.youtube.com/watch?v=IsUsVbTj2AY
youtube-dl -tc -x --audio-format mp3 _hsa8DfV_i8

t : guardar con el titulo original.
c : si se descontinua la descarga, la continua al ejecutar de nuevo.
x : extraer audio, y borrar video.
k : no permitir que borre el video al extraer audio.

- **rsync**: Transferir archivos por scp, cuando hay interrupciones, se reanuda el archivo donde
  quedo.

    rsync -avz openerp8.tar kathy@192.168.1.120:~/

- **du**: Funciona para saber el espacio que ocupa un archivo o directorio en el disco duro.

Modo de empleo: du [OPCIÓN]... [FICHERO]...
       o bien:  du [OPCIÓN]... --files0-from=F

Muestra un resumen del uso de disco para cada FICHERO, recursivamente para
directorios.

Los argumentos obligatorios para las opciones largas son también obligatorios
para las opciones cortas.

Los valores se muestran en unidades del primer TAMAÑO disponible de
--block-size, y las variables de entorno DU_BLOCK_SIZE, BLOCK_SIZE y BLOCKSIZE.
En caso contrario, las unidades son 1024 bytes (o 512 si se ha
establecido POSIXLY_CORRECT).

TAMAÑO puede ser (o puede ser un entero seguido opcionalmente por) uno
de los siguientes:

KB 1000, K 1024, MB 1000*1000, M 1024*1024, y así sucesivamente para G, T, P,
E, Z, Y.

- **elynx**: navegador por consola
- **mutt**:

http://lifehacker.com/5574557/how-to-use-the-fast-and-powerful-mutt-email-client-with-gmail

Hola Yanina & Katherine esta es una prueba de Mutt el cliente de correo en Consola
del que les hablé temprano en el correo de Sabías que: ...

El cuerpo del correo se escribe en vim, bueno eso porque configuras
a vim como el editor de texto.

Incluso pude agregar un adjunto, para poder colocarles una copia de la
hojas de atajos de Vim que una vez imprimimos.

Como dice katherine estamos abusando del poder de la consola,

pero tambien me parece que hay muchos elementos en la web que nos
distraen de lo que tenemos que hacer muchas veces como la ventanita del
hangout parpadeando.

esto es en sentido evangelizador de la palabra, puro, jejeje

Que tengan una feliz noche.

Hablamos mañana al respecto.

Saludos.

**formatear pendriver**:
sudo fdisk -l
sudo mkfs.vfat -F 32 -n Mi_Memoria /dev/sdc1

**comandos en dropbox**:

- **dropbox filestatus**

- **rm:**

  Para colocar un mensaje de estar seguro de borrar un archivo con rm se debe crear un alias
  alias rm='rm -i'

- **wc <file>**: contar palabras de un archivo

- **screen**: 

  crear screen: screen -S NOMBRE
  salir de screen: Ctrl + a -> d
  listar screen: screen -list
  entrar a screen: screen -x NOMBRE
    (o el id en su defecto, el que aparece en -list)

- **chmod**:

Relación Numérica con los Permisos

0 = Ningún permiso (Lectura = 0 + Escritura = 0 + Ejecución = 0)
1 = Permiso de Ejecución (Lectura = 0 + Escritura = 0 + Ejecución = 1)
2 = Permiso de Escritura (Lectura = 0 + Escritura = 2 + Ejecución = 0)
3 = Permiso de Escritura y Ejecución (Lectura = 0, Escritura = 2, Ejecución = 1)
4 = Permiso de Lectura (Lectura = 4 + Escritura = 0 + Ejecución = 0)
5 = Permiso de Lectura y Ejecución (Lectura = 4 + Escritura = 0 + Ejecución = 1)
6 = Permiso de Lectura y Escritura (Lectura = 4 + Escritura = 2 + Ejecución = 0)
7 = Permiso de Lectura, Escritura y Ejecución (Lectura = 4 + Escritura = 2 + Ejecución = 1)

Luego, por cada Identidad, podemos obtener un número comprendido entre 0 y 7, que delimitarán por
Identidad, claramente, sus privilegios en particular sobre un archivo o carpeta.

¿Entonces, que es, por ejemplo, chmod 644?  Son los Permisos que tiene asignados cada Identidad,
sobre un archivo o carpeta, según su Relación Numérica. Siempre siguiendo este orden:

Propietario = 6 (Puede Leer y Escribir)
Grupo = 4 (solo puede Leer)
Otros = 4 (solo puede Leer)

Nota: Evidentemente el comando chmod contiene muchas más opciones y formas de asignar permisos,
puedes consultarlas consultando el manual del comando, para ello abre un terminal y teclea:

**sudo apt-get install fcrackzip*:

Encuentra la contraseña o password de un archivo comprimido (zip)

$ fcrackzip -v -b -p aaaaaa -u your_zip_file.zip

- **py.test --flakes**: probar flakes en los archivos python

**7z**
https://www.ibm.com/developerworks/community/blogs/6e6f6d1b-95c3-46df-8a26-b7efd8ee4b57/entry/how_to_use_7zip_on_linux_command_line144?lang=en
