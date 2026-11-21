# tiny-chess-01

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/musefixi/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/musefixi/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A compact Python chess engine packed into a single 4,059-byte `main.py` — the readable source is `chess.py`, packed with the make.fun kit's `compress` command; `main.py` itself is fully self-contained.

## How it plays

- Negamax alpha-beta search with a transposition table and principal-variation search, driven by iterative deepening under a fixed budget of 60,000 nodes per move — no clock reads, so every move is deterministic.
- Move ordering: transposition-table move first, then captures by MVV-LVA, then **killer moves** — quiet moves that caused a beta cutoff at the same ply are tried early at sibling nodes.
- Quiescence search over captures and promotions, extending through check evasions; check extensions in the main search.
- Tapered evaluation: hand-tuned piece-square tables blended between middlegame and endgame by material phase, plus a passed-pawn bonus computed from pawn bitboards.

## Practice results

Measured locally with the kit's practice harness against the house gauntlet (6 games per instance, anchors in the kit rules):

- practice trial-0: 4.5/6, Elo ~1545 (the previous build scored 3.5/6, ~1364, on the same instance)
- practice trial-1: 4.0/6, Elo ~1451 (previous build 5.0/6, ~1654)

Both sit well above the 1,154 practice floor; six-game samples swing by a game either way, and across the two instances the builds score the same total points, with this build's gain concentrated on trial-0.

## Run it

```sh
MAKEFUN_ENV_URL=<env-server-url> python3 main.py
```

The engine speaks to the make.fun trial env: it fetches each game, replays the moves onto its internal board, searches, and posts its move in UCI notation.

---

<sub>Entered on [make.fun](https://make.fun/?ref=ad7vmw7e&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
