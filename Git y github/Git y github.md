# GIT y Github

Instalación

``git apt-get update``

``git apt-get upgrade``

``sudo apt-get install git``

Para verificar:

``git --version``

Para llevar todos los archivos del directorio que estoy parado a "stage area" (a la lista de archivos a los cuales les estoy siguiendo los cambios) ejecuto:

``git add .``

``git rm archivo`` deja de trackear el archivo.

``git rm --cached`` elimina lo que hay en ram cargado respecto al seguimiento de este archivo. Si yo previamente le había dado add al archivo y lo empecé a modificar, en ram está el seguimiento de ese archivo. Si solo le doy ``rm`` sin ``--cahed`` obtendré un mensaje de error.

Para crear una rama

`` ``

Para moverme a otra rama

``git checkout nombreDelaRama``

Para crear un commit (de lo previamente llevado a stage area)

``git commit -m "mensaje de commit"``

[Documentación sobre buenos mensajes de commit](https://marcoelizalde.com/2020/10/17/como-escribir-excelentes-commits/)

Si no agrego un mensaje voy a entrar a vim (editor de texto):

``esc+i`` para insertar texto

``esc+shift+zz`` para enviar el mensaje

Para llevarlo al repositorio desde el que hice el commit

``git push``

Si el repositorio lo comencé en local debo indicarle el repositorio remoto. Generalmente se lo llama "origin" pero el nombre lo define uno en el comando.

``git remote add nombreDelrepo https://github.com/nombredelrepo.git``

Esto supone que ya estamos hemos cargado nuestras credenciales previamente. Esto se ejecuta con

`` git config --global user.name "javovelez"``

Para ver qué usuario está cargado podemos ejecutar

``git config user.name``

``git config --list`` nos muestra las configuraciones que tenemos seteadas.

Cuando hagamos push nos pedirá la contraseña de github. Para evitar esto podemos crear una clave ssh con el comando 

``ssh-keygen``

Luego dentro de  /home/user/.ssh encontraremos la clave pública llamada d_rsa.pub y para visualizarla ejecutamos

``cat ~/.ssh/id_rsa.pub``

Luego copiamos el resultado y lo pegamos en donde nos indica github.

Para cambiarle el nombre a una rama ejecutamos

``git branch -m <nuevo-nombre>``

Y para cambiarle el nombre al remoto (origin es el nombre que le hemos puesto al repositorio remoto)

``git push --set-upstream origin 13.0``

Para ver en que estado estamos actualmente (que archivos estan en stage area y tienen cambios)

``git status``
Para eliminar una extención debemos agregar al archivo .gitignore *.pdf 

para traer cambios que están en el repositorio ejecutamos

``git pull``

Para ir a un determinado commit previo:

``git reset a330d9c4af1b007fe1436f979ff0b9f66613136e --hard`` si uso ``--soft`` el directorio de trabajo va al commit indicado pero mantiene los cambios ya trackeados en el stage para commitearlos.

si quiero actualizar un repositorio local en base al remoto tengo al menos 3 opciones.

1. git pull
2. Si tengo problemas con cambios locales:  
``git fetch origin  ``  
``git reset --hard origin/master``  
3. Si quiero preservar los cambios locales y al mismo tiempo los remotos (si no existen conflictos)  
``git fetch origin  ``  
``git merge``

``git show`` Nos muestra todos los cambios históricos hechos.

``git log nombreDeArchivo`` nos muestra el historial de cambios del archivo indicado

``git log --stat nombreDeArchivo`` Muestra el historial con más detalles.

Si quiero ver los cambios usamosentre commits:

``git diff   #### ####``  Donde #### son los fingerprint de los commits (ese identificador alfanumérico que representa el código). El orden en que pongo los fingerprints es importante.

Puedo traer un archivo específico de un commit previo ejecutando:

``git checkout #### nombreDeArchivo`` si llego a realizar un commit pierdo todo lo agregado posteriormente al #### usado.

Para volver a la última versión:

``git checkout master nombreDeArchivo``

## Comentarios sobre rm checkout y reset

El comando ``git rm`` elimina el archivo del stage pero no del historial ni del disco duro. ``git rm --force`` lo elimina del stage y del disco pero no del historial.

Cuando ejecutamos ``git checkout ####`` nos permite mirar lo que había en el pasado pero podemos volver. Si ejecutamos ``git reset ####`` perdemos todos los coambios posteriores a ####.
