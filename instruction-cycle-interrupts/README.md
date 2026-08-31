# Ciclo de instrucción e interrupciones

Un procesador simplificado que ejecuta programas cortos **paso a paso**. En la
misma pantalla se ven la memoria, los registros, la pila del sistema, la tira de
etapas del ciclo y una narración de lo que pasó en cada paso. El estudiante avanza
y retrocede; no escribe código.

Se abre haciendo doble clic en `index.html`. No necesita internet ni instalar nada.

Este archivo es la documentación del recurso: qué enseña, cómo editarlo, y qué no
se puede tocar. Si vas a modificar `index.html`, leelo antes.

---

## Los nueve ejemplos

Una escalera: cada ejemplo agrega **un solo concepto nuevo** y da por sabido todo
lo anterior.

| # | Ejemplo | Qué enseña |
|---|---|---|
| 1 | Programa lineal | Las cinco etapas del ciclo, y que el IP apunta a la instrucción *siguiente* |
| 2 | Salto condicional | Que el IP no siempre avanza de a uno, y que la ejecución escribe el PSW |
| 3 | La pila a mano | La pila, el SP, y guardar y restaurar un valor con PUSH y POP |
| 4 | Instrucción privilegiada | Que hay dos modos, y que un programa de usuario no puede hacer todo |
| 5 | Interrupción de teclado | La atención completa de una interrupción: qué guarda el hardware y qué guarda la rutina |
| 6 | Syscall | Que el mecanismo es el mismo que el de una interrupción, y que lo único distinto es el origen |
| 7 | Leer del disco | I/O programada, por interrupciones y con DMA, y cuánto trabajo de CPU cuesta cada una |
| 8 | Interrupciones anidadas | Dos contextos guardados a la vez en la pila, y por qué se devuelven en orden inverso |
| 9 | Cambio de contexto | Que cuando lo que se retoma es otro programa, el contexto ya no alcanza con la pila |

Cada ejemplo termina con una **placa de cierre**: una frase, en un recuadro, que
es lo que el estudiante se tiene que llevar de ese ejemplo. Está en el campo
`closingCard` de cada uno. Y cada título de panel tiene un globito que explica
qué muestra ese panel, en `PANEL_TIPS`.

Los controles son **Primero · Anterior · Siguiente · Último · Reproducir**, con
las flechas del teclado y `Inicio` / `Fin`. Son los mismos que en el otro recurso
de la familia: si acá se agrega o se saca uno, hay que hacer lo mismo allá. La
regla está en `CLAUDE.md`.

Hay dos niveles de detalle, que se cambian en cualquier momento: **detallado**
(cada etapa del ciclo es un paso) y **compacto** (un paso por instrucción). La
atención de una interrupción se muestra paso por paso en los dos.

Atravesando los nueve hay un panel **Contexto** que gana una línea por ejemplo y
termina en la idea de cambio de contexto.

En los botones de arriba cada ejemplo se nombra con una palabra; el título completo
aparece al lado del número de paso. Los paneles que un ejemplo no usa no se
muestran: en el ejemplo 1 no aparece la pila, porque nunca se toca.

---

## Cambiar un texto

Todos los textos que ve un estudiante están juntos, arriba de todo del archivo,
dentro del bloque `<script id="data">`. No hay ni un texto visible escrito en otro
lado.

1. Abrí `index.html` en cualquier editor de texto.
2. Buscá el texto tal como aparece en pantalla. Va a estar entre comillas.
3. Cambialo, guardá y recargá la página.

Dentro de los textos hay tres marcas:

- `[[ip:IP]]` — la palabra `IP` sale con un globito explicativo. Antes de los dos
  puntos va cuál de las explicaciones usa; después, lo que se lee en pantalla.
- `*así*` — sale en negrita.
- `{addr}`, `{value}` — valores que el simulador completa solo (una dirección, un
  número). Se pueden mover de lugar en la oración, pero no conviene borrarlas.

---

## Agregar un ejemplo

En el mismo bloque `<script id="data">`, al final de la lista `EXAMPLES`, se copia
el bloque de un ejemplo parecido y se cambian:

- `number`, `title`, `subtitle`, y `short` (la palabra del botón de arriba)
- `programs`: el listado de instrucciones, escrito igual que en el pizarrón
  (`"MOV AX, 5"`). Las direcciones salen solas a partir de `origin`.
- `defaultGranularity`: `"detallado"` o `"compacto"`
- `interrupts`: en qué instrucción entra cada interrupción, si entra alguna
- `notes`: los comentarios que se agregan en pasos puntuales
- `expectedFinalState`: los valores que tienen que quedar al final

Ese último campo no es decorativo: al abrir la página el simulador corre el ejemplo
entero y avisa en la consola del navegador si el resultado no da lo que declaraste.
Es la forma de darse cuenta de que un ejemplo quedó mal escrito.

No hace falta tocar nada más del archivo. Los ganchos más avanzados (variantes,
contador, DMA, cambio de contexto) están listados en la sección 7.

---

## 1. Hasta dónde llega, y por qué se queda ahí

Este recurso explica al nivel de la materia y no más. **Ser más preciso
técnicamente que el curso es un defecto, no una virtud**: la precisión de más
abre preguntas que el recurso no puede responder y deja al estudiante menos
seguro que antes. El estudiante típico lo abre solo, sin nadie a quien
preguntarle.

El caso testigo: acá **la ejecución es estrictamente secuencial, una instrucción
por vez**. Los procesadores reales solapan instrucciones. Eso está afuera: el
simulador no lo modela, no lo insinúa y no se disculpa por no hacerlo.

### Lo que no aparece en ningún lado

Ni en pantalla, ni en la narración, ni en los globitos, ni en el README:

- Solapamiento de instrucciones: cauce, ejecución fuera de orden, ejecución
  especulativa, varias unidades de ejecución a la vez.
- Memorias intermedias entre la CPU y la memoria principal, de cualquier nivel.
  Jerarquía de memoria.
- Traducción de direcciones, memoria virtual, paginación, segmentación de
  memoria, direcciones lógicas contra físicas, fallos al traer una página.
- Numeración hexadecimal. Códigos de operación. Tamaño de las instrucciones.
  Orden de los bytes.
- Niveles de privilegio numerados, la tabla de descriptores del procesador por su
  sigla, estructuras de descriptores.
- Registros de 64 bits, registros de segmento, modos de direccionamiento.
- Cómo funciona por dentro el controlador de interrupciones. Interferencia del
  DMA con la CPU en el acceso a memoria. Tiempos medidos en ciclos de reloj.
- **Algoritmos de asignación de CPU: ninguno, ni por nombre ni por
  comportamiento explicado.** En el ejemplo 9, "el SO decide", y nada más.
- **El diagrama de estados de un proceso y los nombres formales de esos
  estados.** Se puede escribir "P1 queda esperando el disco". No se puede
  nombrar un estado formal.
- La sigla de la estructura donde el SO guarda el contexto de un programa, salvo
  **una sola vez**, en la placa de cierre del ejemplo 9.
- Hilos, ejecución concurrente, ejecución en paralelo.

Si te parece que un ejemplo necesita alguno de estos conceptos, **el ejemplo está
mal planteado**. Decilo y proponé el arreglo mínimo; no agregues el concepto.

### Vocabulario

La pantalla está en español rioplatense, voseante. Estos son los términos, así
escritos:

ciclo de instrucción · búsqueda de instrucción · decodificación · búsqueda de
operandos · ejecución · write back (queda en inglés, como en la materia) ·
registro · IP (instruction pointer) · SP (stack pointer) · PSW (program status
word) · pila del sistema · apilar / desapilar · interrupción · rutina de atención
de la interrupción · tabla de interrupciones del SO · enmascarable ·
interrupciones deshabilitadas · interrupción anidada · syscall · modo usuario /
modo kernel · instrucción privilegiada · contexto · cambio de contexto ·
I/O programada · I/O por interrupciones · I/O con DMA · controlador de DMA

Dos aclaraciones: al IP nunca se lo llama solo "program counter", y a la tabla de
interrupciones del SO nunca se la nombra por su sigla.

### Voz

La narración suena a alguien explicando, no a epígrafes de figura. Oraciones
cortas. Sin signos de exclamación, sin emojis. **Todo texto que ve un estudiante
lo revisa alguien de la cátedra antes de publicarse.**

---

## 2. Las cuatro distinciones que la narración no puede equivocar

Son la sustancia de lo que el recurso enseña. Errarle a cualquiera de las cuatro
lo vuelve peor que no tener nada.

1. **Una interrupción no es un syscall.** Comparten el mecanismo — se apila el
   contexto mínimo, se cambia de modo, el control salta a una rutina — pero no el
   origen. La interrupción es asincrónica y externa. El syscall lo provoca la
   instrucción que se está ejecutando: es sincrónico y por software.
2. **Una interrupción se atiende entre dos instrucciones, nunca en el medio de
   una.** La instrucción en curso termina completa primero.
3. **El IP se incrementa durante la búsqueda de instrucción.** Por eso la
   dirección que se apila al atender una interrupción es la de la instrucción
   *siguiente*, no la de la que acaba de ejecutarse. Esto tiene que estar
   explícito y tiene su propio globito (`pushedIp`). Es la pregunta que la
   cátedra les hace después.
4. **La atención de una interrupción tiene una mitad de hardware y una mitad de
   software.** El hardware apila el PSW y el IP, cambia el modo y carga el IP
   nuevo. La rutina después guarda, por software, los otros registros que está
   por pisar. Cada paso lleva su etiqueta `[HW]` o `[SW]`.

---

## 3. Modelo de máquina

Didáctico de punta a punta. No le agregues realismo.

**Memoria.** Direcciones decimales, consecutivas, una instrucción o un dato por
celda. Es una simplificación deliberada; la narración la menciona al pasar una
sola vez, en el ejemplo 1, y nunca más. Las regiones son siempre las mismas:

| Rango | Contenido |
|---|---|
| 100–199 | Código del programa de usuario (P1) |
| 200–299 | Datos |
| 300–399 | Código del segundo programa de usuario (P2), solo ejemplo 9 |
| 500–599 | Rutinas de atención de interrupciones |
| 600–699 | Rutinas de syscall |
| 700–799 | Rutina de I/O del kernel |
| 900 | Base de la pila del sistema |

En pantalla se muestran solo las regiones que usa el ejemplo actual.

**Registros.** `AX`, `BX`, `CX` de propósito general, enteros, sin ancho
declarado. `IP`, `SP`, `PSW` de propósito específico. El PSW muestra exactamente
tres bits: `IF`, `ZF`, `MODE`, y `MODE` se lee `usuario` / `kernel`, no 0/1.
Debajo hay una línea en gris chico que aclara que un PSW real lleva más banderas:
ese es todo el tratamiento del tema.

**Pila.** El `SP` arranca en 900 con la pila vacía. `PUSH` baja el SP y después
escribe; `POP` lee y después lo sube. La pila crece hacia direcciones más chicas.
El SP siempre apunta a la última celda ocupada, o a 900 si está vacía. **Cada
celda va etiquetada con lo que contiene** ("IP de P1", "AX que guardó la rutina
del teclado"): sin las etiquetas el dibujo no enseña nada.

Se dibuja como una torre de platos, uno por celda, con el marcador `SP →` al lado
del de más arriba y la base abajo. Los platos vacíos se muestran punteados y dicen
"libre", para que la forma de la pila se entienda aunque esté vacía; el color del
borde dice quién guardó cada cosa (azul el programa, verde la rutina del kernel,
rojo el hardware).

**Set de instrucciones.** Está completo y **no se extiende**:

```
MOV reg, valor · MOV reg, [dir] · MOV [dir], reg
ADD reg, reg|valor|[dir] · SUB reg, reg|valor
CMP reg, reg|valor        afecta solo ZF: ZF=1 cuando son iguales
JMP dir · JZ dir          JZ salta cuando ZF=1
PUSH reg · POP reg
STI · CLI                 privilegiadas
SYSCALL n · IRET · HALT
```

No agregues `MUL`, `DIV`, `CALL`, `RET`, ni saltos por otra condición que no sea
`ZF`. `STI` y `CLI` aparecen solo dentro de rutinas del kernel.

**El ciclo.** Cinco etapas, siempre las cinco, siempre en orden, aunque una no
tenga nada que hacer — y en ese caso la narración lo dice. Es deliberado: el
diagrama de la materia tiene cinco etapas y el estudiante tiene que reconocerlas.

1. Búsqueda de instrucción — trae la instrucción de `[IP]`, después `IP ← IP + 1`
2. Decodificación
3. Búsqueda de operandos
4. Ejecución — acá se actualiza `ZF` cuando corresponde
5. Write back

Después viene la **verificación de interrupciones**, que es un paso visible
propio: si `IF` vale 0 no se atiende nada (y si había algo pendiente, la
narración avisa que quedó esperando); si no hay nada pendiente, vuelve a empezar
el ciclo; si hay más de una pendiente, se atiende la de mayor prioridad.

La **atención de la interrupción** son cuatro pasos visibles, todos `[HW]`:
identificar qué la provocó → apilar el PSW → apilar el IP → poner el modo en
kernel, `IF` en 0 y cargar en el IP la dirección que la tabla de interrupciones
del SO asocia a esa interrupción. `IRET` es el inverso.

**Interrupciones.** Teclado (prioridad baja, rutina en 520) y fin de I/O del
disco (media, 540). Los nombres y las prioridades son didácticos: no uses números
de interrupción reales ni des a entender que este mapa corresponde a hardware que
existe.

Cada interrupción se presenta **una sola vez** y después se reusa: el teclado
aparece en el ejemplo 4 y vuelve en el 7; el fin de I/O del disco aparece en el 6
y vuelve en el 7 y el 8. El teclado va primero porque es la más intuitiva: alguien
aprieta una tecla, es evidente que vino de afuera y que el programa no la pidió, y
no hay que explicar para qué sirve.

No hay interrupción de timer, a propósito: lo único que la justificaría es cómo el
SO reparte la CPU entre programas, y eso está fuera de alcance. Meterla obligaría
a decir "llegó el timer" sin poder decir por qué.

**Instrucciones privilegiadas.** `STI` y `CLI` solo se ejecutan con el `MODE` en
kernel. Si un programa de usuario las intenta, el procesador no las ejecuta, no
cambia nada, y el programa queda cortado ahí. No se modela ningún mecanismo de
aviso al sistema operativo: eso ya sería otro tema.

**Syscalls.** Entrar al kernel por un syscall usa **los mismos cuatro pasos** que
una interrupción (identificar, apilar PSW, apilar IP, cambiar de modo y cargar el
IP), con la misma función del motor. Se muestran igual para que se vea que es el
mismo mecanismo; lo único que cambia es la narración, que dice "igual que con una
interrupción" en vez de volver a explicarlo. Los pasos van después del write back
de la instrucción `SYSCALL` y antes de la verificación de interrupciones, que es
donde se hace visible que el syscall no pasó por ahí.

3 = leer del disco (rutina en 700), 4 = escribir por pantalla
(rutina en 600).

---

## 4. Granularidad

Dos modos, que el estudiante cambia cuando quiere, y cada ejemplo declara el
suyo por defecto (`defaultGranularity`).

- **Detallado**: cada etapa es un paso. Cinco pasos más la verificación por
  instrucción.
- **Compacto**: un paso por instrucción. La verificación sigue siendo un paso
  propio.

En los dos modos la tira de etapas está siempre en pantalla. En compacto las cinco
primeras se dibujan **encerradas en un solo bloque** que se enciende entero, para
que se lea como "todo esto pasó en este paso" en vez de como cinco cosas pasando a
la vez. El ciclo siempre está *presente* aunque no se esté *narrando*.

En modo compacto, el paso de verificación de interrupciones **solo aparece cuando
tiene algo que decir**: cuando hay una interrupción pendiente, cuando se atiende
una, o cuando el ejemplo lo fuerza con el campo `forceCheck`. Si no, se pliega
dentro del paso de la instrucción. En modo detallado aparece siempre. Sin esto, la
mitad de los pasos de un ejemplo eran la misma frase repetida.

**La atención de una interrupción no se colapsa nunca**, en ningún modo.

Los ejemplos 1 a 4 arrancan en detallado; del 5 en adelante, en compacto. El
ejemplo 5 avisa una vez, con todas las letras (campo `granularityNote`), que el
ciclo se muestra comprimido porque ya se conoce. **Nunca cambies de granularidad
en silencio.**

---

## 5. El hilo conductor: contexto

"Contexto" es exactamente lo que el nombre dice en la materia: todo lo que hay que
guardar de un programa para poder retomarlo como si nada hubiera pasado. No es
otro sentido de la palabra.

La idea se construye a lo largo de la escalera en vez de definirse de entrada.
Cada ejemplo con `contextLine` gana una línea cuando el estudiante llega al paso
indicado por `revealAfter`. En ese paso la línea aparece **dentro de la narración**
como una placa destacada, que es donde el estudiante está mirando; después queda
guardada en el desplegable "Contexto" de arriba a la derecha, que va contando
cuántas ideas juntó. Las líneas se acumulan durante la sesión.

| Ejemplo | Lo que agrega |
|---|---|
| 3 | Guardar y restaurar un valor a mano ya es guardar y restaurar contexto |
| 5 | El contexto mínimo es PSW e IP; el hardware lo guarda solo, la rutina guarda el resto por software |
| 6 | Un syscall guarda el mismo contexto mínimo aunque no lo haya causado nada externo |
| 8 | Se pueden guardar dos contextos a la vez, y la pila los devuelve en orden inverso |
| 9 | Cuando lo que se retoma es otro programa, la pila no alcanza: eso es un cambio de contexto |

El ejemplo 9 cierra con una placa, casi textual:

> Acabás de ver un **cambio de contexto**. La estructura donde el SO guardó todo
> lo de P1 tiene nombre, y los programas que el SO administra así también. Eso es
> la clase que viene.

La sigla de esa estructura no se nombra antes de esa placa, y adentro se nombra
una sola vez.

---

## 6. La escalera de ejemplos

Cada peldaño agrega **exactamente un concepto** y da por sabido todo lo de abajo.
Sacar cualquiera deja un hueco: sin el 2 las banderas nunca se ven escribirse;
sin el 3, el 5 parece magia; sin el 4 el syscall aparece de la nada; sin el 6 la
distinción entre interrupción y syscall no tiene dónde vivir; sin el 7 el DMA
nunca queda justificado. Partir el 7 en tres sería un error: es una lección con un
interruptor, y el punto es que el programa de usuario es idéntico en los tres
modos.

**El tramo 4 → 5 → 6 es una sola cadena y el orden importa.** El 4 muestra a un
programa de usuario chocando contra una instrucción que no tiene permitida y
quedando cortado ahí: deja planteada la pregunta "¿y entonces cómo consigue lo que
necesita del sistema?". El 5 enseña el mecanismo completo de atención de una
interrupción. El 6 usa *ese mismo mecanismo* para responder la pregunta que dejó
abierta el 4. Si el 6 fuera antes que el 5, tendría que enseñar el mecanismo desde
cero y dejaría de ser el ejemplo delgado que solo señala la diferencia de origen.

Los programas están escritos instrucción por instrucción en el bloque de datos:
**usalos como están.** Si alguno no funciona como está escrito, decilo y proponé
el arreglo mínimo.

Lo que no se puede perder de cada uno:

- **1** — Es también el tutorial de la interfaz. Mantenelo mínimo.
- **2** — `CMP` no guarda resultado en ningún registro: lo único que hace es
  dejar `ZF = 1`. Es el primer caso donde write back no escribe nada. Las celdas
  103 y 104 nunca se ejecutan, y la memoria lo muestra.
- **3** — Guarda **un solo** valor: apilarlo, pisarlo, recuperarlo. El orden
  inverso de los `POP` no se enseña acá, porque con una sola cosa guardada no hay
  orden que mostrar: queda como una línea que apunta al ejemplo 8, que es donde
  hay dos contextos apilados a la vez y el orden es la razón de que funcione.
- **4** — El `CLI` de la celda 101 no se ejecuta: el `MODE` dice usuario. Lo que
  la narración tiene que dejar clarísimo es que **la instrucción no está mal
  escrita, está prohibida para ese programa**, y que la misma instrucción dentro
  de una rutina del kernel corre sin problema. Cierra apuntando al ejemplo 6.
- **5** — Es la pieza central. La interrupción llega mientras se ejecuta la 101 y
  se atiende **después** de que la 101 termina. El IP apilado vale **102**.
- **6** — El mecanismo es **el mismo** que el del 5, dicho explícitamente, para
  que la atención vaya a la diferencia. **No vuelvas a explicar por qué se apila
  ni qué hace el cambio de modo**: los cuatro pasos se muestran igual, pero la
  narración solo dice "igual que con una interrupción". La diferencia se marca en
  el paso de verificación de interrupciones, que es donde se ve que no tuvo nada
  que ver. Y arranca recordando el ejemplo 4.
- **7** — Un contador visible: "instrucciones ejecutadas por la CPU para hacer la
  transferencia". Ese contador es todo el argumento. No lo midas en tiempo. El
  mismo programa de usuario en los tres modos. Panel de cierre comparando con dos
  columnas: ¿la CPU espera? y ¿la CPU copia?
- **8** — Llega primero la del teclado (baja) y encima entra la de fin de I/O del
  disco (media), las dos ya conocidas: lo único nuevo del ejemplo es el
  anidamiento. El dibujo de la pila en su profundidad máxima, con las seis celdas
  etiquetadas, es el entregable. La rutina del teclado queda cortada al medio y la
  interfaz lo muestra así, con la posición marcada.
- **9** — Las dos cajas de contexto lado a lado, con los valores cambiando, son
  la imagen principal. Por qué P2 y no otro: "el SO decide", y nada más.

---

## 7. Cómo está armado el archivo

Tres bloques, en este orden, y **sin comentarios en ningún lado**:

1. `<script id="data">` — todo lo declarativo: los textos, los globitos, la tabla
   de interrupciones y los ejemplos. Se puede agregar un ejemplo entero, o
   reescribir cualquier línea, sin tocar nada más.
2. `<script id="engine">` — el intérprete y el generador de la traza.
3. `<script id="ui">` — el dibujo y los controles.

**La traza está precomputada.** Al elegir un ejemplo, el motor corre la máquina
entera de una sola vez y produce un arreglo de fotos, una por paso. La interfaz
solo se mueve por ese arreglo. Es a propósito, por tres razones: "paso anterior"
queda exacto y trivial, nunca puede desincronizarse de "paso siguiente", y el
estado final de cada ejemplo se puede verificar sin abrir el navegador.

Las dos granularidades salen de **una sola pasada**: los pasos compactos son los
detallados agrupados, no una segunda implementación. Cada foto compacta guarda de
qué paso detallado salió, y eso es lo que permite cambiar de modo sin perder el
lugar.

**No hay comentarios en el código.** Lo que en otro proyecto sería un comentario
acá vive en otro lado: la estructura la dicen las etiquetas `<script id="...">`, y
los valores esperados de cada ejemplo son los campos `expectedFinalState` y
`expectedContexts`, que el motor verifica de verdad al cargar la página y reporta
en la consola del navegador si no dan.

### Ganchos que el motor le ofrece a los datos

Además de los programas y las notas, un ejemplo puede declarar:

- `variants` — varias versiones del mismo ejemplo, con un selector propio. Cada
  variante sobreescribe los campos que le hagan falta. El ejemplo 7 son tres
  variantes con el mismo programa de usuario.
- `counter` — muestra el contador de instrucciones ejecutadas en modo kernel.
- `disk` — modela el estado del disco: cada vez que alguien lee la celda de estado
  el motor cuenta la lectura, y a partir de la enésima devuelve "listo". Es lo que
  hace visible el bucle de espera.
- `dma` — después de una dirección dada, el controlador de DMA escribe celdas de
  memoria en pasos propios, sin que la CPU ejecute nada, y después levanta la
  interrupción.
- `switches` — un cambio de contexto manejado por el SO: tres pasos etiquetados
  `[SO]` que guardan el contexto de un programa en su estructura y cargan el del
  otro. Va junto con `contextNames` e `initialContexts`.
- `forceCheck` — direcciones después de las cuales el paso de verificación de
  interrupciones se muestra siempre, incluso en modo compacto.
- `sidePanel`, `closing`, `closingCard` — paneles fijos y placas de cierre. La
  placa de cierre la tienen los nueve ejemplos, y es lo último que se lee: si se
  agrega un ejemplo, tiene que traer la suya.
- `dataLabels` — el nombre de cada celda de datos, para que no sean números sueltos.
- `short` — la palabra que va en el botón de arriba; el título completo se muestra
  al lado del número de paso.

Las direcciones guardadas como IP en la pila o en una estructura de contexto se
marcan solas en el panel de memoria: así se ve dónde quedó cortada una rutina o un
programa. Y la tira del ciclo solo muestra las filas que el ejemplo usa.

Los identificadores están en inglés. **Todos** los textos que ve un estudiante
están en español y viven en el bloque de datos: no hay ni un solo texto visible
escrito dentro del motor o del dibujo.

---

## 8. Verificación antes de publicar un cambio

1. Los ejemplos corren de punta a punta sin errores en la consola, en las dos
   granularidades.
2. Recorrer un ejemplo entero para adelante y después entero para atrás deja el
   estado inicial idéntico.
3. La consola no reporta ningún `expectedFinalState` que no dé.
4. La pila vuelve a `SP = 900` al final de todos los ejemplos que la usan.
5. Ningún `IRET` desapila algo que no sea suyo.
6. Compacto y detallado terminan en el mismo estado final y pasan por los mismos
   estados de fin de instrucción: lo único que cambia es la cantidad de pasos.
7. Ningún término de la lista de la sección 1 aparece en la página.
8. Abierta con doble clic y sin internet, sigue funcionando igual.
9. Se ve bien a 1280×720 (el proyector del aula) y a 390 px de ancho (un
   teléfono).
10. Los nueve ejemplos terminan mostrando su placa de cierre, y se ve entera sin
    tener que arrastrar la narración.

Valores finales conocidos, para chequear a ojo:

| Ejemplo | Final |
|---|---|
| 1 | `AX=8`, `BX=3`, detenido con `IP=104` |
| 2 | `AX=4`, `CX=1`, `ZF=1`, las celdas 103 y 104 nunca se ejecutaron |
| 3 | `AX=7`, `SP=900` |
| 4 | `AX=1`, cortado con `IP=102`, `IF=1` sin cambiar, las celdas 102 y 103 nunca se ejecutaron |
| 5 | `AX=3`, `SP=900`, modo usuario, `IF=1` |
| 6 | `AX=11`, `BX=0`, `SP=900`, modo usuario, `IF=1` |
| 8 | `AX=3`, `SP=900`, la pila llegó a 6 celdas ocupadas |
| 9 | P1 retomó en 102 con `AX=5`, P2 terminó con `CX=3` |

---

## 9. Cuando algo no está definido

1. Si se puede resolver **sin meter ningún concepto nuevo**, resolvelo y decí la
   decisión en una línea.
2. Si hace falta un concepto que no está en el vocabulario de la sección 1, **no
   lo agregues**: preguntá, y llevá pensada la alternativa que lo evita.
3. Si un ejemplo no funciona como está escrito, decilo y proponé el arreglo
   mínimo. No lo reescribas por tu cuenta.

Y una regla que gana sobre cualquier detalle de este documento: **entre más
completo y más claro, elegí más claro.**

---

## 10. Decisiones abiertas

- **Paleta.** La cátedra no tiene identidad visual definida. Este recurso usa una
  paleta propia, para revisar:

  | Color | Qué significa, siempre |
  |---|---|
  | Azul | Programa de usuario, y modo usuario |
  | Verde | Kernel: sus rutinas, el modo kernel, y las interrupciones habilitadas |
  | Rojo | Interrupciones: la que llega, lo que hace el hardware, y las interrupciones deshabilitadas |
  | Violeta | La pila y el SP |
  | Ámbar | Un programa suspendido a mitad de instrucción, y el contador de instrucciones |
  | Grafito | Lo que cambió en el paso actual |

  El rojo marca "acá se interrumpe", no "acá hay un error".

- **Lo que cambió en un paso se marca con tinta, no con color.** Antes era ámbar.
  Cada tono nombra una cosa del material, y "esto cambió recién" no es una cosa del
  material: es información sobre el paso. La marca es acromática (`--marked` y
  `--marked-soft`) y se lee mejor que cualquier tono en un proyector mediocre. El
  recurso de procesos hace lo mismo, porque los roles de color son chasis.

- **El gris de los rótulos secundarios se oscureció a `#6b7484`.** Estaba en
  `#8c95a3`, que sobre blanco da 3,0:1 — por debajo del piso de 4,5:1 que pide la
  skill— y lo usaban los títulos de panel, las direcciones de memoria, los nombres de
  los registros y las banderas del PSW. Todo eso se lee, y se lee proyectado. Si
  alguna vez se toca, verificar el contraste sobre el fondo más claro donde aparezca,
  no sólo sobre blanco.

- **El ejemplo 7 se aparta del planteo original en dos puntos**, los dos aprobados
  por la cátedra, y los dos por la misma razón: el set de instrucciones no puede
  recorrer direcciones consecutivas (`MOV [dir], reg` escribe siempre en una
  dirección fija) y no se le van a agregar modos de direccionamiento.
  1. **Lee 2 palabras, no 4.** Con 4 el bucle hay que desenrollarlo cuatro veces y
     la rutina se va a unas 40 instrucciones. El programa de usuario ya usaba solo
     las celdas 200 y 201.
  2. **En I/O por interrupciones el disco avisa una sola vez**, cuando tiene las
     dos palabras listas, y la rutina las copia juntas. La alternativa (una
     interrupción por palabra) obliga a poner un `CMP`/`JZ` adentro de la rutina
     de atención para elegir la celda destino, y eso es más complejo que el
     concepto que el ejemplo quiere enseñar. Lo que se pierde es "una palabra, una
     interrupción". Lo que se conserva —y es el argumento del ejemplo— son las dos
     preguntas: la CPU no espera, pero copia igual.
