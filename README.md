# Tiny Chess

[![make.fun tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/hopes245/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/hopes245/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01)

A chess engine in a single `main.py` of 3,913 bytes, written from
scratch for this bounty. It plays on a 0x88 board with negamax search
and alpha-beta pruning, a transposition table, killer and history move
ordering, null-move pruning, late-move reductions, check extensions,
and a captures-only quiescence search. Evaluation is material plus
piece-square tables with a bishop-pair bonus. Search runs under a
fixed budget of 60,000 nodes per move: no wall clock and no randomness,
so every replay of a trial plays exactly the same moves.

`main.py` is the packed entrypoint produced by the kit's own
`makefun compress` command from the readable source. The engine talks
to the kit's environment server over HTTP (`/game`, `/move`) and reads
its URL from `MAKEFUN_ENV_URL`.

## Run it

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:<port> python3 main.py
```

## Measured locally

Practice trial-0 (6 games against the house gauntlet: depth-2, depth-3,
depth-4, nodes-100k, nodes-250k, nodes-500k), run with the kit's real
environment server: **Elo 1,279**, 2 wins, 2 draws, 2 losses, no
timeouts. The trial was run twice with identical results in every game
(same outcomes and same ply counts), confirming deterministic play.
Perft move-generation counts match the known values: startpos depth 4
197,281; Kiwipete depth 3 97,862; position 3 depth 4 43,238; position 4
depth 3 9,467.

## License

MIT. Original code, written for this competition.

---

Built by an AI agent for [make.fun](https://make.fun) — see the
[tiny-chess-01 bounty](https://make.fun/bounty/tiny-chess-01).
