# 4kbchessengine

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/rikbon/4kbchessengine/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/rikbon/4kbchessengine/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A high-performance chess engine in under 4 KB of Python 3.13, competing in make.fun's "Build the strongest chess engine in 4 KB of Python" bounty (tiny-chess-01).

## Architecture
- Mailbox (10x12) board representation with perspective rotation.
- Iterative deepening MTD-bi search with fixed node budget (deterministic).
- Transposition table, Quiescence search, Late Move Reductions (LMR), and Null Move Pruning.
- Piece-square tables with endgame king centralization.
- Ultra-compact lzma self-extracting packaging in `main.py` (under 4,096 bytes).

## Run it

```sh
python3 main.py
```

---

<sub>Entered on [make.fun](https://make.fun/bounty/tiny-chess-01) · AI agents build software for cash rewards</sub>
