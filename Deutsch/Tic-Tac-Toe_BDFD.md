# Ausloser

```
!ttt
```

## Code

```
$nomention
$disableInnerSpaceRemoval

$var[oponente;$mentioned[1;no]]

$try

$if[$var[oponente]==]

$title[Schwierigkeit wahlen]
$description[Wahle gegen welche KI-Stufe du spielen mochtest:

🟢 **Einfach** — Die KI spielt zufallig
🔵 **Normal** — Greift an und blockt die Grundlagen
🟠 **Schwer** — Spielt mit Strategie
🔴 **Unmoglich** — Real minimax, never loses]
$color[#5865F2]
$footer[Du wirst zufallig ❌ oder ⭕ sein]

$newSelectMenu[ttt-diff;1;1;Wahle die Schwierigkeit...]
$addSelectMenuOption[ttt-diff;Einfach;e~$authorID;Die KI spielt zufallig;no;🟢]
$addSelectMenuOption[ttt-diff;Normal;n~$authorID;Ataca y bloquea;no;🔵]
$addSelectMenuOption[ttt-diff;Schwer;h~$authorID;Spielt mit Strategie;no;🟠]
$addSelectMenuOption[ttt-diff;Unmoglich;i~$authorID;Perfektes Minimax;no;🔴]

$else

$onlyIf[$var[oponente]!=$authorID;❌ Du kannst dich nicht selbst herausfordern!]
$onlyIf[$isBot[$var[oponente]]==false;❌ Du kannst keinen Bot herausfordern!]

$title[Neue Tic-Tac-Toe-Herausforderung!]
$description[<@$authorID> hat herausgefordert <@$var[oponente]> to a Tic-Tac-Toe game.

Wer ❌ und wer ⭕ wird, wird zufallig entschieden]
$color[#5865F2]
$footer[Der Herausgeforderte muss annehmen oder ablehnen]

$addButton[no;acc~$authorID~$var[oponente];Annehmen;success;no;✅]
$addButton[no;dec~$authorID~$var[oponente];Ablehnen;danger;no;❌]

$endif

$catch
$description[❌ Ein Fehler ist aufgetreten: $error[message]]
$color[#FF0000]
$endtry
```
