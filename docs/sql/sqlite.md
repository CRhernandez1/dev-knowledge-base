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

- FROM & JOIN (¿De dónde saco los datos?)
- WHERE (Filtrar filas)
- GROUP BY (Agrupar)
- HAVING (Filtrar grupos)
- SELECT (¿Qué columnas muestro?)
- ORDER BY (Ordenar resultados)
- LIMIT (Limitar cantidad)

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
--También para rangos, tenemos el BETWEEN
-- Con between sería WHERE "year" BETWEEN 2021 AND 2022;
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

## La importancia del modelo relacional

Cuando usamos una sola tabla para todo (como una hoja de cálculo gigante), parece más fácil al principio porque "lo ves todo junto". Sin embargo, esto genera tres problemas graves que destruyen la calidad de tus datos a largo plazo.

Supongamos que tenemos esta tabla única de Libros:

| Título           | Autor           | Email Autor        | Editorial  | Dirección Editorial |
|------------------|-----------------|--------------------|------------|---------------------|
| Harry Potter 1   | J.K. Rowling    | jk@rowling.com     | Salamandra | Calle Falsa 123     |
| Harry Potter 2   | J.K. Rowling    | jk@rowling.com     | Salamandra | Calle Falsa 123     |
| Harry Potter 3   | J.K. Rowling    | jk@rowling.com     | Salamandra | Calle Falsa 123     |
| El Principito    | Saint-Exupéry   | ant@exupery.fr     | Salamandra | Calle Falsa 123     |



**Redundancia de Datos (Desperdicio de Espacio)**

El Problema: Fíjate en la tabla de arriba. Hemos escrito "J.K. Rowling", su email, "Salamandra" y la dirección "Calle Falsa 123" repetidas veces.

La Consecuencia: Si la biblioteca tiene 1 millón de libros, estarás guardando millones de veces la misma dirección de la editorial. Esto hace que la base de datos sea pesada, lenta y costosa de almacenar.

**La Pesadilla de la Actualización (Anomalía de Modificación)**

Este es el problema más peligroso.

El Escenario: Imagina que la Editorial Salamandra se muda de "Calle Falsa 123" a "Avenida Real 45".

En una sola tabla: Tienes que buscar todas las filas donde aparezca "Salamandra" (quizás sean 50.000 libros) y cambiar la dirección una por una.

El Riesgo: Si el sistema falla a la mitad o se te olvida una fila, tendrás datos corruptos: algunos libros dirán que la editorial está en la calle vieja y otros en la nueva. Tu base de datos ha dejado de ser confiable.

**Rigidez de Datos (Anomalía de Inserción)**

El Problema: En una tabla única, no puedes guardar información sobre algo si no tienes el dato completo de la fila.

Ejemplo: Quieres registrar a un nuevo autor prometedor en tu base de datos, pero aún no ha publicado ningún libro.

El Bloqueo: Como la tabla es de "Libros", no puedes crear una fila solo para el autor (o tendrías que dejar el título como NULL, lo cual es sucio). El sistema te obliga a inventar un libro falso o a no guardar al autor.

**La Solución: El Modelo Relacional (Varias Tablas)**

La solución es dividir la información en entidades lógicas (autores, editoriales, libros) y conectarlas mediante IDs.

- Ventajas Inmediatas:

  Fuente Única de la Verdad (SSOT):
  La dirección de "Salamandra" se escribe una sola vez en la tabla publishers.

  Si se mudan, solo cambias ese dato una vez. Automáticamente, los 50.000 libros que apuntan a ese ID de editorial estarán "actualizados" porque simplemente están leyendo la referencia.

- Eficiencia:

  En lugar de repetir el texto "J.K. Rowling" (12 bytes) millones de veces, solo repites el número 1 (4 bytes). El ahorro de espacio es gigantesco.

- Flexibilidad:

  Puedes añadir un autor a la tabla authors aunque no tenga libros todavía. Existe independientemente.

## Tipos de Relaciones 

**Uno a Uno (One-to-One):** Un autor escribe solo un libro y un libro es escrito por solo un autor (poco común en la realidad).

**Uno a Muchos (One-to-Many):** Un editor (publisher) publica muchos libros, pero un libro específico pertenece a un solo editor.

**Muchos a Muchos (Many-to-Many):** Un autor puede escribir muchos libros, y un libro puede tener múltiples autores (coautores).

### Diagramas ER (Entity Relationship)

  Se usan para visualizar estas relaciones. Se utiliza la Notación de Pata de Gallo (Crow's Foot notation):
  
  - Línea simple: Relación "Uno".
  - Pata de gallo (tres líneas): Relación "Muchos".
  - Círculo: Opcional (Cero).

## Keys (Claves): El Pegamento de los Datos

Las claves son el mecanismo que permite que las relaciones funcionen. Sin ellas, las tablas serían islas aisladas.

- A. Primary Key (PK) - La Identidad
  Qué es: Es la columna que identifica de forma única e irrepetible a cada fila de una tabla.

  Reglas:

  - Nunca puede ser NULL (vacío).
  - Nunca puede repetirse.

  > Mejor Práctica: Aunque existen identificadores naturales (como el DNI o ISBN), en bases de datos casi siempre usamos un número entero autoincremental (id: 1, 2, 3...) porque es más rápido para el ordenador procesarlo.


- B. Foreign Key (FK) - La Referencia
  Qué es: Es una columna que apunta a la Primary Key de otra tabla. Es como guardar un "link" o acceso directo hacia otra fila.

  Función: Crea la restricción de integridad referencial. (No puedes poner un publisher_id = 99 si la editorial 99 no existe en la tabla de editoriales).

**Ejemplo práctico de conexión entre tablas (PK y FK)**
  Tabla: Editoriales (publishers)

  La Primary Key (PK) es id, identifica de forma única a cada editorial.

  | id (PK) | nombre      |
  |---------|-------------|
  | 5       | Salamandra  |
  | 6       | Anagrama    |

  Tabla: Libros (books)

  Aquí id es la Primary Key (PK) y publisher_id es la Foreign Key (FK) que apunta a publishers.id.

  | id (PK) | titulo          | publisher_id (FK) |
  |---------|-----------------|-------------------|
  | 101     | Harry Potter    | 5                 |
  | 102     | El Principito   | 5                 |
  | 103     | Seda            | 6                 |

  - Interpretación (cómo lo entiende SQL)
  - El libro con id = 101 tiene publisher_id = 5
  - SQL va a la tabla publishers
  - Busca id = 5
  - Encuentra Salamandra
  - Conclusión: “Harry Potter es de la editorial Salamandra”

  > El número 5 es el pegamento que conecta ambas tablas.

## Subconsultas (Subqueries)

Son consultas anidadas (una dentro de otra). Se usan cuando necesitas un dato de otra tabla para completar tu filtro.

Sintaxis Básica
La consulta interna (entre paréntesis) se ejecuta primero.

Ejemplo: Encontrar todos los libros publicados por "Fitzcarraldo Editions".

- Primero necesitamos el ID de la editorial.
- Luego usamos ese ID para buscar los libros.


```sql
SELECT title 
FROM books 
WHERE publisher_id = (
    SELECT id 
    FROM publishers 
    WHERE publisher = 'Fitzcarraldo Editions'
);
```
### El Operador IN

Si la subconsulta devuelve más de un resultado (ej. un autor con múltiples IDs o múltiples libros), no podemos usar =, debemos usar IN.

Ejemplo: Encontrar libros de la autora "Fernanda Melchor".

```sql
SELECT title 
FROM books 
WHERE id IN (
    SELECT book_id 
    FROM authored 
    WHERE author_id = (
        SELECT id 
        FROM authors 
        WHERE name = 'Fernanda Melchor'
    )
);
```
> Consejo de estilo: Es recomendable indentar las subconsultas para facilitar la lectura.

## JOINs (Uniones de Tablas)

Las subconsultas son útiles, pero JOIN permite combinar filas de dos o más tablas en una sola tabla de resultados temporal.

Sintaxis JOIN (o INNER JOIN)
Combina tablas basándose en una columna común. Solo muestra filas donde hay coincidencia en ambas tablas.

Ejemplo (Base de datos de Leones Marinos): Queremos ver qué leones marinos (sea_lions) tenemos rastreados en la tabla de migraciones (migrations).


```sql
SELECT * FROM sea_lions
JOIN migrations ON migrations.id = sea_lions.id;
```

Tipos de JOIN

- INNER JOIN (Por defecto): Excluye filas que no tienen pareja en la otra tabla (ej. un león marino sin datos de migración desaparece del resultado).

- LEFT JOIN: Muestra todas las filas de la tabla izquierda (la primera mencionada), aunque no tengan coincidencia en la derecha (rellena con NULL).

```
SELECT * FROM sea_lions LEFT JOIN migrations ON ...
```

- RIGHT JOIN: Muestra todas las filas de la tabla derecha, aunque no tengan coincidencia en la izquierda.

- FULL JOIN: Muestra todo de ambas tablas, rellenando con NULL donde falten datos.

- NATURAL JOIN
  Es un atajo. Si las dos tablas tienen una columna con el mismo nombre (ej. ambas tienen una columna id), SQL las une automáticamente sin que escribas la cláusula ON.

```sql
SELECT * FROM sea_lions NATURAL JOIN migrations;
```

## Operaciones de Conjuntos (Sets)

SQL permite tratar los resultados como conjuntos matemáticos (Diagramas de Venn). Requisito: Las tablas deben tener el mismo número y tipo de columnas.

- UNION (O): Combina los resultados de dos consultas (A + B). Elimina duplicados.

    >Ejemplo: Autores O Traductores.

- INTERSECT (Y): Devuelve solo los elementos que aparecen en ambas consultas.

    >Ejemplo: Personas que son Autores Y TAMBIÉN Traductores.

- EXCEPT (Menos): Devuelve los elementos de la primera consulta menos los de la segunda.

    >Ejemplo: Autores que NO son Traductores.

Ejemplo de sintaxis:

```sql
SELECT name FROM authors
INTERSECT
SELECT name FROM translators;
```

## Agrupamiento (Groups)

A veces queremos calcular estadísticas por categorías, no de toda la tabla.

GROUP BY

Agrupa filas que tienen el mismo valor en una columna especificada para aplicar funciones de agregación (AVG, COUNT, MAX, etc.).

Ejemplo: Calcular el promedio de calificación (rating) por cada libro.
```
SELECT book_id, AVG(rating) 
FROM ratings 
GROUP BY book_id;
```
HAVING
Es el equivalente a WHERE, pero para grupos.

- WHERE: Filtra filas individuales antes de agrupar.

- HAVING: Filtra grupos después de agrupar.

Ejemplo: Mostrar solo los libros con un promedio de calificación mayor a 4.0.


```sql
SELECT book_id, AVG(rating) 
FROM ratings 
GROUP BY book_id 
HAVING AVG(rating) > 4.0;
```

## Diseño de Bases de Datos Relacionales

El diseño de bases de datos es el proceso de definir la estructura lógica (el "plano" o *schema*) que organizará los datos. El objetivo es asegurar la integridad, reducir la redundancia y facilitar el acceso a la información.

### Normalización de Datos

La normalización es el proceso de organizar los datos en tablas para minimizar la redundancia y evitar anomalías en la inserción, actualización o borrado de datos.

#### El Problema de la Tabla Única (Desnormalización)
Almacenar toda la información en una sola tabla provoca duplicidad de datos y dificulta el mantenimiento.

**Ejemplo de mal diseño (Sistema de Metro):**
Imagina una tabla única `movimientos_metro` que registra cada vez que alguien pasa una tarjeta.

| Pasajero | Estación | Línea | Acción | Saldo |
| :--- | :--- | :--- | :--- | :--- |
| Juan Pérez | Central | Roja | Entrar | 2.50 |
| Ana Gómez | Norte | Verde | Salir | 5.00 |
| Juan Pérez | Sur | Azul | Entrar | 1.00 |

* **Redundancia:** El nombre "Juan Pérez" se repite. Si Juan cambia su nombre, hay que actualizar múltiples filas.

* **Ambigüedad:** Si hay dos "Juan Pérez", no podemos distinguirlos.

* **Ineficiencia:** Guardamos el nombre de la línea ("Roja") cada vez que alguien entra en la estación "Central", gastando espacio innecesariamente.

#### La Solución: Entidades y Relaciones
Separamos los conceptos en **Entidades** (tablas propias):

1.  **Pasajeros (Riders):** Información única de la persona.

2.  **Estaciones (Stations):** Información única del lugar.

3.  **Transacciones (Visits/Swipes):** El evento que conecta a ambos.

---

### Tipos de Datos en SQLite: Clases y Afinidades

SQLite es único porque utiliza un sistema de **Tipado Dinámico**. A diferencia de otros motores SQL (como PostgreSQL o MySQL) donde el tipo de dato se fija rígidamente en la columna, SQLite se fija en el valor en sí mismo, utilizando dos conceptos clave:

#### A. Clases de Almacenamiento (Storage Classes)
Es cómo se guardan físicamente los datos en el disco duro. SQLite solo tiene 5 clases fundamentales:

1.  **NULL:** Representa la ausencia de valor.
2.  **INTEGER:** Números enteros con signo (1, 2, -50). Se usan para IDs, contadores o cantidades exactas. Ocupan 1, 2, 3, 4, 6, u 8 bytes dependiendo de la magnitud.
3.  **REAL:** Números de punto flotante IEEE (decimales como 3.14, -0.01). Se usan para mediciones físicas.
4.  **TEXT:** Cadenas de caracteres (UTF-8, UTF-16). Nombres, descripciones.
5.  **BLOB (Binary Large Object):** Datos binarios almacenados tal cual se introducen (imágenes, archivos, audio).

#### B. Afinidades de Tipo (Type Affinity)
Cuando creas una tabla (`CREATE TABLE`), defines un tipo para la columna. SQLite mapea ese tipo declarado a una de las 5 "Afinidades". La afinidad es la **preferencia** de la columna sobre cómo guardar el dato.

Las 5 Afinidades son:

1.  **TEXT:** Preferida para columnas declaradas como `VARCHAR`, `TEXT`, `CLOB`.
    * *Comportamiento:* Si intentas guardar un número, lo convierte a texto.
2.  **NUMERIC:** Preferida para columnas declaradas como `NUMERIC`, `DECIMAL`, `DATE`.
    * *Comportamiento:* Intenta convertir texto a INTEGER o REAL si es posible. Si insertas `'10.5'`, lo guarda como número `10.5`. Si insertas `'hola'`, lo guarda como texto.
3.  **INTEGER:** Preferida para columnas declaradas como `INT`, `INTEGER`, `BIGINT`.
    * *Comportamiento:* Se comporta como NUMERIC, pero si el número tiene decimales y cabe en un entero (ej: `10.0`), lo convierte a entero (`10`).
4.  **REAL:** Preferida para columnas declaradas como `REAL`, `DOUBLE`, `FLOAT`.
    * *Comportamiento:* Fuerza la conversión a punto flotante.
5.  **BLOB:** Preferida para columnas declaradas como `BLOB` o si no se especifica tipo.
    * *Comportamiento:* No intenta convertir nada. Guarda el dato tal cual llega.

**Ejemplo de Conversión (Coerción de Tipos):**

* Si tienes una columna con afinidad **INTEGER**.

* Insertas el texto `'150'`.

* SQLite detecta que parece un número -> Lo convierte y lo guarda como el número `150` (Clase INTEGER).

* Si insertas `'Cien'`, no puede convertirlo -> Lo guarda como `'Cien'` (Clase TEXT).

> **Consejo Pro:** Para datos monetarios, se recomienda usar **INTEGER** (almacenando centavos) en lugar de **REAL** para evitar errores de redondeo de punto flotante.

---

### Data Definition Language (DDL)

DDL son los comandos SQL utilizados para definir y modificar la estructura de la base de datos.

#### Crear Tablas (CREATE TABLE)
Define el nombre de la tabla, sus columnas y los tipos de datos preferidos.

```sql
CREATE TABLE riders (
    id INTEGER,
    name TEXT
);
```

#### Modificar Tablas (ALTER TABLE)

Permite cambiar la estructura de una tabla existente sin perder sus datos.
```sql

-- Renombrar tabla:

ALTER TABLE riders RENAME TO passengers;

--Añadir columna:

ALTER TABLE passengers ADD COLUMN email TEXT;

--Renombrar columna:

ALTER TABLE passengers RENAME COLUMN name TO full_name;

--Eliminar columna:

ALTER TABLE passengers DROP COLUMN email;
```

#### Eliminar tabla (DROP TABLE)

Eliminar Tablas (DROP TABLE)
Borra la tabla y todos sus datos permanentemente.

```sql
DROP TABLE passengers;
```

## Restricciones de Integridad (Constraints)

Las restricciones son reglas que la base de datos impone para garantizar la validez, precisión y consistencia de los datos.

### Restricciones de Tabla (Identidad y Relación)

**PRIMARY KEY (Clave Primaria):**

  * Columna (o grupo de columnas) que identifica de forma **única** a cada fila de la tabla.

  * Implica `UNIQUE` y `NOT NULL`.

  * Es la base para buscar registros rápidamente.

  * **Convención:** Usar una columna `id INTEGER PRIMARY KEY`.

**FOREIGN KEY (Clave Foránea):**

  * Columna que crea un vínculo con la `PRIMARY KEY` de otra tabla.

  * Garantiza la **Integridad Referencial**: no puedes tener una transacción de una tarjeta que no existe en la tabla de tarjetas.

### Restricciones de Columna (Reglas de Datos)

**NOT NULL:**

  * Prohíbe que el valor sea nulo. Obligatorio tener dato.

  * *Ejemplo:* Una estación debe tener nombre.

**UNIQUE:**

  * Asegura que todos los valores de la columna sean diferentes entre sí.

  * *Ejemplo:* No pueden existir dos tarjetas con el mismo número de serie.

**DEFAULT:**

  * Asigna un valor predeterminado si no se especifica uno al insertar.

  * *Ejemplo:* `DEFAULT CURRENT_TIMESTAMP` para guardar la hora exacta del registro automáticamente.

**CHECK:**

  * Permite definir una condición lógica personalizada que debe cumplirse. Es muy potente para validar reglas de negocio.

  * *Ejemplo:* El saldo no puede ser negativo (`amount >= 0`).

  * *Ejemplo:* El tipo de transacción solo puede ser uno de una lista (`IN ('enter', 'exit')`).


## Ejemplo Práctico de Diseño Completo

(Caso: Tarjeta de Transporte)

A continuación, un esquema robusto que aplica todos los conceptos anteriores para un sistema de tarjetas de metro (como la "CharlieCard").

* Entidad 1: Tarjetas (Cards)
Identificamos a los usuarios por su tarjeta, no por su nombre (anonimato/seguridad).

```sql
CREATE TABLE cards (
    id INTEGER PRIMARY KEY
);
```

* Entidad 2: Estaciones (Stations)
Catálogo de lugares. El nombre debe ser único para evitar confusiones.

```sql
CREATE TABLE stations (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL UNIQUE,
    line TEXT NOT NULL
);
```

+ Entidad 3: Transacciones (Swipes) - La tabla "Asociativa"
Esta tabla conecta Tarjetas con Estaciones y registra eventos. Implementa múltiples restricciones para asegurar la calidad del dato.

```sql
CREATE TABLE swipes (
    id INTEGER PRIMARY KEY,
    
    -- Claves Foráneas: Conexión con las otras tablas
    card_id INTEGER,
    station_id INTEGER,
    
    -- Tipo de transacción validada con CHECK
    type TEXT NOT NULL CHECK(type IN ('enter', 'exit', 'deposit')),
    
    -- Fecha y hora automática
    datetime NUMERIC NOT NULL DEFAULT CURRENT_TIMESTAMP,
    
    -- Dinero manejado (no permitimos transacciones de valor 0)
    amount NUMERIC NOT NULL CHECK(amount != 0),
    
    -- Definición explícita de las relaciones
    FOREIGN KEY(card_id) REFERENCES cards(id),
    FOREIGN KEY(station_id) REFERENCES stations(id)
);
```

## Modelado de Relaciones

Al diseñar, debemos identificar cómo interactúan las entidades:

**Uno a Uno (1:1):**

  * Una fila en la tabla A corresponde a una única fila en la tabla B.

  * *Nota:* Es poco común; normalmente si tienen relación 1:1 se suelen unir los datos en una sola tabla.

**Uno a Muchos (1:M):**

  * Una fila en la tabla A se relaciona con muchas filas en la tabla B.

  * *Ejemplo:* Una **Línea** de metro tiene muchas **Estaciones**.

  * *Implementación:* La `FOREIGN KEY` se coloca en la tabla del lado "Muchos" (la estación tendría una columna `line_id`).

**Muchos a Muchos (M:N):**

  * Muchas filas en la tabla A se relacionan con muchas filas en la tabla B.

  * *Ejemplo:* Un **Pasajero** visita muchas **Estaciones**, y una **Estación** recibe muchos **Pasajeros**.

  * *Implementación:* Se requiere crear una **Tabla Intermedia** (como la tabla `swipes` del ejemplo anterior) que contenga las claves foráneas de ambas entidades para unirlas.
