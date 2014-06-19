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

- **screen**: 

  crear screen: screen -S NOMBRE
  salir de screen: Ctrl + a -> d
  listar screen: screen -list
  entrar a screen: screen -x NOMBRE
    (o el id en su defecto, el que aparece en -list)

 **chmod**:

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
