# Fundamentos teóricos del lenguaje

## **Inclusión de JavaScript en HTML**

La filosofía clave en programación es la **separación de preocupaciones** (separation of concern): **HTML** para la estructura, **CSS** para el estilo y **JavaScript** para el comportamiento dinámico. Deben mantenerse en archivos separados.

Existen varias maneras de incluir código JavaScript en HTML, cada una con pros y contras:

| Método | Dónde se coloca | Comportamiento | Uso Recomendado |
| --- | --- | --- | --- |
| **Inline** | Dentro de la etiqueta `<script>` en el HTML. | Se ejecuta inmediatamente al ser leído. | No recomendado, viola la separación de preocupaciones. |
| **Link en head** | `<script src="archivo.js"></script>`. | El navegador pausa el *parsing* del HTML, descarga y ejecuta el script de inmediato. | Causa errores si el script necesita acceder a elementos del DOM que aún no se han creado. |
| **Link al final del body** | `<script src="archivo.js"></script>`. | Garantiza que el script se ejecuta solo después de que todo el HTML se ha parseado y el DOM está listo. | Soluciona errores de acceso al DOM, pero retrasa la interactividad si el script es pesado. |
| **Con async** | En `<head>`: `<script async src="archivo.js"></script>`. | **Descarga en paralelo** con el parsing del HTML, pero pausa el parsing para **ejecución inmediata** al completarse la descarga. | Scripts externos que **no tienen dependencia** del DOM (ej. analytics o chatbots). |
| **Con defer** | En `<head>`: `<script defer src="archivo.js"></script>`. | **Descarga en paralelo** con el parsing del HTML, pero **pospone la ejecución** hasta que el parsing del HTML haya finalizado. | **Recomendado** para la mayoría de los scripts internos, ya que mejora la velocidad y garantiza la disponibilidad del DOM. |

## Fases de Compilación

Esta sección explica cómo el motor procesa y ejecuta tu código, y cómo los navegadores modernos han difuminado la línea entre "interpretado" y "compilado".

Antes de que ocurra cualquier acción, el motor (Engine) transforma tu código fuente:

- Tokenizing (Tokenización):
El motor rompe tu código en piezas individuales llamadas "tokens" (palabras clave let, operadores =, nombres variable).

- Parsing (Análisis):
Organiza esos tokens en un árbol de estructura lógica llamado AST (Abstract Syntax Tree). Aquí ocurre el chequeo de gramática; si hay un error de sintaxis, el proceso se detiene antes de ejecutar nada.

- Code Generation:
Convierte ese AST en código ejecutable (Bytecode) para la máquina.

### Ejecución Híbrida: Interpretación + JIT

Aunque clásicamente se define a JS como un lenguaje interpretado (lee y ejecuta línea por línea, lo que permite ver errores parciales al instante), los motores modernos (como V8 en Chrome) utilizan una técnica llamada JIT (Compilación Justo a Tiempo) que lo hace comportarse en rendimiento casi como un lenguaje compilado.

### ¿Cómo funciona este proceso "mágico"?

- Inicio Rápido (Intérprete):
Al principio, el motor usa un intérprete ("Ignition" en V8) para ejecutar el Bytecode inmediatamente. Esto permite que la página cargue y arranque rápido sin esperar a compilar todo el programa.

- El Monitor (Profiler):
Mientras el código corre, un "monitor" observa qué partes se usan más. Si detecta una función que se ejecuta muchas veces (código "caliente" o hot code), la marca.

- Compilación y Optimización:
El compilador ("TurboFan" en V8) toma esas partes marcadas y las recompila a código máquina nativo altamente optimizado.

- Resultado:
Obtienes la flexibilidad de un lenguaje interpretado (inicio inmediato, tipado dinámico) con la velocidad de ejecución de uno compilado (código máquina directo al procesador) en las tareas repetitivas.


## LA ARQUITECTURA DE JAVASCRIPT

Para entender cómo funciona JS, debemos dividir el proceso en dos grandes mundos:

1.  **El Motor de JS (V8):** Quien lee y ejecuta el código (Contextos, Stack, Memoria).

2.  **El Entorno (Navegador):** Quien proporciona herramientas extra y gestiona la asincronía (Web APIs, Event Loop).

---

### El Motor y el Contexto de Ejecución

Cuando el motor recibe tu archivo, no empieza a ejecutar líneas inmediatamente. Primero tiene que preparar el terreno.

#### 1. Execution Context (Contexto de Ejecución)

Es el entorno físico, la "caja", donde se procesa y transforma el código.
Existen dos tipos:

* **GEC (Global Execution Context):** Se crea **una sola vez** al arrancar el script. Es la base.

* **FEC (Function Execution Context):** Se crea una caja nueva **cada vez** que se invoca una función.

#### 🔎 Detalle Técnico: Funciones Estándar vs. Arrow Functions
* **Function (`function a() {}`):** Genera un contexto "pesado". Crea su propio entorno léxico, su propio objeto `arguments` y, lo más importante, define su propio valor de `this` (Binding).
* **Arrow Function (`const a = () => {}`):** Genera un contexto "ligero". **NO** crea su propio `this` ni `arguments`. Si usas `this` dentro de una arrow function, el motor no lo encuentra en esa caja y sale a buscarlo al contexto superior (Lexical `this`).

---

### El Ciclo de Vida

**Aquí es donde entra el Lexical Environment.** Todo contexto pasa obligatoriamente por dos fases.

#### FASE A: La Fase de Creación

El motor escanea el código **sin ejecutarlo**. Aquí se construye la "infraestructura".

En esta fase ocurren 3 cosas críticas:

1.  **Creación del Objeto Global:** (`window` en navegadores).

2.  **Creación del `this`:** Se decide a qué hace referencia `this`.

3.  **SETUP DEL LEXICAL ENVIRONMENT (Entorno Léxico):**

    El motor crea un objeto interno (la memoria de este contexto). Este objeto tiene dos partes:

    * **Parte 1: Environment Record (El Inventario):** El motor busca todas las declaraciones.

        * **`function`:** Se guarda la función entera en memoria. (Por esto podemos usar funciones antes de declararlas).

        * **`var`:** Se reserva el espacio y **se asigna `undefined`**. (Esto es el Hoisting clásico).

        * **`let` / `const`:** Se reserva el nombre en memoria pero se marca como **"Uninitialized"** (No inicializado). Esto crea la **TDZ (Temporal Dead Zone)**. Si intentas acceder aquí, el motor lanza un error porque la variable existe pero tiene el acceso bloqueado.

    * **Parte 2: Outer Environment Reference (El Enlace):**

        * Se guarda una referencia (un puntero) al Lexical Environment del padre. Esto es lo que crea la **Scope Chain**.


#### FASE B: La Fase de Ejecución 

El motor vuelve al principio y recorre el código línea por línea.

* **Asignación de Variables:** Aquí es donde la variable deja de ser `undefined`.

    * *Ejemplo:* `var a = 10;`

    * En Fase Creación: `a` era `undefined`.

    * En Fase Ejecución: El motor busca `a` en el Environment Record y actualiza el valor a `10`.
* **Ejecución de Código:** Si encuentra una llamada a función, pausa este contexto y crea uno nuevo (repitiendo Fase A y B para esa función).

---

### Call Stack 

Es la estructura de datos que lleva el orden de ejecución. Guarda **"Registros"** (Stack Frames).

Cada vez que se crea un Contexto, se crea un Registro en la pila que contiene:

1.  El nombre de la función.

2.  La línea de código actual.

3.  Una referencia al **Lexical Environment** que acabamos de crear.

**Regla:** LIFO (Last In, First Out). El último en entrar es el primero en salir.

---

### Scope (Alcance) y Scope Chain

El Scope no es un objeto físico, es la **regla de visibilidad**. El Lexical Environment es la implementación física de esa regla.

1.  **Scope Chain (La búsqueda):**
    Cuando el código pide una variable (`console.log(x)`), el motor hace esto:
    * Mira en el **Environment Record** actual. ¿Está? Úsala.
    * ¿No está? Usa el puntero **Outer Reference** para subir al entorno del padre.
    * ¿No está? Sube al abuelo.
    * Llega al Global. ¿No está? -> `ReferenceError`.

2.  **Scope de Bloque (La diferencia var vs let):**
    * Un bloque es `{ ... }` (if, for, switch).
    * **`let` / `const`:** Crean un nuevo Entorno Léxico declarativo para ese bloque. Las variables mueren cuando cierra la llave `}`.
    * **`var`:** Ignora los bloques. Se guarda en el Entorno Léxico de la **Función** más cercana (o el Global). Por eso "contamina" y se escapa de los `if`.

---

### Event Loop 

Javascript es **Single Threaded** (un solo hilo). Solo tiene una Call Stack. Si hacemos algo lento (como pedir datos a un servidor o esperar 5 segundos), la página se congelaría.

Para evitarlo, el navegador ayuda con componentes extra.

#### 1. Los Componentes
* **Call Stack:** Donde ocurre el JS síncrono.
* **Web APIs:** Herramientas del navegador (`setTimeout`, `DOM`, `fetch`, `localStorage`). Trabajan en hilos separados (C++).
* **Colas (Queues):** Salas de espera.
    * **Microtask Queue (VIP):** Aquí van las **Promesas** (`.then`) y `MutationObserver`.
    * **Task Queue (Macrotask):** Aquí van los `setTimeout`, `setInterval`, eventos DOM (`click`).

#### 2. El Render Pipeline (La Pantalla)

**Coordinación del Event Loop con la Pantalla**

El Event Loop no solo maneja tareas y microtareas; tiene que coordinarse con la pantalla. Tu pantalla se refresca (normalmente) 60 veces por segundo. Eso significa que el navegador tiene una ventana de **16.6 milisegundos** para hacer todo.

**El Render Pipeline (Tubería de Renderizado)**

Es el proceso que ocurre cuando hay cambios visuales. Pasos detallados:

1.  **JavaScript:** Se ejecuta tu código (ej. cambiar una clase, añadir un elemento).

2.  **Style (Cálculo de Estilos):** El navegador recalcula qué reglas CSS aplican a cada elemento (CSSOM).

3.  **Layout (Reflow/Geometría):** El paso más costoso. Calcula cuánto mide y dónde está cada caja (div, botón) en píxeles. _"Este div mide 200x200 y está en la posición (10, 50)"_.

4.  **Paint (Pintura):** Rellena los píxeles. Colores, fondos, textos, sombras, bordes.

5.  **Composite (Composición):** Si hay capas (z-index, transformaciones 3D), el navegador las aplasta en una sola imagen final para la pantalla.


El Event Loop es un guardia de tráfico inteligente:

1.  Ejecuta **TODO** el Javascript síncrono (Call Stack).

2.  Ejecuta **TODAS** las Microtareas (Promesas).

3.  **MOMENTO DE DECISIÓN:** El Event Loop mira el reloj.

    * *¿Han pasado 16ms? ¿Hay cambios visuales pendientes?*
    * **SÍ:** Ejecuta el Render Pipeline (Style -> Layout -> Paint).
    * **NO:** Se salta el renderizado para ahorrar energía.

4.  Ejecuta **UNA** Macrotarea (setTimeout).

5.  Vuelve a empezar.

> **Conclusión:** Por eso `setTimeout(..., 0)` no es inmediato. Tiene que esperar a que el JS acabe, las microtareas acaben, y posiblemente a que el navegador decida si necesita pintar la pantalla antes.

#### 3. La Lógica del Event Loop (El Bucle Infinito)
El Event Loop es un algoritmo que gira constantemente haciendo estas comprobaciones estrictas:

1.  **¿Hay algo en la Call Stack?**
    * **SÍ:** ¡Espera! No hagas nada más. Deja que el JS acabe.
    * **NO:** (El Stack está vacío). Continúa.

2.  **¿Hay algo en la Microtask Queue? (Prioridad Absoluta)**
    * **SÍ:** Mueve la primera microtarea al Stack. Ejecútala. Repite hasta que la cola de microtareas esté **VACÍA** (Si una microtarea crea otra, también se ejecuta ahora).
    * **NO:** Continúa.

3.  **Oportunidad de Renderizado (Render Pipeline):**
    * El navegador decide: "¿Es necesario pintar? ¿Han pasado 16ms? ¿Hay cambios visuales?"
    * **SÍ:** Ejecuta Style -> Layout -> Paint.
    * **NO:** Se salta este paso.

4.  **¿Hay algo en la Task Queue (Macrotareas)?**
    * **SÍ:** Toma **SOLAMENTE UNA** tarea (la más antigua). Muévela al Call Stack. Ejecútala.
    * (Nota: A diferencia de las microtareas, no vacía toda la cola de golpe. Hace una y vuelve a empezar el ciclo).

---

### EL EJEMPLO COMPLETO

Vamos a analizar un código que usa **todo** lo explicado.

```javascript
// --- [GLOBAL CONTEXT - FASE CREACIÓN] ---
// 1. Lexical Env Global:
//    - Record: { 
//        titulo: undefined (var), 
//        config: <uninitialized> (let),
//        iniciar: <function ref>
//      }
//    - Outer: null

console.log("1. Inicio"); 

var titulo = "Demo JS";       // Asignación (Fase Ejecución)
let config = { delay: 0 };    // Inicialización (Fin TDZ)

// Arrow function (No crea 'this', usa el Global)
const procesar = () => {
    // Scope Chain: Busca 'titulo'. No está en procesar -> Sube a Global.
    console.log("4. Procesando: " + titulo); 
    
    // MICROTAREA (Promesa)
    Promise.resolve().then(() => {
        console.log("5. Microtarea (Promesa)");
        titulo = "Demo Finalizada"; // Cambia variable global
    });

    // RENDER: Cambio visual
    document.body.style.background = 'blue'; 
};

function iniciar() {
    // --- [INICIAR CONTEXT - FASE CREACIÓN] ---
    // 1. Lexical Env Iniciar:
    //    - Record: { estado: undefined }
    //    - Outer: Global Lexical Env
    
    var estado = "Activo"; // Asignación

    // MACROTAREA (setTimeout)
    // Web API recibe el timer. Como es 0ms, lo manda a la Task Queue YA.
    setTimeout(() => {
        console.log("6. Macrotarea (Timeout)");
    }, 0);

    procesar(); // Llamada síncrona
    
    console.log("3. Fin de función iniciar");
}

iniciar();

console.log("2. Fin del Script Global");
```

#### LA NARRATIVA PASO A PASO

1.  **Fase Creación Global:**
    * Se crea el contexto Global.
    * Se crea el **Lexical Environment**. `titulo` es `undefined`. `config` está en zona muerta. `iniciar` está listo para usarse.

2.  **Ejecución Síncrona (Call Stack):**
    * `console.log("1. Inicio")`.
    * `titulo` recibe valor "Demo JS".
    * `config` recibe el objeto.
    * Llamada a `iniciar()`.

3.  **Dentro de `iniciar`:**
    * Se crea Contexto y Entorno Léxico de `iniciar`.
    * **SetTimeout:** JS llama a la Web API. La Web API ve 0ms y mete el callback en la **Task Queue (Macrotarea)**.
    * Llamada a `procesar()`.

4.  **Dentro de `procesar` (Arrow Function):**
    * `console.log("4. Procesando: " + titulo)`. (Usó Scope Chain para leer `titulo`).
    * **Promesa:** El callback (`.then`) se va directo a la **Microtask Queue**.
    * **Cambio DOM:** `background = 'blue'`. El navegador marca una "flag" interna diciendo "Necesito repintar la pantalla".
    * `procesar` termina. Sale del Stack.

5.  **Cierre de `iniciar` y Global:**
    * `console.log("3. Fin de función iniciar")`.
    * `iniciar` termina. Sale del Stack.
    * `console.log("2. Fin del Script Global")`.
    * Global termina. **Call Stack VACÍA.**

6.  **El Event Loop toma el control:**

    * **Paso A (Microtareas):**
        * ¿Hay microtareas? SÍ (La promesa).
        * Mueve callback al Stack -> `console.log("5. Microtarea")`.
        * Cambia `titulo` a "Demo Finalizada".
        * ¿Más microtareas? NO.

    * **Paso B (Render Pipeline):**
        * El Event Loop ve la "flag" de repintado y que el stack está vacío.
        * Calcula estilos -> Layout -> Pinta el fondo azul. (El usuario ve el cambio ahora).

    * **Paso C (Macrotareas):**
        * ¿Hay macrotareas? SÍ (El Timeout).
        * Mueve callback al Stack -> `console.log("6. Macrotarea")`.

**Resultado final en consola:**

1.  `Inicio`

2.  `Procesando: Demo JS`

3.  `Fin de función iniciar`

4.  `Fin del Script Global`

5.  `Microtarea (Promesa)`

6.  `Macrotarea (Timeout)`