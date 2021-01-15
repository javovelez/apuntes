Comandos linux
sudo -s Inicia root
/opt/odoo11/odoo/odoo-bin Inicia Odoo

estado RAID 1 , si hay F es de Fail:
cat /proc/mdstat

adduser --system --home=/opt/odoo11 --group odoo11

tail -f /var/log/odoo/odoo-server.log PAra ver el log de samba

smb status para ver ess

Para ver usuarios existentes:
cat /etc/passwd | cut -d":" -f1 

chmod -R 0770 fichero
umask 0007

nano /etc/login.defs 

useradd si pongo "-d /ruta al home" crea el home en la tura especificada. si pongo -m crea la carpeta de lo contrario debo crearla previamente. 
		al final va el nombre del usuario
passwd  cambia el pass del usuario logueado
		con sudo o como root y luego del comando especifico un usuario puedo cambiar el pass de otro usuario
		
adduser

manjo de usuarios y grupos
http://www.ite.educacion.es/formacion/materiales/85/cd/linux/m1/administracin_de_usuarios_y_grupos.html
groups user te dice los grupos a los que pertenece user

fg N° de proceso trae proceso a primer plano luego de contro + c
jobs para ver los procesos ejecutandose
https://blog.carreralinux.com.ar/2016/09/procesos-en-segundo-plano-linux/
ip a : muestra ip 