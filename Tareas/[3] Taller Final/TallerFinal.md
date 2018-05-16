Este es el taller final del curso Análisis Estadístico con R. Deben
elaborar un documento en el que se refleje el código utilizado, los
resultados de los cálculos y la interpretación respectiva. Los
resultados/gráficos pueden ser capturas de pantalla. Se debe también
adjuntar el script utilizado. Los grupos pueden ser de hasta cuatro
integrantes, recuerden indicar el nombre de todos los integrantes tanto
en el documento como en el scritp.

Buena suerte (:

VMO

Regresión
=========

Se desea saber si hay diferencias salariales en el Ecuador. Realice un
resumen estadístico de las variables involucradas en los modelos. Ajusta
los siguientes modelos e interpreta los resultados:

*i**n**g**r**e**s**o* = *β*<sub>0</sub> + *β*<sub>1</sub>*s**e**x**o*

*i**n**g**r**e**s**o* = *β*<sub>0</sub> + *β*<sub>1</sub>*s**e**x**o* + *β*<sub>2</sub>*e**d**a**d* + *β*<sub>3</sub>*e**d**a**d*<sup>2</sup> + *a**r**e**a*

Nota:

-   la fuente de datos es: `per12_2010.dta`, la variable *ingreso* es
    `ingrl`, *sexo* es `p02` *edad* es `p03` y *area* es `area`
-   En la variable ingreso debes tomar únicamente los valores entre 0
    y 500000.

Modelos de probabilidad
=======================

En base a los siguientes modelos:

$$
P(ingreso&gt;=\\bar{ingreso}) = \\beta\_0+\\beta\_1sexo
$$

$$
P(ingreso&gt;=\\bar{ingreso}) = \\beta\_0+\\beta\_1sexo+\\beta\_2edad+area
$$

-   ¿cuál es la probabilidad de tener un ingreso superior al promedio en
    ecuador? Use un modelo *logit*
-   ¿Los modelos son significativos? (test de Wald)
-   Calcule los efectos marginales e interprete los resultados.

Nota:

-   Crear una variable dicotómica que sea igual a 1 si el ingreso es
    superior al promedio y 0 caso contrario. Recuerde que el ingreso con
    el que trabaja está acotado entre 0 y 500000.

Series de tiempo
================

Utilice la tabla de datos `pib_ec.csv` ubicada en `DataLectures`. Se
trata de la serie trimestral del PIB del Ecuador desde enero del 2000.

Verifique que la serie sea estacionaria. Debe *pasar* tanto la prueba DF
como PP para que la considere estacionaria.

Encuentre el mejor modelo AR, el mejor MA y luego contraste sus
resultados con un ARMA(2,2). Recuerde compruebar si los residuos de los
modelos estimados son ruido blanco.
