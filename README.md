# Tic-Tac-Toe for BDFD

A Tic-Tac-Toe game for Bot Designer For Discord (BDFD), with a PvP mode and a vs-AI mode with 4 difficulty levels. The Impossible difficulty uses minimax with a precomputed lookup table covering all 19,683 board states, so it never loses.

## Features

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

**Why two `$onInteraction` callbacks?** The minimax lookup table is 19,683 characters long and doesn't fit in a single callback, so it's split in two. Each one uses `$stop` so they don't interfere with each other.

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

For the Impossible difficulty, a Python script precomputes the optimal move for all 19,683 possible board states and stores them in a lookup table. In BDFD, cells are mapped to numbers, the board state is hashed, the optimal move is looked up in the table, and that move gets played.

### Random X/O assignment

At the start of a game (new, restart, or rematch), `$random[0;2]` decides who's X: half the time you start, half the time the AI starts (and plays the center automatically). The AI is parameterized with `$var[iaSym]` and `$var[userSym]`, so it works the same whether it's X or O.

### Rematch (PvP)

When a PvP game ends, the button changes to "Rematch (0/2)". Each player votes by pressing it, and once it hits 2/2 the board resets with a new random X/O assignment. Only the two original players can vote; anyone else gets an error.

## Usage

Vs AI: type `!ttt` and pick a difficulty from the dropdown. You'll be assigned X or O at random.

Vs another player: `!ttt @user`. The challenged player accepts with the button, and X/O gets assigned randomly too.

During the game: click an empty cell (⬜) to play, "Forfeit" to give up, and once it's over, "Restart" (vs AI) or "Rematch" (PvP) to go again.

## Credits

Made by Nexusify team. Feel free to use the code, just keep the credits.

## License

Free to use with credit. Don't pass it off as your own.
