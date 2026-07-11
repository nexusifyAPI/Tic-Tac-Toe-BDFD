# Attivatore

```
$onInteraction
```

## Codice

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
$description[❌ Non puoi usare questo menu, non e tuo!]
$stop
$endif
$defer
$var[diffName;Facile]
$var[diffColor;#57F287]
$if[$var[dif]==n]
$var[diffName;Normal]
$var[diffColor;#5865F2]
$endif
$if[$var[dif]==h]
$var[diffName;Difficile]
$var[diffColor;#FEE75C]
$endif
$if[$var[dif]==i]
$var[diffName;Impossibile]
$var[diffColor;#ED4245]
$endif
$title[Partita vs IA - Livello: $var[diffName]]
$var[rnd;$random[0;2]]
$if[$var[rnd]==0]
$description[Giocatore: <@$authorID> (❌)
IA: 🤖 (⭕)

Turno: ❌ (Inizi tu!)]
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
$addButton[yes;act;🏳️ Arrenditi;danger;no;🏳️]
$else
$description[IA: 🤖 (❌)
Giocatore: <@$authorID> (⭕)

Turno: ⭕ (L'IA inizia!)]
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
$addButton[yes;act;🏳️ Arrenditi;danger;no;🏳️]
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
$description[❌ ❌ Solo <@$var[retado]> puo accettare questa sfida!]
$stop
$endif
$defer
$var[rnd;$random[0;2]]
$if[$var[rnd]==0]
$title[Partita PvP - Tris]
$description[Giocatore ❌: <@$var[retador]>
Giocatore ⭕: <@$var[retado]>

Turno: ❌ (<@$var[retador]>)]
$color[#5865F2]
$footer[$var[retador]|$var[retado]|-|e.e.e.e.e.e.e.e.e|n|n]
$else
$title[Partita PvP - Tris]
$description[Giocatore ❌: <@$var[retado]>
Giocatore ⭕: <@$var[retador]>

Turno: ❌ (<@$var[retado]>)]
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
$addButton[yes;act;🏳️ Arrenditi;danger;no;🏳️]
$stop
$endif

$if[$checkContains[$customID;dec~]==true]
$textSplit[$customID;~]
$var[retador;$splitText[2]]
$var[retado;$splitText[3]]
$if[$authorID!=$var[retado]]
$ephemeral
$removeButtons
$description[❌ ❌ Solo <@$var[retado]> puo rifiutare questa sfida!]
$stop
$endif
$defer
$title[Sfida rifiutata]
$description[❌ <@$var[retado]> ha rifiutato <@$var[retador]>.]
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
$description[❌ Impossibile caricare lo stato della partita.]
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
$description[❌ Solo il giocatore puo riavviare!]
$stop
$endif
$else
$if[$authorID!=$var[pX]]
$ephemeral
$removeButtons
$description[❌ Solo il giocatore puo riavviare!]
$stop
$endif
$endif
$defer

$var[diffName;Facile]
$var[diffColor;#57F287]
$if[$var[dif]==n]
$var[diffName;Normal]
$var[diffColor;#5865F2]
$endif
$if[$var[dif]==h]
$var[diffName;Difficile]
$var[diffColor;#FEE75C]
$endif
$if[$var[dif]==i]
$var[diffName;Impossibile]
$var[diffColor;#ED4245]
$endif
$title[Partita vs IA - Livello: $var[diffName]]
$var[rnd2;$random[0;2]]
$if[$var[rnd2]==0]
$description[Giocatore: <@$authorID> (❌)
IA: 🤖 (⭕)

🔄 Nuova partita iniziata!

Turno: ❌ (Inizi tu!)]
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
$addButton[yes;act;🏳️ Arrenditi;danger;no;🏳️]
$else
$description[IA: 🤖 (❌)
Giocatore: <@$authorID> (⭕)

🔄 Nuova partita iniziata!

Turno: ⭕ (L'IA inizia!)]
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
$addButton[yes;act;🏳️ Arrenditi;danger;no;🏳️]
$endif
$stop
$endif

$if[$authorID!=$var[pX]]
$if[$authorID!=$var[pO]]
$ephemeral
$removeButtons
$description[❌ Solo i giocatori di questa partita possono richiedere la rivincita!]
$stop
$endif
$endif

$if[$authorID==$var[pX]]
$if[$var[revX]==y]
$ephemeral
$removeButtons
$description[❌ Hai gia votato per la rivincita!]
$stop
$endif
$var[revX;y]
$endif

$if[$authorID==$var[pO]]
$if[$var[revO]==y]
$ephemeral
$removeButtons
$description[❌ Hai gia votato per la rivincita!]
$stop
$endif
$var[revO;y]
$endif

$var[votos;0]
$if[$var[revX]==y]$var[votos;$sum[$var[votos];1]]$endif
$if[$var[revO]==y]$var[votos;$sum[$var[votos];1]]$endif

$if[$var[votos]<2]
$defer
$editButton[act;🔄 Rivincita ($var[votos]/2);success;no;🔄]
$title[Partita PvP - Rivincita]
$description[Giocatore ❌: <@$var[pX]>
Giocatore ⭕: <@$var[pO]>

🔄 Rivincita richiesta: **$var[votos]/2**

Entrambi i giocatori devono premere per riavviare.]
$color[#FEE75C]
$footer[$var[pX]|$var[pO]|-|$var[c0].$var[c1].$var[c2].$var[c3].$var[c4].$var[c5].$var[c6].$var[c7].$var[c8]|$var[revX]|$var[revO]]
$stop
$endif

$defer

$var[rnd3;$random[0;2]]
$if[$var[rnd3]==0]
$title[Partita PvP - Tris]
$description[Giocatore ❌: <@$var[pX]>
Giocatore ⭕: <@$var[pO]>

🔄 Rivincita iniziata!

Turno: ❌ (<@$var[pX]>)]
$color[#5865F2]
$footer[$var[pX]|$var[pO]|-|e.e.e.e.e.e.e.e.e|n|n]
$else
$title[Partita PvP - Tris]
$description[Giocatore ❌: <@$var[pO]>
Giocatore ⭕: <@$var[pX]>

🔄 Rivincita iniziata!

Turno: ❌ (<@$var[pO]>)]
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
$addButton[yes;act;🏳️ Arrenditi;danger;no;🏳️]
$stop
$endif

$if[$var[mode]==ia]
$if[$var[pX]==AI]
$if[$authorID!=$var[pO]]
$ephemeral
$removeButtons
$description[❌ Solo il giocatore puo arrendersi!]
$stop
$endif
$else
$if[$authorID!=$var[pX]]
$ephemeral
$removeButtons
$description[❌ Solo il giocatore puo arrendersi!]
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
$description[❌ Non e il tuo turno, non puoi arrenderti!]
$stop
$endif
$endif

$defer

$var[gameOver;true]
$var[winLine;-1]

$if[$var[mode]==ia]
$title[Partita vs IA - Resa]
$if[$var[pX]==AI]
$description[IA: 🤖 (❌)
Player: <@$var[pO]> (⭕)

🏳️ <@$var[pO]> si e arreso. L'IA vince!]
$else
$description[Player: <@$var[pX]> (❌)
IA: 🤖 (⭕)

🏳️ <@$var[pX]> si e arreso. L'IA vince!]
$endif
$color[#ED4245]
$else
$var[winner;$var[pX]]
$var[winnerSym;X]
$if[$var[turn]==X]
$var[winner;$var[pO]]
$var[winnerSym;O]
$endif
$title[Partita PvP - Resa]
$description[Giocatore ❌: <@$var[pX]>
Giocatore ⭕: <@$var[pO]>

🏳️ <@$authorID> si e arreso. $var[winnerSym] <@$var[winner]> vince!]
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
$addButton[yes;act;🔄 Riavvia;success;no;🔄]
$else
$addButton[yes;act;🔄 Rivincita (0/2);success;no;🔄]
$endif
$stop
$endif

$if[$checkContains[$customID;p0;p1;p2;p3;p4;p5;p6;p7;p8]==false]
$stop
$endif

$catch
$ephemeral
$removeButtons
$description[❌ Si e verificato un errore: $error[message]]
$color[#FF0000]
$endtry
```