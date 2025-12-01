# 回溯算法

其实回溯算法和我们常说的 DFS 算法基本可以认为是同一种算法。抽象地说，解决一个回溯问题，实际上就是遍历一棵决策树的过程，树的每个叶子节点存放着一个合法答案。你把整棵树遍历一遍，把叶子节点上的答案都收集起来，就能得到所有的合法答案

站在回溯树的一个节点上，你只需要思考 3 个问题：

1. 路径：也就是已经做出的选择
1. 选择列表：也就是你当前可以做的选择
1. 结束条件：也就是到达决策树底层，无法再做选择的条件

## 算法框架

其核心就是 for 循环里面的递归，在递归调用之前「做选择」，在递归调用之后「撤销选择」

```python
result = []
def backtrack(路径, 选择列表):
    if 满足结束条件:
        result.add(路径)
        return
    for 选择 in 选择列表:
        做选择
        backtrack(路径, 选择列表)
        撤销选择
```

写 backtrack 函数时，需要维护走过的「路径」和当前可以做的「选择列表」，当触发「结束条件」时，将「路径」记入结果集

```python
for 选择 in 选择列表:
    # 做选择
    将该选择从选择列表移除
    路径.add(选择)
    backtrack(路径, 选择列表)
    # 撤销选择
    路径.remove(选择)
    将该选择再加入选择列表
```

举例：全排列

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251201171923474.jpg)

## 与DFS的关联

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251201172046695.jpg)

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251201172125543.jpg)

## 例题

### N位二进制数的所有可能

这道题可以认为是数独游戏和 N 皇后问题的简化版：

- 这道题相当于让你对一个长度为 n 的一维数组中的每一个位置进行穷举，其中每个位置可以填 0 或 1
- 数独游戏相当于让你对一个 9x9 的二维数组中的每个位置进行穷举，其中每个位置可以是数字 1~9，且同一行、同一列和同一个 3x3 的九宫格内数字不能重复
- N 皇后问题相当于让你对一个 N x N 的二维数组中的每个位置进行穷举，其中每个位置可以不放皇后或者放置皇后（相当于 0 或 1），且不能存在多个皇后在同一行、同一列或同一对角线上

看到这种题目，脑海中应该立刻出现一棵递归树；有了这棵树，你就有办法使用回溯算法框架 无遗漏地穷举所有可能的解；能无遗漏地穷举所有解，就一定能找到符合题目要求的答案

递归树的节点就是穷举过程中产生的状态，以不同的视角来看这些节点，会有不同的作用。具体来说就是以下问题：

应该如何理解这棵树每一层的节点；应该如何理解这棵树每一列（每条树枝）的节点；应该如何理解每个节点本身？

1. 从层的角度来看，第 i 层的节点表示的就是长度为 i 的所有二进制数（i 从 0 开始算），比如第 2 层的节点分别是 00、01、10、11，这些就是长度为 2 的所有二进制数
1. 从列（树枝）的角度来看，每条树枝上的节点记录了状态转移的过程。比如 001 这个节点，即首先选择 0，然后选择 0，最后选择 1
1. 从节点的角度来看，每个节点都是一个状态，从该节点向下延伸的树枝，就是当前状态的所有可能的选择。比如第二层的节点 01，它的前两位已经确定，现在要对第三位进行穷举。第三位可以是 0 或 1，所以从 01 这个节点向下延伸的两个子节点就是 010 和 011

```go
func generateBinaryNumber(n int) []string {
    res := []string{}
    // 回溯路径
    track := []byte{}

    // 回溯算法核心框架
    var backtrack func(i int)
    backtrack = func(i int) {
        if i == n {
            // 当递归深度等于 n 时，说明找到一个长度为 n 的二进制数，结束递归
            res = append(res, string(track))
            return
        }

        // 选择有两种，0 或 1
        // 所以递归树是一棵二叉树
        for _, val := range []byte{'0', '1'} {
            // 做选择
            track = append(track, val)

            backtrack(i + 1)

            // 撤销选择
            track = track[:len(track)-1]
        }
    }

    backtrack(0)
    return res
}
```

### 数独

对于一个 MxN 的二维数组，可以把二维坐标 (i, j) 映射到一维坐标 index = i \* N + j，反之可以通过 i = index / N 和 j = index % N 得到原来的二维坐标。数独游戏的 9x9 二维数组就可以在逻辑上看做一个长度为 81 的一维数组，然后按照一维数组的递归树来穷举即可

我们应该可以发现一个有趣的现象：

- 按照人类的一般理解，规则越多，问题越难；但是对于计算机来说，规则越多，问题反而越简单
- 因为它要穷举嘛，规则越多，越容易剪枝，搜索空间就越小，就越容易找到答案。反之，如果没有任何规则，那就是纯暴力穷举，效率会非常低
- 我们对回溯算法的剪枝优化，本质上就是寻找规则，提前排除不合法的选择，从而提高穷举的效率

```go
func solveSudoku(board [][]byte) {
    // 标记是否已经找到可行解
    found := false
    // 路径：board 中小于 index 的位置所填的数字
    // 选择列表：数字 1~9
    // 结束条件：整个 board 都填满数字
    var backtrack func(index int)
    backtrack = func(index int) {
        if found {
            // 已经找到一个可行解，立即结束
            return
        }
        m, n := 9, 9
        i, j := index/n, index%n
        if index == m*n {
            // 找到一个可行解，触发 base case
            found = true
            return
        }
        if board[i][j] != '.' {
            // 如果有预设数字，不用我们穷举
            backtrack(index + 1)
            return
        }
        for ch := byte('1'); ch <= '9'; ch++ {
            // 剪枝：如果遇到不合法的数字，就跳过
            if !isValid(board, i, j, ch) {
                continue
            }
            // 做选择
            board[i][j] = ch
            backtrack(index + 1)
            if found {
                // 如果找到一个可行解，立即结束
                // 不要撤销选择，否则 board[i][j] 会被重置为 '.'
                return
            }
            // 撤销选择
            board[i][j] = '.'
        }
    }
    backtrack(0)
}

// 判断是否可以在 (r, c) 位置放置数字 num
func isValid(board [][]byte, r, c int, num byte) bool {
    for i := 0; i < 9; i++ {
        // 判断行是否存在重复
        if board[r][i] == num {
            return false
        }
        // 判断列是否存在重复
        if board[i][c] == num {
            return false
        }
        // 判断 3 x 3 方框是否存在重复
        if board[(r/3)*3+i/3][(c/3)*3+i%3] == num {
            return false
        }
    }
    return true
}
```

### N皇后

按理说可以直接参照数独游戏的代码实现 N 皇后问题
但 N 皇后问题相对数独游戏还有一个优化：我们可以以行为单位进行穷举，而不是像数独游戏那样以格子为单位穷举

举个直观的例子，在数独游戏中，如果我们设置 board[i][j] = 1，接下来呢，要去穷举 board[i][j+1] 了对吧？而对于 N 皇后问题，我们知道每行必然有且只有一个皇后，所以如果我们决定在 board[i] 这一行的某一列放置皇后，那么接下来就不用管 board[i] 这一行了，应该考虑 board[i+1] 这一行的皇后要放在哪里

所以 N 皇后问题的穷举对象是棋盘中的行，每一行都持有一个皇后，可以选择把皇后放在该行的任意一列

```go
var res [][]string

// 输入棋盘边长 n，返回所有合法的放置
func solveNQueens(n int) [][]string {
    res = [][]string{}
    // '.' 表示空，'Q' 表示皇后，初始化空棋盘
    board := make([]string, n)
    for i := 0; i < n; i++ {
        board[i] = strings.Repeat(".", n)
    }
    backtrack(board, 0)
    return res
}

// 路径：board 中小于 row 的那些行都已经成功放置了皇后
// 选择列表：第 row 行的所有列都是放置皇后的选择
// 结束条件：row 超过 board 的最后一行
func backtrack(board []string, row int) {
    // 触发结束条件
    if row == len(board) {
        temp := make([]string, len(board))
        copy(temp, board)
        res = append(res, temp)
        return
    }
    n := len(board[row])
    for col := 0; col < n; col++ {
        // 排除不合法选择
        if !isValid(board, row, col) {
            continue
        }
        // 做选择
        newRow := []rune(board[row])
        newRow[col] = 'Q'
        board[row] = string(newRow)
        // 进入下一行决策
        backtrack(board, row + 1)
        // 撤销选择
        newRow[col] = '.'
        board[row] = string(newRow)
    }
}

// 是否可以在 board[row][col] 放置皇后？
func isValid(board []string, row int, col int) bool {
    n := len(board)
    // 检查列是否有皇后互相冲突
    for i := 0; i < row; i++ {
        if board[i][col] == 'Q' {
            return false
        }
    }
    // 检查右上方是否有皇后互相冲突
    for i, j := row-1, col+1; i >= 0 && j < n; i, j = i-1, j+1 {
        if board[i][j] == 'Q' {
            return false
        }
    }
    // 检查左上方是否有皇后互相冲突
    for i, j := row-1, col-1; i >= 0 && j >= 0; i, j = i-1, j-1 {
        if board[i][j] == 'Q' {
            return false
        }
    }
    return true
}
```
