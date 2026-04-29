# Latest Strategy Progress

更新时间：2026-04-30 01:34

## 本轮做了什么

本轮主线已推进到 `v860_dynamic_exit_entry_cost_and_sector_context_fill_no_route`。

承接 `v858` / `v859`，我继续补动态卖点 runtime sidecar 的字段完整性。现在动态卖点 logger 不只记录候选，还会记录 entry/cost 来源状态，以及同板块 top24 分钟线 peer 的 post-trigger sector context。

这仍然是 no-route evidence，不是卖出规则。

## 新增文件

- `B:\qt\quant_project\build_v860_dynamic_exit_entry_cost_and_sector_context_fill_no_route_v1.py`
- `B:\qt\quant_project\dynamic_exit_entry_cost_and_sector_context_fill_no_route_20260430.md`
- `B:\qt\quant_project\9_26推进节奏计划_20260430_v860补充.md`
- `C:\app\models\v860_dynamic_exit_entry_cost_and_sector_context_fill_no_route\v860_validation.json`
- `C:\app\models\v860_dynamic_exit_entry_cost_and_sector_context_fill_no_route\v860_dynamic_exit_entry_cost_and_sector_context_fill_no_route_report.md`

## 关键读数

- clean baseline：`v540`
- clean publish：`v590_revalidated_v540_clean_master`
- runtime execution baseline：`v91`
- 最新研究锚点：`v860`
- runtime smoke run：`B:\qt\quant_project\models\sim_trading_runs\20260430_013011`
- official_buy_rows：`0`
- action_plan_rows：`0`
- executed_rows：`0`
- `repair_weak` quota：`0`
- dynamic exit sidecar rows：`1`
- sector context available rows：`1`
- sector peer count：`24`
- post-trigger sector breadth：`0.6667`
- post-trigger sector mean ret：`0.006817`
- entry price available rows：`0`
- acceptance tests：`8/8`

## 是否 no-route/no-paper/no-live/no-training

- no-route：是
- no-paper：是
- no-live：是
- no-training：是
- `repair_weak` quota：未打开
- `global max2`：未打开
- action scorer / ranker：未训练
- 2026 frozen：仍然只做 diagnostic，不反向调参

## 当前 blocker

动态卖点已经进入 runtime logger / accumulator，并补上 sector context，但仍是 evidence-only。

当前 blocker：

- entry / cost 来源仍缺可信字段
- top24 sector peer 扫描会增加 runtime 时间，需要 cache 或性能契约
- dynamic exit 只有单日 forward evidence，不能晋级
- clean `v540/v590` 与 runtime `v91` 仍未完全对齐
- `quant_project` 当前不是 Git 仓库，无法本地 git 提交主线状态

## 下一步候选

首选下一步：

`v861_dynamic_exit_runtime_perf_cache_and_cost_source_contract_no_route`

目标：给 sector peer context 做 cache，并建立 entry/cost source contract。仍然只写 sidecar，不改卖点、不下单、不训练。

并行但降优先级：

- holding-day T 继续 evidence-only
- pre-v485 scout / ranker 继续 no-route evidence pool
- runtime alignment 继续推进 `v91 -> v540/v590`

## Codex 提交固定要求

以后每次 Codex 提交或整理主线时，必须同步更新本文档，并至少包含：

- 本轮做了什么
- 新增文件
- 关键读数
- 是否 no-route/no-paper/no-live/no-training
- 当前 blocker
- 下一步候选

