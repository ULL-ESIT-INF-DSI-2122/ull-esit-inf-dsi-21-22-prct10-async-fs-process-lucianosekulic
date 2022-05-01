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

  - Procede a entrar en **pila** la función anónima main

* 2º

  - En este paso, se proceden a cargar las diversas librerías y argumentos, entrando access en la **API**

* 3º

  - Acaba la función anónima main, access ya no está en la **API** y procede a entrar el callback en la **cola**

* 4º

  - Se añade a la pila el callback para que se pueda ejecutar correctamente la función y poder devolver un valor

  - Tenemos ahora en la pila -> callback y console.log(starting to wacth file ${filename})

* 5º

  - Se retorna en el output el valor de la función
  - Sigue estando en la pila el callback

* 6º 

  - Procede a intriducirse la funcion watch en la pila

* 7º

  - Después de que se ejecute wacth, procede a llamar a watch.on y se introduce en la **API**

* 8º

  - Se procede a ejecutar la siguiente función, en este caso, el console.log(File ${filename} is no longer watched), entrando en la pila

* 9º

  - Se ejecuta y devuelve el valor ( File ${filename} is no longer watched ) esperado en el output

* 10º

  - Watcher.on ahora pasa a ser callback y entra en la cola de ejecución

* 11º

  - Como la pila ahora mismo está vacía, el callback de la siguiente función entra en la pila y procede a ejecutarse

* 12º 

  - La función console.log(File ${filename} has been modified somehow), procede a ejecutarse entrando en la pila

* 13º

  - Se devuelve un valor por el output (File ${filename} has been modified somehow)

### ¿Qué hace la función access?
La función ***access*** es un método utilizado que prueba los permisos de un determinado fichero o directorio. Estos permisos se podrán pasar como un parámetro usando costantes de acceso.

### ¿Para qué sirve el objeto constants? 
Un objeto ***constants*** es importante en un sistema de archivos, es utilizado debido a que contiene las constantes para poder realizar las operaciones.


## Ejercicio 3

```
import * as fs from 'fs';
import * as yargs from 'yargs';
import * as chalk from 'chalk'

yargs.command({
  command: 'watch',
  describe: 'Observa un determinado directorio para saber si han habido cambios',
  builder: {
    usuario: {
      describe: 'usuario',
      demandOption: true,
      type: 'string',
    },
    ruta: {
      describe: 'Ruta del archivo a observar',
      demandOption: true,
      type: 'string',
    },
  },
  handler(argv) {
    if (typeof argv.usuario === 'string' && typeof argv.ruta === 'string') {
        funcionObservar(argv.ruta);
    }
    else {
      console.log(chalk.red("Ha habido un error con la ruta, asegurese de que es la correcta"));
    }
  },
});

yargs.parse();

export function funcionObservar(ruta: string){
  if (fs.existsSync(ruta)) {
    let contador = 0;

    fs.readdir(ruta, (err, prev) => {
      if (err) 
        console.log(chalk.red("Ha habido un problema con el directorio"));
      else {
        fs.watch(ruta, (evenType, nombreFichero) => {
          contador += 1;

          if (contador % 5 == 1) {
            if (evenType == "rename"){
              fs.readdir(ruta, (err, cur) => {
                if (err) 
                  console.log(chalk.red("Ha habido un problema con el directorio"));
                else {
                  if(prev.length < cur.length) {
                    console.log(`Se ha creado ${nombreFichero}`);
                    prev = cur;
                  }
                  else {
                    console.log(`Se ha eliminado ${nombreFichero}`);
                    prev = cur;
                  }
                }
              });
              contador += 1;
            }
            if (evenType == "change")
              console.log(`Se ha modificado el archivo ${nombreFichero}`)
          }
        });
      }
    });
  }
  else
    console.log(chalk.red("Ha habido un problema con el directorio"))
}
```

En primer lugar se creará un comando ***watch*** el cual se utilizará para observar. Tendrá los argumentos usuario y ruta, en el handler se comprueba que los argumentos que se intriducen son de tipo string, en el caso de que se cumpla se llama a ***funcionObservar*** y sino se lanza un mensaje de error de color rojo mediante el uso de chalk.

En la función se comprueba de que exista la ruta especificada en el argumento, en caso de ser correcta se crea un contador que hará la función de temporizador. Si el directorio se puede leer, se actuliza el contador y se ejecuta la funcion fs.watch. Para que no compruebe continuamente y sea más eficiente, se le ha indicado en que momento se debe de realizar dicha comprobación. Luego, se lee de nuevo el directorio y se comprueba el numero de archivos que hay en el, si se detecta una variación es que se ha añadido o eliminado ficheros.

* ¿Cómo haría para mostrar, no solo el nombre, sino también el contenido del fichero, en el caso de que haya sido creado o modificado?
Para poder mostrar el contenido del fichero lo que se podría utilizar es la función fs.readFile() una vez se ha comprobado que se haya creado o modificado y tras esto hacer un console.log para imprimir por pantalla el resultado.

* ¿Cómo haría para que no solo se observase el directorio de un único usuario sino todos los directorios correspondientes a los diferentes usuarios de la aplicación de notas?
Para realizar esta modificación lo que se puede realizar es quedarse con la carpeta raíz que contiene los usuarios y una vez ahí ejecutar el fs.watch() activando la opción de recursividad, ya que si la tenemos habilitada se podrá observar y analizar todos los directorios y subdirectorios que tenga la carpeta raíz.

## Ejercicio 4

```
import * as fs from 'fs';
import * as yargs from 'yargs';
import * as chalk from 'chalk';
import {spawn} from 'child_process';


yargs.command({
  command: 'check',
  describe: 'Dada una ruta concreta, mostrar si es un directorio o un fichero',
  builder: {
    ruta: {
      describe: 'ruta',
      demandOption: true,
      type: 'string',
    },
  },
  handler(argv) {
    if (typeof argv.ruta === 'string') {
      funcionComprobar(argv.ruta);
    }
    else {
      console.log(chalk.red("Existe un problema con los parametros introducidos"));
    }
  },
});

yargs.command({
  command: 'mkdir',
  describe: 'Crear un nuevo directorio a partir de una nueva ruta que recibe como parámetro',
  builder: {
    ruta: {
      describe: 'ruta',
      demandOption: true,
      type: 'string',
    },
  },
  handler(argv) {
    if (typeof argv.ruta === 'string') {
      funcionMKDIR(argv.ruta);
    }
    else {
      console.log(chalk.red("Existe un problema con los parametros introducidos"));
    }
  },
});

yargs.command({
  command: 'ls',
  describe: 'Listar los ficheros dentro de un directorio',
  builder: {
    ruta: {
      describe: 'ruta',
      demandOption: true,
      type: 'string',
    },
  },
  handler(argv) {
    if (typeof argv.ruta === 'string') {
      funcionLS(argv.ruta);
    }
    else {
      console.log(chalk.red("Existe un problema con los parametros introducidos"));
    }
  },
});

yargs.command({
  command: 'cat',
  describe: 'Mostrar el contenido de un fichero',
  builder: {
    ruta: {
      describe: 'ruta',
      demandOption: true,
      type: 'string',
    },
  },
  handler(argv) {
    if (typeof argv.ruta === 'string') {
      funcionCAT(argv.ruta);
    }
    else {
      console.log(chalk.red("Existe un problema con los parametros introducidos"));
    }
  },
});

yargs.command({
  command: 'rm',
  describe: 'Borrar ficheros y directorios',
  builder: {
    ruta: {
      describe: 'ruta',
      demandOption: true,
      type: 'string',
    },
  },
  handler(argv) {
    if (typeof argv.ruta === 'string') {
      funcionRM(argv.ruta);
    }
    else {
      console.log(chalk.red("Existe un problema con los parametros introducidos"));
    }
  },
});

yargs.command({
  command: 'mv',
  describe: 'Mover y copiar ficheros y/o directorios de una ruta a otra',
  builder: {
    origen: {
      describe: 'ruta origen',
      demandOption: true,
      type: 'string',
    },
    destino: {
      describe: 'ruta destino',
      demandOption: true,
      type: 'string',
    },
  },
  handler(argv) {
    if (typeof argv.origen === 'string' && typeof argv.destino === 'string') {
      funcionMV(argv.origen, argv.destino);
    }
    else {
      console.log(chalk.red("Existe un problema con los parametros introducidos"));
    }
  },
});

yargs.parse();

function funcionComprobar(ruta: string) {
  if (fs.existsSync(ruta)) {
    fs.readdir(ruta, (err, files) => {
      if (err)
        console.log(`${ruta} es un fichero`);
      else
        console.log(`${ruta} es un directorio`);
        files
    });
  }
  else {
    console.log(chalk.red("Existe un problema con la ruta"));
  }
}

function funcionMKDIR(ruta: string) {
  if (fs.existsSync(ruta))
    console.log(chalk.red("El directorio que pretende crear, ya existe"))
  else { 
    spawn('mkdir', [ruta]);
  }
}

function funcionLS(ruta: string) {
  if (fs.existsSync(ruta))
    spawn('ls', [ruta]).stdout.pipe(process.stdout);
  else {
    console.log(chalk.red("Existe un problema con la ruta"));
  }
}

function funcionCAT(ruta: string) {
  if (fs.existsSync(ruta))
    spawn('cat', [ruta]).stdout.pipe(process.stdout);
  else {
    console.log(chalk.red("Existe un problema con la ruta"));
  }
}

function funcionRM(ruta: string) {
  if (fs.existsSync(ruta))
    spawn('rm', ['-r', ruta]);
  else {
    console.log(chalk.red("Existe un problema con la ruta"));
  }
}

function funcionMV(origen: string, destino: string) {
  if (fs.existsSync(origen))
    if (fs.existsSync(destino))
      spawn('mv', [origen, destino]);
    else {
      console.log(chalk.red("Existe un problema con la ruta destino"));
    }
  else { 
    console.log(chalk.red("Existe un problema con la ruta origen"));
  }
}
```

Se ha utilizado ***yargs*** para realizar los comandos y para la creación de los mismos se ha tenido que pasar como argumento una ruta de archivo o directorio. Son los siguientes:
* check: Se comprueba si la ruta existe, si es así se ejecuta fs.readdir para ver si es un fichero o directorio.

* mkdir: Se comprueba si existe la ruta, se informe al usuario de que ya existe dicha carpeta y si no existe se ejecuta un spawn con mkdir especificando la ruta que se ha recibido del usuario.

* ls: Se comprueba si existe la ruta, si es así se lista el contenido del directorio y en caso de que no exista se lanza un mensaje al usuario.

* cat: Se comprueba si existe la ruta, si es así mediante spawn se ejecutará un cat y se imprime por pantalla el contenido del archivo que se ha indicado a través de la ruta.

* rm: Se comprueba si existe la ruta, si es así se elimina lo que esté en la ruta. Con la ayuda de spawn se realiza un rm -r para poder eliminar a lo que esté apuntando la ruta que se ha recibido. En caso de que la ruta no sea la correcta se lanza un mensaje al usuario.

* mv: Se comprueba que existe la ruta que es el origen, si se cumple se vuelve a hacer otra comprobación con la ruta destino. En caso de que alguna de las dos falle, el error se muestra al usuario e indicando cuál no se ha introducido correctamente. Sin embargo, si ambas rutas existen se realiza utilizando spawn un mv y se le pasa como parámetros en un array el origen y el destino.




