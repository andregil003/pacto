# PROGRESO — bitácora

## 2026-10-06 · v0.1

### Diseño (4 rondas de entrevista, André decidió)

| Ronda | Decisión |
|---|---|
| 1 | Carretera 1 (el 3D es tu reino). Rival: 1 rey IA. Repo nuevo `pacto`. |
| 2 | 9 eventos. Amenaza visible + sombra del rival. Final = época + puntaje. |
| 3 | El evento se abre en un monumento marcado (hay que caminar). Rival = 1 variable. Traición al llegar a 100. |
| 4 | Solo se marcan amongstros pagables. Todo se paga en Recursos. Se reusan las 8 épocas del PACTO. |

### Construcción

1. Lectura del motor ASCII 3D y del `index.html` original de PACTO (todo el contenido textual:
   16 eventos, las 2 ramas, el PACTO y la lógica de los 8 finales).
2. Copia del motor a `pacto/index.html` + inyección del contenido de PACTO con un script, sin
   reescribir una línea de texto.
3. Capa de juego: estado, turnos, cartas, amenaza, puntuación, finales.
4. **Fase de limpieza** — borrado de la dead code heredada del motor base (cueva, combate,
   misiones, guardado) con un removedor de funciones consciente de llaves. ~1.100 líneas fuera.
5. **Fase de entrada** — sistema de un solo modo activo.
6. QA automatizada en navegador con pathfinder BFS propio.

### Bugs encontrados por QA (5+3)

| # | Bug | Cómo se descubrió |
|---|---|---|
| B-001 | `clamp()` sin defaults → stats fuera de rango | la QA Always daba el mismo final |
| B-002 | `if (c.pact)` rompía la rama "sin autoridad" | los turnos 8-9 abrían `undefined` |
| B-003 | teclado muerto (faltaba `keys[k] = true`) | André: "ya no me puedo mover con teclado" |
| B-004 | cámara del mando a 30 fps | André: "se ve todo trabado al cambiar la cámara" |
| B-005 | el haz de luz salía negro | captura de pantalla |
| — | árboles encima de los monumentos | la QA no encontraba ruta |
| — | aldeanos bloqueando el paso | la QA no encontraba ruta |
| — | turno 7 imposible con pocos recursos | la QA se trabó en el turno 7 |
| — | `FRACTURA TOTAL` siempre | los 5 finales tests dieron el mismo |
| — | Traición inalcanzable (Amenaza top 91) | instrumentación |

### Pendiente

- [ ] **Probar el mando real** — la QA automatizada nunca dispara un gamepad.
- [ ] Probar en el móvil (táctil) en un Moto G56.
- [ ] Probar los 8 finales (la QA solo alcanzó 4 de forma natural).
- [ ] Semilla fija para compartir un reino concreto por URL.

### Ideas para la v0.2

- Más de un rival (3 reinas IA) en la misma plaza.
- Que los aldeanos tengan nombre y den frases al cruzarse contigo.
- Pronóstico de la Amenaza ("tu Liberalidad alimenta al rival") en el HUD.