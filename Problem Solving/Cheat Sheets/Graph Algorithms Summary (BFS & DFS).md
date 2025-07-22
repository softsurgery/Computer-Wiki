This is a summary of graph algorithms, focusing on BFS, DFS, shortest path strategies, and example code in Python.

---
## 1. Breadth-First Search (BFS)

**Definition**:  
BFS explores a graph level by level using a queue (FIFO). It starts at the source node and visits all neighbors before going deeper.

**Key Properties**:

- Guarantees the shortest path in unweighted graphs.
- Uses a queue to keep track of nodes to explore.
- Tracks visited nodes to avoid cycles.

**Use Cases**:

- Shortest path finding (unweighted)
- Network traversal
- Solving mazes
- Finding connected components

**Code (Simple BFS Traversal)**:

```python
from collections import deque

def bfs(graph, start):     
	visited = set()     
	queue = deque([start])
	while queue:
		node = queue.popleft()         
		if node not in visited:
			print(node)             
			visited.add(node)             
			queue.extend(neighbor for neighbor in graph[node] if neighbor not in visited)
```
---

## 2. BFS to Find the Shortest Path

To get not just the number of steps but the actual path, track the "parent" of each node when it's first discovered.

**Code (Shortest Path with BFS)**:

```python
from collections import deque

def bfs_shortest_path(graph, start, goal):
	queue = deque([start])     
	parent = {start: None}      
	while queue:         
		current = queue.popleft()          
		if current == goal:             
		# Reconstruct path from goal to start
			path = []             
			while current is not None:
				path.append(current)
				current = parent[current]             
				return path[::-1]          
		for neighbor in graph[current]:
			if neighbor not in parent:                 
				parent[neighbor] = current                 
				queue.append(neighbor)      
	return None  # No path found

```
---

## 3. Depth-First Search (DFS)

**Definition**:  
DFS goes as deep as possible down one path before backtracking. It uses a stack (or recursion).

**Key Properties**:

- Does NOT guarantee shortest path.
- Great for full exploration and puzzle solving.
- Useful for detecting cycles, checking connectivity, and topological sorting.

**Code (DFS to Find One Path)**:


```python
def dfs(graph, start, goal, path=None, visited=None):
	if visited is None:
		visited = set()     
	if path is None:
		path = []      
	visited.add(start)
	path.append(start)      
	
	if start == goal:         
		return path      
	for neighbor in graph[start]:
		if neighbor not in visited:
			result = dfs(graph, neighbor, goal, path, visited)
			if result:                 
				return result      
	path.pop()     
	return None
```
---

## 4. BFS in 2D Grids or Mazes

Each cell is treated like a graph node. Use BFS to find the shortest path in a grid maze.

**Code (BFS in Grid Maze)**:

```python
from collections import deque

def min_steps_in_maze(maze):
	rows, cols = len(maze), len(maze[0])     
	directions = [(-1,0), (1,0), (0,-1), (0,1)]
	queue = deque([(0, 0, 0)])     
	visited = set([(0, 0)])      
	while queue:
		x, y, steps = queue.popleft()
		if (x, y) == (rows-1, cols-1):
			return steps          
		for dx, dy in directions:             
			nx, ny = x+dx, y+dy
			if 0 <= nx < rows and 0 <= ny < cols and maze[nx][ny] == 0:
				if (nx, ny) not in visited:
					visited.add((nx, ny))
					queue.append((nx, ny, steps + 1))      
	return -1
```

Note:
- 0 = open cell
- 1 = wall
- BFS guarantees the shortest number of steps

---

## 5. BFS vs DFS (Comparison)

|Feature|BFS|DFS|
|---|---|---|
|Search Order|Level by level|Deep path first|
|Uses|Queue (FIFO)|Stack or recursion|
|Shortest Path|Yes (in unweighted graphs)|No|
|Memory Use|Higher (wide graphs)|Lower (deep graphs)|
|Common Uses|Routing, AI, pathfinding|Puzzles, connectivity, cycles|

---

## 6. Disconnected Graphs

- If the graph is disconnected, BFS or DFS may **not reach** all nodes.
- If the goal is not reachable, the function will return `None` or `-1` depending on how you write it.
- Always track visited nodes to avoid infinite loops.

---

## 7. Other Graph Algorithms

- Dijkstra’s Algorithm — Shortest path with weights (no negative weights)    
- Bellman-Ford — Handles negative weights
- Floyd-Warshall — All pairs shortest paths
- A* Search — Best path using heuristics
- Prim’s and Kruskal’s Algorithms — Minimum spanning tree
- Topological Sort — For scheduling tasks (DAGs)
- Tarjan’s and Kosaraju’s — Strongly connected components
- Union-Find / DSU — Cycle detection and component tracking

---

## Summary

- Use **BFS** when you need the **shortest path** in **unweighted graphs**.
- Use **DFS** when you want to explore all paths, detect cycles, or solve puzzles.
- Track parent links in BFS to reconstruct the actual shortest path.
- For grids and mazes, treat each cell as a graph node and use BFS to find the shortest path.
- Use DFS for deeper structural analysis like cycles, SCCs, and topological order.

