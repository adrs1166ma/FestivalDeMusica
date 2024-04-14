
## 1. iniciar npm
`npm init`
name p (enter)
v (enter)
description: `Proyecto con Gulp, SASS y HTML`
entry point (enter) 
test command (enter)               
git repository (enter si no lo tienes)             
keywords: `SASS, Gulp,`
author: `Anderson Mancilla Aquino`
license: (ISC) (enter)
`yes`

se genera un archivo package.json
- se ejecuta comandos y dependencias `npm`
- se puede compartir los .json para saber que dependencias usa

## 2. instalar SASS con NPM

`npm install sass`

### 2 tipos de dependencias
dp normales - cuando se realiza un deploy en un servidor que soporta node.js, lo instala
dp desarrollo - aca no pero si podras tenerlo localmente

la debemos de tener de desarrollo
borramos : 

```
"dependencies": {
    "sass": "^1.74.1"
  }
```

e instalamos como dependencia de desarrollo

`npm install sass --save-dev`

- en este proyecto, se trabajara con dependecias de desarrollo, que se subira un proyecto compilado y listo

* se creo el dependiente de sass `package-lock.json` donde se puede iditar pero no se deberia
* tambien el node_modules

una vez finalises el proyecto se puede eliminar el node_modules
lo puedes recuperar con `npm i`

## 3. Compilando SASS con NPM

creamos una carpeta `src`, dentro de ella una carpeta `scss`, dentro de ella un archivo `app.scss`.
no es necesario buscar en extenciones de vdcode `Live Sass Compiler`, ya que veremos una mas avanzada.

escribimos en app.scss
```
$rojo: red;

body {
    background-color: $rojo;
}
```
pero no podemos agregarla al html, tenemos que compilarlo por medio del `package.json`.
```
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
```
cambiamos el test por `"sass": "sass src/scss:build/css"`.
el contenido a lado de `:` es el contenido que tenemos en los binarios (los conocimientos de `sass`), mientras que el lado izquierdo es el script que manda a llamar `sass`.
luego del sass se coloca los parametros para que compile el archivo `src/scss`.
Pero lo tiene que guardar en un lugar, compilado en una carpeta en especifico como css `:build/css`.

para correr un script: `npm run sass`

para cambiar el color de rojo a azul ocualquier cambio, se tiene que volver a compilar con `npm run sass`

`watch` - escuchar cambios y ejecutar una accion.
`"sass": "sass --watch src/scss:build/css"`.

una vez que se finalize el proyecto, precionas `crtl + c`

## 4. Instar Gulp y crear el Gulpfile

permite multiples tareas y multiples acciones
compilador de sass 
optimizar, inificar imagenes en diferentes formatos
codigo de js y css mejorado

work flow - algo que automatice estas tareas

`npm i -D gulp`

ese gulp requiere de un archivo en la raiz `gulpfile.js`

## 5. creando y llamando tareas en Gulp con NPX

automatizacion de tareas en funcion a js y mandarlas a llamar

`npx gulp primerTarea`
npx : forma de descargar paquetes, sin instalarlos gobalmente
gulp : es un binario, por lo que se manda a llamar
pT : como npx es parte de node, se le puede mandar a llamar a la tarea

al ejecutar sale como un error de no completo
solucion con una funcion callback
callback : funcion que se llama despues de otra funcion

* `npx` es una forma de hacerlo
* tambien se puede hacerp or medio del `package.json`

## 6. Llamando tareas en Gulp con NPM
en `package.json`

```
"scripts": {
    "sass": "sass --watch src/scss:build/css"
}
```

despues del `css"` agregamos , y:
```
"scripts": {
    "sass": "sass --watch src/scss:build/css",
    "tarea" : "gulp tarea"
}
```

tarea izquierda sera el nombre de npm, pero ejecutara gulp tarea

### por
* NPX :  npx gulp `tarea`
* NPM :  npm run `tarea`

## 7. Compilando SASS con Gulp

pasando esta linea de:
`"sass": "sass --watch src/scss:build/css",`
para la parte de gulpfile

eliminamos lo del gulpfile anteriormente:

```
function tarea(done) {
    console.log("mi primer tarea");

    done();
}

exports.tarea = tarea;

// exports : lenguaje de node
//.primerTarea : identificador
//done : callback, forma de tener codigo asincrono en js, osea que cuando se llama esa funcion ya sabemos que el codigo termino de ejecutarse
// .tarea : cambiamos .primerTarea por recomendacion, por que es la parte donde vas a mandar a llamar.
```
src, dest : identificar, guardarlo

gulp tiene una api,
pipes : una accion que se realiza despues de otra

`src('src/scss/app.scss').pipe( sass() ). pipe( dest("build/css") );`

identifica el archivo.
ejecuta el primer pipe.
compila y finaliza.
ejecuta el siguiente pipe.

si escribimos en el cmd: `npx gulp css` - no fucionara, ya que sass no es una funcion.
Se le puede llamar de nom pero estamos usando gulp.
para que se conencte.
`npm i --save-dev gulp-sass`

```
"gulp": "^5.0.0", // crear y automatizar las tareas
"gulp-sass": "^5.1.0", //conecta gulp con sass
"sass": "^1.74.1" // todo el conocimiento de sass, sintaxis valida y compila
```

en gulp se escribe una nueva sintaxis para sass

`const sass = require("gulp-sass")(require('sass'));`

o 

`"css" : "gulp css"`

y compilamos con npm run css.

##  8. Añadiendo un watch - reflejar los cambios en automativo
 * creando y exportando la funcion dev
watch para 
* npx gulp dev
* npm run dev

el usara gulp dev - en sus video lo pondra de golpe

## 9. Creando diferentes archivos de SCSS

se crean carpetas base y contenido dentro de la carpeta de scss: en base se crea el archivo `_variables.scss` para que identifique la API de SASS que este archivo ira dentro de otro y no una hoja de estilos propia de ese mismo

* antigua forma de importar un archivo scss.

## 10. Diferencias entre @use y @forward con @import
* nueva forma de importar scss

create a file  `_index.scss` en cada una de las carpetas
 * dentro de ella, el `@forward` apunta hacia las otras carpetas o archivos
 * cambiando el @import por `@use` - para compilarlo

 entonces:
  * @forward : indica a lso archivos
  * @use : los compila - local
  * @import : global

  aca se mandan a llamar las variables por files.
  ahora como hacemos para hacerle saber que hay mas archivos que neesitan escuchan sus cambios de sass 

## 11. Compilar todos los archivos SCSS

 * solo basta con cambiar 
 ```
 function css(done) {
    src('src/scss/app.scss') // Identificar el archivo SASS
        .pipe( sass() )      // Compilarlo
        .pipe( dest("build/css") ) // Almacenarla en el disco duro

    done (); // Callback que avisa a gulp cuando llegamos al final
}
```
 * en la linea de :
 `src('src/scss/app.scss') // Identificar el archivo SASS`
 * cambiamos por: 
 `src('src/scss/**/*.scss') // Identificar el archivo SASS`
 * de forma recursiva detectara todos los .scss mientras esten en la carpeta scss.

 tambien modificamos la funcion de dev.

 finalizamos, eliminando al carpeta contenido y su @use

## 12. Creando las variables

se agrego tipografia y colores

## 13. Añadiendo Normalize

añadido

## 14. Añadiendo Plumber

para que no se detenga la tarea por un error

## 15. Creando el Header

en el scss

## 16. Creando el arcihvo SCSS _globales.scss

config css globales 

## 17. Aplicando SCSS al HEADER

uso del amperson con a y hover

## 18. Que son y como crear Mixins?

es una piesa de codigo que se puede reutilizar en multiples lugares de hojas de estilos

- se conoce como directivasa  los @ ---
 * @mixin
 * @include

como una funcion que mandas a llamar cuando sea necesario.

## 19. Creando mixins inteligentes con parametros

los headings usan fondo de 700px
y los parrafos de 400px 
por defecto

 * le paso argumentos dentro de ($...) y repitiendo el $... en alguna variable e incluso se pueden usar con if.

## 20. Creando mixins para Media Queries
con :
@content;

en scss primero el selector y luegoi el mq 

## 21. Creando un snippet para los Media Queries

snippet : truquito
crtl + shift + p
snippet
scss

 ```
  "media query": {
		"prefix": "mq",
		"body": [
			"@include m.$1 {\n\t $2 \n}"
		]
	}
 ```
en la sintaxis anterior de sass no es necesario poner la m. o mixins.

## 22. Creando un snippet para importar variables y mixins

 ```
 ,
	"importar mixins": {
		"prefix": "imm",
		"body": [
			"@use 'base/mixins' as m;"
		]
	},
	"importar variables": {
		"prefix": "imv",
		"body": [
			"@use 'base/variables' as v;"
		]
	}
 ```
## 23. finalizando el header

se finalizar el responsive del header
solucion de abrir denuevo con code .
es abrirlo en su misma carpeta 
para que el historial del proyecto se abra normalmente y no fuera del proyecto
## 24. video en HTML5

 ```
 <video autoplay muted loop controls>
        <source src="video/concierto.mp4" type="video/mp4">
        <source src="video/concierto.ogg" type="video/ogg">
        <source src="video/concierto.webm" type="video/webm">

    </video>
 ```
 autoplay : inicia en automatico
 muted : lo mutea
 loop : vuelve a iniciar el video 
 contorls : mete los controles

## 25. Añadiendo texto donde mostraremos el video

con divs

## 26. Creando un Overlay

apoyado de esta pagina `https://cssgradient.io/`

## 27. 

.. 
convertir de jpg a webP
`https://www.online-convert.com/es/result#j=18d1cada-e11c-4bac-9bc4-9cbbac2678ab`


* para no estar descargando y haciendo todos esos pasos
 - `npm install --save-dev gulp-webp`

problemas de version para usar parallel 

 * `Run npm install` again to install the downgraded version.
 ### 188 - 189

## aligerar  imgs con gulp 

con `npm i --save-dev gulp-imagemin@7.1.0`

 * cuidado con no instalar el cache

 `npm i --save-dev gulp-cache`

## creando imagenes avif

 * recuerda instalar las dependencias: `npm install --save-dev gulp-avif`
 * es muy ligero pero no tiene mucho soporte con los navegadores 

## snipet para imgs
## elemento fijo scss

## colocando galeria con js - 197

## barra de navegacion fija con deteccion scroll - 205

## CSS NANO
hacer que el css sea mas lijero con `npm i --save-dev cssnano autoprefixer postcss gulp-postcss`
## saber donde tenemos que modificar cuando ya esta modificado en los archivos de sass

con  `npm i --save-dev gulp-sourcemaps`

## ahora para js

con `npm i gulp-terser-js`

