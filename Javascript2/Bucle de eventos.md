# Explicación del bucle de eventos de JavaScript #
La respuesta corta es que el lenguaje JavaScript es de un solo subproceso y el comportamiento asincrónico no es parte del lenguaje JavaScript en sí, sino que se construyen sobre el lenguaje JavaScript central en el navegador (o el entorno de programación) y se accede a través de las API del navegador. .

### **Arquitectura Básica**

![alt text](bucledeeventos.png)

* Montón : los objetos se asignan en un montón, que es solo un nombre para indicar una gran región de memoria en su mayoría no estructurada.
* Stack : representa el hilo único proporcionado para la ejecución del código JavaScript. Las llamadas a funciones forman una pila de marcos (más sobre esto a continuación)
* No son parte del lenguaje JavaScript en sí, sino que se construyen sobre el lenguaje JavaScript central, lo que le brinda superpoderes adicionales para usar en su código JavaScript.proporciona algunas construcciones de JavaScript simples para recuperar datos de ubicación para que pueda trazar su ubicación en un mapa de Google. En segundo plano, el navegador en realidad está utilizando un código complejo de nivel inferior (p. ej., C++) para comunicarse con el hardware GPS del dispositivo (o lo que esté disponible para determinar los datos de posición), recuperar datos de posición y devolverlos al entorno del navegador para su uso. en tu código. Pero, de nuevo, la API abstrae esta complejidad.La API de geolocalización proporciona algunas construcciones de JavaScript simples para recuperar datos de ubicación para que pueda trazar su ubicación en un mapa de Google. En segundo plano, el navegador en realidad está utilizando un código complejo de nivel inferior (p. ej., C++) para comunicarse con el hardware GPS del dispositivo (o lo que esté disponible para determinar los datos de posición), recuperar datos de posición y devolverlos al entorno del navegador para su uso. en tu código. Pero, de nuevo, la API abstrae esta complejidad.

### **Fragmento de código 1: Intriga la mente**

```javascript
function main(){
  console.log('A');
  setTimeout(
    function display(){ console.log('B'); }
  ,0);
	console.log('C');
}
main();
//	Output
//	A
//	C
//  B
```

Aquí tenemos la función principal que tiene 2 comandos console.log que registran 'A' y 'C' en la consola. Intercalado entre ellos hay una llamada setTimeout que registra 'B' en la consola con un tiempo de espera de 0 ms.

![alt text](bucledeeventos1.png)

1. La llamada a la función principal se inserta primero en la pila (como un marco). Luego, el navegador inserta la primera instrucción en la función principal en la pila, que es console.log('A'). Esta declaración se ejecuta y, una vez completada, se abre ese marco. El alfabeto A se muestra en la consola.
La siguiente instrucción (setTimeout() con devolución de llamada exec() y tiempo de espera de 0 ms) se inserta en la pila de llamadas y comienza la ejecución. La función setTimeout utiliza una API de navegador para retrasar una devolución de llamada a la función proporcionada. El marco (con setTimeout) aparece una vez que se completa el traspaso al navegador (para el temporizador).
console.log('C') se empuja a la pila mientras el temporizador se ejecuta en el navegador para la devolución de llamada a la función exec(). En este caso particular, como la demora proporcionada fue de 0 ms, la devolución de llamada se agregará a la cola de mensajes tan pronto como el navegador la reciba (idealmente).
2. Después de la ejecución de la última instrucción en la función principal, el marco main() se extrae de la pila de llamadas, dejándolo vacío. Para que el navegador envíe cualquier mensaje de la cola a la pila de llamadas, la pila de llamadas debe estar vacía primero. Es por eso que, aunque la demora proporcionada en setTimeout() fue de 0 segundos, la devolución de llamada a exec() debe esperar hasta que se complete la ejecución de todos los marcos en la pila de llamadas.
3. Ahora, la devolución de llamada exec() se inserta en la pila de llamadas y se ejecuta. El alfabeto C se muestra en la consola. Este es el ciclo de eventos de javascript.

*Por lo tanto, el parámetro de retraso en setTimeout(function, delayTime) no representa el retraso de tiempo preciso después del cual se ejecuta la función. Representa el tiempo de espera mínimo después del cual en algún momento se ejecutará la función.*

### **Fragmento de código 2: Comprensión más profunda**

```javascript
function main(){
  console.log('A');
  setTimeout(
    function exec(){ console.log('B'); }
  , 0);
  runWhileLoopForNSeconds(3);
  console.log('C');
}
main();
function runWhileLoopForNSeconds(sec){
  let start = Date.now(), now = start;
  while (now - start < (sec*1000)) {
    now = Date.now();
  }
}
// Output
// A
// C
// B
```

![alt text](bucledeeventos2.png)

* La función runWhileLoopForNSeconds() hace exactamente lo que significa su nombre. Comprueba constantemente si el tiempo transcurrido desde el momento en que se invocó es igual a la cantidad de segundos proporcionados como argumento a la función. El punto principal a recordar es que while loop (como muchos otros) es una declaración de bloqueo, lo que significa que su ejecución ocurre en la pila de llamadas y no usa las API del navegador. Por lo tanto, bloquea todas las declaraciones posteriores hasta que finaliza la ejecución.
* Entonces, en el código anterior, aunque setTimeout tiene un retraso de 0 s y el ciclo while se ejecuta durante 3 s, la devolución de llamada exec() está atascada en la cola de mensajes. El ciclo while continúa ejecutándose en la pila de llamadas (hilo único) hasta que transcurren 3 segundos. Y después de que la pila de llamadas se vacía, el callback exec() se mueve a la pila de llamadas y se ejecuta.
* Entonces, el argumento de retraso en setTimeout() no garantiza el inicio de la ejecución después de que el temporizador termine el retraso. Sirve como tiempo mínimo para la parte de retraso.