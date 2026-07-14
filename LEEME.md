# Tres en Raya para BDFD

Juego de Tres en Raya para Bot Designer For Discord (BDFD), con modo PvP y modo contra IA en 4 dificultades. La dificultad Imposible usa minimax con una tabla precalculada de los 19,683 estados posibles del tablero, así que no pierde nunca.

## Características

- 4 niveles de IA: Fácil, Normal, Difícil, Imposible (minimax)
- Modo PvP con botones de aceptar/rechazar el reto
- Asignación aleatoria de X/O en cada partida
- Revancha: los dos jugadores tienen que votar para reiniciar
- Rendirse y reiniciar en modo IA
- La línea ganadora se resalta en verde
- Contador de movimientos (X/9)
- No usa variables de servidor, el estado se guarda en el footer del embed
- Partidas simultáneas sin límite, cada tablero es independiente

## Instalación

Hacen falta 3 comandos en tu bot:

| # | Disparador | Lenguaje | Archivo | Qué hace |
|---|-----------|----------|---------|----------|
| 1 | `!ttt` | BDScript 2 | `Tres-en-raya_BDFD.md` | Comando principal (menú de IA o reto PvP) |
| 2 | `$onInteraction` | BDScript 2 | `Callback 1 ($onInteraction).md` | Menú, retos, rendirse, revancha, reiniciar |
| 3 | `$onInteraction` | BDScript 2 | `Callback 2 ($onInteraction).md` | Jugadas + IA minimax |

**¿Por qué dos callbacks `$onInteraction`?** La tabla minimax ocupa 19,683 caracteres y no entra junto al resto del código en un solo callback, así que se dividió en dos. Cada uno usa `$stop` para no pisarse con el otro.

Permisos que necesita el bot: enviar mensajes, gestionar mensajes (para poder editar el tablero) y usar componentes (botones y menús).

## Cómo funciona

### Estado del juego

Se guarda en el footer del embed, con este formato:

```
ID_jugadorX|ID_jugadorO|dificultad|celda0.celda1.celda2.celda3.celda4.celda5.celda6.celda7.celda8|votoRevanchaX|votoRevanchaO
```

- `ID_jugadorX` / `ID_jugadorO`: ID de Discord del jugador (o `IA`)
- `dificultad`: `e` Fácil, `n` Normal, `h` Difícil, `i` Imposible
- `celda0` a `celda8`: `e` vacía, `X` u `O`
- `votoRevanchaX` / `votoRevanchaO`: `y` o `n`

Como no depende de variables de servidor, cada partida queda aislada y podés tener varias corriendo a la vez en el mismo canal.

### Dificultades

| Dificultad | Cómo juega |
|-----------|------------|
| Fácil | Movimientos al azar |
| Normal | Gana si puede, bloquea si hay amenaza, si no juega al azar |
| Difícil | Gana, bloquea, arma forks, bloquea forks del rival, centro, esquina opuesta, esquina, lado |
| Imposible | Minimax con tabla precalculada para los 19,683 estados. No pierde. |

### El algoritmo minimax

Para la dificultad Imposible, un script de Python precalcula la jugada óptima para los 19,683 estados posibles del tablero y los guarda en una tabla de consulta. En BDFD, las celdas se mapean a números, se hashea el estado del tablero, se busca la jugada óptima en la tabla y se juega esa casilla.

### Asignación de X/O

Al empezar una partida (nueva, reinicio o revancha), `$random[0;2]` decide quién es X: la mitad de las veces empezás vos, la otra mitad empieza la IA (y juega el centro automáticamente). La IA está parametrizada con `$var[iaSym]` y `$var[userSym]`, así que funciona igual sea X u O.

### Revancha (PvP)

Cuando termina una partida PvP, el botón cambia a "Revancha (0/2)". Cada jugador vota apretándolo, y cuando llega a 2/2 el tablero se reinicia con X/O asignados de nuevo al azar. Solo los dos jugadores originales pueden votar; si otro intenta, le sale error.

## Uso

Contra la IA: escribí `!ttt` y elegí dificultad en el menú desplegable. Se te asigna X u O al azar.

Contra otro jugador: `!ttt @usuario`. El retado acepta con el botón, y X/O también se reparte al azar.

Durante la partida: clic en una casilla vacía (⬜) para jugar, "Rendirse" para abandonar, y al terminar "Reiniciar" (vs IA) o "Revancha" (PvP).

## Créditos

Hecho por Nexusify team. Podés usar el código libremente, solo mantené los créditos.

## Licencia

Uso libre con créditos. No lo presentes como propio.
