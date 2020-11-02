# Ejercicio

Desarrollar los siguientes métodos:

## allocMatrix

```c
/**
 * @param   n_rows        Número de filas
 * @param   n_columns     Número de columnas
 * @return                Puntero a la matriz
 */
int** allocMatrix(int n_rows, int n_columns);
```

Reserva memoria dinámica para una matriz de dos dimensiones.

## fillMatrix

```c
/**
 * @param   matrix        Puntero a la matriz
 * @param   n_rows        Número de filas
 * @param   n_columns     Número de columnas
 */
void fillMatrix(int** matrix, int n_rows, int n_columns);
```

Rellena la matriz. Cada celda será el resultado de sumar el valor de sus coordenadas. Así la celda (0,0) valdrá 0 y la celda (2,1) valdrá 3.

## printMatrix

```c
/**
 * @param   matrix        Puntero a la matriz
 * @param   n_rows        Número de filas
 * @param   n_columns     Número de columnas
 */
void printMatrix(int** matrix, int n_rows, int n_columns);
```

Imprime la matriz dada por la salida estándar.

## removeRow

```c
/**
 * @param   matrix              Puntero a la matriz
 * @param   n_rows              Número de filas
 * @param   n_columns           Número de columnas
 * @param   index_to_remove     Índice de la fila a borrar
 * @return                      Puntero a la nueva matriz
 */
int** removeRow(int** matrix, int n_rows, int n_columns, int index_to_remove);
```

Hace una copia de la matrix indicada, pero eliminando una de las filas indicada por el último parámetro.

## freeMatrix

```c
/**
 * @param   matrix        Puntero a la matriz
 * @param   n_rows        Número de filas
 */
void freeMatrix(int*** matrix, int n_rows);
```

Libera la memoria dinámica de la matriz indicada como parámetro y reinicia el puntero a NULL.

## Ejecución del programa

Crearemos una aplicación que secuencialmente realice las siguientes acciones por orden:

1. Reserve memoria para una matrix de 3x4.
1. Rellene la matriz con la función `fillMatrix`.
1. Imprima la matriz por pantalla.
1. Haga una copia eliminando la segunda fila con `removeRow`.
1. Imprima la nueva matriz por pantalla.
1. Libere la memoria de las dos matrices.

## Código de la aplicación

A continuación mostramos una posible implementación de lo solicitado.

!!! warning

    El código tiene errores de forma intencionada para poder arreglarlos en los siguientes capítulos. De todas maneras, desde el Aula no recomendamos nunca desarrollar una aplicación totalmente sino ir implementando y probando función a función, siguiendo la metodología TDD. Pero la parte de desarrollo orientada a pruebas está fuera del alcance de este taller.

!!! tip

    **Aritmética de punteros**

    Las expresiones con punteros se muestran con aritmética de punteros en vez de con el operador `[]`. Para pasar de un método a otro recuerda que: `x[i]` equivale a `*(x + i)`; es decir, a la variable le sumas el índice (para obtener la dirección de memoria del elemento n-ésimo) y entre paréntesis pones el operador `*` (para obtener el contenido de a donde apunta la dirección de memoria).

    Si el array es multidimensional repite el proceso de fuera a dentro:

    * `x[i][j]`
    * `*(x[i] + j)`
    * `*(*(x + i) + j)`

    Para hacer el ejercicio opuesto, empieza por el interior.

```c
#include <stdio.h>
#include <stdlib.h>
#include "matrix.h"

int main()
{
    int **matrix = NULL;
    matrix = allocMatrix(3, 4);
    fillMatrix(matrix, 3, 4);
    printMatrix(matrix, 3, 4);

    int **newMatrix = NULL;
    newMatrix = removeRow(matrix, 3, 4, 1);
    printMatrix(newMatrix, 2, 4);

    freeMatrix(&newMatrix, 2);
    freeMatrix(&matrix, 3);

    return 0;
}

int **allocMatrix(int n_rows, int n_columns)
{
    int **matrix = NULL;

    matrix = calloc(sizeof *matrix, n_rows);

    for (int i = 0; i < n_rows; i++)
    {
        *(matrix + i) = calloc(sizeof **matrix, n_columns);
    }

    return matrix;
}

void fillMatrix(int **matrix, int n_rows, int n_columns)
{
    for (int i = 0; i < n_rows; i++)
    {
        for (int j = 0; j < n_columns; j++)
        {
            *(*(matrix + i) + j) = i + j;
        }
    }
}

void printMatrix(int **matrix, int n_rows, int n_columns)
{
    printf("Imprimiendo matriz...\n");
    for (int i = 0; i < n_rows; i++)
    {
        for (int j = 0; j < n_columns; j++)
        {
            // Aritmética de punteros
            // Equivale a matrix[i][j]
            printf("%d ", *(*(matrix + i) + j));
        }
        printf("\n");
    }
    printf("Hecho\n\n");
}

int **removeRow(int **matrix, int n_rows, int n_columns, int index_to_remove)
{
    if (n_rows <= 1)
    {
        return NULL;
    }

    int **newMatrix = NULL;
    newMatrix = allocMatrix(n_rows - 1, n_columns);

    int dest_row = 0;

    for (int i = 0; i < n_rows; i++)
    {
        for (int j = 0; j < n_columns; j++)
        {
            dest_row = i < index_to_remove ? i : i - 1;
            *(*(newMatrix + dest_row) + j) = *(*(matrix + i) + j);
        }
    }

    return newMatrix;
}

void freeMatrix(int ***matrix, int n_rows)
{
    for (int i = 0; i < n_rows; i++)
    {
        free(*((*matrix) + i));
        *((*matrix) + i) = NULL;
    }

    *matrix = NULL;
}
```
