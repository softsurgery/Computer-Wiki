```python
class Solution(object):
	opt = [(0,0),(0,3),(0,6),(3,0),(3,3),(3,6),(6,0),(6,3),(6,6)]
	def isValidSudoku(self, board):
		for i in range(9):
			row = [x for x in board[i] if x!='.']
			if len(set(row))!=len(row):
				return False
			col = [board[x][i] for x in range(9) if board[x][i]!='.']
			if len(set(col))!=len(col):
				return False
			mr = self.opt[i][0]
			mc = self.opt[i][1]
			elem = [board[x][y] for x in range(mr,mr+3) for y in range(mc,mc+3) if board[x][y]!='.']
			if len(set(elem))!=len(elem):
				return False
		return True
```