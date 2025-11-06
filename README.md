# Solución de Ecuaciones y Sistemas No Lineales
Equipo: José Gormaz, Octavio Marchant, MIguel Quispe, Sthefanne Urzua
# Guía de Uso y Demostración
Para ver una demostración en acción de todos los métodos, deberá ejecutar en Google Colab el archivo "Solvers_Demo.ipynb", donde se encuentran las implementaciones de cada método junto a su respectivo ejemplo.
Todos los métodos estan implementados como clases.
Debe ejecutar primero el solver y luego el demo.

# Métodos de Bracketing:
# 1. Bisection
Este es un método que se basa en el Teorema del Valor Intermedio. Requiere un intervalo [a,b] donde la función cambia de signo  para asegurar que existe al menos una raíz. En cada paso, reduce el intervalo a la mitad.
