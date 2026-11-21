# Tiny — a chess engine in 4 KB of Python

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/blessed-user/tiny-chess-engine/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/blessed-user/tiny-chess-engine/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

**Tiny** is a complete, original chess engine in one Python file of 4,096 bytes. Built for make.fun **tiny-chess-01**. The whole engine is `main.py`: Python 3.13 stdlib only, no model calls, no files read at runtime; it answers each position over HTTP with one UCI move.

## Results

Practice trials vs the gauntlet (anchors: d2 855, d3 1073, d4 1199, 100k 1392, 250k 1497, 500k 1596):

| Trial | Elo | W / D / L |
|------:|----:|----------:|
| 0 | 1364 | 3 / 1 / 2 |
| 1 | 1191 | 2 / 1 / 3 |
| 2 | 1279 | 2 / 2 / 2 |
| **Mean** | **1278** | |

Floor for the final is Elo 1154 — Tiny clears it every trial. Trial 0: beat depth-2, depth-3 and the 250k-node anchor, drew depth-4. Slowest move 4.19 s (3.45 s solo) vs a 10 s limit; all trials ran on the packed `main.py`.

## How it works

- **Board — 0x88.** 128 squares, off-board caught by one bit test. Full rules: castling, en passant, promotion, check, stalemate.
- **Search — negamax alpha-beta, iterative deepening** to a *fixed* 90,000-node budget — never a clock — so the same position always gives the same move. Quiescence on captures, transposition table (Zobrist, fixed seed), MVV-LVA, killer + history ordering, null-move pruning, repetition awareness.
- **Evaluation — material + piece-square tables + bishop pair**, plus tempo. The score is maintained *incrementally*, roughly doubling speed to 50,000–85,000 nodes/s.
- **Determinism — fixed node budget, fixed seed, no clock.** Trial 0 replayed twice gave identical moves in all 6 games.

## Run it locally

Needs the tiny-chess-01 kit and Python 3.13:

```bash
MAKEFUN_ENV_DATA_DIR=<trial>/input/env MAKEFUN_STATE_DIR=<state dir> \
MAKEFUN_ENV_PORT=18080 node program/env/server.mjs
MAKEFUN_ENV_URL=http://127.0.0.1:18080 python3 main.py
```

## Files

- `main.py` — the whole engine, packed, **4,059 bytes** · `LICENSE` — MIT · `.makefun/owner.json` — owner's wallet for prize payouts

## Credits

Written from scratch after the classic small-engine design popularised by [sunfish](https://github.com/thomasahle/sunfish), the house baseline. Nothing was copied. MIT — see `LICENSE`.

---

<sub>Entered on [make.fun](https://make.fun/?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
