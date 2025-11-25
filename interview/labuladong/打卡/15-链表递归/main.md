# 单链表的花式反转

## 反转整个链表

### 迭代解法

这道题的常规做法就是迭代解法，通过操作几个指针，将链表中的每个节点的指针方向反转，没什么难点，主要是指针操作的细节问题

```go
// 反转以 head 为起点的单链表
func reverseList(head *ListNode) *ListNode {
    if head == nil || head.Next == nil {
        return head
    }
    // 由于单链表的结构，至少要用三个指针才能完成迭代反转
    // cur 是当前遍历的节点，pre 是 cur 的前驱结点，nxt 是 cur 的后继结点
    var pre, cur, nxt *ListNode
    pre, cur, nxt = nil, head, head.Next
    for cur != nil {
        // 逐个结点反转
        cur.Next = pre
        // 更新指针位置
        pre = cur
        cur = nxt
        if nxt != nil {
            nxt = nxt.Next
        }
    }
    // 返回反转后的头结点
    return pre
}
```

操作指针的时候，有一些很基本很简单的小技巧，可以让你写代码的思路更清晰：

1. 一旦出现类似 nxt.Next 这种操作，就要条件反射地想到，先判断 nxt 是否为 null，否则容易出现空指针异常
1. 注意循环的终止条件。你要知道循环终止时，各个指针的位置，这样才能保返回正确的答案。如果你觉得有点复杂想不清楚，那就动手画一个最简单的场景跑一下算法，比如这道题就可以画一个只有两个节点的单链表 1->2，然后就能确定循环终止后各个指针的位置了

### 递归解法

递归反转单链表的关键在于，这个问题本身是存在子问题结构的。这就是「分解问题」的思路，通过递归函数的定义，把原问题分解成若干规模更小. 结构相同的子问题，最后通过子问题的答案组装原问题的解

```
reverseList(1->2->3->4) = reverseList(2->3->4) -> 1
```

```go
// 定义：输入一个单链表头结点，将该链表反转，返回新的头结点
func reverseList(head *ListNode) *ListNode {
	if head == nil || head.Next == nil {
		return head
	}
	last := reverseList(head.Next)
	head.Next.Next = head
	head.Next = nil
	return last
}
```

## 反转链表前N个节点

### 迭代解法

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251124143841880.jpeg)

```go
func reverseN(head *ListNode, n int) *ListNode {
    if head == nil || head.Next == nil {
        return head
    }
    var pre, cur, nxt *ListNode
    pre, cur, nxt = nil, head, head.Next
    for n > 0 {
        cur.Next = pre
        pre = cur
        cur = nxt
        if nxt != nil {
            nxt = nxt.Next
        }
        n--
    }
    // 此时的 cur 是第 n + 1 个节点，head 是反转后的尾结点
    head.Next = cur
    // 此时的 pre 是反转后的头结点
    return pre
}
```

### 递归解法

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251124144139474.jpeg)

```go
// 后驱节点
var successor *ListNode
// 反转以 head 为起点的 n 个节点，返回新的头结点
func reverseN(head *ListNode, n int) *ListNode {
    if n == 1 {
        // 记录第 n + 1 个节点
        successor = head.Next
        return head
    }
    // 以 head.Next 为起点，需要反转前 n - 1 个节点
    last := reverseN(head.Next, n - 1)
    head.Next.Next = head
    // 让反转之后的 head 节点和后面的节点连起来
    head.Next = successor
    return last
}
```

## 反转链表一部分

给你一个索引区间，让你把单链表中这部分元素反转，其他部分不变

### 迭代解法

```go
func reverseBetween(head *ListNode, m int, n int) *ListNode {
    if m == 1 {
        return reverseN(head, n)
    }
    // 找到第 m 个节点的前驱
    pre := head
    for i := 1; i < m - 1; i++ {
        pre = pre.Next
    }
    // 从第 m 个节点开始反转
    pre.Next = reverseN(pre.Next, n - m + 1)
    return head
}

func reverseN(head *ListNode, n int) *ListNode {
    if head == nil || head.Next == nil {
        return head
    }
    pre, cur, nxt := (*ListNode)(nil), head, head.Next
    for n > 0 {
        cur.Next = pre
        pre = cur
        cur = nxt
        if nxt != nil {
            nxt = nxt.Next
        }
        n--
    }
    // 此时的 cur 是第 n + 1 个节点，head 是反转后的尾结点
    head.Next = cur
    // 此时的 pre 是反转后的头结点
    return pre
}
```

### 递归解法

```go
func reverseBetween(head *ListNode, m int, n int) *ListNode {
    // 后驱节点
    var successor *ListNode

    // 反转以 head 为起点的 n 个节点，返回新的头结点
    var reverseN func(head *ListNode, n int) *ListNode
    reverseN = func(head *ListNode, n int) *ListNode {
        if n == 1 {
            // 记录第 n + 1 个节点
            successor = head.Next
            return head
        }
        last := reverseN(head.Next, n - 1)

        head.Next.Next = head
        head.Next = successor
        return last
    }

    // base case
    if m == 1 {
        return reverseN(head, n)
    }

    // 前进到反转的起点触发 base case
    head.Next = reverseBetween(head.Next, m - 1, n - 1)
    return head
}
```

```go

```

## K个一组反转

大致的算法流程：

1. 先反转以 head 开头的 k 个元素。这里可以复用前面实现的 reverseN 函数
1. 将第 k + 1 个元素作为 head 递归调用 reverseKGroup 函数
1. 将上述两个过程的结果连接起来

```go
func reverseKGroup(head *ListNode, k int) *ListNode {
    if head == nil {
        return nil
    }
    // 区间 [a, b) 包含 k 个待反转元素
    a, b := head, head
    for i := 0; i < k; i++ {
        // 不足 k 个，不需要反转了
        if b == nil {
            return head
        }
        b = b.Next
    }
    // 反转前 k 个元素
    newHead := reverseN(a, k)
    // 此时 b 指向下一组待反转的头结点
    // 递归反转后续链表并连接起来
    a.Next = reverseKGroup(b, k)
    return newHead
}

// 上文实现的反转前 N 个节点的函数
func reverseN(head *ListNode, n int) *ListNode {
    if head == nil || head.Next == nil {
        return head
    }
    pre, cur, nxt := (*ListNode)(nil), head, head.Next
    for n > 0 {
        cur.Next = pre
        pre = cur
        cur = nxt
        if nxt != nil {
            nxt = nxt.Next
        }
        n--
    }
    head.Next = cur
    return pre
}
```

## 回文链表

寻找回文串的核心思想是从中心向两端扩展：

```go
// 在 s 中寻找以 s[left] 和 s[right] 为中心的最长回文串
func palindrome(s string, left int, right int) string {
    // 防止索引越界
    for left >= 0 && right < len(s) && s[left] == s[right] {
        // 双指针，向两边展开
        left--
        right++
    }
    // 返回以 s[left] 和 s[right] 为中心的最长回文串
    return s[(left + 1):right]
}
```

### 模仿双指针实现回文判断

```go
func isPalindrome(head *ListNode) bool {
    // 从左向右移动的指针
    left := head
    // 从右向左移动的指针
    var right *ListNode
    // 记录链表是否为回文
    res := true
    var traverse func(right *ListNode)
    traverse = func(right *ListNode) {
        if right == nil {
            return
        }
        // 利用递归，走到链表尾部
        traverse(right.Next)
        // 后序遍历位置，此时的 right 指针指向链表右侧尾部
        // 所以可以和 left 指针比较，判断是否是回文链表
        if left.Val != right.Val {
            res = false
        }
        left = left.Next
    }
    right = head
    traverse(right)
    return res
}
```

### 优化空间复杂度

```go
func isPalindrome(head *ListNode) bool {
	var slow, fast *ListNode
	slow, fast = head, head
	for fast != nil && fast.Next != nil {
		slow = slow.Next
		fast = fast.Next.Next
	}

	if fast != nil {
		slow = slow.Next
	}

	left, right := head, reverse(slow)
	for right != nil {
		if left.Val != right.Val {
			return false
		}
		left = left.Next
		right = right.Next
	}

	return true
}

func reverse(head *ListNode) *ListNode {
	var pre, cur *ListNode
	pre, cur = nil, head
	for cur != nil {
		next := cur.Next
		cur.Next = pre
		pre = cur
		cur = next
	}
	return pre
}
```
