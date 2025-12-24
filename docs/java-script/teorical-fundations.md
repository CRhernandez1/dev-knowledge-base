# Fundamentos teóricos del lenguaje: Cómo funciona JS por debajo

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
