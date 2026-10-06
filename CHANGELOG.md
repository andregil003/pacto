# CHANGELOG

## v0.1 — 2026-10-06

Primer jugable completo. Un solo archivo `index.html`, cero dependencias, offline.

### Juego
- 9 turnos: 6 situaciones barajadas de 16, el PACTO en el turno 7, 2 de la rama elegida.
- 5 estadísticas (población, recursos, libertad, seguridad, conflicto) con barras en el HUD.
- 5 monumentos con coste en recursos: La Plaza 5 · El Almacén 7 · El Árbol 3 · La Guardia 9 ·
  La Hoguera 6.
- El estado del reino se ve en el mundo: aldeanos, centinelas y hogueras escalan con sus
  estadísticas.
- El Rey Aureliano: sombra en la plaza que se acerca al centro conforme sube la Amenaza,
  más minimapa, más viñeta roja en pantalla.
- Traición del rival al llegar a Amenaza 88.
- 8 épocas finales + puntaje de legado, con desglose de estadísticas y lista de decretos.
- Flauta de tonos sintetizados con WebAudio (sin ficheros).

### Entrada
- Detección de modo: mando / teclado / táctil. Gana el último usado; HUD, prompts, suelo y
  ayuda se reescriben.
- `AJUSTES`: sensibilidad, tamaño de texto, campo de visión, invertir Y, minimapa,
  táctil auto/on/off y **forzar modo**.
- Persistencia en `localStorage` (`pacto_opts_v1`).
- Táctil: joystick, zona de mirar a la derecha, botones A y B, y ▲▼ para las cartas.
- Gamepad API escrita a mano, poll a 30 fps, autodetección, deadzone 0.28.

### Motor
- Raycaster ASCII software heredado de `ascii3DWorld` con su sky, clima, sombras y minimapa.
- Tres sprites nuevos: haz de luz, anillo de suelo y `!` grande.

### Corregido
- `clamp()` sin valores por defecto hacía que los stats salieran de rango.
- `if (c.pact)` ignoraba la rama de "rechazar el pacto" y rompía los turnos 8-9.
- El teclado no registraba `WASD` ni las flechas.
- La cámara del mando se aplicaba a 30 fps y daba escalones.
- La Vegetación decorativa tapaba los monumentos y bloqueaba el paso.
- Los aldeanos con colisión formaban tapones: la partida se hacía injugable.
- Con recursos bajos el turno del PACTO era imposible: ahora se paga lo que haya.
- El balance dejaba siempre `FRACTURA TOTAL`: los otros 7 finales eran inalcanzables.
- La Traición a Amenaza 100 nunca se disparaba.

### Eliminado
Cueva, combate, espadas, arcos, escudos, vida, aguante, enemigos, misiones, herrero, tienda,
inventarios, objetos recogibles, guardado, interiores de casas, menú y opciones del juego base,
controles táctiles del juego base. ~1.100 líneas.