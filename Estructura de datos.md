# Estructura de datos en JavaScript
(thonly)

A medida que la lógica comercial se mueve cada vez más de atrás hacia adelante, la experiencia en Ingeniería Frontend se vuelve cada vez más crucial. Como ingenieros frontend , dependemos de bibliotecas de visualización como React para ser productivos. Las bibliotecas de vista, a su vez, dependen de bibliotecas estatales como Redux para administrar los datos. Juntos, React y Redux se suscriben al paradigma de programación reactiva en el que las actualizaciones de la interfaz de usuario reaccionan a los cambios de datos. Cada vez más, los backends actúan simplemente como servidores API, proporcionando puntos finales solo para recuperar y actualizar los datos. En efecto, el backend simplemente "reenvía" la base de datos al frontend, esperando que el ingeniero de frontend maneje toda la lógica del controlador. La creciente popularidad de los microservicios y GraphQL dan fe de esta tendencia creciente.

*Nota: el nivel de experiencia de un ingeniero se puede inferir de su capacidad para distinguir cuándo y por qué usar una estructura de datos en particular.*

**Los malos programadores se preocupan por el código. Los buenos programadores se preocupan por las estructuras de datos y sus relaciones.
— Linus Torvalds, creador de Linux y Git**

A un alto nivel, existen básicamente tres tipos de estructuras de datos. **Las pilas y las colas** son estructuras similares a matrices que difieren solo en cómo se insertan y eliminan los elementos. **Las listas enlazadas, los árboles y los gráficos** son estructuas con *nodos* que mantienen *referencias* a otros nodos. **Las tablas hash** dependen de *funciones hash* para guardar y localizar datos.

En términos de complejidad, `stacks` son `Queues` los más simples y se pueden construir a partir de `Linked List.` `Trees` y `Graphs` son lo más complejos porque amplían el concepto de lista enlazada. `Hash Tables` necesita utilizar estas estructuras de datos para funcionar de manera confiable. En términos de eficiencia, las **listas enlazadas** son las más óptimas para *registrar* y *almacenar* datos, mientras que las **tablas hash** son más eficaces para *buscar y recuperar* datos.

### **Apilar**
Podría decirse que el más importante `Stack`en JavaScript es **la pila de llamadas** donde insertamos el alcance de `function`cada vez que lo ejecutamos. Programáticamente, es solo un `array`con dos operaciones de principio: `push`y `pop`. **Push** agrega elementos a la parte superior de la matriz, mientras que **Pop** los elimina de la misma ubicación. En otras palabras, las pilas siguen el protocolo "Último en entrar, primero en salir" (LIFO).
A continuación se muestra un ejemplo de un `Stack`código. Tenga en cuenta que podemos invertir el orden de la pila: la parte inferior se convierte en la parte superior y la parte superior se convierte en la parte inferior. Como tal, podemos usar los métodos `unshift`y de la matriz en lugar de y , respectivamente.`shift push pop`.

A medida que aumentan el número de elementos,`push`/`pop` se vuelve cada vez más eficaz que `unshift`/ `shift` porque cada elemento debe volver a indexarse en el último, pero no en el primero.

### **Cola**
JavaScript es un lenguaje **de programación basado en** eventos que permite admitir operaciones *sin bloqueo* . Internamente, el navegador administra solo un *subproceso* para ejecutar todo el código JavaScript, utilizando la **cola de eventos** para poner en cola`listeners` y el **bucle de eventos** para *escuchar* los archivos `events`. Para admitir la asincronía en un entorno de subproceso único (para ahorrar recursos de CPU y mejorar la experiencia web), `listener` `functions` *elimine* la cola y ejecute solo cuando **la pila de llamadas** esté vacía. `Promises`dependen de esta **arquitectura** *basada en eventos*para permitir una ejecución de "estilo síncrono" de código asíncrono que no bloquea otras operaciones.

Programáticamente, `Queues`son solo arreglos con dos operaciones principales: `unshift`y `pop`. **Unshift pone en** *cola los* elementos al *final* de la matriz, mientras que **Pop** los *quita* de la cola desde el *principio* de la matriz. En otras palabras, las **colas** siguen el protocolo "primero en entrar, primero en salir" (FIFO). Si se invierte la dirección, podemos reemplazar `unshift`y `pop`con `push`y `shift`, respectivamente.

### **Lista enlazada**
Al igual que las matrices, `Linked Lists`almacena elementos de datos en orden secuencial . En lugar de mantener índices, las listas enlazadas contienen punteros a otros elementos. *El primer nodo* se llama **cabeza** , mientras que el *último nodo* se llama **cola** . En una **lista de enlaces simples** , cada nodo tiene solo un puntero al *siguiente* nodo. Aquí, la *cabeza* es donde comenzamos nuestro recorrido por el resto de la lista. En una lista **doblemente enlazada** , también se mantiene un puntero al nodo *anterior* . Por tanto, también podemos empezar por la *cola* y caminar “hacia atrás” hacia la cabeza.

Las listas vinculadas tienen *inserciones* y eliminaciones **de tiempo constante** porque solo podemos cambiar los punteros. Para hacer las mismas operaciones en matrices se requiere *un tiempo lineal* porque los elementos subsiguientes deben cambiarse. Además, las listas vinculadas pueden crecer siempre que haya espacio. Sin embargo, incluso los arreglos "dinámicos" que cambian de tamaño automáticamente podrían volverse inesperadamente costosos. Por supuesto, siempre hay una compensación. Para buscar o editar un elemento en una lista enlazada, es posible que tengamos que recorrer toda la longitud, lo que equivale a un tiempo lineal. Sin embargo, con los índices de matriz, tales operaciones son triviales.

Al igual que las matrices, las listas enlazadas pueden funcionar como *pilas* . Es tan simple como que la cabeza sea el único lugar para la inserción y extracción. Las listas enlazadas también pueden funcionar como *colas* . Esto se puede lograr con una lista doblemente enlazada, donde la inserción se produce en la cola y la eliminación se produce en la cabeza, o viceversa. Para una gran cantidad de elementos, esta forma de implementar colas es más eficaz que usar arreglos porque `shift`las `unshift`operaciones al principio de los arreglos requieren un tiempo lineal para volver a indexar cada elemento subsiguiente.
Las listas enlazadas son útiles tanto en el cliente como en el servidor. En el cliente, las bibliotecas de gestión de estado como **Redux** estructuran su lógica de middleware en forma de lista enlazada. Cuando se envían *acciones* , se canalizan de un middleware al siguiente hasta que se visita todo antes de llegar a los *reductores* . En el servidor, los marcos web como **Express** también estructuran su lógica de middleware de manera similar. Cuando se recibe una *solicitud* , se canaliza de un middleware al siguiente hasta que se emite una respuesta .

### **Árbol**
A `Treees` como una *lista enlazada* , excepto que mantiene referencias a *muchos* **nodos secundarios** en una estructura *jerárquica* . En otras palabras, cada nodo no puede tener más de un padre. El **Modelo de objetos del documento (DOM)** es una estructura de este tipo, con un `html`nodo raíz que se ramifica en los nodos `head`y `body`, que se subdividen en todas las *etiquetas html* familiares . Debajo del capó, *la herencia y composición* de prototipos con componentes React también producen estructuras de árbol. Por supuesto, como representación en memoria del DOM, el **DOM virtual** de React también es una estructura de árbol.

**El árbol de búsqueda binaria** es especial porque cada nodo no puede tener más de dos hijos . El **hijo izquierdo** debe tener un valor *menor* o igual que su padre, mientras que el **hijo derecho** debe tener un *valor mayor* . Estructurado y balanceado de esta manera, podemos *buscar* cualquier valor en tiempo *logarítmico* porque podemos ignorar la mitad de la ramificación con cada iteración. La *inserción y la eliminación* también pueden ocurrir en tiempo logarítmico. Además, el *valor más pequeño* y el *más grande* se pueden encontrar fácilmente en el *extremo izquierdo*.y la hoja más *a la derecha* , respectivamente.

El recorrido a través del árbol puede ocurrir en un procedimiento *vertical* u *horizontal* . En **Depth-First Traversal** (DFT) en la dirección vertical, un algoritmo recursivo es más elegante que uno iterativo. Los nodos se pueden atravesar en *orden previo , en orden o posterior al pedido* . Si necesitamos explorar las raíces antes de inspeccionar las hojas, debemos elegir *pre-ordenar* . Pero, si necesitamos explorar las hojas antes que las raíces, debemos optar por el *post-orden* . Como su nombre lo indica, *en orden* nos permite recorrer los nodos en orden *secuencial* . Esta propiedad hace que los árboles de búsqueda binarios sean óptimos para*clasificación* _

En **Breadth-First Traversal** (BFT) en la dirección horizontal, un enfoque iterativo es más elegante que uno recursivo. Esto requiere el uso de un `queue`para rastrear todos los nodos secundarios con cada iteración. Sin embargo, la memoria necesaria para tal cola podría no ser trivial. Si la forma de un árbol es más ancha que profunda, BFT es una mejor opción que DFT. Además, la ruta que toma BFT entre dos nodos cualesquiera es la más corta posible.

### **Grafico**
Si un árbol es libre de tener más de un padre, se convierte en un `Graph`. **Los bordes** que conectan los nodos en un gráfico pueden ser *dirigidos o no dirigidos, ponderados o no ponderados* . Las aristas que tienen dirección y peso son análogas a los vectores .
Las herencias múltiples en forma de *Mixins* y objetos de datos que tienen relaciones de muchos a muchos producen estructuras gráficas. Una red social y la propia Internet también son gráficos. El gráfico más complicado de la naturaleza es nuestro cerebro humano, que **las redes neuronales** intentan replicar para dar *superinteligencia* a las máquinas .

### **Tabla de picadillo**
Una **tabla hash** es una estructura similar a un diccionario que empareja *claves* con *valores* . La ubicación en la memoria de cada par está determinada por un `hash function`, que acepta una *clave* y devuelve la *dirección* donde se debe insertar y recuperar el par. Pueden producirse colisiones si dos o más claves se convierten en la misma dirección. Para mayor solidez, `getters`y `setters`debe anticipar estos eventos para garantizar que todos los datos se puedan recuperar y que no se sobrescriba ningún dato. Por lo general, `linked lists`ofrezca la solución más simple. Tener mesas muy grandes también ayuda.

Si sabemos que nuestras direcciones estarán en secuencias enteras, simplemente podemos usarlas `Arrays`para almacenar nuestros pares clave-valor. Para asignaciones de direcciones más complejas, podemos usar `Maps`o `Objects`. Las tablas hash tienen inserción y búsqueda de tiempo *constante* en promedio. Debido a las colisiones y al cambio de tamaño, este costo insignificante podría crecer hasta convertirse en un tiempo lineal. En la práctica, sin embargo, podemos suponer que las funciones hash son lo suficientemente inteligentes como para que las colisiones y el cambio de tamaño sean raros y baratos. Si las claves representan direcciones y, por lo tanto, no se necesita hash, una simple `object literal`puede ser suficiente. Por supuesto, siempre hay una compensación. La simple correspondencia entre claves y valores, y la simple asociatividad entre claves y direcciones, sacrifican las relaciones *entre*datos. Por lo tanto, las tablas hash no son óptimas para *almacenar* datos.

Si una decisión de compensación favorece la recuperación sobre el almacenamiento, ninguna otra estructura de datos puede igualar la velocidad de las tablas hash para *búsqueda , inserción y eliminación* . Por lo tanto, no sorprende que se use en *todas partes* . Desde la base de datos hasta el servidor y el cliente, las *tablas hash* y, en particular, las *funciones hash* son cruciales para el rendimiento y la seguridad de las aplicaciones de software. La velocidad de las **consultas a la base** de datos depende en gran medida de mantener tablas de *índices que apunten* a los registros en orden. De esta manera, *las búsquedas binarias* se pueden realizar en forma *logarítmica* .tiempo, una gran ganancia de rendimiento, especialmente para **Big Data** .

Tanto en el cliente como en el servidor, muchas bibliotecas populares utilizan la **memorización** para maximizar el rendimiento. Al mantener un registro de las *entradas y salidas* en una tabla hash, las funciones se ejecutan solo una vez para las mismas entradas. La popular biblioteca **Reselect** utiliza esta estrategia de almacenamiento en caché para optimizar `mapStateToProps`funciones en aplicaciones habilitadas para **Redux** . De hecho, bajo el capó, el motor de JavaScript también utiliza tablas hash llamadas *montones* para almacenar todo lo `variables`que `primitives`creamos. Se accede a ellos desde *punteros* en *la pila de llamadas*.

La propia **Internet** también se basa en *algoritmos hash* para funcionar de forma segura. La estructura de Internet es tal que cualquier computadora puede comunicarse con cualquier otra computadora a través de una *red* de dispositivos interconectados. Cada vez que un dispositivo inicia sesión en Internet, también se convierte en un *enrutador* a través del cual pueden viajar los flujos de datos. Sin embargo, es un arma de doble filo. Una arquitectura *descentralizada* significa que cualquier dispositivo en la red puede escuchar y manipular los paquetes de datos que ayuda a transmitir. Las funciones hash como MD5 y SHA256 juegan un papel fundamental en la prevención de tales ataques de *intermediarios* . El comercio electrónico a través de HTTPS es seguro solo porque se utilizan estas funciones hash.

Inspiradas en Internet, las tecnologías de cadena de **bloques** buscan *abrir* la estructura misma de la web a *nivel de protocolo* . Mediante el uso de funciones hash para crear *huellas dactilares* inmutables para cada *bloque de datos* , esencialmente toda la base de datos puede existir *abiertamente* en la web para que cualquiera pueda verla y contribuir. Estructuralmente, las cadenas de bloques son solo listas de árboles binarios de hashes criptográficos enlazados individualmente. ¡El hashing es tan críptico que cualquier persona puede crear y actualizar una base de datos de transacciones financieras *al aire libre* ! La increíble implicación es el asombroso poder de crear dinero. sí mismo. Lo que antes solo era posible para los gobiernos y los bancos centrales, ¡ahora *cualquiera* puede crear su propia *moneda* de *forma segura* ! Esta es la idea clave realizada por el fundador de Ethereum y el fundador seudónimo de Bitcoin .
A medida que más y más bases de datos salgan a la luz, la necesidad de ingenieros frontend que puedan abstraer todas las complejidades criptográficas de bajo nivel también se agravará. En este futuro, *el principal diferenciador* será la **experiencia de usuario** .

### **Conclusión**
A medida que la lógica se mueve cada vez más del servidor al cliente, la *capa de datos* en la interfaz se vuelve primordial. La adecuada gestión de esta capa implica el dominio de las estructuras de datos sobre las que descansa la lógica. Ninguna estructura de datos es perfecta para todas las situaciones porque optimizar para una propiedad siempre equivale a perder otra. Algunas estructuras son más eficientes en el almacenamiento de datos, mientras que otras son más eficaces para buscar en ellos. Por lo general, uno se sacrifica por el otro. En un extremo, las **listas enlazadas** son óptimas para el almacenamiento y pueden convertirse en **pilas y colas** ( tiempo lineal ). Por otro lado, ninguna otra estructura puede igualar la velocidad de búsqueda de las **tablas hash** (tiempo constante ). **Las estructuras de árbol** se encuentran en algún lugar en el medio ( tiempo logarítmico ), y solo un **gráfico** puede representar la estructura más compleja de la naturaleza: el cerebro humano ( tiempo polinomial ). Tener el conjunto de habilidades para distinguir *cuándo* y articular *por qué* es un sello distintivo de un ingeniero de rockstar.
Se pueden encontrar ejemplos de estas estructuras de datos en todas partes . Desde la base de datos, hasta el servidor, el cliente e incluso el propio motor de JavaScript, estas estructuras de datos concretan lo que esencialmente están encendidos y apagados."cambia" los chips de silicio en "objetos" realistas. Aunque solo son digitales, el impacto que estos objetos tienen en la sociedad es tremendo. Su capacidad para leer este artículo de forma libre y segura da fe de la asombrosa arquitectura de Internet y la estructura de sus datos. Sin embargo, esto es solo el comienzo. La inteligencia artificial y las cadenas de bloques descentralizadas en las próximas décadas redefinirán lo que significa ser humano y el papel de las instituciones que gobiernan nuestras vidas. Las intuiciones existenciales y la desintermediación institucional serán características de una internet que finalmente ha madurado.

## **Algoritmos y estructuras de datos en JavaScript**
(Oleksii Trekhleb)

Además de esas estructuras de datos, **se implementan más de 50 algoritmos populares**. Entre ellos se encuentran la clasificación, los algoritmos de búsqueda, los algoritmos relacionados con gráficos/ árboles/ conjuntos/ cadenas/ matemáticas. Todos los algoritmos también se clasifican por sus paradigmas:
* **Algoritmos de fuerza bruta**: observe todas las posibilidades y seleccione la mejor solución.
* **Algoritmos codiciosos**: elija la mejor opción en el momento actual, sin tener en cuenta el futuro.
* **Algoritmos divide y vencerás**: divide el problema en partes más pequeñas y luego resuelve esas partes.
* **Algoritmos de programación dinámica**: desarrolle una solución utilizando subsoluciones encontradas previamente.
* **Algoritmos de retoceso**: de manera similar a la fuerza bruta, intente generar todas las soluciones posibles, pero cada vez que genere una prueba de solución si cumple con todas las condiciones, y solo entonces continúe generando soluciones posteriores. De lo contrario, retroceda y siga un camino diferente para encontrar la solución.

[**EL repositorio de algoritmos y estructuras de datos de javascript**](https://github.com/trekhleb/javascript-algorithms)

## **Jugando con estructuras de datos en JavaScript**
(Anish K.)

Javascript está evolucionando cada día. Con el rápido crecimiento de marcos y plataformas como React , Angular , Vue , NodeJS , Electron , React Native , se ha vuelto bastante común usar JavaScript para aplicaciones a gran escala.
También significa que ahora es importante comprender cuál es la herramienta adecuada para un problema específico, para escribir aplicaciones eficientes y de alto rendimiento en javascript.

**Las estructuras de datos** son muy importantes en este contexto. 

### **Pilas**

![alt text](https://miro.medium.com/max/1400/0*bbTnCsiT0klMbi3q.png)

Las pilas son fáciles de entender. si observa la imagen de arriba, puede comprender la mayoría de las propiedades de la pila. El primer elemento en la pila que se muestra arriba es la parte 'roja' (con 'fuera' escrito sobre ella). Pero sería el último elemento en eliminarse, ya que debemos eliminar el resto de elementos, apilados encima de él, antes de sacarlo. En pocas palabras, el primer elemento de la pila es el último elemento que se elimina.
Pasemos a un código para ver la implementación exacta y, con suerte, eso aclararía mucho las cosas. Estaría usando la sintaxis ES6 para mantener las cosas más limpias y fáciles de comprender.
Así es como debería verse nuestra pila:
```javascript
class Stack { 
  constructor(){ 
    
  } 
  
  push(item){ 
    //empujar elemento a la pila 
  } 
  
  pop(){ 
    //sacar el elemento superior (último elemento) de la pila 
  } 
  
  peek(){ 
    //ver cuál es el último elemento en la pila 
  } 
  
  tamaño(){ 
    //no. de elementos en la pila 
  } 
  
  isEmpty(){ 
    // devuelve si la pila está vacía o no 
  } 
}
```
Comencemos a unir las piezas. Podemos usar una matriz debajo del capó para almacenar los elementos de la pila, así:
```javascript
clase Pila { 
  constructor(){ 
    this._items = []; 
  } 
  ...
```
_(guión bajo) antes de que se haya usado un nombre de variable para transmitir que se supone que esta variable se usa solo internamente, y que ninguna instancia de Stack debería acceder a ella de todos modos.

Implementemos las otras piezas faltantes de nuestra pila:
```javascript
class Stack { 
  constructor(){ 
    this._items = [] 
  } 
  
  push(item){ 
    //empujar elemento a la pila 
    return this._items.push(item) 
  } 
  
  pop(){ 
    //sacar el elemento superior (último elemento ) from stack 
    return this._items.pop() 
  } 
  
  peek(){ 
    // ver cuál es el último elemento en la pila 
    return this._items[this._items.length-1] 
  } 
  
  size(){ 
    //no. de elementos en la pila 
    devuelve this._items.length 
  } 
  
  isEmpty(){ 
    // devuelve si la pila está vacía o no 
    devuelve this._items.length==0 
  } 
}
```
Tenemos listas la estructura básica para la pila. Pero todavía hay algunas características que nos gustaría implementar:
1. Tras la inicialización, podemos pasar algunos elementos a la pila.
2. Deberíamos poder apilar y desapilar varios elementos a la vez.
```javascript
class Stack { 
  constructor(...items){ 
    this._items = [] 
    
    if(items.length>0) 
      items.forEach(item => this._items.push(item) ) 
      
  } 
  
  push(...items){ 
    //empujar elemento a la pila 
     items.forEach(item => this._items.push(item) ) 
     return this._items; 
    
  } 
  
  pop(count=0){ 
    //sacar el elemento superior (último elemento) de la pila 
    if(count===0) 
      devuelve this._items.pop() 
     else 
       devuelve this._items.splice( -count, count ) 
  } 
  
  peek(){ 
    // ver cuál es el último elemento en la pila 
    devuelve this._items[this._items.length-1] 
  } 
  
  size(){
    //no. de elementos en la pila 
    return this._items.length 
  } 
  
  isEmpty(){ 
    // devuelve si la pila está vacía o no 
    return this._items.length==0 
  } 
  
  toArray(){ 
    return _items; 
  } 
}
```
Ahora nuestra implementación de pila se ve bastante mejor. Pero todavía tenemos que resolver algunos problemas:

1. Cualquier puede acceder a nuestra variable _items y cambiarla externamente. Podemos usar WeakMap en ES6 para resolver este problema.
2. Nuestra pila debe tener un espacio de nombres adecuado para evitar contaminar el espacio de nombres global. Para esto, envolveremos toda nuestra implementación en un cierre.
3. Necesitamos una forma de ver los elementos actuales en nuestra pila. Para esto agregaremos un método toArray()

Aquí está la implementación final, con estos problemas resueltos:
```javascript
let Stack = (()=>{ 
  
let map = new WeakMap(); 
let _items = [];
  class Stack { 
  constructor(...items){ 
    // almacenemos nuestra matriz de elementos dentro del mapa débil 
    map.set(this, []); 
    // obtengamos la matriz de elementos del mapa y almacenémosla en _items para facilitar el acceso a otros lugares 
    _items = map.get(this); 
    
    //si el constructor recibe algún elemento, debe apilarse 
    this.push(..items) 
      
  } 
  
  push(...items){ 
    //empujar elemento a la pila 
     items.forEach(item => _items.push(item) ) 
     devolver _elementos; 
    
  } 
  
  pop(count=1){ 
    //sacar el elemento superior (último elemento) de la pila 
       return _items.splice( -count, count ) 
  } 
  
  peek(){ 
    // ver cuál es el último elemento de la pila
    return _items[_items.length-1] 
  } 
  
  size(){ 
    //no. de elementos en la pila 
    return _items.length 
  } 
  
  isEmpty(){ 
    // devuelve si la pila está vacía o no 
    return _items.length==0 
  } 
  
   toArray(){ 
     return _items; 
   }
}
  
pila de retorno;
})();
```

### **Uso de la pila**

Aquí  hay un pequeño fragmento que ilustra cómo podemos usar esta pila:
```javascript
let my_stack = new Stack(1,24,4); 
// [1, 24, 4]
mi_pila.push(23) 
//[1, 24, 4, 23]
mi_pila.push(1,2,342); 
//[1, 24, 4, 23, 1, 2, 342]
mi_pila.pop(); 
//[1, 24, 4, 23, 1, 2]
mi_pila.pop(3) 
//[1, 24, 4]
my_stack.isEmpty() 
// falso
mi_pila.tamaño(); 
//3
mi_pila.toArray(); 
//[1, 24, 4]
```
