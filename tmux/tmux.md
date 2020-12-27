# Es un multiplexor de Terminales

Permite lanzar múltiples terminales (ventanas y paneles) dentro de única pantalla. 

Cada terminal es totalmente gestionable y separada de las demás: podemos cambiar entre ellas y lanzar procesos, crear paneles dentro de cada ventana, etc.

Para instalar tmux ejecutamos desde la consola:

``apt-get install tmux``

Existen tres niveles de anidamiento en tmux, los cuales consisten en:
Sesión/ventana/panel




La sesión es un conjunto de ventanas que a la vez pueden dividirse en paneles (cada panel es una consola independiente). 

Al iniciar una nueva sesión ejecutando el comando ``tmux`` abre una ventana con un único panel por defecto. Las modificaciones de este panel se realizan generalmente presionando las taclas ``ctrl+b`` y luego se presiona una tecla o una combinación de teclas para realizar alguna acción.

Las más comunes son:

- ``ctrl+b``->``"`` para el panel actual en dos paneles horizontales
- ``ctrl+b``->``%`` para dividir el panel actual en dos paneles verticales.
- ``ctrl+b``->``c`` para crear nueva ventana
-``ctrl+b``->``p`` o ``n`` para ir a la ventana anterior (previus) o la ventana siguiente (next) respectivamente.
- ``ctrl+b``->``NUMERO`` para ir a una ventana determinada. notar que en la barra inferior de tmux están numeradas las ventanas y nos indica en cuál estamos parados con un asterísco.
- ``ctrl+b``->``&`` Cerrar la ventana actual (aunque si hacemos un exit se cierra).

Para renombrar una ventana ``ctrl+,``

Para movernos entre paneles podemos presionar ``ctrl+b``->``flechas`` y para modificar el tamaño de un panel presionamos ``ctrl+b``->``ctrl+flecha``

Para maximizar un panel presionamos ``Ctrl+b``->``z`` y lo mismo para volver a su tamaño original. 	

Para salir de una sesión presionamos ``ctrl+b``->``d`` (deattach)

Para recuperar una sesión ejecutamos ``tmux ls`` y nos lista las sesiones activas y para activar la sesión 0 ejecutamos ``tmux attach-session -t 0``

Para crear una sesión con nombre personalizado ejecutamos ``tmux new -s "Nombre de sesión"``



Para realizar más configuraciones o utilizar plugins debemos crear un archivo .tmux.conf en el home de nuestro usuario. En el mismo daremos instrucciones para realizar modificaciones y luego debemos ejecutar ``tmux source-file ~/.tmux.conf`` para que al iniciar tmux este tome en cuenta nuestras personalizaciones.

Yo dentro del archivo .tmux.conf tengo "set -g mouse on" que permite utilizar el mouse dentro para interactuar con los paneles.

En mi caso particular cloné un plugin en :

``git clone https://github.com/tmux-plugins/tmux-resurrect ~/tmux-resurrect``

y luego agregué al archivo de configuración la siguiente línea:

``run-shell /home/javo/tmux-plugins/tmux-resurrect/resurrect.tmux``

Este plugin permite grabar una sesión de manera de poder recuperarla al resetear el servidor. 


Para grabar la sesión abierta (luego de haber instalado el plug in) ejecutamos ``ctrl+b``->``s``

Para restaurar la sesión podemos ejecutar ``ctrl+b``->``r``

Agrego a .tmux.conf la línea set-option -g prefix C-a que permite que ahora la "clave" de comandos sea ``ctrl+a``

para que tenga impacto debo matar el servidor tmux con ``tmux kill-server``, luego ejecuto ``tmux`` nuevamente.

También puede ser util el comando ``tmux kill-session -t 0 (o el nombre que tenga la sesión)

Fuentes:

https://www.youtube.com/watch?v=ufp45qfWjMw&ab_channel=MoldeoInteractive

http://www.sromero.org/wiki/linux/aplicaciones/tmux


