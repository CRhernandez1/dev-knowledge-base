# Definición de SQLite3

**SQLite3** es un sistema de gestión de bases de datos relacionales (RDBMS) escrito en lenguaje **C**.  
A diferencia de otros sistemas populares como **MySQL** o **PostgreSQL**, SQLite no es un motor de base de datos cliente/servidor, sino que es **integrado (embedded)**.

**En términos sencillos:**  
Es una base de datos completa contenida en un único archivo, que no requiere instalación de servidores ni configuraciones complejas.

---

## Características Principales

🖥️ Sin Servidor (Serverless)
La mayoría de las bases de datos requieren un proceso de servidor ejecutándose en segundo plano.  
**SQLite no.**

- El motor de SQLite se integra directamente en la aplicación que lo usa.
- No hay un proceso intermediario.

---

📁 Archivo Único
Toda la base de datos (tablas, índices, datos y vistas) reside en un solo archivo en el disco  
(generalmente con extensión `.db` o `.sqlite`).

- Facilita enormemente la **portabilidad**
- Simplifica las **copias de seguridad**

---

⚙️ Configuración Cero
No necesitas:

- Crear cuentas de usuario
- Asignar permisos complejos
- Configurar puertos de red

Es **plug-and-play**:  
👉 si tienes acceso al archivo, tienes acceso a la base de datos.

---

🔐 Cumple con ACID
Garantiza:

- **Atomicidad**
- **Consistencia**
- **Aislamiento**
- **Durabilidad**

Las transacciones se procesan de manera confiable y segura frente a fallos del sistema o cortes de energía.

---

Casos de Uso Comunes

- **📱 Aplicaciones Móviles**  
  Estándar en Android e iOS para guardar datos locales de las apps.

- **🖥️ Aplicaciones de Escritorio**  
  Navegadores web y programas de software lo usan para gestionar configuraciones, historial y cachés.

- **🌐 Internet de las Cosas (IoT)**  
  Ideal para dispositivos pequeños por su ligereza y bajo consumo de recursos.

- **🧪 Desarrollo y Prototipado**  
  Perfecto para fases iniciales de desarrollo o aprendizaje de SQL sin configurar servidores.

- **📦 Formato de Archivo**  
  Usado como formato eficiente para transferir conjuntos de datos entre sistemas.

---

Diferencias: SQLite3 vs. Motores Cliente-Servidor

| Característica   | SQLite3                                   | MySQL / PostgreSQL / Oracle                |
|------------------|-------------------------------------------|--------------------------------------------|
| Arquitectura     | Librería integrada en la aplicación       | Cliente ↔ Servidor de red                  |
| Almacenamiento   | Un único archivo en disco                 | Sistema de archivos gestionado por servidor|
| Usuarios         | Sin usuarios (usa permisos del archivo)   | Sistema interno de usuarios y roles        |
| Concurrencia     | Limitada (pocas escrituras simultáneas)   | Alta (miles de usuarios simultáneos)       |
| Uso Ideal        | Apps locales, dispositivos, tráfico bajo  | Sistemas empresariales, web masiva         |

> Nota: El "3" en SQLite3 se refiere a la versión 3 del software, que es el estándar actual estable y el más utilizado globalmente.

## Buenas Prácticas y Sintaxis

Esto es clave para que tu código no dé errores tontos.

### Comillas Simples vs. Dobles:
---

'Texto': Úsalas siempre para datos (strings). Ejemplo: WHERE titulo = 'El Principito'.

"Identificador": Úsalas para nombres de tablas o columnas que tengan espacios o caracteres raros (aunque es mejor evitar poner nombres raros a las tablas).

### Alias (AS)
---
Cuando usas funciones, el nombre de la columna resultado queda feo (ej: AVG(calificacion)).

Sin Alias: La columna se llama AVG(calificacion).

Con Alias: SELECT AVG(calificacion) AS Promedio ... -> La columna se llamará "Promedio". Esto es vital cuando tus datos van a ser leídos por otro programa o mostrados en un reporte.

### El flujo lógico de una consulta
---

Aunque tú lo escribes en un orden, SQL lo "piensa" así:

FROM: ¿De qué tabla saco los datos?

WHERE: Filtro lo que no sirve.

SELECT: Elijo las columnas que quiero.

ORDER BY: Ordeno el resultado final.

LIMIT: Corto el resultado si es necesario.

## `SELECT`

El comando `SELECT` es la base de todo, nos permite seleccionar las columnas que que queremos de la tabla seleccionada.

```sql
SELECT "author", "title" FROM "books";
-- Esto nos trae todas las filas de las columnas específicadas.
SELECT * FROM "books";
-- También podemos pedir mediante el asterisco todo los registro de la base de datos.
SELECT * FROM "books" LIMIT 5;
-- Con Limit podemos estipular cuantas filas queremos.
```
> Es recomendable no abusar del `*`, ya que en bases de datos con muchos datos, al traer todos los datos de la misma, puede ser muy lento, y por tanto poco eficiente.

## Filtrado de datos

Con la ayuda de la cláusula `WHERE` podemos decidir que filas ver.

¿Cómo funciona esto? La base de datos revisa fila por fila de las requeridas preguntando si la misma cumple la condición establecida.

También tenemos operadores lógicos: `AND`, `OR`, `NOT`

```sql
SELECT "title", "year" FROM "books"
WHERE "year" = 2021 or "year" = 2022;
```

En el caso de que la tabla books tuviera un campo / columna year (año de publicación del libro). Si solo queremos los que salieron entre el año 2021 y el 2022 podemos combinar el where con un or. OJO: No un and, ya que si miramos una fila concreta y primero preguntamos year es igual a 2021 y la respuesta es si, ya no es posible que ese year sea 2022. En cambio con el or, si no es igual a 2021, revisa si es 2022, y si es alguno de los dos, obtine la fila.

> También se pueda usar paréntesis, por ejemplo, para mezclar conjuntos de condiciones.

También podemos filtrar por `NULL`. Si un campo es nulo o no lo es. 
¿Qué quiere decir esto? que ese dato en concreto es desconocido o presenta ausencia de valor.

```sql
SELECT "tile", "translator" FROM "books";
-- Esto nos devolvera todos los campos, algunos de translator vendrán como NULL porque ese libro no habrán libros que no tengan traducción.

SELECT "title", "translator" FROM "books"
WHERE "translator" IS NOT NULL;
-- Esto trae todos las filas en la que ese campo no es Nulo. Para obter los nulos sería igual pero con IS NULL
```

### Búsqueda de patrones

Para buscar **coincidencias no exactas** en las cadenas de texto almacenadas en la base de datos se utilizan principalmente los operadores **`LIKE`** y los **comodines** (`%` y `_`).

Estos permiten filtrar resultados cuando no conocemos el valor exacto del texto.

### Operador `LIKE`
---

El operador `LIKE` se usa en la cláusula `WHERE` para comparar una columna de texto con un **patrón**.

Sintaxis básica

```sql
SELECT "title" 
FROM "books"
WHERE "title" LIKE 'The %';
-- Empieza por The y puede acabar por cualquier cosa
```

## Ordenamiento

Para ordenar las filas resultantes de una consulta utilizamos la cláusula `ORDER BY`.

```sql
SELECT "tile", "year", "rating" FROM libros
ORDER BY "year" DESC, "rating" DESC;
-- Criterio 1: Ordena primero por 'year' (los más recientes arriba).
-- Criterio 2: Si dos libros son del mismo año (empate), usa 'rating' para desempatar (el mejor valorado primero).
```

**ASC (Ascendente): Es el valor por defecto. Si no escribes nada, SQL asume este orden.**

Números: De menor a mayor (1, 2, 3...).

Texto: De la A a la Z.

---

**DESC (Descendente): Debes especificarlo explícitamente.**

Números: De mayor a menor (10, 9, 8...).

Texto: De la Z a la A.

## Funciones de agregación

Estas funciones toman muchas filas y las "aplastan" para devolverte un solo dato resumen.

- COUNT
    ```
    COUNT(*) VS COUNT("translator")

    El primero cuenta filas totales y el segundo cuenta las filas en las que el valor no es nulo.
    ```
    
- AVG

    ```Nos devuelve la media de todos los registros de una columna```

- MIN

    ```Nos devuelve el valor mínimo de la columna selecciona```

- MAX

    ```Nos devuelve el valor máximo de la columna selecciona```
    
- SUM

    ```Suma todos los valores de una columna```

> Podemos usar ROUND("valor", 2), para redondear un resultado decimal, el 2 puede variar al número de decimales que se desee

## Valores Únicos

Sirve para responder preguntas como: "¿Cuántos autores diferentes tengo?".

- Sin DISTINCT: SELECT "author" FROM "books"; -> Si tienes 10 libros de J.K. Rowling, su nombre saldrá 10 veces.

- Con DISTINCT: SELECT DISTINCT "author" FROM "books"; -> Su nombre saldrá solo una vez.

```sql
SELECT COUNT(DISTINCT autor) FROM libros;
-- Cuenta cuántos autores únicos existen en la base de datos.
```