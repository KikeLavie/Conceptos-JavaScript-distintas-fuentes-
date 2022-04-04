## **Cierres**
Un cierre es la combinación agrupada(encerrada) con referencias a su estado circundante (el **entorno léxico**). En otras palabras, un cierre le da acceso al alcance de una función externa desde una función interna.En JavaScript, los cierres se crean cada vez que se crea una función, en el momento de la creación de la función. 

## **Alcance léxico**
considere el siguiente código de ejemplo:
```javascript
function init() {
  var name = 'Mozilla'; // name is a local variable created by init
  function displayName() { // displayName() is the inner function, a closure
    alert(name); // use variable declared in the parent function
  }
  displayName();
}
init();
```
`init()`crea una variable local llamada `name`y una función llamada `displayName()`. La `displayName()`función es una función interna que se define dentro `init()`y está disponible solo dentro del cuerpo de la `init()`función. Tenga en cuenta que la `displayName()`función no tiene variables locales propias. Sin embargo, dado que las funciones internas tienen acceso a las variables de las funciones externas, `displayName()`pueden acceder a la variable `name`declarada en la función principal, `init()`.

Ejecute el código utilizando este enlace JSFiddle y observe que la `alert()`declaración dentro de la `displayName()`función muestra correctamente el valor de la `name`variable, que se declara en su función principal. Este es un ejemplo de alcance léxico , que describe cómo un analizador resuelve los nombres de las variables cuando las funciones están anidadas. La palabra léxico se refiere al hecho de que el alcance léxico usa la ubicación donde se declara una variable dentro del código fuente para determinar dónde está disponible esa variable. Las funciones anidadas tienen acceso a las variables declaradas en su ámbito externo.init()crea una variable local llamada namey una función llamada displayName(). La displayName()función es una función interna que se define dentro init()y está disponible solo dentro del cuerpo de la init()función. Tenga en cuenta que la displayName()función no tiene variables locales propias. Sin embargo, dado que las funciones internas tienen acceso a las variables de las funciones externas, displayName()pueden acceder a la variable namedeclarada en la función principal, init().

Ejecute el código utilizando este enlace JSFiddle y observe que la alert()declaración dentro de la displayName()función muestra correctamente el valor de la namevariable, que se declara en su función principal. Este es un ejemplo de alcance léxico , que describe cómo un analizador resuelve los nombres de las variables cuando las funciones están anidadas. La palabra léxico se refiere al hecho de que el alcance léxico usa la ubicación donde se declara una variable dentro del código fuente para determinar dónde está disponible esa variable. Las funciones anidadas tienen acceso a las variables declaradas en su ámbito externo.

### **Cierre**

Considere el siguiente ejemplo de código:
```javascript
function makeFunc() {
  var name = 'Mozilla';
  function displayName() {
    alert(name);
  }
  return displayName;
}

var myFunc = makeFunc();
myFunc();
```
Ejecutar este código tiene exactamente el mismo efecto que el ejemplo anterior de la `init()`función anterior. Lo que es diferente (e interesante) es que la `displayName()`función interna se devuelve desde la función externa antes de ejecutarse .

A primera vista, puede parecer poco intuitivo que este código todavía funcione. En algunos lenguajes de programación, las variables locales dentro de una función existen solo durante la ejecución de esa función. Una vez `makeFunc()`que termine de ejecutarse, es posible que espere que la variable de nombre ya no sea accesible. Sin embargo, debido a que el código aún funciona como se esperaba, obviamente este no es el caso en JavaScript.

La razón es que las funciones en JavaScript forman cierres. Un cierre es la combinación de una función y el entorno léxico dentro del cual se declaró esa función. Este entorno consta de cualquier variable local que estuviera dentro del alcance en el momento en que se creó el cierre. En este caso, `myFunc`es una referencia a la instancia de la función `displayName`que se crea cuando `makeFunc`se ejecuta. La instancia de `displayName`mantiene una referencia a su entorno léxico, dentro del cual `name`existe la variable. Por esta razón, cuando `myFunc`se invoca, la variable `name`permanece disponible para su uso y se pasa "Mozilla" a `alert`.

Aquí hay un ejemplo un poco más interesante: una `makeAdder` función:
```javascript
function makeAdder(x) {
  return function(y) {
    return x + y;
  };
}

var add5 = makeAdder(5);
var add10 = makeAdder(10);

console.log(add5(2));  // 7
console.log(add10(2)); // 12
```
En este ejemplo, hemos definido una función `makeAdder(x)`que toma un solo argumento `x`y devuelve una nueva función. La función que devuelve toma un único argumento `y`y devuelve la suma de `x`y `y`.

En esencia, `makeAdderes` una fábrica de funciones. Crea funciones que pueden agregar un valor específico a su argumento. En el ejemplo anterior, la fábrica de funciones crea dos funciones nuevas: una que agrega cinco a su argumento y otra que agrega 10.

`add5` y `add10` ambos son cierres. Comparten la misma definición de cuerpo de función, pero almacenan diferentes entornos léxicos. En `add5`el entorno léxico de , `x`es 5, mientras que en el entorno léxico de `add10`, `x`es 10.

## **Nunca entendí los cierres**
(Olivier de meulder)

**Antes que empecemos**

Es importante asimilar algunos conceptos antes de poder asimilar los cierres. Uno de ellos es el contexto de ejecución. 

*Cuando el código se ejecuta en JavaScript, el entorno en el que se ejecuta es muy importante y se evalúa como uno de los siguientes:*
**Código global**:
 *el entorno predeterminado donde se ejecuta su código por primera vez.*
 **Código de función**: *cada vez que el flujo de ejecución ingresa al cuerpo de una función.*

(...)

*(...), pensemos en el término `execution context`como el entorno/alcance en el que se evalúa el código actual.*

En otras palabras, cuando iniciamos el programa, comenzamos en el contexto de ejecución global. Algunas variables se declaran dentro del contexto de ejecución global. Llamamos a estas variables globales. Cuando el programa llama a una función, ¿qué sucede? Unos pasos:
1. JavaScript crea un nuevo contexto de ejecución, un contexto de ejecución local
2. Ese contexto de ejecución local tendrá su propio conjunto de variables, estas variables serán locales para ese contexto de ejecución.
3. El nuevo contexto de ejecución se lanza a la pila de ejecución . Piense en la pila de ejecución como un mecanismo para realizar un seguimiento de dónde se encuentra el programa en su ejecución.
¿Cuándo termina la función? Cuando se encuentra con una `return`declaración o se encuentra con un paréntesis de cierre `}`. Cuando finaliza una función, ocurre lo siguiente:
1. Los contextos de ejecución locales aparecen fuera de la pila de ejecución.
2. Las funciones envían el valor devuelto al contexto de llamada. El contexto de llamada es el contexto de ejecución que llamó a esta función, podría ser el contexto de ejecución global u otro contexto de ejecución local. Depende del contexto de ejecución de la llamada manejar el valor devuelto en ese punto. El valor devuelto podría ser un objeto, una matriz, una función, un valor booleano, cualquier cosa en realidad. Si la función no tiene `return`declaración, `undefined`se devuelve.
3. El contexto de ejecución local se destruye. Esto es importante. Destruido. Todas las variables que se declararon dentro del contexto de ejecución local se borran. Ya no están disponibles. Por eso se llaman variables locales.
**Un ejemplo muy básico**
Antes de llegar a los cierres, echemos un vistazo a la siguiente pieza de código. Parece muy sencillo, cualquiera que lea este artículo probablemente sepa exactamente lo que hace.
```javascript
1: let a = 3 
2: function sumarDos(x) { 
3: let ret = x + 2 
4: return ret 
5: } 
6: let b = sumarDos(a) 
7: console.log(b)
```
Para comprender cómo funciona realmente el motor de JavaScript, analicemos esto con gran detalle.
1. En la línea 1 declaramos una nueva variable `a`en el contexto de ejecución global y le asignamos el número `3`.
2. A continuación se vuelve complicado. Las líneas 2 a 5 están realmente juntas. ¿Qué pasa aquí? Declaramos una nueva variable nombrada `addTwo`en el contexto de ejecución global. ¿Y qué le asignamos? Una definición de función. Lo que esté entre los dos corchetes `{ }`se asigna a `addTwo`. El código dentro de la función no se evalúa, no se ejecuta, solo se almacena en una variable para uso futuro.
3. Así que ahora estamos en la línea 6. Parece simple, pero hay mucho que desempacar aquí. Primero declaramos una nueva variable en el contexto de ejecución global y la etiquetamos `b`. Tan pronto como se declara una variable, tiene el valor de `undefined`.
4. A continuación, todavía en la línea 6, vemos un operador de asignación. Nos estamos preparando para asignar un nuevo valor a la variable `b`. A continuación vemos que se llama a una función. Cuando ve una variable seguida de corchetes `(…)`, esa es la señal de que se está llamando a una función. Avance rápido, cada función devuelve algo (ya sea un valor, un objeto o `undefined`). Lo que se devuelva de la función se asignará a la variable `b`.

5. Pero primero necesitamos llamar a la función etiquetada `addTwo`. JavaScript irá y buscará en su memoria de contexto de ejecución global una variable llamada `addTwo`. Oh, encontró uno, se definió en el paso 2 (o en las líneas 2 a 5). Y he aquí que la variable `addTwo`contiene una definición de función. Tenga en cuenta que la variable ase pasa como argumento a la función. JavaScript busca una variable `a`en su memoria de contexto de ejecución global , la encuentra, encuentra que su valor es `3`y pasa el número `3`como argumento a la función. Listo para ejecutar la función.
6. Ahora el contexto de ejecución cambiará. Se crea un nuevo contexto de ejecución local, llamémoslo 'contexto de ejecución addTwo'. El contexto de ejecución se inserta en la pila de llamadas. ¿Qué es lo primero que hacemos en el contexto de ejecución local?
7. Puede sentirse tentado a decir: "Se `ret`declara una nueva variable en el contexto de ejecución local ". Esa no es la respuesta. La respuesta correcta es que primero tenemos que mirar los parámetros de la función. Se declara una nueva variable `x`en el contexto de ejecución local. Y como el valor `3`se pasó como argumento, a la variable x se le asigna el número `3`.
8. `ret`El siguiente paso es: Se declara una nueva variable en el contexto de ejecución local . Su valor se establece en indefinido. (línea 3)
9. Aún en la línea 3, se debe realizar una adición. Primero necesitamos el valor de `x`. JavaScript buscará una variable `x`. Primero buscará en el contexto de ejecución local. Y encontró uno, el valor es `3`. Y el segundo operando es el número `2`. El resultado de la suma ( `5`) se asigna a la variable `ret`.
10. Línea 4. Devolvemos el contenido de la variable `ret`. Otra búsqueda en el contexto de ejecución local . `ret`contiene el valor `5`. La función devuelve el número `5`. Y la función termina.

11. Líneas 4–5. La función termina. El contexto de ejecución local se destruye. Las variables `x`y `ret`se eliminan. Ya no existen. El contexto se extrae de la pila de llamadas y el valor de retorno se devuelve al contexto de llamada. En este caso, el contexto de llamada es el contexto de ejecución global, porque la función `addTwo`se llamó desde el contexto de ejecución global.

12. Ahora retomamos donde lo dejamos en el paso 4. El valor devuelto (número `5`) se asigna a la variable `b`. Seguimos en la línea 6 del programacito.
13. No voy a entrar en detalles, pero en la línea 7, el contenido de la variable `b`se imprime en la consola. En nuestro ejemplo el número `5`.

Esa fue una explicación muy larga para un programa muy simple, y aún no hemos tocado los cierres. Llegaremos allí, lo prometo. Pero primero tenemos que tomar otro desvío o dos.

## **Comprenda los cierres de JavaScript con facilidad**
(Richard Bovell)
Los cierres permiten a los programadores de JavaScript escribir mejor código. Creativo, expresivo y conciso. 

El cierre tiene tres cadenas de alcance: tiene acceso a su propio alcance (variables definidas entre llaves), tiene acceso a las variables de la función externa y tiene acceso a las variables globales.

La función interna tiene acceso no solo a las variables de la función externa, sino también a los parámetros de la función externa. Sin embargo, tenga en cuenta que la función interna no puede llamar al objeto de argumentos de la función externa , aunque puede llamar directamente a los parámetros de la función externa.

Creas un cierre agregando una función dentro de otra función.
Un ejemplo básico de cierres en JavaScript:
```javascript
function showName (firstName, lastName) {
var nameIntro = "Your name is ";
    // this inner function has access to the outer function's variables, including the parameter
function makeFullName () {        
return nameIntro + firstName + " " + lastName;    
}

return makeFullName ();
}

showName ("Michael", "Jackson"); // Your name is Michael Jackson
```
Los cierres se usan mucho en Node.js; son caballos de batalla en la arquitectura asíncrona y sin bloqueo de Node.js. Los cierres también se usan con frecuencia en jQuery y en casi todos los fragmentos de código JavaScript que lee.

**Reglas y efectos secundarios de los cierres**

1. **Los cierres tienen acceso a la variable de la función externa incluso después de que la función externa regresa:**
una de las características más importantes y delicadas de los cierres es que la función interna aún tiene acceso a las variables de la función externa incluso después de que la función externa haya regresado. Sí, has leído bien. Cuando se ejecutan funciones en JavaScript, usan la misma cadena de ámbito que estaba en vigor cuando se crearon. Esto significa que incluso después de que la función externa haya regresado, la función interna aún tiene acceso a las variables de la función externa. Por lo tanto, puede llamar a la función interna más adelante en su programa. Este ejemplo demuestra:
```javascript
function celebrityName (firstName) {
    var nameIntro = "This celebrity is ";
    // this inner function has access to the outer function's variables, including the parameter
   function lastName (theLastName) {
        return nameIntro + firstName + " " + theLastName;
    }
    return lastName;
}

var mjName = celebrityName ("Michael"); // At this juncture, the celebrityName outer function has returned.

// The closure (lastName) is called here after the outer function has returned above
// Yet, the closure still has access to the outer function's variables and parameter
mjName ("Jackson"); // This celebrity is Michael Jackson
```
2. **Los cierres almacenan referencias a las variables de la función externa**; no almacenan el valor real. Los cierres se vuelven más interesantes cuando el valor de la variable de la función externa cambia antes de que se llame al cierre. Y esta poderosa función se puede aprovechar de formas creativas, como este ejemplo de variables privadas demostrado por primera vez por Douglas Crockford:
```javascript

            // It will return the current value of celebrityID, even after the changeTheID function changes it
          return celebrityID;
        },
        setID: function (theNewID)  {
            // This inner function will change the outer function's variable anytime
            celebrityID = theNewID;
        }
    }

}

var mjID = celebrityID (); // At this juncture, the celebrityID outer function has returned.
mjID.getID(); // 999
mjID.setID(567); // Changes the outer function's variable
mjID.getID(); // 567: It returns the updated celebrityId variable
```
## **Comprender los cierres de JavaScript: un enfoque práctico**
(Paul Upendo)

### **Ámbito léxico**

**El alcance léxico de JavaScript** se determina durante la fase de compilación. Establece el alcance de una variable para que solo pueda llamarse/ referenciarse desde dentro del bloque de código en el que está definida.
Una función declarada dentro de un bloque de función circundante tiene acceso a variables en el ámbito léxico de la función circundante. 
```javascript
var initialBalance = 300 // Global Scope

function withdraw (amount) {
  /**
   * Local Scope
   * Code here has access to anything declared in the global scope
   */
  var balance = parseInt(initialBalance) - parseInt(amount)

  const actualBalance = (function () {
    const TRANSACTIONCOST = 35
    return balance - TRANSACTIONCOST /**
     * Accesses balance variable from the lexical scope
     */
  })() // Immediately Invoked Function expression. IIFE

  // console.log(TRANSACTIONCOST) // ReferenceError: Can't find variable: TRANSACTIONCOST
  return actualBalance
}
```
Invocar una función interna fuera de su función envolvente y, sin embargo, mantener el acceso a las variables en su función envolvente (alcance léxico) crea un cierre de JavaScript.
```javascript
function person () {
  var name = 'Paul'  // Local variable

  var actions = {
    speak: function () {
    //  new function scope
      console.log('My name is ', name) /**
      * Accessing the name variable from the outer function scope (lexical scope)
      */
    }
  } // actions object with a function

  return actions /**
  * We return the actions object
  * We then can invoke the speak function outside this scope
  */
}

person().speak() // Inner function invoked outside its lexical Scope
```
Un Closure nos permite exponer una interfaz pública y, al mismo tiempo, ocultar y preservar el contexto de ejecución del ámbito externo.

Algunos patrones de diseño de JavaScript utilizan cierres.

### **Más ilustraciones sobre Cierres**

Cuando pensamos una función a `setTimeout` o cualquier tipo de devolución de llamada. La función aún recuerda el ámbito léxico debido a la clausura.
```javascript
function foo () {
  var bar = 'bar'
  setTimeout(function () {
    console.log(bar)
  }, 1000)
}

foo() // bar
```
Cierre y bucles
```javascript
for (var i = 1; i <= 5; i++) {
  (function (i) {
    setTimeout(function () {
      console.log(i)
    }, i * 1000)
  })(i)
}
/**
* Prints 1 through 5 after each second
* Closure enables us to remember the variable i
* An IIFE to pass in a new value of the variable i for each iteration
* IIFE (Immediately Invoked Function expression)
*/
```
```javascript
for (let i = 1; i <= 5; i++) {
  (function (i) {
    setTimeout(function () {
      console.log(i)
    }, i * 1000)
  })(i)
}
/**
* Prints 1 through 5 after each second
* Closure enabling us to remember the variable i
* The let keyword rebinds the value of i for each iteration
*/
```