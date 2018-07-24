-   [Lo básico](#lo-basico)
-   [Variables cualitativas](#variables-cualitativas)
-   [Funciones](#funciones)
-   [Datos](#datos)

<script type="text/x-mathjax-config">
    MathJax.Hub.Config({ TeX: { equationNumbers: {autoNumber: "all"} } });
  </script>

------------------------------------------------------------------------

<!--
La revisión metodológica aquí vertida se basa en [@Wang_2012].
-->
Lo básico
=========

1
-

Ingresa los siguientes datos en R y llámalos P1:

23,45,67,46,57,23,83,59,12,64

Calcular: máximo, mínimo, promedio

2
-

¡No! hubo un error de tipeo, el noveno valor es en realidad 42. Cámbialo
y mira cómo cambia la media.

-   ¿Cuántos valores son mayores a 40?
-   ¿Cuál es la media de los valores mayores a 40?

3
-

Usa los datos del problema 2 y encuentra

-   La suma de P1
-   La media (usando `sum` y `length`)
-   log en base 10 de P1
-   la diferencia entre cada valor de P1 y su media

4
-

Si tenemos dos vectores `a<-11:20` y `b<-c(2,4,6,8)`, predice (sin
ejecutar el código) el resultado de:

-   `a*2`
-   `a[b]`
-   `b[a]`
-   `c(a,b)`
-   `a+b`

Variables cualitativas
======================

1
-

Usa `rep()` para ingresar la secuencia `1 1 1 1 2 2 2 2 3 3 3 3`
repetida 3 veces. Ahora conviértelo a un factor con los niveles "Bajo",
"Medio" y "Alto". ¿Puedes cambiar los niveles a "Nunca", "Rara vez", "A
veces"?

2
-

Convierte el factor del problema 1 en un vector de caracteres y guárdalo
(asígnale un nombre). ¿Puedes convertir el vector de caracteres en un
vector numérico?

3
-

Crea un factor con valores
`2 4 3 3 2 1 1 2 3 4 2 3 3 4 1 3 2 1 4 3 2 4`. Conviértelo en un factor
ordenado con los niveles "Sm", "Med", "Lg", "X-Lg" (con 1 para "Sm").
¿Cuántos valores son iguales o mayores que "Lg"?

4
-

Podemos usar la salida de `table()` con `barplot()` para ver la
frecuencia de niveles en un factor. Ejecuta y explica lo que hace el
sigueinte código:

    barplot(table(sample(x=1:6,size=1000,replace=TRUE)))

![](Prueba1_files/figure-markdown_strict/unnamed-chunk-1-1.png)

Funciones
=========

1
-

Fija una semilla de números aleatorios en `8519` (`set.seed(8519)`).
Crea números aleatorios con `x <- rnorm(100,300,20)`. Crea y ejecuta una
función, donde x será la entrada de la función, que:

-   Reporte los cuantiles 0.15, 0.95
-   Media
-   El primer menos el último valor de x
-   Gráfico de x

Guarda la salida en una lista (pues el reporte contiene datos de
diferente longitud, la lista será de lingitud 3).

2
-

Usando la misma información del ejercicio anterior, crea y ejecuta una
función que estandarice ($z\_i = \\frac{x\_i-\\bar x}{s}$, donde s es la
desviación estándar de *x*.) los datos de entrada. Es decir, que
calcule:

-   Un nuevo vector estandarizado
-   La media original de los datos
-   La desviación estándar original de los datos.
-   Gráfico de boxplot

Guarda la salida en una lista (pues el reporte contiene datos de
diferente longitud, la lista será de lingitud 3).

Datos
=====

1
-

Usando la tabla *Mundo.csv*, calcula un resumen estadístico del producto
nacional bruto per cápita y de las calorías. Repite el cálculo
desagregado por cada región. Comenta los resultados.

2
-

Utiliza los datos `iris` que corresponden a mediciones (en centímetros)
de 4 variables: largo y ancho de los pétalos y sépalos; para 50 flores
de 3 especies distintas de plantas Iris setosa, versicolor, y virginica.

Queremos responder a las siguientes preguntas:

-   ¿Cuántos datos (o casos) tenemos para cada especie? y ¿qué
    porcentaje representan del total de casos? Realice los gráficos
    pertinentes para cada tipo de variable (cualitativa vs.
    cuantitativa).

-   ¿Cuál es la media del ancho del sépalo para cada especie?. Realice
    diagrama de cajas.
