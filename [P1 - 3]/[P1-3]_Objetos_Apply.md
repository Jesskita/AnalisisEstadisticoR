-   [Objetos](#objetos)
    -   [Factor](#factor)
    -   [Listas](#listas)
        -   [Missing Values](#missing-values)
    -   [Matrices y Dataframes](#matrices-y-dataframes)
        -   [Matrices](#matrices)
        -   [Data.frame](#data.frame)
        -   [Operaciones con matrices](#operaciones-con-matrices)
    -   [Arrays](#arrays)
-   [Bucles](#bucles)
    -   [FOR](#for)
    -   [While](#while)
    -   [If - Else](#if---else)
        -   [Oasis: Algo de cálculo](#oasis-algo-de-calculo)
        -   [VAN](#van)
-   [La Familia Apply](#la-familia-apply)
    -   [lapply](#lapply)
    -   [sapply](#sapply)
    -   [apply](#apply)
    -   [tapply](#tapply)
-   [Aggregate y By](#aggregate-y-by)
    -   [By](#by)
    -   [Aggregate](#aggregate)

<!--
La revisión metodológica aquí vertida se basa en [@Wang_2012].
-->
Objetos
=======

Ya hemos visto la definición de un objeto, además de nuestro primer
objeto: un vector. Ahora veremos cuatro de los objetos más usados en los
primetos pasos en R: factores, listas, matrices y data.frames.

Factor
------

-   Un tipo de vector para datos categóricos

<!-- -->

    z <- factor(LETTERS[1:3], ordered = TRUE)
    x <- factor(c("a", "b", "b", "a"))
    x

    ## [1] a b b a
    ## Levels: a b

Los factores son útiles cuando se conocen los valores posibles de una
variable puede tomar, incluso si no se ve todos los valores en un
determinado conjunto de datos. El uso de un factor en lugar de un vector
de caracteres hace evidente cuando algunos grupos no contienen
observaciones:

    sex_char <- c("m", "m", "m")
    sex_factor <- factor(sex_char, levels = c("m", "f"))

    table(sex_char)

    ## sex_char
    ## m 
    ## 3

    table(sex_factor)

    ## sex_factor
    ## m f 
    ## 3 0

Listas
------

Es *vector generalizado*. Cada lista está formada por componentes (que
pueden ser otras listas), y cada componente puede ser de un tipo
distinto. Son unos "contenedores generales”.

    n = c(2, 3, 5) 
    s = c("aa", "bb", "cc", "dd", "ee") 
    b = c(TRUE, FALSE, TRUE, FALSE, FALSE) 
    x = list(n, s, b, 3)

A las listas a veces se les llama *vectores recursivos*, porque pueden
contener otras listas.

    x <- list(list(list(list())))
    str(x)

    ## List of 1
    ##  $ :List of 1
    ##   ..$ :List of 1
    ##   .. ..$ : list()

    is.recursive(x)

    ## [1] TRUE

`c()` combinará varias listas en una sola. Si se tiene una combinación
de vectores y listas, `c()` coerciona a los vectores como listas antes
de combinarlos. Compara los resultados de `list()` y `c()`:

    x <- list(list(1, 2), c(3, 4))
    y <- c(list(1, 2), c(3, 4))
    str(x)

    ## List of 2
    ##  $ :List of 2
    ##   ..$ : num 1
    ##   ..$ : num 2
    ##  $ : num [1:2] 3 4

    str(y)

    ## List of 4
    ##  $ : num 1
    ##  $ : num 2
    ##  $ : num 3
    ##  $ : num 4

### Missing Values

Los valores perdidos se denotan por `NA` o `NaN` para operaciones
matemáticas no definidas.

-   `is.na()` se usa para comprobar si un objeto es `NA`
-   `is.nan()` se usa para comprobar si un objeto es `NaN`
-   `NA` también pertenecen a una clase como numeric `NA`, existe
    character `NA`, etc.
-   Un `NaN` también es un `NA` pero al revés no es cierto

<!-- -->

    x <- c(1, 2, NA, 10, 3)
    is.na(x)

    ## [1] FALSE FALSE  TRUE FALSE FALSE

    is.nan(x)

    ## [1] FALSE FALSE FALSE FALSE FALSE

    x <- c(1, 2, NaN, NA, 4)
    is.na(x)

    ## [1] FALSE FALSE  TRUE  TRUE FALSE

    is.nan(x)

    ## [1] FALSE FALSE  TRUE FALSE FALSE

Matrices y Dataframes
---------------------

En esta clase, vamos a cubrir las *matrices* y *data frames*. Ambos
representan los tipos de datos "rectangulares", lo que significa que se
utilizan para almacenar datos tabulares, con filas y columnas.

La principal diferencia, como se verán, es que las matrices sólo pueden
contener una sola clase de datos, mientras que las data frames pueden
consistir en muchas clases diferentes de datos.

### Matrices

Es un tipo de objeto que contiene elementos del mismo tipo. A diferencia
de los vectores, este tiene el atributo `dim`, veamos:

-   Vamos a crear un vector que contiene los números del 1 al 20 con el
    operador `:` . Almacenar el resultado en una variable llamada
    my\_vector.

-   Escribe `dim(my_vector)`. Resulta que no tiene este atributo.

-   Sin embargo, la función `dim` se usa para pedir o asignar este
    atributo. Escribe `dim(my_vector) <- c(4, 5)`

-   Ahora, mira cuál es la dimensión de `my_vector`
    -   Al igual que en la clase de matemáticas, cuando se trata de un
        objeto de 2 dimensiones (piense mesa rectangular), el primer
        número es el número de filas y el segundo es el número de
        columnas. Por lo tanto, `my_vector` ahora tiene 4 filas y 5
        columnas.
    -   ¡Pero espera! Eso no suena como un vector más. Bueno, no lo es.
        Ahora es una matriz. Ver el contenido de `my_vector` ahora para
        ver lo que parece. Imprime el contenido de `my_vector`
-   Ves, ahora tenemos una matriz, confirmemos esto usando a función
    `class()`, así: `class(my_vector)`.

-   Efectivamente, `my_vector` es ahora una matriz. Deberíamos
    almacenarlo en una nueva variable que nos ayuda a recordar lo que
    es. Almacena el valor de `my_vector` en una nueva variable llamada
    `my_matrix`.

El código del ejemplo anterior sería:

    my_vector <- 1:20
    dim(my_vector)

    ## NULL

    dim(my_vector) <- c(4, 5)
    my_vector

    ##      [,1] [,2] [,3] [,4] [,5]
    ## [1,]    1    5    9   13   17
    ## [2,]    2    6   10   14   18
    ## [3,]    3    7   11   15   19
    ## [4,]    4    8   12   16   20

    class(my_vector)

    ## [1] "matrix"

    my_matrix <- my_vector

El ejemplo que hemos utilizado hasta ahora estaba destinado a ilustrar
el punto de que una matriz es simplemente un vector con un atributo de
dimensión. Un método más directo de la creación de la misma matriz
utiliza la función de `matrix()`.

-   Mira la ayuda de `matrix()`. Encuentra la manera de crear una matriz
    que contiene los mismos números (1-20) y dimensiones (4 filas, 5
    columnas) usando a la función de `matrix()`. Almacenar el resultado
    en una variable llamada `my_matrix2`.

-   Ahora veamos si `my_matrix` y `my_matrix2` son idénticas. Usamos la
    función `identical()`

El código sería:

    my_matrix2 <- matrix(1:20, nrow = 4, ncol = 5)
    identical(my_matrix , my_matrix2)

    ## [1] TRUE

Ahora, imagina que los números en la mesa representan algunas medidas de
un experimento clínico, donde cada fila representa un paciente y cada
columna representa una variable para la que se tomaron mediciones.

Podemos querer etiquetar las filas, para que sepamos qué números
pertenecen a cada paciente en el experimento. Una forma de hacer esto es
agregar una columna a la matriz, que contiene los nombres de las cuatro
personas.

-   Vamos a empezar por la creación de un vector de caracteres que
    contiene los nombres de nuestros pacientes - Josefa, Gina, Jose, y
    Julio Recuerda que las comillas dobles dicen R que algo es una
    cadena de caracteres. Almacena el resultado en la variable llamada
    `patients`.

-   Ahora vamos a utilizar la función `cbind()` para *combinar
    columnas*. No te preocupes por guardar el resultado en una nueva
    variable. Sólo tienes que usar `cbind()` con dos argumentos - el
    vector de los pacientes y `my_matrix`.

    -   Algo está raro en el resultado! Parece que la combinación del
        vector *character* con nuestra matriz de números hizo que todo
        esté entre comillas dobles. Esto significa que nos quedamos con
        una matriz de caracteres, lo que no es bueno.
    -   Si recuerdas, dijimos que las matrices sólo pueden contener un
        tipo de datos. Por lo tanto, cuando tratamos de combinar un
        vector de caracteres con una matriz numérica, `R` se vio
        obligado a *coeccionar* los números en caracteres, de ahí las
        comillas dobles.

### Data.frame

-   Por lo tanto, estamos todavía con la cuestión de cómo incluir los
    nombres de nuestros pacientes en la tabla sin dañar de nuestros
    datos numéricos. Prueba lo siguiente -
    `my_data <- data.frame(patients, my_matrix)`

Parece que la función `data.frame()` nos permitió guardar nuestro vector
de caracteres de los nombres justo al lado de nuestra matriz de números.
Eso es exactamente lo que esperábamos!

-   Chequea el tipo de objeto que hemos creado con `class(my_data)`

También es posible asignar nombres a las filas y columnas de un data
frame, lo cual es otra posible forma de determinar qué fila de valores
en nuestra tabla pertenece a cada paciente.

-   Ya que tenemos seis columnas (incluyendo nombres de los pacientes),
    tendremos que crear primero un vector que contiene un elemento para
    cada columna. Crea un vector de caracteres llamado `cnames` que
    contiene los valores siguientes (en orden) - *patient*, *age*,
    *weight*, *bp*, *rating*, *test*.

-   Ahora, utilice los `colnames()` para establecer el atributo
    `colnames` para nuestro data frame. Es similar a la función `dim()`
    que usamos antes. Imprime `my_data`.

El código sería:

    patients <- c("Josefa", "Gina", "Jose", "Julio")
    cbind(patients,my_matrix)

    ##      patients                       
    ## [1,] "Josefa" "1" "5" "9"  "13" "17"
    ## [2,] "Gina"   "2" "6" "10" "14" "18"
    ## [3,] "Jose"   "3" "7" "11" "15" "19"
    ## [4,] "Julio"  "4" "8" "12" "16" "20"

    my_data <- data.frame(patients, my_matrix)
    cnames <- c("patient", "age", "weight", "bp", "rating", "test")
    colnames(my_data) <- cnames

Desde luego, podemos crear un data frame directamente, por ejemplo

    my.data.frame <- data.frame(
        ID = c("Carla", "Pedro",    "Laura"), 
        Edad = c(10, 25, 33), 
        Ingreso = c(NA, 34, 15),
        Sexo = c(TRUE, FALSE, TRUE),
      Etnia = c("Mestizo","Afroecuatoriana","Indígena")
    )

### Operaciones con matrices

-   R posee facilidades para manipular y hacer operaciones con matrices.
    Las funciones `rbind()` y `cbind()` unen matrices con respecto a sus
    filas o columnas respectivamente:

<!-- -->

    m1 <- matrix(1, nr = 2, nc = 2) 
    m2 <- matrix(2, nr = 2, nc = 2) 

    rbind(m1, m2) 
    cbind(m1,m2)

-   El operador para el producto de dos matrices es `%*%`. Por ejemplo,
    considerando las dos matrices m1 y m2:

<!-- -->

    ma=rbind(m1, m2) %*% cbind(m1, m2) 

-   La transpuesta de una matriz se realiza con la función `t`; esta
    función también funciona con data frames.

<!-- -->

    t(ma)

    ##      [,1] [,2] [,3] [,4]
    ## [1,]    2    2    4    4
    ## [2,]    2    2    4    4
    ## [3,]    4    4    8    8
    ## [4,]    4    4    8    8

-   Para el cálculo de la inversa se usa `solve`

<!-- -->

    solve(ma)

Otro uso de la función `solve()` es la solución de sistemas de
ecuaciones, por ejemplo:

$$
\\begin{eqnarray}
3x + 2y + z & = & 1 \\\\
5x + 3y + 4z & = & 2 \\\\
x + y -z & = & 1
\\end{eqnarray}
$$

Cuya solución en `R` sería:

    A <- matrix(c(3,5,1,2,3,1,1,4,-1),ncol=3)
    b <- c(1,2,1)
    solve(A,b)

    ## [1] -4  6  1

Arrays
------

-   Generalización multidimensional de vector. Elementos del mismo tipo.

<!-- -->

    x <- array(1:20, dim=c(4,5))

Bucles
======

-   Una ventaja de R comparado con otros programas estadísticos con
    “menus y botones” es la posibilidad de programar de una manera muy
    sencilla una serie de análisis que se puedan ejecutar de manera
    sucesiva.

-   Por ejemplo, definamos un vector con 50.000 componentes y calculemos
    el cuadrado de cada componente primero usando las propiedades de R
    de realizar cálculos componente a componente y luego usando un
    ciclo.

<!-- -->

    x <- 1:50000
    y <- x^2

-   Con bucles:

<!-- -->

    z=0
    for (i in 1:50000) z[i] <- x[i]^2 

FOR
---

La sintaxis de la intrucción es:

> for (i in valores ) { instrucciones }

Ejemplo:

    for (i in 1:5)
    {
      print (i)
    }

    ## [1] 1
    ## [1] 2
    ## [1] 3
    ## [1] 4
    ## [1] 5

Ejemplos

-   ivalores: numérico

<!-- -->

    for (i in c(3,2,9,6)){
        print(i^2)
    }

    ## [1] 9
    ## [1] 4
    ## [1] 81
    ## [1] 36

-   i caractér e ivalores vector:

<!-- -->

    medios.transporte <- c("carro", "camion","metro", "moto")
        for (vehiculo in medios.transporte)
        {print (vehiculo)}

    ## [1] "carro"
    ## [1] "camion"
    ## [1] "metro"
    ## [1] "moto"

While
-----

-   Ejemplo 1:

<!-- -->

    i <- 1
    while (i<=10) i <- i+4
    i

    ## [1] 13

-   Ejemplo 2:

<!-- -->

    i <- 1
    while (TRUE){ # Loop similar al anterior
      i <- i+4
      if(i>10) break
    }
    i

    ## [1] 13

If - Else
---------

-   Empecemos con un ejemplo simple:

<!-- -->

    r=5
    if(r==4){
      x <-1
    }else{
      x <- 3
      y <- 4
    }

**Combinando lo aprendido**

-   Realicemos una función que cuenta el número de elementos impares en
    un vector:

<!-- -->

    oddcount <- function(x)
    { 
      k <- 0 # se asigna 0 a k
      for( n in x)
        {
        if(n%%2 ==1) k <- k+1
        }
      return(k)
    }

-   Probemos la función

<!-- -->

    x <- seq(1:3)
    oddcount(x)

    ## [1] 2

### Oasis: Algo de cálculo

-   Derivada de *f*(*x*)=*e*<sup>2*x*</sup>

<!-- -->

    D(expression(exp(x^2)),"x") 

    ## exp(x^2) * (2 * x)

-   Integral de ∫<sub>0</sub><sup>1</sup>*x*<sup>2</sup>

<!-- -->

    integrate(function(x) x^2,0,1)

    ## 0.3333333 with absolute error < 3.7e-15

-   Chequear el paquete `ryacas` para mas cálculo simbólico

### VAN

Su expresión es $VAN = \\sum\_{i=0}^{n}\\frac{Vi}{(1+K)^i} - I\_{0}$

    VAN <- function(I0,n,K,V)
      {
      for (i in 1:n)
        {
        y[i] <- V/(1+K)^i
        }
      sum(y) - I0
      }

La Familia Apply
================

-   Existen algunas funciones que nos facilitan la vida en lugar de usar
    *loops*:

    -   lapply: Itera sobre una lista y evalúa una función en cada
        elemento.
    -   sapply: Lo mismo que laaply pero trata de simplifica el
        resultado.
    -   apply: Aplica una función sobre las dimensiones de un array.
    -   tapply: Aplica una función sobre subconjuntos de un vector
    -   mapply: Versión multivariada de lapply

lapply
------

-   *lapply* siempre retorna una lista, independientemente de la clase
    del objeto de entrada

<!-- -->

    x <- list(a = 1:5, b = rnorm(10))
    lapply(x, mean)

    ## $a
    ## [1] 3
    ## 
    ## $b
    ## [1] 0.0264061

    x <- list(a = 1:4, b = rnorm(10), 
    c = rnorm(20, 1), d = rnorm(100, 5))
    lapply(x, mean)

    ## $a
    ## [1] 2.5
    ## 
    ## $b
    ## [1] 0.1725631
    ## 
    ## $c
    ## [1] 0.8230073
    ## 
    ## $d
    ## [1] 4.862065

    x <- 1:4
    lapply(x, runif)

    ## [[1]]
    ## [1] 0.3843159
    ## 
    ## [[2]]
    ## [1] 0.2033737 0.0259053
    ## 
    ## [[3]]
    ## [1] 0.15840701 0.06124935 0.29282368
    ## 
    ## [[4]]
    ## [1] 0.9975102 0.8380641 0.4774508 0.7992111

sapply
------

-   *sapply* tratará de simplificar el resultado de lapply de ser
    posible

-   Si el resultado es una lista donde cada elemento es de longitud 1,
    entonces retorna un vector
-   Si el resultado es una lista donde cada elemento es un vector de la
    misma longitud (&gt;1), retorna una matriz.
-   Si lo puede descifrar las cosas, retorna una lista

<!-- -->

    x <- list(a = 1:4, b = rnorm(10), c = rnorm(20, 1), d = rnorm(100, 5))
    lapply(x, mean)

    ## $a
    ## [1] 2.5
    ## 
    ## $b
    ## [1] 0.03378258
    ## 
    ## $c
    ## [1] 0.8367706
    ## 
    ## $d
    ## [1] 4.961338

    sapply(x, mean)

    ##          a          b          c          d 
    ## 2.50000000 0.03378258 0.83677056 4.96133797

    mean(x)

    ## Warning in mean.default(x): argument is not numeric or logical: returning
    ## NA

    ## [1] NA

apply
-----

-   *apply* se use para evaluar una función sobre las dimensiones de un
    array
-   Es más usado para evaluar una función sobre las filas o columnas de
    una matriz
-   En general no es más rápido que un loop, pero cabe en una sola línea
    (:

<!-- -->

    x <- matrix(rnorm(200), 20, 10)
    apply(x, 2, mean)

    ##  [1]  0.10809143  0.07707672  0.08802427 -0.11327371 -0.10371889
    ##  [6] -0.08553906  0.08811626  0.32774436 -0.13240621 -0.07126382

    apply(x, 1, sum)

    ##  [1] -0.1302894  1.4780489  1.9731588  0.2846633 -9.6385427  2.7468087
    ##  [7]  7.3199165  0.1371131  1.8961687  1.1932070  2.9384321 -3.2854800
    ## [13]  1.2261066 -1.0786123  1.4911185  0.6589886  2.7509972  1.6649310
    ## [19] -1.1943110 -8.7753967

-   Para sumas y medias de matrices tenemos algunos :

    -   `rowSums = apply(x, 1, sum)`
    -   `rowMeans = apply(x, 1, mean)`
    -   `colSums = apply(x, 2, sum)`
    -   `colMeans = apply(x, 2, mean)`

-   Las funciones cortas son más rápidas, pero no se nota menos que se
    use matrices grades.

tapply
------

-   *tapply* Se usa para aplicar funciones sobre subconjuntos de un
    vector.

-   Tomamos medias por grupo:

<!-- -->

    x <- c(rnorm(10), runif(10), rnorm(10, 1))
    f <- gl(3, 10)
    f

    ##  [1] 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2 3 3 3 3 3 3 3 3 3 3
    ## Levels: 1 2 3

    tapply(x, f, mean)

    ##         1         2         3 
    ## 0.3240321 0.4659992 0.8207216

-   Para encontrar rangos por grupo:

<!-- -->

    tapply(x, f, range)

    ## $`1`
    ## [1] -1.454084  1.833629
    ## 
    ## $`2`
    ## [1] 0.04091485 0.76104505
    ## 
    ## $`3`
    ## [1] -1.084187  2.262947

Aggregate y By
==============

By
--

-   Para ejecutar esta función, usaremos la base de datos *InsectSprays*

<!-- -->

    data(InsectSprays)
    InsectSprays$x <- rnorm(length(InsectSprays$count))
    by(InsectSprays,InsectSprays$spray,summary)

    ## InsectSprays$spray: A
    ##      count       spray        x           
    ##  Min.   : 7.00   A:12   Min.   :-1.95251  
    ##  1st Qu.:11.50   B: 0   1st Qu.:-0.87870  
    ##  Median :14.00   C: 0   Median :-0.08103  
    ##  Mean   :14.50   D: 0   Mean   :-0.26476  
    ##  3rd Qu.:17.75   E: 0   3rd Qu.: 0.38132  
    ##  Max.   :23.00   F: 0   Max.   : 1.45845  
    ## -------------------------------------------------------- 
    ## InsectSprays$spray: B
    ##      count       spray        x          
    ##  Min.   : 7.00   A: 0   Min.   :-1.5538  
    ##  1st Qu.:12.50   B:12   1st Qu.:-0.6344  
    ##  Median :16.50   C: 0   Median :-0.3137  
    ##  Mean   :15.33   D: 0   Mean   :-0.2659  
    ##  3rd Qu.:17.50   E: 0   3rd Qu.: 0.1254  
    ##  Max.   :21.00   F: 0   Max.   : 0.7925  
    ## -------------------------------------------------------- 
    ## InsectSprays$spray: C
    ##      count       spray        x          
    ##  Min.   :0.000   A: 0   Min.   :-1.7387  
    ##  1st Qu.:1.000   B: 0   1st Qu.:-0.4213  
    ##  Median :1.500   C:12   Median : 0.1925  
    ##  Mean   :2.083   D: 0   Mean   : 0.2555  
    ##  3rd Qu.:3.000   E: 0   3rd Qu.: 0.9906  
    ##  Max.   :7.000   F: 0   Max.   : 1.9895  
    ## -------------------------------------------------------- 
    ## InsectSprays$spray: D
    ##      count        spray        x           
    ##  Min.   : 2.000   A: 0   Min.   :-1.73489  
    ##  1st Qu.: 3.750   B: 0   1st Qu.:-0.50905  
    ##  Median : 5.000   C: 0   Median :-0.09253  
    ##  Mean   : 4.917   D:12   Mean   :-0.12983  
    ##  3rd Qu.: 5.000   E: 0   3rd Qu.: 0.33560  
    ##  Max.   :12.000   F: 0   Max.   : 1.03556  
    ## -------------------------------------------------------- 
    ## InsectSprays$spray: E
    ##      count      spray        x          
    ##  Min.   :1.00   A: 0   Min.   :-2.2513  
    ##  1st Qu.:2.75   B: 0   1st Qu.:-0.7806  
    ##  Median :3.00   C: 0   Median :-0.1059  
    ##  Mean   :3.50   D: 0   Mean   :-0.1711  
    ##  3rd Qu.:5.00   E:12   3rd Qu.: 0.3822  
    ##  Max.   :6.00   F: 0   Max.   : 1.4324  
    ## -------------------------------------------------------- 
    ## InsectSprays$spray: F
    ##      count       spray        x          
    ##  Min.   : 9.00   A: 0   Min.   :-1.3517  
    ##  1st Qu.:12.50   B: 0   1st Qu.:-0.6895  
    ##  Median :15.00   C: 0   Median : 0.1674  
    ##  Mean   :16.67   D: 0   Mean   : 0.1536  
    ##  3rd Qu.:22.50   E: 0   3rd Qu.: 0.6735  
    ##  Max.   :26.00   F:12   Max.   : 2.4889

Aggregate
---------

    aggregate(InsectSprays[,-2],
    list(InsectSprays$spray),median)

    ##   Group.1 count           x
    ## 1       A  14.0 -0.08103163
    ## 2       B  16.5 -0.31367848
    ## 3       C   1.5  0.19251313
    ## 4       D   5.0 -0.09253051
    ## 5       E   3.0 -0.10590169
    ## 6       F  15.0  0.16737811
