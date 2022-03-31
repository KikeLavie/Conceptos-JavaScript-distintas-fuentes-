# **Fábricas y Clases**

## Funciones de fábrica en JavaScript ##

A medida que pasamos de una era de complementos de jQuery y complementos de secuencias de comandos a un mundo de CommonJS y arquitecturas modulares, se vuelve cada vez más importante administrar JavaScript de manera inteligente. Una de las características que falta actualmente en JavaScript es la capacidad de crear clases, aunque eso pronto cambiará con ES6. Exiten múltiples métodos y patrones para probar y recrear esta funcionalidad. Uno de esos patrones es Factory Functions.

Cuando necesita agregar funcionalidad a un objeto existente, las funciones de fábrica le permiten agregarlo con facilidad y muy poca refactorización. Eche un vistazo al ejemplo de código a continuación de una función de fábrica.

```javascript 
// Función 
función de fábrica Coche ( )  { 
  var  self  =  { 
    marca :  'Honda' , 
    modelo :  'Accord' , 
    color :  '#cc0000' , 
    pintura :  función ( color ) { 
      self . color = color ; 
    } 
  } ; 
  return  uno mismo ; 
}
```

**var** miCoche = Coche ();

Como puede ver, las funciones de fábrica no son tan diferentes de otros métodos de creación de JavaScript modular.

### **Sintaxis sencilla**
El uso de funciones de fábrica no solo lo ayudará a largo plazo, sino que también son muy simples de codificar inicialmente. Al crear una función constructora, debe usar 'nuevo' para asegurarse de que crea una instancia de un nuevo objeto. 'nuevo' nos permite poder usar la variable 'esto' dentro de nuestro constructor. 'esto' nos permite aplicar propiedades, métodos y eventos a nuestro nuevo objeto instanciado. Usar el método constructor también es muy rígido en términos de estructura. Debe seguir las reglas para devolver e instanciar correctamente un objeto.

Sin embargo, con las funciones de fábrica, la sintaxis es mucho más simple. No es necesario llamar a 'nuevo' delante de su constructor, simplemente puede llamar a la función y adjuntarla a una variable:
```javascript
var miCoche = Coche ( ) ;
```
También tiene más acceso a la variable 'esto' en las funciones de fábrica. En las funciones de fábrica, 'esto' se refiere al objeto principal, por lo que en la función myCar.paint(), 'this' se refiere a 'myCar'. Mientras que en las funciones de constructor 'esto' se refiere al método y no al objeto principal.

### **Hola funciones públicas/privadas**
Cuando usa la arquitectura de fábrica de funciones, tiene flexibilidad en la forma en que construye su objeto. Las funciones de fábrica le permiten definir variables y métodos privados dentro de la función de creación de instancias. En el siguiente ejemplo, la función 'Car' tiene un método privado de year() y una variable privada de ubicación. Tanto este método como la variable no son accesibles desde fuera de la función del automóvil.
```javascript
function Car ( )  { 
  // variable privada 
  var ubicación =  'Denver' ; 
  función año ( )  { 
    self . año =  nueva  Fecha ( ) . obtenerAñoCompleto ( ) ; 
  }
 
  var  self  =  { 
    marca :  'Honda' , 
    modelo :  'Accord' , 
    color :  '#cc0000' , 
    pintura :  función ( color ) { 
      self . color = color ; 
    } 
  } ;
 
  if  ( ! self . año ) { 
    año ( ) ; 
  }
 
  return  uno mismo ; 
}
 ```
 ```
var miCoche = Coche ( ) ;
```
Dado que el método del año es privado, lo siguiente NO funcionará:
var miCoche = Coche ( ) ; 
miCoche . año ( ) ;
Su consola mostrará un error de una función no definida. Si quisiera ejecutar la función year() fuera de la inicialización inicial de 'Car', tendría que agregar el método year() en el objeto self. Me gusta esto:
```javascript
function Car ( )  { 
  var  self  =  { 
    marca :  'Honda' , 
    modelo :  'Accord' , 
    color :  '#cc0000' , 
    pintura :  function ( color ) { 
      self . color = color ; 
    } , 
    año :  función ( ) { 
      return  self . año =  nueva  Fecha ( ) .obtenerAñoCompleto ( ) ; 
    } 
  } ;
 
  devolver  uno mismo ; 
}
 
var miCoche = Coche ( ) ; 
miCoche . año ( ) ;
```
### **Objetos dinámicos**
Esto también trae a colación el hecho de que las funciones de fábrica son muy dinámicas. Dado que podemos tener funciones públicas/privadas, podemos usar declaraciones if/else para manipular fácilmente la estructura de nuestro objeto. Esto brinda la máxima flexibilidad para permitir la ambigüedad de la función raíz y permitir que los parámetros determinen cuál debe ser el objeto devuelto. Por ejemplo, si desea agregar métodos de desarrollo específicos a su objeto en función de una variable de entorno, puede pasar fácilmente un parámetro a la función de fábrica y ajustar el objeto en consecuencia.
```javascript
función Dirección ( param )  { 
  var  self  =  { } ;
 
  if  ( param ===  'dev' ) { 
    self  =  { 
      state : 'Colorado' , 
      saveToLog :  function ( ) { 
        // escribir información en un archivo de registro 
      } 
    } ; 
  }  else  { 
    self  =  { 
      estado :  'Colorado' 
    } ; 
  }
 
  return  uno mismo ; 
}
 
var devAddress = Dirección ( 'dev' ) ; 
var direcciónProducción = Dirección ( ) ;
```
Puede ver en el ejemplo anterior que podemos permitir que el objeto se ajuste a las necesidades de los parámetros. En nuestro entorno de desarrollo, agregamos un método que puede guardar información en un archivo de registro, mientras que nuestra versión de producción puede estar sin este método y también tener menos memoria. Esto nos da una forma más granular de controlar nuestro objeto.

En general, las funciones de fábrica están preparadas para el futuro, son flexibles y dinámicas. Usarlos para escribir nuestro JavaScript permitirá una arquitectura más controlada que se puede mantener fácilmente a lo largo de la vida de un proyecto.

# **Comprender las clases en JavaScript**
Las clases en JavaScript en realidad no ofrecen funcionalidad adicional a menudo, se las describe como "azúcar sintáctica" sobre los prototipos y la herencia, ya que ofrecen una sintaxis más limpia y elegante. Debido a que otros lenguajes de programación usan clases, la sintaxis de clase en JavaScript hace que sea más sencillo para los desarrolladores moverse entre lenguajes.

### **Las clases son funciones**

Una clase JavaScript es un tipo de función. Las clases se declaran con la `class` palabra clave. Usaremos la sintaxis de expresión de función pra inicializar una función y la sintaxis de expresiónde clase para inicializar una clase.
```javascript
//Initializing  function with a function expression
const x = function(){
```
```javascript
//Initializing  class with a class expression
const y = class {}
```
Podemos acceder a la `[[Prototype]]` de un objeto usando el `Object.getPrototypeOf()método.` Usemos eso pra probar la función vacía que creamos  
```javascript  
Object.getPrototypeOf(x);
```
```javascript
Output
ƒ () { [native code] }
```

También podemos usar ese método en la clase que acabamos de crear.
```javascript
Object.getPrototypeOf(y);
```
```javascript
Output
ƒ () { [native code] }
```
El código declarado con functiony classambos devuelven una función [[Prototype]]. Con prototipos, cualquier función puede convertirse en una instancia de constructor utilizando la `new` palabra clave.
```javascript
const x = function() {}
```
```javascript
// Initialize a constructor from a function
const constructorFromFunction = new x();
console.log(constructorFromFunction);
```
```javascript
Output
x {}
constructor: ƒ ()
```
Esto también se aplica a las clases.
```javascript
const y = class {}

// Initialize a constructor from a class
const constructorFromClass = new y();

console.log(constructorFromClass);
```
```javascript
Output
y {}
constructor: 
```

Por lo demás, estos ejemplos de constructores de prototipos están vacíos, pero podemos ver cómo debajo de la sintaxis, ambos métodos están logrando el mismo resultado final.

### **Definición de una clase**

En el tutorial de prototipos y herencia , creamos un ejemplo basado en la creación de personajes en un juego de rol basado en texto. Continuemos con ese ejemplo aquí para actualizar la sintaxis de funciones a clases.

Una **función constructora** se inicializa con una serie de parámetros, que se asignarían como propiedades de this, en referencia a la función misma. La primera letra del identificador se escribiría en mayúscula por convención.

```javascript
// Initializing a constructor function
function Hero(name, level) {
	this.name = name;
	this.level = level;
}
```
Cuando traducimos esto a la sintaxis de **clase** , que se muestra a continuación, vemos que está estructurado de manera muy similar.

```javascript
// Initializing a class definition
class Hero {
	constructor(name, level) {
		this.name = name;
		this.level = level;
	}
}
```
Sabemos que una función constructora está destinada a ser un modelo de objeto por el uso de mayúsculas en la primera letra del inicializador (que es opcional) y por la familiaridad con la sintaxis. La `class ` palabra clave comunica de una manera más directa el objetivo de nuestra función.

La única diferencia en la sintaxis de la inicialización es usar la `class` palabra clave en lugar de function y asignar las propiedades dentro de un `constructor()` método.

### **Definición de métodos**
La práctica común con las funciones de constructor es asignar métodos directamente al prototypeen lugar de en la inicialización, como se ve en el `greet()` método a continuación.

```javascript
function Hero(name, level) {
	this.name = name;
	this.level = level;
}

// Adding a method to the constructor
Hero.prototype.greet = function() {
	return `${this.name} says hello.`;
}
```
Con las clases, esta sintaxis se simplifica y el método se puede agregar directamente a la clase. Usando la abreviatura de definición de método introducida en ES6, definir un método es un proceso aún más conciso.

```javascript
class Hero {
	constructor(name, level) {
		this.name = name;
		this.level = level;
	}

	// Adding a method to the constructor
	greet() {
		return `${this.name} says hello.`;
    }
}
```
Echemos un vistazo a estas propiedades y métodos en acción. Crearemos una nueva instancia de `Hero` uso de la `new`palabra clave y le asignaremos algunos valores.
```javascript
const hero1 = new Hero('Varg', 1);
```
Si imprimimos más información sobre nuestro nuevo objeto con console.log(hero1), podemos ver más detalles sobre lo que sucede con la inicialización de la clase.
```javascript
Output
Hero {name: "Varg", level: 1}
__proto__:
  ▶ constructor: class Hero
  ▶ greet: ƒ greet()
  ```
Podemos ver en el resultado que las funciones `constructor()y greet()` se aplicaron a `__proto__, o [[Prototype]]` de `hero1`, y no directamente como un método en el hero1objeto. Si bien esto es claro al hacer funciones de constructor, no es obvio al crear clases. Las clases permiten una sintaxis más simple y sucinta, pero sacrifican algo de claridad en el proceso.

### **Extender una clase**
Una característica ventajosa de las funciones y clases de constructores es que pueden extenderse a nuevos planos de objetos basados ​​en el padre. Esto evita la repetición de código para objetos que son similares pero necesitan algunas características adicionales o más específicas.

Se pueden crear nuevas funciones de constructor a partir del padre usando el  `call()` método. En el siguiente ejemplo, crearemos una clase de carácter más específica llamada  `Mage`, y le asignaremos las propiedades  `Hero` usando  `call()`, además de agregar una propiedad adicional.

 ```javascript
// Creating a new constructor from the parent
function Mage(name, level, spell) {
	// Chain constructor with call
	Hero.call(this, name, level);

	this.spell = spell;
}
 ```
En este punto, podemos crear una nueva instancia de Mageusar las mismas propiedades, Heroasí como una nueva que agregamos.
 ```javascript
const hero2 = new Mage('Lejon', 2, 'Magic Missile');
 ```
Enviando  `hero2` a la consola, podemos ver que hemos creado una nueva  `Mage` basada en el constructor.
 ```javascript
Output
Mage {name: "Lejon", level: 2, spell: "Magic Missile"}
__proto__:
    ▶ constructor: ƒ Mage(name, level, spell)
```
Con las clases ES6, la  `super` palabra clave se usa en lugar de  `call` para acceder a las funciones principales. Usaremos  `extends` para referirnos a la clase padre.

 ```javascript
// Creating a new class from the parent
class Mage extends Hero {
	constructor(name, level, spell) {
		// Chain constructor with super
		super(name, level);

		// Add a new property
		this.spell = spell;
	}
}
 ```
Ahora podemos crear una nueva Mageinstancia de la misma manera.
 ```javascript
const hero2 = new Mage('Lejon', 2, 'Magic Missile');
 ```
Imprimiremos  `hero2 ` en la consola y veremos la salida.
 ```javascript
Output
Mage {name: "Lejon", level: 2, spell: "Magic Missile"}
__proto__: Hero
    ▶ constructor: class Mage
```
El resultado es casi exactamente el mismo, excepto que en la construcción de la clase `[[Prototype]] `está vinculado al padre, en este caso  `Hero`.

A continuación se muestra una comparación lado a lado de todo el proceso de inicialización, adición de métodos y herencia de una función constructora y una clase.

 ```javascript
function Hero(name, level) {
	this.name = name;
	this.level = level;
}

// Adding a method to the constructor
Hero.prototype.greet = function() {
	return `${this.name} says hello.`;
}

// Creating a new constructor from the parent
function Mage(name, level, spell) {
	// Chain constructor with call
	Hero.call(this, name, level);

	this.spell = spell;
}
 ```
  ```javascript

// Initializing a class
class Hero {
	constructor(name, level) {
		this.name = name;
		this.level = level;
	}

	// Adding a method to the constructor
	greet() {
		return `${this.name} says hello.`;
    }
}

// Creating a new class from the parent
class Mage extends Hero {
	constructor(name, level, spell) {
		// Chain constructor with super
		super(name, level);

		// Add a new property
		this.spell = spell;
	}
}
 ```
Aunque la sintaxis es bastante diferente, el resultado subyacente es casi el mismo entre ambos métodos. Las clases nos brindan una forma más concisa de crear planos de objetos, y las funciones de construcción describen con mayor precisión lo que sucede debajo del capó.

### **Conclusión**
En este tutorial, aprendimos sobre las similitudes y diferencias entre las funciones del constructor de JavaScript y las clases de ES6. Tanto las clases como los constructores imitan un modelo de herencia orientado a objetos para JavaScript, que es un lenguaje de herencia basado en prototipos.

Comprender la herencia prototípica es fundamental para ser un desarrollador de JavaScript efectivo. Estar familiarizado con las clases es extremadamente útil, ya que las bibliotecas populares de JavaScript como React hacen un uso frecuente de la  `class ` sintaxis.