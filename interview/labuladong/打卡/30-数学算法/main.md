# 数学算法

## 技巧

### 模运算

加法：(a + b) % k = (a % k + b % k) % k
乘法：(a _ b) % k = (a % k) _ (b % k) % k
减法：(a - b) % k = (a % k - b % k + k) % k

### 快速幂

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251211140345949.png)

```java
// 计算 (a ^ b) % k
long quickPow(long a, long b, long k) {
    long res = 1;
    // 防止 a 大于 k
    a %= k;
    while (b > 0) {
        // 判断奇数，等价于 b % 2 == 1
        if ((b & 1) == 1) {
            res = (res * a) % k;
        }
        a = (a * a) % k;
        // 右移一位，相当于除以 2
        b >>= 1;
    }
    return res;
}
```

### 最大公约数

最常用的是欧几里得算法（辗转相除法）：gcd(a, b) = gcd(b, a % b)，直到 b == 0，此时 a 就是最大公约数

```java
int gcd(int a, int b) {
    if (b == 0) return a;
    return gcd(b, a % b);
}
```

### 最小公倍数

lcm(a,b) = (a/gcd(a,b)) \* b

## 素数

如果在 [2,sqrt(n)] 这个区间之内没有发现可整除因子，就可以直接断定 n 是素数了，因为在区间 [sqrt(n),n] 也一定不会发现可整除因子

Sieve of Eratosthenes

```go
func countPrimes(n int) int {
    isPrime := make([]bool, n)
    for i := 0; i < n; i++ {
        isPrime[i] = true
    }
    for i := 2; i * i < n; i++ {
        if isPrime[i] {
            for j := i * i; j < n; j += i {
                isPrime[j] = false
            }
        }
    }
    count := 0
    for i := 2; i < n; i++ {
        if isPrime[i] {
            count++
        }
    }
    return count
}
```

## 阶乘

### 阶乘后的零

两个数相乘结果末尾有 0，一定是因为两个数中有因子 2 和 5，因为 10 = 2 x 5
也就是说，问题转化为：n! 最多可以分解出多少个因子 2 和 5
这个主要取决于能分解出几个因子 5，因为每个偶数都能分解出因子 2，因子 2 肯定比因子 5 多得多
现在，问题转化为：n! 最多可以分解出多少个因子 5

```go
func trailingZeroes(n int) int {
    res := 0
    for d := n; d/5 > 0; d = d / 5 {
        res += d / 5
    }
    return res
}
```

### 阶乘后K个零

现在就明确了问题：在区间 [0, LONG_MAX] 中寻找满足 trailingZeroes(n) == K 的左侧边界和右侧边界
对于这种具有单调性的函数，用 for 循环遍历，可以用二分查找进行降维打击

```go
func preimageSizeFZF(K int) int {
    // 左边界和右边界之差 + 1 就是答案
    return int(right_bound(K) - left_bound(K) + 1)
}

// 逻辑不变，数据类型全部改成 long
func trailingZeroes(n int64) int64 {
    var res int64 = 0
    for d := n; d/5 > 0; d = d / 5 {
        res += d / 5
    }
    return res
}

// 搜索 trailingZeroes(n) == K 的左侧边界
func left_bound(target int) int64 {
    lo, hi := int64(0), int64(math.MaxInt64)
    for lo < hi {
        mid := lo + (hi-lo)/2
        if trailingZeroes(mid) < int64(target) {
            lo = mid + 1
        } else if trailingZeroes(mid) > int64(target) {
            hi = mid
        } else {
            hi = mid
        }
    }
    return lo
}

// 搜索 trailingZeroes(n) == K 的右侧边界
func right_bound(target int) int64 {
    lo, hi := int64(0), int64(math.MaxInt64)
    for lo < hi {
        mid := lo + (hi-lo)/2
        if trailingZeroes(mid) < int64(target) {
            lo = mid + 1
        } else if trailingZeroes(mid) > int64(target) {
            hi = mid
        } else {
            lo = mid + 1
        }
    }
    return lo - 1
}
```

## 寻找缺失和重复元素

其实很容易解决这个问题，先遍历一次数组，用一个哈希表记录每个数字出现的次数，然后遍历一次 [1..N]，看看那个元素重复出现，那个元素没有出现，就 OK 了
但问题是，这个常规解法需要一个哈希表，也就是 O(N) 的空间复杂度。题目给的条件那么巧，在 [1..N] 的几个数字中恰好有一个重复，一个缺失，那肯定想让我们用更巧妙的方法来求解

改造下问题，暂且将 nums 中的元素变为 [0..N-1]，这样每个元素就和一个数组索引完全对应了，这样方便理解一些
如果说 nums 中不存在重复元素和缺失元素，那么每个元素就和唯一一个索引值对应
有一个元素重复了，同时导致一个元素缺失了，会导致有两个元素对应到了同一个索引，而且会有一个索引没有元素对应过去
如果找到这个重复对应的索引，不就是找到了那个重复元素么？找到那个没有元素对应的索引，不就是找到了那个缺失的元素

如何不使用额外空间判断某个索引有多少个元素对应呢？这就是这个问题的精妙之处了：通过将每个索引对应的元素变成负数，以表示这个索引被对应过一次了

```go
func findErrorNums(nums []int) []int {
    n := len(nums)
    dup := -1
    for i := 0; i < n; i++ {
        // 现在的元素是从 1 开始的
        index := abs(nums[i]) - 1
        if nums[index] < 0 {
            dup = abs(nums[i])
        } else {
            nums[index] *= -1
        }
    }
    missing := -1
    for i := 0; i < n; i++ {
        if nums[i] > 0 {
            // 将索引转换成元素
            missing = i + 1
        }
    }
    return []int{dup, missing}
}

// Helper function to calculate absolute value
func abs(x int) int {
    if x < 0 {
        return -x
    }
    return x
}
```

对于这种数组问题，关键点在于元素和索引是成对儿出现的，常用的方法是排序、异或、映射：

- 映射的思路就是我们刚才的分析，将每个索引和元素映射起来，通过正负号记录某个元素是否被映射
- 排序的方法也很好理解，对于这个问题，可以想象如果元素都被从小到大排序，如果发现索引对应的元素如果不相符，就可以找到重复和缺失的元素
- 异或运算也是常用的，因为异或性质 a ^ a = 0, a ^ 0 = a，如果将索引和元素同时异或，就可以消除成对儿的索引和元素，留下的就是重复或者缺失的元素
