# Planificación de procesos

El estudiante **pinta el diagrama de Gantt**: elige un recurso —la CPU, cada
dispositivo de I/O— y pinta, casillero por casillero o arrastrando, quién lo
ocupa en cada instante. El recurso le dice si acertó y, cuando no, le muestra la
respuesta correcta y por qué es esa. Es un ensayo del ejercicio de parcial, con
la misma tabla y el mismo dibujo, hecho de la misma forma: dibujando.

Se abre haciendo doble clic en `index.html`. No necesita internet ni instalar
nada.

Es el tercer recurso de la familia de **Ciclo de instrucción e interrupciones** y
**Procesos e hilos**, y da por sabido todo lo de ese segundo: estados, PCB,
cambio de contexto, KLT y ULT, y los tres planificadores.

Este archivo es la documentación del recurso: qué enseña, cómo editarlo y qué no
se puede tocar. Si vas a modificar `index.html`, leelo antes.

---

## Los diecisiete ejemplos

Van en cuatro grupos, y así aparecen en la escalera de arriba. El de
**Dispositivos** tiene uno solo: es otro eje, no un algoritmo más, y por eso no
va en el medio de la progresión de la CPU.

| # | Ejemplo | Qué enseña |
|---|---|---|
| **Base** | | |
| 1 | FIFO | Cómo se lee el tablero, y que el que espera el dispositivo no ocupa la CPU |
| 2 | SJF sin desalojo | Que el criterio deja de ser el orden de llegada |
| 3 | SJF con desalojo | Que cada llegada vuelve a abrir la decisión |
| 4 | SJF con estimadores, sin desalojo | Que se planifica con la estimación, y que la estimación puede errarle |
| 5 | SJF con estimadores, con desalojo | La misma tabla del 4: que la estimación también decide a quién desalojan |
| 6 | HRRN | Que el tiempo de espera entra en el criterio |
| 7 | Round Robin | El quantum, y el orden de reinserción de RR |
| 8 | Virtual Round Robin | Ready+ y el quantum sobrante, y a costa de quién |
| 9 | Colas multinivel con feedback | Bajar de cola, subir de cola, y desalojar entre colas |
| **Dispositivos** | | |
| 10 | Dos dispositivos | Que con dispositivos distintos las I/O se superponen |
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

Los pares **2 y 3**, **4 y 5**, **7 y 8**, **13 y 14** comparten enunciado: la
diferencia entre los dos diagramas es toda la lección. El **15** hace lo mismo
con dos variantes adentro de un ejemplo.

---

## Dos modos sobre el mismo motor

- **Resolver** — el modo interactivo. Arriba del diagrama hay **un botón por
  recurso**: CPU, dispositivo, y los que el ejemplo agregue. El botón elegido dice
  qué estás pintando, y **está pintado del color del estado que produce**: azul la
  CPU, porque tener la CPU es estar en RUNNING; rojo el dispositivo, porque tener
  una I/O es estar en BLOCKED. Se cambia con un clic, con las teclas `1`…`9`, o
  con la **barra espaciadora**, que rota entre los recursos sin sacar la mano del
  diagrama.

  **Se pinta sobre el diagrama.** Al pasar el mouse, el casillero se muestra con
  opacidad —así se ve dónde vas a marcar antes de marcar— y se pinta con un clic o
  **arrastrando** para varios instantes seguidos. `Esc` cancela el trazo antes de
  soltar y **`Ctrl+Z` deshace** el último, todas las veces que haga falta. La vista
  previa muestra siempre el color del recurso que tenés elegido, y aparece
  exactamente en los casilleros donde el clic va a hacer algo: si no hay vista
  previa, no hay clic. Sale igual sobre un proceso que todavía no llegó, porque
  pintarlo ahí es un error que el recurso sabe explicar, y de paso no adelanta nada
  del diagrama.

  **Cada casillero es un `<button>`.** Se llega con `Tab` y se pinta con `Enter` o
  con la barra espaciadora, igual que cualquier otro control de la página. Por eso
  la barra espaciadora rota de recurso sólo cuando el foco **no** está en un botón:
  adentro de un botón, la barra es lo que lo activa. Las teclas `1`…`9` funcionan
  siempre.

  **La pregunta es siempre la misma: quién ocupa este recurso en este instante.**
  Por eso la unidad de corrección es el par instante–recurso y no el casillero: en
  el 16 un proceso puede tener el dispositivo **y** estar fuera de memoria a la
  vez, y son dos respuestas distintas. Por eso también el color de un casillero no
  se elige, se deriva: en el 16 pintar «fuera de memoria» sobre alguien que además
  tiene el dispositivo lo pinta de BLOCKED SUSPENDED, y sin el dispositivo, de
  READY SUSPENDED. No es una excepción: es la regla de siempre, que en los otros
  dieciséis ejemplos da siempre el mismo color porque ahí cada recurso corresponde
  a un estado y nada más.

  **Sólo se puede pintar lo que se puede corregir.** Cada recurso se resuelve en
  orden, y el borde de cada uno es el primer instante suyo que todavía no
  contestaste. Los casilleros más allá del borde **no se pintan**: no tienen el
  cursor de cruz, no muestran la vista previa y el clic no hace nada. Tampoco se
  pinta un casillero de un proceso que el tablero ya da por terminado —ahí dice
  `FIN`, y volver a ocuparlo no es un error interesante, es una contradicción.

  El borde es **por recurso**, no por columna, y de ahí salen dos cosas que
  importan: el arrastre largo sigue siendo posible —A puede tener el dispositivo del
  2 al 6 aunque en el medio la CPU cambie de dueño tres veces, porque el trazo
  arranca en el borde del dispositivo y avanza con él— y un recurso que en ese
  instante no ocupa nadie no traba nada. Un trazo arranca en el borde y pinta
  **corrido** desde ahí; si el mouse va rápido y saltea casilleros, se rellenan
  solos, sin agujeros.

  **Esto reemplazó al «en suspenso».** Antes se podía pintar en cualquier lado y lo
  adelantado quedaba esperando, con la etiqueta `antes`, hasta que se resolviera lo
  anterior. Se sacó porque medido daba mal: pintando la vida de un proceso de
  izquierda a derecha —la forma más natural de encarar el tablero— **el 26 % de las
  respuestas correctas caía en suspenso**, y el estudiante no tenía forma de saber
  por qué, porque lo que faltaba era otro recurso en otra fila. Pintando por columna
  o por recurso caía el 0 %: la misma acción daba dos respuestas distintas según un
  orden invisible. Y era lo único que podía poner una mentira en el tablero: un
  proceso ocupando algo antes de llegar, o volviendo a ocupar algo después de
  terminar. Bloquear en vez de diferir arregla las tres cosas y hace que el diagrama
  coincida con lo que ya decía el resto de la pantalla —el anillo del instante
  actual, el contador y el panel de estado apuntan todos al mismo lugar.

  **Al corregir, el diagrama pasa a decir la verdad.** El casillero correcto queda
  pintado; si te equivocaste, el que sí iba aparece con la etiqueta `iba`, y el que
  pintaste de más queda rayado y tachado. Lo que el estudiante
  mira al final es siempre el diagrama correcto, no su borrador. La devolución
  agrupa instantes contiguos: un trazo de cuatro casilleros errados sale como una
  sola frase, no como cuatro. Siempre se avanza; nunca queda trabado. Hay un
  contador discreto de aciertos, **que arranca de cero en cada ejemplo** —el número
  está al lado de un ejemplo, así que habla de ese ejemplo— y al que **deshacer no
  toca**: recuperás el casillero para volver a pintarlo, no el punto.

  **Una marca sólo puede señalar lo que el casillero muestra.** Son tres y nada más:
  el relleno del estado (lo que pasó de verdad), la etiqueta `iba` (esto es lo que
  iba acá y pusiste otra cosa) y el rayado (pintaste acá y no iba nada). Conviene
  tener presente por qué son sólo tres, porque ya se rompió una vez. El rayado
  funciona porque *reemplaza* el contenido del casillero: lo único que hay ahí es tu
  error. La etiqueta `iba` funciona porque señala exactamente lo que el casillero
  dice: «esto es lo que iba acá, y te lo perdiste». Pero cuando el casillero muestra
  un hecho verdadero —el proceso ocupaba otro recurso, o ya había terminado, o es la
  banda de estados del 16— **no hay nada ahí que una etiqueta de error pueda
  señalar**, y ponerle una se lee como si negara lo que se ve. Pasó tal cual: pintar
  el dispositivo donde iba la CPU y después acertar la CPU dejaba el casillero
  diciendo «CPU» con una etiqueta «no iba» al lado, mientras la narración decía
  «Bien: A tiene la CPU de 0 a 1». Ese error vive en la devolución, que **nombra el
  recurso** y por eso no se presta a confusión.

  **Cuando se erra la CPU, la devolución dice por qué.** No alcanza con «era B»:
  en los ejemplos que deciden con una cuenta, lo que hay que ver son los dos
  números. La explicación sale de la cola del instante, que ya viene anotada con
  el número que decide, así que dice lo mismo que el panel de estado y no puede
  contradecirlo. Sale una sola por trazo, pegada al primer error. **Nunca dice algo
  que la traza no diga**: si el que elegiste no estaba en la cola, la frase no puede
  hablar de su lugar en la cola. De ahí salen las formas:

  | Cuando | Dice |
  |---|---|
  | El que elegiste todavía no había llegado | «C todavía no había llegado: llega recién en el 2» |
  | Los dos estaban en la cola con números distintos | «Ahí el criterio era la ráfaga restante: B tenía 1, y C, 2» |
  | Los dos tenían el mismo número | «Ahí los dos tenían Q1, así que decidía el orden de la cola» |
  | El algoritmo no anota números y los dos estaban en la cola | «Ahí decidía el orden de la cola, y A estaba antes que B» |
  | El algoritmo no anota números y el que elegiste no estaba | «Ahí decidía el orden de la cola, y el primero era A» |
  | El que elegiste no estaba en la cola | «B tenía 2. A no estaba en la cola de listos en ese instante» |
  | El que ganó ya venía ejecutando | «A ya venía ejecutando, y este algoritmo no desaloja» |

  **En los ejemplos de dos niveles la explicación es de dos niveles.** La cola de
  listos que ahí importa no es la de KLT —esa tiene KLT adentro, no ULT— sino la
  lista de ULT del KLT que ganó, y el criterio es el de la biblioteca
  (`ultAlgorithm`), no el del sistema operativo. Si el ULT que elegiste es de otro
  KLT, la explicación es sobre KLT y no sobre ráfagas:

  | Cuando | Dice |
  |---|---|
  | El KLT del que elegiste estaba bloqueado | «KLT1 estaba bloqueado: mientras un ULT suyo espera una I/O, ninguno de sus ULT puede correr» |
  | Su KLT sí tenía otro procesador (el 15) | «KLTB sí tenía procesador —estaba en la CPU 2—, pero ahí su biblioteca eligió a ULT3, no a ULT4» |
  | El que elegiste ya estaba en el otro procesador | «Ahí la CPU 2 fue para KLTB, y adentro su biblioteca eligió a ULT3» |
  | Su KLT no fue el elegido | «Ahí la CPU fue para KLT1: el sistema operativo elige KLT, y KLT2 no era el elegido» |

  Y cuando el error es **de recurso** y no de proceso —pintar el router donde iba
  la impresora— la devolución lo dice con esas palabras: «K1 estaba en la
  impresora, y el router no era de nadie». Es el error propio del 10.

  Esa última es la que más aparece en los ejemplos sin desalojo, y es a propósito:
  el error típico ahí no es equivocarse de cuenta, es no darse cuenta de que la
  decisión ni siquiera estaba abierta. De ahí sale el campo `noPreempt` de
  `ALGORITHMS`: separa «no desaloja» de «desaloja» para poder decirlo con esas
  palabras.

  El botón **«Terminé»** cierra el diagrama: corrige lo que haya en suspenso y
  completa lo que falte. Lo que nunca contestaste **no lleva marca**: la marca `iba`
  es la corrección de una respuesta tuya, y cerrar un diagrama en blanco no puede
  llenar el tablero de etiquetas que no corrigen nada. Terminé **borra el historial
  de deshacer**, así que no se puede usar para espiar la respuesta y volver atrás
  con el contador intacto.

---

## El chasis se comparte; el canvas se deriva

Está también en `CLAUDE.md` porque vale para todo el repositorio.

**El chasis es fijo:** la escalera de ejemplos arriba, el panel de narración, el
contador, la barra de controles (primero / anterior / siguiente / último, más
reproducir en modo Ver y deshacer / terminé en modo Resolver), las teclas
(flechas, `Inicio`, `Fin`), un tooltip en cada título de panel, una tarjeta de
cierre por ejemplo, una sola fila de elección en el encabezado, y el favicon del
repositorio.

**El canvas se deriva por ejemplo, y acá se nota en cuatro lugares:**

- **Cuántos botones tiene la barra de herramientas.** Uno por recurso, siempre,
  y salen de `view.resources`: el motor no sabe cuántos son. Casi todos los
  ejemplos tienen dos (CPU y dispositivo); el 10 tiene tres (CPU, Impresora,
  Router) y el 15 tiene tres (CPU 1, CPU 2 y dispositivo). Cuando dos recursos
  comparten color —las dos CPU del 15— lo que los distingue es el nombre en el
  botón y el `short` que queda escrito en el casillero.
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

  El 15 es el que más apretado queda —tres recursos y cuatro filas de diagrama— y
  es el que hay que medir cada vez que se agrega un renglón al pie. Con el pie en dos
  renglones, su panel de estado **se pasa 14 px en el instante 2 y 4 px en el 3**, y
  ningún otro ejemplo se pasa en ningún instante. Viene de antes de que se pintara
  sobre el diagrama y es puramente aritmético: la fila del diagrama es `auto` y la del
  panel de estado es `1fr`, así que cada píxel que crece el pie se lo saca al panel de
  estado, y en el 15 el panel necesita 189 px y a dos renglones le quedan 175. Se
  cierra de dos maneras, las dos con costo: bajar el tope a un solo renglón —y perder
  la mitad de la devolución en los diecisiete— o dejar de darle fila propia al bloque
  de eventos cuando la cantidad de bloques es par, que ahorraría 60 px pero cambia el
  acomodo del panel en varios ejemplos. Las dos son decisión de cátedra.
- **Cuándo aparece el `FIN`.** Se pinta solo, en cuanto se responde **el último
  instante que ese proceso ocupa algo**, sin esperar a que cierre la columna ni a
  que se cierre el ejemplo. No revela nada: que un proceso terminó se deduce del
  enunciado —ya gastó todas sus ráfagas— y de lo que el estudiante acaba de
  contestar. Sale de `ends`, no de «la última ráfaga de CPU»: en el 10, K1 termina
  con una I/O.

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
   parámetros. **Cuando el ejemplo tiene más de un dispositivo, el encabezado de
   cada columna de I/O dice cuál es** —`IMP`, `ROU`— en vez de un `I/O` genérico:
   sin eso, el 10 no se puede resolver, porque no hay forma de saber que la
   primera ráfaga va a la impresora y la segunda al router. El encabezado sale de
   los `dev` de esa columna y sólo aparece si todas las filas usan el mismo; si no,
   vuelve a decir `I/O`. Es la misma tabla que se da en el parcial. Las ráfagas ya
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
4. **Narración.** La devolución del último trazo, más la razón del instante
   cuando la columna quedó resuelta. Moverse con las flechas borra la devolución:
   habla del trazo que acabás de hacer, no del instante que estás mirando.

El panel de decisión ya no existe: la decisión se toma sobre el diagrama. En modo
Resolver el diagrama ocupa el lugar que tenía, igual que en modo Ver.

---

## Cambiar un texto

Todos los textos visibles están en el bloque `<script id="data">`, y ninguno está
escrito adentro del motor ni del render.

- `UI_TEXT` — rótulos, botones, títulos de panel, y las **plantillas** de eventos
  y de razones. Cambiar `UI_TEXT.reasons.fifo` cambia la frase en los diez
  instantes donde FIFO elige a alguien.
- `UI_TEXT.paint` — todo lo del modo Resolver: los rótulos de la barra, las
  la etiqueta `iba`, las plantillas de devolución y las `why*`,
  que son las que explican por qué la decisión de CPU estaba mal. Hay una familia por
  tipo de recurso: `hit` / `missOther` / `missFree` / `missing` para los que se
  ocupan, y las mismas con el sufijo `Mem` para «fuera de memoria», que no se
  puede decir con las mismas palabras.
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
- `resources` sólo hace falta si el ejemplo no es una CPU y un dispositivo. Cada
  recurso lleva `label` (el botón), `short` (lo que se escribe en el casillero) y
  `phrase` (cómo se lo nombra en la devolución, con artículo: «la impresora», «el
  router»). Si falta `phrase`, las frases de corrección salen rotas.
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
  cola del dispositivo. En el ejemplo 1 —donde ese es justamente el tema— el
  casillero lleva además el borde punteado.
- **El tope de líneas de la devolución: dos, contando todo.** `FEEDBACK_LINES`. Y
  **una sola cuando el ejemplo ya se cerró**, que además reemplaza a la razón del
  instante en vez de sumarse a ella. No es estética: el tope tiene que contar todos
  los renglones, incluido el de «y N más», no sólo los veredictos. La devolución vive
  en el pie, el pie empuja al `main`, y el `main` es de donde salen los paneles: sin
  tope, terminar un ejemplo con «Terminé» listaba diez
  líneas y comprimía el panel de estado unos 50 px. Con el tope, el recurso
  scrollea igual o menos que la versión de selectores en los diecisiete.

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

- **El ejemplo 5 es el único que no viene de una resolución previa de la
  cátedra.** Lo derivó un agente a partir de las convenciones del 3 y del 4, y
  cierra contra su propio enunciado —`verifyView` lo verifica—. Las tres
  convenciones de las que depende **están confirmadas por la cátedra**, y son las
  que hay que releer si alguna vez el diagrama parece mal:
  1. **Se compara la estimación restante**, no la estimación completa: B entra
     con 2 contra los 4 que le quedaban a A, que había arrancado con 5. Es la que
     más cambia el resultado — comparando la estimación completa, en el instante 6
     B (3) desalojaría a A (5) y la segunda mitad del diagrama sería otra.
  2. **Un empate no desaloja.** En el instante 6, B vuelve de I/O con 3 y a A le
     quedan 3: sigue A. Lo respalda el ejemplo 3, donde con `C(2),B(2)` entra C,
     que estaba antes en la cola.
  3. **Al que se le agota la estimación no se lo desaloja por eso.** B estimó 2 y
     ocupa la CPU 4 instantes; termina su ráfaga. Acá no cambia nada —A tiene 4
     estimados y B pasa a 0— pero es una convención igual.

  Lo que queda sin cotejar contra una solución escrita a mano es el diagrama en
  sí, que se derivó de esas tres reglas.

- **El 1 dice que FIFO no desaloja, pero no lo demuestra.** En su tabla los dos
  procesos llegan en el 0, así que no hay ninguna llegada tardía que pudiera
  desalojar a nadie: el no-desalojo está en la bajada, que describe el algoritmo,
  y no en la tarjeta de cierre, que sólo puede decir lo que el diagrama muestra
  —de ahí que cierre con el dispositivo—. Quien primero lo ve de verdad es el 3,
  por contraste con el 2. Esto quedó así al sacar el ejemplo introductorio, que sí
  tenía una llegada tardía; si molesta, la salida no es reescribir el cierre sino
  darle al 1 una tabla con alguien que llegue tarde.

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
- **Lo correcto y lo incorrecto se marcan sin color.** En el diagrama, el rayado
  y las etiquetas `iba` y `no iba` son achromáticas —tinta, no tono—; en el 17, la
  narración dice qué elegiste y qué era. El rojo y el verde ya significan `BLOCKED`
  y `READY` acá, así que gastarlos en «esto estuvo mal» los haría chocar con el
  material. Ojo con `antes`, que sí usa el naranja de `BLOCKED SUSPENDED`: es la
  única marca con tono, y se banca porque no dice «mal» sino «todavía no».
- **Se pinta sobre el diagrama, y eso trajo de vuelta el modo.** Hubo dos
  versiones anteriores: un desplegable por recurso —dos clics, opciones
  escondidas, la vista lejos del diagrama— y después un grupo de botones por
  recurso en un panel aparte. Pintar sobre los casilleros se había **descartado**
  con este argumento, que conviene tener presente porque sigue siendo cierto:
  obliga a tener una herramienta activa —un modo—, y equivocarse de modo es de los
  errores más caros que hay. Se revirtió esa decisión a propósito, porque pintar es
  lo que el estudiante hace en el parcial y el panel era un intermediario. Lo que
  paga el costo del modo son cuatro cosas: el botón activo está pegado al
  diagrama y pintado del color que va a salir, la vista previa con opacidad muestra
  el resultado antes del clic, la barra espaciadora hace barato cambiar, y
  `Ctrl+Z` deshace. La vista previa sale en cualquier casillero que acepte el clic,
  justamente para que el modo se vea antes de usarlo.
  **Lo que el deshacer no borra es el error del marcador**: si
  esto se revisa alguna vez, esa es la decisión a revisar primero, porque hoy
  equivocarse de modo cuesta un punto.

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
6. En modo Resolver, sobre el diagrama: pintar un trazo bien y uno mal, arrastrar
   una ráfaga entera de un tirón y también de un saque rápido, deshacer, y «Terminé».
   Que el contador y la corrección hagan lo que dicen.
7. Que en modo Resolver el diagrama termine siendo el diagrama correcto, se haya
   respondido bien o mal.
8. Los dos diagramas del ejemplo 17, respondiendo uno bien y uno mal.
9. Cambiar de modo y volver, y cambiar de variante en el 15: pintar después de
   cada cambio. Es el camino que rompió una vez, porque reiniciar el progreso y
   reconstruir la vista tienen que pasar en ese orden.
10. Con el teclado solo: llegar a un casillero con `Tab`, pintarlo con `Enter` y
    con la barra espaciadora, y comprobar que el foco no se va al activar un botón
    de la escalera, del modo o de la barra de recursos.
11. Mirar el diagrama con **cada** recurso elegido, no sólo con el primero: lo que
    ya está contestado se tiene que ver igual con cualquier herramienta activa.
12. Pintar un recurso donde iba otro y después acertar el que iba: el casillero tiene
    que quedar limpio, diciendo la verdad, sin ninguna marca que contradiga a la
    narración.
13. Con cada recurso elegido, mirar dónde está el borde: los casilleros pintables
    tienen borde lleno y cruz; el resto, borde punteado y nada. El anillo del
    instante actual tiene que caer sobre la columna pintable de la CPU.
14. Intentar pintar sobre un `FIN` y sobre un casillero lejano: no tiene que pasar
    nada, y el contador no se tiene que mover.
