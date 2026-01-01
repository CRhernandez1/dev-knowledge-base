# Asincronía en JavaScript

JavaScript es un lenguaje **Single-Threaded** (un solo hilo). Esto significa que solo puede hacer **una cosa a la vez**. Sin embargo, la asincronía permite que tareas pesadas (como pedir datos a un servidor o esperar un temporizador) se ejecuten en "segundo plano" sin bloquear el hilo principal.

Para entender la asincronía, hay que visualizar las piezas que componen el entorno de ejecución (como el navegador):

1.  **Call Stack (Pila de Ejecución):** Es donde se ejecuta el código JS síncrono. Lo que entra aquí, bloquea el hilo.
2.  **Web APIs:** Funcionalidades que **NO** son parte de JS, sino del navegador (DOM, `fetch`, `setTimeout`, Geolocalización). Aquí es donde se ejecutan realmente las tareas asíncronas.
3.  **Callback Queue (Cola de Callbacks):** Lista de espera para funciones listas para ejecutarse (ej. el callback de un `setTimeout` o un evento `click`).
4.  **Microtasks Queue (Cola de Microtareas):** Una cola VIP con **prioridad alta**. Aquí van las callbacks de las **Promesas**.

**El Event Loop (Bucle de Eventos)**

Es el coordinador central. Su trabajo es infinito y simple:
1.  Mira el **Call Stack**. ¿Está vacío?
2.  Si está vacío, mira la **Microtasks Queue**. ¿Hay algo? Pásalo al Stack.
3.  Si no hay microtareas, mira la **Callback Queue**. Pasa el siguiente elemento al Stack.

> **Regla de Oro:** El Event Loop nunca moverá nada de las colas al Stack hasta que el Stack esté totalmente vacío (hasta que el código síncrono haya terminado).

---

## AJAX

**AJAX** (Asynchronous JavaScript and XML) es la técnica tradicional para comunicar el cliente con el servidor de forma asíncrona (sin recargar la página).

**El problema: Callback Hell**

Antes de las promesas, gestionábamos la asincronía anidando funciones (callbacks). Si tenías que hacer varias peticiones secuenciales (ej. buscar usuario -> buscar sus pedidos -> buscar detalle del pedido), el código se volvía una pirámide ilegible.

```javascript
// ❌ Callback Hell (Difícil de leer y mantener)
setTimeout(() => {
    console.log('1. Petición inicial');
    setTimeout(() => {
        console.log('2. Datos recibidos');
        setTimeout(() => {
            console.log('3. Proceso finalizado');
        }, 1000);
    }, 1000);
}, 1000);
```

## Promesas

Una **Promesa** es un objeto contenedor para un valor futuro. Es como un "recibo" que te garantiza que recibirás un valor (éxito) o un error (fracaso) más adelante.

**Estados de una Promesa**

1.  **Pending (Pendiente):** Estado inicial, esperando respuesta.
2.  **Settled (Resuelta/Finalizada):** Ya no está pendiente. Puede ser:
    * **Fulfilled (Cumplida):** Éxito (ej. datos recibidos).
    * **Rejected (Rechazada):** Error (ej. servidor caído).

**Consumir Promesas (`.then`, `.catch`, `.finally`)**

Usamos la API moderna `fetch` para pedir datos. `fetch` devuelve una Promesa inmediatamente.

```javascript
// Ejemplo: Pedir datos de un producto ficticio
const request = fetch('[https://api.ejemplo.com/productos/1](https://api.ejemplo.com/productos/1)');

request
    .then(response => {
        // fetch devuelve primero los metadatos, hay que convertir el body a JSON
        // .json() TAMBIÉN es asíncrono y devuelve otra promesa
        return response.json(); 
    })
    .then(data => {
        // Aquí ya tenemos los datos finales
        console.log('Producto:', data.name);
    })
    .catch(err => {
        // Captura errores de red (ej. sin internet)
        console.error('⚠️ Error detectado:', err.message);
    })
    .finally(() => {
        // Se ejecuta SIEMPRE (útil para ocultar spinners de carga)
        console.log('Operación terminada');
    });
```

**Manejo de Errores Manual (throw)**

Un detalle crucial de fetch es que no rechaza la promesa en errores HTTP (404, 500). Solo rechaza si falla la red. Debemos lanzar el error manualmente.

```js
fetch('url-que-no-existe')
    .then(response => {
        if (!response.ok) { // .ok es false si el status es 404 o 500
            throw new Error(`Producto no encontrado (${response.status})`);
        }
        return response.json();
    })
    .catch(err => console.error(err)); // Aquí atrapamos nuestro error manual
```

## Construir Promesas

A veces necesitamos convertir APIs antiguas basadas en callbacks (como `setTimeout` o `navigator.geolocation`) en Promesas para usarlas cómodamente.

```javascript
// El constructor recibe una función ejecutora con dos argumentos: resolve y reject
const esperar = function(segundos) {
    return new Promise((resolve) => {
        // Cuando termine el timer, marcamos la promesa como "Fulfilled"
        setTimeout(resolve, segundos * 1000);
    });
};

// Uso elegante sin callbacks anidados
esperar(2)
    .then(() => {
        console.log('Han pasado 2 segundos');
        return esperar(1);
    })
    .then(() => console.log('Ha pasado 1 segundo más'));
```

## Async / Await (ES2017)

Es "Azúcar Sintáctica" sobre las Promesas. Permite escribir código asíncrono que **parece y se comporta como código síncrono**.

* **`async`:** Convierte una función para que devuelva automáticamente una promesa.

* **`await`:** **Detiene la ejecución** de la función en esa línea hasta que la promesa se resuelve. (No bloquea el Call Stack principal, solo la función).

```javascript
const obtenerDatosUsuario = async function() {
    // try...catch es la forma estándar de JS para errores, usada aquí
    try {
        const res = await fetch('[https://api.ejemplo.com/usuario/1](https://api.ejemplo.com/usuario/1)');
        
        if (!res.ok) throw new Error('Error en la petición');
        
        const data = await res.json();
        console.log(`Usuario: ${data.name}`);
        
        return data; // Esto será el valor "resolved" de la promesa que devuelve la función
        
    } catch (err) {
        console.error('Error capturado:', err.message);
        // Importante: Si queremos que el error se propague fuera, debemos relanzarlo
        throw err;
    }
};
```

## Ejecución en Paralelo 

A veces necesitamos hacer varias peticiones que no dependen una de la otra. Si usamos `await` una tras otra, perdemos tiempo. Para esto usamos los métodos estáticos de `Promise`.

**1. `Promise.all([prom1, prom2])` (Todo o Nada)**

* Ejecuta todas las promesas a la vez (en paralelo).

* **Éxito:** Devuelve un array con todos los resultados en el mismo orden de entrada.

* **Fallo:** Si **UNA** falla, todo explota (*Short-circuit*). La promesa global se rechaza inmediatamente con ese error.

```javascript
const obtenerVarios = async function() {
    try {
        const data = await Promise.all([
            fetch('[https://api.com/users](https://api.com/users)'),
            fetch('[https://api.com/orders](https://api.com/orders)'),
            fetch('[https://api.com/products](https://api.com/products)')
        ]);
        
        // data es un array de respuestas [resUsers, resOrders, resProducts]
        const responses = await Promise.all(data.map(res => res.json()));
        console.log(responses[0]); // Usuarios
    } catch (err) {
        console.error(err);
    }
};
```

**2. Promise.race([prom1, prom2]) (El más rápido)**

* Devuelve el resultado de la primera promesa que se complete (ya sea éxito o error).

* Uso común: Temporizadores de cancelación (Timeout). Si la petición tarda más de X segundos, la rechazamos manualmente.

```js
const timeout = function(sec) {
    return new Promise((_, reject) => {
        setTimeout(() => reject(new Error('La petición tardó demasiado')), sec * 1000);
    });
};

// Competición: Petición real vs Timer de 5 segundos
Promise.race([
    fetch('[https://api.lenta.com/datos](https://api.lenta.com/datos)'),
    timeout(5)
])
.then(res => console.log(res))
.catch(err => console.error(err));
```

**3. Promise.allSettled([prom1, prom2]) (ES2020)**

* Espera a que todas terminen, sin importar si fallaron o no.

* Nunca hace cortocircuito.

* Devuelve un array de objetos con el estado detallado:

    * { status: 'fulfilled', value: ... }

    * { status: 'rejected', reason: ... }

**4. Promise.any([prom1, prom2]) (ES2021)**

* Busca el primer éxito (primera promesa fulfilled).

* Ignora los errores (rejected) a menos que todas fallen.

* Es conceptualmente lo opuesto a Promise.all.