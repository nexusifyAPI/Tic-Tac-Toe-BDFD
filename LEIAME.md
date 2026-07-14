# Jogo da Velha para BDFD

Um jogo de Jogo da Velha para Bot Designer For Discord (BDFD), com modo PvP e modo contra IA em 4 dificuldades. A dificuldade Impossível usa minimax com uma tabela pré-computada de todos os 19.683 estados do tabuleiro, então nunca perde.

## O que inclui

- 4 dificuldades de IA: Fácil, Normal, Difícil, Impossível (minimax)
- Modo PvP com botões de aceitar/rejeitar para desafios
- Atribuição aleatória de X/O a cada partida
- Sistema de revanche: os dois jogadores precisam votar para reiniciar
- Render-se e reiniciar no modo IA
- A linha vencedora é destacada em verde
- Contador de jogadas (X/9)
- Não precisa de variáveis de servidor, o estado do jogo vive no rodapé do embed
- Partidas simultâneas ilimitadas, cada tabuleiro é independente

## Configuração

Você precisa de 3 comandos no seu bot:

| # | Gatilho | Linguagem | Arquivo | O que faz |
|---|---------|----------|---------|-----------|
| 1 | `!ttt` | BDScript 2 | `Jogo-da-Velha_BDFD.md` | Comando principal (menu de IA ou desafio PvP) |
| 2 | `$onInteraction` | BDScript 2 | `Callback 1 ($onInteraction).md` | Menu, desafios, render-se, revanche, reiniciar |
| 3 | `$onInteraction` | BDScript 2 | `Callback 2 ($onInteraction).md` | Jogadas + IA minimax |

**Por que dois callbacks `$onInteraction`?** O BDFD permite ter mais de um, e roda todos eles a cada interação. A tabela minimax tem 19.683 caracteres e não cabe junto com o resto do código em um único callback, então foi dividida em dois: o primeiro cuida do menu de dificuldade, aceitar/rejeitar e render-se/revanche/reiniciar; o segundo cuida dos cliques nas células e da tabela minimax. Cada um usa `$stop` para não interferir com o outro.

Permissões que o bot precisa: enviar mensagens, gerenciar mensagens (para editar o tabuleiro) e usar componentes (botões/menus de seleção).

## Como funciona

### Estado do jogo

Guardado no rodapé do embed, com este formato:

```
ID_jogadorX|ID_jogadorO|dificuldade|celula0.celula1.celula2.celula3.celula4.celula5.celula6.celula7.celula8|votoRevancheX|votoRevancheO
```

- `ID_jogadorX` / `ID_jogadorO`: ID de usuário do Discord (ou `IA`)
- `dificuldade`: `e` Fácil, `n` Normal, `h` Difícil, `i` Impossível
- `celula0`–`celula8`: `e` vazia, `X` ou `O`
- `votoRevancheX` / `votoRevancheO`: `y` ou `n`

Como não depende de variáveis de servidor, cada partida é autônoma e você pode ter várias rodando ao mesmo tempo no mesmo canal.

### Dificuldades da IA

| Dificuldade | Estratégia |
|-----------|----------|
| Fácil | Jogadas aleatórias |
| Normal | Ganha se pode, bloqueia ameaça, senão aleatório |
| Difícil | Ganhar, bloquear, criar forks, bloquear forks do adversário, centro, canto oposto, canto, lado |
| Impossível | Minimax com tabela pré-computada para os 19.683 estados. Nunca perde. |

### O algoritmo minimax

Para a dificuldade Impossível:

1. `minimax_solver.py` roda um minimax exaustivo sobre os 3⁹ = 19.683 estados possíveis do tabuleiro, e para cada um em que é a vez da IA calcula a jogada ótima assumindo que o adversário joga perfeitamente.
2. Isso é guardado como uma tabela de 19.683 caracteres: cada posição corresponde a um estado (indexado por um hash em base 3), e o caractere lá é a jogada ótima (0-8), ou `-` se aquele estado não for válido.
3. No BDFD: as células são mapeadas para números (`e=0`, `iaSym=1`, `userSym=2`), o hash é calculado como `hash = n0*1 + n1*3 + n2*9 + ... + n8*6561`, consultado com `$splitText[$sum[$var[hash];1]]`, e essa jogada é feita.

### Atribuição aleatória de X/O

No início de uma partida (nova, reinício ou revanche), `$random[0;2]` decide quem é X: metade das vezes você começa, metade a IA começa (e joga o centro automaticamente). A IA é parametrizada com `$var[iaSym]` e `$var[userSym]`, então funciona igual seja X ou O.

### Revanche (PvP)

Quando uma partida PvP termina, o botão muda para "Revanche (0/2)". Cada jogador vota apertando ele, e quando chega a 2/2 o tabuleiro reinicia com uma nova atribuição aleatória de X/O. Só os dois jogadores originais podem votar; se outro tentar, recebe um erro.

## Estrutura do repo

```
Repository/
│
├── README.md                          (versão em inglês)
├── LEIAME.md                          (este arquivo)
│
├── English/
│   ├── Tic-Tac-Toe_Code.md
│   ├── Callback 1 ($onInteraction).md
│   └── Callback 2 ($onInteraction).md
│
└── Português/
    ├── Jogo-da-Velha_BDFD.md
    ├── Callback 1 ($onInteraction).md
    └── Callback 2 ($onInteraction).md
```

## Uso

Contra a IA: digite `!ttt` e escolha uma dificuldade no menu suspenso. Você será designado X ou O aleatoriamente.

Contra outro jogador: `!ttt @usuário`. O jogador desafiado aceita com o botão, e X/O é atribuído aleatoriamente.

Durante a partida: clique numa célula vazia (⬜) para jogar, "Render-se" para desistir, e quando acabar "Reiniciar" (vs IA) ou "Revanche" (PvP) para jogar de novo.

## Detalhes técnicos

- Linguagem: BDScript 2 (usa `$eval`, `$var`, `$try`, `$async`)
- Cada callback fica abaixo do limite de 65.500 caracteres do BDFD
- Sem dependências externas, tudo vive nos 3 comandos
- Sem variáveis de servidor, o estado vive nos rodapés dos embeds

## Créditos

Feito pela equipe Nexusify. Sinta-se livre para usar o código, só mantenha os créditos.

## Licença

Uso livre com créditos. Não apresente como seu.
