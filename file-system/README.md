# File system

La teoría de file system **en el orden de la cátedra**, con los números de las filminas
cuando las filminas los traen: las operaciones sobre un archivo, los locks, las tablas
de archivos abiertos, los permisos, los archivos mapeados a memoria, los métodos de
acceso, la asignación de bloques, el espacio libre, el journaling, y las dos
implementaciones, FAT y EXT2.

Es el recurso de **teoría**: explica cada concepto una vez. Las cuentas de la guía
quedan para la práctica. Cuando un concepto necesita números que la filmina no trae,
se usan **números de juguete** que entran en pantalla, como hace la propia filmina con
su «FAT4».

Se abre haciendo doble clic en `index.html`. No necesita internet ni instalar nada.

---

## Los dieciséis ejemplos

Son **271 pasos** contando todas las variantes, y **9 preguntas**. Ningún ejemplo usa
algo que se explica en uno posterior: del 1 al 10 no aparecen FAT, EXT2, inodos ni
clusters; la posición de lectura/escritura aparece en el 1 y recién en el 4 se dice
dónde vive.

| # | Tramo | Ejemplo | Variantes | Unidad del paso | Pasos |
|---|---|---|---|---|---|
| 1 | Archivos | Operaciones básicas | — | una operación | 9 |
| 2 | Archivos | Operaciones compuestas | Copiar · Renombrar · Mover · Mover a otro file system | una operación básica | 5 + 2 + 3 + 6 |
| 3 | Archivos | Locks | Obligatorio · Sugerido | un pedido o una acción | 9 + 8 |
| 4 | Archivos | Archivos abiertos | — | una apertura, un cierre o una operación | 8 |
| 5 | Archivos | Permisos | Archivo · Directorio | un usuario que intenta una operación | 14 + 11 |
| 6 | Archivos | Archivos mapeados a memoria | Con read · Mapeado | una lectura | 7 + 9 |
| 7 | En el disco | Métodos de acceso | Secuencial · Directo · Indexado · Hashed | un registro o una búsqueda en el índice | 6 + 2 + 3 + 3 |
| 8 | En el disco | Asignación de bloques | Contigua · Enlazada · Indexada | un evento de asignación o una lectura | 9 + 9 + 13 |
| 9 | En el disco | Espacio libre | Bit vector · Enlazada · Indexada · Lista de bloques · Lista de contiguos | un paso de la representación o un pedido | 3 × 5 |
| 10 | En el disco | Journaling | Sin journal · Con journal | una operación o la falla | 6 + 12 |
| 11 | FAT | La FAT funcionando | — | una búsqueda en la FAT o un evento | 14 |
| 12 | FAT | Tamaños de la FAT | FAT12 · FAT16 · FAT32 | una cuenta | 12 + 13 + 13 |
| 13 | EXT2 | El inodo | — | el archivo crece un bloque | 12 |
| 14 | EXT2 | Las indirecciones | — | un acceso a un bloque | 18 |
| 15 | EXT2 | Crear y encontrar un archivo | — | una actualización; después, un paso de la resolución | 11 |
| 16 | EXT2 | Links | — | un comando o su consecuencia | 9 |

Cada ejemplo termina con una **placa de cierre**; las dieciséis frases vienen del
planteo, palabra por palabra. Cada título de panel tiene su globito.

**El hilo del recurso** son dos ejes distintos: el método de acceso (cómo pide los datos
el programa, ejemplo 7) y el método de asignación (dónde están los bloques, ejemplo 8).
La asignación decide cuánto cuesta cada acceso. La narración lo dice en el 7 (la placa
de cierre agrega que es el eje lógico), en el 8 (cada lectura se compara con el 7), en
el 11 (FAT es enlazada con la tabla en memoria) y en el 14 (en EXT2 el camino se
calcula).

### Qué hay en cada uno

1. **Operaciones básicas.** `notas.txt` en `/docs`, bloques de 4 bytes, libres el 2, el
   5, el 6 y del 8 al 11. Crear, abrir, escribir `HOLA MUNDO` (bloques 2 `HOLA`, 5
   ` MUN`, 6 `DO`), posicionar en 5, leer `MUNDO`, truncar a 4, cerrar, eliminar.
2. **Operaciones compuestas.** Copiar = crear + leer + truncar + escribir (3 leídos y 3
   escritos); renombrar y mover dentro del mismo file system no tocan ningún bloque de
   datos; mover a otro file system = copiar allá y eliminar acá (3, 3 y 3 liberados).
3. **Locks.** Dos procesos y dos saldos que suman 400. Obligatorio: P2 queda BLOCKED
   hasta que P1 libera. Sugerido: P2 escribe igual y P1 lee 350.
4. **Archivos abiertos.** Tabla global y tablas por proceso; abrir y cerrar en el orden
   de la Bitácora; el contador; la posición, una por proceso.
5. **Permisos.** `-rw-r-----` (640) y `drwxr-x--x` (751), nueve intentos en cada uno, el
   octal, y una ACL para que carla lea sin cambiar el grupo.
6. **Archivos mapeados.** Seis lecturas 0, 0, 1, 0, 2, 1: 6 syscalls con `read`; 1
   syscall, 3 page faults y 3 accesos comunes mapeado.
7. **Métodos de acceso.** Legajo 166 en ocho registros: 5, 1, 2 y 1 lecturas.
8. **Asignación de bloques.** El volumen y el bloque lógico; después A, B, C, A crece y
   D, en contigua, enlazada e indexada, con dos lecturas de A en cada una; al final de la
   indexada, los esquemas enlazado, multinivel y combinado.
9. **Espacio libre.** Los mismos libres (0, 2, 3, 8, 9, 14, 15) en cinco
   representaciones, y un pedido en cada una.
10. **Journaling.** Una transacción sin journal que deja un bloque huérfano; el journal de
    la filmina (A, B, F) con el rollback de F.
11. **La FAT funcionando.** La FAT de la filmina 4 de la clase 12, el recorrido de `moo`,
    el 4.º cluster de `snow` (3 búsquedas en memoria y 1 lectura a disco), el primer
    cluster libre y el volumen.
12. **Tamaños de la FAT.** La FAT4 de las filminas 9 y 10, y FAT12/16/32 sobre 1 GiB con
    clusters de 4 KiB.
13. **El inodo.** El archivo crece de 0 a 8 bloques, la fragmentación interna y el tamaño
    máximo, 1376 bytes.
14. **Las indirecciones.** Los bytes 20, 70 y 200, y el rango 0–90: 1, 2, 3 y 7 accesos.
15. **Crear y encontrar un archivo.** Un grupo de juguete; crear `/docs/notas.txt` y
    después resolver la ruta.
16. **Links.** Los comandos de la filmina 21, `rm fileloco`, links entre file systems, y
    el grafo acíclico.

---

## De dónde sale cada número, y qué lo verifica

**No son comentarios: son campos que el código chequea al abrir**, y si algo no da lo
dice la consola del navegador. Cada ejemplo tiene un **modelo** (un simulador chico) que
ejecuta las operaciones; los paneles se dibujan con el estado del modelo y los números
de la narración son marcas como `{bloques}` que completa el mismo modelo. Cada número del
planteo está además escrito en el campo `expect` del paso donde aparece, y el recurso
compara lo que da el modelo contra eso.

| Ejemplo | Números | De dónde | Qué lo verifica |
|---|---|---|---|
| 1 | bloques 2, 5, 6; `MUNDO`; tamaño 10 y 4; posición 0, 10, 5, 10 | inventados (planteo) | modelo `files`, `expect` de cada paso |
| 2 | 3 + 3; 0; 0; 3 + 3 + 3; bloques 8–10 y 3–5 | inventados (planteo) | modelo `files`, contador y `expect` |
| 3 | 100 + 300 = 400; 350 a medio escribir | inventados | modelo `locks`; además, falla si P2 escribe sin el exclusivo en obligatorio |
| 4 | contadores 1, 2, 1, 1, 0; fd 0, 0, 1; posiciones 6 y 20 | inventados (planteo) | modelo `open` |
| 5 | 640, 751; los dieciocho ✓/✗ | planteo | modelo `perms`, `expect` de cada intento |
| 6 | 6 syscalls; 1, 3 y 3 | planteo | modelo `mmap` |
| 7 | 5, 1, 2, 1; 166 mod 8 = 6; restos distintos | planteo | modelo `access` (falla si dos legajos chocan) |
| 8 | todas las ubicaciones, 7 libres con tramo de 2, 4 y 0 libres; lecturas 1/3, 3/3, 2/4 | planteo | modelo `alloc`, que asigna con primer ajuste |
| 9 | `0100111100111100`; las cinco representaciones y los cinco pedidos | planteo | modelo `free` |
| 10 | el bloque 2; A y B quedan, F se deshace | filmina 24 de la clase 11 (journal) e inventado (bitmap) | modelo `journal` |
| 11 | la FAT entera; las tres cadenas; 3 búsquedas y 1 lectura; libre la 2 | filmina 4 de la clase 12 | el modelo `fat` recorre cada cadena y la compara con la del planteo, y verifica que cada entrada usada sea de un solo archivo |
| 12 | 16, 64 bits, 8 bytes, 128 bytes; las cadenas y el contenido de cada bloque | filminas 9 y 10 de la clase 12 | modelo `fatSize`: cadenas y bytes por bloque |
| 12 | teórico, real, entradas, tamaño y copia de FAT12/16/32 | inventados (planteo) sobre la filmina 8 | modelo `fatSize`, `expect` de cada cuenta |
| 13 | b3 … b13; 120 bytes; 8 de 16, 1 de 4, 2 de 4; 1376 bytes, 86 bloques | inventados (números de bloque) y planteo | modelo `inode`, que asigna de arriba hacia abajo |
| 14 | caminos y 1, 2, 3, 7 accesos | planteo | modelo `indirect`, que vuelve a correr el asignador del 13 hasta 14 bloques y compara los bloques de datos |
| 15 | i4; b3 y b4; [i4, 20, 9, «notas.txt»]; 5 → 4 y 13 → 11 | inventados (planteo) | modelo `ext`; el largo de cada registro se recalcula |
| 16 | la salida de `ls -li`, con las dos correcciones | filmina 21 de la clase 12 | el modelo `links` genera la salida y la compara con la del planteo, línea por línea |

Además: que cada pregunta tenga su opción correcta y un `why` en cada opción; que no
quede ningún `{dato}` sin completar; que todo globito invocado exista y que todo globito
escrito se use; que cada panel tenga nombre, globito y algo que mostrar en cada paso; y
que **cuando un panel sale de la pantalla, la narración lo diga**.

### Datos inventados

Todo lo que el planteo marca como inventado, más lo que eligió el recurso:

- **Ejemplo 1:** los bloques de otros archivos (0, 1, 3, 4 y 7), para que los libres en
  orden sean 2, 5 y 6; las horas de las fechas del FCB.
- **Ejemplo 2:** el segundo file system, de 6 bloques con los tres primeros ocupados, y su
  directorio `/backup`.
- **Ejemplo 3:** los dos saldos (A 100, B 300) y la transferencia de 50.
- **Ejemplo 4:** los números de FCB (12 y 15).
- **Ejemplo 10:** el bitmap de 8 bloques (`11000000`) y `nuevo.txt`.
- **Ejemplo 13:** los números de bloque (los 0, 1 y 2 de otros archivos; el resto, el
  primer libre cada vez). El ejemplo 14 sigue al mismo archivo hasta 14 bloques: b3, b4;
  el simple en b5; el doble en b10, con sus simples en b11 y b16.
- **Ejemplo 15:** el segundo grupo, que se ve sólo como miniatura; los largos de registro
  son 8 + el nombre redondeado a 4 (ver abajo).

---

## El chasis se comparte; el canvas se deriva

El chasis es el de **Memoria**, entero, y se comparó propiedad por propiedad con estilos
calculados: la escalera (sólo números, por tramo), el renglón de elecciones en la línea
del título, los controles, las teclas, el globito en cada título de panel y la placa de
cierre. **Cero diferencias.**

El canvas se derivó ejemplo por ejemplo: un panel está cuando es el tema o cuando
cambia. Cuando uno sale, la narración lo dice.

| # | Lo que muta | Canvas |
|---|---|---|
| 1 | el directorio, el FCB, los bloques y la posición | Directorio + FCB · Disco; con el archivo abierto, también la posición |
| 2 | las entradas y los bloques tocados | Directorios + contador · Disco (dos discos en la variante entre file systems) |
| 3 | los estados, los locks y los saldos | Procesos · Locks · Contenido |
| 4 | las dos tablas y lo que hace el SO | Tabla global · Tablas por proceso · Qué hace el SO |
| 5 | qué grupo se consulta y qué pudo cada uno | Permisos + Usuarios · Intentos; después, Octal o ACL |
| 6 | dónde ve el proceso los datos, y el costo | Archivo + Memoria del proceso · Lecturas · Costo |
| 7 | qué registro se lee | Archivo · Accesos (más Índice o Cuentas en su variante) |
| 8 | el disco y las lecturas | Disco y volúmenes; después Disco · Directorio + Lecturas; al final, Bloques índice |
| 9 | la representación | Disco · Espacio libre |
| 10 | RAM contra disco; el journal | RAM · Disco; con journal, Journal · Transacciones |
| 11 | la cadena, en los clusters y en la FAT | Directorio + Costo · (FAT) · Disco; al final, el volumen |
| 12 | las cuentas | Cuentas · Directorio + FAT · Bloques; después Cuentas · Datos |
| 13 | el inodo y el disco | Inodo · Disco; al final, las cuentas |
| 14 | el camino de cada lectura | Cuentas · Inodo · Bloques de punteros · Accesos |
| 15 | bitmaps, superbloque, inodos, registros | Estructura del volumen; después Bitmaps + Superbloque · Inodos + Registros · Bloques |
| 16 | la terminal, el directorio y los inodos | Terminal · Directorio + Inodos; al final, la estructura de directorios |

### Colores

- **Estados de proceso** (ejemplo 3): los de la cátedra. El 4 no muestra estados.
- **Identidad de archivo:** cada archivo tiene su tono en todos los paneles de su ejemplo,
  y la referencia debajo de cada disco dice qué tono es de qué archivo. Los tonos son
  **violeta, verde azulado, magenta y ocre**, en ese orden. No se usa ni rojo, ni verde,
  ni azul, ni amarillo, ni naranja, para no chocar con los estados. La filmina 4 de la
  clase 12 pinta sus tres archivos rojo, violeta y verde; acá son violeta, verde azulado y
  magenta, por la misma razón.
- **Tipos de bloque**, que se distinguen también sin color: datos con borde simple y
  relleno del archivo; punteros (bloque índice) con **borde doble**; directorio con una
  **barra gruesa a la izquierda**; libre con **borde punteado** y la palabra «libre»; de
  otros archivos, en gris.
- **Lo que cambió o se leyó en el paso**, en grafito, con un solo borde.
- **Nada rojo significa error**: los fallos van en tinta, con ✗.

---

## Cambiar un texto

Todos los textos que ve un estudiante están en el bloque `<script id="data">`. Las
marcas son las de toda la familia: `[[fcb:FCB]]` sale con globito, `*así*` en negrita,
`` `así` `` en letra de código, y `{bloques}`, `{pos}`, `{tamanio}` los completa el
modelo —**no los reemplaces por el número escrito a mano**: si el número cambia, cambia
el dato del modelo, y el `expect` del paso dice si da—. Los textos que arma el propio
modelo (las líneas de «Qué hace el SO», las de las lecturas del ejemplo 8, los rótulos
de los paneles) están en `UI_TEXT`.

## Hasta dónde llega

Al nivel de las filminas y la Bitácora, y no más. No aparecen: formatos reales en disco
(bytes de una entrada FAT, nombres 8.3, las reglas reales de largo de registro de ext2,
números de inodo reales, `.` y `..`), ext3/ext4 ni ningún otro file system, modos de
journaling ni recuperación después de un commit, planificación de disco, RAID, `fsync`,
por qué los file descriptors reales empiezan en 3, sintaxis de `setfacl`, sticky bit,
setuid, `umask`, argumentos de `mmap`, hardlinks a directorios, números de la guía. Nunca
se dice qué entra o qué no entra en el parcial.

### Términos

«file system» en el texto corrido; «bloque» en general y «cluster» en FAT; «bit vector»
en el ejemplo 9 y «bitmap» en EXT2, como cada clase; «E/S», como la clase 11 (los
recursos de memoria dicen «I/O», como la suya); «page fault», como el recurso de memoria;
«symbolic link» como forma principal, con «softlink» en el globito y en la placa de
cierre.

---

## Decisiones que conviene confirmar con la cátedra

1. **Los bloques de la FAT4** (clase 12, filmina 10) pierden el espacio después de «soy»,
   porque los bloques son de 8 bytes. Se reproducen tal cual, y en ningún lado se escribe
   la frase entera con un espacio que no está en los bytes.
2. **Los largos de nombre de la filmina 16** no coinciden con los nombres («file» con 5,
   «NombreEjemplo.c» con 14). El recurso usa los largos reales.
3. **Filmina 21:** el `2v` del contador de `hl_fileloco` se muestra como `2`, y el tamaño
   del symbolic link como 8 (lo que mide «fileloco»), no 4.
4. **Mover entre file systems** = copiar + eliminar sale de la Bitácora, no de la filmina.
5. **Links → grafo acíclico** sale de Silberschatz, no de la filmina.
6. **«Lista de bloques»** se lee como el agrupamiento de Silberschatz: cada bloque guarda
   N direcciones, N − 1 libres para usar y la última, el bloque donde sigue la lista. Al
   pedir un bloque se da la primera dirección anotada en la cabeza (el 2), no la cabeza.
7. **Los file descriptors empiezan en 0**, como índice de la tabla del proceso.
8. **«E/S» contra «I/O»**: este recurso sigue a la clase 11; los de memoria, a la suya.
9. **Journaling después de un commit** no se muestra: lo que la Bitácora dice de
   «retomar» no coincide con el modelo habitual.
10. **Cómo se cuentan los accesos** (el inodo no cuenta; un bloque de punteros se lee una
    vez por rango) sale de la resolución del ejercicio 3 de la clase 14.
11. **Errores de la Bitácora que el recurso no repite:** la desventaja de «punteros» en la
    asignación contigua (no tiene); los tamaños de entrada de FAT como «2^1,5 / 2^2 / 2^4
    bytes» (son 1,5, 2 y 4 bytes); las fórmulas de tamaño de EXT, que están mal escritas
    (el ejemplo 13 no calcula el tamaño del file system); y que las otras estructuras de
    espacio libre sean «más pesadas» que el bit vector.
12. **Los largos de registro del ejemplo 15** son inventados (8 + el nombre redondeado a
    4) y el recurso no los explica.

**Que eligió el recurso y conviene mirar:**

13. **Ejemplo 8, esquema combinado:** el planteo pide la frase «Lo retomamos en EXT2»,
    pero también pide que del 1 al 10 no se nombre EXT2. Dice «Lo retomamos en el último
    tramo», y el último tramo de la escalera se llama EXT2.
14. **Ejemplo 1, truncar:** la posición queda en 10 aunque el archivo pase a medir 4, y la
    narración lo dice en una frase («la posición no se toca»).
15. **Ejemplo 2, copiar:** truncar el archivo nuevo a 10 bytes **reserva** sus tres bloques
    sin escribirlos; el contador no los cuenta hasta que se escriben.
16. **Ejemplo 3:** los estados alternan entre RUNNING y READY porque hay una sola CPU: está
    en RUNNING el que actúa en el paso.

---

## Verificación antes de publicar un cambio

1. Abrirlo con doble clic, sin servidor y sin internet.
2. La consola limpia, en los dieciséis ejemplos y todas las variantes.
3. Recorrer cada ejemplo para adelante y para atrás.
4. A 1280×720, nada scrollea; a 320 y 390 px, una columna y la página no se va para el
   costado; entre 1001 y 1239 px, tampoco.
5. Con el almacenamiento bloqueado, las preguntas funcionan igual.

Lo medido al armarlo, con un Chromium automatizado: **301 tableros** (los 271 pasos y
cada opción de cada pregunta) a 1280×720 sin un solo panel desbordado, sin scroll de
página y sin que la caja de la pregunta scrollee por dentro; a 320, 390, 768, 1001,
1100, 1239, 1440 y 1920 px, la página nunca se va para el costado (a 320 y 390, las
tablas anchas y la terminal se desplazan dentro de su propia caja); **271 pasos** de ida
y de vuelta, idénticos; consola limpia, ningún pedido de red, y lo mismo con el
almacenamiento bloqueado. Los estilos del chasis, comparados con Memoria: cero
diferencias. Cada `expect` se probó cambiándolo a propósito y viendo el error en la
consola.

---

## Lo que falta

**Toda la narración la escribió un agente y no la revisó nadie de la cátedra**, salvo
las dieciséis frases de cierre y las frases que el planteo daba textuales (la pregunta
del ejercicio 1 de la guía en el ejemplo 11, la advertencia «FAT 4 ¡NO EXISTE!» y las
salidas de la filmina 21). Es lo primero que hay que leer antes de mandar el link.
