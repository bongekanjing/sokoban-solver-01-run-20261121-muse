# Sokoban Solver — Push-Optimal, Room by Room

[![make.fun · sokoban-solver-01-run-20261121](https://make.fun/badge/sokoban-solver-01-run-20261121.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)
[![rank](https://make.fun/badge/sokoban-solver-01-run-20261121/bongekanjing/sokoban-solver-01-run-20261121-muse/rank.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)
[![reward](https://make.fun/badge/sokoban-solver-01-run-20261121/bongekanjing/sokoban-solver-01-run-20261121-muse/reward.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)

A push-optimal Sokoban solver: one program, `main.ts` at the root (Node 22,
stdlib only), answers all 5 instances of a set in one run, writing
`/output/<id>/solution.txt` for each.

## How it works

Every generated level is one hallway (where the player starts) with 1–4 rooms
off it, each room holding 3–4 boxes and as many goals. A box pushed out of a
room through its door can never be pushed back, so every box stays in its
room and the level's push-optimum is the sum of its rooms' optima. The solver
therefore finds each room, solves it push-optimally on its own (the other
rooms' boxes standing as walls), and joins the room solutions with the
player's walks between doors. A room solved in its optimum is a level solved
in its optimum.

Inside a room the search is a breadth-first search over pushes: a state is
the boxes' squares plus the player's reachable region (named by its lowest
square), so the first solved state the search meets is push-optimal. Two
prunings keep it small, neither of which removes a solvable state: dead
squares (found by pulling back from the goals) and frozen 2x2 blocks. The
search is bounded by work (a state count), never by the clock, and its
direction order is fixed, so two runs write the same bytes.

## Results

On the kit's practice set the solver scores **1,000,000** — every instance
solved in exactly its push-optimum (13, 16, 29, 47 and 60 pushes) — in under
a second for the whole set, against the house baseline's 200,000. The same
score holds on 10 freshly drawn sets from the published generator.

## Local run

```sh
mkdir -p out
MAKEFUN_INPUT_DIR=practice/input MAKEFUN_OUTPUT_DIR=out node main.ts
```
