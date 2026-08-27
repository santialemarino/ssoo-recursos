# ssoo-recursos

Recursos para la cátedra de Sistemas Operativos. Cada recurso es **una sola página
HTML** que se abre haciendo doble clic: no hay que instalar nada y no necesita
internet.

Se mandan por link para que los estudiantes los recorran solos, y se proyectan en
clase.

## Recursos

| Recurso | Qué es |
|---|---|
| [Ciclo de instrucción e interrupciones](instruction-cycle-interrupts/) | Un procesador simplificado que ejecuta programas cortos paso a paso: las cinco etapas del ciclo, los registros, la pila del sistema y cómo se atiende una interrupción. Nueve ejemplos en escalera, del programa lineal al cambio de contexto. |
| [Procesos e hilos](process-lifecycle/) | Un proceso mirado por dentro, paso a paso: `text`, `data`, `heap` y `stack`, lo que cada llamada usa del stack, los estados, el cambio de contexto, la suspensión y el SWAP, los planificadores y los hilos. Ocho ejemplos en escalera, del programa contra el proceso a un proceso con tres hilos. |

Cada recurso tiene su propio `README.md` al lado del `index.html`, con lo que hay
que saber para editarlo: qué enseña, cómo cambiar un texto, cómo agregar un caso y
qué no se puede tocar. **Esa es la documentación del recurso**; acá solo va la
línea que dice qué es.

## Cómo abrir uno

Doble clic en su `index.html`. En la raíz hay un `index.html` que los lista todos,
por si es más cómodo entrar desde ahí.

Para mandárselo a alguien alcanza con el link: los recursos están publicados en
https://santialemarino.github.io/ssoo-recursos/. También se puede mandar el
archivo suelto, que anda igual sin internet.

## Cómo agregar un recurso

1. Crear una carpeta en la raíz, `<nombre-en-ingles>/index.html`: un solo
   archivo, sin dependencias, sin paso de build.
2. Agregarlo a la lista de `index.html` y a la tabla de acá arriba, con una línea.
3. Escribir el `README.md` de su carpeta con las notas de mantenimiento.

Las convenciones del repo (un archivo por recurso, textos visibles en español,
identificadores en inglés, sin comentarios en el código) están en `CLAUDE.md` y en
`.claude/skills/`.

## Estructura

```
index.html            índice de recursos
<nombre>/index.html   un recurso, autocontenido
<nombre>/README.md    su documentación
CLAUDE.md / AGENTS.md instrucciones para los agentes que trabajan acá
.claude/skills/       convenciones del repo, para Claude Code
.agents/skills/       las mismas, para Codex
```
