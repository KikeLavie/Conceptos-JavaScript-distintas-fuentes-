# **Programación funcional de JavaScript: mapear, filtrar y reducir**

El **map()** método crea una nueva matriz con los resultados de llamar a una función proporcionada en cada elemento de la matriz que llama.
**Ejemplo:**
```javascript
números constantes = [2, 4, 8, 10];
const mitades = numeros.map(x=> x/2);

//mitades es [1, 2, 4, 5]
```

El **filter()** método crea una nueva matriz con todos los elementos que pasan la prueba implementada por la función proporcionada.
**Ejemplo**:
```javascript
const palabras = ["spray","limite","elite","exuberante","destruccion", "presente"];

const longWords = palabras.filtro(palabra => palabra.longitud > 6);
// longWords es ["exuberante", "destruccion", "presente"]
```

Al igual que el "filtro" del mapa no es nada especial. Repasa los valores en la matriz de "palabras" para verificar qué valores pasan la prueba, en este caso, qué valor tiene una longitud superior a 6 y luego devuelve una nueva matriz con esos valores.

**Reduce()** es increíblemente versátil y flexible! Se puede usar en una tonelada de escenarios es una lástima que se use principalmente para obtener la suma de números en una matriz. 

El **reduce()** método aplica una función contra un acumulador y cada elemento de la matriz (de izquierda a derecha) para reducirlo a un solo valor.
**Ejemplo:**
```javascript
const total = [0, 1, 2, 3]. reduce((suma,valor)) => suma + valor, 1);
// el total es 7
```


## **Un enfoque funcional**

Ver `filter` comunica el comportamiento del código para que sepa exactamente lo que está haciendo. 

**Úselo cuando**: desee eliminar elementos no deseados en función de una condición. 
Ejemplo: eliminar elementos duplicados de una matriz. 

```javascript
var uniqueArray = array.filter(function(elem, index, array)c{
    return array.indexOf(elem) === index;
}
};
//ES6
//array.filter((elem, index, arr) => arr.indexOf(elem) === index);
```
**Que hace**: como `map` si atravesara la matriz de izquierda a derecha invocando una función de devolución de llamada en cada elemento. El valor devuelto debe ser un valor booleano que identifique si el elemento se mantendrá o descartará. Después de que todos los elementos han sido recorridos `filter()`, devuelve una nueva matriz con todos los elementos que devolvieron verdadero. 

Tiene los mismos parámetros que `map()`
**parámetros**
```javascript
array.filter(function(elem, index, array) {
    ....
}, thisArg);
```
**parámetro**: elemento. **sentido**: valor del elemento.

**parámetro**: índice. **sentido**: índice en cada recorrido, moviéndose de izquierda a derecha.

**parámetro**: formación. **sentido**: matriz original que invoca el método.

**parámetro**: esteArg. **sentido** (Opcional) objeto al que se hará referencia como `this` devolución de llamada.

### **Reduce()**
**Úselo cuando**: desee encontrar un valor acumulativo o concatenado en función de los elementos de la matriz. 

Ejemplo: resuma los lanzamientos de cohetes orbitales en 2014.
```javascript
var rockets = [
    { country:'Russia', launches:32 },
    { country:'US', launches:23 },
    { country:'China', launches:16 },
    { country:'Europe(ESA)', launches:7 },
    { country:'India', launches:4 },
    { country:'Japan', launches:3 }
];

var sum = rockets.reduce(function(prevVal, elem) {
    return prevVal + elem.launches;
}, 0);

// ES6
// rockets.reduce((prevVal, elem) => prevVal + elem.launches, 0); 

sum // 85
```
**Qué hace**: como `map()` se atravesara la matriz de izquierda a derecha invocando una función de devolución de llamada en cada elemento. El valor devuelto es el valor acumulativo pasado de devolución de llamada a devolución de llamada. Una vez que se han recorrido todos los elementos, `reduce()` devuelve el valor acumulativo. 

**parámetros**
```javascript
array.reduce(function(prevVal, elem, index, array) {
    ...
}, initialValue);
```

**parámetro**: valor anterior. **Sentido**: valor acumulado devuelto a través de cada devolución de llamada.

**parámetro**: elemento. **sentido**: valor del elemento.

**parámetro**: índice. **sentido**: índice del recorrido, moviéndose de izquierda a derecha.

**parámetro**: formación. **sentido**: matriz original que invoca el método.

**parámetro**: valor inicial. **sentido**: Objeto(opcional) utilizando como primer argumento en la primera devolución de llamada (más a la izquierda).

### **Extras**
* Cada uno son métodos en el objeto prototipo de Array.
* Cambiar un elemento en el arrayparámetro en cualquier devolución de llamada persistirá en todas las devoluciones de llamada restantes, pero no tiene efecto en la matriz devuelta.
* Las funciones de devolución de llamada se invocan en índices con cualquier valor, incluso undefined, pero no en los que se eliminaron o nunca se les asignó un valor.
Vale la pena señalar que los `for` bucles todavía tienen un lugar cuando se trabaja con arreglos grandes (por ejemplo, más de 1000 elementos) o cuando se necesita interrumpir el recorrido si se cumple una condición.

En esta publicación, cubrimos cómo `map()`, `reduce()` y `filter()`comunican más fácilmente el comportamiento del código y probablemente reducen la necesidad de rastrear los efectos secundarios. También cubrimos casos de uso, muestras de código, comportamiento y parámetros de cada método. Espero que esto haya sido útil para usted.

## **JavaScript: mapear, filtrar, reducir** 
(Guillermo Vicente)

Los métodos map , filter y reduce array incorporados de JavaScript son invaluables para un desarrollador de JavaScript moderno. Presentados por primera vez en 2011, estos métodos van mucho más allá del forciclo tradicional para atravesar una matriz. Hacen que la programación funcional y sus aplicaciones en cosas como React (¡hola , componentes sin estado !) sean mucho, mucho más fáciles.

### **Mapa()**
Utilice el mapa cuando desee aplicar una transformación a todos los elementos de una matriz.

Digamos que desea multiplicar todos los números de una matriz por 2:
```javascript
const numbers = [1, 2, 3, 4, 5]
numbers.map(x => x * 2); // [2, 4, 6, 8, 10]
```
Si desea guardar la salida, deberá crear una nueva variable para ella:
```javascript
const numbers = [1, 2, 3, 4, 5]
const numbersTimesTwo = numbers.map(x => x * 2)
numbersTimesTwo; // [2, 4, 6, 8, 10]
```
Un ejemplo más real es transformar una lista de distancias de millas a kilómetros:
```javascript
miles = [1, 5, 10, 75]
miles.map(num => num * 1.609344); // [1.609344, 8.04672, 16.09344, 120.70080000000002]
```
En realidad, limpiemos eso un poco y solo mostremos dos decimales usando toFixed() , que devuelve una cadena:
```javascript
miles = [1, 5, 10, 75]
kilometers = miles.map(num => (num * 1.609344).toFixed(2));
kilometers; // ["1.61", "8.05", "16.09", "120.70"]
```

### **Filtrar**
Úselo `filter()`para **eliminar** elementos no deseados en función de una condición. Por ejemplo, podríamos eliminar números mayores o iguales a 3 de una matriz. Solo cuando la expresión es true se devolverá un elemento.
```javascript
const numbers = [1, 2, 3, 4, 5];
numbers.filter(x => x >= 3); // [3, 4, 5]
```
Un ejemplo más real es eliminar duplicados de una matriz. Aquí podemos ser un poco inteligentes. Primero, `filter()`en realidad devuelve una función de devolución de llamada para cada elemento de una matriz que se invoca con tres argumentos: elemento, índice y matriz. En segundo lugar, el método de matriz incorporado `indexOf` devuelve el primer y solo el primer resultado de índice de una matriz.

Por lo tanto, si filtramos usando `indexOf` , solo se devolverá un resultado para cada valor.
```javascript
duplicateNumbers = [1, 1, 2, 3, 4, 4, 5];
duplicateNumbers.filter((elem, index, arr) => arr.indexOf(elem) === index); // [1, 2, 3, 4, 5]
```
### **Reducir**
Úselo `reduce()`para aplicar una función en una matriz completa y devolver un solo **valor** .

Bajo el capó `reduce()`, ejecuta una función de devolución de llamada para cada elemento que contiene cuatro argumentos: acumulador, valor actual, índice actual y matriz.

Aquí hay un ejemplo simple de cómo sumar cada valor numérico en una matriz:
```javascript
const numbers = [1, 2, 3, 4, 5];
numbers.reduce((prev, curr) => prev + curr); // 15
```
Hay mucho más que podemos hacer usando los cuatro argumentos, así que mire los documentos de MDN para obtener una imagen completa.

### **Conclusión**
Es difícil imaginar no tener estos tres métodos dado lo poderosos que son y la concisión del código generado. Vale la pena el esfuerzo de dominar los tres: los encontrará apareciendo una y otra vez en su código en el futuro.

