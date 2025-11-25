# 随机算法

## 不带权重

### 洗牌算法

避开「在数组中随机选择 k 个元素」这个问题，把问题转化成「如何随机打乱一个数组」

现在想随机初始化 k 颗雷的位置，你可以先把这 k 颗雷放在 board 开头，然后把 board 数组随机打乱，这样雷不就随机分布到 board 数组的各个地方了吗？洗牌算法，或者叫随机乱置算法就是专门解决这个问题的

洗牌算法的时间复杂度是 O(N)，而且逻辑很简单

分析洗牌算法正确性的准则：产生的结果必须有 n! 种可能。这个很好解释，因为一个长度为 n 的数组的全排列就有 n! 种，也就是说打乱结果总共有 n! 种。算法必须能够反映这个事实，才是正确的

```go
type Solution struct {
    nums []int
    rand *rand.Rand
}

func Constructor(nums []int) Solution {
    return Solution{nums: nums, rand: rand.New(rand.NewSource(time.Now().UnixNano()))}
}

func (s *Solution) Reset() []int {
    return s.nums
}

// 洗牌算法
func (s *Solution) Shuffle() []int {
    n := len(s.nums)
    var cp = make([]int, n)
    copy(cp, s.nums)
    for i := 0; i < n; i++ {
        // 生成一个 [i, n-1] 区间内的随机数
        r := i + s.rand.Intn(n - i)
        // 交换 cp[i] 和 cp[r]
        cp[i], cp[r] = cp[r], cp[i]
    }
    return cp
}
```

### 水塘抽样算法

常见的随机抽样场景，常用的解法是水塘抽样算法（Reservoir Sampling）。水塘抽样算法是一种随机概率算法

- 当你遇到第 i 个元素时，应该有 1/i 的概率选择该元素，1 - 1/i 的概率保持原有的选择
- 同理，如果要在单链表中随机选择 k 个数，只要在第 i 个元素处以 k/i 的概率选择该元素，以 1 - k/i 的概率保持原有选择即可

```go
// 返回链表中 k 个随机节点的值
func getRandom(head *ListNode, k int) []int {
    r := rand.New(rand.NewSource(time.Now().UnixNano()))
    res := make([]int, k)
    p := head

    // 前 k 个元素先默认选上
    for i := 0; i < k && p != nil; i++ {
        res[i] = p.Val
        p = p.Next
    }

    i := k
    // while 循环遍历链表
    for p != nil {
        i++
        // 生成一个 [0, i) 之间的整数
        j := r.Intn(i)
        // 这个整数小于 k 的概率就是 k/i
        if j < k {
            res[j] = p.Val
        }
        p = p.Next
    }
    return res
}
```

### 蒙特卡洛验证法

记得高中有道数学题：往一个正方形里面随机打点，这个正方形里紧贴着一个圆，告诉你打点的总数和落在圆里的点的数量，让你计算圆周率

这其实就是利用了蒙特卡罗方法：当打的点足够多的时候，点的数量就可以近似代表图形的面积。结合面积公式，可以很容易通过正方形和圆中点的数量比值推出圆周率的。当然，打的点越多，算出的圆周率越准确，充分体现了大力出奇迹的道理

## 带权重

核心思路主要分几步：

1. 根据权重数组 w 生成前缀和数组 preSum

1. 生成一个取值在 preSum 之内的随机数，用二分搜索算法寻找大于等于这个随机数的最小元素索引

1. 最后对这个索引减一（因为前缀和数组有一位索引偏移），就可以作为权重数组的索引，即最终答案:
