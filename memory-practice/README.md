# Práctica avanzada de memoria

Ocho ejercicios de memoria **de parcial**, resueltos paso a paso: la tabla de
segmentos reconstruida desde pistas, la segmentación paginada con límite y
permisos, adivinar el algoritmo de sustitución mirando el estado final, Clock
modificado con sustitución global, el tamaño máximo de un proceso, LRU con
tablas de dos niveles, la tabla de 2do nivel que tampoco está en memoria, y una
TLB que guarda el proceso junto con un bitmap de marcos libres.

Es la lista del documento «Práctica avanzada de Memoria» de la cátedra. Cada
ejemplo dice de qué parcial o guía viene, y las preguntas paran antes de cada
resultado para que el estudiante lo calcule primero.

Se abre haciendo doble clic en `index.html`. No necesita internet ni instalar
nada.

Da por sabido lo de **Paginación y segmentación** y **Sustitución de páginas**:
qué es una página, un marco, la tabla, el bit de presencia y los algoritmos. Acá
no se explican desde cero; se usan.

Este archivo es la documentación del recurso: qué enseña, cómo editarlo y qué no
se puede tocar. Si vas a modificar `index.html`, leelo antes.

**Todavía no está enlazado desde la portada**: se llega con el link directo.

---

## Los ocho ejemplos

Son **89 pasos** y **30 preguntas**. La escalera va agrupada en tres tramos.

| # | Tramo | Ejemplo | De dónde viene | Qué se resuelve | Pasos |
|---|---|---|---|---|---|
| 1 | Segmentación | La tabla al revés | Guía de ejercicios, ej. 11 | Bases y límites desde traducciones conocidas, el segmento de código y la fragmentación | 9 |
| 2 | Segmentación | Segmentación paginada, para ejecutar | 1er recuperatorio 2C2025 | Los bits salen de las notas; cuatro direcciones para ejecución: dos físicas, un segmentation fault y un error de protección | 16 |
| 3 | Sustitución | ¿Qué algoritmo fue? | Guía de parciales, ej. 2 | Las víctimas salen del estado final; FIFO, LRU y Clock pueden ser, Óptimo y Clock modificado no | 10 |
| 4 | Sustitución | Clock modificado, global | 2do parcial 1C2025, TM | Cuatro físicas, las vueltas del puntero y las tablas finales | 13 |
| 5 | Paginación | El tamaño máximo de un proceso | Parcial 2C2022, TM, consigna a | 4 GiB: lo pone la dirección lógica, no los marcos | 2 |
| 6 | Paginación | LRU con tablas de dos niveles | Parcial 1C2022, TM | 5 fallos y 7 accesos a disco; más marcos no ayudan; 3 accesos sin fallo y 1 con TLB | 14 |
| 7 | Paginación | Cuando la tabla tampoco está | 2do parcial 2C2022 | Fallos, tablas consultadas y disco por referencia; qué cambia con TLB; localidad espacial | 12 |
| 8 | Paginación | TLB con procesos y bitmap | Parcial 2C2018, TT | Acierto en TLB, fila de otro proceso, un 1er nivel con P = 0 y el primer marco libre | 13 |

Cada ejemplo termina con una **placa de cierre** (`closing`), y cada título de
panel tiene su globito (`PANEL_TIPS`). Los controles y las teclas son los de toda
la familia: **Primero · Anterior · Siguiente · Último · Reproducir**, las flechas,
`Inicio` y `Fin`.

### Las preguntas

Treinta pasos traen una pregunta antes de mostrar la respuesta: el paso
pregunta, y el siguiente resuelve. Funcionan igual que en **Paginación y
segmentación**: cada opción incorrecta es un error típico con su explicación
(«1023 es lo que se desperdicia, no la página», «eso sería FIFO», «la TLB guarda
traducciones, no evita fallos»), y **nunca bloquean**.

**Las respuestas se guardan en el navegador** (`localStorage`, clave
`ssoo-recursos:memory-practice:v1`). Si el navegador no deja guardar, todo
funciona igual y sólo se olvidan al recargar.

---

## Lo que el recurso verifica solo al abrirse

**No son comentarios: son campos que el código chequea**, y si algo no da lo dice
la consola del navegador.

- **El ejemplo 2 no está escrito a mano: lo traduce el recurso.** Declara la
  máquina —bits, segmentos con límite, permisos y tabla de páginas— y cada paso
  dice sólo qué dirección y hasta dónde se tradujo (`phase`: `split`, `limit`,
  `perm`, `page`, `compose`). Los campos, los controles y la física salen de la
  cuenta, y `expect` dice qué tiene que dar.
- **Las sustituciones se simulan.** El campo `facts` de los ejemplos 3, 4 y 6
  corre FIFO, LRU, Óptimo, Clock o Clock modificado sobre el estado inicial y
  las referencias, y compara las víctimas, las páginas finales, los bits, el
  puntero, los fallos y las escrituras con lo que muestra el ejemplo. En el 3,
  además, comprueba que el Óptimo y Clock modificado —con el puntero en
  cualquiera de los tres marcos— **no** llegan al estado final.
- **Las físicas y las páginas se recalculan**: cada `fact` de tipo `physical`
  hace marco × tamaño + desplazamiento, y cada `page` divide la dirección por el
  tamaño de página.
- Que los campos de bits de una dirección, pegados, den la dirección; que cada
  fila de una tabla tenga tantas celdas como columnas; que dos celdas marcadas
  no se toquen; que cada pregunta tenga su opción correcta y un `why` en cada
  opción; que no quede ningún `{dato}` sin completar; que todo globito invocado
  exista y que todo globito escrito se use.

Cuando se armó, esas verificaciones dieron todo bien, y la resolución de los
ocho ejercicios coincidió con una segunda resolución hecha por separado.

---

## El chasis se comparte; el canvas se deriva

El chasis —escalera, narración, controles, teclas, preguntas, placa de cierre,
colores de rol— es el de **Paginación y segmentación**, sin cambios. El canvas
se derivó ejemplo por ejemplo:

| # | Unidad del paso | Lo que muta y hay que mirar | Canvas |
|---|---|---|---|
| 1 | una pista | la tabla que se completa | Pistas + Dirección + Tabla de segmentos + Memoria física |
| 2 | una parte de la traducción | los controles y la página que se busca | Dirección + Tabla de segmentos + Tablas de páginas |
| 3 | una deducción | las víctimas, y qué algoritmo puede ser | Dirección + Tabla de páginas + Marcos + Resultados |
| 4 | un acceso, o la búsqueda de una víctima | los bits U y M, el puntero, las tres tablas | Dirección + Marcos + Tablas de páginas |
| 5 | una cuenta | los bits de la dirección | Dirección + Cuentas |
| 6 | una referencia | qué hay en cada marco y cuándo se usó | Referencias + Marcos + Cuentas |
| 7 | una referencia, o una parte de ella | qué tablas están en memoria | Dirección + Cuentas + Tablas de páginas + Referencias |
| 8 | una parte de la traducción | la TLB, las tablas y el bitmap | Dirección + Bitmap + TLB + Tablas de páginas |

Decisiones de canvas que conviene entender antes de cambiarlas:

- **El 4 no tiene panel de resultados.** No entraba a 1280×720 junto con las tres
  tablas de A, B y C en una sola fila, y las tablas son la respuesta de la
  consigna b. Las cuatro físicas las repite la narración del último paso.
- **En el 4 el puntero es una flecha ◀ al lado del número de marco**, y no una
  columna aparte: con la columna, las tres tablas no entraban en una fila.
- **El 6 no tiene panel de dirección.** La página sale de dividir por 8192; los
  bits no agregan nada.
- **En el 8, la tabla de 2do nivel «sin número» del enunciado aparece sólo en el
  paso 3**, que es donde sirve —para decidir que las tablas están en decimal—.
  En los demás pasos, en su lugar está la de la página 3. La narración lo dice.

---

## Decisiones que conviene confirmar con la cátedra

1. **Ejemplo 7, cómo se cuentan los accesos a tablas.** El recurso cuenta una
   consulta por tabla y por referencia: 2 por referencia, 8 en total. Si la
   cátedra cuenta también el recorrido que se repite cuando la instrucción se
   reejecuta después de cada fallo, la primera referencia da 5 y la tercera 4.
   Los fallos (3) y los accesos a disco (3) no cambian.
2. **Ejemplo 8 b, la fila 3 del 1er nivel con P = 0.** El recurso la lee como
   «la tabla de 2do nivel de la página 3 no está en memoria»: un fallo para
   traerla (al marco 20) y otro para la página (al marco 25). La física da
   001910ABh igual si se ignora esa P, pero cambia la cantidad de fallos y qué
   marcos del bitmap se usan.
3. **Ejemplo 8, decimal o hexa.** La TLB dice «valores en hexa» y las tablas no.
   El recurso las lee en decimal porque la tabla «sin número» tiene en su fila 1
   el marco 12 con P = 1, y la TLB pone la A101h en el marco Ch: cierra si esa
   tabla es la de la fila A1h.
4. **Ejemplo 6, accesos a disco.** 7 = 5 lecturas + 2 escrituras de víctimas
   modificadas. Si la cátedra cuenta sólo los fallos, son 5.
5. **Ejemplo 4, las filas que salen de memoria.** Quedan con P = 0, U = 0 y M = 0
   (la C0 se escribió en disco, así que deja de estar modificada); el número de
   marco queda escrito como estaba. La narración aclara que en esas filas sólo
   importa la P.
6. **Ejemplo 3, Clock.** «Puede ser» supone que el puntero recorre los marcos en
   orden —3, 4, 7— y arranca en el 3. Lo dice la nota del paso.
7. **Ejemplo 5.** El enunciado del documento está recortado: sólo se ve la
   consigna a. El recurso resuelve sólo ésa.

---

## Cambiar un texto

Todos los textos que ve un estudiante están en el bloque `<script id="data">`,
arriba de todo del archivo. **No hay ni un texto visible escrito en el motor ni en
el dibujo.**

1. Abrí `index.html` en cualquier editor.
2. Buscá el texto tal como aparece en pantalla.
3. Cambialo, guardá y recargá.

Dentro de los textos hay tres marcas, las mismas de los otros recursos:
`[[fallo:fallo de página]]` sale con un globito (`TOOLTIPS`), `*así*` sale en
negrita, y `{fisica}`, `{marco}`, `{desp}` son valores que completa la máquina
del ejemplo 2 —**no los reemplaces por el número escrito a mano**—.

Si cambiás un número de un ejemplo con `facts`, la consola avisa si la
simulación ya no da lo que muestran las tablas: hay que cambiar las dos cosas.

## Agregar un ejemplo

Un ejemplo es un objeto más en `EXAMPLES`, con `number`, `group`, `source`,
`title`, `short`, `adds`, `subtitle`, `stepUnit`, `canvas`, `closing` y
`steps`. Cada paso declara su `narration` y lo que cambia en cada panel
(`views`); lo que no declara, queda como estaba. Las listas `calc` y `log` se
acumulan solas. Si el ejemplo tiene números que se pueden recalcular, agregá
sus `facts`.

---

## Hasta dónde llega

Resuelve los ocho ejercicios del documento y no más. No agrega teoría: para eso
están **Paginación y segmentación** y **Sustitución de páginas**.

Tampoco se dice nunca qué entra o qué no entra en el parcial.

### Colores

El recurso no estrena nada: la tipografía, los colores de rol (página violeta,
desplazamiento verde azulado, marco ocre, segmento magenta, 1er nivel índigo) y
la marca de «cambió en este paso» en grafito son los de **Paginación y
segmentación**. Los fallos y las víctimas se marcan con tinta y `✗`, no con rojo.

---

## Verificación antes de publicar un cambio

1. Abrirlo con doble clic, sin servidor y sin internet.
2. La consola limpia, en los ocho ejemplos.
3. Recorrer cada ejemplo para adelante y para atrás: cada paso igual en los dos
   sentidos.
4. A 1280×720, nada scrollea: ni la página ni ningún panel.
5. A 390 px, una columna y nada se va para el costado.
6. Teclado: flechas, `Inicio`, `Fin`, `Tab` con el foco visible, y las preguntas
   contestables sin mouse.
7. Con el almacenamiento bloqueado, las preguntas funcionan igual.

Lo medido al construirlo, con un Chromium automatizado: **89 pasos** a 1280×720
y a 390 px sin desborde de paneles ni scroll de página, con la consola limpia y
sin un solo pedido de red; **89 vueltas** ida contra vuelta, todas idénticas; las
**87 opciones** de las 30 preguntas, contestadas una por una, sin desbordar los
paneles; las respuestas sobreviven a la recarga y el recurso anda con el
almacenamiento bloqueado. Con una respuesta incorrecta abierta, la caja de la
pregunta puede scrollear unos píxeles por dentro, igual que en **Paginación y
segmentación**.

---

## Lo que falta

**Toda la narración la escribió un agente y no la revisó nadie de la cátedra**:
las 89 narraciones y sus notas, las 30 preguntas con sus 87 explicaciones, las
ocho frases de cierre y los globitos. Lo que viene dado son los enunciados y sus
números. Las resoluciones también las hizo el agente —dos veces, por separado, y
coinciden—, pero las decisiones de arriba son las que hay que mirar primero.
