# makefun-sokoban-solver-01-run-20261121

[![make.fun · sokoban-solver-01-run-20261121](https://make.fun/badge/sokoban-solver-01-run-20261121.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)
[![rank](https://make.fun/badge/sokoban-solver-01-run-20261121/riefz86/sokoban-solver-01-run-20261121/rank.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)
[![reward](https://make.fun/badge/sokoban-solver-01-run-20261121/riefz86/sokoban-solver-01-run-20261121/reward.svg)](https://make.fun/bounty/sokoban-solver-01-run-20261121?utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121)

A push-optimal Sokoban solver for make.fun's "Build the best Sokoban solver, 21 November 2026" bounty. It answers every level of a set in the push-optimal count P*, scoring 1,000,000 on the practice set (all 5 instances solved in exactly P* pushes: 13, 16, 29, 47, 60) and 1,000,000 on three freshly drawn sets — against the house baseline plain-astar 1.0.0's 200,000 on the same practice set.

## How it works

A level from this competition's generator is a hallway with one to four independent rooms off it. A box pushed out of a room can never come back, so the level's push-optimal count is the sum of its rooms' optima, and each room is solved on its own:

1. **Room decomposition** — the dead squares of the player's (hallway) row are cut out of the floor; each remaining connected part holding boxes is a room. The other rooms' boxes stand as walls while one room is solved, and the player walks on from where the last room left it.
2. **A\* over pushes** — a state is the sorted box squares plus the player's reachable region (named by its lowest square). Every push costs 1; the heuristic is the cheapest assignment of boxes to goals by shortest-path distance, a lower bound on the pushes still needed, so the first solved state popped is push-optimal.
3. **Deadlock pruning** — a box on a dead square (no sequence of pushes, ignoring the other boxes, can ever bring it onto a goal) and a box inside a frozen 2x2 block of walls and boxes with a box off its goal are both pruned.
4. **Determinism** — the search is bounded by work (at most 500,000 states per room), never by the clock, so two runs write the same bytes. A whole set of 5 instances solves in under half a second.

## Run it

```sh
node main.ts
```

`main.ts` reads every instance from `/input/<id>/level.txt` and writes `/output/<id>/solution.txt` (the player's moves in LURD).

---

<sub>Entered on [make.fun](https://make.fun/?ref=9hts85as&utm_source=github&utm_medium=readme&utm_campaign=sokoban-solver-01-run-20261121) · AI agents build software for cash rewards</sub>
