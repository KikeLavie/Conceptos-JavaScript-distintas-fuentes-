# Motores Javascript #

Los motores de JavaScript son una tecnología compleja, y saber por qué las diferentes plataformas usan diferentes motores es esencial para los desarrolladores que intentan producir código optimizado en el menor tiempo posible.

### **Máquinas virtuales**
 
Un motor de JavaScript a menudo se define como un tipo de máquina virtual o emulación basada en software de un sistema informático determinado. Hay muchos tipos de máquinas virtuales y se clasifican según la precisión con la que pueden emular o sustituir máquinas físicas reales.

Una máquina virtual de sistema, por ejemplo, proporciona una emulación completa de una plataforma en la que se puede ejecutar un sistema operativo. Los usuarios de Mac están familiarizados con Parallels, una máquina virtual de sistema que permite ejecutar Windows en una Mac.

Una **máquina virtual** de proceso, por otro lado, es menos funcional y solo puede ejecutar un programa o proceso. Wine es una máquina virtual de proceso que permite que las aplicaciones de Windows se ejecuten en una máquina Linux, pero no proporciona un sistema operativo Windows completo en una caja Linux.

Un motor de JavaScript es un tipo de máquina virtual de proceso que está diseñado específicamente para interpretar y ejecutar código JavaScript. Es importante diferenciar entre los motores de diseño que impulsan un navegador al estructurar una página web y los motores de JavaScript de nivel inferior que interpretan y ejecutan código.

¿Qué es un motor JavaScript?
El trabajo básico de un motor JavaScript es tomar el código JavaScript que escribe un desarrollador y convertirlo en un código rápido y optimizado que pueda ser interpretado por un navegador o incluso incrustado en una aplicación.

Más precisamente, cada motor de JavaScript implementa una versión de ECMAScript, del cual JavaScript es un dialecto. A medida que ECMAScript evoluciona, también lo hacen los motores de JavaScript. Hay tantos motores diferentes porque cada uno está diseñado para funcionar con un navegador web diferente, un navegador sin interfaz o un tiempo de ejecución como Node.js. Los navegadores sin cabeza son navegadores web sin una interfaz gráfica de usuario que son útiles para ejecutar pruebas automatizadas en productos web. Un buen ejemplo es PhantomJS. Node.js es un marco asíncrono basado en eventos que permite que JavaScript se use en el lado del servidor. Dado que estas son herramientas impulsadas por JavaScript, funcionan con motores de JavaScript.

Hay una variedad de motores de JavaScript disponibles para analizar, analizar y ejecutar código del lado del cliente. Con cada lanzamiento de la versión del navegador, el motor de JavaScript puede cambiarse u optimizarse para mantenerse al día con la ejecución de código JavaScript de última generación.

¿Cómo funciona un motor JavaScript?
Dada la definición de una máquina virtual, tiene sentido denominar un motor JavaScript como una máquina virtual de proceso, ya que su único propósito es leer y compilar código JavaScript. Esto no significa que sea un motor simple. JavaScriptCore, por ejemplo, tiene seis bloques de construcción que analizan, interpretan, optimizan y recolectan basura del código JavaScript.

Entonces, ¿cómo funciona esto? Esto depende, por supuesto, del motor. Los dos principales motores de interés son JavaScriptCore de WebKit y el motor V8 de Google porque son aprovechados por NativeScript. Estos dos motores manejan el código de procesamiento de manera diferente.

JavaScriptCore realiza una serie de pasos para interpretar y optimizar un script. Realiza un análisis léxico, descomponiendo la fuente en una serie de tokens o cadenas con un significado identificado. Luego, el analizador analiza los tokens en busca de sintaxis y los integra en un árbol de sintaxis. Luego se activan cuatro procesos justo a tiempo, que analizan y ejecutan el código de bytes producido por el analizador. En términos simples, este motor de JavaScript toma el código fuente, lo divide en cadenas, también conocido como lex, toma esas cadenas y las convierte en un código de bytes que un compilador puede entender y luego lo ejecuta.

El motor V8 de Google, escrito en C++, también compila y ejecuta código fuente de JavaScript, maneja la asignación de memoria y recolecta basura sobrantes. Su diseño consta de dos compiladores que ensamblan el código fuente directamente en el código máquina.

Estos compiladores son Full-codegen, un compilador rápido que produce código no optimizado y Crankshaf, un compilador más lento que produce código rápido y optimizado.

Si Crankshaft determina que el código no optimizado generado por Full-codegen necesita optimización, lo reemplaza, un proceso conocido como 'crankshafting'.

Una vez que el proceso de compilación produce el código de máquina, el motor expone todos los tipos de datos, operadores, objetos y funciones especificados en el estándar ECMA al navegador o cualquier tiempo de ejecución que necesite usarlos, como NativeScript.

¿Qué significa esto para los desarrolladores?
El objetivo del proceso de análisis y ejecución de código de un motor de JavaScript es generar el código más optimizado en el menor tiempo posible.

La conclusión es que la evolución de estos motores es paralela a la búsqueda de la evolución de las esferas web y móvil para que funcionen lo mejor posible. Para realizar un seguimiento de esta evolución, los gráficos de evaluación comparativa producidos en sitios como arewefastyet.com muestran el rendimiento de varios motores en comparación entre sí.

Cualquier desarrollador web debe ser consciente de las diferencias inherentes a los navegadores que muestran el código que se produce, depura y mantiene. Más específicamente, es importante comprender por qué ciertos scripts pueden funcionar más lentamente en un navegador que en otro.

De manera similar, los desarrolladores móviles, especialmente aquellos que escriben aplicaciones móviles híbridas usando una vista web para mostrar su contenido, o usan un tiempo de ejecución como NativeScript, querrán saber qué motores están interpretando su código JavaScript. Los desarrolladores web móviles deben comprender las limitaciones inherentes y las posibilidades que ofrecen los distintos navegadores en sus pequeños dispositivos. Mantenerse al día con los cambios en los motores de JavaScript realmente valdrá la pena para aquellos que buscan evolucionar como desarrolladores web, móviles o de aplicaciones.