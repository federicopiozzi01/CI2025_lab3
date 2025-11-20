# CI2025_lab3 Federico Piozzi

## Problem Description
The goal of this project is to solve a path-finding problem. The challenge involves finding the shortest path between two nodes in various configurations, specifically addressing two scenarios:
1. **Positive Weights:** Standard shortest path problem.
2. **Negative Weights:** Handling graphs where edge weights can be negative (due to noise), which introduces the risk of negative cycles.

The project requires implementing search algorithms to find the shortest path (if it exists and is positive without loops) and analyzing performance across different problem sizes (`size`), densities (`density`), and noise levels (`noise_level`).

## Implementation Details

### 1. Problem Generator Modification
To enable **Informed Search (A*)**, the standard problem generator was modified to return the **coordinates (x, y)** of the cities alongside the adjacency matrix. Without coordinates, it is impossible to compute a spatial heuristic (like Euclidean distance), forcing any algorithm to degrade to Uninformed Search (Dijkstra), which is less efficient for this specific spatial problem.

### 2. Algorithms Used

#### A. Informed Search: A* (A-Star)
Used when `negative_values = False`. As heuristic I use **Euclidean Distance** scaled by 1000. Since edge weights are defined as `EuclideanDist + Noise`, the Euclidean distance is a consistent and admissible heuristic (never overestimates the true cost if noise $\ge$ 0). The problem generator scales weights by 1000 and rounds them to integers, for this reason the heuristic function was also scaled by 1000 to remain admissible and consistent with the edge weights. Compared with Dijkstra (same implementation of A* but with heuristic_function=0) A* educes the number of explored nodes ensuring optimal performance on large graphs.

#### B. Bellman-Ford
Used when `negative_values = True`. I used Bellman-Ford because A* and Dijkstra cannot handle negative edge weights reliably. Bellman-Ford is necessary to correctly calculate paths with negative edges and also to detect negative cycle. It is computationally expensive making it significantly slower than both A* and Dijkstra for large instances, as shown in the performance analysis.

### 3. Methodology & Experiments
The solution was tested against a grid of hyperparameters:
* Sizes: `[10, 20, 50, 100, 200, 500, 1000]`
* Densities: `[0.2, 0.5, 0.8]`
* Noise: `[0.0, 0.1, 0.5]`
* Negative Values: `[False, True]`

**Performance Metrics:**
* Execution Time: Measured using `time.perf_counter()`.
* Success Rate: Percentage of valid paths found in negative scenarios (filtering out negative cycles).
* Explored Nodes: Comparison between A* and Dijkstra to demonstrate heuristic efficiency.

I have also shown some result just to compare the cost values in some of the test. As we can see, for istance, the cost of A* and Dijkstra are equal, what is different is that Dijkstra explore way more nodes then A* (see graphs).

Due to the extensive execution time required to process all possible combinations, I opted to test only 5 random pairs of points for each configuration. This approach was chosen to keep the total runtime within manageable limits.

