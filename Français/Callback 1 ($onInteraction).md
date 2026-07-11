# Declencheur

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
$description[❌ Tu ne peux pas utiliser ce menu, il n'est pas a toi!]
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
$var[diffName;Impossible]
$var[diffColor;#ED4245]
$endif
$title[Partie vs IA - Niveau: $var[diffName]]
$var[rnd;$random[0;2]]
$if[$var[rnd]==0]
$description[Joueur: <@$authorID> (❌)
IA: 🤖 (⭕)

Tour: ❌ (Tu commences!)]
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
$addButton[yes;act;🏳️ Abandonner;danger;no;🏳️]
$else
$description[IA: 🤖 (❌)
Joueur: <@$authorID> (⭕)

Tour: ⭕ (L'IA commence!)]
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
$addButton[yes;act;🏳️ Abandonner;danger;no;🏳️]
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
$description[❌ ❌ Seul <@$var[retado]> peut accepter ce defi!]
$stop
$endif
$defer
$var[rnd;$random[0;2]]
$if[$var[rnd]==0]
$title[Partie PvP - Morpion]
$description[Joueur ❌: <@$var[retador]>
Joueur ⭕: <@$var[retado]>

Tour: ❌ (<@$var[retador]>)]
$color[#5865F2]
$footer[$var[retador]|$var[retado]|-|e.e.e.e.e.e.e.e.e|n|n]
$else
$title[Partie PvP - Morpion]
$description[Joueur ❌: <@$var[retado]>
Joueur ⭕: <@$var[retador]>

Tour: ❌ (<@$var[retado]>)]
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
$addButton[yes;act;🏳️ Abandonner;danger;no;🏳️]
$stop
$endif

$if[$checkContains[$customID;dec~]==true]
$textSplit[$customID;~]
$var[retador;$splitText[2]]
$var[retado;$splitText[3]]
$if[$authorID!=$var[retado]]
$ephemeral
$removeButtons
$description[❌ ❌ Seul <@$var[retado]> peut refuser ce defi!]
$stop
$endif
$defer
$title[Defi refuse]
$description[❌ <@$var[retado]> a refuse <@$var[retador]>.]
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
$description[❌ Impossible de charger l'etat de la partie.]
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
$description[❌ Seul le joueur peut redemarrer!]
$stop
$endif
$else
$if[$authorID!=$var[pX]]
$ephemeral
$removeButtons
$description[❌ Seul le joueur peut redemarrer!]
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
$var[diffName;Impossible]
$var[diffColor;#ED4245]
$endif
$title[Partie vs IA - Niveau: $var[diffName]]
$var[rnd2;$random[0;2]]
$if[$var[rnd2]==0]
$description[Joueur: <@$authorID> (❌)
IA: 🤖 (⭕)

🔄 Nouvelle partie lancee!

Tour: ❌ (Tu commences!)]
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
$addButton[yes;act;🏳️ Abandonner;danger;no;🏳️]
$else
$description[IA: 🤖 (❌)
Joueur: <@$authorID> (⭕)

🔄 Nouvelle partie lancee!

Tour: ⭕ (L'IA commence!)]
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
$addButton[yes;act;🏳️ Abandonner;danger;no;🏳️]
$endif
$stop
$endif

$if[$authorID!=$var[pX]]
$if[$authorID!=$var[pO]]
$ephemeral
$removeButtons
$description[❌ Seuls les joueurs de cette partie peuvent demander une revanche!]
$stop
$endif
$endif

$if[$authorID==$var[pX]]
$if[$var[revX]==y]
$ephemeral
$removeButtons
$description[❌ Tu as deja vote pour une revanche!]
$stop
$endif
$var[revX;y]
$endif

$if[$authorID==$var[pO]]
$if[$var[revO]==y]
$ephemeral
$removeButtons
$description[❌ Tu as deja vote pour une revanche!]
$stop
$endif
$var[revO;y]
$endif

$var[votos;0]
$if[$var[revX]==y]$var[votos;$sum[$var[votos];1]]$endif
$if[$var[revO]==y]$var[votos;$sum[$var[votos];1]]$endif

$if[$var[votos]<2]
$defer
$editButton[act;🔄 Revanche ($var[votos]/2);success;no;🔄]
$title[Partie PvP - Revanche]
$description[Joueur ❌: <@$var[pX]>
Joueur ⭕: <@$var[pO]>

🔄 Revanche demandee: **$var[votos]/2**

Les deux joueurs doivent appuyer pour redemarrer.]
$color[#FEE75C]
$footer[$var[pX]|$var[pO]|-|$var[c0].$var[c1].$var[c2].$var[c3].$var[c4].$var[c5].$var[c6].$var[c7].$var[c8]|$var[revX]|$var[revO]]
$stop
$endif

$defer

$var[rnd3;$random[0;2]]
$if[$var[rnd3]==0]
$title[Partie PvP - Morpion]
$description[Joueur ❌: <@$var[pX]>
Joueur ⭕: <@$var[pO]>

🔄 Revanche lancee!

Tour: ❌ (<@$var[pX]>)]
$color[#5865F2]
$footer[$var[pX]|$var[pO]|-|e.e.e.e.e.e.e.e.e|n|n]
$else
$title[Partie PvP - Morpion]
$description[Joueur ❌: <@$var[pO]>
Joueur ⭕: <@$var[pX]>

🔄 Revanche lancee!

Tour: ❌ (<@$var[pO]>)]
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
$addButton[yes;act;🏳️ Abandonner;danger;no;🏳️]
$stop
$endif

$if[$var[mode]==ia]
$if[$var[pX]==AI]
$if[$authorID!=$var[pO]]
$ephemeral
$removeButtons
$description[❌ Seul le joueur peut abandonner!]
$stop
$endif
$else
$if[$authorID!=$var[pX]]
$ephemeral
$removeButtons
$description[❌ Seul le joueur peut abandonner!]
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
$description[❌ Ce n'est pas ton tour, tu ne peux pas abandonner!]
$stop
$endif
$endif

$defer

$var[gameOver;true]
$var[winLine;-1]

$if[$var[mode]==ia]
$title[Partie vs IA - Abandon]
$if[$var[pX]==AI]
$description[IA: 🤖 (❌)
Player: <@$var[pO]> (⭕)

🏳️ <@$var[pO]> a abandonne. L'IA gagne!]
$else
$description[Player: <@$var[pX]> (❌)
IA: 🤖 (⭕)

🏳️ <@$var[pX]> a abandonne. L'IA gagne!]
$endif
$color[#ED4245]
$else
$var[winner;$var[pX]]
$var[winnerSym;X]
$if[$var[turn]==X]
$var[winner;$var[pO]]
$var[winnerSym;O]
$endif
$title[Partie PvP - Abandon]
$description[Joueur ❌: <@$var[pX]>
Joueur ⭕: <@$var[pO]>

🏳️ <@$authorID> a abandonne. $var[winnerSym] <@$var[winner]> gagne!]
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
$addButton[yes;act;🔄 Redemarrer;success;no;🔄]
$else
$addButton[yes;act;🔄 Revanche (0/2);success;no;🔄]
$endif
$stop
$endif

$if[$checkContains[$customID;p0;p1;p2;p3;p4;p5;p6;p7;p8]==false]
$stop
$endif

$catch
$ephemeral
$removeButtons
$description[❌ Une erreur s'est produite: $error[message]]
$color[#FF0000]
$endtry
```