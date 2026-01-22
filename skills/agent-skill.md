# AI 工具当前已安装能力统计

https://skillsmp.com/zh(汇总)

https://github.com/anthropics/skills（openAI）

## 说明

本文档记录当前各 AI 工具已安装且正在使用的能力。
能力来源分为两类：
1.  **继承自公共大脑 (Inherited)**：在 `AI_Common/extensions/` 有明确的对应定义文档，属于跨模型通用的标准能力。
2.  **原生/专家能力 (Native)**：模型特有的工具特性、领域专长，或仅在本地维护的特定脚本。

---

## 🟢 Gemini (全栈工程与调试)

### 1. 来源一：继承自公共大脑
| 技能名称 | 对应公共定义 | 说明 |
| :--- | :--- | :--- |
| **`deep-thinker`** | `think.md` | 苏格拉底式深度思考，提供多维架构分析。（对应公共定义的架构师/思考者模式） |
| **`milvus`** | `milvus-toolkit.md` | 向量数据库标准操作工具箱。 |
| **`juejin-writer`** | `juejin-writer.md` | 掘金技术文章自动写作与发布。 |
| **`feishu-writer`** | `feishu-writer.md` | 飞书团队文档生成与知识库协作。 |

### 2. 来源二：Gemini 原生/插件能力
| 技能名称 | 领域 | 说明 |
| :--- | :--- | :--- |
| **`chrome-devtools-mcp`** | **调试** | 控制 Chrome 浏览器，进行 DOM 检查、网络抓包与自动化测试。 |
| **`context7`** | **文档** | 实时检索第三方官方技术文档（Context7 协议）。 |

---

## 🔵 Codex (UI 设计与前端专精)

### 1. 来源一：继承自公共大脑
| 技能名称 | 对应公共定义 | 说明 |
| :--- | :--- | :--- |
| **`socratic-think`** | `think.md` | 实现了公共大脑定义的深度思考协议。 |
| **`milvus`** | `milvus-toolkit.md` | 向量数据库日常 CRUD 操作。 |

### 2. 来源二：Codex 原生/专家能力
| 技能名称 | 领域 | 说明 |
| :--- | :--- | :--- |
| **`milvus-setup`** | **运维** | (本地脚本) 快速部署 Milvus Docker 环境与依赖配置。 |
| **`theme-factory`** | **UI 设计** | 自动生成配色方案、Design Tokens 和主题配置。 |
| **`canvas-design`** | **UI 设计** | 针对 Canvas 场景的绘图算法与布局辅助。 |
| **`frontend-design`** | **前端** | 生成复杂的前端交互组件代码与逻辑。 |
| **`init-ui-pro-skill`** | **工程化** | 初始化高阶 UI 项目脚手架（Pro 级配置）。 |
| **`start`** | **系统** | Codex 环境引导与技能加载器。 |

---

## 🔄 更新记录
- **更新日期**: 2026-01-19
- **修正**: 移除了不再使用的 `minio-login` 和 `wechat-release`。
- **调整**: 将 `milvus-setup` 归类为 Codex 原生运维能力（因为它更偏向具体的 Docker 脚本实现，而非公共定义的通用数据操作）。
