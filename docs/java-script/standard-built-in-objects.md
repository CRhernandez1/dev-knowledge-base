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



