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

  -a, --all             muestra resultados para todos los ficheros, no sólo
                        para los directorios
      --apparent-size   muestra los tamaños aparentes, en lugar del uso de
                          disco; el tamaño aparente es normalmente más pequeño,
                          puede ser más grande debido a agujeros en ficheros
                          dispersos, fragmentación interna, bloques indirectos,
                          etc.
  -B, --block-size=TAM  escala los tamaños por TAM antes de mostrarlos.
                          P. ej., '-BM' muestra los tamaños en unidades de
                          1.048.576 bytes. Vea el formato de TAMAÑO más abajo.
  -b, --bytes           equivalente a '--apparent-size --block-size=1'
  -c, --total           produce un "total"
  -D, --dereference-args  sigue solamente los enlaces listados en la línea de
                          órdenes
      --files0-from=F   resume el uso de disco de los nombres de ficheros
                          terminados en NUL especificados en el fichero F;
                          Si F es - entonces lee los nombres de la entrada
                          estándar
  -H                    equivalente a --dereference-args (-D)
  -h, --human-readable  muestra los tamaños de forma legible
                        (p. ej., 1K 234M 2G)
      --si              como -h, pero utiliza potencias de 1000 y no de 1024
  -k                    como --block-size=1K
  -l, --count-links     cuenta los tamaños varias veces si hay enlaces fuertes
  -m                    como --block-size=1M
  -L, --dereference     desreferencia todos los enlaces simbólicos
  -P, --no-dereference  no sigue ningún enlace simbólico (predeterminado)
  -0, --null            termina cada línea por un byte 0 en vez de nueva línea
  -S, --separate-dirs   no incluye el tamaño de los subdirectorios
  -s, --summarize       muestra solamente un total para cada argumento
  -x, --one-file-system  se salta los directorios de otros sistemas de ficheros
  -X, --exclude-from=FICH  excluye los ficheros que coinciden con
                                cualquier patrón en FICH.
      --exclude=PATRÓN  excluye los ficheros que coinciden con PATRÓN.
  -d, --max-depth=N     muestra el total para un directorio (o fichero,
                        con --all) solamente si está N o menos niveles por
                        debajo del argumento de la línea de órdenes;
                        --max-depth=0 es lo mismo que --summarize
      --time             muestra la fecha/hora de la última modificación de
                           cualquier fichero dentro del directorio, o de
                           cualquiera de sus subdirectorios
      --time=PALABRA     muestra la fecha/hora como PALABRA en lugar de la
                           fecha de modificación:
                           atime, access, use, ctime o status
      --time-style=ESTILO muestra las fechas/horas usando el estilo ESTILO:
                          full-iso, long-iso, iso, +FORMATO
                          FORMATO se intepreta como 'date'
      --help     muestra esta ayuda y finaliza
      --version  informa de la versión y finaliza

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
