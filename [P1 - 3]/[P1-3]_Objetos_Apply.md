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
-   [Funciones](#funciones)
    -   [Oasis: Algo de cálculo](#oasis-algo-de-calculo)
    -   [VAN](#van)
-   [La Familia Apply](#la-familia-apply)
    -   [lapply](#lapply)
    -   [sapply](#sapply)
    -   [apply](#apply)
    -   [tapply](#tapply)
        -   [Ejercicio](#ejercicio)
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

    n <- c(2, 3, 5) 
    s <- c("aa", "bb", "cc", "dd", "ee") 
    b <- c(TRUE, FALSE, TRUE, FALSE, FALSE) 
    x <- list(n, s, b, 3)

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

    ma <- rbind(m1, m2) %*% cbind(m1, m2) 

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

    z <- 0
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

    r <- 5
    if(r==4){
      x <-1
    }else{
      x <- 3
      y <- 4
    }

Funciones
=========

Las funciones son declaradas con `function(x,y,...)` seguido de llaves
`{}`. Los valores dentro de la función son los parámetros o valores de
entrada. Dentro de las llaves se ubican las operaciones a realizar con
dichos parámetros.

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
    ## [1] 0.1036668

    x <- list(a = 1:4, b = rnorm(10), 
    c = rnorm(20, 1), d = rnorm(100, 5))
    lapply(x, mean)

    ## $a
    ## [1] 2.5
    ## 
    ## $b
    ## [1] 0.01170515
    ## 
    ## $c
    ## [1] 1.158547
    ## 
    ## $d
    ## [1] 5.074743

    x <- 1:4
    lapply(x, runif)

    ## [[1]]
    ## [1] 0.5424215
    ## 
    ## [[2]]
    ## [1] 0.7061515 0.6289035
    ## 
    ## [[3]]
    ## [1] 0.2126211 0.4728819 0.6153962
    ## 
    ## [[4]]
    ## [1] 0.01294977 0.82542887 0.06251819 0.17994112

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
    ## [1] -0.1012816
    ## 
    ## $c
    ## [1] 1.106519
    ## 
    ## $d
    ## [1] 4.869479

    sapply(x, mean)

    ##          a          b          c          d 
    ##  2.5000000 -0.1012816  1.1065186  4.8694794

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

    ##  [1] -0.03287329  0.25130233 -0.11444505  0.02843504 -0.13591201
    ##  [6]  0.11449986  0.04083881 -0.36202407 -0.05670987 -0.03203557

    apply(x, 1, sum)

    ##  [1] -1.5280661 -1.2097397 -5.7300001 -2.8786035 -1.3685879  9.5015141
    ##  [7] -2.4183382 -4.1743886  1.0066074  3.5804879  4.0577008  2.2879738
    ## [13]  0.2150905 -3.4123630  1.6775966  1.9728069 -1.4731446 -2.9304204
    ## [19] -5.6687923  2.5141899

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

    ##          1          2          3 
    ## 0.01734637 0.43810064 1.25342106

-   Para encontrar rangos por grupo:

<!-- -->

    tapply(x, f, range)

    ## $`1`
    ## [1] -1.725586  2.649358
    ## 
    ## $`2`
    ## [1] 0.03868276 0.79229829
    ## 
    ## $`3`
    ## [1] 0.4873724 2.1143489

### Ejercicio

Al evaluar la relación estadística de dos variables, hay muchas
alternativas a la medida de correlación estándar (correlación
producto-momento de Pearson). Algunos pueden haber oído hablar de la
correlación de rangos de Spearman, por ejemplo. Estas medidas
alternativas tienen varias motivaciones, como la solidez de valores
atípicos, que son elementos de datos extremos y posiblemente erróneos.

Aquí, propongamos una nueva medida de este tipo (en realidad se
relaciona con uno de amplio uso, **τ* de Kendall*), pero para ilustrar
algunas de las técnicas de programación R introducidas hasta el momento,
especialmente `ifelse ()`.

Ayuda: Considere los vectores `x` e `y`, que son series temporales, por
ejemplo, para las mediciones de las acciones de dos empresas recogidas
una vez por hora. Definiremos nuestra medida de asociación entre ellos
como la *fracción del tiempo `x` y `y` que aumentan o disminuyen
juntos*, es decir, la proporción de `i` para la cual `y[i + 1] -y [i]`
tiene el mismo signo que `x[i + 1] -x [i]`.

    x <- c(5,12,13,3,6,0,1,15,16,8,88)
    y <- c(4,2,3,23,6,10,11,12,6,3,2)
    udcorr(x,y)

    ## [1] 0.4

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
    ##  Min.   : 7.00   A:12   Min.   :-1.47311  
    ##  1st Qu.:11.50   B: 0   1st Qu.:-0.42303  
    ##  Median :14.00   C: 0   Median : 0.20753  
    ##  Mean   :14.50   D: 0   Mean   : 0.03747  
    ##  3rd Qu.:17.75   E: 0   3rd Qu.: 0.65958  
    ##  Max.   :23.00   F: 0   Max.   : 1.16995  
    ## -------------------------------------------------------- 
    ## InsectSprays$spray: B
    ##      count       spray        x          
    ##  Min.   : 7.00   A: 0   Min.   :-1.4925  
    ##  1st Qu.:12.50   B:12   1st Qu.:-0.8017  
    ##  Median :16.50   C: 0   Median :-0.4574  
    ##  Mean   :15.33   D: 0   Mean   :-0.1410  
    ##  3rd Qu.:17.50   E: 0   3rd Qu.: 0.8493  
    ##  Max.   :21.00   F: 0   Max.   : 1.1400  
    ## -------------------------------------------------------- 
    ## InsectSprays$spray: C
    ##      count       spray        x          
    ##  Min.   :0.000   A: 0   Min.   :-2.4216  
    ##  1st Qu.:1.000   B: 0   1st Qu.:-0.6679  
    ##  Median :1.500   C:12   Median : 0.1808  
    ##  Mean   :2.083   D: 0   Mean   : 0.1136  
    ##  3rd Qu.:3.000   E: 0   3rd Qu.: 0.9172  
    ##  Max.   :7.000   F: 0   Max.   : 2.7003  
    ## -------------------------------------------------------- 
    ## InsectSprays$spray: D
    ##      count        spray        x          
    ##  Min.   : 2.000   A: 0   Min.   :-2.0907  
    ##  1st Qu.: 3.750   B: 0   1st Qu.:-0.3795  
    ##  Median : 5.000   C: 0   Median :-0.1792  
    ##  Mean   : 4.917   D:12   Mean   : 0.1569  
    ##  3rd Qu.: 5.000   E: 0   3rd Qu.: 0.5825  
    ##  Max.   :12.000   F: 0   Max.   : 3.1879  
    ## -------------------------------------------------------- 
    ## InsectSprays$spray: E
    ##      count      spray        x          
    ##  Min.   :1.00   A: 0   Min.   :-0.9015  
    ##  1st Qu.:2.75   B: 0   1st Qu.:-0.5427  
    ##  Median :3.00   C: 0   Median : 0.4464  
    ##  Mean   :3.50   D: 0   Mean   : 0.4102  
    ##  3rd Qu.:5.00   E:12   3rd Qu.: 1.0716  
    ##  Max.   :6.00   F: 0   Max.   : 2.3189  
    ## -------------------------------------------------------- 
    ## InsectSprays$spray: F
    ##      count       spray        x          
    ##  Min.   : 9.00   A: 0   Min.   :-3.3216  
    ##  1st Qu.:12.50   B: 0   1st Qu.:-0.9185  
    ##  Median :15.00   C: 0   Median :-0.1921  
    ##  Mean   :16.67   D: 0   Mean   :-0.4913  
    ##  3rd Qu.:22.50   E: 0   3rd Qu.: 0.1916  
    ##  Max.   :26.00   F:12   Max.   : 0.3576

Aggregate
---------

    aggregate(InsectSprays[,-2],
    list(InsectSprays$spray),median)

    ##   Group.1 count          x
    ## 1       A  14.0  0.2075261
    ## 2       B  16.5 -0.4574206
    ## 3       C   1.5  0.1808310
    ## 4       D   5.0 -0.1792082
    ## 5       E   3.0  0.4464450
    ## 6       F  15.0 -0.1920908
