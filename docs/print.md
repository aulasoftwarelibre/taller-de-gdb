# Visualizar el estado de la aplicación

Ya tenemos localizado el error, que podemos ver en la línea 5 de la figura anterior: el error se ha producido en la función `fillMatrix`, que fue llamada con los valores de los parámetros indicados, y concretamente el fallo se ha producido en la línea 47.

Veamos el contexto del programa. Podemos ver el código en nuestro editor o usar el comando `list`:

    (gdb) list 47
    42      {
    43          for (int i = 0; i < n_rows; i++)
    44          {
    45              for (int j = 0; i < n_columns; j++)
    46              {
    47                  *(*(matrix + i) + j) = - i + j;
    48              }
    49          }
    50      }
    51

!!! question

    ¿Puedes encontrar el error en ese bloque de código? Inténtalo antes de seguir leyendo.

Estamos recorriendo una matriz, así que el error más común es un acceso fuera de rango. Necesitamos saber los valores de `i` y `j`. Para ver los valores de las variables podemos usar la orden `print`:

    (gdb) print i
    $8 = 0
    (gdb) print j
    $9 = 33612

!!! info

    Cuando necesitamos ver muchas variables, imprimir una a una puede ser un engorro, en ese caso podemos usar `info locals` para visualizar todas las variables del contexto en el que estamos:

        (gdb) info locals
        j = 33612
        i = 0

    Hay más opciones como `info args` e `info stack`. Puedes verlas todas con `help info`.

Lo tenemos, estamos cometiendo un error al iterar sobre `j` y se nos ha salido de rango.

Revisando la aplicación encontramos que el error está en la línea 45, al comparar el límite de `j` con el valor de `i`. Hacemos el siguiente cambio:

    45- for (int j = 0; i < n_columns; j++)
    45+ for (int j = 0; j < n_columns; j++)

Salimos, volvemos a compilar y volvemos a probar, pero ya ejecutando siempre con gdb:

    (gdb) quit
    A debugging session is active.

            Inferior 1 [process 38009] will be killed.

    Quit anyway? (y or n) y
    $ gcc -g -o matrix matrix.c
    $ gdb -q matrix
    Reading symbols from matrix...
    (gdb) r
    Starting program: /home/sergio/Developer/aulasoftwarelibre/taller-de-gdb/example/matrix
    Matriz completa:
    Imprimiendo matrix...
    0  1  2  3  4
    1  2  3  4  5
    2  3  4  5  6
    3  4  5  6  7
    4  5  6  7  8
    Hecho

    Matriz sin la segunda fila:
    Imprimiendo matrix...
    0  1  2  3  4
    1  2  3  4  5
    2  3  4  5  6
    3  4  5  6  7
    Hecho

    [Inferior 1 (process 38194) exited normally]
    (gdb)

Comprobamos que ya no tenemos ningún error de segmentación pero, sin embargo, la aplicación no funciona como debería. ¿Cómo encontramos el error ahora? Necesitamos analizar el funcionamiento de la aplicación e ir viendo lo que hace conforme se ejecuta.

Lo veremos en la siguiente sección.
