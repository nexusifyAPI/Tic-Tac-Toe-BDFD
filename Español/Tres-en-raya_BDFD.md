# Disparador

```
!ttt
```

## Codigo

```
$nomention
$disableInnerSpaceRemoval

$var[oponente;$mentioned[1;no]]

$try

$if[$var[oponente]==]

$title[Selecciona la dificultad]
$description[Elige contra que nivel de IA quieres jugar:

🟢 **Facil** — La IA juega al azar
🔵 **Normal** — Ataca y bloquea lo basico
🟠 **Dificil** — Juega con estrategia
🔴 **Imposible** — Minimax real, nunca pierde]
$color[#5865F2]
$footer[Tu seras ❌ o ⭕ al azar]

$newSelectMenu[ttt-diff;1;1;Elige la dificultad...]
$addSelectMenuOption[ttt-diff;Facil;e~$authorID;La IA juega al azar;no;🟢]
$addSelectMenuOption[ttt-diff;Normal;n~$authorID;Ataca y bloquea;no;🔵]
$addSelectMenuOption[ttt-diff;Dificil;h~$authorID;Juega con estrategia;no;🟠]
$addSelectMenuOption[ttt-diff;Imposible;i~$authorID;Minimax perfecto;no;🔴]

$else

$onlyIf[$var[oponente]!=$authorID;❌ ¡No puedes retarte a ti mismo!]
$onlyIf[$isBot[$var[oponente]]==false;❌ ¡No puedes retar a un bot!]

$title[¡Nuevo Reto de 3 en Raya!]
$description[<@$authorID> ha retado a <@$var[oponente]> a un 3 en Raya.

Quien sera ❌ y quien ⭕ se decidira al azar]
$color[#5865F2]
$footer[El retado debe aceptar o rechazar]

$addButton[no;acc~$authorID~$var[oponente];Aceptar;success;no;✅]
$addButton[no;dec~$authorID~$var[oponente];Rechazar;danger;no;❌]

$endif

$catch
$description[❌ Ocurrio un error: $error[message]]
$color[#FF0000]
$endtry
```
