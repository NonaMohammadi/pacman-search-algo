# Pacman Search Algorithms

Implementation of the search algorithms and search problems from UC Berkeley's [CS188 Pacman AI projects](http://ai.berkeley.edu). Pacman uses these algorithms to navigate mazes, visit all four corners, and eat all the food dots efficiently.

## What's Implemented

### `search.py` — Core Search Algorithms

- **Depth-First Search (`depthFirstSearch`)** — Explores as deep as possible along each branch using a stack (LIFO) frontier before backtracking.
- **Breadth-First Search (`breadthFirstSearch`)** — Explores all nodes at the current depth before moving deeper, using a queue (FIFO) frontier. Finds the shortest path in terms of number of steps.
- **A\* Search (`aStarSearch`)** — Uses a priority queue ordered by `cost so far + heuristic estimate to goal`. Finds the optimal (lowest-cost) path when the heuristic is admissible.

All three algorithms share the same shape: track visited states, expand a state's neighbors via `problem.expand()`, and return the list of actions once a goal state is reached.

### `searchAgents.py` — Search Problems & Heuristics

- **`PositionSearchProblem`** — Find a path from Pacman's start to a single fixed goal position.
- **`CornersProblem`** — Find the shortest path that touches all four corners of the maze. The search state tracks both Pacman's position *and* which corners have been visited so far, as `(position, (visited_NW, visited_NE, visited_SW, visited_SE))`.
- **`cornersHeuristic`** — Admissible heuristic for `CornersProblem`: the maximum Manhattan distance from the current position to any unvisited corner.
- **`FoodSearchProblem`** — Find the shortest path that eats all food dots in the maze. State is `(position, remaining_food_grid)`.
- **`foodHeuristic`** — Admissible heuristic for `FoodSearchProblem`: the maximum Manhattan distance from Pacman's position to any remaining food dot.
- **`ClosestDotSearchAgent`** — A greedy strategy that repeatedly uses BFS to find and eat the single closest dot until none remain (not guaranteed optimal, but fast).
- **`AnyFoodSearchProblem`** — Helper problem used by `ClosestDotSearchAgent`; goal test succeeds as soon as Pacman reaches *any* cell containing food.

## Running It

This project runs on top of the Berkeley Pacman game engine (`pacman.py`, `game.py`, `util.py`, etc. — not included in this repo, available from the [Berkeley AI course site](http://ai.berkeley.edu/search.html)).

Example commands once the full project files are in place:

```bash
# Depth-first search to a fixed goal
python pacman.py -l tinyMaze -p SearchAgent -a fn=depthFirstSearch

# Breadth-first search
python pacman.py -l mediumMaze -p SearchAgent -a fn=breadthFirstSearch

# A* with the Manhattan distance heuristic
python pacman.py -l bigMaze -z .5 -p SearchAgent -a fn=aStarSearch,heuristic=manhattanHeuristic

# A* visiting all four corners
python pacman.py -l mediumCorners -p AStarCornersAgent -z 0.5

# A* eating all the food
python pacman.py -l trickySearch -p AStarFoodSearchAgent
```

## Notes

- All algorithms perform **graph search** (they track visited states to avoid re-expanding them), not tree search.
- `foodHeuristic` and `cornersHeuristic` are both admissible (never overestimate the true cost), which is what makes A* optimal here.
- This is coursework based on UC Berkeley's CS188 Pacman projects — see the license headers in `search.py` / `searchAgents.py` for attribution and usage terms.
