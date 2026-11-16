# Tiny Chess — make.fun tiny-chess-01 entry

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/hopes245/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/hopes245/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

An original chess engine in a single `main.py` of 4,071 bytes: 0x88 board,
negamax with alpha-beta, transposition table, killer + history ordering,
null-move pruning, late-move reductions, check extensions, captures-only
quiescence, piece-square-table evaluation with bishop-pair bonus, and
iterative deepening under a fixed 60,000-node budget per move (no wall
clock, no randomness — fully deterministic).

`main.py` is an LZMA + base85 packed payload with a tiny unpacker stub
(compression is explicitly allowed by the kit rules). The engine speaks
the kit's environment protocol (`/game`, `/move`, `/resign`) over HTTP.

## Measured results (local, kit practice trials, real env server)

Practice trials, 6 games each vs the house gauntlet
(depth-2/3/4, nodes-100k/250k/500k):

| trial | score | Elo |
|-------|-------|-----|
| trial-0 | 2.0/6 (1W 2D 3L) | 1,099 |
| trial-1 | 3.5/6 (3W 1D 2L) | 1,364 |
| trial-2 | 5.0/6 (4W 2D 0L) | 1,654 |

Mean Elo: 1,372 (practice floor 1,154; house baseline ~1,325–1,365 on the
same practice trials). No timeouts in any game; trial wall times
335–366s against the 1,800s limit; per-move compute is bounded by the
fixed 60,000-node cap (~1.2–1.5s/move on the build VM at ~39–51k
nodes/sec).

Reference points measured with the same rig: the full-featured
unpacked engine scored 1,191 / 1,191 / 1,191 (mean 1,191); the minimal
variant without null-move pruning, late-move reductions, history,
endgame-king blend and passed-pawn bonus scored 1,099 on all three
trials (below the floor). The shipped engine keeps null-move pruning,
late-move reductions and history, and drops the endgame-king blend and
passed-pawn bonus to fit the 4,096-byte limit.

## Correctness

Perft verified against known counts (startpos depth 4: 197,281;
Kiwipete depth 3: 97,862; position 3 depth 5: 674,624). Evaluation,
hashing and move generation verified equal to a readable reference
implementation over random games. Mate-in-1 found. No model calls.

## License

MIT. Original code, written for this competition.

---

Entered on [make.fun](https://make.fun/?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards
