# Tic-Tac-Toe (Console)

Simple Tic-Tac-Toe game written in Python.

## Description

This is a console-based Tic-Tac-Toe (noughts and crosses) game. The player plays as `X` and the computer plays as `O`. The computer currently chooses its moves randomly.

## Features

* 3×3 board display in the terminal
* Player vs Computer (random move selection)
* Win / tie detection (rows, columns, diagonals)

## Requirements

* Python 3.7+
* No external libraries required (uses only the built-in `random` module)

## How to run

1. Save your script (for example `tictactoe.py`).
2. From a terminal run:

```bash
python tictactoe.py
```

## How to play

* When prompted, enter a number from `1` to `9` to place your `X` on the board.
* The positions map to the board like this:

```
1 | 2 | 3
---------
4 | 5 | 6
---------
7 | 8 | 9
```

* After your move, the program checks for a win or tie, then the computer (O) will make a move.

## Example session

```
1 | 2 | 3
---------
4 | 5 | 6
---------
7 | 8 | 9
enter a number from 1-9 : 5
... (game progresses) ...
Th winner is X
```

## Code overview

Your main pieces of code are:

* `printBoard(board)`: prints the 3×3 board.
* `playerinput(board)`: reads player's input and places `X`.
* `compmove(board)`: computer picks a random free position and places `O`.
* `check1`, `check2`, `check3`: check rows, columns and diagonals for a winner.
* `wincheck(board)`: uses the checks to determine a winner or a tie and ends the game.

## Known issues and suggested fixes

Your current implementation works for many cases but has a few problems you may want to fix:

1. **No input validation for non-integers**

   * `playerinput` calls `int(input(...))` without catching `ValueError`. If the user types text, the program will crash.
   * **Fix:** wrap the conversion in a `try/except` block and re-prompt the player until a valid move is entered.

2. **Invalid move doesn't reprompt**

   * When the user enters an invalid or occupied number the program prints `oops error` and continues. Better UX is to ask again until a valid move is made.

3. **Game can hang after a win/tie**

   * After a player move, `wincheck(board)` may set `gamestart = False`. But the loop continues to call `Turnchange()` and `compmove()` in the same iteration, and `compmove()` can enter an infinite loop if no free cells remain.
   * **Fix:** after each `wincheck(board)` return to the loop (or check `if not gamestart: break`), so the computer move is not attempted when the game has ended.

4. **Minor typos in output**

   * Output message: `Th winner is {winner}` — consider `The winner is {winner}`.

5. **Computer is purely random**

   * The AI is simple: it picks a random free cell. You can improve it by implementing a strategy (block immediate wins, choose center, corners) or full Minimax.

## Suggested improvements and enhancements

* Implement input validation & re-prompting loop for the human player.
* Improve `compmove()` with a deterministic strategy (center-first, corners, block/win checks) or implement Minimax for an unbeatable AI.
* Add an option to choose `X` or `O` and to choose who goes first.
* Add a simple command-line menu to play again without restarting the script.
* Add tests for the board win/tie detection functions.
* Add nicer UX: clear screen between moves (using `os.system('cls' if os.name == 'nt' else 'clear')`) and colorized output.

### Minimal code fixes (concept)

Below is a short conceptual example (do not paste this over your whole file automatically) that shows how to avoid the infinite loop and re-prompt for valid input:

```python
# after player move
wincheck(board)
if not gamestart:
    break  # stop the game loop immediately when there's a win or tie

Turnchange()
compmove(board)
wincheck(board)
```

And for safe input:

```python
while True:
    try:
        inp = int(input("Enter a number from 1-9: "))
    except ValueError:
        print("Please enter a valid number.")
        continue
    if 1 <= inp <= 9 and board[inp-1] == str(inp):
        board[inp-1] = currentPlayer
        break
    else:
        print("That cell is taken or invalid — try again.")
```

## Contributing

Feel free to open issues or submit pull requests. If you want help implementing a Minimax AI or improving input handling, tell me which direction you'd like to go and I can provide a patch.

## License

This project is provided under the MIT License — feel free to reuse and modify the code.

---

If you want, I can also:

* Provide a fixed/refactored version of your script with the bug fixes mentioned.
* Implement a simple Minimax AI and include it in the repository.
* Add a `requirements.txt` (not needed for this simple script) and a basic test file.

Tell me which one you want next!
