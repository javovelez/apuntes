# Comandos ubuntu

``sudo -s`` Cambia a usuario root

``cat`` Para mostrar el contenido de un archivo de texto

``tail`` Muestra las últimas líneas de un archivo

``tail -n 100`` muestra las últimas 100 línas

``tail -f`` la f es de follow (seguir) lo que muestra en tiempo real lo que se va agragando al archivo. Su uso típico es para ver logs en tiempo real.

``tail -f /var/log/appx/appx.log`` Para ver el log de una aplicación x

``man`` es un comado para conocer el manual de un comando por ejemplo ``man tail`` nos trae la explicaición de como usar el comando tail

``top`` nos muestra un resumen de procesos con sus respecticos usos de recursos

``htop`` lo mismo que top pero mejorado.

Si presiono la letra ``q`` dentro de ``man`` o ``top`` o ``htop`` Salimos de la aplicación

``find -name nombreDelArchivo`` para buscar un archivo a través del nombre.

``du -h archivo``  para saber el tamaño de un archivo.

## Opciones de disco

``sudo lsblk -fm``   Para ver los discos y sus particiones

``df -h`` Para ver el espacio ocupado/disponible en las particiones. La h es para ser leida por un humano (en MB, KB, etc )

``cat /proc/mdstat``  Para saber el estado de RAID 1. Si devuelve algo con ``f`` es de Fail

``clear`` para limpiar pantalla o ``ctrl+l``
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