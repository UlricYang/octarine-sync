# 差分数组

差分数组的主要适用场景是频繁对原始数组的某个区间的元素进行增减

构造差分数组 diff，就可以快速进行区间增减的操作，如果你想对区间 nums[i..j] 的元素全部加 3，那么只需要让 diff[i] += 3，然后再让 diff[j+1] -= 3 即可

只要花费 O(1) 的时间修改 diff 数组，就相当于给 nums 的整个区间做了修改。多次修改 diff，然后通过 diff 数组反推，即可得到 nums 修改后的结果

第一个问题，想要使用差分数组技巧，必须创建一个长度和区间长度一样的差分数组 diff，那如果我有一个非常大的区间，比如 [0, 10^9]，那岂不是上来就要创建一个长度为 10^9 的数组，才能开始区间增减操作？

第二个问题，前缀和技巧可以快速进行区间查询，差分数组可以快速进行区间增减。能不能把他俩结合起来，既可以快速进行区间增减，又可以随时进行区间查询？

其实这两个问题是处理区间问题的常见问题，终极答案是 线段树 这种数据结构，它可以在 O(logN) 的时间复杂度内完成任意长度的区间增减和区间查询操作

```go
// 差分数组工具类
type Difference struct {
    // 差分数组
    diff []int
}

// 输入一个初始数组，区间操作将在这个数组上进行
func NewDifference(nums []int) *Difference {
    diff := make([]int, len(nums))
    // 根据初始数组构造差分数组
    diff[0] = nums[0]
    for i := 1; i < len(nums); i++ {
        diff[i] = nums[i] - nums[i-1]
    }
    return &Difference{diff: diff}
}

// 给闭区间 [i, j] 增加 val（可以是负数）
func (d *Difference) Increment(i, j, val int) {
    d.diff[i] += val
    if j+1 < len(d.diff) {
        d.diff[j+1] -= val
    }
}

// 返回结果数组
func (d *Difference) Result() []int {
    res := make([]int, len(d.diff))
    // 根据差分数组构造结果数组
    res[0] = d.diff[0]
    for i := 1; i < len(d.diff); i++ {
        res[i] = res[i-1] + d.diff[i]
    }
    return res
}
```
