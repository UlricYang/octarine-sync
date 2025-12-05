# 动态规划

首先，动态规划问题的一般形式就是求最值。动态规划其实是运筹学的一种最优化方法，只不过在计算机问题上应用比较多，比如说让你求最长递增子序列呀，最小编辑距离呀等等。求解动态规划的核心问题是穷举。因为要求最值，肯定要把所有可行的答案穷举出来，然后在其中找最值呗

- 虽然动态规划的核心思想就是穷举求最值，但是问题可以千变万化，穷举所有可行解其实并不是一件容易的事，需要你熟练掌握递归思维，只有列出正确的「状态转移方程」，才能正确地穷举
- 需要判断算法问题是否具备「最优子结构」，是否能够通过子问题的最值得到原问题的最值
- 动态规划问题存在「重叠子问题」，如果暴力穷举的话效率会很低，所以需要你使用「备忘录」或者「DP table」来优化穷举过程，避免不必要的计算

以上提到的重叠子问题、最优子结构、状态转移方程就是动态规划三要素。在实际的算法问题中，写出状态转移方程是最困难的，总结的一个思维框架，辅助思考状态转移方程：

> 明确「状态」-> 明确「选择」 -> 定义 dp 数组/函数的含义

```python
# 自顶向下递归的动态规划
def dp(状态1, 状态2, ...):
    for 选择 in 所有可能的选择:
        # 此时的状态已经因为做了选择而改变
        result = 求最值(result, dp(状态1, 状态2, ...))
    return result

# 自底向上迭代的动态规划
# 初始化 base case
dp[0][0][...] = base case
# 进行状态转移
for 状态1 in 状态1的所有取值：
    for 状态2 in 状态2的所有取值：
        for ...
            dp[状态1][状态2][...] = 求最值(选择1，选择2...)
```

## 斐波那契数列

### 暴力递归

```go
// f(n) 计算第 n 个斐波那契数
func fib(n int) int {
    // base case
    if n == 0 || n == 1 {
        return n
    }
    return fib(n-1) + fib(n-2)
}
```

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251204212615019.jpg)

递归算法的时间复杂度怎么计算？就是用子问题个数乘以解决一个子问题需要的时间

- 首先计算子问题个数，即递归树中节点的总数。这棵递归树的高度为 n，所以二叉树的节点总数为 2^n
- 然后计算解决一个子问题的时间，在本算法中，没有循环，只有 f(n - 1) + f(n - 2) 一个加法操作，时间为 O(1)
- 所以算法的时间复杂度为二者相乘，即 O(2^n)，指数级别，爆炸

观察递归树，很明显发现了算法低效的原因：存在大量重复计算

#### 备忘录

即然耗时的原因是重复计算，那么我们可以造一个「备忘录」，每次算出某个子问题的答案后顺便记到「备忘录」里；每次遇到一个子问题别急着计算，先去「备忘录」里查一查，如果发现之前已经解决过这个问题了，直接把答案拿出来用，不要再耗时去计算了

```go
func fib(n int) int {
    // 备忘录全初始化为 -1
    // 因为斐波那契数肯定是非负整数，所以初始化为特殊值 -1 表示未计算

    // 因为数组的索引从 0 开始，所以需要 n + 1 个空间
    // 这样才能把 `f(0) ~ f(n)` 都记录到 memo 中
    memo := make([]int, n+1)
    for i := range memo {
        memo[i] = -1
    }

    return dp(memo, n)
}

// 带着备忘录进行递归
func dp(memo []int, n int) int {
    // base case
    if n == 0 || n == 1 {
        return n
    }
    // 已经计算过，不用再计算了
    if memo[n] != -1 {
        return memo[n]
    }
    // 在返回结果之前，存入备忘录
    memo[n] = dp(memo, n-1) + dp(memo, n-2)
    return memo[n]
}
```

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251204213107628.jpg)

实际上，带「备忘录」的递归算法，把一棵存在巨量冗余的递归树通过「剪枝」，改造成了一幅不存在冗余的递归图，极大减少了子问题（即递归图中节点）的个数，每个子问题都只会被计算一次

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251204213151494.jpg)

递归算法的时间复杂度怎么计算？就是用子问题个数乘以解决一个子问题需要的时间

- 子问题个数，即图中节点的总数，由于本算法不存在冗余计算，子问题就是 f(0), f(1), f(2) ... f(20)，数量和输入规模 n = 20 成正比，所以子问题个数为 O(n)
- 解决一个子问题的时间，同上，没有什么循环，时间为 O(1)
- 所以，本算法的时间复杂度是 O(n)，比起指数级复杂度的暴力算法，已经非常高效了

#### 自顶向下 V.S. 自底向上

动态规划解法确实有两种表现形式：

1. 带备忘录的递归解法，或称为「自顶向下」的解法，一个递归函数带一个 memo 备忘录
1. DP table 的迭代解法，或称为「自底向上」的解法，用 for 循环去迭代 dp 数组进行求解

这两者的本质是一样的，可以互相转化。迭代解法中的那个 dp 数组，就是递归解法中的 memo 数组

- 啥叫「自顶向下」？递归树从上向下生长，从一个规模较大的原问题 f(5)，向下逐渐分解规模，直到 f(0) 和 f(1) 这两个 base case，然后逐层返回答案
- 啥叫「自底向上」？直接从最底下、最简单、问题规模最小、已知结果的 f(0) 和 f(1)（base case）开始往上推出 f(2), f(3)... 最后推出我们想要的 f(5)

其实「自底向上」和「自顶向下」本质是一样的，只是视角不同而已

dp数组的迭代（递推）解法

```go
func fib(n int) int {
    if n == 0 || n == 1 {
        return n
    }
    // dp table
    dp := make([]int, n+1)
    // base case
    dp[0], dp[1] = 0, 1
    // 状态转移
    for i := 2; i <= n; i++ {
        dp[i] = dp[i-1] + dp[i-2]
    }

    return dp[n]
}
```

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251204213834755.jpg)

### 状态转移方程

f(n) 的函数参数会不断变化，所以你把参数 n 想做一个状态，这个状态 n 是由状态 n - 1 和状态 n - 2 转移（相加）而来，这就叫状态转移，仅此而已

上面的几种解法中的所有操作，例如 return f(n - 1) + f(n - 2)，dp[i] = dp[i - 1] + dp[i - 2]，以及对备忘录或 DP table 的初始化操作，都是围绕这个方程式的不同表现形式。可见列出「状态转移方程」的重要性，它是解决问题的核心，而且很容易发现，其实状态转移方程直接代表着暴力解法

千万不要看不起暴力解，动态规划问题最困难的就是写出这个暴力解，即状态转移方程。只要写出暴力解，优化方法无非是用备忘录或者 DP table，再无奥妙可言

根据斐波那契数列的状态转移方程，当前状态 n 只和之前的 n-1, n-2 两个状态有关，其实并不需要那么长的一个 DP table 来存储所有的状态，只要想办法存储之前的两个状态就行了。这一般是动态规划问题的最后一步优化，如果我们发现每次状态转移只需要 DP table 中的一部分，那么可以尝试缩小 DP table 的大小，只记录必要的数据，从而降低空间复杂度

```go
func fib(n int) int {
    if n == 0 || n == 1 {
        // base case
        return n
    }
    // 分别代表 dp[i - 1] 和 dp[i - 2]
    dp_i_1, dp_i_2 := 1, 0
    for i := 2; i <= n; i++ {
        // dp[i] = dp[i - 1] + dp[i - 2];
        dp_i := dp_i_1 + dp_i_2
        // 滚动更新
        dp_i_2 = dp_i_1
        dp_i_1 = dp_i
    }
    return dp_i_1
}
```

## 凑零钱

### 暴力递归

这个问题是动态规划问题，因为它具有「最优子结构」的。要符合「最优子结构」，子问题间必须互相独立

假设你有面值为 1, 2, 5 的硬币，你想求 amount = 11 时的最少硬币数（原问题），如果你知道凑出 amount = 10, 9, 6 的最少硬币数（子问题），你只需要把子问题的答案加一（再选一枚面值为 1, 2, 5 的硬币），求个最小值，就是原问题的答案。因为硬币的数量是没有限制的，所以子问题之间没有相互制，是互相独立的

如何列出正确的状态转移方程：

1. 确定「状态」，也就是原问题和子问题中会变化的变量。由于硬币数量无限，硬币的面额也是题目给定的，只有目标金额会不断地向 base case 靠近，所以唯一的「状态」就是目标金额 amount
1. 确定「选择」，也就是导致「状态」产生变化的行为。目标金额为什么变化呢，因为你在选择硬币，你每选择一枚硬币，就相当于减少了目标金额。所以说所有硬币的面值，就是你的「选择」
1. 明确 dp 函数/数组的定义。我们这里讲的是自顶向下的解法，所以会有一个递归的 dp 函数，一般来说函数的参数就是状态转移中会变化的量，也就是上面说到的「状态」；函数的返回值就是题目要求我们计算的量。就本题来说，状态只有一个，即「目标金额」，题目要求我们计算凑出目标金额所需的最少硬币数量。所以我们可以这样定义 dp 函数：dp(n) 表示，输入一个目标金额 n，返回凑出目标金额 n 所需的最少硬币数量

> dp(n) = min([dp(n-coin)+1 for coin in coins])

```go
func coinChange(coins []int, amount int) int {
    // 题目要求的最终结果是 dp(amount)
    return dp(coins, amount)
}

// 定义：要凑出目标金额 amount，至少要 dp(coins, amount) 个硬币
func dp(coins []int, amount int) int {
    // base case
    if amount == 0 {
        return 0
    }
    if amount < 0 {
        return -1
    }

    res := math.MaxInt32
    for _, coin := range coins {
        // 计算子问题的结果
        subProblem := dp(coins, amount - coin)
        // 子问题无解则跳过
        if subProblem == -1 {
            continue
        }
        // 在子问题中选择最优解，然后加一
        res = min(res, subProblem + 1)
    }

    if res == math.MaxInt32 {
        return -1
    }
    return res
}

func min(x, y int) int {
    if x < y {
        return x
    }
    return y
}
```

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251204214927341.jpg)

#### 备忘录

```go
import "math"

func coinChange(coins []int, amount int) int {
    memo := make([]int, amount + 1)
    for i := range memo {
        memo[i] = -666
    }
    // 备忘录初始化为一个不会被取到的特殊值，代表还未被计算
    return dp(coins, amount, memo)
}

func dp(coins []int, amount int, memo []int) int {
    if amount == 0 {
        return 0
    }
    if amount < 0 {
        return -1
    }
    // 查备忘录，防止重复计算
    if memo[amount] != -666 {
        return memo[amount]
    }

    res := math.MaxInt32
    for _, coin := range coins {
        // 计算子问题的结果
        subProblem := dp(coins, amount - coin, memo)
        // 子问题无解则跳过
        if subProblem == -1 {
            continue
        }
        // 在子问题中选择最优解，然后加一
        res = min(res, subProblem + 1)
    }
    // 把计算结果存入备忘录
    if res == math.MaxInt32 {
        memo[amount] = -1
    } else {
        memo[amount] = res
    }
    return memo[amount]
}
```

#### dp数组的迭代解法

```go
func coinChange(coins []int, amount int) int {
    // 数组大小为 amount + 1，初始值也为 amount + 1
    dp := make([]int, amount+1)
    for i := range dp {
        dp[i] = amount + 1
    }

    // base case
    dp[0] = 0
    // 外层 for 循环在遍历所有状态的所有取值
    for i := 0; i < len(dp); i++ {
        // 内层 for 循环在求所有选择的最小值
        for _, coin := range coins {
            // 子问题无解，跳过
            if i - coin < 0 {
                continue
            }
            dp[i] = min(dp[i], 1 + dp[i - coin])
        }
    }
    if dp[amount] == amount+1 {
        return -1
    }
    return dp[amount]
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251204215445961.jpg)

## 最长递增子序列

动态规划的核心设计思想是数学归纳法

我们想证明一个数学结论，那么我们先假设这个结论在 k < n 时成立，然后根据这个假设，想办法推导证明出 k = n 的时候此结论也成立。如果能够证明出来，那么就说明这个结论对于 k 等于任何数都成立

设计动态规划算法，不是需要一个 dp 数组吗？我们可以假设 dp[0...i-1] 都已经被算出来了，然后问自己：怎么通过这些结果算出 dp[i]

dp[i] 表示以 nums[i] 这个数结尾的最长递增子序列的长度
根据这个定义，我们的最终结果（子序列的最大长度）应该是 dp 数组中的最大值

### 动态规划解法

现在想求 dp[5] 的值，也就是想求以 nums[5] 为结尾的最长递增子序列

nums[5] = 3，既然是递增子序列，我们只要找到前面那些结尾比 3 小的子序列，然后把 3 接到这些子序列末尾，就可以形成一个新的递增子序列，而且这个新的子序列长度加一

```go
func lengthOfLIS(nums []int) int {
    // 定义：dp[i] 表示以 nums[i] 这个数结尾的最长递增子序列的长度
    dp := make([]int, len(nums))
    // base case：dp 数组全都初始化为 1
    for i := range dp {
        dp[i] = 1
    }
    for i := 0; i < len(nums); i++ {
        for j := 0; j < i; j++ {
            if nums[i] > nums[j] {
                // dp[i] = Math.max(dp[i], dp[j] + 1);
                dp[i] = max(dp[i], dp[j] + 1);
            }
        }
    }
    res := 0
    for i := 0; i < len(dp); i++ {
        res = max(res, dp[i])
    }
    return res
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

### 二分查找解法

patience sorting（耐心排序）

首先给你一排扑克牌，我们像遍历数组那样从左到右一张一张处理这些扑克牌，最终要把这些牌分成若干堆。处理这些扑克牌要遵循以下规则：

- 只能把点数小的牌压到点数比它大的牌上
- 如果当前牌点数较大没有可以放置的堆，则新建一个堆，把这张牌放进去
- 如果当前牌有多个堆可供选择，则选择最左边的那一堆放置

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251204220550812.jpeg)

为什么遇到多个可选择堆的时候要放到最左边的堆上呢？因为这样可以保证牌堆顶的牌有序

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251204220842832.jpeg)

按照上述规则执行，可以算出最长递增子序列，牌的堆数就是最长递增子序列的长度

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251204220920768.jpeg)

```go
func lengthOfLIS(nums []int) int {
    top := make([]int, len(nums))
    // 牌堆数初始化为 0
    var piles int
    for i := 0; i < len(nums); i++ {
        // 要处理的扑克牌
        poker := nums[i]

        // * 搜索左侧边界的二分查找 *
        var left, right int = 0, piles
        for left < right {
            mid := (left + right) / 2
            if top[mid] > poker {
                right = mid
            } else if top[mid] < poker {
                left = mid + 1
            } else {
                right = mid
            }
        }
        //

        // 没找到合适的牌堆，新建一堆
        if left == piles {
            piles++
        }
        // 把这张牌放到牌堆顶
        top[left] = poker
    }
    // 牌堆数就是 LIS 长度
    return piles
}
```

### 俄罗斯信封套娃

这道题目其实是最长递增子序列的一个变种，因为每次合法的嵌套是大的套小的，相当于在二维平面中找一个最长递增的子序列，其长度就是最多能嵌套的信封个数

前面说的标准 LIS 算法只能在一维数组中寻找最长子序列，而我们的信封是由 (w, h) 这样的二维数对形式表示的

这道题的解法比较巧妙：

- 先对宽度 w 进行升序排序，如果遇到 w 相同的情况，则按照高度 h 降序排序（对宽度 w 从小到大排序，确保了 w 这个维度可以互相嵌套，所以我们只需要专注高度 h 这个维度能够互相嵌套即可）
- 之后把所有的 h 作为一个数组，在这个数组上计算 LIS 的长度就是答案（两个 w 相同的信封不能相互包含，所以对于宽度 w 相同的信封，对高度 h 进行降序排序，保证二维 LIS 中不存在多个 w 相同的信封）

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251204221321190.jpg)

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251204221351385.jpg)

```go
func maxEnvelopes(envelopes [][]int) int {
    n := len(envelopes)
    // 按宽度升序排列，如果宽度一样，则按高度降序排列
    sort.Slice(envelopes, func(i, j int) bool {
        if envelopes[i][0] == envelopes[j][0] {
            return envelopes[i][1] > envelopes[j][1]
        }
        return envelopes[i][0] < envelopes[j][0]
    })
    // 对高度数组寻找 LIS
    height := make([]int, n)
    for i := 0; i < n; i++ {
        height[i] = envelopes[i][1]
    }

    return lengthOfLIS(height)
}

// 返回 nums 中 LIS 的长度
func lengthOfLIS(nums []int) int {
    piles := 0
    n := len(nums)
    top := make([]int, n)
    for _, poker := range nums {
        // 要处理的扑克牌
        left, right := 0, piles
        // 二分查找插入位置
        for left < right {
            mid := (left + right) / 2
            if top[mid] >= poker {
                right = mid
            } else {
                left = mid + 1
            }
        }
        if left == piles {
            piles++
        }
        // 把这张牌放到牌堆顶
        top[left] = poker
    }
    // 牌堆数就是 LIS 长度
    return piles
}
```

## 下降路径最小和

```go
func minFallingPathSum(matrix [][]int) int {
    n := len(matrix)
    res := int(^uint(0) >> 1) // Initialize res to the maximum integer value
    // 备忘录里的值初始化为 66666
    memo := make([][]int, n)
    for i := range memo {
        memo[i] = make([]int, n)
        for j := range memo[i] {
            memo[i][j] = 66666
        }
    }
    // 终点可能在 matrix[n-1] 的任意一列
    for j := 0; j < n; j++ {
        res = min(res, dp(matrix, n-1, j, memo))
    }
    return res
}

// 备忘录
func dp(matrix [][]int, i, j int, memo [][]int) int {
    // 1、索引合法性检查
    if i < 0 || j < 0 || i >= len(matrix) || j >= len(matrix[0]) {
        return 99999
    }
    // 2、base case
    if i == 0 {
        return matrix[0][j]
    }
    // 3、查找备忘录，防止重复计算
    if memo[i][j] != 66666 {
        return memo[i][j]
    }
    // 进行状态转移
    memo[i][j] = matrix[i][j] + min3(
        dp(matrix, i-1, j, memo),
        dp(matrix, i-1, j-1, memo),
        dp(matrix, i-1, j+1, memo),
    )

    return memo[i][j]
}


func min3(a, b, c int) int {
    if a < b {
        if a < c {
            return a
        }
        return c
    }
    if b < c {
        return b
    }
    return c
}
```

### base case的条件如何确定

base case 为什么是 i == 0，返回值为什么是 matrix[0][j]，这是根据 dp 函数的定义所决定的

回顾我们的 dp 函数定义：
从第一行（matrix[0][..]）向下落，落到位置 matrix[i][j] 的最小路径和为 dp(matrix, i, j)

根据这个定义，我们就是从 matrix[0][j] 开始下落。那如果我们想落到的目的地就是 i == 0，所需的路径和当然就是 matrix[0][j]

### 备忘录的初始值如何确定

备忘录 memo 数组的作用是什么？

就是防止重复计算，将 dp(matrix, i, j) 的计算结果存进 memo[i][j]，遇到重复计算可以直接返回

那么，我们必须要知道 memo[i][j] 到底存储计算结果没有，对吧？如果存结果了，就直接返回；没存，就去递归计算。所以，memo 的初始值一定得是特殊值，和合法的答案有所区分

### 边界情况的返回值如何确定

对于不合法的索引，返回值应该如何确定，这需要根据我们状态转移方程的逻辑确定

对于这道题，状态转移的基本逻辑如下：

```java
int dp(int[][] matrix, int i, int j) {
    return matrix[i][j] + min(
            dp(matrix, i - 1, j),
            dp(matrix, i - 1, j - 1),
            dp(matrix, i - 1, j + 1)
        );
}
```

显然，i - 1, j - 1, j + 1 这几个运算可能会造成索引越界，对于索引越界的 dp 函数，应该返回一个不可能被取到的值。因为我们调用的是 min 函数，最终返回的值是最小值，所以对于不合法的索引，只要 dp 函数返回一个永远不会被取到的最大值即可

### 题目给定的其他信息

除了题意本身，一定不要忽视题目给定的其他信息。本文举的例子，测试用例数据范围可以确定「什么是特殊值」，从而帮助我们将思路转化成代码

除此之外，数据范围还可以帮我们估算算法的时间/空间复杂度：

- 有的算法题，你只想到一个暴力求解思路，时间复杂度比较高。如果发现题目给定的数据量比较大，那么肯定可以说明这个求解思路有问题或者存在优化的空间
- 除了数据范围，有时候题目还会限制我们算法的时间复杂度，这种信息其实也暗示着一些东西

比如要求我们的算法复杂度是 O(NlogN)，你想想怎么才能搞出一个对数级别的复杂度呢？肯定得用到
二分搜索 或者二叉树相关的数据结构，比如 TreeMap，PriorityQueue 之类的对吧

比如有时候题目要求你的算法时间复杂度是 O(MN)，这可以联想到什么？可以大胆猜测，常规解法是用
回溯算法 暴力穷举，但是更好的解法是动态规划，而且是一个二维动态规划，需要一个 M \* N 的二维 dp 数组，所以产生了这样一个时间复杂度

## 子结构

### 最优子结构

最优子结构：可以从子问题的最优结果推出更大规模问题的最优结果
想满足最优子结，子问题之间必须互相独立
遇到这种最优子结构失效情况，怎么办？策略是：改造问题

最优子结构并不是动态规划独有的一种性质，能求最值的问题大部分都具有这个性质；但反过来，最优子结构性质作为动态规划问题的必要条件，一定是让你求最值的，以后碰到那种恶心人的最值题，思路往动态规划想就对了，这就是套路

动态规划不就是从最简单的 base case 往后推导吗，可以想象成一个链式反应，以小博大。但只有符合最优子结构的问题，才有发生这种链式反应的性质。找最优子结构的过程，其实就是证明状态转移方程正确性的过程，方程符合最优子结构就可以写暴力解了，写出暴力解就可以看出有没有重叠子问题了，有则优化，无则 OK。这也是套路

### 重叠子问题

可以运用备忘录等方法去优化算法效率

#### 如何判断是否存在重叠子问题

首先，最简单粗暴的方式就是画图，把递归树画出来，看看有没有重复的节点

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251205101926878.jpg)

其实根本没必要画图，可以通过递归框架直接判断是否存在重叠子问题
具体操作就是直接删掉代码细节，抽象出该解法的递归框架：

```java
int dp(int[][] grid, int i, int j) {
    dp(grid, i - 1, j), // #1
    dp(grid, i, j - 1)  // #2
}
```

可以看到 i, j 的值在不断减小，那么如果想从状态 (i, j) 转移到 (i-1, j-1)，有几种路径？显然有两种路径，可以是 (i, j) -> #1 -> #2 或者 (i, j) -> #2 -> #1，不止一种，说明 (i-1, j-1) 会被多次计算，所以一定存在重叠子问题

## dp数组

我们把 dp 数组初始化为 int[m+1][n+1]，让索引整体偏移一位，把索引 0 留出来作为 base case 表示空串，然后定义 dp[i+1][j+1] 存储 s1[0..i] 和 s2[0..j] 的编辑距离

至于遍历方向，只要把住两点就行了：

1. 遍历的过程中，所需的状态必须是已经计算出来的
1. 遍历结束后，存储结果的那个位置必须已经被计算出来

主要就是看 base case 和最终结果的存储位置，保证遍历过程中使用的数据都是计算完毕的就行

## 子序列问题解题模板

一旦涉及到子序列和最值，那几乎可以肯定，考察的是动态规划技巧，时间复杂度一般都是 O(n^2)

### 两种思路

一个一维的 dp 数组：

```java
int n = array.length;
int[] dp = new int[n];

for (int i = 1; i < n; i++) {
    for (int j = 0; j < i; j++) {
        dp[i] = 最值(dp[i], dp[j] + ...)
    }
}
```

- dp 数组的定义是：在子数组 arr[0..i] 中，以 arr[i] 结尾的子序列的长度是 dp[i]

一个二维的 dp 数组：

```java
int n = arr.length;
int[][] dp = new dp[n][n];

for (int i = 0; i < n; i++) {
    for (int j = 0; j < n; j++) {
        if (arr[i] == arr[j])
            dp[i][j] = dp[i][j] + ...
        else
            // 计算最值
            dp[i][j] = min(...)
    }
}
```

- 涉及两个字符串/数组的场景，dp 数组的定义：在子数组 arr1[0..i] 和子数组 arr2[0..j] 中，我们要求的子序列长度为 dp[i][j]

- 只涉及一个字符串/数组的场景，dp 数组的定义：在子数组 array[i..j] 中，我们要求的子序列的长度为 dp[i][j]

## 经典题目

### 0-1背包问题

dp[i][w] 的定义如下：对于前 i 个物品，当前背包的容量为 w，这种情况下可以装的最大价值是 dp[i][w]

```go
func knapsack(W int, wt, val []int) int {
    N := len(wt)
    // base case 已初始化
    dp := make([][]int, N+1)
    for i := range dp {
        dp[i] = make([]int, W+1)
    }
    for i := 1; i <= N; i++ {
        for w := 1; w <= W; w++ {
            if w-wt[i-1] < 0 {
                // 这种情况下只能选择不装入背包
                dp[i][w] = dp[i-1][w]
            } else {
                // 装入或者不装入背包，择优
                dp[i][w] = max(
                    dp[i-1][w-wt[i-1]]+val[i-1],
                    dp[i-1][w],
                )
            }
        }
    }

    return dp[N][W]
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

### 子集背包问题

dp[i][j] = x 表示，对于前 i 个物品（i 从 1 开始计数），当前背包的容量为 j 时，若 x 为 true，则说明可以恰好将背包装满，若 x 为 false，则说明不能恰好将背包装满

```go
func canPartition(nums []int) bool {
    sum := 0
    for _, num := range nums {
        sum += num
    }
    // 和为奇数时，不可能划分成两个和相等的集合
    if sum%2 != 0 {
        return false
    }

    n := len(nums)
    sum /= 2

    dp := make([]bool, sum+1)
    // base case
    dp[0] = true

    for i := 0; i < n; i++ {
        for j := sum; j >= 0; j-- {
            if j - nums[i] >= 0 {
                dp[j] = dp[j] || dp[j-nums[i]]
            }
        }
    }
    return dp[sum]
}
```

### 完全背包问题

dp[i][j] 的定义：若只使用前 i 个物品（可以重复使用），当背包容量为 j 时，有 dp[i][j] 种方法可以装满背包

翻译回题目的意思：若只使用 coins 中的前 i 个（i 从 1 开始计数）硬币的面值，若想凑出金额 j，有 dp[i][j] 种凑法

```go
func change(amount int, coins []int) int {
    n := len(coins)
    dp := make([][]int, n+1)
    for i := range dp {
        dp[i] = make([]int, amount+1)
    }
    // base case
    for i := 0; i <= n; i++ {
        dp[i][0] = 1
    }

    for i := 1; i <= n; i++ {
        for j := 1; j <= amount; j++ {
            if j-coins[i-1] >= 0 {
                dp[i][j] = dp[i-1][j] + dp[i][j-coins[i-1]]
            } else {
                dp[i][j] = dp[i-1][j]
            }
        }
    }
    return dp[n][amount]
}
```

### 编辑距离

解决两个字符串的动态规划问题，一般都是用两个指针 i, j 分别指向两个字符串的头部或尾部，然后尝试写状态转移方程

比方说让 i, j 分别指向两个字符串的尾部，把 dp[i], dp[j] 定义为 s1[0..i], s2[0..j] 子串的编辑距离，那么 i, j 一步步往前移动的过程，就是问题规模（子串长度）逐步减小的过程

当然，你想让让 i, j 分别指向字符串头部，然后一步步往后移动也可以，本质上并无区别，只要改一下 dp 函数/数组的定义即可

#### 备忘录解法

```go
func minDistance(s1 string, s2 string) int {
    // 备忘录
    memo := make([][]int, len(s1))
    for i := range memo {
        memo[i] = make([]int, len(s2))
        for j := range memo[i] {
            memo[i][j] = -1
        }
    }
    return dp(s1, len(s1)-1, s2, len(s2)-1, memo)
}

func dp(s1 string, i int, s2 string, j int, memo [][]int) int {
    if i == -1 {
        return j + 1
    }
    if j == -1 {
        return i + 1
    }
    // 查备忘录，避免重叠子问题
    if memo[i][j] != -1 {
        return memo[i][j]
    }
    // 状态转移，结果存入备忘录
    if s1[i] == s2[j] {
        memo[i][j] = dp(s1, i-1, s2, j-1, memo)
    } else {
        memo[i][j] = min(
            dp(s1, i, s2, j-1, memo)+1,
            dp(s1, i-1, s2, j, memo)+1,
            dp(s1, i-1, s2, j-1, memo)+1,
        )
    }
    return memo[i][j]
}

func min(a int, b int, c int) int {
    if a < b {
        if a < c {
            return a
        }
        return c
    }
    if b < c {
        return b
    }
    return c
}
```

#### DP table 解法

```go
func minDistance(s1 string, s2 string) int {
    m, n := len(s1), len(s2)
    dp := make([][]int, m+1)
    for i := range dp {
        dp[i] = make([]int, n+1)
    }
    // base case
    for i := 1; i <= m; i++ {
        dp[i][0] = i
    }
    for j := 1; j <= n; j++ {
        dp[0][j] = j
    }
    // 自底向上求解
    for i := 1; i <= m; i++ {
        for j := 1; j <= n; j++ {
            if s1[i-1] == s2[j-1] {
                dp[i][j] = dp[i-1][j-1]
            } else {
                dp[i][j] = min(
                    dp[i-1][j]+1,
                    dp[i][j-1]+1,
                    dp[i-1][j-1]+1,
                )
            }
        }
    }
    // 储存着整个 s1 和 s2 的最小编辑距离
    return dp[m][n]
}

func min(a, b, c int) int {
    if a < b {
        if a < c {
            return a
        }
        return c
    }
    if b < c {
        return b
    }
    return c
}
```

### 最大子数组

dp 数组的含义：以 nums[i] 为结尾的「最大子数组和」为 dp[i]

```go
func maxSubArray(nums []int) int {
    n := len(nums)
    if n == 0 {
        return 0
    }
    // base case
    dp_0 := nums[0]
    var dp_1, res int = 0, dp_0

    for i := 1; i < n; i++ {
        // dp[i] = max(nums[i], nums[i] + dp[i-1])
        dp_1 = max(nums[i], nums[i]+dp_0)
        dp_0 = dp_1
        // 顺便计算最大的结果
        res = max(res, dp_1)
    }

    return res
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

### 最长公共子序列

对于两个字符串求子序列的问题，都是用两个指针 i 和 j 分别在两个字符串上移动，大概率是动态规划思路

```go
func longestCommonSubsequence(s1 string, s2 string) int {
	m := len(s1)
	n := len(s2)
	// 备忘录值为 -1 代表未曾计算
	memo := make([][]int, m)
	for i := range memo {
		memo[i] = make([]int, n)
		for j := range memo[i] {
			memo[i][j] = -1
		}
	}

	// 定义：计算 s1[i..] 和 s2[j..] 的最长公共子序列长度
	// 使用闭包，以便使用 memo 来记录中间结果
	var dp func(int, int) int
	dp = func(i int, j int) int {
		// base case
		if i == len(s1) || j == len(s2) {
			return 0
		}
		// 如果之前计算过，则直接返回备忘录中的答案
		if memo[i][j] != -1 {
			return memo[i][j]
		}
		// 根据 s1[i] 和 s2[j] 的情况做选择
		if s1[i] == s2[j] {
			// s1[i] 和 s2[j] 必然在 lcs 中
			memo[i][j] = 1 + dp(i+1, j+1)
		} else {
			// s1[i] 和 s2[j] 至少有一个不在 lcs 中
			memo[i][j] = max(dp(i+1, j), dp(i, j+1))
		}
		return memo[i][j]
	}

	// 计算 s1[0..] 和 s2[0..] 的 lcs 长度
	return dp(0, 0)
}

func max(a int, b int) int {
	if a > b {
		return a
	}
	return b
}
```
