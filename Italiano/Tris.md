# Attivatore

```
!ttt
```

## Codice

```
$nomention
$disableInnerSpaceRemoval

$var[oponente;$mentioned[1;no]]

$try

$if[$var[oponente]==]

$title[Seleziona difficolta]
$description[Scegli contro quale livello di IA vuoi giocare:

🟢 **Facile** — L'IA gioca casualmente
🔵 **Normale** — Attacca e blocca le basi
🟠 **Difficile** — Gioca con strategia
🔴 **Impossibile** — Real minimax, never loses]
$color[#5865F2]
$footer[Sarai ❌ o ⭕ casualmente]

$newSelectMenu[ttt-diff;1;1;Scegli la difficolta...]
$addSelectMenuOption[ttt-diff;Facile;e~$authorID;L'IA gioca casualmente;no;🟢]
$addSelectMenuOption[ttt-diff;Normal;n~$authorID;Ataca y bloquea;no;🔵]
$addSelectMenuOption[ttt-diff;Difficile;h~$authorID;Gioca con strategia;no;🟠]
$addSelectMenuOption[ttt-diff;Impossibile;i~$authorID;Minimax perfetto;no;🔴]

$else

$onlyIf[$var[oponente]!=$authorID;❌ Non puoi sfidare te stesso!]
$onlyIf[$isBot[$var[oponente]]==false;❌ Non puoi sfidare un bot!]

$title[Nuova Sfida Tris!]
$description[<@$authorID> ha sfidato <@$var[oponente]> to a Tic-Tac-Toe game.

Chi sara ❌ e chi sara ⭕ sara deciso casualmente]
$color[#5865F2]
$footer[Lo sfidato deve accettare o rifiutare]

$addButton[no;acc~$authorID~$var[oponente];Accetta;success;no;✅]
$addButton[no;dec~$authorID~$var[oponente];Rifiuta;danger;no;❌]

$endif

$catch
$description[❌ Si e verificato un errore: $error[message]]
$color[#FF0000]
$endtry
```
