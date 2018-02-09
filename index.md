---
layout: index

title: Vargax	
tagline: vargax01.github.io
boton1: Inicio
boton2: Sobre Mi
enbot1: ./
enbot2: ./sobremi
---

# Oracle, Mysql, Postgres
![debian](./img/oracle.jpg)
<br>
Antes de empezar solo recalcar que no estamos echándole tierra a Mysql y Postgres, ya que esas base de datos están para lo que
estan no significa que no sirvan para nada ;).<br>
## Cláusulas de almacenamiento<br>
Bien nos encontramos con la primera limitación que no nos proporciona ni mysql ni postgres pero si nos lo proporciona oracle. Bien pero ¿Qué es esto? Las cláusulas de almacenamiento
como bien dice su nombre es un cláusula que podemos añadir tanto al crear tablas como tablespaces en oracle que nos permite configurar las extensiones de dicho objeto como por ejemplo:<br>
-Tamaño inicial de la primera extensión<br>
-Tamaño de las próximas extensiones que se creen<br>
-Extensiones reservadas<br>
-Total de extensiones que puede tener<br>
Las cláusulas de almacenamiento quizás nos puedan servir en un entorno complejo pero si nos vamos a dedicar a crear tablas y poco
más quizás no nos convenga.on las cláusulas de almacenamiento podemos controlar los límites permitidos a estos objetos en la base de datos.
## Cuotas
No existe una implementación del uso de quotas en los tablespaces de postgresSQL o Mysql. En mysql existe un límite de recursos de la máquina a los que un usuario puede acceder (cantidad de consultas/updates/conexiones en una hora y conexiones
concurrentes). Las opciones para establecer cuotas sobre tablespaces son:<br>
Crear un tablespace para cada usuario en sistemas de ficheros separados y establecer quota a nivel de sistema operativo (ej. con ZFS o LVM). Requiere permisos de administrador en la maquina. En
postgres puede causar los problemas derivados de falta de espacio ( https://www.postgresql.org/docs/9.1/static/disk-full.html ) como evitar la recuperacion de la base de datos por no poder escribir
en los ficheros de 'Write-Ahead Logging' pg_xlog y pg_clog (esto ultimo se puede solucionar mediante otro sistema de ficheros y symlinks).<br>
Crear un script que monitorice el espacio usado por el usuario y elimine sus permisos de escritura una vez alcanzado el límite establecido.
Ejemplos:<br>
postgres<br>
-https://gist.github.com/javisantana/1277<br>
mysql <br>
-http://datacharmer.blogspot.fr/table-quotas-in-mysql.html<br>
## Tablespaces temporales<br>
Sigamos con otra limitación más, esta vez nos encontramos con una que quizás ya nos deje un mal sabor de boca con mysql y postgres y es que en estos sistemas de base de datos no nos es posible crear
tablespaces temporales. Eso sí tanto mysql como postgres nos permite crear tablas temporales pero claro no es lo mismo. Los tablespaces temporales pueden ser de mucha utilidad para balancear
la carga si tenemos en nuestro sistema de base de datos muchos usuarios. Pero claro mysql y postgres no están preparados para tener una base de datos con miles y miles de usuarios. mantener el
rendimiento en una base de datos con el reuso de grandes ejecuciones de operaciones de reordenación. Tanto mysql como postgres no están preparadas para el volumen de trabajo posible en
una base de datos Oracle.<br>
## Mas de un fichero en una tablespace<br>
Otra limitación, ni postgres ni mysql nos permite añadir más de un fichero a un tablespace. Esto sin duda es una limitación que
quizás no pueda limitar un poco mas si quizás queremos tener nuestro tablespace como en un pequeño cluster de ficheros. Si ocupamos todo el espacio de un dispositivo o si es necesario su
particionado para aumentar el rendimiento.<br>
## Opción Max Size Tablespace<br>
Bueno y esta quizás sea la limitación más grande que nos pone mysql y postgres y la que de verdad nos dirá que gestor de base de
datos usar. Al crear Tablespaces en oracle podemos indicarle el máximo tamaño que podrá tener el tablespace para que nuestros usuarios no puedan sobrepasar el límite y puedan crear objetos por
doquier. Mysql y postgres permite crear los objetos que se quieran y se podría poner en peligro el almacenamiento de nuestro servidor.<br>
## Conclusión<br>
En conclusión podemos decir:<br>
- ¿En tu esquema vas a tener a muchos usuarios y un gran volumen de trabajo? Escoge Oracle que te permite un mejor gestión del
almacenamiento y mayor nivel de transacciones.<br>
- ¿En tu esquema apenas vas a tener usuarios, solo crearás algunos objetos y no necesitas controlar como se almacenen? Escoge mysql o
Postgres, además de ser más sencillo de administrar esta pensado para base de datos pequeñas
