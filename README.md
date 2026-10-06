# 🏰 PACTO — El Salón del Trono

**Juego por turnos, en primera persona, en ASCII 3D. Un solo archivo HTML. Sin dependencias,
sin cuentas, sin costo. Funciona sin conexión.**

Caminas por tu reino y en cada turno presentas **un decreto** en uno de los cinco monumentos.
Lo que decides se ve en el mundo: tu gente aparece o desaparece de la plaza, aparecen
centinelas, las hogueras arder más o menos. Y hay un vecino, el **Rey Aureliano**, que te
mide y que crece conforme le cedes poder.

---

## Qué es

Un juego de decisiones por turnos con exploración en primera persona. El mapa es un raycaster
ASCII (proyección tipo Wolfenstein dibujada con caracteres, sin GPU). No hay combate: se
decide, y el mundo responde.

**9 turnos.** Seis decisiones barajadas, una ninth que lo cambia todo, y dos más que dependen
de esa. Al cerrar el reigning recibes el **nombre de tu época** y tu **puntaje de legado**.

---

## Los cinco monumentos

Cada turno se marca una sola audiencia. Te acercas y se abre la carta con 2 o 3 opciones.

| Monumento | Coste | Lo que ves cambiar en el mundo |
|---|---|---|
| **La Plaza** | 5 📦 | cuántos aldeanos hay caminando (1-15) |
| **El Almacén** | 7 📦 | — |
| **El Árbol** | 3 📦 | — |
| **La Guardia** | 9 📦 | cuántos centinelas armados rondan la plaza (0-7) |
| **La Hoguera** | 6 📦 | cuántas hogueras arden (0-6) |

Nunca te atascas: si no te alcanza el coste, pagas lo que tengas.

---

## La Amenaza

La barra roja es el Rey Aureliano. No es un temporizador: **sube porque tú lo subes**.

```
+7 por turno
+1,5 por punto de poder que le cediste a la autoridad
+3 extra si tu libertad baja de 30
+2 extra si tu conflicto pasa de 70
+6 de golpe si aceptaste el pacto
```

Orden y libertad son lo que más le nutren. Si llegas al final por encima de 88, viene a cobrar.

---

## Controles

El juego detecta cómo estás jugando y cambia solo: **mando**, **teclado + ratón** o
**táctil**. No se apilan los tres. Puedes forzarlo en *Ajustes*.

| | Mando | Teclado + ratón | Táctil |
|---|---|---|---|
| Caminar | stick izq. | `W A S D` | joystick |
| Mirar | stick der. | ratón (clic en el mundo) | arrastrar a la derecha |
| Hablar / confirmar | `A` | `Enter` | botón `A` |
| Cerrar | `B` | `Esc` | botón `B` |
| Elegir opción | cruceta `↑↓` | `↑↓` | `▲▼` |

Extras con teclado: `N` noche · `H` pausa el día · `M` minimapa · `C` clima · `O` ajustes ·
`-` `=` tamaño del texto.

---

## Jugar

Abre `index.html` en el navegador. No necesita servidor ni instalación.

Opcional, para probarlo desde el móvil en la misma red:

```bash
python -m http.server 8080 --bind 0.0.0.0
```

Se puede fijar un reino concreto con `?seed=1234` para repetirlo.

---

## Finales

Ocho épocas distintas según cómo gobiernes: desde **Orden de Hierro** (seguridad alta,
libertad por los suelos) hasta **Libertad Frágil** (mucha autonomía y poca protección), más
**Equilibrio Imperfecto**, **Paz en Tensión**, **Cooperación Inestable** y otras.

---

## Licencia

MIT.