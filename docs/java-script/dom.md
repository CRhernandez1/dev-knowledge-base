# Introducción a la Manipulación del DOM

Una vez comprendida la sintaxis del lenguaje JavaScript, el siguiente paso lógico es aprender a interactuar con la interfaz de usuario. En el contexto del navegador, el objeto `document` juega el papel central.

Dado que es imposible abarcarlo todo, algunas de las siguientes fuentes pueden ser útiles:

* **MDN (Mozilla Developer Network):** La referencia estándar del día a día. Ideal para consultas rápidas de sintaxis, métodos y ejemplos prácticos (`MDN localstorage`).
* **The Modern JavaScript Tutorial (javascript.info):** Un gran recurso para entender los conceptos profundos y la teoría detrás del código de forma clara.
* **Especificaciones (WHATWG):** La fuente de la verdad técnica. Es denso y complejo, pero ofrece el conocimiento más sólido sobre cómo funciona el navegador internamente (`WHATWG localstorage`).


---

## Document

JavaScript nació para los navegadores, pero hoy se ejecuta en múltiples plataformas (servidores, electrodomésticos, etc.). A este contexto donde corre el código se le llama **Entorno de Host**.

**El Objeto `window` (La Raíz)**

En el navegador, tenemos un objeto raíz llamado `window` que cumple **dos roles** simultáneos:

1.  **Objeto Global:** Contiene todas las variables y funciones globales de JavaScript (como se vio en la teoría del lenguaje).
2.  **Interfaz del Navegador:** Representa la ventana física del navegador y ofrece métodos para controlarla.

```javascript
// Rol 1: Objeto Global
function sayHi() { alert("Hola"); }
window.sayHi(); // Las funciones globales son métodos de window

// Rol 2: Ventana del Navegador
alert(window.innerHeight); // Muestra la altura de la ventana
```
**Los 3 Pilares del Entorno Navegador**

Cuando JS se ejecuta en una web, tenemos acceso a tres grandes modelos:

**DOM (Document Object Model)**

Representa todo el contenido de la página web como objetos modificables.

* **Punto de entrada:** El objeto `document`.
* **Función:** Crear, leer o modificar cualquier cosa en la página (HTML).
* **Universalidad:** No es exclusivo de navegadores; herramientas de servidor que procesan HTML también usan DOM.

```javascript
// Ejemplo: Cambiar el fondo a rojo y quitarlo tras 1 segundo
document.body.style.background = "red";
setTimeout(() => document.body.style.background = "", 1000);
```

**BOM (Browser Object Model)**

Son objetos adicionales que proporciona el entorno (host) para interactuar con todo lo que **NO** es el documento (la página en sí).

* **`navigator`:** Información del navegador y S.O.
    * `navigator.userAgent`: Info del navegador.
    * `navigator.platform`: Info del sistema (Win/Mac/Linux).
* **`location`:** Gestión de la URL.
    * `location.href`: Leer URL actual o redirigir.
* **Funciones de Interfaz:** `alert`, `confirm`, `prompt`.

```javascript
// Ejemplo de redirección con BOM
if (confirm("¿Ir a Wikipedia?")) {
    location.href = "[https://wikipedia.org](https://wikipedia.org)";
}
```

**CSSOM (CSS Object Model)**

* Especificación separada para manipular reglas y hojas de estilo CSS como objetos.

* Se usa junto al DOM.

* Rara vez modificamos reglas CSS puras desde JS (es complejo y poco común). Generalmente, manipulamos clases CSS a través del DOM.

### El Árbol del DOM (Document Object Model)

El DOM es la representación del documento HTML como una estructura de objetos manipulables por JavaScript.

* **Principio básico:** Cada etiqueta HTML es un objeto.
* **Jerarquía:** Las etiquetas anidadas son "hijas" de la etiqueta contenedora.
* **Texto:** El texto dentro de una etiqueta también es un objeto.



**Estructura de Árbol**

El DOM representa el HTML como una estructura jerárquica (un árbol).

* **Nodos de Elemento (Element Nodes):** Son las etiquetas HTML (`<body>`, `<a>`, `<div>`). Forman la estructura del árbol.
* **Nodos de Texto (Text Nodes):** Contienen únicamente cadenas de texto. Son siempre las "hojas" del árbol (no pueden tener hijos). Se etiquetan como `#text`.

Como todo es un objeto, podemos acceder a propiedades como `style`, `innerHTML`, `offsetWidth`, etc.

```javascript
// document.body es el objeto que representa la etiqueta <body>
document.body.style.background = 'red'; 

setTimeout(() => {
    document.body.style.background = ''; // Vuelve al original
}, 3000);
```

**El tratamiento de los Espacios en Blanco**

Una de las características más importantes (y confusas) del DOM es que **los espacios y saltos de línea son nodos válidos**.

* **Regla:** Si hay espacios o nuevas líneas (`\n`) en el código HTML, se convierten en **nodos de texto** en el DOM.

**Excepciones:**

1.  **Espacios antes de `<head>`:** Se ignoran por razones históricas.

2.  **Cualquier contenido después de `</body>`:**

    * Ya sean espacios, texto o etiquetas HTML, el navegador los moverá automáticamente **dentro** del `body`, al final.

    * *Razón:* La especificación HTML requiere que todo el contenido visible esté estrictamente dentro del cuerpo del documento.

> **Nota:** Las herramientas de desarrollador suelen ocultar estos nodos de texto vacíos para limpiar la visualización, pero **existen** y afectan cuando navegamos por el DOM con JS.

**Autocorrección del Navegador**

El navegador intenta arreglar el HTML malformado al generar el DOM.

* **Etiquetas faltantes:** Si el HTML es solo la palabra "Hola", el navegador envolverá automáticamente eso en `<html>` y `<body>`.
* **Cierre de etiquetas:** Si olvidas cerrar etiquetas (ej. `<li>`), el navegador las cierra en el DOM.

El caso especial de las Tablas (`<tbody>`)
Este es un punto crítico. Según la especificación DOM, una tabla **siempre** debe tener un `<tbody>`.

* Si escribes tu HTML **sin** `<tbody>`, El navegador creará automáticamente el `tbody` en el DOM.

**Tipos de Nodos**

Aunque existen 12 tipos de nodos en la especificación, en la práctica trabajamos principalmente con 4:

1.  **Document:** El "punto de entrada" al DOM.
2.  **Element Nodes:** Las etiquetas HTML (los bloques de construcción).
3.  **Text Nodes:** El contenido de texto.
4.  **Comments:** Los comentarios HTML (``).

> **Regla:** Si está en el HTML, está en el DOM. Los comentarios se convierten en nodos `#comment`. JavaScript puede leerlos, aunque no sean visibles en la página.

### Recorriendo el DOM (DOM Traversing)

Una vez tenemos el objeto `document`, podemos movernos por él para seleccionar cualquier parte de la página. Imagina el DOM como un árbol genealógico donde los elementos tienen padres, hijos y hermanos.

**Puntos de acceso principales**

Los nodos más importantes están disponibles directamente desde la raíz `document`:

* **`<html>`:** `document.documentElement`
* **`<head>`:** `document.head`
* **`<body>`:** `document.body`

> **⚠️ Cuidado: `document.body` puede ser `null`**
> Un script no puede acceder a un elemento que aún no ha sido "leído" por el navegador.
> * Si pones un `<script>` dentro del `<head>` e intentas acceder a `document.body`, te dará `null` porque el body aún no existe.

---

**Nodos vs. Elementos (La forma "sucia" vs "limpia")**

Al navegar por el DOM, existen dos grupos de propiedades.

1.  **Propiedades de Nodo (Ej. `childNodes`):** Ven **TODO**. Etiquetas, comentarios, y sobre todo, **saltos de línea y espacios** (nodos de texto).

2.  **Propiedades de Elemento (Ej. `children`):** Ven **SOLO ETIQUETAS**. Ignoran texto y comentarios.

🔥 **En el 99% de los casos, usarás la navegación por Elementos.** Queremos manipular la estructura (`div`, `ul`, `li`), no los espacios en blanco invisibles.

---

**Navegación Descendente (Hacia los hijos)**

**A. La forma recomendada (Solo Elementos)**

* **`elem.children`:** Devuelve una colección solo con los hijos que son etiquetas HTML.
* **`elem.firstElementChild`:** El primer hijo (etiqueta).
* **`elem.lastElementChild`:** El último hijo (etiqueta).

**B. La forma "cruda" (Todos los Nodos)**

* `elem.childNodes`: Colección de todos los hijos (incluye textos y espacios).
* `elem.firstChild` / `elem.lastChild`: Primer y último nodo (a menudo será un espacio en blanco `#text`).

```javascript
const list = document.querySelector('ul');

// ✅ Forma Correcta
console.log(list.children); // HTMLCollection [li, li, li]

// ❌ Forma Ruidosa
console.log(list.childNodes); // NodeList [text, li, text, li, text...]
```

**Navegación Lateral (Hermanos/Siblings)**

Los **hermanos** son nodos que comparten el mismo padre (por ejemplo, varios `<li>` dentro de un `<ul>`).

* **`elem.nextElementSibling`:** Devuelve el siguiente hermano que sea una **etiqueta**.
* **`elem.previousElementSibling`:** Devuelve el hermano anterior que sea una **etiqueta**.

**La forma "cruda"**

* `elem.nextSibling` / `elem.previousSibling`: Acceden al siguiente nodo inmediato, sea lo que sea.
* **El Problema:** A menudo devuelven **nodos de texto vacíos** (saltos de línea o espacios invisibles en el código) en lugar del elemento HTML que esperas encontrar.

**Navegación Ascendente (Hacia el padre)**


* **`elem.parentElement`:** Devuelve el elemento padre. (**Es lo que usarás casi siempre**).
* **`elem.parentNode`:** Devuelve el nodo padre.

**¿Cuál es la diferencia?**

Normalmente devuelven exactamente lo mismo. La única excepción importante ocurre en la raíz del documento (`<html>`):

1.  **`document.documentElement.parentElement`** -> `null`

    * *Razón:* El padre de `<html>` es `document`. Como `document` **no es una etiqueta HTML** (no es un elemento), esta propiedad devuelve `null`.

2.  **`document.documentElement.parentNode`** -> `document`

    * *Razón:* Devuelve el padre sea del tipo que sea.

**Características de las Colecciones del DOM**

Cuando usamos `children` o `childNodes`, lo que obtenemos **NO es un Array**, es una **Colección** (un objeto especial iterable).

1.  **Son Iterables:** Podemos usar `for..of` para recorrerlas.
2.  **Son de Solo Lectura:** No podemos modificar el DOM haciendo `children[0] = ...`. Hay que usar métodos específicos de inserción/borrado para cambiar la estructura.
3.  **Son "Vivas" (Live):** Reflejan el estado actual en tiempo real. Si modificas el DOM (ej. añades un `div`), la variable que contiene la colección se actualiza automáticamente. No necesitas volver a capturarla.
4.  **No tienen métodos de Array:** No funcionan `.map`, `.filter`, `.push`, etc.
    * **Solución:** Usar `Array.from(elem.children)` para convertirlo en un array real si necesitas esos métodos.

```javascript
for (let node of document.body.children) {
    console.log(node); // ✅ Itera correctamente
}

// document.body.children.filter(...) // ❌ Error! filter no es una función
```

**Atajos Especiales: Tablas**

Ciertos elementos complejos como las tablas tienen una API especial para evitar tener que navegar por docenas de `div`s o `tr`s manuales.



**Si tienes un elemento `<table>`:**

* **`table.rows`:** Colección de `<tr>` (filas).

* **`table.tBodies`:** Colección de `<tbody>`.

* **`table.tFoot` / `table.tHead`:** Referencias directas al pie y cabecera.

**Si tienes un elemento `<tr>` (Fila):**

* **`tr.cells`:** Colección de `<td>` y `<th>` (celdas).

* **`tr.rowIndex`:** Número de índice de la fila.

```javascript
// Cambiar color de la primera celda de la primera fila
table.rows[0].cells[0].style.background = 'red';
```

Aquí tienes los apuntes sobre Búsqueda en el DOM, organizados por métodos modernos vs. antiguos, e incluyendo la tabla comparativa técnica que pediste al final.

Markdown

### Búsqueda de Elementos en el DOM 

Cuando los elementos no están cerca (no son padre/hijo directo), necesitamos métodos de búsqueda para localizarlos en cualquier parte de la página.

**1. Búsqueda por ID (La más rápida)**

* **Método:** `document.getElementById('id')`
* **Contexto:** Solo funciona en `document`.
* **Reglas:**
    * El `id` debe ser **único** en toda la página.
    * Si hay duplicados, el comportamiento es impredecible.

> **⚠️ Cuidado con las variables globales automáticas**
> Los navegadores crean variables globales para cada `id` (ej. `window['mi-id']`). **No las uses**. Es una mala práctica por compatibilidad antigua que causa conflictos. Usa siempre `getElementById`.

---

**Búsqueda por Selectores CSS (La estándar moderna)**

Estos son los métodos más versátiles y usados hoy en día. Permiten usar selectores como `.clase`, `#id`, `ul > li`, `:hover`, etc.

`elem.querySelectorAll(css)`

* Devuelve **todos** los elementos que coincidan.
* Devuelve una colección **estática** (no se actualiza sola).
* Permite pseudoclases.

```javascript
let elements = document.querySelectorAll('ul > li:last-child');
for (let elem of elements) {
    alert(elem.innerHTML); 
}
```

`elem.querySelector(css)`

* Devuelve solo el primer elemento que coincida.

* Es más eficiente que querySelectorAll si solo buscas uno.

* Equivale a querySelectorAll(css)[0].

**3. Comprobación y Ancestros (`matches`, `closest`)**

A veces no queremos buscar *dentro* (hacia abajo), sino comprobar el elemento actual o mirar hacia *arriba* (hacia los padres).

* **`elem.matches(css)`:**

    * Devuelve `true/false`.

    * Comprueba si el elemento actual cumple con el selector CSS.

    * Es muy útil para filtrar elementos cuando iteramos sobre una lista.

* **`elem.closest(css)`:**

    * Busca el **ancestro** (padre, abuelo...) más cercano que cumpla el selector.

    * **Importante:** Sube por la cadena de padres hasta encontrarlo o llegar a `null`.

    * **Se incluye a sí mismo** en la búsqueda (si el propio elemento cumple, se devuelve a sí mismo).

* **`elemA.contains(elemB)`:**

    * Devuelve `true` si `elemB` está **dentro** de `elemA` (es su descendiente) o si son el mismo elemento.

**Métodos "Legacy" (Antiguos pero vivos)**

Estos métodos son historia, pero es importante conocerlos. A diferencia de `query...`, estos devuelven **Colecciones Vivas**.

* **`elem.getElementsByTagName(tag)`:** Busca por etiqueta (`div`, `a`, `*`).
* **`elem.getElementsByClassName(className)`:** Busca por clase.
* **`document.getElementsByName(name)`:** Busca por atributo `name` (en todo el documento).


**Colecciones Vivas vs Estáticas**

Esta es la diferencia técnica más importante:

* **Colección Viva (`getElementsBy*`):** Se "auto-actualiza". Si creas un nuevo `div` con JS, la variable que contenía los divs aumenta su longitud automáticamente.

* **Colección Estática (`querySelectorAll`):** Es una "foto fija" del momento en que buscaste. Si el DOM cambia después, esta colección no se entera.

**Resumen de Métodos**

| Método | Busca por... | ¿Puede llamarse en un elemento? | ¿Es colección Viva? |
| :--- | :--- | :--- | :--- |
| `querySelector` | Selector CSS | ✔ | - |
| `querySelectorAll` | Selector CSS | ✔ | ❌ (Estática) |
| `getElementById` | ID | ❌ (Solo document) | - |
| `getElementsByName` | Name | ❌ (Solo document) | ✔ (Viva) |
| `getElementsByTagName` | Etiqueta | ✔ | ✔ (Viva) |
| `getElementsByClassName`| Clase | ✔ | ✔ (Viva) |


| Característica | NodeList | HTMLCollection |
|---------------|----------|----------------|
| ¿Quién lo devuelve? | `querySelectorAll()` (estática)<br>`childNodes` (viva) | `getElementsByTagName()`<br>`getElementsByClassName()`<br>`children` |
| ¿Es una colección viva? | Depende del método | **Sí (siempre)** |
| ¿Qué contiene? | Puede contener cualquier tipo de nodo (elementos, texto, comentarios) | Solo elementos HTML |
| ¿Tiene `.forEach()`? | Sí (navegadores modernos) | No |
| ¿Es un Array real? | No | No |
| Cómo iterar | `for...of`, `.forEach()` | `for...of` |
| Conversión a Array | `Array.from(nodeList)` | `Array.from(htmlCollection)` |

> **Nota importante sobre `NodeList`**
>
> Que una `NodeList` *pueda* contener nodos de texto, comentarios o elementos **no significa** que siempre los contenga.
>
> `NodeList` describe el **tipo de colección**, no el **tipo de contenido**.
>
> El contenido depende del método que la genera:
>
> - `querySelector()` / `querySelectorAll()` → devuelven una `NodeList` que contiene **solo elementos HTML**.
> - `childNodes` → devuelve una `NodeList` que incluye **todos los nodos**: texto (espacios y saltos de línea), comentarios y elementos.
>
> Por eso, es correcto decir que una `NodeList` *puede* contener cualquier tipo de nodo, aunque en la práctica, el método más usado (`querySelectorAll`) solo devuelva elementos.

### Propiedades del Nodo: Tipo, Etiqueta y Contenido

Una vez seleccionado un elemento del DOM, necesitamos leer o modificar su información. Para ello, debemos entender qué tipo de objeto es y qué propiedades hereda.

**Jerarquía de Clases del DOM**

Cada nodo en el DOM es un objeto que pertenece a una clase. Estas clases forman una jerarquía de herencia, lo que significa que un elemento específico (ej. `<input>`) tiene sus propiedades propias más las de todos sus padres.



1.  **`EventTarget` (Raíz):** Clase abstracta base. Permite que los nodos soporten eventos (clicks, teclas, etc.).
2.  **`Node`:** Base de todos los nodos del DOM. Proporciona navegación (`parentNode`, `nextSibling`, `childNodes`).
3.  **`Element`:** Base de las etiquetas HTML/XML. Proporciona navegación por elementos (`children`, `nextElementSibling`) y búsquedas (`querySelector`).
4.  **`HTMLElement`:** Base de todos los elementos HTML estándar. Proporciona atributos comunes (`id`, `title`, `hidden`).
5.  **`HTMLInputElement` (y similares):** Clases concretas para cada etiqueta con propiedades específicas (`.value`, `.checked`).

**¿Cómo saber la clase de un nodo?**

Podemos inspeccionarlo en la consola o usar `instanceof`.

```javascript
// Ver el nombre de la clase
alert(document.body.constructor.name); // HTMLBodyElement

// Verificar herencia
alert(document.body instanceof HTMLBodyElement); // true
alert(document.body instanceof Node);            // true
```
**Truco de Consola:**

* console.log(elem): Muestra el elemento como estructura HTML (árbol).

* console.dir(elem): Muestra el elemento como objeto JSON, ideal para explorar todas sus propiedades.

**Propiedades de Tipo y Nombre**

`nodeType`

Propiedad numérica antigua (pero rápida) para saber qué tipo de nodo es.

* **`1`:** Elemento (Etiqueta HTML).

* **`3`:** Nodo de Texto.

* **`9`:** Documento.

**`tagName` vs `nodeName`**

Ambas devuelven el nombre de la etiqueta en mayúsculas (ej. `"DIV"`, `"BODY"`), pero con una diferencia sutil:

* **`tagName`:** Existe solo en **Elementos**.
* **`nodeName`:** Existe en **Cualquier Nodo** (Elementos, Textos, Comentarios).

```javascript
// En un comentario: alert(comentario.tagName);  // undefined (no es un elemento)
alert(comentario.nodeName); // "#comment"
```

**Manipulación de Contenido**

Existen 4 formas principales de leer/escribir el contenido. Elegir la incorrecta puede causar bugs o problemas de seguridad.

**A. `innerHTML` (HTML interno)**

* **Lectura:** Devuelve el contenido HTML dentro del elemento como string.
* **Escritura:** El navegador **parsea** el string y crea los elementos HTML correspondientes.

> **⚠️ El peligro de `innerHTML +=`**
> Nunca uses `elem.innerHTML += "algo"`.
> Aunque parece una simple concatenación, en realidad realiza una **sobrescritura completa**:
> 1.  Borra **todo** el contenido anterior.
> 2.  Vuelve a crear el contenido antiguo + el nuevo desde cero.
>
> **Consecuencias:** Se pierden estados (texto escrito en inputs, enfoque, selecciones) y se recargan imágenes innecesariamente.

---

**B. `outerHTML` (HTML completo)**

Contiene el HTML interno **más** la propia etiqueta envolvente.



> **⚠️ La trampa de escritura:**
> A diferencia de `innerHTML`, escribir en `outerHTML` **NO actualiza la variable JS** que usaste, pero **SÍ actualiza el DOM**.

```javascript
let div = document.querySelector('div'); // <div>Hola</div>

// Reemplazamos el div por un párrafo en el DOM real
div.outerHTML = '<p>Nuevo</p>'; 

// ¡Sorpresa! La variable 'div' sigue teniendo el valor antiguo
alert(div.outerHTML); // <div>Hola</div> 

// Razón: El div original fue "arrancado" del documento.
// La variable 'div' sigue apuntando a ese elemento "fantasma" en memoria, no al nuevo <p>.
```

**C. textContent (Texto seguro)**

Accede solo al texto, **ignorando todas las etiquetas**.

* **Lectura:** Devuelve el texto limpio, sin tags.

* **Escritura (Seguridad):** Inserta el texto de forma literal. Si el texto contiene símbolos como `<b>`, los **escapa** para que se vean como texto y no se ejecuten como código.

* **Uso:** Ideal para insertar datos que vienen de usuarios para prevenir ataques **XSS (Cross-Site Scripting)**.

```javascript
let input = "<b>Hola</b>";

// Opción Peligrosa (interpreta HTML)
elem.innerHTML = input;   // Se vuelve negrita

// Opción Segura (escapa HTML)
elem.textContent = input; // Se lee "<b>Hola</b>" literalmente en la pantalla
```

**D. data / nodeValue**

Se usan para leer contenido de nodos que **no son elementos** (donde `innerHTML` no existe o no tiene sentido).

Principalmente se usa en:

* **Nodos de Texto.**

* **Comentarios** (sí, JS puede leer los comentarios del código).

**Otras Propiedades Útiles**

`hidden`

Propiedad booleana (`true`/`false`).

* Controla la visibilidad del elemento.

* Funciona técnicamente igual que CSS `style="display: none"`, pero es más corto de escribir.

```javascript
elem.hidden = true; // Oculta el elemento
```

**Atributos Específicos**

Dependiendo de la clase específica del elemento (su prototipo), tendremos acceso directo a sus atributos estándar mapeados como propiedades:

* Inputs: input.value, input.checked

* Enlaces: a.href

* Imágenes: img.src

> Nota: La mayoría de atributos HTML estándar tienen su propiedad DOM correspondiente. Por lo que hay más que las mencionadas aquí.

**Tabla Resumen de Propiedades**

| Propiedad | Descripción | ¿Puede escribir HTML? | ¿Qué nodos la tienen? |
| :--- | :--- | :--- | :--- |
| **`innerHTML`** | Contenido interno | ✅ Sí | Elementos |
| **`outerHTML`** | Contenido + Etiqueta propia | ✅ Sí (Reemplaza en DOM) | Elementos |
| **`textContent`** | Solo texto (sin tags) | ❌ No (Lo escapa) | Todos (Node) |
| **`data`** | Contenido de texto | ❌ No | Textos y Comentarios |
| **`nodeValue`** | Igual que data | ❌ No | Textos y Comentarios |
| **`hidden`** | Visibilidad | - | Elementos |

### Atributos HTML y Propiedades DOM

Cuando el navegador carga una página, realiza dos pasos distintos:

1.  **Lee (Parsea) el HTML:** Lee el texto plano que escribiste (`<div id="test">...</div>`).

2.  **Genera el DOM:** Crea objetos en la memoria de JavaScript (`div.id = "test"`).

El problema (y la solución) es que **no siempre coinciden 1 a 1**. Entender la diferencia es vital para evitar bugs.

---

**Propiedades DOM (El Objeto en Memoria)**

Los nodos del DOM son **objetos JavaScript regulares**. Heredan de clases (como vimos en el tema anterior) y podemos modificarlos como cualquier otro objeto.

**Características Clave:**

* **Son Case-Sensitive:** Distinguen mayúsculas/minúsculas (`elem.nodeType` funciona, `elem.NoDeTyPe` no).

* **Son Extensibles:** Puedes inventarte propiedades nuevas o incluso métodos al vuelo.

* **Tienen Tipado Real:** Pueden ser booleanos, objetos, números, etc.

**Ejemplo de personalización:**

Podemos agregar datos arbitrarios a un elemento:
```javascript
document.body.myData = {
    nombre: 'Cesar',
    titulo: 'Emperador'
};
console.log(document.body.myData.titulo); // "Emperador"
```

**Ejemplo de modificación de Prototipos (Avanzado):** Podemos modificar Element.prototype para que todas las etiquetas HTML tengan un nuevo método:

```js

Element.prototype.decirHola = function() {
    alert(`Hola, soy una etiqueta ${this.tagName}`);
};
document.body.decirHola(); // "Hola, soy una etiqueta BODY"
```

**Atributos HTML (El Texto en la Etiqueta)**

Los atributos son lo que está escrito literalmente dentro de las etiquetas en el código fuente HTML (`<input type="text">`).

**Características Clave:**

* **Son Case-Insensitive:** No distinguen mayúsculas (`ID` es igual a `id`).

* **Son SIEMPRE Strings:** No importa qué escribas, para el HTML todo es una cadena de texto.

* **Viven en el HTML:** Se accede a ellos a través de métodos específicos, no con el punto (`.`).

**Métodos de manipulación de Atributos**

Para trabajar con atributos (especialmente los no estándares), usamos estos métodos:

* **`elem.hasAttribute(nombre)`:** Devuelve `true` si existe.

* **`elem.getAttribute(nombre)`:** Devuelve el valor (siempre string).

* **`elem.setAttribute(nombre, valor)`:** Escribe/Modifica el valor.

* **`elem.removeAttribute(nombre)`:** Borra el atributo.

* **`elem.attributes`:** Devuelve una colección iterable de todos los atributos.

```javascript
let body = document.body;
// HTML: <body something="extra">

alert(body.something); // undefined (No se crea propiedad automática porque no es estándar)
alert(body.getAttribute('something')); // "extra" (Acceso correcto al atributo)
```

**La Gran Diferencia de Tipos**

Aquí es donde ocurre la "magia" y la confusión. El navegador intenta convertir el atributo (String HTML) en una propiedad útil (Objeto JS), pero lo hace de formas diferentes según el atributo.


**A. El caso `style`**

* **El Atributo HTML (`getAttribute('style')`):** Es un **STRING**.
    * Es literalmente el texto que escribiste: `"color:red; font-size:12px;"`.

* **La Propiedad DOM (`elem.style`):** Es un **OBJETO**.
    * El navegador "parsea" ese string y crea un objeto `CSSStyleDeclaration` para que puedas manipular los estilos programáticamente (`elem.style.color`).

**Por eso:**

* `div.getAttribute('style')` te da una cadena de texto gigante.
* `div.style` te da un objeto estructurado.

**B. El caso `href` (Enlaces)**

* **El Atributo HTML:** Puede ser una ruta relativa (`#hola` o `page.html`).

* **La Propiedad DOM:** Siempre se convierte a una **URL Absoluta completa**.



```javascript
// HTML: <a id="link" href="#test">
alert(link.getAttribute('href')); // "#test" (Lo que escribiste)
alert(link.href);                 // "[http://misitio.com/pagina#test](http://misitio.com/pagina#test)" (La realidad)
```

**C. El caso checked (Inputs)**

* **El Atributo HTML:** Es un string ("checked" o string vacío).

* **La Propiedad DOM:** Es un BOOLEANO (true/false).

**Sincronización**

Generalmente, atributos y propiedades están vinculados ("espejo"). Si cambias uno, cambia el otro.

* **Regla General:** Atributo Estándar <==> Propiedad DOM.

**La Excepción Crítica: `input.value`**

En los campos de texto, la sincronización es **unidireccional** para proteger el valor original.

1.  **Atributo (`getAttribute('value')`):** Representa el valor **INICIAL** (lo que venía en el HTML).
2.  **Propiedad (`input.value`):** Representa el valor **ACTUAL** (lo que el usuario ha escrito).

* Si cambias el *atributo*, la *propiedad* se actualiza.
* Si el usuario (o JS) cambia la *propiedad*, el *atributo* **NO cambia**.

> **Utilidad:** Esto permite saber qué puso el programador originalmente (`getAttribute`) vs qué escribió el usuario (`value`).

**Atributos Personalizados (`data-*` y `dataset`)**

A veces necesitamos pasar datos desde el HTML a JS (ej. el ID de un usuario, el estado de un pedido).

* **❌ Mala práctica:** Inventar atributos propios (ej. `<div estado="nuevo">`).
    * *Riesgo:* HTML podría estandarizar el atributo "estado" en el futuro y romper tu código.
* **✅ Buena práctica:** Usar atributos que empiecen por **`data-`**.

**La propiedad `dataset`**

Todos los atributos `data-*` se mapean automáticamente a la propiedad `dataset` del objeto DOM.

**Regla de conversión:**
El prefijo `data-` desaparece y los guiones intermedios se eliminan, convirtiendo el texto a **CamelCase**.

* HTML: `data-order-state`
* JS: `dataset.orderState`

```html
<div id="pedido" data-order-state="pendiente" data-user-id="123"></div>

<script>
  // Leer (CamelCase automático)
  console.log(pedido.dataset.orderState); // "pendiente"
  console.log(pedido.dataset.userId);     // "123"

  // Escribir (Actualiza automáticamente el HTML)
  pedido.dataset.orderState = "enviado";
  // Ahora el HTML se actualiza: <div data-order-state="enviado" ...>
</script>
```
**Resumen Comparativo Final**

| Característica | Propiedad DOM (`elem.prop`) | Atributo HTML (`getAttribute`) |
| :--- | :--- | :--- |
| **Naturaleza** | Variable en memoria (JS) | Texto en etiqueta (HTML) |
| **Tipo de dato** | Dinámico (Obj, Bool, String) | **Siempre String** |
| **Mayúsculas** | Sensible (Case-Sensitive) | Insensible (Case-Insensitive) |
| **Atributos Custom** | No se crean automáticamente | Acceso total |
| **Sincronización** | Se sincroniza con Attr (salvo excepciones) | Se sincroniza con Prop |

### Modificando el Documento: Creación y Manipulación

La capacidad de crear, insertar y borrar elementos "al vuelo" es lo que hace que una página sea dinámica.

---

**Creación de Nodos (La Fábrica)**

Antes de poner nada en la página, primero debemos crear el objeto en memoria.

**Métodos de Creación**

* **`document.createElement(tag)`**: Crea un nodo de **elemento**.
    ```javascript
    let div = document.createElement('div');
    ```
* **`document.createTextNode(text)`**: Crea un nodo de **texto** (Raramente usado directamente, ya que los métodos de inserción modernos crean texto automáticamente).
    ```javascript
    let texto = document.createTextNode('Hola');
    ```

> **Nota:** Al crear el elemento, este **solo existe en una variable**. No aparecerá en la página hasta que lo insertes con uno de los métodos siguientes.

---

**2. Métodos de Inserción Modernos**

Estos métodos son versátiles: permiten insertar **nodos** (elementos) o **strings** (texto) en diferentes posiciones relativas a un elemento.



**Los 4 Métodos Principales**

Asumamos que tenemos un elemento `node`.

1.  **`node.append(...nodos/texto)`**: Inserta **dentro**, al **final**.

2.  **`node.prepend(...nodos/texto)`**: Inserta **dentro**, al **principio**.

3.  **`node.before(...nodos/texto)`**: Inserta **fuera**, justo **antes** (como hermano anterior).

4.  **`node.after(...nodos/texto)`**: Inserta **fuera**, justo **después** (como hermano siguiente).

5.  **`node.replaceWith(...nodos/texto)`**: **Reemplaza** el `node` con lo que le pases.

**Características Clave**

* **Múltiples argumentos:** Puedes insertar varias cosas a la vez: `div.append(elem1, elem2, "texto")`.

* **Seguridad (Strings):** Si insertas un string (ej. `append('<p>Hola</p>')`), se inserta **como texto literal**, no como HTML. Los caracteres especiales se escapan automáticamente (seguro contra XSS).

---

**Inserción de HTML Crudo (`insertAdjacentHTML`)**

Si tienes un string que contiene HTML (ej. `"<div class='alert'>...</div>"`) y quieres que el navegador lo interprete y renderice, los métodos anteriores (`append`, etc.) no sirven porque lo tratarán como texto plano.

Para esto usamos **`elem.insertAdjacentHTML(where, html)`**.

**Posiciones (`where`)**

1.  **`"beforebegin"`**: Antes del elemento (hermano anterior).

2.  **`"afterbegin"`**: Dentro del elemento, al principio (primer hijo).

3.  **`"beforeend"`**: Dentro del elemento, al final (último hijo).

4.  **`"afterend"`**: Después del elemento (hermano siguiente).

```javascript
div.insertAdjacentHTML('beforebegin', '<p>Hola</p>');
```

> Variantes (Raras): Existen insertAdjacentText y insertAdjacentElement, pero raramente se usan porque append/before son más limpios.

**Eliminación y Clonación**

**Eliminar (`remove`)**

En el DOM moderno, un elemento se puede borrar a sí mismo directamente.

```javascript
div.remove(); 
```

> Si quieres mover un elemento de un lugar a otro, no necesitas borrarlo. Al insertarlo en el nuevo lugar (ej. nuevoPadre.append(viejoDiv)), se elimina/arranca automáticamente de su ubicación anterior.

**Clonar (`cloneNode`)**

A veces es más fácil copiar una estructura existente que crearla desde cero (especialmente si es compleja).

* **`elem.cloneNode(true)`:** Clonación **profunda**.
    * Copia el elemento, sus atributos y **todos sus hijos/contenido**.
    * *Es la opción habitual.*
* **`elem.cloneNode(false)`:** Clonación **superficial**.
    * Copia solo la etiqueta (atributos incluidos), pero **sin contenido** ni hijos.

**`DocumentFragment`**

Es un nodo especial **"invisible"** que sirve como envoltorio temporal.

* **Funcionamiento:** Si le agregas elementos y luego insertas el fragmento en el DOM, **el fragmento desaparece** y solo se insertan sus hijos.
* **Uso:** Evita múltiples "repintados" (reflows) del navegador al insertar listas grandes, mejorando el rendimiento.

```javascript
let fragment = new DocumentFragment();

// ... añadimos muchos <li> al fragment (ocurre en memoria) ...

ul.append(fragment); // Se insertan los <li>, el fragment se esfuma.
```

**Métodos "Legacy"**

Encontrarás estos métodos en scripts antiguos o tutoriales desactualizados. Son más "verbosos" (largos) porque **siempre requieren llamar al elemento padre** para modificar un hijo.

| Método Antiguo (Requiere padre) | Equivalente Moderno (Directo) |
| :--- | :--- |
| `parent.appendChild(node)` | `parent.append(node)` |
| `parent.removeChild(node)` | `node.remove()` |
| `parent.replaceChild(new, old)` | `old.replaceWith(new)` |
| `parent.insertBefore(node, next)` | `next.before(node)` |

**El extraño caso de `document.write`**

Un método prehistórico que sobrevive por razones de compatibilidad.

* **`document.write(html)`:** Escribe HTML directamente en la página "aquí y ahora".

1.  **Durante la carga:** Si se llama mientras el navegador lee el HTML, funciona rapidísimo (inyecta el texto en el stream de carga sin tocar el DOM).
2.  **Después de la carga:** Si se llama cuando la página ya cargó (ej. en un `setTimeout` o al hacer click en un botón), **BORRA TODO EL DOCUMENTO** y escribe solo el nuevo contenido.

> **Veredicto:** No lo uses en código moderno, salvo casos muy específicos de optimización extrema en scripts de tracking/anuncios de terceros.

---

**Resumen**

| Acción | Método Recomendado |
| :--- | :--- |
| **Crear** | `document.createElement('tag')` |
| **Insertar (Texto/Nodos)** | `append`, `prepend`, `before`, `after` |
| **Insertar (HTML String)** | `insertAdjacentHTML(where, html)` |
| **Eliminar** | `elem.remove()` |
| **Mover** | Simplemente haz un `append` en el nuevo lugar (se mueve solo) |
| **Clonar** | `elem.cloneNode(true)` |

### Estilos y Clases

Antes de tocar código, hay una **Regla de Oro**:

1.  **Preferir Clases CSS:** Para cambios de diseño estáticos (color, visibilidad, tema), usa clases. Es más limpio y fácil de mantener.

2.  **Usar `style` (JS):** Solo cuando las clases no sirvan, por ejemplo, para coordenadas dinámicas calculadas en tiempo real (`top`, `left`, barras de progreso).

---

**Manipulación de Clases (`className` y `classList`)**

Cambiar clases es la forma más común de alterar el diseño.

**A. `className` (La forma "bruta")**

Corresponde directamente al atributo `class="..."`.

* **Problema:** Si asignas algo (`elem.className = "nueva"`), **reemplazas toda la cadena** de clases anteriores.

* **Uso:** Útil para un reset total de clases.

**B. `classList` (La forma moderna y recomendada)**

Es un objeto especial iterable que permite manipular clases individualmente sin borrar las demás.

* **`elem.classList.add("clase")`**: Añade una clase.

* **`elem.classList.remove("clase")`**: Elimina una clase.

* **`elem.classList.toggle("clase")`**: Si existe la borra, si no existe la añade.

* **`elem.classList.contains("clase")`**: Devuelve `true/false`.

```javascript
// Iterar sobre clases
for (let name of document.body.classList) {
  alert(name); 
}
```

**Manipulación de Estilos en Línea (`elem.style`)**

La propiedad `elem.style` es un objeto que corresponde **solo al atributo `style="..."`** de la etiqueta HTML (no ve las clases CSS).

**Reglas de Escritura**

* **CamelCase:** Las propiedades CSS de varias palabras (`background-color`) se convierten a camelCase (`backgroundColor`).

* **Prefijos:** Los prefijos de navegador también siguen la regla (`-moz-border-radius` -> `MozBorderRadius`).

* **Unidades Obligatorias:** JS **no añade "px"** por ti. Si olvidas la unidad, no funcionará.

```javascript
elem.style.backgroundColor = "red"; // ✅ Correcto (CamelCase)
elem.style.marginTop = "20px";      // ✅ Correcto (con unidad)
elem.style.marginTop = 20;          // ❌ Incorrecto (ignorado por falta de 'px')
```

**Reseteo de Estilos**

Para eliminar un estilo puesto con JS y permitir que el elemento vuelva a usar el estilo definido en el CSS (hoja de estilos) o el default del navegador:

Existen dos formas principales:

**Opción A: Asignar una cadena vacía `""`**

Es la forma más común. Al limpiar el valor, el navegador ignora el estilo en línea y busca en las clases CSS.

```javascript
elem.style.display = "none"; // 1. Ocultar con JS (inline style)

// ... luego ...

elem.style.display = "";     // 2. Volver a la normalidad 
                             // (Se borra el estilo en línea, aplica el CSS externo)
```

**Opción B: Usar el método removeProperty**

Es la forma "formal". Requiere el nombre de la propiedad (en formato CSS, con guiones si es necesario).

```js
elem.style.backgroundColor = "red"; // Aplicamos color rojo

setTimeout(() => {
    // Eliminamos la propiedad explícitamente
    elem.style.removeProperty("background-color"); 
    // Ahora el elemento vuelve a su color original definido en el CSS
}, 1000);
```

**`style.cssText` (Escritura masiva)**

Permite escribir **todo** el atributo `style` como una cadena de texto completa.

> **⚠️ Cuidado:** Esta propiedad **borra/sobreescribe todos los estilos en línea anteriores** que tuviera el elemento.
> No agrega estilos nuevos a los existentes, los *reemplaza* por completo.

```javascript
div.style.cssText = `color: red !important; background: yellow;`;
```

**Lectura de Estilos Calculados (`getComputedStyle`)**

Aquí es donde muchos se confunden al intentar leer propiedades visuales.

* **`elem.style`:** Solo lee lo que está escrito manualmente en el atributo `style` del HTML. **No ve las clases CSS.**
* **`getComputedStyle(elem)`:** Lee el estilo **FINAL** (el resultado de aplicar CSS externo, clases, cascada, herencia y estilos del navegador).

```javascript
let estilos = getComputedStyle(elemento, [pseudo]);
```

**Características Clave**

* **Solo Lectura:** No puedes escribir en él. Es un reporte del estado final del elemento.
* **Valores Resueltos:** Devuelve siempre **unidades absolutas**.
    * Si tu CSS dice `font-size: 1em`, esto devolverá `16px` (o el cálculo correspondiente).
    * Si tu CSS dice `color: red`, esto devolverá `rgb(255, 0, 0)`.
* **Privacidad:** No puedes leer estilos de enlaces `:visited` (para evitar que las webs rastreen tu historial de navegación).
* **Propiedades Completas:** Debes pedir la propiedad exacta (ej. `paddingLeft` en vez de `padding`) para garantizar el valor correcto, ya que los atajos (*shorthands*) a veces dan resultados vacíos en ciertos navegadores.

| Objetivo | Método | Ejemplo |
| :--- | :--- | :--- |
| **Añadir/Quitar Clase** | `classList` | `div.classList.add('active')` |
| **Resetear todas las Clases** | `className` | `div.className = 'nueva'` |
| **Escribir estilo dinámico** | `style` | `div.style.left = '50px'` |
| **Leer estilo inline** | `style` | `alert(div.style.color)` |
| **Leer estilo FINAL (CSS)** | `getComputedStyle` | `getComputedStyle(div).color` |