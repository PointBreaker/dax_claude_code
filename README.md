[![GitHub stars](https://img.shields.io/github/stars/centminmod/my-claude-code-setup.svg?style=flat-square)](https://github.com/centminmod/my-claude-code-setup/stargazers) [![GitHub forks](https://img.shields.io/github/forks/centminmod/my-claude-code-setup.svg?style=flat-square)](https://github.com/centminmod/my-claude-code-setup/network) [![GitHub issues](https://img.shields.io/github/issues/centminmod/my-claude-code-setup.svg?style=flat-square)](https://github.com/centminmod/my-claude-code-setup/issues)

* Threads - https://www.threads.com/@george_sl_liu
* BlueSky - https://bsky.app/profile/georgesl.bsky.social

# Claude Code 项目启动配置指南

本仓库提供了 Claude Code 项目的启动设置、钩子函数和斜杠命令配置，供用户参考使用。使用前请务必先阅读官方 Claude Code 文档：https://docs.anthropic.com/en/docs/claude-code/overview，并注册 [Claude AI 付费账户](https://claude.ai/) 以使用 Claude Code。可选择以下付费方案：Claude Pro 20美元/月、Claude Max 100美元/月 或 Claude Max 200美元/月。不同付费方案包含不同的使用配额和速率限制，详见[官方说明](https://support.anthropic.com/en/articles/9797557-usage-limit-best-practices)。

## 🚀 快速开始

1. 将本仓库的文件复制到你的项目目录（即你的代码库所在位置）
2. 根据你的需求修改模板文件和 `CLAUDE.md`。`.claude/settings.json` 需要安装 macOS 的 Terminal-Notifier（https://github.com/centminmod/terminal-notifier-setup）。如果你不使用 macOS，可以删除 `.claude/settings.json`
3. 首次在项目目录中启动 Claude Code 后，运行 `/init` 命令，让 Claude Code 分析你的代码库并根据 `CLAUDE.md` 的说明填充记忆库系统文件
4. **强烈推荐**：安装 Visual Studio Code（[新手视频教程](https://www.www.bilibili.com/video/BV1yW411s7og)）和 [Claude Code VSCode 扩展](https://marketplace.visualstudio.com/items?itemName=anthropic.claude-code)
5. **强烈推荐**：注册 [GitHub 账户](https://github.com/) 并为 VSCode 安装 Git。查看视频教程：[Git 入门教程](https://www.bilibili.com/video/BV1FE411P7B6/)
6. `CLAUDE.md` 已更新，指导模型使用更快的工具。对于 macOS 用户：运行 `brew install ripgrep fd jq`

## 🔧 MCP 服务器配置

我还安装了以下 MCP 服务器（[安装命令](#claude-code-mcp-服务器)）：

* [Gemini CLI MCP](https://github.com/centminmod/gemini-cli-mcp-server)
* [Cloudflare 文档 MCP](https://github.com/cloudflare/mcp-server-cloudflare/tree/main/apps/docs-vectorize)
* [Context 7 MCP](https://github.com/upstash/context7)
* [Chrome 开发者工具 MCP](https://github.com/ChromeDevTools/chrome-devtools-mcp)
* [Notion MCP](https://github.com/makenotion/notion-mcp-server)

## 🪝 Claude Code 钩子函数

Claude Code 的 `STOP` 钩子使用 Terminal-Notifier 在 macOS 桌面显示通知，当 Claude Code 停止并完成响应时触发（https://github.com/centminmod/terminal-notifier-setup）。

## 🤖 Claude Code 子代理

Claude Code 子代理是专门设计用于自主处理复杂多步骤任务的工具。其主要优势在于使用独立的上下文窗口，并可以使用自定义提示。了解更多[子代理官方文档](https://docs.anthropic.com/en/docs/claude-code/sub-agents)。

### memory-bank-synchronizer（记忆库同步器）

- **用途**：同步记忆库文档与实际代码库状态，确保内存文件中的架构模式与实现保持一致
- **位置**：`.claude/agents/memory-bank-synchronizer.md`
- **主要职责**：
  - 模式文档同步
  - 架构决策更新
  - 技术规范对齐
  - 实现状态跟踪
  - 代码示例新鲜度验证
  - 交叉引用验证
- **使用**：主动维护 CLAUDE-*.md 文件与源代码之间的一致性，确保文档保持准确可信

### code-searcher（代码搜索器）

- **用途**：专门用于高效搜索代码库、查找相关文件和总结代码的代理。支持标准详细分析和可选的[草稿链 (CoD)](https://github.com/centminmod/or-cli/blob/master/examples/example-code-inspection-prompts3.md)超简洁模式，在明确请求时可减少 80% 的令牌使用
- **位置**：`.claude/agents/code-searcher.md`
- **主要职责**：
  - 高效的代码库导航和搜索
  - 函数和类定位
  - 代码模式识别
  - 错误源定位协助
  - 功能实现分析
  - 集成点发现
  - 草稿链 (CoD) 模式，使用最少的令牌进行超简洁推理
- **使用**：需要在代码库中定位特定函数、类或逻辑时使用。请求 "使用 CoD"、"草稿链" 或 "草稿模式" 可获得减少约 80% 令牌的超简洁响应
  - **标准模式**："查找支付处理代码" → 完整详细分析
  - **CoD 模式**："使用 CoD 查找支付处理代码" → "支付→glob:*payment*→found:payment.service.ts:45"

### get-current-datetime（获取当前日期时间）

- **用途**：简单的日期时间工具，提供准确的澳大利亚布里斯班时区 (GMT+10) 值。执行 bash date 命令并仅返回原始输出，无需格式化或解释
- **位置**：`.claude/agents/get-current-datetime.md`
- **主要职责**：
  - 执行 `TZ='Australia/Brisbane' date` 命令
  - 提供准确的布里斯班时区时间戳
  - 支持多种格式选项（默认、文件名、可读、ISO）
  - 消除时区混淆和月份错误
  - 返回原始命令输出，无需额外处理
- **使用**：创建带时间戳的文件、生成带日期的报告或需要准确的澳大利亚时区值时使用

### ux-design-expert（UX设计专家）

- **用途**：全面的 UX/UI 设计指导专家，结合用户体验优化、高级界面设计和可扩展设计系统，使用 Tailwind CSS 和 Highcharts 数据可视化
- **位置**：`.claude/agents/ux-design-expert.md`
- **主要职责**：
  - UX 流程优化和摩擦减少
  - 使用复杂视觉层次结构的高级 UI 设计
  - 使用 Tailwind CSS 的可扩展设计系统架构
  - 使用 Highcharts 实现的数据可视化策略
  - 可访问性合规性和性能优化
  - 使用原子方法论的组合库设计
- **使用**：用于仪表板 UX 改进、高级组件库、复杂用户流程优化、设计系统创建或任何全面的 UX/UI 设计指导需求

## ⌨️ Claude Code 斜杠命令

### `/anthropic` 命令

- **`/apply-thinking-to`** - 专家提示工程专家，应用 Anthropic 的扩展思维模式，使用高级推理框架增强提示
  - 使用渐进式推理结构转换提示（开放式→系统化）
  - 应用顺序分析框架和系统验证与测试用例
  - 包括约束优化、偏见检测和扩展思维预算管理
  - 用法：`/apply-thinking-to @/path/to/prompt-file.md`

- **`/convert-to-todowrite-tasklist-prompt`** - 将复杂的、上下文繁重的提示转换为高效的 TodoWrite 任务列表方法，具有并行子代理执行
  - 通过并行处理实现 60-70% 的速度提升
  - 将冗长的工作流程转换为专门的任务委托
  - 通过战略性文件选择防止上下文溢出（每个任务最多 5 个文件）
  - 用法：`/convert-to-todowrite-tasklist-prompt @/path/to/original-slash-command.md`

- **`/update-memory-bank`** - 简单的命令来更新 CLAUDE.md 和记忆库文件
  - 用法：`/update-memory-bank`

### `/ccusage` 命令

- **`/ccusage-daily`** - 生成全面的 Claude Code 使用成本分析和统计
  - 运行 `ccusage daily` 命令并将输出解析为结构化 markdown
  - 提供执行摘要，包括总成本、高峰使用日和缓存效率
  - 创建详细表格，显示每日成本、令牌使用和模型统计
  - 包括使用洞察、建议和成本管理分析
  - 用法：`/ccusage-daily`

### `/cleanup` 命令

- **`/cleanup-context`** - 记忆库优化专家，减少文档中的令牌使用
  - 删除重复内容并消除过时文件
  - 整合重叠文档，同时保留基本信息
  - 实施历史文档的存档策略
  - 通过系统优化实现 15-25% 的令牌减少
  - 用法：`/cleanup-context`

### `/documentation` 命令

- **`/create-readme-section`** - 为 README 文件生成特定部分，具有专业格式
  - 创建结构良好的部分，如安装、使用、API 参考、贡献等
  - 遵循 markdown 最佳实践，具有适当的标题、代码块和格式
  - 分析项目上下文以提供相关内容
  - 匹配现有 README 的风格和语气
  - 用法：`/create-readme-section "为我的 Python 项目创建安装部分"`

### `/security` 命令

- **`/security-audit`** - 对代码库进行全面的安全审计
  - 使用 OWASP 指南识别潜在漏洞
  - 检查身份验证、输入验证、数据保护和 API 安全
  - 按严重程度（严重、高、中、低）分类问题
  - 提供具体的修复步骤和代码示例
  - 用法：`/security-audit`

- **`/check-best-practices`** - 根据特定语言的最佳实践分析代码
  - 检测语言和框架以应用相关标准
  - 检查命名约定、代码组织、错误处理和性能
  - 提供可操作的反馈，包括前后代码示例
  - 优先处理有影响的改进，而不是吹毛求疵
  - 用法：`/check-best-practices`

- **`/secure-prompts`** - 企业级安全分析器，用于检测提示注入攻击和恶意指令
  - 使用先进的 AI 特定检测模式检测提示注入攻击、隐藏内容和恶意指令
  - 提供全面的威胁分析和自动时间戳报告生成
  - 将报告保存到 `reports/secure-prompts/` 目录以供审计跟踪
  - 分析文件内容和直接文本输入的安全威胁
  - 用法：`/secure-prompts @suspicious_file.txt` 或 `/secure-prompts "要分析的内容"`
  - 示例提示注入提示位于 `.claude/commands/security/test-examples`，你可以运行 `/secure-prompts` 进行测试
  - 示例生成的报告：`/secure-prompts .claude/commands/security/test-examples/test-encoding-attacks.md` [查看这里](reports/secure-prompts/security-analysis_20250719_072359.md)

### `/architecture` 命令

- **`/explain-architecture-pattern`** - 识别和解释代码库中的架构模式
  - 分析项目结构并识别设计模式
  - 解释架构决策背后的原理
  - 提供图表的视觉表示
  - 显示具体的实现示例
  - 用法：`/explain-architecture-pattern`

### `/promptengineering` 命令

- **`/convert-to-test-driven-prompt`** - 将请求转换为测试驱动开发风格的提示
  - 使用 Given/When/Then 格式定义明确的测试用例
  - 包括成功标准和边缘情况
  - 为红-绿-重构循环构建提示
  - 创建可测量的、具体的测试场景
  - 用法：`/convert-to-test-driven-prompt "添加用户身份验证功能"`

- **`/batch-operations-prompt`** - 优化提示以进行多个文件操作和并行处理
  - 识别可并行化的任务以最大化效率
  - 按冲突潜力对操作进行分组
  - 与 TodoWrite 集成以进行任务管理
  - 在批处理操作之间包括验证步骤
  - 用法：`/batch-operations-prompt "更新所有 API 调用以使用新的身份验证标头"`

### `/refactor` 命令

- **`/refactor-code`** - 仅分析的重构专家，创建全面的重构计划而不修改代码
  - 分析代码复杂性、测试覆盖率和架构模式
  - 识别安全的提取点和重构机会
  - 创建详细的逐步重构计划，包括风险评估
  - 在 `reports/refactor/` 目录中生成时间戳报告
  - 专注于安全性、增量进度和可维护性
  - 用法：`/refactor-code`

## 📊 Claude Code 付费方案周限制

如果你使用 Claude 月度订阅计划，从 2025 年 8 月 28 日起将适用新的周限制，以及每月最多 50 次 5 小时会话限制：

| 方案               | Sonnet 4 (小时/周) | Opus 4 (小时/周) |
|--------------------|---------------------|-------------------|
| Pro (20美元/月)    | 40–80               | –                 |
| Max (100美元/月)   | 140–280             | 15–35             |
| Max (200美元/月)   | 240–480             | 24–40             |

## ⚙️ Claude Code 设置

\u003e 使用全局和项目级设置以及环境变量配置 Claude Code。

Claude Code 提供各种设置来配置其行为以满足你的需求。在使用交互式 REPL 时可以运行 `/config` 命令来配置 Claude Code。

### 设置文件

`settings.json` 文件是我们通过分层设置配置 Claude Code 的官方机制：

* **用户设置** 在 `~/.claude/settings.json` 中定义，适用于所有项目
* **项目设置** 保存在你的项目目录中：
  * `.claude/settings.json` 用于纳入源代码控制并与团队共享的设置
  * `.claude/settings.local.json` 用于不纳入源代码控制的设置，适用于个人偏好和实验。Claude Code 会在创建时配置 git 忽略 `.claude/settings.local.json`

#### 可用设置

`settings.json` 支持多种选项：

| 键                   | 描述                                                                                                                                                                                                    | 示例                         |
| :-------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------ |
| `apiKeyHelper`        | 自定义脚本，在 `/bin/sh` 中执行，生成身份验证值。此值通常作为 `X-Api-Key`、`Authorization: Bearer` 和 `Proxy-Authorization: Bearer` 头部发送给模型请求 | `/bin/generate_temp_api_key.sh` |
| `cleanupPeriodDays`   | 本地保留聊天记录的时间（默认：30 天）                                                                                                                                                 | `20`                            |
| `env`                 | 将应用于每个会话的环境变量                                                                                                                                                    | `{\"FOO\": \"bar\"}`                |
| `includeCoAuthoredBy` | 是否在 git 提交和拉取请求中包含 `co-authored-by Claude` 署名（默认：`true`）                                                                                                       | `false`                         |
| `permissions`         | 权限结构见下表                                                                                                                                                                  |                                 |

#### 权限设置

| 键                           | 描述                                                                                                                                        | 示例                          |
| :----------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------- |
| `allow`                        | 允许工具使用的[权限规则](/en/docs/claude-code/iam#configuring-permissions)数组                                                    | `[ \"Bash(git diff:*)\" ]`         |
| `deny`                         | 拒绝工具使用的[权限规则](/en/docs/claude-code/iam#configuring-permissions)数组                                                     | `[ \"WebFetch\", \"Bash(curl:*)\" ]` |
| `additionalDirectories`        | Claude 可以访问的额外[工作目录](iam#working-directories)                                                                | `[ \"../docs/\" ]`                 |
| `defaultMode`                  | 打开 Claude Code 时的默认[权限模式](iam#permission-modes)                                                                           | `\"acceptEdits\"`                  |
| `disableBypassPermissionsMode` | 设置为 `\"disable\"` 以防止激活 `bypassPermissions` 模式。见[托管策略设置](iam#enterprise-managed-policy-settings) | `\"disable\"`                      |

#### 设置优先级

设置按以下优先顺序应用：

1. 企业策略（见 [IAM 文档](/en/docs/claude-code/iam#enterprise-managed-policy-settings)）
2. 命令行参数
3. 本地项目设置
4. 共享项目设置
5. 用户设置

### 环境变量

Claude Code 支持以下环境变量来控制其行为：

\u003cNote\u003e
  所有环境变量也可以在 [`settings.json`](#可用设置) 中配置。这有助于自动为每个会话设置环境变量，或为整个团队或组织推出一组环境变量。
\u003c/Note\u003e

| 变量                                   | 用途                                                                                                                                |
| :----------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------- |
| `ANTHROPIC_API_KEY`                        | 作为 `X-Api-Key` 头部发送的 API 密钥，通常用于 Claude SDK（交互式使用，运行 `/login`）                                 |
| `ANTHROPIC_AUTH_TOKEN`                     | `Authorization` 和 `Proxy-Authorization` 头部的自定义值（你在此处设置的值将以 `Bearer ` 为前缀）        |
| `ANTHROPIC_CUSTOM_HEADERS`                 | 要添加到请求中的自定义头部（格式为 `Name: Value`）                                                                |
| `ANTHROPIC_MODEL`                          | 要使用的自定义模型名称（见[模型配置](/en/docs/claude-code/bedrock-vertex-proxies#model-configuration)）               |
| `ANTHROPIC_SMALL_FAST_MODEL`               | 用于后台任务的 [Haiku 类模型](/en/docs/claude-code/costs)                                                           |
| `ANTHROPIC_SMALL_FAST_MODEL_AWS_REGION`    | 使用 Bedrock 时覆盖小/快速模型的 AWS 区域                                                                        |
| `BASH_DEFAULT_TIMEOUT_MS`                  | 长时间运行的 bash 命令的默认超时                                                                                         |
| `BASH_MAX_TIMEOUT_MS`                      | 模型可以为长时间运行的 bash 命令设置的最大超时                                                                       |
| `BASH_MAX_OUTPUT_LENGTH`                   | bash 输出在被中间截断之前的最大字符数                                                          |
| `CLAUDE_BASH_MAINTAIN_PROJECT_WORKING_DIR` | 每个 bash 命令后返回原始工作目录                                                                       |
| `CLAUDE_CODE_API_KEY_HELPER_TTL_MS`        | 使用 `apiKeyHelper` 时凭据刷新的间隔（毫秒）                                          |
| `CLAUDE_CODE_IDE_SKIP_AUTO_INSTALL`        | 跳过 IDE 扩展的自动安装（默认为 false）                                                                           |
| `CLAUDE_CODE_MAX_OUTPUT_TOKENS`            | 设置大多数请求的最大输出令牌数                                                                              |
| `CLAUDE_CODE_USE_BEDROCK`                  | 使用 [Bedrock](/en/docs/claude-code/amazon-bedrock)                                                                                     |
| `CLAUDE_CODE_USE_VERTEX`                   | 使用 [Vertex](/en/docs/claude-code/google-vertex-ai)                                                                                    |
| `CLAUDE_CODE_SKIP_BEDROCK_AUTH`            | 跳过 Bedrock 的 AWS 身份验证（例如，使用 LLM 网关时）                                                                   |
| `CLAUDE_CODE_SKIP_VERTEX_AUTH`             | 跳过 Vertex 的 Google 身份验证（例如，使用 LLM 网关时）                                                                 |
| `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` | 等效于设置 `DISABLE_AUTOUPDATER`、`DISABLE_BUG_COMMAND`、`DISABLE_ERROR_REPORTING` 和 `DISABLE_TELEMETRY`                 |
| `DISABLE_AUTOUPDATER`                      | 设置为 `1` 以禁用自动更新。这将优先于 `autoUpdates` 配置设置。                           |
| `DISABLE_BUG_COMMAND`                      | 设置为 `1` 以禁用 `/bug` 命令                                                                                               |
| `DISABLE_COST_WARNINGS`                    | 设置为 `1` 以禁用成本警告消息                                                                                            |
| `DISABLE_ERROR_REPORTING`                  | 设置为 `1` 以选择退出 Sentry 错误报告                                                                                        |
| `DISABLE_NON_ESSENTIAL_MODEL_CALLS`        | 设置为 `1` 以禁用非关键路径（如装饰文本）的模型调用                                                              |
| `DISABLE_TELEMETRY`                        | 设置为 `1` 以选择退出 Statsig 遥测（注意，Statsig 事件不包括用户数据，如代码、文件路径或 bash 命令） |
| `HTTP_PROXY`                               | 指定网络连接的 HTTP 代理服务器                                                                                      |
| `HTTPS_PROXY`                              | 指定网络连接的 HTTPS 代理服务器                                                                                     |
| `MAX_THINKING_TOKENS`                      | 强制模型的思维预算                                                                                                  |
| `MCP_TIMEOUT`                              | MCP 服务器启动的超时（毫秒）                                                                                         |
| `MCP_TOOL_TIMEOUT`                         | MCP 工具执行的超时（毫秒）                                                                                         |
| `MAX_MCP_OUTPUT_TOKENS`                    | MCP 工具响应中允许的最大令牌数（默认：25000）                                                                |
| `VERTEX_REGION_CLAUDE_3_5_HAIKU`           | 使用 Vertex AI 时覆盖 Claude 3.5 Haiku 的区域                                                                              |
| `VERTEX_REGION_CLAUDE_3_5_SONNET`          | 使用 Vertex AI 时覆盖 Claude 3.5 Sonnet 的区域                                                                             |
| `VERTEX_REGION_CLAUDE_3_7_SONNET`          | 使用 Vertex AI 时覆盖 Claude 3.7 Sonnet 的区域                                                                             |
| `VERTEX_REGION_CLAUDE_4_0_OPUS`            | 使用 Vertex AI 时覆盖 Claude 4.0 Opus 的区域                                                                               |
| `VERTEX_REGION_CLAUDE_4_0_SONNET`          | 使用 Vertex AI 时覆盖 Claude 4.0 Sonnet 的区域                                                                             |

### 配置选项

我们正在将全局配置迁移到 `settings.json`。

`claude config` 将被 [settings.json](#设置文件) 取代

要管理你的配置，请使用以下命令：

* 列出设置：`claude config list`
* 查看设置：`claude config get <键>`
* 更改设置：`claude config set <键> <值>`
* 添加到设置（用于列表）：`claude config add <键> <值>`
* 从设置中删除（用于列表）：`claude config remove <键> <值>`

默认情况下，`config` 会更改你的项目配置。要管理全局配置，请使用 `--global`（或 `-g`）标志。

#### 全局配置

要设置全局配置，请使用 `claude config set -g <键> <值>`：

| 键                     | 描述                                                                                                                                                                                        | 示例                                                                    |
| :---------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------- |
| `autoUpdates`           | 是否启用自动更新（默认：`true`）。启用后，Claude Code 会在后台自动下载并安装更新。重新启动 Claude Code 时会应用更新。 | `false`                                                                    |
| `preferredNotifChannel` | 你希望接收通知的位置（默认：`iterm2`）                                                                                                                                        | `iterm2`、`iterm2_with_bell`、`terminal_bell` 或 `notifications_disabled` |
| `theme`                 | 颜色主题                                                                                                                                                                                        | `dark`、`light`、`light-daltonized` 或 `dark-daltonized`                  |
| `verbose`               | 是否显示完整的 bash 和命令输出（默认：`false`）                                                                                                                                   | `true`                                                                     |

### Claude 可用工具

Claude Code 可以访问一组强大的工具，帮助其理解和修改你的代码库：

| 工具             | 描述                                          | 需要权限 |
| :--------------- | :--------------------------------------------------- | :------------------ |
| **Agent**        | 运行子代理以处理复杂的、多步骤的任务 | 否                  |
| **Bash**         | 在你的环境中执行 shell 命令          | 是                 |
| **Edit**         | 对特定文件进行有针对性的编辑               | 是                 |
| **Glob**         | 基于模式匹配查找文件                | 否                  |
| **Grep**         | 在文件内容中搜索模式               | 否                  |
| **LS**           | 列出文件和目录                          | 否                  |
| **MultiEdit**    | 在单个文件上原子地执行多次编辑  | 是                 |
| **NotebookEdit** | 修改 Jupyter 笔记本单元                      | 是                 |
| **NotebookRead** | 读取和显示 Jupyter 笔记本内容         | 否                  |
| **Read**         | 读取文件内容                          | 否                  |
| **TodoRead**     | 读取当前会话的任务列表                | 否                  |
| **TodoWrite**    | 创建和管理结构化任务列表            | 否                  |
| **WebFetch**     | 从指定的 URL 获取内容                 | 是                 |
| **WebSearch**    | 执行带域过滤的 Web 搜索          | 是                 |
| **Write**        | 创建或覆盖文件                          | 是                 |

可以使用 `/allowed-tools` 或在[权限设置](/en/docs/claude-code/settings#available-settings)中配置权限规则。

#### 使用钩子扩展工具

你可以使用 [Claude Code 钩子](/en/docs/claude-code/hooks)在任何工具执行之前或之后运行自定义命令。

例如，你可以在 Claude 修改 Python 文件后自动运行 Python 格式化程序，或者通过阻止对某些路径的写入操作来防止对生产配置文件进行修改。

## 🔌 Claude Code MCP 服务器

### Gemini CLI MCP 服务器

[Gemini CLI MCP](https://github.com/centminmod/gemini-cli-mcp-server)

```bash
claude mcp add gemini-cli /path/to/.venv/bin/python /path/to/mcp_server.py -s user -e GEMINI_API_KEY='GEMINI_API_KEY' -e OPENROUTER_API_KEY='OPENROUTER_API_KEY'
```

### Cloudflare MCP 文档

[Cloudflare 文档 MCP](https://github.com/cloudflare/mcp-server-cloudflare/tree/main/apps/docs-vectorize)

```bash
claude mcp add --transport sse cf-docs https://docs.mcp.cloudflare.com/sse -s user
```

### Context 7 MCP 服务器

[Context 7 MCP](https://github.com/upstash/context7)

```bash
claude mcp add --transport sse context7 https://mcp.context7.com/sse -s user
```

### Notion MCP 服务器

[Notion MCP](https://github.com/makenotion/notion-mcp-server)

```bash
claude mcp add-json notionApi '{"type":"stdio","command":"npx","args":["-y","@notionhq/notion-mcp-server"],"env":{"OPENAPI_MCP_HEADERS":"{\"Authorization\": \"Bearer ntn_API_KEY\", \"Notion-Version\": \"2022-06-28\"}"}}' -s user
```

### Chrome 开发者工具 MCP 服务器

[Chrome 开发者工具 MCP](https://github.com/ChromeDevTools/chrome-devtools-mcp)

此 MCP 服务器可能占用 Claude 多达 17K 的上下文窗口，因此我仅在项目需要时通过运行 Claude 客户端时的 `--mcp-config` 参数安装它：

```bash
claude --mcp-config .claude/mcp/chrome-devtools.json
```

其中 `.claude/mcp/chrome-devtools.json`：

```json
{
  "mcpServers": {
    "chrome-devtools": {
      "command": "npx",
      "args": [
        "-y",
        "chrome-devtools-mcp@latest"
      ]
    }
  }
}
```

Chrome 开发者工具 MCP 服务器在 26 个 MCP 工具中占用约 16,977 个令牌

```bash
     mcp__chrome-devtools__list_console_messages (chrome-devtools): 584 tokens
     mcp__chrome-devtools__emulate_cpu (chrome-devtools): 651 tokens
     mcp__chrome-devtools__emulate_network (chrome-devtools): 694 tokens
     mcp__chrome-devtools__click (chrome-devtools): 636 tokens
     mcp__chrome-devtools__drag (chrome-devtools): 638 tokens
     mcp__chrome-devtools__fill (chrome-devtools): 644 tokens
     mcp__chrome-devtools__fill_form (chrome-devtools): 676 tokens
     mcp__chrome-devtools__hover (chrome-devtools): 609 tokens
     mcp__chrome-devtools__upload_file (chrome-devtools): 651 tokens
     mcp__chrome-devtools__get_network_request (chrome-devtools): 618 tokens
     mcp__chrome-devtools__list_network_requests (chrome-devtools): 783 tokens
     mcp__chrome-devtools__close_page (chrome-devtools): 624 tokens
     mcp__chrome-devtools__handle_dialog (chrome-devtools): 645 tokens
     mcp__chrome-devtools__list_pages (chrome-devtools): 582 tokens
     mcp__chrome-devtools__navigate_page (chrome-devtools): 642 tokens
     mcp__chrome-devtools__navigate_page_history (chrome-devtools): 656 tokens
     mcp__chrome-devtools__new_page (chrome-devtools): 637 tokens
     mcp__chrome-devtools__resize_page (chrome-devtools): 629 tokens
     mcp__chrome-devtools__select_page (chrome-devtools): 619 tokens
     mcp__chrome-devtools__performance_analyze_insight (chrome-devtools): 649 tokens
     mcp__chrome-devtools__performance_start_trace (chrome-devtools): 689 tokens
     mcp__chrome-devtools__performance_stop_trace (chrome-devtools): 586 tokens
     mcp__chrome-devtools__take_screenshot (chrome-devtools): 803 tokens
     mcp__chrome-devtools__evaluate_script (chrome-devtools): 775 tokens
     mcp__chrome-devtools__take_snapshot (chrome-devtools): 614 tokens
     mcp__chrome-devtools__wait_for (chrome-devtools): 643 tokens
```

## ⭐ 星标历史

[![Star History Chart](https://api.star-history.com/svg?repos=centminmod/my-claude-code-setup&type=Date)](https://www.star-history.com/#centminmod/my-claude-code-setup&Date)

## 📈 统计数据

![Alt](https://repobeats.axiom.co/api/embed/715da1679915da77d87deb99a1f527a44e76ec60.svg "Repobeats analytics image")