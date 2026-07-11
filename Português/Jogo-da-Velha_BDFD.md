# Gatilho

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

$title[Selecione a dificuldade]
$description[Escolha contra qual nivel de IA voce quer jogar:

🟢 **Facil** — A IA joga aleatoriamente
🔵 **Normal** — Ataca e bloqueia o basico
🟠 **Dificil** — Joga com estrategia
🔴 **Impossivel** — Real minimax, never loses]
$color[#5865F2]
$footer[Voce sera ❌ ou ⭕ aleatoriamente]

$newSelectMenu[ttt-diff;1;1;Escolha a dificuldade...]
$addSelectMenuOption[ttt-diff;Facil;e~$authorID;A IA joga aleatoriamente;no;🟢]
$addSelectMenuOption[ttt-diff;Normal;n~$authorID;Ataca y bloquea;no;🔵]
$addSelectMenuOption[ttt-diff;Dificil;h~$authorID;Joga com estrategia;no;🟠]
$addSelectMenuOption[ttt-diff;Impossivel;i~$authorID;Minimax perfeito;no;🔴]

$else

$onlyIf[$var[oponente]!=$authorID;❌ Voce nao pode desafiar a si mesmo!]
$onlyIf[$isBot[$var[oponente]]==false;❌ Voce nao pode desafiar um bot!]

$title[Novo Desafio Jogo da Velha!]
$description[<@$authorID> desafiou <@$var[oponente]> to a Tic-Tac-Toe game.

Quem sera ❌ e quem sera ⭕ sera decidido aleatoriamente]
$color[#5865F2]
$footer[O desafiado deve aceitar ou rejeitar]

$addButton[no;acc~$authorID~$var[oponente];Aceitar;success;no;✅]
$addButton[no;dec~$authorID~$var[oponente];Rejeitar;danger;no;❌]

$endif

$catch
$description[❌ Ocorreu um erro: $error[message]]
$color[#FF0000]
$endtry
```
