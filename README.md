# X O  Tic-Tac-Toe

![C++](https://img.shields.io/badge/Language-C%2B%2B17-blue?style=flat-square&logo=cplusplus)
![SFML](https://img.shields.io/badge/Graphics-SFML%202.x-green?style=flat-square)
![Console](https://img.shields.io/badge/Version-Console%20%2B%20GUI-blueviolet?style=flat-square)
![Game](https://img.shields.io/badge/Type-2--Player%20Game-orange?style=flat-square)

> Two complete C++ implementations of Tic-Tac-Toe — a terminal version and a fully graphical SFML game with themes, sprite textures, and sound effects.

---

## Overview

This project ships two playable versions of Tic-Tac-Toe written in C++. Both versions share the same core game logic (board initialisation, move validation, win/draw detection) but differ in their presentation layer:

- **`TikTakTow-Logic.cpp`** — Terminal/console version using a dynamic 2D pointer array (`char**`) and keyboard input
- **`TikTakToe-SFML.cpp`** — SFML graphical version with mouse-click gameplay, sprite pieces, 4 colour themes, and audio

---

## Screenshots

| SFML Gameplay | Visual Studio — Console Code |
|:---:|:---:|
| ![Gameplay](Output/Screenshot-2026-03-20-211600.png) | ![VS_Code](Output/Screenshot-2026-02-07-020335.png) |

> Player X wins via left-column diagonal. Yellow game-over overlay with restart/exit prompt.

---

## Project Structure

```
TicTacToe/
├── TikTakTow.cpp         # Console / terminal version
├── TikTakToe.cpp         # SFML graphical version
├── tictak/
│   ├── X.png             # X piece sprite texture
│   └── oo.png            # O piece sprite texture
├── Font/
│   └── arial.ttf         # UI font
└── audio/
    ├── move1.wav          # X move sound
    ├── move2.wav          # O move sound
    ├── win.wav            # Win sound
    └── draw.wav           # Draw sound
```

---

## Shared Game Logic

Both versions use identical logic for all core game operations:

| Function | Description |
|----------|-------------|
| `initialBoard()` | Fills all cells with `'*'` (empty) |
| `inBoard(r, c)` | Bounds check — returns true if row/col are in range |
| `validMove(r, c)` | True if cell is in-bounds and currently empty |
| `makeMove(r, c, s)` | Places `'X'` or `'O'` at the given cell |
| `checkRow()` | Scans all rows for a completed line |
| `checkCol()` | Scans all columns for a completed line |
| `diagonal()` | Checks the main diagonal |
| `antiDiagonal()` | Checks the anti-diagonal |
| `win()` | OR-combines all four checks and returns winner |

### Win & draw detection

Win is only checked once `moveCount >= n*2-1` — the earliest a win is mathematically possible in a 3×3 grid is move 5. A draw is declared when `moveCount == n*n` with no winner found.

```cpp
if (moveCount >= n * 2 - 1 && win(p1, p2)) {
    // X or O wins — announce winner
} else if (moveCount == n * n) {
    // Board full, no winner — draw
}
```

---

## Version Comparison

| Feature | Console (`TikTakTow-Logic.cpp`) | Graphical (`TikTakToe-SFML.cpp`) |
|---------|--------------------------|------------------------------|
| Board storage | `char**` — dynamic 2D pointer | `char[n][n]` — static array |
| Input method | Row & column via `cin` | Mouse left-click on cell |
| Display | ASCII art grid in terminal | SFML sprites + coloured grid |
| Dependencies | None | SFML 2.x |
| Themes | — | 4 colour themes (press 1–4) |
| Sound | — | 4 `.wav` sound effects |
| Memory cleanup | Manual `delete[]` | Automatic (SFML manages) |

---

## SFML Controls

| Input | Action |
|-------|--------|
| Left click on a cell | Place your piece (X or O) |
| `R` | Reset / restart the game |
| `X` | Exit the window |
| `1` | Theme: Blue-Gray |
| `2` | Theme: Ocean |
| `3` | Theme: Purple-Orange |
| `4` | Theme: Green-Teal |

---

## Colour Themes (SFML version)

| Key | Name | Background | Cell A | Cell B |
|-----|------|-----------|--------|--------|
| `1` | Blue-Gray | `#34495e` | `#95a5a6` | `#ecf0f1` |
| `2` | Ocean | `#1a5276` | `#4ecdc4` | `#ff6b6b` |
| `3` | Purple-Orange | `#4834d4` | `#8563f2` | `#f8c291` |
| `4` | Green-Teal | `#145a32` | `#38ada9` | `#fdcb6e` |

---

## How to Build & Run

### Console version — no dependencies needed

```bash
# Compile
g++ -std=c++17 -o tictactoe TikTakTow-Logic.cpp

# Run
./tictactoe
```

### SFML graphical version

**Linux / macOS**
```bash
g++ -std=c++17 TikTakToe-SFML.cpp -o tictactoe_sfml \
    -lsfml-graphics -lsfml-window -lsfml-system -lsfml-audio

./tictactoe_sfml
```

**Windows (MinGW)**
```bash
g++ -std=c++17 TikTakToe.cpp -o tictactoe_sfml.exe ^
    -lsfml-graphics -lsfml-window -lsfml-system -lsfml-audio

tictactoe_sfml.exe
```

> [!NOTE]
> SFML 2.x must be installed. On Ubuntu: `sudo apt install libsfml-dev`. On macOS: `brew install sfml`. On Windows, download from [sfml-dev.org](https://www.sfml-dev.org/download.php) and link manually.

> [!WARNING]
> The graphical version expects `tictak/X.png`, `tictak/oo.png`, `Font/arial.ttf`, and the four `audio/*.wav` files **relative to the executable**. Missing audio files fall back gracefully with a console warning. Missing textures or font will exit with an error.

---

## Console Gameplay Example

```
-----------------------

1|   *     *     *   |

2|   *     *     *   |

3|   *     *     *   |

_______________________
 |   1     2     3   |
-----------------------

Player X's Turn
Enter row and column e.g. 1 2 (First-Row 2nd-col) :
```

---

## How to Play (Console)

1. Players take turns entering a row and column (e.g. `1 2` = row 1, column 2)
2. The board re-renders after each valid move
3. Invalid moves print `OOPs!! Invalid Move !!` and re-prompt
4. First player to complete a row, column, or diagonal wins
5. If all 9 cells fill with no winner, it's a draw

---

## Author

Built as part of a C++ programming coursework series.  
FAST National University of Computer & Emerging Sciences (NUCES)

---

<p align="center"><sub>Two versions. One logic. Zero draws tolerated.</sub></p>
