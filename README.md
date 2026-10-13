# Codex 协作规范

[简体中文](README.md) | [English](README.en.md)

面向初级到高级开发者的通用 `AGENTS.md`：帮助 AI 编程助手核实关键假设、说明工程取舍、控制修改范围，并围绕可验收目标完成任务。

适用于个人开发、团队协作和不同规模的项目。规则关注高内聚与低耦合、最小充分实现、正式项目文档、可信下载来源，以及有明确边界的产物清理；沟通语言、解释深度和工具选择按实际用户与环境调整。

本仓库融合多个公开仓库的组织方式与行为原则，并加入实际协作偏好。它是一份可按项目调整的规则模板，不是官方标准，也不代替代码审查、测试或需求验收。

## 它帮助解决什么问题

- **需求判断**：区分目标、事实和假设，查证关键判断；发现问题时解释依据与影响，让开发者能理解方案选择。
- **第一性原则与对抗性审查**：从目标、事实、约束和验收条件出发，寻找反例、失败路径和隐藏依赖，也审查 AI 自己提出的方案。
- **工程设计**：保持高内聚、低耦合，为实际问题建立边界，减少未经要求的功能、配置和抽象。
- **修改范围**：每项变更都能追溯到任务；清理本次变更造成的孤立内容，保留无关的原有成果。
- **完成标准**：定义可验收目标，对复杂任务明确检查方式，持续推进到交付，并给出有价值的下一步。
- **环境适配**：根据用户语言、经验和项目工具链协作，使用可信下载来源并核验完整性。
- **团队文档与清理**：文档可独立使用，示例避免绑定个人环境；仅自动清理来源和用途明确的本次临时产物。

规则偏向降低非简单任务中的错误成本。简单改动直接处理；低风险实现细节允许采用可验证的合理假设，不要求每次都停下提问或额外编写设计文档。

## 仓库内容

| 文件 | 用途 |
| --- | --- |
| [AGENTS.md](AGENTS.md) | 规则正文，权威来源 |
| [AGENTS.en.md](AGENTS.en.md) | 英文翻译，与规则正文保持一致 |
| [README.md](README.md) | 面向读者的适用范围、使用方法和来源说明 |
| [README.en.md](README.en.md) | 英文介绍与使用说明 |
| [CLAUDE.md](CLAUDE.md) | Claude Code 的引用入口，不复制规则正文 |

这是一份指令模板，无运行时依赖，无需安装 Skill 或插件。

两种语言版本使用相同的行为规则，不限定交流语言、开发者经验、操作系统或下载地域。Codex 默认不会自动发现 `AGENTS.en.md`；使用英文规则时，将其内容合并或复制为目标位置的 `AGENTS.md`，只安装一个语言版本。翻译有歧义时以规则正文为准。

## 使用方法

### Codex 全局规则

先阅读 [AGENTS.md](AGENTS.md)，按用户偏好、项目约定和实际开发环境调整。

Codex 默认在 `~/.codex` 查找全局指令；设置了 `CODEX_HOME` 时使用该目录。如果已有全局规则，先备份并合并，不直接覆盖。

以下示例适用于 PowerShell，将审阅后的文件放入默认位置：

```powershell
$codexConfigDir = if ($env:CODEX_HOME) {
    $env:CODEX_HOME
} else {
    Join-Path $env:USERPROFILE '.codex'
}

$agentsPath = Join-Path $codexConfigDir 'AGENTS.md'
New-Item -ItemType Directory -Path $codexConfigDir -Force | Out-Null

if (Test-Path -LiteralPath $agentsPath) {
    throw '已有 AGENTS.md；请先备份并手动合并。'
}

Copy-Item -LiteralPath './AGENTS.md' -Destination $agentsPath
```

在 macOS 或 Linux 中，使用同一全局位置，并按实际 Shell 执行文件复制与合并；本模板不要求安装 PowerShell。

### 项目规则

将需要的条目合并到项目根目录的 `AGENTS.md`，项目规则写清实际架构、运行命令、验收条件及操作边界。子目录可提供更具体的规则。

避免同时在全局和项目文件中复制整份正文。项目文件维护项目差异，全局文件维护共同偏好。

### 加载与优先级

Codex 在启动运行时构建指令链，全局规则先加载，项目及更深目录的规则随后补充或覆盖。每个目录优先读取 `AGENTS.override.md`，否则读取 `AGENTS.md`；更高优先级的系统、平台和安全约束仍然适用。

修改后应在新的运行或会话中核对生效内容，不假定所有客户端都会在现有会话中自动重载。可让 Codex 概述当前实际加载的规则，并检查是否有 `AGENTS.override.md` 或自定义 `CODEX_HOME`。

详细行为以 [官方 AGENTS.md 文档](https://learn.chatgpt.com/docs/agent-configuration/agents-md) 为准。

### Claude Code

本仓库的 `CLAUDE.md` 用 `@AGENTS.md` 引用规则正文。使用时保持两个文件相邻，并按客户端实际支持的导入方式核对加载结果。其他工具的文件名和加载机制应分别确认。

## 融合思路与参考来源

| 来源 | 参考内容 | 本仓库的处理 |
| --- | --- | --- |
| [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills) | 编码前思考、简单优先、精确修改、目标驱动四项原则 | 合并到现有判断与实现规则；按影响处理歧义，避免绝对化流程 |
| [richling98/karpathy-codex-skills](https://github.com/richling98/karpathy-codex-skills) | Codex 指令包装、适用场景与安装说明 | 将规则正文和读者说明分开，提供保留已有配置的安装方法 |
| [zzusec/andrej-karpathy-skills-codex](https://github.com/zzusec/andrej-karpathy-skills-codex) | Codex 专用规则文件的发布方式 | 使用标准 `AGENTS.md` 名称，并提示检查实际加载情况 |
| [OpenAI：AGENTS.md](https://learn.chatgpt.com/docs/agent-configuration/agents-md) | 全局与项目指令发现、覆盖关系 | 用于核对使用说明 |
| [OpenAI：精简 Skills 与提示词](https://developers.openai.com/blog/rethinking-skills-and-prompts-for-gpt-6-astra) | 精简常驻上下文、明确触发条件与完成边界 | 合并重复概念，按任务规模采用流程 |

四项编码原则来自 Karpathy-inspired 社区整理；参考仓库不代表 Andrej Karpathy 本人或 OpenAI 对本仓库的背书。高内聚与低耦合、环境适配、正式文档及清理边界来自维护者的协作需求。README 采用独立表述，未整段复制其他仓库的说明。

## 调整与贡献

以具体失败案例调整规则：说明希望改变的行为、当前条目为何不足，以及新规则对简单任务的影响。优先合并重复要求，避免增加无条件的审批、测试或计划流程。

观察规则是否有效，应关注无关修改是否减少、工程取舍是否更清楚、验收是否完成，以及澄清是否发生在会影响结果的决策之前。当前没有跨模型、跨项目的对照实验，不宣称普遍提升效果或完全消除指令冲突。

行为规则以 `AGENTS.md` 为权威来源，修改后同步更新 `AGENTS.en.md`，不在翻译中单独引入行为差异；安装方式、受众或参考来源变化时同步更新中英文 README。

## 来源与授权

本仓库通过 GitHub 公开分享，当前未指定开源许可证。公开可见与允许任意再分发属于不同授权范围；采用开源许可证后以仓库的 LICENSE 为准。参考项目分别遵循各自许可证。

文档在维护者指导下使用 AI 辅助整理，内容由维护者维护。
