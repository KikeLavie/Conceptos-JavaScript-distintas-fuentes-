# Operadores bit a bit, arreglos de tipos y búferes de arreglos #

### **¿Que son los bits?**

Técnicamente para la computadora todo se reduce a 1 y 0. No opera con dígitos ni caracteres ni cadenas, utiliza únicamente dígitos binarios(bits).
La versión corta de esta explicación es que todo se almacena en forma binaria. Luego, la computadora usa codificaciones como UTF-8 para asignar las combinaciones de bits guardadas a caracteres, dígitos o símbolos diferentes(la versión ELI5)

Cuanto más bits tenga, más permutaciones y más cosas podrá representar. 

Tomemos el número 113 por ejemplo. La forma más fácil en JS de obtener su forma binaria es así: Number(113).toString(2). Esto nos dará 1110001. Sabiendo que todo son solo fragmentos debajo del capó, ahora veremos cómo podemos manipularlos.

Muchos artículos tienen ejemplos con números hexadecimales. En este, solo veremos números decimales y binarios. El razonamiento detrás de eso es que encuentro que esto es más intuitivo de entender. En caso de duda, básicamente puede escribir los bits y todas las operaciones en una hoja de papel y rastrear lo que está sucediendo.

Otra cosa a tener en cuenta es que no hay forma de ingresar binarios directamente en JavaScript. Si desea convertir un número binario a decimal, puede usar la parseIntfunción: parseInt(1111, 2) // 15.

### **& (Y)**
Al igual que el `&&` operador lógico que ya hemos estado usando en nuestras tareas diarias de programación, este devolverá 1 si ambos bits comparados son 1 y 0 en todos los demás casos. Toma un número en ambos lados (número, no su forma binaria) y luego compara sus bits uno por uno.

Visualicemos eso. Los números 12y 15tienen las representaciones binarias de 1100y 1111. Usemos el &operador en esos números. Si acaba de cerrar la sesión, lo recibirá de 12nuevo. Extraño, ¿eso hizo algo?

Sí, comparó cada bit de 12con el bit correspondiente de 15y, debido a cómo funciona el operador, 1100volvió a obtenerlo, que de hecho es 12.

Un truco interesante con el &operador es averiguar si un número es par o impar. Si un número es impar, su primer bit siempre será 1. Por lo tanto, podemos usar &y comparar el número con 1, por lo que si el número es impar, el resultado siempre será 1. Sin embargo, no recomiendo usar esto en su base de código real porque no está tan claro lo que está haciendo.

### **| (O)**
Se utiliza para comparar dos números binarios bit a bit devolviendo un 1 para cada comparación en la que hay al menos uno(1),y 0, cuando los bits comparados son ambos 0. Si tomamos el ejemplo anterior y usamos este operador 12 | 15, devolverá 15. ¿porque?

1100| 1111 devolverá un 1 para cada comparación que nuevamente es igual a 1111 o 15.

### **~ (NO)**
Este es un NO bit a bit. El resultado es un número negativo en la aritmética de complemento a dos. Lo que haces es que revierte todos los bits de 1 a 0 y viceversa. 

Sin embargo, si cierra la sesión ~15, verá que el resultado es, -16, aunque los bits sean correctos. Esto sedebe a que en la aritmética del complemento a dos, para obtener la representación negativa de un número, primero debe invertir sus bits y luego agregarle 1. 

Puede buscarlo en Google para obtener una mejor explicación, pero esta es una de esas cosas que debe dar por sentado.

### **^ (XOR)**

Este operador se conoce como operador XOR u OR exclusivo. Al igual que los operadores & y | toma los números de ambos lados, diferenciando en la forma en que hace la comparación.

Comparará los bits correspondientes y devolverá un 1 solo cuando solo haya un solo 1. Tal vez esta no sea una buena explicación, así que vamos a dar una más visual. 1 ^ 0regresará 1_ Pero 1 ^ 1volveré 0.

El ^operador devuelve 1 solo en el caso específico en el que comparamos 1con 0.

### **Operadores de desplazamiento**

Hay dos operadores que se ocupan del desplazamiento de bits:  >>y <<. Como puede adivinar, la diferencia entre ellos es la posición en la que desplazan los bits de un número.

El <<operador desplaza todos los bits de un número nveces. Lo que hay que tener en cuenta aquí es que los espacios vacíos que se producen cuando se desplaza el número se rellenan con ceros.

El >>operador, por otro lado, se desplaza hacia la derecha. La diferencia entre este y el operador de desplazamiento anterior es que éste llenará los bits de un número positivo con 0 y los bits de un número negativo con 1.

Aquí está el lugar para señalar que, por lo general, el primer bit del número se usa para representar su signo. Si es un 1entonces es negativo, si es 0es positivo. De ahí el razonamiento detrás del cambio a la derecha: está destinado a mantener el signo del número que estamos cambiando.

### **Manipulación de bits**
Ahora que sabemos lo que hacen los operadores, echemos un vistazo a cómo podemos utilizarlos para manipular bits.

Digamos que queremos establecer un bit en una posición dada. Queremos que el segundo bit de la derecha se establezca (to be 1). Esto nos lleva al concepto de máscaras . Las máscaras son números en forma binaria donde solo tenemos configurado el bit que queremos modificar 1o 0dependiendo de lo que queramos lograr. También podemos decir que se utilizan como bandera para definir qué bits se van a cambiar.

Si queremos establecer el primer bit, la máscara será 0001. En caso de que queramos poner el segundo será 0010y así sucesivamente.

Un ejemplo lo hará más claro:
```javascript
const setBit = (num, position) => {
  let mask = 1 << position
  return num | mask
}

// Set the bit at position 1
setBit(12, 1) // return 14 -> 1110
```
Hasta aquí todo bien. Veamos cómo podemos borrar un poco: configúrelo en 0. Esto no será tan fácil como el ejemplo anterior porque necesitamos mantener todos los demás bits como están.
```javascript
const clearBit = (num, position) => {
  // We use the ~/NOT operator after placing the bit
  // We want 1s everywhere and 0 only where we want to modify
  let mask = ~(1 << position)
  
  // We use AND which will modify only the bits compared to 0
  return num & mask
}

clearBit(15, 1) // 12 -> 1100
```
La diferencia aquí es que queremos tener una máscara llena de 1 y 0solo en la posición donde queremos borrar. Luego usamos el &operador que establecerá solo la posición requerida en 0.

Ahora sabemos cómo configurar y borrar bits, pero ¿qué pasa si queremos voltearlo? No sabemos si el bit está configurado o no, pero realmente deseamos cambiar su estado actual. Este es un trabajo para XOR.
```javascript
const flipBit = (num, position) => {
  let mask = 1 << position
  // If the current state of the bit is 0, XOR will return 1
  // If the bit is 1, XOR will set it to 0
  return num ^ mask
}

flipBit(15, 1) // 13 -> 1101
```
XOR se usa en este caso porque usarlo con 1uno garantiza cambiar el valor del bit.

**Conclusión**

La intención de esta publicación fue cubrir los fundamentos de la manipulación de bits y los operadores. Aunque apenas hemos arañado la superficie, esto es suficiente para desmitificar el tema para que puedas aventurarte en el mundo de los 1 y 0 por tu cuenta y hacer diferentes tipos de travesuras bit a bit.

Gracias por leer y espero que este artículo te haya ayudado. ¡Puede ayudarme manteniendo presionado el botón de aplaudir por un momento (juego de palabras) y compartiendo este artículo con un amigo que pueda estar interesado!
