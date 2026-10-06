# 🏰 PACTO — El Salón del Trono

**Juego por turnos con mando. Un solo archivo HTML. Cero dependencias. 100% offline y gratis.**

Caminas por tu reino en primera persona (ASCII 3D) y en cada turno presentas **un decreto**
en uno de los cinco monumentos. Lo que decides se ve en el mundo y lo ve tu vecino, el
**Rey Aureliano**, que nunca deja de odiarte por delegar poder.

```
   D-pad / stick izq ──►  ┌────────────────────────────┐
   A / Cross ───────────►  │          PACTO            │  ◄──── 9 turnos, 2 ramas
   B / Circle ─────────►  │   5 monumentos · 8 épocas  │
                          └────────────────────────────┘  ◄── El Rey Aureliano
```

---

## ⚡ Jugar ahora mismo (0 installs)

Un solo archivo. Doble click y listo. Nada de `npm install`, nada de build.

```bash
# opcional: servirlo en la LAN para probarlo desde el móvil
cd /c/Users/andre/Documents/GitHub/pacto
python -m http.server 8080 --bind 0.0.0.0
# → http://localhost:8080
```

Puedes fijar el reino con `?seed=1234` para repetir la misma partida.

---

## 🎮 Controles

El juego **detecta cómo estás jugando** y cambia solo. No apila mando + teclado + táctil:
gana el último que usaste, el indicador de la barra superior lo dice, y la ayuda se
reescribe. Puedes forzarlo en **AJUSTES → Forzar modo de entrada**.

| Acción | Mando | Teclado + ratón | Táctil |
|---|---|---|---|
| Caminar | stick izq. | `W` `A` `S` `D` | joystick izquierdo |
| Mirar | stick der. | ratón (clic en el mundo) | arrastrar en la mitad derecha |
| Hablar / confirmar | **A** / Cross | `Enter` / `Espacio` | botón **A** |
| Cerrar | **B** / Circle | `Esc` | botón **B** |
| Elegir opción | cruceta ↑↓ | `↑` `↓` | **▲▼** |
| Girar (sin stick der.) | cruceta ←→ | `←` `→` | — |
| Ayuda | — | `O` | — |
| Noche / pausar el día | — | `N` / `H` | — |
| Minimapa / clima | — | `M` / `C` | — |
| Tamaño del texto | — | `-` `=` | — |

Mapeo estándar Xbox/PS. El hook de Gamepad API está escrito a mano (~60 líneas), sin librería.

---

## 🏛️ Los cinco monumentos

Cada turno el juego marca **una sola audiencia** con un `!` dorado, un anillo en el suelo y
un haz de luz visible desde el otro extremo de la aldea. Te acercas, pulsas A y se abre la carta.

| Monumento | Cuesta | Estadística que ves en el mundo |
|---|---|---|
| **La Plaza** (la fuente) | 5 📦 | Aldeanos caminando: de 1 a 15 según población |
| **El Almacén** (los toneles) | 7 📦 | — |
| **El Árbol** (el roble del primer rey) | 3 📦 | — |
| **La Guardia** (el farol) | 9 📦 | Centinelas armados rondando la plaza: 0 a 7 |
| **La Hoguera** (la antorcha) | 6 📦 | Hogueras encendidas: 0 a 6 según conflicto |

**Nunca te atascas:** si no te alcanza el coste, pagas lo que tienes en despensa. Si no te
alcanza para nada, se marca el monumento más barato.

---

## 🎲 La partida

1. **Turnos 1-6** —seis situaciones barajadas de entre 16, con tres opciones cada una.
2. **Turno 7** — **EL PACTO**. Aceptas crear una autoridad común o la rechazas. Aquí se decide
   el resto de tu reinado.
3. **Turnos 8-9** — dos situaciones de la rama que elegiste.
4. **Traición** — si llegas con la Amenaza por encima de 88, el Rey Aureliano manda a su
   mensajero. Solo viene si te convertiste en el tirano.

**Las 5 estadísticas** (0-100) son las del PACTO original: población, recursos, libertad,
seguridad, conflicto. Al final tienes una **época** (el nombre de tu reinado) y un
**puntaje de legado**.

| Final | Se llega cuando… |
|---|---|
| FRACTURA TOTAL | colapso: población ≤55 o recursos ≤4 |
| ORDEN DE HIERRO | pacto + libertad ≤22 + seguridad ≥72 + mucho poder |
| ORDEN ESTABLE | pacto + orden sinFacetura |
| LIBERTAD FRÁGIL | sin pacto + libertad ≥72 pero sin seguridad |
| COOPERACIÓN INESTABLE | sin pacto y todo equilibrado |
| SEGURIDAD SIN RESERVAS | inviertes en control hasta vaciar la despensa |
| EQUILIBRIO IMPERFECTO | ningún valor domina |
| PAZ EN TENSIÓN | evitas el colapso sin resolver la tensión |

---

## 🕹️ La Amenaza

La barra roja de arriba es el **Rey Aureliano**. No es un temporizador: sube porque **tú**
lo subes.

```
  +7 por turno
  +1,5 por punto de poder que le cediste a la autoridad
  +3 extra si tu libertad baja de 30
  +2 extra si tu conflicto pasa de 70
  +6 de golpe si aceptaste el pacto
```

Mientras más orden y menos libertad → más amenaza. El mundo lo muestra: el rival se acerca al
centro de la plaza, el cielo se pone rojo y el minimapa lo marca.

---

## 🏗️ Qué se reusó y qué se tiró

**Del motor ASCII 3D** (`ascii3DWorld`) se quedó el motor de render: raycasting, texturas,
cielo, clima, sombras, minimapa. Es un raycaster software en ASCII puro, sin GPU.

**Del juego PACTO** se reusó todo el contenido textual: los 16 eventos, las dos ramas, el
PACTO y la lógica de los 8 finales.

**Se borró** (no se escondió con CSS): la cueva, el combate, las espadas y arcos, los
escudos, la vida y el aguante, los enemigos, las misiones, el herrero y la tienda, los
inventarios, los objetos recogibles, el guardado de partida, los interiores de las casas, el
menú de opciones del juego base y los controles táctiles del juego base. ~1.100 líneas.

---

## 📜 Documentación

- [`DECISIONES.md`](DECISIONES.md) — por qué está hecho así, con los bugs que costaron sangre
- [`PROGRESO.md`](PROGRESO.md) — bitácora
- [`CHANGELOG.md`](CHANGELOG.md) — versiones

---

## 📜 Licencia

MIT.

---

**Hecho por André con PUCK**, el Project Manager, Arquitecto y Líder de QA.