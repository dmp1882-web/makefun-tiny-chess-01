[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/dmp1882-web/makefun-tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/dmp1882-web/makefun-tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

# tiny-chess-01

A chess engine in 4 KB of Python. It plays the make.fun house gauntlet
(depth-2 to nodes-2M) through the env: POST /game serves the game in play,
POST /move plays a move in UCI.

## How it plays

- 0x88 board, alpha-beta with iterative deepening to a fixed node budget
  (60,000 nodes a move, about 3 seconds; never a clock, so a replay plays
  the same moves).
- Quiescence over captures and promotions (delta-pruned), and over every
  reply to a check.
- Transposition table, MVV-LVA captures, killer moves and history ordering.
- Evaluation: material (pawn 100, knight 320, bishop 330, rook 500,
  queen 900) plus piece-square tables and the bishop pair.

## Size

- `main.py` is the whole engine: 3,350 bytes as committed, Python 3.13
  standard library only. It is the minified source packed by the kit's
  `makefun compress` (a `makefun-compress/v1` stub around an LZMA stream
  of the source); the stub and the stream both count toward the 4,096-byte
  cap. The readable source is `dev/engine.py`; the packed source is the
  minified form in `src.py`, written by the agent and packed with
  `makefun compress src.py --out main.py`.
- The whole repository (main.py, LICENSE, README) is under the 8,192-byte
  cap, no file over 4,096.

## Local result

Practice trial-0 against the kit's own env server (the real house engines):
2 won, 1 drawn, 3 lost — about 1,191 Elo, above the 1,154 practice floor.
It beats depth-2 and depth-4 and draws depth-3; it loses to the node rungs.
