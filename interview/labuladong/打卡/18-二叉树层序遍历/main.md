# 二叉树层序遍历

二叉树大部分题目都可以用递归的算法解决，但少部分题目用递归比较麻烦的话，我们可以考虑使用层序遍历的方式解决

## BFS

```go
func levelOrder(root *TreeNode) [][]int {
    var res [][]int
    if root == nil {
        return res
    }
    q := []*TreeNode{root}
    // while 循环控制从上向下一层层遍历
    for len(q) > 0 {
        sz := len(q)
        // 记录这一层的节点值
        level := make([]int, sz)
        // for 循环控制每一层从左向右遍历
        for i := 0; i < sz; i++ {
            cur := q[0]
            q = q[1:]
            level[i] = cur.Val
            if cur.Left != nil {
                q = append(q, cur.Left)
            }
            if cur.Right != nil {
                q = append(q, cur.Right)
            }
        }
        res = append(res, level)
    }
    return res
}
```

## 二叉树节点编号

编号还可以通过父节点的编号计算得出左右子节点的编号：假设父节点的编号是 x，左子节点就是 2 _ x，右子节点就是 2 _ x + 1

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251129010601840.png)

如果按照 BFS 层序遍历的方式遍历完全二叉树，队列最后留下的应该都是空指针：

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251129010714037.jpeg)
