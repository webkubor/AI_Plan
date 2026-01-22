# AI_Plan

一个集成了大语言模型（LLM）、检索增强生成（RAG）和 MCP 协议的 AI 智能规划系统。

## 📖 项目简介

AI_Plan 是一个功能强大的 AI 智能规划平台，旨在提供智能化的任务规划、知识检索和自动化执行能力。通过整合先进的 LLM 技术和 RAG 系统，能够实现高效的知识管理和智能决策支持。

## ✨ 核心特性

- **大语言模型集成**：支持多种 LLM 模型，提供强大的自然语言处理能力
- **检索增强生成（RAG）**：基于知识库的智能检索和内容生成
- **MCP 协议支持**：标准化的模型控制协议，确保系统互操作性
- **模块化架构**：清晰的模块划分，便于扩展和维护
- **智能技能系统**：可配置的技能库，支持自定义任务执行
- **完善的数据库支持**：高效的数据存储和检索机制

## 🚀 快速开始

### 环境要求

- Python 3.8+
- Node.js 16+（如需前端支持）
- Git

### 安装步骤

1. **克隆仓库**
```bash
git clone https://github.com/webkubor/AI_Plan.git
cd AI_Plan
```

2. **安装依赖**
```bash
# 安装 Python 依赖
pip install -r requirements.txt

# 如需前端支持
cd server
npm install
```

3. **配置环境变量**
```bash
cp .env.example .env
# 编辑 .env 文件，填入必要的配置信息
```

4. **初始化数据库**
```bash
python scripts/init_db.py
```

5. **启动服务**
```bash
# 启动后端服务
python server/main.py

# 或使用 Docker
docker-compose up -d
```

## 📁 项目结构

```
AI_Plan/
├── .claude/          # Claude AI 配置文件
├── LLM/              # 大语言模型相关模块
├── RAG/              # 检索增强生成系统
├── mcp/              # MCP 协议实现
├── server/           # 服务器端代码
├── share/            # 共享资源和工具
├── skills/           # 技能库和任务执行
├── 基础/             # 基础组件和工具
├── 数据库/           # 数据库相关文件
├── 运行/             # 运行时配置和脚本
└── README.md         # 项目说明文档
```

## 💡 使用示例

### 基本使用

```python
from AI_Plan import AIPlanner

# 初始化规划器
planner = AIPlanner()

# 执行智能规划
result = planner.plan(
    task="制定一个项目开发计划",
    context="需要包含需求分析、设计、开发和测试阶段"
)

print(result)
```

### RAG 检索

```python
from AI_Plan.RAG import KnowledgeRetriever

# 创建检索器
retriever = KnowledgeRetriever()

# 检索相关知识
docs = retriever.search(
    query="如何优化 LLM 推理性能",
    top_k=5
)

for doc in docs:
    print(f"{doc['title']}: {doc['content']}")
```

## 🔧 配置说明

主要配置文件位于 `server/config/` 目录下：

- `config.yaml` - 主配置文件
- `models.yaml` - 模型配置
- `database.yaml` - 数据库配置

## 🤝 贡献指南

我们欢迎所有形式的贡献！请查看 [CONTRIBUTING.md](CONTRIBUTING.md) 了解详细信息。

### 贡献流程

1. Fork 本仓库
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启 Pull Request

## 📝 开发路线图

- [ ] 支持更多 LLM 模型
- [ ] 优化 RAG 检索性能
- [ ] 增强技能系统
- [ ] 完善文档和示例
- [ ] 添加更多测试用例

## 🐛 问题反馈

如果您发现任何问题或有改进建议，请在 [Issues](https://github.com/your-username/AI_Plan/issues) 中提出。

## 📄 许可证

本项目采用 MIT 许可证 - 查看 [LICENSE](LICENSE) 文件了解详情。

## 👥 维护者

- **webkubor** - *主要开发者* - [GitHub](https://github.com/webkubor)

## 🙏 致谢

感谢所有为本项目做出贡献的开发者！

## 📮 联系方式

- 项目主页: [https://github.com/webkubor/AI_Plan](https://github.com/webkubor/AI_Plan)
- 问题反馈: [https://github.com/webkubor/AI_Plan/issues](https://github.com/webkubor/AI_Plan/issues)

---

⭐ 如果这个项目对你有帮助，请给我们一个 Star！