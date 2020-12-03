# Código de la aplicación

A continuación mostramos una posible implementación de lo solicitado. Copia el contenido y guardalo en dos ficheros con el nombre que se indica en la cabecera.

!!! tip

    En la esquina superior derecha de cada caja de texto con el código, verás un icono que te copiará el texto al portapeles.

!!! tip

    **Aritmética de punteros**

    Las expresiones con punteros se muestran con aritmética de punteros en vez de con el operador `[]`. Para pasar de un método a otro recuerda que: `x[i]` equivale a `*(x + i)`; es decir, a la variable le sumas el índice (para obtener la dirección de memoria del elemento n-ésimo) y entre paréntesis pones el operador `*` (para obtener el contenido de a donde apunta la dirección de memoria).

    Si el array es multidimensional repite el proceso de fuera a dentro:

    * `x[i][j]`
    * `*(x[i] + j)`
    * `*(*(x + i) + j)`

    Para hacer el ejercicio opuesto, empieza por el interior.

## Fichero: matrix.h

```c
#ifndef _MATRIX_H_
#define _MATRIX_H_

int **allocMatrix(int n_rows, int n_columns);
void fillMatrix(int **matrix, int n_rows, int n_columns);
void printMatrix(int **matrix, int n_rows, int n_columns);
int **removeRow(int **matrix, int n_rows, int n_columns, int index_to_remove);
void freeMatrix(int ***matrix, int n_rows);

#endif // _MATRIX_H_
```

## Fichero: matrix.c

!!! warning

    El código tiene errores de forma intencionada para poder arreglarlos en los siguientes capítulos. De todas maneras, desde el Aula no recomendamos nunca desarrollar una aplicación totalmente sino ir implementando y probando función a función, siguiendo la metodología TDD. Pero la parte de desarrollo orientada a pruebas está fuera del alcance de este taller.

```c
#include <stdio.h>
#include <stdlib.h>
#include "matrix.h"

#define ROWS 5
#define COLUMNS 5

int main()
{
    int **matrix = NULL;
    matrix = allocMatrix(ROWS, COLUMNS);
    fillMatrix(matrix, ROWS, COLUMNS);
    printf("Matriz completa:\n");
    printMatrix(matrix, ROWS, COLUMNS);

    int **newMatrix = NULL;
    newMatrix = removeRow(matrix, ROWS, COLUMNS, 1);
    printf("Matriz sin la segunda fila:\n");
    printMatrix(newMatrix, ROWS-1, COLUMNS);

    freeMatrix(&newMatrix, ROWS-1);
    freeMatrix(&matrix, ROWS);

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
        for (int j = 0; i < n_columns; j++)
        {
            *(*(matrix + i) + j) = - i + j;
        }
    }
}

void printMatrix(int **matrix, int n_rows, int n_columns)
{
    printf("Imprimiendo matrix...\n");
    for (int i = 0; i < n_rows; i++)
    {
        for (int j = 0; j < n_columns; j++)
        {
            printf("%2d ", *(*matrix + i) + j);
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

    int orig_row = 0;

    for (int i = 0; i < n_rows - 1; i++)
    {
        for (int j = 0; j < n_columns; j++)
        {
            orig_row = i < index_to_remove ? i : i - 1;
            *(*(newMatrix + i) + j) = *(*(matrix + orig_row) + j);
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
