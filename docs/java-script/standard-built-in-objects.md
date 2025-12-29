# Standard Built-in Objects


##  Introducción

En JavaScript, no empiezas desde cero. El lenguaje viene con una "caja de herramientas" preinstalada lista para usar en cualquier parte de tu código, sin necesidad de importar librerías externas. A estos se les llama **Objetos Estándar Incorporados**.

¿Qué son exactamente?

Son objetos que existen en el **ámbito global** (Global Scope). Esto significa que están disponibles en cualquier script y en cualquier momento.

Se dividen principalmente en tres categorías:

1.  **Envoltorios de Primitivos:** Objetos que nos permiten tratar datos simples (texto, números) como si fueran objetos complejos (`String`, `Number`, `Boolean`).
2.  **Estructuras de Datos:** Herramientas para organizar información (`Array`, `Map`, `Set`, `Object`).
3.  **Utilidades:** Objetos que funcionan como una librería de funciones matemáticas, manejo de fechas o procesamiento de datos (`Math`, `Date`, `JSON`).

¿Qué nos proporcionan?

Estos objetos nos facilitan la vida proporcionando dos cosas clave: **Propiedades** (datos) y **Métodos** (acciones).

1. Métodos de manipulación
En lugar de escribir una función compleja para buscar un texto dentro de otro, el objeto `String` ya trae `.includes()`. En lugar de calcular una raíz cuadrada a mano, `Math` trae `.sqrt()`.

2. Conversión de Tipos
Nos permiten transformar datos de un tipo a otro de forma segura.
Ejemplo: `Number("10")` convierte un string a número.

3. Constantes Universales
Valores que nunca cambian y son necesarios matemáticamente o por configuración.
Ejemplo: `Math.PI` o `Number.MAX_VALUE`.

## Strings

JavaScript proporciona una gran cantidad de métodos para manipular texto.

⚠️ **Inmutabilidad:**
Recuerda que los Strings en JS son **inmutables**. Ninguno de estos métodos modifica la variable original; todos devuelven un **nuevo string** con los cambios aplicados.

---
### Longitud

**length**

```javascript
const str = 'hola';
console.log(str.length); // 4
```
Devuelve el número de caracteres de un string.
> ⚠️ OJO: Es una propiedad, no un método, por lo tanto no se llama con los ()

### Acceso a caracteres

Para obtener un carácter a través de un índice podemos hacerlo de la forma normal con [] o con el método .at()(índices negativos)

```javascript
const str = 'Hola';
console.log(str[0]); // H
// [] vs .at()
console.log(str[str.length - 1]); // a
console.log(str.at(-1)); // a
```

### Capitalización

```javascript
const str = 'Hola';
console.log(str.toLowerCase); // hola
console.log(str.toUpperCase); // HOLA
```

### Subcadenas

**str.indexOf**
```javascript
const frase = "El gato persigue al ratón y el gato juega.";

// 1. Búsqueda básica
// Encuentra la primera vez que aparece "gato"
const primeraPosicion = frase.indexOf("gato");
console.log(primeraPosicion); 
// Salida: 3 (Recuerda: los índices empiezan a contar en 0: E-0, l-1, [espacio]-2, g-3)


// 2. Cuando el texto NO existe
// Si no lo encuentra, siempre devuelve -1
const noEncontrado = frase.indexOf("perro");
console.log(noEncontrado); 
// Salida: -1


// 3. Case Sensitive (Distingue mayúsculas)
// "Gato" con mayúscula no es lo mismo que "gato"
const mayuscula = frase.indexOf("Gato");
console.log(mayuscula); 
// Salida: -1


// 4. Usando el segundo parámetro (fromIndex)
// Le decimos: "Empieza a buscar a partir del índice 10"
// Así saltamos el primer "gato" y encontramos el segundo
const segundaPosicion = frase.indexOf("gato", 10);
console.log(segundaPosicion); 
// Salida: 31 (La posición del segundo "gato")

// 5. Comprobación en condiciones 
if (frese.indexOf('El') != -1) {
    console.log('Existe');
}
// debemos realizarlo así ya que en casos como este que la subcadena
// comienza en cero, no entraría por el if correctamente.
```

**includes, startsWith, endsWith**

```javascript
const texto = "Curso de JavaScript";

// 1. includes() -> ¿Contiene este texto en cualquier parte?
console.log(texto.includes("JavaScript")); // true
console.log(texto.includes("Python"));     // false


// 2. startsWith() -> ¿Empieza exactamente con esto?
console.log(texto.startsWith("Curso"));    // true
console.log(texto.startsWith("curso"));    // false 


// 3. endsWith() -> ¿Termina exactamente con esto?
console.log(texto.endsWith("Script"));     // true
console.log(texto.endsWith("Java"));
```
**substring y slice**

Ambos sirven para extraer una parte del string sin modificar el original. Funcionan casi igual (inicio, fin), pero tienen diferencias clave en los casos "raros".

El parámetro fin es exclusivo (no se incluye el carácter de esa posición).


```js
const str = "JavaScript";

// 1. Uso Básico (Idénticos)
// Extraer desde índice 0 hasta el 4 (sin incluir el 4)
console.log(str.slice(0, 4));      // "Java"
console.log(str.substring(0, 4));  // "Java"

console.log(str.slice(4)) // desde el 4 hacia adelante
str.slice(-4, -1) // rip, slice permite negativos
```

| Método                          | Selecciona…                                              | Negativos |
|---------------------------------|----------------------------------------------------------|-----------|
| `slice(comienzo, final)`        | Desde `comienzo` hasta `final` (sin incluir `final`)     | Permite negativos |
| `substring(comienzo, final)`    | Entre `comienzo` y `final` (no incluye `final`)          | Valores negativos se tratan como `0` |

Lo recomendable es usar slice.

### Compararar strings

Los strings en JavaScript son codificados usando UTF-16. Por lo que cuando ordenamos se basan en esto.

**str.codePointAt(pos)**

Devuelve un número decimal que representa el código de carácter en la posición pos

**String.fromCodePoint(code)**

Crea un carácter por su código numérico

```js
console.log('a' > 'z'); // faslse

console.log("z".codePointAt(0)); // 122

console.log(String.fromCodePoint(90)); // Z
console.log(String.fromCodePoint(0x5a)); // Se puede pasar en hexadecimal también
```

Comparar strings correctamente es complejo porque **cada idioma tiene su propio alfabeto y reglas**. Letras que parecen iguales pueden ocupar **posiciones distintas** según el lenguaje.

Para resolver esto, los navegadores modernos implementan el estándar **ECMA 402**, que permite comparaciones internacionalizadas.

`str.localeCompare(str2)`
Este método compara dos strings según las reglas del idioma y devuelve:

- `1` → `str` es mayor que `str2`
- `-1` → `str` es menor que `str2`
- `0` → ambos strings son equivalentes

## Números

En JavaScript moderno, existen dos tipos principales de números:

1. **Números regulares:** Almacenados en formato de 64 bits (IEEE-754), también conocidos como números de coma flotante de doble precisión. Son los que usamos la gran mayoría del tiempo.

2. **BigInt:** Utilizados para enteros que superan el límite seguro de ±(2⁵³-1).

A continuación, nos centraremos en los **números regulares**.

Para facilitar la lectura de cifras grandes, JavaScript permite el uso de guiones bajos `_` como separadores visuales. El motor ignora estos caracteres.

```js
const num = 1_000_000_000;
```

También podemos usar la notación científica añadiendo la letra "e". Esto multiplica el número por 1 con la cantidad de ceros indicada después de la "e".

```js
const num = 3e3; // 3 * 1000 = 3000
const decimalNum = 2.5e6; // 2.5 * 1000000 = 2500000
```

Para micro decimales, un número negativo tras la "e" indica una división por 1 con esa cantidad de ceros.

```js
const num = 1e-6; // 1 / 1000000 = 0.000001
```

**Sistemas numéricos alternativos**

JavaScript soporta prefijos para escribir en otros sistemas además del decimal:

- Hexadecimal (0x): Común para colores y bytes.

- Binario (0b):

- Octal (0o):

```js
const hex = 0xAF;      // 175
const binario = 0b101; // 5
const octal = 0o10;    // 8
```

**Conversión a String con base: toString(base)**

El método num.toString(base) convierte un número a su representación en cadena en la base especificada (entre 2 y 36).

```js
const n = 200;
console.log(n.toString(16)); // "c8" (hexadecimal)
console.log(n.toString(2));  // "11001000" (binario)
```

Nota sobre sintaxis: Si intentas llamar a este método directamente sobre un número entero literal, debes usar dos puntos .. o paréntesis para evitar errores de sintaxis, ya que JS espera decimales tras el primer punto.

```js
123..toString(36); // Correcto
(123).toString(36); // Correcto
```

**Redondeo**

Existen varias funciones en el objeto `Math` para tratar decimales:

| Método        | Descripción                                  | Ejemplo (Entrada 5.7) | Ejemplo (Entrada -2.3) |
|--------------|----------------------------------------------|------------------------|-------------------------|
| Math.floor   | Devuelve el entero mayor ≤ al número (−∞)    | 5                      | -3                      |
| Math.ceil    | Devuelve el entero menor ≥ al número (+∞)    | 6                      | -2                      |
| Math.round   | Al entero más cercano                         | 6                      | -2                      |
| Math.trunc   | Elimina los decimales (hacia 0)               | 5                      | -2                      |

**Redondear a una precisión específica**

Para redondear a n dígitos (ej. moneda), tenemos dos estrategias:

1. Multiplicar y dividir: Desplazar la coma, redondear y restaurar.

2. toFixed(n): Redondea y devuelve un string.

```js
const price = 15.6789;
alert( Math.round(price * 100) / 100 ); // 15.6789 -> 1567.89 -> 15 -> 15.67

// Método toFixed
console.log(precio.toFixed(2)); // "15.68" (String)

// Conversión a número con +
console.log(+precio.toFixed(2)); // 15.68 (Number)
```

**Problemas de Precisión (IEEE-754)**

Debido a que los números se guardan en formato binario de 64 bits, hay fracciones decimales simples (como 0.1) que son infinitas en binario. Esto causa pérdidas de precisión.

```js
console.log(0.1 + 0.2 === 0.3); // false
console.log(0.1 + 0.2); // 0.30000000000000004
```

Solución: Usar toFixed para redondear el resultado o trabajar con enteros (ej. céntimos en vez de euros) y dividir al final.

**Valores Especiales y Comprobaciones**

**Infinity y NaN**

- Infinity: Ocurre al desbordar la capacidad de almacenamiento.

- NaN (Not a Number): Resultado de errores matemáticos. NaN es único porque no es igual a nada, ni siquiera a sí mismo (NaN === NaN es false).

**Funciones de validación**

En un sentido, Number.isNaN y Number.isFinite son más simples y directas que las funciones isNaN e isFinite. Pero en la práctica isNaN e isFinite son las más usadas, porque son más cortas.

1. isNaN(valor) vs Number.isNaN(valor):

    * isNaN convierte el valor a número primero. isNaN("hola") es true.

    * Number.isNaN verifica que sea tipo number Y sea NaN. Number.isNaN("hola") es false.

2. isFinite(valor) vs Number.isFinite(valor):

    * Verifica si un número es regular (ni NaN, ni Infinity).

    * Number.isFinite no realiza conversión de tipos implícita.

3. Number.isInteger(valor):

    * Comprueba si un número es entero.

**Resumen:**

* isNaN = "¿Esto se convierte en un error matemático?"
* Number.isNaN = "¿Es esto el valor específico de error matemático?"

**Object.is**

Funciona como === pero maneja correctamente dos casos borde:

- Object.is(NaN, NaN) === true

- Object.is(0, -0) === false

**Parseo de Números (Lectura "suave")**

A diferencia de Number() o el unario + que son estrictos, parseInt y parseFloat leen una cadena hasta que encuentran un carácter no numérico y devuelven lo leído.

```js
console.log(parseInt("150px")); // 150
console.log(parseFloat("12.5em")); // 12.5
console.log(parseInt("a123")); // NaN (falla si el primer carácter no es numérico)
```

parseInt acepta un segundo argumento para la base (radix), útil para hex o binario:

```js
console.log(parseInt("ff", 16)); // 255
```

**Objeto Math**

Contiene funciones matemáticas y constantes.

- Math.random(): Aleatorio entre 0 y 1 (excluyendo el 1).

- Math.max(a, b...) / Math.min(a, b...): Devuelve el mayor/menor argumento.

- Math.pow(n, p): Eleva n a la potencia p.

## Arrays

Los objetos son excelentes para guardar datos etiquetados (clave-valor), pero cuando necesitamos **orden** (primero, segundo, tercero...), los objetos se quedan cortos.

Para colecciones ordenadas existe el **Array** (también llamado matriz, arreglo o vector).

### Creación de Arrays

Existen dos formas, aunque la segunda es la estándar:

```javascript
// 1. Sintaxis literal (La más usada)
const fruits = ["Manzana", "Naranja", "Pera"]; 
const emptyFruit = [];

// 2. Constructor (Rara vez se usa)
const emptyArray = new Array();
```

> Array.isArray(valor): La única forma fiable de saber si algo es un array (porque typeof [] da object).

### Acceso y Modificación

Los elementos se enumeran comenzando desde el índice 0.

```js
const data = ["Hola", "Mundo"];

// Leer
console.log(data[0]); // "Hola"

// Modificar
data[1] = "Javascript"; // ["Hola", "Javascript"]

// Añadir nuevo índice
data[2] = "Adios"; // ["Hola", "Javascript", "Adios"]

// Longitud
console.log(data.length); // 3
```

> Un array puede mezclar tipos de datos (strings, números, objetos, funciones...).

---

#### Acceso al último elemento (at)

Tradicionalmente se usaba `arr[arr.length - 1]`.  
Ahora podemos usar el método `.at()` que acepta índices negativos para contar desde el final.

```js
let lista = ["A", "B", "C"];

console.log(lista[lista.length - 1]); // "C" (Clásico)
console.log(lista.at(-1));            // "C" (Moderno y limpio)
```

### Colas y Pilas (Métodos básicos)

Los arrays en JS funcionan como Deque (Double-ended queue), permitiendo operar eficientemente en ambos extremos.

**Final del Array (Muy rápidos)**

- **push(...items)**: Añade al final.

- **pop()**: Extrae y devuelve el último elemento.

```js

const stack = ["Plato 1"];
stack.push("Plato 2"); // ["Plato 1", "Plato 2"]
const last = stack.pop(); // Devuelve "Plato 2" y el array queda ["Plato 1"]
```

**Principio del Array (Más lentos)**

Son más lentos porque al quitar/poner el primero, el motor debe renumerar todos los índices siguientes (el 1 pasa a ser 0, el 2 a ser 1, etc.).

- **shift()**: Extrae y devuelve el primer elemento.
- **unshift(...items)**: Añade al principio.

```js
const queue = ["Cliente 1", "Cliente 2"];
const firstClient = queue.shift(); // Saca "Cliente 1". Queda ["Cliente 2"]
queue.unshift("Cliente VIP"); // Queda ["Cliente VIP", "Cliente 2"]
```


**Puntos en común**

- shift y pop, devuelven el elemento eliminando, y solo eliminan de uno en uno.

- unshift y push, devuelven la nueva length del array, y pueden agregar varios argumentos a la vez.

### Búsqueda

**`indexOf(item, fromIndex)`**

Busca un elemento comenzando desde el principio (izquierda a derecha) y te dice **dónde está**.

* **¿Qué devuelve?** El **índice (número)** de la *primera* vez que aparece el elemento.

* **¿Si no está?** Devuelve **`-1`**.

* **Comparación:** Usa igualdad estricta (`===`).

```javascript
const fruits = ['Manzana', 'Pera', 'Manzana', 'Uva'];

console.log(fruits.indexOf('Pera'));    // 1
console.log(fruits.indexOf('Manzana')); // 0 (Solo devuelve la primera que encuentra)
console.log(fruits.indexOf('Melón'));   // -1 (No existe)
```

El segundo parámetro (fromIndex): Puedes decirle desde qué índice empezar a buscar.

```js
// Buscar 'Manzana' pero empezando a mirar desde el índice 1
console.log(fruits.indexOf('Manzana', 1)); // 2
```

**`lastIndexOf(item, fromIndex)`**

Funciona igual que indexOf, pero busca de derecha a izquierda (empieza por el final del array).

**`includes(item, fromIndex)`**

Es la versión moderna (ES2016). En lugar de decirte la posición, te dice si existe o no.

* ¿Qué devuelve? Un booleano: true o false.

* Ventaja clave: Es el único método que puede encontrar NaN.

**`find(funcion)`**

Recorre el array y ejecuta la función para cada elemento hasta que uno devuelva `true`.

* **Devuelve:** El **primer elemento** (el valor en sí) que cumpla la condición.
* **Si no encuentra nada:** Devuelve **`undefined`**.

```javascript
const inventario = [
    { nombre: "Manzanas", cantidad: 2 },
    { nombre: "Plátanos", cantidad: 0 },
    { nombre: "Cerezas", cantidad: 5 }
];

// Queremos encontrar el primer objeto que tenga cantidad 0
const resultado = inventario.find(fruta => fruta.cantidad === 0);

console.log(resultado); 
// Resultado: { nombre: "Plátanos", cantidad: 0 }
```

**`findIndex(funcion)`**

Hace exactamente la misma búsqueda que `find`, pero le interesa la **posición**.

- **Devuelve**: El índice (número) del primer elemento que cumpla la condición.

- **Si no encuentra nada**: Devuelve `-1`.

```js
const numeros = [5, 12, 8, 130, 44];

// Queremos saber la POSICIÓN del primer número mayor que 100
const indice = numeros.findIndex(num => num > 100);

console.log(indice); 
// Resultado: 3 (Porque el 130 está en el índice 3)
```

### Transformar y reordenar

**`map(funcion)`**

Se utiliza cuando quieres **transformar** cada elemento de un array.

* **Entrada:** Un array de longitud *N*.

* **Salida:** Un **NUEVO array** de la **MISMA longitud** *N*, con los elementos modificados.

* **Analogía:** Una fábrica. Entra materia prima (metal), la máquina la procesa, y salen piezas terminadas.

```javascript
const numeros = [1, 2, 3, 4];

// Queremos el doble de cada número
const dobles = numeros.map(num => num * 2);

console.log(numeros); // [1, 2, 3, 4] (Original intacto)
console.log(dobles);  // [2, 4, 6, 8] (Nuevo array transformado)
```

**`filter(funcion)`**

Se utiliza cuando quieres seleccionar un subconjunto de elementos.

* Entrada: Un array de longitud N.

* Salida: Un NUEVO array de longitud igual o menor a N.

* Funcionamiento: La función debe devolver true (se queda) o false (se va).

```js
const precios = [100, 50, 200, 30];

// Queremos solo los precios baratos (menores de 80)
const baratos = precios.filter(precio => precio < 80);

console.log(baratos); // [50, 30]
```

`reduce(funcion, valorInicial)`

Se utiliza para **reducir todo el array a un único valor** (aunque ese valor puede ser un número, un objeto, o incluso otro array).

**Entrada:** Un array.  
**Salida:** Un único valor acumulado.

Argumentos

- **acumulador**: El resultado de la vuelta anterior.
- **actual**: El elemento que estamos recorriendo ahora.

```javascript
arr.reduce((acumulador, actual) => acumulador + actual, valorInicial);

const numeros = [10, 20, 30];

const total = numeros.reduce((acumulador, numeroActual) => {
    return acumulador + numeroActual;
}, 0); // <--- El 0 es el valor inicial del acumulador

console.log(total); // 60
```

**`split(separador)`**

Este método divide un objeto de tipo String en un array de cadenas mediante la separación de la cadena en subcadenas.

* **Método de:** String.

* **Devuelve:** Un **Nuevo Array**.

```javascript
const frase = "Hola-mundo-cruel";

// 1. Separar por un carácter específico (guión)
// Elimina el separador y crea elementos nuevos
const palabras = frase.split("-");
console.log(palabras); // ["Hola", "mundo", "cruel"]

// 2. Separar por espacios (típico para contar palabras)
const texto = "Aprender JavaScript es divertido";
const arrayPalabras = texto.split(" ");
console.log(arrayPalabras); // ["Aprender", "JavaScript", "es", "divertido"]

// 3. Separar letra por letra (String vacío)
// Muy útil para manipular caracteres individualmente
const letras = "Hola".split("");
console.log(letras); // ["H", "o", "l", "a"]
```

**`join(unión)`**

Une todos los elementos de un array (o un objeto similar a un array) en una cadena y devuelve esta cadena.

* Método de: Array.

* Devuelve: Un String.

```js
const tecnologias = ["HTML", "CSS", "JS"];

// 1. Unir con un separador visual
console.log(tecnologias.join(" / ")); // "HTML / CSS / JS"

// 2. Unir sin nada (pegado total)
console.log(tecnologias.join("")); // "HTMLCSSJS"

// 3. Por defecto (sin argumentos)
// Si no le pasas nada, JS usa una coma automáticamente
console.log(tecnologias.join()); // "HTML,CSS,JS"
```

**`sort(funcionComparadora)`**

Ordena los elementos de un array localmente y devuelve el array ordenado.

⚠️ El gran peligro de sort() por defecto
Si usas sort() sin argumentos, JavaScript convierte cada elemento a String y compara sus códigos UTF-16 (orden alfabético/lexicográfico).

Esto es un desastre para los números:

```js
const numeros = [1, 5, 10, 2, 25];

// ERROR COMÚN:
numeros.sort();
console.log(numeros); 
// Resultado: [1, 10, 2, 25, 5] 
// ¿Por qué? Porque el texto "10" va antes que "2" alfabéticamente.
```

✅ La Solución: La Función de Comparación
Para ordenar números correctamente, debemos pasarle una función que tome dos valores (a y b) y devuelva un número:

* Si devuelve negativo: a va antes que b.

* Si devuelve positivo: b va antes que a.

* Si devuelve cero: Se quedan igual.

Fórmulas Mágicas:

```js
const precios = [1, 5, 10, 2, 25];

// Orden Ascendente (Menor a Mayor): a - b
precios.sort((a, b) => a - b); 
console.log(precios); // [1, 2, 5, 10, 25]

// Orden Descendente (Mayor a Menor): b - a
precios.sort((a, b) => b - a);
console.log(precios); // [25, 10, 5, 2, 1]
```

**`reverse()`**

Invierte el orden de los elementos del array "in-place" (en el mismo sitio). El primero pasa a ser el último, y viceversa.

* **Devuelve:** El mismo array (la referencia al original), ya invertido.

```javascript
const cuentaAtras = [1, 2, 3, 4, 5];

cuentaAtras.reverse();

console.log(cuentaAtras); // [5, 4, 3, 2, 1] (El original ha cambiado)
```

### Some y Every

**`some(funcion)`**

Comprueba si **al menos un elemento** del array cumple la condición.

* **Funcionamiento:** Recorre el array y se detiene en cuanto encuentra el **primer** `true`.
* **Analogía:** El operador **OR (`||`)**. "¿Hay algún médico en la sala?". Con que uno levante la mano, la respuesta es SÍ (`true`).

```javascript
const temperaturas = [-5, 10, 25, 8];

// ¿Hay alguna temperatura bajo cero (helada)?
const hayHelada = temperaturas.some(temp => temp < 0);

console.log(hayHelada); 
// Resultado: true (Porque encontró el -5 y paró de buscar)
```

**`every(funcion)`**

Comprueba si todos y cada uno de los elementos cumplen la condición.

- Funcionamiento: Recorre el array y se detiene en cuanto encuentra el primer false (un intruso).

- Analogía: El operador AND (&&). "¿Sois todos mayores de edad?". Si uno solo dice "tengo 15", la respuesta global es NO (false).

```js
const notas = [10, 8, 9, 4];

// ¿Han aprobado TODOS los alumnos (nota >= 5)?
const todosAprobados = notas.every(nota => nota >= 5);

console.log(todosAprobados); 
// Resultado: false (Falló al llegar al 4 y paró)
```

### Slice y Splide

**`slice(inicio, final)`**

Crea un **nuevo array** copiando una porción del original. El array original se queda **intacto**.

* **Significado:** "Rebanada" o "Trozo".

* **Parámetros:**
    * `inicio`: Índice donde empieza el corte (incluido).
    * `final` (Opcional): Índice donde termina el corte (**NO incluido**). Si no se pone, va hasta el final.

```javascript
const original = ["a", "b", "c", "d", "e"];

// Copiar desde el índice 1 hasta el 3 (sin incluir el 3)
const copia = original.slice(1, 3);

console.log(copia);    // ["b", "c"]  (El nuevo trozo)
console.log(original); // ["a", "b", "c", "d", "e"] (INTACTO)
```

Trucos de slice:

* Clonar un array: arr.slice() (sin argumentos) crea una copia superficial exacta.

* Últimos elementos: arr.slice(-2) te da los dos últimos.

**`splice(inicio, cantidadBorrar, ...insertar)`**

Este método es una "navaja suiza". Modifica el array original insertando, borrando o reemplazando elementos "in-place".

Devuelve: Un array con los elementos eliminados (no el array modificado).

**A. Borrar elementos**

```js
const meses = ["Ene", "Feb", "Mar", "Abr"];

// Desde índice 1, borra 1 elemento
const eliminados = meses.splice(1, 1);

console.log(meses);      // ["Ene", "Mar", "Abr"] (¡Cambió!)
console.log(eliminados); // ["Feb"] (Lo que sacamos)
```

**B. Insertar elementos (sin borrar)**
Ponemos el segundo parámetro (deleteCount) a 0.

```js

const colores = ["Rojo", "Azul"];

// En la posición 1, no borres nada, mete "Verde"
colores.splice(1, 0, "Verde");

console.log(colores); // ["Rojo", "Verde", "Azul"]

```
**C. Reemplazar (Borrar e Insertar a la vez)**
```js

const menu = ["Hamburguesa", "Papas", "Soda"];

// En pos 1, borra 1 ("Papas") y mete "Ensalada"
menu.splice(1, 1, "Ensalada");

console.log(menu); // ["Hamburguesa", "Ensalada", "Soda"]
```

## For Each

El método **`.forEach()`** ejecuta una función **una vez por cada elemento** de una colección. 

Se puede utilizar en Arrays, sets y maps.

👉 No es para transformar datos, es para **ejecutar acciones**.

**Sintaxis básica**

```js

array.forEach((elemento, indice, arrayCompleto) => {
    // código a ejecutar
});
```

**Parámetros del callback:**

- `elemento`: valor actual (el más usado)

- `indice`: posición (0, 1, 2…)

- `arrayCompleto`: array original (poco usado)


**Solo el elemento (caso más común)**

```js

const frutas = ["Manzana", "Pera", "Uva"];

frutas.forEach(fruta => {
    console.log("Me gusta la " + fruta);
});
```


**Usando el índice**

```js

const ranking = ["Oro", "Plata", "Bronce"];

ranking.forEach((medalla, posicion) => {
    console.log(`Puesto ${posicion + 1}: ${medalla}`);
});
```


**NO devuelve nada**

- Siempre devuelve `undefined`

- No crea un array nuevo

```js

const nums = [1, 2, 3];
const resultado = nums.forEach(n => n * 2);

console.log(resultado); // undefined

👉 Si necesitas un array nuevo → usa `.map()`
```


**NO se puede detener**

- `break` y `continue` **NO funcionan**

- El método siempre recorre todo el array

```js
nums.forEach(n => {
    if (n > 2) {
        return; // solo salta esta iteración
    }
    console.log(n);
});
```


**Uso en `Map` y `Set`**

- **Map:** `(valor, clave, map)`
- **Set:** `(valor, valor, set)`


**Comparativa rápida: `forEach` vs `map`**

| Característica | forEach() | map() |
|---------------|-----------|-------|
| Objetivo | Ejecutar acciones | Transformar datos |
| Devuelve | `undefined` | Nuevo array |
| Modifica original | No (salvo que lo hagas) | No |
| Uso típico | Logs, DOM, BD | Crear arrays nuevos |

---

**Regla mental rápida**

- **Usa `map`** → cuando quieres **otra lista**
- **Usa `forEach`** → cuando quieres **hacer algo**, no obtener algo


## Maps y Sets

### Maps

**`new Map()`**

**Explicación:**

Es el **constructor**. Crea una nueva instancia de un objeto Map en memoria.

* Si lo llamas sin argumentos `new Map()`, crea un mapa vacío.

* Si le pasas un **iterable** (generalmente un array de arrays), lo usa para rellenar el mapa inicialmente. Cada sub-array debe tener exactamente dos elementos: `[clave, valor]`.

**Ejemplo:**
```javascript
// 1. Crear vacío
const mapaVacio = new Map();

// 2. Crear con datos iniciales (Array de Arrays)
const precios = new Map([
    ['manzana', 1.5],  // Clave: 'manzana', Valor: 1.5
    ['pera', 2.0],     // Clave: 'pera', Valor: 2.0
    ['uva', 3.5]       // Clave: 'uva', Valor: 3.5
]);

console.log(precios); // Map(3) { 'manzana' => 1.5, 'pera' => 2, 'uva' => 3.5 }
```

---

**`map.set(clave, valor)`**
**Explicación:**

Este método **agrega** un nuevo par clave-valor al mapa.

* **Importante:** Si la clave **ya existe**, el método **sobrescribe** el valor antiguo con el nuevo. No duplica la clave.

* **Retorno:** Devuelve el propio objeto Map actualizado. Esto permite el **"encadenamiento"** (chaining), es decir, llamar a varios `.set()` seguidos en la misma línea.

**Ejemplo:**
```javascript
const usuarios = new Map();

// Agregar uno por uno
usuarios.set('admin', 'Juan Pérez');
usuarios.set('invitado', 'Ana Gómez');

// Sobreescribir (Actualizar valor)
usuarios.set('admin', 'Juan ACTUALIZADO'); 

// Encadenamiento (Chaining)
usuarios
    .set(1, 'usuario numero uno')
    .set(true, 'usuario activo');

console.log(usuarios.get('admin')); // "Juan ACTUALIZADO"
console.log(usuarios.get(1));       // "usuario numero uno"
```

---

**`map.get(clave)`**

**Explicación:**

Recupera el valor almacenado asociado a una clave específica.

* Busca la clave usando el algoritmo *SameValueZero* (casi igual a `===`).

* **Ojo:** Si la clave no se encuentra, devuelve `undefined`. Esto puede ser ambiguo si tú guardaste explícitamente un `undefined` como valor, por lo que a veces es mejor usar `has()` antes.

**Ejemplo:**
```javascript
const inventario = new Map([
    ['teclado', 50],
    ['mouse', 25]
]);

// 1. Clave que existe
console.log(inventario.get('teclado')); // 50

// 2. Clave que NO existe
console.log(inventario.get('monitor')); // undefined

// ⚠️ Detalle técnico con Objetos como clave:
const objClave = { id: 1 };
inventario.set(objClave, 'Datos del objeto');

// Tienes que usar LA MISMA referencia de memoria
console.log(inventario.get(objClave)); // "Datos del objeto"
console.log(inventario.get({ id: 1 })); // undefined (Parecen iguales, pero son objetos distintos en memoria)
```

---

**`map.has(clave)`**

**Explicación:**

Verifica la **existencia** de una clave. Es la forma correcta de preguntar "¿Está esto en el mapa?".

* Devuelve un **Booleano**: `true` si la clave está, `false` si no.

* Es muy rápido (más eficiente que buscar en un array).

**Ejemplo:**

```javascript
const codigos = new Map([
    ['ES', 'España'],
    ['MX', 'México']
]);

if (codigos.has('MX')) {
    console.log("México está en la lista");
} else {
    console.log("No encontrado");
}

console.log(codigos.has('AR')); // false
```

---

**`map.delete(clave)`**

**Explicación:**

Elimina el par clave-valor específico del mapa.

* Devuelve `true` si el elemento existía y fue borrado.

* Devuelve `false` si el elemento no existía (no hizo nada).

**Ejemplo:**
```javascript
const tareas = new Map([
    [1, 'Comprar leche'],
    [2, 'Pagar luz']
]);

// Intentamos borrar la tarea 1
const fueBorrado = tareas.delete(1); 

console.log(fueBorrado); // true (Lo borró exitosamente)
console.log(tareas.has(1)); // false (Ya no existe)

// Intentamos borrar algo que no existe
console.log(tareas.delete(99)); // false
```

---

**`map.clear()`**

**Explicación:**

Es el botón de **"Formatear"**. Elimina **todos** los elementos del mapa de golpe.

* No devuelve nada útil (`undefined`).

* El mapa queda vacío (`size` pasa a ser 0), pero la variable `map` sigue existiendo y se puede volver a usar.

**Ejemplo:**
```javascript
const papelera = new Map([
    ['archivo1', 'tesis.doc'],
    ['archivo2', 'foto.jpg']
]);

console.log(papelera.size); // 2

papelera.clear(); // ¡BOOM! Todo fuera.

console.log(papelera.size); // 0
console.log(papelera);      // Map(0) {}
```

---

**Propiedad `map.size`**

**Explicación:**

Es una **propiedad** (no un método, por eso no lleva paréntesis `()`). Devuelve un número entero que indica cuántas entradas (pares clave-valor) tiene el mapa actualmente.

* Equivale al `.length` de los arrays, pero para Maps se llama `.size`.

**Ejemplo:**
```javascript
const equipo = new Map();

equipo.set('portero', 'Courtois');
equipo.set('delantero', 'Vinicius');

console.log("Jugadores en el mapa: " + equipo.size); // Jugadores en el mapa: 2

equipo.delete('portero');

console.log(equipo.size); // 1
```

### Sets

**`new Set()`**

**Explicación:**

Es el **constructor**. Crea una nueva instancia de un conjunto en memoria.

* Si se llama sin argumentos `new Set()`, crea un conjunto vacío.

* Si se le pasa un **iterable** (como un Array), lo usa para inicializar el set.

* **Nota importante:** Si el iterable original contiene duplicados, el constructor los elimina automáticamente al crear el Set.

**Ejemplo:**

```javascript
// 1. Crear vacío
const vacio = new Set();

// 2. Crear con datos (Filtrado automático de duplicados)
const numeros = new Set([1, 2, 2, 2, 3]);

console.log(numeros); // Set(3) { 1, 2, 3 }
```

**`set.add(valor)`**

**Explicación:** Agrega un nuevo valor al conjunto.

* Unicidad: Si el valor ya existe, el método no hace nada (ignora la petición para mantener la unicidad).

* Nota: Devuelve el propio objeto set, lo que permite el encadenamiento de llamadas.

**Ejemplo:**

```js
const lista = new Set();

// Encadenar múltiples add
lista.add("A").add("B").add("C");

// Intentar agregar un duplicado (no pasa nada)
lista.add("A"); 

console.log(lista); // Set(3) { "A", "B", "C" }
```

**`set.delete(valor)`**

**Explicación:**  
Elimina el valor especificado del conjunto.

**Nota:** Devuelve un booleano (`true` o `false`):

- `true`: Si el valor existía y fue borrado.
- `false`: Si el valor no existía (no hubo cambios).

**Ejemplo:**

```js
const frutas = new Set(["Manzana", "Pera"]);

const borrado = frutas.delete("Manzana"); 
console.log(borrado); // true (Fue borrado)

const intento = frutas.delete("Piña");
console.log(intento); // false (No existía)
```

**`set.has(valor)`**

**Explicación:**  
Comprueba si un valor existe dentro del set.

**Nota:**  
Devuelve `true` si existe, `false` si no.

**Rendimiento:** 

Es extremadamente rápido (complejidad **O(1)**) comparado con buscar en un array, gracias a su estructura hash interna.

**Ejemplo:**

```js
const ids = new Set([101, 102]);

if (ids.has(101)) {
    console.log("ID encontrado");
}

console.log(ids.has(999)); // false
```

**`set.clear()`**

**Explicación:**  

Elimina todos los elementos del set de golpe, dejándolo vacío.

**Nota:**  

No devuelve nada (`undefined`). La propiedad `.size` pasa a ser `0`.

**Ejemplo:**

```js
const datos = new Set([1, 2, 3]);

console.log(datos.size); // 3

datos.clear();

console.log(datos.size); // 0
```

**`Propiedad set.size`**

Funciona igual que en Map

### Métodos de iteración

**`.keys()`**

* **Map:** Devuelve las **claves** reales (lo que usaste para guardar el dato).

* **Set:** Devuelve los **valores**. (Como no tiene claves, usa el valor como identificador).

**`.values()`**

* **Map:** Devuelve los **valores** (el dato guardado).

* **Set:** Devuelve los **valores** (Exactamente lo mismo que `.keys()` en un Set).

**`.entries()`**

* **Map:** Devuelve un array `[clave, valor]`.

* **Set:** Devuelve un array `[valor, valor]`.
    * *¿Por qué?* Por compatibilidad. Si tienes un código genérico que espera recibir parejas (dos argumentos), el Set duplica el valor para no romper ese código.

**`.forEach((val, key, collection) => ...)`**

* **Map:** La función recibe `(valor, clave, mapa)`.

* **Set:** La función recibe `(valor, valor, set)`.
    * El segundo argumento se repite intencionalmente para imitar la firma del Map.

---

**Diferencia en `for..of` (Bucle por defecto)**

Aunque tienen los mismos métodos, si los metes directamente en un `for..of` sin especificar método (iteración por defecto), se comportan distinto:

* `for (let x of map)` -> Itera sobre **.entries()** (Recibes `[clave, valor]`).

* `for (let x of set)` -> Itera sobre **.values()** (Recibes solo el `valor`).

---


Ejemplo 1: El curioso caso de `.entries()`

```javascript
const miMap = new Map([ ['user', 'Eduardo'] ]);
const miSet = new Set([ 'Eduardo' ]);

// En Map: [Clave, Valor]
// Obtenemos el primer elemento del iterador
console.log(miMap.entries().next().value);
// Resultado: ['user', 'Eduardo']

// En Set: [Valor, Valor] (¡Duplicado por compatibilidad!)
console.log(miSet.entries().next().value);
// Resultado: ['Eduardo', 'Eduardo']
```

**Ejemplo 2: forEach (Compatibilidad)**

Fíjate cómo podemos usar la misma estructura de callback, aunque en el Set el segundo argumento sea redundante.

```js
const mapa = new Map([['a', 10], ['b', 20]]);
const conjunto = new Set([10, 20]);

console.log("--- Iterando MAP ---");
map.forEach((valor, clave) => {
    console.log(`Clave: ${clave}, Valor: ${valor}`);
});
// Salida:
// Clave: a, Valor: 10
// Clave: b, Valor: 20

console.log("--- Iterando SET ---");
conjunto.forEach((valor, valorAgain) => {
    console.log(`Clave (falsa): ${valorAgain}, Valor: ${valor}`);
});
// Salida:
// Clave (falsa): 10, Valor: 10
// Clave (falsa): 20, Valor: 20
```

**Ejemplo 3: Iteración por defecto (for..of)**
Aquí se ve la única diferencia real de uso directo.

```js
const m = new Map([['x', 1]]);
const s = new Set(['x']);

// Map devuelve arrays
for (let item of m) {
    console.log(item); // ['x', 1]
}

// Set devuelve valores sueltos
for (let item of s) {
    console.log(item); // 'x'
}
```

## Objetos 

A diferencia de los maps, en los objetos debemos usar esta sintaxis para sacar, las claves, el valor o ambos.

* Object.keys(obj) – devuelve un array de propiedades.

* Object.values(obj) – devuelve un array de valores.

* Object.entries(obj) – devuelve un array de pares [propiedad, valor]

| Característica        | Map            | Objeto                     |
|-----------------------|----------------|----------------------------|
| Sintaxis de llamada   | map.keys()     | Object.keys(obj) (no obj.keys()) |
| Devuelve              | iterable       | Array “real”               |

* Object.fromEntries(iterable) - A partir de un array, crea un objeto.

* obj.hasOwnProperty(clave) - Evalúa si tiene una propiedad o no, es más seguro que `in` (busca en toda la cadena de herencia) que también sirve para eso.


**Object.assign(destino, ...fuentes)**

Copia todas las propiedades de uno o más objetos fuente al objeto destino.

* Ojo: Modifica el primer objeto (destino).

* Nota: Hoy en día se usa más el "Spread Operator" (...), pero assign sigue siendo útil.

* Uno de sus usos es la clonación de objetos planos. Para clonación anidada es mejor usar structuredClone, aunque también falla en algunos casos y hay que recurrir a librerías externas o a un código personalizado.

const target = { a: 1 };
const source = { b: 2 };

```js
const target = { a: 1 };
const source = { b: 2 };

Object.assign(target, source);

console.log(target); // { a: 1, b: 2 }
```
