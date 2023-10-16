Programación funcional en R y como dejar de pensar en bucle
================
Julen Astigarraga y Verónica Cruz-Alonso
10/16/23

- <a href="#presentación" id="toc-presentación">Presentación</a>
  - <a href="#estructura-del-curso" id="toc-estructura-del-curso">Estructura
    del curso</a>
  - <a href="#quiénes-somos" id="toc-quiénes-somos">Quiénes somos</a>
- <a href="#introducción-a-la-programación-funcional"
  id="toc-introducción-a-la-programación-funcional">Introducción a la
  programación funcional</a>
  - <a href="#cuándo-hay-que-usar-una-función"
    id="toc-cuándo-hay-que-usar-una-función">¿Cuándo hay que usar una
    función?</a>
- <a href="#teoría-sobre-funciones-en-r"
  id="toc-teoría-sobre-funciones-en-r">Teoría sobre funciones en R</a>
- <a href="#cómo-escribir-funciones" id="toc-cómo-escribir-funciones">Cómo
  escribir funciones</a>
  - <a href="#argumentos" id="toc-argumentos">Argumentos</a>
  - <a href="#valores-de-retorno" id="toc-valores-de-retorno">Valores de
    retorno</a>
- <a href="#programación-imperativa"
  id="toc-programación-imperativa">Programación imperativa</a>
- <a href="#programación-funcional"
  id="toc-programación-funcional">Programación funcional</a>
- <a href="#iteraciones-sobre-un-argumento"
  id="toc-iteraciones-sobre-un-argumento">Iteraciones sobre un
  argumento</a>
  - <a href="#nuestro-primer-funcional-generando-listas-map"
    id="toc-nuestro-primer-funcional-generando-listas-map">Nuestro primer
    funcional: generando listas, <code>map()</code></a>
  - <a href="#nuestro-segundo-funcional-generando-vectores-map_"
    id="toc-nuestro-segundo-funcional-generando-vectores-map_">Nuestro
    segundo funcional: generando vectores, <code>map_*()</code></a>
- <a href="#iteraciones-sobre-múltiples-argumentos"
  id="toc-iteraciones-sobre-múltiples-argumentos">Iteraciones sobre
  múltiples argumentos</a>
  - <a href="#nuestro-tercer-funcional-dos-entradas-map2"
    id="toc-nuestro-tercer-funcional-dos-entradas-map2">Nuestro tercer
    funcional: dos entradas, <code>map2()</code></a>
  - <a href="#nuestro-cuarto-funcional-múltiples-entradas-pmap"
    id="toc-nuestro-cuarto-funcional-múltiples-entradas-pmap">Nuestro cuarto
    funcional: múltiples entradas, <code>pmap()</code></a>
- <a href="#sin-salida" id="toc-sin-salida">Sin salida</a>
  - <a href="#nuestro-quinto-funcional-walk-walk2-y-pwalk"
    id="toc-nuestro-quinto-funcional-walk-walk2-y-pwalk">Nuestro quinto
    funcional: <code>walk()</code>, <code>walk2()</code> y
    <code>pwalk()</code></a>
- <a href="#operadores-funcionales"
  id="toc-operadores-funcionales">Operadores funcionales</a>
- <a href="#funcionales-predicate-y-demás"
  id="toc-funcionales-predicate-y-demás">Funcionales predicate y demás</a>
- <a href="#enlaces-de-interés" id="toc-enlaces-de-interés">Enlaces de
  interés</a>

![](images/Logo_ecoinf_10.jpg)

## Presentación

Los **objetivos** de este taller son:

- aprender a escribir funciones

- aplicar funciones en programación iterativa mediante el paquete
  {purrr} de {tidyverse}

- aprender estilos de código que facilitan su comprensión (📝)

### Estructura del curso

| Bloques                               | Tiempo estimado |
|---------------------------------------|-----------------|
| Introducción                          | 15 min          |
| Teoría sobre funciones                | 25 min          |
| Cómo escribir funciones               | 25 min          |
| Programación imperativa vs. funcional | 25 min          |
| Descanso                              | 15 min          |
| Iteraciones con {purrr}               | 75 min          |

### Quiénes somos

![](images/1_N_0YimgDh2_IbBT9jJNtOg.jpg)

Y vosotros ¿quiénes sois?

![QR](images/mentimeter_qr_code.png)
<https://www.menti.com/alyyd29vgomt>

## Introducción a la programación funcional

La creciente disponibilidad de datos y de versatilidad de los programas
de análisis han provocado el incremento en la cantidad y complejidad de
los análisis que realizamos en ecología. Esto hace cada vez más
necesaria la eficiencia en el proceso de gestión y análisis de datos.
Una de las posibles formas para optimizar estos procesos y acortar los
tiempos de trabajo para los usuarios de R es la programación basada en
funciones. Las funciones permiten automatizar tareas comunes (por
ejemplo, leer diferentes bases de datos) simplificando el código.

Las **funciones** son objetos de R que toman un input y consiguen un
output haciendo una acción concreta (funcionalidad específica). Son los
*bloques de construcción* fundamentales en cualquier script de R que es
un lenguaje funcional.

![](images/function.png)

> Para comprender la computación en R, resultan útiles dos lemas:
>
> \- Todo lo que existe es un objeto.
>
> \- Todo lo que sucede es una llamada a función.
>
> — John Chambers ([Advanced R](https://adv-r.hadley.nz/index.html))

Se puede llamar a una función a través de otra función e iterar el
proceso lo que hace que R sea una herramienta muy potente. Las
**iteraciones** sirven para realizar la misma acción a múltiples inputs.
Existen dos grandes paradigmas de iteración: la programación imperativa
y la programación funcional. En este taller, nos centraremos
principalmente en la **programación funcional** y aprenderemos a
utilizar el paquete {purrr}, que proporciona funciones para eliminar
muchos bucles comunes.

``` r
# install.packages("tidyverse")
# install.packages("palmerpenguins")
library(tidyverse)
library(palmerpenguins)

glimpse(penguins)
```

    Rows: 344
    Columns: 8
    $ species           <fct> Adelie, Adelie, Adelie, Adelie, Adelie, Adelie, Adel…
    $ island            <fct> Torgersen, Torgersen, Torgersen, Torgersen, Torgerse…
    $ bill_length_mm    <dbl> 39.1, 39.5, 40.3, NA, 36.7, 39.3, 38.9, 39.2, 34.1, …
    $ bill_depth_mm     <dbl> 18.7, 17.4, 18.0, NA, 19.3, 20.6, 17.8, 19.6, 18.1, …
    $ flipper_length_mm <int> 181, 186, 195, NA, 193, 190, 181, 195, 193, 190, 186…
    $ body_mass_g       <int> 3750, 3800, 3250, NA, 3450, 3650, 3625, 4675, 3475, …
    $ sex               <fct> male, female, female, NA, female, male, female, male…
    $ year              <int> 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007…

``` r
#
df <- penguins |> 
  select(bill_length_mm, bill_depth_mm, flipper_length_mm, body_mass_g)

df$bill_length_mm <- (df$bill_length_mm - min(df$bill_length_mm, na.rm = TRUE))/(max(df$bill_length_mm, na.rm = TRUE) - min(df$bill_length_mm, na.rm = TRUE)) 
df$bill_depth_mm <- (df$bill_depth_mm- min(df$bill_depth_mm, na.rm = TRUE))/(max(df$bill_depth_mm, na.rm = TRUE) - min(df$bill_length_mm, na.rm = TRUE)) 
df$flipper_length_mm <- (df$flipper_length_mm - min(df$flipper_length_mm, na.rm = TRUE))/(max(df$flipper_length_mm, na.rm = TRUE) - min(df$flipper_length_mm, na.rm = TRUE)) 
df$body_mass_g <- (df$body_mass_g - min(df$body_mass_g, na.rm = TRUE))/(max(df$body_mass_g, na.rm = TRUE) - min(df$body_mass_g, na.rm = TRUE))

#
df <- penguins |> 
  select(bill_length_mm, bill_depth_mm, flipper_length_mm, body_mass_g)

rescale01 <- function(x) {
  rng <- range(x, na.rm = TRUE)   
  (x - rng[1]) / (rng[2] - rng[1]) 
} 

df$bill_length_mm <- rescale01(df$bill_length_mm) 
df$bill_depth_mm <- rescale01(df$bill_depth_mm) 
df$flipper_length_mm <- rescale01(df$flipper_length_mm) 
df$body_mass_g <- rescale01(df$body_mass_g)  

head(df)
```

    # A tibble: 6 × 4
      bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
               <dbl>         <dbl>             <dbl>       <dbl>
    1          0.255         0.667             0.153       0.292
    2          0.269         0.512             0.237       0.306
    3          0.298         0.583             0.390       0.153
    4         NA            NA                NA          NA    
    5          0.167         0.738             0.356       0.208
    6          0.262         0.893             0.305       0.264

``` r
#
df <- penguins |> 
  select(bill_length_mm, bill_depth_mm, flipper_length_mm, body_mass_g)

rescaled_df <- lapply(df, rescale01)

head(rescaled_df[[1]])
```

    [1] 0.2545455 0.2690909 0.2981818        NA 0.1672727 0.2618182

``` r
head(rescaled_df[[4]])
```

    [1] 0.2916667 0.3055556 0.1527778        NA 0.2083333 0.2638889

Las principales **ventajas de la programación funcional** (uso de
funciones e iteraciones) son:

- Facilidad para ver la intención del código y, por tanto, mejorar la
  **comprensión** para uno mismo, colaboradores y revisores:
  - Las funciones tienen un nombre evocativo.
  - El código queda más ordenado.

💡Los bucles pueden ser más explícitos en cuanto a que se ve claramente
la iteración, pero se necesita más tiempo para entender que se está
haciendo.

- **Rapidez** si se necesitan hacer cambios ya que las funciones son
  piezas independientes que resuelven un problema concreto.
- **Disminuye la probabilidad de error**.

<!--# Enseñar error de arriba -->

### ¿Cuándo hay que usar una función?

Se recomienda seguir el principio “do not repeat yourself” ([DRY
principle](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself#:~:text=%22Don't%20repeat%20yourself%22,redundancy%20in%20the%20first%20place.)):
cada unidad de conocimiento o información debe tener una representación
única, inequívoca y autoritativa en un sistema.

Escribir una función ya merece la pena cuando has copiado y pegado más
de dos veces lo mismo. Cuantas más veces esté repetido un código en más
sitios necesitarás actualizarlo si hay algun cambio y más aumenta la
probabilidad de error.

## Teoría sobre funciones en R

Las funciones tienen tres componentes:

- `body()` (*cuerpo*): código dentro de la función.
- `formals()`: lista de *argumentos* que controlan como se ejecuta la
  función.
- `environment()`: la estructura que alimenta el *scoping* de la
  función, es decir, el *entorno* donde se ubica la función.

``` r
body(rescale01)
```

    {
        rng <- range(x, na.rm = TRUE)
        (x - rng[1])/(rng[2] - rng[1])
    }

``` r
formals(rescale01)
```

    $x

``` r
environment(rescale01)
```

    <environment: R_GlobalEnv>

💡El [*lexical scoping* (ámbito
léxico)](https://adv-r.hadley.nz/functions.html?q=lexica#lexical-scoping)
son el conjunto de normas sobre cómo los valores de las variables son
extraidos del entorno en cada lenguaje de programación, es decir, como
se asocia una variable a un valor. En R tiene cuatro normas básicas,
pero la más importante para empezar con programación funcional es el
*name masking*: si un argumento no está definido en una función, R
buscará ese nombre en el nivel del entorno inmediatamente superior.

``` r
f <- function(x) {
  x + y
}

y <- 100
f(10)
```

    [1] 110

``` r
y <- 1000
f(10)
```

    [1] 1010

Las **funciones primitivas** son la excepción ya que no tienen los
citados componentes. Están escritas en C en lugar de en R y sólo
aparecen en el paquete *base*. Son más eficientes pero se comportan
diferente a otras funciones, así que R Core Team intenta no crear nuevas
funciones primitivas. El resto de funciones siguen la estructura
indicada arriba.

``` r
sum
```

    function (..., na.rm = FALSE)  .Primitive("sum")

``` r
body(sum)
```

    NULL

Según el tipo de output generado hay dos tipos de funciones:

- Las **funciones de transformación** transforman el objeto que entra en
  la función (primer argumento) y devuelven otro objeto o el anterior
  modificado. Los funcionales son tipos especiales de funciones de
  transformación.

- Las **funciones secundarias** (*side-effect functions*) tienen efectos
  colaterales y ejecutan una acción, como guardar un archivo o dibujar
  un plot. Algunos ejemplos que se usan comunmente son: `library()`,
  `setwd()`, `plot()`, `write.csv()`… Estas funciones retornan *de forma
  invisible* el primer argumento, que no se guarda, pero puede ser usado
  en un pipeline.

💡Los operadores infijos (`+`), de flujo (`for`, `if`), de subdivisión
(`[ ]`, `$`), de reemplazo (`<-`) o incluso las llaves (`{ }`) también
son funciones. La tilde invertida “\`” permite referirse a funciones o
variables que de otro modo tienen “nombre ilegales”.

``` r
3 + 2
```

    [1] 5

``` r
`+`(3, 2)
```

    [1] 5

``` r
for (i in 1:2) print(i)
```

    [1] 1
    [1] 2

``` r
`for`(i, 1:2, print(i))
```

    [1] 1
    [1] 2

En general, sintácticamente, las funciones tienen tres componentes:

- Función `function()` (primitiva)
- Argumentos: lista de inputs.
- Cuerpo: trozo de código que sigue a `function()`, tradicionalmente
  entre llaves.

``` r
nombre1_v1 <- function(x, y) {
  paste(x, y, sep = "_") }  

nombre1_v2 <- function(x, y) paste(x, y, sep = "_")  

nombre1_v3 <- \(x, y) paste(x, y, sep = "_")  

nombre1_v1("Vero", "Cruz") 
```

    [1] "Vero_Cruz"

``` r
nombre1_v2("Vero", "Cruz") 
```

    [1] "Vero_Cruz"

``` r
nombre1_v3("Vero", "Cruz") 
```

    [1] "Vero_Cruz"

📝 Si la función tiene más de dos lineas es mejor usar llaves siempre
para que quede bien delimitada. La llave de apertura nunca debe ir sola
pero sí la de cierre (excepto con *else*). Las sangrías también ayudan
mucho a entender la jerarquía del código dentro de las funciones. En
este sentido recomendamos usar *Code \> Reindent lines/Reformat code* en
el menú de RStudio.

En general las funciones tienen un nombre que se ejecuta cuando se
necesita como hemos visto hasta ahora, pero esto no es obligatorio.
Algunos paquetes como {purrr} o las funciones de la familia `apply`
permiten el uso de **funciones anónimas** para iterar.

``` r
nxcaso <- lapply(penguins, function(x) length(unique(x)))

models <- penguins %>%
  split(.$species) %>%
  map( ~ lm(body_mass_g ~ bill_length_mm, data = .)) #Metodo abreviado donde solo se utiliza un lado de la fórmula de la función
```

## Cómo escribir funciones

Imaginad que para un set de datos quisieramos hacer un gráfico de
distribución de cada variable, en función de otra variable categórica
que nos interese especialmente, para ver como se distribuye.

``` r
glimpse(penguins)
```

    Rows: 344
    Columns: 8
    $ species           <fct> Adelie, Adelie, Adelie, Adelie, Adelie, Adelie, Adel…
    $ island            <fct> Torgersen, Torgersen, Torgersen, Torgersen, Torgerse…
    $ bill_length_mm    <dbl> 39.1, 39.5, 40.3, NA, 36.7, 39.3, 38.9, 39.2, 34.1, …
    $ bill_depth_mm     <dbl> 18.7, 17.4, 18.0, NA, 19.3, 20.6, 17.8, 19.6, 18.1, …
    $ flipper_length_mm <int> 181, 186, 195, NA, 193, 190, 181, 195, 193, 190, 186…
    $ body_mass_g       <int> 3750, 3800, 3250, NA, 3450, 3650, 3625, 4675, 3475, …
    $ sex               <fct> male, female, female, NA, female, male, female, male…
    $ year              <int> 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007…

``` r
#Nos interesan las diferencias entre especie y sexo

ggplot(penguins, aes(x = species, y = bill_length_mm, color = sex)) +
  geom_point(position = position_jitterdodge(), alpha = 0.3) +
  geom_boxplot(alpha = 0.5) +
  scale_color_manual(values = c("turquoise", "goldenrod1")) +
  theme_light()
```

![](intro_prog_fun_files/figure-commonmark/de%20codigo%20repetido%20a%20funcion-1.png)

``` r
ggplot(penguins, aes(x = species, y = bill_depth_mm, color = sex)) +
  geom_point(position = position_jitterdodge(), alpha = 0.3) +
  geom_boxplot(alpha = 0.5) +
  scale_color_manual(values = c("turquoise", "goldenrod1")) +
  theme_light()
```

![](intro_prog_fun_files/figure-commonmark/de%20codigo%20repetido%20a%20funcion-2.png)

``` r
ggplot(penguins, aes(x = species, y = island, color = sex)) +
  geom_jitter() +
  scale_color_manual(values = c("turquoise", "goldenrod1")) +
  theme_light()
```

![](intro_prog_fun_files/figure-commonmark/de%20codigo%20repetido%20a%20funcion-3.png)

``` r
#Etc
```

Hemos copiado un código más de dos veces para realizar una misma acción
(es decir, un gráfico para ver como se distribuye una variable) así que
hay que considerar la posibilidad de que estamos necesitando una
función. A continuación vamos a seguir unos sencillos pasos para
transformar cualquier código repetido en función.

1.  Analizar el código: ¿cuáles son las partes replicadas? ¿cuantos
    inputs tenemos? ¿cuáles varían y cuáles no?

2.  Simplificar y reanalizar duplicaciones

``` r
var_cont <- penguins$bill_length_mm
var_cat <- penguins$island

ggplot(penguins, aes(x = species, y = var_cont, color = sex)) +
  geom_point(position = position_jitterdodge(), alpha = 0.3) +
  geom_boxplot(alpha = 0.5) +
  scale_color_manual(values = c("turquoise", "goldenrod1")) +
  theme_light()
```

![](intro_prog_fun_files/figure-commonmark/de%20codigo%20repetido%20a%20funcion,%20simplificar-1.png)

``` r
ggplot(penguins, aes(x = species, y = var_cat, color = sex)) +
  geom_jitter() +
  scale_color_manual(values = c("turquoise", "goldenrod1")) +
  theme_light()
```

![](intro_prog_fun_files/figure-commonmark/de%20codigo%20repetido%20a%20funcion,%20simplificar-2.png)

``` r
var_cont <- penguins$bill_length_mm
var_cat <- penguins$island
miformato <- list(scale_color_manual(values = c("turquoise", "goldenrod1")),
                  theme_light())

ggplot(penguins, aes(x = species, y = var_cont, color = sex)) +
  geom_point(position = position_jitterdodge(), alpha = 0.3) +
  geom_boxplot(alpha = 0.5) +
  miformato
```

![](intro_prog_fun_files/figure-commonmark/de%20codigo%20repetido%20a%20funcion,%20simplificar2-1.png)

``` r
ggplot(penguins, aes(x = species, y = var_cat, color = sex)) +
  geom_jitter() +
  miformato
```

![](intro_prog_fun_files/figure-commonmark/de%20codigo%20repetido%20a%20funcion,%20simplificar2-2.png)

``` r
var <- "island"
miformato <- list(scale_color_manual(values = c("turquoise", "goldenrod1")),
                  theme_light())

p <- ggplot(penguins, aes(x = species, y = pull(penguins, var), color = sex)) +
  miformato

if (is.numeric(pull(penguins, var))) {
  
  p + 
    geom_point(position = position_jitterdodge(), alpha = 0.3) +
    geom_boxplot(alpha = 0.5) 
  
} else {
  
  p + 
    geom_jitter()
  
}
```

![](intro_prog_fun_files/figure-commonmark/de%20codigo%20repetido%20a%20funcion,%20simplificar3-1.png)

📝 Crear objetos con cálculos intermedios (en el ejemplo, el caso del
objeto “p”), es una buena práctica porque deja más claro lo que el
código está haciendo.

3.  Elegir un nombre para la función (📝). Idealmente tiene que ser
    corto y evocar lo que la función hace. En general, debe ser un verbo
    (p.e. imputar_valores) mientras que los argumentos son nombres (p.e.
    data, variable, etc.). Usar un nombre para una función está
    permitido si la función calcula algo muy conocido (p.e. `mean()`) o
    si sirve para acceder a partes de un objeto (p.e. `residuals()`).
    También se recomienda evitar verbos muy genéricos (p.e. calcular) y
    si el nombre tiene varias palabras separarlas con guión bajo o
    mayúsculas, pero ser consistente. Si programas varias funciones que
    hacen cosas parecidas se recomienda usar el mismo prefijo para todas
    (p.e. “str\_” en el paquete {stringr}).

``` r
#Ejemplos de nombres que no hay que usar

T <- FALSE
c <- 10
mean <- function(x) sum(x)

rm(T, c, mean)
```

4.  Enumerar los argumentos dentro de function y poner el código
    simplificado dentro de las llaves.

``` r
#Varias opciones

exp_plot <- function (var) {
  miformato <-
    list(scale_color_manual(values = c("turquoise", "goldenrod1")),
         theme_light())
  p <- ggplot(penguins, aes(x = species, y = pull(penguins, var), color = sex)) +
    ylab(var) +
    miformato
  if (is.numeric(pull(penguins, var))) {
    p +
      geom_point(position = position_jitterdodge(), alpha = 0.3) +
      geom_boxplot(alpha = 0.5)
    
  } else {
    p +
      geom_jitter()
    
  }
}
```

📝 Utiliza comentarios (#) para explicar el razonamiento detrás de tus
funciones. Se debe evitar explicar qué se está haciendo o cómo, ya que
el propio código ya lo comunica. También se recomienda usar \# para
separar apartados (Cmd/Ctrl + Shift + R).

5.  Probar con inputs diferentes

``` r
exp_plot(var = "island") 
```

![](intro_prog_fun_files/figure-commonmark/de%20codigo%20repetido%20a%20funcion,%20pruebas-1.png)

``` r
exp_plot(var = "year") 
```

![](intro_prog_fun_files/figure-commonmark/de%20codigo%20repetido%20a%20funcion,%20pruebas-2.png)

``` r
exp_plot(var = "body_mass_g") 
```

    Warning: Removed 2 rows containing non-finite values (`stat_boxplot()`).

    Warning: Removed 2 rows containing missing values (`geom_point()`).

![](intro_prog_fun_files/figure-commonmark/de%20codigo%20repetido%20a%20funcion,%20pruebas-3.png)

💡Puedes querer convertir estas pruebas en **test** formales. En
funciones complejas sirven para que, aunque hagas cambios, se pueda
comprobar que la funcionalidad no se ha roto. Si estás interesado mira
este enlace:
<a href="#0" class="uri">https://r-pkgs.org/testing-basics.html</a>

#### Ejercicio 1

Genera una función para escalar (es decir, restar la media y dividir por
la desviación típica) las variables numéricas de penguins.

### Argumentos

En general hay dos grupos: los que especifican los **datos** y los que
especifican **detalles** de la ejecución de la función. Normalmente los
que especifican datos se colocan primero y los de detalle después. Estos
últimos suelen tener valores por defecto (los más comunes), para cuando
no se especifique nada.

<!--# Ver ayuda de quantile -->

📝 Los nombres de los argumentos deben ser cortos y descriptivos. Hay
algunos comunes pero poco descriptivos que también se suelen usar (p.e.
x, w, df, n, p, etc.), además de otros que ya existen y que no conviene
definir de nuevo (p.e. `na.rm()`).

A la hora de ejecutar la función los argumentos se pueden
**especificar** utilizando el nombre completo, una abreviatura
unequívoca o el órden de su posición (*unnamed arguments*), siendo esta
secuencia (nombre \> abreviatura \> posición) el órden de prioridad a la
hora de hacer corresponder los argumentos con lo que se escribe.

📝 Generalmente sólo se usa el orden de posición para para los primeros
argumentos, los más comunes que todo el mundo conoce. Si se cambia un
argumento de detalle con valor por defecto conviene poner siempre el
combre completo.

📝 Usar espacios antes y después de `=` y después de `,` hace mucho más
fácil identificar los argumentos de la función y, en general, todos los
componentes.

``` r
average <- mean(rnorm(10, mean = 50, sd = 25) / 12, trim = 0.2)

average <- mean(rnorm(10,mean=50,sd=25)/12,trim=0.2)
```

Hay un argumento especial llamado “…”, que captura cualquier otro
argumento que no se corresponde con los nombrados en la función. Se
utiliza para transmitir argumentos a otras funciones incluidas en
nuestra función.

``` r
?plot

plot(1:5, 1:5)

plot(1:5, 1:5, main = "Estoy usando argumentos de title()")
```

📝 Usar `…` hace que las funciones sean muy flexibles, pero hace
necesario leer cuidadosamente la documentación para poder usarlo.
Además, si se escribe mal un argumento no sale error.

``` r
sum(1, 2, 5, na.mr = TRUE)
```

    [1] 9

``` r
sum(1, 2, NA, na.mr = TRUE)
```

    [1] NA

### Valores de retorno

La última expresión ejecutada en una función es el valor de retorno. Es
el resultado de ejecutar la función, a no ser que se especifique
`invisible()`. Las funciones arrojan un sólo objeto. Si se quieren
obtener más, tendrá que ser en formato de lista.

<!--# Se os ocurre algún caso donde usar invisible? -->

📝 La función `return()` se usa para indicar explicitamente qué se
quiere obtener en una función. Se recomienda su uso cuando el retorno no
se espera al final de la función. P.e. en las ramas de una estructura
if/else sobre todo hay alguna rama larga y compleja.

#### Ejercicio 2

¿Cómo generalizarías la función `exp_plot()` para que te sirviera para
cualquier base de datos y cualquier variable categórica?

## Programación imperativa

Los bucles for y bucles while (for loops y while loops) son
recomendables para adentrarse en el mundo de las iteraciones porque
hacen que cada iteración sea muy explícita por lo que está claro lo que
está ocurriendo.

``` r
df_ej <- data.frame(
  a = rnorm(5),
  b = rnorm(5),
  c = rnorm(5)
)

salida <- vector("double", ncol(df_ej)) # 1. salida
for (i in seq_along(df_ej)) {           # 2. secuencia
  salida[[i]] <- max(df_ej[[i]])        # 3. cuerpo
}
salida
```

    [1] 1.7977915 0.9483706 1.3032218

1.  Salida: aquí determinamos el espacio de la salida. Esto es muy
    importante para la eficiencia puesto que si aumentamos el tamaño del
    for loop en cada iteración con `c()`, el bucle for será mucho más
    lento.

``` r
x <- c()
system.time(
  for(i in 1:20000) {
    x <- c(x, i)
  }
)
```

       user  system elapsed 
       0.68    0.35    1.03 

``` r
y <- vector("double", length = 20000)
system.time(
  for(i in seq_along(y)) {
    y[i] <- i
  }
)
```

       user  system elapsed 
          0       0       0 

2.  Secuencia: aquí determinamos sobre lo que queremos iterar. Cada
    ejecución del bucle for asignará i a un valor diferente de
    `seq_along(df)`. Si generamos un vector de longitud cero
    accidentalmente, si utilizamos `1:length(x)`, podemos obtener un
    error.

3.  Cuerpo: aquí determinamos lo que queremos que haga cada iteración.
    Se ejecuta repetidamente, cada vez con un valor diferente para `i`.

Existen distintas [variaciones de los bucles
for](https://r4ds.had.co.nz/iteration.html#for-loop-variations): (i)
modificar un objeto existente; (ii) bucles sobre nombres o valores;
(iii) bucles cuando desconocemos la longitud de la salida; (iv) bucles
cuando desconocemos la longitud de la secuencia de entrada, bucles
while.

Algunos [errores comunes](https://adv-r.hadley.nz/control-flow.html)
cuando se utilizan bucles for (ver 5.3.1 Common pitfalls).

Sin embargo, en R los bucles for no son tan importantes como pueden ser
en otros lenguajes porque R es un lenguaje de programación funcional.
Esto significa que *es posible envolver los bucles for en una función* y
llamar a esa función en vez de utilizar el bucle.

Existe la creencia de que los bucles for son lentos, pero la desventaja
real de *los bucles for es que son demasiado flexibles*. Cada funcional
está diseñado para una tarea específica, por lo que en cuanto lo ves en
el código, inmediatamente sabes por qué se está utilizando. Es decir, la
principal ventaja es su claridad al hacer que el código sea más fácil de
escribir y de leer (ver este ejemplo avanzado para entenderlo:
<https://adv-r.hadley.nz/functionals.html>, 9.3 Purrr style).

De todas formas, nunca os sintáis mal por utilizar un bucle en vez de un
funcional. Los funcionales necesitan un paso más de abstracción y pueden
requerir tiempo hasta que los comprendamos. Lo más importante es que
soluciones el problema y poco a poco ir escribiendo código cada vez más
sencillo y elegante.

> Para ser significativamente más fiable, el código debe ser más
> transparente. En particular, las condiciones anidadas y los bucles
> deben considerarse con gran recelo. Las esctructuras de control
> complicados confunden a los programadores. El código desordenado suele
> ocultar errores.
>
> — Bjarne Stroustrup ([Advanced R](https://adv-r.hadley.nz/index.html))

![“Representación gráfica del funcionamiento de los bucles for donde se
ve claramente que se está realizando una iteración. Ilustración de
Allison Horst obtenido de la charla de Hadley Wickham The Joy of
Functional Programming (para ciencia de datos)”](images/forloops.png)

![“Representación gráfica del funcionamiento de `map()` donde el foco
está en la operación realizada. Ilustración de Allison Horst obtenido de
la charla de Hadley Wickham The Joy of Functional Programming (para
ciencia de datos)”](images/map_frosting.png)

## Programación funcional

R es un lenguaje de programación funcional. Esto significa que se basa
principalmente en un estilo de resolución de problemas centrado en
funciones (<https://adv-r.hadley.nz/fp.html>). Un funcional es una
función que toma una función como entrada y devuelve un vector como
salida.

``` r
aleatorizacion <- function(f) {
  f(rnorm(5))
}
aleatorizacion(median)
```

    [1] -0.3417002

Primero, solucionamos el problema para un elemento. Después, generamos
una función que nos permita envolver la solución en una función. Por
último, *aplicamos la función a todos los elementos que estamos
interesados.*

La ventaja de utilizar {purrr} en vez de bucles for es que nos permiten
distinguir en funciones los desafíos comunes de manipulación de listas,
y por lo tanto cada bucle for tiene su propia función. La familia apply
de R base soluciona problemas similares, pero purrr es más consistente
y, por lo tanto, más fácil de aprender. Una vez que dominemos la
programación funcional, podremos solventar muchos problemas de iteración
con menos código, más facilidad y menos errores.

Iteracionar sobre un vector es tan común que el paquete {purrr}
proporciona una familia de funciones (la familia `map()`) para ello.
Recordad que los data frames son listas de vectores de la misma longitud
por lo que cualquier cálculo por filas o columnas supone iteracionar
sobre un vector. Existe una función en {purrr} para cada tipo de salida.
Los sufijos indican el tipo de salida que queremos:

- `map()` genera una lista.
- `map_lgl()` genera un vector lógico.
- `map_int()` genera un vector de números enteros.
- `map_dbl()` genera un vector de números decimales.
- `map_chr()` genera un vector de caracteres.
- `map_vec()` genera un tipo arbitrario de vector, como fechas y
  factores.

💡¿[Por qué está función se llama
*map*](https://adv-r.hadley.nz/functionals.html#map)?

``` r
map_dbl(df_ej, mean)
```

             a          b          c 
    0.09303762 0.33901796 0.23860149 

``` r
df_ej |> 
  map_dbl(mean)
```

             a          b          c 
    0.09303762 0.33901796 0.23860149 

Comparando con un bucle el foco está en la operación que se está
ejecutando (`mean()`), y no en el código necesario para iterar sobre
cada elemento y guardar la salida.

## Iteraciones sobre un argumento

`map_*()` está vectorizado sobre un argumento, e.g. `(x)`, es decir, la
función operará en todos los elementos del vector `x`.

### Nuestro primer funcional: generando listas, `map()`

Toma un vector y una función, llama a la función una vez por cada
elemento del vector y devuelve los resultados en una lista.
`map(1:3, f)` es equivalente a `list(f(1), f(2), f(3))`. Es el
equivalente de `lapply()` de R base.

``` r
cuadratica <- function(x) {
  x ^ 2
}

map(1:4, cuadratica)
```

    [[1]]
    [1] 1

    [[2]]
    [1] 4

    [[3]]
    [1] 9

    [[4]]
    [1] 16

``` r
lapply(X = 1:4, FUN = cuadratica)
```

    [[1]]
    [1] 1

    [[2]]
    [1] 4

    [[3]]
    [1] 9

    [[4]]
    [1] 16

``` r
# algun uso mas interesante
glimpse(penguins)
```

    Rows: 344
    Columns: 8
    $ species           <fct> Adelie, Adelie, Adelie, Adelie, Adelie, Adelie, Adel…
    $ island            <fct> Torgersen, Torgersen, Torgersen, Torgersen, Torgerse…
    $ bill_length_mm    <dbl> 39.1, 39.5, 40.3, NA, 36.7, 39.3, 38.9, 39.2, 34.1, …
    $ bill_depth_mm     <dbl> 18.7, 17.4, 18.0, NA, 19.3, 20.6, 17.8, 19.6, 18.1, …
    $ flipper_length_mm <int> 181, 186, 195, NA, 193, 190, 181, 195, 193, 190, 186…
    $ body_mass_g       <int> 3750, 3800, 3250, NA, 3450, 3650, 3625, 4675, 3475, …
    $ sex               <fct> male, female, female, NA, female, male, female, male…
    $ year              <int> 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007…

``` r
# atajo de para generar una funcion anonima
map(penguins, \(x) length(unique(x)))
```

    $species
    [1] 3

    $island
    [1] 3

    $bill_length_mm
    [1] 165

    $bill_depth_mm
    [1] 81

    $flipper_length_mm
    [1] 56

    $body_mass_g
    [1] 95

    $sex
    [1] 3

    $year
    [1] 3

``` r
# salida dataframe
map_df(penguins, \(x) length(unique(x)))
```

    # A tibble: 1 × 8
      species island bill_length_mm bill_depth_mm flipper_length_mm body_mass_g
        <int>  <int>          <int>         <int>             <int>       <int>
    1       3      3            165            81                56          95
    # ℹ 2 more variables: sex <int>, year <int>

![](images/map.png)

#### Ejercicio 3

Generad un vector, una función y aplicarle la función a cada uno de los
elementos del vector utilizando `map()`.

#### Implementación de map()

``` r
imple_map <- function(x, f, ...) {
  out <- vector("list", length(x))
  for (i in seq_along(x)) {
    out[[i]] <- f(x[[i]], ...)
  }
  out
}

imple_map(1:4, cuadratica)
```

    [[1]]
    [1] 1

    [[2]]
    [1] 4

    [[3]]
    [1] 9

    [[4]]
    [1] 16

La función de {purrr} está escrita en C para maximizar el rendimiento,
conserva los nombres y admite algunos atajos (e.g. `\(x)`).

### Nuestro segundo funcional: generando vectores, `map_*()`

#### Ejercicio 4

Dedicadle un par de minutos a entender lo que hacen las siguientes
funciones:

``` r
map_lgl(penguins, is.numeric)
```

              species            island    bill_length_mm     bill_depth_mm 
                FALSE             FALSE              TRUE              TRUE 
    flipper_length_mm       body_mass_g               sex              year 
                 TRUE              TRUE             FALSE              TRUE 

``` r
penguins_num <- penguins[ , map_lgl(penguins, is.numeric)]
map_dbl(penguins_num, median, na.rm = T)
```

       bill_length_mm     bill_depth_mm flipper_length_mm       body_mass_g 
                44.45             17.30            197.00           4050.00 
                 year 
              2008.00 

``` r
map_chr(penguins, class)
```

              species            island    bill_length_mm     bill_depth_mm 
             "factor"          "factor"         "numeric"         "numeric" 
    flipper_length_mm       body_mass_g               sex              year 
            "integer"         "integer"          "factor"         "integer" 

``` r
map_int(penguins, \(x) length(unique(x)))
```

              species            island    bill_length_mm     bill_depth_mm 
                    3                 3               165                81 
    flipper_length_mm       body_mass_g               sex              year 
                   56                95                 3                 3 

``` r
1:4 |> 
  map_vec(\(x) as.Date(ISOdate(x + 2023, 10, 16)))
```

    [1] "2024-10-16" "2025-10-16" "2026-10-16" "2027-10-16"

Los argumentos que varían para cada ejecución vienen antes de la función
y los argumentos que son los mismos para cada ejecución vienen después
(`na.rm = T`).

![](images/map+fix.png)

R base tiene dos funciones de la familia `apply()` que pueden devolver
vectores atómicos: `sapply()` y `vapply()`. Recomendamos evitar
`sapply()` porque intenta simplificar el resultado y elige un formato de
salida por defecto, pudiendo devolver una lista, un vector o una matriz.
`vapply()` es más seguro porque permite indicar el formato de salida con
FUN.VALUE. La principal desventaja de `vapply()` es que se necesitan
especificar más argumentos que en `map_*()`.

``` r
vapply(penguins_num, median, na.rm = T, FUN.VALUE = double(1))
```

       bill_length_mm     bill_depth_mm flipper_length_mm       body_mass_g 
                44.45             17.30            197.00           4050.00 
                 year 
              2008.00 

``` r
map(penguins, \(x) class(x))
```

    $species
    [1] "factor"

    $island
    [1] "factor"

    $bill_length_mm
    [1] "numeric"

    $bill_depth_mm
    [1] "numeric"

    $flipper_length_mm
    [1] "integer"

    $body_mass_g
    [1] "integer"

    $sex
    [1] "factor"

    $year
    [1] "integer"

``` r
glimpse(penguins)
```

    Rows: 344
    Columns: 8
    $ species           <fct> Adelie, Adelie, Adelie, Adelie, Adelie, Adelie, Adel…
    $ island            <fct> Torgersen, Torgersen, Torgersen, Torgersen, Torgerse…
    $ bill_length_mm    <dbl> 39.1, 39.5, 40.3, NA, 36.7, 39.3, 38.9, 39.2, 34.1, …
    $ bill_depth_mm     <dbl> 18.7, 17.4, 18.0, NA, 19.3, 20.6, 17.8, 19.6, 18.1, …
    $ flipper_length_mm <int> 181, 186, 195, NA, 193, 190, 181, 195, 193, 190, 186…
    $ body_mass_g       <int> 3750, 3800, 3250, NA, 3450, 3650, 3625, 4675, 3475, …
    $ sex               <fct> male, female, female, NA, female, male, female, male…
    $ year              <int> 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007…

``` r
# quitamos na's
penguins <- penguins |> 
  drop_na()

penguins_nested <- penguins |>
  group_by(species) |>
  nest() |> 
  mutate(
    lm_obj = map(data, \(dat) lm(
      bill_length_mm ~ body_mass_g,
      data = dat))
  )

# seleccionar cada elemento de la lista
penguins_nested[["lm_obj"]]
```

    [[1]]

    Call:
    lm(formula = bill_length_mm ~ body_mass_g, data = dat)

    Coefficients:
    (Intercept)  body_mass_g  
       27.11291      0.00316  


    [[2]]

    Call:
    lm(formula = bill_length_mm ~ body_mass_g, data = dat)

    Coefficients:
    (Intercept)  body_mass_g  
       26.53788      0.00413  


    [[3]]

    Call:
    lm(formula = bill_length_mm ~ body_mass_g, data = dat)

    Coefficients:
    (Intercept)  body_mass_g  
      32.174193     0.004463  

``` r
penguins_nested |>
  pluck("lm_obj")
```

    [[1]]

    Call:
    lm(formula = bill_length_mm ~ body_mass_g, data = dat)

    Coefficients:
    (Intercept)  body_mass_g  
       27.11291      0.00316  


    [[2]]

    Call:
    lm(formula = bill_length_mm ~ body_mass_g, data = dat)

    Coefficients:
    (Intercept)  body_mass_g  
       26.53788      0.00413  


    [[3]]

    Call:
    lm(formula = bill_length_mm ~ body_mass_g, data = dat)

    Coefficients:
    (Intercept)  body_mass_g  
      32.174193     0.004463  

## Iteraciones sobre múltiples argumentos

### Nuestro tercer funcional: dos entradas, `map2()`

`map2()` está vectorizado sobre dos argumentos, e.g. `(x, y)`

``` r
potencia <- function(base, exponente) {
  base ^ exponente
}

x <- map(1:4, \(x) sample(5))
y <- map(1:4, \(x) sample(5))

map2(x, y, potencia)
```

    [[1]]
    [1] 256  25 243   2   1

    [[2]]
    [1]   27    1    4    4 3125

    [[3]]
    [1]  64   5 243   4   1

    [[4]]
    [1] 125   9   4  16   1

⚡¡Importante! La primera iteración corresponde al primer valor del
vector `x` y al primer valor del vector `y`. La segunda iteración
corresponde al segundo valor del vector `x` y al segundo valor del
vector `y`. No se hacen todas las combinaciones posibles entre ambos
vectores.

![](images/map2.png)

``` r
imple_map2 <- function(x, y, f, ...) {
  out <- vector("list", length(x))
  for (i in seq_along(x)) {
    out[[i]] <- f(x[[i]], y[[i]], ...)
  }
  out
}

imple_map2(x, y, potencia)
```

    [[1]]
    [1] 256  25 243   2   1

    [[2]]
    [1]   27    1    4    4 3125

    [[3]]
    [1]  64   5 243   4   1

    [[4]]
    [1] 125   9   4  16   1

``` r
penguins_nested <- penguins |>
  group_by(species) |>
  nest() |> 
  mutate(
    lm_obj = map(data, \(dat) lm(
      bill_length_mm ~ body_mass_g,
      data = dat)),
    pred = map2(lm_obj, data,
                \(x, y) predict(x, y))
  )

# unnest()
penguins_nested |> 
  unnest(pred) |> 
  select(!c(data, lm_obj))
```

    # A tibble: 333 × 2
    # Groups:   species [3]
       species  pred
       <fct>   <dbl>
     1 Adelie   39.0
     2 Adelie   39.1
     3 Adelie   37.4
     4 Adelie   38.0
     5 Adelie   38.6
     6 Adelie   38.6
     7 Adelie   41.9
     8 Adelie   37.2
     9 Adelie   39.1
    10 Adelie   41.0
    # ℹ 323 more rows

#### Ejercicio 5

Calculad la correlación entre las predicciones y `bill_length_mm`.
Pista: hay que utilizar `map2_dbl()`

### Nuestro cuarto funcional: múltiples entradas, `pmap()`

Toma una lista con cualquier número de argumentos de entrada.

``` r
# son analogos
map2(x, y, potencia)
```

    [[1]]
    [1] 256  25 243   2   1

    [[2]]
    [1]   27    1    4    4 3125

    [[3]]
    [1]  64   5 243   4   1

    [[4]]
    [1] 125   9   4  16   1

``` r
pmap(list(x, y), potencia)
```

    [[1]]
    [1] 256  25 243   2   1

    [[2]]
    [1]   27    1    4    4 3125

    [[3]]
    [1]  64   5 243   4   1

    [[4]]
    [1] 125   9   4  16   1

``` r
z <- map(1:4, \(x) sample(5))

pmap(list(x, y, z), rnorm)
```

    [[1]]
    [1] 5.6822618 8.8920018 2.8035551 0.7200827 3.1937351

    [[2]]
    [1]  9.621788  2.786051  1.419168  2.387328 11.181470

    [[3]]
    [1] -0.9486085 -0.2173513  7.1610989 11.0028184  3.9983866

    [[4]]
    [1]  6.243105 -1.115881  2.200586  2.731040  4.832554

``` r
# si no nombramos los elementos de la lista, pmap() usara los elementos de la lista en su orden para los argumentos consecutivos de la función
args3 <- list(mean = x, sd = y, n = z)
args3 |> 
  pmap(rnorm)
```

    [[1]]
    [1] 2.384602 7.214090 3.381156 2.513419 2.434247

    [[2]]
    [1]  1.1600578 -0.6947196  2.5704992  2.4318832 -0.4977608

    [[3]]
    [1] 7.895207 4.221315 8.821764 1.130986 3.395119

    [[4]]
    [1]  4.022494  4.781531  3.242094 -2.228335  2.145377

![](images/pmap.png)

## Sin salida

### Nuestro quinto funcional: `walk()`, `walk2()` y `pwalk()`

Cuando queremos utilizar funciones por sus efectos secundarios/side
effects (e.g. `ggsave()`) y no por su valor resultante. Lo importante es
la acción y no el valor u objeto resultante en R.

#### Ejercicio 6

En base a lo que dice en la definición sobre la familia `walk()`, corred
este código y entended lo que hace.

``` r
penguins_nested <- penguins_nested |> 
  mutate(path = str_glue("results/penguins_{species}.csv"))

penguins_nested
```

    # A tibble: 3 × 5
    # Groups:   species [3]
      species   data               lm_obj pred        path                          
      <fct>     <list>             <list> <list>      <glue>                        
    1 Adelie    <tibble [146 × 7]> <lm>   <dbl [146]> results/penguins_Adelie.csv   
    2 Gentoo    <tibble [119 × 7]> <lm>   <dbl [119]> results/penguins_Gentoo.csv   
    3 Chinstrap <tibble [68 × 7]>  <lm>   <dbl [68]>  results/penguins_Chinstrap.csv

``` r
walk2(penguins_nested$data, penguins_nested$path, write_csv)
```

💡Ejemplos de algunas tareas específicas con {purrr}:
<https://r4ds.hadley.nz/iteration>

## Operadores funcionales

Cuando utilizamos las funciones `map()` para repetir muchas operaciones,
aumenta la probabilidad de que una de esas operaciones falle y no
obtenamos ninguna salida. {purrr} proporciona algunos operadores
funcionales (function operators) en forma de adverbios para asegurar que
un error no arruine todo el proceso: `safely()`, `possibly()`,
`quietly()`. Para más información ver:
<https://r4ds.had.co.nz/iteration.html>, 21.6 Dealing with failure.

``` r
x <- list(10, "b", 3)

x |> 
  map(log)
```

    Error in `map()`:
    ℹ In index: 2.
    Caused by error:
    ! non-numeric argument to mathematical function

``` r
x |> 
  map(safely(log))
```

    [[1]]
    [[1]]$result
    [1] 2.302585

    [[1]]$error
    NULL


    [[2]]
    [[2]]$result
    NULL

    [[2]]$error
    <simpleError in .Primitive("log")(x, base): non-numeric argument to mathematical function>


    [[3]]
    [[3]]$result
    [1] 1.098612

    [[3]]$error
    NULL

``` r
x |> 
  map(safely(log)) |> 
  transpose()
```

    $result
    $result[[1]]
    [1] 2.302585

    $result[[2]]
    NULL

    $result[[3]]
    [1] 1.098612


    $error
    $error[[1]]
    NULL

    $error[[2]]
    <simpleError in .Primitive("log")(x, base): non-numeric argument to mathematical function>

    $error[[3]]
    NULL

``` r
x |> 
  map(possibly(log, NA_real_))
```

    [[1]]
    [1] 2.302585

    [[2]]
    [1] NA

    [[3]]
    [1] 1.098612

## Funcionales predicate y demás

Los predicados son funciones que devuelven un solo TRUE o FALSE (e.g.,
`is.character()`). Así, un predicado funcional aplica un predicado a
cada elemento de un vector: `keep()`, `discard()`, `some()`, `every()`,
`detect()`, `detect_index()`… Para más información ver:
<https://r4ds.had.co.nz/iteration.html>, 21.9.1 Predicate functions.

``` r
penguins |> 
  keep(is.numeric)
```

    # A tibble: 333 × 5
       bill_length_mm bill_depth_mm flipper_length_mm body_mass_g  year
                <dbl>         <dbl>             <int>       <int> <int>
     1           39.1          18.7               181        3750  2007
     2           39.5          17.4               186        3800  2007
     3           40.3          18                 195        3250  2007
     4           36.7          19.3               193        3450  2007
     5           39.3          20.6               190        3650  2007
     6           38.9          17.8               181        3625  2007
     7           39.2          19.6               195        4675  2007
     8           41.1          17.6               182        3200  2007
     9           38.6          21.2               191        3800  2007
    10           34.6          21.1               198        4400  2007
    # ℹ 323 more rows

``` r
penguins |> 
  discard(is.numeric)
```

    # A tibble: 333 × 3
       species island    sex   
       <fct>   <fct>     <fct> 
     1 Adelie  Torgersen male  
     2 Adelie  Torgersen female
     3 Adelie  Torgersen female
     4 Adelie  Torgersen female
     5 Adelie  Torgersen male  
     6 Adelie  Torgersen female
     7 Adelie  Torgersen male  
     8 Adelie  Torgersen female
     9 Adelie  Torgersen male  
    10 Adelie  Torgersen male  
    # ℹ 323 more rows

``` r
penguins |> 
  every(is.numeric)
```

    [1] FALSE

`dplyr::across()` es similar a `map()` pero en lugar de hacer algo con
cada elemento de un vector, hace algo con cada columna en un data frame.

`reduce()` es una forma útil de generalizar una función que funciona con
dos entradas (función binaria) para trabajar con cualquier número de
entradas.

``` r
penguins_scaled <- penguins |>
  mutate(across(where(is.numeric), scale))

ls <- list(
  age = tibble(name = c("Vero", "Julen"), age = c(100, 140)),
  sex = tibble(name = c("Vero", "Julen"), sex = c("F", "M")),
  height = tibble(name = c("Vero", "Julen"), height = c("180", "150"))
)

ls |> 
  reduce(full_join, by = "name")
```

    # A tibble: 2 × 4
      name    age sex   height
      <chr> <dbl> <chr> <chr> 
    1 Vero    100 F     180   
    2 Julen   140 M     150   

Este taller está principalmente basado en la primera edición del libro
[R for Data Science](https://r4ds.had.co.nz/) de Hadley Wickham &
Garrett Grolemund y la segunda edición del libro [Advanced
R](https://adv-r.hadley.nz/index.html) de Hadley Wickham.

## Enlaces de interés

- R for data Science (functions):
  <https://r4ds.had.co.nz/functions.html>

- Advanced R (functions): <https://adv-r.hadley.nz/functions.html>

- R for data Science (iteration):
  <https://r4ds.had.co.nz/iteration.html>

- Advanced R (functionals): <https://adv-r.hadley.nz/functionals.html>

- purrr 1.0.0: <https://www.tidyverse.org/blog/2022/12/purrr-1-0-0/>

- Learn to purrr (Rebecca Barter):
  <https://www.rebeccabarter.com/blog/2019-08-19_purrr>

- Sacando el máximo partido a Tidyverse:
  <https://github.com/Julenasti/intro_tidyverse/blob/main/04-scripts/intro_tidyverse.md>

- R for Data Science (2e): <https://r4ds.hadley.nz/>

- Style guide: <http://adv-r.had.co.nz/Style.html>

- Quince consejos para mejorar nuestro código y flujo de trabajo con R:
  <https://www.revistaecosistemas.net/index.php/ecosistemas/article/view/2129>

------------------------------------------------------------------------

<details>
<summary>
Session Info
</summary>

``` r
Sys.time()
```

    [1] "2023-10-16 09:57:42 CEST"

``` r
sessionInfo()
```

    R version 4.2.2 (2022-10-31 ucrt)
    Platform: x86_64-w64-mingw32/x64 (64-bit)
    Running under: Windows 10 x64 (build 19045)

    Matrix products: default

    locale:
    [1] LC_COLLATE=English_United Kingdom.utf8 
    [2] LC_CTYPE=English_United Kingdom.utf8   
    [3] LC_MONETARY=English_United Kingdom.utf8
    [4] LC_NUMERIC=C                           
    [5] LC_TIME=English_United Kingdom.utf8    

    attached base packages:
    [1] stats     graphics  grDevices utils     datasets  methods   base     

    other attached packages:
     [1] palmerpenguins_0.1.1 forcats_0.5.1        stringr_1.5.0       
     [4] dplyr_1.1.2          purrr_1.0.1          readr_2.1.2         
     [7] tidyr_1.3.0          tibble_3.2.1         ggplot2_3.4.2       
    [10] tidyverse_1.3.2     

    loaded via a namespace (and not attached):
     [1] lubridate_1.8.0     assertthat_0.2.1    digest_0.6.29      
     [4] utf8_1.2.3          R6_2.5.1            cellranger_1.1.0   
     [7] backports_1.4.1     reprex_2.0.1        evaluate_0.18      
    [10] httr_1.4.3          pillar_1.9.0        rlang_1.1.1        
    [13] googlesheets4_1.0.0 readxl_1.4.0        rstudioapi_0.13    
    [16] rmarkdown_2.16      labeling_0.4.2      googledrive_2.0.0  
    [19] bit_4.0.5           munsell_0.5.0       broom_1.0.0        
    [22] compiler_4.2.2      modelr_0.1.8        xfun_0.39          
    [25] pkgconfig_2.0.3     htmltools_0.5.3     tidyselect_1.2.0   
    [28] fansi_1.0.4         crayon_1.5.2        tzdb_0.3.0         
    [31] dbplyr_2.2.1        withr_2.5.0         grid_4.2.2         
    [34] jsonlite_1.8.0      gtable_0.3.3        lifecycle_1.0.3    
    [37] DBI_1.1.3           magrittr_2.0.3      scales_1.2.1       
    [40] cli_3.6.1           stringi_1.7.12      vroom_1.5.7        
    [43] farver_2.1.1        fs_1.5.2            xml2_1.3.3         
    [46] ellipsis_0.3.2      generics_0.1.3      vctrs_0.6.3        
    [49] tools_4.2.2         bit64_4.0.5         glue_1.6.2         
    [52] hms_1.1.1           parallel_4.2.2      fastmap_1.1.0      
    [55] yaml_2.3.5          colorspace_2.1-0    gargle_1.2.0       
    [58] rvest_1.0.2         knitr_1.40.1        haven_2.5.0        

</details>
