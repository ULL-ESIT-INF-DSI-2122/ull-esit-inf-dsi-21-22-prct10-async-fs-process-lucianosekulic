# SonarCloud
[![Sonar-Cloud](https://github.com/ULL-ESIT-INF-DSI-2122/ull-esit-inf-dsi-21-22-prct10-async-fs-process-lucianosekulic/actions/workflows/sonarcloud.yml/badge.svg)](https://github.com/ULL-ESIT-INF-DSI-2122/ull-esit-inf-dsi-21-22-prct10-async-fs-process-lucianosekulic/actions/workflows/sonarcloud.yml)

# Coveralls
[![Coveralls](https://github.com/ULL-ESIT-INF-DSI-2122/ull-esit-inf-dsi-21-22-prct10-async-fs-process-lucianosekulic/actions/workflows/coveralls.yml/badge.svg)](https://github.com/ULL-ESIT-INF-DSI-2122/ull-esit-inf-dsi-21-22-prct10-async-fs-process-lucianosekulic/actions/workflows/coveralls.yml)

# Tests
[![Tests](https://github.com/ULL-ESIT-INF-DSI-2122/ull-esit-inf-dsi-21-22-prct10-async-fs-process-lucianosekulic/actions/workflows/node.js.yml/badge.svg)](https://github.com/ULL-ESIT-INF-DSI-2122/ull-esit-inf-dsi-21-22-prct10-async-fs-process-lucianosekulic/actions/workflows/node.js.yml)

# Practica 10: Aplicacion de procesamiento de notas de texto

## Ejercicio 1

* Inicialmente: En un principio al iniciarse, las colas están vacías.
* 1º

** Procede a entrar en **pila** la función anónima main

* 2º

** En este paso, se proceden a cargar las diversas librerías y argumentos, entrando access en la **API**

* 3º

** Acaba la función anónima main, access ya no está en la **API** y procede a entrar el callback en la **cola**

* 4º

** Se añade a la pila el callback para que se pueda ejecutar correctamente la función y poder devolver un valor

** Tenemos ahora en la pila -> callback y console.log(starting to wacth file ${filename})

* 5º

** Se retorna en el output el valor de la función
** Sigue estando en la pila el callback

* 6º 

** Procede a intriducirse la funcion watch en la pila

* 7º

** Después de que se ejecute wacth, procede a llamar a watch.on y se introduce en la **API**

* 8º

** Se procede a ejecutar la siguiente función, en este caso, el console.log(File ${filename} is no longer watched), entrando en la pila

* 9º

** Se ejecuta y devuelve el valor ( File ${filename} is no longer watched ) esperado en el output

* 10º

** Watcher.on ahora pasa a ser callback y entra en la cola de ejecución

* 11º

** Como la pila ahora mismo está vacía, el callback de la siguiente función entra en la pila y procede a ejecutarse

* 12º 

** La función console.log(File ${filename} has been modified somehow), procede a ejecutarse entrando en la pila

* 13º

** Se devuelve un valor por el output (File ${filename} has been modified somehow)
