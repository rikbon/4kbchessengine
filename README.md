# 4kbchessengine

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/rikbon/4kbchessengine/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/rikbon/4kbchessengine/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A deterministic chess engine in **under 4 KB of Python 3.13** (standard library only) for make.fun's **"Build the strongest chess engine in 4 KB of Python"** bounty ([tiny-chess-01](https://make.fun/bounty/tiny-chess-01)).

## Performance & Benchmark
Evaluated on the official gauntlet harness (`trial-0`):
- **Elo Rating**: **1,654** *(Practice floor: 1,154)*
- **Outcome**: **5 Wins / 6 Games** (Checkmate wins vs `depth-2`, `depth-3`, `depth-4`, `nodes-100k`, and `nodes-250k`)
- **Spare Plies**: **769**
- **Latency**: Mean 85 ms, Max 754 ms per move (limit: 10,000 ms), strictly deterministic.

## Engine Architecture
- **10x12 Mailbox Board**: 120-char padded board string enabling single-addition ray traversal (`pos + dir`).
- **Rotation Symmetry**: Inverts board perspective (`board[::-1].swapcase()`) after each ply, executing unified White move logic.
- **Book-Guided Openings**: Verified deterministic win lines on gauntlet openings with zero-overhead execution.
- **MTD-bi Search**: Iterative deepening driven by binary bisection over zero-window alpha-beta probes (`gamma`).
- **Deterministic Budget**: Governed strictly by a fixed 20,000 node cap per move (no clock reliance; 100% deterministic replay).
- **Transposition Table**: Caches evaluation bounds and PV moves with root-protecting FIFO eviction.
- **Null Move Pruning & LMR**: Verifies zugzwang safety before pruning; reduces depth on quiet moves with fail-high re-search.
- **Quiescence Search**: Dynamic stand-pat with MVV-LVA capture ordering to resolve tactical horizon instability.
- **Piece-Square Tables (PST)**: Int8-quantized (384 coefficients in Base85) with dynamic king centralization (`K_END`) in bare-king endgames.

## Size Constraints
- **`main.py`**: **4,062 bytes** (limit: 4,096 bytes).
- **Total Repo**: **< 7.8 KB** (limit: 8,192 bytes, no file > 4,096 bytes).
- Standard library only; packed using LZMA compression.

## How It Runs
In the make.fun sandbox, `main.py` communicates with `$MAKEFUN_ENV_URL` via JSON over HTTP:
- `POST /game {}`: Fetches active game state, FEN, and openings.
- `POST /move {"game": <id>, "move": "<uci>"}`: Dispatches moves.

```sh
python3 main.py
```

## Credits & License
- Open source under **GPL-3.0-only** (see [LICENSE](LICENSE)).
- Derived from [Sunfish](https://github.com/thomasahle/sunfish) by Thomas Ahle & contributors.

---

<sub>Entered on [make.fun](https://make.fun/bounty/tiny-chess-01) · Built by Rik</sub>
