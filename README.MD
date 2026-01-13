# Maze Pathfinder - Console Visualization

A Python maze solver with real-time visualization using the curses library. This program implements a Breadth-First Search (BFS) algorithm to find the shortest path through a maze, with animated step-by-step visualization.

## Features

- 🎮 **Real-time visualization** of the pathfinding process
- 🧠 **BFS algorithm** for finding the shortest path
- 🎨 **Color-coded display** with curses
- ⏱️ **Step-by-step animation** showing the search process
- 📍 **Visual markers** for start (O), end (X), and path (X in red)

## Prerequisites

- Python 3.x
- Standard library modules: `curses`, `queue`, `time`

## Installation

No additional installation required - uses only Python standard libraries:

```bash
# Clone or download the script
# No pip install needed
```

## Usage

Run the script with:

```bash
python maze_solver.py
```

### Controls

- The program runs automatically
- Watch the BFS algorithm explore the maze
- Red 'X' marks show the current search path
- Press any key at the end to exit

## Maze Representation

The maze is represented as a 2D grid with the following symbols:

| Symbol  | Meaning      | Color |
| ------- | ------------ | ----- |
| #       | Wall         | Blue  |
| O       | Start point  | Blue  |
| X       | End point    | Blue  |
| X (red) | Current path | Red   |
| Space   | Open path    | Blue  |

### Default Maze Structure

```
# # # # # O # # #
#               #
#   # #   # #   #
#   #       #   #
#   #   #   #   #
#   #   #   #   #
#   #   #   # # #
#               #
# # #   # # # X #
```

## Algorithm Details

### Breadth-First Search (BFS) Implementation

The program uses BFS to guarantee finding the shortest path:

1. **Start at 'O'** (orange start position)
2. **Explore neighbors** in all four directions (up, down, left, right)
3. **Queue-based traversal** ensures level-order exploration
4. **Visited set** prevents re-exploration
5. **Stop when 'X'** (exit) is found

### Key Functions

- `find_start(maze, start)`: Locates the starting position 'O'
- `find_neighbors(maze, row, col)`: Returns valid adjacent positions
- `find_path(maze, stdscr)`: Main BFS algorithm with visualization
- `print_maze(maze, stdscr, path=[])`: Displays maze with optional path overlay

## Customization

### Modify the Maze

Edit the `maze` variable in the script:

```python
maze = [
    ["#","O","#"],
    [" "," "," "],
    ["#","#","X"]
]
```

### Adjust Speed

Change the animation delay in `find_path()`:

```python
time.sleep(0.5)  # Slower animation (0.5 seconds per step)
time.sleep(0.1)  # Faster animation (0.1 seconds per step)
```

### Change Colors

Modify color pairs in `main()`:

```python
curses.init_pair(1, curses.COLOR_GREEN, curses.COLOR_BLACK)  # Change walls to green
curses.init_pair(2, curses.COLOR_YELLOW, curses.COLOR_BLACK) # Change path to yellow
```

## Terminal Requirements

- Minimum terminal size: **18 columns × 9 rows**
- For best results, use a terminal with at least 80×24 resolution
- Supports most modern terminals (Linux, macOS Terminal, Windows PowerShell/CMD with curses support)

## Common Issues and Solutions

### 1. Terminal Too Small

**Error**: Curses errors or maze not displaying properly
**Solution**:

- Resize terminal to at least 80×24
- Or reduce maze dimensions

### 2. Curses Not Available (Windows)

**Error**: `ModuleNotFoundError: No module named '_curses'`
**Solution**:

- Install `windows-curses`: `pip install windows-curses`
- Or use WSL/Linux subsystem on Windows

### 3. Maze Not Solving

**Possible causes**:

- No valid path exists (walls block the way)
- Start/end positions are walls
- Maze borders not properly enclosed

### 4. Animation Too Fast/Slow

**Adjust**: Change `time.sleep()` value in `find_path()` function

## Performance Notes

- BFS has O(n) time complexity where n = maze cells
- Each step includes screen refresh (slower but good for visualization)
- For larger mazes, consider reducing animation speed

## Extending the Project

### Add New Algorithms

Replace BFS with:

- Depth-First Search (DFS)
- A\* Search (with heuristics)
- Dijkstra's Algorithm

### Additional Features

1. **Multiple mazes**: Load from file
2. **Interactive mode**: Let user place walls
3. **Statistics**: Show steps taken, path length
4. **Different characters**: Use ASCII art for better visuals
5. **Sound effects**: Add beeps for steps

## Code Structure

```
maze_solver.py
├── Global Variables
│   └── maze (2D list)
├── Helper Functions
│   ├── find_start()
│   ├── print_maze()
│   └── find_neighbors()
├── Main Algorithm
│   └── find_path() - BFS implementation
└── Entry Point
    └── main() - Curses setup and execution
```

## Example Output

```
Step 1: Start at O
Step 2: Explore adjacent cells
Step 3: Expand frontier
...
Final: Red X's show shortest path from O to X
```

## Troubleshooting

### If nothing displays:

1. Check terminal size: `echo $COLUMNS x $LINES` (Linux/Mac)
2. Try simpler maze (3×3) to test
3. Run without wrapper: `python -c "import curses; print('Curses available')"`

### If path doesn't reach end:

1. Verify maze has valid path
2. Check that 'X' is not blocked by walls
3. Ensure border walls don't block movement

### If colors don't work:

1. Terminal might not support colors
2. Try basic version without colors first

## License

This project uses only Python standard libraries and is free to use for educational purposes.

## Contributing

Feel free to:

1. Report bugs or issues
2. Suggest new features
3. Submit pull requests with improvements
4. Share your modified versions

## Learning Resources

- [Python curses documentation](https://docs.python.org/3/howto/curses.html)
- [BFS Algorithm explanation](https://en.wikipedia.org/wiki/Breadth-first_search)
- [Maze generation algorithms](https://en.wikipedia.org/wiki/Maze_generation_algorithm)

---

**Note**: This is a console-based application. For best experience, run in a terminal with color support and adequate dimensions.
