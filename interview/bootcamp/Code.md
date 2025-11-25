# 好的代码

优秀的代码如同一份精心撰写的法律合同：清晰、无歧义、经得起推敲，且为未来的变化预留空间

| 维度     | 核心要求                 | 反面案例                 |
| -------- | ------------------------ | ------------------------ |
| 可读性   | 自解释、简洁、命名清晰   | 缩写、冗长函数、魔法数字 |
| 可靠性   | 显式错误处理、防御性编程 | 忽略空指针、无超时机制   |
| 可维护性 | 低耦合、高内聚、依赖倒置 | 硬编码、全局状态         |
| 性能     | 精准优化、算法合理       | 过度优化、忽视瓶颈       |
| 安全     | 输入验证、最小权限       | 未验证SQL、明文存储密码  |

它不仅是功能的实现，更是对系统长期价值的投资。记住：代码被阅读的次数远大于被编写的次数，编写他人（包括未来的自己）能轻松理解的代码，才是专业开发者的核心素养

## 核心原则

单一职责原则 (SRP)：每个模块/类只负责一个功能领域，避免"上帝对象"

开闭原则 (OCP)：对扩展开放，对修改关闭。通过抽象和多态实现新功能而无需修改现有代码

里氏替换原则 (LSP)：子类必须能够替换其父类，不影响程序正确性

接口隔离原则 (ISP)：使用细粒度接口，避免"胖接口"导致的强制依赖

依赖倒置原则 (DIP)：高层模块不应依赖低层模块，都应依赖抽象，而非具体实现

### SOLID

#### S - 单一职责原则 (Single Responsibility Principle)

一个类应该只有一个引起它变化的原因。每个类应该只有一个职责，并且这个职责应该被完整地封装在该类中

实践建议：

- 保持类功能专注，避免"上帝对象"
- 每个类或模块应该对单一功能模块负责
- 当类变得过于复杂且需要频繁修改时，考虑拆分

示例对比：

```go
// 违反SRP - 类处理多种职责
type User struct {
    // 用户数据字段
}

func (u *User) SaveToDatabase() error {
    // 数据库保存逻辑
}

func (u *User) SendWelcomeEmail() error {
    // 发送邮件逻辑
}

// 遵循SRP - 分离职责
type User struct {
    // 用户数据字段
}

type UserRepository struct{}

func (r *UserRepository) Save(user *User) error {
    // 数据库保存逻辑
}

type EmailService struct{}

func (e *EmailService) SendWelcomeEmail(user *User) error {
    // 邮件发送逻辑
}
```

#### O - 开闭原则 (Open/Closed Principle)

软件实体（类、模块、函数等）应该对扩展开放，对修改关闭。即在不修改现有代码的情况下扩展系统功能

实践建议：

- 使用抽象接口定义契约
- 通过依赖注入实现可扩展性
- 利用多态和策略模式实现新功能

示例对比：

```typescript
// 违反OCP - 需要修改现有代码
function calculateArea(shape: Shape) {
  if (shape.type === "rectangle") {
    return shape.width * shape.height;
  } else if (shape.type === "circle") {
    return Math.PI * shape.radius * shape.radius;
  }
  // 添加新形状需要修改此函数
}

// 遵循OCP - 使用多态扩展
interface Shape {
  area(): number;
}

class Rectangle implements Shape {
  constructor(
    public width: number,
    public height: number,
  ) {}
  area(): number {
    return this.width * this.height;
  }
}

class Circle implements Shape {
  constructor(public radius: number) {}
  area(): number {
    return Math.PI * this.radius * this.radius;
  }
}

// 添加新形状只需创建新类
class Triangle implements Shape {
  // 实现略...
}
```

#### L - 里氏替换原则 (Liskov Substitution Principle)

子类型必须能够替换其基类型。所有引用基类的地方必须能透明地使用其子类的对象

实践建议：

- 子类不应改变父类的预期行为
- 子类可以扩展功能，但不能削弱父类的约定
- 避免在子类中抛出父类未声明的异常

示例对比：

```python
# 违反LSP - 子类改变了父类行为
class Bird:
    def fly(self):
        return "Flying"

class Penguin(Bird):
    def fly(self):
        raise Exception("Penguins can't fly!")

# 正确实现 - 不让企鹅继承鸟类
class Bird:
    def move(self):
        return "Moving"

class FlyingBird(Bird):
    def fly(self):
        return "Flying"

class Penguin(Bird):
    def swim(self):
        return "Swimming"
```

#### I - 接口隔离原则 (Interface Segregation Principle)

客户端不应该依赖它不需要的接口。使用多个专门的接口比使用单一的总接口要好

实践建议：

- 创建小而专注的接口
- 避免庞大接口（"接口污染"）
- 客户端只依赖它们实际使用的部分

示例对比：

```go
// 违反ISP - 单一庞大接口
type Worker interface {
    Work()
    Eat()
    Sleep()
}

// 普通工人实现
type HumanWorker struct{}

func (h HumanWorker) Work() { /* ... */ }
func (h HumanWorker) Eat() { /* ... */ }
func (h HumanWorker) Sleep() { /* ... */ }

// 机器人实现（不需要吃和睡）
type RobotWorker struct{}

func (r RobotWorker) Work() { /* ... */ }
func (r RobotWorker) Eat() { /* 无实现 */ }
func (r RobotWorker) Sleep() { /* 无实现 */ }

// 遵循ISP - 分离接口
type Worker interface {
    Work()
}

type Eater interface {
    Eat()
}

type Sleeper interface {
    Sleep()
}

// 实现具体接口组合
type HumanWorker struct{}

func (h HumanWorker) Work() { /* ... */ }
func (h HumanWorker) Eat() { /* ... */ }
func (h HumanWorker) Sleep() { /* ... */ }

type RobotWorker struct{}

func (r RobotWorker) Work() { /* ... */ }
// 只实现Worker接口
```

#### D - 依赖倒置原则 (Dependency Inversion Principle)

高层模块不应依赖低层模块，两者都应依赖抽象。抽象不应依赖细节，细节应依赖抽象

实践建议：

- 依赖抽象接口而非具体实现
- 使用依赖注入控制依赖关系
- 避免在高层模块中直接实例化具体类

示例对比：

```python
# 违反DIP - 高层模块依赖低层实现
class Database:
    def query(self):
        return "Database data"

class Application:
    def __init__(self):
        self.db = Database()  # 直接依赖具体实现

    def process(self):
        data = self.db.query()
        return data

# 遵循DIP - 依赖抽象
from abc import ABC, abstractmethod

class DataStorage(ABC):
    @abstractmethod
    def query(self):
        pass

class Database(DataStorage):
    def query(self):
        return "Database data"

class Application:
    def __init__(self, storage: DataStorage):  # 依赖抽象
        self.storage = storage

    def process(self):
        data = self.storage.query()
        return data

# 使用依赖注入
db = Database()
app = Application(db)
```

### 综合应用

这些原则不是孤立的，而是相互补充的：

组合关系：

- ISP和DIP经常一起使用，确保依赖关系明确
- SRP和OCP共同促进代码可维护性

实践场景：

- 微服务架构：每个微服务遵循SRP，通过定义良好接口(ISP)实现松耦合
- 领域驱动设计(DDD)：聚合边界体现SRP，领域服务对象间依赖通过接口抽象
- 插件系统：使用DIP实现可插拔组件，OCP支持新插件添加

平衡与权衡：

- 初期可适当简化某些原则(如快速原型)
- 随系统成熟度提高，逐步完善原则应用
- 避免过度设计，在复杂度和可维护性间找平衡

## 其他原则

| 原则  | 关键问题                     | 最适合场景                 | 风险                       |
| ----- | ---------------------------- | -------------------------- | -------------------------- |
| DRY   | 这个逻辑/代码在多处重复吗？  | 功能稳定、复用性高的系统   | 过度抽象、过度设计         |
| KISS  | 这是最简单直接的解决方案吗？ | 快速原型开发、小型系统     | 功能缺失、长期可维护性差   |
| YAGNI | 当前真的需要这个功能吗？     | 需求不确定、快速迭代的项目 | 忽略长期架构规划、频繁重构 |

### DRY - KISS - YAGNI

#### DRY (Don't Repeat Yourself) - 不要重复自己

DRY原则强调在软件开发中，每一个知识（即任何系统中的信息或逻辑）都应该有单一、清晰、权威的定义。避免代码重复是其中的关键表现

价值与目的

- 降低维护成本：修改逻辑只需在一处进行
- 提高一致性：确保相同逻辑在系统各处实现一致
- 减少bug：避免因多处实现相同逻辑导致的不一致问题
- 增强可读性：减少重复代码，使代码更简洁

实践建议

- 将重复的代码块提取成函数、方法或类
- 将共享数据和配置集中管理
- 使用设计模式如策略、工厂等减少条件分支重复
- 创建可复用的组件和库

常见误区

- 过度抽象：为避免重复而创建过于复杂的抽象层
- 不合理的泛化：将不相关的逻辑强行合并以减少重复
- 忽视文档重复：注释和文档同样也应遵循DRY原则

#### KISS (Keep It Simple, Stupid) - 保持简单明了

KISS原则强调应该优先选择最简单易懂的解决方案，而不是过度设计或使用过于复杂的实现方案

价值与目的

- 提高可读性：简单直观的代码更容易理解
- 降低维护成本：简单系统更容易修改和调试
- 减少错误：复杂逻辑更容易出错
- 加快开发速度：简单解决方案通常更快实现

实践建议

- 按功能最小粒度设计模块和函数
- 使用标准库和成熟框架而非自己实现
- 避免过度设计模式应用
- 优先考虑直接解决方案而非巧妙方案
- 保持函数短小，职责单一

常见误区

- 将"简单"等同于"简陋"：简单不是功能缺失，而是实现方式直接
- 忽视长远可维护性：简单有时需要适量抽象以支持未来扩展
- 避免所有优化：某些关键路径的性能优化可能需要复杂实现

#### YAGNI (You Ain't Gonna Need It) - 你不会需要它

YAGNI原则鼓励开发者只实现当前需要的功能，避免为了所谓的"未来需求"而添加不必要的功能和抽象

价值与目的

- 提高开发效率：专注于当前需求，避免开发无用功能
- 减少技术债务：避免因不确定需求导致的设计变更
- 加快交付速度：简化开发流程，快速交付核心功能
- 降低系统复杂度：减少不必要的代码和逻辑

实践建议

- 先实现MVP(最小可行产品)
- 使用迭代开发方法，逐步完善系统
- 对"未来可能需要"的功能谨慎实现
- 在添加新功能前，确认真有当前需求
- 在重构时移除未使用的功能

常见误区

- 将YAGNI理解为"不做规划"：必要的架构设计和架构边界定义仍是必要的
- 忽视可扩展性考量：在保持简单的同时，设计良好接口和模块结构
- 过度拆分功能：为简单而简单，忽略功能完整性

### 综合应用

问题驱动原则

- 遵循"先解决问题，再考虑重构"的思路
- 在确保功能正确的前提下，再考虑应用这些原则

情境适应性：

- 初创阶段：YAGNI为主，快速验证
- 成长阶段：平衡DRY与KISS
- 成熟阶段：强化DRY，保持系统可维护性

优先级排序：

- 当原则冲突时，依此优先级：YAGNI > KISS > DRY
- 避免为了DRY而添加未使用的抽象

渐进式应用：

- 随着系统演化，逐步应用这些原则
- 先确保功能正确，再改善代码质量

## 各个维度

### 可读性与表达力

清晰的命名

- 变量/函数名：`calculateTax()` 而非 `doCalc()`
- 类名：`PaymentProcessor` 而非 `PP`
- 避免缩写（除非行业通用如 `ID`、`URL`）

简洁的函数/方法

- 单一功能，5-15行为佳
- 开门见山：`if validateInput() && processRequest()`
- 避免嵌套（不超过2-3层缩进）

自注释代码

```python
# 不推荐的注释
# 获取用户ID
user_id = get_user_id()

# 推荐的注释（解释"为什么"）
# 使用UUID避免用户ID泄露隐私
user_id = generate_uuid()
```

### 可维护性与扩展性

低耦合与高内聚

- 模块内功能紧密相关
- 模块间通过接口而非直接实现交互

依赖注入

```java
// 避免硬编码依赖
public class PaymentService {
    private final PaymentGateway gateway;

    public PaymentService(PaymentGateway gateway) {
        this.gateway = gateway; // 注入而非new()
    }
}
```

设计模式合理应用

- 策略模式：替换算法实现
- 工厂模式：解耦对象创建
- 观察者模式：解耦事件发布/订阅

### 六、测试与质量保障

高测试覆盖率

- 单元测试：验证函数逻辑
- 集成测试：验证模块协作
- 契约测试：验证服务边界

持续集成/持续交付(CI/CD)

- 自动化流水线：编译→测试→部署
- 代码质量门禁（SonarQube、ESLint）

可测试性设计

- 依赖注入便于Mock
- 纯函数（无副作用）易于单元测试

### 性能与效率

避免过早优化，通过性能分析工具定位瓶颈，而非凭假设优化

算法与数据结构选择

- 数据量小：优先代码简单性
- 数据量大：优先时间/空间复杂度
  _例如：哈希表(O(1)查询) vs 有序数组(O(logn)查询)_

资源管理优化

- 惰性初始化
- 对象池技术（如Go的`sync.Pool`）
- 缓存策略（LRU、TTL等）

### 健壮性与可靠性

显式错误处理

- Go风格：`err != nil` 检查
- Python风格：`try-except` 包含具体异常
- TypeScript：`Result` 类型或 `throws` 声明

防御性编程

```javascript
// 避免空指针/未定义操作
const user = data?.user ?? defaultUser;
```

资源释放

- 使用 `defer` (Go)、`with` (Python)、`using` (C#)
- 实现 `AutoCloseable` 接口或上下文管理器

### 代码组织与架构

模块化结构

```plaintext
/src
  /domain    // 业务核心逻辑
  /infrastructure  // 数据库/外部服务实现
  /interfaces      // API层/用户接口
  /tests
```

明确分层

- 表现层 → 业务逻辑 → 数据访问
- 遵循依赖倒置（高层模块不依赖低层）

避免全局状态

- 使用依赖容器管理状态
- 并发安全（如Go的`sync.Map`、原子操作）

### 安全性与合规

输入验证

- 严格校验用户输入（SQL注入/XSS防护）
- 使用参数化查询

最小权限原则

- 服务只拥有必需的权限
- 敏感操作需要审计日志

敏感数据保护

- 加密存储密码等敏感信息
- 使用HTTPS、API密钥管理

### 文档与知识传递

文档即代码

- API文档（Swagger/OpenAPI）
- 部署文档（IaC如Terraform）
- 架构决策记录(ADR)

知识共享

- 代码审查（CR）机制
- 技术分享会/文档站点
