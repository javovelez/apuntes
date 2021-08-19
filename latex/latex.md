# Latex
[Link al curso](https://www.youtube.com/watch?v=uCThSyxMLcY&list=PLLEjLl-_Qg7oCo8lb5p9eAswEyijypvfV&index=2&ab_channel=Mr.Paradox)
## Introducción
Todos los comando de Latex tienen el siguiente fotmato:

\NombreDelComando[modificadores]{argumento principal}

Un documento de latex, está dividido en dos partes, una es el preámbulo donde se definen las configuraciones, la otra es el cuerpo del documento.

En el preámbulo suele haber:

\documentclass[]{}: define qué tipo de documento estamos escribiendo. Dependiendo si se trata de una carta, libro o artículo. 

Un ejemplo de preámbulo:

\documentclass[12pt,letterpaper]{article}
%Define un artículo con papel carta y tamaño de letra 12

Para usar latex español:
\usepackage[utf8]{inputenc} %para introducir acentos desde el teclado

\usepackage[spanish]{babel} %para compilar en español

\usepackage[left=2.5cm,right=2.5cm,top=3cm,bottom=3cm]{geometry}%Establece los márgenes
\setlength{\parskip}{1em} %Espaciamiento entre párrafos
\linespread{1} %espacio entre líneas

Se pueden usar diferentes tipos de unidades:
- pt: puntos tipográficos
- mm: milímetros
- cm: centímetros
- in: pulgadas
- ex: altura de una "x"
- altura de una "M"

\usepackage{amsmath}
\author{javo}
\title{Primer documento}
\date{\today}

\parindent=0cm %modificar sangría
\usepackage{graphicx}%para poder colocar gráficas
\usepackage{epstopdf}%para poder convertir eps a pdf
\usepackage{float}%para poder ubicar con precisión la tabla o figura

\usepackage{subfigure}%para permitir subfiguras 
\usepackage{array}%para dar formato a las tablas
\usepackage{longtable}%para dar formato a las tablas
\usepackage{bm}%para colocar texto en negritas en expresiones matemáticas.
Un ejemplo de cuerpo:

\begin{document} %todo el documento esta dentro de un begin y un end
\maketitle

\section{Introduction}
Esta es la introducción. de \LaTex\
\end{document}

Podemos agregar subsecciones y subsubsecciones con los comandos \subsection{nomre de la subsección} y \subsubsection{nombre de la sub sub sección} respectivamente. Si usamos un asterisco después del comando podemos evitar que lo numere, esto sería \subsection*{Título de la subsección}



## Ecuaciones

Podemos colocar una ecuación usamos 
\begin{equation}
2+2
\end{equation}

para encontrar cómo se expresa un símbolo matemático podemos usar   detexify

## Mejorando el estilo

\usepackage{fancyhdr}% permite modificar el encabezado
\pagestyle{fancy} %le agrega una línea 
\fancyhead{}%borra el estado predifinido
\fancyhead[L]{Título del artículo}%pone lo que pongamos a la izquierda

Para el pié de página
\fancyfoot{}%borra estilo anterior
\fancyfoot[R]{\thepage}%agrega a la derecha el nº de página
\fancyfoot[L]{el piecito}%pone a la izquierda el texto en las llaves

Para configurar la línea de la cabecera y del pié:

\renewcommand{headrulewidth}{0.9pt}%
\renewcommand{footdrulewidth}{0.5pt}
Si no queremos que esté la lina le pones 0pt

Para crear un portada personalizada:

\begin{titlepage}
\begin{center}
\vspace*{2\baselineskip} %mueve el texto hacia abajo (sin asterisco no funciona al principio del documento)
\hrule height 3pt
\vspace*{0.5\baselineskip} 
\end{center}
{\Huge Algo que sea el título}
\vfill{llena de espacios en blanco y escribe esto al final}
\end{titlepage}