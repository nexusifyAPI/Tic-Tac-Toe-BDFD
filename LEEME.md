# Tres en Raya para BDFD - Version Definitiva

Un juego completo de Tres en Raya para **Bot Designer For Discord** (BDFD) con modo **PvP** y modo **vs IA** (4 dificultades).

La dificultad **Imposible** usa un **algoritmo minimax real** con una tabla precomputada de 19,683 estados, lo que la hace matematicamente perfecta — nunca pierde.

---

## Caracteristicas

- **4 dificultades de IA**: Facil, Normal, Dificil, Imposible (minimax)
- **Modo PvP**: Reta a otro jugador con botones de aceptar/rechazar
- **Asignacion aleatoria de X/O**: A veces empiezas tu, a veces empieza la IA
- **Sistema de revancha**: Ambos jugadores deben votar para reiniciar (PvP)
- **Rendirse/Reiniciar**: Rindete en cualquier momento, reinicia tras terminar (vs IA)
- **Resaltado de linea ganadora**: Las 3 casillas ganadoras se ponen en verde
- **Contador de movimientos**: Muestra X/9 movimientos realizados
- **Sin variables de servidor**: El estado del juego se guarda en el footer del embed
- **Partidas simultaneas ilimitadas**: Cada tablero es independiente

---

## Configuracion

### 1. Crear los comandos

Necesitas crear **3 comandos** en tu bot de BDFD:

| # | Disparador | Lenguaje | Archivo | Descripcion |
|---|-----------|----------|---------|-------------|
| 1 | `!ttt` | BDScript 2 | `Tres-en-raya_BDFD.md` | Comando principal (menu IA o reto PvP) |
| 2 | `$onInteraction` | BDScript 2 | `Callback 1 ($onInteraction).md` | Maneja menu, retos, rendirse, revancha, reiniciar |
| 3 | `$onInteraction` | BDScript 2 | `Callback 2 ($onInteraction).md` | Maneja jugadas + IA minimax |

### 2. Por que dos callbacks `$onInteraction`?

BDFD soporta multiples callbacks `$onInteraction` — ambos se ejecutan en cada interaccion. La tabla minimax (19,683 caracteres) es demasiado grande para caber en un solo callback, por lo que el codigo se divide:

- **Callback 1**: Maneja las rutas A (menu de dificultad), B (aceptar reto), C (rechazar reto), E (rendirse/revancha/reiniciar)
- **Callback 2**: Maneja la ruta D (jugar casilla) + la tabla minimax

Cada callback usa `$stop` para no interferir con el otro.

### 3. Permisos del bot

El bot necesita estos permisos:
- Enviar mensajes
- Gestionar mensajes (para editar el tablero)
- Usar componentes (botones / menus de seleccion)

---

## Como funciona

### Almacenamiento del estado

El estado del juego se guarda en el **footer del embed** (no en variables de servidor), usando el formato:

```
ID_jugadorX|ID_jugadorO|dificultad|celda0.celda1.celda2.celda3.celda4.celda5.celda6.celda7.celda8|votoRevanchaX|votoRevanchaO
```

- `ID_jugadorX` / `ID_jugadorO`: IDs de usuario de Discord (o `IA` para modo IA)
- `dificultad`: `e` (Facil), `n` (Normal), `h` (Dificil), `i` (Imposible)
- `celda0`-`celda8`: `e` (vacia), `X`, o `O`
- `votoRevanchaX` / `votoRevanchaO`: `y` (voto) o `n` (no voto)

Esto hace que cada partida sea independiente — partidas simultaneas ilimitadas en el mismo canal.

### Dificultades de IA

| Dificultad | Estrategia |
|-----------|------------|
| **Facil** | Movimientos aleatorios |
| **Normal** | Ganar si es posible, bloquear si hay amenaza, sino aleatorio |
| **Dificil** | Ganar, bloquear, crear forks, bloquear forks del oponente, centro, esquina opuesta, esquina, lado |
| **Imposible** | **Minimax real** — movimiento optimo precomputado para los 19,683 estados. Nunca pierde. |

### Algoritmo minimax

La IA Imposible usa una **tabla minimax** precomputada en Python:

1. **Solver Python** (`minimax_solver.py`): Ejecuta un minimax exhaustivo sobre los 19,683 estados posibles del tablero (3^9). Para cada estado valido donde es el turno de la IA, calcula el movimiento optimo asumiendo juego perfecto del oponente.

2. **Tabla precomputada** (19,683 caracteres): Cada posicion de la cadena representa un estado del tablero (indexado por hash base-3). El caracter en esa posicion es el movimiento optimo (0-8) o `-` si el estado no es valido.

3. **En BDFD**, para la dificultad Imposible:
   - Mapea las celdas a numeros: `e=0`, `iaSym=1`, `userSym=2`
   - Calcula el hash: `hash = n0*1 + n1*3 + n2*9 + ... + n8*6561`
   - Busca en la tabla: `$splitText[$sum[$var[hash];1]]`
   - Ejecuta ese movimiento optimo

### Asignacion aleatoria de X/O

Al iniciar una partida (nueva, reiniciar o revancha), un numero aleatorio (`$random[0;2]`) determina quien es X y quien es O:

- **50% de probabilidad**: El usuario es X (empieza primero)
- **50% de probabilidad**: La IA es X (empieza primero, juega el centro automaticamente)

La IA esta completamente parameterizada con `$var[iaSym]` y `$var[userSym]` para funcionar correctamente sin importar si es X o O.

### Sistema de revancha (PvP)

Despues de que termina una partida PvP:
1. El boton cambia a "Revancha (0/2)"
2. Cada jugador presiona para votar — el boton se actualiza a "Revancha (1/2)"
3. Cuando ambos votan (2/2), el tablero se reinicia con nueva asignacion aleatoria de X/O
4. Solo los dos jugadores pueden votar; otros reciben un error

---

## Estructura de archivos

```
Repositorio/
│
├── README.md                          (version en ingles)
├── LEEME.md                           (este archivo - español)
│
├── English/
│   ├── Tic-Tac-Toe_Code.md            (comando !ttt)
│   ├── Callback 1 ($onInteraction).md (menu, retos, rendirse, revancha)
│   └── Callback 2 ($onInteraction).md (jugadas + IA minimax)
│
└── Español/
    ├── Tres-en-raya_BDFD.md           (comando !ttt)
    ├── Callback 1 ($onInteraction).md (menu, retos, rendirse, revancha)
    └── Callback 2 ($onInteraction).md (jugadas + IA minimax)
```

---

## Uso

### Jugar vs IA

```
!ttt
```

Selecciona una dificultad del menu desplegable. Se te asignara X o O aleatoriamente.

### Jugar vs otro jugador

```
!ttt @usuario
```

El jugador retado debe presionar "Aceptar" para empezar. X/O se asigna aleatoriamente.

### Durante la partida

- Haz clic en cualquier casilla vacia (⬜) para colocar tu marca
- Presiona "Rendirse" para rendirte
- Despues de terminar, presiona "Reiniciar" (vs IA) o "Revancha" (PvP) para jugar de nuevo

---

## Detalles tecnicos

- **Lenguaje**: BDScript 2 (necesario para `$eval`, `$var`, `$try`, `$async`)
- **Limite de caracteres**: Cada callback esta por debajo del limite de 65,500 caracteres de BDFD
- **Sin dependencias externas**: Todo esta autocontenido en los 3 comandos
- **Sin variables de servidor**: El estado del juego vive en los footers de los embeds

---

## Creditos

Creado por **Nexusify team**.

Eres libre de usar este codigo, pero por favor no quites los creditos. Tomo 3 meses de trabajo — respeta el esfuerzo.

---

## Licencia

Uso libre con creditos. No reclames el codigo como tuyo.
