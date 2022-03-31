# **el operador 'new'**

### **Las Cuatro Reglas.**
La forma más sencilla de entender el `new` operador es entender lo que hace. 
Cuando usas `new`, suceden cuatro cosas:

1. Crea un nuevo objeto vacío.
2. Se une `this` a nuestro objeto recién creado.
3. Agrega una propiedad a nuestro objeto recién creado llamado  "__ proto __" que apunta al objeto prototipo de la función constructora. 
4. Agrega un `return this` al final de la función, de modo que el objeto que se crea se devuelve desde la función.

Cree una función constructora llamada `Student`. Esta función tomará dos parámetros, `name`, y `age`. Luego establecerá estas propiedades en el valor de `this`.
```javascript
function Estudiante(nombre, edad) { 
  this.name = nombre; 
  esta.edad = edad; 
}
```
Perfecto. Ahora invoquemos nuestra función constructora con el `new` operador. Vamos a pasar dos argumentos: `'John'`, y `26`.

`var primero = nuevo Estudiante('Juan', 26);`

Entonces, ¿qué sucede cuando ejecutamos el código anterior?
1. Se crea un nuevo objeto: el `first` objeto.
2. `this` está ligado a nuestro firstobjeto. Entonces, cualquier referencia a `this` apuntará a `first`.
3. Se agrega nuestro __proto__. `first.__proto__` ahora apuntará a `Student.prototype.`
Después de que todo esté hecho, nuestro nuevo `first` objeto se devuelve a nuestra nueva `first` variable.
Ahora podemos ejecutar algunas `console.log` declaraciones simples para probar si funcionó:
```javascript
consola.log(nombre.nombre); 
// Juan
console.log(primera.edad); 
// 26
```
Increíble. Profundicemos más en la `__proto__` parte de la nueva palabra clave.

## **New y This, palabras claves**

El operador **new** se usa para crear una instancia de un objeto que tiene una función constructora. 
Cuando llamamos a la función constructora con new, automatizamos las siguientes acciones:
* se crea un nuevo objeto.
* se une `this` al objeto.
* El objeto prototipo de la función constructora se convierte en la propiedad __ proto __ del nuevo objeto. 
* Devuelve el objeto de la función.

¡Esto es fantástico, porque la automatización da como resultado un código menos repetitivo!

```javascript 
function User(name, points) {
 this.name = name; 
 this.points = points;
}
User.prototype.increment = function(){
 this.points++;
}
User.prototype.login = function() {
 console.log(“Please login.”)
}

let user1 = new User(“Dylan”, 6);
user1.increment();
```
Al usar el patrón prototipo, cada método y propiedad se agregan directamente en el prototipo del objeto.

El intérprete subirá por la cadena prototípica y encontrará la función de incremento en la propiedad prototipo del Usuario, que también es un objeto con la información dentro. Recuerde: todas las funciones en JavaScript también son objetos . Ahora que el intérprete ha encontrado lo que necesita, puede crear un nuevo contexto de ejecución local para ejecutar user1.increment().

Prototype es una propiedad de la función constructora que determina qué se convertirá en la propiedad __ proto__ en el objeto construido.

Entonces, __proto __ es la referencia creada, y esa referencia se conoce como el vínculo de la cadena prototipo.

## **Creación y uso de constructores**

Los constructores son como funciones regulares, pero los usamos con la newpalabra clave. Hay dos tipos de constructores: constructores integrados como Array y Object que están disponibles utomáticamente en el entorno de ejecución en tiempo de ejecución; y constructores personalizados, que definen propiedades y métodos para su propio tipo de objeto.

Un constructor es útil cuando desea crear varios objetos similares con las mismas propiedades y métodos. Es una convención usar mayúsculas en el nombre de los constructores para distinguirlos de las funciones regulares. Considere el siguiente código:
```javascript
function Book(){
    //unfinished code
}

var myBook= new Book();
```
La última linea del código crea una instancia Book y la asigna a una variable. Aunque el Book constructor no hace nada, myBook sigue siendo una instancia del mismo. Como puede ver, no hay diferencia entre esta función y las funciones regulares excepto que se llama con la newpalabra clave y el nombre de la función está en mayúsculas.

## **Determinr el tipo de instancia**

Para saber si un objeto es una instancia de otro, usamos el `instanceofoperator`:
```javascript
myBook instanceof Book // true
myBook instanceof String // false
```
Tenga en cuenta que si el lado derecho del instanceofoperator no es una función, arrojará un error:
```javascript
myBook instanceof{};
//TypeError: invalid ´instanceof´ operand ({})
```
 Otra forma de encontrar el tipo de una instancia es usar la constructorpropiedad. Considere el siguiente fragmento de código:
```javascript
myBook.constructor === Book;   // true
```
La propiedad constructora de myBookapunta a Book, por lo que el operador de igualdad estricta devuelve true. Cada objeto en JavaScript hereda una `constructorpropiedad` de su prototipo, que apunta a la función constructora que ha creado el objeto:
```javascript
var s = new String("text");
s.constructor === String;      // true

"text".constructor === String; // true

var o = new Object();
o.constructor === Object;      // true

var o = {};
o.constructor === Object;      // true

var a = new Array();
a.constructor === Array;       // true

[].constructor === Array;      // true
```
Tenga en cuenta, sin embargo, que usar la `constructorpropiedad` para verificar el tipo de una instancia generalmente se considera una mala práctica porque se puede sobrescribir.

## **Funciones de constructor personalizadas**
Un constructor es como un cortador de galletas para hacer múltiples objetos con las mismas propiedades y métodos. Considere el siguiente ejemplo:
```javascript
function Book(name, year) {
  this.name = name;
  this.year = '(' + year + ')';
}
```
El `Bookconstructor` espera dos parámetros: `name` y `year`. Cuando se llama al constructor con la newpalabra clave, asigna los parámetros recibidos a la propiedad `name` y `year` de la instancia actual, como se muestra a continuación:
```javascript
var firstBook = new Book("Pro AngularJS", 2014);
var secondBook = new Book("Secrets Of The JavaScript Ninja", 2013); 
var thirdBook = new Book("JavaScript Patterns", 2010);
 
console.log(firstBook.name, firstBook.year);           
console.log(secondBook.name, secondBook.year);           
console.log(thirdBook.name, thirdBook.year);  
```
Este código registra lo siguiente en la consola:
![alt text](https://i0.wp.com/css-tricks.com/wp-content/uploads/2015/09/console-log.png)

Como puede ver, podemos construir rápidamente una gran cantidad de diferentes objetos de libro invocando al `Bookconstructor` con diferentes argumentos. Este es exactamente el mismo patrón que utiliza JavaScript en sus constructores integrados como `Array()` y `Date()`.

## **El método Object.defineProperty()**
El Object.`defineProperty()` método se puede usar dentro de un constructor para ayudar a realizar toda la configuración de propiedades necesaria. Considere el siguiente constructor:
```javascript
function Book(name) { 
  Object.defineProperty(this, "name", { 
      get: function() { 
        return "Book: " + name;       
      },        
      set: function(newName) {            
        name = newName;        
      },               
      configurable: false     
   }); 
}

var myBook = new Book("Single Page Web Applications");
console.log(myBook.name);    // Book: Single Page Web Applications

// we cannot delete the name property because "configurable" is set to false
delete myBook.name;    
console.log(myBook.name);    // Book: Single Page Web Applications

// but we can change the value of the name property
myBook.name = "Testable JavaScript";
console.log(myBook.name);    // Book: Testable JavaScript
```
Este código se utiliza `Object.defineProperty()` para definir las propiedades del descriptor de acceso. Las propiedades de acceso no incluyen ninguna propiedad o método, pero definen un `getter` para llamar cuando se lee la propiedad y un `setter` para llamar cuando se escribe en la propiedad.

Se espera que un `getter` devuelva un valor, mientras que un `setter` recibe el valor asignado a la propiedad como argumento. El constructor anterior devuelve una instancia cuya `namepropiedad` se puede establecer o cambiar, pero no se puede eliminar. Cuando obtenemos el valor de name, el captador antepone la cadena Book:al nombre y lo devuelve.

### **Se prefieren las notaciones literales de objetos a los constructores.**

El lenguaje JavaScript tiene *nueve constructores integrados* : `Object(), Array(), String(), Number(), Boolean(), Date(), Function()y .` Al crear valores, somos libres de usar literales de objetos o constructores. Sin embargo, los objetos literales no solo son más fáciles de leer, sino también más rápidos de ejecutar, ya que se pueden optimizar en el momento del análisis. Por lo tanto, para objetos simples es mejor apegarse a los literales:Error()RegExp()
```javascript
// a number object
// numbers have a toFixed() method
var obj = new Object(5);
obj.toFixed(2);     // 5.00

// we can achieve the same result using literals
var num = 5;
num.toFixed(2);     // 5.00

// a string object
// strings have a slice() method 
var obj = new String("text");
obj.slice(0,2);     // "te"

// same as above
var string = "text";
string.slice(0,2);  // "te"
```
Como puede ver, apenas hay diferencia entre los objetos literales y los constructores. Lo que es más interesante es que todavía es posible llamar a métodos en literales. Cuando se llama a un método en un literal, JavaScript convierte automáticamente el literal en un objeto temporal para que el método pueda realizar la operación. Una vez que el objeto temporal ya no es necesario, JavaScript lo descarta.

### **Usar la nueva palabra clave es esencial**
Es importante recordar usar la `new` palabra clave antes de todos los constructores. Si olvida accidentalmente `new`, estará modificando el objeto global en lugar del objeto recién creado. Considere el siguiente ejemplo:
```javascript
function Book(name, year) {
  console.log(this);
  this.name = name;
  this.year = year;
}

var myBook = Book("js book", 2014);  
console.log(myBook instanceof Book);  
console.log(window.name, window.year);

var myBook = new Book("js book", 2014);  
console.log(myBook instanceof Book);  
console.log(myBook.name, myBook.year);
```
Esto es lo que este código registra en la consola:
![alt text](https://i0.wp.com/css-tricks.com/wp-content/uploads/2015/09/console-log-2.png)

Cuando llamamos al ` Bookconstructor` sin `new`, de hecho estamos llamando a una función sin declaración de retorno. Como resultado, `this` dentro del constructor apunta a Window(en lugar de myBook) y se crean dos variables globales. Sin embargo, cuando llamamos a la función con `new`, el contexto cambia de global (Ventana) a la instancia. Entonces, `this` apunta correctamente a `myBook`.

Tenga en cuenta que en modo estricto este código generaría un error porque el modo estricto está diseñado para proteger al programador de llamar accidentalmente a un constructor sin la `newpalabra` clave.

### **Constructores de alcance seguro**
Como hemos visto, un constructor es simplemente una función, por lo que puede llamarse sin la `newpalabra` clave. Pero, para los programadores sin experiencia, esto puede ser una fuente de errores. Un constructor de alcance seguro está diseñado para devolver el mismo resultado independientemente de si se llama con o sin `new`, por lo que no sufre esos problemas.

La mayoría de los constructores integrados, como Object, Regexy Array, son seguros para el ámbito. Usan un patrón especial para determinar cómo se llama al constructor. Si `new` no se usa, devuelven una instancia adecuada del objeto llamando al constructor nuevamente con `new`. Considere el siguiente código:
```javascript
function Fn(argument) { 

  // if "this" is not an instance of the constructor
  // it means it was called without new  
  if (!(this instanceof Fn)) { 

    // call the constructor again with new
    return new Fn(argument);
  } 
}
```
Por lo tanto, una versión segura de alcance de nuestro constructor se vería así:
```javascript
function Book(name, year) { 
  if (!(this instanceof Book)) { 
    return new Book(name, year);
  }
  this.name = name;
  this.year = year;
}

var person1 = new Book("js book", 2014);
var person2 = Book("js book", 2014);

console.log(person1 instanceof Book);    // true
console.log(person2 instanceof Book);    // true
```

### **Conclusión**
Es importante comprender que la declaración de clase introducida en ES2015 simplemente funciona como un azúcar sintáctico sobre la herencia basada en prototipos existente y no agrega nada nuevo a JavaScript. Los constructores y prototipos son la forma principal de JavaScript de definir objetos similares y relacionados.

En este artículo, hemos analizado detenidamente cómo funcionan los constructores de JavaScript. Aprendimos que los constructores son como funciones regulares, pero se usan con la `newpalabra` clave. Vimos cómo los constructores nos permiten crear rápidamente varios objetos similares con las mismas propiedades y métodos, y por qué el `instanceofoperador` es la forma más segura de determinar el tipo de una instancia. Finalmente, analizamos los constructores de alcance seguro, a los que se puede llamar con o sin `new`.