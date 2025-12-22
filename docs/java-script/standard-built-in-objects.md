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

## Métodos de String

JavaScript proporciona una gran cantidad de métodos para manipular texto.

> ⚠️ **Inmutabilidad:**
> Recuerda que los Strings en JS son **inmutables**. Ninguno de estos métodos modifica la variable original; todos devuelven un **nuevo string** con los cambios aplicados.


Leyenda de Frecuencia

- 🟢 **Esencial:** De uso diario.
- 🟡 **Ocasional:** Para casos específicos.
- 🔴 **Raro / Legacy:** Existen formas más modernas o mejores de hacerlo.


### Búsqueda y Comprobación
Responden preguntas sobre el contenido. Devuelven `boolean` (true/false) o índices numéricos.

| Uso | Método | Descripción | Ejemplo |
| :---: | :--- | :--- | :--- |
| 🟢 | **`.includes(txt)`** | ¿Contiene el texto? | `"Hola".includes("o")` // `true` |
| 🟢 | **`.startsWith(txt)`** | ¿Empieza por...? | `"img.jpg".startsWith("img")` |
| 🟢 | **`.endsWith(txt)`** | ¿Termina en...? | `"file.js".endsWith(".js")` |
| 🟡 | **`.indexOf(txt)`** | Posición donde empieza (o `-1`). | `"Hola".indexOf("a")` // `3` |
| 🟡 | **`.lastIndexOf(txt)`** | Igual, pero busca desde el final. | `"anana".lastIndexOf("a")` // `4` |
| 🔴 | **`.search(regex)`** | Busca con Regex. Suele usarse `.match`. | `"Hola".search(/H/)` |

### Extracción y Corte
Para obtener fragmentos del texto o convertirlo en Arrays.

| Uso | Método | Descripción | Ejemplo |
| :---: | :--- | :--- | :--- |
| 🟢 | **`.slice(ini, fin)`** | Corta un trozo. Admite negativos. | `"React".slice(0, 2)` // `"Re"` |
| 🟢 | **`.split(sep)`** | Divide el texto en un **Array**. | `"a-b".split("-")` // `['a','b']` |
| 🔴 | **`.substring(ini, fin)`** | Como slice, pero sin negativos. | *Usa `.slice()` mejor.* |
| ⛔ | **`.substr()`** | **Obsoleto**. No usar. | -- |

### Modificación
Transforman el texto (retornando uno nuevo).

| Uso | Método | Descripción | Ejemplo |
| :---: | :--- | :--- | :--- |
| 🟢 | **`.replace(a, b)`** | Reemplaza la **primera** coincidencia. | `"x x".replace("x", "y")` // `"y x"` |
| 🟢 | **`.replaceAll(a, b)`** | Reemplaza **todas** las coincidencias. | `"x x".replaceAll("x", "y")` // `"y y"` |
| 🟢 | **`.toUpperCase()`** | Convierte a MAYÚSCULAS. | `"a".toUpperCase()` // `"A"` |
| 🟢 | **`.toLowerCase()`** | Convierte a minúsculas. | `"B".toLowerCase()` // `"b"` |
| 🟢 | **`.trim()`** | Quita espacios a los lados. | `" x ".trim()` // `"x"` |
| 🟡 | **`.trimStart()`** | Quita espacios solo del inicio. | `" x".trimStart()` |
| 🟡 | **`.repeat(n)`** | Repite el string `n` veces. | `"Ha".repeat(3)` // `"HaHaHa"` |
| 🔴 | **`.concat(str)`** | Une textos. | *Usa el operador `+` o Template Strings.* |

### Caracteres y Relleno
Manipulación visual o acceso a caracteres.

| Uso | Método | Descripción | Ejemplo |
| :---: | :--- | :--- | :--- |
| 🟡 | **`.charAt(i)`** | Letra en la posición `i`. | `"Hola".charAt(0)` // `"H"` |
| 🟡 | **`.padStart(len, c)`** | Rellena al inicio. | `"5".padStart(2,"0")` // `"05"` |
| 🟡 | **`.padEnd(len, c)`** | Rellena al final. | `"Hi".padEnd(4,".")` // `"Hi.."` |
| 🔴 | **`.charCodeAt(i)`** | Devuelve el código Unicode. | `"A".charCodeAt(0)` // `65` |

---

### Buenas Prácticas Modernas

### Slice vs Substring
Usa siempre **`.slice()`**. Es más versátil porque permite índices negativos para contar desde el final (ej: "los últimos 2 caracteres").

```javascript
const text = "JavaScript";
console.log(text.slice(-6)); // "Script"
// .substring(-6) no funcionaría
```

