### Claude Code 项目启动配置指南

本仓库提供 Claude Code 的关键配置、常用子代理、MCP 服务器与斜杠命令示例，便于在现有代码库中快速落地。请先阅读官方文档以获得最新与完整信息：[Claude Code 文档](https://docs.anthropic.com/en/docs/claude-code/overview)。

- 官方账户与配额说明：请参考官方说明以选择适合的付费方案与限额政策（链接随官方更新而更新）。

---

### 🪝 钩子（Hooks）

- STOP 钩子：在响应完成时，通过 macOS Terminal-Notifier 显示桌面通知（需按需安装 Terminal-Notifier）。

---

### 🤖 子代理（Sub-Agents）

- memory-bank-synchronizer（记忆库同步器）
  - 用途：同步记忆库文档与代码库实现，确保架构/模式/规范与实现一致
  - 位置：`.claude/agents/memory-bank-synchronizer.md`
  - 职责：模式文档同步、架构决策更新、技术规范对齐、实现状态跟踪、示例新鲜度验证、交叉引用校验

- code-searcher（代码搜索器）
  - 用途：高效定位函数/类/逻辑与集成点；支持标准模式与 CoD（草稿链）极简模式
  - 位置：`.claude/agents/code-searcher.md`
  - 用法示例：
    - 标准模式：查找并分析“支付处理”相关代码
    - CoD 模式：使用 CoD 进行极简搜索提示（更低令牌）

- get-current-datetime（获取当前日期时间）
  - 用途：返回澳大利亚布里斯班时区（GMT+10）的原始 `date` 输出，支持多格式
  - 位置：`.claude/agents/get-current-datetime.md`

- ux-design-expert（UX 设计专家）
  - 用途：提供系统化 UX/UI 指导、Tailwind CSS 设计系统与 Highcharts 可视化建议
  - 位置：`.claude/agents/ux-design-expert.md`

---

### 🔧 MCP 服务器

以下列出常用的 MCP 服务器与添加示例（以实际环境为准）：

- Gemini CLI MCP
  - 仓库：`https://github.com/centminmod/gemini-cli-mcp-server`
  - 添加示例：
    ```bash
    claude mcp add gemini-cli /path/to/.venv/bin/python /path/to/mcp_server.py -s user -e GEMINI_API_KEY='GEMINI_API_KEY' -e OPENROUTER_API_KEY='OPENROUTER_API_KEY'
    ```

- Cloudflare 文档 MCP
  - 仓库：`https://github.com/cloudflare/mcp-server-cloudflare/tree/main/apps/docs-vectorize`
  - 添加示例：
    ```bash
    claude mcp add --transport sse cf-docs https://docs.mcp.cloudflare.com/sse -s user
    ```

- Context 7 MCP
  - 仓库：`https://github.com/upstash/context7`
  - 添加示例：
    ```bash
    claude mcp add --transport sse context7 https://mcp.context7.com/sse -s user
    ```

- Notion MCP
  - 仓库：`https://github.com/makenotion/notion-mcp-server`
  - 添加示例：
    ```bash
    claude mcp add-json notionApi '{"type":"stdio","command":"npx","args":["-y","@notionhq/notion-mcp-server"],"env":{"OPENAPI_MCP_HEADERS":"{\"Authorization\": \"Bearer ntn_API_KEY\", \"Notion-Version\": \"2022-06-28\"}"}}' -s user
    ```

- Chrome 开发者工具 MCP（按需启用）
  - 仓库：`https://github.com/ChromeDevTools/chrome-devtools-mcp`
  - 说明：该服务器会占用较多上下文窗口，仅当项目需要浏览器自动化/性能分析等功能时再加载。
  - 启用示例：
    ```bash
    claude --mcp-config .claude/mcp/chrome-devtools.json
    ```
  - 配置示例（`.claude/mcp/chrome-devtools.json`）：
    ```json
    {
      "mcpServers": {
        "chrome-devtools": {
          "command": "npx",
          "args": ["-y", "chrome-devtools-mcp@latest"]
        }
      }
    }
    ```

---

### ⌨️ 常用斜杠命令（精选）

- /anthropic
  - /apply-thinking-to：将开放式提示转换为系统化的渐进推理结构
  - /convert-to-todowrite-tasklist-prompt：将复杂提示转换为可并行的 TodoWrite 任务列表
  - /update-memory-bank：更新 `CLAUDE.md` 和记忆库文件

- /ccusage
  - /ccusage-daily：生成 Claude Code 使用成本与统计（结构化 Markdown 输出）

- /cleanup
  - /cleanup-context：优化记忆库，去重、删旧、合并重叠，减少令牌占用

- /documentation
  - /create-readme-section：为 README 生成特定章节（安装/使用/API/贡献等）

- /security
  - /security-audit：按 OWASP 指南进行安全审计并分类问题与修复建议
  - /check-best-practices：按语言/框架最佳实践给出可操作改进建议
  - /secure-prompts：检测提示注入与恶意指令，生成时间戳审计报告

- /architecture
  - /explain-architecture-pattern：识别并解释代码库中的架构模式，含图示与示例

- /promptengineering
  - /convert-to-test-driven-prompt：将请求转换为 TDD 风格提示（Given/When/Then）
  - /batch-operations-prompt：优化多文件操作与并行处理提示

- /refactor
  - /refactor-code：仅分析的重构专家，生成分步重构计划与风险评估

---

### ⚙️ Claude Code 设置要点（精简）

- 设置层级与文件：
  - 用户设置：`~/.claude/settings.json`
  - 项目共享设置：`.claude/settings.json`
  - 项目本地设置（git 忽略）：`.claude/settings.local.json`
  - 优先级（高→低）：企业策略 → 命令行参数 → 项目本地 → 项目共享 → 用户设置

- 常见配置方式：
  - 列出设置：`claude config list`
  - 查看设置：`claude config get <key>`
  - 更改设置：`claude config set <key> <value>`（加 `-g` 为全局）

- 典型键值与用途（示例）：
  - `apiKeyHelper`：在 `/bin/sh` 中执行脚本生成临时鉴权值
  - `env`：为每个会话注入环境变量
  - `permissions`：允许/拒绝的工具权限与额外工作目录等
  - `defaultMode`：默认权限模式

- 环境变量（示例，按需设置）：
  - `ANTHROPIC_API_KEY`、`ANTHROPIC_AUTH_TOKEN`
  - `CLAUDE_CODE_MAX_OUTPUT_TOKENS`、`MAX_THINKING_TOKENS`
  - `HTTP_PROXY`、`HTTPS_PROXY`
  - `MCP_TIMEOUT`、`MCP_TOOL_TIMEOUT`

- 可用工具（概览）：
  - Agent、Bash、Edit、Glob、Grep、LS、MultiEdit、Read、TodoRead、TodoWrite、WebFetch、WebSearch、Write、NotebookRead/NotebookEdit

- 钩子扩展示例：
  - 在修改某类文件后自动运行格式化工具，或阻止写入特定路径的生产配置

---

### 参考与说明

- 更详细的配置、权限、模型与代理集成方式，请以官方文档为准：
  - 文档入口：`https://docs.anthropic.com/en/docs/claude-code/overview`
  - 权限与 IAM：`/en/docs/claude-code/iam`
  - Bedrock/Vertex 代理与模型：`/en/docs/claude-code/bedrock-vertex-proxies`
  - Hooks、工具与命令：`/en/docs/claude-code/hooks`、`/en/docs/claude-code/settings`、`/en/docs/claude-code/commands`
