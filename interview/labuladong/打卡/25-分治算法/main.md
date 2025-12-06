# 分治算法

分而治之的思想是广泛存在的，并不是所有算法问题使用分治的思想都能带来效率的提升。但是有些问题，通过分治思想可以带来效率的提升
一般来说复杂度为多项式级别的算法，才有可能使用分治算法来提升效率

## 思想

分治思想就是把一个问题分解成若干个子问题，然后分别解决这些子问题，最后合并子问题的解得到原问题的解，这种思想广泛存在于递归算法中

- 比如斐波那契数列的递归解法，把原问题 fib(n) 分解成 fib(n-1) 和 fib(n-2) 两个子问题，根据子问题的解合并得到原问题的解，这就是分解问题的思路
- 比如让你计算一棵二叉树总共有多少个节点：把整棵树的节点个数（原问题）分解成了左子树的节点个数和右子树的节点个数（子问题），然后递归计算左右子树的节点个数，最后根据左右子树的节点个数得到整棵树的节点个数
- 所有动态规划算法都是把大问题分解成了结构相同规模更小的子问题，通过子问题的最优解合并得到原问题的最优解，只不过这个过程中有一些特殊的优化操作罢了

## 算法

狭义的分治算法也是运用分治思想的递归算法，但它有一个特征是上面列举的算法所不具备的：把问题分解后进行求解，相比于不分解直接求解，时间复杂度更低

### 无效分治

理论上讲，很多算法都可以用分解问题的思路改写成递归算法，但大部分情况下这种改写是无意义的

例如，求一个数组的和，这个算法的时间复杂度是 O(n)，空间复杂度是 O(1)：

```java
int getSum(int[] nums) {
    int sum = 0;
    for (int i = 0; i < nums.length; i++) {
        sum += nums[i];
    }
    return sum;
}
```

用分解问题的思路把这个问题改写成递归算法：

```java
// 定义：返回 nums[start..] 的元素和
int getSum2(int[] nums, int start) {
    // base case
    if (start == nums.length) {
        return 0;
    }
    // nums[start..] 的元素和可以分解成第一个元素和剩余元素的和
    return nums[start] + getSum2(nums, start + 1);
}
```

如果不考虑子数组复制所产生的复杂度，这个算法的时间复杂度是 O(n)，空间复杂度是 O(n)
这个算法的时间复杂度并没有比迭代算法更低，而且还多了 O(n) 的空间复杂度

### 有效分治

例子：LeetCode 21

这个算法使用两个指针，把两个链表都遍历了一遍，所以时间复杂度是 O(l1+l2)，l1 和 l2 分别是两个链表的长度
如何合并 k 个有序链表，先想一个暴力解，运用上面的这个 mergeTwoLists 函数把 k 个链表两两合并，都合并到第一个链表上：

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        // 虚拟头结点
        ListNode dummy = new ListNode(-1), p = dummy;
        ListNode p1 = l1, p2 = l2;

        while (p1 != null && p2 != null) {
            // 比较 p1 和 p2 两个指针
            // 将值较小的的节点接到 p 指针
            if (p1.val > p2.val) {
                p.next = p2;
                p2 = p2.next;
            } else {
                p.next = p1;
                p1 = p1.next;
            }
            // p 指针不断前进
            p = p.next;
        }

        if (p1 != null) {
            p.next = p1;
        }

        if (p2 != null) {
            p.next = p2;
        }

        return dummy.next;
    }
}

ListNode mergeKLists(ListNode[] lists) {
    if (lists.length == 0) {
        return null;
    }
    // 把 k 个有序链表都合并到 lists[0] 上
    ListNode l0 = lists[0];
    for (int i = 1; i < lists.length; i++) {
        l0 = mergeTwoLists(l0, lists[i]);
    }
    return l0;
}
```

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251206171305918.png)

如果将mergeKLists改为递归形式

```java
// 定义：合并 lists[start..] 为一个有序链表
ListNode mergeKLists2(ListNode[] lists, int start) {
    if (start == lists.length - 1) {
        return lists[start];
    }
    // 合并 lists[start + 1..] 为一个有序链表
    ListNode subProblem = mergeKLists2(lists, start + 1);
    // 合并 lists[start] 和 subProblem，就得到了 lists[start..] 的有序链表
    return mergeTwoLists(lists[start], subProblem);
}
```

不难发现重复的次数取决于树高，上面这个算法的递归树很不平衡，导致递归树退化成链表，树高变为 O(k)
如果能让递归树尽可能地平衡，就能减小树高，进而减少链表的重复遍历次数，提高算法的效率
如何让递归树平衡呢？把链表从中间分成两部分，分别递归合并为有两个序链表，最后再将这两部分合并成一个有序链表

```go
// 用分治算法合并 k 个有序链表
func mergeKLists(lists []*ListNode) *ListNode {
    if len(lists) == 0 {
        return nil
    }
    return mergeKLists3(lists, 0, len(lists)-1)
}

// 定义：合并 lists[start..end] 为一个有序链表
func mergeKLists3(lists []*ListNode, start, end int) *ListNode {
    if start == end {
        return lists[start]
    }
    mid := start + (end-start)/2
    // 合并左半边 lists[start..mid] 为一个有序链表
    left := mergeKLists3(lists, start, mid)
    // 合并右半边 lists[mid+1..end] 为一个有序链表
    right := mergeKLists3(lists, mid+1, end)
    // 合并左右两个有序链表
    return mergeTwoLists(left, right)
}

// 双指针技巧合并两个有序链表
// https://labuladong.online/algo/essential-technique/linked-list-skills-summary/
func mergeTwoLists(l1, l2 *ListNode) *ListNode {
    dummy := &ListNode{Val: -1}
    p := dummy
    p1, p2 := l1, l2

    for p1 != nil && p2 != nil {
        if p1.Val > p2.Val {
            p.Next = p2
            p2 = p2.Next
        } else {
            p.Next = p1
            p1 = p1.Next
        }
        p = p.Next
    }

    if p1 != nil {
        p.Next = p1
    }

    if p2 != nil {
        p.Next = p2
    }

    return dummy.Next
}
```

该算法的空间复杂度只有递归树堆栈的开销，也就是 O(logk)，要优于优先级队列解法的O(k)
该算法的时间复杂度相当于是把 k 条链表分别遍历 O(logk) 次
那么假设 k 条链表的元素总数是 N，该算法的时间复杂度就是 O(Nlogk)，和单链表双指针技巧汇总中介绍的优先级队列解法相同
