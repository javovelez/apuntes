# Comandos ubuntu

[400 comandos](https://blog.desdelinux.net/mas-de-400-comandos-para-gnulinux-que-deberias-conocer/)

``sudo -s`` Cambia a usuario root

``cat`` Para mostrar el contenido de un archivo de texto

``less`` como cat pero paginado. Para buscar puedo apretar "/palabra". con "n" o sift+n navego entre los resultados.



``tail`` Muestra las últimas líneas de un archivo

``tail -n 100`` muestra las últimas 100 línas

``tail -f`` la f es de follow (seguir) lo que muestra en tiempo real lo que se va agragando al archivo. Su uso típico es para ver logs en tiempo real.

``tail -f /var/log/appx/appx.log`` Para ver el log de una aplicación x

``head`` similar a tail pero muestra el principio del archivo.

``man`` es un comado para conocer el manual de un comando por ejemplo ``man tail`` nos trae la explicaición de como usar el comando tail

``top`` nos muestra un resumen de procesos con sus respecticos usos de recursos

``htop`` lo mismo que top pero mejorado.

``w`` nos muestra usuarios logueados y procesos de ese usuario

``env`` o ``printenv`` nos muestra todas las variables de entorno

``$PATH`` Nos muestra todos los directorios donde el SO busca los programas ejecutables que podemos llamar desde la línea de comandos. 

Si en .bashrc agrego ´´PATH=$PATH:/home/javo/bin´´ agrega al path ese directorio para también buscar comandos dentro de él.

``which comando``  nos muestra el directorio donde se guarda el comando pasado como argumento.

Si presiono la letra ``q`` dentro de ``man`` o ``top`` o ``htop`` Salimos de la aplicación

``find -name nombreDelArchivo`` para buscar un archivo a través del nombre.  También con ''``-type [df]`` busco directorios o archivos respectivamente

``du -h archivo``  para saber el tamaño de un archivo.

``alias nombreComando="ls -a"`` Cuando ejecute ``nombreComando`` se ejecutará ``ls -a``

``touch nombreDeArchivo`` Crea archivo de texto

``pwd`` nos indica por pantalla en qué directorio estamos parados.

``file nombreDeArchivo`` nos dice el tipo de archivo

``cd -`` me perimite volver al último directorio que estaba.

``history`` nos muestra el historial de comandos ejecutados.

Si ejecuto ``!numeroDeComandoHistorial`` se vuelve a ejecutar el comando.

``clear`` para limpiar pantalla o ``ctrl+l``

``ls`` para listar los archivos del directorio donde estoy parado. ``ls -l`` los muestra en forma de lista. ``ls -a`` muestra los archivos ocultos (los que su nombre empiezan con un punto). Puedo sumar los parámetros que les paso al comando. Por ejemplo ``ls -la`` muestra todos los archivos incluidos los ocultos en forma de lista. El modificador ``-t`` ordena los archivos por fecha de modificación. ``-x`` ordena por nombre y extención. ``-R`` para listar contenido de los directorios y subdirectorios.


``rm`` para eliminar un archivo. ``rm -r`` para eliminar una carpeta con todo su contenido. la ``r`` es de recursivo. par aforzar la eliminación ejecuto ``rm -rf``.

``rmdir``para eliminar un directorio.

``cp`` para copiar un archivo de un directorio a otro

``mv`` para mover un archivo o directori. Tambié se usa para cambiar de nombre a archivo o directorio.


## Opciones de disco

``sudo lsblk -fm``   Para ver los discos y sus particiones

``df -h`` Para ver el espacio ocupado/disponible en las particiones. La h es para ser leida por un humano (en MB, KB, etc )

``cat /proc/mdstat``  Para saber el estado de RAID 1. Si devuelve algo con ``f`` es de Fail



## Comandos de usuario y permisos

``adduser --system --home=/opt/appx --group appx`` Agraga un system user. Este tiene la característica de no tener pass para iniciar sesión, no poder ingresar por ssh. Usado en general para que ese usuario inicie un proceso en el arranque.


Para ver usuarios existentes:

``cat /etc/passwd | cut -d":" -f1 ``

``chmod -R 0770 fichero``  

``groups user`` te dice los grupos a los que pertenece user


Fuentes:

http://www.ite.educacion.es/formacion/materiales/85/cd/linux/m1/administracin_de_usuarios_y_grupos.html

# Samba

``smb status`` para ver estado


nano /etc/login.defs 

useradd si pongo "-d /ruta al home" crea el home en la ruta especificada. si pongo -m crea la carpeta de lo contrario debo crearla previamente. 
		al final va el nombre del usuario


passwd  cambia el pass del usuario logueado
		con sudo o como root y luego del comando especifico un usuario puedo cambiar el pass de otro usuario
		
Habiendo instalado: sudo  apt-get install smbfs smbclient
sino esto: sudo apt-get install cifs-utils
sudo mount -t cifs "//192.168.1.9/ServerDoc" /media/NAS -o user=admin
sudo unmount /media/NAS

## Procesos

``Control + c`` 

``fg n`` n es el número de proceso y este comando trae ese proceso a primer plano 
jobs para ver los procesos ejecutandose
https://blog.carreralinux.com.ar/2016/09/procesos-en-segundo-plano-linux/
ip a : muestra ip 

## Servicios con systemd

Systemd es la forma que tiene Ubuntu de administrar servicios.

Con un archivo ``/etc/systemd/system/servicio.service`` se crean un servicio que se ejecutará al inicio. En el archivo se definen los servicios de los cuales depende este servicio, y de qué manera se ejecutará, entre otras cosas.

Ua vez definido este archivo se debe actualizar el "demonio". El demonio es el programa que se ejecuta cuando se inicia el SO, que levanta todos los servicios registrados. Para actualizar el demonio ejecutamos ``sudo systemctl daemon-reload``.

Para habilitar que un servicio se ejecute al inicio debemos ejecutar ``sudo systemctl enable servicio``. Para iniciarlo manualmente ejecutamos ``sudo systemctl start servicio``. Para pararlo ejecutamos ``sudo systemctl stop servicio``. Luego tenemos también las opciones ``restart`` y ``reload``, dependiendo el servicio va admitir uno o el otro o los dos. El comando ``restart`` reinicia el servicio desde 0. el comando ``reload`` solo actualiza el archivo de configuración.

sudo systemctl start stop status odoo
Por último tenemos el comando ``status`` que nos permite ver el estado en el que se encuentra un servicio.


## Comandos avanzados

``grep palabraABuscar archivo`` trae las líneas del archivo que contengan la palabra a buscar. ``-i`` para que no sea case sensitive.  

Si colocamos un ``$`` en el final forzamos a que busque líneas terminadas en la palabra a buscar.
Si colocamos ``^`` al principio, forzamos a que busque líneas que comiencen con con la palabra buscada.


``sed`` Screen Editor, tratamiento de flujos de caracteres. Este comando nos permite reemplazar una expresión por otra.
ejemplos:

``sed ‘s/hanks/selleck/g’ dump1.sql'`` = [comando][subcomando- sustitución][expresión original][nueva expresión][modificador-(/g de global, indica que tiene que hacerse a lo largo de todo el flujo)][Indicar cual es el flujo a utilizar (Archivo de texto)] La ``s`` es de sustitución.

SED no modifica el archivo, lo que hace es crear un nuevo flujo con la modificación

Para eliminar la ultima linea podemos utilizar:
``sed ‘$d’ nuevasPelis.csv`` = [Comando][’$sub-comando’][archivo]

``awk`` Trataminento de texto delimitado, este comando sirve para trabajar con archivos de textos delimitados por comas.

``awk -F ‘;’ ‘{ print $1}’ nuevasPelis.csv``

## Cómo comprimir y descomprimir archivos en Linux usando el terminal

### Ficheros tar

``tar -cvf archivo.tar /dir/a/comprimir/`` 


* -c : indica a tar que cree un archivo.

* -v : indica a tar que muestre lo que va empaquetando.

* -f : indica a tar que el siguiente argumento es el nombre del fichero.tar

En cambio para poder desempaquetar los ficheros .tar, utilizamos el siguiente comando:



``tar -xvf archivo.tar``


* -x : indica a tar que descomprima el fichero.tar.

* -v : indica a tar que muestre lo que va desempaquetando.

* -f : indica a tar que el siguiente argumento es el nombre del fichero a desempaquetar.

Si se quiere ver el contenido de un fichero .tar, se utiliza el siguiente comando:

``tar -tf archivo.tar``

* -t : Lista el contenido del fichero .tar

* -f : indica a tar que el siguiente argumento es el nombre del fichero a ver.

### Ficheros gz

Para comprimir ficheros en formato .gz, se utiliza el siguiente comando:

``gzip -9 fichero``

* -9 : le indica a gz que utilice el mayor factor de compresión posible.

Para descomprimir ficheros .gz, se utilizara el siguiente comando:

``gzip -d fichero.gz``

-d : indica descompresión

### Ficheros tar.gz

``tar -czfv archivo.tar.gz ficheros``

* -c : indica a tar que cree un archivo

* -z : indica que use el compresor gzip

* -f : indica a tar que el siguiente argumento es el nombre del fichero.tar

* -v : indica a tar que muestre lo que va empaquetando

Para descomprimir ficheros con extensión tar.gz, se usa el siguiente comando:

``tar -xzvf archivo.tar.gz``

* -x : le dice a tar que extraiga el contenido del fichero tar.gz

* -z : le indica a tar que esta comprimido con gzip

* -v : va mostrando el contenido del fichero

* -f : le dice a tar que el siguiente argumento es el fichero a descomprimir.

Para poder ver el contenido de un fichero comprimido en tar.gz, se usa el siguiente comando:

``tar -tzf archivo.tar.gz``

Fuente: 
https://www.ubuntizando.com/como-comprimir-y-descomprimir-archivos-en-linux-usando-el-terminal/


## Liberar memoria inutilizada

``sudo sync``  
``sudo sysctl -w vm.drop_caches=3``

## Crontab

Para editar crontab:

``crontab -e`` Para mostrar la tabla ``crontab -l``



Formato básico de una tarea en el Crontab, básicamente consta de dos mitades:

1.  El momento en el cual se ejecutará la tarea del Crontab:
	Cada minuto: En intervalos de entre 0 a 59.  
	Cada hora: En intervalos de entre 0 a 23.  
	Cada día: En intervalos de entre 0 a 31.  
	Cada mes: En intervalos de entre 0 a 12 (0==12 y 12 == Diciembre).  
	Cada día de la semana: En intervalos de entre 0 a 7 (0==7 y 7 == domingo).  


	~~~
	* * * * * comando ha ser ejecutado
	- - - - -
	| | | | |
	| | | | ----- Día de la semana (0 - 7)
	| | | ------- Mes (1 - 12)
	| | --------- Día del mes (1 - 31)
	| ----------- Hora (0 - 23)
	------------- Minuto (0 - 59)
	~~~
	Intervalos de tiempo  
	Ejecutar un script de lunes a viernes a las 	2:30 horas:  

	~~~
	30 2 * * 1-5 /bin/ejecutar/script.sh
	~~~
	Ejecutar un script de lunes a viernes cada 	10 minutos desde las 2:00 horas durante una 	hora:
	~~~
	0,10,20,30,40,50 2 * * 1-5 /bin/ejecutar/	script.sh
	~~~

	Esto quizá puede ser largo. La sintaxis de 	crontab permite lo siguiente. Imaginemos que 	queremos ejecutarlo cada 5 minutos:
	~~~
	*/5 2 * * 1-5 /bin/ejecutar/script.sh
	~~~

2.  El comando BASH
	~~~
	* * * * * /bin/ejecutar/script.sh
	~~~

[Fuente](https://geekytheory.com/programar-tareas-en-linux-usando-crontab)

**Palabras reservadas**

@reboot: se ejecuta una única vez al inicio.
@yearly/@annually: ejecutar cada año.
@monthly: ejecutar una vez al mes.
@weekly: una vez a la semana.
@daily/@midnight: una vez al día.
@hourly: cada hora.

Por ejemplo, para ejecutar el script cada hora:

``@hourly /bin/ejecutar/script.sh
``

## Operaciones con multiples comandos

``|``  envía la salida del comando de la izquierda hacia la entrada del de la derecha.

Si coloco ``&`` al final de un comando este se ejecua en segundo plano

``nohup nombreDeScript &`` se ejecuta en segundo plano y genera el archivo nohup.out que contiene la salida del log de ese comando.

Si separo comandos con ; se ejecutan de izquierda a derecha

Si separo comandos con & se ejecutan de manera independiente

o con && si falla el primero no se ejecuta el segundo

## Scripting

Primero se define que programa ejecutará el script. Lo más común es bash o python. Para definirlo la primer línea del archivo debe ser #!/bin/bash o la ruta al intérprete de python que querramos usar.

Las variables simplemente se asignan con signo ``=``. Si quiero que sea de solo lectura debo comenzas con ``readonly`.

mivariable=50

Si quiero que la variable se asigne con el resultado de la ejecución de un comando:

``mivariable="$(comando1 "$(comando2)")"``

Para acceder a la variable creada:

``echo $mivariable``

Para crear funciones:

~~~sh
function nombreDeLaFucncion {
	local readonly mensaje=$1 #crea una variable local de solo lectura llamada mensaje donde le asigna el primer argumento que se le da a la cunción
}
~~~

Función if

~~~sh
if [[ ]]; then
	echo "algo"
fi

~~~
[fuente](https://www.atareao.es/tutorial/scripts-en-bash/condicionales-en-bash/)


