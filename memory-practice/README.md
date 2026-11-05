# Práctica de memoria

Los ejercicios de memoria **de la guía y de parciales**, resueltos paso a paso: la
dirección partida en bits, la segmentación con su límite y sus permisos, la tabla de
segmentos reconstruida desde pistas, la memoria virtual y el bit de presencia, los
algoritmos de sustitución, la TLB, y siete ejercicios de parcial.

Es el recurso de **práctica**: hace las cuentas. Los mecanismos —qué es una página,
un marco, la TLB, el bit de presencia, cómo elige cada algoritmo— se explican en el
recurso de teoría, **Memoria**, que va antes. Acá se usan; cuando hace falta
nombrarlos, la narración lo dice en una frase y remite a **Memoria**.

Se abre haciendo doble clic en `index.html`. No necesita internet ni instalar
nada.

Este archivo es la documentación del recurso: qué enseña, de dónde salió cada
ejemplo, cómo editarlo y qué no se puede tocar. Si vas a modificar `index.html`,
leelo antes.

---

## Los veintitrés ejemplos

Son **250 pasos** en modo Ver, **92 referencias** para contestar en modo Resolver y
**47 preguntas**. La escalera va agrupada en seis tramos.

| # | Tramo | Ejemplo | De dónde viene | Antes estaba en |
|---|---|---|---|---|
| 1 | Guía · paginación | La dirección partida en dos | Guía, ej. 1 | Paginación y segmentación, ej. 1 |
| 2 | Guía · paginación | Lo que entra y lo que sobra | Guía, ej. 2 | Paginación y segmentación, ej. 3 |
| 3 | Guía · segmentación | Base y límite | Guía, ej. 3 | Paginación y segmentación, ej. 4 |
| 4 | Guía · segmentación | Permisos | Guía, ej. 4 | Paginación y segmentación, ej. 5 |
| 5 | Guía · segmentación | La tabla al revés | Guía, ej. 11 | Paginación y segmentación, ej. 7 |
| 6 | Guía · segmentación | Los huecos | Guía, ej. 11 c | Paginación y segmentación, ej. 6 |
| 7 | Guía · memoria virtual | Más grande que la memoria | Guía, ej. 5 y 6 a | Paginación y segmentación, ej. 9 |
| 8 | Guía · memoria virtual | Una tabla por proceso | Guía, ej. 6 b, 6 c y 8 b | Paginación y segmentación, ej. 10 |
| 9 | Guía · memoria virtual | La página que no está | Guía, ej. 7 | Paginación y segmentación, ej. 11 |
| 10 | Guía · sustitución | FIFO | Guía, ej. 8 | Sustitución de páginas, ej. 1 |
| 11 | Guía · sustitución | Óptimo | Guía, ej. 8 | Sustitución de páginas, ej. 2 |
| 12 | Guía · sustitución | LRU | Guía, ej. 8 | Sustitución de páginas, ej. 3 |
| 13 | Guía · sustitución | Clock | Guía, ej. 8 | Sustitución de páginas, ej. 4 |
| 14 | Guía · sustitución | LRU con páginas modificadas | Guía, ej. 9 | Sustitución de páginas, ej. 6 |
| 15 | Guía · sustitución | Clock modificado | Guía, ej. 9 | Sustitución de páginas, ej. 7 |
| 16 | Guía · TLB | Cuándo sirve la TLB | Guía, ej. 10 | Sustitución de páginas, ej. 8 |
| 17 | De parcial | El tamaño máximo de un proceso | Parcial 2C2022, TM | Práctica avanzada, ej. 5 |
| 18 | De parcial | Segmentación paginada, para ejecutar | 1er recuperatorio 2C2025 | Práctica avanzada, ej. 2 |
| 19 | De parcial | LRU con tablas de dos niveles | Parcial 1C2022, TM | Práctica avanzada, ej. 6 |
| 20 | De parcial | Cuando la tabla tampoco está | 2do parcial 2C2022 | Práctica avanzada, ej. 7 |
| 21 | De parcial | TLB con procesos y bitmap | Parcial 2C2018, TT | Práctica avanzada, ej. 8 |
| 22 | De parcial | ¿Qué algoritmo fue? | Guía de parciales, ej. 2 | Práctica avanzada, ej. 3 |
| 23 | De parcial | Clock modificado, global | 2do parcial 1C2025, TM | Práctica avanzada, ej. 4 |

Cada ejemplo dice de dónde viene en el campo `source`, que el recurso exige.

### De dónde salió el recurso

Junta tres recursos que se habían armado por separado y nunca se publicaron:

- **Paginación y segmentación** (`memory-translation`, del fork de Lucho): sus
  ejemplos de la guía quedaron acá, del 1 al 9. Los de teoría —de página a marco,
  segmentación paginada, el fallo de página, las tablas grandes— pasaron a
  **Memoria**.
- **Sustitución de páginas** (`page-replacement`, del fork de Lucho): sus ejemplos
  de la guía quedaron acá, del 10 al 16, con el modo Resolver. La anomalía de Belady
  pasó a **Memoria**, como cierre de FIFO.
- **Práctica avanzada de memoria** (este mismo recurso, en `main`): siete de sus
  ocho ejercicios quedaron acá, del 17 al 23. El que salió es «La tabla al revés» de
  ese recurso, porque los ejemplos 5 y 6 ya resuelven el ejercicio 11 de la guía.

Las carpetas `memory-translation` y `page-replacement` no existen en `main`: fueron
materia prima.

**Las resoluciones no cambiaron.** Al portar, se comparó cada paso de cada ejemplo
contra su recurso de origen —lo que da cada traducción, cada panel, cada pregunta,
cada referencia de sustitución, cada estado de marcos y cada respuesta correcta— y
no hay ninguna diferencia. Lo único que cambió son las frases de la sección
siguiente.

### Lo que se ajustó al portar

- **Las definiciones se achicaron a una frase** cuando el concepto lo explica
  **Memoria**: qué es paginar (ej. 1), qué es un segmento (ej. 3), la
  fragmentación externa (ej. 6), la memoria virtual (ej. 7), el bit de presencia
  (ej. 8), cómo se atiende un fallo de página (ej. 9), y cómo elige cada algoritmo
  en las presentaciones de los ejemplos 10 a 15. Donde hace falta, la frase remite a
  **Memoria**.
- **«alcance local» pasó a «sustitución local»**, que es como lo dice el apunte.
- **«frames» pasó a «marcos»** en el ejemplo 17.
- **Se sacaron todas las menciones** a «Paginación y segmentación» y a
  «Sustitución de páginas», y las referencias a ejemplos que ya no están acá.
- La frase de Clock modificado decía «el bit de uso de el marco»; ahora dice «del».
- **Cinco frases ganaron una cláusula** porque en ese paso un panel sale de la
  pantalla, y la regla del repositorio pide decirlo: «La memoria sale de la pantalla.»
  (ej. 5, paso 8), «Las cuentas salen de la pantalla.» y «Las tablas salen de la
  pantalla.» (ej. 20, pasos 3 y 11), «El bitmap sale de la pantalla.» (ej. 21, paso
  13), «Las direcciones salen de la pantalla.» (ej. 22, paso 3) y «La dirección sale
  de la pantalla.» (ej. 23, paso 13).
- **Tres errores de los recursos de origen, corregidos**: el ejemplo 2 decía 35
  marcos libres y son 38; en el 23, la tabla de B seguía con U = 1 para la B2
  después de que la segunda vuelta de Clock modificado lo bajara, y las dos búsquedas
  de víctima (pasos 6 y 11) dejaban las tablas un paso atrás de los marcos; ahora las
  tablas acompañan a los marcos en cada paso. Y en Resolver las teclas elegían el
  marco por posición: en el 14 y el 15, cuyos marcos son 1, 3, 5 y 8, el `3` respondía
  «marco 5». Ahora cada tecla elige el marco con ese número.
- **Los números de cinco cifras o más llevan punto** en la narración de los ejemplos
  18, 19 y 23, como ya lo llevaban sus paneles.

---

## Dos modos, sólo en los ejemplos 10 a 16

Los ejemplos de sustitución y de TLB tienen **Modo · Resolver / Ver**, en el renglón
del título, a la derecha. En los demás ejemplos el selector no aparece, y su lugar
queda reservado: **nada se mueve** cuando aparece o desaparece.

- **Resolver**: el estudiante llena la tabla de marcos referencia por referencia,
  tocando el marco donde queda la página, o escribiendo su número. En el 14 y el
  15 está además el botón «Dirección inválida»; en el 16 se elige entre acierto en la
  TLB, fallo de TLB con la página en memoria, y fallo de página. La columna se revela
  con la respuesta correcta se haya acertado o no, y la narración explica por qué.
  Los contadores y el panel de estado muestran siempre lo que había **antes** de la
  referencia abierta.
- **Ver**: un paso es una referencia, con la tabla revelada hasta ahí.

Cuando un ejemplo de sustitución está resuelto entero, su botón de la escalera
muestra un ✓. **El lugar del ✓ está reservado en los veintitrés botones**, para
que la escalera tenga una sola disposición; sólo se llena en los ejemplos con modo
Resolver.

En el panel de estado de FIFO, Óptimo, LRU y los ejemplos 14 y 16 hay un interruptor
**Pistas · Mostrar / Ocultar**, sólo en Resolver: ocultarlas saca el dato con el que
decide el algoritmo, que hay que llevar de memoria.

## Las preguntas

Cuarenta y siete pasos traen una pregunta antes de mostrar el resultado. Cada opción
incorrecta es un error típico con su explicación, y **nunca bloquean**: se puede
seguir sin contestar.

**Las respuestas se guardan en el navegador** (`localStorage`, clave
`ssoo-recursos:memory-practice:v2`), las de las preguntas y las de las tablas de
Resolver. La clave cambió de `v1` a `v2` porque cambió el conjunto de ejemplos; lo
que haya guardado en `v1` se ignora. Si el navegador no deja guardar, todo funciona
igual y sólo se olvida al recargar.

---

## Lo que el recurso verifica solo al abrirse

**No son comentarios: son campos que el código chequea**, y si algo no da lo dice la
consola del navegador.

- **Las traducciones las hace el recurso**: los ejemplos 3, 4, 9 y 18 declaran su
  máquina (paginación, segmentación o segmentación paginada) y cada paso dice sólo
  qué dirección y hasta dónde se tradujo. Cada `expect` se compara con la cuenta.
- **Las sustituciones las simula el recurso, con un solo simulador.** Los ejemplos 10
  a 16 se simulan enteros —fallos, escrituras, víctimas en orden, accesos a TLB,
  memoria y disco— y se comparan con su `expect`. Los `facts` de los ejemplos 19, 22 y
  23 corren **el mismo simulador**: el recurso tenía dos y ahora tiene uno.
- Que cada dirección dé la página y el desplazamiento que dice el dato; que los
  campos de bits, pegados, den la dirección; que cada fila de una tabla tenga tantas
  celdas como columnas; que dos celdas marcadas no se toquen; que cada pregunta tenga
  su opción correcta y un `why` en cada opción; que cada ejemplo tenga `source`,
  `closing` y `stepUnit`; que no quede ningún `{dato}` sin completar; que todo
  globito invocado exista y que todo globito escrito se use; que los tramos de la
  escalera estén contiguos.

## El chasis se comparte; el canvas se deriva

El chasis es el de **Deadlock**: la escalera de números con los tramos que no se
parten, los controles y las teclas, el globito en cada título de panel, la placa de
cierre en cada ejemplo, un solo renglón para las elecciones y el favicon de todo el
repositorio. Se comparó con Deadlock propiedad por propiedad y no hay diferencias.

El canvas de cada ejemplo es el de su recurso de origen, con su derivación, **salvo
en siete ejemplos que hubo que reacomodar** para que entren a 1280×720 con la escalera
en dos renglones (los recursos de origen la tenían en uno):

| # | Qué cambió |
|---|---|
| 5 | la tabla de segmentos va arriba de la memoria, en la tercera columna; desde el paso 8, la memoria —que ya no cambia— sale |
| 18 | columnas de ancho fijo para las dos tablas, para que la dirección entre en un renglón |
| 19 | tres columnas: referencias, marcos y cuentas, en lugar de marcos y cuentas apilados |
| 20 | desde el paso 3 salen las cuentas, y vuelven en el 11, cuando salen las tablas |
| 21 | en el paso 13 sale el bitmap, que ya no cambia |
| 22 | desde el paso 3 salen las direcciones: lo que sigue es probar algoritmos |
| 23 | en el último paso sale la dirección |

Cada vez que un panel sale, la narración lo dice una vez. El motor de pasos acepta
ahora un `canvas` por paso, que sigue valiendo hasta el próximo que declare otro.

Tres ajustes de dibujo valen para los dos recursos de memoria: la tira de la memoria
toma el alto que necesitan sus bloques (22 px cada uno) en lugar de un mínimo fijo;
las tablas de sustitución tienen los casilleros un poco más bajos; y en la caja de la
pregunta, «Probar otra» va arriba a la derecha y no en un renglón propio.

**El renglón de elecciones comparte línea con el título.** Con veintitrés ejemplos
la escalera ocupa dos renglones; un renglón más abajo, reservado para que nada se
mueva, dejaba el área de los paneles 34 px más baja que en Deadlock. En la línea del
título no cuesta nada.

## Cambiar un texto

Todos los textos que ve un estudiante están en el bloque `<script id="data">`,
arriba de todo del archivo.

1. Abrí `index.html` en cualquier editor.
2. Buscá el texto tal como aparece en pantalla.
3. Cambialo, guardá y recargá.

Dentro de los textos: `[[fallo:fallo de página]]` sale con un globito (`TOOLTIPS`),
`*así*` sale en negrita, y `{fisica}`, `{marco}`, `{fallos_12}` son valores que
completa el recurso —**no los reemplaces por el número escrito a mano**—. La
narración de cada referencia de los ejemplos 10 a 16 se arma con las plantillas de
`UI_TEXT.narr`: cambiar una frase ahí la cambia en todas las referencias.

Si cambiás un número de un ejemplo con máquina, `expect` o `facts`, la consola avisa
si la cuenta ya no da: hay que cambiar las dos cosas.

## Agregar un ejemplo

- **De pasos**: un objeto en `EXAMPLES` con `number`, `group`, `source`, `title`,
  `short`, `adds`, `subtitle`, `stepUnit`, `canvas`, `closing` y `steps`; cada paso
  declara su `narration` y lo que cambia en cada panel (`views`).
- **De sustitución**: un objeto con `algorithm` (`fifo`, `optimal`, `lru`, `clock`,
  `clockModified` o `tlb`), `frames`, `references`, `expect`, `intro` y `closing`. El
  modo Resolver aparece solo.

---

## Hasta dónde llega

Hace las cuentas de la guía y de los parciales y no más. No re-explica mecanismos:
para eso está **Memoria**. No se dice nunca qué entra o qué no entra en el parcial.

El ejemplo 16 es el ejercicio 10 de la guía y necesita una TLB de 4 entradas que se
reemplaza con FIFO. El planteo de esta versión pedía que el tamaño y el reemplazo de
la TLB no aparecieran en ninguno de los dos recursos; acá aparecen porque el
ejercicio no se puede resolver sin ellos.

### Términos

La forma principal es **«fallo de página»**, como en los tres recursos de origen; el
globito dice que en inglés es *page fault*. **Memoria**, en cambio, usa «page fault»,
que es como titula la filmina la atención. Es una de las decisiones de abajo.

### Colores

No estrena nada. Los colores de rol son los de los recursos de origen, que ya
coincidían: página violeta `#6d28d9`, desplazamiento verde azulado `#0b6a62`, marco
ocre `#9a4507`, segmento magenta `#be185d`, 1er nivel índigo `#4338ca`, y lo que
cambió en el paso en grafito.

**Los fallos y las víctimas se marcan con tinta, no con rojo**: el rojo es
`BLOCKED`. En **Sustitución de páginas** el fallo era rojo y se cambió al portar. En
las tablas de sustitución el fallo sigue siendo la letra **F**, en tinta, y no un
`✗`: en esas tablas el `✗` ya quiere decir «respuesta incorrecta», en la fila «Tu
respuesta», y un símbolo no puede decir dos cosas.

---

## Decisiones que conviene confirmar con la cátedra

**Nuevas, de esta versión:**

1. **Re-ejecución.** ¿Los accesos a tablas se cuentan otra vez cuando la instrucción
   se re-ejecuta después de un fallo? Los ejemplos 16 y 20 cuentan una sola vez.
2. **Orden de los controles en segmentación**: límite antes que permiso. El ejemplo 4
   (la 0BEEFh de la guía) depende de eso: da segmentation fault y no error de
   protección.
3. **Empates en Óptimo**: si varias páginas no vuelven a usarse, sale la que entró
   primero. Es la regla que reproduce la tabla de la filmina; reemplaza la de
   **Sustitución de páginas** («la del marco de más arriba»). Con los datos de la guía
   no hay empates, y ninguna resolución cambió. Aplicada; falta confirmar.
4. **Fallos en tinta, no en rojo.** Aplicado; falta confirmar.
5. **«fallo de página» o «page fault»** como forma principal de cada recurso.

**Que venían de Paginación y segmentación:**

6. **Ejercicio 6 b, «64Kb».** Se leyó como 64 KiB: 128 marcos de 0,5 KiB, 7 bits de
   marco y 16 bytes de tabla de marcos libres. Si eran kilobits, cambian las tres
   cuentas (ejemplo 8).
7. **Ejercicio 6 b, el formato de la fila.** La guía dice «proponga». Se propone
   presencia + marco, 8 bits; para el 8 b, R, W, X, uso, modificado, presencia y 5 bits
   de marco: 11 bits (ejemplo 8).
8. **Ejercicio 11 b.** El código es «posiblemente» el segmento 1, como dice la guía;
   no se distingue cuál de los otros dos es la pila y cuál los datos (ejemplo 5).

**Que venían de Sustitución de páginas:**

9. **La dirección 172100 del ejercicio 9** cae en la página 21 de un proceso de 20
   páginas: se trata como dirección inválida, el proceso termina y la última
   referencia no se ejecuta (ejemplos 14 y 15).
10. **Cómo se cuentan los accesos del ejercicio 10**: acierto en la TLB, 1 a la TLB y
    1 a memoria; fallo de TLB con la página en memoria, 1 y 2; fallo de página, 1, 2 y
    1 a disco. P1 da 13 / 18 / 5 y P2, 9 / 18 / 9 (ejemplo 16).
11. **Convenciones de Clock**: el bit de uso se pone en 1 al cargar y al referenciar;
    en un acierto el puntero no se mueve; en un fallo queda en el marco siguiente al
    reemplazado; cargar en un marco libre también lo avanza (ejemplo 13).
12. **Clock modificado en vueltas**: (U = 0, M = 0) sin tocar nada, después
    (U = 0, M = 1) bajando los bits de uso, y se repite (ejemplo 15).
13. **Con marcos libres, la página va al primero libre** (al del puntero, en Clock).
14. **LRU del ejercicio 9 decide con los instantes del enunciado** (28, 3 y 15), y
    toda referencia nueva cuenta como posterior a ellos (ejemplo 14).

**Que venían de la Práctica avanzada:**

15. **Ejemplo 21 b, la fila 3 del 1er nivel con P = 0**: se lee como «la tabla de 2do
    nivel no está en memoria», con un fallo para traerla y otro para la página.
16. **Ejemplo 21, decimal o hexa**: las tablas se leen en decimal.
17. **Ejemplo 19, accesos a disco**: 7 = 5 lecturas + 2 escrituras. Si la cátedra
    cuenta sólo los fallos, son 5.
18. **Ejemplo 23, las filas que salen de memoria** quedan con P = 0, U = 0 y M = 0, y
    el número de marco escrito como estaba.
19. **Ejemplo 22, Clock**: «puede ser» supone que el puntero recorre los marcos en
    orden —3, 4, 7— y arranca en el 3.
20. **Ejemplo 17**: el enunciado está recortado y sólo se ve la consigna a.

---

## Verificación antes de publicar un cambio

1. Abrirlo con doble clic, sin servidor y sin internet.
2. La consola limpia, en los veintitrés ejemplos, las variantes y los dos modos.
3. Recorrer cada ejemplo para adelante y para atrás: cada paso igual en los dos
   sentidos.
4. Resolver los ejemplos 10 a 16 enteros, acertando y equivocándose.
5. A 1280×720, nada scrollea: ni la página ni ningún panel.
6. A 390 px, una columna y nada se va para el costado; y entre 1001 y 1239 px, nada
   se va para el costado.
7. Con el almacenamiento bloqueado, las preguntas y las tablas funcionan igual.

Lo medido al armar esta versión, con un Chromium automatizado:

- **Las resoluciones** de los veintitrés ejemplos, paso por paso, idénticas a las de
  su recurso de origen.
- **250 pasos** de ida y de vuelta, todos idénticos; consola limpia y ningún pedido
  de red; lo mismo con el almacenamiento bloqueado.
- **492 tableros** (todos los pasos, los dos modos, las 92 referencias de Resolver y
  cada opción de cada pregunta contestada) a 320, 390, 768, 1001, 1100, 1239, 1440 y
  1920 px: nada se va para el costado ni desborda.
- **A 1280×720, ningún panel desborda** en ninguno de los 492 tableros, la página no
  scrollea, y la caja de la pregunta tampoco scrollea por dentro: mientras hay una
  pregunta abierta la narración llega hasta 176 px. En los recursos de origen la caja
  scrolleaba entre 16 y 52 px, en 57 tableros de la Práctica avanzada y en 54 de
  Paginación y segmentación.

---

## Lo que falta

**Toda la narración la escribió un agente y no la revisó nadie de la cátedra**: la de
los tres recursos de origen, más los ajustes de esta versión. Es lo primero que hay
que leer antes de mandar el link.
