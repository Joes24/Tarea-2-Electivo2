# Solución de Ecuaciones y Sistemas No Lineales (En Python)
Equipo: José Gormaz, Octavio Marchant, Miguel Quispe, Sthefanne Urzua
# Guía de Uso y Demostración
Para ver una demostración en acción de todos los métodos, deberá ejecutar en Google Colab el archivo "Solvers_Demo.ipynb", donde se encuentran las implementaciones de cada método junto a su respectivo ejemplo.
Todos los métodos estan implementados como clases.
Debe ejecutar primero el solver y luego el demo.

# Métodos de Bracketing:
# 1. Bisection
Este es un método que se basa en el Teorema del Valor Intermedio. Requiere un intervalo [a,b] donde la función cambia de signo  para asegurar que existe al menos una raíz. En cada paso, reduce el intervalo a la mitad.
# Cómo Utilizar el método
1. Importar la Clase "from solvers import BisectionSolver"
2. Definir la Función a Resolver:
Ejemplo: f(x) = cos(x) - x
f_false_position = lambda x: math.cos(x) - x
3. Instanciar y Configurar
solver = FalsePositionSolver(f=f_false_position, tol=1e-8)
4. Encontrar la Raíz:
Llamada: find_root(limite_inferior, limite_superior)
root = solver.find_root(a=0.0, b=1.0)
print(f"Raíz: {root}")

# 2. False position (regula falsi)
El método de falsa posición (Regula Falsi) es un método numérico para encontrar las raíces de una ecuación no lineal, que usa la interpolación lineal entre dos puntos iniciales, a y b, cuyos valores de la función, f(a) y f(b), tengan signos opuestos. Su objetivo es encontrar la intersección de la recta que une (a, f(a)) y (b, f(b)) con el eje x, la cual sirve como una nueva aproximación de la raíz. 
Condiciones iniciales: Se debe tener una función continua y un intervalo inicial [a, b] tal que f(a) * f(b)<0.
Cálculo de la nueva aproximación: Se calcula la raíz de la recta secante que une los puntos (a, f(a)) y (b, f(b)). La fórmula para obtener la nueva aproximación (Xr) es: (Xr}= b - f(b) * (b - a)/(f(b) - f(a)).
Actualización del intervalo: Si el signo de f(Xr) es el mismo que el de f(a), el nuevo intervalo será [Xr,b].
Si el signo de f(Xr) es el mismo que el de f(b), el nuevo intervalo será [a,Xr].
Este proceso se repite iterativamente hasta que la diferencia entre aproximaciones consecutivas sea menor que un criterio de error predefinido. 
Este es un Método de Bracketing que, a diferencia de Bisección, usa la secante para refinar la estimación de la raíz, lo que resulta en una convergencia más rápida. También requiere bracketing.

# Cómo Utilizar el método
1. Importar la Clase:
"from solvers import FalsePositionSolver"
"import math"
2. Definir la Función a Resolver:
Ejemplo: f(x) = cos(x) - x
"f_false_position = lambda x: math.cos(x) - x"
3. Instanciar y Configurar
"solver = FalsePositionSolver(f=f_false_position, tol=1e-8)"
4. Encontrar la Raíz:
"Llamada: find_root(limite_inferior, limite_superior)
root = solver.find_root(a=0.0, b=1.0)
print(f"Raíz: {root}")"

# Métodos abiertos:
# 1. Secant
Este es un Método Abierto que no requiere la derivada. La aproxima utilizando los dos puntos previos de la iteración.
# cómo utilizar el método
Al igual que hicimos con el metodo Fixed Point, creamos una clase, con el mismo nombre, luego definimos la funcion inicial en la cual, le pedire al usuario que defina, la funcion a iterar, un valor inicial y un valor previo al valor inicial, para explicarlo rapidamente, si el valor inicial es x_i, el valor previo al valor inicial seria x_(i - 1), el error esperado y el max. de iteraciones se mantienen igual que el de Fixed Point. A diferencia del metodo de Fixed Point, en este caso le agregaremos algo adicional a la funcion inicial, le diremos al usuario que si o si, el valor inicial y el valor previo al valor inicial deben ser decimales utilizando el condicional if y la funcion isinstance. En caso contrario, al usuario le saltara un error explicandole que debe utilizar decimales.

Despues definimos la funcion Solve, al igual que en el Solve de Fixed Point, le indicaremos que este solve se ocupara si y solo si el usuario quiere utilizar el metodo Secant, utilizando el condicional if le indicamos lo anterior utilizando la misma regla de la funcion lower explicada con el metodo anteriormente nombrado. Continuaremos indicandole que reescribiremos los valores x0 y x1 con x_previo y x_actual respectivamente.

Comenzando con la iteración con un rango de 1 hasta maxiter + 1, devido a que este metodo ocupa fracciones le indico que el dividendo = a la funcion evaluada por el x_actual - la funcion evaluada por el x_previo, creo un condicional if que le indique que, si el dividendo == 0 le salte un error al usuario diciendole que x0 =! x1, para evitar una divicion por 0. Calculamos x_siguiente siguiendo la formula del metodo Secant, creamos la condicional if en caso de que x_siguiente = 0 para evitar tener que dividir por 0 el error, en caso contrario simplemente calculamos el error de forma estandar. 

Imprimimos el numero del intento [k], el valor del x resultante [x_(i + 1) con 5 decimales] y el error [error con 2 decimales multiplicado por e^c, c como constante, para poder resumir el valor resultante], le indicamos que al momento de que el error determinado sea menor o igual al error calculado, detenga la iteración y devuelva la x final y el error resultante.

Por ultimo, le indicamos que x_previo = x_actual y que x_actual = x_siguiente para poder crear la iteracion

Explicacion de uso:
El funcionamiento es el mismo que con Fixed Point, el usuario define una funcion g = lambda x: f(x), un valor inicial x1 y el valor previo al inicial x0, usando el class, Prueba = NoLinearSolver(g, x0, x1), luego usamos la funcion Solve con Resultado = Prueba.Solve(method='Secant') e imprimimos el resultado con print(f"{Resultado}")

# 2. Newton Raphson
Se basa en la aproximación por la línea tangente en el punto inicial. Es el único de los métodos de una sola variable que requiere la función f(x) Y su primera derivada f'(x).
# Cómo utilizar el método
1. Importar la Clase
"from solvers import NewtonRaphsonSolver"
2. Definir la Función y su Derivada:Defina tanto la función f(x) como su derivada f'(x). Ejemplo: Buscando la raíz de f(x) = x^3 - 1
f_newton = lambda x: x**3 - 1
df_newton = lambda x: 3 * x**2 
3. Instanciar y Configurar
solver = NewtonRaphsonSolver(f=f_newton, df=df_newton, tol=1e-12)
4. Encontrar la Raíz: Llame al método "find_root(), proporcionando un único punto inicial (x_init)"

# 3. Fixed point
Conocido como iteración simple, se basa en transformar la ecuación original f(x)=0 en la forma x = g(x). La convergencia depende críticamente de que la derivada de g(x) cumpla que |g'(x)| < 1 cerca de la raíz.
# Cómo utilizar el método
Para instalarlo primero debemos crearle una clase, en este caso la llamaremos NoLinearSolver, luego, definimos una funcion inicial en la cual, le pediremos al usuario que la ocupe, que defina su funcion a iterar y su valor inicial o su valor iterativo, el cual seria x0, al inicio, tenia pensado que el usuario tambien definiera el porcentaje de error, aunque lo deje en decimal, al cual quisiera acercarse y lo mismo para la cantidad de iteraciones que este quisiera realizar, pero lo termine definiendo como una constante de error = 0.01 (o 1%) y max. de iteraciones = 100, ya que son valores bastante predeterminados que permiten resolver el problema con 100% de efectividad.
Importante en este caso, colocar 'self', con el proposito de decirle que se va a configurar a ella misma.

Luego, defino la función Solve, en esta función, iria todo lo correspondiente al codigo para poder resolver la ecuación no lineal.

Primero, siguiendo con la linea de la funcion inicial, le coloco 'self' primeramente, luego, le indico que va a ocupar el metodo de Fixed Point si y solo si, el usuario le especifica utilizando el comando de method = 'Fixed Point', para que funcione SIEMPRE, le indico al metodo que, sea cual sea el imput que se le ingrese (Fixed Point, fixed point, FIXED POINT, etc.) lo deje en minusculas utilizando method = method.lower( ), asi nos ahorramos este molesto problema, luego con la condicional if, le indicamos que, si el method == 'fixed point' utilice el este solve, IMPORTANTE, como indicamos previamente, al estar con la funcion lower( ), el input siempre va a estar en minusculas, si hubieramos hecho method == 'Fixed Point' no funcionaria, aclaración importante.

Segundo, empezamos con la construccion del solve, el try que esta colocado justo debajo del if es, digamos, un limite teorico, lo coloque de todas formas, aunque es poco probable que funcione, ya que es una muestra del estudio del metodo, luego, le indicamos que, a la variable x0 le vamos a cambiar el nombre por x_inicial, despues creamos el for para las iteraciones, indicandole que iremos desde range(1, maxiter + 1), para hacer un rango completo, despues, le indicamos que x_nuevo = a la funcion evaluada en la x_inicial, para calcular el error creamos un condicional if, por que? El error = |(x_(i + 1) - x_i)/x_(i + 1)|, que pasaria si x_(i + 1), que en este caso seria x_nuevo, fuera = 0? Bueno, que habria una divición por 0, por lo tanto, en caso de que fuera asi, el error se calcularia como |x_(i + 1) - x_i| simplemente, que tambien es valido, en caso contrario, entonces el error se calcularia de forma estandar. Luego de calcular el error, se empezarian a imprimir el numero del intento [k], el x resultante [x_(i + 1) con 5 decimales] y el error [error con 2 decimales multiplicado por e^c, c como constante, para poder resumir el valor resultante] y, con la parte final del codigo, con la condicional if le indicaria que, si el error resultante es menor o igual al error esperado 1%, entonces que retornara la x resultante y el error final y para terminar, indicandole que la x_inicial = x_final para que siga iterando.

Explicación de uso:
Para usarlo es super sencillo, el usuario debe indicarle al metodo cual es su función a iterar, para hacerlo, simplemente debe colocar g = lambda x: f(x), de esta forma define la función a iterar, importante, en caso de querer iterar funciones logaritmicas, trigonometricas, inversas trigonometricas, etc. utilizar la libreria math, despues el usuario debera especificar su valor inicial, x0.

Una vez hecho esto simplemente debe ocupar el class asi, Prueba = NoLinearSolver(g, x0), y, utilizando su funcion Resultado = Prueba.Solve(method='Fixed Point'), luego imprimiendolo con print(f"{Resultado}").

# 4. Algoritmo de Brent
Este es un método combina la robustez de Bisección con la alta velocidad de la interpolación cuadrática inversa y la Secante, garantiza la convergencia y ofrece una alta eficiencia.
# Cómo utilizar el método
1. Importar la Clase
"from solvers import BrentSolver"
"import math"
2. Definir la Función a Resolver:Defina la función f(x). Este método requiere que el intervalo inicial cambie de signo. Ejemplo: Buscando la raíz de f(x) = x^3 - 2
f_brent = lambda x: x**3 - 2 
3. Instanciar y Configurar
solver = BrentSolver(f=f_brent, tol=1e-12, max_iter=100)
4. Encontrar la Raíz: Llame al método "find_root()", proporcionando el intervalo inicial [a,b] donde haya un cambio de signo.
