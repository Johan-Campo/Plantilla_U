---
name: nueva-actividad
description: "Usar al iniciar una actividad nueva: crea la carpeta Materias/<Materia>/<fecha>_<slug>/soporte/, mueve los archivos correspondientes desde _bandeja-entrada/, y agrega la fila en ESTADO.md. Trigger: nueva actividad, actividad nueva, /nueva-actividad."
---

# Nueva actividad

Automatiza el arranque de una actividad: crear su carpeta, mover los
documentos de soporte y dejar registro en `ESTADO.md`.

## 1. Datos necesarios

- Materia.
- Nombre de la actividad.
- Fecha de entrega (YYYY-MM-DD).

Si el usuario no da los tres, preguntar solo el dato que falte.

## 2. Validar la materia

Comparar contra la lista canónica del período actual en `CLAUDE.md`, sección
"Materias del período". Si no coincide con ninguna, preguntar al usuario si
es una materia nueva (en ese caso, agregarla a esa lista) o si es una de las
existentes mal escrita.

## 3. Generar el slug de la actividad

A partir del nombre de la actividad: minúsculas, sin tildes/diacríticos,
espacios y guiones bajos convertidos a guion medio, y eliminar cualquier
carácter que no sea alfanumérico o guion medio.

## 4. Guía y fuentes bibliográficas: archivo o texto en el chat

Para la guía de aprendizaje y las fuentes bibliográficas, ambos caminos son
igual de válidos y pueden alternarse libremente entre actividades:

- Si llegan como archivo en `_bandeja-entrada/`, seguir el paso 5 normal.
- Si llegan como texto plano pegado en el chat (p. ej. una lista de
  referencias con links), guardar ese texto verbatim —sin resumir,
  parafrasear ni corregir— en un archivo `.md` (`guia-aprendizaje.md` o
  `fuentes-bibliograficas.md`) y tratarlo de ahí en adelante exactamente
  igual que un archivo entregado por el profesor.

El anexo y la rúbrica no tienen este segundo camino: siempre llegan como
archivo real en `_bandeja-entrada/`.

## 5. Revisar `_bandeja-entrada/` antes de mover nada

Si hay archivos de más de una actividad mezclados, o no es obvio cuáles
pertenecen a la actividad que se está creando, preguntar al usuario cuáles
archivos corresponden. Nunca mover a ciegas si hay ambigüedad.

## 6. Crear la estructura y mover archivos

- Crear `Materias/<Materia>/<fecha>_<slug>/soporte/`.
- Mover ahí los archivos identificados desde `_bandeja-entrada/` (incluyendo
  los `.md` creados en el paso 4, si aplica).

## 7. Actualizar `ESTADO.md`

Agregar una fila: `Materia | Actividad | Pendiente | Entrega`.

## 8. Confirmar al usuario

Reportar: carpeta creada, archivos movidos, y qué documentos de soporte
faltan. Ejemplos:

- El anexo siempre debe estar en soporte/, pero su rol (plantilla a rellenar
  vs. guía de recomendaciones para informe libre) se determina leyendo su
  contenido al construir el informe, no en este paso — ver skill
  `plantilla-informe-docx`, sección 0.
- Si faltan fuentes bibliográficas: avisarlo explícitamente — no se inventan
  citas (ver skill `redaccion-natural`).
