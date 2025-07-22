```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.word = None  # Stores word at the end node


class Solution(object):
    def findWords(self, board, words):
        # Step 1: Build Trie
        root = TrieNode()
        for word in words:
            node = root
            for char in word:
                if char not in node.children:
                    node.children[char] = TrieNode()
                node = node.children[char]
            node.word = word  # Store complete word at the end node

        rows, cols = len(board), len(board[0])
        result = []

        # Step 2: DFS from each cell
        def dfs(x, y, node):
            char = board[x][y]
            if char not in node.children:
                return

            next_node = node.children[char]
            if next_node.word:
                result.append(next_node.word)
                next_node.word = None  # Avoid duplicates

            board[x][y] = "#"  # Mark visited
            for dx, dy in [(-1, 0), (1, 0), (0, -1), (0, 1)]:  # 4 directions
                nx, ny = x + dx, y + dy
                if 0 <= nx < rows and 0 <= ny < cols and board[nx][ny] != "#":
                    dfs(nx, ny, next_node)
            board[x][y] = char  # Restore

        for i in range(rows):
            for j in range(cols):
                dfs(i, j, root)

        return result

```