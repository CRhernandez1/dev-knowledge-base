# Programación Orientada a Objetos (OOP)

Estos apuntes recogen los fundamentos y patrones de diseño de la Programación Orientada a Objetos (OOP) en JavaScript.

**Nota Importante:**

El contenido hace especial hincapié en las **Clases de ES6**, ya que representan el estándar moderno y profesional más utilizado en la industria actual.

Aunque se explican las mecánicas subyacentes y tradicionales (como las *Constructor Functions* y `Object.create`) para comprender cómo funciona el motor por debajo, no se cubren todos sus casos borde. En caso de necesitar profundizar en sintaxis *legacy* o detalles muy específicos de implementaciones antiguas, [documentación oficial de MDN](https://developer.mozilla.org/).

## Conceptos Fundamentales de OOP

**¿Qué es la OOP?**
Es un paradigma de programación basado en el concepto de **objetos**. Usamos objetos para modelar aspectos del mundo real (ej. un usuario, una lista de tareas) o abstracto (ej. estructura de datos HTML).

* Los objetos contienen **datos** (propiedades) y **código** (métodos).
* Los objetos son bloques de construcción "autocontenidos".
* Interactúan entre sí a través de una API pública (métodos accesibles).

**Los 4 Pilares de la OOP**

1.  **Abstracción:** Ignorar u ocultar los detalles complejos "under the hood" (como funciona por dentro) y exponer solo lo necesario (ej. usar `addEventListener` sin saber cómo el navegador maneja el evento internamente).
2.  **Encapsulamiento:** Mantener propiedades y métodos privados dentro de la clase, inaccesibles desde fuera. Esto previene que el estado se modifique accidentalmente por código externo. (En JS "tradicional" es difícil, pero existen convenciones y métodos modernos).
3.  **Herencia:** Hacer que una clase (hija) herede propiedades y métodos de otra clase (padre). Evita duplicar código.
4.  **Polimorfismo:** Una clase hija puede sobrescribir un método heredado del padre para adaptarlo a sus necesidades.

---

## Prototipos

A diferencia de lenguajes "Clásicos" (Java, C++, Python) donde las clases copian su comportamiento a las instancias, JavaScript utiliza **Prototipos**.

* **Herencia Prototípica (Prototypal Inheritance):** Todos los objetos en JS están vinculados a un objeto prototipo.
* **Delegación:** Cuando intentamos acceder a una propiedad o método en un objeto, si JS no lo encuentra allí, busca en su prototipo. El objeto "delega" la tarea.

**Las 3 formas de implementar OOP en JS**

1.  **Constructor Functions (Funciones Constructoras):** Técnica desde el inicio de JS. Se usan funciones normales llamadas con `new`.
2.  **ES6 Classes:** Sintaxis moderna (2015). **IMPORTANTE:** Es solo **"Azúcar Sintáctica" (Syntactic Sugar)**. Por debajo siguen siendo funciones constructoras y prototipos; no cambiaron cómo funciona el lenguaje, solo cómo se escribe.
3.  **Object.create():** La forma más directa de vincular un objeto a un prototipo específico (menos común en el día a día básico).

---

## Constructor Functions

Las Funciones Constructoras son patrones para crear objetos programáticamente.

* **Convención:** Siempre empezar con **Mayúscula** (`PersonObject`, no `personObject`).

* **Restricción:** No se pueden usar *Arrow Functions* porque no tienen su propio `this`.

**Proceso de Ejecución (Los 4 pasos de `new`)**

Cuando ejecutamos `const jonas = new Person('Jonas', 1991);`, el motor hace esto:

1.  **Creación:** Se crea un nuevo objeto vacío `{}`.

2.  **Binding:** El `this` de la función se asigna a ese nuevo objeto.

3.  **Linking (Enlace):** El nuevo objeto se vincula a su prototipo (`__proto__` se conecta a `Person.prototype`).

4.  **Return:** La función devuelve automáticamente el objeto.

```javascript
const Person = function(firstName, birthYear) {
    // Propiedades de instancia (Instance Properties)
    this.firstName = firstName;
    this.birthYear = birthYear;

    // ⛔️ NUNCA crear métodos dentro del constructor
    // this.calcAge = function() { ... };
    // Razón: Cada nueva instancia crearía una copia de la función.
    // Si tienes 1000 usuarios, tendrías 1000 copias idénticas en memoria.
};

const matilda = new Person('Matilda', 2017);
// matilda es una INSTANCIA de Person
```

* Cada función en JS (incluidos los constructores) tiene una propiedad llamada `.prototype`.

**Regla de Oro:**
* `Person.prototype` NO es el prototipo de `Person`.
* Es el prototipo que usarán los objetos creados por `Person`.

### Añadiendo métodos (Solución de rendimiento)
* Añadimos los métodos al objeto `prototype`.
* Así solo existe una copia en memoria y todas las instancias la comparten mediante referencia.

```js

// Prototypal Inheritance en acción  
Person.prototype.calcAge = function() {  
    console.log(2037 - this.birthYear);  
};

matilda.calcAge(); // 20  

* JS busca `calcAge` en `matilda` → No está.
* Mira el prototipo (`matilda.__proto__`) → Ahí está. Lo ejecuta.
```

### Propiedades y Verificación

* `__proto__`: Propiedad de la instancia que apunta al prototipo del constructor.

matilda.__proto__ === Person.prototype // true  

* `isPrototypeOf`: Método para comprobar la vinculación.

Person.prototype.isPrototypeOf(matilda) // true  

Person.prototype.isPrototypeOf(Person) // false (Confusión común)

### Propiedades propias vs Heredadas

* También podemos poner propiedades en el prototipo (`Person.prototype.species = 'Homo Sapiens'`).

matilda.hasOwnProperty('firstName') // true (Está en el objeto)  

matilda.hasOwnProperty('species') // false (Está en el prototipo)

---

### Prototypal Inheritance y The Prototype Chain

* La "Cadena de Prototipos" es la serie de enlaces entre objetos.
* Es similar al Scope Chain, pero para buscar propiedades.

#### El flujo de búsqueda (Lookup)

1. Busca en el objeto propio.
2. Si no encuentra, mira en su `__proto__`.
3. Si no encuentra, mira en el `__proto__` de ese prototipo.
4. Continúa hasta llegar a `Object.prototype` (el tope de la cadena).
5. Si supera eso, llega a `null` y devuelve `undefined`.

#### Visualización de la cadena (Ejemplo Person)

matilda → Person.prototype → Object.prototype → null  

* `matilda`: Tiene `firstName`, `birthYear`.
* `Person.prototype`: Tiene `calcAge`, `species`.
* `Object.prototype`: Tiene métodos universales como `.hasOwnProperty()`, `.toString()`.

---

### Prototypal Inheritance en Objetos Nativos.

* Cualquier array, función o elemento DOM en JS funciona igual.

#### Arrays

* Cuando haces `const arr = [1, 2, 3]`, es una abreviatura de `new Array(1, 2, 3)`.
* `arr` hereda de `Array.prototype`.

* Por eso tienes acceso a `.map`, `.filter`, `.push`.
* Esos métodos no están en tu array, están en el prototipo maestro de `Array`.

#### Extender Prototipos (Mala Práctica)

* Podrías hacer esto:

```js

Array.prototype.unique = function() {  
    return [...new Set(this)];  
};
```

* Aunque funciona, no se recomienda:
  * La próxima versión de JS podría añadir un método con el mismo nombre y romper tu código.
  * En equipos grandes genera confusión sobre qué métodos son estándar y cuáles custom.

## Classes

Introducidas en ECMAScript 2015, son la sintaxis moderna para hacer OOP en JavaScript.

Es vital entender que las clases en JS **NO** funcionan como en Java o C++.

* **No traen un nuevo modelo de herencia:** Por debajo, siguen utilizando **Funciones Constructoras** y **Herencia Prototípica**.

* Son simplemente una capa estética ("azúcar") para escribir lo mismo de forma más limpia y organizada.

* `class`: Palabra clave para definirla.

* `constructor()`: Método obligatorio (con ese nombre exacto) que funciona igual que la función constructora antigua. Se ejecuta inmediatamente al usar `new`.

* **Métodos:** Se escriben dentro de la clase, pero fuera del constructor. **Automáticamente se añaden al `.prototype`**.

```javascript
// Class expression
// const PersonCl = class {}

// Class declaration
class PersonCl {
    constructor(firstName, birthYear) {
        this.firstName = firstName;
        this.birthYear = birthYear;
    }

    // Los métodos se añadirán a .prototype automáticamente
    calcAge() {
        console.log(2037 - this.birthYear);
    }
}

const jessica = new PersonCl('Jessica', 1996);
jessica.calcAge(); 
// jessica.__proto__ === PersonCl.prototype (TRUE)
```

* **No Hoisting:** A diferencia de las declaraciones de funciones, las clases no se elevan. Debes definirlas antes de usarlas.

* **First-Class Citizens:** Se pueden pasar a funciones y retornar desde funciones.

* **Strict Mode:** El cuerpo de una clase se ejecuta siempre en "use strict", incluso si no lo activaste en el archivo.

### Getters y Setters 

Son propiedades especiales que parecen valores normales pero que por debajo ejecutan funciones. Sirven para transformar datos al leerlos (`get`) o validarlos al escribirlos (`set`).

**En Objetos Literales**
```javascript
const account = {
    owner: 'Jonas',
    movements: [200, 530, 120, 300],

    // GETTER: Se usa como propiedad, no como función
    get latest() {
        return this.movements.slice(-1).pop();
    },

    // SETTER: Debe tener exactamente un parámetro
    set latest(mov) {
        this.movements.push(mov);
    }
};

console.log(account.latest); // 300 (Sin paréntesis)
account.latest = 50;         // Se asigna con =
```

**En Clases: Validación de Datos**

Un patrón muy común es usar Setters para validar datos al momento de crear el objeto.

* **El conflicto de nombres:** Si el setter se llama igual que la propiedad del constructor (ej. `fullName`), se crea un bucle infinito (*Stack Overflow*).

* **Solución:** Usar la convención `_` (guion bajo) para crear una variable "privada" temporal.

```javascript
class PersonCl {
    constructor(fullName, birthYear) {
        this.fullName = fullName; // Esto dispara el setter automáticamente
        this.birthYear = birthYear;
    }

    // Validación: El nombre debe tener espacios
    set fullName(name) {
        if (name.includes(' ')) {
            this._fullName = name; // Guardamos en variable con _
        } else {
            alert(`${name} is not a full name!`);
        }
    }

    get fullName() {
        return this._fullName; // Devolvemos la variable con _
    }
}
```

### Static Methods

Son métodos que están adjuntos al **Constructor** (la clase en sí), no al prototipo. Por tanto, las instancias (los objetos creados) **NO** pueden usarlos.

* **Ejemplo nativo:** `Array.from()`.

    * `Array.from(document.querySelectorAll('h1'))` funciona.

    * `[1, 2, 3].from()` falla, porque `.from` no está en el prototipo de los arrays, está en el constructor `Array`.

**Creación en Clases**

Se usa la palabra clave `static`. Son útiles para funciones de utilidad (helpers) relacionadas con la clase pero que no dependen de una instancia específica.

```javascript
class PersonCl {
    constructor(firstName) {
        this.firstName = firstName;
    }

    // Método de Instancia (Heredable)
    calcAge() { console.log('Edad...'); }

    // Método Estático (Solo accesible desde PersonCl)
    static hey() {
        console.log('Hey there 👋');
        console.log(this); // Aquí 'this' es la CLASE completa, no el objeto
    }
}

PersonCl.hey(); // Funciona
// jessica.hey(); // Error: jessica.hey is not a function
```

### Herencia entre Clases

La herencia permite que una clase hija (Child) derive funcionalidades de una clase padre (Parent).

Se utilizan dos palabras clave fundamentales:

1.  **`extends`:** Establece la relación de herencia. Conecta los prototipos por debajo.

2.  **`super`:** Función especial que llama al constructor de la clase padre.

**Cuando una clase extiende a otra, el comportamiento del `constructor` cambia drásticamente:**

1.  **Si NO necesitas nuevas propiedades:**

    No hace falta escribir un `constructor` en la clase hija. JS lo invoca automáticamente y pasa los argumentos al padre.

2.  **Si SÍ necesitas nuevas propiedades (El caso común):**

    * Debes escribir el constructor.

    * **OBLIGATORIO:** Debes llamar a `super()` **antes** de usar la palabra clave `this`.

    * **¿Por qué?** En una clase derivada, el objeto `this` no se crea hasta que se ejecuta `super()`. Si intentas acceder a `this` antes, obtendrás un *ReferenceError*.

```javascript
class PersonCl {
    constructor(fullName, birthYear) {
        this.fullName = fullName;
        this.birthYear = birthYear;
    }
    
    calcAge() { console.log(2037 - this.birthYear); }
}

// StudentCl HEREDA de PersonCl
class StudentCl extends PersonCl {
    constructor(fullName, birthYear, course) {
        // 1. Siempre primero: Llamada al constructor padre
        // Esto crea el 'this' inicial
        super(fullName, birthYear);
        
        // 2. Luego añadimos las propiedades nuevas
        this.course = course;
    }

    introduce() {
        console.log(`Hola, soy ${this.fullName} y estudio ${this.course}`);
    }

    // Polimorfismo: Sobrescribir un método del padre
    calcAge() {
        console.log(`Soy estudiante y tengo ${2037 - this.birthYear} años (más o menos)`);
    }
}

const martha = new StudentCl('Martha Jones', 2002, 'Computer Science');
martha.introduce(); // Usa método propio
martha.calcAge();   // Usa método sobrescrito
```

### Encapsulamiento: Class Fields 

Durante mucho tiempo, JS no tuvo privacidad real. Se usaba la convención `_propiedad` (guion bajo), pero eso no impedía que alguien modificara el dato desde fuera ("Fake Encapsulation").

La propuesta moderna de **Class Fields** introduce 4 tipos de campos para solucionar esto y limpiar la sintaxis.

**Public Fields**

Son variables que se definen al nivel superior de la clase (fuera del constructor).

* **Diferencia clave:** Se añaden a **todas las instancias** (al objeto en sí), **NO** al prototipo.

* No usan `const` ni `let`.

```javascript
class Account {
    // Public Fields (Instancias)
    locale = navigator.language;
    
    // Estos campos estarán presentes en CADA objeto creado
    constructor(owner, currency) {
        this.owner = owner;
        this.currency = currency;
    }
}
```

**Private Fields**

Utilizan el símbolo almohadilla **`#`**.

* **Seguridad:** Son verdaderamente privados. Si intentas acceder a ellos desde fuera de la clase (`acc.#movements`), JS lanzará un error de sintaxis.

* **Regla:** Deben declararse (definirse) arriba del todo, fuera del constructor, antes de usarse dentro.

```javascript
class Account {
    // 1. Definición obligatoria al inicio
    #movements = [];
    #pin; // Podemos definir sin inicializar

    constructor(owner, currency, pin) {
        this.owner = owner;
        this.currency = currency;
        
        // 2. Asignación dentro del constructor
        this.#pin = pin;
        // this.#movements es accesible aquí dentro
    }

    // API Pública para interactuar con datos privados
    deposit(val) {
        this.#movements.push(val);
    }
    
    getMovements() {
        return this.#movements;
    }
}

const acc1 = new Account('Jonas', 'EUR', 1111);
acc1.deposit(100);
// console.log(acc1.#movements); ❌ ERROR: Private field '#movements' must be declared in an enclosing class
```
**Private Methods**

* **Sintaxis:** `#nombreMetodo()`.

* Útiles para lógica interna auxiliar que no quieres que el usuario final toque.

* *(Nota: El soporte en navegadores para esto llegó un poco más tarde que para los campos, pero hoy es estándar).*

```javascript
class Account {
    // Método público
    requestLoan(val) {
        if (this.#approveLoan(val)) {
            this.deposit(val);
            console.log('Préstamo aprobado');
        }
    }

    // Método Privado (Solo accesible dentro de la clase)
    #approveLoan(val) {
        return true; // Lógica compleja oculta
    }
}
```

**Static Public/Private Fields**

Podemos combinar static con lo anterior.

* `static numInstances` = 0; -> Accesible solo como Account.numInstances.

* `static #helper()` -> Accesible solo dentro de la clase.


## Object.create()

Es la tercera forma de implementar herencia prototípica. Es la más "pura" porque desconecta la idea de clases/constructores y simplemente vincula objetos con objetos.

**Funcionamiento**

* No usa `new`.

* No usa `.prototype` property de la misma forma simulada.

* No tiene constructor automático.

Simplemente creas un objeto que servirá de prototipo, y luego creas nuevos objetos diciéndoles explícitamente: "Tu prototipo será este objeto de aquí".

```javascript
// 1. Definimos el objeto que servirá de Prototipo
const PersonProto = {
    calcAge() {
        console.log(2037 - this.birthYear);
    },
    
    // Función manual para inicializar (no es un constructor real)
    init(firstName, birthYear) {
        this.firstName = firstName;
        this.birthYear = birthYear;
    }
};

// 2. Creamos el objeto vinculado
// 'steven' será un objeto vacío con PersonProto en su __proto__
const steven = Object.create(PersonProto);

console.log(steven); // {}
steven.name = 'Steven';
steven.birthYear = 2002;
steven.calcAge(); // Funciona por delegación

// 3. Manera más organizada usando el método init manual
const sarah = Object.create(PersonProto);
sarah.init('Sarah', 1979);
sarah.calcAge();
```