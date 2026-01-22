# 技术分享：从 openCode 到 AionUI —— 构建全栈 AI 协作工作流

**演讲人**：WebKubor
**日期**：2026-01-21
**主题**：工具演进、MCP 协议实战、本地模型集成

---

## 1. 开场：工具的进化论

大家好！今天想和大家聊聊我最近在 AI 辅助开发工具链上的探索与演进。

如果在座的各位和我一样，是一个喜欢折腾工具的前端工程师，可能经历过这样的阶段：
*   **阶段一**：用 ChatGPT/Claude 网页版，复制粘贴代码。
*   **阶段二**：使用 `openCode` 这样的 CLI 或者 GUI工具，在终端里让 AI 帮我们写脚本、改 bug。这就像雇了一个**“自动化包工头”**，它很听话，但它是个“瞎子”，只能看代码，看不见运行效果。

而今天，我想介绍的是 **阶段三**：**AionUI**。
它不再是一个黑漆漆的终端窗口，而是一个**“超级浏览器”**。它不仅能由 AI 驱动，还能通过 **MCP (Model Context Protocol)** 连接万物，甚至能把昂贵的云端大脑换成我们在座每一位电脑里都能跑的 **本地模型 (Ollama)**。

---

## 2. 核心演进：从 CLI 到 GUI 的质变

### 2.1 为什么我们需要 AionUI？

> *“运行在终端里的自动化包工头” -> “能接各种 AI 模型的超级浏览器”*

*   **痛点**：在 `openCode` 时代，我们面临的最大问题是**上下文缺失**。CLI 无法直观地看到网页渲染出的 DOM 结构、样式错乱或是交互动效。
*   **解决**：AionUI 将 AI 植入浏览器环境。
    *   **可视化调试**：AI 可以“看到”网页（通过截图或 DOM 树），不再是盲写代码。
    *   **所见即所得**：修复样式的同时，直接在右侧窗口看到变化。

*(参考：[掘金文章 - 从 openCode 到 AionUI](https://juejin.cn/post/7597346583114481690))*

---

## 3. 能力扩展：PlaywrightMcp 与标准协议

浏览器只是躯壳，**操作能力**才是灵魂。如何让 AI 像测试工程师一样点击按钮、输入表单？

### 3.1 引入 MCP (Model Context Protocol)
MCP 是 Anthropic 推出的一套标准，它解决了“模型如何使用工具”的标准化问题。

### 3.2 实战 PlaywrightMcp
我们不需要自己写复杂的 Puppeteer 脚本，只需要配置一个 MCP Server。

**配置示例 (`config.json` / `package.json`)**:
```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": [
        "@playwright/mcp@latest"
      ]
    }
  }
}
```

**效果**：
配置完成后，AI 自动获得了以下能力：
*   `navigate(url)`: 打开网页
*   `click(selector)`: 精准点击
*   `fill(selector, text)`: 自动填表
*   `screenshot()`: 视觉验证

这让自动化测试、数据抓取变得前所未有的简单。

---

## 4. 掌控力：本地模型 (Ollama) 的无缝接入

云端模型（GPT-4, Claude 3.5）虽然强，但有三个问题：**贵、慢、隐私担忧**。
AionUI 的强大之处在于它的**模型无关性**。

### 4.1 架构原理：The OpenAI Compatible Adapter
AionUI 支持“自定义平台”，只要符合 OpenAI 的 API 格式即可。而 Ollama 完美支持这一点。

### 4.2 配置步骤（保姆级教程）

**第一步：后端启动 (Ollama)**
在终端运行你想要的模型（确保服务运行在 11434 端口）：

```bash
ollama run gpt-oss:20b  # 或者 deepseek-coder
```

**第二步：前端配置 (AionUI)**
进入 `Settings` -> `Model Settings` -> `Add Model`：

1.  **Platform**: 选择 `Custom Platform`
2.  **Base URL**: 填入 `http://localhost:11434/v1` (关键！)
3.  **API Key**: 随便填（例如 `ollama`），因为本地服务不校验 Key，但前端校验必填。
4.  **Model Name**: 填入 `gpt-oss:20b` (需与本地 pull 的模型一致)。

### 4.3 代码验证
如果你是 Python 开发者，原理是一样的：
```python
from openai import OpenAI

client = OpenAI(
    base_url='http://localhost:11434/v1/',
    api_key='ollama',
)

# 像调用 GPT-4 一样调用本地模型
response = client.chat.completions.create(
    model='gpt-oss:20b',
    messages=[{'role': 'user', 'content': 'Hello AionUI!'}]
)
```

---

## 5. 总结

从 `openCode` 到 `AionUI`，不仅是工具形态的变化，更是我们**开发范式**的升级：
1.  **交互升级**：从 CLI 盲操作 -> GUI 可视化协作。
2.  **能力升级**：通过 **MCP** 协议，低成本接入 Playwright 等专业工具。
3.  **算力升级**：通过 **Ollama**，实现本地与云端算力的自由切换。

这就是我今天的分享，希望大家都能构建出属于自己的“超级浏览器”工作流。谢谢大家！
