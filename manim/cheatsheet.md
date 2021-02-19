# Métodos y parámetros de uso frencuentes

## Mobjects

- ``tex()`` para textos latex.

Para cambiar la fuente


Geométricos:

Circle()

### Métodos de mobjects

``Rotate(angulo,about_point)`` angulo: (PI/4) | (45*DAGREES) . Sin about_point= ponit gira sobre su centro geométrico

``flip(eje)`` eje: UP RIGHT


#### Ubicación de mobjects

Parámetros  ``UP``, ``DOWN``, ``RIGHT`` y ``LEFT``. 

- Posición absoluta

``to_edge(pos, buff=1)`` pos = UP DOWN LEFT RIGHT  ; args: buff: más pegado a la esquina cuando se acerca a 0

``to_corner(pos, buff=1)`` pos = UR UL DR DL; args: buff: más pegado a la esquina cuando se acerca a 0

- Posición relativa

``shift(pos)`` pos:  UP DOWN LEFT RIGHT u operacines con ellos. La nueva posición es referente a la posición actual del objeto.

``move_to(arg)``

arg: (up*2+RIGHT) respecto al centro | (np.array([x,y,0])) | (otroMobject) Se alinean en el centro geométrico de ese mobject | (otroMobject.get_center()+5*RIGHT) 

La distancia es entre los centro geométricos de la figura

``next_to(ref,pos, buff=1)`` 

ref: mbject ; pos:  UP DOWN LEFT RIGHT;  buff: más pegado a la esquina cuando se acerca a 0

La distancia es entre los bordes de los objetos

``align_to()``



#### Estilo de mobjects

``set_fill()``

``set_stroke()``
~~~py
class Fuentes(Scene):
	def construct(self):
		textoNormal = Text("{Texto normal 012.\\#!?} Texto normal")
		textoItalica = Text("\\textit{Texto en itálicas 012.\\#!?} Texto normal")
		textoMaquina = Text("\\texttt{Texto en máquina 012.\\#!?} Texto normal")
		textoNegritas = Text("\\textbf{Texto en negritas 012.\\#!?} Texto normal")
		textoSL = Text("\\textsl{Texto en sl 012.\\#!?} Texto normal")
		textoSC = Text("\\textsc{Texto en sc 012.\\#!?} Texto normal")
		textoNormal.to_edge(UP)
		textoItalica.next_to(textoNormal,DOWN,buff=.5)
		textoMaquina.next_to(textoItalica,DOWN,buff=.5)
		textoNegritas.next_to(textoMaquina,DOWN,buff=.5)
		textoSL.next_to(textoNegritas,DOWN,buff=.5)
		textoSC.next_to(textoSL,DOWN,buff=.5)
		self.add(textoNormal,textoItalica,textoMaquina,textoNegritas,textoSL,textoSC)
		self.wait(3)
~~~        
## Escenas 

### Métodos

play()
wait()
add()
remove()

### Animaciones

Las encontramos todas en ``manim/animations/

De tex:

Write() ``run_time=3`` la animación durará 3 segundos

FadeIn()

