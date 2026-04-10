# Mermaid 语法完整参考手册

## 初始化配置（init block）

### 基本结构
```
%%{init: {'theme': 'base', 'themeVariables': {'fontSize': '20px', 'fontFamily': 'Microsoft YaHei, Arial'}}}%%
```

### 主题选项

| 主题名 | 特点 | 适用场景 |
|--------|------|---------|
| `base` | 极简白底，最可定制 | 自定义配色时首选 |
| `default` | 蓝色调默认主题 | 通用场景 |
| `dark` | 深色背景 | 暗色文档、演示 |
| `forest` | 绿色调自然风格 | 非正式场景 |
| `neutral` | 灰度无色彩 | 打印文档 |

### themeVariables 可配置项

```
%%{init: {
  'theme': 'base',
  'themeVariables': {
    'primaryColor': '#f6f9fe',
    'primaryTextColor': '#1a1a2e',
    'primaryBorderColor': '#3285c2',
    'lineColor': '#666666',
    'secondaryColor': '#f5fefd',
    'tertiaryColor': '#fffbf5',
    'background': '#ffffff',
    'mainBkg': '#f6f9fe',
    'nodeBorder': '#3285c2',
    'clusterBkg': '#fafafa',
    'titleColor': '#1a1a2e',
    'edgeLabelBackground': '#ffffff',
    'fontSize': '16px',
    'fontFamily': 'Microsoft YaHei, Arial, sans-serif'
  }
}}%%
```

### ELK 布局引擎

ELK（Eclipse Layout Kernel）提供更好的自动布局，适合复杂图形：

```
%%{init: {'flowchart': {'defaultRenderer': 'elk'}}}%%
graph TB
    A --> B
    A --> C
    B --> D
    C --> D
```

ELK 适用于：节点超过 15 个、存在大量交叉连线、需要更均匀的层级分布。

---

## graph / flowchart 完整语法

### 方向声明

```
graph TB    % Top to Bottom（从上到下，最常用）
graph BT    % Bottom to Top
graph LR    % Left to Right（流程图常用）
graph RL    % Right to Left
```

`flowchart` 是 `graph` 的别名，功能相同，推荐用 `flowchart`。

### 节点形状大全

```
graph TB
    A[方形节点]               %% 默认方形，表示普通组件/服务
    B(圆角矩形)               %% 表示过程/步骤
    C([椭圆形/胶囊])          %% 表示开始/结束节点
    D[[子程序形状]]           %% 表示子程序或预定义流程
    E[(圆柱形/数据库)]        %% 表示数据库/存储
    F((圆形))                 %% 表示事件或连接点
    G{菱形/判断}              %% 表示决策/条件判断
    H{{六边形}}               %% 表示准备/条件
    I[/平行四边形/]           %% 表示输入/输出
    J[\反向平行四边形\]       %% 表示输入/输出（变体）
    K[/梯形\]                 %% 表示手工操作
    L[\反梯形/]               %% 表示手工输入
```

### 连接线类型

```
graph LR
    A --> B          %% 实线箭头（强依赖）
    C --- D          %% 实线无箭头（关联）
    E -.-> F         %% 虚线箭头（弱依赖/计划中）
    G -.- H          %% 虚线无箭头（松散关联）
    I ==> J          %% 粗实线箭头（强调/主路径）
    K === L          %% 粗实线无箭头
    M ~~~ N          %% 不可见连接（仅用于控制布局）
    O --o P          %% 圆形终点
    Q --x R          %% 叉形终点（表示终止）
    S o--o T         %% 双向圆形
    U <--> V         %% 双向箭头
```

### 连接线标签

```
graph LR
    A -->|"REST/JSON"| B
    C ---|"同步调用"| D
    E -.->|"异步消息"| F
    G -->|"SQL :5432"| H
```

**注意**：标签中有空格时必须用引号包裹。

### 连接线长度控制

```
graph TB
    A --> B       %% 默认长度
    C ---> D      %% 额外一个 - 增加一级长度
    E ----> F     %% 增加两级长度
```

### subgraph 分层嵌套

```
graph TB
    subgraph frontend["前端层"]
        direction TB
        web["Web App<br/>Vue 3"]
        mobile["移动端<br/>React Native"]
    end

    subgraph backend["后端层"]
        direction TB
        api["API 网关<br/>FastAPI"]
        agent["AI Agent<br/>LangChain"]
    end

    subgraph data["数据层"]
        direction TB
        pg[("PostgreSQL<br/>:5432")]
        redis[("Redis<br/>:6379")]
    end

    frontend --> backend
    backend --> data
```

**三层嵌套（最大深度）**
```
graph TB
    subgraph outer["外层"]
        subgraph middle["中层"]
            subgraph inner["内层"]
                node["节点"]
            end
        end
    end
```

### 不可见 subgraph 分组技巧

用于将节点视觉分组但不显示边框：

```
graph TB
    subgraph g1[" "]
        A
        B
    end
    style g1 fill:none,stroke:none
```

---

## sequenceDiagram

### 基本结构

```mermaid
sequenceDiagram
    autonumber
    participant U as "用户"
    participant API as "API 网关"
    participant Agent as "AI Agent"
    participant DB as "数据库"

    U ->> API: POST /chat {"message": "..."}
    activate API
    API ->> Agent: 转发请求
    activate Agent
    Agent ->> DB: SELECT * FROM products
    DB -->> Agent: 返回产品列表
    Agent -->> API: 推荐结果
    deactivate Agent
    API -->> U: 200 OK {"reply": "..."}
    deactivate API
```

### 参与者声明

```
participant A as "显示名称"    %% 矩形框（服务、系统）
actor B as "用户角色"          %% 人形图标（人类用户）
```

### 箭头类型

```
A ->> B      %% 实线箭头（同步请求）
A -->> B     %% 虚线箭头（响应/返回值）
A -> B       %% 实线无箭头
A --> B      %% 虚线无箭头
A -x B       %% 带叉箭头（表示异步消息，丢弃）
A -) B       %% 带圆箭头（异步）
```

### 激活与停用

```
activate A
A ->> B: 调用
deactivate A

%% 简写形式（+/- 符号）
A ->>+ B: 激活并调用
B -->>- A: 返回并停用
```

### Note 注释

```
Note over A, B: 跨两个参与者的注释
Note left of A: 左侧注释
Note right of B: 右侧注释
```

### 循环和条件块

```
loop 轮询（每 30 秒）
    A ->> B: 心跳检查
    B -->> A: pong
end

alt 用户已登录
    A ->> B: 获取用户数据
else 用户未登录
    A ->> B: 跳转登录页
end

opt 仅在有缓存时
    A ->> Cache: 读取缓存
end
```

### 并行块

```
par 并行调用
    A ->> B: 调用服务 B
and
    A ->> C: 调用服务 C
end
```

### critical 块（错误处理）

```
critical 获取数据库连接
    A ->> DB: 连接
option 超时
    A ->> Log: 记录超时
option 连接被拒
    A ->> Log: 记录拒绝
end
```

### rect 背景高亮

```
rect rgba(50, 133, 194, 0.1)
    Note over A, B: 认证流程
    A ->> B: 发送 Token
    B -->> A: 验证结果
end
```

---

## stateDiagram-v2

### 基本语法

```mermaid
stateDiagram-v2
    [*] --> 待审核
    待审核 --> 审核中 : 提交申请
    审核中 --> 已批准 : 审核通过
    审核中 --> 已拒绝 : 审核拒绝
    已批准 --> [*]
    已拒绝 --> [*]
```

### 复合状态

```
stateDiagram-v2
    state 处理中 {
        [*] --> 解析请求
        解析请求 --> 调用模型
        调用模型 --> 格式化结果
        格式化结果 --> [*]
    }
    [*] --> 处理中
    处理中 --> 完成
```

### 选择节点（Choice）

```
stateDiagram-v2
    state 判断金额 <<choice>>
    [*] --> 判断金额
    判断金额 --> 小额支付 : 金额 < 1000
    判断金额 --> 大额支付 : 金额 >= 1000
```

### 并发状态

```
stateDiagram-v2
    state 订单处理 {
        [*] --> 库存检查
        --
        [*] --> 支付验证
        --
        [*] --> 风控检测
    }
```

### Fork / Join

```
stateDiagram-v2
    state fork_state <<fork>>
    state join_state <<join>>

    [*] --> fork_state
    fork_state --> 任务A
    fork_state --> 任务B
    任务A --> join_state
    任务B --> join_state
    join_state --> [*]
```

### 状态注释

```
stateDiagram-v2
    A --> B
    note right of A
        这里是注释内容
        可以多行
    end note
```

---

## erDiagram

### 关系符号说明

```
||--||    一对一（强制）
||--o|    一对零或一
||--|{    一对多（至少一个）
||--o{    一对零或多
}|--||    多对一（至少一个）
}o--o{    零或多对零或多
```

**符号含义**
- `|` = exactly one（恰好一个）
- `o` = zero or one（零或一）
- `{` = one or more（一或多）

### 完整示例

```mermaid
erDiagram
    USER {
        bigint id PK "主键"
        varchar(100) name "用户姓名"
        varchar(200) email UK "邮箱（唯一）"
        timestamp created_at "创建时间"
    }

    INSURANCE_PRODUCT {
        bigint id PK
        varchar(100) name "产品名称"
        varchar(50) category "产品类别"
        decimal premium "保费"
        bigint provider_id FK "保险公司 ID"
    }

    ORDER {
        bigint id PK
        bigint user_id FK
        bigint product_id FK
        varchar(20) status "待支付/已支付/已退保"
        timestamp order_time
    }

    PROVIDER {
        bigint id PK
        varchar(100) name "保险公司名称"
    }

    USER ||--o{ ORDER : "下单"
    INSURANCE_PRODUCT ||--o{ ORDER : "被购买"
    PROVIDER ||--|{ INSURANCE_PRODUCT : "提供"
```

### 字段标记

| 标记 | 含义 |
|------|------|
| PK | Primary Key（主键）|
| FK | Foreign Key（外键）|
| UK | Unique Key（唯一键）|

---

## gantt

### 基本语法

```mermaid
gantt
    title AI 保险系统开发计划
    dateFormat YYYY-MM-DD
    excludes weekends

    section 基础建设
    数据库设计           :done,    db,    2024-01-01, 2024-01-07
    API 框架搭建         :done,    api,   2024-01-05, 7d
    用户认证模块         :active,  auth,  after api, 5d

    section AI 功能
    LLM 集成             :         llm,   2024-01-15, 10d
    Agent 编排           :crit,    agent, after llm, 14d
    推荐引擎             :         rec,   after agent, 10d

    section 里程碑
    MVP 上线             :milestone, m1, 2024-02-20, 0d
```

### 任务状态

| 关键字 | 含义 | 视觉效果 |
|--------|------|---------|
| `done` | 已完成 | 深色填充 |
| `active` | 进行中 | 蓝色高亮 |
| `crit` | 关键路径 | 红色高亮 |
| `milestone` | 里程碑 | 菱形标记 |

### 时间引用

```
after task_id          %% 紧接在指定任务之后
2024-01-15             %% 绝对日期
7d                     %% 相对天数
```

---

## C4 语法（实验性功能）

### C4Context

```mermaid
%%{init: {'theme': 'base'}}%%
C4Context
    title 系统上下文图

    Person(customer, "客户", "购买保险的用户")
    Person_Ext(broker, "经纪人", "第三方销售渠道")

    System(ins_sys, "保险规划系统", "AI 驱动的保险推荐平台")
    System_Ext(pay, "支付宝", "第三方支付")
    System_Ext(crm, "CRM 系统", "销售管理平台")
    SystemDb_Ext(legacy, "旧核心系统", "历史保单数据")

    Rel(customer, ins_sys, "使用", "HTTPS")
    Rel(broker, ins_sys, "接入", "API")
    Rel(ins_sys, pay, "收款", "REST")
    Rel(ins_sys, crm, "同步", "REST")
    Rel(ins_sys, legacy, "查询历史", "JDBC")
    
    UpdateLayoutConfig($c4ShapeInRow="3", $c4BoundaryInRow="1")
```

### C4Container

```mermaid
C4Container
    title 容器图

    Person(user, "用户")

    System_Boundary(sys, "保险系统") {
        Container(spa, "单页应用", "Vue 3 / TypeScript", "用户界面")
        Container(api, "API 服务", "FastAPI / Python", "REST API")
        Container(agent_srv, "Agent 服务", "LangChain / Python", "AI 编排")
        ContainerDb(db, "主数据库", "PostgreSQL 16", "产品与用户数据")
        ContainerDb(vec_db, "向量库", "pgvector", "语义检索索引")
        Container(cache, "缓存", "Redis 7", "会话与热点数据")
    }

    Rel(user, spa, "访问", "HTTPS :443")
    Rel(spa, api, "调用", "REST/JSON :8000")
    Rel(api, agent_srv, "分发", "内部 HTTP")
    Rel(agent_srv, db, "读写", "SQL :5432")
    Rel(agent_srv, vec_db, "向量检索", "SQL :5432")
    Rel(api, cache, "读写", "Redis :6379")
```

### C4Component

```mermaid
C4Component
    title 组件图 - Agent 服务

    Container_Boundary(agent, "Agent 服务") {
        Component(supervisor, "Supervisor", "路由决策，分发请求到下游 Agent")
        Component(profile, "Profile Agent", "分析用户需求，构建画像")
        Component(recommend, "Recommend Agent", "基于画像推荐产品")
        Component(analysis, "Analysis Agent", "比较和解释产品方案")
        Component(memory, "Memory Manager", "管理对话上下文")
    }

    ComponentDb(db, "数据库", "PostgreSQL")
    Component_Ext(llm, "LLM API", "OpenAI / Claude")

    Rel(supervisor, profile, "分发")
    Rel(supervisor, recommend, "分发")
    Rel(supervisor, analysis, "分发")
    Rel(profile, memory, "读写上下文")
    Rel(recommend, db, "查询产品")
    Rel(profile, llm, "调用模型")
    Rel(recommend, llm, "调用模型")
```

### UpdateRelStyle（关系样式）

```
UpdateRelStyle(person, system, $textColor="blue", $lineColor="blue", $offsetY="-10")
```

### UpdateLayoutConfig

```
UpdateLayoutConfig($c4ShapeInRow="4", $c4BoundaryInRow="2")
```

---

## classDef 和 click

### classDef 定义样式

```mermaid
graph TB
    classDef frontend fill:#f6f9fe,stroke:#3285c2,stroke-width:2px,color:#1a1a2e
    classDef backend fill:#f5fefd,stroke:#23a5b4,stroke-width:2px
    classDef database fill:#f9f7fa,stroke:#834d9d,stroke-width:2px
    classDef alert fill:#fef5f6,stroke:#c80705,stroke-width:2px,stroke-dasharray:5

    A["Web 前端"]:::frontend
    B["API 服务"]:::backend
    C[("数据库")]:::database
    D["⚠ 待开发"]:::alert

    A --> B --> C
```

### 批量应用样式

```
class A,B,C frontend    %% 同时给多个节点应用同一 classDef
```

### click 交互

```mermaid
graph TB
    A["GitHub 仓库"]
    B["文档站"]

    click A "https://github.com/example/repo" _blank
    click B "https://docs.example.com" "点击查看文档" _blank
```

### click 回调（用于嵌入 HTML 时）

```
click A callback         %% 调用 JS 函数 callback(nodeId)
click A call myFunc()   %% 调用带参数的 JS 函数
```

---

## 高级技巧

### Edge ID（连接线样式定制）

```mermaid
graph LR
    A -- id1@--> B
    A -- id2@-.-> C

    style id1 stroke:#ff0000,stroke-width:3px
    style id2 stroke:#00aa00,stroke-dasharray:5
```

### 不可见连接线（布局控制）

用于强制特定节点的位置关系：

```mermaid
graph TB
    A["节点 A"]
    B["节点 B"]
    C["节点 C"]
    D["节点 D"]

    A --> B
    C --> D
    B ~~~ C    %% 不可见连接，让 B 和 C 对齐
```

### Tooltip

```mermaid
graph TB
    A["API 网关"]
    click A callback "FastAPI 0.104, 处理所有入站请求，限流 1000 req/s"
```

### 主题定制进阶

完整覆盖所有颜色变量以实现品牌一致性：

```
%%{init: {
  'theme': 'base',
  'themeVariables': {
    'primaryColor': '#e8f4f8',
    'primaryBorderColor': '#2196F3',
    'primaryTextColor': '#1a237e',
    'secondaryColor': '#e8f5e9',
    'secondaryBorderColor': '#4CAF50',
    'tertiaryColor': '#fff3e0',
    'tertiaryBorderColor': '#FF9800',
    'noteBkgColor': '#fffde7',
    'noteTextColor': '#333',
    'noteBorderColor': '#FFC107',
    'lineColor': '#546e7a',
    'textColor': '#1a1a2e',
    'mainBkg': '#ffffff',
    'clusterBkg': '#fafafa',
    'clusterBorder': '#e0e0e0',
    'fontFamily': 'Microsoft YaHei, PingFang SC, Arial, sans-serif',
    'fontSize': '16px'
  }
}}%%
```

---

## 布局控制高级技巧

### 声明顺序控制布局

Mermaid 的 dagre 引擎按代码中节点的**声明顺序**排列：
- 先声明的节点排在前面（TB模式中靠上，LR模式中靠左）
- 按阅读顺序声明：用户/入口在前，数据存储在后
- 研究表明，减少边交叉可提高读者理解准确率 30-40%

### 隐形链接

用 `~~~` 创建不可见的连接，强制两个节点相邻：
```
A ~~~ B  %% A和B相邻但无可见连线
```

### 间距调节

```
%%{init: {'flowchart': {'nodeSpacing': 80, 'rankSpacing': 60, 'curve': 'linear'}}}%%
```
- `nodeSpacing`: 同层节点间距（默认50）
- `rankSpacing`: 层间距（默认50）
- `curve`: `basis`(平滑) / `linear`(直线) / `stepBefore`(阶梯)

### ELK 布局引擎

大型复杂图使用 ELK 获得更智能的布局：
```
%%{init: {'flowchart': {'defaultRenderer': 'elk'}}}%%
```

---

## 中文特有问题与解决方案

### 问题 1：中文字体显示

**问题**：未指定中文字体时，汉字显示为方块或乱码。

**解决**：
```
%%{init: {'themeVariables': {'fontFamily': 'Microsoft YaHei, PingFang SC, Arial'}}}%%
```

字体优先级：Windows 使用 `Microsoft YaHei`，macOS 使用 `PingFang SC`，无中文字体时回退到 `Arial`。

---

### 问题 2：中文字符宽度导致布局错乱

**问题**：中文字符占 2 个英文字符宽度，导致节点大小不一致，布局混乱。

**解决**：限制每行字符数，使用 `<br/>` 换行：

```
A["AI 推荐引擎<br/>Recommendation Agent"]
B["PostgreSQL 16<br/>:5432 pgvector"]
```

**规则**：每行不超过 12 个汉字（约 25 个字符宽度）。

---

### 问题 3：节点 ID 使用中文

**问题**：中文 ID 在某些渲染环境下解析失败。

**错误示例**：
```
graph TB
    数据库 --> 服务器    %% 可能报错
```

**正确做法**：节点 ID 用英文，显示文本用中文：
```
graph TB
    db["数据库"]
    server["服务器"]
    db --> server
```

---

### 问题 4：subgraph ID 与显示文本

**语法**：
```
subgraph eng_id["中文显示标题"]
    内容节点
end
```

**注意**：`eng_id` 用于内部引用和样式定义，`"中文显示标题"` 是渲染时显示的文本。两者分开可以避免引用时中文编码问题。

---

### 问题 5：特殊字符转义

在节点标签中，以下字符需要用引号包裹或转义：

```
A["包含 (括号) 的标签"]
B["包含 [方括号] 的标签"]
C["包含 {大括号} 的标签"]
D["包含 >尖括号< 用 &gt; &lt;"]
```

---

### 推荐的中文架构图模板

```mermaid
%%{init: {
  'theme': 'base',
  'themeVariables': {
    'fontFamily': 'Microsoft YaHei, PingFang SC, Arial',
    'fontSize': '15px',
    'primaryColor': '#f0f4f8',
    'primaryBorderColor': '#3285c2'
  }
}}%%
graph TB
    classDef fe fill:#f6f9fe,stroke:#3285c2,stroke-width:2px
    classDef api fill:#f5fefd,stroke:#23a5b4,stroke-width:2px
    classDef biz fill:#fffbf5,stroke:#ff8000,stroke-width:2px
    classDef db fill:#f9f7fa,stroke:#834d9d,stroke-width:2px

    subgraph frontend["前端层"]
        web["Web 应用<br/>Vue 3"]:::fe
    end

    subgraph gateway["API 层"]
        api_gw["API 网关<br/>FastAPI :8000"]:::api
    end

    subgraph business["业务层"]
        agent["AI Agent<br/>LangChain"]:::biz
    end

    subgraph storage["数据层"]
        pg[("PostgreSQL<br/>:5432")]:::db
        redis[("Redis<br/>:6379")]:::db
    end

    web -->|"REST/JSON"| api_gw
    api_gw -->|"内部调用"| agent
    agent -->|"SQL"| pg
    agent -->|"缓存"| redis
```
