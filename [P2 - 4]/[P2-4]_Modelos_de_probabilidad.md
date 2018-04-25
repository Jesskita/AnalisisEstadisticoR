-   [Probabilidad lineal](#probabilidad-lineal)
-   [Logit](#logit)
    -   [Ejemplo 1](#ejemplo-1)
    -   [Ejemplo 2](#ejemplo-2)
-   [Probit](#probit)
-   [Tobit](#tobit)
-   [Referencias](#referencias)

<!--
La revisión metodológica aquí vertida se basa en [@Wang_2012].
-->
Probabilidad lineal
===================

En este caso la variable dependiente es una dummy

![](RL_Im7.png)

Se trata de modelos del tipo:

Veamos un ejemplo: Abrir la base `MROZ` de Wooldridge y ajuste el
modelo:

*i**n**l**f* = *β*<sub>0</sub>*n**w**i**f**e**i**n**c* + *β*<sub>1</sub>*e**d**u**c* + *β*<sub>2</sub>*e**x**p**e**r* + *β*<sub>3</sub>*e**x**p**e**r**s**q* + *β*<sub>4</sub>*a**g**e* + *β*<sub>5</sub>*k**i**d**s**l**t*6 + *β*<sub>6</sub>*k**i**d**s**g**e*6

En R:

    uu <- "https://raw.githubusercontent.com/vmoprojs/DataLectures/master/mroz.csv"
    datos <- read.csv(url(uu),header=FALSE,na.strings = ".")
    # str(datos)

    # De ser necesario, quitar comentario y ejecutar:
    # datos$V1[1] <- 1
    # datos$V1 <- as.numeric(as.character(datos$V1))

    names(datos) <- c("inlf","hours",  "kidslt6", "kidsge6", 
                   "age", "educ",  "wage",    
                   "repwage",
                   "hushrs"  ,  "husage", "huseduc" ,
                   "huswage"  , "faminc", "mtr","motheduc",
                   "fatheduc" , "unem"    ,  "city"     , "exper"   ,  "nwifeinc" , "lwage" ,    "expersq" )
    attach(datos)

    reg1 <- lm(inlf~nwifeinc+educ + exper +
                expersq + age + kidslt6 + 
                kidsge6)
    summary(reg1)

    ## 
    ## Call:
    ## lm(formula = inlf ~ nwifeinc + educ + exper + expersq + age + 
    ##     kidslt6 + kidsge6)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -0.93432 -0.37526  0.08833  0.34404  0.99417 
    ## 
    ## Coefficients:
    ##               Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  0.5855192  0.1541780   3.798 0.000158 ***
    ## nwifeinc    -0.0034052  0.0014485  -2.351 0.018991 *  
    ## educ         0.0379953  0.0073760   5.151 3.32e-07 ***
    ## exper        0.0394924  0.0056727   6.962 7.38e-12 ***
    ## expersq     -0.0005963  0.0001848  -3.227 0.001306 ** 
    ## age         -0.0160908  0.0024847  -6.476 1.71e-10 ***
    ## kidslt6     -0.2618105  0.0335058  -7.814 1.89e-14 ***
    ## kidsge6      0.0130122  0.0131960   0.986 0.324415    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.4271 on 745 degrees of freedom
    ## Multiple R-squared:  0.2642, Adjusted R-squared:  0.2573 
    ## F-statistic: 38.22 on 7 and 745 DF,  p-value: < 2.2e-16

¿Qué hemos ajustado?

    plot(inlf~educ)
    abline(coef(lm(inlf~educ)))

![](%5BP2-4%5D_Modelos_de_probabilidad_files/figure-markdown_strict/unnamed-chunk-2-1.png)

-   Excepto *kidsge6* los coeficientes son significativos.
-   Se introdujo la experiencia cuadrática para capturar un efecto
    decreciente en el efecto deseado (inlf). ¿Cómo lo interpretamos?

`.039 - 2(.0006)exper = 0.39 - .0012exper`

-   El punto en el que la experiencia ya no tiene efecto en *inlf* es
    .039/.0012 = 32.5. ¿Cuantos elementos de la muestra tienen más de 32
    años de experiencia?

Se añade exper al cuadrado porque queremos dar la posibilidad que los
años adicionales de expericnecia contribuyan con un efecto decreciente.

Trabajemos ahora con la predicción, y revisemos el resultado:

    prediccion <- predict(reg1)
    head(cbind(inlf,prediccion))

    ##   inlf prediccion
    ## 1    1  0.6636123
    ## 2    1  0.7009166
    ## 3    1  0.6727286
    ## 4    1  0.7257441
    ## 5    1  0.5616358
    ## 6    1  0.7928528

    summary(prediccion)

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ## -0.3451  0.4016  0.5880  0.5684  0.7592  1.1272

¿Qué podemos notar?

-   Existen valores mayores a 1 e inferiores a 0.
-   *R*<sup>2</sup> ya no es interpretable en estas regresiones
-   Usaremos una probabilidad de ocurrencia, digamos 0.5

<!-- -->

    prediccion.inlf <- (prediccion>=0.5)*1
    head(cbind(inlf,prediccion,prediccion.inlf))

    ##   inlf prediccion prediccion.inlf
    ## 1    1  0.6636123               1
    ## 2    1  0.7009166               1
    ## 3    1  0.6727286               1
    ## 4    1  0.7257441               1
    ## 5    1  0.5616358               1
    ## 6    1  0.7928528               1

    sum(inlf)-sum(prediccion.inlf)

    ## [1] -44

-   Viendo la tabla, ¿cuál será la tasa de predicción del modelo actual?

<!-- -->

    tabla1 <- table(inlf,prediccion.inlf)
    tabla1

    ##     prediccion.inlf
    ## inlf   0   1
    ##    0 203 122
    ##    1  78 350

    porcentaje.correcto <- (tabla1[1,1]+tabla1[2,2])/length(prediccion)
    porcentaje.correcto

    ## [1] 0.7343958

Para resolver el problema anterior de los valores fuera del intervalo
0-1, se propone una función diferente.

Logit
=====

La regresión logística puede entenderse simplemente como encontrar los
parámtros *β* que mejor asjuten:

$$
y={\\begin{cases}1&\\beta\_{1}+\\beta\_{2}X\_{1}+\\cdots+\\beta\_{k}X\_{k}+u &gt;0\\\\0&{\\text{en otro caso}}\\end{cases}}
$$

Donde se asume que el error tiene una [distribución logística
estándar](https://es.wikipedia.org/wiki/Distribuci%C3%B3n_log%C3%ADstica)

$$
{\\displaystyle f(x;\\mu ,s)={\\frac {e^{-{\\frac {x-\\mu }{s}}}}{s\\left(1+e^{-{\\frac {x-\\mu }{s}}}\\right)^{2}}}={\\frac {1}{s\\left(e^{\\frac {x-\\mu }{2s}}+e^{-{\\frac {x-\\mu }{2s}}}\\right)^{2}}}={\\frac {1}{4s}}\\operatorname {sech} ^{2}\\!\\left({\\frac {x-\\mu }{2s}}\\right).}
$$
 Donde *s* es el parámetro de escala y *μ* el de locación (*sech* es la
función secante hiperbólico).

Otra forma de entender la regresión logística es a través de la función
logística:

$$
\\sigma (t)={\\frac {e^{t}}{e^{t}+1}}={\\frac {1}{1+e^{-t}}}
$$

donde $ t$ y 0 ≤ *σ*(*t*)≤1.

Asumiento *t* como una función lineal de una variable explicativa *x*,
tenemos:

*t* = *β*<sub>0</sub> + *β*<sub>1</sub>*x*

Ahora la función logística se puede expresar:

$$
p(x)={\\frac {1}{1+e^{-(\\beta \_{0}+\\beta \_{1}x)}}}
$$

Ten en cuenta que *p*(*x*) se interpreta como la probabilidad de que la
variable dependiente iguale a *éxito* en lugar de un *fracaso*. Está
claro que las variables de respuesta *Y*<sub>*i*</sub> no se distribuyen
de forma idéntica: $ P (Y\_ {i} = 1  mid X )$ difiere de un punto
*X*<sub>*i*</sub> a otro, aunque son independientes dado que la matriz
de diseño *X* y los parámetros compartidos *β*.

Finalmente definimos la inversa de la función logística, *g*, el
**logit** (log odds):

$$
{\\displaystyle g(p(x))=\\ln \\left({\\frac {p(x)}{1-p(x)}}\\right)=\\beta \_{0}+\\beta \_{1}x,}
$$

lo que es equivalente a:

$$
{\\frac {p(x)}{1-p(x)}}=e^{\\beta \_{0}+\\beta \_{1}x}
$$

**Interpretación**:

-   *g* es la función logit. La ecuación para *g*(*p*(*x*)) ilustra que
    el logit (es decir, log-odds o logaritmo natural de las
    probabilidades) es equivalente a la expresión de regresión lineal.
-   *l**n* denota el logaritmo natural.
-   *p*(*x*) es la probabilidad de que la variable dependiente sea igual
    a un caso, dada alguna combinación lineal de los predictores. La
    fórmula para *p*(*x*) ilustra que la probabilidad de que la variable
    dependiente iguale un caso es igual al valor de la función logística
    de la expresión de regresión lineal. Esto es importante porque
    muestra que el valor de la expresión de regresión lineal puede
    variar de infinito negativo a positivo y, sin embargo, después de la
    transformación, la expresión resultante para la probabilidad
    *p*(*x*) oscila entre 0 y 1.
-   *β*<sub>0</sub> es la intersección de la ecuación de regresión
    lineal (el valor del criterio cuando el predictor es igual a cero).
-   *β*<sub>1</sub>*x* es el coeficiente de regresión multiplicado por
    algún valor del predictor.
-   la base *e* denota la función exponencial.

<!-- \begin{eqnarray} -->
<!-- Y &=& f(\beta_{1}+\beta_{2}X_{1}+\cdots+\beta_{k}X_{k}) + u\nonumber \\ -->
<!-- f(z) &=& \frac{e^{z}}{1+e^{z}}\nonumber \\ -->
<!-- E[Y]&=& P(Y = 1) = \frac{e^{\beta_{1}+\beta_{2}X_{1}+\cdots+\beta_{k}X_{k}}}{1+e^{\beta_{1}+\beta_{2}X_{1}+\cdots+\beta_{k}X_{k}}} \nonumber -->
<!-- \end{eqnarray} -->
#### Ejemplo 1

-   Abra la base de datos `wells.dat`
-   Note que existe una variable llamada `switch`. Dado que esta palabra
    es un condicional, debemos cambiar el nombre de la variable:
    `names(datos)[1]="Switch"`
-   Realice un gráfico del cambio vs arsénico e interprete

<!-- -->

    uu <- "https://raw.githubusercontent.com/vmoprojs/DataLectures/master/wells.dat"
    datos <- read.csv(url(uu),sep="",dec=".",header=TRUE)
    names(datos)[1]="Switch"
    attach(datos)

    ## The following object is masked from datos (pos = 3):
    ## 
    ##     educ

    plot(Switch~arsenic, main="Cambio VS contenido de arsenico")

![](%5BP2-4%5D_Modelos_de_probabilidad_files/figure-markdown_strict/unnamed-chunk-6-1.png)

-   Corra el siguiente
    modelo:`ajuste2 <- glm(Switch ~ dist1,family=binomial(link="logit"),x=T)`
-   Donde: `dist1 <- dist/100`
-   Interpretación: Si la distancia es cero, la probabilidad de cambio
    es de 0.6, es decir 60%

<!-- -->

    dist1=dist/100
    ajuste2 <- glm(Switch~dist1,family=binomial(link="logit"), x = T)
    summary(ajuste2)

    ## 
    ## Call:
    ## glm(formula = Switch ~ dist1, family = binomial(link = "logit"), 
    ##     x = T)
    ## 
    ## Deviance Residuals: 
    ##     Min       1Q   Median       3Q      Max  
    ## -1.4406  -1.3058   0.9669   1.0308   1.6603  
    ## 
    ## Coefficients:
    ##             Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)  0.60596    0.06031  10.047  < 2e-16 ***
    ## dist1       -0.62188    0.09743  -6.383 1.74e-10 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 4118.1  on 3019  degrees of freedom
    ## Residual deviance: 4076.2  on 3018  degrees of freedom
    ## AIC: 4080.2
    ## 
    ## Number of Fisher Scoring iterations: 4

-   Para la interpretación se suele usar los *efectos marginales*.
-   Instalar el paquete *erer*
-   Correr el comando:

`ea <- maBina(w = ajuste2, x.mean = T, rev.dum = TRUE) ea$out`

-   Interpretación: La probabilidad de que se cambien a un pozo seguro
    disminuye 15% en una familia que est? a una distancia de una unidad
    respecto a la distancia promedio (0.48)

<!-- -->

    library(erer)

    ## Loading required package: lmtest

    ## Loading required package: zoo

    ## 
    ## Attaching package: 'zoo'

    ## The following objects are masked from 'package:base':
    ## 
    ##     as.Date, as.Date.numeric

    ea <- maBina(w = ajuste2, x.mean = TRUE, rev.dum = TRUE)
    ea$out

    ##             effect error t.value p.value
    ## (Intercept)  0.148 0.014  10.404       0
    ## dist1       -0.152 0.024  -6.382       0

-   Aunque no tan exacto, una forma de obtener los efectos marginales
    (aunque sin tanta precisión), es:

`coef(ajuste2)/4` - Interpretación: La probabilidad de que se cambien a
un pozo seguro disminuye 15% en una familia que está a una distancia de
una unidad respecto a la distancia promedio (0.48)

    coef(ajuste2)/4

    ## (Intercept)       dist1 
    ##   0.1514898  -0.1554705

-   Ajuste un nuevo modelo incluyendo la variable *arsenic*
-   Calcule los efectos marginales
-   Interpretación:
    -   cambio disminuye 22% para una casa que está a una unidad
        adicional de la distancia promedio.
    -   a una distancia fija, comparando un pozo de contenido de
        arsénico promedio más una unidad, la probabilidad aumenta un 11%

<!-- -->

    ajuste3 <- glm(Switch~dist1+arsenic,family=binomial(link="logit"), x = T)
    summary(ajuste3)

    ## 
    ## Call:
    ## glm(formula = Switch ~ dist1 + arsenic, family = binomial(link = "logit"), 
    ##     x = T)
    ## 
    ## Deviance Residuals: 
    ##     Min       1Q   Median       3Q      Max  
    ## -2.6351  -1.2139   0.7786   1.0702   1.7085  
    ## 
    ## Coefficients:
    ##              Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)  0.002749   0.079448   0.035    0.972    
    ## dist1       -0.896644   0.104347  -8.593   <2e-16 ***
    ## arsenic      0.460775   0.041385  11.134   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 4118.1  on 3019  degrees of freedom
    ## Residual deviance: 3930.7  on 3017  degrees of freedom
    ## AIC: 3936.7
    ## 
    ## Number of Fisher Scoring iterations: 4

    ea <- maBina(w = ajuste3, x.mean = TRUE, rev.dum = TRUE)
    ea$out

    ##             effect error t.value p.value
    ## (Intercept)  0.001 0.019   0.035   0.972
    ## dist1       -0.218 0.025  -8.598   0.000
    ## arsenic      0.112 0.010  11.217   0.000

¿Cual de las dos variables es más importante en la decisión de cambio?

-   Se debe calcular los coeficientes estandarizados.

<!-- -->

    #este es el coeficiente estandarizado de la distancia
    d=sd(dist1)*(-0.896644) 
    # este es el coeficiente estandarizado del ars?nico
    a=sd(arsenic)*(0.460775) 
    abs(a);abs(d)

    ## [1] 0.5102563

    ## [1] 0.3450167

    # de modo que el arsénico es mas importante, 
    # pero para decirlo por probabilidades:

#### Ejemplo 2

Abra la tabla 15.7 - Los datos son el efecto del Sistema de Ense?anza
Personalizada (PSI) sobre las calificaciones. - Ajuste el siguiente
modelo:
`ajuste1 <- glm(GRADE~GPA+TUCE+PSI, family=binomial(link="logit"),x=T)`
- Interprete el modelo

En los modelos cuya variable regresada binaria, la bondad del ajuste
tiene una importancia secundaria. Lo que interesa son los signos
esperados de los coeficientes de la regresión y su importancia práctica
y/o estadística

Probit
======

Tobit
=====

Referencias
===========
