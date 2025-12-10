# 最短路径

Dijkstra 算法和 A\* 算法是 图的 BFS 遍历 的拓展，可以处理不包含负权重的单源最短路径问题

SPFA 算法（基于队列的 Bellman-Ford 算法）是图的 BFS 遍历 的拓展，可以处理包含负权重的单源最短路径问题

Floyd 算法是 动态规划 的应用，可以处理多源最短路径问题

## 分类

### 单源最短路径

让你计算从某个起点出发，到其他所有顶点的最短路径
单源最短路径算法最终得到的输出应该是一个一维数组 distTo，distTo[i] 表示从起点到节点 i 的最短路径长度
比较有代表性的单源最短路径算法有：

1. Dijkstra 算法，其本质是 BFS 算法 + 贪心思想，效率较高，但是不能处理带有负权重的图
1. 基于队列的 Bellman-Ford 算法，其本质也是 BFS 算法，可以处理带有负权重的图，但效率比 Dijkstra 算法低

### 点对点最短路径

很多算法题中不需要我们计算起点到所有其他节点的最短路径，仅需要计算从起点 src 到某一个目标节点 dst 的最短路径
一种专门处理点对点问题的算法：A\* 算法（A Star Algorithm）
点对点最短路径问题（已知起点和终点）比单源最短路径问题（已知起点）多了终点信息，所以完全有可能利用这个信息来提高算法的效率
A\* 算法的关键就在这里：它能够充分利用已知信息，有方向性地进行搜索，更快地找到终点。我们称这类算法为启发式搜索算法（Heuristic Search Algorithm）。但是请注意，这个猜测只是经验法则，并不一定总是正确。所以启发式算法需要合理设置启发函数（Heuristic Function），在经验法则和实际情况中找到平衡，确保在经验法则失效时，算法的效率也不会太差

### 多源最短路径

计算任意两节点之间的最短路径
多源最短路径算法最终得到的输出应该是一个二维数组 dist，dist[i][j] 表示从节点 i 到节点 j 的最短路径长度
比较有代表性的多源最短路径算法有：

1. Floyd 算法，其本质是动态规划算法

### 负权重边的影响

在计算最短路径时，需要着重注意的是这幅图是否包含负权重边；一旦包含负权重边，一定要检查是否包含负权重环
想让 Dijkstra 这类包含贪心思想的算法成立，需要一个前提：它假设随着经过的边的数量增加，路径权重和一定也会增加
但负权重边的出现打破了这一假设，导致算法失效
常见最短路径算法中：

- Dijkstra 算法和 A\* 算法不能处理含有负权重边的图
- Floyd 算法和 Bellman-Ford 算法可以处理负权重边
- Bellman-Ford 算法常用来检测负权重环

## Dijkstra算法

Dijkstra 算法是一种用于计算图中单源最短路径的算法，其本质是标准 BFS 算法 + 贪心思想
如果图中包含负权重边，会让贪心思想失效，所以 Dijkstra 只能处理不包含负权重边的图
Dijkstra 算法和标准的 BFS 算法的区别只有两个：

1. 标准 BFS 算法使用普通队列，Dijkstra 算法使用优先级队列，让距离起点更近的节点优先出队（贪心思想的体现）
1. 标准 BFS 算法使用一个 visited 数组记录访问过的节点，确保算法不会陷入死循环；Dijkstra 算法使用一个 distTo 数组，确保算法不会陷入死循环，同时记录起点到其他节点的最短路径

### 代码

```go
package main

import (
	"container/heap"
	"math"
)

// 记录队列中的状态
type State struct {
	// 当前节点 ID
	node int
	// 从起点 s 到当前 node 节点的最小路径权重和
	distFromStart int
}

// 优先级队列，distFromStart 较小的节点排在前面
type PriorityQueue []*State

func (pq PriorityQueue) Len() int { return len(pq) }
func (pq PriorityQueue) Less(i, j int) bool {
	return pq[i].distFromStart < pq[j].distFromStart
}
func (pq PriorityQueue) Swap(i, j int) { pq[i], pq[j] = pq[j], pq[i] }

func (pq *PriorityQueue) Push(x interface{}) {
	*pq = append(*pq, x.(*State))
}

func (pq *PriorityQueue) Pop() interface{} {
	old := *pq
	n := len(old)
	item := old[n-1]
	*pq = old[:n-1]
	return item
}

// 输入不包含负权重边的加权图 graph 和起点 src
// 返回从起点 src 到其他节点的最小路径权重和
func dijkstra(graph Graph, src int) []int {
	// 记录从起点 src 到其他节点的最小路径权重和
	// distTo[i] 表示从起点 src 到节点 i 的最小路径权重和
	distTo := make([]int, graph.Size())
	// 都初始化为正无穷，表示未计算
	for i := range distTo {
		distTo[i] = math.MaxInt32
	}

	pq := &PriorityQueue{}
	heap.Init(pq)

	// 从起点 src 开始进行 BFS
	heap.Push(pq, &State{node: src, distFromStart: 0})
	distTo[src] = 0

	for pq.Len() > 0 {
		state := heap.Pop(pq).(*State)
		curNode := state.node
		curDistFromStart := state.distFromStart

		if distTo[curNode] < curDistFromStart {
			// 在 Dijkstra 算法中，队列中可能存在重复的节点 state
			// 所以要在元素出队时进行判断，去除较差的重复节点
			continue
		}

		for _, e := range graph.neighbors(curNode) {
			nextNode := e.to
			nextDistFromStart := curDistFromStart + e.weight

			if distTo[nextNode] <= nextDistFromStart {
				continue
			}
			// 将 nextNode 节点加入优先级队列
			heap.Push(pq, &State{node: nextNode, distFromStart: nextDistFromStart})
			// 记录 nextNode 节点到起点的最小路径权重和
			distTo[nextNode] = nextDistFromStart
		}
	}

	return distTo
}
```

Dijkstra 算法和标准 BFS 算法的逻辑几乎完全相同，主要的修改如下：

1. 给 State 类增加一个 distFromStart 字段，用于记录从起点到当前节点的路径权重和。使用
   优先级队列 代替普通队列，让 distFromStart 最小的节点优先出队
1. 用 distTo 数组替代 visited 数组。标准 BFS 算法中，是仅当 visited[node] == false 时才会让节点入队；Dijkstra 算法中是仅当节点能够让 distTo[node] 更小的时候才会让节点 State 入队
1. 在元素出队时，对 distTo[curNode] < curDistFromStart 的情况进行剪枝

整体的时间复杂度就是 O(ElogE)，空间复杂度是 O(V+E)

### 点对点优化

```java
// 计算 src 到 dst 的最短路径权重和
int dijkstra(Graph graph, int src, int dst) {
    while (!pq.isEmpty()) {
        State state = pq.poll();
        int curNode = state.node;
        int curDistFromStart = state.distFromStart;

        if (distTo[curNode] < curDistFromStart) {
            continue;
        }
        // 节点出队时进行判断，遇到终点时直接返回
        if (curNode == dst) {
            return curDistFromStart;
        }
        ...

    }
    return -1;
}
```

### 带限制的最短路径

每个节点自己维护了一个 State 对象，所以我们可以很容易地扩展标准的 Dijkstra 算法，完成更复杂的任务
举个简单的例子，现在不仅让你求最短路径，还要求最短路径最多不能超过 k 条边
这个场景下依然可以使用 Dijkstra 算法，但是需要修改 distTo 数组，且需要给 State 类增加额外的字段

```go
type State struct {
    node           int
    // 从起点到当前节点的路径权重和
    distFromStart  int
    // 从起点到当前节点经过的边的条数
    edgesFromStart int
}

func dijkstra(graph [][][]int, src int, k int) [][]int {
    n := len(graph)
    // distTo[i][j] 的值就是起点 src 到达节点 i 的最短路径权重和，且经过的边数不超过 j
    distTo := make([][]int, n)
    for i := range distTo {
        distTo[i] = make([]int, k+1)
        for j := range distTo[i] {
            distTo[i][j] = int(^uint(0) >> 1) // Integer.MAX_VALUE
        }
    }

    pq := &StatePQ{}
    heap.Init(pq)
    heap.Push(pq, &State{node: src, distFromStart: 0, edgesFromStart: 0})
    distTo[src][0] = 0

    for pq.Len() > 0 {
        state := heap.Pop(pq).(*State)
        curNode := state.node
        curDistFromStart := state.distFromStart
        curEdgesFromStart := state.edgesFromStart

        if distTo[curNode][curEdgesFromStart] < curDistFromStart {
            continue
        }

        for _, e := range graph[curNode] {
            nextNode := e[0]
            nextDistFromStart := curDistFromStart + e[1]
            nextEdgesFromStart := curEdgesFromStart + 1

            // 若已超过 k 条边，或无法优化路径权重和，直接跳过
            if nextEdgesFromStart > k || distTo[nextNode][nextEdgesFromStart] < nextDistFromStart {
                continue
            }

            heap.Push(pq, &State{node: nextNode, distFromStart: nextDistFromStart, edgesFromStart: nextEdgesFromStart})
            distTo[nextNode][nextEdgesFromStart] = nextDistFromStart
        }
    }

    return distTo
}

type StatePQ []*State

func (pq StatePQ) Len() int { return len(pq) }
func (pq StatePQ) Less(i, j int) bool { return pq[i].distFromStart < pq[j].distFromStart }
func (pq StatePQ) Swap(i, j int) { pq[i], pq[j] = pq[j], pq[i] }

func (pq *StatePQ) Push(x interface{}) {
    *pq = append(*pq, x.(*State))
}

func (pq *StatePQ) Pop() interface{} {
    old := *pq
    n := len(old)
    x := old[n-1]
    *pq = old[:n-1]
    return x
}
```

其中的关键修改有三个：

1. 给 State 类增加一个 edgesFromStart 字段，用于记录从起点到当前节点经过的边的条数
1. 把一维的 distTo 数组改成二维的 distTo 数组，其定义为：distTo[i][j] 表示起点 src 到达节点 i，且经过不超过 j 条边的最短路径权重和
1. 修改节点出队和入队时的条件，添加了对 k 条边的判断

## 其他算法

### A Star算法

对于任意节点 x，我们用 g(x) 表示从起点 src 到节点 x 的距离，用 h(x) 表示启发函数，用于估计从节点 x 到终点
dst 的距离

- 那么 Dijkstra 算法就是借助优先级队列，让 g(x) 最小的节点先出队，从而保证终点第一次出队时，就找到了最短路径
- 而 A\* 算法中稍作了一点改动：让 f(x)=g(x)+h(x) 最小的节点优先出队，终点第一次出队时，就找到了最短路径

仔细想想，f(x) 函数虽然简单，却非常精妙：

- 无论是接近终点还是远离终点，g(x) 肯定是不断增大的
- 但是接近终点时，h(x) 会逐渐减小；远离终点时，h(x) 会逐渐增大

也就是说，接近终点时 f(x) 的增速慢，节点更容易出队，算法也就会优先向终点的方向进行搜索；反之，远离终点时 f(x) 的增速快，搜索的优先级就会降低

### Bellman-Ford / SPFA 算法

当我们说 Bellman-Ford 算法时，一般是指 朴素 Bellman-Ford 算法
当我们说 SPFA 算法时，是指对朴素 Bellman-Ford 算法的一种优化，即 基于队列的 Bellman-Ford 算法

- 标准 BFS 算法使用一个布尔数组 visited 确保每个节点只会遍历一次，避免算法陷入死循环
- SPFA 算法中，需要使用 inQueue, count 数组配合，确保算法不会陷入死循环，同时用一个 distTo 数组记录起点到其他节点的最短路径

### Floyd算法

假设图中节点 i 和节点 j 之间存在最短路径，应该如何计算节点 i 到节点 j 的最短路径呢？

- 如果节点 i 和节点 j 之间有直接相连的边，那么节点 i 到节点 j 的最短路径就是这条边的权重
- 如果节点 i 和节点 j 之间没有直接相连的边，那么肯定要经过至少 1 个其他节点
- 对于任意一个其他节点 k，如果最短路径经过 k，那么节点 i 到节点 j 的最短路径就是节点 i 到节点 k 的最短路径加上节点 k 到节点 j 的最短路径

原问题「i 到 j 的最短路径」就变成了两个结构相同，规模更小的子问题「i 到 k 的最短路径」和「k 到 j 的最短路径」
