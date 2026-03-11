# INFO5995 Assignment 1：作业说明与执行指南（中文正式版）

> 作业主题：**AI-Assisted Cryptographic Vulnerability Discovery**  
> 占比：**20% of final grade**  
> 满分：**15 分**  
> 截止时间：**Mon Mar 30, 2026 5:00pm**  
> 形式：**小组提交**（优先 5 人；6 人团队仅在至少有 1 名 non-technical 成员时允许）

---


## 一、文档说明

本文件依据以下课程材料整理而成：
- `assignment1-spec.pdf`
- `assignment1-rubric-2.pdf`

本文件的作用是为小组成员提供结构化执行参考，便于统一理解作业要求、评分重点、交付物与罚分规则。若本文件与课程官方 PDF 存在差异，应以课程官方文件为准。

---

## 二、作业目标

Assignment 1 要求团队对课程提供的 Android APK 进行分析，并完成一条完整的 AI-assisted security analysis workflow。重点不在于 Android 开发背景，而在于是否能够在陌生平台上：

1. 建立分析环境并完成 APK 反编译；
2. 理解应用行为并构建简化系统模型；
3. 找到至少一个安全漏洞（重点提示：**security-sensitive randomness / cryptographic misuse**）；
4. 解释漏洞成因、攻击者条件与影响；
5. 以规范、简洁、可验证的方式完成报告与展示。

---

## 三、作业总体结构

| Task | 内容 | 目标 |
|---|---|---|
| Task 1 | Unpack and Decompile the APK | 建立反编译与代码审计流程 |
| Task 2 | Understand the App and Build a Simple Model | 理解功能、资产、组件与数据流 |
| Task 3 | Find at Least One Vulnerability | 识别至少一个安全相关弱点 |
| Task 4 | Explain the Weakness and Threat Model | 说明漏洞成因、攻击路径与影响 |
| Task 5 | Write Up Your Findings | 完成正式报告、AI 日志与演示材料 |

---

## 四、Task 1：Unpack and Decompile the APK

### 要求

| 要点 | 说明 |
|---|---|
| APK 定义 | 需用自己的话说明什么是 APK，以及为何在审计中需要反编译 |
| AI 辅助工具选择 | 可使用 AI 帮助选择或配置 `jadx`、`apktool` 等工具 |
| AI 记录 | 至少保留一段 step-by-step AI interaction |
| 反编译结果确认 | 需要能够定位 package name、main activity、manifest，以及至少一个 login/registration class |
| 截图要求 | 至少提供 1 张相关反编译类截图（例如登录逻辑） |

### 建议产出
- 反编译命令记录
- 工具选择依据
- Manifest 截图
- 登录/注册逻辑截图
- 关键类路径说明

---

## 五、Task 2：Understand the App and Build a Simple Model

### 要求

| 要点 | 说明 |
|---|---|
| 动态观察 | 最好在模拟器或真机上运行并交互（注册、登录、登出）；若无法运行，可结合 tutor demo 与静态分析 |
| 功能总结 | 用自己的语言在 3–5 句内总结 app 目的与主要功能 |
| 系统模型图 | 绘制主组件、关键资产与数据流的简化模型 |
| 攻击目标说明 | 明确核心假设、攻击者可观测/可控制内容以及攻击目标 |

### 系统模型建议覆盖内容
- UI
- 本地存储
- 外部服务
- credentials / tokens
- 数据流向
- 信任边界

---

## 六、Task 3：Find at Least One Vulnerability

> 重点提示：**Randomness and Crypto**

### 要求

| 要点 | 说明 |
|---|---|
| 定位对象 | 静态分析所有被视为“random”“codes”“tokens”或类似用途的值 |
| 记录维度 | 对每个位置记录 class / method、generator API/helper、security role |
| AI 辅助分析 | 可借助 AI 判断某些 generator/pattern 是否不适用于 security-sensitive values |
| 核心漏洞选择 | 至少选定一个安全相关弱点作为主漏洞 |

### 建议的证据表字段
- Class
- Method
- Generator / Helper
- Value Role
- Security Relevance
- Weakness Type
- Initial Impact Hypothesis

---

## 七、Task 4：Explain What Went Wrong and the Threat Model

### 要求

| 要点 | 说明 |
|---|---|
| 成因解释 | 说明值在哪里生成、为何生成方式不适合其安全角色 |
| 攻击者模型 | 给出真实攻击者模型，如 network observer、another app、rooted-device user |
| 能力描述 | 说明攻击者可观测与可控制的条件 |
| 攻击路径 | 提供简洁、具体、可理解的 step-by-step attack scenario |

### 建议写法
- 漏洞点位置
- 弱点原因
- 可利用条件
- 攻击步骤
- 造成的结果（如 impersonation、auth bypass、token prediction 等）

---

## 八、Task 5：Write Up Your Findings

### 报告要求

| 项目 | 要求 |
|---|---|
| 模板 | 必须使用 **USENIX official paper template**（LaTeX 或 Word） |
| 页数 | **最多 2 页**，包括 figures 与 references |
| 内容范围 | 必须覆盖引言、系统/威胁模型、漏洞发现方法、漏洞细节、修复方案 |

### 报告中必须覆盖的内容
1. 简短引言（目标与为何采用 APK 分析）
2. System / threat model（protected assets、attacker、capabilities）
3. 漏洞发现过程（包括 tooling 与 AI usage）
4. 漏洞细节（代码位置、风险、利用路径）
5. 修复方案与有效性说明

### 额外提交要求

| 项目 | 说明 |
|---|---|
| `ai-log/` | AI 使用日志，允许写总结版，不要求逐字记录 |
| tools/docs references | 需记录所用工具与文档参考 |
| rubric-driven LLM mock Q&A | 提交前至少完成一次，并将关键问题与改进点写入 AI log |
| `presentation.mp4` | 录制演示视频，**≤ 5 分钟** |
| `pocs/` | PoC 视频与 README |
| supporting scripts/config | 分析中使用的脚本与配置 |

---

## 九、Submission Package

| 文件/目录 | 说明 |
|---|---|
| `report.pdf` | 按 USENIX 模板排版的正式报告 |
| `ai-log/` | prompt-response summaries 与验证记录 |
| `pocs/` | 视频与 README |
| `presentation.mp4` | 录制演示视频（≤5 分钟） |
| supporting materials | 任何支撑分析的脚本、配置与辅助文件 |

---

## 十、Tutorial 与罚分规则

| 项目 | 规则 |
|---|---|
| Tutorial 总时长 | 最多 10 分钟（严格） |
| Presentation playback | 5 分钟 |
| Q&A | 最多 5 分钟 |
| 超时罚分 | 演示视频每超出 10 秒，Assignment 1 扣 1 分（/15） |
| 报告合规罚分 | 若报告超过 2 页或未使用官方 USENIX 模板，扣 3 分 |

### Tutorial Q&A 中可能被问到的内容
- 你们的分析质量如何保证
- 你们想达到的 rubric level 是什么
- 每位组员分别做了什么
- AI 在流程中扮演了什么角色

---

## 十一、建议执行顺序

| 阶段 | 建议任务 | 建议产出 |
|---|---|---|
| 第 1 阶段 | 反编译 APK，跑通工具链 | Manifest、主类、关键截图 |
| 第 2 阶段 | 理解功能与系统结构 | App summary、system model |
| 第 3 阶段 | 检查 random/token/crypto 相关逻辑 | 候选漏洞表 |
| 第 4 阶段 | 锁定主漏洞并构建攻击路径 | threat model、impact reasoning |
| 第 5 阶段 | 压缩为 2 页正式报告 | `report.pdf` 草稿 |
| 第 6 阶段 | 完成 AI log 与 mock Q&A | `ai-log/` |
| 第 7 阶段 | 录制 5 分钟以内 presentation | `presentation.mp4` |

---

## 十二、总结

Assignment 1 的评分重点在于：
- 是否形成完整分析链路；
- 是否能正确识别并解释一个安全相关漏洞；
- 是否能把技术证据、AI 辅助过程与正式写作要求结合起来；
- 是否能在篇幅受限的前提下，完成高质量表达。



---

## 十三、Assignment 1 Rubric（补充整理）

### 评分结构

| 项目 | 分值 | 评价重点 |
|---|---:|---|
| System & Threat Model | 4 | 系统模型、关键资产、数据流与攻击者目标/能力是否明确，并与漏洞形成清晰联系 |
| Vulnerability Discovery & Explanation | 6 | 是否正确识别 randomness / cryptography 相关漏洞，并说明发现方法、风险与攻击路径 |
| Fix / Mitigation | 2 | 修复方案是否具体、技术正确、能解释为何有效 |
| Recorded Presentation & Q&A | 3 | 5 分钟内是否清楚讲明模型、漏洞、修复，并能在 Q&A 中证明理解与贡献 |
| **总计** | **15** |  |

### 评分说明

| 档位 | 要点 |
|---|---|
| 高分 | 准确识别与 randomness / cryptography 相关的核心漏洞，并给出完整攻击路径与证据 |
| 中档 | 基本识别到预期问题，但论证深度或证据质量略有不足 |
| 低档 | 仅部分正确，或分析混入不相关漏洞类别 |
| 0 分风险 | 未识别出有效的 randomness / cryptography 漏洞 |

### 重要修改与罚分项（基于 rubric）

| 规则 | 说明 |
|---|---|
| 评分聚焦 | Assignment 1 只按 **randomness / cryptography** 相关漏洞评分 |
| AI 使用 | 允许使用 AI，但最终结果必须能由团队自行解释与答辩 |
| 必交文件 | `report.pdf`、`ai-log/`、`pocs/`、`presentation.mp4`、`activity-log.pdf` |
| 缺少必交项 | **每缺少任一 required item，扣 5 分** |
| 演示超时 | 每超 10 秒，扣 1 分（/15） |
| 报告不合规 | 超过 2 页或未用 USENIX 模板，扣 3 分 |
| 成员发言时长 | 每人至少 40 秒，否则按公式折算个人分数 |
| activity-log 格式 | 必须采用 **N×N contribution matrix** |

### 当前建议

由于当前阶段正在完成 Assignment 1，建议团队优先准备：
- `a1_case1.apk` 的分析材料
- `assignment1-rubric-2.pdf` 对应的评分项证据
- `activity-log.pdf` 的 N×N contribution matrix
- 一份能在 5 分钟内完成且每名成员均满足 40 秒发言要求的 presentation


---

## 十四、当前阶段建议

由于当前小组工作重点为 Assignment 1，建议团队优先完成以下内容：
- `a1_case1.apk` 的反编译与代码定位；
- 按 rubric 准备系统模型、漏洞证据、修复方案；
- 完成 `activity-log.pdf` 的 N×N contribution matrix；
- 录制一份满足时长要求且每位成员发言时间合规的 presentation。
