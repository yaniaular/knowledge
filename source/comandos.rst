Comandos
========
- **find, xargs y sed**: find . -name "index*" | xargs sed -i "s/jquery\-ui\.min\.js/info\.js/g"

- **wget**: wget --wait=20 --limit-rate=20K -r -p -U jmarin http://ajmarin.alwaysdata.net/

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

  
