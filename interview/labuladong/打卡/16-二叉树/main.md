# 二叉树

二叉树只有递归遍历和层序遍历这两种，再无其他。递归遍历可以衍生出 DFS 算法，层序遍历可以衍生出 BFS 算法

递归遍历二叉树节点的顺序是固定的，但是有三个关键位置，在不同位置插入代码，会产生不同的效果

层序遍历二叉树节点的顺序也是固定的，但是有三种不同的写法，对应不同的场景

## 遍历

### DFS

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251126121125043.jpeg)

```go
// 基本的二叉树节点
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

// 二叉树的递归遍历框架
func traverse(root *TreeNode) {
    if root == nil {
        return
    }
    // 前序位置
    traverse(root.Left)
    // 中序位置
    traverse(root.Right)
    // 后序位置
}
```

递归遍历的顺序，即 traverse 函数访问节点的顺序确实是固定的。二叉搜索树（BST）的中序遍历结果是有序的，这是 BST 的一个重要性质

### BFS

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251126121402135.jpeg)

```go
type State struct {
    node  *TreeNode
    depth int
}

func levelOrderTraverse(root *TreeNode) {
    if root == nil {
        return
    }
    q := []State{{root, 1}}

    for len(q) > 0 {
        cur := q[0]
        q = q[1:]
        // 访问 cur 节点，同时知道它的路径权重和
        fmt.Printf("depth = %d, val = %d\n", cur.depth, cur.node.Val)

        // 把 cur 的左右子节点加入队列
        if cur.node.Left != nil {
            q = append(q, State{cur.node.Left, cur.depth + 1})
        }
        if cur.node.Right != nil {
            q = append(q, State{cur.node.Right, cur.depth + 1})
        }
    }
}
```

## 算法核心

二叉树解题的思维模式分两类：

1. 是否可以通过遍历一遍二叉树得到答案？如果可以，用一个 traverse 函数配合外部变量来实现，这叫「遍历」的思维模式

1. 是否可以定义一个递归函数，通过子问题（子树）的答案推导出原问题的答案？如果可以，写出这个递归函数的定义，并充分利用这个函数的返回值，这叫「分解问题」的思维模式

无论使用哪种思维模式，你都需要思考：如果单独抽出一个二叉树节点，它需要做什么事情？需要在什么时候（前/中/后序位置）做？其他的节点不用你操心，递归函数会帮你在所有节点上执行相同的操作


二叉树这种结构无非就是二叉链表，它没办法简单改写成 for 循环的迭代形式，所以我们遍历二叉树一般都使用递归形式。所谓前序位置，就是刚进入一个节点（元素）的时候，后序位置就是即将离开一个节点（元素）的时候，那么进一步，你把代码写在不同位置，代码执行的时机也不同：
