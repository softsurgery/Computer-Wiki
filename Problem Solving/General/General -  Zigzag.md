```python
class Solution(object):
    def convert(self, s, numRows):
        if numRows == 1 or numRows >= len(s):
            return s

        rows = [''] * numRows
        curr_row = 0
        going_down = False

        for c in s:
            rows[curr_row] += c
            if curr_row == 0 or curr_row == numRows - 1:
                going_down = not going_down
            curr_row += 1 if going_down else -1

        return ''.join(rows)

# Example usage
s = Solution()
print(s.convert("PAYPALISHIRING", 4))  # Output: "PINALSIGYAHRPI"

```

```js
/**
 * @param {string} s
 * @param {number} numRows
 * @return {string}
 */
var convert = (s, numRows) => {
        if (numRows === 1 || numRows >= s.length) {
            return s;
        }

        const rows = new Array(numRows).fill('');
        let currRow = 0;
        let goingDown = false;

        for (let c of s) {
            rows[currRow] += c;
            if (currRow === 0 || currRow === numRows - 1) {
                goingDown = !goingDown;
            }
            currRow += goingDown ? 1 : -1;
        }

        return rows.join('');
    }
```