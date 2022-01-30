# Funciones

Una función es un bloque de código que tiene asociado un nombre, de manera que cada vez que se quiera ejecutar el bloque de código basta con invocar el nombre de la función. Las funciones permite dividir el código en unidades lógicas que resultan más fáciles de manejar y mantener.

En R las funciones son objetos en sí mimas y pueden usarse como cualquier otro dato. El tipo de dato de las funciones es `function`.

## Definición y llamada a funciones

Para definir una función se utiliza la siguiente estructura de código:

    nombre.funcion <- function (parámetros) {
      código
    }

El código que va entre llaves se conoce como _cuerpo de la función_.

Para llamar a la función y que se ejecute el código de su cuerpo hay que utilizar el nombre de la función y a continuación los valores pasados a sus parámetros entre paréntesis.

::: {.example}
A continuación se muestra un ejemplo de creación y llamada a una función.

```r
# Definición de la función
saludo <- function() {
  print("¡Hola!")
}
class(saludo)
#> [1] "function"
# Llamada a la función
saludo()
#> [1] "¡Hola!"
```
:::

## Parámetros y argumentos de una función

Una función puede recibir valores cuando se invoca a través de unas variables conocidas como _parámetros_ que se definen entre paréntesis en la declaración de la función. En el cuerpo de la función se pueden usar estos parámetros como si fuesen variables.

Los valores que se pasan a la función en una llamada o invocación concreta de ella se conocen como _argumentos_ y se asocian a los parámetros de la declaración de la función.

::: {.example}
A continuación se muestra un ejemplo de una función con parámetros.

```r
# Función con un parámetro
saludo <- function(nombre) {
  print(paste("¡Hola ", nombre, "!", sep = ""))
}
# Llamada a la función con un argumento
saludo("Alf")
#> [1] "¡Hola Alf!"
```
En este ejemplo la función `saludo` tiene un parámetro `nombre`. En la llamada a la función se pasa la cadena `Alf` como argumento que se asocia al parámetro `nombre` en el cuerpo de la función. 
:::

### Paso de argumentos a una función

Los argumentos de una función pueden pasarse de dos formas: 

- **Argumentos posicionales**: Se asocian a los parámetros de la función en el mismo orden que aparecen en la definición de la función.
- **Argumentos nominales**: Se indica explícitamente el nombre del parámetro al que se asocia un argumento de la forma `parametro = argumento`. En este caso el orden de los argumentos no importa.

::: {.example}
A continuación se muestran varios ejemplos de pasos de argumentos posicionales y nominales.

```r
# Función con un argumento por defecto
area.triangulo <- function(base, altura) {
  base * altura / 2
}
# Cálculo del área de un triángulo de base 4 y altura 3
# Paso de argumentos por posición. 
area.triangulo(4, 3)
#> [1] 6
# Paso de argumentos por nombre
area.triangulo(altura = 3, base = 4)
#> [1] 6
```
:::

### Argumentos por defecto 

En la definición de una función se puede asignar a cada parámetro un argumento por defecto, de manera que si se invoca la función sin proporcionar ningún argumento para ese parámetro, se utiliza el argumento por defecto.

::: {.example}
A continuación se muestra un ejemplo de definición de una función con un argumento por defecto.

```r
saludo <- function(nombre, lenguaje = "R") {
  print(paste("¡Hola ", nombre, "! ¡Bienvenido a ", lenguaje, "!", sep = ""))
}
# Llamada a la función con un argumento
saludo("Alf")
#> [1] "¡Hola Alf! ¡Bienvenido a R!"
```
:::

## Retorno de una función

Una función puede devolver un objeto de cualquier tipo tras su invocación. Para ello se utiliza la función `return()`, indicando entre paréntesis el valor que devuelve la función. El retorno suele realizarse al final del cuerpo de la función, porque con él finaliza la ejecución de la función y se devuelve el control de la ejecución al punto desde donde se llamó a la función, de manera que cualquier instrucción de cuerpo que vaya después no se ejecutará. Si no se indica ningún objeto, la función devolverá el valor de la última expresión calculada en el cuerpo de la función. 

::: {.example}
A continuación se muestran varios ejemplos de retornos de funciones.

```r
# Función que devuelve el area de un triángulo
area.triangulo <- function(base, altura) {
  return(base * altura / 2)
}
area.triangulo(4, 3)
#> [1] 6
# Función que devuelve el valor absoluto de un número
valor.absoluto <- function(x) {
  if (x < 0)
    return(x * -1)
  else
    return(x)
}
valor.absoluto(-1)
#> [1] 1
valor.absoluto(2)
#> [1] 2
```
:::

Para devolver más de un valor se pueden utilizar estructuras de datos como vectores, listas, matrices o data frames.

::: {.example}
A continuación se muestra un ejemplo de una función de devuelve una lista.

```r
circulo <- function(radio) {
  return(list(perimetro = 2 * pi * radio, area = pi * radio ^ 2))
}
circulo(5)
#> $perimetro
#> [1] 31.41593
#> 
#> $area
#> [1] 78.53982
circulo(5)$perimetro
#> [1] 31.41593
circulo(5)$area
#> [1] 78.53982
```
:::

## Entorno y ámbito de las variables

El entorno de un programa en R es el conjunto de todos los objetos (funciones, variables, etc.) creados durante la ejecución del programa. Cuando se ejecuta el interprete de R siempre se crea un primer entorno `R_GlobalEnv` conocido como entorno global. Es posible referirse a él en cualquier momento con la constante `.GlobalEnv`.

Para ver el entorno activo en cada momento de la ejecución y el contenido del mismo se utiliza la siguiente función:

- `environment()`: Devuelve el nombre del entorno actual. 
- `ls()`: Devuelve un vector con los nombres de las objetos (variables, funciones, etc.) que contiene el entorno global.



::: {.example}
A continuación se muestra un ejemplo acceso al entorno global de un programa.

```r
x <- 4
y <- 3
area.triangulo <- function(base, altura) {
  base * altura / 2
}
environment()
#> <environment: R_GlobalEnv>
ls()
#> [1] "area.triangulo" "x"              "y"
```
:::

Como se puede observar en el ejemplo anterior, los parámetros de la función `base` y `altura` no aparecen en el entorno global. En R, cuando se ejecuta una función se crea un nuevo entorno hijo dentro del entorno al que pertenece la función. Durante la ejecución de la función este pasa a ser el entorno activo y cuando termina la ejecución de la función deja de serlo y vuelve a activarse el entorno padre desde donde se llamó a la función.

::: {.example}
A continuación se muestra un ejemplo de activación del entorno de una función.

```r
x <- 4
y <- 3
area.triangulo <- function(base, altura) {
  print("Entorno de la función area.triangulo") 
  print(environment())
  print(ls())
  return(base * altura / 2)
}
print("Entorno fuera de la función")
#> [1] "Entorno fuera de la función"
environment()
#> <environment: R_GlobalEnv>
ls()
#> [1] "area.triangulo" "x"              "y"
area.triangulo(x, y)
#> [1] "Entorno de la función area.triangulo"
#> <environment: 0x55e174cbaf30>
#> [1] "altura" "base"
#> [1] 6
```
:::

Los parámetros y los objetos (funciones, variables, etc.) definidos dentro de una función son de _ámbito local_, mientras que los objetos definidos fuera de ella en alguno de los entornos ancestros son de _ámbito global_.

Tanto los parámetros como las variables del ámbito local de una función sólo están accesibles durante la ejecución de la función, es decir, cuando termina la ejecución de la función estas variables desaparecen y no son accesibles desde fuera de la función.

Cuando una función declara un objeto (función, variable, etc.) que ya existe en alguno de los entornos ancestros con ámbito global, durante la ejecución de la función el objeto global queda eclipsado por el local y no es accesible hasta que finaliza la ejecución de la función.

::: {.example}
A continuación se muestra un ejemplo de eclipse de una variable de ámbito global por otra de ámbito local.

```r
lenguaje = "Python"
saludo <- function(lenguaje) {
  print(paste("Bienvenido a", lenguaje))  
}
saludo("R")
#> [1] "Bienvenido a R"
```
Obsérvese cómo al ejecutar la función anterior, la variable `lenguaje` queda inaccesible al tener la función un parámetro con el mismo nombre. 
:::

Las variables globales están accesibles siempre que no sean eclipsadas por otras con el mismo nombre de ámbito local. Si embargo, cuando se intenta asignar un valor a una variable global en el ámbito local, se crea una nueva variable local. Para asignar valores a variables globales en el ámbito local se tiene que utilizar el operador de superasignación `<<-`. Cuando se utiliza este operador para asignar un valor a una variable, R busca la variable entorno padre, y si no existe continua con la búsqueda en los entornos ancestros hasta llegar a entorno global. Si la búsqueda tiene éxito, asigna el nuevo valor a la variable global, mientras que si no tiene éxito se crea una nueva variable de ámbito local y se le asigna el valor.

::: {.example}
A continuación se muestra un ejemplo del uso del operador de superasignación.

```r
saludo <- function() {
  lenguaje <<- "R"
  return(paste("Bienvenido a", lenguaje))
}
lenguaje
#> [1] "Python"
```
:::

## Componentes de una función

Los tres componentes de una función son:

- **Cuerpo**: Es el código dentro de la función.
- **Parámetros**: Es la lista de parámetros que requiere la función.
- **Entorno**: Es donde se ubican las variables de la función.

Para acceder a estos componentes se pueden utilizar las siguientes funciones:

- `body(f)`: Devuelve el cuerpo de la función `f`.
- `formals(f)`: Devuelve la lista de parámetros de la función `f`.
- `environment(f)`: Devuelve el entorno de la función `f`.

::: {.example}
A continuación se muestra un ejemplo de acceso a los componentes de una función.

```r
# Definición de la función
area.triangulo <- function(base, altura) {
  base * altura / 2
}
body(area.triangulo)
#> {
#>     base * altura/2
#> }
formals(area.triangulo)
#> $base
#> 
#> 
#> $altura
environment(saludo)
#> <environment: R_GlobalEnv>
```
:::

## Funciones recursivas

Una función recursiva es una función que en su cuerpo contiene una llama a sí misma.

La recursión es una práctica común en la mayoría de los lenguajes de programación ya que permite resolver las tareas recursivas de manera más natural.

Para garantizar el final de una función recursiva, las sucesivas llamadas tienen que reducir el grado de complejidad del problema, hasta que este pueda resolverse directamente sin necesidad de volver a llamar a la función. De lo contrario la recursión no tendría fin y nunca terminaría la ejecución de la función.

::: {.example}
A continuación se muestra un ejemplo de una función recursiva.

```r
factorial <- function(n) {
  if (n <= 1) return(n)
  else return(n * factorial(n - 1))
}
factorial(4)
#> [1] 24
```
:::

## Paquetes

Para facilitar la reutilización código y datos R permite la creación de paquetes que pueden importarse desde otros programas. Un paquete es una colección de código, funciones y datos que se almacenan en un fichero dentro de un directorio llamado `library` en el entorno de R. Para ver la ubicación de este directorio dentro del sistema de archivos local se puede utilizar la función `.libPaths()`.

::: {.exmple}
A continuación se muestra un ejemplo de la ubicación del directorio `library`.

```r
.libPaths()
#> [1] "/home/alf/R/x86_64-pc-linux-gnu-library/4.1"
#> [2] "/usr/lib/R/library"
```
:::

Durante la instalación de R también se instalan varios paquetes básicos que están disponibles en cualquier sesión de trabajo con R. Pero añadir nuevas funciones o procedimientos es necesario instalar el paquete que los contiene y después cargarlo en la sesión de trabajo. 

Para ver los paquetes instalados en un ordenador se utiliza la función `library()`.

### Instalación de paquetes

La mayor parte de los paquetes para R están disponibles en el repositorio oficial [CRAN](Comprehensive R Archive Network) (Comprehensive R Archive Network), aunque cualquier persona puede desarrollar un paquete y ponerlo a disposición de la comunidad en cualquier otro repositorio. 

Existen distintas formas de instalar un paquete en R: 

- Directamente desde el repositorio oficial CRAN
- Desde otros repositorios no oficiales (por ejemplo Github)
- Descargando el paquete e instalándolo manualmente. 

#### Instalación de paquetes desde el repositorio CRAN

Para instalar un paquete desde el repositorio oficial CRAN se utiliza la siguiente función: 

- `install.packages(x)`: Obtiene el paquete con el nombre `x` desde un servidor con el repositorio CRAN y lo instala localmente en el directorio `library` del entorno de R. Se puede instalar más de un paquete a la vez pasando un vector con los nombres de los paquetes.

::: {.exmple}
A continuación se muestra un ejemplo de instalación de paquetes desde el repositorio CRAN.

```r
install.packages("devtools")
```
:::

#### Instalación desde otros repositorios (GitHub, GitLab, etc.)

El paquete `remotes` incorpora funciones para instalar paquetes alojados en otros repositorios habituales para el desarrollo de software como [GitHub](https://github.com/), [GitLab](httpsb://gitlab.com/) o [Bioconductor](https://www.bioconductor.org/).

::: {.exmple}
A continuación se muestra un ejemplo de instalación de paquetes desde GitHub.

```r
install.packages("remotes")
remotes::install_github("rkward-community/rk.Teaching")
```
:::

#### Instalación manual

Finalmente es posible instalar un paquete manualmente a partir de su código fuente. Para ello hay previamente hay que descargar el código fuente del paquete en un fichero comprimido en formato zip y después utilizar la siguiente función:

- `ìnstall.packages(x, repos = NULL, type = "source")`: Instala el paquete ubicado en la ruta `x` del sistema de archivos local en la librería `library`.

Una vez instalado un paquete ya está disponible para cargarlo en cualquier sesión de trabajo de R y no es necesario volver a instalarlo.

### Carga de un paquete

Una vez instalado un paquete, para poder ejecutar su contenido es necesario cargarlo en el entorno de trabajo de R. Para ello se utiliza la siguiente función:

- `library(x)`: Ejecuta el código del paquete `x` en la sesión de trabajo activa.

::: {.exmple}
A continuación se muestra un ejemplo de carga de un paquete.

```r
library("remotes")
```
:::

### Paquetes habituales


