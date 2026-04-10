# Architecture Diagram Skill

A Claude Code skill for creating professional architecture diagrams using diagram-as-code tools.

**[中文说明](#中文说明) | [English](#english)**

---

## English

### What is this?

A skill that teaches Claude Code how to draw professional architecture diagrams. It provides a systematic methodology, Mermaid syntax reference, reusable templates, and a validated color palette.

### Features

- **5-Step Methodology**: Audience -> Diagram Type -> Elements -> Relationships -> Output + Verify
- **C4 Model Support**: Context, Container, Component, Code levels
- **9 Mermaid Templates**: System overview, C4 context/container, ER model, deployment, sequence, state machine, gantt, component
- **PACR Design Principles**: Proximity, Alignment, Contrast, Repetition
- **6-Color Semantic Palette**: Battle-tested fill/stroke pairs for architecture layers
- **Alternative Tools Reference**: D2, PlantUML, Structurizr DSL, Kroki, Python diagrams
- **Chinese Text Support**: Microsoft YaHei font configuration for CJK rendering

### Supported Diagram Types

| Type | Mermaid Syntax | Template |
|------|---------------|----------|
| System Overview | `graph TB` + subgraph | `templates/system-overview.mmd` |
| C4 Context | `C4Context` | `templates/c4-context.mmd` |
| C4 Container | `C4Container` | `templates/c4-container.mmd` |
| ER Data Model | `erDiagram` | `templates/data-model.mmd` |
| Deployment | `graph TB` + subgraph | `templates/deployment.mmd` |
| Sequence / Dataflow | `sequenceDiagram` | `templates/sequence-flow.mmd` |
| State Machine | `stateDiagram-v2` | `templates/state-machine.mmd` |
| Gantt Roadmap | `gantt` | `templates/roadmap.mmd` |
| Component | `graph TB/LR` | `templates/component.mmd` |

### Installation

Clone this repository into your Claude Code skills directory:

```bash
git clone https://github.com/CamelliaLilium/architecture-diagram-skill.git ~/.claude/skills/architecture-diagram
```

Or download and copy manually to `~/.claude/skills/architecture-diagram/`.

### Usage

Once installed, Claude Code will automatically trigger this skill when you:
- Ask to draw any architecture diagram
- Mention Mermaid, D2, PlantUML, or Structurizr
- Request system visualization, data models, deployment diagrams, etc.
- Simply say "draw a diagram" or "help me visualize the system"

### Color Palette

| Layer | Fill | Stroke | Semantic |
|-------|------|--------|----------|
| Frontend/UI | `#f6f9fe` | `#3285c2` | Blue - Interaction |
| API/Gateway | `#f5fefd` | `#23a5b4` | Teal - Interface |
| Business Logic | `#fffbf5` | `#ff8000` | Orange - Orchestration |
| Compute Engine | `#f9f7fa` | `#834d9d` | Purple - Algorithm |
| Data/Storage | `#f8faf9` | `#699261` | Green - Persistence |
| Not Ready/Alert | `#fef5f6` | `#c80705` | Red - Pending |

### Knowledge Sources

- Zhihu, ProcessOn, AWS China, Tencent Cloud (Chinese architecture diagram tutorials)
- C4 Model (c4model.com) by Simon Brown
- Excalidraw Diagram Skill design philosophy
- Architecture Antipatterns (Thoughtworks)
- Harvard Mermaid Guide, DEV Community
- Real-world production diagrams (11 Mermaid diagrams)

---

## 中文说明

### 这是什么？

一个 Claude Code 技能（Skill），教 Claude 如何绘制专业的架构图。提供系统化的方法论、Mermaid 语法参考、可复用模板和经过验证的配色方案。

### 核心功能

- **五步画图法**: 明受众 -> 定图型 -> 提要素 -> 理关系 -> 出成图+验证
- **C4 模型支持**: Context、Container、Component、Code 四层渐进
- **9个 Mermaid 模板**: 系统总览、C4上下文/容器、ER模型、部署、时序、状态机、甘特、组件
- **PACR 设计原则**: 亲密、对齐、对比、重复
- **6色语义色板**: 极淡fill + 饱和stroke，实战验证
- **备选工具参考**: D2、PlantUML、Structurizr DSL、Kroki、Python diagrams
- **中文支持**: 微软雅黑字体配置，中英混排最佳实践

### 安装

```bash
git clone https://github.com/CamelliaLilium/architecture-diagram-skill.git ~/.claude/skills/architecture-diagram
```

或手动下载并复制到 `~/.claude/skills/architecture-diagram/` 目录。

### 使用方式

安装后，当你对 Claude Code 说以下内容时会自动触发此技能：
- "画一张系统架构图"
- "帮我画个部署图"
- "用 Mermaid 画 ER 图"
- "帮我理清系统结构"
- 甚至只是"画个图"

### 目录结构

```
architecture-diagram/
├── SKILL.md               # 主技能文件（五步法、PACR、图型食谱）
├── references/
│   ├── diagram-types.md   # C4模型 + 4+1视图 + 7种架构图详解
│   ├── mermaid-syntax.md  # Mermaid 完整语法速查
│   ├── design-principles.md # 设计原则 + 色彩理论 + 反模式
│   └── alternative-tools.md # D2/PlantUML/Structurizr/Kroki
├── templates/             # 9个可复用 Mermaid 模板
├── README.md
└── LICENSE
```

---

## License

MIT
