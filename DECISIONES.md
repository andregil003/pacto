# 🧠 DECISIONES — PACTO · El Salón del Trono

Registro de decisiones: **qué** se decidió y **por qué**. Y sobre todo **qué bugs costaron
tiempo**, para que nadie los repita.

---

## D-001 · Carretera 1: el 3D es tu reino, no un fondo

**Decisión:** el raycaster ASCII es el espacio jugable. El rey camina por su aldea.

**Por qué:** se evaluaron tres caminos y se eligió el híbrido. La alternativa "anillo de duelo"
habría tirado el alma filosófica del PACTO; la de "mundo ASCII puro" era mucho riesgo para el
resultado. Con el híbrido, el mando tiene **dos trabajos reales** (caminar y elegir carta) en
vez de ser decorativo.

**Decidido por André** en la ronda 1 del diseño.

---

## D-002 · Rival = una sola variable, no cinco stats

**Decisión:** el Rey Aureliano es **una sola variable: Amenaza (0-100)**.

**Por qué:** se evaluaron (a) 1 rey IA, (b) 2-4 reyes, (c) sin rival + ranking, (d) 2 jugadores
pass & play. André eligió "la más simple de a o b" → 1 rey. Y en vez de darle stats propias
(inventar un segundo sistema de juego), su Amenaza **se deriva de tus decisiones**:

```
+7 por turno · +1,5 por punto de poder cedido · +3 si libertad < 30 · +2 si conflicto > 70
```

Así el rival es un **espejo de tu política** y no un contrincante artificial. Y de paso se
reutiliza la variable `authorityPower` que el PACTO original ya tenía: no hizo falta inventar
nada.

---

## D-003 · El turno tiene que *ir* a un sitio

**Decisión:** el evento no se abre en cualquier parte. Cada turno se marca **una audiencia en un
monumento concreto** y hay que caminar hasta allí.

**Por qué:** si la carta se abría en el sitio, el juego 3D era un fondo y la caminata no
importaba. Que el objetivo del turno sea "llegar a X" es lo que convierte el mando en
necesario. En la prueba de QA con 15 aldeanos usando pathfinding propio, la caminata es el
80% de la partida.

---

## D-004 · Las 5 estadísticas = los 5 monumentos

**Decisión:** un monumento por estadística, y el estado del reino **se ve en el mundo**.

| Stat | Monumento | Qué se ve |
|---|---|---|
| Población | La Plaza (fuente) | 1-15 aldeanos caminando |
| Recursos | El Almacén (toneles) | — |
| Libertad | El Árbol (roble) | — |
| Seguridad | La Guardia (farol) | 0-7 centinelas armados |
| Conflicto | La Hoguera (antorcha) | 0-6 hogueras encendidas |

**Por qué:** se eligió expresamente "decorativo pero reactivo": el evento **no** depende del
monumento (si dependiera, te perderías eventos y te frustrarías) pero el mundo **sí** cambia.
El coste de hacerlo así fue casi cero porque el renderer ya sabía dibujar aldeanos, antorchas
y faroles.

---

## D-005 · Todo el arte del juego sale del motor. Cero pixel art nuevo.

**Decisión:** los 5 monumentos reusan `fountain`, `barrel`, `oak`, `lamp` y `torch`, que ya
existían en el motor. Solo se escribieron tres piezas nuevas: `beam`, `ring` y un `marker`
más grande.

**Por qué:** André pidió explícitamente no "copiar todo". Añadir arte propio era el único sitio
donde había que escribir pixel art de verdad, y se minimizó a tres piezas signaling.

---

## D-006 · Entrada: UN modo activo, no tres apilados

**Decisión:** el juego tiene un `IN.mode` ∈ {`key`, `pad`, `touch`}. Gana **el último que se
usó** (una tecla, un stick, un dedo) y todo lo demás se recalcula: el indicador del HUD, el
texto de los prompts, el mensaje del suelo y la ayuda.

**Por qué:** la versión anterior pintaba joystick táctil en un PC de escritorio y leía teclado
y mando a la vez. Se accumulatesen tres一套 controles compitiendo. Además André lo pidió
explícitamente: *"detecta si estamos jugando en mando o en teclado o en teléfono, porque eso sí
está configurado"*.

Se respeta `OPT.touch` (`auto` / `on` / `off`) del juego base, y se añade `OPT.force` para
fijar el modo a mano.

**Persistencia:** `localStorage` bajo la clave `pacto_opts_v1` (la del juego base era
`asciifps5opts`).

---

## D-007 · Se borró la dead code, no se escondió

**Decisión:** cueva, combate, misiones, tienda, inventarios, guardado e interiores **se
eliminaron del archivo**. ~1.100 menos.

**Por qué:** la primera versión los dejaba ocultos con `display:none !important` y funcionando:
"copiado todo". Eso es deuda con intereses. Además la cueva era actively confusa: aparecía en
el minimapa, en las paredes, en los mensajes de los aldeanos ("no me meto a la cueva") y en el
código, sin existir para nada. Un jugador que lo descubriera se sentiría engañado.

El mundo pasó a ser **un solo espacio**: la aldea y sus alrededores.

**Cómo se borró:** con un removedor de funciones consciente de llaves y de comillas
(`scripts` de la fase A), no a mano. Un borrado por número de línea se comió código válido
— se detectó y se revirtió antes de seguir.

---

## D-008 · La Federación de aldeanos no bloquea al rey

**Decisión:** `blockedPlayer()` ignora a los actores. El rey atraviesa a la gente.

**Por qué:** con 15 aldeanos con colisión en una aldea de 32×24, se formaban **tapones**: el
flood fill de QA devolvió "sin ruta" porque un aldeano bloqueaba el tile entero. No es un
detalle: es la diferencia entre una partida jugable y una partida imposible. Los muros, el
agua, las montañas y los monumentos **sí** bloquean.

---

## D-009 · Nunca hay un turno imposible

**Decisión:** `payAmount() = min(coste, recursos)`. Si no te alcanza, pagas lo que tienes. El
turno del PACTO y la Traición son gratis. Si no puedes pagar nada, se marca el monumento más
barato.

**Por qué:** la primera versión rechazaba la audiencia si no llegabas al coste. Con el turno 7
(forzado a La Plaza, coste 8) y 5 de recursos, **la partida se trababa para siempre** en la
QA. La regla de diseño era "solo se marca amongstros que puedes pagar" pero el turno del PACTO
rompe esa regla por diseño.

**Corolario:** el equilibrio se ajustó para que la economía no se vacíe por los costes:

| | Antes | Ahora |
|---|---|---|
| Plaza / Almacén / Árbol / Guardia / Hoguera | 8 / 12 / 6 / 15 / 10 | 5 / 7 / 3 / 9 / 6 |
| Recursos iniciales | 70 | 85 |
| Umbral de FRACTURA TOTAL | recursos ≤12 | recursos ≤4 |

Antes, los costes de audiencia (48 pts en 6 turnos) dejaban siempre los recursos en 0-11 y la
primera regla de `determineEnding()` era `resources <= 12`: **los otros 7 finales eran
inalcanzables**. Se verificó: 5 estrategias distintas daban el mismo final.

---

## D-010 · El `!` had three capas, no una

**Decisión:** la señal de la audiencia es (1) un **haz de luz** dorado visible a más de 6.5 m,
(2) un **anillo pulsante en el suelo**, (3) el `!` flotante, (4) una **brújula en el HUD** con
nombre + flecha + distancia, (5) el minimapa con el punto resaltado.

**Por qué:** André dijo que el `!` "no es muy intuitivo o no lo miro yo". Un `!` de 3 celdas
en un mundo ASCII a 130×64 no se ve. El haz se apaga a menos de 6.5 m porque a 4 m una columna
de 4.6 unidades tapa media pantalla — es una señal de **larga distancia**.

---

## D-011 · La Traición se dispara a 88, no a 100

**Decisión:** el mensajero del Rey Aureliano aparece cuando `amenaza >= 88`.

**Por qué:** se probaron los dos caminos extremos y medidos:
camino autoritario → Amenaza 91 · camino libertario → 83. Con el umbral en 100 el evento
**nunca ocurría**: era una dead feature. A 88 solo te viene si te convertiste en el tirano,
que es exactamente la intención de diseño.

---

## 🐛 BUGS QUE COSTARON SANGRE

Los cinco que发现了 la QA automatizada. No volver a repetirlos.

### B-001 · `clamp()` sin valores por defecto

```js
// mal (el del motor, sin defaults)
const clamp = (v, a, b) => v < a ? a : (v > b ? b : v);
// bien
const clamp = (v, a = 0, b = 100) => v < a ? a : (v > b ? b : v);
```

El motor usaba `clamp(x, 0, 1)` siempre con 3 argumentos, pero la capa de juego llamaba
`clamp(x)`. Comparar contra `undefined` da `false`, así que **devolvía el valor sin tocar**:
los stats salían negativos (`resources: -4`, `freedom: 104`). Pasaba inadvertido porque la
barra del HUD se dibuja con `clamp(v, 0, 100)` y se recortaba sola.

**Regla:** si tocas `clamp`, revisa todos los sitios que la llaman con menos de 3 argumentos.

### B-002 · `if (c.pact)` con `false` como valor legítimo

El PACTO tiene dos opciones: `pact: true` (aceptar) y `pact: false` (rechazar). `if (c.pact)`
solo entraba en la rama de aceptar. Resultado: **rechazar el pacto no ejecutaba su rama**,
`G.branch` se quedaba vacío y los turnos 8-9 abrían `undefined`.

**Regla:** `if (c.pact !== undefined)`. En general: cuando un flag puede ser `false` como
respuesta válida, no uses su verdad como discriminante.

### B-003 · El teclado no movía: faltaba `keys[k] = true`

Al reescribir el handler de teclado para no apilar las tres entradas, se olvidó registrar
las teclas de movimiento. Solo se guardaban `arrowleft` y `arrowright`. Resultado: **WASD
muerto**, y como `arrowup`/`arrowdown` tenían un `return` antes, también las flechas.

**Regla:** cuando reescribas un handler de input, prueba cada tecla documentada con un
teclado real, no con la lógica.

### B-004 · La cámara con mando iba a 30 fps

El giro del stick se aplicaba dentro del poll del gamepad (30 fps) mientras el
render iba a 60. Resultado: **un salto de cámara cada 2 frames**, que André reportó como
"se ve todo trabado". Ahora el poll solo guarda el valor crudo y `step()` (60 fps) lo aplica
con suavizado exponencial. La cruceta da un impulso instantáneo, no una velocidad.

### B-005 · El haz de luz salía negro

`S(bg, bm, fg, fm, ch, 1)` con `em=1` significa "ignora la iluminación y usa estos colores tal
cual". Los colores del haz estaban multiplicados por un factor pequeño, así que a la puesta de
sol salía una **columna negra** en medio de la pantalla. Con `em=0` lo ilumina el mundo, pero
entonces de noche no se ve. Solución: `em=1` con colores fijos y brillantes.

---

## 📐 Verificación

- `node --check` sobre el JS extraído del HTML.
- Auditor de texto propio: busca mojibake (UTF-8 leído como latin-1), `U+FFFD` y palabras de
  la versión FPS que no deberían existir (cueva, zombi, herrero, espada, escudo, misión).
- QA automatizada en navegador: 5 partidas completas con estrategias distintas, con un
  pathfinder BFS propio, comprobando 9 cartas, 0 atascos, stats dentro de rango, y los finales.

**Limitación conocida:** las pruebas automatizadas nunca disparan un gamepad real. El hook está
escrito y el modo se fuerza con `OPT.force = "pad"`, pero el mando **lo tiene que probar André**.