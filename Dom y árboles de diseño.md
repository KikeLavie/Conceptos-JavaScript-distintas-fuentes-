# DOM Y Árbol de diseño

**¿Qué es el DOM?**

El DOM(Document Object Model) es una interfaz que representa cómo el navegador lee sus documentos HTML y XML. Permite que un lenguaje (JavaScript) manipule, estructure y diseñe su sitio web. Después de que el navegador lee su documento HTML, crea un árbol de representación llamado Modelo de objeto del documento y define cómo se puede acceder a ese árbol.

### **Ventajas**
Al manipular el DOM, tiene infinitas posibilidades. Puedes crear aplicaciones que actualicen los datos de la página sin necesidad de un refresco. Además, puede crear aplicaciones personalizables por el usuario y luego cambiar el diseño de la página sin actualizar. Puede arrastrar, mover, y eliminar elementos. 

Como dije tiene infinitas posibilidades, solo necesitas usar tu creatividad.

1. **Documento**: Trata todos los documentos HTML.
2. **Elementos**: todas las etiquetas que están dentro de su HTML o XML se convierten en un elemento DOM.
3. **Texto**: Todo el contenido de las etiquetas. 
4. **Atributos**: todos los atributos de un elemento HTML específico. En la imágen, el atributo class="hero" es un atributo del elemento <p>.

### **Manipulando el DOM**
Ahora estamos llegando a la mejor parte: comenzar a manipular el DOM. primero, vamos a crear un elemento HTML como ejemplo para ver algunos métodos y cómo funcionan los eventos.
```
<!DOCTYPE html> <html lang="pt-br"> <head>
   <meta charset="UTF-8">      <meta name="viewport" content="width=device-width, initial-scale=1.0">      <meta http-equiv="X-UA-Compatible" content="ie=edge">      <title>Entendendo o DOM (Document Object Model)</title>  </head>  <body>      <div class="container">          <h1><time>00:00:00</time></h1>          <button id="start">Start</button>          <button id="stop">Stop</button>          <button id="reset">Reset</button>      </div>  </body>  </html>
   ```
   Muy sencillo,¿verdad? Ahora vamos a aprender más sobre los métodos DOM: Cómo obtener nuestros elementos y comenzar a manipularlos.

   ### **Métodos**
   El DOM tiene muchos métodos. Son la conexión entre nuestros nodos (elementos) y eventos, algo de lo que aprenderemos más adelante. Te mostraré algunos de los métodos más importantes y cómo se usan. Hay muchos más métodos que no voy a mostrarte aquí, pero puedes verlos todo aquí 
   [I´m an inline-style link](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)

   ### **getElementByld()**
   Este método devuelve el elemento que contiene el ID de nombre pasado. Como sabemos, las identificaciones deben ser únicas, por lo que es un método muy útil para obtener solo el elemento que desea.
     
```
var myStart=document.getElementById(´start´);
 ```
 **myStart**: nuestro nombre de variable que se parece a nuestra identificación pasada.

 **inicio**: id aprobado. Y en caso de que no tengamos ningún id con ese nombre en concreto, devuelve nulo.

 ### **getElementsByClassName()**
 Este método devuelve una HTMLCollection de todos los elementos que contienen la clase de nombre específica pasada. 
  ```
  var myContainer = document.getElementsByClassName('container');
  ```
  **myContainer**: nuestro nombre de variable que se parece a nuestra clase aprobada.
  **.container**: nuestra clase pasó. En caso de que no tengamos ninguna clase con ese nombre en concreto, devuelve nulo.

  ### **getElementsByTagName()**
  Esto funciona de la misma manera que los métodos anteriores: también devuelve una HTMLCollection, pero esta vez con una sola diferencia: devuelve todos esos elementos con el nombre de la etiqueta pasado.

 ```
    var buttons = document.getElementsByTagName(´button´);
```
  **botones** : nuestro nombre de variable que se parece a nuestro nombre de etiqueta pasado.

**button** : nombre de la etiqueta que queremos obtener.

### **selector de consulta()**
Devuelve el primer elemento que tiene pasado el selector CSS específico. Solo recuerda que el selector debe seguir la sintaxis CSS . En caso de no tener ningún selector , devuelve nulo .
```
var resetButton = document.querySelector('#reset');
```
**resetButton** : nuestro nombre de variable que se parece a nuestro selector pasado.

**#reset** : el selector pasó, y si no tiene ningún selector que coincida, devuelve nulo .

### **consultaSelectorAll()**
Muy similar al método     `querySelector()` , pero con una única diferencia: devuelve todos los elementos que coinciden con el selector CSS pasado. El selector también debe seguir la sintaxis CSS . En caso de no tener ningún selector , devuelve nulo .
```
var myButtons = document.querySelector('#buttons');
```
**myButtons** : nuestro nombre de variable que se parece a nuestros selectores pasados.

**#botones** : selector pasado, si no tiene ningún selector que coincida, devuelve nulo .

Estos son algunos de los métodos DOM más utilizados. Hay varios métodos más que puede usar, como createElement(), que crea un nuevo elemento en su página HTML, y setAttribute() que le permite establecer nuevos atributos para elementos HTML. Puedes explorarlos por tu cuenta.

Nuevamente, puede encontrar todos los métodos aquí , en el lado izquierdo en Métodos . Realmente le recomiendo que eche un vistazo a algunos de los otros porque es posible que necesite uno de ellos pronto.

Ahora, vamos a aprender acerca de los eventos ; después de todo, sin ellos no podemos hacer animaciones en nuestras páginas.

## **Eventos**
Los elementos DOM tienen métodos, como acabamos de comentar, pero también tienen eventos. Estos eventos son los encargados de hacer posible la interactividad en nuestra página. Pero aquí hay una cosa que quizás no sepas: los eventos también son métodos. 
### **hacer clic**
Uno de los eventos más utilizados es el evento clic. Cuando el usuario hace clic en un elemento específico, realizará alguna acción.
```
myStart.addEventListener(´click´, function(event) {
```

```
//Do something here.
```

```
} , false);
```

Los parámetros de addEventListener() son:
1. El tipo de evento que desea(en este caso clic´).
2. Una función de devolución de llamada.
3. El *useCaputure* por defecto es falso. Indica si desea o no "capturar" el evento.

Seleccione
Este evento es para cuando desea enviar algo cuando se selecciona un elemento específico. En ese caso, enviaremos una alerta simple .

myStart.addEventListener('select', function(event) {
alert('Element selected!');
}, false);
Estos son algunos de los eventos más utilizados. Por supuesto, tenemos muchos otros eventos que puede usar, como eventos de arrastrar y soltar: cuando un usuario comienza a arrastrar algún elemento, puede realizar alguna acción, y cuando suelta ese elemento, puede realizar otra acción.

Ahora, veremos cómo podemos atravesar el DOM y usar algunas propiedades.

Atravesando el DOM
Puede atravesar el DOM y usar algunas propiedades que veremos ahora. Con estas propiedades, puede devolver elementos, comentarios, texto, etc.

.childNodes
Esta propiedad devuelve una lista de nodos de nodos secundarios del elemento dado. Devuelve texto, comentarios, etc. Entonces, cuando quieras usarlo, debes tener cuidado.

var container = document.querySelector('.container');
var getContainerChilds = container.childNodes;
.primer hijo
Esta propiedad devuelve el primer hijo del elemento dado.

var container = document.querySelector('.container');
var getFirstChild = container.firstChild;
.nombre del nodo
Esta propiedad devuelve el nombre del elemento dado. En este caso, pasamos un div , por lo que devolverá " div ".

var container = document.querySelector('.container');
var getName = container.nodeName;
.nodeValue
Esta propiedad es específica para textos y comentarios , ya que devuelve o establece el valor del nodo actual . En este caso, dado que pasamos un div, devolverá nulo .

var container = document.querySelector('.container')
var getValue = container.nodeValue;
.tipo de nodo
Esta propiedad devuelve el tipo del elemento dado. En este caso, devuelve “ 1 ”.

var container = document.querySelector('.container')
var getValue = container.nodeType;
Pero, ¿qué significa " 1 " de todos modos? Es básicamente el tipo de nodo del elemento dado. En este caso, es un _ELEMENT_NODE_ y devuelve nulo. Si esto fuera un atributo, nos sería devuelto como " 2 " y el valor del atributo.

![alt text](dom1.png)

### **Elementos**
Estas propiedades, en lugar de las anteriores, nos devuelven solo elementos . Se usan y recomiendan con más frecuencia porque pueden causar menos confusión y son más fáciles de entender.

### **.parentNode**

Esta propiedad devuelve el padre del nodo dado.
```javascript
var container = document.querySelector('.container')
var getParent = container.parentNode;
```

### **.firstElementChild**

Devuelve el primer elemento secundario del elemento dado.
```javascript
var container = document.querySelector('.container')
var getValue = container.firstElementChild;
```

### **.lastElementChild**
Devuelve el último elemento secundario del elemento dado.
```javascript
var container = document.querySelector('.container')
```
```javascript
var getValue = container.
lastElementChild;
```

Estas son algunas de las muchas propiedades que tiene el DOM. Es muy importante que conozcas los conceptos básicos sobre el DOM, cómo funciona, sus métodos y propiedades, porque algún día puedes necesitarlo.

### **Conclusión**
El DOM nos proporciona varias API importantes para crear aplicaciones fantásticas e innovadoras. Si entiendes los conceptos básicos, puedes crear cosas increíbles. Si desea leer más sobre el DOM, puede hacer clic aquí y leer todos los documentos de MDN.