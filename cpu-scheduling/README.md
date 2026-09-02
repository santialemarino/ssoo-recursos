# Planificación de procesos

El estudiante **arma el diagrama de Gantt instante por instante**: para cada
columna elige quién ocupa cada recurso —la CPU, cada dispositivo de I/O— y
confirma. El recurso le dice si acertó y, cuando no, le muestra la respuesta
correcta y por qué es esa. Es un ensayo del ejercicio de parcial, con la misma
tabla y el mismo dibujo.

Se abre haciendo doble clic en `index.html`. No necesita internet ni instalar
nada.

Es el tercer recurso de la familia de **Ciclo de instrucción e interrupciones** y
**Procesos e hilos**, y da por sabido todo lo de ese segundo: estados, PCB,
cambio de contexto, KLT y ULT, y los tres planificadores.

Este archivo es la documentación del recurso: qué enseña, cómo editarlo y qué no
se puede tocar. Si vas a modificar `index.html`, leelo antes.

---

## Los diecisiete ejemplos

Van en tres grupos, y así aparecen en la escalera de arriba.

| # | Ejemplo | Qué enseña |
|---|---|---|
| **Base** | | |
| 1 | Empecemos: FIFO | Cómo se lee el tablero, y que FIFO no desaloja |
| 2 | El dispositivo es uno solo | Que las I/O se serializan y que esperar el dispositivo no ocupa la CPU |
| 3 | Dos dispositivos | Que con dispositivos distintos las I/O se superponen |
| 4 | SJF sin desalojo | Que el criterio deja de ser el orden de llegada |
| 5 | SJF con desalojo | Que cada llegada vuelve a abrir la decisión |
| 6 | SJF con estimadores | Que se planifica con la estimación, y que la estimación puede errarle |
| 7 | HRRN | Que el tiempo de espera entra en el criterio |
| 8 | Round Robin | El quantum, y el orden de reinserción de RR |
| 9 | Virtual Round Robin | Ready+ y el quantum sobrante, y a costa de quién |
| 10 | Colas multinivel con feedback | Bajar de cola, subir de cola, y desalojar entre colas |
| **Hilos** | | |
| 11 | Hilos de kernel | Que la unidad planificable es el KLT, no el proceso |
| 12 | Cada nivel con su algoritmo | Que el SO y la biblioteca deciden por separado |
| 13 | Sin jacketing | Que la I/O de un ULT bloquea a todo su KLT, y lo que cuesta |
| 14 | Con jacketing | La misma tabla del 13: 7 instantes contra 5 |
| **Cierre** | | |
| 15 | Dos procesadores | Que dos CPU no destraban un KLT bloqueado; el jacketing sí |
| 16 | Grado de multiprogramación y swap | Los dos estados suspendidos y el planificador de mediano plazo |
| 17 | ¿Qué algoritmo era? | Reconocer la regla mirando un diagrama ya resuelto |

Cada ejemplo agrega **un solo concepto** y da por sabido todo lo anterior. Cada
uno termina con una frase de cierre, en letra grande, que es lo que el
estudiante se tiene que llevar. Los ejemplos son cortos a propósito: entre 5 y
13 instantes.

Los pares **4 y 5**, **8 y 9**, **13 y 14** comparten enunciado: la diferencia
entre los dos diagramas es toda la lección. El **15** hace lo mismo con dos
variantes adentro de un ejemplo.

---

## Dos modos sobre el mismo motor

- **Resolver** — el modo interactivo. Un **grupo de opciones por recurso**: una
  fila de botones con «nadie» y cada proceso o hilo que ya llegó. Se elige con un
  clic, sin desplegar nada, y **el botón elegido se pinta del color del estado que
  esa elección produce**: azul bajo CPU, porque tener la CPU es estar en RUNNING;
  rojo bajo el dispositivo, porque tener una I/O es estar en BLOCKED. En el 16 esa
  misma regla se vuelve interesante: el botón de «fuera de memoria» no tiene un
  color fijo, porque estar afuera no es un estado —es naranja si además tiene el
  dispositivo (BLOCKED SUSPENDED) y amarillo si no (READY SUSPENDED)—. No es una
  excepción: es la regla de siempre, que en los otros dieciséis ejemplos da siempre
  el mismo color porque ahí cada recurso corresponde a un estado y nada más.

  **Mientras elegís, la columna se dibuja sola.** Los casilleros del instante
  actual se van pintando con borde punteado —todavía sin confirmar— así que la
  decisión se ve donde importa, en el diagrama, y no sólo en el panel.

  **En el ejemplo 16 hay un selector más: «fuera de memoria».** El estudiante no
  elige el estado —eso sería copiar la respuesta—, elige **quién está afuera**, y de
  ahí sale todo lo demás: con la CPU es RUNNING; con el dispositivo es BLOCKED si
  está en memoria y BLOCKED SUSPENDED si no; sin nada es READY adentro y READY
  SUSPENDED afuera. Es la única forma de que el 16 sea un ejercicio y no una
  película: lo que enseña es justamente esa decisión, y la toma el planificador de
  mediano plazo.

  **Un proceso no puede estar en dos lados a la vez.** Si ponés a B en la CPU y
  después lo elegís para el dispositivo, B se mueve: deja la CPU, que vuelve a
  «nadie». No es un bloqueo con cartel de error, es que un proceso está en un solo
  estado. La misma invariante está verificada sobre las dieciocho trazas al abrir
  el archivo. Las combinaciones se bloquean por lo que es posible, no por comodidad:
  **el dispositivo y «fuera de memoria» conviven** —eso es exactamente BLOCKED
  SUSPENDED—, pero **la CPU y «fuera de memoria» no**, porque un proceso que ejecuta
  está en memoria por definición. Darle la CPU a un proceso suspendido lo devuelve a
  memoria, y sacarlo de memoria le saca la CPU.

  Se confirma la columna entera, y la validación es por recurso: si acertaste la
  CPU y erraste el dispositivo, sólo se marca el dispositivo. Al confirmar queda
  encendida **la opción correcta** y la que habías elegido queda tachada. Siempre
  se avanza; nunca queda trabado. Hay un contador discreto de aciertos.
- **Ver** — recorre la traza ya resuelta con los mismos paneles, para adelante y
  para atrás, con reversibilidad exacta. Es el modo para proyectar en clase y
  para repasar.

El botón **«Igual que el instante anterior»** completa todos los selectores con
la asignación del instante anterior. Los tramos largos sin cambios son comunes,
y ahí la decisión real es justamente que no cambia nada.

---

## El chasis se comparte; el canvas se deriva

Está también en `CLAUDE.md` porque vale para todo el repositorio.

**El chasis es fijo:** la escalera de ejemplos arriba, el panel de narración, el
contador, la barra de controles (primero / anterior / siguiente / último, más
reproducir en modo Ver), las teclas (flechas, `Inicio`, `Fin`), un tooltip en
cada título de panel, una tarjeta de cierre por ejemplo, una sola fila de
elección en el encabezado, y el favicon del repositorio.

**El canvas se deriva por ejemplo, y acá se nota en cuatro lugares:**

- **Cuántos selectores hay.** Uno por recurso, siempre. Casi todos los ejemplos
  tienen dos (CPU y dispositivo); el 3 tiene tres (CPU, Impresora, Router) y el
  15 tiene tres (CPU 1, CPU 2 y dispositivo).
- **Qué muestra el panel de estado.** Con un solo nivel de planificación,
  una cola de listos. Con dos niveles (12 a 15), la cola de KLT y además la lista
  de ULT listos de cada KLT. El 16 agrega qué hay en memoria.
- **Cuánto respira cada panel.** El acomodo de paneles y **el espaciado son los
  mismos en los diecisiete**. Hubo una versión en la que el panel de estado se
  apretaba solo cuando tenía muchos bloques, y estaba mal: se veía como si los
  ejemplos de dos niveles hubieran quedado sin arreglar al lado de los primeros. Lo
  único que se deriva es el alto de los casilleros del diagrama (más chico con
  cuatro filas) y el de la tabla del enunciado (más chico con más de tres). Todo
  sale de contar filas, nunca de una lista de ejemplos.
- **Cuántos bloques tiene el panel de estado.** Con dos niveles de planificación,
  las colas van **en un solo bloque** con una fila por nivel (`KLT`, `ULT · KLT1`,
  `ULT · KLT2`) en vez de un bloque por cola. No es sólo estética: tres bloques con
  su encabezado y su línea no entran, y son lo que obligaba a apretar el espaciado.
  El bloque de eventos ocupa **lo que quede de su fila**: si arranca fila propia se
  extiende a lo ancho, y si no, comparte fila con el de dispositivos.

  El 15 es el que más apretado queda —tres recursos, cinco opciones cada uno y
  cuatro filas de diagrama— y durante un tiempo tuvo un acomodo propio. Se sacó: lo
  que hacía falta eran 22 px, y aparecieron sin tocar el acomodo. El más grande de
  esos ahorros vale la pena entenderlo: al confirmar, el veredicto («era ULT1»)
  estaba **después** de los botones y empujaba la fila a dos renglones, 23 px de
  golpe. Ahora va **abajo de la etiqueta del recurso**, en una columna que ya
  existía y estaba vacía. Si volvés a moverlo al lado de los botones, el 15 se
  desborda de nuevo.
- **Qué pinta el diagrama.** En todos los ejemplos menos el 16, el diagrama es de
  **ocupación de recursos**: azul si tiene la CPU, rojo si tiene un dispositivo,
  gris si terminó, y vacío si no ocupa nada. El **16 es la excepción**: ahí el
  diagrama es una **banda de estados**, porque las transiciones son el tema.

El ejemplo 17 tiene su propio canvas: el enunciado se achica, el diagrama va
completo desde el principio, y cuando se responde la lista de opciones se va de
la pantalla y los descartes ocupan todo el ancho. Las opciones dejaron de ser el
tema; los descartes pasaron a serlo.

---

## Los paneles

1. **Enunciado.** La tabla de procesos con llegada y ráfagas, el algoritmo y los
   parámetros. Es la misma tabla que se da en el parcial. Las ráfagas ya
   consumidas se van tachando, como cuando se resuelve a mano.
2. **Diagrama.** Una fila por proceso —o por hilo, agrupado por KLT o por
   proceso— y una columna por instante. La columna de etiquetas mide lo que miden
   las etiquetas y nada más, así que el diagrama arranca pegado al borde del panel
   y los casilleros se quedan con todo el ancho que sobra. **El casillero que dice `n` es el
   intervalo de `n−1` a `n`**, igual que en la planilla de la cátedra: por eso el
   `0` está a la izquierda, pegado a las etiquetas. Abajo, la referencia de
   colores, que dice explícitamente que **CPU = RUNNING** y que **I/O = BLOCKED**.
3. **Estado del instante.** Cómo queda el sistema **después** de procesar las
   llegadas y los fines de I/O y **antes** de que el planificador elija. Por eso
   el que está por entrar a la CPU **aparece** en la cola en ese instante y ya no
   está en el siguiente: es el estado sobre el que hay que razonar. Cada entrada
   viene anotada con el número que decide (ráfaga restante, estimación, razón de
   respuesta, `Q`/`Q+`, `Q1`/`Q2`), salvo en FIFO y RR, donde no decide ningún
   número.
4. **Tu decisión.** Un grupo de opciones por recurso, el botón de confirmar y el
   de «igual que el anterior». El título del panel dice de qué instante a qué
   instante estás decidiendo. En modo Ver este panel no está.
5. **Narración.** La devolución de la última columna confirmada.

---

## Cambiar un texto

Todos los textos visibles están en el bloque `<script id="data">`, y ninguno está
escrito adentro del motor ni del render.

- `UI_TEXT` — rótulos, botones, títulos de panel, y las **plantillas** de eventos
  y de razones. Cambiar `UI_TEXT.reasons.fifo` cambia la frase en los diez
  instantes donde FIFO elige a alguien.
- `TOOLTIPS` — el glosario. Se enlaza desde cualquier texto con `[[clave:texto]]`.
- `PANEL_TIPS` — el tooltip de cada título de panel.
- `EXAMPLES[n].title`, `.subtitle`, `.closing` — el título, la bajada y la frase
  de cierre de cada ejemplo.
- `EXAMPLES[n].trace[t].reason` — la razón **escrita a mano** de un instante
  clave. Sólo existe donde vive el concepto del ejemplo; el resto se genera con
  las plantillas. Eso es lo que garantiza que la razón nunca contradiga la traza.

---

## Agregar o cambiar un ejemplo

Un ejemplo es un objeto de `EXAMPLES`. Lo mínimo:

```js
{
  number: 18, id: "A11", group: "base",
  title: "...", short: "...", subtitle: "...", closing: "...",
  algorithm: "fifo",
  table: { kind: "process", rows: [
    { id: "A", arrival: 0, bursts: [{ k: "cpu", v: 2 }, { k: "io", v: 2 }] }
  ] },
  ends: { A: 4 },
  trace: [
    { cpu: "A", io: null, ready: "A", ev: [] },
    ...
  ]
}
```

- `ready` se escribe con la misma notación que las trazas de la cátedra:
  `"B(1),C(2)"`, o `""` si está vacía.
- `ev` es una lista de eventos estructurados, `[["ioStart", "A"]]`. El texto sale
  de `UI_TEXT.events`, nunca del ejemplo.
- `resources` sólo hace falta si el ejemplo no es una CPU y un dispositivo.
- `rows` sólo hace falta si las filas no son los procesos de la tabla (hilos).
- `variants` reemplaza a `trace` y `ends` cuando el ejemplo tiene dos versiones.

Hay que agregarlo también a la tabla de acá arriba y, si abre un grupo nuevo, a
`UI_TEXT.groups`.

### Lo que el recurso verifica solo al abrirse

Al cargar, `verifyView` recorre **las dieciocho trazas** (los diecisiete
ejemplos, con las dos variantes del 15) y comprueba, contra el enunciado:

- que cada fila ocupe la CPU exactamente tantos instantes como suman sus ráfagas
  de CPU;
- que ocupe cada dispositivo exactamente tantos instantes como suman sus ráfagas
  de I/O en ese dispositivo;
- que el último instante que ocupa algo más uno sea igual al fin declarado en
  `ends`;
- que ninguna asignación ni ninguna cola nombre a alguien que no es una fila;
- que nadie ocupe dos recursos en el mismo instante;
- que en el 16, el estado declarado en cada casillero sea **el mismo que sale** de
  la asignación de CPU, dispositivo y memoria. Es decir: la regla que usa el modo
  Resolver para derivar los estados está chequeada contra los estados que la
  cátedra resolvió a mano, en los doce instantes.

Si algo no cierra, tira un error con el ejemplo y el instante, y la consola deja
de estar limpia. **Eso reemplaza a los comentarios**: la expectativa está escrita
como un campo que el código chequea, no como un texto que nadie corre. Si
agregás un ejemplo y la consola queda limpia, la traza es consistente con su
propio enunciado.

Lo que **no** puede verificar es si la traza es la correcta según el algoritmo.
Eso lo garantiza el origen, y por eso está la sección que sigue.

---

## Qué no se puede tocar

- **Las trazas.** Cada una se resolvió a mano y se cotejó contra las soluciones
  de la cátedra. Varias dependen de convenciones que no se deducen de un libro
  de planificación cualquiera: el dispositivo compartido, los **dos órdenes de
  reinserción distintos** (en RR y VRR el fin de quantum se encola antes que el
  fin de I/O; en el resto, primero el fin de I/O), y que un KLT con un ULT listo
  no suelta la CPU cuando uno de sus ULT termina. Si una traza te parece mal,
  **preguntá antes de corregirla**. Un ejemplo "arreglado" por alguien que no
  tenía esas convenciones enseña algo falso a cientos de estudiantes, y nadie se
  entera hasta el parcial.
- **Que el casillero de un proceso que pidió el dispositivo y no lo tiene quede
  vacío.** El diagrama dibuja ocupación de recursos, no estados: pintarlo de rojo
  diría que tiene un dispositivo que no tiene. Que está esperando se ve en la
  cola del dispositivo. En el ejemplo 2 —donde ese es justamente el tema— el
  casillero lleva además el borde punteado.
- **La numeración del eje.** El casillero `n` es el intervalo `n−1` a `n`. Así lo
  dibuja la cátedra y así lo dibuja el estudiante en el parcial.
- **La referencia de colores se genera, no se escribe.** Cada renglón sale de
  `UI_TEXT.cells[estado]` más `STATES[estado].label`, así que dice `CPU = RUNNING`,
  `I/O = BLOCKED`, `LISTO = READY`, `FIN = TERMINATED` y, en el 16, también
  `R/S = READY SUSPENDED` y `B/S = BLOCKED SUSPENDED`. Ojo con esa `R`: era `L/S`,
  de «listo», y contradecía al `READY` que tiene al lado. Las abreviaturas de los
  dos estados suspendidos salen del nombre en inglés justamente para que no pase. Si escribís los seis a mano, tarde o temprano uno queda con otro
  nombre que el resto del recurso. Por lo mismo, la cola de listos dice `READY` en
  su encabezado: el estudiante tiene que poder atar el diagrama al diagrama de
  estados que ya vio.
- **El `padding: 3px` y el `gap: 7px` de `.gantt-grid`.** No son estéticos, y los
  dos salen del mismo hecho: el instante actual se marca con un anillo de 2 px
  hecho con `box-shadow`, y un `box-shadow` **no ocupa lugar**.
  - Sin el **padding**, el anillo de la primera fila y el de la última columna
    caen afuera del contenedor que scrollea y quedan recortados: se ve como si el
    diagrama estuviera cortado del lado derecho.
  - Sin el **gap de 7 px**, dos casilleros marcados de la misma columna se pisan.
    La cuenta es la de la skill `html-resource`: si los dos lados que se enfrentan
    pueden estar marcados, `gap >= 2 × anillo + 3px`, o sea 7. Bajarlo hace que la
    columna marcada se lea como un solo bloque con muescas.

  El ejemplo 17 usa `gap: 4px` porque ahí el diagrama está resuelto y **no hay
  ningún casillero marcado**: sin anillos, no hay nada que separar.
- **El techo temático.** Nada de afinidad, balanceo de carga o migración en el
  15; nada de sincronización, deadlock ni gestión de memoria; ningún nombre de
  planificador real; nada de inversión de prioridades ni tiempo real; de las
  interrupciones, sólo el nombre del evento. El 16 puede nombrar los estados
  suspendidos y el planificador de mediano plazo —que ya vieron— y nada más.
- **Los comentarios.** No hay ninguno y no va ninguno. La estructura se lee en
  los tres `<script id="...">`.

---

## Decisiones que conviene revisar

- **Los textos en español los redactó un agente.** Están todos en el bloque de
  datos, en español rioplatense, y **todavía no los revisó nadie de la cátedra**.
  Es lo primero que hay que leer antes de mandarles el link a los estudiantes.
- **En el ejemplo 17, primer diagrama, «SJF sin desalojo» y «SJF con desalojo»
  dan el mismo diagrama.** Con esa tabla ninguna llegada desaloja a nadie, así
  que las dos opciones son indistinguibles. Se decidió **dejar las dos y
  decirlo**, en la tarjeta «Ojo con estos dos»: es un hecho sobre los algoritmos,
  no un defecto de la pregunta. La alternativa era juntarlas en una sola opción.
- **En el ejemplo 17, primer diagrama no se ofrecen RR ni VRR**, porque el
  enunciado no declara ningún quantum. En vez de inventar uno, la tarjeta final
  dice justamente eso: fijarse si hay quantum es lo primero que conviene mirar.
- **La paleta sale del recurso anterior.** Los colores de estado son literalmente
  los de `process-lifecycle` (`RUNNING` azul, `BLOCKED` rojo, `READY` verde,
  `TERMINATED` gris, y los dos suspendidos naranja y amarillo). No se inventó
  ningún color acá.
- **Lo correcto y lo incorrecto se marcan sin color.** Al confirmar queda
  encendida la opción correcta, la elegida queda tachada, y al lado dice «bien» o
  «era X». El rojo y el verde ya significan `BLOCKED` y `READY` acá, así que
  gastarlos en «esto estuvo mal» los haría chocar con el material.
- **La decisión se toma con botones, no con un `select`.** Se probó primero con un
  desplegable por recurso: son dos clics, esconde las opciones hasta que lo abrís,
  y deja la vista lejos del diagrama. Con los botones es un clic, están todas las
  opciones a la vista, y el color de cada uno enseña la equivalencia CPU/RUNNING y
  I/O/BLOCKED. Se evaluó también pintar directamente sobre los casilleros, como
  quien dibuja a mano: se descartó porque obliga a tener una herramienta activa
  —un modo—, y equivocarse de modo es de los errores más caros que hay. La vista
  previa punteada da el mismo efecto de «estoy dibujando» sin ningún modo.

---

## Publicación

La página va a `https://santialemarino.github.io/ssoo-recursos/cpu-scheduling/`.
Hay que agregarla a la lista de `index.html` de la raíz y a la tabla del
`README.md` de la raíz.

## Verificación antes de publicar un cambio

1. Abrir el archivo con doble clic, **sin servidor y sin red**.
2. Consola limpia en todo el recurso. Si una traza no cierra contra su
   enunciado, se entera acá.
3. A 1280×720: todo entra, ningún panel scrollea, la página no scrollea.
4. A 390 px de ancho: legible, en una columna, sin scroll horizontal del `body`.
5. Recorrido completo para adelante y para atrás en modo Ver: el estado inicial
   queda idéntico.
6. En modo Resolver: confirmar una columna bien y una mal, y que el contador y la
   corrección hagan lo que dicen.
7. Los dos diagramas del ejemplo 17, respondiendo uno bien y uno mal.
