# Conceptos-JavaScript-distintas-fuetes-
# Conceptos claves Javascript #

## Pila de llamada ##

####  Los puntos claves de la pila de llamadas son:
 * Es de subproceso único. Lo que significa que solo puede hacerse una cosa a la vez.
 * La ejecución del código es sincrónica.
 * La  invocación de una función crea un marco de pila que ocupa una memoria temporal. 
 * Funciona como una estructura de datos  LIFO: último  en entrar, primero en salir. 

## El motor consta de dos componentes principales:
 * Montón de memoria: aquí es donde ocurre la asignación de memoria.
 *  Pila de llamadas: aquí es donde se encuentran los marcos de la pila mientras se ejecuta el código.

Tenemos eas cosas llamadas API web que proporcionan los navegadores, como DOM, AJAX,setTimeout y mucho más. 
Y luego, tenemos el bucle de eventos tan popular y la cola de devolución de llamada.

El **Call Stack** es una estructura de datos que registra básicamente en qué parte del programa nos encontramos. Si entramos en una función, la colocamos en la parte superior de la pila. Si volvemos de una función, salimos de la parte superior de la pila. Eso es todo lo que la pila puede hacer.
Veamos un ejemplo:

``` javascript
función multiplicar(x, y) { 
    devuelve x * y; 
}
function imprimirCuadrado(x) { 
    var s = multiplicar(x, x); 
    consola.log(s); 
}
imprimirCuadrado(5);
```
Cuando el motor comience a ejecutar este código, la `pila de llamada` estara vacia. Posteriormente, los pasos serán los siguientes:

Cada entrada en `la pila de llamadas` se denomina `trama de pila`. 

Y así es exactamente como se construyen los seguimientos de la pila cuando se lanza una excepción: es básicamente el estado de la pile de llamadas cuando ocurrió la excepción. Echa un vistazo al siguiente código:
```javascript
function foo() { 
    throw new Error('SessionStack lo ayudará a resolver bloqueos :)'); 
}
barra de funciones () { 
    foo (); 
}
función inicio() { 
    barra(); 
}
comienzo();
```
Si esto se ejecuta en Chrome ( asumiendo que este código está en un archivo llamado foo.js), se producirá el siguiente seguimiento de pila:



**“Borrar la pila”**: esto sucede cuando alcanza el tamaño máximo de la pila de llamadas. Y eso podría suceder con bastante facilidad, especialmente si está usando recursividad sin probar su código de manera exhaustiva. Echa un vistazo a este código de muestra:

``` javascript
funcion foo() {
    foo();

}
foo ();
```
Cuando el motor comienza este código,comienza llamando a la función “foo”. Esta función, sin embargo, es recursiva y comienza a llamarse a sí misma sin condiciones de terminación. Entonces, en cada paso de la ejecución, la misma función se agrega a la ***pila de llamadas*** una y otra vez. Se ve algo como esto:

En algún momento,sin embargo, la cantidad de llamadas a funciones en ***la pila de llamadas*** excede el tamaño real de la pila de llamadas, y el navegador decide tomar medidas, arrojando un error, que puede verse así:



Ejecutar código en un solo subproceso puede ser bastante fácil, ya que no tiene que lidiar con escenarios complicados que surgen en entornos de subprocesos múltiples, por ejemplo, interbloqueos.
Para ejecutar en un solo hilo también es bastante limitante. Dado que `Javascript tiene una sola pila de llamadas`, ¿qué sucede cuando las cosas son lentas?

### Concurrencia y el bucle de eventos

¿Qué sucede cuando tiene llamadas a funciones en la pila de llamadas que tardan mucho tiempo en procesarse? Por ejemplo, imagina que quieres hacer una transformación de imagen compleja con JavaScript en el navegador.
Puede preguntar: ¿por qué es esto un problema? El problema es que, si bien **Call Stack** tiene funciones para ejecutar, el navegador en realidad no puede hacer nada más: se bloquea. Esto significa que el navegador no puede procesar, no puede ejecutar ningún otro código, simplemente está atascado. Y esto crea problemas si desea interfaces de usuario agradables y fluidas en su aplicación.
Y ese no es el único problema. Una vez que su navegador comience a procesar tantas tareas en **Call Stack**, es posible que deje de responder durante bastante tiempo. Y la mayoría de los navegadores toman medidas al generar un error y le preguntan si desea cerrar la página web.


2. ## Tipos Primitivos:
### Los básicos

Los objetos son agregaciones de propiedades. Una propiedad puede hacer referencia a un objeto o a una primitiva. Las primitivas son valores, no tienen propiedades. 

`En Javascript hay 5 tipos primitivos`: **undefined, null, boolean y todo lo demás es un objeto**. 

***Los tipos primitivos: boolean, string y number pueden ser envueltos por sus contrapartes de objetos.Estos objetos son instancias de los constructores, y respectivamente. string, number, boolean, string, Number.***

Si las primitivas no tienen propiedades, ¿por qué "abc".length, devuelven un valor?
Porque JavaScript coaccionará fácilmente entre primitivos y objetos. En este caso, el valor de la cadena se coacciona a un objeto de cadena para acceder a la longitud de la propiedad. 

**Los tipos primitivos** no tienen métodos adjuntos a ellos; para que nunca veas 
undefined.toString() . También debido a esto, los tipos primitivos son inmutables, porque no tienen métodos adjuntos que puedan mutarlos.
Puede reasignar un tipo primitivo a una variable, pero será un valor nuevo, el anterior no está ni puede ser mutado.
```javascript
const answer = 42
answer.foo = “bar”;
answer.foo; // undefined
```
Los tipos primitivos son inmutables.

Además, los tipos primitivos se almacenan como el valor en sí mismos, a diferencia de los objetos, que se almacenan como una referencia. Esto tiene implicaciones al realizar comprobaciones de igualdad.
``` javascript
"dog" === "dog"; // true
14 === 14; // true

{} === {}; // false
[] === []; // false
(function () {}) === (function () {}); // false
```
Los tipos primitivos se almacenan por valor, los objetos se almacenan por referencia.


3. # Tipos de valor, tipos de referencia y ámbito en JavaScript #

Solo hay dos cosas fundamentales para JavaScript: 

**objetos y funciones.**

(entiende objetos y funciones, y entiendes Javascript)
Las funciones en JavaScript son objetos. De hecho, todo en JavaScript es un objeto. Sin embargo, el lenguaje contiene optimizaciones de tipo específicas conocidas como tipos de valor que tienen una semántica diferente.

**Los tipos de valor** son número, símbolo, booleano, nulo e indefinido. String también es un tipo de valor, aunque se implementa con un comportamiento ligeramente diferente para ahorrar memoria.
Las cadenas se pueden considerar como tipos de valores, pero con la sutil diferencia de que la asignación no copiará el valor completo. En su lugar, las implementaciones copiarán una referencia a una sola representación de la cadena mantenida internamente.
Los objetos son tipos de referencia. Los objetos tienen una semántica de copia por valor de la referencia. 
Los objetos se pueden declarar de dos maneras:

     var o = nuevo objeto();

           o
  
     var o = {};

Las dos anteriores son equivalentes. Esta última se denomina sintaxis literal del objeto. 

La semántica de copia por valor de la referencia significa que las referencias a los objetos se transmiten en lugar de los objetos mismos. Esto se debe a razones de rendimiento, además de ser lo que la mayoría de los desarrolladores esperarían.

## LAS FUNCIONES DEFINEN EL ALCANCE EN JAVASCRIPT.

Explicando el valor frente a la referencia en Javascript.
Una simple mirada a la memoria de la computadora explica lo que está sucediendo

*Javascript tiene 3 tipos de datos que se pasan por referencia :* **Array, Function y Object.** Todos estos son técnicamente Objetos, por lo que nos referiremos a ellos colectivamente como Objetos .

A las variables a las que se le asigna un valor no primitivo se les asigna una referencia a ese valor. Esa referencia apunta la ubicación del objeto en la memoria. Las variables en realidad no contienen el valor.

Cuando asignamos y usamos una variable de tipo de referencia, lo que escribimos y vemos es:
```javascript
var arr = [ ];
arr.push(1);
```

## Asignación por referencia
Cuando un valor de tipo de referencia, un objeto, se copia a otra variable usando =, la dirección de ese valor es lo que realmente se copia como si fuera una primitiva. Los objetos se copian por referencia en lugar de por valor.
![alt text](referencia%20variable.png)

```javascript

var referencia = [1];
 var refCopiar = referencia;
```

Cada variable hora contiene una referencia a la misma matriz. Eso significa que si modificamos reference, refCopy veremos esos cambios:

  ```javascript

        referencia.push(2);
        consola.log(referencia,copiaref);//  ---> [1, 2], [1, 2]
```
![alt text](referencia%20variable%201.png)


Hemos empujado 2 a la matriz en la memoria. Cuando usamos reference y refCopy, estamos apuntando a esa misma matriz.

## Reasignación de una referencia ##
La reasignación de una variable de referencia reemplaza la referencia anterior.

``` javascript
      var obj ={primero: “referencia” };
```
En memoria:
![alt text](referencia%20variable%202.png)

Cuando tenemos una segunda línea:
 ``` javascript
       var obj = { primero: “referencia” };
       obj = { segundo :”ref2” }

```
La dirección almacenada en obj los cambios. El primer objeto todavía está presente en la memoria, al igual que el siguiente objeto:

![alt text](referencia%20variable%203.png)
Cuando no vemos referencias a un objeto, como vemos en la dirección #234 anterior, el motor Javascript puede realizar la recolección de elementos no utilizados. Esto solo significa que el programador ha perdido todas las referencias al objeto y ya no puede usar el objeto, por lo que el motor puede seguir adelante y eliminarlo de la memoria de manera segura. En este caso, el objeto { first: “reference”} ya no es accesible y está disponible para el motor para la recolección de elementos no utilizados.

## == y === ##

Cuando los operadores de igualdad ==y ===, se utilizan en variables de tipo de referencia, verifican la referencia. Si las variables contienen una referencia al mismo elemento, la comparación dará como resultado true.
```javascript
var arrRef = ['¡Hola!'];

var Refarr2 = Refarr;

consola.log(arrRef === arrRef2); // -> verdadero
```
Si son objetos distintos, incluso si contienen propiedades idénticas, la comparación dará como resultado false.
```javascript
var arr1 = ['¡Hola!']; 
var arr2 = ['¡Hola!'];
consola.log(arr1 === arr2); // -> falso
```

Si tenemos dos objetos distintos y queremos ver si sus propiedades son las mismas, la forma más fácil de hacerlo es convertirlos en cadenas y luego compararlas. Cuando los operadores de igualdad comparan primitivas, simplemente verifican si los valores son iguales.
``` javascript
var arr1str = JSON.stringify(arr1); 
var arr2str = JSON.stringify(arr2);
consola.log(arr1str === arr2str); // cierto
```
Otra opción sería recorrer recursivamente los objetos y asegurarse de que cada una de las propiedades sea la misma.

## Funciones puras ##

Nos referimos a funciones que no afectan nada en el ámbito externo como funciones puras . Siempre que una función solo tome valores primitivos como parámetros y no use ninguna variable en su ámbito circundante, automáticamente es pura, ya que no puede afectar nada en el ámbito externo. Todas las variables creadas en el interior se recolectan como basura tan pronto como la función regresa.
Muchas funciones de matrices nativas, incluidas Array.mapy Array.filter, se escriben como funciones puras. Toman una referencia de matriz e internamente, copian la matriz y trabajan con la copia en lugar del original. Esto hace que el original no se toque, el alcance externo no se vea afectado y se nos devuelva una referencia a una nueva matriz.
Veamos un ejemplo de una función pura vs. impura.
``` javascript
función cambiarEdadImpuro(persona) {
           persona.edad = 25; 
           persona de retorno; 
}
var alex = { 
    nombre: 'Alex', 
    edad: 30 
};
var changeAlex = changeAgeImpure(alex);
consola.log(alex); // -> { nombre: 'Alex', edad: 25 } 
console.log(cambiadoAlex); // -> { nombre: 'Alex', edad: 25 }
```
Esta función impura toma un objeto y cambia la propiedad age de ese objeto a 25. Debido a que actúa sobre la referencia que se le dio, cambia directamente el objeto alex. Tenga en cuenta que cuando devuelve el person objeto, devuelve exactamente el mismo objeto que se pasó alex y alex Changed contiene la misma referencia. Es redundante devolver la person variable y almacenar la referencia en una nueva variable.

Veamos una función pura. 
```javascript
function changeAgePure(persona) { 
    var newPersonObj = JSON.parse(JSON.stringify(persona)); 
    newPersonObj.edad = 25; 
    devuelve nuevoObjPersona; 
}
var alex = { 
    nombre: 'Alex', 
    edad: 30 
};
var alexChanged = changeAgePure(alex);
consola.log(alex); // -> { nombre: 'Alex', edad: 30 } 
console.log(alexChanged); // -> { nombre: 'Alex', edad: 25 }
```
En esta función, usamos JSON.stringify para transformar el objeto que pasamos en una cadena y luego lo analizamos nuevamente en un objeto con JSON.parse. Al realizar esta transformación y almacenar el resultado en una nueva variable, creamos un nuevo objeto. Hay otras formas de hacer lo mismo, como recorrer el objeto original y asignar cada una de sus propiedades a un nuevo objeto, pero esta es la forma más sencilla. El nuevo objeto tiene las mismas propiedades que el original pero es un objeto claramente separado en la memoria.
Cuando cambiamos la age propiedad de este nuevo objeto, el original no se ve afectado. Esta función ahora es pura. No puede afectar ningún objeto fuera de su propio alcance, ni siquiera el objeto que se pasó. El nuevo objeto debe devolverse y almacenarse en una nueva variable o, de lo contrario, se recolectará basura una vez que se complete la función, ya que el objeto no es de mayor alcance.

**Pruébate**

Valor versus referencia es un concepto que a menudo se prueba en entrevistas de codificación. Trate de averiguar por sí mismo lo que está registrado aquí.
```javascript
       function cambiarEdadYReferencia(persona) { 
           persona.edad = 25; 
           persona = { 
                   nombre: 'Juan', 
                   edad: 50 
    }; 
    
    persona de retorno; 
}
var personObj1 = { 
    nombre: 'Alex', 
    edad: 30 
};
var personObj2 = changeAgeAndReference (personObj1);
consola.log(personaObj1); // -> ? 
consola.log(personaObj2); // -> ?
```

La función primero cambia la propiedad age del objeto original al que se pasó. Luego, reasigna la variable a un objeto completamente nuevo y devuelve ese objeto. Esto es lo que los dos objetos están desconectados.
```javascript
consola.log(personaObj1); // -> { nombre: 'Alex', edad: 25 } 
console.log(personObj2); // -> { nombre: 'Juan', edad: 50 }
```
Recuerde que la asignación a través de parámetros de función es esencialmente lo mismo que la asignación con =. La variable person en la función contiene una referencia al personObj1objeto, por lo que inicialmente actúa directamente sobre ese objeto. Una vez que reasignamos persona un nuevo objeto, deja de afectar al original.
Esta reasignación no cambia el objeto al que personObj1 apunta en el ámbito externo. person tiene una nueva referencia porque fue reasignada pero esta reasignación no cambia personObj1.
Una pieza de código equivalente al bloque anterior sería:
```javascript
var personObj1 = { 
    nombre: 'Alex', 
    edad: 30 
};
var persona = personObj1; 
persona.edad = 25;
persona = { 
  nombre: 'john', 
  edad: 50 
};
var personObj2 = persona;
consola.log(personaObj1); // -> { nombre: 'Alex', edad: 25 } 
console.log(personObj2); // -> { nombre: 'Juan', edad: '50' }
```
La única diferencia es que cuando usamos la función, person ya no está dentro del alcance una vez que finaliza la función.
__________

# Coerción de tipo JavaScript explicada #

La coerción de tipos es el proceso de convertir valores de un tipo a otro(como cadena a número, objeto a booleano, etc).Cualquier tipo, ya sea primitivo o un objeto, es un sujeto válido para la coerción de tipos. Para recordar, las primitivas son: número, cadena, booleano, nulo, indefinido + símbolo (agregado en ES6).
Como ejemplo de coerción de tipos en la práctica, observe la tabla de comparación de JavaScript== , que muestra cómo se comporta el operador de igualdad flexible para diferentes tipos a y b.
```javascript

true + false
12 / "6"
"number" + 15 + 3
15 + 3 + "number"
[1] > null
"foo" + + "bar"
'true' == true
false == 'false'
null == “ ''
!!"false" == !!"true"
[‘x’] == ‘x’
[] + null + 1
[1,2,3] == [1,2,3]
{}+[]+{}+[1]
!+[]+[]+![]
new Date(0) - 0
new Date(0) + 0
 ```     
En el 90% de los casos de uso, es mejor evitar la coerción de tipos implícita.

### Coerción implícita vs. explícita

Cuando un desarrollador expresa la intención de convertir entre tipos escribiendo el código apropiado, como Number(value), se denomina coerción de tipo explícita( o conversión de tipo).
Dado que JavaScript es un lenguaje de tipo débil, los valores también se pueden convertir entre diferentes tipos automáticamente, y se denomina coerción de tipo implícita. Por lo general, sucede cuando aplica operadores a valores de diferentes tipos, como 1 == null, 2/”5”, null + new date (), o puede activarse por el contexto circundante, como with if (value) {...}, donde value se convierte en booleano.

Un operador que no activa la coerción de tipos implícita es **===** , que se denomina operador de igualdad estricta. El operador de igualdad suelta, == por otro lado, hace tanto la comparación como la coerción de tipo si es necesario.

La coerción de tipos implícita es un arma de doble filo: es una gran fuente de frustración y defectos pero también un mecanismo útil que nos permite escribir menos código sin perder la legibilidad.  

**Tres tipos de conversión**

La primera regla a saber es que solo hay tres tipos de conversión en JavaScript:

 * Encadenar.
 * a booleano.
 * Al número.

En segundo lugar, la lógica de conversión para primitivos y objetos funciona de manera diferente, pero tanto los primitivos como los objetos solo se pueden convertir de esas tres formas.

Comencemos primero con las **primitivas**.

**Conversión de cadenas**

Para convertir valores explícitamente en una cadena, aplique la **String()función**. La coerción implícita es activada por el +operador binario, cuando cualquier operando es una cadena:
```javascript
String(123) // explicit
123 + ''    // implicit
```
Todos los valores primitivos se convierten en cadenas de forma natural, como cabría esperar:
```javascript
String(123)                   // '123'
String(-12.3)                 // '-12.3'
String(null)                  // 'null'
String(undefined)             // 'undefined'
String(true)                  // 'true'
String(false)                 // 'false'
```
La conversión de símbolos es un poco complicada, porque solo se puede convertir explícitamente, pero no implícitamente. 
Lea más sobre Symbollas reglas de coerción.
``` javascript
String(Symbol('my symbol'))   // 'Symbol(my symbol)'
'' + Symbol('my symbol')      // TypeError is thrown
```
**conversión booleana**

Para convertir explícitamente un valor en booleano, aplique la **Boolean()función**.
La conversión implícita ocurre en un contexto lógico o se desencadena mediante operadores lógicos ( || && !) .

```javascript
Boolean(2)          // explicit
if (2) { ... }      // implicit due to logical context
!!2                 // implicit due to logical operator
2 || 'hello'        // implicit due to logical operator
```
Nota : los operadores lógicos como || y & & realizan conversiones booleanas internamente, pero en realidad devuelven el valor de los operandos originales , incluso si no son booleanos.
```javascript
// returns number 123, instead of returning true
// 'hello' and 123 are still coerced to boolean internally to calculate the expression
let x = 'hello' && 123;   // x === 123
```
Tan pronto como solo haya 2 resultados posibles de conversión booleana: true o false, es más fácil recordar la lista de valores falsos.
```javascript
Boolean('')           // false
Boolean(0)            // false     
Boolean(-0)           // false
Boolean(NaN)          // false
Boolean(null)         // false
Boolean(undefined)    // false
Boolean(false)        // false
```
Cualquier valor que no esté en la lista se convierte en true, incluido el objeto, la función, Array, el Date tipo definido por el usuario, etc. Los símbolos son valores de verdad. Los objetos vacíos y las matrices también son valores verdaderos:
```javascript
Boolean({})             // true
Boolean([])             // true
Boolean(Symbol())       // true
!!Symbol()              // true
Boolean(function() {})  // true
```
**Conversión numérica**

Para una conversión explícita, simplemente aplique la **Number()función**, igual que hizo con **Boolean()y String().**

La conversión implícita es complicada, porque se activa en más casos:
```javascript
operadores de comparación ( >, <, <=, >=)
operadores bit a bit ( | & ^ ~)
operadores aritméticos ( - + * / %). 
Tenga en cuenta que el binario + no desencadena la conversión numérica, cuando cualquier operando es una cadena.
+ operador unario
operador de igualdad flexible ==(incl. !=).
Tenga en cuenta que == no activa la conversión numérica cuando ambos operandos son cadenas.
Number('123')   // explicit
+'123'          // implicit
123 != '456'    // implicit
4 > '5'         // implicit
5/null          // implicit
true | 0        // implicit
```
Así es como los valores primitivos se convierten en números:
```javascript
Number(null)                   // 0
Number(undefined)              // NaN
Number(true)                   // 1
Number(false)                  // 0
Number(" 12 ")                 // 12
Number("-12.34")               // -12.34
Number("\n")                   // 0
Number(" 12s ")                // NaN
Number(123)                    // 123
```
Al convertir una cadena en un número, el motor primero recorta los espacios en blanco iniciales y finales \n, \t caracteres, y regresa NaN si la cadena recortada no representa un número válido. Si la cadena está vacía, devuelve 0.

null y undefined se manejan de manera diferente: null se convierte en 0, mientras que undefined se convierte en NaN.

Los símbolos no se pueden convertir en un número ni explícita ni implícitamente. Además, TypeError se lanza, en lugar de convertirse silenciosamente en NaN, como sucede con undefined. Vea más sobre las reglas de conversión de símbolos en MDN .
```javascript
Number(Symbol('my symbol'))    // TypeError is thrown
+Symbol('123')                 // TypeError is thrown 
```
**Hay dos reglas especiales para recordar:**

Cuando se aplica == a null o undefined, la conversión numérica no ocurre. null es igual solo a null o undefined, y no es igual a nada más.
null == 0               // false, null is not converted to 0
null == null            // true
undefined == undefined  // true
null == undefined       // true

`NaN no es igual a nada ni siquiera a sí mismo:`
```javascript
if (value !== value) { console.log("we're dealing with ```NaN here”)} 
```
Coerción de tipos para objetos
Cuando se trata de objetos y el motor encuentra expresiones como [1] + [2,3], primero necesita convertir un objeto a un valor primitivo, que luego se convierte al tipo final. Y aún así, solo hay tres tipos de conversión: numérica, de cadena y booleana.
El caso más simple es la conversión booleana: cualquier valor no primitivo siempre se fuerza a true, sin importar si un objeto o una matriz están vacíos o no.

Los objetos se convierten en primitivos a través del [[**ToPrimitive**]]método interno, que es responsable de la conversión numérica y de cadenas.

Aquí hay una pseudo implementación del [[**ToPrimitive**]]método:

[[**ToPrimitive**]]se pasa con un valor de entrada y tipo preferido de conversión: Number o String. preferredType es opcional.

Tanto la conversión numérica como la de cadena utilizan dos métodos del objeto de entrada: **valueOf y toString**. Ambos métodos están declarados *Object.prototype* y, por lo tanto, están disponibles para cualquier tipo derivado, como Date, Array, etc.

En general el algoritmo es el siguiente:

      1. Si la entrada ya es una primitiva, no haga nada y devuélvala.
      2. Llame input.toString(), si el resultado es primitivo, devuélvalo.

      3. Llame input.valueOf(), si el resultado es primitivo, devuélvalo.

      4. Si ni input.toString()ni input.valueOf() da primitivo, tira TypeError.

La conversión numérica primero llama a **valueOf(3)** con un respaldo a **toString(2)**. La conversión de cadenas hace lo contrario: **toString(2)** seguido de **valueOf(3)**.

La mayoría de los tipos incorporados no tienen valueOf, o no tienen un objeto de valueOf retorno this en sí mismo, por lo que se ignora porque no es un primitivo. Es por eso que la conversión numérica y de cadena podría funcionar igual: ambas terminan llamando a toString().

Diferentes operadores pueden desencadenar una conversión numérica o de cadena con la ayuda de un preferredType parámetro. Pero hay dos excepciones: la igualdad flexible y los operadores == binarios desencadenan modos de conversión predeterminados ( no se especifica o es igual a ). En este caso, la mayoría de los tipos incorporados asumen la conversión numérica como opción predeterminada, excepto que hace la conversión de cadenas.+preferredType default Date

Este es un ejemplo de Date comportamiento de conversión:

Puede anular el valor predeterminado toString() y los valueOf() métodos para enlazar con la lógica de conversión de objeto a primitivo.

Observe cómo obj + ‘ ’   regresa ‘101’ como una cadena. +El operador activa un modo de conversión predeterminado y, como se dijo antes, Object asume la conversión numérica como predeterminada, por lo que usa el valueOf()método primero en lugar de toString().

En ES6 puede ir más allá y reemplazar [[**ToPrimitive**]]por completo la rutina interna implementando el [**Symbol.Primitive**] método en un objeto.

Ejemplos

Armados con la teoría, ahora volvamos a nuestros ejemplos:
```javascript
true + false             // 1
12 / "6"                 // 2
"number" + 15 + 3        // 'number153'
15 + 3 + "number"        // '18number'
[1] > null               // true
"foo" + + "bar"          // 'fooNaN'
'true' == true           // false
false == 'false'         // false
null == ''               // false
!!"false" == !!"true"    // true
['x'] == 'x'             // true 
[] + null + 1            // 'null1'
[1,2,3] == [1,2,3]       // false
{}+[]+{}+[1]             // '0[object Object]1'
!+[]+[]+![]              // 'truefalse'
new Date(0) - 0          // 0
new Date(0) + 0          // 'Thu Jan 01 1970 02:00:00(EET)0'
```
_____________
# Ámbito de función, ámbito de bloque y ámbito léxico #

*var* siempre ha tenido este aura especial de concepto erróneo, probablemente debido a cómo el comportamiento de las variables declaradas con  var  se distingue
 de la mayoría de los otros lenguajes de programación. Dicho esto, todo tiene una explicación bastante natural: el alcance. 

### *var*— alcance de la función ###

Como se mencionó, una variable que se declara usando var tendrá un ámbito de función, lo que significa que existirá dentro del ámbito de la función en la que se declara.
```javascript
function myFunc() {  
  var name = 'Luke'
  console.log(name); // 'Luke'
}

myFunc();
console.log(name); // name is not defined  

Como puede ver, la variable declarada var dentro de la función no es accesible desde fuera de la función.

Dicho esto, otros tipos de bloques, como declaraciones if, bucles, etc., no se considerarán como un alcance.

if(true) {  
  var name = 'Luke'
}
console.log(name); // 'Luke'  
```
Al usar *var*, la variable name está disponible fuera de la instrucción if en la que se declaró. Esto se debe a que están en el mismo ámbito.
Sin embargo, con la introducción de ES6, se introdujeron dos nuevas formas de declarar variables.
let y const— la introducción del alcance del bloque
En ES6, lety constse introdujeron como formas alternativas de declarar variables, ambas con alcance bloqueado.
Esto probablemente resonará mucho mejor contigo si estás acostumbrado a cualquier otro lenguaje que no sea JavaScript.
En el alcance del bloque, cualquier bloque será un alcance. Esto le dará un comportamiento más consistente.
Esto significa que una función sigue siendo un ámbito válido al igual que con var.
```javascript
function myFunc() {  
  let name = 'Luke'
  console.log(name); // 'Luke'
}

myFunc();

console.log(name); // name is not defined 
```
Pero en este caso también otro tipo de bloques califican como alcance, como las declaraciones if. 
```javascript
if(true) {  
  let name = 'Luke'
}
console.log(name); // name is not defined 
```
Cuando el alcance de la función se vuelve confuso
Ahora que cubrimos la diferencia entre el alcance de la función y el alcance del bloque, veamos por qué esto puede volverse confuso rápidamente.

Tener una variable local dentro de un ámbito con el mismo nombre que una variable en el ámbito externo está perfectamente bien.
```javascript
var name = 'Luke';

const func = () => {  
  var name = 'Phil';
  console.log(name); // 'Phil'
}

func();

console.log(name); // 'Luke' 
```
Como era de esperar, name en el ámbito externo mantiene el valor de la declaración inicial 'Luke' incluso después func de que se haya ejecutado, que contiene una variable local con el mismo nombre.

Sin embargo, el problema es que dado que el alcance de la función solo cubre funciones y no otros tipos de bloques, obtendríamos un comportamiento bastante diferente con otros bloques.
```javascript
var name = 'Luke';

if (true) {  
  var name = 'Phil';
  console.log(name); // 'Phil'
}

console.log(name); // 'Phil' 
```
En este escenario, se imprimirá 'Phil' en ambos lugares. Esto se debe a que ambas variables están en el mismo ámbito, lo que hace que 'Phil' anule la declaración de la primera variable.

Como puede imaginar, con el aumento de la complejidad, esto podría convertirse rápidamente en un verdadero dolor de cabeza.

Brindando consistencia con el alcance bloqueado
Si observamos  let, que tiene un alcance de bloque,esto se mantendría consistente para todos los bloques.
```javascript
let name = 'Luke';

const func = () => {  
  let name = 'Phil';
  console.log(name); // 'Phil'
}

func();

console.log(name); // 'Luke' 

let name = 'Luke';

if (true) {  
  let name = 'Phil';
  console.log(name); // 'Phil'
}

console.log(name); // 'Luke' 
```
### ¿Qué pasa con los bucles?

Echemos un vistazo a otro ejemplo para comprender realmente los diferentes comportamientos.

Digamos que queremos hacer un bucle que envíe funciones perezosas a una matriz. Cada una de estas funciones imprimirá el índice actual.

Comencemos por ver qué pasaría si usáramos var.
```javascript
var printsToBeExecuted = [];

for (var i = 0; i < 3; i++) {  
  printsToBeExecuted.push(() => console.log(i));
}

printsToBeExecuted.forEach(f => f());  
// Output: 3, 3, 3
```
Nuevamente, si está acostumbrado a bloquear el alcance, esto se sentiría un poco extraño. ¿Esperarías 0, 1, 2, verdad?

La explicación es simplemente que un bucle no es un alcance cuando se usa var. Entonces, en lugar de crear una variable local ipara cada incremento, terminará imprimiendo el valor final de la variable para todas las funciones.

Una solución que funcionaría sería envolver la función dentro de otra función y luego ejecutarla directamente. De esta manera obtendríamos un alcance adecuado para cada elemento.
```javascript
var printsToBeExecuted = [];

for (var i = 0; i < 3; i++) {  
  printsToBeExecuted.push(
    ((ii) => () => console.log(ii))(i));
}

printsToBeExecuted.forEach(f => f());  
// Output: 0, 1, 2
```
Genial, obtuvimos el resultado que esperábamos, pero fue un poco detallado, ¿no?

Si ahora miramos una solución usando el bloque con alcance let para la variable de iteración, obtendríamos la simplicidad del primer ejemplo así como el resultado esperado.
```javascript
var printsToBeExecuted = [];

for (let i = 0; i < 3; i++) {  
  printsToBeExecuted.push(() => console.log(i));
}

printsToBeExecuted.forEach(f => f());  
// Output: 0, 1, 2
```
## Alcance en JavaScript ##

La comprensión scope hará que su código se destaque, reducirá los errores y lo ayudará a crear patrones de diseño poderosos con él. 

*¿Qué es el alcance?*

El alcance es la accesibilidad de variables, funciones y objetos en alguna parte particular de su código durante el tiempo de ejecución. En otras palabras, el alcance determina la visibilidad de las variables y otros recursos en áreas de su código. 
Alcance en JavaScript
En el lenguaje JavaScript existen dos tipos de ámbitos:

### Alcance global - Ámbito local ###

Las variables definidas dentro de una función están en el **ámbito local**, mientras que las variables definidas fuera de una función están en el **ámbito global**. Cada función, cuando se invoca, crea un nuevo ámbito.



*Alcance global*

Cuando comienza a escribir JavaScript en un documento, ya se encuentra en el ámbito Global. Solo hay un alcance global en todo un documento de JavaScript. Una variable está en el ámbito global si se define fuera de una función.
```javascript
// the scope is by default global
var name = 'Hammad';
```
Se puede acceder a las variables dentro del ámbito global y modificarlas en cualquier otro ámbito. 
```javascript
var name = 'Hammad';

console.log(name); // logs 'Hammad'

function logName() {
    console.log(name); // 'name' is accessible here and everywhere else
}

logName(); // logs 'Hammad'
```
*Ámbito local*

Las variables definidas dentro de una función están en el ámbito local. Y tienen un alcance diferente para cada llamada de esa función. Esto significa que las variables que tienen el mismo nombre se pueden usar en diferentes funciones. Esto se debe a que esas variables están vinculadas a sus respectivas funciones, cada una con diferentes ámbitos, y no son accesibles en otras funciones.
```javascript
// Global Scope
function someFunction() {
    // Local Scope #1
    function someOtherFunction() {
        // Local Scope #2
    }
}

// Global Scope
function anotherFunction() {
    // Local Scope #3
}
// Global Scope
```
*Declaraciones en bloque*

Bloquear declaraciones como if y switch condiciones o for y while bucles, a diferencia de las funciones, no crean un nuevo alcance. Las variables definidas dentro de una declaración de bloque permanecerán en el ámbito en el que ya estaban.
```javascript
if (true) {
    // this 'if' conditional block doesn't create a new scope
    var name = 'Hammad'; // name is still in the global scope
}

console.log(name); // logs 'Hammad'
```
Al contrario de la  palabra clave var , las palabras clave let y const admiten la declaración de alcance local dentro de las declaraciones de bloque.
```javascript
if (true) {
    // this 'if' conditional block doesn't create a scope

    // name is in the global scope because of the 'var' keyword
    var name = 'Hammad';
    // likes is in the local scope because of the 'let' keyword
    let likes = 'Coding';
    // skills is in the local scope because of the 'const' keyword
    const skills = 'JavaScript and PHP';
}

console.log(name); // logs 'Hammad'
console.log(likes); // Uncaught ReferenceError: likes is not defined
console.log(skills); // Uncaught ReferenceError: skills is not defined
```
### Contexto

Muchos desarrolladores a menudo confunden alcance y contexto como si se refieren igualmente a los mismos conceptos. Pero este no es el caso. El alcance es lo que discutimos anteriormente y Contexto se usa para referirse al valor de **this** alguna parte particular de su código. El alcance se refiere a la visibilidad de las variables y el contexto se refiere al valor de **this** en el mismo alcance. También podemos cambiar el contexto usando métodos de función, que discutiremos más adelante. En el contexto de ámbito global siempre es el objeto Ventana.
```javascript
// logs: Window {speechSynthesis: SpeechSynthesis, caches: CacheStorage, localStorage: Storage…}
console.log(this);

function logFunction() {
    console.log(this);
}
// logs: Window {speechSynthesis: SpeechSynthesis, caches: CacheStorage, localStorage: Storage…}
// because logFunction() is not a property of an object
log Function();
Si el alcance está en el método de un objeto, el contexto será el objeto del que forma parte el método.
```


