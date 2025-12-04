# 字典树

Trie 树就是多叉树结构 的延伸，是一种针对字符串进行特殊优化的数据结构

Trie 树在处理字符串相关操作时有诸多优势：

- 节省公共字符串前缀的内存空间
- 方便处理前缀操作
- 支持通配符匹配

## 主要应用场景

Trie 树是一种针对字符串有特殊优化的数据结构，这也许它又被叫做字典树的原因

### 节约存储空间

比如说这样存储几个键值对，键值对会被存到 table 数组中，也就是说它真的创建 "apple"、"app"、"appl" 这三个字符串，占用了 12 个字符的内存空间

但是注意，这三个字符串拥有共同的前缀，"app" 这个前缀被重复存储了三次，"l" 也被重复存储了两次

如果换成 TrieMap 来存储，Trie 树底层并不会重复存储公共前缀，所以只需要 "apple" 这 5 个字符的内存空间来存储键

```java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 1);
map.put("app", 2);
map.put("appl", 3);

// Trie 树的键类型固定为 String 类型，值类型可以是泛型
TrieMap<Integer> map = new TrieMap<>();
map.put("apple", 1);
map.put("app", 2);
map.put("appl", 3);
```

### 方便处理前缀操作

```java
// Trie 树的键类型固定为 String 类型，值类型可以是泛型
TrieMap<Integer> map = new TrieMap<>();
map.put("that", 1);
map.put("the", 2);
map.put("them", 3);
map.put("apple", 4);

// "the" 是 "themxyz" 的最短前缀
System.out.println(map.shortestPrefixOf("themxyz")); // "the"

// "them" 是 "themxyz" 的最长前缀
System.out.println(map.longestPrefixOf("themxyz")); // "them"

// "tha" 是 "that" 的前缀
System.out.println(map.hasKeyWithPrefix("tha")); // true

// 没有以 "thz" 为前缀的键
System.out.println(map.hasKeyWithPrefix("thz")); // false

// "that", "the", "them" 都是 "th" 的前缀
System.out.println(map.keysWithPrefix("th")); // ["that", "the", "them"]
```

### 可以使用通配符

```java
// Trie 树的键类型固定为 String 类型，值类型可以是泛型
// 支持通配符匹配，"." 可以匹配任意一个字符
TrieMap<Integer> map = new TrieMap<>();

map.put("that", 1);
map.put("the", 2);
map.put("team", 3);
map.put("zip", 4);

// 匹配 "t.a." 的键有 "team", "that"
System.out.println(map.keysWithPattern("t.a.")); // ["team", "that"]

// 匹配 ".ip" 的键有 "zip"
System.out.println(map.hasKeyWithPattern(".ip")); // true

// 没有匹配 "z.o" 的键
System.out.println(map.hasKeyWithPattern("z.o")); // false

```

### 可以按照字典序遍历键

```java
// Trie 树的键类型固定为 String 类型，值类型可以是泛型
TrieMap<Integer> map = new TrieMap<>();

map.put("that", 1);
map.put("the", 2);
map.put("them", 3);
map.put("zip", 4);
map.put("apple", 5);

// 按照字典序遍历键
System.out.println(map.keys()); // ["apple", "that", "the", "them", "zip"]
```

## 基本结构

```go
// Trie 树节点实现
type TrieNode struct {
    val      interface{}
    children [256]*TrieNode
}
```

这个 val 字段存储键对应的值，children 数组存储指向子节点的指针

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251204171316066.jpeg)

但是和普通多叉树节点不同，TrieNode 中 children 数组的索引是有意义的，代表键中的一个字符，比如说 children[97] 如果非空，说明这里存储了一个字符 'a'，因为 'a' 的 ASCII 码为 97

模板只考虑处理 ASCII 字符，所以 children 数组的大小设置为 256。不过这个可以根据具体问题修改。一个节点有 256 个子节点指针，但大多数时候都是空的，可以省略掉不画，所以一般你看到的 Trie 树长这样

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251204171514974.jpeg)

白色节点代表 val 字段为空，橙色节点代表 val 字段非空。TrieNode 节点本身只存储 val 字段，并没有一个字段来存储字符，字符是通过子节点在父节点的 children 数组中的索引确定的。形象理解就是，Trie 树用「树枝」存储字符串（键），用「节点」存储字符串（键）对应的数据（值）。所以图中把字符标在树枝，键对应的值 val 标在节点上

![](https://raw.githubusercontent.com/UlricYang/FigureBed/main/img/20251204171802729.jpeg)

Trie 树也叫前缀树了，因为其中的字符串共享前缀，相同前缀的字符串集中在 Trie 树中的一个子树上，给字符串的处理带来很大的便利
