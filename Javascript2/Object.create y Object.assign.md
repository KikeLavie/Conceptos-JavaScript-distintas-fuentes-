# **Objeto.create()**
El `Object.create()` método crea un nuevo objeto, utilizando un objeto existente como prototipo del objeto recién creado.
```javascript
const person = {
  isHuman: false,
  printIntroduction: function() {
    console.log(`My name is ${this.name}. Am I human? ${this.isHuman}`);
  }
};

const me = Object.create(person);

me.name = 'Matthew'; // "name" is a property set on "me", but not on "person"
me.isHuman = true; // inherited properties can be overwritten

me.printIntroduction();
// expected output: "My name is Matthew. Am I human? true"
```
### **Sintxis**
```javascript
Object.create(proto)
Object.create(proto, propertiesObject)
```
### **Parámetros**
`proto`

El objeto que debería ser el prototipo del objeto recién creado.

`propertiesObject`

Si se especifica y no `undefined`, un objeto cuyas propiedades enumerables propias (es decir, aquellas propiedades definidas sobre sí mismo y propiedades no enumerables a lo largo de su cadena de prototipo) especifica descriptores de propiedad que se agregarán al objeto recién creado, con los nombres de propiedad correspondientes. Estas propiedades corresponden al segundo argumento de `Object.defineProperties()`

### **Valor de retorno**
Un nuevo objeto con el objeto prototipo y las propiedades especificados.

### **Excepciones**
El `proto`parámetro tiene que ser

`null` o
una `Object` exclusión de objetos primitivos de envoltorio .
Si protono es ninguno de estos a `TypeError` se tira.

### **Objetos personalizados y nulos**

Un nuevo objeto creado a partir de un objeto completamente personalizado (especialmente uno creado a partir del `null`objeto, que es básicamente un objeto personalizado SIN miembros) puede comportarse de formas inesperadas. Esto es especialmente cierto durante la depuración, ya que las funciones comunes de utilidad de detección/conversión de propiedad de objeto pueden generar errores o perder información (especialmente si se utilizan trampas de error silenciosas que ignoran los errores). Por ejemplo, aquí hay dos objetos:
```javascript
oco = Object.create( {} );   // create a normal object
ocn = Object.create( null ); // create a "null" object

> console.log(oco) // {} -- Seems normal
> console.log(ocn) // {} -- Seems normal here too, so far

oco.p = 1; // create a simple property on normal obj
ocn.p = 0; // create a simple property on "null" obj

> console.log(oco) // {p: 1} -- Still seems normal
> console.log(ocn) // {p: 0} -- Still seems normal here too. BUT WAIT...
```

Como se muestra arriba, todo parece normal hasta ahora. Sin embargo, al intentar utilizar estos objetos, sus diferencias se hacen evidentes rápidamente:
```jvascript
> "oco is: " + oco // shows "oco is: [object Object]"

> "ocn is: " + ocn // throws error: Cannot convert object to primitive value
```
Probar solo algunas de las muchas funciones integradas más básicas muestra la magnitud del problema más claramente:
```javascript
> alert(oco) // shows [object Object]
> alert(ocn) // throws error: Cannot convert object to primitive value

> oco.toString() // shows [object Object]
> ocn.toString() // throws error: ocn.toString is not a function

> oco.valueOf() // shows {}
> ocn.valueOf() // throws error: ocn.valueOf is not a function

> oco.hasOwnProperty("p") // shows "true"
> ocn.hasOwnProperty("p") // throws error: ocn.hasOwnProperty is not a function

> oco.constructor // shows "Object() { [native code] }"
> ocn.constructor // shows "undefined"
```
Como se dijo, estas diferencias pueden hacer que la depuración, incluso los problemas aparentemente simples, se desvíe rápidamente. Por ejemplo:

*Una función de depuración común simple:*
```javascript
// display top-level property name:value pairs of given object
function ShowProperties(obj){
  for(var prop in obj){
    console.log(prop + ": " + obj[prop] + "\n" );
  }
}
```
Resultados no tan simples: (especialmente si la captura silenciosa de errores hubiera ocultado los mensajes de error)
```javascript
ob={}; ob.po=oco; ob.pn=ocn; // create a compound object using the test objects from above as property values

> ShowProperties( ob ) // display top-level properties
- po: [object Object]
- Error: Cannot convert object to primitive value

Note that only first property gets shown.
```

(Pero si el mismo objeto se crea en un orden diferente, al menos en algunas implementaciones...)
```javascript
ob={}; ob.pn=ocn; ob.po=oco; // create same compound object again, but create same properties in different order

> ShowProperties( ob ) // display top-level properties
- Error: Cannot convert object to primitive value

Note that neither property gets shown.
```
Tenga en cuenta que un orden tan diferente puede surgir estáticamente a través de codificaciones fijas dispares como aquí, pero también dinámicamente a través del orden en que se ejecutan dichas ramas de código de adición de propiedades en tiempo de ejecución, ya que depende de las entradas y/o variables aleatorias. Por otra parte, el orden de iteración real no está garantizado sin importar los miembros del orden que se agreguen.

Tenga en cuenta, también, que el uso de Object.entries() en un objeto creado a través de Object.create() dará como resultado que se devuelva una matriz vacía.
```javascript
var obj = Object.create({ a: 1, b: 2 });

> console.log(Object.entries(obj)); // shows "[]"
```
### **Algunas NO-soluciones**
Una buena solución para los métodos de objetos faltantes no es evidente de inmediato.

Agregar el método de objeto faltante directamente desde el objeto estándar **NO** funciona:
```javascript
ocn = Object.create( null ); // create "null" object (same as before)

ocn.toString = Object.toString; // since new object lacks method then try assigning it directly from standard-object

> ocn.toString // shows "toString() { [native code] }" -- missing method seems to be there now
> ocn.toString == Object.toString // shows "true" -- method seems to be same as the standard object-method

> ocn.toString() // error: Function.prototype.toString requires that 'this' be a Function
```
Agregar el método de objeto faltante directamente al "prototipo" del nuevo objeto tampoco funciona, ya que el nuevo objeto no tiene un prototipo real (que es realmente la causa de TODOS estos problemas) y no se puede agregar uno **directamente** :
```javascript
ocn = Object.create( null ); // create "null" object (same as before)

ocn.prototype.toString = Object.toString; // Error: Cannot set property 'toString' of undefined

ocn.prototype = {};                       // try to create a prototype
ocn.prototype.toString = Object.toString; // since new object lacks method then try assigning it from standard-object

> ocn.toString() // error: ocn.toString is not a function
```
Agregar el método de objeto faltante llamando `Object.setPrototypeOf()`con el nombre del objeto estándar como segundo argumento tampoco funciona:
```javascript
ocn = Object.create( null );        // create "null" object (same as before)
Object.setPrototypeOf(ocn, Object); // wrong; sets new object's prototype to the Object() function

> ocn.toString() // error: Function.prototype.toString requires that 'this' be a Function
```
 ### **Algunas buenas soluciones**

 Nuevamente, agregar el método del objeto faltante directamente desde el **objeto estándar** No funciona. Sin embargo, agregar el método **genérico** directamente, SÍ:
 ```javascript
 ocn = Object.create( null ); // create "null" object (same as before)

ocn.toString = toString; // since new object lacks method then assign it directly from generic version

> ocn.toString() // shows "[object Object]"
> "ocn is: " + ocn // shows "ocn is: [object Object]"

ob={}; ob.pn=ocn; ob.po=oco; // create a compound object (same as before)

> ShowProperties(ob) // display top-level properties
- po: [object Object]
- pn: [object Object]
```
**Sin embargo, configurar el prototipo** genérico como el prototipo del nuevo objeto funciona aún mejor:
```javascript
ocn = Object.create( null );                  // create "null" object (same as before)
Object.setPrototypeOf(ocn, Object.prototype); // set new object's prototype to the "generic" object (NOT standard-object)
```
### **Herencia clásica con `Object.create()`**
A continuación se muestra un ejemplo de cómo utilizar `Object.create()`para lograr la herencia clásica. Esto es para una herencia única, que es todo lo que admite JavaScript.
```javascript
// Shape - superclass
function Shape() {
  this.x = 0;
  this.y = 0;
}

// superclass method
Shape.prototype.move = function(x, y) {
  this.x += x;
  this.y += y;
  console.info('Shape moved.');
};

// Rectangle - subclass
function Rectangle() {
  Shape.call(this); // call super constructor.
}

// subclass extends superclass
Rectangle.prototype = Object.create(Shape.prototype);

//If you don't set Rectangle.prototype.constructor to Rectangle,
//it will take the prototype.constructor of Shape (parent).
//To avoid that, we set the prototype.constructor to Rectangle (child).
Rectangle.prototype.constructor = Rectangle;

var rect = new Rectangle();

console.log('Is rect an instance of Rectangle?', rect instanceof Rectangle); // true
console.log('Is rect an instance of Shape?', rect instanceof Shape); // true
rect.move(1, 1); // Outputs, 'Shape moved.'
```
# **Object.assign()**

The `Object.assign()` método copia todas las propiedades propias enumerables de uno o más objetos de origen a un objeto de destino. Devuelve el objeto de destino modificado.
```javascript
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };

const returnedTarget = Object.assign(target, source);

console.log(target);
// expected output: Object { a: 1, b: 4, c: 5 }

console.log(returnedTarget);
// expected output: Object { a: 1, b: 4, c: 5 }
```
### **Sintaxis**
```javascript
Object.assign/target,...sources)
```
### **Parámetros**

`target`
El objeto de destino: a qué aplicar las propiedades de las fuentes, que se devuelve después de modificarlo.

`sources`
Los objetos de origen:objetos que contienen las propiedades que desea aplicar.

### **Valor de retorno**

El objeto de destino.

### Descripción
Las propiedades del objeto de destino se sobrescriben con las propiedades de los orígenes si tienen la misma `clave` . Las propiedades de las fuentes posteriores sobrescriben las anteriores.

El `Object.assign()`método solo copia propiedades enumerables y propias de un objeto de origen a un objeto de destino. Se utiliza `[[Get]]`en el origen y `[[Set]]` en el destino, por lo que invocará getters y setters . Por lo tanto, asigna propiedades, en lugar de copiar o definir nuevas propiedades. Esto puede hacer que no sea adecuado para fusionar nuevas propiedades en un prototipo si las fuentes de fusión contienen captadores.

Para copiar definiciones de propiedades (incluida su enumerabilidad) en prototipos, use `Object.getOwnPropertyDescriptor()`y `Object.defineProperty()`en su lugar.

Tanto `String`las `Symbol`propiedades como se copian.

En caso de error, por ejemplo, si una propiedad no se puede escribir, `TypeError`se genera una y el `target`objeto cambia si se agrega alguna propiedad antes de que se genere el error.

### **Ejemplos**
**Clonar un objeto**
```javascript
const obj = { a: 1 };
const copy = Object.assign({}, obj);
console.log(copy); // { a: 1 }
```
(Para la clonación profunda , necesitamos usar alternativas, porque Object.assign() copia los valores de propiedad.

Si el valor de origen es una referencia a un objeto, solo copia el valor de referencia.)

### **Fusión de objetos**
```javascript
const o1 = { a: 1 };
const o2 = { b: 2 };
const o3 = { c: 3 };

const obj = Object.assign(o1, o2, o3);
console.log(obj); // { a: 1, b: 2, c: 3 }
console.log(o1);  // { a: 1, b: 2, c: 3 }, target object itself is changed.
```

### **Fusionar objetos con las mismas propiedades**
```javascript
const o1 = { a: 1, b: 1, c: 1 };
const o2 = { b: 2, c: 2 };
const o3 = { c: 3 };

const obj = Object.assign({}, o1, o2, o3);
console.log(obj); // { a: 1, b: 2, c: 3 }
```
Las propiedades se sobrescriben con otros objetos que tienen las mismas propiedades más adelante en el orden de los parámetros.

### **Copiar propiedades de tipo símbolo**
```javascript
const o1 = { a: 1 };
const o2 = { [Symbol('foo')]: 2 };

const obj = Object.assign({}, o1, o2);
console.log(obj); // { a : 1, [Symbol("foo")]: 2 } (cf. bug 1207182 on Firefox)
Object.getOwnPropertySymbols(obj); // [Symbol(foo)]
```

### **Las propiedades de la cadena de prototipos y las propiedades no enumerables no se pueden copiar**
```javascript
const obj = Object.create({ foo: 1 }, { // foo is on obj's prototype chain.
  bar: {
    value: 2  // bar is a non-enumerable property.
  },
  baz: {
    value: 3,
    enumerable: true  // baz is an own enumerable property.
  }
});

const copy = Object.assign({}, obj);
console.log(copy); // { baz: 3 }
```

### **Las primitivas se envolverán en objetos**
```javascript
const v1 = 'abc';
const v2 = true;
const v3 = 10;
const v4 = Symbol('foo');

const obj = Object.assign({}, v1, null, v2, undefined, v3, v4);
// Primitives will be wrapped, null and undefined will be ignored.
// Note, only string wrappers can have own enumerable properties.
console.log(obj); // { "0": "a", "1": "b", "2": "c" }
```

### **Copiar accesores**
```javascrip
const obj = {
  foo: 1,
  get bar() {
    return 2;
  }
};

let copy = Object.assign({}, obj);
console.log(copy);
// { foo: 1, bar: 2 }
// The value of copy.bar is obj.bar's getter's return value.

// This is an assign function that copies full descriptors
function completeAssign(target, ...sources) {
  sources.forEach(source => {
    let descriptors = Object.keys(source).reduce((descriptors, key) => {
      descriptors[key] = Object.getOwnPropertyDescriptor(source, key);
      return descriptors;
    }, {});

    // By default, Object.assign copies enumerable Symbols, too
    Object.getOwnPropertySymbols(source).forEach(sym => {
      let descriptor = Object.getOwnPropertyDescriptor(source, sym);
      if (descriptor.enumerable) {
        descriptors[sym] = descriptor;
      }
    });
    Object.defineProperties(target, descriptors);
  });
  return target;
}

copy = completeAssign({}, obj);
console.log(copy);
// { foo:1, get bar() { return 2 } }
```

## **Comprender la diferencia entre Object.create() y el operador new.(Jonathan Voxland)**

Comencemos examinando lo que hace Object.create:
```javascript
var dog = { 
    comer: función () { 
        console.log ( este .eatFood) 
    } 
}; 

var maddie = Object.create(perro); 
console.log(perro.esPrototipoDe(maddie)); //true 
maddie.eatFood = 'NomNomNom'; 
maddie.comer(); //NOM Nom Nom
```
Vayamos paso a paso a través del ejemplo anterior y veamos qué está sucediendo exactamente.

1. Cree un objeto literal llamado **perro** que tenga un solo método llamado comer.
2. Inicialice maddie usando **Object.create(dog)**, que crea un objeto completamente nuevo con su prototipo establecido en dog.
3. Prueba para ver si **el perro** es un prototipo de maddie.
4. Configure la cadena para que se emita a través de **this.eatFood**
5. Llame a la función comer con el objeto recién creado, **maddie** .
6. Javascript pasará por la cadena de prototipos y encontrará el método eat **en dog** con la palabra clave **"this"** establecida en **maddie**.
7. Emite NomNomNom en la consola.

Object.create() creó un objeto **Maddie completamente nuevo**, con su prototipo establecido en perro. **El objeto maddie** recién creado ahora tiene acceso al método **eat en dog**.
Ahora, veamos el nuevo operador:
```javascript
var Perro = function (){ 
    this .eatFood = 'NomNomNom'; 
    este .eat = function (){ 
        console.log( este .eatFood) 
    } 
}; 

var maddie = nuevo (Perro); 
console.log( instancia de Maddie de Dog); // Verdadero 
maddie.comer(); //NOM Nom Nom
```
Veamos cómo se aplica el operador **new** a esta función y qué hace.

1. Crea un nuevo objeto llamado maddie
2. maddie hereda el prototipo de la función constructora
3. Ejecuta el constructor con **"este"** establecido en el objeto creado en el paso uno.
4. devuelve el objeto creado (a menos que el constructor devuelva el objeto)

Puede estar pensando, ¿cuál es la diferencia entre Object.create() y el operador **new** ? Ambos parecen hacer lo mismo. Ambos crean un nuevo objeto y heredan un prototipo.

Esperemos que este ejemplo pueda aclarar cualquier confusión:
```javascript
function Perro(){ 
    este .pupper = 'Pupper'; 
}; 

Dog.prototype.pupperino = 'Cachorros.'; 
var maddie = nuevo Perro(); 
var buddy = Object.create(Dog.prototype); 

//Usando Object.create() 
console.log(buddy.pupper); //La salida no está definida 
console.log(buddy.pupperino); //La salida es cachorros. 

//Usando la nueva palabra clave 
console.log(maddie.pupper); //La salida es Pupper console.log 
(maddie.pupperino); //La salida es cachorros.
```
La clave a tener en cuenta en este ejemplo es:
```javascript
consola.log(amigo.pupper); //La salida no está definida
```
Observe que la salida de **buddy.pupper** no está definida. Aunque Object.create() establece su prototipo en Dog, buddy no tiene acceso a **this.pupper** en el constructor.Esto se debe a la importante diferencia que`new` `Dog`en realidad ejecuta el código del constructor, mientras que`Object.create`voluntad no ejecutar el código constructor.
Esperemos que esto le haya dado un poco más de información sobre cómo funcionan Object.create() y el nuevo operador, así como también cómo se diferencian entre sí.

