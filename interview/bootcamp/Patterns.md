# 设计模式

设计模式是软件开发经验的结晶，不是教条而是指南。合理使用设计模式可以提高代码质量，使系统更加健壮、可维护和可扩展，但应避免为了模式而模式，在简单问题中过度使用复杂模式。

## 设计模式应用原则

- 单一职责原则 (SRP)：一个类只有一个引起变化的原因
- 开闭原则 (OCP)：对扩展开放，对修改关闭
- 里氏替换原则 (LSP)：子类可替换父类
- 接口隔离原则 (ISP)：使用多个专门接口而非单一总接口
- 依赖倒置原则 (DIP)：高层模块不应依赖低层模块

## 设计模式选择指南

- 创建需求：关注对象创建方式 → 创建型模式
- 结构组织：关注类和对象组合 → 结构型模式
- 行为交互：关注对象间通信 → 行为型模式
- 领域问题：根据特定领域场景选择对应模式
- 设计原则：符合SOLID原则，避免过度设计

## 创建型设计模式（5种）

关注对象的创建过程，封装了实例化逻辑，使系统独立于对象的创建方式

1. 单例模式 (Singleton)
   - 核心思想：确保一个类只有一个实例，并提供全局访问点
   - 适用场景：日志记录器、配置管理器、数据库连接池
   - 优点：节省资源，全局访问点，唯一控制权
   - 缺点：可能隐藏依赖关系，线程安全问题

2. 工厂方法模式 (Factory Method)
   - 核心思想：定义创建对象的接口，让子类决定实例化哪个类
   - 适用场景：需要创建多种相似对象，但客户代码不应知道具体类
   - 优点：解耦创建者与产品，便于扩展
   - 缺点：可能类数量增加

3. 抽象工厂模式 (Abstract Factory)
   - 核心思想：提供接口创建相关对象家族，而不需指定具体类
   - 适用场景：需要创建产品族（如跨平台UI组件）
   - 优点：确保产品族一致性，产品间约束明确
   - 缺点：扩展产品族困难

4. 建造者模式 (Builder)
   - 核心思想：将复杂对象的构建与表示分离
   - 适用场景：构建复杂对象，具有多个可选参数
   - 优点：灵活控制构建过程，可创建不同表示
   - 缺点：代码结构更复杂

5. 原型模式 (Prototype)
   - 核心思想：通过复制原型实例创建新对象
   - 适用场景：创建大量相似对象，避免工厂层次
   - 优点：避免构造函数复杂性，动态创建对象
   - 缺点：克隆过程可能复杂，循环引用难处理

## 结构型设计模式（7种）

关注类和对象的组合，形成灵活且高效的更大结构

6. 适配器模式 (Adapter)
   - 核心思想：将一个类的接口转换成期望的接口
   - 适用场景：集成第三方库，统一不同接口格式
   - 优点：无需修改现有代码，实现接口兼容
   - 缺点：增加系统复杂度，可能降低性能

7. 桥接模式 (Bridge)
   - 核心思想：将抽象与实现分离，使它们独立变化
   - 适用场景：类存在多个维度变化，避免深层继承
   - 优点：避免类爆炸，提高可扩展性
   - 缺点：增加系统复杂度，设计难度较高

8. 组合模式 (Composite)
   - 核心思想：将对象组合成树形结构表示"部分-整体"层次
   - 适用场景：树形结构数据处理（文件系统、UI控件树）
   - 优点：统一处理单个和组合对象
   - 缺点：设计复杂度增加，限制类接口

9. 装饰器模式 (Decorator)
   - 核心思想：动态地给对象添加额外职责
   - 适用场景：在运行时扩展对象功能，避免生成子类
   - 优点：比继承更灵活，可叠加多个功能
   - 缺点：产生大量小对象，调试困难

10. 外观模式 (Facade)
    - 核心思想：为复杂子系统提供统一简化接口
    - 适用场景：简化复杂系统接口，降低耦合
    - 优点：降低子系统与客户端的耦合度
    - 缺点：可能限制系统的灵活性

11. 享元模式 (Flyweight)
    - 核心思想：运用共享技术支持大量细粒度对象
    - 适用场景：创建大量相似对象，状态可共享
    - 优点：显著减少对象数量，节省内存
    - 缺点：增加了内部/外部状态区分的复杂度

12. 代理模式 (Proxy)
    - 核心思想：提供代理对象控制对目标对象的访问
    - 适用场景：控制对象访问、延迟初始化、远程访问
    - 优点：增加额外控制，实现精细访问权限
    - 缺点：可能降低系统性能，增加层级

## 行为型设计模式（11种）

关注对象之间的通信和职责分配，描述对象间如何交互及划分责任

13. 责任链模式 (Chain of Responsibility)
    - 核心思想：将请求的发送者和接收者解耦，多个对象处理请求
    - 适用场景：多个对象可能处理同一请求，具体处理者由运行时决定
    - 优点：松耦合，动态指定处理者，可配置性高
    - 缺点：可能没有处理者，请求可能不被处理

14. 命令模式 (Command)
    - 核心思想：将请求封装成对象，支持参数化和可撤销操作
    - 适用场景：需要支持撤销/重做、操作队列、宏命令
    - 优点：解耦请求发送者与接收者，支持操作日志
    - 缺点：可能产生大量具体命令类

15. 解释器模式 (Interpreter)
    - 核心思想：定义语言的文法表示并解释句子
    - 适用场景：需要解释执行特定语言的表达式
    - 优点：简单语言的表达式解释，易于扩展语法
    - 缺点：类数量增加，解释器效率可能不高

16. 中介者模式 (Mediator)
    - 核心思想：用中介对象封装一系列对象交互，减少耦合
    - 适用场景：对象间复杂交互，需要集中控制交互
    - 优点：减少子类数量，集中控制交互，降低耦合
    - 缺点：中介者可能变成庞大对象，成为系统瓶颈

17. 备忘录模式 (Memento)
    - 核心思想：在不破坏封装前提下捕获并保存对象内部状态
    - 适用场景：需要保存和恢复对象状态，支持撤销操作
    - 优点：保持对象封装性，支持状态撤销
    - 缺点：消耗内存，状态可能不一致

18. 观察者模式 (Observer)
    - 核心思想：定义对象间一对多依赖关系，状态改变时通知所有依赖者
    - 适用场景：对象改变需通知其他对象，但不关心具体哪些对象
    - 优点：松耦合，支持广播通信
    - 缺点：可能导致意外的更新，通知过程不可靠

19. 状态模式 (State)
    - 核心思想：对象内部状态改变时改变其行为
    - 适用场景：对象行为依赖于状态，避免大量条件判断
    - 优点：状态转换显式化，避免条件语句
    - 缺点：增加类数量，可能引起状态转换逻辑复杂

20. 策略模式 (Strategy)
    - 核心思想：定义算法族，把它们封装起来并可以相互替换
    - 适用场景：多种算法，需要运行时选择，避免条件语句
    - 优点：替代条件语句，可灵活更换算法
    - 缺点：客户端必须了解所有策略，策略过多时管理困难

21. 模板方法模式 (Template Method)
    - 核心思想：定义算法骨架，子类实现具体步骤
    - 适用场景：多个子类有公共算法框架，只有部分步骤不同
    - 优点：复用公共代码，灵活定义变化部分
    - 缺点：可能通过子类化扩展模板，导致类数量增加

22. 访问者模式 (Visitor)
    - 核心思想：在不改变类的前提下定义作用于这些对象的新操作
    - 适用场景：数据结构稳定，操作经常变化
    - 优点：操作集中，数据结构与操作分离
    - 缺点：增加新元素困难，破坏封装

23. 迭代器模式 (Iterator)
    - 核心思想：提供一种方法顺序访问聚合对象，不暴露内部表示
    - 适用场景：需要遍历聚合对象，不暴露内部结构
    - 优点：统一遍历接口，支持多种遍历方式
    - 缺点：单独定义接口，增加类复杂度

# 工厂 vs 抽象工厂

## 演化关系

抽象工厂是工厂方法的扩展：

- 工厂方法关注单个产品层次结构
- 抽象工厂关注多个产品族的创建

抽象工厂可以包含多个工厂方法：

- 抽象工厂通常由多个工厂方法组成
- 每个工厂方法可以应用工厂方法模式

## 核心区别

工厂模式（包括工厂方法模式和抽象工厂模式）是创建型设计模式的核心，但它们解决不同层次的问题，有明确的分界线。工厂方法和抽象工厂模式的区别本质上是单一维度创建与多维度创建的区别

- 工厂方法：解决"如何创建产品"，关注单个产品的创建
- 抽象工厂：解决"如何创建产品族"，关注整个产品生态系统的构建

选择哪种模式取决于你是需要独立创建产品，还是需要创建相互依赖的产品集合。理解这种差异有助于正确选择最适合当前需求的模式，并为系统的有效架构提供基础。

## 关键区别

| 维度       | 工厂方法模式                 | 抽象工厂模式                               |
| ---------- | ---------------------------- | ------------------------------------------ |
| 产品范围   | 创建单个产品或一组相关产品   | 创建多个产品族（一组相关或相互依赖的产品） |
| 创建复杂度 | 简单，一个工厂只创建一种产品 | 复杂，一个工厂创建整个产品族               |
| 扩展性     | 添加新产品时需创建新工厂     | 添加新产品族时需创建新工厂                 |
| 设计层次   | 产品等级结构（单维度）       | 产品族（多维度）                           |
| 接口复杂度 | 简单接口，通常只有一个方法   | 复杂接口，包含多个创建方法                 |
| 关联性     | 产品之间各自独立             | 产品之间高度关联，构成一个完整产品         |
| 实现难度   | 实现相对简单                 | 实现相对复杂，协调问题更多                 |

## 细节区别

### 工厂

定义一个创建对象的接口，但让子类决定实例化哪一个类。工厂方法使一个类的实例化延迟到其子类

选择：

- 只需要创建一个产品
- 产品之间相互独立
- 希望保持接口简单
- 需要快速扩展产品种类

应用场景：

- 当类不知道它所必须创建的对象的类时
- 当类希望由其子类来指定它所创建的对象时
- 当类将创建对象的职责委托给多个帮助子类中的某一个，并且你希望将哪一个帮助子类是代理者这一信息局部化时

例子：

- 数据库连接工厂（根据数据库类型创建不同连接器）
- 文件解析器工厂（根据文件类型创建不同解析器）

### 抽象工厂

提供一个接口，用于创建相关或依赖对象的家族，而不需要明确指定具体类

选择：

- 需要创建一组相关产品
- 产品之间有内在联系
- 需要保证产品之间的兼容性
- 系统有多个产品维度

应用场景：

- 当系统需要独立于它的产品创建、组合和表示时
- 当一个系统要由多个产品系列中的一个来配置时
- 当你要强调一系列相关的产品对象的设计以便进行联合使用时
- 当你提供一个产品类库，而只想显示它们的接口而不是实现时

例子：

- 跨平台UI组件工厂（创建Windows/Mac风格的所有UI组件）
- 主题工厂（创建深色/浅色主题的所有元素）
- 游戏资源工厂（创建不同场景下的所有资源）

# 创建型设计模式

主要关注对象的创建过程，它们封装了实例化逻辑，使系统更具灵活性和可扩展性，独立于如何创建、组合和表示对象

| 模式     | 核心思想                 | 适用场景           | 主要优点           |
| -------- | ------------------------ | ------------------ | ------------------ |
| 单例     | 确保类只有一个实例       | 日志、配置、连接池 | 节省资源，全局访问 |
| 工厂方法 | 子类决定实例化哪个类     | 多种相似对象的创建 | 解耦创建者和产品   |
| 抽象工厂 | 创建产品族               | 跨平台UI、主题系统 | 确保产品一致性     |
| 建造者   | 分离复杂对象的构建与表示 | 复杂对象配置步骤   | 灵活控制构建过程   |
| 原型     | 通过复制创建新对象       | 运行时动态创建对象 | 避免工厂层次结构   |

## 单例模式

单例模式（Singleton Pattern）是一种创建型设计模式，它确保一个类只有一个实例，并提供一个全局访问点。

在分布式配置中心、数据库连接池、日志客户端等组件里，往往要求：

- 全局唯一实例（节省资源、保证一致性）。
- 延迟初始化（启动快、按需创建）。
- 并发安全（高并发服务不能创建多个实例）。
- 可测试（UT 里能替换/mock 单例）。

经典单例 + 接口抽象 最符合需求

| 模式         | 是否满足需求 | 备注                         |
| ------------ | ------------ | ---------------------------- |
| 单例         | ✅           | 全局唯一+延迟初始化          |
| 依赖注入容器 | ✅           | 也能保证单例，但引入额外框架 |
| 简单全局变量 | ❌           | 无延迟初始化、无法 mock      |

### 常见分类

#### 饿汉模式（静态实例化）

核心：程序启动时直接创建实例
优点：线程安全，初始化简单
缺点：资源浪费，实例可能未被使用

```go
package singleton

type Singleton struct{}

var instance *Singleton = &Singleton{} // 启动即实例化

func GetInstance() *Singleton {
    return instance
}
```

```python
class Singleton:
    instance = None  # 类属性，全局唯一

    def __init__(self):
        if Singleton.instance is not None:
            return TypeError("Cannot create multiple instances")

    @classmethod
    def create_instance(cls):
        if Singleton.instance is None:
            Singleton.instance = cls()
            print("Instance created")
        return Singleton.instance
```

```typescript
class Singleton {
  private static instance: Singleton;

  private constructor() {}

  static getInstance(): Singleton {
    if (!Singleton.instance) {
      Singleton.instance = new Singleton();
    }
    return Singleton.instance;
  }
}
```

#### 懒汉模式

核心：延迟实例化，当首次调用 `getInstance()` 时创建
优化：添加锁防止多线程并发问题

```go
package singleton

var (
    instance *Singleton
    mu        sync.Mutex
)

type Singleton struct{}

func GetInstance() *Singleton {
    mu.Lock()
    defer mu.Unlock()
    if instance == nil {
        instance = &Singleton{}
    }
    return instance
}
```

```python
class Singleton:
    _instance = None
    _lock = threading.Lock()

    def __new__(cls, *args, kwargs):
        with cls._lock:
            if cls._instance is None:
                cls._instance = super().__new__(cls)
        return cls._instance
```

#### 静态方法 + 泛型优化（多实例场景）

扩展：通过泛型支持 `Double Check Locking (DCL)` 实现双重检查锁定模式

```typescript
class Singleton<T = any> {
  private static _instances: { [key: string]: Singleton<T> } = {};

  getInstance(key: string): Singleton<T> {
    if (!this._instances[key]) {
      this._instances[key] = new Singleton<T>();
    }
    return this._instances[key];
  }
}
```

#### 初始化器模式（Golang 特色）

使用函数返回实例，适合功能较复杂的单例

```go
package singleton

var createInstance func() *Singleton

func Init(createFunc func() *Singleton) {
    createInstance = createFunc
}

func GetInstance() *Singleton {
    if createInstance == nil {
        panic("Singleton not initialized")
    }
    return createInstance()
}
```

#### 使用sync.Once实现（Golang特色）

```go
package singleton

import "sync"

type Singleton struct{}

var instance *Singleton
var once sync.Once

func GetInstance() *Singleton {
    once.Do(func() {
        instance = &Singleton{} // 确保只执行一次
    })
    return instance
}

```

#### 密封类约束（TypeScript 语言特性）

场景：确保类继承链的控制权在 Singleton 内部

```typescript
class SingletonBase extends FinalClass {
    private constructor() {
        super();
    }
    static instance(): SingletonBase {
        return new SingletonBase();
    }
}

sealed class SingletonBase {
    int(): void { }
}
```

### 常见问题

反射破解（尤其是 Python/C#）

```python
import sys
sys.modules[__name__].instances.append(new Singleton()) # 绕过 init
```

子类继承破坏（未考虑派生类）

```typescript
class Child extends Singleton {}
const s1 = new Child();
const s2 = new Child(); // 实例冲突
```

### 模式对比

| 实现方式             | 延迟加载 | 线程安全 | 性能 | 防反序列化 |
| -------------------- | -------- | -------- | ---- | ---------- |
| 懒汉式（线程不安全） | ✅       | ❌       | 高   | ❌         |
| 懒汉式（同步方法）   | ✅       | ✅       | 低   | ❌         |
| 双重检查锁           | ✅       | ✅       | 高   | ❌         |
| 饿汉式               | ❌       | ✅       | 高   | ❌         |
| 静态内部类           | ✅       | ✅       | 高   | ❌         |
| 枚举（Java特有）     | ❌       | ✅       | 高   | ✅         |

| 模式         | 优点                 | 缺点                   | 适用场景                   |
| ------------ | -------------------- | ---------------------- | -------------------------- |
| 饿汉模式     | 线程安全、初始化简单 | 资源浪费               | Web 框架（如 Gin）静态路由 |
| 懒汉模式     | 延迟初始化节省资源   | 需手动加锁             | 配置缓存、数据库连接池     |
| 泛型单例     | 支持多实例           | 兼容性弱（TypeScript） | 日志系统、缓存管理         |
| 初始化器模式 | 灵活配置             | 需显式初始化           | 微服务配置加载             |

### 注意事项

并发问题：必须避免竞态条件（采用 `sync.Mutex` / `threading.Lock`）
依赖注入：现代框架（如 Gin/Django）更建议使用依赖注入替代单例
设计陷阱：过度使用单例可能导致全局依赖和难以测试的代码

在实际项目中，许多团队已弃用单例模式，改用依赖注入或注册模式（如 Python 的 `singleton-decorator` 库），因为单例只能隐藏复杂性，不能解决根本问题

## 工厂方法模式 (Factory Method)

定义一个用于创建对象的接口，但让子类决定实例化哪个类。工厂方法使一个类的实例化延迟到其子类

### 适用场景

- 不同类型的文档处理（PDF、Word等）
- 不同类型的数据库适配器
- 不同类型的支付处理器
- 根据用户类型创建不同的账户

### 实现示例

#### Golang 实现

```go
package factory

import "fmt"

// Product 接口
type PaymentProcessor interface {
    Process(amount float64) string
}

// 具体产品 A
type CreditCardProcessor struct{}

func (c *CreditCardProcessor) Process(amount float64) string {
    return fmt.Sprintf("信用卡支付金额: %.2f", amount)
}

// 具体产品 B
type PayPalProcessor struct{}

func (p *PayPalProcessor) Process(amount float64) string {
    return fmt.Sprintf("PayPal支付金额: %.2f", amount)
}

// 工厂接口
type PaymentProcessorFactory interface {
    CreateProcessor() PaymentProcessor
}

// 具体工厂 A
type CreditCardFactory struct{}

func (c *CreditCardFactory) CreateProcessor() PaymentProcessor {
    return &CreditCardProcessor{}
}

// 具体工厂 B
type PayPalFactory struct{}

func (p *PayPalFactory) CreateProcessor() PaymentProcessor {
    return &PayPalProcessor{}
}

// 客户端代码
func ProcessPayment(factory PaymentProcessorProcessorFactory, amount float64) {
    processor := factory.CreateProcessor()
    result := processor.Process(amount)
    fmt.Println(result)
}

func main() {
    // 使用信用卡支付
    creditCardFactory := &CreditCardFactory{}
    ProcessPayment(creditCardFactory, 100.0)

    // 使用PayPal支付
    payPalFactory := &PayPalFactory{}
    ProcessPayment(payPalFactory, 200.0)
}
```

#### Python 实现

```python
from abc import ABC, abstractmethod

# 产品接口
class PaymentProcessor(ABC):
    @abstractmethod
    def process(self, amount):
        pass

# 具体产品 A
class CreditCardProcessor(PaymentProcessor):
    def process(self, amount):
        return f"信用卡支付金额: {amount:.2f}"

# 具体产品 B
class PayPalProcessor(PaymentProcessor):
    def process(self, amount):
        return f"PayPal支付金额: {amount:.2f}"

# 工厂接口
class PaymentFactory(ABC):
    @abstractmethod
    def create_processor(self):
        pass

# 具体工厂 A
class CreditCardFactory(PaymentFactory):
    def create_processor(self):
        return CreditCardProcessor()

# 具体工厂 B
class PayPalFactory(PaymentFactory):
    def create_processor(self):
        return PayPalProcessor()

# 客户端代码
def process_payment(factory: PaymentFactory, amount: float):
    processor = factory.create_processor()
    print(processor.process(amount))

# 使用示例
process_payment(CreditCardFactory(), 100.0)
process_payment(PayPalFactory(), 200.0)
```

#### TypeScript 实现

```typescript
// 产品接口
interface PaymentProcessor {
  process(amount: number): string;
}

// 具体产品 A
class CreditCardProcessor implements PaymentProcessor {
  process(amount: number): string {
    return `信用卡支付金额: ${amount.toFixed(2)}`;
  }
}

// 具体产品 B
class PayPalProcessor implements PaymentProcessor {
  process(amount: number): string {
    return `PayPal支付金额: ${amount.toFixed(2)}`;
  }
}

// 工厂接口
abstract class PaymentFactory {
  abstract createProcessor(): PaymentProcessor;
}

// 具体工厂 A
class CreditCardFactory extends PaymentFactory {
  createProcessor(): PaymentProcessor {
    return new CreditCardProcessor();
  }
}

// 具体工厂 B
class PayPalFactory extends PaymentFactory {
  createProcessor(): PaymentProcessor {
    return new PayPalProcessor();
  }
}

// 客户端代码
function processPayment(factory: PaymentFactory, amount: number) {
  const processor = factory.createProcessor();
  console.log(processor.process(amount));
}

// 使用示例
processPayment(new CreditCardFactory(), 100);
processPayment(new PayPalFactory(), 200);
```

---

## 抽象工厂模式 (Abstract Factory)

提供一个接口，用于创建相关或依赖对象的家族，而不需要明确指定具体类

### 适用场景

- 跨平台UI组件创建（Windows、macOS）
- 不同品牌设备的UI组件
- 不同类型的数据库连接和查询对象
- 不同主题的系统组件

### 实现示例

#### Golang 实现

```go
package abstractfactory

import "fmt"

// 产品族接口 A
type Button interface {
    Render()
    OnClick()
}

// 产品族接口 B
type Checkbox interface {
    Render()
    Toggle()
}

// 具体产品 A1
class WindowsButton implements Button {
    Render() {
        fmt.Println("渲染Windows风格按钮")
    }

    OnClick() {
        fmt.Println("Windows按钮点击事件")
    }
}

// 具体产品 A2
class MacButton implements Button {
    Render() {
        fmt.Println("渲染macOS风格按钮")
    }

    OnClick() {
        fmt.Println("macOS按钮点击事件")
    }
}

// 具体产品 B1
class WindowsCheckbox implements Checkbox {
    Render() {
        fmt.Println("渲染Windows风格复选框")
    }

    Toggle() {
        fmt.Println("Windows复选框切换状态")
    }
}

// 具体产品 B2
class MacCheckbox implements Checkbox {
    Render() {
        fmt.Println("渲染macOS风格复选框")
    }

    Toggle() {
        fmt.Println("macOS复选框切换状态")
    }
}

// 抽象工厂接口
type GUIFactory interface {
    CreateButton() Button
    CreateCheckbox() Checkbox
}

// 具体工厂 1
class WindowsFactory implements GUIFactory {
    CreateButton() Button {
        return new WindowsButton()
    }

    CreateCheckbox() Checkbox {
        return new WindowsCheckbox()
    }
}

// 具体工厂 2
class MacFactory implements GUIFactory {
    CreateButton() Button {
        return new MacButton()
    }

    CreateCheckbox() Checkbox {
        return new MacCheckbox()
    }
}

// 客户端代码
func CreateUI(factory GUIFactory) {
    button := factory.CreateButton()
    checkbox := factory.CreateCheckbox()

    button.Render()
    button.OnClick()

    checkbox.Render()
    checkbox.Toggle()
}

func main() {
    // 创建Windows UI
    windowsFactory := new WindowsFactory()
    CreateUI(windowsFactory)

    // 创建macOS UI
    macFactory := new MacFactory()
    CreateUI(macFactory)
}
```

#### Python 实现

```python
from abc import ABC, abstractmethod

# 产品族接口 A
class Button(ABC):
    @abstractmethod
    def render(self):
        pass

    @abstractmethod
    def on_click(self):
        pass

# 产品族接口 B
class Checkbox(ABC):
    @abstractmethod
    def render(self):
        pass

    @abstractmethod
    def toggle(self):
        pass

# 具体产品 A1
class WindowsButton(Button):
    def render(self):
        print("渲染Windows风格按钮")

    def on_click(self):
        print("Windows按钮点击事件")

# 具体产品 A2
class MacButton(Button):
    def render(self):
        print("渲染macOS风格按钮")

    def on_click(self):
        print("macOS按钮点击事件")

# 具体产品 B1
class WindowsCheckbox(Checkbox):
    def render(self):
        print("渲染Windows风格复选框")

    def toggle(self):
        print("Windows复选框切换状态")

# 具体产品 B2
class MacCheckbox(Checkbox):
    def render(self):
        print("渲染macOS风格复选框")

    def toggle(self):
        print("macOS复选框切换状态")

# 抽象工厂接口
class GUIFactory(ABC):
    @abstractmethod
    def create_button(self):
        pass

    @abstractmethod
    def create_checkbox(self):
        pass

# 具体工厂 1
class WindowsFactory(GUIFactory):
    def create_button(self):
        return WindowsButton()

    def create_checkbox(self):
        return WindowsCheckbox()

# 具体工厂 2
class MacFactory(GUIFactory):
    def create_button(self):
        return MacButton()

    def create_checkbox(self):
        return MacCheckbox()

# 客户端代码
def create_ui(factory: GUIFactory):
    button = factory.create_button()
    checkbox = factory.create_checkbox()

    button.render()
    button.on_click()
    checkbox.render()
    checkbox.toggle()

# 使用示例
create_ui(WindowsFactory())
create_ui(MacFactory())
```

#### TypeScript 实现

```typescript
// 产品族接口 A
interface Button {
  render(): void;
  onClick(): void;
}

// 产品族接口 B
interface Checkbox {
  render(): void;
  toggle(): void;
}

// 具体产品 A1
class WindowsButton implements Button {
  render(): void {
    console.log("渲染Windows风格按钮");
  }

  onClick(): void {
    console.log("Windows按钮点击事件");
  }
}

// 具体产品 A2
class MacButton implements Button {
  render(): void {
    console.log("渲染macOS风格按钮");
  }

  onClick(): void {
    console.log("macOS按钮点击事件");
  }
}

// 具体产品 B1
class WindowsCheckbox implements Checkbox {
  render(): void {
    console.log("渲染Windows风格复选框");
  }

  toggle(): void {
    console.log("Windows复选框切换状态");
  }
}

// 具体产品 B2
class MacCheckbox implements Checkbox {
  render(): void {
    console.log("渲染macOS风格复选框");
  }

  toggle(): void {
    console.log("macOS复选框切换状态");
  }
}

// 抽象工厂接口
abstract class GUIFactory {
  abstract createButton(): Button;
  abstract createCheckbox(): Checkbox;
}

// 具体工厂 1
class WindowsFactory extends GUIFactory {
  createButton(): Button {
    return new WindowsButton();
  }

  createCheckbox(): Checkbox {
    return new WindowsCheckbox();
  }
}

// 具体工厂 2
class MacFactory extends GUIFactory {
  createButton(): Button {
    return new MacButton();
  }

  createCheckbox(): Checkbox {
    return new MacCheckbox();
  }
}

// 客户端代码
function createUI(factory: GUIFactory) {
  const button = factory.createButton();
  const checkbox = factory.createCheckbox();

  button.render();
  button.onClick();
  checkbox.render();
  checkbox.toggle();
}

// 使用示例
createUI(new WindowsFactory());
createUI(new MacFactory());
```

## 建造者模式 (Builder)

将一个复杂对象的构建与其表示分离，使得同样的构建过程可以创建不同的表示

### 适用场景

- 构建复杂对象（如House、Person等）
- SQL查询构建
- HTTP请求构建
- 配置对象创建

### 实现示例

#### Golang 实现

```go
package builder

import "fmt"

// 产品
type House struct {
    foundation  string
    structure   string
    roof        string
    interior    string
}

func (h *House) String() string {
    return fmt.Sprintf("房子结构: 地基=%s, 结构=%s, 屋顶=%s, 内部=%s",
        h.foundation, h.structure, h.roof, h.interior)
}

// 建造者接口
type HouseBuilder interface {
    BuildFoundation()
    BuildStructure()
    BuildRoof()
    BuildInterior()
    GetHouse() *House
}

// 具体建造者
type ConcreteHouseBuilder struct {
    house *House
}

func (b *ConcreteHouseBuilder) BuildFoundation() {
    b.house.foundation = "混凝土地基"
}

func (b *ConcreteHouseBuilder) BuildStructure() {
    b.house.structure = "钢结构"
}

func (b *ConcreteHouseBuilder) BuildRoof() {
    b.house.roof = "混凝土屋顶"
}

func (b *ConcreteHouseBuilder) BuildInterior() {
    b.house.interior = "豪华装修"
}

func (b *ConcreteHouseBuilder) GetHouse() *House {
    return b.house
}

// 指挥者
type Director struct {
    builder HouseBuilder
}

func (d *Director) SetBuilder(builder HouseBuilder) {
    d.builder = builder
}

func (d *Director) ConstructHouse() {
    d.builder.BuildFoundation()
    d.builder.BuildStructure()
    d.builder.BuildRoof()
    d.builder.BuildInterior()
}

func main() {
    builder := &ConcreteHouseBuilder{house: &House{}}
    director := &Director{builder: builder}

    // 指挥者控制建造过程
    director.ConstructHouse()

    // 获取最终产品
    house := builder.GetHouse()
    fmt.Println(house)
}
```

#### Python 实现

```python
from abc import ABC, abstractmethod

# 产品
class Pizza:
    def __init__(self):
        self.dough = None
        self.sauce = None
        self.topping = None

    def __str__(self):
        return f"披萨: 面饼={self.dough}, 酱料={self.sauce}, 配料={self.topping}"

# 建造者接口
class PizzaBuilder(ABC):
    @abstractmethod
    def set_dough(self):
        pass

    @abstractmethod
    def set_sauce(self):
        pass

    @abstractmethod
    def set_topping(self):
        pass

    @abstractmethod
    def get_pizza(self):
        pass

# 具体建造者 1
class HawaiianPizzaBuilder(PizzaBuilder):
    def __init__(self):
        self.pizza = Pizza()

    def set_dough(self):
        self.pizza.dough = "薄脆面团"
        return self

    def set_sauce(self):
        self.pizza.sauce = "番茄酱"
        return self

    def set_topping(self):
        self.pizza.topping = "火腿、菠萝"
        return self

    def get_pizza(self):
        return self.pizza

# 具体建造者 2
class SpicyPizzaBuilder(PizzaBuilder):
    def __init__(self):
        self.pizza = Pizza()

    def set_dough(self):
        self.pizza.dough = "厚实面团"
        return self

    def set_sauce(self):
        self.pizza.sauce = "辣番茄酱"
        return self

    def set_topping(self):
        self.pizza.topping = "辣椒、香肠"
        return self

    def get_pizza(self):
        return self.pizza

# 指挥者
class PizzaDirector:
    def __init__(self, builder: PizzaBuilder):
        self.builder = builder

    def make_pizza(self):
        return (self.builder
                .set_dough()
                .set_sauce()
                .set_topping()
                .get_pizza())

# 使用示例
hawaiian_builder = HawaiianPizzaBuilder()
director = PizzaDirector(hawaiian_builder)
hawaiian_pizza = director.make_pizza()
print(hawaiian_pizza)

spicy_builder = SpicyPizzaBuilder()
director = PizzaDirector(spicy_builder)
spicy_pizza = director.make_pizza()
print(spicy_pizza)
```

#### TypeScript 实现

```typescript
// 产品
class Computer {
  cpu!: string;
  ram!: string;
  storage!: string;
  gpu?: string;

  toString(): string {
    return `电脑配置: CPU=${this.cpu}, RAM=${this.ram}, 存储=${this.storage}${this.gpu ? `, GPU=${this.gpu}` : ""}`;
  }
}

// 建造者接口
interface ComputerBuilder {
  setCPU(cpu: string): this;
  setRAM(ram: string): this;
  setStorage(storage: string): this;
  setGPU?(gpu: string): this;
  build(): Computer;
}

// 具体建造者
class GamingComputerBuilder implements ComputerBuilder {
  private computer: Computer = new Computer();

  setCPU(cpu: string): this {
    this.computer.cpu = cpu;
    return this;
  }

  setRAM(ram: string): this {
    this.computer.ram = ram;
    return this;
  }

  setStorage(storage: string): this {
    this.computer.storage = storage;
    return this;
  }

  setGPU(gpu: string): this {
    this.computer.gpu = gpu;
    return this;
  }

  build(): Computer {
    return this.computer;
  }
}

// 指挥者
class ComputerDirector {
  construct(builder: ComputerBuilder): Computer {
    return builder
      .setCPU("Intel i9")
      .setRAM("32GB DDR4")
      .setStorage("1TB SSD")
      .setGPU("NVIDIA RTX 3080")
      .build();
  }
}

// 使用示例
const builder = new GamingComputerBuilder();
const director = new ComputerDirector();
const gamingPC = director.construct(builder);
console.log(gamingPC.toString());
```

---

## 原型模式 (Prototype)

用原型实例指定创建对象的种类，并通过复制这些原型创建新对象。

### 适用场景

- 需要动态创建对象，而不知道具体的类
- 避免工厂类层次结构
- 合并和组合对象
- 深度复制复杂对象

### 实现示例

#### Golang 实现

```go
package prototype

import "fmt"

// 原型接口
type Shape interface {
    Clone() Shape
    Draw()
}

// 具体原型 A
class Circle struct {
    radius int
    color  string
}

func (c *Circle) Clone() Shape {
    cloned := &Circle{
        radius: c.radius,
        color:  c.color,
    }
    return cloned
}

func (c *Circle) Draw() {
    fmt.Printf("绘制圆形，半径: %d, 颜色: %s\n", c.radius, c.color)
}

// 具体原型 B
class Rectangle struct {
    width  int
    height int
    color  string
}

func (r *Rectangle) Clone() Shape {
    cloned := &Rectangle{
        width:  r.width,
        height: r.height,
        color:  r.color,
    }
    return cloned
}

func (r *Rectangle) Draw() {
    fmt.Printf("绘制矩形，宽度: %d, 高度: %d, 颜色: %s\n", r.width, r.height, r.color)
}

func main() {
    // 创建原型实例
    circle := &Circle{radius: 10, color: "红色"}
    rectangle := &Rectangle{width: 20, height: 30, color: "蓝色"}

    // 使用原型创建新对象
    clone1 := circle.Clone()
    clone1.(*Circle).radius = 15
    clone1.(*Circle).color = "绿色"

    clone2 := rectangle.Clone()
    clone2.(*Rectangle).width = 25

    // 显示结果
    circle.Draw()
    clone1.Draw()
    rectangle.Draw()
    clone2.Draw()
}
```

#### Python 实现

```python
import copy

class Shape:
    def __init__(self, color):
        self.color = color

    def clone(self):
        return copy.deepcopy(self)

    def draw(self):
        raise NotImplementedError()

class Circle(Shape):
    def __init__(self, radius, color):
        super().__init__(color)
        self.radius = radius

    def clone(self):
        return Circle(self.radius, self.color)

    def draw(self):
        print(f"绘制圆形，半径: {self.radius}, 颜色: {self.color}")

class Rectangle(Shape):
    def __init__(self, width, height, color):
        super().__init__(color)
        self.width = width
        self.height = height

    def clone(self):
        return Rectangle(self.width, self.height, self.color)

    def draw(self):
        print(f"绘制矩形，宽度: {self.width}, 高度: {self.height}, 颜色: {self.color}")

# 使用示例
original_circle = Circle(10, "红色")
copied_circle = original_circle.clone()
copied_circle.radius = 15
copied_circle.color = "绿色"

original_rectangle = Rectangle(20, 30, "蓝色")
copied_rectangle = original_rectangle.clone()
copied_rectangle.width = 25

original_circle.draw()
copied_circle.draw()
original_rectangle.draw()
copied_rectangle.draw()
```

#### TypeScript 实现

```typescript
// 原型接口
interface Prototype {
  clone(): Prototype;
  draw(): void;
}

// 具体原型 A
class Circle implements Prototype {
  constructor(
    public radius: number,
    public color: string,
  ) {}

  clone(): Prototype {
    return new Circle(this.radius, this.color);
  }

  draw(): void {
    console.log(`绘制圆形，半径: ${this.radius}, 颜色: ${this.color}`);
  }
}

// 具体原型 B
class Rectangle implements Prototype {
  constructor(
    public width: number,
    public height: number,
    public color: string,
  ) {}

  clone(): Prototype {
    return new Rectangle(this.width, this.height, this.color);
  }

  draw(): void {
    console.log(
      `绘制矩形，宽度: ${this.width}, 高度: ${this.height}, 颜色: ${this.color}`,
    );
  }
}

// 客户端代码
function createShapes() {
  // 创建原始对象
  const originalCircle = new Circle(10, "红色");
  const originalRectangle = new Rectangle(20, 30, "蓝色");

  // 克隆对象
  const clonedCircle = originalCircle.clone();
  clonedCircle.radius = 15;
  clonedCircle.color = "绿色";

  const clonedRectangle = originalRectangle.clone();
  clonedRectangle.width = 25;

  // 显示结果
  originalCircle.draw();
  clonedCircle.draw();
  originalRectangle.draw();
  clonedRectangle.draw();
}

// 使用示例
createShapes();
```

# 结构型设计模式

关注类和对象的组合，通过组合它们形成更大的结构。这些模式简化了设计，同时提高了灵活性

| 模式   | 核心思想                          | 适用场景                 | 主要优点                     |
| ------ | --------------------------------- | ------------------------ | ---------------------------- |
| 适配器 | 将不兼容接口转换为兼容接口        | 集成第三方库，统一接口   | 无需修改现有代码             |
| 桥接   | 分离抽象与实现，使它们独立变化    | 多维度变化的类           | 避免继承带来的类爆炸         |
| 组合   | 将对象组合成树形结构表示部分-整体 | 树形结构数据处理         | 统一处理单个和组合对象       |
| 装饰器 | 动态地给对象添加额外职责          | 运行时扩展对象功能       | 比继承更灵活                 |
| 外观   | 为复杂子系统提供简化接口          | 简化复杂系统接口         | 降低子系统与客户端的耦合     |
| 享元   | 复用相似对象以减少内存开销        | 创建大量相似对象         | 显著减少对象数量             |
| 代理   | 为对象提供控制访问的方式          | 控制对象访问、延迟初始化 | 在访问真实对象时增加额外控制 |

结构型模式关注的是类和对象的组合方式，通过灵活的设计实现更高层次的结构和功能。它们不仅提高了代码的复用性，也增强了系统的可维护性

## 适配器模式 (Adapter Pattern)

将一个类的接口转换成客户端希望的另外一个接口，使得原本由于接口不兼容而不能一起工作的那些类可以一起工作。

### 适用场景

- 需要集成第三方库，但其接口与系统现有接口不兼容
- 需要将旧系统中的类适配到新的接口
- 需要统一不同数据格式（如JSON、XML、CSV等）
- 需要为不同的硬件设备驱动提供统一接口

### 实现示例

#### Golang 实现

```go
package adapter

import "fmt"

// 目标接口
type Target interface {
    Request() string
}

// 被适配者 - 已存在的接口不兼容的类
type Adaptee struct{}

func (a *Adaptee) SpecificRequest() string {
    return "Adaptee's specific request"
}

// 适配器
type Adapter struct {
    adaptee *Adaptee
}

func (a *Adapter) Request() string {
    return "Adapter: " + a.adaptee.SpecificRequest()
}

// 客户端代码
func ClientCode(t Target) {
    fmt.Println(t.Request())
}

func main() {
    adaptee := &Adaptee{}
    adapter := &Adapter{adaptee: adaptee}

    // 适配器实现目标接口
    ClientCode(adapter)

    // 也可以适配现有实现
    var target Target = adapter
    fmt.Println(target.Request())
}
```

#### Python 实现

```python
from abc import ABC, abstractmethod

# 目标接口
class Target(ABC):
    @abstractmethod
    def request(self):
        pass

# 被适配者 - 已存在的接口不兼容的类
class Adaptee:
    def specific_request(self):
        return "Adaptee's specific request"

# 适配器
class Adapter(Target):
    def __init__(self, adaptee):
        self.adaptee = adaptee

    def request(self):
        return f"Adapter: {self.adaptee.specific_request()}"

# 客户端代码
def client_code(target: Target):
    print(target.request())

# 使用示例
adaptee = Adaptee()
adapter = Adapter(adaptee)

# 适配器实现目标接口
client_code(adapter)
```

#### TypeScript 实现

```typescript
// 目标接口
interface Target {
  request(): string;
}

// 被适配者 - 已存在的接口不兼容的类
class Adaptee {
  public specificRequest(): string {
    return "Adaptee's specific request";
  }
}

// 适配器
class Adapter implements Target {
  constructor(private adaptee: Adaptee) {}

  public request(): string {
    return `Adapter: ${this.adaptee.specificRequest()}`;
  }
}

// 客户端代码
function clientCode(target: Target) {
  console.log(target.request());
}

// 使用示例
const adaptee = new Adaptee();
const adapter = new Adapter(adaptee);

// 适配器实现目标接口
clientCode(adapter);
```

## 桥接模式 (Bridge Pattern)

将抽象部分与它的实现部分分离，使它们都可以独立地变化。

### 适用场景

- 当一个类存在多个维度的变化时
- 当需要避免多层继承导致的类爆炸时
- 当需要对抽象和实现进行扩展时
- 当需要在运行时切换实现时

### 实现示例

#### Golang 实现

```go
package bridge

import "fmt"

// 实现接口 - 定义实现类的接口
type DrawingAPI interface {
    DrawCircle(radius, x, y float64)
    DrawRectangle(width, height, x, y float64)
}

// 具体实现 1
class SVGDrawingAPI implements DrawingAPI {
    DrawCircle(radius, x, y float64) {
        fmt.Printf("<circle cx='%f' cy='%f' r='%f' fill='blue' />\n", x, y, radius)
    }

    DrawRectangle(width, height, x, y float64) {
        fmt.Printf("<rect x='%f' y='%f' width='%f' height='%f' fill='red' />\n", x, y, width, height)
    }
}

// 具体实现 2
class CanvasDrawingAPI implements DrawingAPI {
    DrawCircle(radius, x, y float64) {
        fmt.Printf("画布: 在 (%.1f, %.1f) 绘制半径为 %.1f 的圆形\n", x, y, radius)
    }

    DrawRectangle(width, height, x, y float64) {
        fmt.Printf("画布: 在 (%.1f, %.1f) 绘制 %.1f×%.1f 的矩形\n", x, y, width, height)
    }
}

// 抽象类
type Shape interface {
    Draw()
    Resize(percent float64)
}

// 扩展抽象类 - 圆形
class Circle struct {
    drawingAPI    DrawingAPI
    radius, x, y  float64
}

func (c *Circle) Draw() {
    c.drawingAPI.DrawCircle(c.radius, c.x, c.y)
}

func (c *Circle) Resize(percent float64) {
    c.radius *= percent / 100
}

// 扩展抽象类 - 矩形
class Rectangle struct {
    drawingAPI     DrawingAPI
    width, height, x, y float64
}

func (r *Rectangle) Draw() {
    r.drawingAPI.DrawRectangle(r.width, r.height, r.x, r.y)
}

func (r *Rectangle) Resize(percent float64) {
    r.width *= percent / 100
    r.height *= percent / 100
}

func main() {
    // 使用SVG API绘制圆形和矩形
    svgAPI := new SVGDrawingAPI()
    circle := &Circle{drawingAPI: svgAPI, radius: 10, x: 100, y: 100}
    rectangle := &Rectangle{drawingAPI: svgAPI, width: 20, height: 30, x: 150, y: 150}

    circle.Draw()
    rectangle.Draw()

    // 使用Canvas API绘制相同的形状
    canvasAPI := new CanvasDrawingAPI()
    circle.drawingAPI = canvasAPI
    rectangle.drawingAPI = canvasAPI

    circle.Draw()
    rectangle.Draw()
}
```

#### Python 实现

```python
from abc import ABC, abstractmethod

# 实现接口 - 定义实现类的接口
class DrawingAPI(ABC):
    @abstractmethod
    def draw_circle(self, radius, x, y):
        pass

    @abstractmethod
    def draw_rectangle(self, width, height, x, y):
        pass

# 具体实现 1
class SVGDrawingAPI(DrawingAPI):
    def draw_circle(self, radius, x, y):
        print(f"<circle cx='{x}' cy='{y}' r='{radius}' fill='blue' />")

    def draw_rectangle(self, width, height, x, y):
        print(f"<rect x='{x}' y='{y}' width='{width}' height='{height}' fill='red' />")

# 具体实现 2
class CanvasDrawingAPI(DrawingAPI):
    def draw_circle(self, radius, x, y):
        print(f"画布: 在 ({x}, {y}) 绘制半径为 {radius} 的圆形")

    def draw_rectangle(self, width, height, x, y):
        print(f"画布: 在 ({x}, {y}) 绘制 {width}×{height} 的矩形")

# 抽象类
class Shape(ABC):
    def __init__(self, drawing_api):
        self.drawing_api = drawing_api

    @abstractmethod
    def draw(self):
        pass

    @abstractmethod
    def resize(self, percent):
        pass

# 扩展抽象类 - 圆形
class Circle(Shape):
    def __init__(self, drawing_api, radius, x, y):
        super().__init__(drawing_api)
        self.radius = radius
        self.x = x
        self.y = y

    def draw(self):
        self.drawing_api.draw_circle(self.radius, self.x, self.y)

    def resize(self, percent):
        self.radius = self.radius * percent / 100

# 扩展抽象类 - 矩形
class Rectangle(Shape):
    def __init__(self, drawing_api, width, height, x, y):
        super().__init__(drawing_api)
        self.width = width
        self.height = height
        self.x = x
        self.y = y

    def draw(self):
        self.drawing_api.draw_rectangle(self.width, self.height, self.x, self.y)

    def resize(self, percent):
        self.width = self.width * percent / 100
        self.height = self.height * percent / 100

# 使用示例
# 使用SVG API绘制圆形和矩形
svg_api = SVGDrawingAPI()
circle = Circle(svg_api, radius=10, x=100, y=100)
rectangle = Rectangle(svg_api, width=20, height=30, x=150, y=150)

circle.draw()
rectangle.draw()

# 使用Canvas API绘制相同的形状
canvas_api = CanvasDrawingAPI()
circle.drawing_api = canvas_api
rectangle.drawing_api = canvas_api

circle.draw()
rectangle.draw()
```

#### TypeScript 实现

```typescript
// 实现接口 - 定义实现类的接口
interface DrawingAPI {
  drawCircle(radius: number, x: number, y: number): void;
  drawRectangle(width: number, height: number, x: number, y: number): void;
}

// 具体实现 1
class SVGDrawingAPI implements DrawingAPI {
  public drawCircle(radius: number, x: number, y: number): void {
    console.log(`<circle cx='${x}' cy='${y}' r='${radius}' fill='blue' />`);
  }

  public drawRectangle(
    width: number,
    height: number,
    x: number,
    y: number,
  ): void {
    console.log(
      `<rect x='${x}' y='${y}' width='${width}' height='${height}' fill='red' />`,
    );
  }
}

// 具体实现 2
class CanvasDrawingAPI implements DrawingAPI {
  public drawCircle(radius: number, x: number, y: number): void {
    console.log(`画布: 在 (${x}, ${y}) 绘制半径为 ${radius} 的圆形`);
  }

  public drawRectangle(
    width: number,
    height: number,
    x: number,
    y: number,
  ): void {
    console.log(`画布: 在 (${x}, ${y}) 绘制 ${width}×${height} 的矩形`);
  }
}

// 抽象类
abstract class Shape {
  constructor(protected drawingAPI: DrawingAPI) {}

  abstract draw(): void;
  abstract resize(percent: number): void;
}

// 扩展抽象类 - 圆形
class Circle extends Shape {
  constructor(
    drawingAPI: DrawingAPI,
    private radius: number,
    private x: number,
    private y: number,
  ) {
    super(drawingAPI);
  }

  public draw(): void {
    this.drawingAPI.drawCircle(this.radius, this.x, this.y);
  }

  public resize(percent: number): void {
    this.radius = (this.radius * percent) / 100;
  }
}

// 扩展抽象类 - 矩形
class Rectangle extends Shape {
  constructor(
    drawingAPI: DrawingAPI,
    private width: number,
    private height: number,
    private x: number,
    private y: number,
  ) {
    super(drawingAPI);
  }

  public draw(): void {
    this.drawingAPI.drawRectangle(this.width, this.height, this.x, this.y);
  }

  public resize(percent: number): void {
    this.width = (this.width * percent) / 100;
    this.height = (this.height * percent) / 100;
  }
}

// 使用示例
// 使用SVG API绘制圆形和矩形
const svgAPI: DrawingAPI = new SVGDrawingAPI();
const circle = new Circle(svgAPI, 10, 100, 100);
const rectangle = new Rectangle(svgAPI, 20, 30, 150, 150);

circle.draw();
rectangle.draw();

// 使用Canvas API绘制相同的形状
const canvasAPI: DrawingAPI = new CanvasDrawingAPI();
circle.drawingAPI = canvasAPI;
rectangle.drawingAPI = canvasAPI;

circle.draw();
rectangle.draw();
```

## 组合模式 (Composite Pattern)

将对象组合成树形结构以表示"部分-整体"的层次结构，组合模式使得用户对单个对象和组合对象的使用具有一致性。

### 适用场景

- 处理树形结构数据（如文件系统、公司组织架构、UI控件树等）
- 忽略组合对象与单个对象之间的差异，统一处理
- 客户端需要以统一方式处理树中的所有对象
- 需要表示"部分-整体"层次结构

### 实现示例

#### Golang 实现

```go
package composite

import (
    "fmt"
    "strings"
)

// 组件接口
type Component interface {
    Render(indent string)
}

// 叶子节点 - 文件
class File struct {
    name string
    size int
}

func (f *File) Render(indent string) {
    fmt.Printf("%s- %s (%d bytes)\n", indent, f.name, f.size)
}

// 组合节点 - 文件夹
class Directory struct {
    name     string
    children []Component
}

func (d *Directory) Add(child Component) {
    d.children = append(d.children, child)
}

func (d *Directory) Render(indent string) {
    fmt.Printf("%s+ %s\n", indent, d.name)
    for _, child := range d.children {
        child.Render(indent + "  ")
    }
}

func main() {
    // 创建文件系统
    root := &Directory{name: "root"}
    docs := &Directory{name: "documents"}
    images := &Directory{name: "images"}

    root.Add(docs)
    root.Add(images)

    docs.Add(&File{name: "report.docx", size: 1024})
    docs.Add(&File{name: "presentation.pptx", size: 2048})

    images.Add(&File{name: "photo1.jpg", size: 3072})
    images.Add(&File{name: "photo2.jpg", size: 4096})

    // 渲染文件系统
    root.Render("")
}
```

#### Python 实现

```python
from abc import ABC, abstractmethod

# 组件接口
class Component(ABC):
    @abstractmethod
    def render(self, indent: str):
        pass

# 叶子节点 - 文件
class File(Component):
    def __init__(self, name: str, size: int):
        self.name = name
        self.size = size

    def render(self, indent: str):
        print(f"{indent}- {self.name} ({self.size} bytes)")

# 组合节点 - 文件夹
class Directory(Component):
    def __init__(self, name: str):
        self.name = name
        self.children = []

    def add(self, child: Component):
        self.children.append(child)

    def render(self, indent: str):
        print(f"{indent}+ {self.name}")
        for child in self.children:
            child.render(indent + "  ")

# 使用示例
# 创建文件系统
root = Directory("root")
docs = Directory("documents")
images = Directory("images")

root.add(docs)
root.add(images)

docs.add(File("report.docx", 1024))
docs.add(File("presentation.pptx", 2048))

images.add(File("photo1.jpg", 3072))
images.add(File("photo2.jpg", 4096))

# 渲染文件系统
root.render("")
```

#### TypeScript 实现

```typescript
// 组件接口
interface Component {
  render(indent: string): void;
}

// 叶子节点 - 文件
class File implements Component {
  constructor(
    private name: string,
    private size: number,
  ) {}

  public render(indent: string): void {
    console.log(`${indent}- ${this.name} (${this.size} bytes)`);
  }
}

// 组合节点 - 文件夹
class Directory implements Component {
  constructor(
    private name: string,
    private children: Component[] = [],
  ) {}

  public add(child: Component): void {
    this.children.push(child);
  }

  public render(indent: string): void {
    console.log(`${indent}+ ${this.name}`);
    this.children.forEach((child) => child.render(indent + "  "));
  }
}

// 使用示例
// 创建文件系统
const root = new Directory("root");
const docs = new Directory("documents");
const images = new Directory("images");

root.add(docs);
root.add(images);

docs.add(new File("report.docx", 1024));
docs.add(new File("presentation.pptx", 2048));

images.add(new File("photo1.jpg", 3072));
images.add(new File("photo2.jpg", 4096));

// 渲染文件系统
root.render("");
```

## 装饰器模式 (Decorator Pattern)

动态地给对象添加一些额外的职责。就增加功能来说，装饰器模式相比生成子类更为灵活。

### 适用场景

- 需要在运行时动态为对象添加功能
- 需要扩展一个类的功能，但又不能使用继承
- 需要撤销或重做添加的功能
- 需要透明地添加功能而不影响其他对象

### 实现示例

#### Golang 实现

```go
package decorator

import "fmt"

// 组件接口
type Component interface {
    Operation() string
}

// 具体组件
class ConcreteComponent struct{}

func (c *ConcreteComponent) Operation() string {
    return "ConcreteComponent operation"
}

// 装饰器抽象
type Decorator struct {
    component Component
}

func (d *Decorator) Operation() string {
    return d.component.Operation()
}

// 具体装饰器 A
class ConcreteDecoratorA struct {
    Decorator
}

func NewConcreteDecoratorA(component Component) *ConcreteDecoratorA {
    return &ConcreteDecoratorA{Decorator: Decorator{component: component}}
}

func (d *ConcreteDecoratorA) Operation() string {
    return "ConcreteDecoratorA(" + d.Decorator.Operation() + ")"
}

// 具体装饰器 B
class ConcreteDecoratorB struct {
    Decorator
    addedState string
}

func NewConcreteDecoratorB(component Component) *ConcreteDecoratorB {
    return &ConcreteDecoratorB{
        Decorator:   Decorator{component: component},
        addedState: "added state",
    }
}

func (d *ConcreteDecoratorB) Operation() string {
    return fmt.Sprintf("ConcreteDecoratorB[%s](%s)", d.addedState, d.Decorator.Operation())
}

func main() {
    // 客户端代码演示
    simple := &ConcreteComponent{}
    fmt.Println("Client: Simple component:")
    fmt.Println(simple.Operation())

    // 装饰器
    decorator1 := NewConcreteDecoratorA(simple)
    decorator2 := NewConcreteDecoratorB(decorator1)

    fmt.Println("\nClient: Decorated component:")
    fmt.Println(decorator2.Operation())
}
```

#### Python 实现

```python
from abc import ABC, abstractmethod

# 组件接口
class Component(ABC):
    @abstractmethod
    def operation(self):
        pass

# 具体组件
class ConcreteComponent(Component):
    def operation(self):
        return "ConcreteComponent operation"

# 装饰器抽象
class Decorator(Component):
    def __init__(self, component: Component):
        self._component = component

    def operation(self):
        return self._component.operation()

# 具体装饰器 A
class ConcreteDecoratorA(Decorator):
    def operation(self):
        return f"ConcreteDecoratorA({super().operation()})"

# 具体装饰器 B
class ConcreteDecoratorB(Decorator):
    def __init__(self, component: Component):
        super().__init__(component)
        self.added_state = "added state"

    def operation(self):
        return f"ConcreteDecoratorB[{self.added_state}]({super().operation()})"

# 使用示例
if __name__ == "__main__":
    # 客户端代码演示
    simple = ConcreteComponent()
    print("Client: Simple component:")
    print(simple.operation())

    # 装饰器
    decorator1 = ConcreteDecoratorA(simple)
    decorator2 = ConcreteDecoratorB(decorator1)

    print("\nClient: Decorated component:")
    print(decorator2.operation())
```

#### TypeScript 实现

```typescript
// 组件接口
interface Component {
  operation(): string;
}

// 具体组件
class ConcreteComponent implements Component {
  public operation(): string {
    return "ConcreteComponent operation";
  }
}

// 装饰器抽象
abstract class Decorator implements Component {
  constructor(protected component: Component) {}

  public operation(): string {
    return this.component.operation();
  }
}

// 具体装饰器 A
class ConcreteDecoratorA extends Decorator {
  public operation(): string {
    return `ConcreteDecoratorA(${super.operation()})`;
  }
}

// 具体装饰器 B
class ConcreteDecoratorB extends Decorator {
  private addedState: string = "added state";

  public operation(): string {
    return `ConcreteDecoratorB[${this.addedState}](${super.operation()})`;
  }
}

// 使用示例
function clientCode(component: Component) {
  console.log(`Client: ${component.operation()}`);
}

// 客户端代码演示
const simple = new ConcreteComponent();
console.log("Client: Simple component:");
clientCode(simple);

// 装饰器
const decorator1 = new ConcreteDecoratorA(simple);
const decorator2 = new ConcreteDecoratorB(decorator1);

console.log("\nClient: Decorated component:");
clientCode(decorator2);
```

## 外观模式 (Facade Pattern)

为一个复杂的子系统提供一个简化的统一接口。

### 适用场景

- 当需要简化一个复杂系统的接口时
- 当客户端与多个子系统之间存在复杂依赖关系时
- 当需要提供一个简单接口用于分阶段开发整个系统时
- 当需要重构一个遗留系统，对外提供一个简洁的接口

### 实现示例

#### Golang 实现

```go
package facade

import (
    "fmt"
    "time"
)

// 子系统 A
class CPU struct{}

func (c *CPU) Freeze() {
    fmt.Println("CPU freeze")
}

func (c *CPU) Jump(position int) {
    fmt.Printf("CPU jump to position %d\n", position)
}

func (c *CPU) Execute() {
    fmt.Println("CPU execute")
}

// 子系统 B
class Memory struct{}

func (m *Memory) Load(position int, data string) {
    fmt.Printf("Memory load data at position %d: %s\n", position, data)
}

// 子系统 C
class HardDrive struct{}

func (h *HardDrive) Read(lba int, size int) string {
    fmt.Printf("HardDrive read %d bytes from %d\n", size, lba)
    return "data"
}

// 外观
class ComputerFacade struct {
    cpu    *CPU
    memory *Memory
    drive  *HardDrive
}

func NewComputerFacade() *ComputerFacade {
    return &ComputerFacade{
        cpu:    &CPU{},
        memory: &Memory{},
        drive:  &HardDrive{},
    }
}

func (c *ComputerFacade) Start() {
    c.cpu.Freeze()
    c.memory.Load(0, c.drive.Read(0, 1024))
    c.cpu.Jump(0)
    c.cpu.Execute()
    time.Sleep(1 * time.Second)
    fmt.Println("Computer started")
}

func main() {
    computer := NewComputerFacade()
    computer.Start() // 通过外观简化了复杂的启动过程
}
```

#### Python 实现

```python
import time

# 子系统 A
class CPU:
    def freeze(self):
        print("CPU freeze")

    def jump(self, position):
        print(f"CPU jump to position {position}")

    def execute(self):
        print("CPU execute")

# 子系统 B
class Memory:
    def load(self, position, data):
        print(f"Memory load data at position {position}: {data}")

# 子系统 C
class HardDrive:
    def read(self, lba, size):
        print(f"HardDrive read {size} bytes from {lba}")
        return "data"

# 外观
class ComputerFacade:
    def __init__(self):
        self.cpu = CPU()
        self.memory = Memory()
        self.drive = HardDrive()

    def start(self):
        self.cpu.freeze()
        self.memory.load(0, self.drive.read(0, 1024))
        self.cpu.jump(0)
        self.cpu.execute()
        time.sleep(1)
        print("Computer started")

# 使用示例
computer = ComputerFacade()
computer.start()  # 通过外观简化了复杂的启动过程
```

#### TypeScript 实现

```typescript
// 子系统 A
class CPU {
  public freeze(): void {
    console.log("CPU freeze");
  }

  public jump(position: number): void {
    console.log(`CPU jump to position ${position}`);
  }

  public execute(): void {
    console.log("CPU execute");
  }
}

// 子系统 B
class Memory {
  public load(position: number, data: string): void {
    console.log(`Memory load data at position ${position}: ${data}`);
  }
}

// 子系统 C
class HardDrive {
  public read(lba: number, size: number): string {
    console.log(`HardDrive read ${size} bytes from ${lba}`);
    return "data";
  }
}

// 外观
class ComputerFacade {
  private cpu: CPU = new CPU();
  private memory: Memory = new Memory();
  private drive: HardDrive = new HardDrive();

  public start(): void {
    this.cpu.freeze();
    this.memory.load(0, this.drive.read(0, 1024));
    this.cpu.jump(0);
    this.cpu.execute();
    console.log("Computer started");
  }
}

// 使用示例
const computer = new ComputerFacade();
computer.start(); // 通过外观简化了复杂的启动过程
```

## 享元模式 (Flyweight Pattern)

运用共享技术有效地支持大量细粒度的对象，避免相似对象的开销。

### 适用场景

- 应用程序需要使用大量相似对象
- 大部分对象状态可以外部化，可以被共享
- 细粒度的对象由于共享而降低了内存占用
- 对象的状态可以分为内部状态和外部状态

### 实现示例

#### Golang 实现

```go
package flyweight

import (
    "fmt"
    "strings"
)

// 享元接口
type Flyweight interface {
    Operation(extrinsicState string)
}

// 具体享元
class ConcreteFlyweight struct {
    intrinsicState string
}

func (f *ConcreteFlyweight) Operation(extrinsicState string) {
    fmt.Printf("ConcreteFlyweight: intrinsicState=%s, extrinsicState=%s\n",
        f.intrinsicState, extrinsicState)
}

// 享元工厂
class FlyweightFactory struct {
    flyweights map[string]Flyweight
}

func NewFlyweightFactory() *FlyweightFactory {
    return &FlyweightFactory{
        flyweights: make(map[string]Flyweight),
    }
}

func (f *FlyweightFactory) GetFlyweight(key string) Flyweight {
    if fw, ok := f.flyweights[key]; ok {
        return fw
    }

    flyweight := &ConcreteFlyweight{intrinsicState: key}
    f.flyweights[key] = flyweight
    return flyweight
}

func (f *FlyweightFactory) CountFlyweights() int {
    return len(f.flyweights)
}

// 客户端代码
func main() {
    factory := NewFlyweightFactory()

    // 使用享元
    fw1 := factory.GetFlyweight("A")
    fw1.Operation("X")

    fw2 := factory.GetFlyweight("B")
    fw2.Operation("X")

    // 共享相同的享元
    fw1 := factory.GetFlyweight("A")
    fw1.Operation("Y")

    fmt.Println("Flyweight count:", factory.CountFlyweights())
}
```

#### Python 实现

```python
class Flyweight:
    @abstractmethod
    def operation(self, extrinsic_state):
        pass

class ConcreteFlyweight(Flyweight):
    def __init__(self, intrinsic_state):
        self.intrinsic_state = intrinsic_state

    def operation(self, extrinsic_state):
        print(f"ConcreteFlyweight: intrinsic_state={self.intrinsic_state}, extrinsic_state={extrinsic_state}")

class FlyweightFactory:
    def __init__(self):
        self._flyweights = {}

    def get_flyweight(self, key):
        if key not in self._flyweights:
            self._flyweights[key] = ConcreteFlyweight(key)
        return self._flyweights[key]

    def count_flyweights(self):
        return len(self._flyweights)

# 使用示例
factory = FlyweightFactory()

# 使用享元
fw1 = factory.get_flyweight("A")
fw1.operation("X")

fw2 = factory.get_flyweight("B")
fw2.operation("X")

# 共享相同的享元
fw3 = factory.get_flyweight("A")
fw3.operation("Y")

print(f"Flyweight count: {factory.count_flyweights()}")
```

#### TypeScript 实现

```typescript
interface Flyweight {
  operation(extrinsicState: string): void;
}

class ConcreteFlyweight implements Flyweight {
  constructor(private intrinsicState: string) {}

  public operation(extrinsicState: string): void {
    console.log(
      `ConcreteFlyweight: intrinsicState=${this.intrinsicState}, extrinsicState=${extrinsicState}`,
    );
  }
}

class FlyweightFactory {
  private flyweights: { [key: string]: Flyweight } = {};

  public getFlyweight(key: string): Flyweight {
    if (!this.flyweights[key]) {
      this.flyweights[key] = new ConcreteFlyweight(key);
    }
    return this.flyweights[key];
  }

  public countFlyweights(): number {
    return Object.keys(this.flyweights).length;
  }
}

// 使用示例
const factory = new FlyweightFactory();

// 使用享元
const fw1 = factory.getFlyweight("A");
fw1.operation("X");

const fw2 = factory.getFlyweight("B");
fw2.operation("X");

// 共享相同的享元
const fw3 = factory.getFlyweight("A");
fw3.operation("Y");

console.log(`Flyweight count: ${factory.countFlyweights()}`);
```

## 代理模式 (Proxy Pattern)

为其他对象提供一种代理以控制对这个对象的访问。

### 适用场景

- 远程代理：为一个位于不同的地址空间的对象提供一个代表
- 虚拟代理：根据需要创建开销很大的对象
- 保护代理：控制对原始对象的访问，用于对象应该有不同的访问权限
- 智能引用代理：取代简单的指针，在访问对象时执行一些附加操作

### 实现示例

#### Golang 实现

```go
package proxy

import "fmt"

// 主题接口
class Subject interface {
    Request()
}

class RealSubject struct{}

func (r *RealSubject) Request() {
    fmt.Println("RealSubject: 处理请求")
}

class Proxy struct {
    realSubject *RealSubject
}

func (p *Proxy) Request() {
    if p.CheckAccess() {
        p.realSubject = &RealSubject{}

        fmt.Println("Proxy: 在请求之前处理")
        p.realSubject.Request()  // 转发请求到真实对象
        p.LogAccess()
        fmt.Println("Proxy: 在请求之后处理")
    }
}

func (p *Proxy) CheckAccess() bool {
    fmt.Println("Proxy: 检查访问权限")
    return true
}

func (p *Proxy) LogAccess() {
    fmt.Println("Proxy: 记录访问请求")
}

func main() {
    proxy := &Proxy{}
    proxy.Request()  // 通过代理访问真实对象
}
```

#### Python 实现

```python
from abc import ABC, abstractmethod

# 主题接口
class Subject(ABC):
    @abstractmethod
    def request(self):
        pass

class RealSubject(Subject):
    def request(self):
        print("RealSubject: 处理请求")

class Proxy(Subject):
    def __init__(self):
        self._real_subject = None

    def request(self):
        if self._check_access():
            if not self._real_subject:
                self._real_subject = RealSubject()

            print("Proxy: 在请求之前处理")
            self._real_subject.request()
            self._log_access()
            print("Proxy: 在请求之后处理")

    def _check_access(self):
        print("Proxy: 检查访问权限")
        return True

    def _log_access(self):
        print("Proxy: 记录访问请求")

# 使用示例
proxy = Proxy()
proxy.request()  # 通过代理访问真实对象
```

#### TypeScript 实现

```typescript
// 主题接口
interface Subject {
  request(): void;
}

class RealSubject implements Subject {
  public request(): void {
    console.log("RealSubject: 处理请求");
  }
}

class Proxy implements Subject {
  private realSubject!: RealSubject;

  public request(): void {
    if (this.checkAccess()) {
      if (!this.realSubject) {
        this.realSubject = new RealSubject();
      }

      console.log("Proxy: 在请求之前处理");
      this.realSubject.request();
      this.logAccess();
      console.log("Proxy: 在请求之后处理");
    }
  }

  private checkAccess(): boolean {
    console.log("Proxy: 检查访问权限");
    return true;
  }

  private logAccess(): void {
    console.log("Proxy: 记录访问请求");
  }
}

// 使用示例
const proxy = new Proxy();
proxy.request(); // 通过代理访问真实对象
```

# 行为型设计模式

行为型设计模式关注对象之间的通信和职责分配，描述对象或类之间如何交互，以及划分责任和算法

| 模式     | 核心思想                   | 适用场景                                 | 主要优点                           |
| -------- | -------------------------- | ---------------------------------------- | ---------------------------------- |
| 责任链   | 将请求的发送者和接收者解耦 | 多个对象处理同一请求，由运行时决定处理者 | 松耦合，增加处理者无需修改其它代码 |
| 命令     | 将请求封装成对象           | 需要支持撤销/重做、操作队列、宏命令      | 分解请求发送者和接收者，支持撤销   |
| 解释器   | 定义语言的文法并解释句子   | 需要解释执行特定语言的表达式             | 简化句子解释，易于扩展语法         |
| 中介者   | 用中介对象封装对象交互     | 多个对象复杂交互，需要松耦合             | 减少子类数量，集中控制交互         |
| 备忘录   | 保存对象状态以便恢复       | 需要撤销操作或保存快照                   | 封装状态，保持对象封装性           |
| 观察者   | 定义对象间一对多依赖关系   | 对象状态变化时通知相关对象               | 松耦合，支持广播通信               |
| 状态     | 对象根据状态改变行为       | 对象行为依赖状态，避免大量条件判断       | 状态转换显式化，易于扩展           |
| 策略     | 封装算法族并使其可互换     | 多种算法，需要在运行时选择算法           | 替代条件语句，开闭原则             |
| 模板方法 | 定义算法骨架，子类实现细节 | 提取公共算法框架，复用代码               | 复用公共代码，灵活定义变化部分     |
| 访问者   | 对结构中的元素定义新操作   | 数据结构稳定，操作经常变化               | 操作集中，数据结构与操作分离       |

这些模式使得程序结构更加灵活、可扩展，同时降低对象间的耦合度。正确使用行为型模式可以构建出更具弹性和可维护性的软件系统

## 责任链模式 (Chain of Responsibility)

将请求的发送者和接收者解耦，让多个对象都有机会处理请求，避免请求发送者和接收者之间的耦合关系

### 适用场景

- 有多个对象可以处理同一请求，具体处理者由运行时决定
- 在不明确指定接收者的情况下向多个对象中的一个提交请求
- 可动态指定处理者集合
- 需要往多个处理者传递请求，直到有对象正确处理

### 实现示例

#### Golang 实现

```go
package chain

import "fmt"

// 处理者接口
type Handler interface {
    SetNext(handler Handler) Handler
    Handle(request string) string
}

// 基础处理者
type AbstractHandler struct {
    next Handler
}

func (h *AbstractHandler) SetNext(handler Handler) Handler {
    h.next = handler
    return handler
}

func (h *AbstractHandler) Handle(request string) string {
    if h.next != nil {
        return h.next.Handle(request)
    }
    return ""
}

// 具体处理者A
class MonkeyHandler struct {
    AbstractHandler
}

func (m *MonkeyHandler) Handle(request string) string {
    if request == "香蕉" {
        return "猴子吃掉了香蕉"
    }

    return m.AbstractHandler.Handle(request)
}

// 具体处理者B
class SquirrelHandler struct {
    AbstractHandler
}

func (s *SquirrelHandler) Handle(request string) string {
    if request == "坚果" {
        return "松鼠吃掉了坚果"
    }

    return s.AbstractHandler.Handle(request)
}

// 具体处理者C
class DogHandler struct {
    AbstractHandler
}

func (d *DogHandler) Handle(request string) string {
    if request == "骨头" {
        return "狗吃掉了骨头"
    }

    return "无法处理该请求"
}

// 客户端代码
func main() {
    // 创建处理链
    monkey := &MonkeyHandler{}
    squirrel := &SquirrelHandler{}
    dog := &DogHandler{}

    monkey.SetNext(squirrel).SetNext(dog)

    // 测试处理链
    foods := []string{"坚果", "香蕉", "骨头", "胡萝卜"}

    for _, food := range foods {
        fmt.Printf("谁吃%s? %s\n", food, monkey.Handle(food))
    }
}
```

#### Python 实现

```python
from abc import ABC, abstractmethod

# 处理者接口
class Handler(ABC):
    @abstractmethod
    def handle(self, request):
        pass

    @abstractmethod
    def set_next(self, handler):
        pass

# 基础处理者
class AbstractHandler(Handler):
    def __init__(self):
        self._next_handler = None

    def set_next(self, handler):
        self._next_handler = handler
        return handler

    def handle(self, request):
        if self._next_handler:
            return self._next_handler.handle(request)
        return ""

# 具体处理者A
class MonkeyHandler(AbstractHandler):
    def handle(self, request):
        if request == "香蕉":
            return "猴子吃掉了香蕉"
        return super().handle(request)

# 具体处理者B
class SquirrelHandler(AbstractHandler):
    def handle(self, request):
        if request == "坚果":
            return "松鼠吃掉了坚果"
        return super().handle(request)

# 具体处理者C
class DogHandler(AbstractHandler):
    def handle(self, request):
        if request == "骨头":
            return "狗吃掉了骨头"
        return "无法处理该请求"

# 使用示例
# 创建处理链
monkey = MonkeyHandler()
squirrel = SquirrelHandler()
dog = DogHandler()

monkey.set_next(squirrel).set_next(dog)

# 测试处理链
foods = ["坚果", "香蕉", "骨头", "胡萝卜"]

for food in foods:
    print(f"谁吃{food}? {monkey.handle(food)}")
```

#### TypeScript 实现

```typescript
// 处理者接口
interface Handler {
  handle(request: string): string;
  setNext(handler: Handler): Handler;
}

// 基础处理者
abstract class AbstractHandler implements Handler {
  protected nextHandler: Handler | null = null;

  public setNext(handler: Handler): Handler {
    this.nextHandler = handler;
    return handler;
  }

  public handle(request: string): string {
    if (this.nextHandler) {
      return this.nextHandler.handle(request);
    }
    return "";
  }
}

// 具体处理者A
class MonkeyHandler extends AbstractHandler {
  public handle(request: string): string {
    if (request === "香蕉") {
      return "猴子吃掉了香蕉";
    }
    return super.handle(request);
  }
}

// 具体处理者B
class SquirrelHandler extends AbstractHandler {
  public handle(request: string): string {
    if (request === "坚果") {
      return "松鼠吃掉了坚果";
    }
    return super.handle(request);
  }
}

// 具体处理者C
class DogHandler extends AbstractHandler {
  public handle(request: string): string {
    if (request === "骨头") {
      return "狗吃掉了骨头";
    }
    return "无法处理该请求";
  }
}

// 使用示例
// 创建处理链
const monkey = new MonkeyHandler();
const squirrel = new SquirrelHandler();
const dog = new DogHandler();

monkey.setNext(squirrel).setNext(dog);

// 测试处理链
const foods: string[] = ["坚果", "香蕉", "骨头", "胡萝卜"];

foods.forEach((food) => {
  console.log(`谁吃${food}? ${monkey.handle(food)}`);
});
```

## 命令模式 (Command)

将请求封装成对象，从而使你可用不同的请求对客户进行参数化，对请求排队或记录请求日志，以及支持可撤销操作

### 适用场景

- 需要将请求调用者和请求接收者解耦
- 需要支持撤销(Undo)和重做(Redo)操作
- 需要实现延迟执行、排队执行或远程执行
- 需要将操作组合成宏命令

### 实现示例

#### Golang 实现

```go
package command

import "fmt"

// 命令接口
type Command interface {
    Execute()
    Undo()
}

// 接收者
class Light struct {
    isOn bool
}

func (l *Light) TurnOn() {
    l.isOn = true
    fmt.Println("灯光已打开")
}

func (l *Light) TurnOff() {
    l.isOn = false
    fmt.Println("灯光已关闭")
}

// 具体命令
class LightOnCommand struct {
    light *Light
}

func (l *LightOnCommand) Execute() {
    l.light.TurnOn()
}

func (l *LightOnCommand) Undo() {
    l.light.TurnOff()
}

class LightOffCommand struct {
    light *Light
}

func (l *LightOffCommand) Execute() {
    l.light.TurnOff()
}

func (l *LightOffCommand) Undo() {
    l.light.TurnOn()
}

// 调用者
class RemoteControl struct {
    commands []Command
    undoCommand Command
}

func (r *RemoteControl) SetCommand(command Command) {
    r.commands = append(r.commands, command)
}

func (r *RemoteControl) PressButton() {
    if len(r.commands) > 0 {
        command := r.commands[len(r.commands)-1]
        command.Execute()
        r.undoCommand = command
    }
}

func (r *RemoteControl) PressUndoButton() {
    if r.undoCommand != nil {
        r.undoCommand.Undo()
    }
}

// 客户端代码
func main() {
    light := &Light{}

    lightOn := &LightOnCommand{light: light}
    lightOff := &LightOffCommand{light: light}

    remote := &RemoteControl{}
    remote.SetCommand(lightOn)
    remote.SetCommand(lightOff)

    remote.PressButton()
    remote.PressUndoButton()
}
```

#### Python 实现

```python
from abc import ABC, abstractmethod

# 命令接口
class Command(ABC):
    @abstractmethod
    def execute(self):
        pass

    @abstractmethod
    def undo(self):
        pass

# 接收者
class Light:
    def __init__(self):
        self.is_on = False

    def turn_on(self):
        self.is_on = True
        print("灯光已打开")

    def turn_off(self):
        self.is_on = False
        print("灯光已关闭")

# 具体命令
class LightOnCommand(Command):
    def __init__(self, light):
        self.light = light

    def execute(self):
        self.light.turn_on()

    def undo(self):
        self.light.turn_off()

class LightOffCommand(Command):
    def __init__(self, light):
        self.light = light

    def execute(self):
        self.light.turn_off()

    def undo(self):
        self.light.turn_on()

# 调用者
class RemoteControl:
    def __init__(self):
        self.commands = []
        self.undo_command = None

    def set_command(self, command):
        self.commands.append(command)

    def press_button(self):
        if self.commands:
            command = self.commands[-1]
            command.execute()
            self.undo_command = command

    def press_undo_button(self):
        if self.undo_command:
            self.undo_command.undo()

# 使用示例
light = Light()

light_on = LightOnCommand(light)
light_off = LightOffCommand(light)

remote = RemoteControl()
remote.set_command(light_on)
remote.set_command(light_off)

remote.press_button()
remote.press_undo_button()
```

#### TypeScript 实现

```typescript
// 命令接口
interface Command {
  execute(): void;
  undo(): void;
}

// 接收者
class Light {
  private isOn: boolean = false;

  public turnOn(): void {
    this.isOn = true;
    console.log("灯光已打开");
  }

  public turnOff(): void {
    this.isOn = false;
    console.log("灯光已关闭");
  }
}

// 具体命令
class LightOnCommand implements Command {
  constructor(private light: Light) {}

  public execute(): void {
    this.light.turnOn();
  }

  public undo(): void {
    this.light.turnOff();
  }
}

class LightOffCommand implements Command {
  constructor(private light: Light) {}

  public execute(): void {
    this.light.turnOff();
  }

  public undo(): void {
    this.light.turnOn();
  }
}

// 调用者
class RemoteControl {
  private commands: Command[] = [];
  private undoCommand: Command | null = null;

  public setCommand(command: Command): void {
    this.commands.push(command);
  }

  public pressButton(): void {
    if (this.commands.length > 0) {
      const command = this.commands[this.commands.length - 1];
      command.execute();
      this.undoCommand = command;
    }
  }

  public pressUndoButton(): void {
    if (this.undoCommand) {
      this.undoCommand.undo();
    }
  }
}

// 使用示例
const light = new Light();

const lightOn = new LightOnCommand(light);
const lightOff = new LightOffCommand(light);

const remote = new RemoteControl();
remote.setCommand(lightOn);
remote.setCommand(lightOff);

remote.pressButton();
remote.pressUndoButton();
```

## 解释器模式 (Interpreter)

给定一门语言，定义它的文法的一种表示，并定义一个解释器，这个解释器使用该表示来解释语言中的句子

### 适用场景

- 有一个简单的语言需要解释执行
- 某个频繁发生的简单问题可以用一个简单的语言来表达
- 复杂的语法需要被解释

### 实现示例

#### Golang 实现

```go
package interpreter

import (
    "fmt"
    "strconv"
)

// 表达式接口
interface Expression interface {
    Interpret() int
}

// 具体表达式 - 数字
class NumberExpression struct {
    num int
}

func (n *NumberExpression) Interpret() int {
    return n.num
}

// 具体表达式 - 变量
class VariableExpression struct {
    name string
    value int
}

func (v *VariableExpression) Interpret() int {
    return v.value
}

// 具体表达式 - 加法
class AddExpression struct {
    left, right Expression
}

func (a *AddExpression) Interpret() int {
    return a.left.Interpret() + a.right.Interpret()
}

// 具体表达式 - 减法
class SubtractExpression struct {
    left, right Expression
}

func (s *SubtractExpression) Interpret() int {
    return s.left.Interpret() - s.right.Interpret()
}

// 环境类
class Context struct {
    variables map[string]int
}

func (c *Context) SetVariable(name string, value int) {
    if c.variables == nil {
        c.variables = make(map[string]int)
    }
    c.variables[name] = value
}

func (c *Context) GetVariable(name string) int {
    if val, ok := c.variables[name]; ok {
        return val
    }
    return 0
}

// 客户端代码
func main() {
    context := &Context{}
    context.SetVariable("x", 5)
    context.SetVariable("y", 10)

    // 解析表达式: x + y - 5
    expression := &SubtractExpression{
        left: &AddExpression{
            left:  &VariableExpression{name: "x"},
            right: &VariableExpression{name: "y"},
        },
        right: &NumberExpression{num: 5},
    }

    result := expression.Interpret()
    fmt.Printf("表达式 x + y - 5 的结果是: %d\n", result) // 应该输出10
}
```

#### Python 实现

```python
from abc import ABC, abstractmethod

# 表达式接口
class Expression(ABC):
    @abstractmethod
    def interpret(self):
        pass

# 具体表达式 - 数字
class NumberExpression(Expression):
    def __init__(self, num):
        self.num = num

    def interpret(self):
        return self.num

# 具体表达式 - 变量
class VariableExpression(Expression):
    def __init__(self, name, value):
        self.name = name
        self.value = value

    def interpret(self):
        return self.value

# 具体表达式 - 加法
class AddExpression(Expression):
    def __init__(self, left, right):
        self.left = left
        self.right = right

    def interpret(self):
        return self.left.interpret() + self.right.interpret()

# 具体表达式 - 减法
class SubtractExpression(Expression):
    def __init__(self, left, right):
        self.left = left
        self.right = right

    def interpret(self):
        return self.left.interpret() - self.right.interpret()

# 环境类
class Context:
    def __init__(self):
        self.variables = {}

    def set_variable(self, name, value):
        self.variables[name] = value

    def get_variable(self, name):
        return self.variables.get(name, 0)

# 使用示例
context = Context()
context.set_variable("x", 5)
context.set_variable("y", 10)

# 解析表达式: x + y - 5
expression = SubtractExpression(
    AddExpression(VariableExpression("x", 5), VariableExpression("y", 10)),
    NumberExpression(5)
)

result = expression.interpret()
print(f"表达式 x + y - 5 的结果是: {result}")  # 应该输出10
```

#### TypeScript 实现

```typescript
// 表达式接口
interface Expression {
  interpret(): number;
}

// 具体表达式 - 数字
class NumberExpression implements Expression {
  constructor(private num: number) {}

  public interpret(): number {
    return this.num;
  }
}

// 具体表达式 - 变量
class VariableExpression implements Expression {
  constructor(
    private name: string,
    private value: number,
  ) {}

  public interpret(): number {
    return this.value;
  }
}

// 具体表达式 - 加法
class AddExpression implements Expression {
  constructor(
    private left: Expression,
    private right: Expression,
  ) {}

  public interpret(): number {
    return this.left.interpret() + this.right.interpret();
  }
}

// 具体表达式 - 减法
class SubtractExpression implements Expression {
  constructor(
    private left: Expression,
    private right: Expression,
  ) {}

  public interpret(): number {
    return this.left.interpret() - this.right.interpret();
  }
}

// 环境类
class Context {
  private variables: { [key: string]: number } = {};

  public setVariable(name: string, value: number): void {
    this.variables[name] = value;
  }

  public getVariable(name: string): number {
    return this.variables[name] || 0;
  }
}

// 使用示例
const context = new Context();
context.setVariable("x", 5);
context.setVariable("y", 10);

// 解析表达式: x + y - 5
const expression = new SubtractExpression(
  new AddExpression(
    new VariableExpression("x", 5),
    new VariableExpression("y", 10),
  ),
  new NumberExpression(5),
);

const result = expression.interpret();
console.log(`表达式 x + y - 5 的结果是: ${result}`); // 应该输出10
```

## 中介者模式 (Mediator)

用一个中介对象来封装一系列的对象交互，中介者使各对象不需要显式地相互引用，从而使其松耦合，而且可以独立地改变它们之间的交互。

### 适用场景

- 一组对象以定义良好但是复杂的方式进行通信
- 定制一个分布在多个类中的行为，而又不想生成太多的子类
- 想创建一个易于对象间通信的-clear架构

### 实现示例

#### Golang 实现

```go
package mediator

import "fmt"

// 中介者接口
type Mediator interface {
    Notify(sender interface{}, event string)
}

// 中介者实现类
class ConcreteMediator struct {
    colleague1 *Colleague
    colleague2 *Colleague
}

func (c *ConcreteMediator) SetColleague1(colleague *Colleague) {
    c.colleague1 = colleague
}

func (c *ConcreteMediator) SetColleague2(colleague *Colleague) {
    c.colleague2 = colleague
}

func (c *ConcreteMediator) Notify(sender interface{}, event string) {
    if event == "A" {
        fmt.Println("中介者正在处理A事件，触发同事B的反应")
        c.colleague2.DoSomethingB()
    }
    if event == "B" {
        fmt.Println("中介者正在处理B事件，触发同事A的反应")
        c.colleague1.DoSomethingA()
    }
}

// 同事接口
type Colleague interface {
    SetMediator(mediator Mediator)
    DoSomethingA()
    DoSomethingB()
}

// 具体同事A
class ConcreteColleagueA struct {
    mediator Mediator
}

func (c *ConcreteColleagueA) SetMediator(mediator Mediator) {
    c.mediator = mediator
}

func (c *ConcreteColleagueA) DoSomethingA() {
    fmt.Println("同事A做事情A")
    c.mediator.Notify(c, "A")
}

func (c *ConcreteColleagueA) DoSomethingB() {
    fmt.Println("同事A原本可以做的事情B，但交给中介者处理")
}

// 具体同事B
class ConcreteColleagueB struct {
    mediator Mediator
}

func (c *ConcreteColleagueB) SetMediator(mediator Mediator) {
    c.mediator = mediator
}

func (c *ConcreteColleagueB) DoSomethingA() {
    fmt.Println("同事B原本可以做的事情A，但交给中介者处理")
}

func (c *ConcreteColleagueB) DoSomethingB() {
    fmt.Println("同事B做事情B")
    c.mediator.Notify(c, "B")
}

// 客户端代码
func main() {
    mediator := &ConcreteMediator{}

    colleague1 := &ConcreteColleagueA{}
    colleague2 := &ConcreteColleagueB{}

    colleague1.SetMediator(mediator)
    colleague2.SetMediator(mediator)

    mediator.SetColleague1(colleague1)
    mediator.SetColleague2(colleague2)

    colleague1.DoSomethingA()
    colleague2.DoSomethingB()
}
```

#### Python 实现

```python
from abc import ABC, abstractmethod

# 中介者接口
class Mediator(ABC):
    @abstractmethod
    def notify(self, sender, event):
        pass

# 中介者实现类
class ConcreteMediator(Mediator):
    def __init__(self):
        self.colleague1 = None
        self.colleague2 = None

    def set_colleague1(self, colleague):
        self.colleague1 = colleague

    def set_colleague2(self, colleague):
        self.colleague2 = colleague

    def notify(self, sender, event):
        if event == "A":
            print("中介者正在处理A事件，触发同事B的反应")
            self.colleague2.do_something_b()
        elif event == "B":
            print("中介者正在处理B事件，触发同事A的反应")
            self.colleague1.do_something_a()

# 同事接口
class Colleague(ABC):
    @abstractmethod
    def set_mediator(self, mediator):
        pass

    @abstractmethod
    def do_something_a(self):
        pass

    @abstractmethod
    def do_something_b(self):
        pass

# 具体同事A
class ConcreteColleagueA(Colleague):
    def __init__(self):
        self.mediator = None

    def set_mediator(self, mediator):
        self.mediator = mediator

    def do_something_a(self):
        print("同事A做事情A")
        self.mediator.notify(self, "A")

    def do_something_b(self):
        print("同事A原本可以做的事情B，但交给中介者处理")

# 具体同事B
class ConcreteColleagueB(Colleague):
    def __init__(self):
        self.mediator = None

    def set_mediator(self, mediator):
        self.mediator = mediator

    def do_something_a(self):
        print("同事B原本可以做的事情A，但交给中介者处理")

    def do_something_b(self):
        print("同事B做事情B")
        self.mediator.notify(self, "B")

# 使用示例
mediator = ConcreteMediator()

colleague1 = ConcreteColleagueA()
colleague2 = ConcreteColleagueB()

colleague1.set_mediator(mediator)
colleague2.set_mediator(mediator)

mediator.set_colleague1(colleague1)
mediator.set_colleague2(colleague2)

colleague1.do_something_a()
colleague2.do_something_b()
```

#### TypeScript 实现

```typescript
// 中介者接口
interface Mediator {
  notify(sender: object, event: string): void;
}

// 中介者实现类
class ConcreteMediator implements Mediator {
  private colleague1!: Colleague;
  private colleague2!: Colleague;

  public setColleague1(colleague: Colleague): void {
    this.colleague1 = colleague;
  }

  public setColleague2(colleague: Colleague): void {
    this.colleague2 = colleague;
  }

  public notify(sender: object, event: string): void {
    if (event === "A") {
      console.log("中介者正在处理A事件，触发同事B的反应");
      this.colleague2.doSomethingB();
    }
    if (event === "B") {
      console.log("中介者正在处理B事件，触发同事A的反应");
      this.colleague1.doSomethingA();
    }
  }
}

// 同事接口
interface Colleague {
  setMediator(mediator: Mediator): void;
  doSomethingA(): void;
  doSomethingB(): void;
}

// 具体同事A
class ConcreteColleagueA implements Colleague {
  private mediator!: Mediator;

  public setMediator(mediator: Mediator): void {
    this.mediator = mediator;
  }

  public doSomethingA(): void {
    console.log("同事A做事情A");
    this.mediator.notify(this, "A");
  }

  public doSomethingB(): void {
    console.log("同事A原本可以做的事情B，但交给中介者处理");
  }
}

// 具体同事B
class ConcreteColleagueB implements Colleague {
  private mediator!: Mediator;

  public setMediator(mediator: Mediator): void {
    this.mediator = mediator;
  }

  public doSomethingA(): void {
    console.log("同事B原本可以做的事情A，但交给中介者处理");
  }

  public doSomethingB(): void {
    console.log("同事B做事情B");
    this.mediator.notify(this, "B");
  }
}

// 使用示例
const mediator = new ConcreteMediator();

const colleague1 = new ConcreteColleagueA();
const colleague2 = new ConcreteColleagueB();

colleague1.setMediator(mediator);
colleague2.setMediator(mediator);

mediator.setColleague1(colleague1);
mediator.setColleague2(colleague2);

colleague1.doSomethingA();
colleague2.doSomethingB();
```

## 备忘录模式 (Memento)

在不破坏封装性的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态，这样就可将该对象恢复到原先保存的状态。

### 适用场景

- 需要保存一个对象在某一时刻的状态（以便恢复）
- 不希望破坏对象封装性，即不希望暴露对象内部实现细节
- 需要提供撤销操作

### 实现示例

#### Golang 实现

```go
package memento

import "fmt"

// 备忘录接口
type Memento interface{}

// 原发器
class Editor struct {
    content string
}

func (e *Editor) SetContent(content string) {
    e.content = content
    fmt.Printf("内容设置为: %s\n", content)
}

func (e *Editor) Save() Memento {
    fmt.Println("保存当前状态")
    return &EditorMemento{content: e.content}
}

func (e *Editor) Restore(memento Memento) {
    state := memento.(*EditorMemento)
    e.content = state.content
    fmt.Printf("恢复状态为: %s\n", e.content)
}

func (e *Editor) GetContent() string {
    return e.content
}

// 备忘录实现
type EditorMemento struct {
    content string
}

// 管理者
class Caretaker struct {
    mementos []Memento
}

func (c *Caretaker) Save(memento Memento) {
    c.mementos = append(c.mementos, memento)
    fmt.Println("状态已保存")
}

func (c *Caretaker) Get(index int) Memento {
    if index >= 0 && index < len(c.mementos) {
        return c.mementos[index]
    }
    return nil
}

func (c *Caretaker) Count() int {
    return len(c.mementos)
}

func (c *Caretaker) Clear() {
    c.mementos = nil
    fmt.Println("所有历史状态已清除")
}

// 客户端代码
func main() {
    editor := &Editor{}
    caretaker := &Caretaker{}

    editor.SetContent("初始文本")
    caretaker.Save(editor.Save())

    editor.SetContent("修改后的文本")
    caretaker.Save(editor.Save())

    editor.SetContent("再次修改的文本")
    caretaker.Save(editor.Save())

    fmt.Printf("当前内容: %s\n", editor.GetContent())

    editor.Restore(caretaker.Get(1))
    fmt.Printf("从状态1恢复后的内容: %s\n", editor.GetContent())

    editor.Restore(caretaker.Get(0))
    fmt.Printf("从状态0恢复后的内容: %s\n", editor.GetContent())
}
```

#### Python 实现

```python
# 备忘录接口
class Memento:
    pass

# 原发器
class Editor:
    def __init__(self):
        self._content = ""

    def set_content(self, content):
        self._content = content
        print(f"内容设置为: {content}")

    def save(self):
        print("保存当前状态")
        return EditorMemento(self._content)

    def restore(self, memento):
        self._content = memento.get_content()
        print(f"恢复状态为: {self._content}")

    def get_content(self):
        return self._content

# 备忘录实现
class EditorMemento(Memento):
    def __init__(self, content):
        self._content = content

    def get_content(self):
        return self._content

# 管理者
class Caretaker:
    def __init__(self):
        self._mementos = []

    def save(self, memento):
        self._mementos.append(memento)
        print("状态已保存")

    def get(self, index):
        if 0 <= index < len(self._mementos):
            return self._mementos[index]
        return None

    def count(self):
        return len(self._mementos)

    def clear(self):
        self._mementos = []
        print("所有历史状态已清除")

# 使用示例
editor = Editor()
caretaker = Caretaker()

editor.set_content("初始文本")
caretaker.save(editor.save())

editor.set_content("修改后的文本")
caretaker.save(editor.save())

editor.set_content("再次修改的文本")
caretaker.save(editor.save())

print(f"当前内容: {editor.get_content()}")

editor.restore(caretaker.get(1))
print(f"从状态1恢复后的内容: {editor.get_content()}")

editor.restore(caretaker.get(0))
print(f"从状态0恢复后的内容: {editor.get_content()}")
```

#### TypeScript 实现

```typescript
// 备忘录接口
interface Memento {
  // 备忘录接口通常不需要方法
}

// 原发器
class Editor {
  private content: string = "";

  public setContent(content: string): void {
    this.content = content;
    console.log(`内容设置为: ${content}`);
  }

  public save(): Memento {
    console.log("保存当前状态");
    return new EditorMemento(this.content);
  }

  public restore(memento: Memento): void {
    const state = memento as EditorMemento;
    this.content = state.getContent();
    console.log(`恢复状态为: ${this.content}`);
  }

  public getContent(): string {
    return this.content;
  }
}

// 备忘录实现
class EditorMemento implements Memento {
  constructor(private content: string) {}

  public getContent(): string {
    return this.content;
  }
}

// 管理者
class Caretaker {
  private mementos: Memento[] = [];

  public save(memento: Memento): void {
    this.mementos.push(memento);
    console.log("状态已保存");
  }

  public get(index: number): Memento | undefined {
    return this.mementos[index];
  }

  public count(): number {
    return this.mementos.length;
  }

  public clear(): void {
    this.mementos = [];
    console.log("所有历史状态已清除");
  }
}

// 使用示例
const editor = new Editor();
const caretaker = new Caretaker();

editor.setContent("初始文本");
caretaker.save(editor.save());

editor.setContent("修改后的文本");
caretaker.save(editor.save());

editor.setContent("再次修改的文本");
caretaker.save(editor.save());

console.log(`当前内容: ${editor.getContent()}`);

editor.restore(caretaker.get(1)!);
console.log(`从状态1恢复后的内容: ${editor.getContent()}`);

editor.restore(caretaker.get(0)!);
console.log(`从状态0恢复后的内容: ${editor.getContent()}`);
```

## 观察者模式 (Observer)

定义对象间的一种一对多依赖关系，使得每当一个对象状态发生改变时，其相关依赖对象皆得到通知并被自动更新。

### 适用场景

- 当一个对象的改变需要同时改变其他对象，而且不知道具体有多少对象需要改变
- 当一个对象必须通知其他对象，而它又不能假定其他对象是谁

### 实现示例

#### Golang 实现

```go
package observer

import (
    "fmt"
    "sync"
)

// 观察者接口
type Observer interface {
    Update(message string)
}

// 主题接口
type Subject interface {
    Register(observer Observer)
    Unregister(observer Observer)
    NotifyAll()
    SetMessage(message string)
}

// 具体主题
class NewsAgency struct {
    observers  []Observer
    news       string
    mutex      sync.RWMutex
}

func (n *NewsAgency) Register(observer Observer) {
    n.mutex.Lock()
    defer n.mutex.Unlock()
    n.observers = append(n.observers, observer)
}

func (n *NewsAgency) Unregister(observer Observer) {
    n.mutex.Lock()
    defer n.mutex.Unlock()
    for i, o := range n.observers {
        if o == observer {
            n.observers = append(n.observers[:i], n.observers[i+1:]...)
            break
        }
    }
}

func (n *NewsAgency) NotifyAll() {
    n.mutex.RLock()
    defer n.mutex.RUnlock()
    for _, observer := range n.observers {
        observer.Update(n.news)
    }
}

func (n *NewsAgency) SetMessage(news string) {
    n.mutex.Lock()
    defer n.mutex.Unlock()
    n.news = news
    n.NotifyAll()
}

func (n *NewsAgency) GetNews() string {
    n.mutex.RLock()
    defer n.mutex.RUnlock()
    return n.news
}

// 具体观察者
class NewsChannel struct {
    name string
}

func (n *NewsChannel) Update(message string) {
    fmt.Printf("%s 收到新闻: %s\n", n.name, message)
}

// 客户端代码
func main() {
    agency := &NewsAgency{}

    channel1 := &NewsChannel{name: "新闻频道1"}
    channel2 := &NewsChannel{name: "新闻频道2"}
    channel3 := &NewsChannel{name: "新闻频道3"}

    agency.Register(channel1)
    agency.Register(channel2)
    agency.Register(channel3)

    agency.SetMessage("Golang 1.20发布")

    agency.Unregister(channel2)
    agency.SetMessage("TypeScript 5.0发布")
}
```

#### Python 实现

```python
from abc import ABC, abstractmethod
from typing import List

# 观察者接口
class Observer(ABC):
    @abstractmethod
    def update(self, message):
        pass

# 主题接口
class Subject(ABC):
    @abstractmethod
    def register(self, observer):
        pass

    @abstractmethod
    def unregister(self, observer):
        pass

    @abstractmethod
    def notify_all(self):
        pass

    @abstractmethod
    def set_message(self, message):
        pass

# 具体主题
class NewsAgency(Subject):
    def __init__(self):
        self._observers: List[Observer] = []
        self._news = ""

    def register(self, observer):
        if observer not in self._observers:
            self._observers.append(observer)

    def unregister(self, observer):
        if observer in self._observers:
            self._observers.remove(observer)

    def notify_all(self):
        for observer in self._observers:
            observer.update(self._news)

    def set_message(self, message):
        self._news = message
        self.notify_all()

    def get_news(self):
        return self._news

# 具体观察者
class NewsChannel(Observer):
    def __init__(self, name):
        self.name = name

    def update(self, message):
        print(f"{self.name} 收到新闻: {message}")

# 使用示例
agency = NewsAgency()

channel1 = NewsChannel("新闻频道1")
channel2 = NewsChannel("新闻频道2")
channel3 = NewsChannel("新闻频道3")

agency.register(channel1)
agency.register(channel2)
agency.register(channel3)

agency.set_message("Python 3.11发布")
agency.unregister(channel2)
agency.set_message("Rust 1.65发布")
```

#### TypeScript 实现

```typescript
// 观察者接口
interface Observer {
  update(message: string): void;
}

// 主题接口
interface Subject {
  register(observer: Observer): void;
  unregister(observer: Observer): void;
  notifyAll(): void;
  setMessage(message: string): void;
}

// 具体主题
class NewsAgency implements Subject {
  private observers: Observer[] = [];
  private news: string = "";

  public register(observer: Observer): void {
    if (!this.observers.includes(observer)) {
      this.observers.push(observer);
    }
  }

  public unregister(observer: Observer): void {
    const index = this.observers.indexOf(observer);
    if (index !== -1) {
      this.observers.splice(index, 1);
    }
  }

  public notifyAll(): void {
    for (const observer of this.observers) {
      observer.update(this.news);
    }
  }

  public setMessage(message: string): void {
    this.news = message;
    this.notifyAll();
  }

  public getNews(): string {
    return this.news;
  }
}

// 具体观察者
class NewsChannel implements Observer {
  constructor(private name: string) {}

  public update(message: string): void {
    console.log(`${this.name} 收到新闻: ${message}`);
  }
}

// 使用示例
const agency = new NewsAgency();

const channel1 = new NewsChannel("新闻频道1");
const channel2 = new NewsChannel("新闻频道2");
const channel3 = new NewsChannel("新闻频道3");

agency.register(channel1);
agency.register(channel2);
agency.register(channel3);

agency.setMessage("JavaScript 2023新特性发布");
agency.unregister(channel2);
agency.setMessage("WebAssembly 2.0版发布");
```

## 状态模式 (State)

允许一个对象在其内部状态改变时改变它的行为，对象看起来似乎修改了它的类。

### 适用场景

- 一个对象的行为取决于它的状态，并且它必须在运行时根据状态改变行为
- 代码中包含大量与对象状态有关的条件语句

### 实现示例

#### Golang 实现

```go
package state

import "fmt"

// 状态接口
type State interface {
    Handle()
}

// 上下文
class Context struct {
    state State
}

func (c *Context) SetState(state State) {
    c.state = state
}

func (c *Context) Request() {
    c.state.Handle()
}

// 具体状态 - 开始
class StartState struct{}

func (s *StartState) Handle() {
    fmt.Println("上下文处于开始状态")
}

// 具体状态 - 运行中
class RunningState struct{}

func (r *RunningState) Handle() {
    fmt.Println("上下文处于运行中状态")
}

// 具体状态 - 结束
class EndState struct{}

func (e *EndState) Handle() {
    fmt.Println("上下文处于结束状态")
}

// 客户端代码
func main() {
    context := &Context{}

    context.SetState(&StartState{})
    context.Request()

    context.SetState(&RunningState{})
    context.Request()

    context.SetState(&EndState{})
    context.Request()
}
```

#### Python 实现

```python
from abc import ABC, abstractmethod

# 状态接口
class State(ABC):
    @abstractmethod
    def handle(self):
        pass

# 上下文
class Context:
    def __init__(self):
        self._state = None

    def set_state(self, state):
        self._state = state
        print(f"上下文当前状态: {type(state).__name__}")

    def request(self):
        if self._state:
            self._state.handle()

# 具体状态 - 开始
class StartState(State):
    def handle(self):
        print("上下文处于开始状态")

# 具体状态 - 运行中
class RunningState(State):
    def handle(self):
        print("上下文处于运行中状态")

# 具体状态 - 结束
class EndState(State):
    def handle(self):
        print("上下文处于结束状态")

# 使用示例
context = Context()

context.set_state(StartState())
context.request()

context.set_state(RunningState())
context.request()

context.set_state(EndState())
context.request()
```

#### TypeScript 实现

```typescript
// 状态接口
interface State {
  handle(): void;
}

// 上下文
class Context {
  private state!: State;

  public setState(state: State): void {
    this.state = state;
    console.log(`上下文当前状态: ${state.constructor.name}`);
  }

  public request(): void {
    this.state.handle();
  }
}

// 具体状态 - 开始
class StartState implements State {
  public handle(): void {
    console.log("上下文处于开始状态");
  }
}

// 具体状态 - 运行中
class RunningState implements State {
  public handle(): void {
    console.log("上下文处于运行中状态");
  }
}

// 具体状态 - 结束
class EndState implements State {
  public handle(): void {
    console.log("上下文处于结束状态");
  }
}

// 使用示例
const context = new Context();

context.setState(new StartState());
context.request();

context.setState(new RunningState());
context.request();

context.setState(new EndState());
context.request();
```

## 策略模式 (Strategy)

定义一系列的算法，把它们一个个封装起来，并且使它们可相互替换。

### 适用场景

实现相似但不完全相同的功能集时：

- 不同的排序算法（快速排序、归并排序、冒泡排序）
- 不同的折扣策略（学生折扣、老年人折扣、批量折扣）
- 不同的加密算法（AES、RSA、DES）

算法的选择与上下文独立时：

- 客户端不需要知道具体的算法细节，只需要依赖抽象策略

算法的选择依赖于运行时参数时：

```python
# 根据支付类型选择不同的支付算法
class PaymentStrategy:
    def pay(self, amount):
        pass

class CreditCard(PaymentStrategy):
    def pay(self, amount):
        # 借记卡支付实现
        pass

class DebitCard(PaymentStrategy):
    def pay(self, amount):
        # 贷记卡支付实现
        pass
```

当需要避免使用大量if-else或switch-case结构时：

```typescript
// 改造前的条件选择
function validate(data: any): boolean {
  if (data.type === "email") {
    // 验证电子邮件逻辑
  } else if (data.type === "phone") {
    // 验证电话号码逻辑
  } else if (data.type === "address") {
    // 验证地址逻辑
  }
  return false;
}

// 使用策略模式的重构
interface ValidatorStrategy {
  validate(data: any): boolean;
}
class EmailValidator implements ValidatorStrategy {
  validate(data: any): boolean {
    /* ... */
  }
}
class PhoneValidator implements ValidatorStrategy {
  validate(data: any): boolean {
    /* ... */
  }
}
class DataValidator {
  private strategy: ValidatorStrategy;
  constructor(strategy: ValidatorStrategy) {
    this.strategy = strategy;
  }
  setStrategy(strategy: ValidatorStrategy) {
    this.strategy = strategy;
  }
  validate(data: any): boolean {
    return this.strategy.validate(data);
  }
}
```

### 实现示例

### 实现示例

#### Golang 实现

```go
package strategy

// 排序策略接口
type SortStrategy interface {
    Sort(data []int)
}

// 原地排序策略 - 冒泡排序
type BubbleSort struct{}

func (b *BubbleSort) Sort(data []int) {
    n := len(data)
    for i := 0; i < n-1; i++ {
        for j := 0; j < n-i-1; j++ {
            if data[j] > data[j+1] {
                data[j], data[j+1] = data[j+1], data[j]
            }
        }
    }
}

// 原地排序策略 - 选择排序
type SelectionSort struct{}

func (s *SelectionSort) Sort(data []int) {
    n := len(data)
    for i := 0; i < n; i++ {
        minIndex := i
        for j := i + 1; j < n; j++ {
            if data[j] < data[minIndex] {
                minIndex = j
            }
        }
        data[i], data[minIndex] = data[minIndex], data[i]
    }
}

// 非原地排序策略 - 归并排序（使用辅助数组）
type MergeSort struct{}

func (m *MergeSort) Sort(data []int) {
    temp := make([]int, len(data))
    mergeSortHelper(data, temp, 0, len(data)-1)
}

// 归并排序辅助方法
func mergeSortHelper(data, temp []int, left, right int) {
    if left < right {
        middle := (left + right) / 2
        mergeSortHelper(data, temp, left, middle)
        mergeSortHelper(data, temp, middle+1, right)
        merge(data, temp, left, middle, right)
    }
}

func merge(data, temp []int, left, middle, right int) {
    // 归并实现
}

// 上下文类：根据策略接口使用不同的排序算法
type SortContext struct {
    strategy SortStrategy
}

func NewSortContext(strategyType string) *SortContext {
    var strategy SortStrategy
    switch strategyType {
    case "bubble":
        strategy = &BubbleSort{}
    case "selection":
        strategy = &SelectionSort{}
    case "merge":
        strategy = &MergeSort{}
    default:
        strategy = &BubbleSort{}
    }
    return &SortContext{strategy: strategy}
}

func (c *SortContext) ExecuteStrategy(data []int) {
    c.strategy.Sort(data)
}
```

#### Python 实现

```python
class PaymentStrategy:
    # 支付策略基类
    def pay(self, amount):
        pass

class CreditCard(PaymentStrategy):
    # 借记卡支付策略
    def pay(self, amount):
        print(f"Processing credit card payment of {amount}")
        # 借记卡支付具体实现

class DebitCard(PaymentStrategy):
    # 贷记卡支付策略
    def pay(self, amount):
        print(f"Processing debit card payment of {amount}")
        # 贷记卡支付具体实现

class PayPal(PaymentStrategy):
    # 支付宝支付策略
    def pay(self, amount):
        print(f"Processing PayPal payment of {amount}")
        # 支付宝支付具体实现

# 上下文类：订单处理
class Order:
    def __init__(self, payment_strategy):
        self.payment_strategy = payment_strategy

    def set_payment_strategy(self, payment_strategy):
        self.payment_strategy = payment_strategy

    def process_payment(self, amount):
        self.payment_strategy.pay(amount)

# 测试支付流程
if __name__ == "__main__":
    order = Order(CreditCard())
    order.process_payment(100)  # 输出：Processing credit card payment of 100

    order.set_payment_strategy(PayPal())
    order.process_payment(200)  # 输出：Processing PayPal payment of 200
```

#### TypeScript 实现

```typescript
// 策略接口：日志输出策略
interface LogStrategy {
  format(message: string, context: string): string;
}

// 控制台日志策略
class ConsoleLogStrategy implements LogStrategy {
  format(message: string, context: string): string {
    return `[${new Date().toISOString()}] ${context}: ${message}`;
  }
}

// 文件日志策略
class FileLogStrategy implements LogStrategy {
  private fs = require("fs");

  format(message: string, context: string): string {
    const filename = "app.log";
    const formatted = `[${new Date().toISOString()}] ${context}: ${message}\n`;
    this.fs.appendFileSync(filename, formatted);
    return formatted;
  }
}

// 邮件日志策略
class EmailLogStrategy implements LogStrategy {
  private nodemailer = require("nodemailer");

  format(message: string, context: string): string {
    const transporter = this.nodemailer.createTransport({
      service: "gmail",
      auth: {
        user: "your-email@gmail.com",
        pass: "your-password",
      },
    });

    const mailOptions = {
      from: "your-email@gmail.com",
      to: "admin@example.com",
      subject: "Log Alert",
      text: `[${new Date().toISOString()}] ${context}: ${message}`,
    };

    transporter.sendMail(mailOptions);
    return `[${new Date().toISOString()}] LOG_SENT: ${message}`;
  }
}

// 上下文类：日志系统
class Logger {
  private log_strategy: LogStrategy;

  constructor(strategy: LogStrategy) {
    this.log_strategy = strategy;
  }

  setStrategy(strategy: LogStrategy) {
    this.log_strategy = strategy;
  }

  log(message: string, context: string = "INFO") {
    const formatted = this.log_strategy.format(message, context);
    console.log(formatted);
  }
}

// 使用示例
const logger = new Logger(new ConsoleLogStrategy());
logger.log("User logged in");

const loggerFile = new Logger(new FileLogStrategy());
loggerFile.log("Server restarted", "SYSTEM");

const loggerEmail = new Logger(new EmailLogStrategy());
loggerEmail.log("Critical error detected!", "ERROR");
```

## 模板方法模式 (Template Method)

它定义了一个算法的骨架流程，并将步骤的具体实现延迟到子类中
这种模式允许子类在不改变整体结构的情况下重新定义或扩展步骤，同时保持核心算法的不变性
将算法的不变部分放在父类，可变部分放在子类，达到代码的复用和扩展的目的

抽象父类（Template Class）- 定义算法的固定执行步骤，即模板方法，具体实现交给子类完成

- 定义模板方法（Template Method），包含逻辑骨架
- 定义基本操作（Primitive Operations）
- 实现钩子方法（Hook Methods）控制流程

具体子类（Concrete Class）- 复用父类中定义的通用逻辑，仅需实现特定步骤

- 实现基本操作的具体行为
- 钩子方法默认实现为"空操作"（do nothing）

### 适用场景

### 适用场景

定义算法框架但允许子类调整具体步骤

- 标准工作流引擎中的任务处理流程
- 在线订单处理中的审批流程
- 游戏开发中的角色状态机

系统中存在大量重复代码

- 当多个类有相同结构但具体实现不同的情况
- 当算法的阶段性逻辑存在于多个子类中时

需要保持算法不变但允许调整某些步骤

- UI框架的渲染流程（如Web框架的渲染生命周期）
- CI/CD管道中的构建流程
- 第三方API集成时的请求处理流程

合理使用钩子方法 - 使默认行为可选而非强制

```typescript
protected canSkipStep(step: string): boolean {
    return process.env.SKIP_STEP.includes(step);
}
```

避免方法过度抽象 - 确保钩子方法有明确定义

```python
class GameTemplate:
    def optional_hook(self) -> int:
        """默认实现允许派生类重载但不能省略，典型违规："""
        return 0  # 这是危险的，因为会强制每子类都包含hook
```

考虑并发场景下的模板方法

```go
type ConcurrentTemplate interface {
    DoStep1() <-chan struct{}
    DoStep2() <-chan struct{}
    DoStep3() error
}
```

模板方法模式的核心价值在于提供结构而不强制实现，通过组合而不是继承来实现灵活的扩展性
遵循开闭原则，父类的骨架逻辑对扩展开放，对修改关闭
在实际项目中，它常与状态模式、策略模式以及其他行为模式结合使用，共同构成强大而灵活的系统设计

### 实现示例

#### Golang 文档处理流程

```go
package template

import (
    "fmt"
    "os"
    "time"
)

// 抽象基类 - 定义报告生成步骤
type DocumentTemplate interface {
    GenerateReport() error
}

// 具体子类#1 - 高级报告生成器
type HRReport struct {
    data string
}

func (r *HRReport) GenerateReport() error {
    fmt.Println("开始生成人事报告...")

    if err := r.fetchData(); err != nil {
        return err
    }
    if err := r.processData(); err != nil {
        return err
    }
    if err := r.renderTemplate(); err != nil {
        return err
    }
    if err := r.saveToFile(); err != nil {
        return err
    }

    fmt.Println("人事报告生成完成")
    return nil
}

// 钩子方法：数据获取
func (r *HRReport) fetchData() error {
    fmt.Printf("从HR系统获取数据 (%s)\n", time.Now().Format("15:04:05"))
    r.data = "人事统计数据已获取"
    return nil
}

// 基本操作：数据处理
func (r *HRReport) processData() error {
    fmt.Println("处理人事数据...")
    // 默认实现是空操作，可根据需要设置默认行为
    return nil
}

// 基本操作：渲染模板
func (r *HRReport) renderTemplate() error {
    fmt.Println("渲染高级人事报告模板...")
    return nil
}

// 基本操作：保存文件
func (r *HRReport) saveToFile() error {
    fmt.Println("将报告保存为PDF格式...")
    file, err := os.Create("hr_report.pdf")
    if err != nil {
        return err
    }
    defer file.Close()
    return nil
}

// 具体子类#2 - 简单报告生成器
type BasicReport struct {
    HRReport
}

func (b *BasicReport) GenerateReport() error {
    // 调用父类的模板方法
    if err := b.HRReport.GenerateReport(); err != nil {
        return err
    }

    // 重定义钩子方法为自定义行为
    b.fetchData = b.customDataFetch
    b.processData = b.simplifiedProcessing

    // 只执行自定义步骤
    return b.renderTemplate()
}

// 自定义钩子方法：数据获取
func (b *BasicReport) customDataFetch() error {
    fmt.Println("从简化数据源获取基本报告数据")
    b.data = "简化的人事摘要"
    return nil
}

// 覆盖基本操作：数据处理
func (b *BasicReport) simplifiedProcessing() error {
    fmt.Println("简化数据处理流程...")
    return nil
}
```

#### Python API请求处理

```python
import time
from abc import ABC, abstractmethod

class BaseAPIClient(ABC):
    """抽象基类 - 定义请求处理流程"""

    @abstractmethod
    def validate_credentials(self) -> bool:
        """验证API凭据，子类必须实现"""
        pass

    @abstractmethod
    def format_params(self, params: dict) -> dict:
        """格式化请求参数，子类必须实现"""
        pass

    def execute_request(self, endpoint: str, params: dict) -> dict:
        """模板方法：请求执行流程"""
        if not self.validate_credentials():
            raise Exception("无效API凭据")

        # 基本步骤：构建完整流程
        self.prepare_headers()
        params = self.format_params(params)
        response = self.make_api_call(endpoint, params)
        self.log_usage(endpoint, response)

        return self.process_response(response)

    def prepare_headers(self) -> None:
        """框架方法 - 设置API头信息"""
        print("准备API请求头...")
        self.headers = {
            "Content-Type": "application/json",
            "Authorization": f"Bearer {self.access_token}"
        }

    def make_api_call(self, endpoint: str, params: dict) -> dict:
        """框架方法 - 基础API调用"""
        print(f"发送请求到 {endpoint}...")
        # 模拟API调用
        time.sleep(0.5)
        return {"status": "success", "data": params}

    def log_usage(self, endpoint: str, response: dict) -> None:
        """可选步骤：记录API使用情况"""
        print(f"端点: {endpoint}, 数据: {response}")

    def process_response(self, response: dict) -> dict:
        """可选步骤：响应处理"""
        print("处理API响应...")
        return response

class EnterpriseClient(BaseAPIClient):
    """企业级API客户端 - 实现代具体业务逻辑"""

    def __init__(self, access_token: str) -> None:
        self.access_token = access_token
        self.headers = {}  # 将被prepare_headers覆盖

    def validate_credentials(self) -> bool:
        """验证企业API凭据"""
        print("验证企业级API权限...")
        return self.access_token.startswith("enterprise_")

    def format_params(self, params: dict) -> dict:
        """格式化企业API参数"""
        print("转换企业API参数...")
        # 企业特定的参数处理
        if "department" in params:
            params["dept_code"] = self.get_dept_code(params["department"])
        return params

    def get_dept_code(self, dept_name: str) -> int:
        # 模拟部门代码映射
        dept_map = {
            "HR": 1001,
            "IT": 1002,
            "Finance": 1003
        }
        return dept_map.get(dept_name, 9999)

class ConsumersClient(BaseAPIClient):
    """消费者级API客户端"""

    def validate_credentials(self) -> bool:
        """验证消费者API凭据"""
        print("验证消费者API权限...")
        return self.access_token.startswith("consumer_")

    def format_params(self, params: dict) -> dict:
        """格式化消费者API参数"""
        print("转换消费者API参数...")
        # 消费者特定的参数处理
        if "payment" in params:
            params["payment"] = self.format_currency(params["payment"])
        return params

    def format_currency(self, amount: float) -> dict:
        """货币格式化"""
        return {
            "amount": amount,
            "currency": "CNY"
        }

# 使用示例
if __name__ == "__main__":
    print("===== 企业API客户端示例 =====")
    enterprise_client = EnterpriseClient("enterprise_abc123")
    params = {"department": "HR", "count": 5}
    result = enterprise_client.execute_request("/api/employee", params)
    print(f"企业API响应: {result}")

    print("\n===== 消费者API客户端示例 =====")
    consumer_client = ConsumersClient("consumer_xyz456")
    payment_params = {"payment": "$42.99", "currency": "USD"}
    consumer_result = consumer_client.execute_request("/api/checkout", payment_params)
    print(f"消费者API响应: {consumer_result}")
```

#### TypeScript 用户认证流程

```typescript
// 抽象基类定义认证流程
abstract class AuthFlow {
    // 模板方法：整体认证流程骨架
    async execute(user: User): Promise<{ success: boolean }> {
        try {
            // 前置检查
            if (!await this.validateToken(user.token)) return { success: false };

            // 核心流程，子类必须实现部分
            await this.authenticate();
            await this.authorize(user.role);
            const session = await this.createSession();

            // 终极钩子：检查自定义条件
            if (await this.shouldCreateAccount(user)) {
                this.createAccountForUser(user);
            }

            return { success: true };
        } catch (error) {
            return { success: false, error };
        }
    }

    // 基本操作1：验证令牌
    protected async validateToken(token: string): Promise<boolean> {
        // 实际实现会连接到身份验证服务
        return true;
    }

    // 钩子方法1：身份验证实现，子类必须实现
    protected abstract authenticate(): Promise<boolean>;

    // 基本操作2：权限验证使用默认实现
    protected authorize(role: string): Promise<boolean> {
        allowedRoles = ['admin', 'user'];
        return new Promise(resolve => {
            allowedRoles.includes(role) ? resolve(true) : resolve(false);
        });
    }

    // 钩子方法2：可选步骤 - 创建用户账户
    protected shouldCreateAccount(user: User): Promise<boolean> {
        return Promise.resolve(false);
    }

    protected createAccountForUser(user: User): void {
        console.log(`正在为用户 ${user.id} 创建账户`);
        // 账户创建逻辑
    }
}

// 具体子类#1 - 社交媒体登录
class SocialAuthFlow extends AuthFlow {
    private provider: string;

    constructor(provider: string) {
        super();
        this.provider = provider;
    }

    // 实现钩子方法：自定义身份验证
    protected async authenticate(): Promise<boolean> {
        console.log(`通过社交媒体 ${this.provider} 进行身份验证`);
        // OAuth流程模拟
        return Math.random() > 0.2; // 80%的成功率模拟网络错误
    }
}

// 具体子类#2 - APIKey认证
class ApiKeyAuthFlow extends AuthFlow {
    private key: string;

    constructor(apiKey: string) {
        super();
        this.key = apiKey;
    }

    // 实现钩子方法：自定义身份验证
    protected async authenticate(): Promise<boolean> {
        console.log(`使用API密钥进行身份验证: ${this.key}`);
        // 密钥验证逻辑
        return this.key === "valid_api_key";
    }
}

// 使用示例
async function demonstrateAuth() {
    const socialFlow = new SocialAuthFlow('google');
    const apiKeyFlow = new ApiKeyAuthFlow('valid_api_key');

    try {
        console.log("使用社交媒体登录:");
        const socialResult = await socialFlow.execute({ id: 'user123', token: 'google_token', role: 'user' });
        console.log(`社交媒体登录结果:`, socialResult);

        console.log("\n使用API密钥登录:");
        const apiKeyResult = await apiKeyFlow.execute({ id: 'user456', token: 'valid_api_key', role: 'admin' });
        console.log(`API密钥登录结果:`, apiKeyResult);

        // 测试自定义账户创建
        console.log("\n测试自定义账户创建:");
        const modifiedFlow = new AuthFlow() {
            async validateToken(token): Promise<boolean> { return true; }
            async authenticate(): Promise<boolean> { return true; }
            shouldCreateAccount(user): Promise<boolean> {
                return Promise.resolve(user.id.length >= 6);
            }
            async authorize(role): Promise<boolean> { return true; }
        };

        const user = { id: "short_id", token: "test", role: "user" };
        const accountResult = await modifiedFlow.execute(user);
        console.log(`账户创建结果:`, accountResult);
    } catch (error) {
        console.error("认证失败:", error);
    }
}
```

## 访问者模式 (Visitor)

表示一个作用于某对象结构中的各元素的操作，它使你可以在不改变各元素的类的前提下定义作用于这些元素的新操作。

### 适用场景

- 一个数据结构包含很多类对象，这些类对象属于不同的类，而没有共同的接口
- 需要对一个对象结构中的对象进行很多不同的并且不相关的操作，而需要避免让这些操作"污染"这些对象的类
- 数据结构对象很少改变，但是需要在此数据结构上定义新的操作

### 实现示例

#### Golang 实现

```go
package visitor

import "fmt"

// 访问者接口
type Visitor interface {
    VisitElementA(element *ElementA)
    VisitElementB(element *ElementB)
}

// 元素接口
type Element interface {
    Accept(visitor Visitor)
}

// 具体元素A
class ElementA struct{}

func (e *ElementA) Accept(visitor Visitor) {
    visitor.VisitElementA(e)
}

func (e *ElementA) OperationA() {
    fmt.Println("元素A的操作")
}

// 具体元素B
class ElementB struct{}

func (e *ElementB) Accept(visitor Visitor) {
    visitor.VisitElementB(e)
}

func (e *ElementB) OperationB() {
    fmt.Println("元素B的操作")
}

// 具体访问者1
class ConcreteVisitor1 struct{}

func (c *ConcreteVisitor1) VisitElementA(element *ElementA) {
    fmt.Println("访问者1正在处理元素A")
    element.OperationA()
}

func (c *ConcreteVisitor1) VisitElementB(element *ElementB) {
    fmt.Println("访问者1正在处理元素B")
    element.OperationB()
}

// 具体访问者2
class ConcreteVisitor2 struct{}

func (c *ConcreteVisitor2) VisitElementA(element *ElementA) {
    fmt.Println("访问者2正在处理元素A")
    element.OperationA()
}

func (c *ConcreteVisitor2) VisitElementB(element *ElementB) {
    fmt.Println("访问者2正在处理元素B")
    element.OperationB()
}

// 对象结构
class ObjectStructure struct {
    elements []Element
}

func (o *ObjectStructure) Add(element Element) {
    o.elements = append(o.elements, element)
}

func (o *ObjectStructure) Remove(element Element) {
    for i, e := range o.elements {
        if e == element {
            o.elements = append(o.elements[:i], o.elements[i+1:]...)
            break
        }
    }
}

func (o *ObjectStructure) Accept(visitor Visitor) {
    for _, element := range o.elements {
        element.Accept(visitor)
    }
}

// 客户端代码
func main() {
    o := &ObjectStructure{}
    o.Add(&ElementA{})
    o.Add(&ElementB{})

    v1 := &ConcreteVisitor1{}
    v2 := &ConcreteVisitor2{}

    fmt.Println("使用访问者1:")
    o.Accept(v1)

    fmt.Println("\n使用访问者2:")
    o.Accept(v2)
}
```

#### Python 实现

```python
from abc import ABC, abstractmethod

# 访问者接口
class Visitor(ABC):
    @abstractmethod
    def visit_element_a(self, element):
        pass

    @abstractmethod
    def visit_element_b(self, element):
        pass

# 元素接口
class Element(ABC):
    @abstractmethod
    def accept(self, visitor):
        pass

# 具体元素A
class ElementA(Element):
    def accept(self, visitor):
        visitor.visit_element_a(self)

    def operation_a(self):
        print("元素A的操作")

# 具体元素B
class ElementB(Element):
    def accept(self, visitor):
        visitor.visit_element_b(self)

    def operation_b(self):
        print("元素B的操作")

# 具体访问者1
class ConcreteVisitor1(Visitor):
    def visit_element_a(self, element):
        print("访问者1正在处理元素A")
        element.operation_a()

    def visit_element_b(self, element):
        print("访问者1正在处理元素B")
        element.operation_b()

# 具体访问者2
class ConcreteVisitor2(Visitor):
    def visit_element_a(self, element):
        print("访问者2正在处理元素A")
        element.operation_a()

    def visit_element_b(self, element):
        print("访问者2正在处理元素B")
        element.operation_b()

# 对象结构
class ObjectStructure:
    def __init__(self):
        self._elements = []

    def add(self, element):
        self._elements.append(element)

    def remove(self, element):
        self._elements.remove(element)

    def accept(self, visitor):
        for element in self._elements:
            element.accept(visitor)

# 使用示例
o = ObjectStructure()
o.add(ElementA())
o.add(ElementB())

v1 = ConcreteVisitor1()
v2 = ConcreteVisitor2()

print("使用访问者1:")
o.accept(v1)

print("\n使用访问者2:")
o.accept(v2)
```

#### TypeScript 实现

```typescript
// 访问者接口
interface Visitor {
  visitElementA(element: ElementA): void;
  visitElementB(element: ElementB): void;
}

// 元素接口
interface Element {
  accept(visitor: Visitor): void;
}

// 具体元素A
class ElementA implements Element {
  public accept(visitor: Visitor): void {
    visitor.visitElementA(this);
  }

  public operationA(): void {
    console.log("元素A的操作");
  }
}

// 具体元素B
class ElementB implements Element {
  public accept(visitor: Visitor): void {
    visitor.visitElementB(this);
  }

  public operationB(): void {
    console.log("元素B的操作");
  }
}

// 具体访问者1
class ConcreteVisitor1 implements Visitor {
  public visitElementA(element: ElementA): void {
    console.log("访问者1正在处理元素A");
    element.operationA();
  }

  public visitElementB(element: ElementB): void {
    console.log("访问者1正在处理元素B");
    element.operationB();
  }
}

// 具体访问者2
class ConcreteVisitor2 implements Visitor {
  public visitElementA(element: ElementA): void {
    console.log("访问者2正在处理元素A");
    element.operationA();
  }

  public visitElementB(element: ElementB): void {
    console.log("访问者2正在处理元素B");
    element.operationB();
  }
}

// 对象结构
class ObjectStructure {
  private elements: Element[] = [];

  public add(element: Element): void {
    this.elements.push(element);
  }

  public remove(element: Element): void {
    const index = this.elements.indexOf(element);
    if (index !== -1) {
      this.elements.splice(index, 1);
    }
  }

  public accept(visitor: Visitor): void {
    for (const element of this.elements) {
      element.accept(visitor);
    }
  }
}

// 使用示例
const o = new ObjectStructure();
o.add(new ElementA());
o.add(new ElementB());

const v1 = new ConcreteVisitor1();
const v2 = new ConcreteVisitor2();

console.log("使用访问者1:");
o.accept(v1);

console.log("\n使用访问者2:");
o.accept(v2);
```
