# Universidad — gestión de actividades e informes (UNAD)

Flujo de trabajo personal para producir actividades e informes universitarios
(no es un proyecto de software con colaboradores). Cada persona usa su propia
copia de este repositorio.

## Qué resuelve este proyecto

Antes, el trabajo se hacía en el chat web de Claude: los archivos de soporte
subidos a la conversación no persistían entre sesiones, y cada actividad
nueva arrancaba de cero repitiendo instrucciones. Acá los archivos viven en
disco (se leen directamente, nunca se "pierden"), las convenciones fijas
viven en este `CLAUDE.md` (se cargan solas en cada sesión), y las skills en
`.claude/skills/` encapsulan el flujo repetible de redacción y construcción
del informe.

## Documentos de soporte fijos por actividad

Cada actividad trae siempre, dados por el profesor, hasta 4 documentos:

- **Guía de aprendizaje** — qué se espera de la actividad.
- **Anexo** — siempre lo entrega el profesor, pero cumple uno de dos roles
  distintos según la actividad:
  - **Plantilla a rellenar**: el anexo mismo es el documento base del
    informe (campos a completar, encabezado/pie de página institucionales
    que se preservan).
  - **Guía de recomendaciones**: el anexo solo orienta cómo redactar un
    informe libre, sin estructura a rellenar — el informe final no reutiliza
    su formato.
  Cuál de los dos aplica no se asume de antemano: se determina leyendo el
  contenido del anexo entregado en cada actividad.
- **Rúbrica** — criterios puntuados de evaluación.
- **Fuentes bibliográficas** — el material específico que hay que citar en
  el informe; nunca reemplazar esto por fuentes inventadas o traídas de
  memoria.

Estos cuatro van siempre en una carpeta `soporte/` dentro de la carpeta de
la actividad correspondiente.

## Consulta continua de la rúbrica y el soporte

La rúbrica y los documentos de soporte (guía, anexo, fuentes) no se
consultan solo al final. Ante cualquier duda durante la redacción o
construcción del informe — qué tan a fondo tratar un tema, qué formato usar,
qué se espera de una sección — la referencia siempre es releer esos
documentos, nunca asumir o generalizar desde memoria. `cumplimiento-rubrica`
sigue siendo el chequeo final obligatorio antes de entregar, pero no es la
única vez que la rúbrica entra en juego.

## Convención de carpetas

```
Materias/
  <Materia>/
    <YYYY-MM-DD>_<slug-actividad>/
      soporte/
        guia-aprendizaje.pdf
        anexo.docx
        rubrica.pdf
        fuentes-bibliograficas.pdf
      informe.docx              (o el formato que corresponda)
```

La fecha al frente del nombre de carpeta (fecha de entrega) hace que se
ordenen solas por urgencia en el explorador de archivos — útil cuando hay
varias actividades abiertas en paralelo.

## Materias del período

Lista canónica de materias del período (evita que una materia quede
duplicada por variaciones de escritura al crear carpetas). Mantenida por la
skill `nueva-actividad` cuando aparece una materia nueva.

- _(vacía: se llena sola al crear la primera actividad)_

## `ESTADO.md` — tablero de seguimiento

`ESTADO.md` en la raíz es la vista rápida de todas las actividades activas
(materia, estado, fecha de entrega). Se actualiza al arrancar o terminar
cada actividad. Existe para poder trabajar cómodo con dos o más actividades
a la vez sin perder de vista cuál va en qué estado — no hace falta abrir
cada carpeta para saberlo. Las skills `nueva-actividad` y `cerrar-actividad`
mantienen esta tabla actualizada automáticamente al abrir y cerrar una
actividad.

## `_plantillas/` — referencia para cuando el anexo es solo una guía

Cuando el anexo de la actividad cumple el rol de guía de recomendaciones (no
de plantilla a rellenar — ver sección "Documentos de soporte fijos por
actividad"), `_plantillas/formato-libre-referencia.docx` es el documento de
referencia transversal a todas las materias: se usa únicamente para tomar
encabezado y pie de página, no como fuente de contenido ni de estilo de
redacción. No pertenece a ninguna actividad puntual, por eso vive en la raíz
y no dentro de `Materias/`.

## `_bandeja-entrada/` — punto de entrada sin fricción

Cualquier archivo descargado del campus virtual se suelta ahí, sin pensar
en estructura ni nombres. Cuando se indique de qué actividad es, se
organiza desde ahí hacia su carpeta definitiva en `Materias/`.

### Guía y fuentes bibliográficas: archivo o texto en el chat, ambos válidos

Para la guía de aprendizaje y las fuentes bibliográficas hay dos caminos
igual de válidos, y cuál se use puede variar de una actividad a otra sin
ningún problema:

- **Archivo real** en `_bandeja-entrada/`, igual que el anexo y la rúbrica.
- **Texto pegado en el chat** (p. ej. una lista de referencias con links).
  En ese caso, ese texto se guarda verbatim —sin resumir, parafrasear ni
  corregir— en un archivo `.md` dentro de `_bandeja-entrada/` (o directamente
  en `soporte/` si la actividad ya existe), y de ahí en adelante se trata
  exactamente igual que un archivo entregado por el profesor.

El anexo y la rúbrica no tienen este segundo camino: siempre llegan como
archivo real.

## Cómo iniciar una sesión de Claude Code acá

Iniciar siempre desde esta carpeta raíz (`Universidad/`), no desde una
subcarpeta de actividad. No está confirmado que el descubrimiento de
skills de proyecto (`.claude/skills/`) funcione igual que el de `CLAUDE.md`
cuando el directorio de trabajo es una subcarpeta anidada — arrancar desde
la raíz y referenciar rutas relativas hacia la actividad evita ese riesgo.

## Skills disponibles

- **`redaccion-natural`** — estilo y voz: que el texto no suene a IA, y
  reglas de integridad (no inventar experiencia personal ni citas).
- **`plantilla-informe-docx`** — mecánica de producir el `.docx` siguiendo
  la plantilla del anexo y formato APA aplicado con Python.
- **`cumplimiento-rubrica`** — paso final: verificar que el informe cubre
  el 100% de los criterios puntuados y que toda cita es verificable contra
  las fuentes bibliográficas entregadas.
- **`nueva-actividad`** — automatiza el inicio de una actividad: crea la
  carpeta, mueve los archivos de soporte y actualiza `ESTADO.md`.
- **`cerrar-actividad`** — marca una actividad como entregada en `ESTADO.md`
  tras confirmar que pasó por `cumplimiento-rubrica`.
