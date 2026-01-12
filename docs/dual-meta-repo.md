# 双元仓库架构(Dual Meta-Repo Architecture)

## 架构定位

这是一种 “矩阵式工程组织”：
- X 轴（平台维度）：qtcloud 集成可运行的子平台；
- Y 轴（领域维度）：quanttide-data 集成可消费的资产（文档 + 代码）；
- 交汇点：qtcloud-data —— 既是平台组件，又是领域开发的基础依赖。

## 适用场景

- 大型技术平台 + 丰富领域生态（如云厂商、AI 基础设施、数据中台）；
- 需要同时支持 系统交付 与 开发者体验；
- 团队分工明确，追求高内聚、低耦合、可独立演进。

## 整体结构

- 系统由 两个顶层“大单仓”（meta-repos / umbrella repos） 构成：
  - qtcloud：平台能力集成仓，面向系统运行与基础设施。
  - quanttide-data：领域资产集成仓，面向知识沉淀与工具复用。
- 这两个仓库 本身不包含核心业务代码，而是通过 git submodule 聚合多个独立子项目。

## 子模块组织方式

- 所有功能单元（平台子系统、文档、工具库等）均为 独立 Git 仓库。
- 顶层 meta-repo 通过 .gitmodules 引用这些子模块，形成逻辑聚合。
- 示例：
    qtcloud/
  ├── core/          → submodule: qtcloud-core
  ├── ai/            → submodule: qtcloud-ai
  └── data/          → submodule: qtcloud-data   ← 共享子平台

  quanttide-data/
  ├── tutorials/     → submodule: qtd-tutorials
  ├── standards/     → submodule: qtd-standards
  ├── libs/          → submodule: qtd-libs
  └── platform/      → submodule: qtcloud-data   ← 同一份子平台

## 核心共享组件

- qtcloud-data 是一个 独立的子平台仓库，提供数据相关的：
  - 运行时服务（API/CLI）
  - SDK / 工具链
  - 数据治理能力（版本、元数据、注册等）
- 它被 同时作为 submodule 引入到 qtcloud 和 quanttide-data 中，实现：
  - 平台侧：作为可部署子系统；
  - 领域侧：作为开发依赖和文档示例基础。

## 设计意图与优势

目标   实现方式
关注点分离   平台能力 vs 领域资产，由不同团队维护

能力复用   qtcloud-data 一份代码，两处引用

灵活组合   新场景只需组合相关 submodules，无需全量克隆

版本可控   每个 meta-repo 独立锁定 submodule 版本，支持渐进升级

权限清晰   子模块可独立设置访问控制



这份共识可作为你团队内部的 架构决策记录（ADR） 或 开发者指南 的核心内容。如果需要，我可以帮你将其转化为 Markdown 文档、架构图（Mermaid）或 ADR 模板。
