---
name: plantilla-informe-docx
description: "Usar para producir el archivo .docx de un informe siguiendo la plantilla institucional (encabezado/pie de página del anexo) y formato APA aplicado con Python. No cubre redacción (ver skill `redaccion-natural`) ni verificación de cobertura de rúbrica (ver skill `cumplimiento-rubrica`)."
---

# Plantilla de informe en Word (.docx)

Flujo para producir un informe .docx que cumpla formato APA, partiendo de un documento "anexo" que el profesor entrega como plantilla.

## 0. Determinar el rol del anexo antes de construir el informe

El anexo siempre existe, pero cumple uno de dos roles — determinarlo leyendo
su contenido, no asumirlo:

- **Es la plantilla a rellenar**: seguir la sección 1 (preservar encabezado/pie
  del anexo, descartar su contenido de cuerpo).
- **Es solo una guía/recomendaciones para un informe libre**: el anexo no
  aporta plantilla utilizable. Usar en su lugar
  `_plantillas/formato-libre-referencia.docx` (raíz del proyecto) únicamente
  para tomar encabezado y pie de página — es un documento personal de
  referencia transversal a todas las materias, nunca fuente de contenido ni
  de estilo de redacción.

El resto del flujo (formato APA, verificación) aplica igual en ambos casos.

## 1. Preservar la plantilla institucional, no el contenido

- Del anexo del profesor, conservar únicamente encabezado y pie de página (logo, colores institucionales) vía python-docx: cargar el documento, borrar todos los párrafos y tablas del cuerpo dejando intacta la `sectPr`, y reconstruir el contenido desde cero.
- Nunca copiar literalmente texto del anexo (p. ej. párrafos sobre la metodología del curso).
- Reglas de integridad, tono y citas: ver skill `redaccion-natural`.

## 2. Formato APA aplicado con helpers reutilizables

Construir un pequeño set de funciones Python (add_math, heading1/heading2, para, mixed_para, portada_line, matrix_table) que apliquen consistentemente:

- Márgenes: 2.54 cm (1 in) los cuatro lados.
- Interlineado: verificar primero si la rúbrica de la actividad puntúa explícitamente el interlineado exacto; si no lo especifica, usar 1.5 por legibilidad, documentando el trade-off frente a APA estricto de 2.0.
- Sangría de primera línea: 1.27 cm (0.5 in) en párrafos de cuerpo; sangría francesa en referencias.
- Alineación IZQUIERDA (no justificada).
- Encabezados: mismo tamaño de fuente que el cuerpo, diferenciados solo con negrita/centrado (nivel 1) o negrita alineada a la izquierda (nivel 2) — nunca agrandar la fuente del título.
- Espaciado antes/después de cada encabezado (p. ej. 14pt antes, 8pt después) para que no quede "pegado" al párrafo siguiente — un interlineado de párrafo en 0pt dentro del texto no es suficiente separación visual entre título y cuerpo.
- Referencias con título en cursiva.
- Notación matemática inline: tokenizar la expresión y renderizar como texto con cursiva en variables sueltas (una letra) y símbolos de un set reservado, upright en operadores/paréntesis/números, y subíndice real vía `run.font.subscript = True` para sub-índices tipo C1, C2. Tamaño igual al cuerpo del texto (no usar imágenes de LaTeX/matplotlib, quedan mal integradas).
- NO intentar insertar ecuaciones nativas de Word (OOXML `<m:oMath>`) generadas a mano: se probó y LibreOffice (usado para verificar visualmente antes de entregar) las descarta silenciosamente incluso con XML válido según el esquema XSD — no hay forma de confirmar que se vean bien en Word real sin ese verificador, y el riesgo de entregar un documento con fórmulas rotas es alto. Si el usuario insiste en cumplir literalmente "usar el editor de ecuaciones de Word", la opción segura es que él mismo retipee las expresiones en Word con Insertar > Ecuación; el asistente puede darle el texto de cada expresión listo para copiar.

## 3. Cumplimiento de rúbrica

Antes de dar el informe por terminado, ejecutar la skill `cumplimiento-rubrica` — no es parte de esta skill.

## 4. Apéndice opcional — diagramas de relaciones/dígrafos

Aplica solo cuando el informe requiere un diagrama de relación binaria o dígrafo (típico de matemática discreta). No forma parte del flujo estándar de un informe cualquiera — no cargar esta sección mentalmente si la actividad no lo requiere.

Para dígrafos de relaciones binarias: usar Graphviz (`dot`) en vez de matplotlib a mano (resultados visuales muy superiores). Puntos clave:

- Colorear aristas por tipo de vínculo (reflexividad, equivalencia, jerarquía, etc.) y agregar una leyenda compuesta con PIL debajo de la imagen — esto resuelve confusión del usuario sobre bucles reflexivos u otras aristas no obvias.
- Si hay bucles de reflexividad en varios nodos, orientarlos con `headport`/`tailport` (w/e/n/s) de forma simétrica según la posición del nodo en el layout — si todos los bucles salen hacia el mismo lado (default de Graphviz), la caja delimitadora del dibujo queda asimétrica y el título centrado se ve descuadrado respecto a los nodos.
- Verificar que el conjunto de aristas dibujadas coincida exactamente con el conjunto de pares ordenados de la relación definida matemáticamente (contar pares) antes de dar el grafo por correcto.

## 5. Verificación iterativa obligatoria

Después de cada cambio: reconstruir el .docx completo (pipeline de scripts que se ejecutan en orden desde una copia limpia del anexo), validar con el script `validate.py` de la skill docx, convertir a PDF con LibreOffice headless, renderizar cada página como imagen y leerla visualmente antes de entregar — nunca asumir que un cambio de formato se ve bien sin confirmarlo así. Repetir este ciclo completo por cada ronda de feedback del usuario, no acumular cambios sin verificar.
