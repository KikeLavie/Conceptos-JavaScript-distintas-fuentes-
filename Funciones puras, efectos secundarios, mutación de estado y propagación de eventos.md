## **Javascript y Programación Funcional — Pt. 3: Funciones puras**
(Omer Goldberg)

1. Una función pura es determinista . Esto significa que dada la misma entrada, la función siempre devolverá la misma salida. Para ilustrar esto como una función en términos matemáticos (¡esto será rápido!) es una función bien definida. Cada entrada devuelve una sola salida, cada vez.
![alt text](https://hackernoon.com/hn-images/1*XIszYg4NZZF1nKZtgyAxCw.png)

Una **función pura**:
```javascript
const add = (x, y) => x + y // Una función pura
```
`add` es una función pura porque su salida depende únicamente de los argumentos que recibe. Por lo tanto, dados los mismos valores, siempre producirá la misma salida. 

¿Que tal este?
```javascript
const magicLetter = "*"
const createMagicPhrase = (frase) => `${magicLetter}abra${phrase}`
```

Algo sobre este es sospechoso... La función createMagicPhrase depende de un valor que es externo a su alcance. ¡Por lo tanto, no es puro!

**Una función impura**
```javascript
const fetchLoginToken = externalAPI.getUserToken
```
¿Es fetchLoginToken una función pura? ¿Devuelve el mismo valor cada vez? ¡Absolutamente no! A veces funcionará, a veces el servidor estará inactivo y obtendremos un error 500, y en algún momento en el futuro, la API puede cambiar para que esta llamada ya no sea válida. Entonces, debido a que la función no es determinista, podemos decir con seguridad que es una función impura.

2. ** Una función pura no causará efectos secundarios . Un efecto secundario es cualquier cambio en el sistema que sea observable para el mundo exterior.**
```javascript
const calcularBill = (sumOfCart, tax) => sumOfCart * tax
```
¿ CalculeBill es puro? Definitivamente :) Exhibe las dos características necesarias:

* La función depende solo de sus argumentos para producir un resultado.
* La función no causa ningún efecto secundario.

La Guía Mayormente Adecuada establece que los efectos secundarios incluyen, pero no se limitan a:

* cambiar el sistema de archivos
* insertar un registro en una base de datos
* hacer una llamada http
* mutaciones
* impresión en la pantalla / registro
* obtener la entrada del usuario
* consultando el DOM
acceder al estado del sistema.

**¿Por qué nuestras funciones deben ser puras?**

**Legibilidad->** Los efectos secundarios hacen que nuestro código sea más difícil de leer. Dado que una función no pura no es determinista, pueden devolver varios valores diferentes para una entrada determinada. Terminamos escribiendo código que debe tener en cuenta las diferentes posibilidades. Veamos otro *ejemplo basado en http*:
```javascript
async function getUserToken(id) {
  const token = await getTokenFromServer(id);
  return token;
}
```
Este fragmento puede fallar de muchas maneras diferentes . ¿Qué pasa si la identificación pasada a getTokenFromServer no es válida? ¿Qué pasa si el servidor falla y devuelve un error, en lugar del token esperado? Hay muchas contingencias que deben planificarse, y olvidar una (¡o varias!) de ellas es muy fácil.

Además, una función pura es más fácil de leer, ya que no requiere contexto . Recibe todos los parámetros necesarios por adelantado y no habla ni modifica el estado de la aplicación.

**Capacidad de prueba ->** Debido a que las funciones puras son deterministas por naturaleza, escribir pruebas unitarias para ellas es mucho más simple. O tu función funciona o no 😁

**Código paralelo ->** Dado que las funciones puras solo dependen de su entrada y no causarán efectos secundarios, son excelentes para escenarios donde se ejecutan subprocesos paralelos y usan memoria compartida.

**Modularidad y reutilización ->** Piense en funciones puras como pequeñas unidades de lógica. Debido a que solo dependen de la entrada que les proporcione, puede reutilizar funciones fácilmente entre diferentes partes de su base de código o diferentes proyectos por completo.

**Transparencia referencial ->** Esto suena tan complicado 🙄🙄 Cuando leí el título por primera vez, ¡quería tomar un café! En pocas palabras, la transparencia referencial significa que una llamada de función podría reemplazarse por su valor de salida, sin cambiar el comportamiento general de nuestro programa. Esto es principalmente útil como marco de pensamiento al crear funciones puras.

**Es puro y todo…. pero hace algo?**
Es importante tener en cuenta que aunque las funciones puras ofrecen muchos beneficios, **no es realista tener solo funciones puras en nuestras aplicaciones** . Después de todo, si lo hiciéramos, nuestra aplicación no tendría efectos secundarios, por lo que no produciría ningún efecto observable en el mundo exterior. Eso sería bastante aburrido 😥😥😥. En su lugar, intentaremos encapsular todos nuestros efectos secundarios en partes específicas de nuestro código base. De esa manera, suponiendo que hayamos escrito pruebas unitarias para nuestras funciones puras y sepamos que están funcionando, si algo falla en nuestra aplicación, será mucho más fácil rastrearlo.

**seamos puros** 

Concluyamos nuestra discusión convirtiendo la siguiente función no pura en pura. Este es un ejemplo artificial, pero demuestra cómo podemos refactorizar fácilmente código no puro a puro.
```javascript
let a = 4;
let b = 5;
let c = 6;
const updateTwoVars = (a) => {
	b++;
	c = a * b;
}

updateTwoVars(a);
console.log(b,c); // b = 6, c = 24
```
Comencemos por revisar por qué esta función no es pura. Nuestra función es impura porque depende de ayb, que son externos a su ámbito. Además, también está mutando (cambiando) directamente los valores de las variables. La forma más rápida de refactorizar esta función es

* Primero asegúrese de que todas las variables de las que depende la función se pasen como argumentos
* En lugar de mutar (manipular) b y c, podemos devolver nuevos valores que reflejarán los nuevos valores.
```javascript
let a = 4;
let b = 5;
let c = 6;
const updateTwoVars = (a, b, c) => [b++, a * b];

const updateRes = updateTwoVars(a,b,c);
b = updateRes[0]
c = updateRes[1]
```

**Resumen**
Hemos cubierto muchos de los beneficios de la transición de nuestra base de código para incluir funciones más puras. Hace que nuestro código sea más fácil de razonar, probar y, lo que es más importante, más predecible. Recuerde, las funciones puras no se tratan de eliminar por completo nuestra base de código de efectos secundarios. Se trata de restringirlos a una ubicación definitiva y eliminar tantos como sea posible. Este enfoque se justificará muchas veces, cuando sus programas comiencen a crecer en tamaño y complejidad.


### **¿Por qué son importantes las funciones puras en JavaScript?**

Las funciones puras se utilizan mucho en la programación funcional. Y, bibliotecas como ReactJS y Redux requieren el uso de funciones puras (ps. Si no conoces ReactJS, apréndelo. Te cambiará la vida).
Pero, las funciones puras también se pueden usar en JavaScript normal que no depende de un solo paradigma de programación. Puedes mezclar funciones puras e impuras y eso está perfectamente bien.
No todas las funciones necesitan ser, o deberían ser, puras. Por ejemplo, un controlador de eventos para presionar un botón que manipula el DOM no es un buen candidato para una función pura. Pero el controlador de eventos puede llamar a otras funciones puras que reducirán la cantidad de funciones impuras en su aplicación.

### **¿Que es una mutación?**
(fknussel)

Un valor inmutable es aquel que, una vez creado nunca se puede cambiar. En JavaScript, los valores primitivos como números, cadenas y booleanos siempre son inmutables. Sin embargo, las estructuras de datos como los objetos y las matrices no lo son. 

Por mutación me refiero a cambiar o afectar un elemento fuente. El objetivo es **mantener el elemento original sin cambios** en todo momento. 

### Una mutación es un efecto secundario: cuantas menos cosas cambien en un programa, menos habrá para realizar un seguimiento, lo que da como resultado un programa más simple.

He marcado las técnicas que implican una mutación en el elemento fuente con un ❌, mientras que los métodos inmutables están marcados con un ✅.

**Agregar nuevos elementos a una matriz**

Array.prototype.push❌
Array.prototype.unshift❌
Array.prototype.concat✅
Operador de propagación (ES6) ✅

`Array.prototype.push` nos permite empujar elementos al final de una matriz. Este método no devuelve una nueva copia, sino que muta la matriz original agregando un nuevo elemento y devuelve la nueva `length` propiedad del objeto sobre el que se llamó al método.
```javascript
numero constantes = [1, 2];
numeros.push(3);// da como resultado que los números sean [1, 2, 3]
```
También hay `Array.prototype.unshift` que podemos usar para agregar elementos al comienzo de una matriz.Al igual que `push`, `unshift` no devuelve una nueva copia de la matriz modificada, sino la nueva `length` de la matriz. 
```javascript
numeros constantes = [2, 3];
numeros.unshift(1);// da como resultado que los numeros sean [1, 2, 3]
```
En estos dos ejemplos estamos mutando o cambiando la matriz original. Recuerde, **el objetivo es mantener intacta la matriz de origen**. Repasemos algunas alternativas para agregar nuevos elementos a una matriz sin alterar la fuente. 

Quizás la salida más fácil sería crear una copia de la matriz de origen y enviarla directamente:
```javascript
numeros constantes =[1, 2];
const masNumeros = numeros.segmento();

masNumeros.push(3);
```
Podríamos llamar a esto una "mutación controlada" ya que efectivamente estamos mutando un array, aunque no el original, sino una copia que creamos para tal fin. Sin embargo, `Array.prototype.slice` puede ser complicado ya que **no crea un clon profundo, sino una copia superficial de la matriz de origen**.

`Array.prototype.concat` de hecho, devuelve una nueva matriz, que es lo que buscamos. Podemos hacer uso de este método para devolver una nueva matriz con un alemento adicional (o elementos) agregados a la original:
```javascript
numeros constantes = [1, 2];
const masNumeros = numeros.concat([3]);
```
Solo un pequeño aviso: `concat` también devuelve una **copia superficial** de la matriz de origen(tal como `slice` lo hace). Esto significa que solo copia las **referencias** a ambos objetos y otras matrices dentro de nuestra lista original. 
Si desea tener una copia completamente de su mmatriz, considere usar Lodash, `cloneDeep` que **clona recursivamente la matriz u objeto** pasado como un parámetro:
```javascript
const personas = [{ nombre: 'Bob' }, { nombre: 'Alice' }]; 
const másPersonas = cloneDeep(personas).concat([{ nombre: 'Juan' }]);
console.log(personas[0] === másPersonas[0]); // devuelve falso
```
Recuerda usar siempre `cloneDeep` si los elementos de su matriz de origen son otras matrices u objetos y necesita mantenerlos intactos. **La clonación profunda es una operación costosa en términos de rendimiento**. Es posible que no siempre necesite un clon profundo; de hecho, y como regla general, probablemente debería evitar crear clones profundos siempre que pueda: solo considere si realmente los necesita y, en caso de que los necesite, tenga en cuenta las compensaciones. 

Volviendo a agregar nuevos elementos a una matriz de forma inmutable, también podríamos usar el operador de propagación, disponible desde ES6, como reemplazo de `concat`:
```javascript
números constantes = [1, 2]; 
const másNúmeros = [...números, 3];
```

Hasta ahora, hemos agregado elementos al principio o al final de la matriz, pero también es posible agregar elementos en cualquier posición usando `Array.prototype.splice`. Tenga en cuenta `splice` que cambia la matriz de origen. 
```javascript
const posiciones = ['Primero', 'Segundo', 'Quinto', 'Sexto', 'Séptimo'];
posiciones.splice(2, 0, 'Tercero', 'Cuarto');
```
En este ejemplo, el segundo argumento (`0`) significa "no eliminar nada".

Nuevamente, podemos lograr lo mismo sin mutar la matriz original usando `concat`:
```javascript
const posiciones = ['Primero', 'Segundo', 'Quinto', 'Sexto', 'Séptimo']; 
const másPosiciones = posiciones 
  .segmento(0, 2) 
  .concat(['Tercero', 'Cuarto']) 
  .concat(posiciones.segmento(2));
```

Y una vez más, el operador de propagación también hace el trabajo:
```javascript
const posiciones = ['Primero', 'Segundo', 'Quinto', 'Sexto', 'Séptimo']; 
const másPosiciones = [ 
  ...posiciones.segmento(0, 2), 
  'Tercera', 
  'Cuarta', 
  ...posiciones.segmento(2) 
];
```
### **Agregar nuevas propiedades a un objeto**
* Adición directa ❌
* Object.assign(ES6) ✅
* Operador de propagación de objetos (experimental, aún no estandarizado) ✅
Podemos agregar fácilmente nuevas propiedades a un objeto configurándolas directamente. Por supuesto, esto constituye una mutación al objeto original:
```javascript
const person = { nombre: 'John Doe', correo electrónico: 'john@doe.com' };
// Usando notación de puntos 
person.age = 27;
// Usando notación de matriz 
person['nationality'] = 'Irish';
```
Probablemente la solución más extendida para agregar más propiedades sin cambiar el objeto de origen es usar `Object.assign`o Lodash `assign`(ambos tienen la misma firma):
```javascript
const person = { nombre: 'John Doe', correo electrónico: 'john@doe.com' }; 
const mismaPersona = Object.assign({}, persona, { 
  edad: 27, 
  nacionalidad: 'Irlandés' 
});
```
Una nota sobre `Object.assign`: ​​¿ves cómo el primer argumento es un objeto nuevo y vacío? Bueno, es importante saber que `Object.assign`muta el primer parámetro que le pasas. Es por eso que paso `{}`y no `person`como el primer argumento, ya que no quiero `person`que este operador me modifique. La conclusión aquí es **pasar siempre un objeto vacío como el primer parámetro si desea mantener intactos los objetos de origen** .
De manera similar a lo que sucede con los arreglos, podemos hacer uso del **operador de extensión de objetos** que, por cierto, aún no está estandarizado. El operador de distribución de objetos es conceptualmente similar al operador de distribución de matrices de ES6.
```javascript
const person = { nombre: 'John Doe', correo electrónico: 'john@doe.com' }; 
const mismaPersona = { ...persona, edad: 27, nacionalidad: 'Irlandesa' };
```
### **Quitar elementos de una matriz**
*  `Array.prototype.splice `❌
*  `Array.prototype.pop `❌
*  `Array.prototype.shift `❌
*  `Array.prototype.slice& Array.prototype.concat ` ✅
*  `Array.prototype.slice ` y el operador de propagación ES6 ✅
* `Array.prototype.filter` ✅
 `Array.prototype.splice(index, number) `elimina  `number `elementos a partir de  `index `y devuelve una matriz de los elementos eliminados.
  ```javascript
const ciudades = ['Oslo', 'Roma', 'Cork', 'Paris', 'Londres', 'Berna'];
ciudades.empalme(2, 1); // elimina 1 elemento del segundo índice (Cork)
```
`Array.prototype.pop` se puede usar para eliminar elementos del **final** de la matriz. También devuelve el elemento eliminado. Esta es una especie de función inversa de `Array.prototype.push`.
```javascript
const ciudades = ['Oslo', 'Roma', 'Cork', 'Paris', 'Londres', 'Berna']; 
const berna = ciudades.pop(); // elimina Berna de la lista de ciudades
```
Luego tenemos `Array.prototype.shift` which elimina el **primer** elemento de una matriz y también lo devuelve. Esta sería la función inversa de `Array.prototype.unshift`.
```javascript
const ciudades = ['Oslo', 'Roma', 'Cork', 'Paris', 'Londres', 'Berna']; 
const oslo = ciudades.shift(); // elimina Oslo de la lista de ciudades
```
`splice`, `pop`y `shift`todos mutan la matriz de origen. Ahora veamos algunas técnicas para mantener intacta la matriz original:
Digamos que queremos derivar una `capitals`matriz de la lista de ciudades de los ejemplos anteriores, lo que significa que debemos eliminar Cork y conservar todas las demás. Para lograr esto, podemos usar una combinación de `slice`y `concat`para crear una copia de un segmento del arreglo de origen (desde el principio hasta el segundo elemento, que no está incluido) y luego concatenarlo con otro segmento del arreglo original, esta vez desde la tercera posición en adelante. De esta manera nos deshicimos del elemento en el segundo índice (en este caso, Cork ).
```javascript
const ciudades = ['Oslo', 'Roma', 'Cork', 'Paris', 'Londres', 'Berna']; 
const capitales = ciudades 
  .slice(0, 2) 
  .concat(cities.slice(3));
  ```
Podemos lograr lo mismo usando el operador de propagación:
```javascript
const ciudades = ['Oslo', 'Roma', 'Cork', 'Paris', 'Londres', 'Berna']; 
const capitales = [ 
  ...ciudades.segmento(0, 2), 
  ...ciudades.segmento(3) 
];
```
También podríamos usar `Array.prototype.filter`para filtrar el elemento (o elementos) que queremos eliminar. Este método **devuelve una nueva matriz**.
```javascript
const ciudades = ['Oslo', 'Roma', 'Cork', 'Paris', 'Londres', 'Berna']; 
const capitales = ciudades.filter(ciudad => ciudad !== 'Cork');
```
También podemos filtrar por índice:
```javascript
const ciudades = ['Oslo', 'Roma', 'Cork', 'Paris', 'Londres', 'Berna']; 
const capitales = ciudades.filtro((ciudad, índice) => índice !== 2);
```
### **Quitar propiedades de un objeto**
* `delete`operador ❌
* Desestructuración de objetos ✅
* Lodash's `pick`y `omit`✅
Podemos eliminar propiedades de un objeto usando el `delete`operador. Esto, por supuesto, cambia o muta el objeto de origen.
```javascript
const person = { nombre: 'John Doe', correo electrónico: 'john@doe.com', edad: 27 };
// notación de puntos 
eliminar persona.edad;
// notación de matriz 
eliminar persona['edad'];
```
Para evitar mutaciones, podemos usar fácilmente el operador de propagación ES6 ( **desestructuración de objetos** ):
```javascript
const persona = { 
  nombre: 'John Doe', 
  correo electrónico: 'john@doe.com', 
  edad: 27, 
  país: 'Australia', 
  idioma: 'Inglés', 
  profesión: 'Desarrollador front-end' 
};
const { profesión, país, ...nuevaPersona } = persona;
consola.log(nuevaPersona);
```
donde `profession`y `country`son las propiedades que queremos eliminar del nuevo objeto.
También podemos recurrir a Lodash's `pick`y `omit`. Ambos tienen la misma firma: el primer argumento es el objeto en el que vas a trabajar, mientras que el segundo puede ser una cadena o una matriz de cadenas de la propiedad o propiedades que queremos conservar o eliminar. **Ambos devuelven un nuevo objeto**.
```javascript
const person = { nombre: 'John Doe', correo electrónico: 'john@doe.com', edad: 27 }; 
const menosDetalles = _.omit(persona, 'edad');
// o podríamos usar pick que es lo contrario de 
omit const lessDetails = _.pick(person, ['name', 'email']);
```
### **la comida para llevar**
* Hay contextos en los que no se permite la mutación (p. ej., reductores de Redux), pero está bien en muchos otros casos. Por eso es fundamental diferenciar entre **mutaciones observables y no observables** . Si crea un objeto en una función y luego solo necesita completarlo, no es muy inteligente tratar de evitar una mutación, y mucho menos muy ineficiente. Este hilo es una buena lectura sobre el tema.
* Recuerde que el operador de propagación, `Array.prototype.concat`, `Array.prototype.slice`, etc. solo devuelven una copia superficial de la matriz. Si esto no es lo suficientemente bueno para su caso de uso, use Lodash's `cloneDeep`.
* Recuerde también que devolver una nueva matriz/objeto en lugar de mutar el original, y particularmente los clones profundos, son costosos en términos de rendimiento. Solo tenga en cuenta que mantener las estructuras de datos inmutables tiene un precio.