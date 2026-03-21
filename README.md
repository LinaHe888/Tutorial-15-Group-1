# INFO5995 Assignment Repository Guide

本仓库用于整理 INFO5995 课程小组作业相关材料，包括 Assignment 1、Assignment 2、原始课程文件以及按任务划分的提交目录。

---

## 当前优先事项

当前阶段建议**优先完成 Assignment 1**，Assignment 2 相关材料可先保留并延后整理。

### 截止时间

| 作业 | 截止时间 |
|---|---|
| INFO5995 Assignment 1 | **Mon Mar 30, 2026 5:00pm** |
| INFO5995 Assignment 2 | **Mon May 11, 2026 5:00pm** |

### 整体进度对齐

> **截止时间：3月30日 5pm**  
> **第一道线（3月24日周二）：各自 Part 完成并提交到仓库**  
> **第二道线（3月27日周五）：整体流程跑通，完整 dry-run**  
> **最终截止（3月30日 5pm）：提交所有 required files**

| 阶段 | 时间节点 | 目标 |
|---|---|---|
| 各自完成 Part | **3月24日（周二）** | 每位组员完成分配部分，commit 并 push 到仓库 |
| 整体对齐 | **3月27日（周五）** | Task 1–5 内容合并，完整 dry-run 一遍，发现问题及时补 |
| 最终提交 | **3月30日（周一）5pm** | `report.pdf`、`presentation.mp4`、`ai-log/`、`pocs/`、`activity-log.pdf` 全部到位 |

### 当前各 Task 进度（main 分支）

| Task | 状态 | 说明 |
|---|---|---|
| Task 1 — APK 反编译 | ✅ 已完成 | 已反编译、定位关键类、整理证据与截图 |
| Task 2 — 系统模型 | ✅ 已完成 | App 组件、数据流、信任边界 |
| Task 3 — 漏洞发现 | ✅ 已完成 | 主漏洞：Session Token 使用 `java.util.Random` |
| Task 4 — 威胁模型 | ✅ 已完成 | 攻击路径、影响分析 |
| Task 5 — 报告与演示 | ⏳ 待完成 | 需在 3月27日前收尾并合并 |

---

## 文档导航

| 文件 | 内容 |
|---|---|
| `README_1.md` | Assignment 1 作业要求、Rubric、交付内容、任务拆解与执行建议 |
| `README_2.md` | Assignment 2 Spec 与 Rubric 的正式整理版说明 |
| `assignment1-spec.pdf` | Assignment 1 原始说明文档 |
| `assignment1-rubric-2.pdf` | Assignment 1 原始 Rubric 文档 |
| `assignment2-spec-2.pdf` | Assignment 2 原始说明文档 |
| `assignment2-rubric-1.pdf` | Assignment 2 原始 Rubric 文档 |
| `a1_case1.apk` | Assignment 1 课程提供 APK 样本 |
| `a2_case1.apk` | Assignment 2 课程提供 APK 样本 |

---

## 目录结构说明

| 目录 | 用途 |
|---|---|
| `submission/task-1-decompilation/` | Assignment 1 Task 1 相关材料 |
| `submission/task-2-system-model/` | Assignment 1 Task 2 相关材料 |
| `submission/task-3-vulnerability-discovery/` | Assignment 1 Task 3 相关材料 |
| `submission/task-4-threat-model/` | Assignment 1 Task 4 相关材料 |
| `submission/task-5-report-and-presentation/` | Assignment 1 Task 5 相关材料 |
| `submission/shared-assets/` | 全组共享材料 |

---

## 使用建议

1. 当前阶段优先阅读 `README_1.md`，集中完成 Assignment 1。
2. Assignment 2 可先参考 `README_2.md` 了解后续要求，但无需立即展开全部工作。
3. 按任务将材料上传到 `submission/` 下对应目录。
4. 提交前统一检查报告页数、模板格式、演示时长、组员发言时长及活动记录。

---

## 当前仓库已上传文件

- `a1_case1.apk`
- `a2_case1.apk`
- `assignment1-spec.pdf`
- `assignment1-rubric-2.pdf`
- `assignment2-spec-2.pdf`
- `assignment2-rubric-1.pdf`
- `README_1.md`
- `README_2.md`


---

## 文档适用范围

本仓库文档主要服务于以下用途：
- 小组内部协作与材料归档；
- 根据课程说明与 rubric 对任务进行拆解；
- 在提交前完成统一的格式、时长与材料完整性检查。

本仓库中的中文说明文件属于对课程原始文档的结构化整理，实际评分仍应以课程官方发布的 PDF 文件为准。

---

## 当前执行重点（建议）

鉴于当前时间节点，建议团队优先完成以下事项：

1. 完成 `a1_case1.apk` 的反编译与关键类定位；
2. 根据 `assignment1-rubric-2.pdf` 整理 Assignment 1 的评分证据；
3. 准备 `report.pdf`、`presentation.mp4`、`ai-log/`、`pocs/` 与 `activity-log.pdf`；
4. 检查 presentation 总时长与每位成员发言时长是否满足要求；
5. 采用 N×N contribution matrix 形式准备活动记录。

---

## 提交前检查清单

| 检查项 | Assignment 1 | Assignment 2 |
|---|---|---|
| 是否使用官方模板 | 必须 | Part A 必须 |
| 是否控制在规定页数内 | 必须 | Part A 必须 |
| 是否准备完整 required items | 必须 | 必须 |
| presentation 是否超时 | 必须检查 | 必须检查 |
| 每位成员是否达到最少发言时长 | 必须检查 | 必须检查 |
| activity-log 是否齐全 | 必须 | 必须 |
| 证据是否可支撑 Q&A | 必须 | 必须 |
