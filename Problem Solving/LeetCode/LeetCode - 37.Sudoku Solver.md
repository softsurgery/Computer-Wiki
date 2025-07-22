```python
class Solution(object):
    def solveSudoku(self, board):
        from collections import defaultdict

        rows = [set() for _ in range(9)]
        cols = [set() for _ in range(9)]
        boxes = [set() for _ in range(9)]

        # Fill in existing digits
        for i in range(9):
            for j in range(9):
                val = board[i][j]
                if val != '.':
                    rows[i].add(val)
                    cols[j].add(val)
                    boxes[(i//3)*3 + (j//3)].add(val)

        def backtrack():
            for i in range(9):
                for j in range(9):
                    if board[i][j] == '.':
                        for d in map(str, range(1, 10)):
                            box_idx = (i//3)*3 + (j//3)
                            if d not in rows[i] and d not in cols[j] and d not in boxes[box_idx]:
                                board[i][j] = d
                                rows[i].add(d)
                                cols[j].add(d)
                                boxes[box_idx].add(d)

                                if backtrack():
                                    return True

                                # backtrack
                                board[i][j] = '.'
                                rows[i].remove(d)
                                cols[j].remove(d)
                                boxes[box_idx].remove(d)
                        return False
            return True

        backtrack()

```

```python
class Solution(object):
    opt = [(0, 0), (0, 3), (0, 6), (3, 0), (3, 3), (3, 6), (6, 0), (6, 3), (6, 6)]

    def solveSudoku(self, board):
        rows = {}
        cols = {}
        cubes = {}
        for i in range(9):
            row = [x for x in board[i] if x != "."]
            col = [board[x][i] for x in range(9) if board[x][i] != "."]

            mr = self.opt[i][0]
            mc = self.opt[i][1]
            cube = [
                board[x][y]
                for x in range(mr, mr + 3)
                for y in range(mc, mc + 3)
                if board[x][y] != "."
            ]

            rows[str(i)] = [x for x in range(1, 10) if str(x) not in row]
            cols[str(i)] = [x for x in range(1, 10) if str(x) not in col]
            cubes[str(i)] = [x for x in range(1, 10) if str(x) not in cube]

```