## Claude Code

Claude Code 是 Anthropic 官方推出的 AI 编程助手 CLI 工具，通过命令行与 Claude 模型交互，提供代码生成、调试、重构等功能。

## 核心优势

- **命令行原生集成**：直接在终端中使用，无需切换窗口
- **多模型支持**：可配置 Claude 3.5 Sonnet、Opus、Haiku 等模型
- **第三方模型兼容**：支持智谱 GLM、DeepSeek 等通过 Anthropic API 兼容接口
- **上下文感知**：自动读取项目文件，理解代码结构
- **MCP 服务器集成**：可连接 GitHub、数据库等外部服务

## 局限性

- **配置封闭**：Claude Code 目前没有官方的 `.cursorrules` 或 `.aider.conf` 类似的自动挂载文件机制。它设计得比较封闭，所有配置必须通过环境变量或 `~/.claude/settings.json` 手动设置
- **无项目级配置**：无法在项目根目录放置配置文件自动加载，每个开发者需要独立配置环境
- **扩展性受限**：不支持自定义插件系统，功能扩展依赖 MCP 服务器生态
- **上下文管理**：虽然支持 `--add-dir`，但无法像 Cursor 那样通过 `.cursorrules` 定义项目特定的代码风格和规则

## 安装（macOS）

### 方式一：NPM 安装（推荐）

```bash
npm install -g @anthropic-ai/claude-code
```

### 方式二：Homebrew 安装

```bash
brew install claude-code
```

### 验证安装

```bash
claude --version
```

## 配置第三方模型（智谱 GLM）

### 环境变量配置

在 `~/.zshrc` 或 `~/.bashrc` 中添加：

```bash
# 智谱 GLM API 配置
export ANTHROPIC_AUTH_TOKEN="你的智谱API密钥"
export ANTHROPIC_BASE_URL="https://open.bigmodel.cn/api/anthropic"
export ANTHROPIC_DEFAULT_HAIKU_MODEL="glm-4.7"
export ANTHROPIC_DEFAULT_SONNET_MODEL="glm-4.7"
export ANTHROPIC_DEFAULT_OPUS_MODEL="glm-4.7"
```

### 重新加载配置

```bash
source ~/.zshrc
```

### 获取智谱 API 密钥

1. 访问 [智谱 AI 开放平台](https://open.bigmodel.cn/)
2. 注册/登录账号
3. 获取 API Key
4. GLM Coding Plan 专享优惠：[https://www.bigmodel.cn/claude-code?ic=RRVJPB5SII](https://www.bigmodel.cn/claude-code?ic=RRVJPB5SII)

## 基础使用

### 启动 Claude Code

```bash
claude
```

### 常用命令

在 Claude Code 会话中：

- `/model` - 查看或切换当前模型
- `/status` - 查看当前状态和配置
- `/clear` - 清除对话历史
- `/exit` - 退出 Claude Code

### 命令行参数

```bash
# 指定模型启动
claude --model claude-3-5-sonnet-20241022

# 添加额外目录到上下文
claude --add-dir /path/to/other/project

# 打印模式（用于脚本）
claude -p "检查代码风格"
```

## 使用外部环境

### Claude Vibe 启动模式

通过自定义 alias 和 `-p` 参数，可以让 Claude Code 在启动时自动加载外部知识库和上下文环境。

**配置 Vibe Coding 启动模式**

在 `~/.zshrc` 或 `~/.bashrc` 中添加：

```bash
# Claude Vibe 启动模式
alias claude-vibe='claude -p "请立即阅读 ~/Documents/AI_Common/index.md 并初始化 Vibe Coding 上下文。"'
```

**使用方法**

```bash
# 重新加载配置
source ~/.zshrc

# 启动 Vibe Coding 模式
claude-vibe
```

### 自定义启动模式

**多环境配置**

```bash
# 开发环境
alias claude-dev='claude -p "请阅读 ~/Documents/AI_Common/dev.md 并初始化开发环境上下文。"'

# 生产环境
alias claude-prod='claude -p "请阅读 ~/Documents/AI_Common/prod.md 并初始化生产环境上下文。"'

# 调试模式
alias claude-debug='claude -p "请阅读 ~/Documents/AI_Common/debug.md 并初始化调试环境上下文。"'
```

**项目特定配置**

```bash
# 前端项目
alias claude-frontend='claude --add-dir ~/projects/frontend -p "请阅读 ~/Documents/AI_Common/frontend.md 并初始化前端开发上下文。"'

# 后端项目
alias claude-backend='claude --add-dir ~/projects/backend -p "请阅读 ~/Documents/AI_Common/backend.md 并初始化后端开发上下文。"'

# 全栈项目
alias claude-fullstack='claude --add-dir ~/projects/frontend --add-dir ~/projects/backend -p "请阅读 ~/Documents/AI_Common/fullstack.md 并初始化全栈开发上下文。"'
```

### 外部知识库集成

**知识库结构示例**

```
~/Documents/AI_Common/
├── index.md              # 主索引文件
├── dev.md                # 开发环境配置
├── prod.md               # 生产环境配置
├── coding-standards.md   # 代码规范
├── project-structure.md  # 项目结构说明
└── tech-stack.md         # 技术栈文档
```

**index.md 示例内容**

```markdown
# AI_Common 知识库索引

## 开发环境
- 参考 dev.md 了解开发环境配置

## 代码规范
- 参考 coding-standards.md 了解代码风格要求

## 项目结构
- 参考 project-standards.md 了解项目组织方式

## 技术栈
- 参考 tech-stack.md 了解使用的技术和框架
```

### 高级用法

**组合多个外部资源**

```bash
alias claude-full='claude -p "请依次阅读以下文件并初始化完整上下文：
1. ~/Documents/AI_Common/index.md
2. ~/Documents/AI_Common/coding-standards.md
3. ~/Documents/AI_Common/project-structure.md
4. ~/Documents/AI_Common/tech-stack.md"'
```

**动态上下文加载**

```bash
# 根据当前目录自动加载对应配置
function claude-auto() {
  local config_file=""
  if [[ -f ".claude-context.md" ]]; then
    config_file=".claude-context.md"
  elif [[ -f "~/Documents/AI_Common/default.md" ]]; then
    config_file="~/Documents/AI_Common/default.md"
  fi
  
  if [[ -n "$config_file" ]]; then
    claude -p "请阅读 $config_file 并初始化上下文。"
  else
    claude
  fi
}
```

**环境变量驱动的配置**

```bash
# 根据环境变量选择不同的上下文
alias claude-env='claude -p "请阅读 ~/Documents/AI_Common/${CLAUDE_ENV:-dev}.md 并初始化上下文。"'

# 使用示例
export CLAUDE_ENV=prod
claude-env  # 加载生产环境配置
```

## 实用技巧

### 1. 上下文管理

**添加项目目录**

```bash
# 启动时添加
claude --add-dir ../backend-api

# 会话中添加
/add-dir ../backend-api
```

**限制上下文范围**

```bash
# 只关注特定文件
claude --file src/utils.ts

# 排除目录
claude --ignore node_modules --ignore .git
```

### 2. 高效提示词

**代码生成**

```
创建一个 React 组件，实现用户登录表单，包含：
- 用户名/邮箱输入
- 密码输入（带显示/隐藏切换）
- 记住我复选框
- 表单验证
```

**代码重构**

```
重构以下代码，提高可读性和性能：
[粘贴代码]

要求：
- 使用现代 JavaScript 特性
- 添加错误处理
- 保持功能不变
```

**调试辅助**

```
这段代码在处理大数据时性能很差，帮我分析瓶颈并优化：
[粘贴代码]
```

### 3. 模型选择策略

**Haiku（快速响应）**
- 简单代码生成
- 快速语法检查
- 小型重构任务

**Sonnet（平衡选择）**
- 日常开发任务
- 中等复杂度重构
- 代码审查

**Opus（复杂推理）**
- 架构设计
- 复杂算法实现
- 深度调试

### 4. 多项目协作

```bash
# 同时引用前后端项目
claude --add-dir ../frontend --add-dir ../backend

# 在会话中切换工作目录
/cd ../backend
```

### 5. MCP 服务器集成

**配置 GitHub MCP**

在 `~/.claude/settings.json` 中添加：

```json
{
  "mcpServers": {
    "github": {
      "url": "https://api.githubcopilot.com/mcp",
      "authType": "oauth2",
      "clientId": "your-client-id",
      "clientSecret": "your-client-secret"
    }
  }
}
```

### 6. 成本优化

**使用 Prompt Caching**

Claude Code 自动启用 prompt caching，减少重复上下文的 API 调用成本。

**模型降级策略**

```bash
# 设置使用阈值
export ANTHROPIC_DEFAULT_SONNET_MODEL="glm-4.7"
export ANTHROPIC_DEFAULT_OPUS_MODEL="glm-4.7"
```

### 7. 工作流集成

**Git Hook 集成**

```bash
# pre-commit hook
#!/bin/bash
claude -p "检查暂存文件的代码风格" --git-staged
```

**CI/CD 集成**

```yaml
# GitHub Actions
- name: Code Review with Claude
  run: |
    claude -p "审查 PR 中的代码变更" --pr ${{ github.event.number }}
```

## 高级配置

### 自定义模型映射

在 `~/.claude/settings.json` 中：

```json
{
  "model": {
    "haiku": "glm-4.7",
    "sonnet": "glm-4.7",
    "opus": "glm-4.7"
  }
}
```

### 环境变量完整列表

```bash
# API 配置
export ANTHROPIC_AUTH_TOKEN="your-api-key"
export ANTHROPIC_BASE_URL="https://api.anthropic.com"

# 模型配置
export ANTHROPIC_DEFAULT_HAIKU_MODEL="claude-3-5-haiku-20241022"
export ANTHROPIC_DEFAULT_SONNET_MODEL="claude-3-5-sonnet-20241022"
export ANTHROPIC_DEFAULT_OPUS_MODEL="claude-3-5-opus-20241022"

# 超时配置
export API_TIMEOUT_MS=300000

# 禁用 Prompt Caching
export DISABLE_PROMPT_CACHING=1
```

## 常见问题

### Q: 如何切换回官方 Claude 模型？

A: 修改环境变量：

```bash
export ANTHROPIC_BASE_URL="https://api.anthropic.com"
export ANTHROPIC_AUTH_TOKEN="你的官方API密钥"
```

### Q: 如何查看当前使用的模型？

A: 在 Claude Code 中运行 `/status` 命令。

### Q: 支持哪些编程语言？

A: Claude Code 支持所有主流编程语言，包括 JavaScript、TypeScript、Python、Go、Rust、Java 等。

### Q: 如何提高响应速度？

A:
1. 使用 Haiku 模型处理简单任务
2. 限制上下文范围
3. 使用 `--file` 参数只关注特定文件

### Q: 可以离线使用吗？

A: 不可以，Claude Code 需要网络连接调用 API。如需离线使用，可考虑 Ollama 等本地模型方案。

## 与其他 AI 编程工具对比

### Claude Code vs Cursor vs Aider

| 特性 | Claude Code | Cursor | Aider |
|------|-------------|--------|-------|
| **配置方式** | 环境变量 + `~/.claude/settings.json` | `.cursorrules` 项目级配置 | `.aider.conf` 项目级配置 |
| **自动挂载** | ❌ 不支持 | ✅ 支持 | ✅ 支持 |
| **项目级规则** | ❌ 无 | ✅ `.cursorrules` 定义代码风格 | ✅ `.aider.conf` 定义规则 |
| **扩展性** | MCP 服务器 | 插件系统 | Git 集成 |
| **上下文管理** | `--add-dir` 手动添加 | 自动扫描项目 | Git diff 智能选择 |
| **多模型支持** | ✅ Claude + 第三方兼容 | ✅ 多模型切换 | ✅ 多模型切换 |
| **成本** | 按量计费 | 订阅制 | 按量计费 |
| **适用场景** | 命令行重度用户 | IDE 集成开发 | Git 工作流 |

### Claude Code vs Gemini Code Assist vs GitHub Copilot

| 特性 | Claude Code | Gemini Code Assist | GitHub Copilot |
|------|-------------|-------------------|----------------|
| **模型** | Claude 3.5 / GLM | Gemini 1.5 Pro | GPT-4 / Codex |
| **部署方式** | CLI 工具 | IDE 插件 | IDE 插件 |
| **配置灵活性** | 中等（环境变量） | 低（Google Cloud 配置） | 低（GitHub 设置） |
| **项目级配置** | ❌ 不支持 | ❌ 不支持 | ❌ 不支持 |
| **上下文理解** | 手动指定目录 | 自动扫描工作区 | 自动扫描工作区 |
| **代码补全** | ❌ 不支持 | ✅ 支持 | ✅ 支持 |
| **对话式交互** | ✅ 原生支持 | ✅ 支持 | ✅ 支持 |
| **成本** | 按量计费 | Google Cloud 计费 | 订阅制 |
| **离线能力** | ❌ 不支持 | ❌ 不支持 | ❌ 不支持 |

### 选择建议

**选择 Claude Code 如果你：**
- 习惯命令行工作流
- 需要灵活的模型切换（官方 Claude / GLM / DeepSeek）
- 想要通过 MCP 集成外部服务
- 不介意手动配置环境变量

**选择 Cursor 如果你：**
- 需要 `.cursorrules` 项目级配置
- 想要自动化的上下文管理
- 重视 IDE 集成体验
- 需要代码补全功能

**选择 Aider 如果你：**
- 深度使用 Git 工作流
- 需要基于 diff 的智能上下文
- 想要项目级 `.aider.conf` 配置
- 重视代码审查流程

**选择 Gemini Code Assist 如果你：**
- 已经使用 Google Cloud 生态
- 需要 IDE 插件形式
- 想要 Gemini 1.5 Pro 的长上下文能力

**选择 GitHub Copilot 如果你：**
- 已经订阅 GitHub Copilot
- 需要最强的代码补全能力
- 想要与 GitHub 生态深度集成
- 团队统一使用 GitHub 工具链

## 相关资源

- [Claude Code 官方文档](https://code.claude.com/docs)
- [智谱 AI 开放平台](https://open.bigmodel.cn/)
- [Claude Code Router (CCR)](https://github.com/musistudio/claude-code-router)
- [GLM Coding Plan](https://www.bigmodel.cn/claude-code?ic=RRVJPB5SII)
