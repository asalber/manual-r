# Estructuras de control

Como en otros lenguajes de programación, en R existen instrucciones para controlar el flujo de ejecución de un programa. Básicamente existen dos tipos: 

- Condicionales: Son instrucciones que bifurcan el flujo del programa en función de si se cumple o no una condición.
- Bucles: Son instrucciones que repiten un bloque de código un numero determinado de veces o hasta que se cumple una condición. 

## Estructuras condicionales

Las estructuras condicionales permiten evaluar el estado del programa y tomar decisiones sobre qué código ejecutar en función del mismo.

### Condicionales (`if`)

La principal estructura condicional comienza con la palabra reservada `if`, lleva asociada expresión de tipo lógico o booleano y permite ejecutar un bloque de código dependiendo de si la evaluación de esa expresión es `TRUE` o `FALSE`.

> `if (`_`<exp>`_`) {`  
&ensp;&ensp;_`<código>`_  
`}`

Si el resultado de evaluar la expresión `<exp>` es `TRUE` entonces se ejecuta el código `<código>`, mientras que si es `FALSE` no.


<div class="figure" style="text-align: center">
<img src="img/condicional-simple.png" alt="Diagrama de flujo de la estructura condicional simple" width="70%" />
<p class="caption">(\#fig:condicional-simple)Diagrama de flujo de la estructura condicional simple</p>
</div>

::: {.example}
A continuación se muestra un ejemplo de estructura condicional con `if`.

```r
x <- 1
y <- 0
if (y != 0){
  print(x / y)
}
```
:::

Si se desea ejecutar un bloque de código alternativo cuando no se cumpla la condición se puede añadir a continuación con la palabra reservada `else`.

> `if (`_`<exp>`_`) {`  
&ensp;&ensp;_`<código 1>`_  
`} else {`  
&ensp;&ensp;_`<código 2>`_  
`}`

En este caso, si la evaluación de la condición es `TRUE` se ejecuta el código `<código 1>` y si es `FALSE` se ejecuta el código `<código 2>`.

<div class="figure" style="text-align: center">
<img src="img/condicional-doble.png" alt="Diagrama de flujo de la estructura condicional doble" width="100%" />
<p class="caption">(\#fig:condicional-doble)Diagrama de flujo de la estructura condicional doble</p>
</div>

::: {.example}
A continuación se muestra un ejemplo de estructura condicional con `if` y `else`.

```r
nota <- 8.5
if (nota < 5){
  print("Suspenso")
} else {
  print("Aprobado")
}
#> [1] "Aprobado"
```
:::

Se puede comprobar más de una condición encadenando otra instrucción `if` tras las instrucción `else`. 

> `if (`_`<exp 1>`_`) {`  
&ensp;&ensp;_`<código 1>`_  
`} else if (`_`<exp 2>`_`) {`  
&ensp;&ensp;_`<código 2>`_`) {`  
...  
`} else {`  
&ensp;&ensp;_`<código n>`_  
`}`

Cuando se encadenan múltiples condiciones de esta forma, solamente se ejecuta el bloque de código asociado a la primera condición cuya evaluación sea `TRUE`. El último bloque de código solamente se ejecuta si todas las condiciones son falsas.

<div class="figure" style="text-align: center">
<img src="img/condicional-multiple.png" alt="Diagrama de flujo de la estructura condicional múltiple" width="70%" />
<p class="caption">(\#fig:condicional-multiple)Diagrama de flujo de la estructura condicional múltiple</p>
</div>

::: {.example}
A continuación se muestra un ejemplo de estructura condicional múltiple.

```r
nota <- 8.5
if (nota < 5){
  print("Suspenso")
} else if (nota < 7) {
  print("Aprobado")
} else if (nota < 9) {
  print("Notable")
} else {
  print("Sobresaliente")
}
#> [1] "Notable"
```
:::


### La función `switch()`

Otra forma de tomar decisiones sobre el código a ejecutar es la función `switch`. 

- `switch(x, l)`: Ejecuta el código del valor de la lista `l` cuyo nombre asociado coincide con el resultado de evaluar la expresión `x`. Si el resultado de evaluar `x` no es ningún nombre de los elementos de la lista devuelve `NULL`.

::: {.example}
A continuación se muestra un ejemplo de uso de la función `switch`.

```r
tipo.iva <- "reducido"
precio <- 1000
iva <- precio * switch(tipo.iva, "superreducido" = 4, "reducido" = 10, "normal" = 21) / 100
iva
#> [1] 100
```
:::


## Bucles

Un bucle es una estructura que permite la repetición de un bloque de código. En R existen dos tipos de bucles, los _bucles iterativos_ y los _bucles condicionales_.

### Bucles iterativos (`for`)

Lo bucles iterativos repiten un bloque de código un número determinado de veces. Comienzan por la palabra reservada `for` y llevan asociado un _iterador_, que es una variable que recorre una secuencia de un tipo de datos compuesto, normalmente un vector o una lista. El bloque de código se ejecuta tantas veces como elementos tenga la secuencia, y en cada repetición el iterador toma como valor un elemento distinto de la secuencia.

> `for (`_`i`_` in `_`<secuencia>`_`) {`  
&ensp;&ensp;_`<código>`_  
`}`

<div class="figure" style="text-align: center">
<img src="img/bucle-for.png" alt="Diagrama de flujo de un bucle iterativo" width="60%" />
<p class="caption">(\#fig:bucle-for)Diagrama de flujo de un bucle iterativo</p>
</div>

::: {.example}
A continuación se muestra varios ejemplos de uso del bucle `for`.

```r
asignaturas <- c("Matemáticas", "Física", "Programación")
for (i in asignaturas) {
  print(i)
}
#> [1] "Matemáticas"
#> [1] "Física"
#> [1] "Programación"
for (i in 1:5) {
  print(paste("El cuadrado de ", i, " es ", i^2))
}
#> [1] "El cuadrado de  1  es  1"
#> [1] "El cuadrado de  2  es  4"
#> [1] "El cuadrado de  3  es  9"
#> [1] "El cuadrado de  4  es  16"
#> [1] "El cuadrado de  5  es  25"
```
:::

También es posible recorrer los elementos de la secuencia por posición ayudándonos de la siguiente función:

- `seq_along(x)`: que devuelve un vector con los enteros desde 1 hasta el número de elementos de la secuencia `x`.

::: {.example}
A continuación se muestra un ejemplo de bucle `for` que recorre los elementos de un vector por posición.

```r
asignaturas <- c("Matemáticas", "Física", "Programación")
for (i in seq_along(asignaturas)){
  print(paste("Asignatura ", i, ":", asignaturas[i]))
}
#> [1] "Asignatura  1 : Matemáticas"
#> [1] "Asignatura  2 : Física"
#> [1] "Asignatura  3 : Programación"
```
:::

Los bucles iterativos se utilizan habitualmente para recorrer estructuras de una dimensión como los vectores y las listas, donde se sabe de antemano el número de elementos que contiene y, por tanto, el número de iteraciones del bucle. No obstante, también se pueden recorrer estructuras de más de una dimensión, como por ejemplo matrices, utilizando varios bucles `for` anidados.

::: {.example}
A continuación se muestra varios ejemplos de dos bucles `for` anidados para recorrer los elementos de una matriz.

```r
x <- matrix(1:6, 2, 3)
for (i in 1:nrow(x)) {
  for (j in 1:ncol(x)){
    print(x[i,j])
  }
}
#> [1] 1
#> [1] 3
#> [1] 5
#> [1] 2
#> [1] 4
#> [1] 6
```
:::

### Bucles condicionales `while`

Los bucles condicionales repiten un bloque de código mientras se cumpla una condición. Comienzan con la palabra reservada `while` y llevan asociada una expresión lógica, de manera que mientras la evaluación de la expresión lógica sea `TRUE` se repite la ejecución del bloque de código que contiene.

> `while (`_`<condición>`_`) {`  
&ensp;&ensp;_`<código>`_  
`}`

La expresión lógica `<condición>` se evalúa antes de ejecutar el bloque de código y solo se ejecuta el `<código>` si el resultado de la evaluación es `TRUE`. Obsérvese que cuando el flujo de ejecución del programa llega al bucle `while` si la condición no es cierta, el código no se ejecuta ni tan siquiera una vez.

<div class="figure" style="text-align: center">
<img src="img/bucle-while.png" alt="Diagrama de flujo de un bucle condicional" width="50%" />
<p class="caption">(\#fig:bucle-while)Diagrama de flujo de un bucle condicional</p>
</div>

::: {.example}
A continuación se muestra un ejemplo de bucle `while`.

```r
i <- 5
while (i >= 0) {
  print(i)
  i <- i - 1
}
#> [1] 5
#> [1] 4
#> [1] 3
#> [1] 2
#> [1] 1
#> [1] 0
```
:::

### La instrucción `break`

La instrucción `break` se utiliza para detener un bucle y salir de él, tanto en bucles iterativos como en bucles condicionales. Normalmente se suele utilizar esta instrucción cuando se cumple una determinada condición en bloque de código del bucle y se decide parar su ejecución y salir del bucle.

::: {.example}
A continuación se muestra un ejemplo de uso de la instrucción `break`.

```r
for (i in -2:2) {
  if (i == 0) {
    break
  } 
  print(i)
}
#> [1] -2
#> [1] -1
```
:::

### La instrucción `next`

La instrucción `next` se utiliza para interrumpir la ejecución del bloque de código de un bucle, pero en lugar de salir del bucle pasa a la siguiente iteración. Si se trata de un bucle iterativo el iterador pasa al siguiente elemento de la secuencia de iteración y si se trata de un bucle condicional se pasa evaluar de nuevo la condición de repetición. 

::: {.example}
A continuación se muestra un ejemplo de uso de la instrucción `break`.

```r
for (i in 1:10) {
  if (i %% 2) {
    next
  }
  print(i)
}
#> [1] 2
#> [1] 4
#> [1] 6
#> [1] 8
#> [1] 10
```
