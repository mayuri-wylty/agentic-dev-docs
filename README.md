# agentic-dev-docs

`agentic-dev-docs` 是一个面向 Codex 的文档优先开发规划 skill。它把项目想法或任务规格转成一套“主控开发控制包”，用于后续新窗口中的 A 主控智能体协调 workstream、Review、审计、测试和交付。

当前版本：`0.6.1`

## 适用场景

当你希望先生成开发控制文档，再让 Codex 按文档进入主控执行阶段时使用本 skill。典型触发方式：

```text
$agentic-dev-docs
```

或：

```text
use agentic-dev-docs skill
```

该 skill 是手动触发型，不会因为普通计划、编码或文档请求自动启用。

## 核心能力

- 生成 docs-first 开发控制包，而不是直接改产品代码。
- 强制只有一个 `A-main-control`。
- 先识别项目画像，再按模块、目录、服务、页面、数据域或文档章节生成 workstream。
- 输出权限矩阵，明确每个角色的可写范围、禁止范围、证据要求、Review owner 和关闭条件。
- 约束 A 主控只做协调、冲突决策、进度维护、Review 调度、审计和汇报，不直接实现已分派给 workstream 的产品代码任务。
- 支持真实子智能体，也支持没有真实子智能体时的逻辑 workstream 记录。
- 在 `00_新窗口启动提示词.md` 中同时输出新窗口启动提示词和独立的 `/goal` 执行目标。

## v0.6.1 重点

`00_新窗口启动提示词.md` 会包含两段可复制内容：

1. 新窗口启动提示词：用于进入主控开发执行阶段。
2. `/goal` 执行目标：用于 Codex 的 `/goal` 模式追踪长期执行目标。

默认规则：执行阶段不要提交代码、不要推送代码、不要创建 PR，除非项目 plan 或用户后续消息明确授权。

## 输出文档包

最小文档集通常包含：

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

标准或完整文档集会根据项目复杂度增加系统设计、接口契约、前后端设计、数据与存储、部署运行、回归测试和风险清单等文档。

## 目录结构

```text
agentic-dev-docs/
├─ SKILL.md
├─ agents/
│  └─ openai.yaml
├─ assets/
│  └─ doc-templates/
├─ references/
│  ├─ agent-roles.md
│  ├─ document-set.md
│  ├─ examples.md
│  ├─ templates.md
│  └─ workflow.md
└─ 更新内容.txt
```

## 安装

把整个 `agentic-dev-docs` 目录放入 Codex skills 目录，例如：

```text
C:\Users\<你的用户名>\.codex\skills\agentic-dev-docs
```

然后在 Codex 中显式调用：

```text
$agentic-dev-docs
```

## 使用流程

1. 说明项目目标或任务规格。
2. 让 skill 澄清会影响计划的关键需求。
3. 选择协作方案和文档集规模。
4. 生成开发控制文档包。
5. 在新 Codex 窗口复制 `00_新窗口启动提示词.md` 中的启动提示词。
6. 如需长期追踪执行目标，再复制其中的 `/goal` 执行目标。

## 边界

规划和文档生成阶段不做产品实现：不创建产品代码、不安装依赖、不运行 dev server、不改产品目录。真正的开发执行阶段从新窗口读取启动文档后开始。