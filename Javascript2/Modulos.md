# ¿Que es un módulo? 
Un módulo es una pieza de código reutilizable que encapsula los detalles de implementación y expone una API pública para que otro código pueda cargarla y usarla fácilmente.
## ¿Por qué necesitamos un módulo?
Técnicamente podemos escribir código sin módulos.

Los módulos son un patrón que los desarrolladores han estado usando en muchas formas y lenguajes de programación diferentes desde los años 60 y 70.

En **JavaScript**, los módulos idealmente deberían permitirnos:
 * **código abstracto**: para delegar la funcionalidad a bibliotecas especializadas para que no tengamos que comprender la complejidad de su implementación real.
 * **encapsular código**: para ocultar el código dentro del módulo si no queremos que se cambie el código. 
 * **reutilizar el código**: para evitar escribir el mismo código una y otra vez.
 * **administrar dependencias:** para cambiar fácilmente las dependencias sin reescribir nuestro código.

### Expresión de función invocada inmediatamente (IIFE)
```javascript
(function() {
    //...
})()
```
Una expresión de función invocada inmediatamente(IIFE)es una función anónima que se invoca cuando se declara.

Observe cómo la función está rodeada por paréntesis. En JavaScript, una línea que comienza con la palabra `function` se considera una declaración de función:
```javascript
// Function declaration
function(){
    consile.log(´test´);
}
```
La invocación inmediata de una declaración de función arroja un error:
```javascript
//Immediately Invoked Function Declaration
function(){
    console.log(´test´);
}()
// => Uncaught SyntaxError: Unexpected token)
```

Poner paréntesis alrededor de la función la convierte en una expresión de función:
```javascript
//Function expression
(function(){
    console.log(´test´);
})
// => returns function(){ console.log(´test´) }
```
La expresión de la función devuelve la función, por lo que podemos llamarla inmediatamente:
```javascript
//Immediately Invoked Function Expression
(function(){
    console.log(´test´);
})()
//writes ´test´to the console and returns undefined
```
Las expresiones de función invocadas inmediatamente nos permiten:
* encapsultar la complejidad del código dentro del IIFE para que no tengamos que entender lo que hace el código IIFE.
* defina variables dentro del IIFE para que no contaminen el alcance global (`var` las declaraciones dentro del IIFE permanecen dentro del cierre del IIFE)

pero no proporcionan un mecanismo para la gestión de dependencias.

## Patrón de módulo revelador

El patrón del módulo revelador es similar a un IIFE, pero asignamos el valor de retorno a una variable:
```javascript
//Expose module as global variable
var singleton = function(){
    //Inner logic
    function sayHello(){
        console.log(´Hello´);
        }

        //Expose API
        return { 
            sayHello: sayHello
        }
    }()
```
Observe cómo no ejecutamos la función en el momento de la declaración.

En su lugar, creamos una instancia de un módulo usando la Modulefunción constructora:
```javascript
var module = new Module();
``` 

para acceder a su API pública:
```javascript
module.sayHello();  
// => Hello
```

El patrón del módulo revelador ofrece beneficios similares a los de un IIFE, pero nuevamente no ofrece un mecanismo para la gestión de dependencias.

A medida que JavaScript evolucionó, se inventaron muchas más sintaxis diferentes para definir módulos, cada una con sus propias ventajas y desventajas.

Los llamamos formatos de módulo.

### **Formatos de módulos**
Un formato de módulo es la sintaxis que podemos usar para definir un módulo.

Antes de EcmaScript 6 o ES2015, JavaScript no tenía una sintaxis oficial para definir módulos. Por lo tanto, los desarrolladores inteligentes propusieron varios formatos para definir módulos en JavaScript.

Algunos de los formatos más ampliamente adaptados y conocidos son:

* Definición de módulo asíncrono (AMD)
* ComúnJS
* Definición de módulo universal (UMD)
* Sistema.registrar
* Formato del módulo ES6

Echemos un vistazo rápido a cada uno de ellos para que pueda reconocer su sintaxis.

### **Definición de módulo asíncrono (AMD)**
El formato **AMD** se usa en los navegadores y utiliza una `define` función para definir módulos:
```javascript
//Calling define with a dependency array and a factory function
define(['dep1', 'dep2'], function (dep1, dep2) {

    //Define the module value by returning a value.
    return function () {};
});
```
### **Formato CommonJS**
El formato CommonJS se usa en Node.js y usa requirey module.exports para definir dependencias y módulos:
```javascript
var dep1 = require('./dep1');  
var dep2 = require('./dep2');

module.exports = function(){  
  // ...
}
```
### **Definición de módulo universal (UMD)**
El formato **UMD** se puede utilizar tanto en el navegador como en Node.js.
```javascript
(function (root, factory) {
  if (typeof define === 'function' && define.amd) {
    // AMD. Register as an anonymous module.
      define(['b'], factory);
  } else if (typeof module === 'object' && module.exports) {
    // Node. Does not work with strict CommonJS, but
    // only CommonJS-like environments that support module.exports,
    // like Node.
    module.exports = factory(require('b'));
  } else {
    // Browser globals (root is window)
    root.returnExports = factory(root.b);
  }
}(this, function (b) {
  //use b in some fashion.

  // Just return a value to define the module export.
  // This example returns an object, but the module
  // can return a function as the exported value.
  return {};
}));
```

### **Sistema.registrar**
El **formato System.register** se diseñó para admitir la sintaxis del módulo ES6 en ES5:
```javascript
import { p as q } from './dep';

var s = 'local';

export function func() {  
  return q;
}

export class C {  
}
```

### **Formato del módulo ES6**
A partir de ES6, JavaScript también admite un formato de módulo nativo.

Utiliza un `export` token para exportar la API pública de un módulo:
```javascript
// lib.js

// Export the function
export function sayHello(){  
  console.log('Hello');
}

// Do not export the function
function somePrivateFunction(){  
  // ...
}
```
y un `import` token para importar partes que exporta un módulo:
```javascript
import { sayHello } from './lib';

sayHello();  
// => Hello
```

Incluso podemos dar a las importaciones un alias usando `as`:
```javascript
import { sayHello as say } from './lib';
say();  
// => Hello
```
o cargar un módulo completo a la vez:
```javascript
import * as lib from './lib';

lib.sayHello();  
// => Hello
```

El formato también admite exportaciones predeterminadas:
```javascript
// lib.js

// Export default function
export default function sayHello(){  
  console.log('Hello');
}

// Export non-default function
export function sayGoodbye(){  
  console.log('Goodbye');
}
```

que puedes importar así:
```javascript
import sayHello, { sayGoodbye } from './lib';

sayHello();  
// => Hello

sayGoodbye();  
// => Goodbye
```

Puede exportar no solo funciones, sino cualquier cosa que desee:
```javascript
// lib.js

// Export default function
export default function sayHello(){  
  console.log('Hello');
}

// Export non-default function
export function sayGoodbye(){  
  console.log('Goodbye');
}

// Export simple value
export const apiUrl = '...';

// Export object
export const settings = {  
  debug: true
}
```

Desafortunadamente, el formato de módulo nativo aún no es compatible con todos los navegadores.

Ya podemos usar el formato de módulo ES6 hoy, pero necesitamos un transpilador como Babel para transpilar nuestro código a un formato de módulo ES5 como AMD o CommonJS antes de que podamos ejecutar nuestro código en el navegador.

## **Cargadores de módulos**
Un cargador de módulos interpreta y carga un módulo escrito en un determinado formato de módulo.

Un cargador de módulos se ejecuta en tiempo de ejecución:

*  carga el cargador de módulos en el navegador
* le dices al cargador de módulos qué archivo principal de la aplicación debe cargar
* el cargador de módulos descarga e interpreta el archivo principal de la aplicación
* el cargador de módulos descarga archivos según sea necesario

Si abre la pestaña de red en la consola de desarrollador de su navegador, verá que el cargador de módulos carga muchos archivos a pedido.

Algunos ejemplos de cargadores de módulos populares son:

*RequireJS* : cargador de módulos en formato AMD
*SystemJS* : cargador de módulos en formato AMD, CommonJS, UMD o System.register
### **Empaquetadores de módulos**
Un empaquetador de módulos reemplaza a un cargador de módulos.

Pero, a diferencia de un cargador de módulos, un paquete de módulos se ejecuta en el momento de la compilación:

* ejecuta el paquete de módulos para generar un archivo de paquete en el momento de la compilación (por ejemplo, bundle.js)
* carga el paquete en el navegador
Si abre la pestaña de red en la consola de desarrollador de su navegador, verá que solo se carga 1 archivo. No se necesita un cargador de módulos en el navegador. Todo el código está incluido en el paquete.

Ejemplos de paquetes de módulos populares son:

*Browserify* : paquete para módulos CommonJS
*Paquete web* : paquete para módulos AMD, CommonJS, ES6
### **Resumen**
Para comprender mejor las herramientas en los entornos modernos de desarrollo de JavaScript, es importante comprender las diferencias entre módulos, formatos de módulos, cargadores de módulos y paquetes de módulos.

Un módulo es una pieza de código reutilizable que encapsula los detalles de implementación y expone una API pública para que otro código pueda cargarla y usarla fácilmente.

Un formato de módulo es la sintaxis que usamos para definir un módulo. En el pasado han surgido diferentes formatos de módulos como AMD , CommonJS , UMD y System.register y ahora hay disponible un formato de módulo nativo desde ES6.

Un cargador de módulos interpreta y carga un módulo escrito en un determinado formato de módulo en tiempo de ejecución. Los ejemplos populares son RequireJS y SystemJS .

Un paquete de módulos reemplaza un cargador de módulos y genera un paquete de todo el código en el momento de la compilación. Los ejemplos populares son Browserify y Webpack .

Ahí lo tiene: ahora está armado con el conocimiento para comprender mejor el desarrollo moderno de JavaScript.

La próxima vez que el compilador de TypeScript le pregunte en qué formato de módulo desea compilar su TypeScript, debe comprender completamente por qué. Si no es así, no se preocupe y vuelva a leer este manual.

Que tengas un maravilloso día y desarróllate con pasión.