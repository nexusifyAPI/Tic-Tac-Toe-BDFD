# Tic-Tac-Toe for BDFD - Definitive Version

A complete Tic-Tac-Toe game for **Bot Designer For Discord** (BDFD) with **PvP** mode and **vs AI** mode (4 difficulties).

The **Impossible** difficulty uses a **real minimax algorithm** with a precomputed lookup table of 19,683 states, making it mathematically perfect — it never loses.

---

## Features

- **4 AI difficulties**: Easy, Normal, Hard, Impossible (minimax)
- **PvP mode**: Challenge another player with accept/reject buttons
- **Random X/O assignment**: Sometimes you start, sometimes the AI starts
- **Rematch system**: Both players must vote to restart (PvP)
- **Forfeit/Restart**: Surrender at any time, restart after game over (vs AI)
- **Winning line highlight**: The 3 winning cells turn green
- **Move counter**: Shows X/9 moves made
- **No server variables needed**: Game state is stored in the embed footer
- **Unlimited simultaneous games**: Each board is independent

---

## Setup

### 1. Create the commands

You need to create **3 commands** in your BDFD bot:

| # | Trigger | Language | File | Description |
|---|---------|----------|------|-------------|
| 1 | `!ttt` | BDScript 2 | `Tic-Tac-Toe_Code.md` | Main command (AI menu or PvP challenge) |
| 2 | `$onInteraction` | BDScript 2 | `Callback 1 ($onInteraction).md` | Handles menu, challenges, forfeit, rematch, restart |
| 3 | `$onInteraction` | BDScript 2 | `Callback 2 ($onInteraction).md` | Handles cell clicks + minimax AI |

### 2. Why two `$onInteraction` callbacks?

BDFD supports multiple `$onInteraction` callbacks — both run on every interaction. The minimax lookup table (19,683 characters) is too large to fit in a single callback, so the code is split:

- **Callback 1**: Handles routes A (difficulty menu), B (accept challenge), C (reject challenge), E (forfeit/rematch/restart)
- **Callback 2**: Handles route D (play a cell) + the minimax lookup table

Each callback uses `$stop` to avoid interfering with the other.

### 3. Bot permissions

The bot needs these permissions:
- Send Messages
- Manage Messages (to edit the board)
- Use Components (buttons / select menus)

---

## How it works

### Game state storage

The game state is stored in the **embed footer** (not in server variables), using the format:

```
playerX_ID|playerO_ID|difficulty|cell0.cell1.cell2.cell3.cell4.cell5.cell6.cell7.cell8|rematchVoteX|rematchVoteO
```

- `playerX_ID` / `playerO_ID`: Discord user IDs (or `AI` for AI mode)
- `difficulty`: `e` (Easy), `n` (Normal), `h` (Hard), `i` (Impossible)
- `cell0`-`cell8`: `e` (empty), `X`, or `O`
- `rematchVoteX` / `rematchVoteO`: `y` (voted) or `n` (not voted)

This makes each game independent — unlimited simultaneous games can run in the same channel.

### AI difficulties

| Difficulty | Strategy |
|-----------|----------|
| **Easy** | Random moves |
| **Normal** | Win if possible, block if threatened, otherwise random |
| **Hard** | Win, block, create forks, block opponent forks, center, opposite corner, corner, side |
| **Impossible** | **Real minimax** — precomputed optimal move for all 19,683 states. Never loses. |

### Minimax algorithm

The Impossible AI uses a **minimax lookup table** precomputed in Python:

1. **Python solver** (`minimax_solver.py`): Runs an exhaustive minimax over all 19,683 possible board states (3^9). For each valid state where it's the AI's turn, it computes the optimal move assuming perfect opponent play.

2. **Lookup table** (19,683 characters): Each position in the string represents a board state (indexed by base-3 hash). The character at that position is the optimal move (0-8) or `-` if the state is invalid.

3. **In BDFD**, for the Impossible difficulty:
   - Maps cells to numbers: `e=0`, `iaSym=1`, `userSym=2`
   - Calculates the hash: `hash = n0*1 + n1*3 + n2*9 + ... + n8*6561`
   - Looks up the table: `$splitText[$sum[$var[hash];1]]`
   - Plays that optimal move

### Random X/O assignment

When a game starts (new game, restart, or rematch), a random number (`$random[0;2]`) determines who is X and who is O:

- **50% chance**: User is X (starts first)
- **50% chance**: AI is X (starts first, plays center automatically)

The AI is fully parameterized with `$var[iaSym]` and `$var[userSym]` so it works correctly regardless of whether it's X or O.

### Rematch system (PvP)

After a PvP game ends:
1. The button changes to "Rematch (0/2)"
2. Each player presses to vote — button updates to "Rematch (1/2)"
3. When both vote (2/2), the board resets with new random X/O assignment
4. Only the two players can vote; others get an error

---

## File structure

```
Repositorio/
│
├── README.md                          (this file - English)
├── LEEME.md                           (Spanish version)
│
├── English/
│   ├── Tic-Tac-Toe_Code.md            (!ttt command)
│   ├── Callback 1 ($onInteraction).md (menu, challenges, forfeit, rematch)
│   └── Callback 2 ($onInteraction).md (cell clicks + minimax AI)
│
└── Español/
    ├── Tres-en-raya_BDFD.md           (comando !ttt)
    ├── Callback 1 ($onInteraction).md (menú, retos, rendirse, revancha)
    └── Callback 2 ($onInteraction).md (jugadas + IA minimax)
```

---

## Usage

### Play vs AI

```
!ttt
```

Select a difficulty from the dropdown menu. You'll be assigned X or O randomly.

### Play vs another player

```
!ttt @user
```

The challenged player must press "Accept" to start. X/O is assigned randomly.

### During the game

- Click any empty cell (⬜) to place your mark
- Press "Forfeit" to surrender
- After the game ends, press "Restart" (vs AI) or "Rematch" (PvP) to play again

---

## Technical details

- **Language**: BDScript 2 (required for `$eval`, `$var`, `$try`, `$async`)
- **Character limit**: Each callback stays under BDFD's 65,500 character limit
- **No external dependencies**: Everything is self-contained in the 3 commands
- **No server variables**: Game state lives in embed footers

---

## Credits

Created by **Nexusify team**.

You are free to use this code, but please do not remove the credits. This took 3 months of work — respect the effort.

---

## License

Free to use with credit. Do not claim as your own.
