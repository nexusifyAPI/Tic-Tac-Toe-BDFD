# Jogo da Velha para BDFD - Versao Definitiva

Um jogo completo de Jogo da Velha para **Bot Designer For Discord** (BDFD) com modo **PvP** e modo **vs IA** (4 dificuldades).

A dificuldade **Impossivel** usa um **algoritmo minimax real** com uma tabela precomputada de 19,683 estados, tornando-o matematicamente perfeito — nunca perde.

---

## Caracteristicas

- **4 dificuldades de IA**: Facil, Normal, Dificil, Impossivel (minimax)
- **Modo PvP**: Desafie outro jogador com botoes de aceitar/rejeitar
- **Atribuicao aleatoria de X/O**: As vezes voce comeca, as vezes a IA comeca
- **Sistema de revanche**: Ambos os jogadores devem votar para reiniciar (PvP)
- **Render-se/Reiniciar**: Renda-se a qualquer momento, reinicie apos o fim (vs IA)
- **Destaque da linha vencedora**: As 3 celulas vencedoras ficam verdes
- **Contador de jogadas**: Mostra X/9 jogadas feitas
- **Sem variaveis de servidor**: O estado do jogo e armazenado no rodape do embed
- **Jogos simultaneos ilimitados**: Cada tabuleiro e independente

---

## Configuracao

### 1. Criar os comandos

Voce precisa criar **3 comandos** no seu bot BDFD:

| # | Gatilho | Linguagem | Arquivo | Descricao |
|---|---------|-----------|---------|-----------|
| 1 | `!ttt` | BDScript 2 | `Jogo-da-Velha_BDFD.md` | Comando principal (menu IA ou desafio PvP) |
| 2 | `$onInteraction` | BDScript 2 | `Callback 1 ($onInteraction).md` | Gerencia menu, desafios, render-se, revanche, reiniciar |
| 3 | `$onInteraction` | BDScript 2 | `Callback 2 ($onInteraction).md` | Gerencia jogadas + IA minimax |

### 2. Por que dois callbacks `$onInteraction`?

O BDFD suporta multiplos callbacks `$onInteraction` — ambos sao executados em cada interacao. A tabela minimax (19,683 caracteres) e muito grande para caber em um unico callback, entao o codigo e dividido:

- **Callback 1**: Gerencia as rotas A (menu de dificuldade), B (aceitar desafio), C (rejeitar desafio), E (render-se/revanche/reiniciar)
- **Callback 2**: Gerencia a rota D (jogar celula) + a tabela minimax

Cada callback usa `$stop` para nao interferir com o outro.

### 3. Permissoes do bot

O bot precisa destas permissoes:
- Enviar mensagens
- Gerenciar mensagens (para editar o tabuleiro)
- Usar componentes (botoes / menus de selecao)

---

## Como funciona

### Armazenamento do estado

O estado do jogo e armazenado no **rodape do embed** (nao em variaveis de servidor), usando o formato:

```
ID_jogadorX|ID_jogadorO|dificuldade|celula0.celula1.celula2.celula3.celula4.celula5.celula6.celula7.celula8|votoRevancheX|votoRevancheO
```

- `ID_jogadorX` / `ID_jogadorO`: IDs de usuario do Discord (ou `IA` para modo IA)
- `dificuldade`: `e` (Facil), `n` (Normal), `h` (Dificil), `i` (Impossivel)
- `celula0`-`celula8`: `e` (vazia), `X`, ou `O`
- `votoRevancheX` / `votoRevancheO`: `y` (votou) ou `n` (nao votou)

Isso torna cada jogo independente — jogos simultaneos ilimitados no mesmo canal.

### Dificuldades de IA

| Dificuldade | Estrategia |
|-------------|------------|
| **Facil** | Jogadas aleatorias |
| **Normal** | Vencer se possivel, bloquear se ameacado, caso contrario aleatorio |
| **Dificil** | Vencer, bloquear, criar forks, bloquear forks do oponente, centro, canto oposto, canto, lado |
| **Impossivel** | **Minimax real** — jogada otima precomputada para todos os 19,683 estados. Nunca perde. |

### Algoritmo minimax

A IA Impossivel usa uma **tabela minimax** precomputada em Python:

1. **Solver Python**: Executa um minimax exaustivo sobre todos os 19,683 estados possiveis do tabuleiro (3^9). Para cada estado valido onde e a vez da IA, calcula a jogada otima assumindo jogo perfeito do oponente.

2. **Tabela precomputada** (19,683 caracteres): Cada posicao na string representa um estado do tabuleiro (indexado por hash base-3). O caractere nessa posicao e a jogada otima (0-8) ou `-` se o estado for invalido.

3. **No BDFD**, para a dificuldade Impossivel:
   - Mapeia celulas para numeros: `e=0`, `iaSym=1`, `userSym=2`
   - Calcula o hash: `hash = n0*1 + n1*3 + n2*9 + ... + n8*6561`
   - Consulta a tabela: `$splitText[$sum[$var[hash];1]]`
   - Executa essa jogada otima

### Atribuicao aleatoria de X/O

Ao iniciar um jogo (novo, reiniciar ou revanche), um numero aleatorio (`$random[0;2]`) determina quem e X e quem e O:

- **50% de chance**: O usuario e X (comeca primeiro)
- **50% de chance**: A IA e X (comeca primeiro, joga o centro automaticamente)

A IA e totalmente parametrizada com `$var[iaSym]` e `$var[userSym]` para funcionar corretamente independentemente de ser X ou O.

### Sistema de revanche (PvP)

Apos um jogo PvP terminar:
1. O botao muda para "Revanche (0/2)"
2. Cada jogador pressiona para votar — o botao atualiza para "Revanche (1/2)"
3. Quando ambos votam (2/2), o tabuleiro reinicia com nova atribuicao aleatoria de X/O
4. Apenas os dois jogadores podem votar; outros recebem um erro

---

## Uso

### Jogar vs IA

```
!ttt
```

Selecione uma dificuldade no menu suspenso. Voce sera atribuido X ou O aleatoriamente.

### Jogar vs outro jogador

```
!ttt @usuario
```

O jogador desafiado deve pressionar "Aceitar" para comecar. X/O e atribuido aleatoriamente.

### Durante o jogo

- Clique em qualquer celula vazia (⬜) para colocar sua marca
- Pressione "Render-se" para se render
- Apos o jogo terminar, pressione "Reiniciar" (vs IA) ou "Revanche" (PvP) para jogar novamente

---

## Creditos

Criado por **Nexusify team**.

Voce e livre para usar este codigo, mas por favor nao remova os creditos. Levou 3 meses de trabalho — respeite o esforco.

---

## Licenca

Uso livre com creditos. Nao reivindique como seu.
