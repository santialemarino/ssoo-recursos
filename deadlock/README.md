# Deadlock

Deadlock **paso a paso**: qué lo distingue de una espera cualquiera, las cuatro
condiciones y cómo se rompen, la evasión con el estado seguro, la detección con
sus costos, y el cierre que compara las tres estrategias. Según lo que enseñe cada
ejemplo, en pantalla aparece el grafo de asignación, las columnas de código con
sus semáforos, la tira de instancias con la tabla de simulación, las matrices, los
carriles de tiempo, o los paneles propios de los dos últimos ejemplos. Siempre hay
una narración de lo que pasó en el paso. El estudiante avanza y retrocede; sólo en
el ejemplo 16 elige algo.

Se abre haciendo doble clic en `index.html`. No necesita internet ni instalar
nada.

Es el quinto recurso de la misma familia que **Ciclo de instrucción e
interrupciones**, **Procesos e hilos**, **Planificación de procesos** y
**Sincronización**, y da por sabido todo lo de esas cuatro clases: estados de
proceso y sus colores, `BLOCKED` y qué lo provoca, el planificador de corto plazo,
el cambio de contexto, `wait` y `signal` con el valor y la cola del semáforo, la
sección crítica, y busy-wait contra bloqueo.

Este archivo es la documentación del recurso: qué enseña, cómo editarlo y qué no
se puede tocar. Si vas a modificar `index.html`, leelo antes.

**No está enlazado desde la portada.**

---

## Los dieciséis ejemplos

Son **176 pasos** en total, contando las variantes. La escalera va agrupada en
cinco tramos con el nombre del tramo a la izquierda de sus botones.

| # | Tramo | Ejemplo | Qué enseña | Pasos |
|---|---|---|---|---|
| 1 | El problema | El que espera para siempre | Que un proceso puede esperar para siempre sin que haya deadlock | 6 |
| 2 | El problema | El ciclo | Qué tiene que pasar para que la espera sea mutua | 5 |
| 3 | El problema | Hay ciclo y no hay deadlock | Que el ciclo solo no alcanza cuando un recurso tiene varias instancias | 8 |
| 4 | El problema | El orden de los waits | Que el mismo código traba o no según cómo se intercale | 4 + 10 |
| 5 | Condiciones y prevención | Romper una condición | Qué pasa —y qué cuesta— al romper cada una de las condiciones | 6 + 5 + 6 + 6 + 6 |
| 6 | Condiciones y prevención | Pedir en orden | La política que rompe la espera circular sin mirar el estado | 12 |
| 7 | Evasión | Lo que cada uno puede llegar a pedir | Estado seguro y secuencia segura, con un solo tipo de recurso | 7 |
| 8 | Evasión | La petición que se bloquea con el recurso libre | Que una petición válida y disponible puede rechazarse igual | 7 |
| 9 | Evasión | Inseguro no quiere decir trabado | Qué afirma y qué no afirma un estado inseguro | 4 + 4 |
| 10 | Evasión | Varios recursos a la vez | El mismo criterio con tres tipos de recurso, y por qué hacen falta matrices | 7 + 6 |
| 11 | Detección y recuperación | Ahora sí están trabados | El mismo algoritmo mirando lo que ya se pidió, no lo que se podría pedir | 4 + 3 |
| 12 | Detección y recuperación | La víctima barata puede salir cara | Qué pasa según a quién se elija para romper el deadlock | 3 + 4 + 4 |
| 13 | Detección y recuperación | Cada cuánto correrlo | Qué se paga al elegir la frecuencia del algoritmo | 12 + 12 |
| 14 | Cierre | Deadlock contra livelock | La forma de trabarse que no se ve en el estado de los procesos | 8 + 8 |
| 15 | Cierre | En qué momento interviene cada una | Las tres estrategias comparadas sobre el mismo pedido | 4 |
| 16 | Cierre | ¿Cuál usarías? | Elegir estrategia según lo que el sistema tolera | 5 |

Cada ejemplo termina con una **placa de cierre**: una frase, en un recuadro, que
es lo que el estudiante se tiene que llevar de ese ejemplo. Está en el campo
`closing`. Y cada título de panel tiene un globito que explica qué muestra ese
panel, en `PANEL_TIPS`.

Los controles son **Primero · Anterior · Siguiente · Último · Reproducir**, con
las flechas del teclado y `Inicio` / `Fin`. Son los mismos que en los otros cuatro
recursos de la familia: si acá se agrega o se saca uno, hay que hacer lo mismo
allá. La regla está en `CLAUDE.md`.

### La escalera no se mueve al cambiar de ejemplo

**Los botones muestran sólo el número, siempre, también el del ejemplo corriente.**
Los recursos hermanos despliegan el rótulo adentro del botón corriente, y acá no,
por una razón medida: el rótulo desplegado hace que el botón cambie de ancho según
qué ejemplo esté abierto, y eso mueve a todos los botones que vienen después. Medido
sobre los dieciséis ejemplos, con el rótulo adentro **catorce de los dieciséis
botones cambiaban de lugar** al navegar, y a 1440 px la escalera pasaba de uno a dos
renglones, con lo que el encabezado crecía 37 px y se corría todo el canvas de abajo.

No se pierde nada, porque el nombre del ejemplo ya está dos renglones más abajo: el
encabezado de la narración muestra el número y el **título completo** —más completo
que el rótulo corto—, y el `title` de cada botón sigue trayendo el rótulo y la frase
de qué agrega, así que al pasar el mouse por un número se ve qué ejemplo es.

Lo otro que la escalera hace distinto es que **cada tramo es un bloque que no se
parte**: el nombre del tramo y sus botones van juntos en un `<span class="group">`.
Sin eso, un tramo podía quedar con el rótulo en un renglón y los botones en el
siguiente.

Con las dos cosas, **la escalera queda quieta**: medido en 79 anchos de 360 a
1920 px, recorriendo los dieciséis ejemplos en cada uno, ningún botón cambia de
posición —ni de renglón ni de lugar en el renglón— y el alto del encabezado no
cambia nunca. A 1280 px el encabezado mide 99 px, el mismo presupuesto que en
`synchronization`.

**Esto es una diferencia con `cpu-scheduling` y `synchronization`**, que tienen el
mismo problema y todavía no el arreglo. Portarlo son pocas líneas en cada uno, y
conviene hacerlo para que los cinco vuelvan a verse igual.

**No usa `localStorage`.** Lo único que el estudiante elige es la estrategia del
ejemplo 16, y eso es exploración, no una respuesta: no se guarda en ningún lado.

---

## Lo que este recurso hace y lo que no

**Muestra trazas ya resueltas.** Las trazas están escritas en el bloque de datos,
paso por paso, y el motor camina una lista de estados declarados. **No hay ningún
simulador de deadlock adentro**: nada se deriva corriendo un algoritmo, y por eso
retroceder es exacto.

Lo que sí se deriva de los datos, porque es lo que hace confiables a las
comprobaciones: los números que aparecen en la narración, las marcas de «esto
cambió en este paso», la referencia de colores de los carriles, y todas las
verificaciones de la última sección.

---

## El chasis se comparte; el canvas se deriva

Está también en `CLAUDE.md` porque vale para todo el repositorio, y acá está la
derivación concreta de cada ejemplo.

**No hay interruptor de granularidad, y es a propósito.** Las unidades de este
recurso no anidan: una petición no se descompone en pasos del algoritmo, y un paso
del algoritmo no se descompone en unidades de tiempo. Dos vistas no serían
igualmente honestas, así que no hay dos vistas.

En su lugar, **cada ejemplo declara su `stepUnit`**, que se ve siempre arriba a la
derecha de la narración. Cuando la unidad cambia, el ejemplo que la cambia trae un
`stepUnitNote` y lo dice una vez, con todas las letras. Pasa tres veces: en el 4
(la unidad pasa a ser la instrucción), en el 7 (pasa a ser un paso del algoritmo)
y en el 13 (pasa a ser una unidad de tiempo). El 11 también trae nota, y no porque
cambie la unidad: cambia lo que significa una columna.

| # | Unidad del paso | Lo que muta y hay que mirar | Canvas |
|---|---|---|---|
| 1 | petición o liberación | quién tiene qué y quién quedó esperando | Grafo |
| 2 | petición o liberación | lo mismo, hasta que las flechas cierran | Grafo |
| 3 | petición o liberación | cuál de las dos instancias está en el ciclo | Grafo |
| 4 | instrucción | el valor de cada semáforo y quién se bloquea | Procesos + Semáforos |
| 5 | petición o liberación | qué cambia la política, y qué se paga | Grafo |
| 6 | petición o liberación | primero la tabla de peticiones; después el ciclo | **Orden de los recursos**, y después Grafo |
| 7 | paso del algoritmo | el disponible, que se mueve en casi todos los pasos | Instancias + Simulación |
| 8 | paso del algoritmo | qué queda después de conceder | Instancias + Simulación |
| 9 | paso del algoritmo | que el máximo declarado es un techo, no una promesa | Instancias + Simulación |
| 10 | paso del algoritmo | los tres tipos de recurso a la vez | Matrices + Simulación |
| 11 | paso del algoritmo | la columna, que ahora dice otra cosa | Instancias + Simulación |
| 12 | paso del algoritmo | cuánto libera cada víctima y si alcanza | Instancias + Simulación |
| 13 | unidad de tiempo | cuánto tarda el sistema en darse cuenta | Carriles de tiempo |
| 14 | unidad de tiempo | el estado de los procesos, que es lo único que distingue los dos casos | Carriles de tiempo |
| 15 | una estrategia | en qué momento frena cada una | La vida de un pedido + Las tres estrategias |
| 16 | un caso | qué condición del enunciado decide | El caso + Por qué las otras no |

Decisiones de canvas que conviene entender antes de cambiarlas:

- **El 6 cambia de paneles a mitad del ejemplo.** Son dos movimientos: la tabla de
  peticiones, que muestra la política funcionando, y el grafo, que muestra por qué
  el ciclo no se puede formar. Cada paso declara su `stage` y el ejemplo declara en
  `stages` qué paneles tiene cada uno. Es el único ejemplo que lo hace.
- **En el 4 no hay panel de memoria compartida.** El ejemplo es sobre el orden de
  dos `wait`, y lo que muta son los semáforos. Una variable compartida sería peso
  muerto.
- **Del 7 al 12 la Simulación es un panel aparte y angosto.** Es una cuenta que
  crece fila por fila mientras el panel de Instancias muestra el estado: si
  estuviera adentro del mismo panel, el estado y la cuenta se leerían como una
  sola cosa, y en el 8 son justamente dos cosas distintas —el estado real y una
  simulación que se descarta—.
- **En el 13 el carril del sistema operativo va arriba de los de los procesos**,
  porque la pregunta del ejemplo es cuándo corre él, no qué hacen ellos.
- **En el 14 no hay carril de sistema operativo.** No hace nada en ese ejemplo, y
  un carril vacío diría que sí.
- **La referencia de colores de los carriles se arma con lo que hay en pantalla en
  ese paso**, no con una lista fija. En el 14 nunca aparece «detección», y no se
  nombra.
- **En el 16, al responder, la lista de opciones se va y los descartes ocupan la
  pantalla en dos columnas.** Las opciones dejaron de ser el tema; los descartes
  pasaron a serlo. Es el mismo movimiento que hace el último ejemplo de
  `cpu-scheduling`.

---

## Los paneles

| Panel | Muestra | Lo usan |
|---|---|---|
| Grafo de asignación | procesos en círculos, recursos en rectángulos con un punto por instancia, y las flechas de asignación y de petición | 1, 2, 3, 5, 6 |
| Procesos | una columna por proceso, con su programa y su línea actual | 4 |
| Semáforos | el valor de cada semáforo y su cola de bloqueados | 4 |
| Instancias | las doce instancias, una por casillero, y una fila por proceso | 7 a 9, 11, 12 |
| Simulación | la cuenta del algoritmo, fila por fila | 7 a 12 |
| Matrices | máximos, asignados, necesidad, y los dos vectores | 10 |
| Orden de los recursos | el número de cada recurso y la tabla de peticiones | 6 |
| Carriles de tiempo | una fila por actor y una columna por unidad de tiempo | 13, 14 |
| La vida de un pedido | los tres momentos de una petición, y dónde frena cada estrategia | 15 |
| Las tres estrategias | la tabla comparativa, que se completa columna por columna | 15 |
| El caso | el enunciado y las cuatro opciones | 16 |
| Por qué las otras no | las cuatro tarjetas con el porqué de cada una | 16 |
| Narración | un párrafo por paso, más una nota cuando hace falta | todos |

Los títulos de panel van **en mayúsculas**, como en los otros cuatro recursos. El
título es un `<button>` y el navegador le resetea `text-transform`, así que la
regla del `.tip` lleva `text-transform: inherit`. Si se copia el panel a otro lado,
esa línea va con él.

### El grafo

- **Los nodos llevan el color del estado**: un proceso que ejecuta es azul, uno
  bloqueado es rojo, uno terminado es gris, uno listo es verde. Debajo de cada
  círculo va el nombre del estado escrito, así que el color nunca es lo único que
  lo dice.
- **Las flechas son todas del mismo gris oscuro.** Lo que distingue los dos roles
  es la dirección —del recurso al proceso, lo tiene; del proceso al recurso, lo
  pidió—, el rótulo que cada flecha lleva al lado (`Asignado`, `Solicita`) y la
  referencia de abajo, que muestra las dos direcciones escritas como `R → P` y
  `P → R`. Ningún rol se cuenta con color.
- **Los rótulos van centrados sobre su propia flecha**, con un halo del color de la
  tarjeta que tapa el trazo debajo del texto. Cuando varias flechas se cruzan, poner
  todos los rótulos en el mismo punto los amontona: cada uno prueba cinco posiciones
  a lo largo de su flecha y se queda con la que más lejos cae de los ya colocados.
- **Un recurso compartible se marca con un segundo borde adentro de su caja**, y la
  referencia aparece sólo cuando hay alguno. No lleva ninguna palabra suelta al lado
  del rectángulo: quedaba flotando afuera del contenedor y se cruzaba con las
  flechas. La caja también lleva un `<title>`, así que en escritorio el texto sale
  al pasar el mouse, pero la referencia es lo que lo explica en cualquier
  dispositivo.
- **El ciclo se marca engrosando el trazo**, no cambiando el color. Es regla del
  repositorio: el color nombra una cosa, la tinta dice que cambió.
- **La flecha termina en la base de la cabeza, no en la punta.** Está en la skill
  `html-resource` con el motivo; la cabeza además escala con el grosor, así que
  las flechas del ciclo tienen cabeza más grande.
- Un recurso con más de una instancia dibuja un punto por instancia, relleno
  cuando esa instancia está asignada.

### La tira de instancias

Doce casilleros, uno por instancia, **con el nombre del proceso que la tiene
escrito adentro**. Abajo, una fila por proceso: una barra con lo asignado y, atrás,
una marca punteada con el máximo declarado (7 a 9) o con adónde llegaría si le
concedieran lo que pide (11 y 12). La columna dice cuál de las dos cosas es, y el
`stepUnitNote` del 11 avisa el cambio.

El **Disponible** va en un recuadro grande arriba a la derecha, porque es el número
que se mueve en casi todos los pasos.

---

## Cambiar un texto

Todos los textos que ve un estudiante están juntos, en el bloque
`<script id="data">`, arriba de todo del archivo. **No hay ni un texto visible
escrito en el motor ni en el dibujo.**

1. Abrí `index.html` en cualquier editor.
2. Buscá el texto tal como aparece en pantalla. Va a estar entre comillas.
3. Cambialo, guardá y recargá la página.

Dentro de los textos hay tres marcas:

- `[[deadlock:deadlock]]` — la palabra sale con un globito explicativo. Antes de
  los dos puntos va cuál de las explicaciones de `TOOLTIPS` usa; después, lo que se
  lee en pantalla.
- `*así*` — sale en negrita.
- `{disponible}`, `{P1_tiene}`, `{trabado}` — valores que el recurso completa solo.
  **Se pueden mover de lugar en la oración, pero no conviene borrarlos**, y sobre
  todo no conviene reemplazarlos por el número escrito a mano: son lo que garantiza
  que la narración diga lo que el panel muestra. Si escribís un `{loQueSea}` que el
  recurso no sabe completar, la consola del navegador lo avisa al cargar.

Las claves disponibles salen del propio paso:

| Dónde | Claves |
|---|---|
| Instancias (7 a 9, 11, 12) | `{disponible}`, `{Pn_tiene}`, `{Pn_declaro}`, `{Pn_falta}`, `{Pn_pide}`, `{pedido}` |
| Matrices (10) | `{disponible}`, `{totales}`, `{Pn_maximo}`, `{Pn_asignado}`, `{Pn_necesidad}`, `{pedido}` |
| Semáforos (4) | `{mutex}`, `{sem1}`, `{sem2}` |
| Orden (6) | `{F_R1}`, `{F_R2}`, `{F_R3}` |
| Carriles (13, 14) | `{corridas}`, `{trabado}`, `{unidad}` |

Y todas las de estado tienen su versión **del paso anterior**, agregando `_antes`:
`{disponible_antes}`, `{P2_falta_antes}`, `{P1_tiene_antes}`. Sirven para las
oraciones del tipo «le faltaban tantas y había tantas»: la cuenta vieja sale del
paso viejo y no se escribe a mano.

---

## Agregar o cambiar un paso

Cada ejemplo, en `EXAMPLES`, tiene una lista `steps`, y **cada paso declara el
estado entero**, no lo que cambió. Es más para escribir y es lo que hace que
retroceder sea exacto y que las comprobaciones sirvan de algo: si un paso declara
un estado imposible, el recurso lo dice por consola al cargar en vez de
arrastrarlo.

Según el canvas, un paso declara:

- **Grafo** — `graph: { states, held, requests, cycle, shareable }`. `held` es, por
  recurso, la lista de procesos que tienen una instancia. `requests` son pares
  `[proceso, recurso]`. `cycle` son las flechas a engrosar, escritas `"P1>R1"`.
  En el segundo movimiento del 6, el paso agrega `inequalities` —las desigualdades
  que se escriben abajo del dibujo, una por flecha— y, en el último, `chain`, que
  es la cadena de las tres.
- **Procesos y semáforos** — `actor`, `lines`, `states`, `semaphores`.
- **Instancias** — `pool: { available, rows, waiting }`, y `ledger` con las filas de
  la simulación. Una fila lleva su `label` visible y, aparte, `act` y `who`, que son
  lo que el recurso usa para rehacer la cuenta: `start`, `choose`, `finish`, `kill`
  y `none`.
- **Matrices** — `matrices: { max, assigned, need, available, waiting }` y `ledger`
  con vectores en vez de números.
- **Carriles** — nada: la variante declara el `program` entero de cada carril y cada
  paso revela una unidad más.
- **Orden** — `cells`, con el veredicto de cada petición, y `current`.

El paso puede traer además `note` (una aclaración en letra más chica),
`stage` (sólo el 6), `pedido` y `ledgerComplete`.

## Agregar un caso al ejemplo 16

Un caso es un paso más en `steps`, con esta forma:

```js
{
  narration: "Sexto caso. Lo que hay que mirar es ...",
  text: "El enunciado, tal como lo lee el estudiante.",
  correct: "evasion",
  why: {
    evasion: "Por qué es la que conviene.",
    prevencion: "Por qué se descarta.",
    deteccion: "Por qué se descarta.",
    noTratarlo: "Por qué se descarta."
  }
}
```

Las cuatro opciones son siempre las mismas y están en `options`. El recurso
verifica al cargar que `correct` sea una de ellas y que **las cuatro** tengan su
explicación: la respuesta completa no es cuál sirve, es por qué cada una de las
otras tres queda afuera.

Ojo con el alto: las cuatro tarjetas van en dos columnas y entran justas a
1280×720. Un `why` de más de unas cuatro líneas va a desbordar, y el arreglo es
acortar el texto, no apretar el panel.

---

## Lo que el recurso verifica solo al abrirse

Al abrir la página el recurso recorre las veintiocho trazas enteras y avisa por la
consola del navegador si algo no da. **No son comentarios: son campos que el código
chequea.**

- que en todos los pasos del 7 al 9, del 11 y del 12, **lo disponible más lo
  asignado dé 12**;
- que en el 10, en los dos variantes y en todos los pasos, **lo asignado más lo
  disponible dé los recursos totales** por tipo, y que **la necesidad sea la resta**
  de las peticiones máximas menos lo asignado;
- que **toda simulación declarada sea de verdad una simulación válida**: se rehace
  la cuenta fila por fila desde el estado que simula, se verifica que al elegir un
  proceso su necesidad entre en lo disponible, y que cada fila diga el número que
  da la cuenta;
- que **toda secuencia declarada completa termine con lo disponible igual al
  total**, y que ninguna fila diga «no hay secuencia» mientras todavía queda alguien
  a quien se le puede dar todo;
- que la **variante B del ejemplo 4 termine exactamente en el estado inicial**
  (`mutex = 1`, `sem1 = 1`, `sem2 = 0`);
- que un semáforo abajo de cero tenga exactamente tantos procesos en la cola como
  su valor negado, y que un proceso esté en `BLOCKED` si y sólo si está en la cola
  de alguno;
- que ningún paso apunte a una línea que no existe o que no se ejecuta;
- que **las cinco variantes del ejemplo 5 arranquen del mismo estado**;
- que **las dos variantes del 13 y las dos del 14 tengan el mismo horizonte y los
  mismos actores**;
- que en los carriles **no haya dos procesos ejecutando en la misma unidad**,
  porque hay una sola CPU;
- que los veredictos del ejemplo 6 sean los que da la política, calculados de nuevo
  a partir de los números de cada recurso;
- que toda flecha marcada como parte del ciclo esté dibujada, que nadie tenga más
  instancias de un recurso que las que el recurso tiene, y que todo proceso declare
  su estado;
- que cada ejemplo tenga `closing` y `stepUnit`, que todo panel que use tenga
  nombre y globito, y que no quede ningún `{dato}` sin completar ni ninguna marca
  `*así*` o `[[globito:así]]` sin cerrar.

Si se agrega una regla nueva, conviene romper los datos a propósito una vez para
confirmar que la consola la reporta y que la página no se cae.

---

## Hasta dónde llega, y por qué se queda ahí

Explica al nivel de la materia y no más. **Ser más preciso técnicamente que el
curso es un defecto, no una virtud.**

No aparece en ningún lado, ni en pantalla, ni en la narración, ni en los globitos:

- Grafos de espera, reducción de grafos, y cualquier técnica de detección que no
  sea la simulación que el recurso muestra.
- Los filósofos comensales ni ningún otro problema clásico con nombre.
- Bloqueo en dos fases, timestamps, y las transacciones como mecanismo. La palabra
  «transacción» aparece una sola vez, en el ejemplo 16, como parte de un sistema
  descrito.
- Memoria, paginación, swapping, marcos, fallos de página. Es la unidad que sigue.
- Sistemas de archivos, discos.
- Herencia e inversión de prioridades: son de la clase anterior y ya están en
  `synchronization`.
- Sistemas distribuidos, redes, varias máquinas.
- Internals de sistemas operativos reales.
- Medidas de tiempo en unidades reales. El tiempo acá es en unidades abstractas.
- Las aristas de declaración del grafo —la flecha punteada de «podría llegar a
  pedirlo»—. La filmina las menciona al pasar y acá no se usan.
- Si un proceso es I/O bound o CPU bound.

Tampoco se dice nunca qué entra o qué no entra en el parcial.

---

## Dónde el recurso se aparta del material, y las decisiones que se tomaron

### Del material de cátedra

1. **Los colores del grafo.** La filmina pinta la asignación de verde, la petición
   de azul y la flecha que cierra el ciclo de rojo. Acá **ninguna flecha lleva
   color**: las dos son del mismo gris oscuro y se distinguen por su dirección, por
   su rótulo y por la referencia; el ciclo se marca con el grosor del trazo; y el
   color queda reservado para los **nodos**, que llevan el estado del proceso. El
   motivo es que los tres tonos que la filmina usa en las flechas —verde, azul y
   rojo— son exactamente los que la cátedra ya tiene asignados a `READY`, `RUNNING`
   y `BLOCKED`.
2. **El ejemplo 3 enuncia una condición que la filmina no.** La filmina presenta la
   espera circular como condición necesaria *y suficiente*, sin salvedad; el resumen
   escrito de la cátedra agrega «y cada recurso involucrado tiene una sola
   instancia». El ejemplo 3 es el contraejemplo y el recurso muestra la versión con
   la salvedad. La filmina se corrige por separado.
3. **La variante B del ejemplo 10 usa valores corregidos.** La filmina dibuja
   `P2 = 0 2 1` y `P3 = 1 0 1` en la matriz de asignados; los dos están mal y se
   contradicen con la matriz de necesidad y el vector de disponibles de esa misma
   filmina. Los valores correctos son `P2 = 0 3 1` y `P3 = 1 0 0`, que son los que
   se usan acá.
4. **Los ejemplos 7 a 9 y 11 a 13 usan números que no están en el material.** La
   clase no tiene un caso de un solo tipo de recurso; estos se escribieron y se
   verificaron para este recurso.
5. **Dos líneas de narración no salen de ninguna filmina**: la del recurso
   consumible al cierre de la variante A del ejemplo 4, y la de cómo se rompe el
   livelock al cierre de la variante B del ejemplo 14.

### Del planteo con el que se construyó

6. **El ejemplo 9, variante B.** El planteo encadenaba «devuelve 10 → disponible
   12 → termina P2», y en ese momento el disponible es 10: P2 todavía retiene 2 y
   recién al terminar se llega a 12. Se construyó la única lectura que cierra, y la
   comprobación de que lo disponible más lo asignado da 12 en todos los pasos es la
   que lo fuerza. **Confirmado con la cátedra antes de escribirlo.**
7. **La tira de instancias no se tiñe por dueño: lleva el nombre del dueño
   escrito.** El planteo pedía teñirla. La paleta no tiene tonos libres —cada uno
   nombra un estado de proceso— y gastar tres en «de quién es esta instancia» los
   hacía chocar con los círculos del grafo. El casillero asignado va con relleno
   neutro y el nombre del proceso adentro, que además se lee sin depender del color.
13. **El rótulo `compartible` no va como texto al lado de la caja.** Primero se
    dibujó así y quedaba flotando fuera del contenedor, cruzándose con las flechas.
    Ahora es un segundo borde adentro de la caja, con su entrada en la referencia.
8. **En el livelock del 14 los dos procesos se alternan, no están los dos en azul
   todo el horizonte.** El planteo pedía los dos carriles azules a la vez. Toda la
   familia modela **una sola CPU**, y dos procesos ejecutando en la misma unidad de
   tiempo estaría metiendo multiprocesamiento por la ventana. Los dos alternan
   `RUNNING` y `READY`, ninguno se bloquea nunca, y los tres renglones de abajo
   —estado, CPU consumida, trabajo terminado— dicen exactamente lo que el planteo
   quería mostrar. El recurso verifica que no haya dos procesos ejecutando a la vez.
9. **En el 13, el algoritmo corre las seis veces que le tocan.** El planteo listaba
   las corridas hasta la detección (2, 4 y 6). Con «cada 2 unidades» y un horizonte
   de 12 son seis corridas, y dibujarlas todas es lo que hace visible el costo en
   CPU, que es la mitad de la comparación.
10. **Los tres renglones de abajo del 14 aparecen en el último paso.** El planteo
    los pedía abajo de los carriles, y ahí están; mostrarlos desde el primer paso
    adelantaría el final del ejemplo.
11. **El ejemplo 6 recorre la tabla por columnas**, como pedía el planteo, y la
    línea sobre que la primera petición siempre vale va en el primer paso.
12. **La escala tipográfica es la de los otros cuatro recursos.** El planteo pedía
    que nada baje de 16 px; el chasis compartido —que el mismo planteo declara no
    negociable— tiene la narración en 15,5 px, los títulos de panel en 11 px y las
    referencias en 10,5 px, en los cuatro recursos publicados. Se respetó el chasis.
    **Si la cátedra prefiere lo otro, es un cambio para los cinco recursos a la vez,
    no para este solo.**

### Decisiones de color y tipografía

**El recurso no estrena tipografía**: usa la misma pila del sistema que los otros
cuatro y que la portada. No hay ninguna fuente web.

De la paleta, lo que viene fijado por la cátedra son los estados de proceso:
`RUNNING` azul, `READY` verde, `BLOCKED` rojo, `TERMINATED` gris. Los suspendidos
no aparecen y el diagrama de Gantt tampoco. Lo que este recurso eligió:

| Rol | Tono | Por qué |
|---|---|---|
| Las dos flechas, asignación y petición | el mismo gris oscuro | los tonos que el planteo pedía son los de los estados de proceso; el rol lo dicen la dirección, el rótulo y la referencia |
| `TERMINATED` | gris, con su relleno claro | es el de la cátedra; este recurso sí lo dibuja, a diferencia de `synchronization` |
| «Cambió en este paso» | grafito acromático | regla del repositorio: el color nombra una cosa, la tinta dice que cambió |
| Instancia asignada, `idle` del sistema operativo, secciones neutras | neutros | no nombran nada del material |

El planteo pedía la flecha de asignación en verde. **Se sacó a pedido de la
cátedra**, y la razón es la buena: el verde ya significa `READY`, y un estudiante
que ve verde en un círculo y verde en una flecha tiene que aprender dos
significados para el mismo tono. Ahora ningún tono significa dos cosas en este
recurso.

---

## Verificación antes de publicar un cambio

1. Abrirlo con doble clic, sin servidor y sin internet. Funciona igual.
2. La consola del navegador limpia, en los dieciséis ejemplos y en las doce
   variantes.
3. Ningún invariante reportado en la consola.
4. Recorrer cada ejemplo entero para adelante y después entero para atrás: cada
   paso intermedio se ve exactamente igual en los dos sentidos. En los ejemplos con
   variantes, las dos —o las cinco— y cambiar de variante a mitad de camino.
5. A 1280×720 (el proyector del aula) entra todo, sin scroll de página y sin que
   ningún panel scrollee. Ojo con el ejemplo 16 respondido: es el más justo.
6. Lo mismo con el sistema en «reducir movimiento».
7. A 390 px de ancho se lee en una columna y nada se va para el costado.
8. Paso completo de teclado: flechas, `Inicio` y `Fin`, `Tab` con el foco siempre
   visible, y el ejemplo 16 respondible sin mouse.
9. Contraste de todo el texto contra el fondo que de verdad le toca, calculado y no
   mirado, con piso de 4,5:1.
10. Ningún término de la lista de más arriba aparece en la página.
11. Nada trata al estudiante de vos.

Lo medido en la construcción, para tener referencia: **176 pasos** y **28 trazas**
sin un invariante roto; **352 tableros** recorridos a 1280×720 y a 390 px sin un
desborde, con la consola limpia y sin un solo pedido de red; **3168 tableros** en
dieciocho anchos de 320 a 1920 sin scroll horizontal de página y sin texto cortado;
**148 vueltas** comparando cada paso de ida contra el mismo paso de vuelta, todas
idénticas; **81 combinaciones** distintas de texto y fondo, todas arriba del piso de
contraste; **174 marcas** con anillo sin que dos vecinas se toquen; **58 paradas de
`Tab`** todas con contorno visible; y los cinco recursos comparados elemento por
elemento del chasis, sin una sola diferencia.

---

## Lo que falta

**Toda la narración la escribió un agente y no la revisó nadie de la cátedra.** Son
176 narraciones, las notas, dieciséis frases de cierre, doce globitos de panel y
diecisiete globitos de término.

Lo que **sí** viene dado por el planteo, y está puesto palabra por palabra, son
**43 cadenas**: doce líneas de narración marcadas como textuales, las dieciséis
frases de cierre, los cinco enunciados del ejemplo 16, las doce celdas de la tabla
del 15 y la pregunta que esa tabla le hace a la prevención. Todo lo demás lo
redactó el agente. Es lo primero que hay que leer antes de mandarles el link a los
estudiantes.
