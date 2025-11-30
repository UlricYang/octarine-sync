# 二叉搜索树

Binary Search Tree 简称BST

## 特性

1. 对于 BST 的每一个节点 node，左子树节点的值都比 node 的值要小，右子树节点的值都比 node 的值大
1. 对于 BST 的每一个节点 node，它的左侧子树和右侧子树都是 BST

BST 的中序遍历结果是有序的（升序）

BST 相关的问题，要么利用 BST 左小右大的特性提升算法效率，要么利用中序遍历的特性满足题目的要求

## 基操

### 判断BST的合法性

我们通过使用辅助函数，增加函数参数列表，在参数中携带额外信息，将这种约束传递给子树的所有节点，这也是二叉树算法的一个小技巧

```go
func isValidBST(root *TreeNode) bool {
    return _isValidBST(root, nil, nil)
}

// 定义：该函数返回 root 为根的子树的所有节点是否满足 max.Val > root.Val > min.Val
func _isValidBST(root *TreeNode, min, max *TreeNode) bool {
    // base case
    if root == nil {
        return true
    }
    // 若 root.Val 不符合 max 和 min 的限制，说明不是合法 BST
    if min != nil && root.Val <= min.Val {
        return false
    }
    if max != nil && root.Val >= max.Val {
        return false
    }
    // 根据定义，限定左子树的最大值是 root.Val，右子树的最小值是 root.Val
    return _isValidBST(root.Left, min, root) && _isValidBST(root.Right, root, max)
}
```

### 在BST中搜索元素

其实不需要递归地搜索两边，类似二分查找思想，根据 target 和 root.val 的大小比较，就能排除一边

```go
// 定义：在以 root 为根的 BST 中搜索值为 target 的节点，返回该节点
func searchBST(root *TreeNode, target int) *TreeNode {
    // 如果根节点为空，返回空
    if root == nil {
        return nil
    }
    // 去左子树搜索
    if root.val > target {
        return searchBST(root.left, target)
    }
    // 去右子树搜索
    if root.val < target {
        return searchBST(root.right, target)
    }
    // 当前节点就是目标值
    return root
}
```

### 在BST中插入一个数

对数据结构的操作无非遍历 + 访问，遍历就是「找」，访问就是「改」。具体到这个问题，插入一个数，就是先找到插入位置，然后进行插入操作

```go
// 定义：在以 root 为根的 BST 中插入 val 节点，返回插入后的根节点
func insertIntoBST(root *TreeNode, val int) *TreeNode {
    if root == nil {
        // 找到空位置插入新节点
        return &TreeNode{Val: val}
    }
    // 去右子树找插入位置
    if root.Val < val {
        root.Right = insertIntoBST(root.Right, val)
    }
    // 去左子树找插入位置
    if root.Val > val {
        root.Left = insertIntoBST(root.Left, val)
    }
    // 返回 root，上层递归会接收返回值作为子节点
    return root
}
```

### 在BST中删除一个数

```go
// 定义：在以 root 为根的 BST 中删除值为 key 的节点，返回完成删除后的根节点
func deleteNode(root *TreeNode, key int) *TreeNode {
    if root == nil {
        return nil
    }
    if root.Val == key {
        // 这两个 if 把情况 1 和 2 都正确处理了
        if root.Left == nil {
            return root.Right
        }
        if root.Right == nil {
            return root.Left
        }
        // 处理情况 3
        // 获得右子树最小的节点
        minNode := getMin(root.Right)
        // 删除右子树最小的节点
        root.Right = deleteNode(root.Right, minNode.Val)
        // 用右子树最小的节点替换 root 节点
        minNode.Left = root.Left
        minNode.Right = root.Right
        root = minNode
    } else if root.Val > key {
        root.Left = deleteNode(root.Left, key)
    } else if root.Val < key {
        root.Right = deleteNode(root.Right, key)
    }
    return root
}

// 获得二叉搜索树中最小的节点
func getMin(node *TreeNode) *TreeNode {
    // BST 最左边的就是最小的
    for node.Left != nil {
        node = node.Left
    }
    return node
}
```

## 构造

思路：

1. 穷举 root 节点的所有可能
1. 递归构造出左右子树的所有有效 BST
1. 给 root 节点穷举所有左右子树的组合

```go
func generateTrees(n int) []*TreeNode {
    if n == 0 {
        return []*TreeNode{}
    }
    // 构造闭区间 [1, n] 组成的 BST
    return build(1, n)
}

// 构造闭区间 [lo, hi] 组成的 BST
func build(lo int, hi int) []*TreeNode {
    res := []*TreeNode{}
    // base case
    if lo > hi {
        // 这里需要装一个 null 元素，这样才能让下面的两个内层 for 循环都能进入，正确地创建出叶子节点
        // 举例来说吧，什么时候会进到这个 if 语句？当你创建叶子节点的时候，对吧
        // 那么如果你这里不加 null，直接返回空列表，那么下面的内层两个 for 循环都无法进入
        // 你的那个叶子节点就没有创建出来，看到了吗？所以这里要加一个 null，确保下面能把叶子节点做出来
        res = append(res, nil)
        return res
    }
    // 1、穷举 root 节点的所有可能
    for i := lo; i <= hi; i++ {
        // 2、递归构造出左右子树的所有有效 BST
        leftTree := build(lo, i - 1)
        rightTree := build(i + 1, hi)
        // 3、给 root 节点穷举所有左右子树的组合
        for _, left := range leftTree {
            for _, right := range rightTree {
                // i 作为根节点 root 的值
                root := &TreeNode{Val: i}
                root.Left = left
                root.Right = right
                res = append(res, root)
            }
        }
    }
    return res
}
```

## 后序

其实二叉树的题目真的不难，无非就是前中后序遍历框架来回倒嘛，只要你把一个节点该做的事情安排好，剩下的抛给递归框架即可。但是对于有的题目，不同的遍历顺序时间复杂度不同。尤其是这个后序位置的代码，有时候可以大幅提升算法效率

如果当前节点要做的事情需要通过左右子树的计算结果推导出来，就要用到后序遍历

我们要尽可能避免递归函数中调用其他递归函数，如果出现这种情况，大概率是代码实现有瑕疵
