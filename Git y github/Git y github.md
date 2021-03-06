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

``git comandoX --help`` nos muestra ayuda del comando.

Para crear un commit (de lo previamente llevado a stage area)

``git commit -m "mensaje de commit"``

Para evitar hacer el ``add .`` previo a cada commit podemos ejecutar ``commit -am "mensaje"``. Esto solo sirve para archivos que ya veníamos siguiendo previamente, no para archivos nuevos a seguir.

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

Luego copiamos el resultado y lo pegamos en donde nos indica github que el origin esté configurado con el link de ssh y no con el link de https. En mi caso no pude cambiarlo así que luego de hacer un push al remoto con usuario y contraseña eliminé mi repositorio local y lo cloné nuevamente con el link de ssh.

Es importante para que luego funcione el push con la key y no te pida usuario y contraseña 

Para ver el nombre del repositorio remoto:

``git remote`` y si le agregamos ``-v`` indicará el url del repositorio.

Si genera una advertencia por "historias no relacionadas" puedo ejecutar:

``git pull origin master --allow-unrelated-histories``

Para crear una rama

``git branch nombreDeLaNuevaRama ``

Para moverme a otra rama

``git checkout nombreDelaRama``

Para ver las ramas creadas:

``git branch``

Para ver las ramas remotas:

``git branch -r``

Para cambiarle el nombre a una rama ejecutamos:

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

``git show-branch --all``  

Para entornos visuales podemos ejecutar  ``gitk``

``git log nombreDeArchivo`` nos muestra el historial de cambios del archivo indicado

``git log --stat nombreDeArchivo`` Muestra el historial con más detalles.

También podemos ejecutar:

``git log --all`` lo muestra todo

``git log --all --graph --decorate --oneline`` Lo muestra más gráfico.

``git log  --oneline`` se usa bastante y muestra el resumen de todos los commitsu que hubieron en la rama que estoy parado.

Si quiero ver los cambios usamosentre commits:

``git diff   #### ####``  Donde #### son los fingerprint de los commits (ese identificador alfanumérico que representa el código). El orden en que pongo los fingerprints es importante.

Puedo traer un archivo específico de un commit previo ejecutando:

``git checkout #### nombreDeArchivo`` si llego a realizar un commit pierdo todo lo agregado posteriormente al #### usado.

Para volver a la última versión:

``git checkout master nombreDeArchivo``

## Comentarios sobre rm checkout y reset

El comando ``git rm`` elimina el archivo del stage pero no del historial ni del disco duro. ``git rm --force`` lo elimina del stage y del disco pero no del historial.

Cuando ejecutamos ``git checkout ####`` nos permite mirar lo que había en el pasado pero podemos volver. Si ejecutamos ``git reset ####`` perdemos todos los coambios posteriores a ####.

## Flujos de trabajo en equipo

Una opción es trabajar con ramas separadas para cada funcionalidad a agregar y después hacer un merge estando parado en master master con la rama que contiene la nueva funcionalidad.
``git merge ramaNoMaster``

Otra manera es realizar un fork de un repositirio al cual se le quieran proponer cambios. luego de realizar los cambios se deberá solicitar un pull request (PR).

Para mantener mi fork al día respecto al repositorio original debo crear un upstream. Es decir agregar un remoto del repositorio original al cual le debo hacer pull para estar al día. Luego debo hacer push sobre mi repositorio para actualizarlo.

``git remote add upstream URLoriginal.git`` "upstram" es un nombre arbitrario pero que se acostumbra.
Luego hacemos ``git pull upstream master`` 
Por último ``git push origin master``


## Tags o etiquetas

``git tag v0.1 -m "Descripción del tag" ####`` En este caso le puse v0.1 pero puede ser un nombre arbitrario.

``git tag`` me mustra los tags existentes.

Si quiero ver a qué hash o commit o #### (diferentes formas que uso de llamarle a lo mismo) está conectado, ejecuto:

``git tag show-ref --tags``

Para enviarlo a github:

``git push origin --tags``

``git tag -d v0.1`` Para borrar un tag. En este caso para reflejarlo en el repo remoto ejecutamos ``git push origin :refs/tags/v0.1``

## github pages

Para tener una github page debo crear un repositorio con en el nombre nombreDeUsuario.github.io de usuario. Dentro de el repositorio debo crear el index.html.

Luego en las settings del proyecto debo elegir la branch que se va a linkear con la dirección
nombreDeUsuario.github.io

## Rebase

Rebase es similar a un merge solo que fusiona las ramas de manera que parezca que todos los commits sucedieron en la misma rama. No queda histora de la bifurcación ni del merge.

Para realizarlo estando en una rama x ejecuto la siguiente serie de comandos:

``git rebase master`` Esto me traerá los cambios realizados en master a la rama x cambiando la historia de la rama como si hubieramos creado la rama desde el head más reciente.

Ahora desde la rama master:

``git rebase x`` esto me traerá los cambios de la rama x a master.

Luego puedo eliminar la rama x y todos los cambios quedaron en master.

``git branch -D x``

##  Stash

El stash me guarda el estado actual del WIP (work in progress). El wip son todos los cambios que he realizado desde el último commit estén en satage o no. Una vez guardado el wip con ``git stash`` puedo ver los stash con ``stash list``. Ahora puedo hacer checkout a otras ramas consultar algo. Ahora puedo volver a la rama master y ejecuto ``git stash pop``.

También puedo volcar el wip a otra rama ejecutando primero ``git stash`` y luego ``git stash nuevaRama``. Crea otra rama con el wip y deja la rama anterior en sin wip. Luego en la nueva rama puedo realizar un commit e impactarán los cambios de wip solo en la nueva rama.

Para eliminar un wip guardado con stash ejecuto ``git stash drop``

## Clean

Cuando por error creara o copiara archivos en el respositorio puedo ejecutar ``git clean -f`` para eliminarlos. Se recomienda previamente ejecutar ``git clean --dry-run`` para observar qué archivos van a ser borrados. 

Archivos con el mismo nombre que otros que estén siendo trackeados no serán eliminados. Tampoco se eliminirá cualquier archivo que sea excluido de trackeo por .gitignore.

## Cherry-pick

Puedo traer commits de otras ramas sin traer el head de la otra ram, o sea traigo commits viejos de otras ramas. Para esto ejecuto:

``git cherry-pick ####``  y trerá a la rama actual el commit ####

## Amend

Si hice un commit que debía contener más cambios de los que realmente hubo, puedo realizar los cambios faltantes y ejecutar ``commit --amend``y me fusiona los cambios actuales al commit anterior.
Tambien me da la posibilidad de cambiar el mensaje.

## Reset y reflog

Ejecutando ``git reflog``tenemos un log más completo que nos sirve para casos de emergencia. Nos permite traer el head en el estado previo al siguiente commit con todos los cambios previos a este.

Con ``git reser --SOFT ####`` resetea a #### pero manteniendo el stage previo al próximo commit. Es equivalente a ``git reser HEAD{X}`` (el HEAD{x} lo saco de reflog)

Con ``git reser --HARD ####`` resetea a #### tal cual despues del último commit.

## Grep 

Con ``git grep palabra`` obtengo todas las líneas donde sale 'palabra'. Si agrego el modificador ``-n`` me indica en qué líneas de qué archivos sale. El modificador ``-c`` cuenta la cantidad de veces que aparece esa palabra.

Si tengo caracteres especiales debo poner la palabra a buscar entre comillas dobles.

Si lo uqe quiero buscar está en la historia de los commits  uso ``git log -S palabra`` y nos trae todo lo que esté relacionado con 'palabra'

## Comandos alias dentro de git
Un ejemplo:

``git config --global alias.stats “shortlog -sn --all --no-merges”``

Ahora cuando ejecute ``git stats`` se ejecutará ``shortlog -sn --all --no-merges``

Sobre el comando que uso en el ejemplo:

NOTAS CLASE:

``git shortlog``: Ver cuantos commits han realizado los miembros del equipo
``git shortlog -sn``: Muestra cuantos commits ha realizado cada miembro del equipo
``git shortlog -sn --all``: Todos los commits (también los borrados)
``git shortlog -sn --all --no-merges``: muestra las estadisticas de los comigs del repositorio donde estoy

## Blame

``git blame archivo`` vemos línea por lína quién escribió cada línea. Con el modificador ``-c`` indenta mejor la salida por consola.  ``-L35,53`` nos muestra solo lo que pasó entre esas líneas.


