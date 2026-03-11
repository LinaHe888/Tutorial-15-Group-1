# INFO5995 Assignment 2：Rubric 与交付要求（中文正式版）

> 作业主题：**AI-Assisted Network Security Analysis & Bug Bounty Exploration**  
> 总分：**25 分**  
> 截止时间：**Mon May 11, 2026 5:00pm**  
> 主要评分依据：**团队提交的单个最高影响力、有效、真实世界（in-the-wild）漏洞**  
> 最高优先级：**具备充分证据支撑的 zero-day candidate**

---

## 当前状态说明

当前仓库已整理 Assignment 2 的 Spec 与 Rubric，供后续参考。
考虑到当前阶段优先完成 **Assignment 1**，Assignment 2 相关工作可暂缓推进，待 Assignment 1 提交后再集中展开。

---

## 一、作业定位与评分导向

Assignment 2 包含 **Part A（系统与网络安全分析）** 与 **Part B（真实世界漏洞发现与影响论证）** 两部分。评分强调团队是否能够：

1. 建立正确的系统与威胁模型；
2. 识别主要网络安全弱点并给出可信证据；
3. 提出合理的修复方案；
4. 在合法范围内提交一个高质量、可验证、影响明确的真实世界 finding；
5. 通过 presentation 与 Q&A 清晰说明影响、严重性、新颖性与修复建议。

### 总体理解

| 项目 | 说明 |
|---|---|
| Part A | 展示系统化安全分析能力 |
| Part B | 展示真实世界 finding 的识别、验证与论证能力 |
| 评分重点 | 团队提交的**最高影响力 valid in-the-wild finding** |
| 高分关键 | scope 合法、证据充分、影响清晰、严重性映射准确、zero-day candidacy 支撑充分 |
| 重要限制 | 未提交有效 in-the-wild finding 时，**总分最高只能达到 15/25** |

---

## 二、评分结构总览

| 部分 | 内容 | 分值 | 评价重点 |
|---|---:|---:|---|
| Part A | System & Threat Model | 3 | 模型、资产、攻击者与 trust boundary 是否清楚 |
| Part A | Vulnerability Discovery & Impact Reasoning | 5 | 是否找对主要网络弱点，证据与影响分析是否充分 |
| Part A | Mitigation | 2 | 修复方案是否技术正确并针对根因 |
| Part B | In-the-Wild Finding Quality & Scope Handling | 2 | finding 是否真实、合法、在 scope 内、可复现 |
| Part B | Highest-Impact Finding (Zero-Day Prioritised) | 10 | 严重性、影响证据、新颖性 |
| Presentation | Recorded Presentation & Q&A | 3 | 演示表达与问答质量 |
| **总计** |  | **25** |  |

---

## 三、Part A 评分标准

## 1）System & Threat Model（3 分）

| 等级 | 分数 | 要求 |
|---|---:|---|
| Exceeds Expectations | 3 | app / network model 清晰；on-path attacker 能力现实；攻击者能力与 protected assets / trust boundaries 关联明确；有 Part A report 证据支持 |
| Meets Expectations | 2 | 模型总体正确，但在攻击者能力或资产关联方面存在轻微不足 |
| Developing | 1 | 模型部分成立，但攻击者描述模糊或与资产联系较弱 |
| Incomplete | 0 | 缺失或明显错误 |

## 2）Vulnerability Discovery & Impact Reasoning（5 分）

| 等级 | 分数 | 要求 |
|---|---:|---|
| Exceeds Expectations | 5 | 正确识别主要网络弱点；具备强代码/配置证据；影响分析清晰可信 |
| Meets Expectations | 3–4 | 识别结果基本正确；证据较合理；影响分析总体可信 |
| Developing | 1–2 | 仅识别部分问题、次要问题，或影响分析偏猜测 |
| Incomplete | 0 | 漏洞 claim 无效 |

## 3）Mitigation（2 分）

| 等级 | 分数 | 要求 |
|---|---:|---|
| Exceeds Expectations | 2 | 修复方案具体、技术上成立、解决根因，并明确解释为何能降低风险 |
| Meets Expectations | 1.5 | 修复方向正确，但存在细节不足 |
| Developing | 0.5–1 | 修复笼统、片面或未针对根因 |
| Incomplete | 0 | 修复无效或未提供 |

---

## 四、Part B 评分标准

## 4）In-the-Wild Finding Quality & Scope Handling（2 分）

| 等级 | 分数 | 要求 |
|---|---:|---|
| Exceeds Expectations | 2 | finding 来自合法 bug bounty program，或作业明确允许的 company-run program；提交中包含 target URL、scope evidence、legal boundaries、reproducible steps、finding-type classification |
| Meets Expectations | 1.5 | finding 基本有效、基本在 scope 内，但有一项内容不完整 |
| Developing | 0.5–1 | scope、复现、分类或证据说明较弱 |
| Incomplete | 0 | finding 无效、不可验证、超出 scope，或来自未经批准项目 |

### 作业明确允许的 company-run program

| 项目 | 状态 |
|---|---|
| Google | 明确允许 |
| Meta | 明确允许 |
| Microsoft | 明确允许 |
| Apple | 明确允许 |
| GitHub | 明确允许 |
| OpenClaw | 明确允许 |
| Anthropic | 明确允许 |
| OpenAI | 明确允许 |

### 特别说明
若目标不属于合法 bug bounty 项目，且也不在以上列表中，则必须取得 **unit coordinator 的 prior written approval**。

---

## 5）Highest-Impact Finding（10 分，Zero-Day Prioritised）

| 等级 | 分数 | 要求 |
|---|---:|---|
| 顶档 | 9–10 | normalized severity = **S4 Critical**；impact evidence 强；平台严重性或 CVSS 映射明确；novelty evidence 强烈支持 zero-day candidate |
| 高档 | 6–8 | normalized severity = **S3 High**，或虽达 S4 但证据/新颖性存在明显不足 |
| 中低档 | 1–5 | normalized severity 仅为 **S1–S2**，或严重性/影响映射较弱 |
| 无效 | 0 | 没有有效 in-the-wild finding |

### 客观评分模型（10 分）

| 组成 | 分值范围 | 说明 |
|---|---:|---|
| Severity score | 0–6 | 将最高影响力 finding 映射到 S0–S4 |
| Impact-evidence score | 0–2 | exploit path、受影响资产/数据、现实后果是否有清楚证据 |
| Novelty score | 0–2 | 是否为有充分证据支撑的 zero-day candidate |
| **总分** | **0–10** | 三部分相加，封顶 10 |

---

## 五、严重性映射（Normalized Severity Tiers）

| 等级 | 分值 | 对应标准 |
|---|---:|---|
| S4 Critical | 6.0 | HackerOne Critical / CVSS ≥ 9.0 / Bugcrowd P1 / Intigriti Critical / Immunefi Critical / YesWeHack Critical |
| S3 High | 4.5 | HackerOne High / CVSS 7.0–8.9 / Bugcrowd P2 / Intigriti High / Immunefi High / YesWeHack High |
| S2 Medium | 3.0 | HackerOne Medium / CVSS 4.0–6.9 / Bugcrowd P3 / Intigriti Medium / Immunefi Medium / YesWeHack Medium |
| S1 Low | 1.5 | HackerOne Low / CVSS 0.1–3.9 / Bugcrowd P4/P5 / Intigriti Low / Immunefi Low / YesWeHack Low |
| S0 None | 0.0 | informational only、policy-only concern、non-exploitable issue、或无可验证安全影响 |

### 严重性优先级规则（Severity Precedence Rule）

| 情况 | 采用方式 |
|---|---|
| 平台已给出 severity / priority | 优先采用平台等级 |
| 平台未给出 | tutor 根据 demonstrated evidence 推导 CVSS v3.1 |
| 仅有学生主张 | 若证据不足，严重性不得高于 **S2** |

### 官方参考链接
- HackerOne severity taxonomy  
  https://docs.hackerone.com/en/articles/8494552-severity-taxonomy
- Bugcrowd VRT and priority model  
  https://www.bugcrowd.com/vulnerability-rating-taxonomy/
- Intigriti triage standards  
  https://kb.intigriti.com/en/articles/4917102-intigriti-triage-standards
- Immunefi vulnerability severity classification  
  https://immunefi.com/immunefi-vulnerability-severity-classification-system-v2-3/
- YesWeHack programs  
  https://yeswehack.com/programs

---

## 六、Recorded Presentation & Q&A（3 分）

| 等级 | 分数 | 要求 |
|---|---:|---|
| Exceeds Expectations | 3 | 录制视频 ≤ 5 分钟；清楚说明 highest-impact finding 的 target、root cause、impact、impact justification 与 mitigation；Q&A 回答 evidence-based 且有说服力 |
| Meets Expectations | 2 | 演示与问答总体清楚，但 impact justification 或技术深度略有不足 |
| Developing | 1 | 演示不够清晰、缺少关键技术/影响证据；Q&A 显示理解有限 |
| Incomplete | 0 | 未提交，或录制不可用 |

---

## 七、关键规则与罚分项

## 1）Primary Grading Rule
Assignment 2 主要依据团队的**单个最高影响力 valid in-the-wild finding**评分。若没有有效真实世界 finding，最高只能得 **15/25**。

## 2）Zero-Day Priority Rule
confirmed non-zero-day finding 可以得分，但 **Part B 顶档（9–10）仅保留给 well-supported zero-day candidates**。

## 3）Timing Penalty

| 规则 | 后果 |
|---|---|
| presentation 必须 ≤ 5 分钟 | 硬性要求 |
| 每超出 10 秒 | Assignment 2 扣 1 分（/25） |

## 4）Presentation Fairness（Updated 27 Feb）
在 5 分钟 presentation 中，每位组员至少发言 **40 秒**。若某成员发言时间 `t < 40` 秒，则其个人成绩为：

**⌊ M × t / 40 ⌋**

其中：
- `M` 为团队 Assignment 2 分数
- `t` 为该成员发言秒数

### 示例
若 `M = 14` 且 `t = 39`，则个人分为：

`⌊14 × 39 / 40⌋ = ⌊13.65⌋ = 13`

## 5）Contribution Fairness（Updated 27 Feb）
团队必须提交 **`activity-log.pdf`**，说明各成员贡献。若贡献不大致均衡，tutor 可调整个人分数。

## 6）Report Compliance Penalty（仅适用于 Part A report）

| 情况 | 后果 |
|---|---|
| 报告超过 2 页 | 扣 3 分 |
| 未使用官方 USENIX template | 扣 3 分 |

---

## 八、证据来源规则（Evidence Source Rule）

| 部分 | 评分依据 |
|---|---|
| Part A | Part A USENIX report + tutor Q&A clarifications |
| Part B | Part B presentation + tutorial Q&A only |

### 重要说明
Part B **没有单独 written report** 用于补充分数，因此关键证据与论证必须直接体现在 presentation 与 Q&A 中。

---

## 九、Tutorial 时间安排

| 环节 | 时间 |
|---|---:|
| Presentation playback | 5 分钟 |
| Q&A | 最多 5 分钟 |
| 总时长 | 最多 10 分钟（严格） |

---

## 十、Bonus Policy

| 项目 | 说明 |
|---|---|
| Top 3 highest severity vulnerabilities | +10 bonus |
| Top 3 highest number of vulnerabilities | +10 bonus |
| 成绩上限 | 100 |
| Top 1 team | 将获邀与 unit coordinator 共进午餐 |

> 共最多 6 个团队获得 bonus。

---

## 十一、建议交付清单

| 项目 | 是否必须 | 说明 |
|---|---|---|
| Part A USENIX 报告 | 必须 | ≤ 2 页 |
| target / scope / legal boundary 说明 | 必须 | Part B 合规基础 |
| reproducible steps | 必须 | 体现 finding 可验证性 |
| severity mapping | 必须 | 平台等级优先，必要时 CVSS fallback |
| impact evidence | 必须 | 资产、数据、后果、利用链 |
| novelty evidence | 强烈建议 | 冲击高分关键 |
| presentation.mp4 | 必须 | ≤ 5 分钟 |
| activity-log.pdf | 必须 | 成员贡献说明 |

---

## 十二、总结

Assignment 2 的评分逻辑明确指向以下能力：
- 是否能够围绕真实系统建立完整威胁模型；
- 是否能够识别并论证主要网络安全弱点；
- 是否能够在合法范围内提交高质量真实世界 finding；
- 是否能够以正式、精确、证据充分的方式完成 presentation 与 Q&A。

