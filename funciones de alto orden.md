# Como usar funciones de orden superior
(M. David Green)

Las  funciones que toman otra funciòn como argumento, o que definen una funciòn como el valor devuelto, se denominan funciones de orden superior. 

Las funciones de primer clase otorgan poderes especiales a JavaScript y nos permiten beneficiarnos de funciones de orden superior. 

### **Las funciones de orden superior pueden tomar una funciòn como argumento**
Una función de devolución de llamada es una función que se ejecuta al final de una operación, una vez que se completan todas las demás operaciones. 

Por lo general, pasamos esta función como último argumento, después de otros parámetros. A menudo se define en línea como una función anónima. Las funciones de devolución de llamada se basan en la capacidad de JavaScript para manejar funciones de orden superior.
JavaScript es un lenguaje de un solo subproceso. Esto significa que solo se puede ejecutar una operación a la vez.

La capacidad de pasar una función como argumento y ejecutarla después de que se completen las demás operaciones de la función principal es esencial para que un lenguaje admita funciones de orden superior.

## **Funciones de orden superior en JavaScript**
(John Hannah)

Una función de orden superior pueden resultar intimidante al principio, pero no son tan difíciles de aprender. Una función de orden superior es simplemente una función que puede aceptar otra función como parámetro o que devuelve una función como resultado. Es uno de los patrones más útiles en JavaScript y tiene una importancia particular en la programación funcional.

Comenzaremos con un ejemplo con el que puede estar familiarizado: el práctico `Array.map()` método de JavaScript. Digamos que tenemos una matriz de objetos y cada uno tiene una propiedad de nombre como esta:
```javascript
const characters = [
  {name: 'Han Solo'},
  {name: 'Luke SkyWalker'},
  {name: 'Leia Organa'}
];
```
Ahora vamos a registrar estos nombres en la consola usando el `map`método.
```javascript
// ES6 syntax
const names = characters.map(character => character.name);
console.log(names); // "Han Solo" "Luke SkyWalker" "Leia Organa"

// ES5 syntax
var names2 = characters.map(function(character) {
  return character.name;
});

console.log(names2); // "Han Solo" "Luke SkyWalker" "Leia Organa"
```
¿Viste cómo el `map`método(también conocido como función) tomó otra función como parámetro?
Eso se ajusta a nuestra definición de una función de orden superior, ¿no es así? Después de todo, las funciones de orden superior no dan tanto miedo. De hecho, es posible que los haya estado usando todo el tiempo sin siquiera darse cuenta.
Ahora, exploremos otro concepto estrechamente relacionado con la programación funcional. **La composición de funciones**  toma la idea de las funciones de orden superior y le permite desarrollarlas de una manera muy poderosa. Podemos tomar funciones pequeñas y enfocadas y combinarlas para construir una funcionalidad compleja. Al mantener estas funciones de "bloque de construcción" pequeñas, son más fáciles de probar, más fáciles de entender y altamente reutilizables.

Para ver cómo se ve esto, echemos un vistazo a otro ejemplo. No se preocupe demasiado si el código es un poco confuso al principio, lo revisaremos en un momento. Comencemos con la versión ES6 :
```javascript
// ES6 version
const characters = [
  {name: 'Luke Skywalker', img: 'http://example.com/img/luke.jpg', species: 'human'},
  {name: 'Han Solo', img: 'http://example.com/img/han.jpg', species: 'human'},
  {name: 'Leia Organa', img: 'http://example.com/img/leia.jpg', species: 'human'},
  {name: 'Chewbacca', img: 'http://example.com/img/chewie.jpg', species: 'wookie'}
];

const humans = data => data.filter(character => character.species === 'human');
const images = data => data.map(character => character.img);
const compose = (func1, func2) => data => func2(func1(data));
const displayCharacterImages = compose(humans, images);

console.log(displayCharacterImages(characters));

/* Logs out the following array
   [ "http://example.com/img/luke.jpg",
     "http://example.com/img/han.jpg",
     "http://example.com/img/leia.jpg"
   ]
*/
```
