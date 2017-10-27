# tstest
Aprendiendo typeScript

<h2><strong>Rodmap</strong></h2>
Si clonaste este proyecto o cualquier otro que sea con Node.js no tenes que hacer lo que dice en <strong>Configuraciones Previas</strong> eso es sólamente un ayudamemoria para arrancar un proyecto
<h3><strong>Configuraciones Previsas</strong></h3>
Este es una ayuda memoria para la instalación de las cosas que necesito para iniciar el desarrollo de una aplicación o website con Node js, TypeScript, gulp y otras yerbas.
<ul>
<li>Primero creo el repositorio en GitHab incluyendo gitignore para node y mientras pienso con que licencia darle al proyecto pongo The Unlicense</li>
<li>En el equipo que voy a hacer el desarollo me voy al directorio donde pongo los proyectos y clono el proyecto.</li>
<li>Como estoy usando Visual Studio Code abro con el la carpeta del proyecto</li>
<li>Abro una teminal y comienzo a bajar las dependencias necesarias:<pre>
npm install express typescript --save
npm install  debug --save-dev
</pre>
<li>Tambien tengo que tener en cuenta que debo bajar los archivos de las definiciones de los paquetes, estos le indican al compilador sobre la estructura de los modulos que estamos ocupando<pre>npm install @types/node @types/express @types/debug --save-dev</pre>
Esto lo tenemos que hacer para todos los paquetes que vayamos a usar en el proyecto</li>
<li>Creamos el archivo de configuración de TypeScript en el directorio raíz del proyecto con el nombre "tsconfig.json" y le escribimos:<pre>{
  "compilerOptions": {
    "target": "es6",
    "module": "commonjs",
    "outDir": "dist"
  },
  "include": [
    "src/**/*.ts"
  ],
  "exclude": [
    "node_modules"
  ]
}</pre>En compilerOptions.target tiene "es6", las opciones son: ECMAScript target version: "ES3" (default), "ES5", "ES6"/"ES2015", "ES2016", "ES2017" or "ESNext".<a href="https://www.typescriptlang.org/docs/handbook/compiler-options.html"><p>Para más informacion click aquí</a></p></li>
<li>Para automatizar nuestras tareas de compilado y transpilado y otras yerbas instalamos gulp y algunos de sus accesorios.<blockquote>NOTA: Al utilizar gulp es recomendable instalarlo primero de forma global<pre>npm install -g gulp</pre>
</blockquote>
<pre>
npm install gulp gulp-typescript gulp-sass --save-dev
</pre>
En este caso no son necesarios los @types porque los procesos de gulp no van a ser parte de nuestra aplicación, a gulp lo usamos para crear nuestra aplicacion y ubicar los archivos en los subdirectorios correspondientes para que funcione.</li>
<li>Ahora debemos crear el archivo de configuracion de gulp en el directorio raiz de nuestro proyecto con el nombre "gulpfile.js". En este archivo creamos los procesos que realizará gulp para armar nuestra aplicación.
<pre>
const gulp = require('gulp');
const ts = require('gulp-typescript');
const JSON_FILES = ['src/*.json', 'src/**/*.json'];

// pull in the project TypeScript config
const tsProject = ts.createProject('tsconfig.json');

gulp.task('scripts', () => {
  const tsResult = tsProject.src()
  .pipe(tsProject());
  return tsResult.js.pipe(gulp.dest('dist'));
});

gulp.task('watch', ['scripts'], () => {
  gulp.watch('src/**/*.ts', ['scripts']);
});

gulp.task('assets', function() {
  return gulp.src(JSON_FILES)
  .pipe(gulp.dest('dist'));
});

gulp.task('default', ['watch', 'assets']);</pre></li>


