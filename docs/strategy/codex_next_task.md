# Codex Next Task

更新时间：2026-04-30

## 当前任务

`v857_dynamic_exit_runtime_shadow_logger_design_no_route`

## 目标

设计并实现一个 **log-only / no-route** 的动态退出 runtime shadow logger，用来记录动态退出触发后的可见字段，但不改变任何交易行为。

## 只允许做

- 新增或更新 runtime shadow sidecar/logger
- 生成 v857 report
- 更新 `mainline_runtime_manifest_v1.json`
- 运行 py_compile / JSON 校验
- 输出验收结果

## 必须记录的 current-visible 字段

- symbol / trade_date / event_time
- current position state
- available sell quantity
- dynamic exit trigger state
- post-trigger 3m / 5m trend
- VWAP reclaim state
- sector breadth / sector mean return at event
- candidate replacement queue availability
- cash reuse candidate count
- active winner protection flag
- route_allowed=false
- paper_allowed=false
- live_allowed=false
- training_allowed=false

## 禁止事项

- 不打开 `repair_weak` quota
- 不开启 paper/live
- 不开启全局 max2
- 不训练 scorer/ranker
- 不用 next-open / next-close / future return 作为 runtime feature
- 不把 review-only 字段写进 runtime feature surface
- 不替换主模型 v540/v590
- 不改变 v91 当前下单逻辑

## 验收标准

1. `official_buy_rows` 不变。
2. `repair_weak quota` 不变。
3. 所有 route/paper/live/order flags 为 false。
4. review-only 字段只出现在 review/diagnostic 区域，不出现在 runtime feature surface。
5. sidecar 缺字段时必须 blocked，不允许用默认值伪造完整性。
6. report 明确写出 no-route / no-paper / no-live / no-training。
7. manifest 更新并通过 JSON 校验。
8. 相关 Python 文件通过 py_compile。
