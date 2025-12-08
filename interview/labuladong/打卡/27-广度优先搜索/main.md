# 广度优先搜索

BFS 算法的本质就是遍历一幅图
图的遍历算法其实就是多叉树的遍历算法加了个 visited 数组防止死循环
多叉树的遍历算法又是从二叉树遍历算法衍生出来的，所以 BFS 算法的本质就是二叉树的层序遍历

BFS 算法经常用来求解最短路径问题？
其实所谓的最短路径，都可以类比成二叉树最小深度这类问题（寻找距离根节点最近的叶子节点），递归遍历必须要遍历整棵树的所有节点才能找到目标节点，而层序遍历不需要遍历所有节点就能搞定，所以层序遍历适合解决这类最短路径问题

在真实的面试笔试题目中，一般不是直接让你遍历树/图这种标准数据结构，而是给你一个具体的场景题，你需要把具体的场景抽象成一个标准的图/树结构，然后利用 BFS 算法穷举得出答案

## 算法框架

```go
// 从 s 开始 BFS 遍历图的所有节点，且记录遍历的步数
// 当走到目标节点 target 时，返回步数
func bfs(graph Graph, s int, target int) int {
    visited := make([]bool, graph.size())
    q := []int{s}
    visited[s] = true
    // 记录从 s 开始走到当前节点的步数
    step := 0
    for len(q) > 0 {
        sz := len(q)
        for i := 0; i < sz; i++ {
            cur := q[0]
            q = q[1:]
            fmt.Printf("visit %d at step %d\n", cur, step)
            // 判断是否到达终点
            if cur == target {
                return step
            }
            // 将邻居节点加入队列，向四周扩散搜索
            for _, to := range neighborsOf(cur) {
                if !visited[to] {
                    q = append(q, to)
                    visited[to] = true
                }
            }
        }
        step++
    }
    // 如果走到这里，说明在图中没有找到目标节点
    return -1
}
```

## 例题

### 滑动谜题

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251208192615129.jpeg)

题目问的其实就是从起点到终点所需的最短路径是多少
起点的邻居节点是谁？把数字 0 和上下左右的数字进行交换，其实就是起点的四个邻居节点（由于本题中棋盘的大小是 2x3，所以索引边界内的实际邻居节点会小于四个）

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251208192739837.jpeg)

其中比较有技巧性的点在于，二维数组有「上下左右」的概念，压缩成一维的字符串后后，还怎么把数字 0 和上下左右的数字进行交换

```go
// 记录一维字符串相邻索引
neighbor := [][]int{
    {1, 3},
    {0, 4, 2},
    {1, 5},
    {0, 4},
    {3, 1, 5},
    {4, 2},
}
```

这个映射的含义就是，在一维字符串中，索引 i 在二维数组中的的相邻索引为 neighbor[i]。例如，我们可以知道 neighbor[4] 的周围元素为 neighbor[3], neighbor[1], neighbor[5]

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251208193027769.jpeg)

### 解开密码锁的最少次数

不管所有的限制条件，不管 deadends 和 target 的限制，就思考一个问题：如果让你设计一个算法，穷举所有可能的密码组合，你怎么做？

就从 "0000" 开始，如果你只转一下锁，有几种可能？总共有 4 个位置，每个位置可以向上转，也可以向下转，也就是可以穷举出 "1000", "9000", "0100", "0900"... 共 8 种密码
然后，再以这 8 种密码作为基础，其中每个密码又可以转动一下衍生出 8 种密码，以此类推
心里那棵递归树出来没有？应该是一棵八叉树，每个节点都有 8 个子节点，向下衍生

还有些问题需要解决：

1. 会走回头路，我们可以从 "0000" 拨到 "1000"，但是等从队列拿出 "1000" 时，还会拨出一个 "0000"，这样的话会产生死循环。这个问题很好解决，其实就是成环了嘛，我们用一个 visited 集合记录已经穷举过的密码，再次遇到时，不要再加到队列里就行了
1. 没有对 deadends 进行处理，按道理这些「死亡密码」是不能出现的。这个问题也好处理，额外用一个 deadends 集合记录这些死亡密码，凡是遇到这些密码，不要加到队列里就行了。或者还可以更简单一些，直接把 deadends 中的死亡密码作为 visited 集合的初始元素，这样也可以达到目的

## 双向BFS优化

传统的 BFS 框架是从起点开始向四周扩散，遇到终点时停止

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251208193532152.jpeg)

而双向 BFS 则是从起点和终点同时开始扩散，当两边有交集的时候停止

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251208193458833.jpeg)

就好比有 A 和 B 两个人，传统 BFS 就相当于 A 出发去找 B，而 B 待在原地不动；双向 BFS 则是 A 和 B 一起出发，双向奔赴。那当然第二种情况下 A 和 B 可以更快相遇

### 局限性

你必须知道终点在哪里，才能使用双向 BFS 进行优化
对于 BFS 算法，我们肯定是知道起点的，但是终点具体是什么，我们在一开始可能并不知道
比如上面的密码锁问题和滑动拼图问题，题目都明确给出了终点，都可以用双向 BFS 进行优化
比如二叉树最小高度的问题，起点是根节点，终点是距离根节点最近的叶子节点，你在算法开始时并不知道终点具体在哪里，所以就没办法使用双向 BFS 进行优化

```go
func openLock(deadends []string, target string) int {
    deads := make(map[string]struct{})
    for _, s := range deadends {
        deads[s] = struct{}{}
    }
    // base case
    if _, found := deads["0000"]; found {
        return -1
    }
    if target == "0000" {
        return 0
    }

    // 用集合不用队列，可以快速判断元素是否存在
    q1 := make(map[string]struct{})
    q2 := make(map[string]struct{})
    visited := make(map[string]struct{})

    step := 0
    q1["0000"] = struct{}{}
    visited["0000"] = struct{}{}
    q2[target] = struct{}{}
    visited[target] = struct{}{}

    for len(q1) != 0 && len(q2) != 0 {
        // 在这里增加步数
        step++

        // 哈希集合在遍历的过程中不能修改，所以用 newQ1 存储邻居节点
        newQ1 := make(map[string]struct{})

        // 获取 q1 中的所有节点的邻居
        for cur := range q1 {
            // 将一个节点的未遍历相邻节点加入集合
            for _, neighbor := range getNeighbors(cur) {
                // 判断是否到达终点
                if _, found := q2[neighbor]; found {
                    return step
                }
                if _, found := visited[neighbor]; !found {
                    if _, found := deads[neighbor]; !found {
                        newQ1[neighbor] = struct{}{}
                        visited[neighbor] = struct{}{}
                    }
                }
            }
        }
        // newQ1 存储着 q1 的邻居节点
        q1 = newQ1
        // 因为每次 BFS 都是扩散 q1，所以把元素数量少的集合作为 q1
        if len(q1) > len(q2) {
            q1, q2 = q2, q1
        }
    }
    return -1
}

// 将 s[j] 向上拨动一次
func plusOne(s string, j int) string {
    ch := []rune(s)
    if ch[j] == '9' {
        ch[j] = '0'
    } else {
        ch[j]++
    }
    return string(ch)
}

// 将 s[i] 向下拨动一次
func minusOne(s string, j int) string {
    ch := []rune(s)
    if ch[j] == '0' {
        ch[j] = '9'
    } else {
        ch[j]--
    }
    return string(ch)
}

func getNeighbors(s string) []string {
    neighbors := []string{}
    for i := 0; i < 4; i++ {
        neighbors = append(neighbors, plusOne(s, i))
        neighbors = append(neighbors, minusOne(s, i))
    }
    return neighbors
}
```

双向 BFS 还是遵循 BFS 算法框架的，但是有几个细节区别：

1. 不再使用队列存储元素，而是改用哈希集合，方便快速判两个集合是否有交集
1. 调整了 return step 的位置。因为双向 BFS 中不再是简单地判断是否到达终点，而是判断两个集合是否有交集，所以要在计算出邻居节点时就进行判断
1. 每次都保持 q1 是元素数量较小的集合，这样可以一定程度减少搜索次数

因为按照 BFS 的逻辑，队列（集合）中的元素越多，扩散邻居节点之后新的队列（集合）中的元素就越多；在双向 BFS 算法中，如果我们每次都选择一个较小的集合进行扩散，那么占用的空间增长速度就会慢一些，效率就会高一些
无论传统 BFS 还是双向 BFS，无论做不做优化，从 Big O 衡量标准来看，时间复杂度都是一样的，只能说双向 BFS 是一种进阶技巧，算法运行的速度会相对快一点，掌握不掌握其实都无所谓