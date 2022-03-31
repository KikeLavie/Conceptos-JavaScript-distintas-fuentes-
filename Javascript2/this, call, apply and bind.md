# **This, call, apply and bind**

## **Call**
El  `call() ` método llama a una función con un  `this ` valor dado y argumentos proporcionados
individualmente.
 ```javascript
 function Product(name, price) {
  this.name = name;
  this.price = price;
}

function Food(name, price) {
  Product.call(this, name, price);
  this.category = 'food';
}

console.log(new Food('cheese', 5).name);
// expected output: "cheese"
 ```
 ### **sintaxis**
  `call()
call(thisArg)
call(thisArg, arg1)
call(thisArg, arg1, arg2)
call(thisArg, arg1, ... , argN)
 `

 ### **Parámetros**
 El valor a usar como  `this` cuando se llama a  `func`.

### **Descripción**
El  `call()` permite que una función/ método perteneciente a un objeto sea asignado y llamado para un objeto diferente.

**call()** proporciona un nuevo valor de  `this` a la función/método. Con  `call()`,puede escribir un método una vez y luego heredarlo en otro objeto, sin tener que volver a escribir el método para el nuevo objeto.

## **Bind**
El  `bind()` método crea una nueva función que, cuando se llama, tiene su  `this` palabra clave establecida en el valor proporcionado, con una secuencia de argumentos que preceden a los proporcionados cuando se llama a la nueva función.

 ```javascript
 const module = {
  x: 42,
  getX: function() {
    return this.x;
  }
};

const unboundGetX = module.getX;
console.log(unboundGetX()); // The function gets invoked at the global scope
// expected output: undefined

const boundGetX = unboundGetX.bind(module);
console.log(boundGetX());
// expected output: 42
```
### **Sintaxis**
`bind(thisArg)
bind(thisArg, arg1)
bind(thisArg, arg1, arg2)
bind(thisArg, arg1, ... , argN)
`
### **Descripción**
La `bind()` función crea una nueva función enlazada, que es un objeto de función exótico (un término de ECMAScript 2015) que envuelve el objeto de función original. Llamar a la función enlazada generalmente da como resultado la ejecución de su función envuelta.
Una función enlazada tiene las siguientes propiedades internas:

`[[BoundTargetFunction]]`
El objeto de función envuelto

`[[BoundThis]]`
El valor que siempre se pasa como thisvalor al llamar a la función envuelta.

`[[BoundArguments]]`
Una lista de valores cuyos elementos se utilizan como primeros argumentos de cualquier llamada a la función envuelta.

`[[Call]]`
Ejecuta código asociado con este objeto. Se invoca a través de una expresión de llamada de función. Los argumentos del método interno son un thisvalor y una lista que contiene los argumentos pasados ​​a la función por una expresión de llamada.

Cuando se llama a una función enlazada, llama al método interno `[Call]]` con los `[[BoundTargetFunction]]` siguientes argumentos `Call(boundThis, ...args)`. Donde `boundThis `es `[[BoundThis]]`, `args` es `[[BoundArguments]]`, seguido de los argumentos pasados ​​por la llamada a la función.

También se puede construir una función enlazada utilizando el `new` operador. 

### **Ejemplos**
**Creando una función enlazada**

El uso más simple de `bind()` es hacer una función que, sin importar cómo se llame, se llama con un `this` valor particular.

Un error común para los nuevos programadores de JavaScript es extraer un método de un objeto, luego llamar a esa función y esperar que use el objeto original como suyo `this`(por ejemplo, usando el método en código basado en devolución de llamada).

Sin embargo, sin un cuidado especial, el objeto original suele perderse. Crear una función enlazada a partir de la función, utilizando el objeto original, resuelve perfectamente este problema:

```javascript
this.x = 9;    // 'this' refers to global 'window' object here in a browser
const module = {
  x: 81,
  getX: function() { return this.x; }
};

module.getX();
//  returns 81

const retrieveX = module.getX;
retrieveX();
//  returns 9; the function gets invoked at the global scope

//  Create a new function with 'this' bound to module
//  New programmers might confuse the
//  global variable 'x' with module's property 'x'
const boundGetX = retrieveX.bind(module);
boundGetX();
//  returns 81
```

## **Apply**
El `apply()` método llama a una función con un `this` valor dado y se `arguments` proporciona como una matriz (o un objeto similar a una matriz ).
```javascript
const numbers = [5, 6, 2, 3, 7];

const max = Math.max.apply(null, numbers);

console.log(max);
// expected output: 7

const min = Math.min.apply(null, numbers);

console.log(min);
// expected output: 2
```
### **Sintaxis**
`
apply(thisArg)
apply(thisArg, argsArray)
`
### **Descripción**
Puede asignar un `this` objeto diferente al llamar a una función existente. `this` se refiere al objeto actual (el objeto que llama). Con `apply`, puede escribir un método una vez y luego heredarlo en otro objeto, sin tener que volver a escribir el método para el nuevo objeto.

applyes muy similar a `call()`, excepto por el tipo de argumentos que admite. Utiliza una matriz de argumentos en lugar de una lista de argumentos (parámetros). Con `apply`, también puede usar un literal de matriz, por ejemplo, `func.apply(this, ['eat', 'bananas'])` o un `Array`objeto, por ejemplo, `func.apply(this, new Array('eat', 'bananas'))`.

También puede utilizar `arguments`para el `argsArray` parámetro. `arguments` es una variable local de una función. Se puede utilizar para todos los argumentos no especificados del objeto llamado. Por lo tanto, no tiene que conocer los argumentos del objeto llamado cuando usa el `apply` método. Puede usar `arguments` para pasar todos los argumentos al objeto llamado. El objeto llamado es entonces responsable de manejar los argumentos.

Desde ECMAScript 5th Edition, también puede usar cualquier tipo de objeto que sea similar a una matriz. En la práctica, esto significa que tendrá una `length` propiedad y propiedades enteras ("índice") en el rango `(0..length - 1)`. Por ejemplo, podría usar un `NodeList` objeto personalizado como `{ 'length': 2, '0': 'eat', '1': 'bananas' }`.

## **Ejemplos**
### **Usar aplicar para agregar una matriz a otra**
Puede usar pushpara agregar un elemento a una matriz. Y, dado que `push` acepta un número variable de argumentos, también puede insertar varios elementos a la vez.

Pero, si pasa una matriz a `push`, en realidad agregará esa matriz como un solo elemento, en lugar de agregar los elementos individualmente. Entonces terminas con una matriz dentro de una matriz.

¿Qué pasa si eso no es lo que quieres? `concat` tiene el comportamiento deseado en este caso, pero no se agrega a la matriz existente ; en su lugar, crea y devuelve una nueva matriz.

Pero quería agregar a la matriz existente... ¿Y ahora qué? ¿Escribir un bucle? ¿Seguramente no?

`apply`¡al rescate!

`const array = ['a', 'b'];
const elements = [0, 1, 2];
array.push.apply(array, elements);
console.info(array); // ["a", "b", 0, 1, 2]`

### **Uso de funciones integradas y de aplicación**
El uso inteligente de `apply` le permite utilizar funciones integradas para algunas tareas que, de otro modo, probablemente se habrían escrito recorriendo los valores de la matriz.

Como ejemplo, aquí están `Math.max/ Math.min`, que se utilizan para averiguar el valor máximo/mínimo en una matriz.
```javascript
// min/max number in an array
const numbers = [5, 6, 2, 3, 7];

// using Math.min/Math.max apply
let max = Math.max.apply(null, numbers);
// This about equal to Math.max(numbers[0], ...)
// or Math.max(5, 6, ...)

let min = Math.min.apply(null, numbers);

// vs. simple loop based algorithm
max = -Infinity, min = +Infinity;

for (let i = 0; i < numbers.length; i++) {
  if (numbers[i] > max) {
    max = numbers[i];
  }
  if (numbers[i] < min) {
    min = numbers[i];
  }
}
```

Pero tenga cuidado: al usar applyesta forma, corre el riesgo de exceder el límite de longitud de argumento del motor de JavaScript. Las consecuencias de aplicar una función con demasiados argumentos (es decir, más de decenas de miles de argumentos) varían según los motores.

Esto se debe a que el límite (y, de hecho, incluso la naturaleza de cualquier comportamiento de pila excesivamente grande) no está especificado. Algunos motores lanzarán una excepción. De forma más perniciosa, otros limitarán arbitrariamente el número de argumentos realmente pasados ​​a la función aplicada. Para ilustrar este último caso: si dicho motor tuviera un límite de cuatro argumentos (los límites reales son, por supuesto, significativamente más altos), sería como si los argumentos 5, 6, 2, 3se hubieran pasado a applylos ejemplos anteriores, en lugar de a la matriz completa.

Si su matriz de valores puede crecer a decenas de miles, use una estrategia híbrida: aplique su función a partes de la matriz a la vez:
```javascript
function minOfArray(arr) {
  let min = Infinity;
  let QUANTUM = 32768;

  for (var i = 0, len = arr.length; i < len; i += QUANTUM) {
    var submin = Math.min.apply(null,
                                arr.slice(i, Math.min(i+QUANTUM, len)));
    min = Math.min(submin, min);
  }

  return min;
}

let min = minOfArray([5, 6, 2, 3, 7]);
```

### **Usando aplicar a los constructores de cadenas**
Puede usar `apply` para encadenar constructores para un objeto (similar a Java).

En el siguiente ejemplo, crearemos un `Function` método global llamado `construct`, que le permitirá usar un objeto similar a una matriz con un constructor en lugar de una lista de argumentos.

`Function.prototype.construct = function(aArgs) {
  let oNew = Object.create(this.prototype);
  this.apply(oNew, aArgs);
  return oNew;
};`

Ejemplo de uso:
```javascript
function MyConstructor() {
  for (let nProp = 0; nProp < arguments.length; nProp++) {
    this['property' + nProp] = arguments[nProp];
  }
}

let myArray = [4, 'Hello world!', false];
let myInstance = MyConstructor.construct(myArray);

console.log(myInstance.property1);                // logs 'Hello world!'
console.log(myInstance instanceof MyConstructor); // logs 'true'
console.log(myInstance.constructor);              // logs 'MyConstructor'
```
# **Métodos Grokking call(), apply() y bind() en JavaScript**

Estas funciones son muy importantes para todos los desarrolladores de JavaScript y se utilizan en casi todas las bibliotecas o marcos de JavaScript.
```javascript
/**
* Creates a function that invokes `func` with arguments reversed.
*
* @since 4.0.0
* @category Function
* @param {Function} func The function to flip arguments for.
* @returns {Function} Returns the new flipped function.
* @see reverse
* @example
*
* const flipped = flip((...args) => args)
*
* flipped('a', 'b', 'c', 'd')
* // => ['d', 'c', 'b', 'a']
*/
function flip(func) {
  if (typeof func !== 'function') {
    throw new TypeError('Expected a function')
  }
  return function(...args) {
    return func.apply(this, args.reverse())
  }
}
	
export default flip
```
Mire la declaración en la línea 21, **devuelva func.apply/this,args.reverse())**

Empecemos con un ejemplo para entenderlos.
Supongamos que eres un estudiante de la universidad X y tu profesor te ha pedido que crees una biblioteca de matemáticas, para una tarea, que calcula el área de un círculo.

```
const calcArea = {
  pi: 3.14,
  area: function(r) {
    return this.pi * r * r;
  }
}
```

`calcArea.area(4);// imprime 50.24`

El profesor te pidió que usaras una pi constante con una precisión de 5 decimales. Pero usaste 3.14 y no 3.14159 como el valor de pi. Ahora no puede volver a cargar la biblioteca porque la fecha límite ya venció. En esta situación, la función call() te salvará.

### **Usando call() o Function.prototype.call()**
`calcArea.area.call({pi:3.14159}, 4)`

Como puede ver, toma un nuevo objeto con un nuevo valor de pi. Y ahora el resultado será

`50.26544`

Analicemos lo que está sucediendo aquí. Si observa la `call()` , toma dos argumentos

* Contexto (objeto)
* Argumentos
Un contexto es un objeto que reemplaza la palabra clave `this` en la función de área. Y luego los argumentos se pasan como argumentos de función.

**invocación de llamada**

`cilindro.volumen.llamada({pi:3.14159}, 2, 4); // 50.26544`

### **Usando apply() o Function.prototype.apply()**
Aplicar es exactamente lo mismo, excepto que los argumentos de la función se pasan como una `matriz` o puede usar un `objeto Array ( new Array(2, 4) )`.
```javascript
cilindro.volumen.aplicar({pi: 3.14159}, [2, 4]);
// 50.26544
```
### **Usando bind() o Function.prototype.bind()**
Nos permite ingresar contexto en una función que devuelve una nueva función con un contexto actualizado. Básicamente significa que `bind()` adjunta un `nuevo this` a una función determinada. A diferencia de `call()` y `apply()`, la función `bind()`no se ejecuta inmediatamente sino más tarde cuando es necesario. `bind()` es útil cuando se trabaja con eventos de JavaScript.
```javascript
const cylinder = {
    pi: 3.14,
    volume: function(r, h) {
        return this.pi * r * r * h;
    }
};


var customVolume = cylinder.volume.bind({pi: 3.14159}); // This will not be instantly called
// In future or after some event is triggered.
customVolume(2,4); // Now pi is 3.14159
```
### **Resumen**
* Las tres funciones tienen una similitud, su primer argumento es siempre el valor o contexto 'este' , que desea pasar a la función a la que llama el método.
* call() y apply() se invocan inmediatamente , bind() devuelve una función vinculada , con el contexto, que se invocará más adelante .
* call() y apply() son similares , la única diferencia es que los argumentos en apply() se pasan como una matriz.