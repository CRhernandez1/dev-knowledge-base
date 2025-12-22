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

Los backticks son comillas de interpolación. Nos permiten inyectar variables y expresiones en una cadena de caracteres encerrándolas en ${...}, por ejemplo:

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



