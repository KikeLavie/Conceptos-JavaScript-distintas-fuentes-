# **Programación: setTimeout y setInterval**
Podemos decidir ejecutar una función no ahora, sino en un momento determinado más adelante. Eso se llama "programar una llamada".

Hay dos métodos para ello:

* `setTimeout` nos permite ejecutar una función una vez después del intervalo de tiempo.
* `setInterval` nos permite ejecutar una función repetidamente, comenzando después del intervalo de tiempo y luego repitiendo continuamente en ese intervalo. 

Estos métodos no forman parte de la especificación de JavaScript. Pero la mayoría de los entornos tienen el planificador interno y proporcionan estos métodos. En particular, son compatibles con todos los navegadores  y Node.js.

### **establecer tiempo de espera**

La sintaxis:

`let timerId = setTimeout(func|code, [delay], [arg1], [arg2], ...)`

Parámetros:

`func/ code`

Función o una cadena de código para ejecutar. Por lo general, eso es una función. Por razones históricas, se puede pasar una cadena de código, pero no se recomienda.

`delay`

El retraso antes de la ejecución, en milisegundos(1000ms = 1 segundo), por defecto 0.

`arg1`, `arg2`...

Argumentos para la función (no compatible con IE9-)

Por ejemplo, este código llama `sayHi()` después de un segundo:
``` javascript
function sayHi(){
    alert("Hello");
}
setTimeout(sayHi, 1000);
```
Con Argumentos:
```javascript
function sayHi(phrase,who){
    alert(phrase +"," + who);
}
setTimeout(sayHi, 1000, "Hello", "John"); // Hello, John
```

Si el primer argumento es una cadena, JavaScript crea una función a partir de ella. 
Entonces, esto también funcionará:

```javascript
setTimeout("alert(("Hello")", 1000);
```
Pero no se recomienda usar cadenas, use funciones de flecha en lugar de ellas, como esta:
```javascript
setTimeout(() => alert('Hello'), 1000);
```
### **Pasar una función, pero no ejecutarla**

Los desarrolladores novatos a veces cometen un error al agregar corchetes ()después de la función:
```javascript
// wrong!
setTimeout(sayHi(), 1000);
```
Eso no funciona, porque `setTimeout `espera una referencia a una función. Y aquí `sayHi()`ejecuta la función, y el resultado de su ejecución se pasa a `setTimeout`. En nuestro caso, el resultado de `sayHi()`es `undefined`(la función no devuelve nada), por lo que no se programa nada.

### **Cancelar con clearTimeout**
Una llamada `setTimeout` devuelve un "identificador de temporizador" `timerId` que podemos usar para cancelar la ejecución.

La sintaxis para cancelar:
```javascript
let timerId = setTimeout(...);
clearTimeout(timerId);
```
En el siguiente código, programamos la función y luego la cancelamos (cambiamos de opinión). Como resultado, no pasa nada:
```javascript
let timerId = setTimeout(() => alert("never happens"), 1000);
alert(timerId); // timer identifier

clearTimeout(timerId);
alert(timerId); // same identifier (doesn't become null after canceling)
```
Como podemos ver `alert` la salida, en un navegador el identificador del temporizador es un número. En otros entornos, esto puede ser otra cosa. Por ejemplo,, Node.js devuelve un objeto de temporizador con métodos adicionales. 

Una vez más, no existe una especificación universal para estos métodos, así que está bien. 

Para los navegadores, los temporizadores se describen en la sección de temporizadores del estándar HTML5.

## **establecerIntervalo**

```javascript
let timerId = setInterval(func|code, [delay], [arg1], [arg2], ...)
```

Todos los argumentos tienen el mismo significado. Pero a diferencia `setTimeout` de esto, ejecuta la función no solo una vez, sino regularmente después del intervalo de tiempo dado. 

Para detener más llamadas, debemos llamar a `clearInterval(timerId)`.

El siguiente ejemplo mostrará el mensaje cada 2 segundos. Después cada 5 segundos, la salida se detiene:
```javascript
// repeat with the interval of 2 seconds
let timerId = setInterval(() => alert('tick'), 2000);

// after 5 seconds stop
setTimeout(() => { clearInterval(timerId); alert('stop'); }, 5000);
```
### **El tiempo pasa mientras `alert` se muestra**

En la mayoría de los navegadores, incluidos Chrome y Firefox, el temporizador interno continúa "marcando" mientras muestra `alert/confirm/prompt`.

Entonces, si ejecuta el código anterior y no cierra la `alert` ventana durante un tiempo, la siguiente `alert` se mostrará inmediatamente cuando lo haga. El intervalo real entre alertas será inferior a 2 segundos. 
_____
## **Establecer tiempo de espera anidado**

Hay dos formas de ejecutar algo regularmente.

`setInterval` uno es El otro es anidado `setTimeout`, así:
```javascript
/** instead of:
let timerId = setInterval(() => alert('tick'), 2000);
*/

let timerId = setTimeout(function tick() {
  alert('tick');
  timerId = setTimeout(tick, 2000); // (*)
}, 2000);
```
Lo `setTimeout `anterior programa la próxima llamada justo al final de la actual `(*)`.

El anidado `setTimeout` es un método más flexible que `setInterval`. De esta manera, la próxima convocatoria puede programarse de manera diferente, dependiendo de los resultados de la actual.

Por ejemplo, necesitamos escribir un servicio que envíe una solicitud al servidor cada 5 segundos pidiendo datos, pero en caso de que el servidor esté sobrecargado, debería aumentar el intervalo a 10, 20, 40 segundos…

Aquí está el pseudocódigo:
```javascript
let delay = 5000;

let timerId = setTimeout(function request() {
  ...send request...

  if (request failed due to server overload) {
    // increase the interval to the next run
    delay *= 2;
  }

  timerId = setTimeout(request, delay);

}, delay);
```
Y si las funciones que estamos programando consumen mucha CPU, entonces podemos medir el tiempo que lleva la ejecución y planificar la próxima llamada tarde o temprano.

**Nested `setTimeout` permite establecer el retraso entre las ejecuciones con más precisión que `setInterval`.**

Comparemos dos fragmentos de código. El primero usa `setInterval`:
```javascript
let i = 1;
setInterval(function() {
  func(i++);
}, 100);
```

El segundo usa neste `setTimeout`:
```javascript
let i = 1;
setTimeout(function run() {
  func(i++);
  setTimeout(run, 100);
}, 100);
```
Para `setInterval` el planificador interno se ejecutará `func(i++)` cada 100ms.

**¡ La demora real entre funcllamadas `setInterval` es menor que en el código!**

Eso es normal, porque el tiempo que toma `func` la ejecución de 'consume' una parte del intervalo.

Es posible que `func` la ejecución de resulte ser más larga de lo que esperábamos y tarde más de 100ms.

En este caso, el motor espera a `func` que se complete, luego verifica el programador y, si se acabó el tiempo, lo vuelve a ejecutar de *inmediato* .

En el caso extremo, si la función siempre se ejecuta durante más de `delay` ms, las llamadas se realizarán sin pausa alguna.

Y aquí está la imagen para el anidado `setTimeout`.

**El anidado `setTimeout` garantiza el retraso fijo(aquí 100ms)**

**Recolección de basura y devolución de llamada setInterval/setTimeout**

Cuando se pasa una función `setInterval/setTimeout`, se crea una referencia interna y se guarda en el programador. Evita que la función se recopile como basura, incluso si no hay otras referencias a ella.
```javascript
// the function stays in memory until the scheduler calls it
setTimeout(function() {...}, 100);
```
Porque `setInterval` la función permanece en la memoria hasta `clearInterval` que se llama.

Hay un efecto secundario. Una función hace referencia al entorno léxico externo, por lo que, mientras vive, las variables externas también viven. Pueden ocupar mucha más memoria que la propia función. Entonces, cuando ya no necesitemos la función programada, es mejor cancelarla, aunque sea muy pequeña.

### **Retardo cero setTimeout**
Hay un caso de uso especial: `setTimeout(func, 0)`, o simplemente `setTimeout(func)`.

Esto programa la ejecución de `func` tan pronto como sea posible. Pero el programador lo invocará solo después de que se complete el script que se está ejecutando actualmente.

Por lo tanto, la función está programada para ejecutarse "justo después" del script actual.

Por ejemplo, esto genera "Hola", luego inmediatamente "Mundo":
```javascript
setTimeout(() => alert("World"));

alert("Hello");
```
La primera línea "pone la llamada en el calendario después de 0ms". Pero el programador solo "verificará el calendario" después de que se complete el script actual, por lo que "Hello"es primero y `"World"`después.

**De hecho, cero retraso no es cero (en un navegador)**
En el navegador, hay una limitación de la frecuencia con la que se pueden ejecutar los temporizadores anidados. El estándar HTML5 dice: "después de cinco temporizadores anidados, el intervalo debe ser de al menos 4 milisegundos".

Demostremos lo que significa con el siguiente ejemplo. La `setTimeout` llamada en él se vuelve a programar sin demora. Cada llamada recuerda el tiempo real de la anterior en la `times` matriz. ¿Cómo son los retrasos reales? Vamos a ver:
 ```javascript
let start = Date.now();
let times = [];

setTimeout(function run() {
  times.push(Date.now() - start); // remember delay from the previous call

  if (start + 100 < Date.now()) alert(times); // show the delays after 100ms
  else setTimeout(run); // else re-schedule
});

// an example of the output:
// 1,1,1,1,9,15,20,24,30,35,40,45,50,55,59,64,70,75,80,85,90,95,100
```
Los primeros temporizadores se ejecutan inmediatamente (tal como está escrito en la especificación), y luego vemos `9, 15, 20, 24...`. Entra en juego el retraso obligatorio de más de 4 ms entre invocaciones.

Ocurre algo similar si usamos `setInterval` en lugar de `setTimeout`: `setInterval`(f)se ejecuta fvarias veces con cero retraso y luego con un retraso de más de 4 ms.

Esa limitación proviene de la antigüedad y muchos guiones se basan en ella, por lo que existe por razones históricas.

Para JavaScript del lado del servidor, esa limitación no existe y existen otras formas de programar un trabajo asíncrono inmediato, como setImmediate para Node.js. Así que esta nota es específica del navegador.

### **Resumen**
Methods `setTimeout(func, delay, ...args)` y `setInterval(func, delay, ...args)` nos permiten ejecutar funcuna vez/regularmente después de delaymilisegundos.
Para cancelar la ejecución, debemos llamar `clearTimeout/clearInterval` con el valor devuelto por `setTimeout/`.
Las llamadas anidadas `setTimeout` son una alternativa más flexible a setInterval, lo que nos permite establecer el tiempo entre ejecuciones con mayor precisión.
La programación de retraso cero con `setTimeout(func, 0)`(igual que `setTimeout(func))` se usa para programar la llamada "tan pronto como sea posible, pero después de que se complete el guión actual".
El navegador limita el retraso mínimo para cinco o más llamadas anidadas de `setTimeout` o para `setInterval`(después de la quinta llamada) a 4 ms. Eso es por razones históricas.
Tenga en cuenta que todos los métodos de programación no garantizan el retraso exacto.

Por ejemplo, el temporizador del navegador puede ralentizarse por muchas razones:

La CPU está sobrecargada.
La pestaña del navegador está en modo de fondo.
El portátil está en la batería.
Todo eso puede aumentar la resolución mínima del temporizador (el retraso mínimo) a 300 ms o incluso a 1000 ms, según el navegador y la configuración de rendimiento del nivel del sistema operativo.