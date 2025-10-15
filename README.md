### Claude Code 项目启动配置指南

本仓库提供 Claude Code 的关键配置、常用子代理、MCP 服务器与斜杠命令示例，便于在现有代码库中快速落地。请先阅读官方文档以获得最新与完整信息：[Claude Code 文档](https://docs.anthropic.com/en/docs/claude-code/overview)。

- 官方账户与配额说明：请参考官方说明以选择适合的付费方案与限额政策（链接随官方更新而更新）。

---

### 🪝 钩子（Hooks）

- STOP 钩子：在响应完成时，通过 macOS Terminal-Notifier 显示桌面通知（需按需安装 Terminal-Notifier）。

---

### 🤖 子代理（Sub-Agents）

- code-searcher（代码搜索器）
  - 用途：高效定位函数/类/逻辑与集成点；支持标准模式与 CoD（草稿链）极简模式
  - 位置：`.claude/agents/code-searcher.md`

- get-current-datetime（获取当前日期时间）
  - 用途：返回澳大利亚布里斯班时区（GMT+10）的原始 `date` 输出，支持多格式
  - 位置：`.claude/agents/get-current-datetime.md`

- uv-env-manager（uv 环境管理）
  - 用途：基于 uv 管理 Python 环境与依赖，适配 Django/DRF 工作流与 CI 缓存
  - 位置：`.claude/agents/uv-env-manager.md`

- openapi-contract-sync（OpenAPI 契约同步）
  - 用途：配置 drf-spectacular 生成 Schema，校验契约质量并生成前端类型与客户端
  - 位置：`.claude/agents/openapi-contract-sync.md`

- docker-compose-orchestrator（Docker Compose 编排）
  - 用途：为后端/前端/数据库生成并维护 compose 配置（profiles、healthcheck、.env）
  - 位置：`.claude/agents/docker-compose-orchestrator.md`

- spec-driven-planner（Prompt 优化 / 规格驱动）
  - 用途：将模糊需求转换为可实施的规范文档（Overview、Goals、User Stories、Domain Model、API/UI/Backend/Data/Security/Perf/Deploy 等）；带反思门控（清晰度/完整性/可行性/接口）
  - 维护模式：支持 **Debug/Bugfix/Improvements**，包含故障复现与分级、根因分析、补丁计划、测试与回滚方案，并为下游代理生成对应的执行提示
  - 位置：`.claude/agents/spec-driven-planner.md`

- doc-analysis（文档分析）
  - 用途：扫描 `docs/` 文件夹，提取需求、API、数据模型与约束，生成可追溯的知识图谱与任务提示，辅助 `fullstack-dev`、`openapi-contract-sync`、`debugging-agent`、`spec-driven-planner` 等实现
  - 位置：`.claude/agents/doc-analysis.md`

- fullstack-dev（全栈开发）
  - 用途：统筹 Django/DRF + Vue 3 + MySQL 的端到端交付，含 OpenAPI 契约与 Docker Compose 运维
  - 位置：`.claude/agents/fullstack-dev.md`

- debugging-agent（调试/修复）
  - 用途：复现问题、根因分析、补丁计划与测试方案，协调后端/前端/契约/运维的一致性修复
  - 位置：`.claude/agents/debugging-agent.md`

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
