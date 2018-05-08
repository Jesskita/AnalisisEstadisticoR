-   [Introducción](#introduccion)
-   [[Rezagos y operadores en
    diferencia](http://www.economia.unam.mx/biblioteca/redeco/Pdf/Rezagos%20y%20Operadores%20de%20Diferencia.pdf)](#rezagos-y-operadores-en-diferencia)
    -   [Operadores de rezagos](#operadores-de-rezagos)
-   [Descomposición de una serie de
    tiempo](#descomposicion-de-una-serie-de-tiempo)
    -   [Ejemplo](#ejemplo-1)
    -   [Descomposición: ¿aditiva o
        multiplicativa?](#descomposicion-aditiva-o-multiplicativa)
    -   [Ejemplo:](#ejemplo-2)
    -   [Detrend:](#detrend)
    -   [Suavizamiento: Holt-Winters](#suavizamiento-holt-winters)
-   [Modelos de series de tiempo](#modelos-de-series-de-tiempo)
    -   [Ruido blanco](#ruido-blanco)
-   [El modelo Autoregresivo](#el-modelo-autoregresivo)
    -   [Simulación:](#simulacion)
    -   [Prueba de Ljung-Box](#prueba-de-ljung-box)
-   [Referencias](#referencias)

<script type="text/x-mathjax-config">
    MathJax.Hub.Config({ TeX: { equationNumbers: {autoNumber: "all"} } });
  </script>

------------------------------------------------------------------------

Introducción
============

Una serie de tiempo es una sucesión de variables aleatorias ordenadas de
acuerdo a una unidad de tiempo, *Y*<sub>1</sub>, …, *Y*<sub>*T*</sub>.

¿Por qué usar series de tiempo?

-   Pronósticos
-   Entender el mecanismo de generación de datos (no visible al inicio
    de una investigación)

[Rezagos y operadores en diferencia](http://www.economia.unam.mx/biblioteca/redeco/Pdf/Rezagos%20y%20Operadores%20de%20Diferencia.pdf)
======================================================================================================================================

Operadores de rezagos
---------------------

Definción:

*Δ**Y*<sub>*t* − *i*</sub> = *Y*<sub>*t*</sub> − *Y*<sub>*t* − *i*</sub>

Ejemplos:

*Δ**Y*<sub>*t*</sub> = *Y*<sub>*t*</sub> − *Y*<sub>*t* − 1</sub>

Caso general:

*L*<sup>*j*</sup>*Y*<sub>*t*</sub> = *Y*<sub>*t* − *j*</sub>

Ejemplos:

*L*<sup>1</sup>*Y*<sub>*t*</sub> = *L**Y*<sub>*t*</sub> = *Y*<sub>*t* − 1</sub>
*L*<sup>2</sup>*Y*<sub>*t*</sub> = *Y*<sub>*t* − 2</sub>

*L*<sup>−2</sup>*Y*<sub>*t*</sub> = *Y*<sub>*t* + 2</sub>

*L*<sup>*i*</sup>*L*<sup>*j*</sup> = *L*<sup>*i* + *j*</sup> = *Y*<sub>*t* − (*i* + *j*)</sub>
 \# Manipulando `ts` en \`R

-   Abrir `IPCEcuador.csv`
-   Se puede ver una inflación variable

<!-- -->

    uu <- "https://raw.githubusercontent.com/vmoprojs/DataLectures/master/IPCEcuador.csv"
    datos <- read.csv(url(uu),header=T,dec=".",sep=",")
    IPC <- ts(datos$IPC,start=c(2005,1),freq=12)
    plot(IPC)

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-1-1.png)

La serie tiene tendencia creciente. Tratemos de quitar esa tendencia:

    plot(diff(IPC)) # Se puede ver una inlfacion estable
    abline(h=0)

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-2-1.png)

Se ha estabilizado, pero podemos hacerlo aún más con el logartimo de la
diferencia:

    plot(diff(log(IPC))) #Tasa de variacion del IPC

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-3-1.png)

La serie no tiene tendecia y es estable. Ahora, si deseo trabajar con un
subconjunto de datos, puedo...

    # Solo quiero trabajar con los datos de agosto 2008
    IPC2 <- window(IPC,start=c(2008,8),freq=1)
    plot(IPC2)

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-4-1.png)

    # IPC de todos los diciembres
    IPC.dic <- window(IPC,start=c(2005,12),freq=T)
    plot(IPC.dic)
    points(IPC.dic)

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-4-2.png)

Si tengo mensuales y necsito trabajar con el IPC anual:

    aggregate(IPC)

    ## Time Series:
    ## Start = 2005 
    ## End = 2012 
    ## Frequency = 1 
    ## [1] 1213.09 1241.75 1262.13 1349.24 1399.26 1435.96 1491.87 1542.53

A continuación algunas transformaciones frecuentes y su
[interpretación](https://www.ucm.es/data/cont/docs/518-2013-10-25-Tema_6_EctrGrado.pdf):

<table>
<colgroup>
<col width="43%" />
<col width="56%" />
</colgroup>
<thead>
<tr class="header">
<th>Transformación</th>
<th>Interpretación</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><span class="math inline"><em>z</em><sub><em>t</em></sub> = ∇<em>y</em><sub><em>t</em></sub> = <em>y</em><sub><em>t</em></sub> − <em>y</em><sub><em>t</em> − 1</sub></span></td>
<td>Cambio en <span class="math inline"><em>y</em><sub><em>t</em></sub></span>. Es un indicador de crecimiento absoluto.</td>
</tr>
<tr class="even">
<td><span class="math inline">$z_t=ln(y_t)-ln(y_{t-1})\approx \frac{y_t-y_{t-1}}{y_{t-1}}$</span></td>
<td>Es la tasa logarítmica de variación de una variable. Es un indicador de crecimiento relativo. Si se multiplica por 100 es la tasa de crecimiento porcentual de la variable</td>
</tr>
<tr class="odd">
<td><span class="math inline"><em>z</em><sub><em>t</em></sub> = ∇[<em>l</em><em>n</em>(<em>y</em><sub><em>t</em></sub>)−<em>l</em><em>n</em>(<em>y</em><sub><em>t</em> − 1</sub>)]</span></td>
<td>Es el cambio en la tasa logarítmica de variación de una variable. Es un indicador de la aceleración de la tasa de crecimiento relativo de una variable.</td>
</tr>
</tbody>
</table>

### Ejemplo

Veamos un gráfico más interesante usando un conjunto de datos anterior,
vamos a:

-   Abrir la base `estadísticas Turismo.csv`
-   Agregar de manera mensual
-   Convertir a `ts` y graficar

<!-- -->

    uu <- "https://raw.githubusercontent.com/vmoprojs/DataLectures/master/estadisticas%20Turismo.csv"
    datos<-read.csv(url(uu),header=T,dec=".",sep=";")
    attach(datos)

    # Visitas a Areas Naturales Protegidas

    # Sumar por mes y año

    mensual<-aggregate(TOTALMENSUAL,by=list(mesnum,Year),FUN="sum") # Los datos sin mes es el total de ese anio

    TOTALmensual<-ts(mensual[,3],start=c(2006,1),freq=12)
    plot(TOTALmensual)

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-6-1.png)

Se ve una tendencia creciente y también una cierta estacionalidad.
Veamos la misma serie en gráficos más atractivos:

    library(latticeExtra)
    library(RColorBrewer)
    library(lattice)
    xyplot(TOTALmensual)

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-7-1.png)

    asTheEconomist(xyplot(TOTALmensual,
                          main="TOTAL VISITAS MENUSALES \n AREAS PROTEGIDAS")
                   ,xlab="Year_mes",ylab="Visitantes")

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-7-2.png)

Descomposición de una serie de tiempo
=====================================

Componentes

-   Tendencia-ciclo: representa los cambios de largo plazo en el nivel
    de la serie de tiempo
-   Estacionalidad: Caracteriza fluctuaciones periódicas de longitud
    constante causadas por factores tales como temperatura, estación del
    año, periodo vacacional, políticas, etc.

*Y*<sub>*t*</sub> = *f*(*S*<sub>*t*</sub>, *T*<sub>*t*</sub>, *E*<sub>*t*</sub>)
 donde *Y*<sub>*t*</sub> es la serie observada, *S*<sub>*t*</sub> es el
componente estacional, *T*<sub>*t*</sub> es la tendencia y
*E*<sub>*t*</sub> es el término de error.

La forma de *f* en la ecuación anterior determina tipos de
[descomposiciones](https://seriesdetiempo.files.wordpress.com/2012/08/descomposicic3b3n-de-series-de-tiempo-base-cap-3-makridakis-et-al-fe-ago-2012.pdf):

<table>
<thead>
<tr class="header">
<th>Descomposición</th>
<th>Expresión</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Aditiva</td>
<td><span class="math inline"><em>Y</em><sub><em>t</em></sub> = <em>S</em><sub><em>t</em></sub> + <em>T</em><sub><em>t</em></sub> + <em>E</em><sub><em>t</em></sub></span></td>
</tr>
<tr class="even">
<td>Multiplicativa</td>
<td><span class="math inline"><em>Y</em><sub><em>t</em></sub> = <em>S</em><sub><em>t</em></sub> * <em>T</em><sub><em>t</em></sub> * <em>E</em><sub><em>t</em></sub></span></td>
</tr>
<tr class="odd">
<td>Transformación logaritmica</td>
<td><span class="math inline"><em>l</em><em>o</em><em>g</em>(<em>Y</em><sub><em>t</em></sub>)=<em>l</em><em>o</em><em>g</em>(<em>S</em><sub><em>t</em></sub>)+<em>l</em><em>o</em><em>g</em>(<em>T</em><sub><em>t</em></sub>)+<em>l</em><em>o</em><em>g</em>(<em>E</em><sub><em>t</em></sub>)</span></td>
</tr>
<tr class="even">
<td>Ajuste estacional</td>
<td><span class="math inline"><em>Y</em><sub><em>t</em></sub> − <em>S</em><sub><em>t</em></sub> = <em>T</em><sub><em>t</em></sub> + <em>E</em><sub><em>t</em></sub></span></td>
</tr>
</tbody>
</table>

### Ejemplo

    visitas.descompuesta<-decompose(TOTALmensual, type="additive")
    plot(visitas.descompuesta)

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-8-1.png)

Dentro de `visitas.decompuesta` tenemos los siguientes elementos:

-   `$x` = serie original
-   `$seasonal` = componente estacional de los datos EJ: en marzo hay un
    decremento de 2502 (para cada dato)
-   `$trend` = tendencia
-   `$random` = visitas no explicadas por la tendencia o la
    estacionalidad
-   `$figure` = estacionalidad (mismo que `seasonal` pero sin
    repetición)

### Descomposición: ¿aditiva o multiplicativa?

Visualmenete:

-   Aditivo:
    -   las fluctuaciones estacionales lucen aproximadamente constantes
        en tamaño con el tiempo y
    -   no parecen depender del nivel de la serie temporal,
    -   y las fluctuaciones aleatorias también parecen ser más o menos
        constantes en tamaño a lo largo del tiempo
-   Multiplciativo
    -   Si el efecto estacional tiende a aumentar a medida que aumenta
        la tendencia
    -   la varianza de la serie original y la tendencia aumentan con el
        tiempo

Forma alternativa de elegir: ver cuál es la que tiene un componente
aleatorio menor.

### Ejemplo:

Los datos en el archivo `wine.dat` son ventas mensuales de vino
australiano por categoría, en miles de litros, desde enero de 1980 hasta
julio de 1995. Las categorías son blanco fortificado (`fortw`), blanco
seco (`dryw`), blanco dulce (`sweetw`), rojo (`red`), rosa (`rose`) y
espumoso (`spark`).

    direccion<- "https://raw.githubusercontent.com/dallascard/Introductory_Time_Series_with_R_datasets/master/wine.dat"
    wine<-read.csv(direccion,header=T,sep="")
    attach(wine)
    head(wine)

    ##   winet fortw dryw sweetw  red rose spark
    ## 1     1  2585 1954     85  464  112  1686
    ## 2     2  3368 2302     89  675  118  1591
    ## 3     3  3210 3054    109  703  129  2304
    ## 4     4  3111 2414     95  887   99  1712
    ## 5     5  3756 2226     91 1139  116  1471
    ## 6     6  4216 2725     95 1077  168  1377

    dulce <- ts(sweetw,start=c(1980,12), freq=12)
    plot(dulce)

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-9-1.png)

Tratemos la serie como un caso aditivo:

En funcion del grafico de la variable, se decide el "type" de la
descomposicion La estacionalidad tiene valores negativos porque se
plantea respecto de la tendencia

    dulce.descompuesta<-decompose(dulce, type="additive") 
    plot(dulce.descompuesta)

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-10-1.png)

    a<-dulce.descompuesta$trend[27] # La tendencia era de 130 en mayo del 82
    b<-dulce.descompuesta$seasonal[27] # El componente de la estacionalidad era este
    c<-dulce.descompuesta$random[27]  # Es el componente aleatorio

    a+b+c # La sumatoria de la descomposicion de la serie da el valor real, si es aditiva

    ## [1] 127

Veamos el caso multiplicativo:

    # Multiplicativa
    dulce.descompuesta1<-decompose(dulce, type="multiplicative")
    a<-dulce.descompuesta1$trend[27] # La tendencia era de 130 en mayo del 82
    b<-dulce.descompuesta1$seasonal[27] # El componente de la estacionalidad era este
    c<-dulce.descompuesta1$random[27]  # Es el componente aleatorio

    a*b*c # La sumatoria de la descomposicion de la serie da el valor real, si es aditiva

    ## [1] 127

Veamos la forma alternativa de elección:

    u1<-var(dulce.descompuesta$random,na.rm=T)
    u2<-var(dulce.descompuesta1$random,na.rm=T)
    cbind(u1,u2)

    ##            u1         u2
    ## [1,] 1970.235 0.02247602

Se escoge la multiplicativa en este caso.

### Detrend:

Las series se ofrecen generalmente sin tendencia ni estacionalidad

    dulce.detrended <- dulce-dulce.descompuesta$trend
    plot(dulce.detrended)
    abline(h=0)

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-13-1.png)

Parece ser que hay un cambio en la varianza desde el 85.

Si descomponemos multiplicativamente en vez de restar se debe dividir.

    plot(dulce-dulce.descompuesta$trend)

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-14-1.png)

    plot(dulce/dulce.descompuesta1$trend)

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-14-2.png)

Existen formas de descomponer más sofisticadas, por ejemplo, usando la
función `stl`.

    dulce.stl<-stl(dulce,s.window="per")
    plot(dulce.stl)

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-15-1.png)

En este caso el calculo de la tendencia cambia, se calcula con formas no
parametricas. La barra del final es la desviacion estándar.

Suavizamiento: Holt-Winters
---------------------------

El método se resume en las f?rmulas siguientes:
El método de Holt-Winters generaliza el método de suavizamiento
exponencial.

### Ejemplo

Veamos un modelo más sencillo:

    dulce.se <- HoltWinters(dulce,beta=0,gamma=0)
    plot(dulce.se) 

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-16-1.png)

Es un suavizamiento HW sin tendencia y sin componente estacional. La
serie roja son los datos con suavizamiento exponencial y la negra son
los observados. `R` buscó el alpha que le pareció apropiado.

Usemos un alpha deliberado:

    dulce.se1 <- HoltWinters(dulce,alpha=0.8,beta=0,gamma=0)
    plot(dulce.se1)

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-17-1.png)

¿Qué pasó con los errores?

    dulce.se$SSE # Suma de los residuos al cuadrado (de un paso)

    ## [1] 963408.2

    dulce.se1$SSE # Suma de los residuos al cuadrado (de un paso)

    ## [1] 1132577

### Ejemplo

Total mensual de pasajeros (en miles) de líneas aéreas internacionales,
de 1949 a 1960.

    data(AirPassengers)
    str(AirPassengers)

    ##  Time-Series [1:144] from 1949 to 1961: 112 118 132 129 121 135 148 148 136 119 ...

    plot(AirPassengers)

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-19-1.png)

Se aprecia tendencia y variabilidad. Podemos usar HW para predicción:

    ap.hw<- HoltWinters(AirPassengers,seasonal="mult")
    plot(ap.hw)

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-20-1.png)

    ap.prediccion <- predict(ap.hw,n.ahead=48)
    ts.plot(AirPassengers,ap.prediccion,lty=1:2,
    col=c("blue","red"))

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-20-2.png)

Modelos de series de tiempo
===========================

Ruido blanco
------------

    n   <-200
    mu  <- 0
    sdt <- 3
    w <- rnorm(n,mu,sdt)

¿Cómo se si algo tiene ruido blanco? : Analizo la funcion de
autocorrelacion muestral.

$$
\\hat\\rho\_k = \\frac{\\hat\\gamma\_k}{\\hat\\gamma\_0}
$$
 donde
$$
\\hat\\gamma\_k = \\frac{\\sum(Y\_t-\\bar{Y})(Y\_{t+k}-\\bar{Y})}{n}
$$
$$
\\hat\\gamma\_0 = \\frac{\\sum(Y\_t-\\bar{Y})^2}{n}
$$

Se asume que $\\hat\\rho\_k \\sim N(0,1/n)$

    acf(w)

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-22-1.png)

Si se sale de las franjas, si hay correlacion y no hay ruido blanco

El modelo Autoregresivo
=======================

Proceso estocástico autoregresivo de primer orden

Donde *δ* es la media de *Y* y *u*<sub>*i*</sub> es un ruido blanco no
correlacionado

    uu <- "https://raw.githubusercontent.com/vmoprojs/DataLectures/master/WCURRNS.csv"
    datos <- read.csv(url(uu),header=T,sep=";")
    names(datos)

    ## [1] "DATE"  "VALUE"

    attach(datos)
    value.ts <- ts(VALUE, start=c(1975,1),freq=54)
    ts.plot(value.ts)

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-23-1.png)

Estacionariedad: La serie es estacionaria si la varianza no cambia

    acf(value.ts)

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-24-1.png)

Esta es la marca de una serie que NO es estacionaria, dado que la
autocorrelacion decrece muy lentamente.

    plot(stl(value.ts,s.window="per"))  

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-25-1.png)

Una forma de trabajar con una serie esacionaria es quitarle el trend

    valuetrend<- value.ts- stl(value.ts,s.window="per")$time.series[,2]
    plot(valuetrend)

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-26-1.png)

Reminder es lo que queda sin tendencia ni estacionalidad

    valuereminder<-
    stl(value.ts,s.window="per")$time.series[,3]

Veamos cómo quedo la serie:

    acf(valuereminder)

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-28-1.png)

Se puede decir que la hicimos una serie estacionaria

Otra forma de hacer estacionaria una serie es trabajar con las
diferencias

    acf(diff(value.ts))

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-29-1.png)

Nos indica que hay una estructura en la serie que no es ruido blanco
pero SI estacionaria (cae de 1 a "casi"" cero)

### Simulación:

El siguiente paso es modelar esta estructura. Un modelo para ello es un
modelo autorregresivo. Simular un AR(1)

    y <- arima.sim(list(ar=c(0.99),sd=1),n=200)
    plot(y)

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-30-1.png)

    acf(y)

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-30-2.png)

¿Cuáles son los parámetros del `arima.sim`? Hemos simulado
*Y*<sub>*t*</sub> = *ϕ*<sub>0</sub> + *ϕ*<sub>1</sub>*Y*<sub>*t* − 1</sub> = *ϕ*<sub>0</sub> + 0.99*Y*<sub>*t* − 1</sub>.

Simulemos el modelo:
*Y*<sub>*t*</sub> = 0.5*Y*<sub>*t* − 1</sub> − 0.7*Y*<sub>*t* − 2</sub> + 0.6*Y*<sub>*t* − 3</sub>

    ar3 <- arima.sim(n=200,list(ar=c(0.5,-0.7,0.6)),sd=5) 
    ar3.ts = ts(ar3)
    plot(ar3.ts)

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-31-1.png)

    acf(ar3)

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-31-2.png)

Las autocorrelaciones decaen exponencialemente a cero

**Autocorrelaciones parciales**: nos ayunda a determinar el orden del
modelo.

La autocorrelación parcial es la correlación entre *Y*<sub>*t*</sub> y
*Y*<sub>*t* − *k*</sub> después de eliminar el efecto de las Y
intermedias.

En los datos de series de tiempo, una gran proporción de la correlación
entre *Y*<sub>*t*</sub> y *Y*<sub>*t* − *k*</sub> puede deberse a sus
correlaciones con los rezagos intermedios
$Y\_1,Y\_2,\\lpoints,Y\_{t-k+1}$. La correlación parcial elimina la
influencia de estas variables intermedias.

    pacf(ar3) 

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-32-1.png)

    ar(ar3)$aic

    ##          0          1          2          3          4          5 
    ## 184.479991 184.838627  91.914972   0.000000   1.931100   3.923678 
    ##          6          7          8          9         10         11 
    ##   4.901181   6.687842   8.534966  10.022077  11.985459  13.635207 
    ##         12         13         14         15         16         17 
    ##  15.081464  16.607561  18.260433  20.241681  22.207918  23.924108 
    ##         18         19         20         21         22         23 
    ##  23.773030  22.467178  23.933922  25.719837  27.626298  29.615839

La tercera autocorrelación es la que esta fuera de las bandas, esto
indica que el modelo es un AR(3)

#### Ejemplo

Datos: precio de huevos desde 1901

    uu <- "https://raw.githubusercontent.com/vmoprojs/DataLectures/master/PrecioHuevos.csv"
    datos <- read.csv(url(uu),header=T,sep=";")
    ts.precio <- ts(datos$precio,start=1901)
    plot(ts.precio)

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-33-1.png)

Veamos las autocorrelaciones:

    par(mfrow=c(2,1))
    acf(ts.precio)
    pacf(ts.precio)

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-34-1.png)

    par(mfrow=c(1,1))

Las auto si decaen, no lo hacen tan rápido. No se puede decir si es
estacionario o no.

Evaluemos un modelo:

    modelo1 <- arima(ts.precio, order=c(1,0,0))
    print(modelo1)

    ## 
    ## Call:
    ## arima(x = ts.precio, order = c(1, 0, 0))
    ## 
    ## Coefficients:
    ##          ar1  intercept
    ##       0.9517   195.5066
    ## s.e.  0.0310    48.1190
    ## 
    ## sigma^2 estimated as 712.3:  log likelihood = -443.28,  aic = 892.56

    modelo1$var.coef

    ##                     ar1    intercept
    ## ar1        0.0009588394   -0.1532017
    ## intercept -0.1532017342 2315.4371739

¿Qué nos recomienda `R`?

    ar.precio <- ar(ts.precio)
    ar.precio

    ## 
    ## Call:
    ## ar(x = ts.precio)
    ## 
    ## Coefficients:
    ##      1  
    ## 0.9237  
    ## 
    ## Order selected 1  sigma^2 estimated as  975.9

Analicemos los residuos

    residuos = ar.precio$resid
    # Los residuos debe estar sin ninguna estructura
    par(mfrow = c(2,1))
    plot(residuos)
    abline(h=0,col="red")
    abline(v=1970,col="blue")
    acf(residuos,na.action=na.pass)

![](%5BP3_-1%5D_SeriesTiempo_files/figure-markdown_strict/unnamed-chunk-37-1.png)

### Prueba de Ljung-Box

La prueba de Ljung-Box se puede definir de la siguiente manera.

*H*<sub>0</sub>: Los datos se distribuyen de forma independiente (es
decir, las correlaciones en la población de la que se toma la muestra
son 0, de modo que cualquier correlación observada en los datos es el
resultado de la aleatoriedad del proceso de muestreo).

*H*<sub>*a*</sub>: Los datos no se distribuyen de forma independiente.

La estadística de prueba es:

$$
Q=n\\left(n+2\\right)\\sum \_{k=1}^{h}{\\frac {{\\hat {\\rho }}\_{k}^{2}}{n-k}}
$$

donde n es el tamaño de la muestra, $\\hat\\rho\_{k}$ es la
autocorrelación de la muestra en el retraso k y h es el número de
retardos que se están probando. Por nivel de significación *α*, la
región crítica para el rechazo de la hipótesis de aleatoriedad es
*Q* &gt; *χ*<sub>1 − *α*, *h*</sub><sup>2</sup>

donde *χ*<sub>1 − *α*, *h*</sub><sup>2</sup> es la *α*-cuantil de la
distribución chi-cuadrado con *m* grados de libertad.

La prueba de Ljung-Box se utiliza comúnmente en autorregresivo integrado
de media móvil de modelado (ARIMA). Tenga en cuenta que se aplica a los
residuos de un modelo ARIMA equipada, no en la serie original, y en
tales aplicaciones, la hipótesis de hecho objeto del ensayo es que los
residuos del modelo ARIMA no tienen autocorrelación. Al probar los
residuales de un modelo ARIMA estimado, los grados de libertad deben ser
ajustados para reflejar la estimación de parámetros. Por ejemplo, para
un modelo ARIMAa (p,0,q), los grados de libertad se debe establecer en
*h* − *p* − *q*.

    Box.test(residuos,lag=20,type="Ljung")

    ## 
    ##  Box-Ljung test
    ## 
    ## data:  residuos
    ## X-squared = 20.526, df = 20, p-value = 0.4255

¿Es ruido blanco?

Referencias
===========
