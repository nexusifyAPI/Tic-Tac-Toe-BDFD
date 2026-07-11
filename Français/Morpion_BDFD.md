# Declencheur

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

$title[Choisir la difficulte]
$description[Choisis contre quel niveau d'IA tu veux jouer:

🟢 **Facile** — L'IA joue au hasard
🔵 **Normal** — Attaque et bloque les bases
🟠 **Difficile** — Joue avec strategie
🔴 **Impossible** — Minimax reel, ne perd jamais]
$color[#5865F2]
$footer[Tu seras ❌ ou ⭕ au hasard]

$newSelectMenu[ttt-diff;1;1;Choisis la difficulte...]
$addSelectMenuOption[ttt-diff;Facile;e~$authorID;L'IA joue au hasard;no;🟢]
$addSelectMenuOption[ttt-diff;Normal;n~$authorID;Ataca y bloquea;no;🔵]
$addSelectMenuOption[ttt-diff;Difficile;h~$authorID;Joue avec strategie;no;🟠]
$addSelectMenuOption[ttt-diff;Impossible;i~$authorID;Minimax parfait;no;🔴]

$else

$onlyIf[$var[oponente]!=$authorID;❌ Tu ne peux pas te defier toi-meme!]
$onlyIf[$isBot[$var[oponente]]==false;❌ Tu ne peux pas defier un bot!]

$title[Nouveau Defi Morpion!]
$description[<@$authorID> a defie <@$var[oponente]> to a Tic-Tac-Toe game.

Qui sera ❌ et qui sera ⭕ sera decide au hasard]
$color[#5865F2]
$footer[Le defie doit accepter ou refuser]

$addButton[no;acc~$authorID~$var[oponente];Accepter;success;no;✅]
$addButton[no;dec~$authorID~$var[oponente];Refuser;danger;no;❌]

$endif

$catch
$description[❌ Une erreur s'est produite: $error[message]]
$color[#FF0000]
$endtry
```
