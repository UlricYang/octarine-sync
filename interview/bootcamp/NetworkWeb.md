# Cookie、Session、Token

## 基本概念

### Cookie：浏览器端的身份凭证

本质：服务器发送给浏览器的小段数据（纯文本）- 浏览器额头的小纸条

存储位置：客户端（浏览器）

工作流程：

1. 服务器响应时 Set-Cookie: data
1. 浏览器自动存储并携带
1. 每次请求自动附加 Cookie: data

| 属性       | 说明                        | 示例                                    |
| ---------- | --------------------------- | --------------------------------------- |
| Name=Value | 键值对数据                  | `sessionId=abc123`                      |
| Expires    | 过期时间（UTC）             | `Expires=Wed, 21 Oct 2025 07:28:00 GMT` |
| Max-Age    | 存活时间（秒，替代Expires） | `Max-Age=3600`（1小时）                 |
| Domain     | 有效域名                    | `Domain=.example.com`                   |
| Path       | 有效路径                    | `Path=/user`                            |
| Secure     | 仅HTTPS传输                 | `Secure`                                |
| HttpOnly   | 防止JS访问（防XSS）         | `HttpOnly`                              |
| SameSite   | 防CSRF策略                  | `SameSite=Strict/Lax/None`              |

安全缺陷：

- 存储在客户端，易被窃取（通过XSS）
- 自动附加所有请求，可能泄露隐私
- 容量限制（通常≤4KB）
- 跨域受限

### Session：服务器端的会话管理

本质：服务器端的临时数据存储 - 服务器端的小房间

存储位置：服务器内存/数据库

工作流程：

1. 客户端首次访问生成 Session ID
1. 服务器创建 Session: {sessionId: data}
1. 通过 Cookie 传递 Session ID
1. 后续请求通过 ID 获取会话数据

```mermaid
graph LR
    A[客户端] --1.请求--> B(服务器)
    B --2.生成Session ID--> C[内存/数据库]
    C --3.Session数据--> D{用户数据}
    D --4.设置Cookie--> E[客户端]
```

| 特性       | 说明                               |
| ---------- | ---------------------------------- |
| 安全性     | 数据存储在服务器端                 |
| 容量限制   | 几乎无限制（受服务器存储能力制约） |
| 生命周期   | 可配置超时（如30分钟无操作失效）   |
| 分布式扩展 | 需借助Redis/Memcached等共享存储    |
| 防CSRF     | 需额外Token防护（如同步Cookie）    |

### Token：无状态的身份认证

本质：加密的JSON数据包 - 自带防伪内容的证件

最常用形式：JWT (JSON Web Token) 和 Opaque（随机字符串）

存储位置：客户端（localStorage/内存）

工作流程：

1. 登录成功后签发 Token
1. 客户端存储 Token
1. 每次请求附加 Token 在 Header
1. 服务端验证 Token 有效性

JWT 结构（三部分，用`.`分隔）：Header.Payload.Signature

```
Header（头部）：
    {
      "alg": "HS256",    // 签名算法
      "typ": "JWT"       // 令牌类型
    }

Payload（载荷）：
    {
      "sub": "12345678", // 用户ID
      "name": "John Doe",
      "iat": 1516239022, // 签发时间
      "exp": 1516242622  // 过期时间
    }

Signature（签名）：
    HMACSHA256(
      base64UrlEncode(header) + "." +
      base64UrlEncode(payload),
      "your-256-bit-secret"
    )
```

| 特性     | 说明                           |
| -------- | ------------------------------ |
| 无状态   | 服务端无需存储会话             |
| 跨域友好 | 解决分布式系统认证问题         |
| 可扩展性 | 天然支持微服务架构             |
| 安全增强 | 签名机制防篡改、过期机制防滥用 |
| 性能优化 | 减少数据库查询                 |

常见使用场景：

```
Authorization: Bearer <token>
或
cookie: access_token=<token>
```

### 三者对比总结

| 特性     | Cookie           | Session                  | Token (JWT)           |
| -------- | ---------------- | ------------------------ | --------------------- |
| 存储位置 | 客户端           | 服务器端                 | 客户端                |
| 数据量   | 小（≤4KB）       | 大（服务器存储）         | 中等（由Payload决定） |
| 安全风险 | XSS窃取/CSRF攻击 | 会话劫持/固定攻击        | Token泄露/重放攻击    |
| 状态性   | 有状态           | 有状态                   | 无状态                |
| 跨域支持 | 弱               | 弱                       | 强                    |
| 扩展性   | 差               | 差（需共享Session）      | 优（天然分布式）      |
| 典型用途 | 记住登录状态     | 敏感会话数据（如购物车） | RESTful API认证       |

| 风险         | 场景           | Shield                                          |
| ------------ | -------------- | ----------------------------------------------- |
| 会话劫持     | XSS            | Cookie 加 `HttpOnly`, `Secure`, `SameSite=Lax`  |
| CSRF         | 302 跳转       | 针对 API 用 Token + SameSite=Strict             |
| JWT 冒用     | 被泄签名       | 短 TTL + 刷新 Token 存 Redis，可手动 revocation |
| Session 洪水 | DDoS           | 改 128 bit ID + 内存/Redis 高速淘汰             |
| 信息泄露     | Token 解码可见 | Payload 里只存 uid & exp，敏感信息放表          |
| 浏览器无痕   | 同一身份       | 统计指纹 + 短暂 Session                         |

| 名称    | 类型                  | 存储地                    | 生命周期                          | 是否可篡改                 | 身份识别实现方式                   |
| ------- | --------------------- | ------------------------- | --------------------------------- | -------------------------- | ---------------------------------- |
| Cookie  | 字符串片段            | 客户端浏览器              | 可设时效 / 会话级                 | 易篡改（除非加密签名）     | 携带 Session ID 或 Token           |
| Session | 结构化服务对象        | 服务端内存 / Redis 等缓存 | 临时 / 可设超时                   | 安全可控                   | 服务端查找关联数据                 |
| Token   | 安全载荷体（JSON 等） | 各方传输 / 内存           | 可设置过期时间（如 JWT 内嵌 exp） | 可自检防篡改（如 JWT签名） | 包含用户信息，无须 server 实时存查 |

- 我要粘浏览器 → 用 Cookie；
- 我要挂内存状态 → 用 Session；
- 我要跨服务或移动端无状态签权 → 用 Token（JWT 疼点大，Opaque更灵活）。

## 运作机制

### Cookie（客户端最小信息传输粒）

本质：一个浏览器支持的 HTTP头字段，由 `Set-Cookie` 指令下发

用途：

- 跟踪见识 Session ID（如 `JSESSIONID`, `PHPSESSID`, 或自定义名 `user_session`）

- 存储小幅状态（如设置偏好、轻量统计）

- 示例传输头字段：

```http
Set-Cookie: sessionid=abc123; Path=/; HttpOnly; Secure; SameSite=Strict
```

- 说明字段含义（Go 设置方式类似）：

```go
http.SetCookie(w, &http.Cookie{
    Name:     "sessionid",
    Value:    "abc123",
    Path:     "/",
    HttpOnly: true,   // 防 XSS
    Secure:   true,   // HTTPS Only
    SameSite: http.SameSiteStrictMode,
})
```

属于易被篡改，只传递字符串，不做逻辑处理，仅为辅助服务端查询 Session 或 Token。

### Session（服务端内存态临时记录）

多见于需“状态保存”的 Web 服务器（如 HTTP + Gin/Fiber）

1. 用户登录成功 ⇒ 服务端创建结构体
   ```go
   type UserSession struct {
       UID      string
       LoginAt  time.Time
       Role     string
   }
   ```
1. 生成 session ID ⇒ `sessionID := uuid.New()`
1. 服务端保存：`sessionStore[sessionID] = userSession`
1. 返回 session ID 到 cookie：
   ```go
   http.SetCookie(w, &http.Cookie{
       Name:  "sessionid",
       Value: sessionID,
   })
   ```

存储介质选择

| 介质    | 优点             | 缺点                 |
| ------- | ---------------- | -------------------- |
| 内存    | 快速，简单       | 重启丢失，扩展困难   |
| Redis   | 高性能，共享安全 | 需支持 Redis cluster |
| DB/文件 | 持久             | 速度慢               |

服务端避免存储大量状态 => Redis 记录 session 是常见架构设计

### Token（分布式场景下的“卡号 + 消磁验证”）

适用多节点服务（如微服务、前后端分离、App API）

JWT 示例说明（常用）

```json
{
  "header": { "alg": "HS256", "typ": "JWT" },
  "payload": {
    "sub": "1234567890",
    "name": "John Doe",
    "iat": 1516239022,
    "exp": 1516242622
  },
  "signature": "签名串"
}
```

Go 中签发与验证

```go
token := jwt.NewWithClaims(jwt.SigningMethodHS256, jwt.MapClaims{
    "user_id": 123,
    "exp":     time.Now().Add(time.Hour * 72).Unix(),
})

tokenString, _ := token.SignedString([]byte("secret"))
```

App 传输技巧

```http
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

| 特点       | 描述                                  |
| ---------- | ------------------------------------- |
| 无状态     | 无需服务端查找会话                    |
| 防篡改     | 若密钥不泄露，_signed payload_ 可信任 |
| 适合分布式 | 每个微服务能直接检验 token合法性      |

存储凭据注意：

- 不应放在 Cookie 中（尤其无 `HttpOnly` 设置）
- 移动端可存于内存或安全沙箱
- 存储在 Cookie 中应配置好 `SameSite/Secure/Path`

## 最佳实践

| 场景/能力    | Cookie              | Session                | Token (JWT/OAuth)           |
| ------------ | ------------------- | ---------------------- | --------------------------- |
| 安全性       | 易中间人攻击        | 安全但严重依赖服务端   | 若被盗则无法反撤            |
| 存储位置     | 客户端              | 服务端                 | 客户端内存（或SecureStore） |
| 状态性       | 无状态传输          | 服务端有状态           | 完全无状态                  |
| 生命期控制   | 前端可监听删        | 后端统一销毁           | 支持自定义有效期控制        |
| 多实例部署   | 不好跨域跟踪        | 依赖共享 session store | 极佳兼容                    |
| 实例热升级   | 丢 Session 实现风险 | 有缓存则无感知         | 一般无影响                  |
| 支持统一认证 | 须二次鉴权          | 同 Session             | 可集成 SSO/OIDC             |

现代Web应用：

- Token (JWT) + 刷新令牌机制
- 刷新令牌存储在HttpOnly Cookie中
- 访问令 Token 存在内存中（防XSS）

传统Web应用：

- Session + 服务器存储
- 配置SameSite=Strict Cookie
- 防护CSRF的Token

安全关键系统：

```mermaid
graph LR
A[用户] --> B{登录}
B -->|成功| C[签发JWT/Session]
B -->|失败| D[失败处理]
C --> E[令牌验证]
E -->|有效| F[访问资源]
E -->|无效| G[重新认证]
```

传统网站/电商系统：`Cookie + Session（Redis）`

前后端分离 App / API服务：`JWT Token` 或 `OAuth2 + Token`

混合场景：

- 登录跳转页用 Cookie 中转 session
- API 接口区使用 token 保障分布式访问安全

# 输入网址到网页显示

从输入网址到网页显示，涉及了完整的网络通信栈：

- 应用层：HTTP协议、HTML解析
- 传输层：TCP连接、流量控制
- 网络层：IP路由、DNS解析
- 链路层：以太网帧、ARP地址解析

## 主要步骤

### 1. URL解析与准备

用户输入如 `https://www.example.com:8080/path?query=param#section` 的URL后，浏览器首先解析URL的各个组成部分：

- 协议（Scheme）：`https` 表示使用安全HTTP协议
- 域名（Host）：`www.example.com` 目标服务器地址
- 端口（Port）：`:8080` 指定通信端口，默认80(HTTP)，443(HTTPS)
- 路径（Path）：`/path` 请求的具体资源位置
- 查询参数（Query）：`?query=param` 传递的参数
- 片段标识符（Fragment）：`#section` 页面内的定位标记

关键处理：

- 浏览器自动补全协议（无协议则默认http://）
- 识别端口号（HTTPS默认443，HTTP默认80）
- 解析URL编码字符（如 `%20` → 空格）

### 2. DNS域名解析 - 互联网地址簿查询

浏览器将域名转换为IP地址的过程称为DNS(Domain Name System)解析

DNS查询流程

```
浏览器缓存检查
    ↓
操作系统DNS缓存 (hosts文件)
    ↓
路由器DNS缓存
    ↓
ISP DNS服务器 (递归解析)
    ↓
根域名服务器 (.)
    ↓
顶级域名服务器 (.com)
    ↓
权威域名服务器 (example.com)
    ↓
返回IP地址 93.184.216.34
```

技术细节：

- DNS查询类型：递归查询 vs 迭代查询
- DNS记录类型：A记录(IPV4)、AAAA记录(IPV6)、CNAME记录(别名)、MX记录(邮件服务器)
- DNS缓存策略：TTL(Time to Live)控制记录有效时间
- DNS安全扩展：DNSSEC防止DNS污染和劫持

### 3. TCP连接建立 - 网络通道搭建

三次握手协议

```
客户端 → 服务器: SYN (seq=x)           # 同步序列号
服务器 → 客户端: SYN+ACK (seq=y, ack=x+1) # 确认并同步
客户端 → 服务器: ACK (ack=y+1)          # 确认连接建立
```

HTTPS安全握手（TLS/SSL）

```
ClientHello                    # 客户端支持的加密算法列表
    ↓
ServerHello + Certificate     # 服务器选择算法并发送证书
    ↓
Certificate Verify           # 客户端验证服务器证书真实性
    ↓
Key Exchange/Finished        # 密钥协商与完成信号
```

### 4. HTTP请求发送 - 数据传输

HTTP请求结构

```
GET /index.html HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)
Accept: text/html,application/xhtml+xml
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Cookie: sessionid=abc123
```

HTTP/2特性优化

```
┌─────────────────────────────────────────┐
│           HTTP/2 多路复用              │
├─────────────┬─────────────┬─────────────┤
│ Stream #1   │ Stream #2   │ Stream #3   │
│ GET /style  │ GET /script │ GET /image  │
│ css         │ js          │ jpg         │
└─────────────┴─────────────┴─────────────┘
```

- HTTP/2多路复用：单个TCP连接并行处理多个请求
- 二进制分帧：提高解析效率
- 头部压缩：HPACK算法减少开销
- 服务器推送：服务器主动推送客户端可能需要的资源

### 5. 服务器处理 - 后端逻辑执行

网络架构处理链

```
负载均衡器 → 反向代理 → Web服务器 → 应用服务器 → 微服务
    ↓           ↓           ↓            ↓            ↓
分发流量    SSL终止    静态资源     业务逻辑    API服务
           压缩       缓存处理     数据库调用   数据处理
```

技术栈处理

- Web服务器：Nginx/Apache处理静态资源
- 反向代理：负责负载均衡和请求路由
- 应用容器：Tomcat/Node.js/Go服务处理动态请求
- 数据库访问：通过连接池管理数据库连接
- 缓存机制：Redis/Memcached提升响应速度

数据库查询优化

```sql
-- 查询优化示例
SELECT u.name, p.title
FROM users u
JOIN posts p ON u.id = p.user_id
WHERE u.status = 'active'
INDEX: users(status), posts(user_id)
```

### 6. HTTP响应返回 - 结果传输

响应结构

```
HTTP/1.1 200 OK
Content-Type: text/html; charset=utf-8
Content-Length: 1234
Cache-Control: max-age=3600
Server: nginx/1.18.0
Date: Mon, 01 Jan 2024 12:00:00 GMT

<!DOCTYPE html>
<html>
<head>...</head>
<body>...</body>
</html>
```

响应状态码分类

```
2xx 成功      3xx 重定向    4xx 客户端错误    5xx 服务器错误
200 OK        301 Moved    400 Bad Request   500 Internal
201 Created   302 Found    401 Unauthorized  502 Bad Gateway
204 No Cnt    304 Not Mod  403 Forbidden     503 Unavailable
206 Partial   307 Redirect 404 Not Found     504 Gateway Time
```

### 7. 浏览器渲染处理

关键渲染路径

```
HTML解析 → DOM树构建 → CSS解析 → CSSOM树 → Render树
   ↓          ↓           ↓          ↓          ↓
 文档对象   样式应用    计算样式    几何计算    像素绘制
```

渲染优化技术

```
关键渲染路径优化:
┌──────────────────────┬──────────────────────┐
│     阻塞渲染         │     非阻塞渲染        │
├──────────────────────┼──────────────────────┤
│ HTML文档解析          │ 异步资源加载         │
│ CSS样式解析(CRITICAL) │ JavaScript(Async)    │
│ 字体加载阻塞          │ 图片懒加载           │
│ 外部样式表加载        │ 视频流媒体加载       │
└──────────────────────┴──────────────────────┘
```

### 8. 后续资源加载

浏览器遇到页面中的外部资源(如CSS、JS、图片、字体等)会重复 DNS解析、TCP连接和资源获取的过程：

资源依赖关系

```
index.html
  ├── style.css
  ├── script.js
  ├── image1.jpg
  ├── image2.png
  └── font.woff2
```

资源加载顺序

- HTML文档是按顺序解析的
- CSS会阻塞渲染直至加载完成
- JavaScript默认会阻塞HTML解析，除非使用async/defer
- 图片等其他资源异步加载

渲染优化

- 关键CSS(Critical CSS)：内联首屏关键样式
- 资源预加载：通过<link rel="preload">提前加载关键资源
- 懒加载：图片等非关键资源延迟加载
- CDN加速：资源分发到全球边缘节点

并行加载优化（并发TCP连接限制）

- HTTP/1.1: 每域名6个连接
- HTTP/2: 单域名多路复用
- HTTP/3: 切换到UDP协议(QUIC)

## 关键网络性能指标

网络层指标

```
┌─────────────────────────────────────────────────────────┐
│ 性能指标                  │ 标准值         │ 优化目标     │
├─────────────────────────────────────────────────────────┤
│ DNS查询时间              │ < 50ms        │ < 20ms      │
│ TCP连接时间              │ < 100ms       │ < 50ms      │
│ SSL握手时间             │ < 200ms       │ < 100ms     │
│ TTFB(首字节时间)         │ < 200ms       │ < 100ms     │
│ DOM内容加载完成时间       │ < 3秒         │ < 2秒       │
│ 页面完全加载时间          │ < 5秒         │ < 3秒       │
└─────────────────────────────────────────────────────────┘
```

缓存策略

```
Cache-Control策略:
max-age=3600          # 1小时浏览器缓存
no-cache              # 每次验证后使用缓存
no-store              # 禁止缓存
must-revalidate       # 强制验证新鲜度
```

安全防护机制

1. HTTPS加密传输
1. HSTS强制HTTPS
1. CSP防范XSS
1. CSRF Token防护
1. 输入验证与过滤
1. Rate Limiting限流
1. WAF防火墙防护

## 数据在网络模型中的流动

应用层（第7层）到物理层（第1层）的封装过程

```text
应用层数据:
GET / HTTP/1.1
Host: www.example.com

↓ 表示层处理
加密数据: [加密后的HTTP请求]

↓ 会话层处理
会话信息 + 加密数据

↓ 传输层处理 (TCP)
TCP头部 + 数据
源端口(54321) 目标端口(443) 序列号(1000) 确认号(2000) 标志位(SYN=0, ACK=1)

↓ 网络层处理 (IP)
IP头部 + TCP段
版本(4) 源IP(192.168.1.100) 目标IP(93.184.216.34) 协议(6) TTL(64)

↓ 数据链路层处理 (以太网)
以太网帧头部 + IP包 + 以太网帧尾部
目标MAC 源MAC 类型(0x0800) | IP数据包 | FCS校验

↓ 物理层处理
010101010101110010101010... (比特流通过物理介质传输)
```

物理层（第1层）到应用层（第7层）的解封装过程

```text
物理层接收:
010101010101110010101010... (比特流)

↓ 数据链路层处理
解析帧头部，检查FCS，提取IP包

↓ 网络层处理
解析IP头部，检查目标IP，提取TCP段

↓ 传输层处理
解析TCP头部，重组数据，检查序列号

↓ 会话层处理
处理会话状态管理

↓ 表示层处理
解密SSL/TLS数据，解压缩

↓ 应用层处理
解析HTTP响应，提取HTML内容
```

# 交换机 V.S. 路由器

| 特性     | 交换机            | 路由器                 |
| -------- | ----------------- | ---------------------- |
| 工作层次 | 第2层(数据链路层) | 第3层(网络层)          |
| 转发依据 | MAC地址           | IP地址                 |
| 处理单元 | 数据帧            | 数据包                 |
| 连接范围 | 同一网络内的设备  | 不同网络之间           |
| 广播处理 | 转发广播帧        | 不转发广播包           |
| 智能程度 | 较低              | 较高                   |
| 底层设备 | 以太网交换机      | 网络设备               |
| 地址学习 | 自学习MAC地址     | 配置静态路由或动态协议 |
| 性能     | 高速、大吞吐量    | 功能全面但有性能开销   |
| 典型应用 | 局域网内部互联    | 网络互联、互联网连接   |

## 基本概念

交换机（Switch）：工作在OSI模型的数据链路层（第2层），主要功能是根据MAC地址（物理地址）在局域网内部转发数据帧。它负责连接同一网络中的多个设备

路由器（Router）：工作在OSI模型的网络层（第3层），主要功能是根据IP地址（逻辑地址）在不同网络之间转发数据包。它负责连接不同的网络

## 核心区别

1. 工作层次不同

```
OSI七层模型：
应用层      (第7层)
表示层      (第6层)
会话层      (第5层)
传输层      (第4层)
网络层    路由器(第3层)
数据链路层  交换机(第2层)
物理层      (第1层)
```

2. 转发依据不同

交换机：基于MAC地址表

```java
// 示例：交换机MAC地址表
Map<String, Integer> macTable = new HashMap<>();
macTable.put("00:1A:2B:3C:4D:5E", 1);  // MAC地址 -> 端口号
macTable.put("00:F1:7A:B2:C3:D4", 2);
```

路由器：基于路由表

```go
// 示例：路由器路由表
type Route struct {
    Destination string  // 目标网络
    Netmask     string  // 子网掩码
    Gateway     string  // 下一跳
    Interface   string  // 出接口
    Metric      int     // 度量值
}

// 路由表条目
var routingTable = []Route{
    {Destination: "192.168.1.0", Netmask: "255.255.255.0", Gateway: "0.0.0.0", Interface: "eth0"},
    {Destination: "0.0.0.0", Netmask: "0.0.0.0", Gateway: "192.168.1.1", Interface: "eth0"},
}
```

3. 连接范围不同

交换机：连接同一网络内的设备（局域网内部）

- 所有设备通常属于同一个广播域
- 使用相同的IP网段

路由器：连接不同网络之间

- 创建独立的广播域
- 连接不同IP网段的设备

4. 地址类型不同

交换机：处理MAC地址（物理地址）

- 48位地址，由设备制造商分配
- 用于局域网内设备标识

路由器：处理IP地址（逻辑地址）

- IPv4为32位，IPv6为128位
- 用于不同网络间设备标识路由

5. 广播处理能力不同

交换机：转发广播帧

- 广播会发送到交换机的所有端口
- 无法隔离广播流量

路由器：不转发广播包

- 广播包被限制在特定网络内
- 减少网络广播风暴

6. 功能复杂度不同

交换机功能较为简单：

- 基本转发功能
- VLAN支持
- 生成树协议(STP)
- MAC地址表学习

路由器功能更复杂丰富：

- 路由决策
- 网络地址转换(NAT)
- 防火墙功能
- 访问控制列表(ACL)
- 动态路由协议支持

7. 性能特点

交换机：

- 转发速度快（通常比起交换机）
- 延迟低
- 吞吐量大

路由器：

- 转发速度相对较慢
- 功能更全面，但会有额外开销
- 支持更多网络协议

8. 应用场景

交换机适用场景：

- 企业局域网构建
- 服务器区域连接
- 高密度设备互联
- 需要高速数据传输的场景

路由器适用场景：

- 互联网连接点
- 不同局域网互联
- 广域网连接
- 网络边界安全控制

## 协同工作

在实际网络中，交换机和路由器经常协同工作：

```
互联网
    |
[路由器] ←→ 连接互联网和内部网络
    |
[交换机] ←→ 连接内部网络中的所有设备
    |   |   |
[PC1][PC2][PC3]
```

在这种配置中：

1. PC1想要访问互联网
1. 数据先到达交换机，交换机根据MAC表将数据转发到路由器
1. 路由器收到数据，检查路由表，决定如何转发到互联网
