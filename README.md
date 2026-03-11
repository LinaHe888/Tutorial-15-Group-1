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

# INFO5995 Assignment 2：评分标准与交付要求（中文完整版）

> 作业主题：**AI-Assisted Network Security Analysis & Bug Bounty Exploration**  
> 总分：**25 分**  
> 核心导向：**Assignment 2 主要看小组提交的“单个最高影响力、有效、真实世界（in-the-wild）漏洞”**。如果没有提交有效的真实世界漏洞，**总分上限为 15/25**。

---

## 一、Assignment 2 到底在考什么

这个作业分成两大块：

### Part A：系统与网络安全分析
老师会看你们是否能：
- 建立一个合理的 app / network system model
- 正确描述攻击者能力、受保护资产、信任边界
- 找出一个正确的网络安全弱点
- 用代码/配置/行为证据解释风险
- 给出能真正降低风险的 mitigation

### Part B：真实世界漏洞（Bug Bounty / Company-Run Program）
老师更看重你们是否能：
- 从**合法 bug bounty program** 或 **作业明确允许的公司项目** 中找到有效漏洞
- 清楚说明目标、scope、复现过程、影响、严重性和新颖性
- 优先奖励**有充分证据支撑的 zero-day candidate**

---

## 二、评分 Rubric（逐项展开）

## Part A: System & Threat Model（3 分）

### 3 分（Exceeds Expectations）
- app / network model 清晰
- 攻击者模型现实且可信，特别是 **on-path attacker**（链路中间人/网络路径上的攻击者）能力描述准确
- 攻击者能力与资产、信任边界之间的关系讲得非常清楚
- 能从 Part A 报告中直接看到证据支撑

### 2 分（Meets Expectations）
- 模型基本正确
- 但攻击者能力或资产关联有一些小缺口

### 1 分（Developing）
- 只有部分模型
- 攻击者描述模糊
- 和资产之间的联系很弱

### 0 分（Incomplete）
- 缺失
- 或整体模型错误

---

## Part A: Vulnerability Discovery & Impact Reasoning（5 分）

### 5 分
- 找到了**正确的主要网络安全弱点**
- 有强证据支撑（代码、配置、行为、网络证据等）
- impact reasoning（影响分析）清晰、完整、可信

### 3–4 分
- 问题找对了
- 证据基本够
- 影响分析大体合理，但还不够顶级

### 1–2 分
- 只找到部分问题，或找的是次要问题
- 影响分析比较弱、比较猜测

### 0 分
- 漏洞 claim 无效
- 漏洞本身不成立

---

## Part A: Mitigation（2 分）

### 2 分
- 修复方案具体、可执行、技术上成立
- 能解决 root cause（根因）
- 能解释为什么这个方案会降低风险

### 1.5 分
- 修复大体正确
- 但仍有一些小缺口

### 0.5–1 分
- 修复模糊
- 或只修了一部分
- 或没有真正针对根因

### 0 分
- 修复无效
- 或完全没写

---

## Part B: In-the-Wild Finding Quality & Scope Handling（2 分）

### 2 分
提交的 finding 必须满足下面这些条件：
- 来自**合法 bug bounty program**，或者作业说明中**明确列出的公司自营项目**
- 作业明确列出的公司项目包括：
  - Google
  - Meta
  - Microsoft
  - Apple
  - GitHub
  - OpenClaw
  - Anthropic
  - OpenAI
- 如果不是上述列出的公司自营项目，必须提前获得 **unit coordinator 的书面批准**
- 提交内容中必须包含：
  - target URL
  - scope evidence（证明目标在 scope 内）
  - legal boundaries（合法边界）
  - reproducible steps（可复现步骤）
  - finding-type classification（类型分类）
    - non-zero-day
    - 或 zero-day candidate

### 1.5 分
- 基本有效、基本在 scope 内
- 但有一项不完整，例如：
  - scope proof 不足
  - 复现细节不完整
  - finding type 的论证不够

### 0.5–1 分
- 证据不完整
- scope 控制不清晰
- 复现性不足
- 分类理由很弱

### 0 分
以下任一情况都可能直接 0 分：
- finding 无效
- 不可验证
- out-of-scope
- 来源不属于合法 bug bounty program
- 来源是未获批准的 company-run program

---

## Part B: Highest-Impact Finding（Zero-Day Prioritised）（10 分）

这是 Assignment 2 最关键的一项。

### 9–10 分
- 归一化严重等级达到 **S4 (Critical)**
- impact evidence 很强
- 严重性映射清楚（平台等级或 CVSS fallback）
- novelty evidence 很强，能充分支持它是 **zero-day candidate**

### 6–8 分
- 严重等级达到 **S3 (High)**
- 或虽然达到 S4，但在证据/新颖性上还有明显缺口
- 影响清楚且可复现，但还没到最顶级档位

### 1–5 分
- 严重等级只是 **S1–S2（Low / Medium）**
- 或者 severity / impact 映射不够强
- 或证据只覆盖一部分

### 0 分
- 没有有效的真实世界 finding

---

## Recorded Presentation & Q&A（3 分）

### 3 分
- 录制视频 **≤ 5 分钟**
- 清楚讲明最高影响力 finding 的：
  - target
  - root cause
  - impact
  - impact justification
  - mitigation
- 回答 Q&A 时有证据支撑，解释有说服力

### 2 分
- presentation 和 Q&A 大部分清楚
- 但 impact justification 或技术深度略有欠缺

### 1 分
- presentation 难以跟上
- 缺少关键技术或影响证据
- Q&A 中表现出理解有限

### 0 分
- 没交 presentation
- 或录制内容不可用

---

## 三、最重要的总规则（Primary Grading Rule）

Assignment 2 的主评分规则是：

- **整个作业主要由你们提交的“单个最高影响力、有效、真实世界漏洞”决定**
- 该漏洞必须来自：
  - 合法 bug bounty program
  - 或作业中明确允许的 company-run programs
  - 其他 company-run targets 必须先拿到 unit coordinator 的书面批准
- **zero-day candidate 有明确优先级**
- 如果没有提交有效 in-the-wild finding，**总分最高只能拿 15/25**

---

## 四、Zero-Day Priority Rule

- confirmed non-zero-day finding 也可以得分
- 但是 **Part B 的最高档（9–10 分）只保留给有充分证据支撑的 zero-day candidate**

换句话说：
- 非零日可以拿不错分数
- 但真正冲顶必须是**有力支持的零日候选**

---

## 五、Part B 的 10 分客观计算模型（Objective Scoring Model）

Part B 的 Highest-Impact Finding 这一项，老师会拆成 3 个子分数：

### 1）Severity score（0–6 分）
根据最高有效 finding 的严重性归一化到 S0–S4：
- S4 Critical = 6.0
- S3 High = 4.5
- S2 Medium = 3.0
- S1 Low = 1.5
- S0 None = 0.0

### 2）Impact-evidence score（0–2 分）
- **2 分**：exploit path、受影响资产/数据、现实后果都很清楚
- **1 分**：只有一部分清楚
- **0 分**：影响主要靠猜

### 3）Novelty score（0–2 分）
- **2 分**：有充分证据支持它是 zero-day candidate
- **1 分**：有效，但已确认是 non-zero-day
- **0 分**：所谓“新颖性”没有证据

### 4）Part B 最终该项分数
- **Severity + Impact-evidence + Novelty**
- 总分 **封顶 10 分**

---

## 六、Normalized Severity Tiers（Part B 用）

## S4 Critical（6.0）
满足以下任一：
- HackerOne Critical
- CVSS ≥ 9.0
- Bugcrowd P1
- Intigriti Critical
- Immunefi Critical
- YesWeHack Critical / CVSS ≥ 9.0
- 或 coordinator 批准的其他项目中的同等级严重性

## S3 High（4.5）
- HackerOne High
- CVSS 7.0–8.9
- Bugcrowd P2
- Intigriti High
- Immunefi High
- YesWeHack High / CVSS 7.0–8.9
- 或 coordinator 批准项目中的等价等级

## S2 Medium（3.0）
- HackerOne Medium
- CVSS 4.0–6.9
- Bugcrowd P3
- Intigriti Medium
- Immunefi Medium
- YesWeHack Medium / CVSS 4.0–6.9

## S1 Low（1.5）
- HackerOne Low
- CVSS 0.1–3.9
- Bugcrowd P4 / P5
- Intigriti Low
- Immunefi Low
- YesWeHack Low / CVSS 0.1–3.9

## S0 None（0.0）
- informational only
- policy-only concern
- non-exploitable issue
- 或没有可验证安全影响

---

## 七、Severity Precedence Rule（严重性优先级规则）

严重性怎么认定？规则如下：

1. **优先使用平台已经给出的 severity / priority**
2. 如果平台没给，就由 tutor 根据证据去推导 **CVSS v3.1 severity**
3. 学生自己声称的严重性，如果证据不够，**最高不能超过 S2**

也就是说：
- 你嘴上说它是 critical 没用
- 必须有平台标注，或足够扎实的证据支持

---

## 八、官方严重性校准参考（老师会参考这些）

- HackerOne severity taxonomy  
  https://docs.hackerone.com/en/articles/8494552-severity-taxonomy

- Bugcrowd VRT and priority model  
  https://www.bugcrowd.com/vulnerability-rating-taxonomy/

- Intigriti triage standards  
  https://kb.intigriti.com/en/articles/4917102-intigriti-triage-standards

- Immunefi vulnerability severity classification  
  https://immunefi.com/immunefi-vulnerability-severity-classification-system-v2-3/

- YesWeHack programs / severity / CVSS guidance  
  https://yeswehack.com/programs

---

## 九、罚分规则（Penalty Rules）

## 1）Presentation 超时罚分
- 演示视频必须 **≤ 5 分钟**
- **每超 10 秒，整个 Assignment 2 直接扣 1 分（/25）**
- 即使你 rubric 本来分很高，也照扣

这是硬规则，不是建议。

---

## 2）Updated 27 Feb：presentation fairness（展示公平规则）

在 5 分钟 presentation 里：
- **每个组员至少要讲 40 秒**

如果某个同学讲话时间 **t < 40 秒**，那这个同学的个人 Assignment 2 分数会变成：

**⌊ M × t / 40 ⌋**

其中：
- `M` = 小组 Assignment 2 分数
- `t` = 该学生实际发言秒数
- `⌊ ⌋` = 向下取整

### 例子
如果：
- 小组分数 `M = 14`
- 某个同学只讲了 `t = 39` 秒

那么这个同学个人分数：
- `⌊14 × 39 / 40⌋ = ⌊13.65⌋ = 13`

也就是说：
- 组分不代表个人一定一样
- 发言不够会被单独砍分

---

## 3）Tutorial 时间格式（严格）

每组 tutorial 总时长最多 **10 分钟**：
- **5 分钟**：播放 presentation
- **最多 5 分钟**：Q&A（如果 tutor 有疑问）

这个时间是 **strict** 的。

---

## 4）Updated 27 Feb：contribution fairness（贡献公平）

团队必须额外提交：
- `activity-log.pdf`

里面要写：
- 每个成员各自做了什么
- 贡献总结

如果 tutor 觉得贡献明显不均衡，**可以调整个人分数**。

---

## 5）Part A 报告合规罚分
这个罚分只针对 **Part A report**：

如果 Part A report：
- 超过 **2 页**
- 或没有使用 **官方 USENIX conference paper template**

则直接：
- **扣 3 分**

---

## 十、证据来源规则（Evidence Source Rule）

这个规则非常关键：

### Part A 怎么评分
- 主要依据：**Part A 的 USENIX 报告**
- tutor 可以结合 tutorial 里的 Q&A clarification 来看

### Part B 怎么评分
- 只看：
  - **Part B presentation**
  - **tutorial Q&A**
- **没有单独的 Part B written report**

也就是说：
- Part A 证据重点在书面报告
- Part B 证据重点在视频和答辩

---

## 十一、Bonus 政策

有额外 bonus：

> “The teams with the top three highest severity vulnerabilities and the top three highest number of vulnerabilities will receive +10 bonus marks, capped at a total of 100 (in total 6 teams).”

翻成中文就是：
- **严重性最高的前 3 个团队**：+10 bonus
- **漏洞数量最多的前 3 个团队**：+10 bonus
- 一共最多 **6 个团队**能拿 bonus
- 总成绩**封顶 100**

另外：
- **Top 1 team（最高影响力有效漏洞）** 会被邀请和 unit coordinator 在 campus 吃 lunch

---

## 十二、老师真正想要你们做到什么（实战理解版）

如果你想冲高分，实际要做到的是：

### Part A
- 系统模型讲清楚
- 攻击者不是乱写，要贴近网络攻击现实
- 漏洞一定要有代码/配置证据
- mitigation 不能喊口号，要解决根因

### Part B
- 漏洞必须真实、合法、可验证、在 scope 内
- 影响必须能讲清楚“攻击路径 → 受影响资产 → 真实后果”
- 严重性要能对上 bounty 平台标准
- 想拿最高档，必须冲**强证据 zero-day candidate**

### Presentation / Q&A
- 视频别超时
- 每个人都要说够 40 秒
- Q&A 一定要证据驱动，不要空口解释

---

## 十三、建议交付清单（按这个准备最稳）

### Part A
- USENIX 模板报告（≤2 页）
- 系统模型图
- 威胁模型
- 网络弱点证据（代码/配置/流量）
- impact reasoning
- mitigation

### Part B
- 一个最高影响力真实世界 finding
- target URL
- scope proof
- legal boundary 说明
- reproducible steps
- finding type（non-zero-day / zero-day candidate）
- severity mapping（平台等级或 CVSS）
- impact evidence
- novelty evidence

### Presentation / Tutorial
- 5 分钟以内视频
- 每人至少 40 秒
- Q&A 准备
- activity-log.pdf

---

## 十四、上传到本仓库的课程文件

当前仓库已包含：

- `a2_case1.apk`
- `assignment1-spec.pdf`
- `assignment2-rubric-1.pdf`

如果后续要继续做完整提交，建议再补：
- `activity-log.pdf`
- Part A 的 USENIX 报告
- presentation script / slides / video 链接
- Part B finding 复现材料

---

## 十五、一句话总结

**Assignment 2 的本质不是“写一份普通报告”，而是：用 Part A 证明你会做系统化安全分析，再用 Part B 拿一个合法、有效、尽量高影响、最好是 zero-day 候选的真实世界漏洞来冲分。**

