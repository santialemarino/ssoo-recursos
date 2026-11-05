# Memoria

La teoría de memoria **en el orden de la cátedra**, con los números de las filminas
cuando las filminas los traen: la ligadura de direcciones y los registros base y
límite, las particiones fijas y dinámicas, la paginación y la TLB, las tablas
jerárquica e invertida, la segmentación simple y la paginada, el page fault con todas
sus variantes, las políticas de asignación y de sustitución, el thrashing y el
conjunto de trabajo, y los cinco algoritmos de sustitución.

Es el recurso de **teoría**, y va antes que la **Práctica de memoria**: explica cada
mecanismo una vez. Las cuentas de la guía y de los parciales están en la práctica.

Se abre haciendo doble clic en `index.html`. No necesita internet ni instalar nada.

**Todavía no está enlazado desde la portada**: figura como «próximamente», y se
publica junto con la práctica, después de la clase de memoria virtual.

---

## Los doce ejemplos

Son **229 pasos** contando todas las variantes, y **8 preguntas**. El orden es el de
las filminas, y ningún ejemplo usa algo que se explica en uno posterior.

| # | Tramo | Ejemplo | Variantes | Unidad del paso | Pasos |
|---|---|---|---|---|---|
| 1 | Contigua | Base y límite | — | un momento (compilar, cargar, ejecutar, mover); después, una referencia | 8 |
| 2 | Contigua | Particiones | Fijas · Dinámicas | un evento: una llegada, una salida o la compactación | 5 + 9 |
| 3 | Paginación | De página a marco | Por división · Por bits | una parte de la cuenta | 7 + 7 |
| 4 | Paginación | TLB | — | una referencia | 8 |
| 5 | Tablas grandes | Paginación jerárquica | — | una cuenta; después, una parte del recorrido | 9 |
| 6 | Tablas grandes | Tabla invertida | Búsqueda lineal · Con hash | una parte de la búsqueda | 6 + 6 |
| 7 | Segmentación | Segmentación simple | — | una referencia | 6 |
| 8 | Segmentación | Segmentación paginada | — | una parte de la traducción; al final, la comparación | 9 |
| 9 | Memoria virtual | El page fault | Hay marco libre · Víctima sin modificar · Víctima modificada · Dirección inválida · Víctima con I/O en curso | una etapa de la atención | 9 + 10 + 14 + 4 + 5 |
| 10 | Memoria virtual | Políticas | Local · fija · Global · dinámica | una referencia | 13 + 13 |
| 11 | Memoria virtual | Thrashing | — | un intento (caso A); un momento del ciclo (caso B); una posición de la ventana | 14 |
| 12 | Sustitución | Algoritmos de sustitución | FIFO · Óptimo · LRU · Clock · Clock modificado | una referencia | 15 + 13 + 13 + 13 + 13 |

Cada ejemplo termina con una **placa de cierre**; las frases de las doce placas
vienen del planteo, palabra por palabra. Cada título de panel tiene su globito.

### Qué hay en cada uno

1. **Base y límite.** La variable en la 500 ligada al compilar (3500), al cargar (500
   → 3500) y al ejecutar (la MMU suma la base). La MMU compara `DL < LÍMITE` antes de
   sumar: con base 3000 y límite 1200, la 500 va a 3500, la 1199 a 4199 —la última
   válida— y la 1200 da Segmentation Fault, porque la comparación es estricta.
   Después el proceso se mueve a la 6000: sólo la ligadura en ejecución sobrevive, y
   la 500 va a 6500.
2. **Particiones.** 64 KiB para los usuarios (el SO va aparte y no se cuenta), y A 8,
   B 10, C 6, D 5, E 9, F 16 y G 4 KiB. **Fijas**: cuatro de 16 KiB, 35 KiB de
   fragmentación interna, E espera y uno de 20 KiB no entra nunca. **Dinámicas**, por
   primer ajuste: salen B, D y F (37 KiB libres en cuatro huecos), los cuatro
   criterios eligen cuatro huecos distintos para un proceso hipotético de 5 KiB, H no
   entra, sale E y se consolida un hueco de 30, H entra, I no entra con 26 libres, se
   compacta —con la pregunta de la filmina, «¿Siempre es posible?»— e I entra en 38 –
   60.
3. **De página a marco.** El ejemplo de la filmina: páginas de 1 KiB, la tabla 0 → 5,
   1 → 6, 2 → 2 y las 3, 4 y 5 con V = 0. Primero la carga, con el bitmap de marcos
   libres; después la 2045, que va a 7165 —dividiendo, o pegando bits— y la 4100, que
   cae en una fila con V = 0. El PTBR está en pantalla como una flecha con su globito.
4. **TLB.** La misma tabla, y las referencias 2045, 100, 1500, 300 y 1030: dos miss y
   tres hit, 10 accesos a memoria sin TLB y 7 con TLB, y la cuenta de la filmina,
   `0,98 · (20 ns + 100 ns) + 0,02 · (20 ns + 100 ns + 100 ns) = 122 ns`.
5. **Paginación jerárquica.** La tabla plana de 16 MiB contiguos con páginas de
   1 KiB; después el esquema de la filmina, 10 | 10 | 12 con páginas de 4 KiB, el PTPR,
   la tabla de páginas global y la tabla de páginas m, y la 0x00403A7C que va a
   0x2B5A7C en tres accesos. Lo que ahorra: 12 KiB contra 4 MiB.
6. **Tabla invertida.** La misma búsqueda de la página 1 de A, recorriendo fila por
   fila (seis filas) y con el hash (dos). Al final, las ventajas y desventajas de la
   filmina.
7. **Segmentación simple.** CS, DS y SS con sus permisos, cuatro referencias —una
   física, un Segmentation Fault, un error de protección y el último byte de la pila—,
   y la fragmentación externa de vuelta.
8. **Segmentación paginada.** Una tabla de páginas por segmento, y al final la tabla
   comparativa de la filmina.
9. **El page fault.** La paginación bajo demanda, el detalle de la fila con P = 0 que
   todavía dice un marco, y la atención de la filmina con quién hace cada etapa (MMU,
   SO, DMA), en cinco variantes. La de la víctima modificada termina con el page
   buffering; la de I/O en curso muestra por qué hace falta el bit de lockeo.
10. **Políticas.** La matriz de las cuatro combinaciones, con la que no existe, y la
    misma secuencia en local · fija (A 6 page faults, B 1) y en global · dinámica (A 3,
    B 1). La víctima es «la que entró primero», sin nombrar a FIFO: los algoritmos
    vienen en el ejemplo 12.
11. **Thrashing.** El caso A de la filmina (la instrucción que nunca se ejecuta), el
    caso B (el ciclo del sistema entero) con su curva, y el conjunto de trabajo en t1
    (7 ≤ 8) y en t2 (11 > 8).
12. **Algoritmos de sustitución**, sólo para mirar: la secuencia de la filmina con
    los cinco algoritmos. FIFO cierra con la anomalía de Belady.

---

## De dónde sale cada número, y qué lo verifica

**No son comentarios: son campos que el código chequea al abrir**, y si algo no da lo
dice la consola del navegador.

| Ejemplo | Números | De dónde | Qué lo verifica |
|---|---|---|---|
| 1 | base 3000, límite 1200; 500 → 3500, 1199 → 4199, 1200 → Segmentation Fault; base 6000, 500 → 6500 | inventados | la máquina `relocation` calcula cada traducción y la compara con `expect`; `facts` de tipo `relocation` |
| 2, fijas | particiones de 16 KiB, interna 35 KiB | inventados | `facts` de tipo `fixedPartitions` |
| 2, dinámicas | cada ubicación, los huecos y los libres de cada paso, el puntero en 58, los cuatro criterios, H e I rechazados, la compactación | inventados | `facts` de tipo `dynamicPartitions`: simula primer ajuste y compara cada punto |
| 3 | tabla 0 → 5, 1 → 6, 2 → 2; 2045 → 7165; bitmap final | filmina 16 a 18 | la máquina `paging` traduce; `expect` 7165 e `invalid` |
| 3 | 4100 → V = 0 | inventado | ídem |
| 4 | 2045, 100, 1500, 300, 1030 → 7165, 5220, 6620, 5420, 6150; 10 y 7 accesos | inventados | la máquina `tlb` simula la TLB; `expect` de cada referencia y del total |
| 4 | 122 ns | filmina 21 | `facts` de tipo `eat` |
| 5 | 16 MiB | filmina 23 | `facts` de tipo `equals` |
| 5 | 0x00403A7C → 1, 3, 0xA7C → 0x2B5 → 0x2B5A7C; 4 MiB contra 12 KiB | inventados | `hierCheck` y `facts` de tipo `equals` |
| 6 | la tabla invertida, el hash, 5420 | inventados (de Paginación y segmentación) | la máquina `inverted` busca, de las dos formas |
| 7 | los tres segmentos y las cuatro referencias | inventados | la máquina `segments`; `facts` de tipo `holes` para los huecos y el pedido de 1600 |
| 8 | la segmentación paginada | inventados (de Paginación y segmentación) | la máquina `segPaging` |
| 10 | A 6 / B 1 y A 3 / B 1 | inventados | la máquina `policy` simula los marcos de los dos procesos |
| 11 | la traza del caso A | filmina 12 | `facts` de tipo `thrash` |
| 11 | las tres secuencias, t1 y t2 | filminas 15 y 16 | `facts` de tipo `workingSet` |
| 12 | las cinco tablas, celda por celda, los page faults y los accesos a disco por referencia | filminas 17 a 23 | el simulador de sustitución y `expect` de cada variante |
| 12 | Belady: 9 y 10 page faults | bibliografía | ídem |

Además: que los campos de bits, pegados, den la dirección; que cada pregunta tenga su
opción correcta y un `why` en cada opción; que no quede ningún `{dato}` sin completar;
que todo globito invocado exista y que todo globito escrito se use; que cada panel
tenga nombre, globito y algo que mostrar en cada paso; y que **cuando un panel sale de
la pantalla, la narración lo diga**.

**El simulador de sustitución es el mismo de la Práctica de memoria**, con la misma
regla de empate en Óptimo.

### Datos inventados

Todo lo que la tabla de arriba marca como inventado. En los ejemplos 6 y 8 son los de
**Paginación y segmentación**, sin cambios; en el 6 se sacó la búsqueda de la página 6
de B, que terminaba en un fallo de página antes de que el recurso lo explique.

### La tabla comparativa del ejemplo 8

En la filmina 33 la tabla se sale de la diapositiva y la fila de segmentación queda
cortada. Las otras tres filas están transcriptas tal cual; **la de segmentación se
completó con la filmina 29**.

### La secuencia de Belady

**No es de las filminas**: la filmina 17 nombra la anomalía pero no trae un ejemplo.
La secuencia 1 2 3 4 1 2 5 1 2 3 4 5 es la de la bibliografía.

---

## El chasis se comparte; el canvas se deriva

El chasis es el de **Deadlock**, entero, y se comparó propiedad por propiedad: la
escalera, los controles, las teclas, el globito en cada título de panel, la placa de
cierre y el renglón de elecciones. Ese renglón comparte línea con el título, como en
la práctica, y queda reservado aunque el ejemplo no tenga variantes: nada se mueve al
cambiar de ejemplo.

El canvas se derivó ejemplo por ejemplo, y **cambia dentro de un ejemplo** cuando
cambia lo que hay que mirar. Cada vez que un panel sale, la narración lo dice una vez.

| # | Lo que muta y hay que mirar | Canvas |
|---|---|---|
| 1 | la dirección por compilador, cargador y MMU; después la comparación y la suma de la MMU | Ligadura + Memoria; desde la ejecución, también la MMU |
| 2 | la barra de memoria y qué le pasa a cada proceso | Memoria + Procesos |
| 3 | los campos de la dirección, la fila de la tabla, la física | Dirección + Tabla de páginas + Memoria, con el bitmap mientras se carga |
| 4 | la TLB y los accesos | Dirección + TLB + Tabla de páginas + Accesos; al final, las cuentas |
| 5 | las cuentas; después el recorrido | Dirección + Cuentas; Dirección + Tabla global + Tabla m; al final, otra vez las cuentas |
| 6 | la búsqueda | Dirección + Tabla invertida, y la de hash en su variante |
| 7 | la MMU y el mapa | Dirección + Tabla de segmentos + Memoria; al final, sin la dirección |
| 8 | las dos tablas | Dirección + Tabla de segmentos + Tablas de páginas + Memoria; al final, la comparación sola |
| 9 | quién hace qué, la tabla, la TLB, los marcos | Quién hace qué y Memoria + Tabla de páginas + TLB + Atención; en el page buffering, el tiempo de espera en lugar de la atención; en la de I/O en curso, los marcos con su bit de lockeo |
| 10 | de quién es cada marco, y los page faults | Políticas + Marcos |
| 11 | los marcos y los intentos; el ciclo y la curva; la ventana | tres canvas, uno por caso |
| 12 | la tabla de marcos y lo que mira el algoritmo | Marcos + Estado; en Belady, las dos tablas lado a lado |

## Cambiar un texto

Todos los textos que ve un estudiante están en el bloque `<script id="data">`. Las
marcas son las de toda la familia: `[[pageFault:page fault]]` sale con globito,
`*así*` en negrita, y `{fisica}`, `{marco}`, `{texto}` los completa el recurso —**no
los reemplaces por el número escrito a mano**—. La narración de cada referencia del
ejemplo 12 se arma con las plantillas de `UI_TEXT.narr`; la del 10, con
`UI_TEXT.policy`.

## Hasta dónde llega

Explica cada mecanismo al nivel de las filminas, y no más: dos niveles de tabla y no
más; la TLB sin su tamaño ni su reemplazo; ninguna caché más que la TLB; ningún
algoritmo de asignación de bloques; ninguna arquitectura real; ningún registro fuera
de los de la filmina (PTBR, PTPR, CS, DS, SS). Nunca se dice qué entra o qué no entra
en el parcial.

**No dice que en el thrashing los procesos esperan a la MMU**, como dice la filmina 13:
dice que esperan que sus páginas se carguen desde el disco, que es cierto en
cualquier caso. Es una de las decisiones de abajo.

### Términos

La forma principal es **«page fault»**, como titula la filmina la atención; el
globito dice que también se dice fallo de página. «Marco» en el texto corrido;
«frame» sólo dentro de una frase transcripta de una filmina. «I/O», no «E/S».

### Colores

Los de la práctica: página violeta, desplazamiento verde azulado, marco ocre,
segmento magenta, 1er nivel índigo, y lo que cambió en el paso en grafito. Los
estados de proceso son los de la cátedra. Los page faults y las víctimas, en tinta.
La curva del ejemplo 11 va en tinta: no nombra nada que tenga color.

---

## Decisiones que conviene confirmar con la cátedra

1. **Re-ejecución.** ¿Los accesos a tablas se cuentan otra vez cuando la instrucción
   se re-ejecuta después de un page fault? El ejemplo 9 no da ningún total que
   dependa de eso.
2. **Orden de los controles en segmentación**: límite antes que permiso. El ejemplo 7
   evita a propósito una referencia que falle los dos.
3. **Empates en Óptimo**: sale la que entró primero. Reproduce la tabla de la filmina
   (en la referencia 10 sale la 3) y reemplaza la regla de **Sustitución de páginas**,
   «la del marco de más arriba». Aplicada; falta confirmar.
4. **Page faults en tinta, no en rojo.** Aplicado; falta confirmar.
5. **La filmina 13 dice que en el thrashing los procesos esperan a la MMU.** El recurso
   no lo dice. La cátedra decide si la filmina queda así.
6. **La tabla de la filmina 33 se sale de la diapositiva**, y su fila de segmentación
   se completó con la filmina 29.
7. **«page fault» o «fallo de página»** como forma principal: acá es «page fault»; en
   la práctica, «fallo de página».
8. **La secuencia de Belady sale de la bibliografía**, no de las filminas.

**Que venían de Paginación y segmentación:**

9. **Los ejemplos 6 y 8 no vienen de la guía**: los números se armaron para el recurso.
10. **El hash del ejemplo 6**, (número de proceso + página) mod 8, es el más simple
    posible, para que se pueda hacer de cabeza.

---

## Verificación antes de publicar un cambio

1. Abrirlo con doble clic, sin servidor y sin internet.
2. La consola limpia, en los doce ejemplos y todas las variantes.
3. Recorrer cada ejemplo para adelante y para atrás.
4. A 1280×720, nada scrollea; a 390 px, una columna y nada se va para el costado;
   entre 1001 y 1239 px, nada se va para el costado.
5. Con el almacenamiento bloqueado, las preguntas funcionan igual.

Lo medido al armarlo, con un Chromium automatizado: **255 tableros** (los 229 pasos,
y cada opción de cada pregunta contestada) a 1280×720 sin un solo desborde ni scroll
de página, y a 390 px y seis anchos entre 1001 y 1239 sin nada hacia el costado;
**229 pasos** de ida y de vuelta, todos idénticos; consola limpia, ningún pedido de
red, y lo mismo con el almacenamiento bloqueado.

---

## Lo que falta

**Toda la narración la escribió un agente y no la revisó nadie de la cátedra**, salvo
las doce frases de cierre y las frases que el planteo daba textuales (la de V = 0, el
texto del bit de lockeo, el del PTBR, las cinco del ciclo del thrashing y la tabla
comparativa). Es lo primero que hay que leer antes de mandar el link.
