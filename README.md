# INFO5995 Assignment 1：你需要做什么

> 作业主题：**AI-Assisted Cryptographic Vulnerability Discovery**  
> 占比：**20%**（总分 15 分）  
> 形式：**小组作业**（优先 5 人；6 人仅在组内至少 1 名 non-technical 时允许）

---

## 一、作业核心目标（老师要看什么）

你们要分析一个给定 APK（Android 安装包），完成一条完整安全分析链路：

1. 反编译 APK 并搭建分析流程
2. 理解 App 行为并建一个简化系统模型
3. 找到至少 1 个漏洞（重点提示：**安全敏感随机数/密码学相关用法**）
4. 解释漏洞原理、攻击者能力和攻击路径
5. 写成规范报告 + AI 日志 + 演示视频

---

## 二、你必须交的文件（Submission Package）

1. `report.pdf`  
   - **必须用 USENIX 官方模板（LaTeX 或 Word）**
   - **最多 2 页（包含图和参考文献）**

2. `ai-log/`  
   - 记录 AI 使用日志（可用总结，不要求全量逐字）
   - 包含工具/文档引用
   - 包含至少一次“基于 rubric 的 LLM 模拟问答”记录

3. `pocs/`  
   - 漏洞 PoC 视频 + README

4. `presentation.mp4`  
   - **最多 5 分钟**

5. 其他支持材料（脚本、配置等）

---

## 三、分任务拆解（按老师原始 Task 1~5）

## Task 1：反编译 APK

你要完成：
- 用自己的话解释：什么是 APK，为什么审计要先反编译
- 使用 AI 帮你选/配工具（如 `jadx`, `apktool`），保留至少一段 step-by-step AI 交互记录
- 反编译后确认你能定位：
  - package name
  - main activity
  - manifest
  - 至少 1 个登录/注册相关 class
- 放 1 张相关代码截图（如登录逻辑）

## Task 2：理解 App + 建模

你要完成：
- 最好在模拟器/真机跑起来（注册、登录、登出）；跑不起来可用 tutor demo + 静态分析
- 用 3~5 句总结 app 目的和主要功能
- 画一个简化系统模型图：
  - 组件（UI/存储/外部服务）
  - 关键资产（凭证/token）
  - 数据流
- 写清核心假设和攻击目标

## Task 3：至少找到 1 个漏洞（重点：随机数/密码学）

你要完成：
- 静态分析定位所有“random/code/token”等值
- 每个点记录：
  - class/method
  - 生成 API/helper
  - 安全角色（UI-only / identifier / token / key）
- 判断哪些生成方式不适合安全用途（可用 AI 辅助）
- 选 1 个安全相关弱点作为主漏洞

## Task 4：解释漏洞 + 威胁模型

你要完成：
- 解释值在哪里生成、为什么方法不安全
- 写攻击者模型（如网络监听者、恶意 app、root 用户）
- 写一个短攻击路径（步骤化）

## Task 5：成文与展示

报告 2 页内必须覆盖：
- 引言（目标 + 为什么 APK 分析）
- 系统/威胁模型
- 漏洞发现过程（含工具与 AI 使用）
- 漏洞细节（代码位置、风险、利用路径）
- 修复方案与有效性说明

同时：
- 做 rubric 驱动 LLM 彩排问答并记录在 AI log
- 录一个 ≤5 分钟演示视频

---

## 四、扣分雷区（非常重要）

1. **报告超过 2 页 或 未使用官方 USENIX 模板**  
   → 扣 **3 分（/15）**

2. **演示超过 5 分钟**  
   → 每超 10 秒扣 **1 分（/15）**

---

## 五、建议你的执行顺序（可直接照做）

1. 组队并明确每人负责模块（工具/建模/漏洞/写作/视频）
2. 当天先完成反编译和代码定位（Task 1）
3. 次日完成系统图与威胁模型（Task 2 + 4）
4. 用“随机数与 token 生成点”做主线挖漏洞（Task 3）
5. 先写 2 页报告骨架，再填证据
6. 做一次 LLM rubric rehearsal，修正薄弱点
7. 录 ≤5 分钟视频并压时长

---

## 六、你现在立刻该做的 3 件事

1. 确认小组成员与分工（谁负责逆向、谁负责报告、谁负责演示）
2. 把 APK 用 `jadx` / `apktool` 跑通并截图关键类
3. 建一个漏洞证据表（class/method/API/安全角色/风险）

---

## 参考（作业要求中给出的模板）

- USENIX paper template:  
  https://www.usenix.org/conferences/author-resources/paper-templates

---

## 七、6人小组分工建议（可直接执行）

> 建议按“产出链路”分工，不要按 Task 平均切分。

### 成员分工

1. **逆向负责人 A（技术）**  
   - 负责 Task 1 主体：反编译、代码定位、关键截图  
   - 任务量：**18%**

2. **逆向负责人 B（技术）**  
   - 负责 Task 3 主体：定位 random/token/key 生成点、整理候选漏洞  
   - 任务量：**20%**

3. **威胁建模负责人（技术/半技术）**  
   - 负责 Task 2 + Task 4：系统模型图、攻击者模型、攻击路径  
   - 任务量：**16%**

4. **漏洞论证与修复负责人（技术）**  
   - 负责收敛主漏洞、写 exploit 路径与 mitigation  
   - 任务量：**18%**

5. **报告整合负责人（写作强）**  
   - 负责 Task 5 报告压缩到 2 页、模板合规、证据串联  
   - 任务量：**16%**

6. **AI日志与演示负责人（可非技术）**  
   - 负责 ai-log、rubric mock Q&A、5 分钟演示视频与讲稿  
   - 任务量：**12%**

### 按 Task 的任务量估算

- Task 1（反编译与定位）：**20%**
- Task 2（系统理解与建模）：**15%**
- Task 3（漏洞发现，核心）：**25%**
- Task 4（威胁模型与攻击解释）：**15%**
- Task 5（报告 + AI日志 + 演示）：**25%**

### 推荐节奏（4天）

- **D1**：反编译跑通 + 关键类定位 + 候选漏洞池
- **D2**：系统模型图 + 威胁模型 + 锁定主漏洞
- **D3**：报告初稿（2页内）+ AI log + mock Q&A
- **D4**：视频录制 + 全组答辩彩排 + 最终提交检查---

# INFO5995 Assignment 2：评分标准与交付要求（中文正式版）

> 作业主题：**AI-Assisted Network Security Analysis & Bug Bounty Exploration**  
> 总分：**25 分**  
> 评分核心：**以团队提交的单个最高影响力、有效、真实世界（in-the-wild）漏洞为主要评价依据**  
> 最高优先级：**具备充分证据支撑的 zero-day candidate**

---

## 一、作业定位与评分导向

Assignment 2 由 **Part A（系统与网络安全分析）** 与 **Part B（真实世界漏洞发现与论证）** 两部分构成。整体评分强调团队对真实安全问题的分析能力、证据组织能力、影响论证能力，以及对合法漏洞披露边界的理解。

本作业的主要评分依据不是材料数量，而是团队是否能够围绕一个**有效、可验证、影响明确的高质量 finding**，形成完整、合规、证据充分的技术论证。

### 总体理解

| 项目 | 说明 |
|---|---|
| Part A | 用于展示团队对目标系统、威胁模型、网络弱点、影响与修复方案的系统化分析能力 |
| Part B | 用于展示团队在真实漏洞奖励项目或指定公司项目中识别、验证并论证真实世界漏洞的能力 |
| 评分重点 | 团队提交的**单个最高影响力 valid in-the-wild finding** |
| 高分关键 | 合法 scope、强证据、清晰影响链、明确严重性映射、具备充分支撑的 zero-day candidate |
| 重要限制 | 若未提交有效的 in-the-wild finding，则 **Assignment 2 总分最高为 15/25** |

---

## 二、Assignment 2 评分结构总览

| 部分 | 内容 | 分值 | 评价重点 |
|---|---:|---:|---|
| Part A | System & Threat Model | 3 | 系统模型是否准确，攻击者能力、受保护资产与信任边界是否表述清晰 |
| Part A | Vulnerability Discovery & Impact Reasoning | 5 | 是否识别正确的主要网络安全弱点，并给出充分证据与可信影响分析 |
| Part A | Mitigation | 2 | 修复方案是否技术正确、针对根因、能够降低风险 |
| Part B | In-the-Wild Finding Quality & Scope Handling | 2 | finding 是否真实、合法、在 scope 内，并具备可复现性 |
| Part B | Highest-Impact Finding (Zero-Day Prioritised) | 10 | 严重性、影响证据与新颖性（zero-day 优先） |
| Presentation | Recorded Presentation & Q&A | 3 | 演示是否清晰、完整，Q&A 是否基于证据、回答有说服力 |
| **总计** |  | **25** |  |

---

## 三、Part A 评分标准

## 1）System & Threat Model（3 分）

| 等级 | 分数 | 要求 |
|---|---:|---|
| Exceeds Expectations | 3 | 系统模型与网络模型清晰；攻击者能力现实且合理；攻击者能力与受保护资产、信任边界之间的关系明确；相关论证可在 Part A 报告中直接获得证据支持 |
| Meets Expectations | 2 | 模型整体正确，但在攻击者能力描述、资产关联或边界说明上仍存在轻微缺口 |
| Developing | 1 | 仅形成部分模型；攻击者描述较为模糊；与资产、边界之间的联系不充分 |
| Incomplete | 0 | 模型缺失，或核心内容明显错误 |

### 建议在本部分明确呈现的内容
- 系统中的主要组件与网络交互对象
- 攻击者类型及其可行能力
- 关键资产（例如凭证、token、敏感数据、会话信息等）
- 信任边界与跨边界数据流
- 攻击目标与安全假设

---

## 2）Vulnerability Discovery & Impact Reasoning（5 分）

| 等级 | 分数 | 要求 |
|---|---:|---|
| Exceeds Expectations | 5 | 正确识别主要网络弱点；具有充分的代码、配置、行为或流量证据；影响分析完整、可信、逻辑严密 |
| Meets Expectations | 3–4 | 识别结果基本正确；证据较为合理；影响分析总体可信，但论证深度略有不足 |
| Developing | 1–2 | 仅识别部分问题或识别到次要问题；影响分析较弱或存在较强推测成分 |
| Incomplete | 0 | 漏洞结论无效，或无法成立 |

### 本部分建议提供的证据类型
- 代码证据（网络请求逻辑、校验逻辑、异常处理逻辑等）
- 配置证据（如 TLS、证书校验、明文通信配置）
- 运行行为证据（测试行为、抓包结果、可观测现象）
- 从漏洞到后果的完整逻辑链条：**弱点 → 可利用条件 → 受影响资产 → 现实后果**

---

## 3）Mitigation（2 分）

| 等级 | 分数 | 要求 |
|---|---:|---|
| Exceeds Expectations | 2 | 修复方案具体、技术正确、能够解决根因，并明确说明为何该方案能有效降低风险 |
| Meets Expectations | 1.5 | 修复方向总体正确，但在完整性或细节上存在轻微不足 |
| Developing | 0.5–1 | 修复表述较为笼统，或仅覆盖部分问题，或未有效针对根因 |
| Incomplete | 0 | 修复方案无效，或未提供 |

### 高质量 mitigation 的基本特征
- 明确说明应修改的实现位置与机制
- 针对 root cause，而非仅处理表面症状
- 能解释风险如何被降低，以及是否存在残余风险

---

## 四、Part B 评分标准

## 4）In-the-Wild Finding Quality & Scope Handling（2 分）

| 等级 | 分数 | 要求 |
|---|---:|---|
| Exceeds Expectations | 2 | finding 来自合法 bug bounty 项目，或作业明确允许的公司自营项目；提交材料包含 target URL、scope evidence、legal boundaries、reproducible steps 以及 finding-type classification |
| Meets Expectations | 1.5 | finding 基本有效且基本在 scope 内，但在 scope proof、复现细节或 finding classification 中存在一项不完整 |
| Developing | 0.5–1 | 证据较弱；scope 控制不清晰；复现或分类说明不充分 |
| Incomplete | 0 | finding 无效、不可验证、超出 scope、来源不合法，或来自未经批准的公司项目 |

### 作业明确允许的 company-run programs

| 项目 | 说明 |
|---|---|
| Google | 明确允许 |
| Meta | 明确允许 |
| Microsoft | 明确允许 |
| Apple | 明确允许 |
| GitHub | 明确允许 |
| OpenClaw | 明确允许 |
| Anthropic | 明确允许 |
| OpenAI | 明确允许 |

### 合规限制说明
- 若 finding 不来自合法 bug bounty 项目，且目标也不在上述公司名单中，必须事先取得 **unit coordinator 的书面批准**。
- 未满足该条件的 finding，可能在本项中直接记为 **0 分**。

---

## 5）Highest-Impact Finding（10 分，Zero-Day Prioritised）

| 等级 | 分数 | 要求 |
|---|---:|---|
| 顶档 | 9–10 | normalized severity 达到 **S4 Critical**；impact evidence 强；严重性映射清晰；novelty evidence 充分支持其为 zero-day candidate |
| 高档 | 6–8 | normalized severity 达到 **S3 High**，或虽达到 S4 但在证据或新颖性上存在明显缺口 |
| 中低档 | 1–5 | normalized severity 仅为 **S1–S2**，或严重性映射、影响证据不足 |
| 无效 | 0 | 未提交有效 in-the-wild finding |

### Part B 最高影响力 finding 的客观评分模型

| 组成部分 | 分值范围 | 说明 |
|---|---:|---|
| Severity score | 0–6 | 根据 finding 的 normalized severity tier 赋分 |
| Impact-evidence score | 0–2 | 判断 exploit path、受影响资产/数据及现实后果是否有充分证据 |
| Novelty score | 0–2 | 判断其是否为有充分支撑的 zero-day candidate，或为已确认 non-zero-day |
| **总分** | **0–10** | 三部分相加，最终封顶 10 分 |

---

## 五、严重性映射（Normalized Severity Tiers）

| 等级 | 分值 | 典型映射 |
|---|---:|---|
| S4 Critical | 6.0 | HackerOne Critical / CVSS ≥ 9.0 / Bugcrowd P1 / Intigriti Critical / Immunefi Critical / YesWeHack Critical |
| S3 High | 4.5 | HackerOne High / CVSS 7.0–8.9 / Bugcrowd P2 / Intigriti High / Immunefi High / YesWeHack High |
| S2 Medium | 3.0 | HackerOne Medium / CVSS 4.0–6.9 / Bugcrowd P3 / Intigriti Medium / Immunefi Medium / YesWeHack Medium |
| S1 Low | 1.5 | HackerOne Low / CVSS 0.1–3.9 / Bugcrowd P4/P5 / Intigriti Low / Immunefi Low / YesWeHack Low |
| S0 None | 0.0 | informational only / policy-only issue / non-exploitable issue / no verifiable security impact |

### 严重性优先级规则（Severity Precedence Rule）

| 情况 | 采用方式 |
|---|---|
| 平台已提供 severity / priority | 优先采用平台定义 |
| 平台未提供等级 | 由 tutor 基于证据推导 CVSS v3.1 严重性 |
| 仅有学生自述严重性 | 若缺乏充分证据，严重性最高不得超过 **S2** |

### 官方校准参考资料
- HackerOne severity taxonomy  
  https://docs.hackerone.com/en/articles/8494552-severity-taxonomy
- Bugcrowd VRT and priority model  
  https://www.bugcrowd.com/vulnerability-rating-taxonomy/
- Intigriti triage standards  
  https://kb.intigriti.com/en/articles/4917102-intigriti-triage-standards
- Immunefi vulnerability severity classification  
  https://immunefi.com/immunefi-vulnerability-severity-classification-system-v2-3/
- YesWeHack programs severity/CVSS guidance  
  https://yeswehack.com/programs

---

## 六、Recorded Presentation & Q&A（3 分）

| 等级 | 分数 | 要求 |
|---|---:|---|
| Exceeds Expectations | 3 | 演示视频长度不超过 5 分钟；清楚说明最高影响力 finding 的 target、root cause、impact、impact justification 与 mitigation；Q&A 回答基于证据，逻辑清晰且有说服力 |
| Meets Expectations | 2 | 演示与 Q&A 整体清楚，但在影响论证或技术深度方面存在轻微不足 |
| Developing | 1 | 演示表达不够清晰，缺少关键技术或影响证据；Q&A 表现出理解有限 |
| Incomplete | 0 | 未提交演示，或录制文件不可用 |

---

## 七、重要规则与扣分项

## 1）Primary Grading Rule

Assignment 2 的主要评分依据为团队提交的**单个最高影响力 valid in-the-wild finding**。若团队未提交有效真实世界 finding，则**Assignment 2 最高总分为 15/25**。

## 2）Zero-Day Priority Rule

已确认的 non-zero-day finding 仍可得分；但 **Part B 最高档（9–10 分）仅保留给具备充分证据支撑的 zero-day candidate**。

## 3）超时罚分（Timing Penalty）

| 规则 | 后果 |
|---|---|
| presentation 必须 ≤ 5 分钟 | 硬性要求 |
| 每超出 10 秒 | **Assignment 2 总分扣 1 分（/25）** |

## 4）发言公平规则（Updated 27 Feb）

在 5 分钟演示中，每位组员必须发言 **至少 40 秒**。若某成员实际发言时间 `t < 40` 秒，则其个人 Assignment 2 分数按下式计算：

**⌊ M × t / 40 ⌋**

其中：
- `M` 为团队 Assignment 2 分数
- `t` 为该成员实际发言秒数
- `⌊ ⌋` 表示向下取整

### 示例
若团队分数 `M = 14`，某成员仅发言 `39` 秒，则该成员个人分数为：

`⌊14 × 39 / 40⌋ = ⌊13.65⌋ = 13`

## 5）贡献公平规则（Updated 27 Feb）

团队必须提交 **`activity-log.pdf`**，用于说明各成员贡献。若 tutor 认为团队成员贡献明显不均衡，可据此调整个人成绩。

## 6）Part A 报告合规罚分

| 情况 | 后果 |
|---|---|
| Part A 报告超过 2 页 | **扣 3 分** |
| Part A 报告未使用官方 USENIX conference paper template | **扣 3 分** |

> 该罚分规则仅适用于 **Part A report**。

---

## 八、证据来源规则（Evidence Source Rule）

| 部分 | 主要评分依据 |
|---|---|
| Part A | **Part A 的 USENIX 报告**，辅以 tutor 在 Q&A 中的澄清判断 |
| Part B | **Part B presentation + tutorial Q&A only** |

### 特别说明
- Part B **不设单独 written report 补充分数**。
- 因此，Part B 的关键证据、严重性映射、影响论证与新颖性说明，必须能够在演示与问答中清晰呈现。

---

## 九、Tutorial 时间安排（严格执行）

| 环节 | 时间 |
|---|---:|
| Presentation 播放 | 5 分钟 |
| Tutor Q&A | 最多 5 分钟 |
| 总时长 | **最多 10 分钟，严格限制** |

---

## 十、Bonus Policy

| 奖励内容 | 条件 |
|---|---|
| +10 bonus | 严重性最高的前 3 个团队 |
| +10 bonus | 漏洞数量最多的前 3 个团队 |
| 总成绩上限 | 100 |
| 额外奖励 | Top 1 team（最高影响力有效漏洞）将获邀与 unit coordinator 共进午餐 |

> 最多共有 6 个团队可获得上述 bonus。

---

## 十一、建议的交付清单

## Part A

| 项目 | 是否建议/必须 | 说明 |
|---|---|---|
| USENIX 模板报告 | 必须 | 且须控制在 **2 页以内** |
| 系统模型图 | 强烈建议 | 应清晰体现信任边界与关键数据流 |
| 威胁模型 | 必须 | 明确 attacker、assets、boundaries |
| 漏洞证据 | 必须 | 代码、配置、抓包或行为证据至少应具备一类，建议多源支撑 |
| Impact reasoning | 必须 | 应构成完整因果链条 |
| Mitigation | 必须 | 应针对 root cause |

## Part B

| 项目 | 是否建议/必须 | 说明 |
|---|---|---|
| 最高影响力真实世界 finding | 必须 | Part B 核心评分对象 |
| target URL | 必须 | 明确目标 |
| scope proof | 必须 | 证明目标在合法范围内 |
| legal boundaries 说明 | 必须 | 说明行为未越界 |
| reproducible steps | 必须 | 可复现性必须明确 |
| finding classification | 必须 | 标明 non-zero-day 或 zero-day candidate |
| severity mapping | 必须 | 优先平台等级，不足时用 CVSS |
| impact evidence | 必须 | 指出受影响资产、后果与 exploit path |
| novelty evidence | 强烈建议 | 是冲击高分的重要因素 |

## Presentation / Tutorial

| 项目 | 是否建议/必须 | 说明 |
|---|---|---|
| ≤5 分钟录制视频 | 必须 | 超时会直接扣分 |
| 每位成员 ≥40 秒发言 | 必须 | 关系到个人成绩 |
| Q&A 准备 | 必须 | 回答需 evidence-based |
| activity-log.pdf | 必须 | 用于贡献公平性判断 |

---

## 十二、建议执行顺序

| 阶段 | 任务目标 | 建议产出 |
|---|---|---|
| 第 1 阶段 | 完成 Part A 系统分析框架 | 系统模型、威胁模型、主要网络弱点初稿 |
| 第 2 阶段 | 锁定 Part B 真实世界 finding | target、scope、复现路径、影响证据 |
| 第 3 阶段 | 完成严重性与新颖性映射 | severity mapping、impact evidence、novelty evidence |
| 第 4 阶段 | 组织 presentation 内容 | 5 分钟脚本、演讲分配、可视化材料 |
| 第 5 阶段 | 完成 Q&A 彩排 | 针对 tutor 可能提问准备证据型回答 |
| 第 6 阶段 | 提交前检查合规性 | 时长、页数、模板、发言时长、activity-log |

---

## 十三、当前仓库已包含的课程文件

| 文件 | 说明 |
|---|---|
| `a2_case1.apk` | Assignment 相关 APK 样本 |
| `assignment1-spec.pdf` | Assignment 1 说明文档 |
| `assignment2-rubric-1.pdf` | Assignment 2 Rubric 原始 PDF |

### 建议后续补充的文件

| 文件/目录 | 用途 |
|---|---|
| `activity-log.pdf` | 成员贡献说明 |
| `report.pdf` | Part A 正式报告 |
| `presentation.mp4` | 最终演示视频 |
| `presentation-script.md` | 演讲稿与发言分配 |
| `finding-evidence/` | Part B 截图、抓包、复现材料 |
| `severity-mapping.md` | 严重性、新颖性与影响论证 |

---

## 十四、总结

Assignment 2 的本质并非单纯撰写一份课程报告，而是要求团队：

1. 在 **Part A** 中展示系统化的网络安全分析能力；
2. 在 **Part B** 中提交一个**合法、有效、可验证、影响明确且尽可能高价值**的真实世界 finding；
3. 以正式、合规、证据充分的方式完成展示与答辩。

团队若希望获得高分，应将重点放在：**finding 的合法性、证据完整性、影响论证质量、严重性映射准确性，以及 zero-day candidate 的充分支撑**。
