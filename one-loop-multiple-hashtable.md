This a common situation that we need to handle complex situation for an array with one or multiple dimensions.

One way to solve this problem is to use multiple hashtables

Determine if a 9x9 Sudoku board is valid. Only the filled cells need to be validated **according to the following rules**:

1. Each row must contain the digits 
   `1-9`
   without repetition.
2. Each column must contain the digits 
   `1-9`
    without repetition.
3. Each of the 9
   `3x3`
   sub-boxes of the grid must contain the digits 
   `1-9`
    without repetition.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)  
A partially filled sudoku which is valid.

The Sudoku board could be partially filled, where empty cells are filled with the character`'.'`.

**Example 1:**

```
Input:

[
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]

Output:
 true
```

**Example 2:**

```
Input:

[
  ["8","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]

Output:
 false

Explanation:
 Same as Example 1, except with the 
5
 in the top left corner being 
    modified to 
8
. Since there are two 8's in the top left 3x3 sub-box, it is invalid.
```

**Note:**

* A Sudoku board \(partially filled\) could be valid but is not necessarily solvable.
* Only the filled cells need to be validated according to the mentioned rules.
* The given board contain only digits
  `1-9`
  and the character
  `'.'`
  .
* The given board size is always
  `9x9`
  .

```py
class Solution:
    def isValidSudoku(self, board):
        """
        :type board: List[List[str]]
        :rtype: bool
        """
        dic_row = [{},{},{},{},{},{},{},{},{}]
        dic_col = [{},{},{},{},{},{},{},{},{}]
        dic_box = [{},{},{},{},{},{},{},{},{}]

        for i in range(len(board)):
            for j in range(len(board)):
                num = board[i][j]
                if num == ".":
                    continue
                if num not in dic_row[i] and num not in dic_col[j] and num not in dic_box[3*(i//3)+(j//3)]:
                    dic_row[i][num] = 1
                    dic_col[j][num] = 1
                    dic_box[3*(i//3)+(j//3)][num] = 1
                else:
                    return False

        return True
            
```









