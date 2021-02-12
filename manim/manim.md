# Manim

``$ manim scene.py SquareToCircle -pql``

~~~
from manim import *

class SquareToCircle(Scene):
    def construct(self):
        circle = Circle()                    # Crea un círculo
        circle.set_fill(PINK, opacity=0.5)   # configura color y transparencia

        square = Square()                    # Crea un cuadrado
        square.flip(RIGHT)                   # lo espeja horizontalmente
        quare.rotate(-3 * TAU / 8)           # Lo rota un determinado ángulo

        self.play(ShowCreation(square))      # anima la creación de un cuadrado
        self.play(Transform(square, circle)) # interpola el cuadrado hacia un círculo
        self.play(FadeOut(square))           # realiza un "fade out" de la animación
~~~
![Output](images/SquareToCircle.gif)

Esto creará la escena de la clase SquareToCircle. Para renderizar todas las escenas de un .py se le pasa el parámetro ``-a``

El parámetro ``-p `` indica que reproduzca la escena cuando termine. Si en vez de reproducir el archivo al finalizar el render queremos que nos abra el directorio que contiene el video usamos el parámetro ``-f``

El parámetro ``-ql`` indica low quality (480p) y ``-qm`` (720p) ``-qh`` (1080p) ``-qk``(4k).

Manim genera por defecto archivos .mp4. Si deseamos .gif usamos el parámetro ``-i``

## Bloques de construcción de Manim

Existen tres diferentes conceptos que se pueden orquestar juntos para producir animaciones matemáticas. el objeto matemático (``mobject``) la animación y la escena. Cada uno de estos conceptos es una clase de python separada.

### Mobject

Es el bloque base de construcción de animaciones. Representa un objeto que puede ser mostrado en la pantalla como círculos, flechas, rectángulos y otros ejemplos más complicados como ejes cartecianos, funciones de graficación y gráficos de barra.

Existe una clase deribada de mObject llamada VMobject la cual usa ``graficos vectoriales`` (no en el sentido matemático sino en tenología utilizada para graficar como lo hace Ilustrator). 

Usualmente todo el código de un script Manim es colocado dentro del método ``construct()`` de una clase ``Scene``. Para mostrar un mobject en la pantalla llamamos al método ``add()`` de la escena. Esta es la manera de mostrar en pantalla un mobject cuando este no es animado. Para remover un mobject de la pantalla, llamamos al método ``remove()``.

~~~
class CreatingMobjects(Scene):
    def construct(self):
        circle = Circle()
        self.add(circle)
        self.wait(1)
        self.remove(circle)
        self.wait(1)
~~~

![Output](images/CreatingMobjects.gif)


### Ubicando mobjects

Por defecto los mobjects son colocados en el centro de la pantalla que se define como el origen de coordenadas. 

Se puede usar el método ``shift()`` para mover las figuras en la pantalla. Para esto se pueden usar los argumentos UP, DOWN, RIGHT y LEFT.

Ejemplo:

~~~
class Shapes(Scene):
    def construct(self):
        circle = Circle()
        square = Square()
        triangle = Triangle()

        circle.shift(LEFT)
        square.shift(UP)
        triangle.shift(RIGHT)

        self.add(circle, square, triangle)
        self.wait(1)
~~~

![Output](images/Shapes.gif)

Otras maneras de colocar objetos en la pantalla son ``move_to()``, ``next_to()``, y ``align_to()``

~~~
class MobjectPlacement(Scene):
    def construct(self):
        circle = Circle()
        square = Square() # podríamos haber ejecutado square.shift(LEFT) por ejemplo
        triangle = Triangle()

        # Coloca el círculo dos unidades a la izquierda del origen
        circle.move_to(LEFT * 2)
        # Coloca el cuadrado a la izquierda del círculo
        square.next_to(circle, LEFT)
        # Alinea el borde izquierdo del triángulo con el borde izquierdo del círculo
        triangle.align_to(circle, LEFT)

        self.add(circle, square, triangle)
        self.wait(1)
~~~
![Output](images/MobjectPlacement.gif)

### Agregando estilo a los mobjects

Los métodos más básicos son ``set_stroke()`` para configurar el estilo del borde y ``set_fill()`` para configurar el estilo del relleno.

~~~
class MobjectStyling(Scene):
    def construct(self):
        circle = Circle().shift(LEFT)
        square = Square().shift(UP)
        triangle = Triangle().shift(RIGHT)

        circle.set_stroke(color=GREEN, width=20)
        square.set_fill(YELLOW, opacity=1.0)
        triangle.set_fill(PINK, opacity=0.5)

        self.add(circle, square, triangle)
        self.wait(1)
~~~

![Output](images/MobjectStyling.gif)

Cuando utilizamos el método add() tenemos que tener en cuenta el orden en el que le entregamos los mobjects ya que este orden (de izquierda a derecha) representa el orden de aparición en pantalla por ende sabremos que objeto aparecerá encima de otro.

### Animaciones.

Generalmente se agrega animaciones a una escena con el método play().

Las animaciones son procedimientos que realiza una interpolación entre dos mobjects. Por ejemplo. el método FadeIn(square) comienza con una con una versión totalmente transparente del cuadrado y la interpola gradualmente  a una versión totalmente opaca.

~~~
class SomeAnimations(Scene):
    def construct(self):
        square = Square()
        self.add(square)

        # some animations display mobjects, ...
        self.play(FadeIn(square))

        # ... some move or rotate mobjects around...
        self.play(Rotate(square, PI/4))

        # some animations remove mobjects from the screen
        self.play(FadeOut(square))

        self.wait(1)
~~~
![Output](images/SomeAnimations.gif)

Una propiedad de un mobject que puede ser cambiada puede ser animada. De hecho, cualquier método que cambia una propiedad de un mobject puede ser usada como una animación a través del uso de ``ApplyMethod``.

~~~
class ApplyMethodExample(Scene):
    def construct(self):
        square = Square().set_fill(RED, opacity=1.0)
        self.add(square)

        # animate the change of color
        self.play(ApplyMethod(square.set_fill, WHITE))
        self.wait(1)

        # animate the change of position
        self.play(ApplyMethod(square.shift, UP))
        self.wait(1)
~~~

![Output](images/ApplyMethodExample.gif)