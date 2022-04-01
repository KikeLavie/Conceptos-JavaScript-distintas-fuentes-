# **La herencia y la cadena de prototipos

Cuando se trata de herencia, JavaScript solo tiene una construcción: objetos. Cada objeto tiene una propiedad privad que contiene un enlace a otro objeto llamado **prototipo**. Ese objeto prototipo tiene un prototipo propio, y así sucesivamente hasta llegar a un objeto con `null` su prototipo. Por definición, `null` no tiene prototipo y actúa como el eslabón final en esta cadena de prototipos.

Casi todos los objetos en JavaScript son instancias `object` que se encuentran justo debajo `null` en la parte superior de una cadena de prototipos.

## **Herencia con la cadena prototipo**

### **heredar propiedades**

Los objetos de JavaScript son "bolsas" dinámicas de propiedades (referidas como propiedades propias ). Los objetos JavaScript tienen un enlace a un objeto prototipo. Al intentar acceder a una propiedad de un objeto, la propiedad no solo se buscará en el objeto, sino también en el prototipo del objeto, el prototipo del prototipo, y así sucesivamente hasta que se encuentre una propiedad con un nombre coincidente o el final. de la cadena prototipo se alcanza.

Esto es lo que sucede al intentar acceder a una propiedad:
```javascript
// Let's create an object o from function F with its own properties a and b:
const F = function () {
   this.a = 1;
   this.b = 2;
};
const o = new F(); // { a: 1, b: 2 }

// add properties in F function's prototype
F.prototype.b = 3;
F.prototype.c = 4;

// do not set the prototype F.prototype = { b: 3, c: 4 };
// this will break the prototype chain
// o.[[Prototype]] has properties b and c.
// o.[[Prototype]].[[Prototype]] is Object.prototype.
// Finally, o.[[Prototype]].[[Prototype]].[[Prototype]] is null.
// This is the end of the prototype chain, as null,
// by definition, has no [[Prototype]].
// Thus, the full prototype chain looks like:
// { a: 1, b: 2 } ---> { b: 3, c: 4 } ---> Object.prototype ---> null

console.log(o.a); // 1
// Is there an 'a' own property on o? Yes, and its value is 1.

console.log(o.b); // 2
// Is there a 'b' own property on o? Yes, and its value is 2.
// The prototype also has a 'b' property, but it's not visited.
// This is called Property Shadowing

console.log(o.c); // 4
// Is there a 'c' own property on o? No, check its prototype.
// Is there a 'c' own property on o.[[Prototype]]? Yes, its value is 4.

console.log(o.d); // undefined
// Is there a 'd' own property on o? No, check its prototype.
// Is there a 'd' own property on o.[[Prototype]]? No, check its prototype.
// o.[[Prototype]].[[Prototype]] is Object.prototype and
// there is no 'd' property by default, check its prototype.
// o.[[Prototype]].[[Prototype]].[[Prototype]] is null, stop searching,
// no property found, return undefined.
```

## **Heredar "métodos"**

JavaScript no tiene "métodos" en la forma en que los definen los lenguajes basados en clases. En JavaScript, cualquier función se puede agregar a un objeto en forma de propiedad. Una función heredada actúa como cualquier otra propiedad, incluido el sombreado de propiedades como se muestra arriba (en este caso, una forma de anulación de métodos).
Cuando se ejecuta una función heredada, el valor de `this` apunta al objeto heredado, no al objeto prototipo donde la función es una propiedad propia. 
```javascript
const o = {
  a: 2,
  m: function() {
    return this.a + 1;
  }
};

console.log(o.m()); // 3
// When calling o.m in this case, 'this' refers to o

const p = Object.create(o);
// p is an object that inherits from o

p.a = 4; // assign the value 4 to the property 'a' on p
console.log(p.m()); // 5
// when p.m is called, 'this' refers to p.
// So when p inherits the function m of o,
// 'this.a' means p.a, the property 'a' of p
```

## **Uso de prototipos en JavaScript**

Veamos lo que sucede detrás de escena con un poco más de detalle.

En JavaScript, como se mencionó anteriormente, las funciones pueden tener propiedades. Todas las funciones tienen una propiedad especial llamada `prototype`. Tenga en cuenta que el código a continuación es independiente (es seguro asumir que no hay otro JavaScript en la página web que no sea el código a continuación). Para obtener la mejor experiencia de aprendizaje, se recomienda enfáticamente que abra una consola, vaya a la pestaña "consola", copie y pegue el código JavaScript a continuación y ejecútelo presionando la tecla intro/retorno. (La consola se incluye en la mayoría de las herramientas para desarrolladores de los navegadores web)

```javascript
function doSomething() {}
console.log( doSomething.prototype );
//  It does not matter how you declare the function; a
//  function in JavaScript will always have a default
//  prototype property — with one exception: an arrow
//  function doesn't have a default prototype property:
const doSomethingFromArrowFunction = () => {};
console.log(doSomethingFromArrowFunction.prototype);
```

Como se vio arriba, `doSomething()` tiene una propiedad predeterminada `prototype`, como lo demuestra la consola. Después de ejecutar este código, la consola debería haber mostrado un objeto similar a este.
```javascript
{
  constructor: ƒ doSomething(),
  __proto__: {
    constructor: ƒ Object(),
    hasOwnProperty: ƒ hasOwnProperty(),
    isPrototypeOf: ƒ isPrototypeOf(),
    propertyIsEnumerable: ƒ propertyIsEnumerable(),
    toLocaleString: ƒ toLocaleString(),
    toString: ƒ toString(),
    valueOf: ƒ valueOf()
  }
}
```

Podemos agregar propiedades al prototipo de `doSomething()`, como se muestra a continuación.
```javascript
function doSomething() {}
doSomething.prototype.foo = 'bar';
console.log(doSomething.prototype);
```
Esto resulta en:

```javascript
{
  foo: "bar",
  constructor: ƒ doSomething(),
  __proto__: {
    constructor: ƒ Object(),
    hasOwnProperty: ƒ hasOwnProperty(),
    isPrototypeOf: ƒ isPrototypeOf(),
    propertyIsEnumerable: ƒ propertyIsEnumerable(),
    toLocaleString: ƒ toLocaleString(),
    toString: ƒ toString(),
    valueOf: ƒ valueOf()
  }
}
```
Ahora podemos usar el `new` operador para crear una instancia `doSomething()` basada en este prototipo. Para usar el operador new, llame a la función normalmente, excepto que tenga el prefijo `new`. Llamar a una función con el `new` operador devuelve un objeto que es una instancia de la función. Luego se pueden agregar propiedades a este objeto.

Prueba el siguiente código:
```javascript
function doSomething() {}
doSomething.prototype.foo = 'bar'; // add a property onto the prototype
const doSomeInstancing = new doSomething();
doSomeInstancing.prop = 'some value'; // add a property onto the object
console.log(doSomeInstancing);
```
Esto  da como resultado lo siguiente:
```javascript
{
  prop: "some value",
  __proto__: {
    foo: "bar",
    constructor: ƒ doSomething(),
    __proto__: {
      constructor: ƒ Object(),
      hasOwnProperty: ƒ hasOwnProperty(),
      isPrototypeOf: ƒ isPrototypeOf(),
      propertyIsEnumerable: ƒ propertyIsEnumerable(),
      toLocaleString: ƒ toLocaleString(),
      toString: ƒ toString(),
      valueOf: ƒ valueOf()
    }
  }
}
```
Como se vio arriba, el `__proto__` de `doSomeInstancing` es `doSomething.prototype`. Pero, ¿qué hace esto? Cuando accede a una propiedad de `doSomeInstancing`, el navegador primero busca si `doSomeInstancing` tiene esa propiedad.

Si `doSomeInstancing` no tiene la propiedad, entonces el navegador busca la propiedad en el `__proto__`de `doSomeInstancing`(también conocido como doSomething.prototype). Si `__proto__` doSomeInstancing tiene la propiedad que se busca, entonces `__proto__` se usa esa propiedad en doSomeInstancing.

De lo contrario, si `__proto__` doSomeInstancing no tiene la propiedad, entonces se comprueba la propiedad de doSomeInstancing `__proto__`. `__proto__`Por defecto, la __proto__propiedad de prototipo de cualquier función es `window.Object.prototype`. Por lo tanto, el `__proto__`de `__proto__`doSomeInstancing (también conocido como doSomething.prototype ( `__proto__`aka `Object.prototype`)) se revisa para encontrar la propiedad que se está buscando.

Si la propiedad no se encuentra en el `__proto__`de `__proto__` doSomeInstancing, entonces se busca en `__proto__`el `__proto__`de `__proto__` doSomeInstancing. Sin embargo, hay un problema: el de `__proto__` doSomeInstancing no existe. Entonces, y solo entonces, después de revisar toda la cadena prototipo de 's, y no hay más s, el navegador afirma que la propiedad no existe y concluye que el valor de la propiedad es .`__proto__``__proto__``__proto__``__proto__undefined`.

Intentemos ingresar más código en la consola:
```javascript
function doSomething() {}
doSomething.prototype.foo = 'bar';
const doSomeInstancing = new doSomething();
doSomeInstancing.prop = 'some value';
console.log('doSomeInstancing.prop:      ' + doSomeInstancing.prop);
console.log('doSomeInstancing.foo:       ' + doSomeInstancing.foo);
console.log('doSomething.prop:           ' + doSomething.prop);
console.log('doSomething.foo:            ' + doSomething.foo);
console.log('doSomething.prototype.prop: ' + doSomething.prototype.prop);
console.log('doSomething.prototype.foo:  ' + doSomething.prototype.foo);
```

Esto da como resultado lo siguiente:

```javascript
doSomeInstancing.prop:      some value
doSomeInstancing.foo:       bar
doSomething.prop:           undefined
doSomething.foo:            undefined
doSomething.prototype.prop: undefined
doSomething.prototype.foo:  bar
```

## **Diferentes formas de crear objetos y la cadena de prototipos resultante**

### **Objetos creados con construcciones de sintaxis**
```javascript
const o = { a: 1 };

// The newly created object o has Object.prototype as its [[Prototype]]
// o has no own property named 'hasOwnProperty'
// hasOwnProperty is an own property of Object.prototype.
// So o inherits hasOwnProperty from Object.prototype
// Object.prototype has null as its prototype.
// o ---> Object.prototype ---> null

const b = ['yo', 'whadup', '?'];

// Arrays inherit from Array.prototype
// (which has methods indexOf, forEach, etc.)
// The prototype chain looks like:
// b ---> Array.prototype ---> Object.prototype ---> null

function f() {
  return 2;
}

// Functions inherit from Function.prototype
// (which has methods call, bind, etc.)
// f ---> Function.prototype ---> Object.prototype ---> null
```
### **con un constructor**
Un constructor en JavaScript es solo una función que se llama el operador new.
```javascript
function Graph() {
  this.vertices = [];
  this.edges = [];
}

Graph.prototype.addVertex = function(v) {
  this.vertices.push(v);
}

const g = new Graph();
// g is an object with own properties 'vertices' and 'edges'.
// g.[[Prototype]] is the value of Graph.prototype when new Graph() is executed.
```
### **Con `object.create`**

ECMAScript 5 introdujo un nuevo método: `Object.create()`. Llamar a este método crea un nuevo objeto. El prototipo de este objeto es el primer argumento de la función:
```javascript
const a = { a: 1 };
// a ---> Object.prototype ---> null

const b = Object.create(a);
// b ---> a ---> Object.prototype ---> null
console.log(b.a); // 1 (inherited)

const c = Object.create(b);
// c ---> b ---> a ---> Object.prototype ---> null

const d = Object.create(null);
// d ---> null
console.log(d.hasOwnProperty);
// undefined, because d doesn't inherit from Object.prototype
```
### **`delete`Operador con `Object.create` y `new`operador**

El uso `Object.create` de otro objeto demuestra la herencia prototípica con la `delete`operación:
```javascript
const a = { a: 1 };
const b = Object.create(a);

console.log(a.a); // print 1
console.log(b.a); // print 1
b.a = 5;
console.log(a.a); // print 1
console.log(b.a); // print 5
delete b.a;
console.log(a.a); // print 1
console.log(b.a); // print 1 (b.a value 5 is deleted but it showing value from its prototype chain)
delete a.a;       // This can also be done via 'delete Object.getPrototypeOf(b).a'
console.log(a.a); // print undefined
console.log(b.a); // print undefined
```
En el siguiente ejemplo, la llamada `new Graph()`crea una `Graph` instancia que tiene su propia `vertices` propiedad y que no hereda ninguna `vertices` propiedad. Entonces, cuando la `vertices` propiedad se elimina de esa `Graph` instancia, la instancia no tiene ni su propia `vertices` propiedad ni ninguna propiedad heredada `vertices`.
```javascript
function Graph() {
  this.vertices = [4, 4];
}

const g = new Graph();
console.log(g.vertices); // print [4, 4]
console.log(g.__proto__.vertices) // print undefined
g.vertices = 25;
console.log(g.vertices); // print 25
delete g.vertices;
console.log(g.vertices); // print undefined
```
### **con la `class`palabra clave**

ECMAScript 2015 introdujo un nuevo conjunto de palabras clave que implementan `clases`. Las nuevas palabras clave incluyen `class`, `constructor`, `static`, `extends` y `super`.
```javascript
'use strict';

class Polygon {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
}

class Square extends Polygon {
  constructor(sideLength) {
    super(sideLength, sideLength);
  }

  get area() {
    return this.height * this.width;
  }

  set sideLength(newLength) {
    this.height = newLength;
    this.width = newLength;
  }
}

const square = new Square(2);
```

## **Resumen de métodos para extender la cadena de prototipos**

Aquí están las 4 formas y sus ventajas y desventajas. Todos los ejemplos enumerados a continuación crean exactamente el mismo 
`inst`ojeto resultante (por lo tanto, registran los mismos resultados en la consola), excepto de diferentes maneras.

### **1: Nueva inicialización**
ejemplo de código:
```javascript
function foo() {}
foo.prototype.foo_prop = 'foo val';
function bar() {}
const proto = new foo();
proto.bar_prop = 'bar val';
bar.prototype = proto;
const inst = new bar();
console.log(inst.foo_prop);
console.log(inst.bar_prop);
```
1. Para utilizar este método, la función en cuestión debe estar inicializada. Durante esta inicialización, el constructor puede almacenar información única que debe generarse por objeto. Esta información única solo se generaría una vez, lo que podría generar problemas.
2. La inicialización del constructor puede colocar métodos no deseados en el objeto.
Ambos generalmente no son problemas en la práctica.

### **2.`Object.create`**
```javascript
// Technique 1
function foo() {}
foo.prototype.foo_prop = 'foo val';
function bar() {}
const proto = Object.create(foo.prototype);
proto.bar_prop = 'bar val';
bar.prototype = proto;
const inst = new bar();
console.log(inst.foo_prop);
console.log(inst.bar_prop);
```
```javascript
// Technique 2
function foo() {}
foo.prototype.foo_prop = "foo val";
function bar() {}
const proto = Object.create(
  foo.prototype,
  { bar_prop: { value: 'bar val' } }
);
bar.prototype = proto;
const inst = new bar();
console.log(inst.foo_prop);
console.log(inst.bar_prop)
```
**Pro y contras de `object.create`**
Permite la configuración directa de `__proto__` manera que sea un solo evento, lo que permite al navegador optimizar aún más el objeto. También permite la creación de objetos sin prototipo, utilizando `Object.create(null)`.

**Contras**:
No compatible con IE8 y versiones anteriores. Sin embargo, como Microsoft suspendió el soporte extendido para los sistemas que ejecutan IE8 y versiones anteriores, eso no debería ser una preocupación para la mayoría de las aplicaciones. Además, la inicialización lenta del objeto puede ser un agujero negro en el rendimiento si se usa el segundo argumento, porque cada propiedad del descriptor de objeto tiene su propio objeto descriptor independiente. Cuando se trata de cientos de miles de descriptores de objetos en forma de objetos, ese tiempo de retraso puede convertirse en un problema grave.
### **3.`Object.setProtypeOf`**

```javascript
// Technique 1
function foo() {}
foo.prototype.foo_prop = 'foo val';
function bar() {}
const proto = { bar_prop: 'bar val' };
Object.setPrototypeOf(proto, foo.prototype);
bar.prototype = proto;
const inst = new bar();
console.log(inst.foo_prop);
console.log(inst.bar_prop);
```
```javascript
// Technique 2
function foo() {}
foo.prototype.foo_prop = 'foo val';
function bar() {}
const proto = Object.setPrototypeOf(
  { bar_prop: 'bar val' },
  foo.prototype
);
bar.prototype = proto;
const inst = new bar();
console.log(inst.foo_prop);
console.log(inst.bar_prop)
```
**Pros y contras de`object.setPrototypeOf`** 
**Pros**

Compatible con todos los navegadores modernos. Permite la manipulación dinámica del prototipo de un objeto e incluso puede forzar un prototipo en un objeto sin prototipo creado con Object.create(null).
**Contras**	

Mal desempeño. Debería estar en desuso. Muchos navegadores optimizan el prototipo e intentan adivinar la ubicación del método en la memoria cuando llaman a una instancia por adelantado; pero establecer el prototipo interrumpe dinámicamente todas esas optimizaciones. Puede hacer que algunos navegadores vuelvan a compilar su código para desoptimizarlo, para que funcione de acuerdo con las especificaciones. No compatible con IE8 y versiones anteriores.

### **4.Configuración de la `__proto__`propiedad**
```javascript
// Technique 1
function A() {}
A.prototype.foo_prop = 'foo val';
function bar() {}
const proto = {
  bar_prop: 'bar val',
  __proto__: A.prototype
};
bar.prototype = proto;
const inst = new bar();
console.log(inst.foo_prop);
console.log(inst.bar_prop);
```
```javascript
// Technique 2
const inst = {
  __proto__: {
    bar_prop: 'bar val',
    __proto__: {
      foo_prop: 'foo val',
      __proto__: Object.prototype
    }
  }
};
console.log(inst.foo_prop);
console.log(inst.bar_prop)
```
**Pros y contras de establecer la __proto__propiedad.**

**Pros**	

Compatible con todos los navegadores modernos. Establecer `__proto__` algo que no es un objeto solo falla silenciosamente. No lanza una excepción.
**Contras**

Sin rendimiento y en desuso. Muchos navegadores optimizan el prototipo e intentan adivinar la ubicación del método en la memoria cuando llaman a una instancia por adelantado; pero configurar el prototipo interrumpe dinámicamente todas esas optimizaciones e incluso puede obligar a algunos navegadores a recompilar para desoptimizar su código, para que funcione de acuerdo con las especificaciones. No compatible con IE10 y versiones anteriores.

## **`prototype y Object.getPrototypeOf`**

Probablemente ya haya notado que nuestra función A tiene una propiedad especial llamada prototype. newEsta propiedad especial funciona con el operador de JavaScript . La referencia al objeto prototipo se copia en la [[Prototype]]propiedad interna de la nueva instancia. Por ejemplo, cuando lo hace `const a1 = new A()`, JavaScript (después de crear el objeto en la memoria y antes de ejecutar la función `A()`definida `this`) establece `a1.[[Prototype]] = A.prototype`. Cuando accede a las propiedades de la instancia, JavaScript primero verifica si existen en ese objeto directamente y, si no, busca en `[[Prototype]]. prototype` Esto significa que todas las instancias comparten efectivamente todo lo que define , e incluso más tarde puede cambiar partes de `prototype` y hacer que los cambios aparezcan en todas las instancias existentes, si así lo desea.

Si, en el ejemplo anterior, lo hace , en realidad `const a1 = new A()`; `const a2 = new A()`;se `a1.doSomething` referiría a `Object.getPrototypeOf(a1).doSomething`, que es lo mismo `A.prototype.doSomething` que definió, es decir, `Object.getPrototypeOf(a1).doSomething == Object.getPrototypeOf(a2).doSomething == A.prototype.doSomething`.

En resumen, `prototype` es para tipos, mientras que `Object.getPrototypeOf()`es lo mismo para instancias.

[[Prototype]]se examina de forma recursiva , es decir `a1.doSomething`, `Object.getPrototypeOf(a1).doSomething, Object.getPrototypeOf(Object.getPrototypeOf(a1)).doSomething`etc., hasta que se encuentra o `Object.getPrototypeOf` devuelve un valor nulo.

Entonces, cuando llames
```javascript
const o = new Foo();
```

JavaScript en realidad solo lo hace
```javascript
const o = new Object();
o.[[Prototype]] = Foo.prototype;
Foo.call(o);
```
(o algo así) y cuando más tarde lo hagas
```javascript
o.someProp;
```

comprueba si `o`tiene una propiedad `someProp`. Si no, comprueba `Object.getPrototypeOf(o).someProp`, y si no existe, comprueba `Object.getPrototypeOf(Object.getPrototypeOf(o)).someProp`, y así sucesivamente.

### **En conclusión**
Es esencial comprender el modelo de herencia prototípica antes de escribir código complejo que haga uso de él. Además, tenga en cuenta la longitud de las cadenas de prototipos en su código y divídalas si es necesario para evitar posibles problemas de rendimiento. Además, los prototipos nativos nunca deben ampliarse a menos que sea por motivos de compatibilidad con las funciones de JavaScript más nuevas.


## **Comprender JavaScript: prototipo y herencia(Kondov, Alexander)**

### **¿Que es prototipo?**
El prototipo es una referencia a otro objeto y se usa cuando JS no puede encontrar la propiedad que está buscando en el objeto actual. En pocas palabras, cada vez que llama a una propiedad en un objeto y no existe, JavaScript irá al objeto prototipo y lo buscará allí. Si lo encuentra, lo usará, si no,irá a la propiedad de ese objeto y buscará allí. Esto puede aparecer hasta Object.prototype antes de volver indefinido. Esta es la esencia de la cadena de prototipos y el comportamiento que se encuentra detrás de la herencia de JavaScript.

El código anterior puede ser abrumador cuando te encuentras con prototipos por primera vez, analicémoslo. Comenzaremos desde la línea 20: con la nueva palabra clave creamos un nuevo objeto usando la función constructora Perro en la línea 9. Eso nos da un objeto con una propiedad de nombre y una función hacerSonido vinculada a su prototipo. Cuando llamamos a makeSound, se ejecuta en el contexto del objeto actual (perro) y obtenemos el resultado correcto.

Cuando llamamos a sleep(), obviamente no existe en Dog, por lo que sube en la cadena de prototipos hasta Animal. Lo encuentra allí y lo llama. En la última línea llamamos a la función faltante() que no está definida en ninguna parte. Irá hasta el final de la cadena de prototipos hasta Object.prototype y, dado que no lo encontrará allí, arrojará un error.

Como puede ver, no hemos usado la palabra clase en ninguna parte, no hemos definido una clase que extienda la base o algo así. Esto es más una delegación que una herencia
Cada objeto tiene esta propiedad de prototipo que apunta a otro objeto al que se debe delegar la responsabilidad en caso de que la propiedad que estamos buscando no se encuentre en el actual. No hay nada sofisticado en eso, el objeto simplemente está delegando responsabilidades a su superior cada vez que no puede manejar la tarea.

### **Sombreado**

Si miramos a través del prisma de la herencia una vez más, sabemos que a menudo necesitamos anular propiedades y métodos. En la herencia prototípica eso se llama **sombreado**.

Lo que estamos haciendo aquí es crear una propiedad con el mismo nombre en el prototipo de Dog, de modo que cuando la llamemos, la encontrará allí y dentrá el burbujeo de la cadena de prototipos. Como puede ver, no estamos usando la palabra clave anular en ninguna parte, solo estamos declarando la propiedad en el objeto Perro para que JavaScript no comience a buscarlo en la cadena de prototipo.

## **prototipo, __proto__ y herencia de prototipos en JavaScript (Varun Dey)**

## **prototipo y __proto__**

Los prototipos en JavaScript no son más que un conjunto especial de propiedades que tiene un objeto. Cada objeto tiene su propio conjunto de `prototype` propiedades. 
```javascript
var fooFunc = function() {
    return {
        foo: 42
    }
};
fooFunc.prototype.bar = 'baz';
var fooVal = fooFunc();
console.log(fooVal);   // {foo: 42}
console.log(fooFunc.prototype);     // {bar: "baz", constructor: ƒ}
```
La segunda declaración de impresión le da el ejemplo de la herencia prototípica en toda su belleza. La función `fooFunc` se deriva de `Object` la instancia y tiene su propio conjunto de propiedades, es decir, {bar:baz} junto con lo que sea que lleve cuando se instancia desde, `object` es decir `{constructor: f}`.

Entonces, si `fooFunc` se deriva de `Object`,¿puedo subir la cadena y ver `Object` el prototipo de ?

Buena pregunta y absolutamente usted puede. Sin embargo, una cosa que debe tener en cuenta es que, excepto el `function` tipo de JavaScript, todos los demás prototipos de un objeto residen en su `__proto__` propiedad. Veamos a qué me refiero con eso.
```javascript
console.log('prototype of fooFunc:');
console.log(fooFunc.prototype);     // {bar: "baz", constructor: ƒ}
console.log('prototype of Object:');
console.log(fooFunc.prototype.__proto__);   // {constructor: ƒ, __defineGetter__: ƒ, __defineSetter__: ƒ, hasOwnProperty: ƒ, __lookupGetter__: ƒ, …}
```
¿Ves lo que veo? La última instrucción de la consola devuelve un objeto con su propio conjunto de propiedades especiales . Esto no es más que una cadena de prototipos de `Object`. Esto confirma que en realidad podemos atravesar la cadena de prototipos y que nuestra función `fooFunc` se deriva de `Object`.

Entonces, ¿qué sucede cuando trato de atravesar la cadena de prototipos de `Object`?

Veamos qué pasa:
```javascript
console.log(fooFunc.prototype); // {bar: "baz", constructor: ƒ}
console.log(fooFunc.prototype.__proto__);// {constructor: ƒ, __defineSetter__: ƒ, …}
console.log(fooFunc.prototype.__proto__.__proto__);     // null
```
Verá, `Object`en JavaScript es la construcción de nivel superior. Si intenta ver qué propiedades tiene `Object`el padre de , obtendrá un valor nulo porque no hay un padre de `Object`.

En este punto, me gustaría que volvieras al principio y relacionaras todo hasta aquí con lo que dije anteriormente en la publicación.

Los prototipos en JavaScript no son más que un conjunto especial de propiedades que tiene un objeto.

### **Herencia prototípica**

```javascript
var obj = function(){
    this.firstName = 'Varun';
    this.lastName = 'Dey'
}
obj.prototype.age = 25;
var nameObj = new obj()
console.log(nameObj.age);   // 25
```
vamos a desglosar lo que está sucediendo aquí:
* En primer lugar, estamos definiendo una función `obj`.
* Ahora también estamos asignando otra propiedad `age` directamente en `obj` la cadena de prototipos.
* Instanciamos una variable llamada `nameObj` desde `obj`.`nameObj` es un objeto al que se le añaden dos propiedades, a saber, `firstName` y `lastName`.
* Cuando pregunto `newObj` por su `age`propiedad, primero entra en su propio objeto e intenta encontrarlo. ¿Se encuentra `age` en `nameObj` el objeto?
* No. Entonces sube por la cadena, que es `nameObj.__proto__` y busca una `age` propiedad en ese objeto. 
* Encuentra una `age` propiedad aquí porque `nameObj.__proto__` es exactamente igual que `obj.prototype`.

Y de esto se trata la herencia prototípica de JavaScript. Cada vez que le pide a JavaScript que le traiga una clave, primero busca en la propiedad de su propio objeto. Si no encuentra nada, sube a su cadena prototípica ( `obj.__proto__`) y trata de encontrar esa clave entre esas propiedades, si no la encuentra allí, sube un nivel a su cadena prototípica actual ( `obj.__proto__.__proto__`) y hace lo mismo . Sigue repitiendo el mismo proceso hasta que llega a la `Object` cadena de prototipos y regresa indefinido desde allí si no puede encontrarlo ni siquiera allí.

### **Prototipo de contaminación**

Esto hace un caso interesante de herencia en JavaScript que es bastante diferente a otros lenguajes basados en clases como Java/C++:
```javascript
function parent(){
    return{
        foo: 42,
        bar: 'baz'
    }
}
child = new parent()
```
Si miras de cerca, verás que `child` es un objeto instanciado de `parent`. Y `parent`, en última instancia, no es más que un método instanciado de `Object`. Lo que esto significa es que el prototipo de` child`'s' y `parent`'s es el prototipo de `Object`'s
```javascript
child.__proto__ === parent.prototype.__proto__      // true
```
Ahora veamos un ejemplo más:
```javascript
function parent(){
    return{
        foo: 42,
        bar: 'baz'
    }
}
parent.prototype.__proto__.baz = 'I should not belong here'
child = new parent()
console.log(child.__proto__)
```
Aquí se ve un excelente ejemplo de prototipo de contaminación. Creé una propiedad `baz`directamente en `Object`el prototipo repasando la cadena de prototipos de la función. Ahora, esto `baz`se compartirá en todas las instancias de `Object`y es por eso que si ve la declaración de la consola, encontrará que, junto con otras `Object`propiedades, ahora también tenemos `baz: "I should not belong here"`. Esta es una mala práctica y está mal vista ya que rompe la encapsulación.

Del mismo modo, también puedo hacer esto y JavaScript me permitiría hacerlo:
```javascript
function parent(){
    return{
        foo: 42,
        bar: 'baz'
    }
}
delete parent.prototype.constructor
child = new parent()
```
### **Rendimiento**
No hace falta decir que, a medida que avanza en la cadena de prototipos, el tiempo de búsqueda aumenta y, por lo tanto, el rendimiento se ve afectado. Esto se vuelve crítico cuando intenta acceder a una propiedad inexistente en toda la cadena prototípica. Para verificar si la propiedad que necesita está definida en el objeto mismo, puede usar `hasOwnProperty`.
```javascript
child.hasOwnProperty('foo');    // true
parent.hasOwnProperty('baz');   // false
Object.prototype.hasOwnProperty('baz'); // true
```
### **Completando el círculo**
Al principio, dije que excepto nulo e indefinido, todo es `object`instanciación. Probemos que:
```javascript
const foo = 42;
const bar = 'fooBar';
const baz = true;
foo.__proto__.__proto__ === bar.__proto__.__proto__;    // true
bar.__proto__.__proto__ === baz.__proto__.__proto__;    // 
```
Así que ves de lo que estoy hablando. Casi todo en JavaScript proviene de`Object`

### **Conclusión**
Prototypes hace los bloques fundamentales de JavaScript. Espero haber podido ayudarlo a comprender cómo funcionan los prototipos en JavaScript. Una vez que lo domines correctamente, puedes ampliar este conocimiento para comprender cómo `this` funciona en JavaScript. 
