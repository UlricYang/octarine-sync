# 深度优先搜索

## 回溯算法 V.S. DFS

```java
// 回溯算法框架模板
void backtrack(...) {
    if (reached the leaf node) {
        // 到达叶子节点，结束递归
        return;
    }
    for (int i = 0; i < n; i++;) {
        // 做选择
        ...
        backtrack(...)
        // 撤销选择
        ...
    }
}

// DFS 算法框架模板
void dfs(...) {
    if (reached the leaf node) {
        // 到达叶子节点，结束递归
        return;
    }
    // 做选择
    ...
    for (int i = 0; i < n; i++) {
        dfs(...)
    }
    // 撤销选择
    ...
}
```

它俩的本质是一样的，都是「遍历」思维下的暴力穷举算法。唯一的区别在于关注点不同，回溯算法的关注点在「树枝」，DFS 算法的关注点在「节点」

```java
void backtrack(Node root) {
    if (root == null) {
        return;
    }
    for (Node child : root.children) {
        // 做选择
        printf("我在 %s 和 %s 中间的树枝上做选择", root, child);
        // 进入child
        backtrack(child);
        // 撤销选择
        printf("我在 %s 和 %s 中间的树枝上撤销选择", root, child);
    }
}

void dfs(Node root) {
    if (root == null) {
        return;
    }
    // 做选择
    printf("我在 %s 节点上做选择", root);
    for (Node child : root.children) {
        dfs(child);
    }
    // 撤销选择
    printf("我在 %s 节点上撤销选择", root);
}

```

## backtrack/dfs/traverse 函数可以有返回值吗？

理论上你随意，但是我强烈建议，对于 backtrack/dfs/traverse 函数，就作为单纯的遍历函数，请保持 void 类型，不要给它们带返回值

递归算法主要有两种思维模式，一种是「遍历」的思维，一种是「分解问题」的思维：

- 分解问题的思维模式大概率是有返回值的，因为要根据子问题的结果来计算当前问题的结果。这种函数的名字我们一般也要取得有意义，比如 int maxDepth(TreeNode root)，一眼就能看出来这个函数的返回值是什么含义
- 遍历的思维模式，我们习惯上就会创建一个名为 backtrack/dfs/traverse 的函数，它本身只起到遍历多叉树的作用，简单明；如果再搅合进去一个返回值，意思是你这个函数有特殊的定义？那么请问你的思路到底是遍历还是分解问题？功力不到位的话，很容易把自己绕进去，写着写着自己都不知道这个返回值该咋用了

所有利用「遍历」思维的递归函数都是相同的道理，建议设置函数名为 traverse/dfs/backtrack，函数不要有返回值，通过外部变量来控制递归的终止

## base case 和剪枝应该写在哪里？

把能提到函数开头的判断逻辑都提到函数开头，因为递归部分是填写前中后序代码的位置，尽量不要和 base case 的逻辑混到一起，否则容易混乱。因为你剪枝减掉的就是无效的选择呀，所以放在做选择之前，就很合理

## 岛屿问题

岛屿系列题目的核心考点就是用 DFS/BFS 算法遍历二维数组

那么如何在二维矩阵中使用 DFS 搜索呢？如果你把二维矩阵中的每一个位置看做一个节点，这个节点的上下左右四个位置就是相邻节点，那么整个矩阵就可以抽象成一幅网状的「图」结构

```go
// 二叉树遍历框架
func traverse(root *TreeNode) {
    traverse(root.Left)
    traverse(root.Right)
}

// 二维矩阵遍历框架
func dfs(grid [][]int, i, j int, visited [][]bool) {
    m, n := len(grid), len(grid[0])
    if i < 0 || j < 0 || i >= m || j >= n {
        // 超出索引边界
        return
    }
    if visited[i][j] {
        // 已遍历过 (i, j)
        return
    }
    // 进入当前节点 (i, j)
    visited[i][j] = true
    // 进入相邻节点（四叉树）
    // 上
    dfs(grid, i - 1, j, visited)
    // 下
    dfs(grid, i + 1, j, visited)
    // 左
    dfs(grid, i, j - 1, visited)
    // 右
    dfs(grid, i, j + 1, visited)
}
```

一个处理二维数组的常用小技巧，你有时会看到使用「方向数组」来处理上下左右的遍历

```go
var dirs = [][]int{{-1, 0}, {1, 0}, {0, -1}, {0, 1}}

func dfs(grid [][]int, i int, j int, visited [][]bool) {
    m, n := len(grid), len(grid[0])
    if i < 0 || j < 0 || i >= m || j >= n {
        // 超出索引边界
        return
    }
    if visited[i][j] {
        // 已遍历过 (i, j)
        return
    }
    // 进入节点 (i, j)
    visited[i][j] = true
    // 递归遍历上下左右的节点
    for _, d := range dirs {
        next_i := i + d[0]
        next_j := j + d[1]
        dfs(grid, next_i, next_j, visited)
    }
    // 离开节点 (i, j)
}
```

### 岛屿数量

为什么每次遇到岛屿，都要用 DFS 算法把岛屿「淹了」呢？

主要是为了省事，避免维护 visited 数组。因为 dfs 函数遍历到值为 0 的位置会直接返回，所以只要把经过的位置都设置为 0，就可以起到不走回头路的作用（这类 DFS 算法还有个别名叫做 FloodFill 算法）

```go
func numIslands(grid [][]byte) int {
    res := 0
    m, n := len(grid), len(grid[0])
    // 遍历 grid
    for i := 0; i < m; i++ {
        for j := 0; j < n; j++ {
            if grid[i][j] == '1' {
                // 每发现一个岛屿，岛屿数量加一
                res++
                // 然后使用 DFS 将岛屿淹了
                dfs(grid, i, j)
            }
        }
    }
    return res
}

// 从 (i, j) 开始，将与之相邻的陆地都变成海水
func dfs(grid [][]byte, i, j int) {
    m, n := len(grid), len(grid[0])
    if i < 0 || j < 0 || i >= m || j >= n {
        // 超出索引边界
        return
    }
    if grid[i][j] == '0' {
        // 已经是海水了
        return
    }
    // 将 (i, j) 变成海水
    grid[i][j] = '0'
    // 淹没上下左右的陆地
    dfs(grid, i + 1, j)
    dfs(grid, i, j + 1)
    dfs(grid, i - 1, j)
    dfs(grid, i, j - 1)
}
```

### 封闭岛屿

如何判断「封闭岛屿」呢？

把上一题中那些靠边的岛屿排除掉，剩下的不就是「封闭岛屿」了

```go
// 主函数：计算封闭岛屿的数量
func closedIsland(grid [][]int) int {
    m, n := len(grid), len(grid[0])
    for j := 0; j < n; j++ {
        // 把靠上边的岛屿淹掉
        dfs(grid, 0, j)
        // 把靠下边的岛屿淹掉
        dfs(grid, m-1, j)
    }
    for i := 0; i < m; i++ {
        // 把靠左边的岛屿淹掉
        dfs(grid, i, 0)
        // 把靠右边的岛屿淹掉
        dfs(grid, i, n-1)
    }
    // 遍历 grid，剩下的岛屿都是封闭岛屿
    res := 0
    for i := 0; i < m; i++ {
        for j := 0; j < n; j++ {
            if grid[i][j] == 0 {
                res++
                dfs(grid, i, j)
            }
        }
    }
    return res
}

// 从 (i, j) 开始，将与之相邻的陆地都变成海水
func dfs(grid [][]int, i int, j int) {
    m, n := len(grid), len(grid[0])
    if i < 0 || j < 0 || i >= m || j >= n{
        return
    }
    if grid[i][j] == 1 {
        // 已经是海水了
        return
    }
    // 将 (i, j) 变成海水
    grid[i][j] = 1
    // 淹没上下左右的陆地
    dfs(grid, i+1, j)
    dfs(grid, i, j+1)
    dfs(grid, i-1, j)
    dfs(grid, i, j-1)
}
```

### 岛屿最大面积

dfs 函数淹没岛屿的同时，还应该想办法记录这个岛屿的面积

```go
func maxAreaOfIsland(grid [][]int) int {
	// 记录岛屿的最大面积
	var res int
	m := len(grid)
	n := len(grid[0])
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			if grid[i][j] == 1 {
				// 淹没岛屿，并更新最大岛屿面积
				res = max(res, dfs(grid, i, j))
			}
		}
	}
	return res
}

// 淹没与 (i, j) 相邻的陆地，并返回淹没的陆地面积
func dfs(grid [][]int, i, j int) int {
	m := len(grid)
	n := len(grid[0])
	if i < 0 || j < 0 || i >= m || j >= n {
		// 超出索引边界
		return 0
	}
	if grid[i][j] == 0 {
		// 已经是海水了
		return 0
	}
	// 将 (i, j) 变成海水
	grid[i][j] = 0

	return dfs(grid, i+1, j) +
		dfs(grid, i, j+1) +
		dfs(grid, i-1, j) +
		dfs(grid, i, j-1) + 1
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```

### 子岛屿数量

什么情况下 grid2 中的一个岛屿 B 是 grid1 中的一个岛屿 A 的子岛？

- 当岛屿 B 中所有陆地在岛屿 A 中也是陆地的时候，岛屿 B 是岛屿 A 的子岛
- 反过来说，如果岛屿 B 中存在一片陆地，在岛屿 A 的对应位置是海水，那么岛屿 B 就不是岛屿 A 的子岛
- 我们只要遍历 grid2 中的所有岛屿，把那些不可能是子岛的岛屿排除掉，剩下的就是子岛

```go
func countSubIslands(grid1 [][]int, grid2 [][]int) int {
    m := len(grid1)
    n := len(grid1[0])
    for i := 0; i < m; i++ {
        for j := 0; j < n; j++ {
            if grid1[i][j] == 0 && grid2[i][j] == 1 {
                // 这个岛屿肯定不是子岛，淹掉
                dfs(grid2, i, j)
            }
        }
    }
    // 现在 grid2 中剩下的岛屿都是子岛，计算岛屿数量
    res := 0
    for i := 0; i < m; i++ {
        for j := 0; j < n; j++ {
            if grid2[i][j] == 1 {
                res++
                dfs(grid2, i, j)
            }
        }
    }
    return res
}

// 从 (i, j) 开始，将与之相邻的陆地都变成海水
func dfs(grid [][]int, i int, j int) {
    m := len(grid)
    n := len(grid[0])
    if i < 0 || j < 0 || i >= m || j >= n {
        return
    }
    if grid[i][j] == 0 {
        return
    }

    grid[i][j] = 0
    dfs(grid, i + 1, j)
    dfs(grid, i, j + 1)
    dfs(grid, i - 1, j)
    dfs(grid, i, j - 1)
}
```

### 不同岛屿数量

很显然我们得想办法把二维矩阵中的「岛屿」进行转化，变成比如字符串这样的类型，然后利用 HashSet 这样的数据结构去重，最终得到不同的岛屿的个数

如果想把岛屿转化成字符串，说白了就是序列化，序列化说白了就是遍历

对于形状相同的岛屿，如果从同一起点出发，dfs 函数遍历的顺序肯定是一样的。因为遍历顺序是写死在你的递归函数里面的，不会动态改变

```go
func numDistinctIslands(grid [][]int) int {
    m, n := len(grid), len(grid[0])
    // 记录所有岛屿的序列化结果
    islands := make(map[string]bool)
    for i := 0; i < m; i++ {
        for j := 0; j < n; j++ {
            if grid[i][j] == 1 {
                // 淹掉这个岛屿，同时存储岛屿的序列化结果
                var sb strings.Builder
                // 初始的方向可以随便写，不影响正确性
                dfs(grid, i, j, &sb, 666)
                islands[sb.String()] = true
            }
        }
    }
    // 不相同的岛屿数量
    return len(islands)
}

func dfs(grid [][]int, i int, j int, sb *strings.Builder, dir int) {
    m, n := len(grid), len(grid[0])
    if i < 0 || j < 0 || i >= m || j >= n || grid[i][j] == 0 {
        return
    }
    // 前序遍历位置：进入 (i, j)
    grid[i][j] = 0
    sb.WriteString(strconv.Itoa(dir) + ",")

    // 搜索相邻节点
    dfs(grid, i - 1, j, sb, 1)
    dfs(grid, i + 1, j, sb, 2)
    dfs(grid, i, j - 1, sb, 3)
    dfs(grid, i, j + 1, sb, 4)

    // 后序遍历位置：离开 (i, j)
    sb.WriteString(strconv.Itoa(-dir) + ",")
}
```
