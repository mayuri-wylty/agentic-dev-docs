# agentic-dev-docs

`agentic-dev-docs` 是一个面向 Codex 的文档优先开发规划 skill。它不会直接替你开工写产品代码，而是先把项目想法、需求说明或开发任务整理成一套可以交给 Codex 新窗口执行的“主控开发控制包”。

这套控制包的目标是让后续开发过程更可控：先明确项目画像、阶段边界、权限矩阵、workstream 分工、Review 和审计规则，再由唯一的 A 主控智能体协调执行。

当前版本：`0.6.1`

GitHub 仓库：<https://github.com/mayuri-wylty/agentic-dev-docs>

## 为什么需要这个 skill

当一个任务稍微复杂一点，直接让 AI “开始实现” 很容易出现几个问题：

- 主控会一边规划一边写代码，阶段边界混乱。
- 多个子任务同时推进时，谁能改哪些文件不清楚。
- Review、测试、进度记录和最终审计容易被省略。
- 人类文档里的 Developer A、QA、Tech Lead 等标签容易被误映射成错误的 agent 身份。
- A 主控为了“顺手”直接实现 workstream 的任务，导致权限矩阵失效。
- 长任务跨窗口执行时，新窗口缺少统一启动上下文。

`agentic-dev-docs` 的设计思路是：先把这些协作规则写成文档，再让开发执行窗口读取文档工作。

## 适用场景

适合使用本 skill 的场景：

- 你有一个项目想法，需要先生成开发计划和协作文档。
- 你要把现有需求文档转换成 Codex 可执行的开发控制包。
- 你希望用一个 A 主控智能体协调多个 workstream。
- 你希望明确哪些角色只读，哪些角色可以写代码。
- 你希望新 Codex 窗口能通过启动提示词快速接管项目。
- 你希望结合 Codex `/goal` 模式追踪较长的开发执行目标。

不适合的场景：

- 只想问一个普通技术问题。
- 只想让 Codex 直接修一个很小的 bug。
- 只需要生成普通 README、注释或单页文档。
- 已经处在执行阶段，并且不想补充协作文档。

## 手动触发

这是一个严格手动触发 skill。只有明确点名时才应该使用，例如：

```text
$agentic-dev-docs
```

或：

```text
use agentic-dev-docs skill
```

或：

```text
请使用 agentic-dev-docs 为这个项目生成开发控制文档包
```

普通的“帮我规划一下”“帮我写代码”“帮我做文档”不应该自动触发它。

## 核心概念

### A-main-control

每个生成的开发控制包中只能有一个 `A-main-control`。

A 主控负责：

- 协调 workstream。
- 判断冲突和并行边界。
- 维护任务进度。
- 安排 Review。
- 安排进度一致性审计。
- 汇总证据并向用户报告。

A 主控不应该直接实现已经分派给 workstream 的产品代码任务。

### Workstream

Workstream 是按项目实际结构生成的工作流实例，不是固定的前端/后端/测试模板。

它可以按这些边界生成：

- 目录
- 模块
- 服务
- 页面
- 命令
- 数据域
- 文档章节
- 部署或配置范围

示例名称：

```text
sdk-cli-workstream
admin-ui-workstream
storage-executor-workstream
data-schema-workstream
docs-revision-workstream
```

### Permission Matrix

每个可写角色必须出现在权限矩阵中。矩阵至少要说明：

| 字段 | 含义 |
|---|---|
| Role / instance | 精确角色或实例名 |
| Type | A 主控、实现 workstream、Review、审计、文档等 |
| Default agent | 当前会话、worker、explorer 或逻辑实例 |
| Product-code write | 是否允许写产品代码 |
| Writable scope | 可写目录、文件类型、模块或文档范围 |
| Forbidden scope | 禁止触碰的范围 |
| Required evidence | 测试、构建、截图、日志、Review 或审计证据 |
| Review owner | 负责 Review 的角色 |
| Close condition | 关闭任务的客观证明 |

不在权限矩阵中的角色不能修改产品代码。

## 阶段边界

本 skill 明确区分两个阶段。

### 文档生成阶段

当前阶段只做需求澄清、协作设计、计划生成和开发文档生成。

在这个阶段不要：

- 创建产品代码。
- 初始化项目脚手架。
- 安装依赖。
- 生成 migrations。
- 运行 dev server。
- 修改产品代码目录。

### 主控开发执行阶段

执行阶段从新窗口开始。用户复制 `00_新窗口启动提示词.md` 后，新的 Codex 窗口读取开发控制包，确认自己是唯一 A 主控，然后再推进开发。

如果使用 Codex `/goal` 模式，可以再复制同一文件中的 `/goal` 执行目标，让 Codex 长期追踪主控开发目标。

## v0.6.1 新增内容

v0.6.1 增加 Codex `/goal` 模式启动支持。

生成 `00_新窗口启动提示词.md` 时，文件中会包含两段可复制内容：

1. 新窗口启动提示词。
2. 独立的 `/goal` 执行目标。

示例：

```text
/goal 作为本项目唯一 A 主控智能体，按开发文档目录中的控制包完成主控开发执行：先完成启动前环境核验，读取总控入口、任务进度、协作规范和权限矩阵；创建或复用授权 workstream；协调实现、测试、Review 和进度一致性审计；最终向用户汇报证据、风险和人工测试结果。默认不要提交代码、不要推送代码，除非本项目 plan 或用户后续消息明确授权。
```

默认策略：

- 不提交代码。
- 不推送代码。
- 不创建 PR。

只有项目 plan 或用户后续消息明确授权时，才允许把提交、PR 或推送作为执行收尾步骤。

## 生成的文档包

`agentic-dev-docs` 会根据项目复杂度选择最小、标准或完整文档集。

### 最小文档集

适合小脚本、小 CLI、单文件工具和低风险原型。

```text
00_新窗口启动提示词.md
00_启动前环境核验.md
00_总控开发入口.md
01_需求规格说明.md
02_任务进度.md
03_多智能体开发协作规范.md
04_人工测试清单.md
05_文档验收清单.md
```

### 标准文档集

适合常规 Web、CLI、桌面应用和 AI 应用。

```text
00_新窗口启动提示词.md
00_启动前环境核验.md
00_总控开发入口.md
01_项目总规划.md
02_需求规格说明.md
03_系统设计说明.md
04_接口契约规范.md
05_多智能体开发协作规范.md
06_A主控智能体工作规范.md
任务进度.md
开发日志规范.md
人工测试清单.md
文档验收清单.md
后续迭代路线图.md
```

每个被选中的 workstream 还会有自己的工作规范文档。

### 完整文档集

适合中大型、多端、多服务、高风险或长期维护项目。通常在标准文档集基础上增加：

```text
前端设计说明.md
后端设计说明.md
数据与存储设计.md
外部服务接入规范.md
安全与隐私边界.md
部署与运行手册.md
回归测试计划.md
风险清单.md
```

## 安装

把整个 `agentic-dev-docs` 目录放入 Codex skills 目录。

Windows 示例：

```text
C:\Users\<你的用户名>\.codex\skills\agentic-dev-docs
```

如果你从 GitHub 克隆：

```powershell
git clone https://github.com/mayuri-wylty/agentic-dev-docs.git C:\Users\<你的用户名>\.codex\skills\agentic-dev-docs
```

然后在 Codex 中显式调用：

```text
$agentic-dev-docs
```

## 仓库结构

```text
agentic-dev-docs/
├─ SKILL.md
├─ README.md
├─ agents/
│  └─ openai.yaml
├─ assets/
│  └─ doc-templates/
│     ├─ 人工测试清单.template.md
│     ├─ 任务进度.template.md
│     ├─ 启动前环境核验.template.md
│     ├─ 多智能体开发协作规范.template.md
│     ├─ 总控开发入口.template.md
│     ├─ 接口契约规范.template.md
│     ├─ 文档验收清单.template.md
│     └─ 新窗口启动提示词.template.md
├─ references/
│  ├─ agent-roles.md
│  ├─ document-set.md
│  ├─ examples.md
│  ├─ templates.md
│  └─ workflow.md
└─ 更新内容.txt
```

## 使用流程

### 1. 启动 skill

在 Codex 中输入：

```text
$agentic-dev-docs
```

然后描述你的项目或任务。

### 2. 澄清需求

skill 会只询问会影响计划的关键问题，例如：

- 项目类型是什么？
- 主要交付物是什么？
- 技术栈是否已经确定？
- 是否需要真实多智能体并行？
- 哪些目录或模块可以并行开发？
- 是否允许最终提交、推送或创建 PR？

### 3. 生成计划

计划会包含：

- 项目摘要和项目画像。
- 已冻结的决策和假设。
- 文档集规模。
- 要创建的文档列表。
- Workstream 注册表。
- 复用策略。
- 权限矩阵。
- 契约优先规则。
- 启动前环境核验。
- Review 和审计门禁。
- 验收标准。
- 人类责任标签到 workstream 或 Review owner 的映射。

### 4. 生成开发控制文档包

用户确认计划后，skill 才生成或编辑开发文档。文档生成仍然不是产品实现。

### 5. 新窗口执行

打开新的 Codex 窗口，复制 `00_新窗口启动提示词.md` 中的新窗口启动提示词。

如果要使用 `/goal` 模式，再复制同一文件中的 `/goal` 执行目标。

## `/goal` 推荐用法

`/goal` 目标适合长任务，因为它可以让新窗口持续记住顶层目标。

推荐顺序：

1. 先复制新窗口启动提示词。
2. 等 Codex 完成启动前环境核验。
3. 再复制 `/goal` 执行目标。
4. 让 A 主控按任务进度、权限矩阵和 Review/审计门禁推进。

如果项目 plan 没有明确授权提交或推送，A 主控最终只汇报变更、证据和风险，不执行 git commit、git push 或 PR 创建。

## 关键安全规则

- Exactly one `A-main-control`。
- A 主控不直接实现已分派给 workstream 的产品代码任务。
- 产品代码、schema、OpenAPI、typed client、测试和部署脚本修改必须分配给授权 workstream 或逻辑 workstream。
- Review、审计和 explorer 默认只读，除非文档明确授权窄范围写入。
- 每个可写角色必须先进入权限矩阵，再接任务。
- 同一生命周期和复用范围内只能有一个有效实例。
- 重复 workstream 出现时，A 主控必须暂停分派、选择主实例、合并输出并关闭重复实例。
- 最终收尾顺序是：环境核验 -> 实现证据 -> Review 通过 -> 审计通过 -> A 向用户汇报。

## 常见问题

### 这个 skill 会直接写代码吗？

不会。它的职责是生成开发控制文档包。真正写代码发生在新的主控开发执行阶段。

### 没有真实 subagent 也能用吗？

可以。没有真实子智能体时，仍然要创建逻辑 workstream 记录，并按该 workstream 的权限、日志、证据和 Review 规则执行。

### A 主控能不能顺手修一个小问题？

如果任务已经分派给 workstream，不能。除非用户显式授权、权限矩阵更新、进度文档记录职责变更，三者都满足。

### 默认会提交和推送代码吗？

不会。v0.6.1 默认不提交、不推送、不创建 PR。是否允许这些动作，必须在 plan 中选择，或由用户后续明确授权。

### 为什么不用固定的前端/后端/测试角色？

因为不同项目的并行边界不同。这个 skill 要求先识别项目画像，再按实际模块、目录、服务、页面、命令、数据域或文档章节生成 workstream。

## 版本历史

### v0.6.1

- 新增 `00_新窗口启动提示词.md` 中的独立 `/goal` 执行目标。
- 默认禁止提交、推送和创建 PR，除非 plan 或用户后续消息明确授权。
- 要求 plan 记录代码提交/推送策略。
- 自检项增加 `/goal` 目标和默认不提交/推送规则。

### v0.6.0

- 对主 `SKILL.md` 做瘦身重构。
- 将长文档集清单和模板细节下沉到 `references/` 与 `assets/doc-templates/`。
- 保留并强化阶段边界、项目画像驱动 workstream、A 主控不得实现产品代码任务、子智能体复用和权限矩阵。

### v0.5.x

- 引入项目画像驱动 workstream。
- 强制权限矩阵。
- 加强子智能体复用规则。
- 修正人类责任角色被误映射为 A 主控开发任务的问题。

## 维护建议

修改这个 skill 时，优先保持 `SKILL.md` 精简，把长模板、示例和文档集说明放到 `assets/` 或 `references/`。

修改后至少检查：

- `SKILL.md` frontmatter 是否仍然有效。
- 版本号是否更新。
- 模板是否还能生成完整文档包。
- `00_新窗口启动提示词.template.md` 是否仍包含启动提示词和 `/goal` 目标。
- 权限矩阵、workstream 复用和 A 主控边界是否仍然明确。
