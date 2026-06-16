# OfficePilot AI Reference

## Priority Rules

- `高`: executive priority, customer-impacting, revenue/compliance risk, or due within 7 days.
- `中`: important cross-team work, dependency-heavy, or due within 30 days.
- `低`: exploratory, informational, or no immediate execution impact.

## Risk Levels

- `低风险`: clear owner, clear deadline, recent progress, manageable dependency.
- `中风险`: partial ambiguity, unresolved dependency, stale progress, or unclear deliverable.
- `高风险`: likely delay, missing accountable owner, blocked important work, or repeated postponement.
- `严重风险`: forgotten critical decision, missed major deadline, no accountable owner for urgent work, or likely customer/compliance/executive impact.

## Risk Taxonomy

- 责任风险: no owner, multiple owners, unclear department ownership, responsibility conflict.
- 时间风险: no deadline, overdue, repeated postponement, conflicting milestones.
- 依赖风险: waiting for another team, missing approval, blocked interface/data/resource.
- 质量风险: unclear acceptance criteria, inconsistent data, unvalidated deliverable.
- 信息冲突: conflicting owners, deadlines, statuses, or scope across sources.
- 决策遗忘: decision exists but no follow-up task, owner, progress, or checkpoint.

## Decision Lost Detector Checklist

Flag `疑似被遗忘决策` when most of these are true:

1. A prior source contains a concrete decision.
2. Later sources do not contain a matching task or progress record.
3. The decision has no clear owner or no recent owner update.
4. The decision has no deadline, missed deadline, or no checkpoint.
5. The decision has business impact if ignored.

## Cross-Document Linkage

When multiple documents are provided:

- Match by project name, decision phrase, owner, department, deliverable, and deadline.
- Track whether later documents confirm, update, conflict with, or omit earlier decisions.
- Treat omission of a high-priority decision from later progress reports as a risk signal, not proof of abandonment.
- Label uncertain matches as `推断`.

## Enterprise Execution Index

Compute only when enough data exists:

```text
企业执行指数 = 任务完成率 × 35 + 责任明确率 × 25 + (1 - 延期率) × 20 + (1 - 风险率) × 20
```

Definitions:

- 任务完成率 = 已完成任务数 / 任务总数
- 延期率 = 延期任务数 / 任务总数
- 风险率 = 中风险及以上事项数 / 决策或任务总数
- 责任明确率 = 有唯一明确负责人的任务数 / 任务总数

If any denominator is unavailable, output `信息不足，无法判断企业执行指数`.

## Edge Cases

- 无负责人: output `负责人缺失`; do not invent an owner.
- 无截止时间: output `截止时间未知`; do not guess a deadline.
- 信息冲突: output `责任冲突`, `时间冲突`, or `状态冲突` and cite conflicting evidence.
- 信息不足: output `信息不足，无法判断` and list what is missing.

## Quality Checklist

Before final output, verify:

- The unified sections appear in order: 决策事项, 任务清单, 责任矩阵, 风险分析, 催办建议, 管理驾驶舱.
- Severe and high-risk items are shown before medium and low-risk items.
- Every decision has content, priority, time node or `截止时间未知`, department when available, and source evidence.
- Every task has owner or `负责人缺失`, deadline or `截止时间未知`, and status or `信息不足，无法判断`.
- Decision Lost Detector was considered for each decision with missing follow-up.
- Reminder messages are directly sendable and not accusatory.
- Dashboard counts and execution index match the extracted tables or explain missing inputs.
