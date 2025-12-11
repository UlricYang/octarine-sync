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

lcm(a,b) = (a/gcd(a,b)) * b