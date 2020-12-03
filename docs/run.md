# Compilar y ejecutar

Vamos a compilar y ejecutar nuestra aplicación con el compilador gcc:

```sh
$ gcc -o matrix matrix.c
$ ./matrix
[1]    14938 segmentation fault (core dumped)  ./matrix
```

Nuestra aplicación tiene un error y ahora nos toca encontrarlo. Pero, ¿por dónde empezamos?

!!! danger

    Un error muy común entre principiantes es empezar a depurar usando `printf` para encontrar el momento en que la aplicación falla.

        printf("LLEGA 1");
        memcpy(a, b, 100);
        printf("LLEGA 2");

    El problema, además de lo poco cómodo que es ir añadiendo y borrando estas sentencias por toda la aplicación, es que `printf` escribe en la salida estándar del sistema operativo. Esta salida tiene un buffer y es posible que la aplicación falle antes de que se vacíe y dé la sensación de que el error se ha producido antes de dónde realmente tuvo lugar.

    Si realmente tienes la necesidad de depurar así, usa la salida de error y no la estándar:

        #include <stdio.h>
        fprintf(stderr, "LLEGA 1");
        memcpy(a, b, 100);
        fprintf(stderr, "LLEGA 2");

    La salida estándar de error no tiene buffer y el mensaje es inmediatamente impreso por consola.

Gracias al depurador, podemos saber exáctamente en qué lugar y en qué condiciones se ha producido el error, con lo que no necesitamos usar técnicas de dudosa utilidad como `printf`.

Para ello volvamos a compilar la aplicación pero añadiendo las marcas de depuración y arrancaremos el depurador con nuestro ejecutable:

    $ gcc -g -o matrix matrix.c
    $ gdb -q matrix
    Reading symbols from matrix...
    (gdb)

El depurar ya está iniciado y está esperando órdenes. A lo largo del tutorial iremos viendo las más útiles. Pero por el momento comenzaremos con la orden `run`, la cual permite iniciar la aplicación:

    (gdb) run
    Starting program: matrix

    Program received signal SIGSEGV, Segmentation fault.
    0x0... in fillMatrix (matrix=0x55..., n_rows=5, n_columns=5) at matrix.c:47
    47                  *(*(matrix + i) + j) = - i + j;

!!! tip

    Siempre que no haya ambiguedad, podemos abreviar los comandos usando las primeras letras. En el caso de `run`, podemos escribir `r` y el resultado es el mismo. Para otras órdenes como mínimo habrá que escribir dos o tres letras.

El programa sigue fallando, pero esta vez tenemos información exacta de dónde. Ahora tenemos que encontrar el error. Para eso necesitamos visualizar el estado de la aplicación.
