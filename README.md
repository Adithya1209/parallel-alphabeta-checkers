# Parallel Minimax Checkers Engine

A high-performance command-line Checkers (English Draughts) engine built in Python. This project implements the **Minimax algorithm with Alpha-Beta pruning** and utilizes **Python's `multiprocessing`** to parallelize the search tree, significantly reducing move calculation time on multi-core systems.

##  Key Features

* **Parallelized Search Tree:** Uses `multiprocessing.Pool` to distribute the evaluation of the root node's children across all available CPU cores.
* **Alpha-Beta Pruning:** Optimizes the Minimax algorithm to ignore branches that cannot possibly influence the final decision.
* **Heuristic Evaluation:** Custom scoring function based on:
    * Piece difference.
    * Positional advantages (center control, safe edges).
    * Defensive formations.
* **Benchmarks:** Includes both a serial (`checkers_original.py`) and parallel (`checkers_parallel.py`) implementation for performance benchmarking.

##  How It Works

### The Algorithm
The engine looks 4 moves ahead (depth=4) to evaluate the board state.
1.  **Minimax:** Recursively simulates all possible future moves to minimize the maximum possible loss.
2.  **Alpha-Beta Pruning:** Stops evaluating a move as soon as at least one possibility is found that proves the move to be worse than a previously examined move.

### Parallelization Strategy
In the serial version, the engine evaluates every potential root move sequentially. In the parallel version, the engine detects the number of available CPU cores and maps the root moves (the first layer of the decision tree) to a worker pool.

```python
# Snippet from checkers_parallel.py
pool = mp.Pool(mp.cpu_count())
# Distribute the minimax evaluation of first-level moves across cores
p = pool.map(worker_function, first_computer_moves)
