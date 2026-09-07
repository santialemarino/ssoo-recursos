# Sincronización

Semáforos y secciones críticas **paso a paso**. Según lo que enseñe cada ejemplo,
en pantalla aparecen las columnas de código de cada proceso con su línea actual,
los registros propios de cada uno, la memoria compartida, el modo y el `IF`, los
semáforos con su valor y su cola, o los carriles de tiempo. Siempre hay una
narración de lo que pasó en el paso. El estudiante avanza y retrocede; no escribe
código, y sólo en el ejemplo 2 elige algo.

Se abre haciendo doble clic en `index.html`. No necesita internet ni instalar
nada.

Es el cuarto recurso de la misma familia que **Ciclo de instrucción e
interrupciones**, **Procesos e hilos** y **Planificación de procesos**, y da por
sabido todo lo de esas tres clases: estados de proceso, planificador de corto
plazo, cambio de contexto, hilos, `BLOCKED` e interrupciones.

Este archivo es la documentación del recurso: qué enseña, cómo editarlo y qué no
se puede tocar. Si vas a modificar `index.html`, leelo antes.

**No está enlazado desde la portada.**

---

## Los dieciocho ejemplos

| # | Ejemplo | Qué enseña |
|---|---|---|
| 1 | La carrera | Que una línea de código no es una instrucción, y que eso alcanza para perder una cuenta |
| 2 | El intercalado en tus manos | Que el orden no lo decide el código: de los veinte órdenes posibles, sólo dos dan bien |
| 3 | El que sólo lee | Que un lector solo no provoca nada, y las tres condiciones de Bernstein |
| 4 | Dónde empieza la sección crítica | Qué instrucciones tienen que estar adentro de la región, y qué pasa si falta una |
| 5 | Turno | La primera solución por software, y el requisito de progreso |
| 6 | Interesado | La segunda solución por software, y que también le falta progreso |
| 7 | Peterson | La solución que funciona, y las dos cosas que cuesta |
| 8 | Que no lo interrumpan | La primera solución por hardware, y sus cuatro cambios de modo |
| 9 | Test & Set | Una instrucción que lee y escribe en un solo paso indivisible |
| 10 | El semáforo, por dentro | `wait` y `signal`, y qué le pasa al proceso que no puede entrar |
| 11 | El valor negativo | Qué cuenta el valor de un semáforo arriba y abajo de cero |
| 12 | Girar o bloquearse | Cuánta CPU cuesta cada una de las dos esperas |
| 13 | Cinco impresoras | Un semáforo contador: arranca en la cantidad de instancias |
| 14 | MILANESA | Que los semáforos se cruzan, y que los valores iniciales fijan quién arranca |
| 15 | Sacar de una lista vacía | Que un mutex protege y no avisa |
| 16 | Avisar que hay algo | Un semáforo para contar lo que hay, y el productor que sigue sin freno |
| 17 | Lista acotada | El tercer semáforo, el que cuenta el lugar que queda |
| 18 | El de mayor prioridad esperando | Inversión de prioridades, y la herencia como arreglo |

Los doce primeros son **el mecanismo**; del 13 al 18 son **los usos**. Son
**214 pasos** en total, contando las dos variantes del 14 y las dos del 18. El
ejemplo 2 no es una traza: es un árbol de 69 nodos y 20 hojas, y cada camino
tiene 7 pasos.

Cada ejemplo termina con una **placa de cierre**: una frase, en un recuadro, que
es lo que el estudiante se tiene que llevar de ese ejemplo. Está en el campo
`closing`. Y cada título de panel tiene un globito que explica qué muestra ese
panel, en `PANEL_TIPS`.

Los controles son **Primero · Anterior · Siguiente · Último · Reproducir**, con
las flechas del teclado y `Inicio` / `Fin`. Son los mismos que en los otros tres
recursos de la familia: si acá se agrega o se saca uno, hay que hacer lo mismo
allá. La regla está en `CLAUDE.md`.

---

## Lo que este recurso hace y lo que no

**Traza semáforos que ya están escritos.** El código se le da al estudiante; lo
que mira es el intercalado, el valor de cada semáforo y quién termina bloqueado.

**No le pide que escriba los semáforos y se los corrige.** Eso es síntesis, es lo
que pide la guía de ejercicios, y es otro recurso. Por eso acá no hay campos para
escribir, no hay modo «propone tu solución» y no hay puntaje. La única cosa que el
estudiante elige es el orden del intercalado en el ejemplo 2, y eso es
exploración, no una respuesta: **no se guarda en ningún lado.** El recurso no usa
`localStorage`.

---

## El chasis se comparte; el canvas se deriva

Está también en `CLAUDE.md` porque vale para todo el repositorio, y acá está la
derivación concreta de cada ejemplo.

**No hay interruptor de granularidad, y es a propósito.** El recurso hermano del
ciclo tiene uno porque las etapas anidan adentro de las instrucciones y las dos
vistas son honestas: terminan en el mismo estado final. Acá no lo serían.
Colapsar `tareasPendientes++` en un paso da un **valor final distinto** —2 en vez
de 1— y eso rompe justamente el invariante que hace confiable a un interruptor. Y
los ejemplos donde no se perdería nada al colapsar no tienen nada que colapsar,
porque `wait` y `signal` son atómicas por definición.

En su lugar, **cada ejemplo declara su `stepUnit`**, que se ve siempre arriba a la
derecha de la narración. Cuando la unidad cambia entre dos ejemplos seguidos, el
que la cambia trae un `stepUnitNote` y lo dice una vez, con todas las letras.
**Nunca se cambia la unidad del paso en silencio.**

| # | Unidad del paso | Lo que muta y hay que mirar | Canvas |
|---|---|---|---|
| 1 | instrucción | los dos registros y la variable compartida | Procesos + Registros + Memoria compartida |
| 2 | instrucción | lo mismo, con el orden a elección | igual que el 1, con dos botones adentro del panel de Procesos |
| 3 | instrucción | que el registro de P3 quedó viejo | igual que el 1 |
| 4 | instrucción | dónde cae el intercalado respecto de la región dibujada | igual que el 1 |
| 5 | línea | `turno`, y quién queda girando | Procesos + Memoria compartida. **Sin Registros** |
| 6 | línea | las dos banderas | igual que el 5 |
| 7 | línea | las banderas y `turno` juntos | igual que el 5 |
| 8 | instrucción | el `IF`, el modo, y cuántos cambios de modo costó | Procesos + Registros + Memoria compartida + **Modo y IF** |
| 9 | línea | `lock`, y que no hay hueco entre leerlo y escribirlo | Procesos + Memoria compartida |
| 10 | línea | el valor del semáforo y quién se bloquea | Procesos + Memoria compartida + **Semáforos** |
| 11 | línea | el valor negativo y el orden de la cola | igual que el 10 |
| 12 | unidad de tiempo | cuánta CPU se gasta en cada espera | **Carriles de tiempo**, y nada más |
| 13 | línea | el valor del contador y quién queda en la cola | Procesos (un solo listado con seis marcas) + Semáforos. **Sin memoria compartida**: no hay variable, hay semáforo |
| 14 | línea | los dos semáforos y lo que se va imprimiendo | Procesos + Semáforos + **Salida** |
| 15 | línea | la lista y el mutex | Procesos + Memoria compartida (la lista) + Semáforos |
| 16 | línea | la lista pasándose de su tamaño | igual que el 15 |
| 17 | línea | los tres semáforos moviéndose juntos | igual que el 15 |
| 18 | unidad de tiempo | quién tiene la CPU y quién está bloqueado | **Carriles de tiempo**, y nada más |

Cuatro decisiones de canvas que conviene entender antes de cambiarlas:

- **Registros aparece sólo cuando la unidad es la instrucción** (1 a 4 y 8). Es el
  panel que hace visible la carrera: sin él se ven tres pasos, pero no por qué se
  pierde un incremento. Del 5 al 7 y del 9 al 11 la unidad es la línea y el panel
  se va; la narración del 5 lo dice al pasar.
- **En el 3 no está P2.** El proceso que provoca la carrera de ese ejemplo es P1,
  el que incrementa; P2 no ejecuta ni una línea, así que su columna sería peso
  muerto. La narración del primer paso lo dice.
- **En el 9 la definición de `TestAndSet` va abajo de las dos columnas, no como
  tercera columna.** No es un proceso: no tiene puntero ni estado, y ponerla al
  lado de dos que sí los tienen la haría parecer uno.
- **En el 12 no hay panel de código.** El tema es cuánta CPU se gasta, no qué
  línea corre, y con las dos comparaciones en pantalla no queda alto para más. La
  narración del primer paso lo dice.
- **En el 13 hay un solo listado y seis marcas, no seis columnas.** Los seis
  procesos corren el mismo programa, así que seis columnas iguales serían el mismo
  código repetido seis veces —y a 1280 px no entran: cada columna necesita unos
  200 px y el panel tendría que medir 1276 él solo—. En su lugar va el programa una
  vez, con una chapita por proceso al lado de la línea donde está cada uno y una
  fila arriba con los seis y su estado. Es el mismo recurso que usa el ejemplo 8 de
  *Procesos e hilos* para tres hilos sobre una fuente. El campo que lo activa es
  `sharedProgram`, y el recurso verifica al cargar que todos los procesos de ese
  ejemplo corran de verdad el mismo programa.
- **En el 18 los carriles llevan la prioridad en el nombre** (`P1 (baja)`,
  `P2 (media)`, `P3 (alta)`), porque sin eso no se entiende por qué P2 desaloja a
  P1. La herencia no tiene marca propia: lo que se ve es que en la variante con
  herencia P2 **no** se queda con la CPU en la unidad 3, y la narración lo dice.
- **La referencia de colores de los carriles se arma con lo que hay en pantalla.**
  El 12 usa giro y cambios de contexto; el 18 usa «todavía no llegó» y «ya
  terminó». Antes la referencia era una lista fija y mostraba las cinco cosas
  siempre: en el 18 nombraba dos que no estaban y se callaba dos que sí.

---

## Los paneles

| Panel | Muestra | Lo usan |
|---|---|---|
| Procesos | una columna por proceso, con su programa y su línea actual; al lado del nombre, su estado. En el 13, un solo listado con una marca por proceso | 1 a 11, 13 a 17 |
| Registros | el registro propio de cada proceso | 1, 2, 3, 4, 8 |
| Memoria compartida | las variables compartidas, con su valor y una glosa; y la lista, con su barra de lugares ocupados | 1 a 11, 15 a 17 |
| Modo y IF | el modo del procesador, el `IF`, el contador de cambios de modo y la chapita de interrupción pendiente | 8 |
| Semáforos | el valor de cada semáforo y su cola de bloqueados, en orden | 10, 11, 13 a 17 |
| Salida | lo que se fue imprimiendo, un recuadro por `printf` | 14 |
| Carriles de tiempo | un carril de CPU y uno por proceso | 12, 18 |
| Narración | un párrafo por paso, más una nota cuando hace falta | todos |

En el panel de Procesos, la línea del proceso que **ejecuta en este paso** va en
azul; la del que quedó a mitad de camino va con una barrita neutra al costado y
sin relleno. Es la diferencia entre «está corriendo acá» y «se quedó acá»: si las
dos fueran azules, la pantalla diría que tres procesos ejecutan a la vez mientras
las chapitas dicen que dos están en READY. La barrita es `--muted` y no
`--line-strong`: con el gris más claro, en el ejemplo 7 no se veía dónde había
quedado el proceso 1, que es justamente lo que ese paso muestra.

Los títulos de panel van **en mayúsculas**. Los cuatro recursos declaran
`text-transform: uppercase` en `.panel > h2`, pero el título es un `<button>` y el
navegador le resetea `text-transform`, así que hace falta `text-transform: inherit`
en la regla del `.tip`. El recurso del ciclo la tiene completa desde el principio;
`process-lifecycle` y `cpu-scheduling` la habían copiado incompleta y perdieron las
mayúsculas sin que nadie lo decidiera. Se les agregó la línea que faltaba, así que
los cuatro recursos vuelven a coincidir.

Los anchos de columna van con `minmax(<piso>, <fr>)`. El piso de cada panel es lo
que mide su propio título: el `fr` manda a 1280 px y el piso sólo ata entre 1001 y
1180, que es donde antes el encabezado `MEMORIA COMPARTIDA` del ejemplo 8 se salía
del panel. El código fuente no entra en el piso porque `.source` tiene su propio
`overflow-x`: si la columna se angosta, el programa scrollea adentro de su panel en
vez de empujar la página.

Los nombres de los estados van **en inglés y en mayúscula**, como en el apunte:
`RUNNING`, `READY`, `BLOCKED`. Son los tres únicos que este recurso necesita, y
los otros cuatro no se dibujan.

---

## Cómo se nombran los procesos

Vienen del apunte y **no están unificados a propósito**, porque cada filmina los
nombra a su manera:

- **Ejemplos 1 a 4 y 8: `P1`, `P2`, `P3`.** `P1` agrega tareas, `P2` las realiza y
  `P3` sólo lee la cuenta. Son los tres de la filmina 2.
- **Ejemplos 5 a 7: `Proceso 0` y `Proceso 1`,** escritos completos. Los índices
  0 y 1 no son decorativos: son los de `interesado[0]` e `interesado[1]`, así que
  no se pueden renumerar. Van con el nombre entero justamente para que no se
  confundan con el `P1` de los ejemplos 1 a 4, que es otro proceso.
- **Ejemplos 9 a 12: `P1` y `P2`,** genéricos.
- **Ejemplo 13: `A1` a `A4` y `P1` y `P2`.** Acá la letra dice el grupo: `A` de
  alumno, `P` de profesor. Es el único ejemplo donde `P` no quiere decir «proceso»,
  y por eso cada chapita lleva la glosa al lado.
- **Ejemplos 14 a 18: `P1`, `P2` y, en el 18, `P3`.** En el 15, el 16 y el 17 `P1`
  es el consumidor y `P2` el productor, como en la filmina 31. En el 18 el número
  no dice la prioridad: `P3` es la más alta y `P1` la más baja, y por eso el
  carril de cada uno lleva la prioridad escrita.

---

## Cambiar un texto

Todos los textos que ve un estudiante están juntos, en el bloque
`<script id="data">`, arriba de todo del archivo. **No hay ni un texto visible
escrito en el motor ni en el dibujo**, y el recurso lo verifica de una manera
indirecta pero efectiva: los números que aparecen en la narración no están
escritos a mano.

1. Abrí `index.html` en cualquier editor.
2. Buscá el texto tal como aparece en pantalla. Va a estar entre comillas.
3. Cambialo, guardá y recargá la página.

Dentro de los textos hay tres marcas:

- `[[semaforo:semáforo]]` — la palabra sale con un globito explicativo. Antes de
  los dos puntos va cuál de las explicaciones de `TOOLTIPS` usa; después, lo que
  se lee en pantalla.
- `*así*` — sale en negrita.
- `{tp}`, `{reg1}`, `{semVar}`, `{mutex_count}` — valores que el recurso completa
  solo. **Se pueden mover de lugar en la oración, pero no conviene borrarlos**, y
  sobre todo no conviene reemplazarlos por el número escrito a mano: son lo que
  garantiza que la narración diga lo que la traza hace. Si escribís un `{loQueSea}`
  que el recurso no sabe completar, la consola del navegador lo avisa al cargar.

Las claves disponibles en cada ejemplo salen de sus propios datos: cada variable
compartida y cada registro aportan su `key`, y cada semáforo aporta tres —
`{nombre}` para el valor, `{nombre}_count` para el largo de la cola y
`{nombre}_queue` para la cola escrita. El ejemplo 8 agrega `{mode}`, `{flag}` y
`{switches}`; el 12, `{spin_units}`, `{spin_spin}`, `{block_units}` y
`{block_overhead}`.

**Ojo con el largo.** El pie de página crece con el texto y le come alto a los
paneles de arriba. Está acotado con un `max-height` para que no pueda estirarse
sin freno, pero un párrafo mucho más largo en el paso de cierre va a scrollear en
vez de entrar. Los que están más al límite son el 8 y el 12.

---

## Agregar o cambiar un paso

Cada ejemplo, en `EXAMPLES`, tiene una lista `steps`. Un paso es un objeto:

```js
{
  actor: "P1",
  line: 4,
  narration: "P1 copia el valor a su registro, que queda en {reg1}.",
  note: "Una aclaración opcional, en letra más chica.",
  ops: [{ op: "reg.read", reg: "reg1", from: "tp" }]
}
```

- `actor` es el proceso que ejecuta en ese paso. Decide la línea resaltada en azul
  y el estado `RUNNING`.
- `line` es el número de línea del programa de ese proceso, contando desde 1. Si
  el paso no corresponde a ninguna línea —los eventos— se omite, y entonces
  tampoco hace falta `actor`.
- `ops` es lo que cambia. Sin `ops`, el paso solo narra.

Las operaciones disponibles son estas, y no hay otras:

| `op` | Qué hace |
|---|---|
| `reg.read` | Copia una variable compartida al registro del proceso que ejecuta |
| `reg.add` | Le suma `by` al registro |
| `shared.write` | Escribe el registro en una variable compartida |
| `shared.set` | Escribe un valor literal en una variable compartida |
| `reset` | Vuelve las variables compartidas a su valor inicial y vacía los registros y los punteros de línea |
| `region.set` | Redibuja la sección crítica de cada proceso |
| `sem.wait` | Resta 1 al semáforo y, si quedó abajo de cero, bloquea al proceso que ejecuta y lo pone en la cola |
| `sem.signal` | Suma 1 al semáforo y, si quedó en cero o menos, desbloquea al primero de la cola |
| `lock.testAndSet` | Lee la variable, le escribe `true`, y de lo que leyó sale si el proceso gira o entra |
| `mode.set` | Cambia el modo del procesador y, si cambió, cuenta un cambio de modo |
| `flag.set` | Escribe el `IF` |
| `interrupt.raise` / `interrupt.service` | Prende y apaga la chapita de interrupción pendiente |
| `time.advance` | Agrega unidades de tiempo a un tablero de carriles |
| `state.set` | Fuerza el estado de un proceso, después de la derivación |
| `list.push` / `list.take` | Agrega una tarea a la lista, o saca la primera |
| `list.fill` | Deja la lista con `count` tareas de golpe; es para un salto narrado |
| `output.print` | Agrega un recuadro al panel de salida |
| `sem.set` | Le pone un valor a un semáforo sin pasar por wait ni signal. **Sólo vale en un paso sin proceso**, o sea en un salto narrado, y el recurso lo verifica |

**Los valores no se declaran: se calculan.** `reg.read` no lleva el número que va
a quedar en el registro, lo lee de la memoria compartida. Es lo que hace que el
`expectedFinalState` sea una verificación de verdad y no una copia de sí mismo, y
es lo que permite que la narración use `{reg1}` en vez de un número escrito a
mano.

**Los estados se derivan, no se declaran.** Un proceso que está en la cola de
algún semáforo está en `BLOCKED`; de los demás, el que ejecuta está en `RUNNING` y
el resto en `READY`. `state.set` es la única excepción, y existe para el último
paso del ejemplo 8, que es un evento sin `actor`.

**Y el giro también se deriva.** Los ejemplos 5, 6 y 7 declaran en `guards` la
condición de su `while` de espera, y el recurso la evalúa contra el estado para
decidir si el proceso gira o pasa de largo. En el 9 sale del resultado de
`lock.testAndSet`. Ningún paso declara «acá gira»: si la traza dijera que un
proceso pasa cuando su condición se cumple, el recurso lo avisaría por consola.

### El canvas de un ejemplo

```js
canvas: { columns: "1.85fr 0.62fr 0.72fr", panels: ["processes", "registers", "shared"] }
```

`panels` son los paneles y `columns` su ancho, en `fr`. Los de `wide` van en una
fila abajo, a todo el ancho. Además:

- `stepUnit` — el texto que dice cuál es la unidad del paso; aparece arriba a la
  derecha de la narración y **nunca puede faltar**.
- `stepUnitNote` — la nota que se muestra una vez, en el primer paso, cuando el
  ejemplo cambia la unidad.
- `processes` — los procesos, con su `label`, su `program`, su `gloss` opcional y
  su `register` si el ejemplo muestra registros.
- `shared`, `semaphores`, `mode` — el estado inicial de cada panel.
- `regions` — la sección crítica dibujada al empezar.
- `guards` — las condiciones de espera, por proceso y por línea.
- `definition` — un bloque de código que no es de nadie (el `TestAndSet` del 9).
- `boards`, `laneProcesses` y `laneNameWidth` — los tableros de carriles, sus
  procesos y el ancho de la columna de nombres (12 y 18).
- `sharedProgram` — dibuja un solo listado con una marca por proceso (sólo el 13).
- `output` — habilita el panel de salida (sólo el 14).
- `variants` — las variantes del ejemplo (14 y 18).
- `panelNotes` — una aclaración chica en el encabezado de un panel.
- `compare` — la tabla del paso de cierre.
- `mode: "tree"` y `tree` — el árbol del ejemplo 2; ver abajo.

### El árbol del ejemplo 2

El ejemplo 2 es la única excepción a «la traza está precomputada y es lineal». Es
un árbol, y también está precomputado entero al cargar: **69 nodos y 20 hojas.**
El camino es una pila de elecciones, y `Anterior` la desapila.

Los botones de elección van **adentro del panel de Procesos**, uno arriba de cada
columna, y la aclaración de para qué son va en el encabezado de ese panel
(`panelNotes`), no en la narración: es una instrucción sobre los botones y vive
donde están los botones. No son una fila del encabezado a propósito: la única
ranura de elección del chasis es la del encabezado y ahí no cabe una segunda fila.

Las dos filas de botones **siguen en pantalla en la hoja**, con los dos botones
deshabilitados y diciendo que ese proceso ya ejecutó sus tres instrucciones. Si
desaparecieran, el panel daría un salto de alto justo en el paso de cierre.

En ese ejemplo, `Siguiente`, `Último` y `Reproducir` quedan **deshabilitados**,
del mismo modo en que se deshabilitan al final de cualquier traza: el recurso no
puede saber cuál es el paso siguiente, porque eso es lo que el estudiante elige.
`Primero` y `Anterior` funcionan igual que en el resto.

Su `expectedFinalState` no es un estado: es el conjunto de valores finales
alcanzables —**−1, 0 y 1**— con la cantidad de hojas de cada uno, y el recurso lo
verifica contra el árbol al cargar. La cuenta es **9 órdenes terminan en −1, 2 en
0 y 9 en 1**, y la nota de cada hoja la dice con el número que sale del árbol, no
escrito a mano.

### Las variantes de un ejemplo

Los ejemplos 14 y 18 corren la misma situación de dos maneras:

```js
variants: [
  { variantLabel: "Como está en la diapo", semaphores: [...], steps: [...], expectedFinalState: {...} },
  { variantLabel: "Los dos en 1", semaphores: [...], steps: [...], expectedFinalState: {...} }
]
```

Cada variante se mezcla sobre el ejemplo con `Object.assign`, así que hereda todo
lo que no redefine —el programa, el canvas, la tabla de cierre, la frase de
cierre— y redefine lo que le toca. Tiene su propia lista de pasos y su propio
`expectedFinalState`, y el recurso verifica las dos por separado al abrirse.

El selector va en la **única fila de elección del encabezado**, la misma ranura
que el recurso hermano usa para su filtro. En esta mitad no hay filtro, así que la
fila estaba libre; no se agrega una segunda. Cambiar de variante vuelve al paso 1
de la variante nueva: no hay estado que sobreviva al cambio, porque cada variante
tiene sus propios snapshots calculados de antemano.

### La lista y la salida

Dos paneles que la primera mitad no necesitaba.

**La lista** es un ítem del panel de memoria compartida con `kind: "list"`, su
`capacity`, y opcionalmente `fill` y `prefix` para arrancar llena. Se dibuja como
una barra de `capacity` casilleros con los ocupados rellenos, el contero `21 de 20`
al lado del nombre, y los nombres de las tareas cuando hay seis o menos. **Lo que
se pasa de la capacidad se dibuja afuera de la barra**, después de una línea
punteada y con el casillero rayado: es la manera de que el ejemplo 16 muestre que
la lista se pasó sin que haya que creerle a un número.

**La salida** es el panel del ejemplo 14: un recuadro por `printf`, en orden, y
abajo la tirada completa sin separadores, que es donde se lee `MILANESAMILANESA`.

### Los carriles del 18

El campo `laneNameWidth` ensancha la columna de nombres para que entren las
prioridades. Y hay dos estados de carril que no son estados de proceso:
`absent` («todavía no llegó») y `done` («ya terminó»). Son acromáticos a
propósito —`absent` punteado y vacío, `done` con un relleno neutro— porque el
apunte pinta `TERMINATED` de gris y este recurso decidió no dibujar ese estado:
un bloque gris en un carril lo estaría metiendo por la ventana. Los dos llevan su
texto encima y en la referencia, así que no dicen nada sólo con el color.

---

## Lo que el recurso verifica solo al abrirse

Al final de cada ejemplo hay un `expectedFinalState`. **No es decorativo**: al
abrir la página el recurso corre las veinte trazas enteras y avisa por la consola
del navegador si algo no da. Acá importa más que en los recursos hermanos: un
intercalado mal contado por un paso sigue pareciendo plausible en pantalla.

Cada ejemplo declara el valor final de cada variable compartida, de cada
registro, de cada semáforo con su cola, el estado final de cada proceso, y cuántos
pasos se pasaron girando. Además, sin que haya que declarar nada:

- que ningún paso apunte a una línea que no existe o que no es ejecutable —las
  líneas de estructura (`while (1) {`, `}`) y las que sólo glosan su expansión no
  lo son—;
- que ningún paso tenga línea sin decir qué proceso ejecuta;
- que ningún semáforo arranque negativo;
- que en todo momento un semáforo abajo de cero tenga exactamente tantos procesos
  en la cola como su valor negado, y ninguno si está en cero o arriba;
- que un proceso esté en `BLOCKED` si y sólo si está en la cola de algún semáforo;
- que cuando un proceso gira, su próximo paso sea la misma línea;
- que todos los carriles de un tablero sumen las mismas unidades;
- que no quede ningún `{dato}` sin completar en ninguna narración, nota, frase de
  cierre o celda de tabla, y que no quede una marca `*así*` o `[[globito:así]]` sin
  cerrar;
- que toda operación exista y que apunte a un registro, una variable, un semáforo,
  un tablero o un proceso que el ejemplo declare;
- que ningún paso le sume a un registro vacío ni escriba una variable desde uno;
- que `reset` no se use en un ejemplo con semáforos, modo o carriles, que es donde
  reiniciar no alcanzaría;
- que dos cosas distintas no reclamen la misma clave de narración (una variable
  llamada igual que un semáforo, por ejemplo);
- que las condiciones de espera de `guards` lean variables que existen y apunten a
  líneas ejecutables;
- que la definición y las aclaraciones de panel que un ejemplo declara existan;
- que todo panel declarado exista y tenga globito, y que todo programa referido
  exista;
- que la región dibujada caiga sobre líneas ejecutables;
- en el ejemplo 2, que el árbol tenga los nodos, las hojas y los valores finales
  declarados, que cada jugada apunte a una línea ejecutable, y que todo valor final
  alcanzable tenga su nota de cierre.

Es la forma de darse cuenta de que un ejemplo quedó mal escrito. Y para que estas
comprobaciones no sean decorativas, se probaron **rompiendo los datos a propósito**:
veinticinco mutaciones —una operación mal escrita, un registro inexistente, una
línea que no se puede ejecutar, un dato sin completar, un semáforo negativo, el
árbol con otro conteo— y las veinticinco las reporta la consola sin que la página
se caiga. Si agregás una regla nueva, agregale su mutación.

---

## Hasta dónde llega, y por qué se queda ahí

Explica al nivel de la materia y no más. **Ser más preciso técnicamente que el
curso es un defecto, no una virtud**: la precisión de más abre preguntas que el
recurso no puede responder y deja al estudiante menos seguro que antes.

No aparece en ningún lado, ni en pantalla, ni en la narración, ni en los globitos:

- **Multiprocesamiento.** Hay una sola CPU y la ejecución es estrictamente
  secuencial. Nada de paralelismo, núcleos, coherencia de memorias intermedias,
  barreras de memoria, ni instrucciones atómicas más allá del `TestAndSet` del
  curso. La única mención a que podría haber más de una CPU es una fila de la
  tabla de cierre del 12, y está redactada apoyándose en lo que ese tablero
  muestra.
- **Assembler real.** La expansión en tres instrucciones usa la máquina abstracta
  que ya usa el curso: un registro de propósito general por proceso y notación de
  asignación. Nada de x86, nada de ARM, ningún nombre de registro de hardware que
  exista. El punto es que cada proceso tiene su propia copia del valor.
- Solapamiento de instrucciones, ejecución fuera de orden, ejecución especulativa.
- **Monitores y variables de condición.** El apunte les dedica dos filminas y son
  definiciones.
- **`pthreads`, `semaphore.h`, Go.** Esa es la API del TP, no el mecanismo.
- **La tabla de interrupciones, las direcciones de las rutinas, las prioridades de
  interrupción y la secuencia de atención por hardware.** Eso es el tema del
  recurso del ciclo; al ejemplo 8 le alcanza con el `IF` y el modo.
- **Deadlock por el orden de los `wait`.** Clase que viene, recurso que viene.
- **Inanición y equidad de la cola de un semáforo,** más allá de lo que muestra el
  ejemplo 11: `signal` desbloquea al primero que se bloqueó.
- **Salvedades del mundo real.** Ni que `fread` usa buffer, ni que un proceso de
  usuario no puede deshabilitar interrupciones de verdad, ni que el `inc` de x86
  puede ser una sola instrucción. Todas son ciertas y todas son para que alguien
  las conteste en voz alta si preguntan.

Si un ejemplo nuevo necesita algo de esta lista para funcionar, el ejemplo está
mal pensado. Hay que arreglar el ejemplo, no agregar el concepto.

### El timer del ejemplo 8 es deliberado, y es nuevo en la familia

El recurso del ciclo de instrucción **no tiene interrupción de reloj a propósito**:
lo único que la justificaba era explicar cómo el sistema operativo reparte la CPU
entre programas, y eso estaba fuera de alcance en esa clase. Para esta clase ya no
lo está: la planificación se dio en las dos clases anteriores. Así que acá el
timer entra, con nombre, y es el que hace visible por qué deshabilitar
interrupciones funciona.

Lo que **no** entra con él es el modelo de interrupciones del recurso hermano. El
cambio de modo es **un paso, no cuatro**; no hay tabla, ni direcciones, ni
prioridades. Los nombres sí se mantienen —`IF`, `MODE`— para que un estudiante que
hizo ese recurso los reconozca.

Y **el costo en multiprocesador no aparece, ni siquiera en la placa de cierre.**
Es la desventaja principal que enseña el curso, no se puede trazar en un modelo
secuencial de una sola CPU, y plantearla sin poder mostrarla abre una pregunta que
el recurso no puede cerrar.

---

## Dónde el recurso se aparta del apunte

Cuatro lugares, y conviene revisarlos con la cátedra.

1. **El ejemplo 3 usa a P1 como escritor, no a P2, y arranca con la cuenta en 0.**
   El planteo original decía «empezar con una tarea pendiente, que corra el
   decremento de P2, y que P3 imprima que no hay tareas pendientes mientras las
   hay». Esas tres cosas no pueden pasar juntas: si P3 lee 1 y P2 decrementa a 0,
   P3 compara su 1 contra 0, **no** entra al `if` y no imprime nada. Para que
   imprima la frase falsa, P3 tiene que haber leído 0 y alguien tiene que haber
   subido la cuenta después: el escritor tiene que ser el que incrementa. Lo que se
   construyó es eso, porque una frase falsa en pantalla enseña más que una frase
   que falta, y porque el `printf` de la filmina existe justamente para que se lea.
   Si la cátedra prefiere la otra versión —que P3 se calle cuando debería hablar—
   se cambian el valor inicial y tres pasos.
2. **La segunda instrucción de P3 no es un `cmp`.** El planteo la escribía
   `cmp reg₃, 0`, que es assembler real, y el techo técnico del propio planteo lo
   prohíbe. Quedó como `si reg₃ == 0, entrar al if`, y su efecto es visible: el
   puntero entra al cuerpo del `if` en vez de volver al `while`. Son las mismas
   tres instrucciones —leer, decidir, imprimir—, sin un paso que no cambie nada en
   pantalla.
3. **Los programas de los ejemplos 9, 10 y 11 van envueltos en su `while`.** Las
   filminas 16 y 18 muestran el cuerpo sin el bucle. Sin bucle, un proceso que
   ejecuta su última línea se queda sin nada que hacer, y la chapita tendría que
   decir `TERMINATED`, que es un estado que este recurso no dibuja. Con el bucle
   —que es el del ejemplo 1— nadie termina y no hace falta el cuarto estado. El
   programa del ejemplo 8 **no** se envolvió: su traza nunca llega a la última
   línea, y dos líneas más de fuente no entran en un canvas de cuatro paneles.
4. **En el ejemplo 11 los tres procesos hacen lo mismo.** El planteo pedía «tres
   procesos sobre el mismo mutex» sin decir qué hacen adentro. Los tres incrementan
   `tareasPendientes`, así que la cuenta termina en 3 y se puede señalar lo que el
   ejemplo 1 había perdido: con el mutex, los tres incrementos llegan.

Y un número del planteo que no daba: decía que el árbol del ejemplo 2 tiene
**63 nodos**. Tiene **69**. Los caminos hasta un estado (i, j) son C(i+j, i), y la
suma sobre los dieciséis estados alcanzables da 69; el árbol construido los cuenta
y el recurso lo verifica. 63 es 2⁶−1, o sea el árbol binario completo de seis
niveles, que es lo que saldría si cada paso tuviera siempre dos opciones — y no las
tiene, porque un proceso que ya ejecutó sus tres instrucciones no ofrece ninguna.

---

## Decisiones que conviene revisar

- **Toda la narración la escribió un agente y no la revisó nadie de la cátedra.**
  Es lo primero que hay que leer, línea por línea, los 214 pasos. Los programas
  salen del apunte; el español que los rodea, no.
- **El registro es impersonal o en primera del plural** («se apila», «vemos»), no
  voseante. Es el mismo criterio que el recurso de procesos, y se aparta a
  propósito del recurso del ciclo.
- **Paleta.** El recurso no estrena ningún tono: usa los del apunte y los de los
  recursos hermanos.

  | Color | Qué significa, siempre |
  |---|---|
  | Azul | Lo que está ejecutando ahora: la línea actual, `RUNNING`, el bloque de CPU ocupada |
  | Verde | `READY`: podría ejecutar y espera su turno |
  | Rojo | `BLOCKED`, y el tiempo en el que no ejecuta ningún proceso |
  | Grafito | Lo que cambió en este paso: el anillo, el borde y el texto de lo marcado |
  | Neutros | La sección crítica dibujada, los semáforos, el modo y el `IF`, las colas |

- **Girar es una trama, no un color.** Un proceso que gasta CPU en un `while` está
  ejecutando, así que no puede ser rojo, y el rojo ya está ocupado dos veces. Es
  azul con un rayado diagonal. Se lee en un proyector mediocre, que es dónde se
  muestran estos recursos, y no gasta un rol de color. Además el bloque dice
  «gira sin avanzar» y la chapita al lado del nombre dice «girando»: la trama no
  es el único que lo cuenta.
- **El panel de Semáforos no estrena color, y el de Modo y IF tampoco.** En el
  recurso de procesos el ámbar quiere decir PCB y el violeta, hilo, y los roles de
  color son chasis. Acá el valor de un semáforo es texto, la cola son chapitas
  neutras, y el rojo que se ve sale del estado del proceso que espera. En Modo y
  IF, `MODE` y `IF` son valores de texto con su glosa al lado, y la chapita de
  interrupción pendiente es acromática: la palabra `kernel` y el `IF = 0` son la
  información, y no hacía falta gastar un tono en repetirla.
- **La sección crítica se dibuja con un recuadro punteado neutro y su rótulo**, no
  con un color. La paleta no tiene tonos libres, y el punteado se distingue del
  anillo lleno que marca «cambió en este paso» por textura además de por tono.
- **El eje de tiempo del 12 es compartido entre los dos tableros**, y las columnas
  son las mismas para los dos. Es lo que hace que se vea que el segundo tablero
  termina antes: si cada tablero repartiera su ancho entre sus propias unidades,
  un bloque del segundo se vería más grande que uno del primero y la comparación
  diría lo contrario de lo que pasa.
- **El gris de los números de línea es `--muted`, no `--faint`.** Sobre blanco
  `--faint` da 4,7:1 y alcanza, pero los números de línea también caen sobre la
  línea actual, sobre el recuadro de la sección crítica y sobre la trama del giro,
  y ahí daba 4,1:1, 4,3:1 y 3,4:1 — por debajo del piso de 4,5:1. Sobre la línea
  actual van un paso más oscuros todavía. Si se toca, verificar el contraste sobre
  el fondo más claro donde aparezca, no sólo sobre blanco.
- **El ejemplo 4 tiene 22 pasos**, que es casi el doble del que le sigue. Son tres
  tandas sobre el mismo programa —intercalado afuera, intercalado adentro, y la
  región corrida una instrucción— y las tres están en el planteo. La del medio es
  una repetición literal del ejemplo 1, y está a propósito: es el mismo intercalado
  con el recuadro dibujado arriba, para que se vea dónde se rompió.

---

## Verificación antes de publicar un cambio

1. Abrirlo con doble clic, sin servidor y sin internet. Funciona igual.
2. La consola del navegador limpia, en los dieciocho ejemplos y en las cuatro
   variantes (dos del 14 y dos del 18).
3. Ningún `expectedFinalState` ni invariante reportado en la consola.
4. Recorrer cada ejemplo entero para adelante y después entero para atrás: cada
   paso intermedio se ve exactamente igual en los dos sentidos. En el 2, recorrer
   los veinte órdenes; en el 14 y el 18, las dos variantes, y cambiar de variante a
   mitad de camino y volver.
5. A 1280×720 (el proyector del aula) entra todo, sin scroll de página y sin que
   ningún panel scrollee, en los 214 pasos. Ojo con el paso de cierre: el pie crece
   con la placa y la tabla, y los paneles de arriba pierden ese alto.
6. Lo mismo con el sistema en «reducir movimiento».
7. A 390 px de ancho se lee en una columna y nada se va para el costado. Barrer
   además de 320 a 1920: la página no scrollea para el costado en ningún ancho, y
   ningún texto queda cortado.
8. Paso completo de teclado: flechas para avanzar y retroceder, `Inicio` y `Fin`
   para los extremos, `Tab` con el foco siempre visible. El ejemplo 2 se puede
   completar sólo con `Tab` y `Enter`.
9. Ningún término de la lista de más arriba aparece en la página.
10. Nada trata al estudiante de vos.
11. Contraste de todo el texto contra el fondo que de verdad le toca, calculado y
    no mirado, con piso de 4,5:1. La trama del giro y el recuadro de la sección
    crítica son los fondos que hay que revisar primero.
12. Que no quede código tapado: medir `scrollWidth` contra `clientWidth` **del
    contenedor `.source`**, no del `span` del texto. El `span` nunca se recorta
    —crece con su contenido—, así que medirlo a él da cero mientras el listado está
    cortado. Así se pasaron 64 tableros con código tapado en el 15, el 16 y el 17.
13. Que la referencia de los carriles nombre exactamente lo que hay en pantalla, ni
    más ni menos.
14. Ninguna chapita, pastilla o banda tiene su texto pegado al propio borde, y dos
    filas marcadas seguidas no se tocan.
15. Si se tocó el canvas de un ejemplo: la unidad del paso sigue declarada y
    visible, y ningún panel quedó en pantalla sin ser tema ni cambiar.

Lo medido en la construcción, para tener referencia: **856 tableros** recorridos en
cuatro configuraciones sin un desborde a 1280×720, diecisiete anchos de 320 a 1920
sin scroll horizontal de página, sin un texto cortado y sin código tapado de 1280
para arriba, **106 combinaciones** distintas de texto y fondo todas arriba del piso
de contraste, los veinte órdenes del ejemplo 2 recorridos en el navegador, el eje
del 12 y del 18 alineado al pixel en Chromium y en WebKit, y **veintinueve
mutaciones** de los datos que la consola reporta sin que la página se caiga.

**Lo que no está garantizado, y está medido:** entre 1001 y 1180 px de ancho con
una ventana de 720–768 px de alto, el panel de semáforos del 17 scrollea entre 5 y
19 px, y el listado de código de algunos ejemplos scrollea adentro de su panel. De
1180 para arriba no scrollea nada. Para comparar: `process-lifecycle` scrollea
141 px a 1024.

---

## Dónde el recurso se aparta del apunte, en la segunda mitad

- **La filmina 28 tiene un error y el recurso lo corrige.** La columna de
  Profesores termina en `wait(contImpresoras)` donde tiene que decir
  `signal(contImpresoras)`. El ejemplo 13 va con la versión corregida. Si alguien
  compara pantalla y filmina, esta línea es la que difiere.
- **El ejemplo 13 muestra un solo listado, no las dos columnas de la filmina.**
  Los programas de Alumnos y de Profesores son idénticos letra por letra; lo único
  distinto es el comentario del encabezado. Mostrarlos dos veces no enseña nada y
  no entra. Los dos grupos se nombran en la aclaración del panel y en los rótulos
  de los procesos (A1 a A4 alumnos, P1 y P2 profesores).
- **La primera columna de la filmina 31 se salta a propósito.** Es la versión sin
  ningún semáforo, o sea la lección del ejemplo 1 sobre otra variable, y en este
  recurso nada se dice dos veces. El recurso arranca en la segunda columna, que es
  la del mutex solo.
- **El semáforo del 16 y del 17 se llama `tareasPendientes`, igual que la variable
  entera con la carrera del ejemplo 1.** La colisión viene del apunte y no se
  renombra: el recurso no contradice al apunte en un identificador. La narración lo
  aclara una vez, en el primer paso del 16, y el panel lo rotula entre los
  semáforos y no entre las variables.
- **El ejemplo 16 comprime veinte vueltas del productor en un paso.** Llenar la
  lista de a una serían más de cien pasos. El paso es un evento narrado, sin
  proceso, y usa `list.fill` y `sem.set`; `sem.set` está restringido por
  verificación a pasos sin proceso justamente para que no se cuele en una traza
  normal.
- **El ejemplo 17 arranca con la lista llena**, en vez de llenarla en pantalla, por
  el mismo motivo. La narración del primer paso lo dice y el panel lo muestra.
- **Las listas del 15 y del 16 llevan capacidad 20 aunque su código no la
  controle.** La capacidad sólo la declara la filmina 31 en su cuarta columna
  (`lugarEnLista = 20`). Dibujarla desde el 15 es lo que permite que el 16 muestre
  la lista pasándose: sin barra, «se pasó» sería una afirmación de la narración y
  nada más.
- **El ejemplo 18 le pone números al planteo.** La filmina 32 da el orden de los
  hechos (`T=0` P1 toma R, `T=1` llega P3 y se bloquea, `T=2` llega P2 y desaloja)
  y no cuánto dura cada cosa. Acá P1 necesita 3 unidades adentro de su sección
  crítica, P2 necesita 4 y P3 necesita 2, elegidos para que las dos variantes den
  el mismo trabajo total —nueve unidades— y la única diferencia sea quién espera:
  P3 espera 6 unidades sin herencia y 2 con herencia. Si la cátedra tiene otros
  números, se cambian las duraciones y las dos trazas se rehacen.
