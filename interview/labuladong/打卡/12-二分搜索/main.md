# 二分搜索

二分思维的精髓就是：通过已知信息尽可能多地收缩（折半）搜索空间，从而增加穷举效率，快速找到目标

## 代码模版

### 寻找一个数（基本的二分搜索）

```go
func binarySearch(nums []int, target int) int {
    left, right := 0, len(nums)-1

    for left <= right {
        mid := left + (right-left)/2
        if nums[mid] == target {
            ...
        } else if nums[mid] < target {
            left = ...
        } else if nums[mid] > target {
            right = ...
        }
    }
    return ...
}
```

- 分析二分查找的一个技巧是：不要出现 else，而是把所有情况用 else if 写清楚，这样可以清楚地展现所有细节。本文都会使用 else if，旨在讲清楚

- 其中 ... 标记的部分，就是可能出现细节问题的地方，当你见到一个二分查找的代码时，首先注意这几个地方

- 计算 mid 时需要防止溢出，代码中 left + (right - left) / 2 就和 (left + right) / 2 的结果相同，但是有效防止了 left 和 right 太大，直接相加导致溢出的情况

#### 为什么 while 循环的条件是 \<= 而不是 \<

因为初始化 right 的赋值是 nums.length - 1，即最后一个元素的索引，而不是 nums.length

这二者可能出现在不同功能的二分查找中，区别是：前者相当于两端都闭区间 [left, right]，后者相当于左闭右开区间 \[left, right)。因为索引大小为 nums.length 是越界的，所以我们把 right 这一边视为开区间。我们这个算法中使用的是前者 [left, right] 两端都闭的区间。这个区间其实就是每次进行搜索的区间

#### 为什么是 left = mid + 1，right = mid - 1？

有的代码是 right = mid 或者 left = mid，没有这些加加减减，到底怎么回事，怎么判断？

刚才明确了「搜索区间」这个概念，而且本算法的搜索区间是两端都闭的，即 [left, right]。那么当我们发现索引 mid 不是要找的 target 时，下一步应该去搜索哪里呢？当然是去搜索区间 [left, mid-1] 或者区间 [mid+1, right] 对不对？因为 mid 已经搜索过，应该从搜索区间中去除

### 寻找左侧边界的二分搜索

```go
func left_bound(nums []int, target int) int {
    left := 0
    // 注意
    right := len(nums)

    // 注意
    for left < right {
        mid := left + (right - left) / 2
        if nums[mid] == target {
            right = mid
        } else if nums[mid] < target {
            left = mid + 1
        } else if nums[mid] > target {
            // 注意
            right = mid
        }
    }
    return left
}
```

#### 为什么 while 中是 < 而不是 \<=？

因为 right = nums.length 而不是 nums.length - 1。因此每次循环的「搜索区间」是 \[left, right) 左闭右开

while(left < right) 终止的条件是 left == right，此时搜索区间 \[left, left) 为空，所以可以正确终止

#### target 不存在时返回什么？

如果 target 不存在，搜索左侧边界的二分搜索返回的索引是大于 target 的最小索引。举个例子，nums = [2,3,5,7], target = 4，left_bound 函数返回值是 2，因为元素 5 是大于 4 的最小元素

如果想让 target 不存在时返回 -1 其实很简单，在返回的时候额外判断一下 nums[left] 是否等于 target 就行了，如果不等于，就说明 target 不存在。需要注意的是，访问数组索引之前要保证索引不越界

#### 为什么是 left = mid + 1 和 right = mid？

因为我们的「搜索区间」是 \[left, right) 左闭右开，所以当 nums[mid] 被检测之后，下一步应该去 mid 的左侧或者右侧区间搜索，即 \[left, mid) 或 \[mid + 1, right)

#### 为什么该算法能够搜索左侧边界？

关键在于对于 nums[mid] == target 这种情况的处理：

```
if (nums[mid] == target)
    right = mid;
```

可见，找到 target 时不要立即返回，而是缩小「搜索区间」的上界 right，在区间 \[left, mid) 中继续搜索，即不断向左收缩，达到锁定左侧边界的目的

#### 为什么返回 left 而不是 right？

都是一样的，因为 while 终止的条件是 left == right

```go
// 将给定的数字 nums 搜索 target 的左侧边界
func left_bound(nums []int, target int) int {
    left := 0
    right := len(nums) - 1
    // 搜索区间为 [left, right]
    for left <= right {
        mid := left + (right - left) / 2
        if nums[mid] < target {
            // 搜索区间变为 [mid+1, right]
            left = mid + 1
        } else if nums[mid] > target {
            // 搜索区间变为 [left, mid-1]
            right = mid - 1
        } else if nums[mid] == target {
            // 收缩右侧边界
            right = mid - 1
        }
    }
    // 判断 target 是否存在于 nums 中
    if left < 0 || left >= len(nums) {
        return -1
    }
    // 如果越界，target 肯定不存在，返回 -1
    if nums[left] == target {
        // 判断一下 nums[left] 是不是 target
        return left
    }
    return -1
}
```

### 寻找右侧边界的二分搜索

```go
func right_bound(nums []int, target int) int {
    left, right := 0, len(nums)
    for left < right {
        mid := left + (right - left) / 2
        if nums[mid] == target {
            // 注意
            left = mid + 1
        } else if nums[mid] < target {
            left = mid + 1
        } else if nums[mid] > target {
            right = mid
        }
    }
    // 注意
    return left - 1
}
```

#### 为什么这个算法能够找到右侧边界？

关键点还是这里：

```
if (nums[mid] == target) {
    left = mid + 1;
}
```

当 nums[mid] == target 时，不要立即返回，而是增大「搜索区间」的左边界 left，使得区间不断向右靠拢，达到锁定右侧边界的目的

#### 为什么返回 left - 1？

为什么最后返回 left - 1 而不像左侧边界的函数，返回 left？而且我觉得这里既然是搜索右侧边界，应该返回 right 才对

首先，while 循环的终止条件是 left == right，所以 left 和 right 是一样的，你非要体现右侧的特点，返回 right - 1 好了

至于为什么要减一，这是搜索右侧边界的一个特殊点，关键在锁定右边界时的这个条件判断：

```
// 增大 left，锁定右侧边界
if (nums[mid] == target) {
    left = mid + 1;
    // 这样想: mid = left - 1
}
```

因为我们对 left 的更新必须是 left = mid + 1，就是说 while 循环结束时，nums[left] 一定不等于 target 了，而 nums[left-1] 可能是 target

至于为什么 left 的更新必须是 left = mid + 1，当然是为了把 nums[mid] 排除出搜索区间

#### 如果 target 不存在时返回什么？

如果 target 不存在，搜索右侧边界的二分搜索返回的索引是小于 target 的最大索引。比如 nums = [2,3,5,7], target = 4，right_bound 函数返回值是 1，因为元素 3 是小于 4 的最大元素

如果你想在 target 不存在时返回 -1，很简单，只要在最后判断一下 nums[left-1] 是不是 target 就行了，类似之前的左侧边界搜索，做一点额外的判断即可

### 统一化

```go
func binarySearch(nums []int, target int) int {
    left, right := 0, len(nums)-1
    for left <= right {
        mid := left + (right - left) / 2
        if nums[mid] < target {
            left = mid + 1
        } else if nums[mid] > target {
            right = mid - 1
        } else if nums[mid] == target {
            // 直接返回
            return mid
        }
    }
    // 直接返回
    return -1
}

func leftBound(nums []int, target int) int {
    left, right := 0, len(nums)-1
    for left <= right {
        mid := left + (right - left) / 2
        if nums[mid] < target {
            left = mid + 1
        } else if nums[mid] > target {
            right = mid - 1
        } else if nums[mid] == target {
            // 别返回，锁定左侧边界
            right = mid - 1
        }
    }
    // 判断 target 是否存在于 nums 中
    if left < 0 || left >= len(nums) {
        return -1
    }
    // 判断一下 nums[left] 是不是 target
    if nums[left] == target {
        return left
    }
    return -1
}

func rightBound(nums []int, target int) int {
    left, right := 0, len(nums)-1
    for left <= right {
        mid := left + (right - left) / 2
        if nums[mid] < target {
            left = mid + 1
        } else if nums[mid] > target {
            right = mid - 1
        } else if nums[mid] == target {
            // 别返回，锁定右侧边界
            left = mid + 1
        }
    }
    if right < 0 || right >= len(nums) {
        return -1
    }
    if nums[right] == target {
        return right
    }
    return -1
}
```

## 思维框架

### 原始的二分搜索代码

二分搜索的原型就是在「有序数组」中搜索一个元素 target，返回该元素对应的索引

如果该元素不存在，那可以返回一个什么特殊值，这种细节问题只要微调算法实现就可实现

还有一个重要的问题，如果「有序数组」中存在多个 target 元素，那么这些元素肯定挨在一起，这里就涉及到算法应该返回最左侧的那个 target 元素的索引还是最右侧的那个 target 元素的索引，也就是所谓的「搜索左侧边界」和「搜索右侧边界」，这个也可以通过微调算法的代码来实现

在具体的算法问题中，常用到的是「搜索左侧边界」和「搜索右侧边界」这两种场景，很少有让你单独「搜索一个元素」。因为算法题一般都让你求最值，比如让你求吃香蕉的「最小速度」，让你求轮船的「最低运载能力」，求最值的过程，必然是搜索一个边界的过程

### 二分搜索问题的泛化

首先，你要从题目中抽象出一个自变量 x，一个关于 x 的函数 f(x)，以及一个目标值 target

同时，x, f(x), target 还要满足以下条件：

1. f(x) 必须是在 x 上的单调函数（单调增单调减都可以）

1. 题目是让你计算满足约束条件 f(x) == target 时的 x 的值

### 运用二分搜索的套路框架

```go
// 函数 f 是关于自变量 x 的单调函数
func f(x int) int {
    // ...
}

// 主函数，在 f(x) == target 的约束下求 x 的最值
func solution(nums []int, target int) int {
    if len(nums) == 0 {
        return -1
    }
    // 问自己：自变量 x 的最小值是多少？
    left := ...
    // 问自己：自变量 x 的最大值是多少？
    right := ... + 1

    for left < right {
        mid := left + (right - left) / 2
        if f(mid) == target {
            // 问自己：题目是求左边界还是右边界？
            // ...
        } else if f(mid) < target {
            // 问自己：怎么让 f(x) 大一点？
            // ...
        } else if f(mid) > target {
            // 问自己：怎么让 f(x) 小一点？
            // ...
        }
    }
    return left
}
```
