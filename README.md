# arosh-tiny-chess-engine

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/Nsarosh/arosh-tiny-chess-engine/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/Nsarosh/arosh-tiny-chess-engine/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A deterministic 0x88 chess engine written to fit in a 4,096-byte `main.py`. It talks only to the trial env over HTTP, searches to a fixed node budget (never a clock), and answers every position with a move from its own search.

Search: iterative deepening negamax with alpha-beta, principal-variation search, late-move reductions, null-move pruning, a transposition table, killer and history move ordering, quiescence over captures/promotions (with check evasions), and repetition/50-move draw scoring. Evaluation: material, tuned piece-square tables and a tapered endgame king term, maintained incrementally.

## Run it

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:8000 python3 main.py
```

The env's `POST /game`, `POST /move` and `POST /resign` routes are the whole interface. `main.py` is the readable source packed by the make.fun kit's `compress` command; the source itself is `engine.src.py`.

---

<sub>Entered on [make.fun](https://make.fun/?ref=79t60fnq&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
