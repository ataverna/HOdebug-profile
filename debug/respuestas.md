#Respuestas 

##DEBUGS: Varios bugs: /bugs

Utilizando Makefile --> make all compilamos los 4 programas .c y obtuvimos los 4 ejecutables .e .


1. add_array_dynamic.c:
Ejecutamos ./add_array_dynamic.c, y el programa no dio ningun error, pero se puede notar un error en la forma de escribir la funcion add_array(): 'i' va hasta (n + 1) en vez de n.
Para n=3 y n=4 el programa anda bien, pero pasan cosas raras cuando aumento el 'n' (bug). 
Lo corregimos y funciona para cualquier n entero.

Luego corrimos el programa con gdb ./add_... (+ r) sin poner ninguna bandera de debug y no obtuvimos nada.

2. add_array_segfault:
Ejecutamos ./add_array_segfault.e y nos dio segmentation fault.
Arreglamos el bug que encontramos en el programa anterior (<= n+1 por "<" n). 
Ademas agregamos los malloc para a y b porque no tenian lugar asignado en la memoria. Malloc corre utilizando #include <stdlib.h>.
Encontramos estos bugs mirando el codigo. Cuando lo ejecutabamos nuevamente nos seguia dando error segfault.

Compilando el codigo original(con bugs)con flag '-g'(bandera de debug) y ejecutamos usando gdb(+ r) y obtuvimos :

"Program received signal SIGSEGV, Segmentation fault.
0x00000000004005f2 in main (argc=1, argv=0x7fffffffde58) at add_array_segfault.c:22
22      b[i] = i;"

Suponemos que esto indica que no puede hacer la linea anterior 'a[0] = 0;' porque no esta alocada la memoria.

Si compilamos nuestro codigo original utilizando el Flag de optimizacion para debuggear: -O0, el compilador 'debugea?' linea por linea el codigo. Luego obtenemos:
"Program received signal SIGSEGV, Segmentation fault.
0x00000000004005f2 in main (argc=1, argv=0x7fffffffde58) at add_array_segfault.c:22
22      b[i] = i;"
Obtuvimos lo mismo que el caso anterior. Por defoult gcc optimiza con -O1. No hay diferencias.


Si incluimos el flag '-Wall', esta bandera nos devuelve 'warning'. El programa no incluyo la libreria donde esta la funcion abs().El linkeo lo resuelve buscando y encontrando la funcion abs() pero puede ser la que nosotros queremos o cualquier otra. Es necesario saber sobre que libreria estamos trabajando.Incluimos en el programa '#include <stdlib.h>' y desaparecio el warning.


3. add_array_static.c
Cambiamos '<= n+1' por "<" n como los casos anteriores.
Sin este cambio mostraba una suma, que creemos que es el resultado de sumar
los numeros en los lugares de memoria contiguos al 'ultimo lugar de nuestros
arreglos.

4. add_array_nobugs.c
Al igual que el punto 2, el programa no incluyo la libreria donde esta la funcion abs().Incluimos en el programa '#include <stdlib.h>'.

##DEBUGS: Segmentation Fault: /sigsegv/C

Compilamos, ejecutamos y obtuvimos `small.e` y `big.e`. 
small.e --> ningun error
big.e --> segmentation fault

Luego ejecutamos `ulimit -s unlimited` en la terminal y vuelvimos a
correrlo.
small.e --> se sigue comportando igual
big.e   --> no tengo mas segmentation fault.

Si escribimos en la terminal `ulimit -a`, nos devuelve unformacion sobre la memoria de la computadora.
La memoria disponible en el stack esta ilimitada " stack size unlimited" . Esta es la razon por la cual antes teniamos 
segmentation fault.
El `ulimit -s unlimited` me libera la memoria acotada que tenia antes en el stack.

Si escribo `ulimit -a` en la terminal antes de hacer `ulimit -s unlimited` voy a ver que la memoria disponible de stack es 
" stack size 8192"(por lo menos para mi compu. Tal vez el numero puede cambiar)


