# Expresión vs Declaración #
Es importante hacer esta distinción porque las expresiones pueden actuar como declaraciones, razón por la cual también tenemos declaraciones de expresión. Aunque, por otro lado, las declaraciones no pueden actuar como expresiones. 

## Expresiones ##
**Las expresiones producen valor**
Las expresiones son fragmentos de código Javascript que dan como resultado un único valor. Las expresiones pueden ser tan largas como desee, pero siempre darán como resultado un único valor.
```javascript
 2 + 2 * 3 / 2
 (Math.random() * (100 - 20)) + 20

 functionCall()

 window.history ? useHistory() : noHistoryFallback()

 1+1, 2+2, 3+3

 declareVariable

 true && functionCall ()

 true && declareVariable
```

Todo lo anterior son expresiones y pueden aparecer en cualquier lugar donde Javascript espere un valor. Para que el argumento a `console.log`continuación se resuelva en un solo valor, que se registra en la consola. 
```javascript
console.log(true && 2 * 9)//18
```

### Las expresiones no necesariamente cambian de estado ###
por ejemplo,
```javascript
const assigedVariable = 2; //this is a statement, assignedVariable
assignedVariable + 4 //expression
assignedVariable * 10 //expression
assignedVariable - 10 //expression

console.log(assignedVariable) // 2
```
A pesar de todas las expresiones en el fragmento anterior, el valor de la variable asignada sigue siendo 2. Entonces ¿Por qué `necessarily` en el encabezado de esta sección? Es porque las llamadas a funciones son expresiones, pero una función puede contener declaraciones que cambian de estado. Entonces `foo()`, en sí misma es una expresión, que devuelve indefinido o algún otro valor pero si `foo`se escribió como 
```javascript
    const foo = foo () => {
    assignedVariable = 14
    } 
 ```

entonces, aunque su llamado es una expresión, su llamado también ha resultado en un cambio de estado. Entonces, una mejor manera de reescribir la función y la declaración foo sería :
``` javascript
const foo = ()=> { 
    return 14 // explicit return for readability
    }
    assignedVariable = foo ()
 ```
    o mejor 
```javascript
const foo = foo(n) => { 
    return n// explicit return for readability
    }
    assignedVariable = foo(14)
```

De esta manera, su código es más legible, componible y hay una clara distinción y separación entre expresión e instrucciones. Este es un fundamento de Javascript funcional y declarativo

## Declaraciones

Las declaraciones son el dolor de cabeza de la programación función ðŸ˜„.
Básicamente, las declaraciones realizan acciones, hacen cosas. 

En javaScript, las declaraciones nunca se pueden usar donde se espera un valor. Por lo tanto, no se pueden usar como argumentos de función, lado derecho de asignaciones, operadores operandos, valores de retorno...
```javascript
foo (if() { return 2}) // js engine mind = blown
 ```
 Estas son todas las declaraciones de javascript:

1. si
2. si-más
3. tiempo
4. hacer mientras
5. por
6. cambiar
7. para adentro
8. con (obsoleto)
9. depurador
10. declaración de variables

Si escribe el fragmento a continuación en la consola de su navegador y presiona enter 
```javascript
if (true) {9+9}
```
verá que regresa `18`pero a pesar de eso no puede usarlo como una expresión o donde javascript espera un valor. Es extraño porque esperaría que las declaraciones no devuelvan nada, ya que el valor de retorno es bastante inútil si no puede usarlo. Eso es Javascript para ti, raro.

### Declaraciones de funciones, expresiones de funciones y expresiones de funciones con nombre

Una declaración de función es una declaración
```javascript
function foo (func) { 
    return func.name
}
```
Una expresión de función es una expresión, lo que llamas una función anónima
```javascript
console.log(function() {})) // ""
```
Una expresión de función con nombres es una expresión, como función anónima, pero tiene un nombre
```javascript
console.log(foo(function myName() {})) // "myName"
```
La distinción entre la función como expresión y la función como declaración se reduce a comprender esto:

**cada vez que declara la función donde Javascript espera un valor. Intentará tratarla como un valor. Si no puede usarla como un valor se arrojará un error.
Mientras que declarar una función en el nivel global de un script, módulo o nivel superior de una declaración de bloque ( es decir, donde no espera un valor), dará como resultado una declaración de función.**

ejemplos:
```javascript
if () { 
 function foo () {} // top level of block, declaration
}
 function foo () {} // global level, declaration

 function foo () { 
   function bar () {} // top level of block, declaration
}

function foo () { 
return function bar () {} //named function expression
}

foo (function () {}) // anonymous function expression 

function foo() { 
 return function bar () { 
  function baz () {} // top level of block, declaration 
  }
}

function () {} // SyntaxError: function statement requires a name

if (true){
  function () {} //SyntaxError: function statement requires a name
}
```
### Conversión de expresiones en declaraciones: declaraciones de expresión

¿Hay algo simple y  directo con Javascript?
```javascript
2 + 2; //expression statement
foo(); // expression statement
```
Puede convertir expresiones en declaraciones de expresión, simplemente agregando un punto y coma al final de la línea o permitiendo que la inserción automática de punto y coma haga el trabajo. `2+2`en sí mismo es una expresión, pero la línea completa es una declaración.
```javascript

2+2 //on its own is an opposition
foo(2+2) // so you can use it anywhere a value is expected

true ? 2+2 : 1+1

function foo () {return 2+2}

2+2; // expression statement
foo(2+2;)//syntaxError
```

### Operador punto y coma vs coma
Con punto y com, puede mantener varias declaraciones en la misma línea.
```javascript
const a; function foo () {}; const b = 2
```
El operador de coma le permite encadenar varias expresiones, devolviendo solo la última expresión.
```javascript
console.log( (1+2,3,4) ) //4
console.log( (2, 9/3, function () {}) ) // function (){}
console.log( (3, true ? 2+2 : 1+1) ) // 4
```
Nota al margen: una forma de decirle al motor de javascript que espere un valor es a través de paréntesis, (), sin los paréntesis, cada expresión se tratará como un argumento para console.log.
```javascript
function foo () {return 1, 2, 3, 4}
foo () //4
```
Todas las expresiones se evaluarán de izquierda a derecha y se devolverá la última.

### IIFE (Expresiones de función invocadas inmediatamente)
Una función anónima puede ser una expresión, si la usamos donde Javascript espera un valor, eso significa que si podemos decirle a Javascript que espere un valor entre paréntesis, podemos pasar una función anónima como ese valor. 
```javascript
function () {}
```
Entonces, aunque el fragmento de código anterior no es válido, el fragmento de código siguiente es válido. 
```javascript
(function () {}) // this returns function () {}
```
Si poner una función anónima entre paréntesis devuelve inmediatamente la misma función anónima, eso significa que podemos llamarla de inmediato, así:
```javascript
(function () { 
    //do something
})()
```
Entonces, estos son posibles
```javascript
(function () {
  console.log("inmediately invoke anonymous function call")
})() // "inmediately invoke anonymous function call"

(function () { 
 return 3
})()) // 3

//you can also pass an argument to it
(function (a) { 
 return a

})("I´m an argument") //I´m an argument
```



# Función de JavaScript: declaración frente a expresión

Las funciones se consideran ciudadanos de primera clase en Javascript y es muy importante tener claro el concepto de creación de funciones en JS. 

A diferencia de otros lenguajes de programación, podemos crear funciones en JavaScript usando 3 formas distintas:

1. Declaración de función
2. Expresión de función
3. Expresión de función nombrada


1. ### Declación de función

``` javascript
function [nombre] (param1, param2, ...param3) {
  // Cuerpo y lógica de la función
} 
```
En este caso, anteponemos **[ palabra clave de función] al [nombre de función]** respectivo .
Una de las principales ventajas de este enfoque es que la función completa es Hoisted .
### Por eso, podemos ejecutar la función antes de declararla.

Es útil cuando desea abstraer algo de lógica en el cuerpo de una función y la implementación real se realizará en algún momento posterior.
```javascript
  var num1 = 10;
  var num2 = 20;

  var resultado = agregar (num1, num2); // ==> 30 (ejecutar antes de declarar)

  function agregar (param1, param2) { 
    return param1 + param2 ;
    }
  ```
práctica recomendada: siempre declare primero la función y luego ejecútela en lugar de depender de JavaScript Hoisting.


## Expresión de función

Cualquier declaración que asigna algún valor a alguna otra variable se considera como una expresión.
```javascript
var a = 100;
var b = "hola mundo";
```
En el caso de Expresión de función, se crea una función sin ningún nombre y luego se asigna a una variable.
```javascript
var [nombre] = function (param1, param2, ...param3) { 
  //cuerpo y lógica de la función
}

foo(1,3,4);
```
**Limitación**: la función no se puede ejecutar hasta que se defina.

```javascript
var num1 = 10;
var num2 = 20;

var resultado = agregar (num1, num2);
//TypeError no detectado: agregar no es una función

var add = function(param1,param2) {
  return param1 + param2;
}
```
El siguiente enfoque funcionará.

```javascript
var num1 = 10;
var num2 = 20;

var add= function (param1, param2) {
  return param1 + param2 ;
}

var resultado = agregar (num1, num2); // ==> 30
```
### Expresiones de función con nombre: combinación de ambos enfoques
Ahora que comprende las diferencias entre la función Declaración y Expresión, exploremos que sucede cuando mezclamos ambas.
```javascript
var num1 = 10;
var num2 = 20;

var addvariable = function addfunction (param1, param2) { 
  return param1 + param 2;
}
```
Preste mucha atención al hecho de que la función ya **NO** es anónima y tiene un nombre como **addFunction**. 
Además, se asigna a una variable: **addVariable**.

*Ahora, solo porque haya agregado un nombre a la palabra clave function, eso no significa que pueda ejecutarse con ese nombre*.
```javascript
var resultado = addFunction (num1, num2);
// ==>Error de referencia no capturado: addFunction no está definido.
```
Más bien,  solo se puede usar el nombre de variable asignado **addVariable** para referirlo y ejecutarlo.
```javascript
var resultado = addVariable (num1, num2);
// ==> 30
```
#### Puntos importantes a considerar:

1. **addFunction** eclipsa **addVariable** en la pila de llamadas de las herramientas de depuración.
![visual studio code](addfunction.png)

2. En el caso de la expresión de función con nombre, llamar a **addFunction() desde afuera** arroja un error
Error de referenca no capturado: addFunction no está definido

Pero puedes llamarlo desde dentro de la función y eso funcionará
```javascript
var num1 = 10;
var num2 = 20;
var addVariable = function addFunction (param1, param2) {
  var res = param1 + param2;
  if (res === 30) {
      res = addFunction (res, 10);
  }
  return res;
}
var result = addVariable(num1, num2); // ==> 40
```

En este escenario, estamos verificando si res es 30.
Si es así, llamamos a la misma función nuevamente por su nombre addFunction con 30 y 10 como parámetros que devolverán 40 como resultado final.
3. **Una advertencia seria** de usar la expresión de función con nombre en IE8 y versiones anteriores es que crea por error dos objetos de función completamente separados en dos momentos completamente diferentes (doble toma).
