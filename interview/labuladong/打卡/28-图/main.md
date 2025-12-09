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

## 拓扑排序


图的「逆后序遍历」顺序，就是拓扑排序的结果

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251209133103884.jpg)

直观地说就是，让你把一幅图「拉平」，而且这个「拉平」的图里面，所有箭头方向都是一致的，比如上图所有箭头都是朝右的

进行拓扑排序之前，先要确保图中没有环。因为肯定做不到所有箭头方向一致；反过来，如果一幅图是「有向无环图」，那么一定可以进行拓扑排序

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