# 图

图结构就是 多叉树结构 的延伸。图结构逻辑上由若干节点（Vertex）和边（Edge）构成，我们一般用邻接表、邻接矩阵等方式来存储图

在树结构中，只允许父节点指向子节点，不存在子节点指向父节点的情况，子节点之间也不会互相链接；而图中没有那么多限制，节点之间可以相互指向，形成复杂的网络结构

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251209123454117.jpg)

图真的没啥高深的，本质上就是个高级点的多叉树而已，适用于树的 DFS/BFS 遍历算法，全部适用于图

## 逻辑结构

### 邻接

邻接表很直观，把每个节点 x 的邻居都存到一个列表里，然后把 x 和这个列表映射起来，这样就可以通过一个节点 x 找到它的所有相邻节点

邻接矩阵则是一个二维布尔数组，我们权且称为 matrix，如果节点 x 和 y 是相连的，那么就把 matrix[y] 设为 true（上图中绿色的方格代表 true）。如果想找节点 x 的邻居，去扫一圈 matrix[..] 就行了

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251209123628205.jpeg)

```go
// 邻接表
// graph 存储 x 的所有邻居节点
var graph [][]int

// 邻接矩阵
// matrix[y] 记录 x 是否有一条指向 y 的边
var matrix [][]bool
```

默认图节点是一个从 0 开始的整数，所以才能存储到邻接表和邻接矩阵中，通过索引访问
但实际问题中，图节点可能是其他类型，比如字符串、自定义类等，那应该怎么存储呢？
再额外使用一个哈希表，把实际节点和整数 id 映射起来，然后就可以用邻接表和邻接矩阵存储整数 id 了

注意分析两种存储方式的空间复杂度，对于一幅有 V 个节点，E 条边的图，邻接表的空间复杂度是 O(V+E)，而邻接矩阵的空间复杂度是 O(V\*V)。所以如果一幅图的 E 远小于 V^2（稀疏图），那么邻接表会比邻接矩阵节省空间，反之，如果 E 接近 V^2（稠密图），二者就差不多了

### 有向加权图

如果是邻接表，不仅仅存储某个节点 x 的所有邻居节点，还存储 x 到每个邻居的权重
如果是邻接矩阵，matrix[y] 不再是布尔值，而是一个 int 值，0 表示没有连接，其他值表示权重

```go
// 邻接表
// graph 存储 x 的所有邻居节点以及对应的权重
// 具体实现不一定非得这样，可以参考后面的通用实现
type Edge struct {
    to     int
    weight int
}
var graph [][]Edge

// 邻接矩阵
// matrix[y] 记录 x 指向 y 的边的权重，0 表示不相邻
var matrix [][]int
```

### 无向图

所谓的「无向」，是不是等同于「双向」
如果连接无向图中的节点 x 和 y，把 matrix[y] 和 matrix[y] 都变成 true
邻接表也是类似的操作，在 x 的邻居列表里添加 y，同时在 y 的邻居列表里添加 x

## 遍历

图的遍历就是 多叉树遍历 的延伸，主要遍历方式还是深度优先搜索（DFS）和广度优先搜索（BFS）
唯一的区别是，树结构中不存在环，而图结构中可能存在环，所以我们需要标记遍历过的节点，避免遍历函数在环中死循环
由于图结构的复杂性，可以细分为遍历图的「节点」、「边」和「路径」三种场景，每种场景的代码实现略有不同。遍历图的「节点」和「边」时，需要 visited 数组在前序位置做标记，避免重复遍历；遍历图的「路径」时，需要 onPath 数组在前序位置标记节点，在后序位置撤销标记

### DFS

#### 遍历所有节点（visited数组）

```go
// Vertex 图节点
type Vertex struct {
    id        int
    neighbors []*Vertex
}

// 图的遍历框架
// 需要一个 visited 数组记录被遍历过的节点
// 避免走回头路陷入死循环
func traverseGraph(s *Vertex, visited []bool) {
    // base case
    if s == nil {
        return
    }
    if visited[s.id] {
        // 防止死循环
        return
    }
    // 前序位置
    visited[s.id] = true
    fmt.Println("visit", s.id)
    for _, neighbor := range s.neighbors {
        traverseGraph(neighbor, visited)
    }
    // 后序位置
}
```

#### 遍历所有边（二维visited数组）

```go
package main

import "fmt"

// 图节点
type Vertex struct {
    ID        int
    Neighbors []*Vertex
}

// 遍历图的边
// 需要一个二维 visited 数组记录被遍历过的边，visited[u][v] 表示边 u->v 已经被遍历过
func traverseEdges(s *Vertex, visited [][]bool) {
    // base case
    if s == nil {
        return
    }
    for _, neighbor := range s.Neighbors {
        // 如果边已经被遍历过，则跳过
        if visited[s.ID][neighbor.ID] {
            continue
        }
        // 标记并访问边
        visited[s.ID][neighbor.ID] = true
        fmt.Printf("visit edge: %d -> %d\n", s.ID, neighbor.ID)
        traverseEdges(neighbor, visited)
    }
}
```

#### 遍历所有路径（onPath数组）

```go
// 下面的算法代码可以遍历图的所有路径，寻找从 src 到 dest 的所有路径
// onPath 和 path 记录当前递归路径上的节点
var onPath []bool = make([]bool, graph.size())
var path []int

func traverse(graph Graph, src int, dest int) {
    // base case
    if src < 0 || src >= graph.size() {
        return
    }
    if onPath[src] {
        // 防止死循环（成环）
        return
    }
    if src == dest {
        // 找到目标节点
        fmt.Print("find path: ")
        for _, node := range path {
            fmt.Printf("%d->", node)
        }
        fmt.Printf("%d\n", dest)
        return
    }
    // 前序位置
    onPath[src] = true
    path = append(path, src)
    for _, e := range graph.neighbors(src) {
        traverse(graph, e.to, dest)
    }
    // 后序位置
    path = path[:len(path)-1]
    onPath[src] = false
}
```

为啥之前讲的遍历节点就不用撤销 visited 数组的标记，而这里要在后序位置撤销 onPath 数组的标记呢？
因为前文遍历节点的代码中，visited 数组的职责是保证每个节点只会被访问一次。而对于图结构来说，要想遍历所有路径，可能会多次访问同一个节点，这是关键的区别

### BFS

图结构的广度优先搜索其实就是 多叉树的层序遍历，无非就是加了一个 visited 数组来避免重复遍历节点
理论上 BFS 遍历也需要区分遍历所有「节点」和遍历所有「路径」，但是实际上 BFS 算法一般只用来寻找那条最短路径，不会用来求所有路径
当然 BFS 算法肯定也可以求所有路径，但是我们一般会选择用 DFS 算法求所有路径
如果只求最短路径的话，只需要遍历「节点」就可以了，因为按照 BFS 算法一层一层向四周扩散的逻辑，第一次遇到目标节点，必然就是最短路径

#### 不记录遍历步数

```go
// 图结构的 BFS 遍历，从节点 s 开始进行 BFS
func bfs(graph *Graph, s int) {
    visited := make([]bool, graph.size())
    q := list.New()
    q.PushBack(s)
    visited[s] = true
    for q.Len() > 0 {
        e := q.Front()
        cur := e.Value.(int)
        q.Remove(e)
        fmt.Println("visit", cur)
        for _, edge := range graph.neighbors(cur) {
            if visited[edge.to] {
                continue
            }
            q.PushBack(edge.to)
            visited[edge.to] = true
        }
    }
}
```

#### 记录遍历步数

```go
// 从 s 开始 BFS 遍历图的所有节点，且记录遍历的步数
func bfs(graph Graph, s int) {
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
            for _, e := range graph.neighbors(cur) {
                if visited[e.to] {
                    continue
                }
                q = append(q, e.to)
                visited[e.to] = true
            }
        }
        step++
    }
}
```

#### 适配不同权重

```go
// 图结构的 BFS 遍历，从节点 s 开始进行 BFS，且记录遍历步数（从起点 s 到当前节点的边的条数）
// 每个节点自行维护 State 类，记录从 s 走来的遍历步数
type State struct {
    // 当前节点 ID
    node   int
    // 从起点 s 到当前节点的遍历步数
    step int
}

func bfs(graph Graph, s int) {
    visited := make([]bool, graph.size())
    q := []*State{{s, 0}}
    visited[s] = true
    for len(q) > 0 {
        state := q[0]
        q = q[1:]
        cur := state.node
        step := state.step
        fmt.Printf("visit %d with step %d\n", cur, step)
        for _, e := range graph.neighbors(cur) {
            if visited[e.to] {
                continue
            }
            q = append(q, &State{e.to, step + 1})
            visited[e.to] = true
        }
    }
}
```

## 欧拉图

欧拉路径（Eulerian Path）：在图中找到一条路径，使得每条边都被遍历恰好一次的路径
欧拉回路（Eulerian Circuit）：欧拉路径的特殊情况，即起点和终点是同一个节点的欧拉路径

根据这两个概念，我们可以对图进行分类，分为三类：

1. 欧拉图（Eulerian Graph）：存在欧拉回路的图
1. 半欧拉图（Semi-Eulerian Graph）：存在欧拉路径但不存在欧拉回路的图
1. 非欧拉图（Non-Eulerian Graph）：既不存在欧拉路径也不存在欧拉回路的图

「一笔画」游戏的本质是寻找欧拉路径/欧拉回路，可以通过节点的度数判断是否存在欧拉路径/欧拉回路

- 如果所有节点的度数都是偶数，那么可以从任何节点开始，一定可以完成一笔画，且最终会回到起点
- 如果只有两个节点的度数为奇数，那么必须从这两个奇数度节点中的任意一个开始，才能完成一笔画
- 如果上面两种情况都不满足，那么无法完成一笔画

### Hierholzer 算法

用于计算欧拉路径/欧拉回路的经典算法，它是图结构 DFS 算法的拓展，本质就是 DFS 算法的逆后序遍历结果，分为以下几步：

1. 根据每个节点的度数，确定欧拉路径/欧拉回路的起点
1. 从起点开始执行遍历所有边的 DFS 算法，记录后序遍历结果
1. 最后，将后序遍历结果反转，即可得到欧拉路径/欧拉回路。对于无向图，由于边没有方向的区别，所以即便不反转，结果也是对的

```go
// findEulerPath 使用 Hierholzer 算法寻找欧拉路径
// graph: 图的邻接表表示，注意这里使用 map[int][]int 是为了方便修改
// start: 路径的起始节点
func findEulerPath(graph map[int][]int, start int) []int {
	// 最终的欧拉路径，使用栈来实现更高效
	path := []int{}
	// 使用一个栈来模拟递归调用
	stack := []int{start}
	for len(stack) > 0 {
		// 查看栈顶节点，但不弹出
		v := stack[len(stack)-1]
		// 如果栈顶节点还有未访问的边
		if len(graph[v]) > 0 {
			// 取出一条邻边 (注意：这里取出的是最后一个元素，效率为 O(1))
			u := graph[v][len(graph[v])-1]
			// 从邻接表中移除这条边 (模拟“删除”边)
			graph[v] = graph[v][:len(graph[v])-1]
			// 将新节点压入栈中，继续DFS
			stack = append(stack, u)
		} else {
			// 如果栈顶节点没有未访问的边，则将其加入到路径中
			// 并从栈中弹出
			path = append(path, v)
			stack = stack[:len(stack)-1]
		}
	}
	// 由于是后序加入节点，所以路径是反的，需要反转
	for i, j := 0, len(path)-1; i < j; i, j = i+1, j-1 {
		path[i], path[j] = path[j], path[i]
	}
	return path
}
```

## 二分图

二分图的顶点集可分割为两个互不相交的子集，图中每条边依附的两个顶点都分属于这两个子集，且两个子集内的顶点不相邻
给你一幅「图」，请你用两种颜色将图中的所有顶点着色，且使得任意一条边的两个端点的颜色都不相同

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251209132026095.jpg)

判定二分图的算法很简单，就是用代码解决「双色问题」：
遍历一遍图，一边遍历一边染色，看看能不能用两种颜色给所有节点染色，且相邻节点的颜色都不相同

```go
// 图遍历框架
func traverse(graph Graph, visited []bool, v int) {
    visited[v] = true
    // 遍历节点 v 的所有相邻节点 neighbor
    for _, neighbor := range graph.neighbors(v) {
        if !visited[neighbor] {
            // 相邻节点 neighbor 没有被访问过
            // 那么应该给节点 neighbor 涂上和节点 v 不同的颜色
            traverse(graph, visited, neighbor)
        } else {
            // 相邻节点 neighbor 已经被访问过
            // 那么应该比较节点 neighbor 和节点 v 的颜色
            // 若相同，则此图不是二分图
        }
    }
}
```

## 环检测

看到依赖问题，首先想到的就是把问题转化成「有向图」这种数据结构，只要图中存在环，那就说明存在循环依赖

### DFS

```go
func canFinish(numCourses int, prerequisites [][]int) bool {
    // 记录一次递归堆栈中的节点
    onPath := make([]bool, numCourses)
    // 记录节点是否被遍历过
    visited := make([]bool, numCourses)
    // 记录图中是否有环
    hasCycle := false
    graph := buildGraph(numCourses, prerequisites)
    for i := 0; i < numCourses; i++ {
        // 遍历图中的所有节点
        traverse(graph, i, &onPath, &visited, &hasCycle)
    }
    // 只要没有循环依赖可以完成所有课程
    return !hasCycle
}

// 图遍历函数，遍历所有路径
func traverse(graph [][]int, s int, onPath *[]bool, visited *[]bool, hasCycle *bool) {
    if *hasCycle {
        // 如果已经找到了环，也不用再遍历了
        return
    }
    if (*onPath)[s] {
        // s 已经在递归路径上，说明成环了
        *hasCycle = true
        return;
    }
    if (*visited)[s] {
        // 不用再重复遍历已遍历过的节点
        return
    }
    // 前序代码位置
    (*visited)[s] = true
    (*onPath)[s] = true
    for _, t := range graph[s] {
        traverse(graph, t, onPath, visited, hasCycle)
    }
    // 后序代码位置
    (*onPath)[s] = false
}

// buildGraph 构建有向图
func buildGraph(numCourses int, prerequisites [][]int) []([]int) {
    // 图中共有 numCourses 个节点
    graph := make([][]int, numCourses)
    for i := 0; i < numCourses; i++ {
        graph[i] = []int{}
    }
    for _, edge := range prerequisites {
        from, to := edge[1], edge[0]
        // 添加一条从 from 指向 to 的有向边
        // 边的方向是「被依赖」关系，即修完课程 from 才能修课程 to
        graph[from] = append(graph[from], to)
    }
    return graph
}
```

### BFS

```go
func canFinish(numCourses int, prerequisites [][]int) bool {
    // 建图，有向边代表「被依赖」关系
    graph := buildGraph(numCourses, prerequisites)
    // 构建入度数组
    indegree := make([]int, numCourses)
    for _, edge := range prerequisites {
        to := edge[0]
        // 节点 to 的入度加一
        indegree[to]++
    }
    // 根据入度初始化队列中的节点
    q := make([]int, 0)
    for i := 0; i < numCourses; i++ {
        if indegree[i] == 0 {
            // 节点 i 没有入度，即没有依赖的节点
            // 可以作为拓扑排序的起点，加入队列
            q = append(q, i)
        }
    }
    // 记录遍历的节点个数
    count := 0
    // 开始执行 BFS 循环
    for len(q) > 0 {
        // 弹出节点 cur，并将它指向的节点的入度减一
        cur := q[0]
        q = q[1:]
        count++
        for _, next := range graph[cur] {
            indegree[next]--
            if indegree[next] == 0 {
                // 如果入度变为 0，说明 next 依赖的节点都已被遍历
                q = append(q, next)
            }
        }
    }
    // 如果所有节点都被遍历过，说明不成环
    return count == numCourses
}

// 建图函数
func buildGraph(numCourses int, prerequisites [][]int) [][]int {
    // 见前文
}
```

## 拓扑排序

图的「逆后序遍历」顺序，就是拓扑排序的结果

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251209133103884.jpg)

直观地说就是，让你把一幅图「拉平」，而且这个「拉平」的图里面，所有箭头方向都是一致的，比如上图所有箭头都是朝右的

进行拓扑排序之前，先要确保图中没有环。因为肯定做不到所有箭头方向一致；反过来，如果一幅图是「有向无环图」，那么一定可以进行拓扑排序

### DFS

```go
func findOrder(numCourses int, prerequisites [][]int) []int {
    graph := buildGraph(numCourses, prerequisites)
    visited := make([]bool, numCourses)
    onPath := make([]bool, numCourses)
    postorder := make([]int, 0)
    hasCycle := false
    // 遍历图
    for i := 0; i < numCourses; i++ {
        traverse(graph, i, &onPath, &visited, &postorder, &hasCycle)
    }
    // 有环图无法进行拓扑排序
    if hasCycle {
        return []int{}
    }
    // 逆后序遍历结果即为拓扑排序结果
    reverse(postorder)
    return postorder
}

// 图遍历函数
func traverse(graph [][]int, s int, onPath *[]bool, visited *[]bool, postorder *[]int, hasCycle *bool) {
    if (*onPath)[s] {
        // 发现环
        *hasCycle = true
    }
    if (*visited)[s] || *hasCycle {
        return
    }
    // 前序遍历位置
    (*onPath)[s] = true
    (*visited)[s] = true
    for _, t := range graph[s] {
        traverse(graph, t, onPath, visited, postorder, hasCycle)
    }
    // 后序遍历位置
    *postorder = append(*postorder, s)
    (*onPath)[s] = false
}

func reverse(arr []int) {
    i, j := 0, len(arr)-1
    for i < j {
        arr[i], arr[j] = arr[j], arr[i]
        i++
        j--
    }
}

// 建图函数
func buildGraph(numCourses int, prerequisites [][]int) [][]int {
    // 代码见前文
}
```

### BFS

```go
func canFinish(numCourses int, prerequisites [][]int) bool {
    // 建图，有向边代表「被依赖」关系
    graph := buildGraph(numCourses, prerequisites)
    // 构建入度数组
    indegree := make([]int, numCourses)
    for _, edge := range prerequisites {
        to := edge[0]
        // 节点 to 的入度加一
        indegree[to]++
    }

    // 根据入度初始化队列中的节点
    q := make([]int, 0)
    for i := 0; i < numCourses; i++ {
        if indegree[i] == 0 {
            // 节点 i 没有入度，即没有依赖的节点
            // 可以作为拓扑排序的起点，加入队列
            q = append(q, i)
        }
    }

    // 记录遍历的节点个数
    count := 0
    // 开始执行 BFS 循环
    for len(q) > 0 {
        // 弹出节点 cur，并将它指向的节点的入度减一
        cur := q[0]
        q = q[1:]
        count++
        for _, next := range graph[cur] {
            indegree[next]--
            if indegree[next] == 0 {
                // 如果入度变为 0，说明 next 依赖的节点都已被遍历
                q = append(q, next)
            }
        }
    }

    // 如果所有节点都被遍历过，说明不成环
    return count == numCourses
}

// 建图函数
func buildGraph(numCourses int, prerequisites [][]int) [][]int {
    // 见前文
}
```

把二叉树理解成一幅有向图，边的方向是由父节点指向子节点，那么就是下图这样：

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251209133252512.jpeg)

对于标准的后序遍历结果，根节点出现在最后，只要把遍历结果反过来，就是拓扑排序结果：

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251209133336002.jpeg)

## Union Find 并查集

并查集（Union Find）结构是 二叉树结构 的衍生，用于高效解决无向图的连通性问题
可以在 O(1) 时间内：

- 合并两个连通分量
- 查询两个节点是否连通
- 查询连通分量的数量

### 原理

并查集本质上还是树结构的延伸
如果我们想办法把同一个连通分量的节点都放到同一棵树中，把这棵树的根节点作为这个连通分量的代表，那么我们就可以高效实现上面的操作了
并查集底层其实是一片森林（若干棵多叉树），每棵树代表一个连通分量：

- connected(p, q)：只需要判断 p 和 q 所在的多叉树的根节点，若相同，则 p 和 q 在同一棵树中，即连通，否则不连通
- count()：只需要统计一下总共有多少棵树，即可得到连通分量的数量
- union(p, q)：只需要将 p 节点所在的这棵树的根节点，接入到 q 节点所在的这棵树的根节点下面，即可完成连接操作。注意这里并不是 p, q 两个节点的合并，而是两棵树根节点的合并。因为 p, q 一旦连通，那么他们所属的连通分量就合并成了同一个更大的连通分量

### 算法

```go
type UF struct {
    // 连通分量个数
    count int
    // 存储每个节点的父节点
    parent []int
}

// n 为图中节点的个数
func NewUF(n int) *UF {
    parent := make([]int, n)
    for i := 0; i < n; i++ {
        parent[i] = i
    }
    return &UF{
        count:  n,
        parent: parent,
    }
}

// 将节点 p 和节点 q 连通
func (u *UF) Union(p, q int) {
    rootP := u.Find(p)
    rootQ := u.Find(q)
    if rootP == rootQ {
        return
    }
    u.parent[rootQ] = rootP
    // 两个连通分量合并成一个连通分量
    u.count--
}

// 判断节点 p 和节点 q 是否连通
func (u *UF) Connected(p, q int) bool {
    rootP := u.Find(p)
    rootQ := u.Find(q)
    return rootP == rootQ
}

func (u *UF) Find(x int) int {
    if u.parent != x {
        u.parent = u.Find(u.parent)
    }
    return u.parent
}

// 返回图中的连通分量个数
func (u *UF) Count() int {
    return u.count
}
```

## 最小生成树

首先理解什么是生成树。给定一个无向连通图 G，其生成树是 G 的一个子图，它包含 G 中的所有顶点，并且是一棵树（即无环连通图）
换句话说，生成树具有以下特性：

1. 包含原图中的所有顶点
1. 边的数量为顶点数减一（V-1条边）
1. 连通且无环

如果图是加权图，那么最小生成树就是边权重总和最小的生成树

- Kruskal 算法，边排序 + 并查集（Union-Find）来检测环，其本质是贪心思想
- Prim 算法，从单个顶点开始 + 优先队列（最小堆）选择最小边 + 逐步扩展树，只需要对 Dijkstra 算法稍作修改，本质是 BFS + 贪心思想

### Kruskal算法

数据结构

```go
// Edge 定义图的边
type Edge struct {
	Start  int     // 起点
	End    int     // 终点
	Weight float64 // 权重
}

// Graph 定义图
type Graph struct {
	Vertices int     // 顶点数量
	Edges    []Edge // 边集合
}
```

并查集实现（路径压缩 + 按秩合并）

```go
// UnionFind 并查集结构
type UnionFind struct {
	parent []int // 每个节点的父节点
	rank   []int // 树的秩（近似高度）
}

// NewUnionFind 创建并初始化并查集
func NewUnionFind(size int) *UnionFind {
	uf := &UnionFind{
		parent: make([]int, size),
		rank:   make([]int, size),
	}
	for i := 0; i < size; i++ {
		uf.parent[i] = i // 每个节点的父节点初始化为自己
		uf.rank[i] = 0   // 初始秩为0
	}
	return uf
}

// Find 查找元素x的根节点，带路径压缩
func (uf *UnionFind) Find(x int) int {
	// 路径压缩：将查找路径上的所有节点直接连接到根节点
	if uf.parent != x {
		uf.parent = uf.Find(uf.parent)
	}
	return uf.parent
}

// Union 合并两个元素所在的集合，使用按秩合并优化
func (uf *UnionFind) Union(x, y int) bool {
	rootX := uf.Find(x)
	rootY := uf.Find(y)
	// 如果已经在同一个集合中，不需要合并
	if rootX == rootY {
		return false
	}
	// 按秩合并：将秩较小的树连接到秩较大的树
	if uf.rank[rootX] < uf.rank[rootY] {
		uf.parent[rootX] = rootY
	} else if uf.rank[rootX] > uf.rank[rootY] {
		uf.parent[rootY] = rootX
	} else {
		// 秩相等时，任意选择，并将秩加1
		uf.parent[rootY] = rootX
		uf.rank[rootX]++
	}
	return true
}
```

Kruskal 算法主函数

```go
// Kruskal 算法实现
func Kruskal(graph Graph) ([]Edge, float64) {
	// 1. 按权重对边进行排序
	sort.Slice(graph.Edges, func(i, j int) bool {
		return graph.Edges[i].Weight < graph.Edges[j].Weight
	})
	// 2. 初始化并查集
	uf := NewUnionFind(graph.Vertices)
	// 3. 存储最小生成树的边
	mstEdges := make([]Edge, 0, graph.Vertices-1)
	totalWeight := 0.0
	// 4. 遍历排序后的边
	for _, edge := range graph.Edges {
		// 如果起点和终点不在同一个集合中（不形成环）
		if uf.Union(edge.Start, edge.End) {
			// 将边加入最小生成树
			mstEdges = append(mstEdges, edge)
			totalWeight += edge.Weight
			// 如果已经找到 n-1 条边，算法结束
			if len(mstEdges) == graph.Vertices-1 {
				break
			}
		}
	}
	// 5. 检查是否找到完整的最小生成树
	if len(mstEdges) < graph.Vertices-1 {
		fmt.Printf("警告: 图可能不连通，只找到 %d 条边，需要 %d 条边\n",
			len(mstEdges), graph.Vertices-1)
	}
	return mstEdges, totalWeight
}
```

### Prim算法

数据结构

```go
// 定义无穷大，表示无边连接
const INF = math.MaxFloat64

// PrimEdge 表示最小生成树中的一条边
type PrimEdge struct {
	From   int     // 起始顶点（在树中）
	To     int     // 新加入的顶点
	Weight float64 // 边的权重
}

// GraphAdjList 使用邻接表存储图（推荐用于稀疏图）
type GraphAdjList struct {
	Vertices int
	AdjList  [][]AdjNode // 邻接表：每个顶点的邻居列表
}

// AdjNode 表示一条边的终点和权重
type AdjNode struct {
	Vertex int     // 相邻顶点
	Weight float64 // 边的权重
}

// MinHeapItem 是优先队列中的元素
type MinHeapItem struct {
	Vertex int     // 顶点编号
	Weight float64 // 从当前树到该顶点的最小边权
	From   int     // 来自哪个顶点（用于记录边）
}

// MinHeap 实现最小堆接口（heap.Interface）
type MinHeap []MinHeapItem

func (h MinHeap) Len() int           { return len(h) }
func (h MinHeap) Less(i, j int) bool { return h[i].Weight < h[j].Weight } // 最小堆：权重小的优先
func (h MinHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *MinHeap) Push(x interface{}) {
	*h = append(*h, x.(MinHeapItem))
}

func (h *MinHeap) Pop() interface{} {
	old := *h
	n := len(old)
	item := old[n-1]
	*h = old[0 : n-1]
	return item
}
```

构建图

```go
// NewGraph 创建一个带边的邻接表图
func NewGraph(vertices int, edges [][3]float64) GraphAdjList {
	graph := GraphAdjList{
		Vertices: vertices,
		AdjList:  make([][]AdjNode, vertices),
	}
	// 初始化邻接表
	for i := 0; i < vertices; i++ {
		graph.AdjList[i] = make([]AdjNode, 0)
	}
	// 添加边（无向图，双向）
	for _, e := range edges {
		u, v, w := int(e[0]), int(e[1]), e[2]
		graph.AdjList[u] = append(graph.AdjList[u], AdjNode{Vertex: v, Weight: w})
		graph.AdjList[v] = append(graph.AdjList[v], AdjNode{Vertex: u, Weight: w})
	}
	return graph
}
```

Prim 算法主函数（邻接矩阵）

```go
// Prim 执行 Prim 算法，返回最小生成树的边和总权重
func Prim(graph GraphAdjList) ([]PrimEdge, float64) {
	vertices := graph.Vertices

	// 如果图为空或只有一个顶点
	if vertices <= 1 {
		return []PrimEdge{}, 0
	}

	// visited[i]：顶点 i 是否已在 MST 中
	visited := make([]bool, vertices)

	// minWeight[i]：从当前 MST 到顶点 i 的最小边权（初始为无穷）
	minWeight := make([]float64, vertices)
	for i := range minWeight {
		minWeight[i] = INF
	}

	// parent[i]：顶点 i 是从哪个顶点加入 MST 的（用于重建边）
	parent := make([]int, vertices)

	// 优先队列：存储 (顶点, 到树的最小权重, 来自顶点)
	pq := &MinHeap{}
	heap.Init(pq)

	// 从顶点 0 开始（任意起点）
	start := 0
	minWeight[start] = 0
	heap.Push(pq, MinHeapItem{Vertex: start, Weight: 0, From: -1})

	// 结果：MST 的边列表
	mstEdges := make([]PrimEdge, 0, vertices-1) // 预分配容量，避免频繁扩容
	totalWeight := 0.0

	// 主循环：每次加入一个新顶点，直到所有顶点都被访问
	for pq.Len() > 0 && len(mstEdges) < vertices-1 {
		// 取出当前离树最近的顶点
		item := heap.Pop(pq).(MinHeapItem)
		current := item.Vertex

		// 如果该顶点已被加入树，跳过（可能重复入队）
		if visited[current] {
			continue
		}

		// 标记为已访问
		visited[current] = true

		// 如果不是起点，则记录边（parent 记录了它从哪来）
		if item.From != -1 {
			edge := PrimEdge{
				From:   item.From,
				To:     current,
				Weight: item.Weight,
			}
			mstEdges = append(mstEdges, edge)
			totalWeight += item.Weight
		}

		// 遍历当前顶点的所有邻居
		for _, neighbor := range graph.AdjList[current] {
			next := neighbor.Vertex
			weight := neighbor.Weight

			// 如果邻居未访问，且当前边权比之前记录的更小 → 更新
			if !visited[next] && weight < minWeight[next] {
				minWeight[next] = weight
				parent[next] = current
				heap.Push(pq, MinHeapItem{Vertex: next, Weight: weight, From: current})
			}
		}
	}

	// 验证是否构建了完整 MST
	if len(mstEdges) != vertices-1 {
		fmt.Printf("⚠ 警告：图不连通，仅构建了 %d 条边（需要 %d 条）\n", len(mstEdges), vertices-1)
	}

	return mstEdges, totalWeight
}
```
