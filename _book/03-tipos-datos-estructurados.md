# Tipos de datos estructurados

Los tipos estructurados de datos, a diferencia de los simples, son colecciones de datos con una determinada estructura. En R existen varios tipos tipos estructurados de datos que pueden clasificarse de acuerdo a su dimensión y a si son homogéneos (todos sus elementos son del mismo tipo) o heterogéneos. 

| Dimensiones | Homogéneos | Heterogéneos |
|:-:|:-:|:-:|
| 1 | Vector | Lista |
| 2 | Matriz | Data frame |
| n | Array | |

Para averiguar la estructura de un dato estructurado se puede utilizar la función siguiente: 

- `str(x)`: Devuelve una cadena de texto con la estructura de `x` en un formato amigable para los humanos.

## Vectores

El vector es el tipo de dato estructurado más básicos en R. Un vector es una colección ordenada de elementos del mismo tipo. 


### Creación de vectores

Para construir un vector se utiliza la función de combinación `c()`:

- `c(x1, x2, ...)`: Devuelve el vector formado por los elementos `x1`, `x2`, etc.

También es posible utilizar el operador `:` para generar un vector de números enteros consecutivos:

- `x:y`: Devuelve el vector de números enteros consecutivos desde `x` hasta `y`.

::: {.example}
A continuación se muestran varios ejemplos de construcción de vectores.

```r
c(1, 2, 3)
#> [1] 1 2 3
c("uno", "dos", "tres")
#> [1] "uno"  "dos"  "tres"
# Vector vacío
c()
#> NULL
# Vector con elementos perdidos
c(1, NA, 3)
#> [1]  1 NA  3
# Vector de números enteros consecutivos del 2 al 6
2:6
#> [1] 2 3 4 5 6
```
:::

#### Vectores con nombres

Es posible asignar un nombre a cada elemento de un vector. Los nombres son etiquetas de texto que se asocian a cada elemento. Para asociar un nombre a un elemento se utiliza la sintaxis `nombre = valor`, donde `nombre` es una cadena de caracteres y `valor` es el elemento del vector.

::: {.example}
A continuación se muestra un ejemplo de creación de un vector con nombres.

```r
c("Matemáticas" = 8.2, "Física" = 6.5, "Economía" = 4.5)
#> Matemáticas      Física    Economía 
#>         8.2         6.5         4.5
```
:::

Para acceder a los nombres de un vector se utiliza la siguiente función:

- `names(x)`: Devuelve un vector de cadenas de caracteres con los nombres de los elementos del vector `x`.

::: {.example}
A continuación se muestra un ejemplo de acceso a los nombres de un vector.

```r
notas <- c("Matemáticas" = 8.2, "Física" = 6.5, "Economía" = 4.5)
names(notas)
#> [1] "Matemáticas" "Física"      "Economía"
```
:::

### Tamaño de un vector

El número de elementos de un vector es su _tamaño_ y puede averiguarse con la siguiente función.

- `lenght(x)`: Devuelve el número de elementos del vector `x`.

::: {.example}
A continuación se muestran varios ejemplos de la obtención del tamaño de un vector.

```r
length(c(1, 2, 3))
#> [1] 3
length(c())
#> [1] 0
```
:::

### Coerción de elementos

Puesto que los elementos de un vector tienen que ser del mismo tipo, cuando se crea un vector con datos de distintos tipos, la función `c()` los convertirá al mismo tipo, lo que se conoce como _coerción_ de tipos. La coerción se produce de los tipos menos flexibles a los más flexibles: `logical` < `integer` < `double` < `character`.

::: {.example}
A continuación se muestran varios ejemplos de coerciones.

```r
c(1, 2.5)
#> [1] 1.0 2.5
c(FALSE, TRUE, 2)
#> [1] 0 1 2
c(FALSE, TRUE, 2, "tres")
#> [1] "FALSE" "TRUE"  "2"     "tres"
```
:::

### Acceso a los elementos de un vector

Para acceder a los elementos de un vector se utiliza un índice. Como veremos a continuación, este índice puede ser entero, lógico o de cadena de caracteres y se indica siempre entre corchetes `[ ]` a continuación del vector. 

#### Acceso mediante un índice entero

Los elementos de un vector están ordenados y el acceso más simple a ellos es mediante su número de orden, es decir, indicando entre corchetes el entero que corresponde a su número de orden. Se puede acceder simultáneamente a varios elementos mediante un vector con sus números de orden. Por otro lado, también es posible usar enteros negativos y en tal caso se obtendrán todos los elementos del vector excepto los que ocupan las posiciones correspondientes al valor absoluto de los índices negativos. Esta es la forma más habitual de eliminar elementos de un vector.

<i class="fa fa-exclamation-triangle" style="color:red;"></i> _En R los índices enteros para acceder a los elementos de un vector comienzan en 1, a diferencia de otros lenguajes de programación que empiezan en 0._

::: {.example}
A continuación se muestran varios ejemplos de acceso a los elementos de un vector mediante índices enteros.

```r
x <- c(2,4,6,8,10)
# Acceso al elemento que está en la tercera posición
x[3]
#> [1] 6
# Acceso a los elementos de las posiciones 2 y 4
x[c(2, 4)]
#> [1] 4 8
# Acceso a todos los elementos excepto el primero y el quinto
x[c(-1, -5)]
#> [1] 4 6 8
```
:::

#### Acceso mediante un índice lógico

Cuando se utiliza un índice lógico, se obtienen los elementos correspondientes a las posiciones donde está el valor booleano `TRUE`.

::: {.example}
A continuación se muestran varios ejemplos de acceso a los elementos de un vector mediante índices lógicos.

```r
x <- c(2,4,6,8,10)
# Acceso al elemento que está en la tercera posición
x[c(F,F,T,F,F)]
#> [1] 6
# Acceso a los elementos de las posiciones 2 y 4
x[c(F,T,F,T,F)]
#> [1] 4 8
```
:::

Esta forma de acceder es útil cuando se genera el vector de índices mediante operadores relacionales. Cuando se aplica un operador relacional a un vector se obtiene otro vector lógico que resulta de aplicar el operador relacional a cada uno de los elementos del vector. De esta manera se puede realizar filtros para obtener los elementos de un vector que cumplen una determinada condición. 

::: {.example}
A continuación se muestran varios ejemplos de acceso a los elementos de un vector mediante condiciones.

```r
x <- 1:6
x %% 2 == 0
#> [1] FALSE  TRUE FALSE  TRUE FALSE  TRUE
# Filtrado de los valores pares
x[x %% 2 == 0]
#> [1] 2 4 6
# Filtrado de los valores pares menores que 5
x[x %% 2 == 0 & x < 5]
#> [1] 2 4
```
:::

#### Acceso mediante un índice de cadena

Si los elementos de un vector tienen nombre, es posible acceder a ellos usando sus nombres como índices.

::: {.example}
A continuación se muestran varios ejemplos de acceso a los elementos de un vector mediante índices de cadena.

```r
notas <- c("Matemáticas" = 8.2, "Física" = 6.5, "Economía" = 4.5)
notas["Física"]
#> Física 
#>    6.5
notas[c("Matemáticas", "Economía")]
#> Matemáticas    Economía 
#>         8.2         4.5
```
:::

### Pertenencia a un vector

Para comprobar si un valor en particular es un elemento de un vector se puede utilizar el operador `%in%`:

- `x %in% y`: Devuelve el booleano `TRUE` si `x` es un elemento del vector `y`, y `FALSE` en caso contrario.

::: {.example}
A continuación se muestran varios ejemplos de pertenencia de elementos a un vector.

```r
x <- 1:3
2 %in% x
#> [1] TRUE
4 %in% x
#> [1] FALSE
```

### Modificación de los elementos de un vector

Para modificar uno o varios elementos de un vector basta con acceder a esos elementos y usar el operador de asignación para asignar nuevos valores.

::: {.example}
A continuación se muestran varios ejemplos de modificación de los elementos de un vector.

```r
x <- c(1, 2, 3)
x[2] <- 0
x
#> [1] 1 0 3
x[c(1, 3)] <- 1
x
#> [1] 1 0 1
```
:::

### Añadir elementos a un vector 

Para añadir nuevos elementos a un vector pueden usarse las siguientes funciones:

- `c(x, y)`: Devuelve el vector que resulta de añadir al vector `x` los elementos del vector `y`.
- `append(x, y, pos)`: Devuelve el vector que resulta de añadir al vector `x` los elementos del vector `y`, a continuación de la posición `pos`. El parámetro `pos` es opcional y si no se indica, los elementos de `y` se añaden al final de los de `x`.

::: {.example}
A continuación se muestran varios ejemplos de añadir nuevos elementos a un vector.

```r
x <- c(1, 2, 3)
y <- c(x, c(4, 5))
y
#> [1] 1 2 3 4 5
y <- append(x, c(4, 5), 2)
y
#> [1] 1 2 4 5 3
```
:::

### Eliminación de un vector 

Para eliminar los elementos de un vector basta con asignar `NULL` a la variable que lo contiene, pero si se quiere liberar la memoria que ocupa la variable se utiliza la función `rm()`.

## Listas

Las listas son colecciones ordenadas de elementos de que pueden ser de distintos tipos. Los elementos de una lista también pueden ser de tipos estructurados (vectores o listas), lo que las convierte en el tipo de dato más versátil de R. Como veremos más adelante, otras estructuras de datos como los _data frames_ o los propios modelos estadísticos se construyen usando listas.

### Creación de listas

Para construir una lista se utiliza la función `list()`:

- `list(x1, x2, ...)`: Devuelve la lista con los elementos `x1`, `x2`, etc.

::: {.example}
A continuación se muestran varios ejemplos de creación de listas.

```r
list(1, "dos", TRUE)
#> [[1]]
#> [1] 1
#> 
#> [[2]]
#> [1] "dos"
#> 
#> [[3]]
#> [1] TRUE
# Lista con vectores y listas
x <- list(1, c("dos", "tres"), list(4, "cinco"))
x
#> [[1]]
#> [1] 1
#> 
#> [[2]]
#> [1] "dos"  "tres"
#> 
#> [[3]]
#> [[3]][[1]]
#> [1] 4
#> 
#> [[3]][[2]]
#> [1] "cinco"
str(x)
#> List of 3
#>  $ : num 1
#>  $ : chr [1:2] "dos" "tres"
#>  $ :List of 2
#>   ..$ : num 4
#>   ..$ : chr "cinco"
# Lista vacía
list()
#> list()
```
:::

#### Listas con nombres

::: {.example}
A continuación se muestra un ejemplo de creación de una lista con nombres.

```r
list("nombre" = "María", "edad" = 21, "dirección" = list("calle" = "Delicias", "número" = 24, "municipio" = "Madrid"))
#> $nombre
#> [1] "María"
#> 
#> $edad
#> [1] 21
#> 
#> $dirección
#> $dirección$calle
#> [1] "Delicias"
#> 
#> $dirección$número
#> [1] 24
#> 
#> $dirección$municipio
#> [1] "Madrid"
```
:::

Para obtener los nombres de una lista se utiliza la siguiente función:

- `names(x)`: Devuelve un vector de cadenas de caracteres con los nombres de los elementos de la lista `x`.

::: {.example}
A continuación se muestra un ejemplo de acceso a los nombres de una lista.

```r
persona <- list("nombre" = "María", "edad" = 21, "dirección" = list("calle" = "Delicias", "número" = 24, "municipio" = "Madrid"))
names(persona)
#> [1] "nombre"    "edad"      "dirección"
```
:::

### Tamaño de una lista

El número de elementos de una lista es su _tamaño_ y puede averiguarse con la siguiente función:

- `lenght(x)`: Devuelve el número de elementos de la lista `x`.

::: {.example}
A continuación se muestran varios ejemplos la obtención del tamaño de una lista.

```r
length(list(1, "dos", TRUE))
#> [1] 3
length(list(1, c("dos", "tres"), list(4, "cinco")))
#> [1] 3
length(list())
#> [1] 0
```
:::

### Acceso a los elementos de una lista

Se accede a los elementos de una lista de forma similar a los vectores, mediante índices enteros, lógicos o de cadena, entre corchetes `[ ]`.

#### Acceso mediante un índice entero

Al igual que los vectores, los elementos de una lista están ordenados y se puede utilizar un índice entero para acceder a los elementos que ocupan una determinada posición.

::: {.example}
A continuación se muestran varios ejemplos de acceso a los elementos de una lista mediante índices enteros.

```r
x <- list(1, "dos", TRUE, 4.5)
# Acceso al elemento que está en la segunda posición
x[2]
#> [[1]]
#> [1] "dos"
# Acceso a los elementos de las posiciones 1 y 3
x[c(1, 3)]
#> [[1]]
#> [1] 1
#> 
#> [[2]]
#> [1] TRUE
# Acceso a todos los elementos excepto el primero y el cuarto
x[c(-1, -4)]
#> [[1]]
#> [1] "dos"
#> 
#> [[2]]
#> [1] TRUE
```
:::

#### Acceso mediante un índice lógico

Cuando se utiliza un índice lógico, se obtienen los elementos correspondientes a las posiciones donde está el valor booleano `TRUE`.

::: {.example}
A continuación se muestran varios ejemplos de acceso a los elementos de una lista mediante índices lógicos.

```r
x <- list(1, "dos", TRUE, 4.5)
x[c(T,F,F,T)]
#> [[1]]
#> [1] 1
#> 
#> [[2]]
#> [1] 4.5
x < 2
#> Warning: NAs introducidos por coerción
#> [1]  TRUE    NA  TRUE FALSE
# Filtrado de valores menores que 2
x[x < 2]
#> Warning: NAs introducidos por coerción
#> [[1]]
#> [1] 1
#> 
#> [[2]]
#> NULL
#> 
#> [[3]]
#> [1] TRUE
```
Obsérvese que para los elementos que no tiene sentido la comparación se obtiene `NA`, y que el acceso mediante este índice devuelve `NULL`.
:::

#### Acceso mediante un índice de cadena

Si los elementos de una lista tienen nombre, se puede acceder a ellos utilizando sus nombres como índices. La única diferencia con el acceso mediante cadenas de vectores es que se obtiene siempre una lista, incluso cuando sólo se quiere acceder a un elemento. Para obtener un elemento, y no una lista con ese único elemento, se utilizan dobles corchetes `[[ ]]`.

::: {.example}
A continuación se muestran varios ejemplos de acceso a los elementos de una lista mediante índices de cadena.

```r
persona <- list("nombre" = "María", "edad" = 21, "dirección" = list("calle" = "Delicias", "número" = 24, "municipio" = "Madrid"))
persona[c("edad", "nombre")]
#> $edad
#> [1] 21
#> 
#> $nombre
#> [1] "María"
persona["nombre"]
#> $nombre
#> [1] "María"
typeof(persona["nombre"])
#> [1] "list"
# Acceso a un único elemento
persona[["nombre"]]
#> [1] "María"
# Acceso a una lista anidada
persona[["dirección"]][["municipio"]]
#> [1] "Madrid"
```
:::

Una alternativa a los dobles corchetes es el operador de acceso a listas `$`. Este operador además permite utilizar coincidencias parciales en los nombres de los elementos para acceder a ellos.

::: {.example}
A continuación se muestran varios ejemplos de acceso a los elementos de una lista mediante el operador `$`.

```r
persona <- list("nombre" = "María", "edad" = 21, "dirección" = list("calle" = "Delicias", "número" = 24, "municipio" = "Madrid"))
# Acceso a un único elemento
persona$nombre
#> [1] "María"
# Acceso mediante coincidencia parcial
persona$nom
#> [1] "María"
# Acceso a una lista anidada
persona$dirección$municipio
#> [1] "Madrid"
```
:::

### Modificación de los elementos de una lista

Para modificar uno o varios elementos de una lista basta con acceder a esos elementos y reasignarles valors con el operador de asignación.

::: {.example}
A continuación se muestran varios ejemplos de modificación de los elementos de una lista.

```r
persona <- list("nombre" = "María", "edad" = 21)
persona$edad <- 22
persona
#> $nombre
#> [1] "María"
#> 
#> $edad
#> [1] 22
```
:::

### Añadir elementos a una lista 

La forma más sencilla de añadir un elemento con nombre a una lista es indicando el nombre con el operador `$` y asignándole un valor con el operador de asignación `<-`:

- `x$nombre <- y`: Añade el elemento `y` a la lista `x` con el nombre `nombre`. 

El nuevo elemento se añade siempre al final de la lista.

Para añadir elementos sin nombre o en una posición determinada se puede utilizar la función `append()`:

- `append(x, y, pos)`: Devuelve la lista vector que resulta de añadir a `x` los elementos de la lista `y`, a continuación de la posición `pos`. El parámetro `pos` es opcional y si no se indica, los elementos de `y` se añaden al final de los de `x`.

::: {.example}
A continuación se muestran varios ejemplos de añadir nuevos elementos a una lista.

```r
persona <- list("nombre" = "María", "edad" = 21)
persona$email <- "maria@ceu.es"
persona
#> $nombre
#> [1] "María"
#> 
#> $edad
#> [1] 21
#> 
#> $email
#> [1] "maria@ceu.es"
append(persona, list("sexo" = "Mujer"), 2)
#> $nombre
#> [1] "María"
#> 
#> $edad
#> [1] 21
#> 
#> $sexo
#> [1] "Mujer"
#> 
#> $email
#> [1] "maria@ceu.es"
```
:::

### Conversión de una lista en un vector

Es posible convertir una lista en un vector con la siguiente función:

- `unlist(x)`: Devuelve el vector que resulta de aplanar recursivamente la lista `x` y convertir todos los elementos al mismo tipo mediante coerción de tipos.

::: {.example}
A continuación se muestran varios ejemplos de conversión de una lista en un vector.

```r
persona <- list("nombre" = "María", "edad" = 21, "dirección" = list("calle" = "Delicias", "número" = 24, "municipio" = "Madrid"))
unlist(persona)
#>              nombre                edad     dirección.calle 
#>             "María"                "21"          "Delicias" 
#>    dirección.número dirección.municipio 
#>                "24"            "Madrid"
typeof(unlist(persona))
#> [1] "character"
```
:::


## Matrices

Una matriz es una estructura de datos bidimensional de elementos del mismo tipo organizados en filas y columnas. Una matriz es similar a un vector pero contiene una atributo adicional con sus dimensiones (número de filas y número de columnas).

### Creación de matrices

Para crear una matriz se utiliza la siguiente función:

- `matrix(x, nrow = m, ncol = n)`: Devuelve la matriz con los elementos del vector `x` organizados en `n` filas y `m` columnas. Habitualmente basta con especificar el número de filas o el número de columnas.

::: {.example}
A continuación se muestran varios ejemplos de creación de matrices.

```r
matrix(1:6, nrow = 2, ncol = 3)
#>      [,1] [,2] [,3]
#> [1,]    1    3    5
#> [2,]    2    4    6
matrix(1:6, nrow = 2)
#>      [,1] [,2] [,3]
#> [1,]    1    3    5
#> [2,]    2    4    6
matrix(1:6, ncol = 3)
#>      [,1] [,2] [,3]
#> [1,]    1    3    5
#> [2,]    2    4    6
# La matriz de 1 x 1 
matrix()
#>      [,1]
#> [1,]   NA
```
:::

Como se puede observar en el ejemplo anterior, los elementos se disponen por columnas, pero se pueden disponer los elementos por filas pasando el parámetro `byrow = TRUE` a la función `matrix`.

::: {.example}
A continuación se muestran varios ejemplos de creación de matrices.

```r
matrix(1:6, nrow = 2)
#>      [,1] [,2] [,3]
#> [1,]    1    3    5
#> [2,]    2    4    6
matrix(1:6, nrow = 2, byrow = TRUE)
#>      [,1] [,2] [,3]
#> [1,]    1    2    3
#> [2,]    4    5    6
```
:::

#### Matrices con nombres de filas y columnas

Es posible poner nombres a las filas y a las columnas de una matriz añadiendo el parámetro `dimnames` y pasándole una lista de dos vectores de cadenas con los nombres de las filas y las columnas respectivamente.

::: {.example}
A continuación se muestran varios ejemplos de creación de matrices con nombres de filas y columnas.

```r
matrix(1:6, nrow = 2, ncol = 3, dimnames = list(c("fila1", "fila2"), c("columna1", "columna2", "columna3")))
#>       columna1 columna2 columna3
#> fila1        1        3        5
#> fila2        2        4        6
```
:::

Para obtener los nombres de las filas y las columnas de una matriz se utilizan las siguientes funciones:

- `rownames(x)`: Devuelve un vector de cadenas de caracteres con los nombres de las filas de la matriz `x`.
- `colnames(x)`: Devuelve un vector de cadenas de caracteres con los nombres de las columnas de la matriz `x`.

::: {.example}
A continuación se muestran varios ejemplos de creación de matrices con nombres de filas y columnas.

```r
x <- matrix(1:6, nrow = 2, ncol = 3, dimnames = list(c("fila1", "fila2"), c("columna1", "columna2", "columna3")))
rownames(x)
#> [1] "fila1" "fila2"
colnames(x)
#> [1] "columna1" "columna2" "columna3"
```
:::

### Tamaño y dimensiones de una matriz

Para obtener el número de elementos y las dimensiones de una matriz se pueden utilizar las siguientes funciones:

- `length(x)`: Devuelve un entero con el número de elementos de la matriz `x`.
- `nrow(x)`: Devuelve un entero con el número de filas de la matriz `x`.
- `ncol(x)`: Devuelve un entero con el número de columnas de la matriz `x`.
- `dim(x)`: Devuelve un vector de dos enteros con el número de filas y el número de columnas de la matriz `x`.

::: {.example}
A continuación se muestran varios ejemplos de acceso a las dimensiones de una matriz.

```r
x <- matrix(1:6, nrow = 2)
length(x)
#> [1] 6
nrow(x)
#> [1] 2
ncol(x)
#> [1] 3
dim(x)
#> [1] 2 3
```
:::

Usando esta última función se pueden modificar las dimensiones de una matriz asignando un vector de dos enteros con las nuevas dimensiones. Esto también permite crear una matriz a partir de un vector.

::: {.example}
A continuación se muestran varios ejemplos de modificación de las dimensiones de una matriz.

```r
x <- 1:6
dim(x) <- c(2, 3)
x
#>      [,1] [,2] [,3]
#> [1,]    1    3    5
#> [2,]    2    4    6
dim(x) <- c(3, 2)
x
#>      [,1] [,2]
#> [1,]    1    4
#> [2,]    2    5
#> [3,]    3    6
```
:::

### Acceso a los elementos de una matriz

Para acceder a los elementos de una matriz se utilizan dos índices (uno para las filas y otro para las columnas), separados por comas y entre corchetes `[]` a continuación de la matriz. Al igual que para los vectores, los índices pueden ser enteros, lógicos o de cadenas de caracteres.

#### Acceso mediante índices enteros

Para acceder a los elementos de una matriz mediante índices enteros se indica el número de fila y el número de columna del elemento entre corchetes:

- `x[i,j]`: Devuelve el elemento de la matriz `x` que está en la fila `i` y la columna `j`.

Se puede acceder a más de un elemento indicando un vector de enteros para las filas y otro para las columnas. De esta manera se obtiene una submatriz. Si no se indica la fila o la columna se obtienen todos los elementos de todas las filas o columnas. Al igual que para vectores, se pueden utilizar enteros negativos para descartar filas o columnas

::: {.example}
A continuación se muestran varios ejemplos de acceso a los elementos de una matriz.

```r
x <- matrix(1:9, nrow = 3)
x
#>      [,1] [,2] [,3]
#> [1,]    1    4    7
#> [2,]    2    5    8
#> [3,]    3    6    9
# Acceso al elemento de la segunda fila y tercera columna
x[2,3]
#> [1] 8
# Acceso a la submatriz de la primera y tercera filas, y tercera y segunda columnas
x[c(1, 3), c(3, 2)]
#>      [,1] [,2]
#> [1,]    7    4
#> [2,]    9    6
# Acceso a la primera fila
x[1, ]
#> [1] 1 4 7
# Acceso a la segunda columna
x[, 2]
#> [1] 4 5 6
# Acceso a la submatriz con todos los elementos salvo la tercera fila y la segunda columna
x[-3, -2]
#>      [,1] [,2]
#> [1,]    1    7
#> [2,]    2    8
```
:::

#### Acceso mediante índices lógicos

Cuando se utilizan índices lógicos, se obtienen los elementos correspondientes a las filas y columnas donde está el valor booleano `TRUE`.

::: {.example}
A continuación se muestran varios ejemplos de acceso a los elementos de una matriz.

```r
x <- matrix(1:9, nrow = 3)
x
#>      [,1] [,2] [,3]
#> [1,]    1    4    7
#> [2,]    2    5    8
#> [3,]    3    6    9
# Acceso al elemento de la segunda fila y tercera columna
x[c(F, T, F), c(F, F, T)]
#> [1] 8
# Acceso a la submatriz de la primera y tercera filas, y segunda y tercera columnas
x[c(T, F, T), c(F, T, T)]
#>      [,1] [,2]
#> [1,]    4    7
#> [2,]    6    9
# Acceso a la primera fila
x[c(T, F, F), ]
#> [1] 1 4 7
# Acceso a la segunda columna
x[, c(F, T, F)]
#> [1] 4 5 6
```
:::

#### Acceso mediante índices de cadena

Si las filas y las columnas de una matriz tienen nombre, es posible acceder a sus elementos usando los nombres de las filas y columnas como índices.


```r
x <- matrix(1:9, nrow = 3, dimnames = list(c("f1", "f2", "f3"), c("c1", "c2", "c3")))
x
#>    c1 c2 c3
#> f1  1  4  7
#> f2  2  5  8
#> f3  3  6  9
# Acceso al elemento de la segunda fila y tercera columna
x["f2", "c3"]
#> [1] 8
# Acceso a la submatriz de la primera y tercera filas, y tercera y segunda columnas
x[c("f1", "f3"), c("c3", "c2")]
#>    c3 c2
#> f1  7  4
#> f3  9  6
```
:::

Finalmente, es posible combinar distintos tipos de índices (enteros, lógicos o de cadena) para indicar las filas y las columnas a las que acceder.


## Data frames


