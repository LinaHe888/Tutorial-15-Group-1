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
- **D4**：视频录制 + 全组答辩彩排 + 最终提交检查
---

# INFO5995 Assignment 2：评分标准与交付要求（中文精排版完整版）

> 作业主题：**AI-Assisted Network Security Analysis & Bug Bounty Exploration**  
> 总分：**25 分**  
> 评分核心：**以团队提交的单个最高影响力、有效、真实世界（in-the-wild）漏洞为主**  
> 最高优先级：**有充分证据支撑的 zero-day candidate**

---

## 一、先看结论：Assignment 2 到底怎么拿分

### 核心结论

| 项目 | 你需要理解的重点 |
|---|---|
| Part A | 证明你会做**系统化网络安全分析**：系统模型、漏洞、影响、修复 |
| Part B | 证明你能找到**真实世界、合法、可验证、尽量高影响**的漏洞 |
| 评分重心 | 主要看 **单个最高影响力 finding**，不是看你写了多少字 |
| 冲高分关键 | **zero-day candidate + 强证据 + 清楚 impact + 合法 scope** |
| 致命风险 | 没有有效 in-the-wild finding 时，**总分最高只能 15/25** |

---

## 二、Assignment 2 整体结构

| 部分 | 内容 | 分值 | 老师看什么 |
|---|---:|---:|---|
| Part A | System & Threat Model | 3 | 模型是否清楚、攻击者是否合理、资产与 trust boundary 是否讲明白 |
| Part A | Vulnerability Discovery & Impact Reasoning | 5 | 是否找对主要网络弱点，证据是否充分，影响分析是否可信 |
| Part A | Mitigation | 2 | 修复是否解决根因，是否真的降低风险 |
| Part B | In-the-Wild Finding Quality & Scope Handling | 2 | finding 是否真实、合法、在 scope 内、可复现 |
| Part B | Highest-Impact Finding (Zero-Day Prioritised) | 10 | 严重性、影响证据、新颖性（zero-day 优先） |
| Presentation | Recorded Presentation & Q&A | 3 | 讲解是否清楚，Q&A 是否有证据支撑 |
| **总计** |  | **25** |  |

---

## 三、Part A 评分细则（逐项看懂）

## 1）System & Threat Model（3 分）

| 档位 | 分数 | 标准 |
|---|---:|---|
| Exceeds Expectations | 3 | app / network model 清晰；攻击者模型现实；攻击者能力与资产、信任边界明确关联；能从 Part A report 中直接看到证据 |
| Meets Expectations | 2 | 模型基本正确，但攻击者能力、资产或 trust boundary 之间仍有小缺口 |
| Developing | 1 | 只做出部分模型；攻击者描述模糊；与资产关联弱 |
| Incomplete | 0 | 缺失或整体错误 |

### 老师想看到的内容
- app / network components 是什么
- 谁是 attacker
- attacker 能做到什么
- 资产是什么（token、session、credential、敏感数据等）
- 信任边界在哪里
- 哪些数据跨边界流动

---

## 2）Vulnerability Discovery & Impact Reasoning（5 分）

| 档位 | 分数 | 标准 |
|---|---:|---|
| Exceeds Expectations | 5 | 找到了**正确的主要网络弱点**；代码/配置/行为证据强；impact reasoning 清楚、可信、完整 |
| Meets Expectations | 3–4 | 问题基本找对；证据较合理；影响分析大体可信 |
| Developing | 1–2 | 只发现了部分问题，或是次要问题；影响分析偏猜测 |
| Incomplete | 0 | 漏洞 claim 无效 |

### 老师最在意的证据
- 配置错误（如证书校验、明文传输、弱 TLS 配置等）
- 代码证据（请求层、验证逻辑、异常处理）
- 抓包/行为证据
- 影响链条是否完整：**弱点 → 可利用条件 → 后果**

---

## 3）Mitigation（2 分）

| 档位 | 分数 | 标准 |
|---|---:|---|
| Exceeds Expectations | 2 | 修复方案具体、技术上正确、能解决 root cause，并解释为什么风险下降 |
| Meets Expectations | 1.5 | 修复大体正确，但仍有小缺口 |
| Developing | 0.5–1 | 修复模糊、片面，或没针对根因 |
| Incomplete | 0 | 修复无效或没写 |

### 好 mitigation 应该长这样
- 不是“加强安全性”这种口号
- 要写**改什么、在哪改、为什么有效**
- 最好能说明残余风险是否仍存在

---

## 四、Part B 评分细则（这是高分关键）

## 4）In-the-Wild Finding Quality & Scope Handling（2 分）

| 档位 | 分数 | 标准 |
|---|---:|---|
| Exceeds Expectations | 2 | finding 来自合法 bug bounty 或作业允许的公司项目；包含 target URL、scope evidence、legal boundaries、repro steps、finding type classification |
| Meets Expectations | 1.5 | finding 基本有效、基本在 scope 内，但 scope proof / reproducibility / classification 有一项不够完整 |
| Developing | 0.5–1 | 证据不够，scope 不清，复现或分类细节弱 |
| Incomplete | 0 | out-of-scope、不可验证、无效 finding、或来自未批准项目 |

### 允许的 company-run program（作业明确列出）

| 已明确允许 | 备注 |
|---|---|
| Google | 合法 program 内 |
| Meta | 合法 program 内 |
| Microsoft | 合法 program 内 |
| Apple | 合法 program 内 |
| GitHub | 合法 program 内 |
| OpenClaw | 合法 program 内 |
| Anthropic | 合法 program 内 |
| OpenAI | 合法 program 内 |

### 重要限制
- 如果目标不是合法 bug bounty program，也不在上表里：
  - **必须先拿到 unit coordinator 的书面批准**
- 否则可能直接 **0 分**

---

## 5）Highest-Impact Finding（10 分，zero-day 优先）

| 档位 | 分数 | 标准 |
|---|---:|---|
| 顶档 | 9–10 | normalized severity = **S4 Critical**；impact evidence 很强；severity mapping 清晰；novelty evidence 强，足以支持 zero-day candidate |
| 高档 | 6–8 | normalized severity = **S3 High**；或虽然到 S4 但在证据/新颖性上仍有缺口 |
| 中低档 | 1–5 | normalized severity 只有 **S1–S2**；或严重性/影响映射较弱 |
| 无效 | 0 | 没有有效 in-the-wild finding |

### 这一项实际上怎么打分
Part B 的 10 分不是拍脑袋给，而是按下面公式拆开：

| 组成 | 分值范围 | 说明 |
|---|---:|---|
| Severity score | 0–6 | 按 S0–S4 映射 |
| Impact-evidence score | 0–2 | exploit path、受影响资产/数据、现实后果是否清楚 |
| Novelty score | 0–2 | 是否是有证据支撑的 zero-day candidate |
| **总分** | **0–10** | 三项相加，封顶 10 |

---

## 五、严重性映射（Normalized Severity Tiers）

| 级别 | 分数 | 对应标准 |
|---|---:|---|
| S4 Critical | 6.0 | HackerOne Critical / CVSS ≥ 9.0 / Bugcrowd P1 / Intigriti Critical / Immunefi Critical / YesWeHack Critical |
| S3 High | 4.5 | HackerOne High / CVSS 7.0–8.9 / Bugcrowd P2 / Intigriti High / Immunefi High / YesWeHack High |
| S2 Medium | 3.0 | HackerOne Medium / CVSS 4.0–6.9 / Bugcrowd P3 / Intigriti Medium / Immunefi Medium / YesWeHack Medium |
| S1 Low | 1.5 | HackerOne Low / CVSS 0.1–3.9 / Bugcrowd P4/P5 / Intigriti Low / Immunefi Low / YesWeHack Low |
| S0 None | 0.0 | informational only / policy-only / no exploitable impact |

### 严重性优先级规则（Severity Precedence Rule）

| 情况 | 采用方式 |
|---|---|
| 平台已有 severity / priority | **优先采用平台等级** |
| 平台没有给等级 | tutor 根据证据推导 **CVSS v3.1** |
| 只有学生自称“critical” | 如果证据不够，**最高不能超过 S2** |

### 官方校准参考（老师会参考）
- HackerOne severity taxonomy  
  https://docs.hackerone.com/en/articles/8494552-severity-taxonomy
- Bugcrowd VRT and priority model  
  https://www.bugcrowd.com/vulnerability-rating-taxonomy/
- Intigriti triage standards  
  https://kb.intigriti.com/en/articles/4917102-intigriti-triage-standards
- Immunefi vulnerability severity classification  
  https://immunefi.com/immunefi-vulnerability-severity-classification-system-v2-3/
- YesWeHack severity/CVSS guidance  
  https://yeswehack.com/programs

---

## 六、Presentation 与 Q&A 评分（3 分）

| 档位 | 分数 | 标准 |
|---|---:|---|
| Exceeds Expectations | 3 | ≤5 分钟，清楚讲明 target、root cause、impact、impact justification、mitigation；Q&A 回答有证据支撑且有说服力 |
| Meets Expectations | 2 | 整体清楚，但 impact justification 或技术深度略有不足 |
| Developing | 1 | presentation 难跟、缺少关键技术/影响证据；Q&A 理解有限 |
| Incomplete | 0 | 没交，或录制不可用 |

---

## 七、最容易丢分的地方（务必看）

## 1）没有有效 in-the-wild finding

| 情况 | 后果 |
|---|---|
| 没有提交有效真实世界 finding | **总分最高只能 15/25** |

## 2）zero-day priority rule

| 情况 | 后果 |
|---|---|
| confirmed non-zero-day | 可以得分 |
| 想冲 Part B 顶档（9–10） | 必须是**有充分证据支撑的 zero-day candidate** |

## 3）超时罚分（Timing Penalty）

| 规则 | 后果 |
|---|---|
| presentation 必须 ≤ 5 分钟 | 硬规则 |
| 每多 10 秒 | **扣 1 分（/25）** |

> 不是“差不多就行”，是真的每超 10 秒扣 1 分。

## 4）每个成员必须说够 40 秒（Updated 27 Feb）

| 规则 | 后果 |
|---|---|
| 每个成员在 5 分钟视频里至少讲 40 秒 | 否则个人分单独被降 |
| 个人分公式 | **⌊ M × t / 40 ⌋** |
| M | 团队 Assignment 2 分数 |
| t | 该学生实际发言秒数 |

### 例子
如果：
- 团队分数 `M = 14`
- 某同学只讲了 `39` 秒

那么这个同学的分数：
- `⌊14 × 39 / 40⌋ = ⌊13.65⌋ = 13`

## 5）贡献不均衡（Updated 27 Feb）

| 要求 | 后果 |
|---|---|
| 必须提交 `activity-log.pdf` | tutor 用来判断成员贡献 |
| 如果贡献明显不均衡 | tutor 可调整**个人分数** |

## 6）Part A 报告格式违规

| 情况 | 后果 |
|---|---|
| 报告超过 2 页 | **扣 3 分** |
| 没使用官方 USENIX 模板 | **扣 3 分** |

> 这个罚分只针对 **Part A report**。

---

## 八、证据来源规则（Evidence Source Rule）

| 部分 | 老师主要看什么 |
|---|---|
| Part A | **USENIX 报告** + tutor Q&A clarification |
| Part B | **presentation + tutorial Q&A only** |

### 重要提醒
- **Part B 没有单独 written report 来补分**
- 所以真实世界 finding 的关键证据，要能在：
  - presentation 里讲清楚
  - Q&A 里答出来

---

## 九、Tutorial 现场时间结构（严格）

| 环节 | 时间 |
|---|---:|
| 播放 presentation | 5 分钟 |
| tutor Q&A | 最多 5 分钟 |
| 总时长 | **最多 10 分钟，strict** |

---

## 十、Bonus 政策

| 奖励 | 条件 |
|---|---|
| +10 bonus | 严重性最高的前 3 个团队 |
| +10 bonus | 漏洞数量最多的前 3 个团队 |
| 封顶 | 总成绩最高 100 |
| 额外奖励 | Top 1 team（最高影响力有效漏洞）会被邀请和 unit coordinator 吃 lunch |

> 一共最多 6 个团队拿到这类 bonus。

---

## 十一、老师真正想看到的高分作业长什么样

### Part A 高分特征

| 维度 | 高分标准 |
|---|---|
| 系统模型 | 结构清楚，不空泛 |
| 攻击者模型 | realistic，不乱写超能力 |
| 漏洞证据 | 有代码/配置/行为/抓包支撑 |
| 影响分析 | 从弱点到后果链条完整 |
| 修复方案 | 修 root cause，不喊口号 |

### Part B 高分特征

| 维度 | 高分标准 |
|---|---|
| finding 来源 | 合法、在 scope 内、可复现 |
| 严重性 | 尽量往 S3 / S4 走 |
| 影响证据 | exploit path、受影响资产、现实后果都讲清楚 |
| 新颖性 | 能证明是 zero-day candidate 最好 |
| 表达方式 | presentation 和 Q&A 证据驱动 |

---

## 十二、最实用的提交清单（照这个准备最稳）

## Part A 提交准备

| 项目 | 是否必须 | 说明 |
|---|---|---|
| USENIX 模板报告 | 必须 | 且 **≤ 2 页** |
| 系统模型图 | 强烈建议 | 最好画清楚 trust boundary |
| 威胁模型 | 必须 | 明确 attacker、asset、boundary |
| 网络弱点证据 | 必须 | 代码/配置/流量至少一种，最好多种 |
| impact reasoning | 必须 | 讲清 exploit path 与现实后果 |
| mitigation | 必须 | 解决根因 |

## Part B 提交准备

| 项目 | 是否必须 | 说明 |
|---|---|---|
| 最高影响力真实世界 finding | 必须 | 核心评分来源 |
| target URL | 必须 | 明确目标 |
| scope proof | 必须 | 证明合法且在范围内 |
| legal boundary | 必须 | 说明你没有越界 |
| reproducible steps | 必须 | tutor 要能理解复现 |
| finding classification | 必须 | non-zero-day / zero-day candidate |
| severity mapping | 必须 | 平台等级优先，没有就 CVSS |
| impact evidence | 必须 | 数据/资产/后果 |
| novelty evidence | 强烈建议 | 冲高分关键 |

## Presentation / Tutorial 准备

| 项目 | 是否必须 | 说明 |
|---|---|---|
| ≤5 分钟视频 | 必须 | 超时扣分 |
| 每人 ≥40 秒发言 | 必须 | 否则个人分下降 |
| Q&A 准备 | 必须 | 要能用证据回答 |
| activity-log.pdf | 必须 | 用于 contribution fairness |

---

## 十三、建议执行顺序（很适合小组照做）

| 阶段 | 目标 | 产出 |
|---|---|---|
| 第 1 步 | 先锁定 Part A | 系统模型、威胁模型、主要网络弱点 |
| 第 2 步 | 再冲 Part B | 合法真实世界 finding + scope + 复现 |
| 第 3 步 | 做 severity / impact / novelty 映射 | 确定是否能冲高分 |
| 第 4 步 | 压缩成 presentation | 确保 5 分钟内讲清楚 |
| 第 5 步 | 练 Q&A | 准备 evidence-based 回答 |
| 第 6 步 | 检查 fairness / compliance | 每人 40 秒、activity-log、Part A 报告页数 |

---

## 十四、当前仓库里已经有什么

| 文件 | 说明 |
|---|---|
| `a2_case1.apk` | Assignment 相关 APK 样本 |
| `assignment1-spec.pdf` | Assignment 1 说明 |
| `assignment2-rubric-1.pdf` | Assignment 2 Rubric 原始 PDF |

### 建议后续继续补充

| 建议新增文件 | 用途 |
|---|---|
| `activity-log.pdf` | 成员贡献说明 |
| `report.pdf` | Part A 正式报告 |
| `presentation.mp4` | 最终演示视频 |
| `presentation-script.md` | 演讲稿 / 分工台词 |
| `finding-evidence/` | Part B 的截图、抓包、复现材料 |
| `severity-mapping.md` | 平台等级 / CVSS / novelty 论证 |

---

## 十五、最后一句话总结

**Assignment 2 不是普通课程作业，而是“Part A 证明你懂系统化安全分析，Part B 证明你能在合法真实环境中找到高价值漏洞”的组合题。真正拉开分差的，是你们提交的那个最高影响力真实世界 finding。**
