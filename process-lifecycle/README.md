# Procesos e hilos

Un proceso mirado por dentro, **paso a paso**. Según lo que enseñe cada ejemplo, en
pantalla aparecen la fuente con la línea actual, el mapa de memoria del proceso
(`text`, `data`, `heap`, `stack`), el PCB, el diagrama de estados, el contexto
cargado en la CPU, una línea de tiempo, la ocupación de memoria o la lista de
transiciones. Siempre hay una narración de lo que pasó en el paso. El estudiante
avanza y retrocede; no escribe código.

Se abre haciendo doble clic en `index.html`. No necesita internet ni instalar
nada.

Es el segundo recurso de la misma familia que **Ciclo de instrucción e
interrupciones**, y lo continúa: ahí el contexto de un programa terminaba
"guardado en una estructura del sistema operativo"; en el ejemplo 5 de acá esa
estructura tiene nombre y se ve.

Este archivo es la documentación del recurso: qué enseña, cómo editarlo y qué no
se puede tocar. Si vas a modificar `index.html`, leelo antes.

---

## Los ocho ejemplos

| # | Ejemplo | Qué enseña |
|---|---|---|
| 1 | Un programa, dos procesos | Que un programa es un archivo y un proceso es una ejecución de ese archivo |
| 2 | El stack | Que cada llamada tiene su lugar en el stack, con sus propias variables locales |
| 3 | El heap | Que el puntero vive en el stack y los bytes pedidos en el heap, y qué pasa si nadie llama a `free` |
| 4 | El proceso se bloquea | Los estados, y que bloquearse no lo decide nadie |
| 5 | Dos procesos, una CPU | El cambio de contexto, el PCB como lugar donde vive el contexto, y lo que cuesta |
| 6 | No entran todos | Suspensión, SWAP, y qué distingue a los dos estados suspendidos |
| 7 | ¿Quién decidió esto? | Los tres planificadores, y las transiciones que no decide ninguno |
| 8 | El mismo programa, con hilos | Qué se duplica cuando hay hilos y qué no |

Cada ejemplo agrega **un solo concepto** y da por sabido todo lo anterior. Cada
uno termina con una frase de cierre, en letra grande, que es lo que el estudiante
se tiene que llevar. Son 96 pasos en total.

---

## El chasis se comparte; el canvas se deriva

Esto es lo primero que hay que entender antes de agregar o mover un panel, y está
también en `CLAUDE.md` porque vale para todo el repositorio.

**El chasis es fijo y no se discute:** la escalera de ejemplos arriba, el panel de
narración, el contador de pasos, la barra de controles, las teclas, la tipografía
y los roles de color. De ahí sale que un estudiante que ya usó el recurso hermano
reconozca este. De ahí, y de nada más.

**El canvas se deriva por ejemplo:** qué paneles están en pantalla, cuánto espacio
recibe cada uno y cuál es la unidad de un paso. Un panel está cuando es el tema
del ejemplo o cuando cambia durante el ejemplo. **Los paneles se pueden ir**, y
cuando uno se va la narración lo dice una vez, en una cláusula. Nada queda en
pantalla porque se introdujo antes.

Las tres preguntas que responde cada ejemplo, y sus respuestas:

| # | Unidad del paso | Lo que muta y hay que mirar | Canvas |
|---|---|---|---|
| 1 | línea, o evento de carga | que hay dos de todo, y que el `data` de uno se mueve y el del otro no | Código fuente + 2 mapas + 2 PCB |
| 2 | línea, o entrar/salir de función | el lugar que cada llamada ocupa en el `stack` | Código fuente + mapa (stack dominante) + PCB |
| 3 | línea, o `malloc`/`free` | los bytes pedidos en el heap, el puntero en el stack, y la flecha que los une | Código fuente + mapa + PCB |
| 4 | línea, o evento | el estado, y la CPU sin nada que ejecutar | Código fuente + Estados + PCB + Línea de tiempo. **Sin mapa de memoria** |
| 5 | evento | quién tiene la CPU, y el contexto saliendo de la CPU hacia el PCB | Estados + CPU + 2 PCB + Línea de tiempo. **Sin fuente ni mapa** |
| 6 | evento | la ocupación de memoria y quién está afuera | Ocupación + Estados + 4 PCB. **Sin línea de tiempo**: el tema es el lugar, no el tiempo |
| 7 | ninguna nueva: relee la traza del 6 | la anotación de la traza, no el estado | Transiciones + Estados + Grados, con filtro por planificador |
| 8 | línea, o evento | un solo `text`/`data`/`heap` y tres `stack`; tres trazas | Código fuente con tres marcas + mapa (vuelve, es el tema) + PCB con 3 TCB |

Tres decisiones de canvas que conviene entender antes de cambiarlas:

- **En el 4 el mapa de memoria se va.** No cambia en todo el ejemplo, y el espacio
  lo necesita el diagrama de estados. La narración del primer paso lo dice.
- **En el 6 el mapa no vuelve.** Ese ejemplo es sobre imágenes enteras entrando y
  saliendo de memoria: cuatro mapas de cuatro regiones enterrarían justamente eso.
  Una barra de ocupación lo dice en un renglón.
- **En el 8 no hay línea de tiempo.** El tema es el mapa —un `text`, un `data`, un
  `heap`, tres `stack`— y necesita el alto. Que ejecute uno por vez lo dicen los
  estados de los TCB (nunca hay dos en RUNNING) y la narración. Si la cátedra
  prefiere ver el intercalado como banda de tiempo, hay que sacar otra cosa del
  canvas: no entran las dos.

Por qué está escrito acá con este detalle: el planteo original de este recurso
heredó la grilla de paneles del recurso hermano sin justificarla. Sirve para los
ejemplos 1 a 3, donde el paso es una línea y lo que muta es la memoria, y no sirve
para los que vienen después. La pista estaba en el propio planteo: la lista de
paneles no incluía un panel de CPU, y el ejemplo 5 lo necesitaba.

---

## Los paneles

| Panel | Muestra | Lo usan |
|---|---|---|
| Código fuente | el programa, con la línea actual resaltada; en el 8, una marca por hilo | 1, 2, 3, 4, 8 |
| Memoria del proceso | `text` / `data` / `heap` / libre / `stack`, con su contenido | 1, 2, 3, 8 |
| PCB | los campos que ese ejemplo declara, con los que cambiaron en ámbar; en el 8, además un TCB por hilo | todos |
| Estados | el diagrama, qué proceso está en cada estado, y la última transición marcada | 4, 5, 6, 7 |
| CPU | el contexto que está cargado en este momento, y de quién es | 5 |
| Línea de tiempo | un carril de CPU y uno por proceso; en rojo, el tiempo en que no ejecuta nadie | 4, 5 |
| Ocupación de memoria | la barra de memoria, el SWAP y los que esperan entrar | 6 |
| Quién decidió cada transición | la traza entera del 6, una fila por transición, con su decisor | 7 |
| Grados | multiprogramación y multiprocesamiento, en vivo | 7 |
| Narración | un párrafo por paso, más una nota cuando hace falta | todos |

El mapa de memoria se lee de abajo hacia arriba, como en el pizarrón: `text`
abajo, después `data`, después el `heap` —que crece hacia arriba—, el espacio
libre, y arriba el `stack`, que crece hacia abajo. El recuadro del `stack` es
siempre del mismo tamaño: lo que cambia es cuánto está ocupado. En el ejemplo 8 esa
región se divide en tres, una por hilo, dibujadas una al lado de la otra.

Los nombres de las regiones van **en inglés y en minúscula, siempre**: `text`,
`data`, `heap`, `stack`. Los de los estados van **en inglés y en mayúscula**:
`NEW`, `READY`, `RUNNING`, `BLOCKED`, `TERMINATED`, `READY SUSPENDED`,
`BLOCKED SUSPENDED`. Las glosas en español van al lado, en letra chica y siempre
visibles: en las regiones al lado del nombre, en los estados en la referencia de
abajo del diagrama. Nunca al revés: la glosa no reemplaza al nombre.

Cada título de panel tiene un globito que dice qué muestra ese panel. Es lo que
reemplaza al interruptor de glosas que había antes: para un estudiante solo, saber
qué es cada panel vale más que poder apagar cuatro palabras.

El diagrama de estados **crece y no se mueve**: cada ejemplo declara qué estados y
qué transiciones dibuja, y las posiciones están fijas en `STATES`, así que cuando
en el 6 aparecen los dos suspendidos, ninguno de los otros se corre de lugar. El
4 no dibuja `fin de quantum` porque el quantum aparece en el 5. Las etiquetas de
las flechas se ubican solas: el dibujo prueba varias distancias y elige la primera
que no pisa ningún nodo.

---

## Cambiar un texto

Todos los textos que ve un estudiante están juntos, en el bloque
`<script id="data">`, arriba de todo del archivo. No hay ni un texto visible
escrito en el motor ni en el dibujo.

1. Abrí `index.html` en cualquier editor.
2. Buscá el texto tal como aparece en pantalla. Va a estar entre comillas.
3. Cambialo, guardá y recargá la página.

Dentro de los textos hay tres marcas:

- `[[malloc:malloc]]` — la palabra sale con un globito explicativo. Antes de los
  dos puntos va cuál de las explicaciones de `TOOLTIPS` usa; después, lo que se
  lee en pantalla.
- `*así*` — sale en negrita.
- `{size}`, `{pid}`, `{used}` — valores que el recurso completa solo. Se pueden
  mover de lugar en la oración, pero no conviene borrarlas.

Las únicas etiquetas que **no** están en el bloque de datos son el `<title>` de la
página y el `<h1>`. Los nombres de los paneles sí están: en `UI_TEXT.panels`.

**Ojo con el largo.** El pie de página crece con el texto: si una narración o una
nota se hacen dos renglones más largas, el pie se estira y los paneles de arriba
pierden ese alto. La lista de verificación de abajo incluye un barrido que avisa si
algo dejó de entrar.

---

## Agregar o cambiar un paso

Cada ejemplo, en `EXAMPLES`, tiene una lista `steps`. Un paso es un objeto:

```js
{
  line: 4,
  focus: "P1",
  thread: "H2",
  narration: "P1 ejecuta la línea 4 y suma 1 a contador.",
  note: "Una aclaración opcional, en letra más chica.",
  ops: [{ op: "data.set", name: "contador", value: "1" }]
}
```

- `line` es el número de línea de la fuente, contando desde 1. Si el paso no
  corresponde a ninguna línea —los eventos— se omite.
- `focus` es el proceso que actúa en ese paso: decide la línea resaltada, la
  transición marcada en el diagrama y, en los ejemplos que lo piden, cuál de los
  procesos se atenúa.
- `thread` es el hilo que actúa, en el ejemplo 8.
- `ops` es lo que cambia. Sin `ops`, el paso solo narra.

Las operaciones disponibles son estas, y no hay otras:

| `op` | Qué hace |
|---|---|
| `process.add` | Crea un proceso: `id`, `pid`, sus globales en `data`, y opcionalmente `footprint` y `location` |
| `frame.push` / `frame.pop` | Agrega o saca el lugar de una llamada en el stack del proceso |
| `var.set` | Escribe una variable local en el lugar de la llamada de arriba; con `pointsTo` la convierte en puntero a lo pedido en el heap y dibuja la flecha |
| `data.set` | Escribe una variable global |
| `heap.set` | Reemplaza el heap entero por la lista `segments` que se le pasa |
| `state.set` | Cambia el estado del proceso |
| `location.set` | Mueve la imagen del proceso: `pending`, `memory` o `swap` |
| `cpu.set` / `cpu.clear` | Carga o vacía el contexto de la CPU |
| `context.set` | Escribe directamente el contexto guardado en un PCB; sirve para el estado inicial |
| `context.save` | Copia el contexto de la CPU al PCB del proceso, y deja la CPU vacía |
| `context.load` | Copia el contexto del PCB a la CPU, y deja vacío el casillero del PCB |
| `time.advance` | Avanza `units` unidades: `cpu: { kind, label }` describe el carril de la CPU (`busy`, `overhead`, `idle`) y cada proceso recibe una banda con el estado que tiene en ese momento |
| `thread.add` | Crea un hilo: `id`, `state`, `line` y sus `frames` |
| `thread.state` | Cambia el estado de un hilo |
| `thread.frame.push` | Agrega el lugar de una llamada en el stack de un hilo |
| `thread.var.set` | Escribe una variable local en la llamada de arriba de un hilo |

Dos cosas de `time.advance` que importan:

- **El orden de los `ops` dentro del paso decide a qué estado se le imputa el
  tiempo.** En los pasos de un cambio de contexto, `time.advance` va **primero**,
  antes del `state.set`: así el proceso que entra recién aparece `RUNNING` cuando
  el cambio terminó, y no durante. Al revés, la línea de tiempo diría que un
  proceso ejecutaba mientras el carril de la CPU dice que no ejecutaba nadie.
- Los carriles se mantienen sincronizados solos, y el recurso lo verifica.

`heap.set` es a propósito así, explícita: el recurso **no simula un administrador
de memoria**. Cada paso declara cómo queda el heap, pedido por pedido
(`kind: "block"` o `kind: "hole"`, con `address`, `size`, y opcionalmente `value`
y `lost`). Es más largo de escribir y mucho más fácil de leer y de corregir.

### El canvas de un ejemplo

```js
canvas: { columns: "0.8fr 1.3fr 0.72fr", panels: ["source", "states", "pcb"], wide: ["timeline"] }
```

`panels` son los paneles de la fila de arriba y `columns` es su ancho, en `fr`.
Los de `wide` van en una fila abajo, a todo el ancho. Además:

- `stepUnit` — el texto que dice cuál es la unidad del paso; aparece arriba a la
  derecha de la narración y **nunca puede faltar**.
- `pcbFields` — qué campos muestra el PCB en ese ejemplo, en orden.
- `pcbLayout: "side"` — las fichas del PCB en dos columnas en vez de una.
- `states` y `transitions` — qué dibuja el diagrama de estados.
- `diagramViewBox` — el recorte del diagrama; los ejemplos 6 y 7 lo agrandan hacia
  abajo para que entren los dos estados suspendidos sin mover los otros cinco.
- `filter: true` — habilita el filtro por planificador y colorea las flechas por
  decisor (solo el 7).
- `capacity` — el tamaño de la memoria, en unidades (solo 6 y 7).
- `sameTraceAs: 6` — declara que la traza es la misma que la de otro ejemplo, y el
  recurso lo verifica op por op.
- `dimInactive: true` — atenúa lo que no es del proceso en foco.
- `addressNote: true` — aclara en el encabezado que las direcciones son de ejemplo.
- `compare` — la tabla de cierre de los ejemplos 3 y 8.

### Lo que el recurso verifica solo al abrirse

Al final de cada ejemplo hay un `expectedFinalState`. **No es decorativo**: al
abrir la página el recurso corre los ocho ejemplos enteros y avisa por la consola
del navegador si algo no da. Chequea procesos, llamadas en curso, pedidos vivos, valor
de cada variable global, estado final, ubicación final, hilos y sus estados,
profundidad máxima del stack, unidades de tiempo, quién quedó con la CPU y cuántas
unidades de memoria quedaron ocupadas. Además, sin que haya que declarar nada:

- que ningún paso apunte a una línea que no existe o que está vacía;
- que ninguna operación toque un proceso que todavía no existe;
- que todo estado usado exista en `STATES`, y que esté declarado en el ejemplo si
  ese ejemplo dibuja el diagrama;
- que toda transición que ocurre exista como flecha, y esté declarada;
- que la memoria nunca tenga más unidades ocupadas que su capacidad;
- que un proceso suspendido esté en el SWAP, uno en NEW esperando entrar, y
  cualquier otro en memoria;
- que todos los carriles de la línea de tiempo sumen lo mismo;
- que la traza del 7 siga siendo idéntica a la del 6.

Es la forma de darse cuenta de que un ejemplo quedó mal escrito.

---

## Qué no se puede tocar

- **El programa del ejemplo 2** está copiado del apunte, tal cual, con los mismos
  nombres (`hacer_algo`, `tope`, `nuevo_tope`). Que sea idéntico al de la filmina
  es el punto.
- **Las frases de cierre** de cada ejemplo. Están acordadas.
- **Los nombres de las regiones y de los estados**, en inglés, escritos igual que
  en el apunte.
- **Los colores de los estados**, que salen del apunte y viven en `STATES`: rosa
  `NEW`, verde `READY`, azul `RUNNING`, rojo `BLOCKED`, gris `TERMINATED`,
  amarillo `READY SUSPENDED`, naranja `BLOCKED SUSPENDED`. El violeta es de todo
  lo que pertenece a un hilo.
- **Un solo procesador y ejecución estrictamente secuencial.** No hay paralelismo
  real en ningún ejemplo, ni lo va a haber. En el 8, nunca hay dos hilos en
  RUNNING al mismo tiempo.
- **`fin de quantum` es una etiqueta y nada más.** La única explicación permitida
  es "el tiempo que le tocaba".
- **La palabra *planificador* no aparece antes del ejemplo 7.** Hasta ahí, las
  transiciones que alguien decide dicen "lo elige el SO".
- **Los hilos del ejemplo 8 no escriben una variable compartida.** Leen la misma
  global y cada uno escribe solo la suya. Eso es a propósito: escribir la misma
  variable abre la pregunta de la sincronización, que es de otra clase.
- **En el ejemplo 8 cada hilo corre hasta bloquearse**, y el cambio de hilo lo
  provoca siempre un bloqueo, nunca un corte arbitrario. Así no queda lugar para la
  pregunta "¿por qué este hilo no terminó antes de que arranque el otro?": ninguno
  se interrumpe a mitad de camino. Que también podrían cortarse por turno lo dice
  una nota, apoyada en el ejemplo 5.

---

## Hasta dónde llega, y por qué se queda ahí

Explica al nivel de la materia y no más. **Ser más preciso técnicamente que el
curso es un defecto, no una virtud**: la precisión de más abre preguntas que el
recurso no puede responder y deja al estudiante menos seguro que antes.

No aparece en ningún lado, ni en pantalla, ni en la narración, ni en los
globitos:

- Ningún algoritmo de planificación con nombre. El 7 dice qué decide cada
  planificador, nunca cómo elige.
- Hilos de usuario contra hilos de kernel, bibliotecas de hilos.
- Nada de sincronización: secciones críticas, condiciones de carrera, semáforos,
  mutex, atomicidad.
- Memoria virtual, paginación, fallos de página, direcciones lógicas contra
  físicas. El SWAP es "un lugar del disco donde se estaciona un proceso suspendido
  entero", y nada más.
- Memorias intermedias, solapamiento de instrucciones, varios procesadores.
- Sistemas de archivos.
- `fork`, `exec`, `wait`, procesos hijos, árboles de procesos.
- Internas del compilador y del enlazador, convenciones de llamada.
- Direcciones reales. Las que se ven son números chicos de ejemplo, y el panel lo
  dice.

Si un ejemplo nuevo necesita algo de esta lista para funcionar, el ejemplo está
mal pensado. Hay que arreglar el ejemplo, no agregar el concepto.

---

## Decisiones que conviene revisar

- **La narración la escribió un agente y todavía no la revisó nadie de la
  cátedra.** Es lo primero que hay que leer línea por línea, los 96 pasos.
- **No se dice *marco*, ni *frame*, ni *bloque*.** Ninguna de las tres aparece en
  la página. Las tres nombran cosas que una clase posterior vuelve a definir
  (marcos de página, bloques de disco), y el recurso hermano sostiene esa línea sin
  una sola excepción. En su lugar se nombra lo que realmente está pasando: *la
  llamada* y *su lugar en el stack*, y *los bytes pedidos* en vez de *bloque*. El
  campo del PCB dice `llamadas en curso` y `pedidos sin liberar`, que además
  explican qué es ese número. Si la cátedra prefiere usar su propia palabra, se
  cambia en la narración y en dos rótulos.
- **Los lugares del stack llevan la llamada en el nombre**: `hacer_algo(3)`,
  `hacer_algo(2)`. Es la forma de mostrar que los cuatro `tope` son cuatro
  variables distintas sin inventar subíndices que el programa no tiene, y sin
  gastar color.
- **El favicon es el mismo en los tres archivos** (índice y los dos recursos).
  Mientras la cátedra no tenga identidad visual, un ícono por recurso es una
  decisión que nadie tomó; cuando la tenga, se cambia en tres lugares.
- **No hay interruptor de glosas.** Las glosas quedan siempre visibles y el lugar
  del interruptor lo ocupa algo más útil: un globito por panel que explica qué
  muestra ese panel. Las glosas de las regiones no son traducciones —`stack` dice
  *variables locales*, no *pila*—, así que son contenido y no ocupan alto: van en
  el mismo renglón del nombre. Las de los estados sí son casi traducciones, y se
  quedan porque la referencia de abajo del diagrama es el único lugar donde
  `READY SUSPENDED` dice *fuera de memoria*, y porque es la clave de color de los
  carriles de la línea de tiempo.
- **La identidad del proceso va en el encabezado del panel** cuando hay uno solo
  (`Memoria del proceso P1 · PID 41`), y como fila arriba de cada columna cuando
  hay más de uno. Es lo que dejó lugar para que el stack respire.
- **El registro es impersonal o en primera del plural** ("se apila", "vemos"), no
  voseante. Se aparta a propósito del recurso hermano, que le habla al estudiante
  de vos.
- **Paleta.** La cátedra no tiene identidad visual definida. Este recurso usa la
  del recurso hermano y le agrega lo mínimo:

  | Color | Qué significa, siempre |
  |---|---|
  | Azul | Lo que se está ejecutando ahora: la línea actual, la llamada de arriba, la CPU ocupada |
  | Ámbar | Lo que cambió en este paso |
  | Rojo | Tiempo en el que no ejecuta ningún proceso, y el estado `BLOCKED` |
  | Violeta | Todo lo que pertenece a un hilo: su stack, su TCB, su marca en la fuente |
  | Gris oscuro | La flecha de un puntero a lo que apunta en el heap |
  | Neutros | Las cuatro regiones de memoria y las cajas de la barra de ocupación |

- **Los colores de los cuatro decisores del ejemplo 7 los elegí yo** y no salen de
  ningún apunte: azul marino para el de largo plazo, verde petróleo para el de
  mediano, magenta para el de corto, gris para "nadie". Están fuera de la paleta
  de estados a propósito, para que no se confundan con ellos. Cada fila y cada
  botón del filtro dicen el nombre además del color.
- **Los dos amarillos/naranjas de los estados suspendidos** (`#a06000` y
  `#b5510a`) son oscuros para que se lean sobre blanco. El apunte dice "amarillo"
  y "naranja"; si la cátedra los quiere más brillantes, hay que revisar el
  contraste del texto.
- **El contexto se muestra como `IP`, `AX` y `SP`**, con valores de ejemplo. Son
  los nombres del recurso hermano, a propósito: el ejemplo 5 es el que empalma con
  él. No se explica cómo una línea de C deja un valor en `AX`.
- **El programa del ejemplo 1 no imprime.** El planteo decía "un contador que
  incrementa una global y la imprime". Imprimir es una llamada, y una llamada
  ocupa lugar en el stack: sería meter el concepto del ejemplo 2 en el ejemplo 1.
- **El ejemplo 8 usa `crear_hilo(contar, 20)`**, que no es una función real de C.
  Es un nombre genérico a propósito: nombrar una biblioteca de hilos está fuera
  del alcance. Si la cátedra prefiere el nombre real, es un cambio de una línea en
  `PROGRAMS`.
- **En el ejemplo 8 hay tres hilos y tres stacks, y uno de los tres es el hilo
  principal** (el que corre `main`, llamado H1). El planteo pedía "tres stacks":
  hacer que `main` cree tres hilos más daría cuatro, y esconder el stack de `main`
  sería mentir.
- **El ejemplo 6 tiene una tercera ubicación, "esperando entrar"**, para los
  procesos en NEW que todavía no fueron admitidos. Sin eso, un proceso en NEW
  aparecería dentro del SWAP, que es otra cosa.
- **El diagrama del 6 y del 7 no dibuja la flecha de `BLOCKED SUSPENDED` a
  `BLOCKED`.** Existe en el diagrama completo del apunte, pero esta traza no la
  usa y, dibujada, apiña esa zona. Si la cátedra la quiere siempre visible, se
  agrega a `transitions`.
- **El campo `estado` del PCB aparece en el ejemplo 4** y el `contexto` en el 5, no
  antes.
- **En el ejemplo 1 no se atenúa ningún proceso**, y la etiqueta "activo en este
  paso" solo aparece cuando hay más de un proceso en pantalla.
- **El ejemplo 4 le da a `READY` cero unidades de tiempo** entre que termina la
  I/O y que le devuelven la CPU: no pasó tiempo medible. El diagrama sí muestra
  `READY` en ese paso.

---

## Publicación

El recurso está **enlazado desde el índice de la raíz**, así que un estudiante que
entra a la portada lo ve y lo abre.

Si en algún momento hay que volver a esconderlo —o cuando entre un recurso nuevo
que todavía no se dio en clase— el mecanismo es el de siempre: en `index.html` de
la raíz la tarjeta pasa de `<a class="card" href="...">` a
`<div class="card soon">` con un `<span class="soon-chip">próximamente</span>` en
el nombre, y en el `README.md` de la raíz la fila lleva *(sin publicar todavía)*.
El estilo de esa tarjeta inerte queda en el índice, listo para el próximo recurso.
Nada de fechas ni de explicaciones: el `próximamente` alcanza.

---

## Verificación antes de publicar un cambio

1. Abrirlo con doble clic, sin servidor y sin internet. Funciona igual.
2. La consola del navegador limpia, en los ocho ejemplos.
3. Ningún `expectedFinalState` ni invariante reportado en la consola.
4. Recorrer cada ejemplo entero para adelante y después entero para atrás: cada
   paso intermedio se ve exactamente igual en los dos sentidos.
5. A 1280×720 (el proyector del aula) entra todo, sin scroll, en los 96 pasos.
6. **Lo mismo con el sistema en "reducir movimiento".** El pie de página se estira
   con el texto, así que un cambio de copy puede hacer que algo deje de entrar, y
   conviene mirarlo en los dos modos.
7. A 390 px de ancho se lee en una columna y nada se va para el costado.
8. Paso completo de teclado: flechas para avanzar y retroceder, `Inicio` y `Fin`
   para los extremos, `Tab` con el foco siempre visible.
9. Ningún término de la lista de más arriba aparece en la página, y *planificador*
   sigue apareciendo solo en el ejemplo 7.
10. Nada trata al estudiante de vos.
11. Si se tocó el canvas de un ejemplo: la unidad del paso sigue declarada y
    visible, y ningún panel quedó en pantalla sin ser tema ni cambiar.
