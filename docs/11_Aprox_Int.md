# Aproximación de integrales

En este capítulo se mostrará cómo aproximar integrales en una y varias dimensiones.

## Aproximación de Laplace unidimensional
Esta aproximación es útil para obtener el valor de una integral usando la expansión de Taylor para una función $f(x)$ unimodal en $\Re$, en otras palabras lo que interesa es:
$$ I = \int_{-\infty}^{\infty} f(x) d(x)$$
Al hacer una expansión de Taylor de segundo orden para $\log(f(x))$ en su moda $x_0$ el resultado es:
$$ \log(f(x)) \approx \log(f(x_0)) + \frac{\log(f)^\prime(x_0)}{1!} (x-x_0) + \frac{\log(f)^{\prime \prime}(x_0)}{2!} (x-x_0)^2 $$
El segundo término de la suma se anula porque $\log(f)^\prime(x_0)=0$ por ser $x_0$ el valor donde está el máximo de $\log(f(x))$. La expresión anterior se simplifica en:
$$ \log(f(x)) \approx \log(f(x_0)) + \frac{\log(f)^{\prime \prime}(x_0)}{2!} (x-x_0)^2 $$
al aislar $f(x)$ se tiene que

\begin{equation} \label{fx}
f(x) \approx f(x_0)  \exp \left( -\frac{c}{2} (x-x_0)^2 \right)
\end{equation}

donde $c=-\frac{d^2}{dx^2} \log(f(x)) \bigg|_{x=x_0}$.

La expresión \ref{fx} se puede reescribir de manera que aparezca el núcleo de la función de densidad de la distribución normal con media $x_0$ y varianza $1/c$, a continuación la expresión

$$
f(x) \approx f(x_0) \frac{\sqrt{2 \pi / c}}{\sqrt{2 \pi / c}}  \exp \left( -\frac{1}{2} \left( \frac{x-x_0}{1/\sqrt{c}} \right)^2 \right)
$$
Así al calcular la integral de $f(x)$ en $\Re$ se tiene que:
\begin{equation} \label{aprox_laplace}
I = \int_{-\infty}^{\infty} f(x) d(x) = f(x_0) \sqrt{2 \pi / c}
\end{equation}

### Ejemplo
Calcular la integral de $f(x)=\exp \left( -(x-1.5)^2 \right)$ en $\Re$ utilizando la aproximación de Laplace.

Primero vamos a dibujar la función $f(x)$ para ver en dónde está su moda $x_0$.

```r
fun <- function(x) exp(-(x-1.5)^2)
curve(fun, from=-5, to=5, ylab='f(x)', las=1)
```

![(\#fig:unnamed-chunk-1)Perfil de la función f(x).](11_Aprox_Int_files/figure-latex/unnamed-chunk-1-1.pdf) 

Visualmente se nota que la moda está cerca del valor 1.5 y para determinar numéricamente el valor de la moda $x_0$ se usa la función `optimize`, los resultados se almacenan en el objeto `res`. El valor de la moda corresponde al elemento `maximum` del objeto `res`.

```r
res <- optimize(fun, interval=c(-10, 10), maximum=TRUE)
res
```

```
## $maximum
## [1] 1.499997
## 
## $objective
## [1] 1
```
Para determinar el valor de $c$ de la expresión \ref{aprox_laplace} se utiliza el siguiente código.

```r
require("numDeriv")
constant <- - as.numeric(hessian(fun, res$maximum))
```
Para obtener la aproximación de la integral se usa la expresión \ref{aprox_laplace} y para tener un punto de comparación se evalua la integral usando la función `integrate`, a continuación el código.

```r
fun(res$maximum) * sqrt(2*pi/constant)
```

```
## [1] 1.772454
```

```r
integrate(fun, -Inf, Inf)  # Para comparar
```

```
## 1.772454 with absolute error < 1.5e-06
```
De los anteriores resultados vemos que la aproximación es buena.

