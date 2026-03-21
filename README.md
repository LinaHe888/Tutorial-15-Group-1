# INFO5995 小组作业仓库

本仓库用于 INFO5995 课程小组作业，目前主要推进 **Assignment 1**。

---

## 当前进度（main 分支）

| 任务 | 状态 | 说明 |
|---|---|---|
| Task 1 — APK 反编译 | ✅ 已完成 | APK 解包、manifest 分析、Login 类定位 |
| Task 2 — 系统模型 | ✅ 已完成 | App 组件、数据流、信任边界 |
| Task 3 — 漏洞发现 | ✅ 已完成 | 主漏洞：Session Token 使用 `java.util.Random` 预测性问题 |
| Task 4 — 威胁模型 | ✅ 已完成 | 攻击路径、影响分析 |
| Task 5 — 报告与演示 | ⏳ 待完成 | 最终报告、Presentation |

**主漏洞结论：**  
App 在 `Login.generateSessionToken()` 中使用 `java.util.Random` 生成 Session Token，Token 承担安全敏感角色，不应使用弱随机源。

---

## 目录结构

```
submission/
├── task-1-decompilation/          # APK 反编译结果与证据
├── task-2-system-model/          # 系统模型与数据流
├── task-3-vulnerability-discovery/  # 漏洞发现与候选分析
├── task-4-threat-model/           # 威胁模型与攻击路径
└── task-5-report-and-presentation/  # 最终报告与演示（待完成）
```

---

## 关键文件

| 文件 | 内容 |
|---|---|
| `a1_case1.apk` | 待分析的 APK 样本 |
| `assignment1-spec.pdf` | Assignment 1 原始说明 |
| `assignment1-rubric-2.pdf` | Assignment 1 评分标准 |
| `README_1.md` | Assignment 1 完整说明与执行建议 |

---

## Assignment 1 必交文件

- `report.pdf`（最多 2 页，USENIX 模板）
- `ai-log/`
- `pocs/`
- `presentation.mp4`
- `activity-log.pdf`（N×N contribution matrix）

---

## 扣分注意事项

- 缺少任一 required item → 扣 5 分
- 演示超时每 10 秒 → 扣 1 分
- 报告超过 2 页或未用正确模板 → 扣 3 分
- 每位组员发言不足 40 秒 → 按公式折算分数

---

## 截止时间

**Mon Mar 30, 2026 5:00pm**

---

## 提交前检查清单

- [ ] Task 1–4 内容完整、证据齐全
- [ ] 报告使用 USENIX 模板，不超过 2 页
- [ ] 演示总时长不超过 5 分钟
- [ ] 所有 required files 准备齐全
- [ ] `ai-log/` 记录 AI 使用情况
- [ ] `activity-log.pdf` 使用 N×N contribution matrix
- [ ] 每位组员在演示中发言至少 40 秒
