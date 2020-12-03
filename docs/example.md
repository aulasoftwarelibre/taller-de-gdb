# Ejercicio

Para aprender a usar el depurador, vamos a implementar una aplicación.
Te sugerimos que pruebes a implementar todas las funciones indicadas del tirón, sin probar la aplicación en ningún momento.

!!! info

    Implementar toda la lógica de una aplicación de golpe, sin hacer pruebas, no es la manera apropiada de programar y no es algo que aconsejemos. Pero en este caso lo que queremos es aumentar las probabilidades de que cometas algún error para que puedas usar el depurador para encontrarlos.

En el siguiente capítulo, presentaremos una solución (con errores), que será la que usemos en el tutorial.

## Métodos

### allocMatrix

```c
/**
 * @param   n_rows        Número de filas
 * @param   n_columns     Número de columnas
 * @return                Puntero a la matriz
 */
int **allocMatrix(int n_rows, int n_columns);
```

Reserva memoria dinámica para una matriz de dos dimensiones.

### fillMatrix

```c
/**
 * @param   matrix        Puntero a la matriz
 * @param   n_rows        Número de filas
 * @param   n_columns     Número de columnas
 */
void fillMatrix(int **matrix, int n_rows, int n_columns);
```

Rellena la matriz. Cada celda de coordenadas (x,y) será el resultado de ejecutar la siguiente formula `f(x,y) = -x + y`. Es decir, la celda de coordenadas _(0,0)_ dará como resultado `f(0,0) = 0` y la de coordenadas _(3,2)_ dará `f(3,2) = -1`.

### printMatrix

```c
/**
 * @param   matrix        Puntero a la matriz
 * @param   n_rows        Número de filas
 * @param   n_columns     Número de columnas
 */
void printMatrix(int **matrix, int n_rows, int n_columns);
```

Imprime la matriz dada por la salida estándar.

### removeRow

```c
/**
 * @param   matrix              Puntero a la matriz
 * @param   n_rows              Número de filas
 * @param   n_columns           Número de columnas
 * @param   index_to_remove     Índice de la fila a borrar
 * @return                      Puntero a la nueva matriz
 */
int **removeRow(int **matrix, int n_rows, int n_columns, int index_to_remove);
```

Hace una copia de la matrix indicada, pero eliminando una de las filas indicada por el último parámetro.

### freeMatrix

```c
/**
 * @param   matrix        Puntero a la matriz
 * @param   n_rows        Número de filas
 */
void freeMatrix(int ***matrix, int n_rows);
```

Libera la memoria dinámica de la matriz indicada como parámetro y reinicia el puntero a NULL.

## Ejecución del programa

Crearemos una aplicación que secuencialmente realice las siguientes acciones por orden (utilizando las funciones indicadas en la sección anterior):

1. Reserve memoria para una matrix de 3x4.
1. Rellene la matriz con la función `fillMatrix`.
1. Imprima la matriz por pantalla.
1. Haga una copia eliminando la segunda fila con `removeRow`.
1. Imprima la nueva matriz por pantalla.
1. Libere la memoria de las dos matrices.
