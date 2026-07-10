# Trigger

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

$title[Select difficulty]
$description[Choose which AI level you want to play against:

🟢 **Easy** — AI plays randomly
🔵 **Normal** — Attacks and blocks the basics
🟠 **Hard** — Plays with strategy
🔴 **Impossible** — Real minimax, never loses]
$color[#5865F2]
$footer[You'll be ❌ or ⭕ at random]

$newSelectMenu[ttt-diff;1;1;Choose the difficulty...]
$addSelectMenuOption[ttt-diff;Easy;e~$authorID;AI plays randomly;no;🟢]
$addSelectMenuOption[ttt-diff;Normal;n~$authorID;Ataca y bloquea;no;🔵]
$addSelectMenuOption[ttt-diff;Hard;h~$authorID;Plays with strategy;no;🟠]
$addSelectMenuOption[ttt-diff;Impossible;i~$authorID;Perfect minimax;no;🔴]

$else

$onlyIf[$var[oponente]!=$authorID;❌ You can't challenge yourself!]
$onlyIf[$isBot[$var[oponente]]==false;❌ You can't challenge a bot!]

$title[New Tic-Tac-Toe Challenge!]
$description[<@$authorID> has challenged <@$var[oponente]> to a Tic-Tac-Toe game.

Who will be ❌ and who will be ⭕ will be decided randomly]
$color[#5865F2]
$footer[The challenged must accept or reject]

$addButton[no;acc~$authorID~$var[oponente];Accept;success;no;✅]
$addButton[no;dec~$authorID~$var[oponente];Reject;danger;no;❌]

$endif

$catch
$description[❌ An error occurred: $error[message]]
$color[#FF0000]
$endtry
```
