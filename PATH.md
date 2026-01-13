The algorithm keeps track of different paths using **Breadth-First Search (BFS) with path history**. Here's how it works:

## Core Data Structures

### 1. **Queue (`queue.Queue()`)**

Stores tuples of `(current_position, path_so_far)`

```python
# Queue contains both current position AND the path that got us there
q.put((start_pos, [start_pos]))
# Example queue state might be:
# [
#   ((1,2), [(0,0), (1,1), (1,2)]),
#   ((2,1), [(0,0), (1,1), (2,1)]),
#   ((0,1), [(0,0), (0,1)])
# ]
```

### 2. **Visited Set (`visited`)**

Tracks which positions have been visited to avoid cycles

```python
visited = set()
# Example: {(1,1), (0,1), (1,2)}
```

## How Different Paths Are Maintained

### Initial State:

```python
Start: O at (0,5)
Queue: [((0,5), [(0,5)])]
Visited: {(0,5)}
```

### Step-by-Step Process:

#### **Step 1:**

- Pop from queue: `current_pos = (0,5)`, `path = [(0,5)]`
- Check neighbors: (1,5), (0,4), (0,6) [with bounds checking]
- For each valid neighbor:
  - Create **new path** = old path + [neighbor]
  - Enqueue `(neighbor, new_path)`

```python
Queue becomes:
[
  ((1,5), [(0,5), (1,5)]),
  ((0,4), [(0,5), (0,4)]),
  ((0,6), [(0,5), (0,6)])
]
Visited: {(0,5), (1,5), (0,4), (0,6)}
```

#### **Step 2:**

- Pop: `((1,5), [(0,5), (1,5)])`
- Check neighbors: (2,5), (0,5), (1,4), (1,6)
- Filter out visited (0,5) and walls
- Create new paths:
  ```python
  Path to (2,5): [(0,5), (1,5), (2,5)]
  Path to (1,4): [(0,5), (1,5), (1,4)]
  ```

Queue now contains **4 different paths** exploring different directions!

## Visual Example

Consider this simplified maze:

```
O # #
  # X
```

### Path Exploration:

```
Initial: Only O at (0,0)
Queue: [Path A: [(0,0)]]

Step 1: From (0,0), can go right or down
Queue: [
  Path B: [(0,0), (0,1)],  # Right
  Path C: [(0,0), (1,0)]   # Down
]

Step 2: Process Path B from (0,1) - can only go down
Queue: [
  Path C: [(0,0), (1,0)],    # Still waiting
  Path D: [(0,0), (0,1), (1,1)]  # New path
]

Step 3: Process Path C from (1,0) - can only go right
Queue: [
  Path D: [(0,0), (0,1), (1,1)],  # Still waiting
  Path E: [(0,0), (1,0), (1,1)]   # New path
]
```

**Notice**: Both Path D and Path E reach (1,1) but through different routes!

## Key Code Segment Explained

```python
def find_path(maze, stdscr):
    start_pos = find_start(maze, "O")
    q = queue.Queue()

    # Each queue entry contains: (position, path_to_that_position)
    q.put((start_pos, [start_pos]))  # Initial path

    visited = set([start_pos])  # Track visited positions

    while not q.empty():
        current_pos, path = q.get()  # Get next position and its path
        row, col = current_pos

        # If we found the end, return the complete path
        if maze[row][col] == "X":
            return path  # This is one complete path from O to X

        # Explore neighbors
        neighbors = find_neighbors(maze, row, col)

        for neighbor in neighbors:
            if neighbor in visited:
                continue  # Skip already visited positions

            r, c = neighbor
            if maze[r][c] == "#":
                continue  # Skip walls

            # Create NEW PATH by appending neighbor to current path
            new_path = path + [neighbor]

            # Enqueue both the neighbor and the NEW path
            q.put((neighbor, new_path))

            # Mark as visited (so other paths don't revisit)
            visited.add(neighbor)
```

## Why This Works for Finding Shortest Path

### BFS Property:

- Explores all paths **level by level** (distance from start)
- First path to reach the destination is guaranteed to be the shortest

### Example:

```
Queue after 2 steps:
[
  Path length 3: [(0,0), (1,0), (2,0)],
  Path length 3: [(0,0), (0,1), (1,1)],
  Path length 4: [(0,0), (1,0), (2,0), (3,0)],  # Not processed yet
  Path length 4: [(0,0), (0,1), (1,1), (2,1)]   # Not processed yet
]
```

All paths of length N are processed before paths of length N+1.

## Memory Consideration

Each path is stored **independently** in memory. For a maze with many branches, this can be memory-intensive. Alternatives:

```python
# Option 1: Store only parent references (more memory efficient)
parent_map = {start_pos: None}
# Reconstruct path by following parents back from end to start

# Option 2: Store distances instead of full paths
distances = {start_pos: 0}
# Then reconstruct using BFS tree
```

## Visualization in Your Code

The visualization shows **only one path at a time** (the current path being explored), but the algorithm is exploring multiple paths in parallel via the queue.

```
Frame 1: Shows path [(0,5)]
Frame 2: Shows path [(0,5), (1,5)]
Frame 3: Shows path [(0,5), (0,4)]
Frame 4: Shows path [(0,5), (0,6)]
Frame 5: Shows path [(0,5), (1,5), (2,5)]
... and so on
```

Each frame shows a **different complete path** from start to the current position, representing one of the many paths being explored simultaneously.

## Summary

The algorithm maintains different paths by:

1. **Storing full path history** with each queue entry
2. **Creating new path copies** when branching occurs
3. **Using a queue** to ensure level-order exploration
4. **Tracking visited nodes** to avoid revisiting and cycles
5. **Each path is independent** in memory until it's either completed (reaches X) or abandoned (hits dead end)

This approach explores **all possible paths** in parallel but only returns the first complete path found (which, due to BFS, is guaranteed to be the shortest).
