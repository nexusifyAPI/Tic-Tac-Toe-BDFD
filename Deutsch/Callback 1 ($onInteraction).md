# Ausloser

```
$onInteraction
```

## Code

```
$nomention
$disableInnerSpaceRemoval
$try

$if[$customID==ttt-diff]
$textSplit[$message;~]
$var[dif;$splitText[1]]
$var[menuAuthor;$splitText[2]]
$if[$authorID!=$var[menuAuthor]]
$ephemeral
$removeButtons
$description[❌ Du kannst dieses Menu nicht verwenden, es ist nicht deins!]
$stop
$endif
$defer
$var[diffName;Einfach]
$var[diffColor;#57F287]
$if[$var[dif]==n]
$var[diffName;Normal]
$var[diffColor;#5865F2]
$endif
$if[$var[dif]==h]
$var[diffName;Schwer]
$var[diffColor;#FEE75C]
$endif
$if[$var[dif]==i]
$var[diffName;Unmoglich]
$var[diffColor;#ED4245]
$endif
$title[Spiel gegen KI - Stufe: $var[diffName]]
$var[rnd;$random[0;2]]
$if[$var[rnd]==0]
$description[Spieler: <@$authorID> (❌)
KI: 🤖 (⭕)

Zug: ❌ (Du startest!)]
$color[$var[diffColor]]
$footer[$authorID|AI|$var[dif]|e.e.e.e.e.e.e.e.e|n|n]
$removeAllComponents
$addButton[no;p0;​;secondary;no;⬜]
$addButton[no;p1;​;secondary;no;⬜]
$addButton[no;p2;​;secondary;no;⬜]
$addButton[yes;p3;​;secondary;no;⬜]
$addButton[no;p4;​;secondary;no;⬜]
$addButton[no;p5;​;secondary;no;⬜]
$addButton[yes;p6;​;secondary;no;⬜]
$addButton[no;p7;​;secondary;no;⬜]
$addButton[no;p8;​;secondary;no;⬜]
$addButton[yes;act;🏳️ Aufgeben;danger;no;🏳️]
$else
$description[KI: 🤖 (❌)
Spieler: <@$authorID> (⭕)

Zug: ⭕ (Die KI startet!)]
$color[$var[diffColor]]
$footer[AI|$authorID|$var[dif]|e.e.e.e.X.e.e.e.e|n|n]
$removeAllComponents
$addButton[no;p0;​;secondary;no;⬜]
$addButton[no;p1;​;secondary;no;⬜]
$addButton[no;p2;​;secondary;no;⬜]
$addButton[yes;p3;​;secondary;no;⬜]
$addButton[no;p4;​;primary;yes;❌]
$addButton[no;p5;​;secondary;no;⬜]
$addButton[yes;p6;​;secondary;no;⬜]
$addButton[no;p7;​;secondary;no;⬜]
$addButton[no;p8;​;secondary;no;⬜]
$addButton[yes;act;🏳️ Aufgeben;danger;no;🏳️]
$endif
$stop
$endif

$if[$checkContains[$customID;acc~]==true]
$textSplit[$customID;~]
$var[retador;$splitText[2]]
$var[retado;$splitText[3]]
$if[$authorID!=$var[retado]]
$ephemeral
$removeButtons
$description[❌ ❌ Nur <@$var[retado]> kann diese Herausforderung annehmen!]
$stop
$endif
$defer
$var[rnd;$random[0;2]]
$if[$var[rnd]==0]
$title[PvP-Spiel - Tic-Tac-Toe]
$description[Spieler ❌: <@$var[retador]>
Spieler ⭕: <@$var[retado]>

Zug: ❌ (<@$var[retador]>)]
$color[#5865F2]
$footer[$var[retador]|$var[retado]|-|e.e.e.e.e.e.e.e.e|n|n]
$else
$title[PvP-Spiel - Tic-Tac-Toe]
$description[Spieler ❌: <@$var[retado]>
Spieler ⭕: <@$var[retador]>

Zug: ❌ (<@$var[retado]>)]
$color[#5865F2]
$footer[$var[retado]|$var[retador]|-|e.e.e.e.e.e.e.e.e|n|n]
$endif
$removeAllComponents
$addButton[no;p0;​;secondary;no;⬜]
$addButton[no;p1;​;secondary;no;⬜]
$addButton[no;p2;​;secondary;no;⬜]
$addButton[yes;p3;​;secondary;no;⬜]
$addButton[no;p4;​;secondary;no;⬜]
$addButton[no;p5;​;secondary;no;⬜]
$addButton[yes;p6;​;secondary;no;⬜]
$addButton[no;p7;​;secondary;no;⬜]
$addButton[no;p8;​;secondary;no;⬜]
$addButton[yes;act;🏳️ Aufgeben;danger;no;🏳️]
$stop
$endif

$if[$checkContains[$customID;dec~]==true]
$textSplit[$customID;~]
$var[retador;$splitText[2]]
$var[retado;$splitText[3]]
$if[$authorID!=$var[retado]]
$ephemeral
$removeButtons
$description[❌ ❌ Nur <@$var[retado]> kann diese Herausforderung ablehnen!]
$stop
$endif
$defer
$title[Herausforderung abgelehnt]
$description[❌ <@$var[retado]> hat abgelehnt <@$var[retador]>.]
$color[#ED4245]
$removeAllComponents
$stop
$endif

$if[$customID==act]
$var[mid;$messageID]
$var[state;$getEmbedData[$channelID;$var[mid];1;footer]]

$if[$var[state]==]
$ephemeral
$removeButtons
$description[❌ Spielstatus konnte nicht geladen werden.]
$stop
$endif

$textSplit[$var[state];|]
$var[pX;$splitText[1]]
$var[pO;$splitText[2]]
$var[dif;$splitText[3]]
$var[cells;$splitText[4]]
$var[revX;$splitText[5]]
$var[revO;$splitText[6]]

$textSplit[$var[cells];.]
$var[c0;$splitText[1]]
$var[c1;$splitText[2]]
$var[c2;$splitText[3]]
$var[c3;$splitText[4]]
$var[c4;$splitText[5]]
$var[c5;$splitText[6]]
$var[c6;$splitText[7]]
$var[c7;$splitText[8]]
$var[c8;$splitText[9]]

$var[mode;pvp]
$if[$var[pX]==AI]
$var[mode;ia]
$endif
$if[$var[pO]==AI]
$var[mode;ia]
$endif

$var[filled;0]
$if[$var[c0]!=e]$var[filled;$sum[$var[filled];1]]$endif
$if[$var[c1]!=e]$var[filled;$sum[$var[filled];1]]$endif
$if[$var[c2]!=e]$var[filled;$sum[$var[filled];1]]$endif
$if[$var[c3]!=e]$var[filled;$sum[$var[filled];1]]$endif
$if[$var[c4]!=e]$var[filled;$sum[$var[filled];1]]$endif
$if[$var[c5]!=e]$var[filled;$sum[$var[filled];1]]$endif
$if[$var[c6]!=e]$var[filled;$sum[$var[filled];1]]$endif
$if[$var[c7]!=e]$var[filled;$sum[$var[filled];1]]$endif
$if[$var[c8]!=e]$var[filled;$sum[$var[filled];1]]$endif

$var[turn;X]
$if[$modulo[$var[filled];2]==1]
$var[turn;O]
$endif

$var[gameEnded;no]
$if[$var[c0]!=e]
$if[$var[c0]==$var[c1]]
$if[$var[c1]==$var[c2]]
$var[gameEnded;yes]
$endif
$endif
$endif
$if[$var[c3]!=e]
$if[$var[c3]==$var[c4]]
$if[$var[c4]==$var[c5]]
$var[gameEnded;yes]
$endif
$endif
$endif
$if[$var[c6]!=e]
$if[$var[c6]==$var[c7]]
$if[$var[c7]==$var[c8]]
$var[gameEnded;yes]
$endif
$endif
$endif
$if[$var[c0]!=e]
$if[$var[c0]==$var[c3]]
$if[$var[c3]==$var[c6]]
$var[gameEnded;yes]
$endif
$endif
$endif
$if[$var[c1]!=e]
$if[$var[c1]==$var[c4]]
$if[$var[c4]==$var[c7]]
$var[gameEnded;yes]
$endif
$endif
$endif
$if[$var[c2]!=e]
$if[$var[c2]==$var[c5]]
$if[$var[c5]==$var[c8]]
$var[gameEnded;yes]
$endif
$endif
$endif
$if[$var[c0]!=e]
$if[$var[c0]==$var[c4]]
$if[$var[c4]==$var[c8]]
$var[gameEnded;yes]
$endif
$endif
$endif
$if[$var[c2]!=e]
$if[$var[c2]==$var[c4]]
$if[$var[c4]==$var[c6]]
$var[gameEnded;yes]
$endif
$endif
$endif
$if[$var[filled]==9]
$var[gameEnded;yes]
$endif

$if[$var[gameEnded]==yes]

$if[$var[mode]==ia]
$if[$var[pX]==AI]
$if[$authorID!=$var[pO]]
$ephemeral
$removeButtons
$description[❌ Nur der Spieler kann neu starten!]
$stop
$endif
$else
$if[$authorID!=$var[pX]]
$ephemeral
$removeButtons
$description[❌ Nur der Spieler kann neu starten!]
$stop
$endif
$endif
$defer

$var[diffName;Einfach]
$var[diffColor;#57F287]
$if[$var[dif]==n]
$var[diffName;Normal]
$var[diffColor;#5865F2]
$endif
$if[$var[dif]==h]
$var[diffName;Schwer]
$var[diffColor;#FEE75C]
$endif
$if[$var[dif]==i]
$var[diffName;Unmoglich]
$var[diffColor;#ED4245]
$endif
$title[Spiel gegen KI - Stufe: $var[diffName]]
$var[rnd2;$random[0;2]]
$if[$var[rnd2]==0]
$description[Spieler: <@$authorID> (❌)
KI: 🤖 (⭕)

🔄 Neues Spiel gestartet!

Zug: ❌ (Du startest!)]
$color[$var[diffColor]]
$footer[$authorID|AI|$var[dif]|e.e.e.e.e.e.e.e.e|n|n]
$removeAllComponents
$addButton[no;p0;​;secondary;no;⬜]
$addButton[no;p1;​;secondary;no;⬜]
$addButton[no;p2;​;secondary;no;⬜]
$addButton[yes;p3;​;secondary;no;⬜]
$addButton[no;p4;​;secondary;no;⬜]
$addButton[no;p5;​;secondary;no;⬜]
$addButton[yes;p6;​;secondary;no;⬜]
$addButton[no;p7;​;secondary;no;⬜]
$addButton[no;p8;​;secondary;no;⬜]
$addButton[yes;act;🏳️ Aufgeben;danger;no;🏳️]
$else
$description[KI: 🤖 (❌)
Spieler: <@$authorID> (⭕)

🔄 Neues Spiel gestartet!

Zug: ⭕ (Die KI startet!)]
$color[$var[diffColor]]
$footer[AI|$authorID|$var[dif]|e.e.e.e.X.e.e.e.e|n|n]
$removeAllComponents
$addButton[no;p0;​;secondary;no;⬜]
$addButton[no;p1;​;secondary;no;⬜]
$addButton[no;p2;​;secondary;no;⬜]
$addButton[yes;p3;​;secondary;no;⬜]
$addButton[no;p4;​;primary;yes;❌]
$addButton[no;p5;​;secondary;no;⬜]
$addButton[yes;p6;​;secondary;no;⬜]
$addButton[no;p7;​;secondary;no;⬜]
$addButton[no;p8;​;secondary;no;⬜]
$addButton[yes;act;🏳️ Aufgeben;danger;no;🏳️]
$endif
$stop
$endif

$if[$authorID!=$var[pX]]
$if[$authorID!=$var[pO]]
$ephemeral
$removeButtons
$description[❌ Nur die Spieler dieses Spiels konnen ein Rematch anfordern!]
$stop
$endif
$endif

$if[$authorID==$var[pX]]
$if[$var[revX]==y]
$ephemeral
$removeButtons
$description[❌ Du hast bereits fur ein Rematch abgestimmt!]
$stop
$endif
$var[revX;y]
$endif

$if[$authorID==$var[pO]]
$if[$var[revO]==y]
$ephemeral
$removeButtons
$description[❌ Du hast bereits fur ein Rematch abgestimmt!]
$stop
$endif
$var[revO;y]
$endif

$var[votos;0]
$if[$var[revX]==y]$var[votos;$sum[$var[votos];1]]$endif
$if[$var[revO]==y]$var[votos;$sum[$var[votos];1]]$endif

$if[$var[votos]<2]
$defer
$editButton[act;🔄 Rematch ($var[votos]/2);success;no;🔄]
$title[PvP-Spiel - Rematch]
$description[Spieler ❌: <@$var[pX]>
Spieler ⭕: <@$var[pO]>

🔄 Rematch angefordert: **$var[votos]/2**

Beide Spieler mussen drucken zum Neustarten.]
$color[#FEE75C]
$footer[$var[pX]|$var[pO]|-|$var[c0].$var[c1].$var[c2].$var[c3].$var[c4].$var[c5].$var[c6].$var[c7].$var[c8]|$var[revX]|$var[revO]]
$stop
$endif

$defer

$var[rnd3;$random[0;2]]
$if[$var[rnd3]==0]
$title[PvP-Spiel - Tic-Tac-Toe]
$description[Spieler ❌: <@$var[pX]>
Spieler ⭕: <@$var[pO]>

🔄 Rematch gestartet!

Zug: ❌ (<@$var[pX]>)]
$color[#5865F2]
$footer[$var[pX]|$var[pO]|-|e.e.e.e.e.e.e.e.e|n|n]
$else
$title[PvP-Spiel - Tic-Tac-Toe]
$description[Spieler ❌: <@$var[pO]>
Spieler ⭕: <@$var[pX]>

🔄 Rematch gestartet!

Zug: ❌ (<@$var[pO]>)]
$color[#5865F2]
$footer[$var[pO]|$var[pX]|-|e.e.e.e.e.e.e.e.e|n|n]
$endif
$removeAllComponents
$addButton[no;p0;​;secondary;no;⬜]
$addButton[no;p1;​;secondary;no;⬜]
$addButton[no;p2;​;secondary;no;⬜]
$addButton[yes;p3;​;secondary;no;⬜]
$addButton[no;p4;​;secondary;no;⬜]
$addButton[no;p5;​;secondary;no;⬜]
$addButton[yes;p6;​;secondary;no;⬜]
$addButton[no;p7;​;secondary;no;⬜]
$addButton[no;p8;​;secondary;no;⬜]
$addButton[yes;act;🏳️ Aufgeben;danger;no;🏳️]
$stop
$endif

$if[$var[mode]==ia]
$if[$var[pX]==AI]
$if[$authorID!=$var[pO]]
$ephemeral
$removeButtons
$description[❌ Nur der Spieler kann aufgeben!]
$stop
$endif
$else
$if[$authorID!=$var[pX]]
$ephemeral
$removeButtons
$description[❌ Nur der Spieler kann aufgeben!]
$stop
$endif
$endif
$else
$var[allowed;$var[pX]]
$if[$var[turn]==O]
$var[allowed;$var[pO]]
$endif
$if[$authorID!=$var[allowed]]
$ephemeral
$removeButtons
$description[❌ Du bist nicht dran, du kannst nicht aufgeben!]
$stop
$endif
$endif

$defer

$var[gameOver;true]
$var[winLine;-1]

$if[$var[mode]==ia]
$title[Spiel gegen KI - Aufgabe]
$if[$var[pX]==AI]
$description[KI: 🤖 (❌)
Player: <@$var[pO]> (⭕)

🏳️ <@$var[pO]> hat aufgegeben. Die KI gewinnt!]
$else
$description[Player: <@$var[pX]> (❌)
KI: 🤖 (⭕)

🏳️ <@$var[pX]> hat aufgegeben. Die KI gewinnt!]
$endif
$color[#ED4245]
$else
$var[winner;$var[pX]]
$var[winnerSym;X]
$if[$var[turn]==X]
$var[winner;$var[pO]]
$var[winnerSym;O]
$endif
$title[PvP-Spiel - Aufgabe]
$description[Spieler ❌: <@$var[pX]>
Spieler ⭕: <@$var[pO]>

🏳️ <@$authorID> hat aufgegeben. $var[winnerSym] <@$var[winner]> gewinnt!]
$color[#57F287]
$endif

$if[$var[c0]==e]$var[c0;d]$endif
$if[$var[c1]==e]$var[c1;d]$endif
$if[$var[c2]==e]$var[c2;d]$endif
$if[$var[c3]==e]$var[c3;d]$endif
$if[$var[c4]==e]$var[c4;d]$endif
$if[$var[c5]==e]$var[c5;d]$endif
$if[$var[c6]==e]$var[c6;d]$endif
$if[$var[c7]==e]$var[c7;d]$endif
$if[$var[c8]==e]$var[c8;d]$endif

$var[nc;$var[c0].$var[c1].$var[c2].$var[c3].$var[c4].$var[c5].$var[c6].$var[c7].$var[c8]]
$footer[$var[pX]|$var[pO]|$var[dif]|$var[nc]|$var[revX]|$var[revO]]

$removeAllComponents
$addButton[no;p0;​;secondary;yes;⬜]
$addButton[no;p1;​;secondary;yes;⬜]
$addButton[no;p2;​;secondary;yes;⬜]
$addButton[yes;p3;​;secondary;yes;⬜]
$addButton[no;p4;​;secondary;yes;⬜]
$addButton[no;p5;​;secondary;yes;⬜]
$addButton[yes;p6;​;secondary;yes;⬜]
$addButton[no;p7;​;secondary;yes;⬜]
$addButton[no;p8;​;secondary;yes;⬜]
$if[$var[mode]==ia]
$addButton[yes;act;🔄 Neustart;success;no;🔄]
$else
$addButton[yes;act;🔄 Rematch (0/2);success;no;🔄]
$endif
$stop
$endif

$if[$checkContains[$customID;p0;p1;p2;p3;p4;p5;p6;p7;p8]==false]
$stop
$endif

$catch
$ephemeral
$removeButtons
$description[❌ Ein Fehler ist aufgetreten: $error[message]]
$color[#FF0000]
$endtry
```