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