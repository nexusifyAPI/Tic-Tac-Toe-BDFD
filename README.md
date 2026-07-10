# Tic-Tac-Toe for BDFD

A Tic-Tac-Toe game for Bot Designer For Discord (BDFD), with a PvP mode and a vs-AI mode with 4 difficulty levels. The Impossible difficulty uses minimax with a precomputed lookup table covering all 19,683 board states, so it never loses.

## What it includes

- 4 AI difficulties: Easy, Normal, Hard, Impossible (minimax)
- PvP mode with accept/reject buttons for challenges
- Random X/O assignment each game
- Rematch system: both players have to vote to restart
- Forfeit and restart in AI mode
- Winning line highlighted in green
- Move counter (X/9)
- No server variables needed, game state lives in the embed footer
- Unlimited simultaneous games, each board is independent

## Setup

You'll need 3 commands in your bot:

| # | Trigger | Language | File | What it does |
|---|---------|----------|------|---------------|
| 1 | `!ttt` | BDScript 2 | `Tic-Tac-Toe_Code.md` | Main command (AI menu or PvP challenge) |
| 2 | `$onInteraction` | BDScript 2 | `Callback 1 ($onInteraction).md` | Menu, challenges, forfeit, rematch, restart |
| 3 | `$onInteraction` | BDScript 2 | `Callback 2 ($onInteraction).md` | Cell clicks + minimax AI |

**Why two `$onInteraction` callbacks?** BDFD lets you have more than one, and it runs all of them on every interaction. The minimax lookup table is 19,683 characters long and doesn't fit alongside the rest of the code in a single callback, so it's split in two: the first handles the difficulty menu, accept/reject, and forfeit/rematch/restart; the second handles cell clicks and the minimax table. Each one uses `$stop` so they don't interfere with each other.

Bot permissions needed: Send Messages, Manage Messages (to edit the board), and Use Components (buttons/select menus).

## How it works

### Game state

Stored in the embed footer, using this format:

```
playerX_ID|playerO_ID|difficulty|cell0.cell1.cell2.cell3.cell4.cell5.cell6.cell7.cell8|rematchVoteX|rematchVoteO
```

- `playerX_ID` / `playerO_ID`: Discord user ID (or `AI`)
- `difficulty`: `e` Easy, `n` Normal, `h` Hard, `i` Impossible
- `cell0`–`cell8`: `e` empty, `X`, or `O`
- `rematchVoteX` / `rematchVoteO`: `y` or `n`

Since it doesn't rely on server variables, each game is self-contained and you can run several at once in the same channel.

### AI difficulties

| Difficulty | Strategy |
|-----------|----------|
| Easy | Random moves |
| Normal | Wins if it can, blocks a threat, otherwise random |
| Hard | Wins, blocks, sets up forks, blocks opponent forks, center, opposite corner, corner, side |
| Impossible | Minimax with a precomputed table for all 19,683 states. Never loses. |

### The minimax algorithm

For the Impossible difficulty:

1. `minimax_solver.py` runs an exhaustive minimax over all 3⁹ = 19,683 possible board states, and for each one where it's the AI's turn, it works out the optimal move assuming the opponent plays perfectly.
2. That gets stored as a 19,683-character table: each position corresponds to a state (indexed by a base-3 hash), and the character there is the optimal move (0-8), or `-` if that state isn't valid.
3. In BDFD: cells get mapped to numbers (`e=0`, `iaSym=1`, `userSym=2`), the hash is calculated as `hash = n0*1 + n1*3 + n2*9 + ... + n8*6561`, looked up with `$splitText[$sum[$var[hash];1]]`, and that move gets played.

### Random X/O assignment

At the start of a game (new, restart, or rematch), `$random[0;2]` decides who's X: half the time you start, half the time the AI starts (and plays the center automatically). The AI is parameterized with `$var[iaSym]` and `$var[userSym]`, so it works the same whether it's X or O.

### Rematch (PvP)

When a PvP game ends, the button changes to "Rematch (0/2)". Each player votes by pressing it, and once it hits 2/2 the board resets with a new random X/O assignment. Only the two original players can vote; anyone else gets an error.

## Repo structure

```
Repositorio/
│
├── README.md                          (English version)
├── LEEME.md                           (Spanish version)
│
├── English/
│   ├── Tic-Tac-Toe_Code.md
│   ├── Callback 1 ($onInteraction).md
│   └── Callback 2 ($onInteraction).md
│
└── Español/
    ├── Tres-en-raya_BDFD.md
    ├── Callback 1 ($onInteraction).md
    └── Callback 2 ($onInteraction).md
```

## Usage

Vs AI: type `!ttt` and pick a difficulty from the dropdown. You'll be assigned X or O at random.

Vs another player: `!ttt @user`. The challenged player accepts with the button, and X/O gets assigned randomly too.

During the game: click an empty cell (⬜) to play, "Forfeit" to give up, and once it's over, "Restart" (vs AI) or "Rematch" (PvP) to go again.

## Technical details

- Language: BDScript 2 (uses `$eval`, `$var`, `$try`, `$async`)
- Each callback stays under BDFD's 65,500 character limit
- No external dependencies, everything lives in the 3 commands
- No server variables, state lives in the embed footers

## Credits

Made by Nexusify team. Feel free to use the code, just keep the credits.

## License

Free to use with credit. Don't pass it off as your own.
