# Document Set Reference

## 文档集选择原则
按项目复杂度选择最小、标准、完整文档集。不要为了形式生成过多文档，也不要省略后续主控开发必须读取的启动提示、环境核验、入口和进度文档。

## 最小文档集
适合小脚本、小 CLI、单文件工具、低风险原型：

- `00_新窗口启动提示词.md`
- `00_启动前环境核验.md`
- `00_总控开发入口.md`
- `01_需求规格说明.md`
- `02_任务进度.md`
- `03_多智能体开发协作规范.md`
- `04_人工测试清单.md`
- `05_文档验收清单.md`

## 标准文档集
适合常规 Web、CLI、桌面、AI 应用：

- `00_新窗口启动提示词.md`
- `00_启动前环境核验.md`
- `00_总控开发入口.md`
- `01_项目总规划.md`
- `02_需求规格说明.md`
- `03_系统设计说明.md`
- `04_接口契约规范.md`
- `05_多智能体开发协作规范.md`
- `06_A主控智能体工作规范.md`
- 每个选定子智能体一份工作规范
- `任务进度.md`
- `开发日志规范.md`
- `人工测试清单.md`
- `文档验收清单.md`
- `后续迭代路线图.md`

## 完整文档集
适合中大型、多端、多服务、高风险、长期维护项目。在标准文档集基础上增加：

- `前端设计说明.md`
- `后端设计说明.md`
- `数据与存储设计.md`
- `外部服务接入规范.md`
- `安全与隐私边界.md`
- `部署与运行手册.md`
- `回归测试计划.md`
- `风险清单.md`

## 项目画像补充
Web app:
- routes/components/state design
- API contract
- browser QA checklist
- UI workstream only when UI code exists

CLI tool:
- command syntax
- stdout/stderr format
- exit code contract
- CLI workstream and contract review

Desktop app:
- OS permission model
- window/process integration notes
- packaging or installer workstream when needed

AI app:
- provider abstraction
- prompt/config strategy
- privacy and logging boundaries
- model/prompt workstream when needed

Data app:
- schema design
- migration plan
- data validation rules
- data/schema workstream and audit role

Docs or thesis project:
- chapter/section workstreams
- citation/format review
- final consistency audit
