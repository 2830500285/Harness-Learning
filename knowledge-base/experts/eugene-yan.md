# Eugene Yan

## Source

- Title: Product Evals in Three Simple Steps
- Date: 2025-11-23
- Link: <https://eugeneyan.com/writing/product-evals/>

## English Summary

Eugene presents a compact workflow for eval harnesses:

1. label a small but useful dataset
2. align LLM evaluators against human judgment
3. run the experiment and eval harness on every change

The post is especially useful because it focuses on operational details: use binary labels when possible, collect enough failures, avoid "god evaluators", and integrate evaluation tightly with experimentation.

## 中文摘要

Eugene 给出了一个非常紧凑但实用的 eval harness 工作流：

1. 先标注一小批但真正有用的数据
2. 让 LLM evaluator 对齐人类判断
3. 每次改动都运行实验和 eval harness

这篇文章特别有价值，因为它强调的是运营层面的细节：尽量使用二元标签、保证失败样本足够、避免做一个什么都评的 "god evaluator"，并把评测紧密接到实验流程里。

## Strong Points

- evaluation is a workflow, not a single metric
- fail cases matter more than vanity averages
- per-dimension evaluators are easier to calibrate

- 评测是一套流程，不是一个分数
- 失败样本比好看的平均值更重要
- 分维度 evaluator 更容易校准

## Actionable Takeaways

- Build your eval harness into iteration, not after iteration. / 把 eval harness 嵌进迭代，而不是迭代结束后再补。
- Track regressions with every config or prompt change. / 每次配置或 prompt 改动都要检查回归。
- Use evals to shrink decision ambiguity. / 用 evals 减少决策模糊，而不是制造更多分数噪音。
