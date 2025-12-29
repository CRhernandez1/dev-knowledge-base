# JavaScript

## Variables

| Declaración | ¿Reasignable? | Scope (Alcance) | Uso recomendado |
| :--- | :---: | :--- | :--- |
| **const** | ❌ No | Block Scope `{}` | **Por defecto (95%)** |
| **let** | ✅ Sí | Block Scope `{}` | Solo si el valor cambiará |
| **var** | ✅ Sí | Function Scope | **NUNCA** (Legacy) |

## Tipos de datos (Primitivos)

Podríamos definir primitivos como tipos de datos inmutables que no se asemejan a los objetos.

1. **Number:** Todo son flotantes (`10` y `10.5` son el mismo tipo). No hay `int` vs `float`.
2. **String:** Una cadena de cáracteres, que forma una palabra. (Ej: 'manzana').
3. **Boolean:** `true` / `false` (Ojo: en minúsculas).
4. **Undefined:** Valor de una variable declarada pero **aún no definida**. (Ej: `let x;`).
5. **Null:** Valor **intencionalmente** vacío. (Curiosidad "typeof symbol", mostrará object)
6. **Symbol:** (ES6) Es un valor único que no se iguala a ningún otro valor.
7. **BigInt:** (ES2020) Para enteros gigantes.

## Expressions y Statements

### Expressions (Expresiones)

**Definición:** Cualquier unidad de código que **produce un valor**.
Piensa en ellas como algo que **responde a una pregunta**. Si puedes ponerlo en el lado derecho de una asignación (`=`), es una expresión.

- **Ejemplos:**
    - `3 + 4` (Produce `7`)
    - `1991` (Produce `1991`)
    - `true && false` (Produce `false`)
    - `miFuncion()` (Si la función retorna algo, la llamada es una expresión).

> La prueba de fuego: Si puedes ponerlo dentro de un console.log( ... ), es una expresión.

### Statements (Sentencias / Declaraciones)

**Definición:** Es una instrucción completa, una acción. **Hacen cosas**, pero **no producen un valor** inmediato que puedas asignar a una variable.
Piensa en ellas como una "oración completa" que termina en punto y coma (`;`).

- **Ejemplos:**
    - `if (else) { ... }` (Es una estructura de control, no devuelve un valor).
    - `switch`
    - `for` / `while`
    - `const x = 5;` (Toda la línea es una *statement*, aunque `5` sea una *expression*).

> La prueba de fuego: No puedes hacer esto: const resultado = if (x > 0) { return 1 }. Eso daría error, porque un if es un Statement.

## Comentarios

1. **Línea simple:** Para notas breves.
2. **Bloque:** Para explicaciones largas o deshabilitar código.

```javascript
// 1. Comentario de una sola línea
const usuario = 'Dev'; // También sirve al final de una instrucción

/*
  2. Comentario de bloque (Multi-línea)
  Todo lo que esté aquí dentro será ignorado.
  Útil para notas extensas o 'apagar' secciones de código.
*/
```

## Comillas

En JavaScript, hay 3 tipos de comillas.

- **Comillas dobles:** "Hola"
- **Comillas simples:** 'Hola'
- **Backticks (comillas invertidas):** \`Hola`

Las comillas dobles y simples son comillas “sencillas” (es decir, funcionan igual). No hay diferencia entre ellas en JavaScript. Queda a elección del programador cual usar, pero por lo general se recomiendan las simples.

Los backticks son comillas de interpolación. Nos permiten inyectar variables y expresiones en una cadena de caracteres encerrándolas en ${...}. Además de hacer strings multilínea.

```javascript
const fistName = 'Cristian';
console.log(`Hello, ${name}`);
```

## Conversión de tipos

### Type Conversion vs. Type Coercion

Aquí es donde JS se vuelve "raro".

- **Conversion:** Manual. `Number('1991')`, `String(23)`.
- **Coercion:** Automática (JS lo hace por ti a escondidas).

**Las reglas del caos:**

1. El operador `+` convierte números a **Strings** si hay un string presente.
2. Los operadores `-`, `*` , `/` convierten strings a **Números**.

```JavaScript

console.log('I am ' + 23 + ' years old'); // 'I am 23 years old' (Todo a String)
console.log('23' - '10' - 3);             // 10 (Todo a Number)
console.log('23' + '10' + 3);             // '23103' (Concatenación)
console.log('4' * '2');                   // 8 (Number)  
console.log(+'7')                         // 7 (Ojo: Aquí lo convierte a Number)

```

### Conversiones (Boolean, String, Number)

#### Conversión a String

Ocurre cuando queremos la representación visual de un valor. Su uso más común es para pasar de números a string, de forma explícita.

```javascript
console.log(String(false));     // "false"
console.log(String(null));      // "null"
console.log(String(undefined)); // "undefined"
console.log(String(123));       // "123"
```

#### Conversión a Number

Para convertir de forma explícita a Number se haría de esta manera.

```javascript
let str = "98";
alert(typeof str); // string

let num = Number(str); // se convierte en 98
let conversionError = Number('754z') //Leer algo distinto a Number como la z, dará NaN
let emptyString = Number('') // Es cero
```

Dependiendo del tipo del valor a convertir tiene sus rarezas: 

| Valor de entrada | Se convierte en… | Notas |
| :--- | :--- | :--- |
| **`undefined`** | `NaN` | No es un número válido. |
| **`null`** | `0` | Se trata como "nada" o vacío numérico. |
| **`true` / `false`** | `1` / `0` | Booleans binarios. |
| **`string`** | `0`, `Number` o `NaN` | Se limpian los espacios antes de convertir. |

#### Conversión a Booleano

Los valores conocidos como falsy, que se explicarán a continuación, se evaluan a false. Mientras el resto evalúa como true

## Truthy & Falsy Values

JS intenta convertir cualquier cosa a booleano si lo pones en un `if`.

**Los 5 valores Falsy (Se convierten a `false`):**

1. `0`
2. `''` (String vacío)
3. `undefined`
4. `null`
5. `NaN` (Not a Number)

**Todo lo demás es Truthy.**

> ⚠️ En JS, un array vacío [] y objeto vacío {} son true.

## Operadores

### Operadores Arítmeticos

Antes que nada es importante distinguir cuando un operador es unario y cuando binario. Si el operador solo tiene un operando, se considera unario.

```javascript
const number = 5;
console.log(-number) // Lo convierte a negativo, -5
console.log(+'5') // Lo convierte a Number, 5
```

Los binarios operan como en la matemática tradicional, y tenemos los siguientes:

| Operador | Precedencia | Asociatividad | Ejemplo |
| :---: | :---: | :--- | :--- |
| **`**`** | **13** | **Derecha** a Izquierda ⬅️ | `2 ** 3 ** 2` ➔ `512` |
| **`*`** | **12** | Izquierda a Derecha ➡️ | `2 * 3 * 2` ➔ `12` |
| **`/`** | **12** | Izquierda a Derecha ➡️ | `10 / 2 / 5` ➔ `1` |
| **`%`** | **12** | Izquierda a Derecha ➡️ | `10 % 3` ➔ `1` |
| **`+`** | **11** | Izquierda a Derecha ➡️ | `1 + 2` ➔ `3` |
| **`-`** | **11** | Izquierda a Derecha ➡️ | `5 - 2` ➔ `3` |

> Cabe destacar, que funcionan así cuando los dos operando son Numbers, sino se empezarían a aplicar las reglas que hemos visto anteriormente de "coercion". Por ejemplo el operador "+", fuera del ámbito arítmetico en JS también sirve para concatenar strings.

### Operadores Lógicos

- `and` -> `&&`
- `or` -> `||`
- `not` -> `!`

### Operador Nullish Coalescing '??'

El operador `||` puede genear problemas cuando solo queremos evaluar si algo es undefined o null, ya que también cuenta el `0` o `''` como false.

El operador nullish arregla esto, ya que solo evalúa como falso, undefined y null.

```javascript
const myMark = 0;

myMark || 'No se ha corregido' //En este caso si que tiene nota, tiene un 0, por lo que fallaría.
myMark ?? 'No se ha corregido' // En este caso mostraría correctamente el 0 como nota.
```

### Operador ternario y typeof

`typeof` es un operador especial que sirve para saber el tipo de dato de un valor o variable.

```javascript
const number = 5;
console.log(typeof number) //Number
console.log(typeof '7') //String
```
El operador ternanrio sirve para expresar un condicional de una manera más compacta.

```javascript
const age = 24;
const isAdult = 24 >= 18 ? 'Es mayor de edad' : 'Es menor de edad';
console.log(isAdult); //Es mayor de edad
```

### Precedence

Todo los operadores se rigen a unas normas para establecer cuales se aplican antes. Esto lo podemos ver más en profundidad. En la [documentación](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_precedence).

> El paréntesis tiene la precedencia más alta, asi que lo podemos usar como lo haríamos en matemáticas, cuando sea necesario, o para aclarar el orden visual de las operaciones para las futuras personas que puedan leer el código.

## Short Circuiting (&& y ||)

En JS, estos operadores no devuelven `true/false`, devuelven **el valor**.

### OR (`||`)

Devuelve el **primer valor TRUTHY** que encuentre. Si todos son falsos, devuelve el último.

- **Uso antiguo:** Asignar valores por defecto.

```javascript
// Si restaurant.guests es 0, undefined o null... guests1 será 10.
const guests1 = restaurant.numGuests || 10;
```

- ⚠️ **Bug:** Si `numGuests` es `0` (un número válido de invitados), JS lo toma como falsy y pone 10. **Error lógico grave**. Para esto fue diseñado el operador nullish explicado anteriormente.

### AND (`&&`)

Devuelve el **primer valor FALSY**. Si todos son verdaderos, devuelve el último.

- **Uso:** Reemplazar `if` simples.

```javascript
// Si orderPizza existe, ejecútala. Si no, no hagas nada.
restaurant.orderPizza && restaurant.orderPizza('Mushrooms');
```

## Equality Operators: `==` vs `===`

El `===` a diferencia de los dos iguales comprueba que el tipo de dato y el valor sean iguales, mientras que el otro solo se fija en el valor, no le importa que tipo de dato sea, porque realiza coerción.

- **`==` (Loose equality):** Hace **Coerción de tipo**. `'18' == 18` es `true`. **No es recomendable.**
- **`===` (Strict equality):** No hace coerción. `'18' === 18` es `false`. **Es preferible usar este aunque haya que hacer conversiones explícitas.**

```javascript
const num = '18';
if (num === 18) console.log('Strict'); // False (String != Number)
if (num == 18)  console.log('Loose');  // True (JS convierte string a numero)`
```

## Condicionales y Switch

### if - else

En JS por lo general el codigo que se ejecutará al evaluarse una condición va entre llaves, salvo que solo se quiera ejecutar una sentencia que en este caso se podrían omitir las llaves. Pero por lo general se recomienda usar las llaves

```javascript
const myMark = 7;

if (myMark >= 5) console.log('Aprobado'); // Sin llaves

if (myMark >= 5) {
    console.log('Aprobado') // Con llaves (opcional)
}

if (myMark >= 5) {
    const result = 'Aprobado';
    console.log(result); // Con llaves (obligario, por ser más de una sentencia)
}
```

La estructura general de los condicionales no cambian mucho con respecto a otros lenguajes.

```javascript
const age = 25;
if (age > 18) {
    console.log('Mayor de 18');
} else if (age < 18) {
    console.log('Menor de 18');
} else {
    console.log('Tiene exactamente 18');
}

// También se podría expresar así, porque en este caso solo es una stament por bloque.

if (age > 18) console.log('Mayor de 18');
else if (age < 18) console.log('Menor de 18');
else console.log('Exactamente 18');
```

### Switch

Cada vez se usa menos, pero es importante conocerlo.

- Usa comparación estricta (`===`).
- ¡Necesita `break` al final de cada caso o ejecuta el siguiente!

JavaScript

`const day = 'monday';
switch(day) {
    case 'monday': // day === 'monday'
        console.log('Plan course structure');
        break; // ¡Importante! case:
    default:
        console.log('Not a valid day');
}`

#### Agrupar varios `case` (Fall-through)

En JavaScript, si no pones la palabra `break`, el código "cae" hacia abajo y ejecuta el siguiente caso. Esto se usa para agrupar lógica.

**Escenario:** Clasificar días en Laborales vs. Fin de semana.

```javascript
const day = 'wednesday';

switch (day) {
    case 'monday': // No hay break, sigue bajando
    case 'tuesday':
    case 'wednesday':
    case 'thursday':
    case 'friday':
        console.log('📅 Es día laboral, a trabajar!');
        // Aquí SÍ ponemos break para que pare y no siga bajando
        break;

    case 'saturday':
    case 'sunday':
        console.log('🎉 Es fin de semana, descansa.');
        break;

    default:
        console.log('❌ Ese día no existe.');
}
```

####  Usar comparaciones (`>=`, `<`, `&&`) con `switch(true)`

El `switch` estándar de JS compara valores exactos (`===`). No puedes hacer `case age > 18` directamente.

Para lograr esto, usamos un **"Hack" muy común**: Hacemos el switch sobre el valor `true`.
JS preguntará: *"¿Es `true` igual a la condición de este caso?"*.

**Escenario:** Clasificar por rangos de edad.


```javascript
const age = 25;

// Truco: Hacemos switch sobre 'true'
switch (true) {
    case age >= 18 && age < 65:
        console.log('🧑 Adulto en edad laboral');
        break;
    
    case age >= 65:
        console.log('👴 Jubilado');
        break;

    case age < 18 && age >= 0:
        console.log('👶 Menor de edad');
        break;

    default:
        console.log('👻 Edad no válida');
}
```

La mayoría de veces es mejor usar un if-else que recurrir a un switch(true)

## Bucles

### Bucle for

#### Tradicional

```
for (begin; condition; step) {
    // codigo
}
```

```javascript
for (let i = 0; i < 10; i++) {
    console.log(i);
}
```
- Primero se inicializa una varible, con let porque cambiara en cada iteración.
- A continuación se comprueba la condición si 0 < 10.
- Si es verdad se ejecuta el codigo definido dentro del bloque.
- Después se le suma 1 y se pasa a la siguiente iteración.
- Esto continua en bucle, de ahí su nombre, hasta que no se cumpla la condición.

> Curiosidad: No se suele usar, pero se pueden omitir partes del bucle. Ejemplo: for(;;){}

#### for of

```javascript
const animals = ['gato', 'perro', 'gallina']
for (const animal of animals) {
    console.log(animal); // En este bucle se muestra cada elemento de la lista. Una similitud sería el for de Python
}
```

#### for in 

El bucle **`for...in`** se utiliza para **recorrer las propiedades (claves)** de un objeto. En cada iteración, devuelve el **nombre de la clave**, no el valor directamente.

```javascript
for (let key in object) {
  // código a ejecutar
}
```

### While

#### While común

Es el while de todo la vida, se suele usar cuando no sabes cuantas veces tendrás que iterar.

```javascript
let tries = 0;

while (tries < 3) {
    console.log(`${tries}`)
    tries++;
}

// También se puede hacer. Pero suele ser peor práctica. No es recomendable, salvó casos necesarios.
while (true) {
    if (tries === 3) {
        break;
    }
    console.log(`${tries}`)
    tries++;
}
```

#### Do While

Es similar al bucle `while`, pero con una diferencia crucial: **garantiza que el bloque de código se ejecute al menos una vez**.

Primero ejecuta el cuerpo del bucle y **después** evalúa la condición. Es ideal para validar datos de entrada (como un `prompt`), ya que necesitamos preguntar al usuario al menos una vez antes de decidir si volvemos a preguntar.

```javascript
let number; // Definimos number fuera, porque si lo hacemos dentro de las llaves no será accesible desde el exterior.

do {
    number = Number(prompt('Introduce un numero del 1 al 10'));
    // El bucle se repite SI el número es menor a 1 O mayor a 10
} while (number < 0 || number > 10);
```
## Funciones

### Tipos principales

#### Function Declaration

```javascript
function calcAge1(birthYear) {
    return 2037 - birthYear;
}
```

> **Característica Clave:** Pueden ser llamadas **antes** de ser definidas en el código gracias al **Hoisting** (elevación). El motor de JS las lee antes de ejecutar nada.

#### Function Expression

Guardamos una función (anónima) dentro de una variable.


```javascript
const calcAge2 = function (birthYear) {
    return 2037 - birthYear;
}
```

> **Característica Clave:** No tienen Hoisting. Te obligan a definir antes de usar (lo cual fomenta un código más ordenado). En JS, **una función es un valor**, igual que un número o un string.

#### Arrow Function 

Sintaxis moderna, rápida y limpia para funciones.

```javascript
const calcAge3 = (birthYear) => 2037 - birthYear;
// En caso de tener solo un argumento no es obligatorio poner el paréntesis, pero se recomienda ponerlo siempre por uniformidad del código.

```
- Cuando la función tiene una sola expresión, el valor se retorna de forma implícita.

- Si tiene un solo parámetro, los paréntesis no son obligatorios, pero se recomienda usarlos por consistencia y legibilidad.

```javascript
const calcAge3 = (birthYear) => {
    const result = 2037 - birthYear;
    return result;
} 
```

Cuando la función tiene más de una instrucción (statement), es obligatorio:

- Usar llaves {}

- Usar return explícito


> **Diferencia Crítica:** No tienen su propio `this`. Toman el `this` del padre (lexical `this`).

### Párametros por defecto

- **Evaluación Dinámica:** En JS, los parámetros por defecto se evalúan en tiempo de llamada, lo que permite usar parámetros anteriores para calcular los siguientes.
    
    ```javascript
    const booking = function(flightNum, numPassengers = 1, price = 199 * numPassengers) {
        // 'price' se calcula usando 'numPassengers' al vuelo
    };
    ```
    
- **Saltar argumentos:** No puedes hacer `booking('LH123', , 200)`. Si quieres usar el default del medio, tienes que pasar explícitamente `undefined`.
    - `booking('LH123', undefined, 200)` -> `numPassengers` será 1.

### Argumentos por valor vs por referencia.

1. **Primitivos (Ej: Numbers, Strings):** Se pasan por **copia**. Si la función cambia el valor, la variable original fuera **NO** cambia.

2. **Objetos (Arrays, Objects):** Se pasa la **referencia (dirección de memoria)**.
Si modificas una propiedad del objeto dentro de la función, **el objeto original fuera de la función muta**.

### Pure Functions (Funciones Puras) 

Aunque no es un concepto único del lenguaje, es importante tener en cuenta la importancia de mantener nustras funciones "puras". Con el fin de hacer que el código sea predecible y fácil de testear (debug).

**¿Qué quiere decir esto?**

Una función que cumpla estrictamente dos reglas:

- Misma entrada = Misma salida: Si le pasas los mismos argumentos (ej. 2 + 2), siempre debe devolver el mismo resultado (4), sin importar cuántas veces o cuándo la llames.

- Sin "Side Effects" (Efectos secundarios): La función no debe modificar nada fuera de su propio cuerpo (variables globales, estado de la aplicación, el DOM, etc.).

```javascript
const msg = 'Hola';

function greeting(name) {
    return `${msg}, ${name}`;
}
```

> Ejemplo de Impureza: Si una función greeting(name) usa una variable externa msg = 'Hola' para crear el saludo, es impura. ¿Por qué? Porque si alguien cambia la variable externa msg a "Adiós", la función devolverá algo diferente para el mismo input nombre.


### First-Class vs Higher-Order Functions

JavaScript trata a las funciones como **First-Class Citizens** (Ciudadanos de primera clase). Significa que las funciones son **valores**, igual que un número `5` o un string `'hola'`.

A. Higher-Order Functions (Funciones de Orden Superior)

Es cualquier función que cumple al menos una de estas dos condiciones:

1. **Recibe** una función como argumento (Callback).
2. **Retorna** una nueva función.

> Concepto callback: Una función que se pasa como argumento a otra función para ser ejecutada ("llamada de vuelta") más tarde dentro de esa función externa.

B. El patrón Callback (Abstracción)


```javascript
// HOF (Recibe la función)
const transformador = function(str, fn) {
    console.log(`Transformando: ${str}`);
    return fn(str); // Ejecuta la función que le pasaron
}

// Callbacks (Herramientas)
const upper = str => str.toUpperCase();
const oneWord = str => str.replace(/ /g, '').toLowerCase();

transformador('JavaScript es genial', upper);
transformador('JavaScript es genial', oneWord);
```

> Por qué lo hacemos: Abstracción. transformador no se preocupa de cómo se transforma el texto, solo delega esa tarea.

### IIFE (Immediately Invoked Function Expressions)

Un patrón antiguo pero que verás en código legacy. Sirve para ejecutar una función una única vez y crear un scope privado (antes de que existieran `let` y `const`).


```javasript

(function() {
    console.log('Esto nunca se ejecutará de nuevo');
    const isPrivate = 23;
})();

```

- **Hoy en día:** Simplemente usamos un bloque `{ const isPrivate = 23; }` para privacidad.

### Recursion

El clásico. Calcular el factorial de un número (ej. `3! = 3 * 2 * 1`).

```javascript
function factorial(n) {
    // 1. Caso Base (El freno de emergencia)
    // Si no ponemos esto, la Call Stack explota (Stack Overflow)
    if (n === 0) {
        return 1;
    }

    // 2. Llamada Recursiva
    // Se pausa este contexto y se crea uno nuevo encima
    return n * factorial(n - 1);
}

console.log(factorial(3)); // Resultado: 6
```

¿Qué pasa en el Motor (Call Stack)?

Imagina la pila de llamadas creciendo y luego bajando.

Fase de Apilado (Ejecución):

1. GEC: Llama a factorial(3).

2. FEC factorial(3): n=3. ¿Es 0? No. Retorna 3 * ¿...?.

    Pausa y llama a factorial(2).

3. FEC factorial(2): n=2. ¿Es 0? No. Retorna 2 * ¿...?.

    Pausa y llama a factorial(1).

4. FEC factorial(1): n=1. ¿Es 0? No. Retorna 1 * ¿...?.

    Pausa y llama a factorial(0).

5. FEC factorial(0): n=0. ¡CASO BASE! Retorna 1.
    
**Fase de Desapilado (Resolución):**

* factorial(0) retorna 1 y sale del stack.

* factorial(1) recibe el 1. Calcula 1 * 1 = 1. Sale del stack.

* factorial(2) recibe el 1. Calcula 2 * 1 = 2. Sale del stack.

* factorial(3) recibe el 2. Calcula 3 * 2 = 6. Sale del stack.

* GEC recibe 6 e imprime.

> El significado de GEC Y FEC se explica con detalle en los fundamentos teóricos

## Objetos 

A diferencia de los datos "primitivos" (que solo guardan un valor único), los objetos permiten almacenar colecciones de datos y entidades más complejas. Son fundamentales en JavaScript.

Conceptos Básicos
---
Imagina un objeto como un gabinete de archivos:

Cada dato se guarda en un "archivo" identificado por una clave (nombre).

Dentro del archivo está el valor.

Creación: Existen dos sintaxis, aunque la de "objeto literal" es la más común:

```javascript
let user = new Object(); // Constructor de objetos (menos común)
let user = {};           // Objeto literal (estándar)
```

Estructura: Se definen mediante pares clave: valor.


```javascript
let user = {
  name: "John", // Clave: "name", Valor: "John"
  age: 30       // Clave: "age", Valor: 30
};
```
Claves: Son siempre strings o símbolos. Si usas otro tipo, se convierte automáticamente a string.

Valores: Pueden ser cualquier tipo de dato (boolean, número, otro objeto, etc.).

### Dot Notation vs. Bracket Notation

Se puede acceder a las propiedades de los objetos de dos formas:

1. **Dot Notation (`objeto.propiedad`):**
    - Más limpio y fácil de leer.
    - Solo funciona si conoces el nombre exacto de la propiedad al escribir el código.
2. **Bracket Notation (`objeto['propiedad']`):**
    - Permite usar **expresiones**. Puedes poner una variable dentro de los corchetes.
    - *Ejemplo:* `objeto['first' + namePart]`

#### Posibles Errores

```javascript
const jonas = {
    firstName: 'Jonas',
    job: 'Teacher',
    age: 30
};
```

// Bucle clásico para objetos 
```javascript
for (const key in jonas) {
    // 'key' es una variable que va cambiando: 'firstName', 'job', 'age'...
    
    console.log(jonas.key);   // ❌ ERROR (Undefined)
    console.log(jonas[key]);  // ✅ CORRECTO
}
```

El problema es cómo el motor de JavaScript "lee" (parsea) tu código.

A. Dot Notation (`jonas.key`) = Literal

Cuando el motor ve un punto, **apaga su cerebro evaluador**. Interpreta lo que viene después como un **String Literal**.

- Instrucción: *"Busca dentro del objeto `jonas` una propiedad que se llame, LITERALMENTE, 'key'"*.
- Resultado: `undefined` (porque `jonas` no tiene ninguna propiedad llamada "key", tiene "firstName", "job", etc.).

B. Bracket Notation (`jonas[key]`) = Expresión

Cuando el motor ve corchetes, sabe que tiene que **evaluar una expresión** primero.

1. Instrucción: *"¿Qué hay dentro de los corchetes? Ah, una variable llamada `key`"*.
2. Evaluación: *"¿Cuánto vale `key` ahora mismo? Vale 'firstName'"*.
3. Ejecución final: *"Busca dentro de `jonas` la propiedad 'firstName'"*.
4. Resultado: `'Jonas'` (Correcto).

## Arrays

Los objetos sirven para almacenar colecciones con claves, pero a menudo necesitamos una colección ordenada (una lista), donde tenemos un primero, un segundo, un tercero, etc. Para eso existen los Arrays.

```javascript
// Dos sintaxis para crear un array vacío
let arr = new Array();
let arr = []; // ✅ La más común

let fruits = ["Manzana", "Naranja", "Ciruela"];

// Acceso por índice (empieza en 0)
alert( fruits[0] ); // Manzana

// Reemplazar o agregar
fruits[2] = "Pera"; // Reemplaza Ciruela
fruits[3] = "Limón"; // Agrega nuevo
```
> Nota: Un array puede almacenar elementos de cualquier tipo: strings, números, booleanos, objetos e incluso otros arrays o funciones, todo mezclado en la misma lista.

### Arrays Multidimensionales (Matrices)

Los arrays pueden contener otros arrays.


```javascript
let matrix = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
];

alert( matrix[1][1] );
```

## Destructuring

Es la forma de "desempaquetar" valores de Arrays u Objetos en variables distintas.

- Arrays

```javascript
const arr = [2, 3, 4];
const [x, y, z] = arr; // x=2, y=3, z=4

// Saltar elementos
const [first, , third] = arr; // first=2, third=4`

// Intercambio de variables
let jugador1 = "Mando";
let jugador2 = "Teclado";

// Intercambiamos los valores en una sola línea
[jugador1, jugador2] = [jugador2, jugador1];

console.log(jugador1); // "Teclado"
console.log(jugador2); // "Mando"
```

- Objetos

```javascript
const usuario = {
  id: 42,
  nombre: "Elena",
  email: "elena@code.com",
  pais: "España",
  premium: true
};

// --- SIN Desestructuración (La forma antigua y repetitiva) ---
const nombre = usuario.nombre;
const email = usuario.email;
const premium = usuario.premium;

// --- CON Desestructuración (La forma moderna) ---
// Le decimos a JS: "Del objeto usuario, sácame las propiedades nombre, email y premium"
const { nombre, email, premium } = usuario;
```

### Valores por Defecto

**El problema:** Imagina que esperas recibir 3 datos de una API, pero solo llegan 2.
En JS normal, la tercera variable sería `undefined`. Eso puede romper cálculos matemáticos (`undefined + 5 = NaN`).

**La Solución:** Puedes asignar un valor de respaldo ("fallback") en la propia declaración.


```javascript
// Simulamos una respuesta de API incompleta
const apiResponse = [10, 20];

// Sin defaults: r sería undefined
// Con defaults:
const [p, q, r = 1] = apiResponse;

console.log(p, q, r); 
// Output: 10, 20, 1 (¡Salvado!)`
```

> Esto es vital cuando lees datos externos que no controlas. Siempre asume que el dato puede no venir.

**En el caso de los objetos**

Sintaxis:

propiedadOriginal: nuevoNombreVariable = valorPorDefecto

```javascript
const restaurant = {
  name: 'Pizza Hut',
  // menu: no existe en este objeto
};

// Extracción clásica vs Desestructuración
const {
  name: restaurantName,  // 1. Toman 'name', lo guardan en 'restaurantName'
  menu: restaurantMenu = [] // 2. 'menu' no existe, así que usa []
} = restaurant;

console.log(restaurantName); // 'Pizza Hut'
console.log(restaurantMenu); // [] (Array vacío, ¡salvados de undefined!)
```

### Destructuring Anidado

A veces recibes un array dentro de otro array. Para sacarlo, tienes que **mimetizar** la estructura original.


```javascript
const nested = [2, 4, [5, 6]];

// Queremos el 2 (primer nivel) y el 6 (segundo nivel)
// Fíjate cómo "dibujamos" la estructura dentro del const
const [i, , [, k]] = nested; 

console.log(i, k); // 2, 6`
```

- **Análisis:**
    - `i` toma el `2`.
    - El primer espacio vacío `,` salta el `4`.
    - `[, k]` entra al array hijo `[5, 6]`.
    - La coma dentro del corchete salta el `5`.
    - `k` toma el `6`.


#### Desestructuración Anidada en Objetos

Los objetos JSON suelen tener muchas capas de profundidad. Para sacar un dato profundo, debes imitar la estructura del objeto dentro de las llaves de desestructuración.


```javascript
const restaurant = {
  openingHours: {
    fri: { open: 11, close: 23 }
  }
};

// Queremos sacar 'open' y 'close' directamente
const {
  openingHours: {       // Entramos al primer nivel
    fri: {              // Entramos al segundo nivel
       open, close      // Extraemos las variables finales
    }
  }
} = restaurant;

console.log(open, close); // 11, 23
```
> Ojo: En este ejemplo, las variables openingHours y fri no se crean. Solo sirven de "camino" para llegar a open y close.

### Mutación de Variables (La Trampa de los Paréntesis)

A veces ya tienes variables creadas y quieres actualizar su valor con datos de un objeto. Aquí JavaScript se confunde.

El problema: Si una línea empieza con {, JavaScript piensa que es un bloque de código (como un if o un bucle), no un objeto.

La solución: Envolver toda la expresión en paréntesis ().


```javascript
let a = 111;
let b = 999;
const obj = { a: 23, b: 7, c: 14 };

// { a, b } = obj;  <-- ❌ Error de sintaxis

({ a, b } = obj); // <-- ✅ Correcto: Los paréntesis indican que es una expresión

console.log(a, b); // 23, 7
```

## Los Operadores `...` (Spread y Rest)

El mismo símbolo `...` tiene **dos efectos totalmente opuestos** dependiendo de dónde lo coloques respecto al signo igual (`=`).

| Símbolo | Posición respecto a `=` | Nombre | Acción | Analogía |
|-------|-------------------------|--------|--------|----------|
| `...` | A la **DERECHA** del `=` | **Spread** | Desempaquetar | Volcar una caja de juguetes al suelo |
| `...` | A la **IZQUIERDA** del `=` | **Rest** | Empaquetar | Recoger los juguetes del suelo y meterlos en una bolsa |

### Spread Operator

Posición: A la derecha (= ...) o en argumentos de función. Misión: Sacar elementos de una colección (Array/String/Objeto) y ponerlos sueltos en un nuevo contexto.

- A. En Arrays (Inmutabilidad y Copia)

    En algunos frameworks modernos puede ser de mucha utilidad conocer está técnica.

    ```javascript
    const menu = ['Pizza', 'Pasta'];

    // ❌ Muta el original
    // menu.push('Risotto');

    // ✅ Crea uno nuevo
    const newMenu = [...menu, 'Risotto']; 
    // Resultado: ['Pizza', 'Pasta', 'Risotto']
    ```

    > Advertencia (Shallow Copy): El Spread hace una copia superficial. Si tu array tiene otros arrays dentro ([[1], [2]]), solo se copian las referencias de los hijos. Si modificas el hijo en la copia, cambias el original.

- B. En Funciones (Pasar argumentos)

    Si tienes los ingredientes en una lista pero la función los pide por separado.

    ```javascript
    const ingredients = ['Setas', 'Queso'];
    const cocinar = (ing1, ing2) => console.log(`Cocinado con ${ing1} y ${ing2}`);

    // El spread "saca" los items del array y los pasa como argumentos sueltos
    cocinar(...ingredients);
    ```
- C. En Objetos (El uso más potente) 🌟

    Fundamental para actualizar propiedades sin mutar el objeto original.

    El Patrón "Override" (Sobrescritura): El orden importa. Las propiedades que pongas después del ...spread sobrescriben a las anteriores.

    ```javascript

    const tarea = { id: 1, texto: 'Estudiar JS', completado: false };

    // Queremos cambiar 'completado' a true, manteniendo el resto igual
    const tareaActualizada = {
        ...tarea,           // 1. Copia todo (id, texto, completado: false)
        completado: true    // 2. Sobrescribe 'completado'
    };

    // ❌ Si lo haces al revés ({ completado: true, ...tarea }), gana el valor antiguo.
    ```

### Rest Pattern

Posición: A la izquierda (const ... =) o recibiendo argumentos en función. Misión: Agrupar los elementos "sobrantes" en un array o un objeto nuevo.

- A. En Desestructuración de Arrays

    Recoge el "resto" de elementos que no has sacado explícitamente.

    ```javascript
    const [primero, segundo, ...resto] = [10, 20, 30, 40, 50];

    console.log(primero); // 10
    console.log(resto);   // [30, 40, 50] (Array con lo que sobró)
    ```

- B. En Objetos: El "Borrado Inmutable" 🌟

    Cómo eliminar una propiedad sin usar delete (que es lento y muta).

    ```javascript
    const user = {
        password: "12345_secret",
        name: "Juan",
        email: "juan@mail.com"
    };

    // Sacamos 'password' a una variable aparte, y guardamos TODO LO DEMÁS en 'userSeguro'
    const { password, ...userSeguro } = user;

    console.log(userSeguro); 
    // { name: "Juan", email: "juan@mail.com" } -> ¡Objeto limpio!
    ```
- C. En Funciones (Argumentos infinitos)

    Permite que una función reciba cualquier cantidad de parámetros.

    ```javascript

    // Recibe todos los argumentos y los mete en un array real llamado 'numeros'
    const sumarTodo = function(...numeros) {
        let total = 0;
        for (let n of numeros) total += n;
        return total;
    };
    sumarTodo(1, 2, 5, 10); // Funciona con cualquier cantidad
    ```

    > Regla de Oro: El Rest element (...resto) siempre debe ser el último en la lista de argumentos o desestructuración. JS necesita saber que ahí es donde "termina" y recoge todo lo que sobra.

---

Diferencia Técnica Crítica

Arrays: Son iterables. Puedes usar ... donde quieras.

Objetos: NO son iterables.

✅ { ...obj }: Sí puedes esparcir un objeto dentro de otro objeto.

❌ [ ...obj ]: No puedes esparcir un objeto dentro de un array (Error: object is not iterable).