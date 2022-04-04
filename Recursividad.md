# Recursividad en JavaScript
(Kevin Ennis)

En términos generales, la recursividad es el proceso de tomar un gran problema y subdividirlo en múltiples instancias más pequeñas del mismo problema. 

En la práctica, eso generalmente significa escribir una función que se llame a sí misma. Probablemente el ejemplo más clásico de este concepto es la función **factorial**.

Tal vez recuerdes de la clase de matemáticas que el factorial de un número n es el producto de todos los números enteros positivos menores o iguales que n. En otras palabras, el factorial de 5 es 5 x 4 x 3 x 2 x 1 . ¡ La notación matemática para esto es 5! .

Algo interesante que quizás hayas notado sobre ese patrón: ¡5! es en realidad sólo 5 x 4! . ¡ Y 4! es solo 4x3! . Así sucesivamente hasta llegar a 1 .

Así es como lo escribiríamos en JavaScript:

Si esto parece confuso, lo animo a que analice mentalmente el código usando el ejemplo de factorial( 3 ) .

Aquí tienes un poco de ayuda, en caso de que la necesites:

factorial( 3 ) es 3 x factorial( 2 ) .
factorial( 2 ) es 2 x factorial( 1 ) .
factorial( 1 ) cumple nuestra condición if , por lo que es solo 1.
Entonces, lo que realmente sucede aquí es que estás completando la pila de llamadas, bajando a 1 y luego deshaciendo la pila. A medida que desenreda la pila de llamadas, multiplica cada resultado. 1 x 2 x 3 es 6 , y ese es su valor devuelto.

### **Escribir una función de map recursividad**

Para nuestro ejemplo final, vamos a escribir una función **map()**.
Queremos poder usarlo así:
1. **map()** siempre debe devolver una *nueva* matriz.
2. Divide el problema en partes más pequeñas. 
3. Recuerda el ejemplo de **reverse()**.

### **Comprender la recursividad en JavaScript**
(Zak Frisch)

**recursividad** en JavaScript es, en pocas palabras, la capacidad de llamar a una función desde dentro de sí mismo. 
**Ejemplo1**: recursividad infinita
```javascript
function demostración() { 
demostración(); 
} 
demostración();
```
Como puede ver, la función anterior ( demo ) se llama/invoca. Luego procede a ejecutar las instrucciones que se encuentran en demo, que consiste en otra llamada/invocación de la función demo . Esto llamará/invocará la función de demostración que llamará/invocará la función de demostración , que llamará/invocará la función de demostración ... y este proceso continuará hasta el infinito hasta que la página o el navegador se bloqueen.
![alt text](https://miro.medium.com/max/1002/0*SlZ9PhUHtnMt-VRX.png)

Esto es **recursividad**. La capacidad de una función de **demostración** para invocar la función de **demostración** desde dentro de sí misma. 

**Ejemplo 2**: Configuración de un evento de salida

Al construir una función **recursiva** en JavaScript, *siempre* debe haber un **Evento** de salida.**¿Qué es un Evento de Salida ?** Es cualquier declaración de control que permite que la función salga del bucle recursivo. Puede ser una sentencia if, un argumento ternario, una sentencia switch, etc.
Un ejemplo de una función de cuenta regresiva simple:

```javascript
función de cuenta regresiva (n) { 
  consola.log (n); 
  si (n >= 1) cuenta regresiva (n-1); 
} 
cuenta regresiva (5);
```
Este código hará una cuenta regresiva de 5 a 0 en su consola.
![alt text](https://miro.medium.com/max/646/1*rYooIjptQIFErSavr-r-pw.png)

Esta simple demostración debería darle una idea de lo que estamos haciendo y, de hecho, puede reconocer que esta funcionalidad imita perfectamente la funcionalidad de un bucle for.
```javascript
function cuenta regresiva (n) { 
  for (let i = n; i> = 0; i--) { 
   console.log (i); 
  } 
} 
cuenta regresiva (5);
```
No debería sorprendernos saber que, sí , en este caso son exactamente iguales. Esto plantea la pregunta, ¿por qué deberíamos usar la recursividad sobre nuestros bucles de JavaScript?

**Ejemplo 3**: Recorrido Recursivo

Uno de los propósitos fundamentales de JavaScript es la capacidad de manipular el DOM. Si no ha programado extensamente en JavaScript, o simplemente no está tan familiarizado con JavaScript básico, pero ha usado bibliotecas para abstraer la funcionalidad, es posible que no haya notado que JavaScript ha tenido algunos agujeros desafortunados. Muchos de estos se han completado últimamente. Sin embargo, hubo un tiempo en que una gran cantidad de bibliotecas de JavaScript almacenarían en caché el DOM para facilitar el acceso. Hicieron esto atravesando el DOM, una forma elegante de decir que rastrearon todos los nodos en un documento.
Puede sonar difícil, pero la recursividad lo hizo mucho más fácil.
Este es un ejemplo de esa funcionalidad:
```javascript
function Traverse(ele, devolución de llamada) { 
  devolución de llamada(ele); 
  ele = ele.primerNiño; 
  while (ele) { 
    Traverse(ele, devolución de llamada); 
    ele = ele.siguienteHermano; 
  } 
}
```
### **Aplicando Recursividad** 
(Alexander Kondov)

Usaremos la recursividad para ilustrar cómo ordenar una lista de categorías en una jerarquía similar a un árbol. Estas son las categorías que obtenemos de un servicio. Tienen el nombre y su categoría principal:
```javascript
const categories = [
  { name: 'tech', parent: null },
  { name: 'hot_right_now', parent: 'tech' },
  { name: 'upcomming_releases', parent: 'tech' },
  { name: 'gadgets', parent: 'tech' },
  { name: 'news', parent: null },
  { name: 'social', parent: 'startups' },
  { name: 'europe', parent: 'news' },
  { name: 'asia', parent: 'news' },
  { name: 'events', parent: 'news' },
  { name: 'startups', parent: null },
  { name: 'funding', parent: 'startups' },
  { name: 'unicorns', parent: 'startups' },
  { name: 'venture_capital', parent: 'funding' },
  { name: 'crowdfunding', parent: 'funding' },
  { name: 'usa', parent: 'news' }
]
```
Lo primero que puede venir a su mente es usar algunos bucles anidados, sin embargo , este no es el enfoque más elegante. De momento funcionará, pero dependerás de que la estructura quede como está ahora mismo y si en algún momento se quita o añade un nivel hijo tendrás que modificar tu código.
Este es un ejemplo perfecto de cuando usar la recursividad puede ser mucho mejor que el enfoque iterativo normal. Comenzaremos creando una función, que tomará dos argumentos: la matriz y el padre de la categoría que estamos buscando. Tenga en cuenta que no solo estamos accediendo a las categorías del estado global, porque las revisaremos de forma recursiva.
```javascript
const arreglarCategorías = (categoría, padre) => { 
  let resultado = {} 
  // cuerpo de la función aquí 
  devolver resultado 
}
```
### **El cuerpo recursivo**
A continuación, necesitamos implementar realmente la recursividad. Nuestro objetivo es tener un algoritmo que no dependa del nivel de anidamiento. Aquí está la función completa que usaremos:
```javascript
const arrangeCategories = (categories, parent = null) => {
  let result = {}
  categories
    .filter(category => category.parent === parent)
    .forEach(category => {
      result[category.name] = arrangeCategories(categories, category.name)
    })

  return result
}
```
Repasemos lo que está pasando aquí.

1. En la línea 4 filtramos a través de las categorías y obtenemos solo aquellas con el padre correcto (que será nulo en la primera llamada)
2. Una vez que tenemos las categorías requeridas, para cada una de ellas las agregamos como una clave en el objeto de resultados y hacemos una llamada recursiva para encontrar todas sus categorías secundarias.
3. Repite los primeros pasos

### **El resultado**
Después de aplicar la función recursiva obtenemos el siguiente resultado:
```javascript
{
  "tech": {
    "hot_right_now": {},
    "upcomming_releases": {},
    "gadgets": {}
  },
  "news": {
    "europe": {},
    "asia": {},
    "events": {},
    "usa": {}
  },
  "startups": {
    "social": {},
    "funding": {
      "venture_capital": {},
      "crowdfunding": {}
    },
    "unicorns": {}
  }
}
```
Todas las categorías están ordenadas creando una jerarquía adecuada que será más fácil de recorrer y enumerar. La recursividad es definitivamente un tema muy amplio y se usa para resolver problemas más difíciles que la simple lista de categorías desordenadas, pero este es un buen lugar para comenzar.