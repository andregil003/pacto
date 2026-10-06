# 🏰 PACTO

**Juego por turnos, en primera persona, en ASCII 3D. Un solo archivo HTML.
Sin dependencias, sin cuentas, sin costo, funciona sin conexión.**

Caminas por tu reino y en cada turno presentas un decreto en uno de los cinco
monumentos. Lo que decides se ve en el mundo. Después de nueve turnos tu reinado
tiene nombre.

---

## Qué es

Un juego de decisiones con exploración en primera persona. El mapa es un
*raycaster* ASCII —proyección tipo Wolfenstein dibujada con caracteres, sin GPU—.
No hay combate: se decide, y el mundo responde.

Caminas, acercas a un monumento, pulsas A, y se abre una carta con dos o tres
opciones. Eso es todo lo que hay que hacer.

---

## Controles

El juego detecta cómo estás jugando —mando, teclado o táctil— y cambia solo. No
se apilan los tres. Puedes forzarlo en *Ajustes*.

| | Mando | Teclado + ratón | Táctil |
|---|---|---|---|
| Caminar | stick izq. | `W A S D` | joystick |
| Mirar | stick der. | ratón (clic en el mundo) | arrastrar a la derecha |
| Hablar / confirmar | `A` | `Enter` | botón `A` |
| Cerrar | `B` | `Esc` | botón `B` |
| Elegir opción | cruceta `↑↓` | `↑↓` | `▲▼` |

Teclado: `N` noche · `H` pausa el día · `M` minimapa · `C` clima · `O` ajustes.

---

## Jugar

Abre `index.html` en el navegador. No necesita servidor ni instalación.

Opcional, para probarlo desde el móvil en la misma red:

```bash
python -m http.server 8080 --bind 0.0.0.0
```

Se puede fijar un reino con `?seed=1234` para repetirlo.

---

## Finales

Ocho épocas distintas según cómo gobiernes.

---

## Licencia

MIT.