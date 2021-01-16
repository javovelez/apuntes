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

``/proc/mdstat``  Para saber el estado de RAID 1. Si devuelve algo con ``f`` es de Fail

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


