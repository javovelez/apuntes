# SSH

## Instalación

    sudo apt-get install open-ssh
Para iniciar el servicio automáticamente
    sudo systemctl enable ssh 

## Acceso a servidor remoto 
Desde el terminal ejecutamos 
    ssh usuario@host

Podemos agregar -X para ejecutar con modo gráfico. Tenemos que tener instalado un servidor X en nuestra máquina

## Generar certificados para ingresar sin contraseñas

Ejecutamos 

    ssh key-gen

Luego debemos copiar el certificado público en el servidor

Para ver el certificado público ejecutamos:

    cat .ssh/id_rsa.pub

Luego nos logueamos en el servidor donde queremos loguearnos sin contraseña.

y editamos el archivo .ssh/authorized_keys

    nano .ssh/authorized_keys 

y pegamos la clave pública

Sino podemos loguernos usando ``ssh-copy-id  usuario@host`` y automáticamente copiará nuestra clave pública en el servidor destino

## Correr comando en un host remoto
Este el -t implica que entrará al servidor, ejecutará el comando e inmediatamente se termine de ejecutar volvera al host local
    ssh -t root@192.168.1.50 ls
## SSH como proxy (tunel ssh)

    ssh -D puerto user@host 
Esto crea un sock en el sevidor destino que escucha ese puerto.
Luego debemos configurar el proxy de nuestro navegador para salir por el

## Conectarse a servidor que no tengo acceso pero que esté en una red con un server al que si tengo acceso

Desde mi máquina puedo acceder a un servidor ``server1`` pero no a `server2`. Pero desde ``server1`` si se puede acceder a ``server2``
Si ejecuto el siguiente comando me voy a conectar al ``server2`` desde mi máquina pasando por ``server1`` como intermediario.

``ssh -L 2002:host2:22 user@host1``

El puerto 2020 se crea en mi máquina que apunta al puerto 22 de ``server2`` a través del ``server1``

Luego ejecuto

``ssh -p 2020 user@localhost``

Una utilidad de este comando es apuntar por ejemplo el puerto 8080 de mi máquina al puerto 80 del ``server1`` siendo el puerto 80 del ``server1`` no expuesto a internet:

``ssh -L 8080:localhost:80 user@host1``

Luego yo puedo ingresar con el navegador a localhost:8080 y veo el puerto 80 (que no es público) del ``server1``

# Tunel ssh reverso

Desde un servidor ``server2`` al que no tengo acceso me pueden dar acceso conectándose a un servidor cualquiera ``server3`` al que tengo acceso  y habilitan un puerto x en `server3` de manera que cuando haga ssh al pueto x del ``server3`` me conecte al ``server2``.
Desde `server2`:
    ssh -R 2020:localhost:22 user@host3
con esto apunto el puerto 2020 de ``host3`` al puerto 22 de localhost que en este caso es ``host2``

Luego desde mi máquina:

``ssh -p 2020 userhost2@host3``

O desde ``server3``

``ssh userhost2@localhost -p2020``

Un ejemplo sería:
No puedo acceder a mi raspi porque tengo nateada la ip
Desde la raspi ejecutar  

``ssh -fNR 2020:localhost:22 userDeServer1@server1`` 

Luego desde un ``server2`` podes ingresar a la raspberry ejecutando ``ssh raspi@server1 -p2020``

