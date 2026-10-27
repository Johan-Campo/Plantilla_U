---
name: cerrar-actividad
description: "Usar al terminar y entregar una actividad: verifica que se ejecutó cumplimiento-rubrica y marca la fila correspondiente en ESTADO.md como Entregado. Trigger: cerrar actividad, entregar actividad, marcar como entregada, /cerrar-actividad."
---

# Cerrar actividad

Cierra formalmente una actividad ya entregada, dejando `ESTADO.md`
actualizado.

## 1. Identificar la actividad

Si hay ambigüedad sobre a cuál actividad se refiere (por nombre o por
carpeta), preguntar cuál es antes de continuar.

## 2. Verificar cumplimiento de rúbrica

Antes de marcar como entregada, confirmar que la skill `cumplimiento-rubrica`
ya se corrió sobre el informe final de esa actividad. Si no hay evidencia de
eso en la conversación, preguntar al usuario si ya se verificó o si debe
correrse ahora.

## 3. Actualizar `ESTADO.md`

Cambiar el campo Estado de la fila correspondiente a `Entregado`.

## 4. Confirmar al usuario

Reportar el cambio hecho en `ESTADO.md`.
