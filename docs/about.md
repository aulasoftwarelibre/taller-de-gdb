# ¿Qué es un depurador?

Los depuradores son herramientas que permiten analizar el comportamiento o detectar fallos en una aplicación software. Son, por tanto, una herramienta **imprescindible** para los desarrolladores y aprender a utilizarlos se considera algo básico.

## Funciones de los depuradores

Gracias a los depuradores, podremos realizar una serie de operaciones mientras analizamos una aplicación que hayamos desarrollado y de la que tengamos el código fuente, tales como:

- Ejecución paso a paso.
- Interrupción de la ejecución en un punto determinado o por eventos.
- Trazar (tracking) las funciones por las que hemos pasado.
- Visualizar el valor de las variables.
- Ejecutar expresiones.
- Modificar el estado de la aplicación.
- Localizar fugas de memoria (memory leaks).

Entre un largo etcétera.

## GDB

En este curso nos vamos a enfocar en GDB. GDB o GNU Debugger es un depurador desarrollado inicialmente por Richard Stallman como parte del proyecto GNU, que funciona con múltiples lenguajes de programación tales como C, C++, Fortran o Go, entre otros.

La instalación es directa con el gestor de paquetes de nuestra distribución de Linux.

    # Distribuciones basadas en Debian (deb/apt)
    apt-get install gdb

    # Distribuciones basadas en RedHat (rpm/yum)
    yum install gdb

En los siguientes capítulos desarrollaremos un ejemplo, donde incorporaremos una serie de errores, e iremos depurando la aplicación paso a paso.
