# Latest Strategy Progress

更新时间：2026-04-30 01:23

## 本轮做了什么

本轮主线已推进到 `v859_dynamic_exit_runtime_shadow_daily_accumulator_no_route`。

先完成了 `v858`：把动态卖点 runtime shadow logger 接进 `B:\qt\quant_project\sim_trading\run_ths_sim_once_v1.py`，并用 `read_only` 跑通真实 smoke，生成 `dynamic_exit_runtime_shadow_sidecar.csv`。

随后完成 `v859`：把各 run 目录里的 dynamic-exit sidecar 聚合成 daily accumulator，方便后续连续多日审计。

## 新增文件

- `B:\qt\quant_project\build_v858_dynamic_exit_runtime_shadow_logger_patch_or_dryrun_no_route_v1.py`
- `B:\qt\quant_project\build_v859_dynamic_exit_runtime_shadow_daily_accumulator_no_route_v1.py`
- `B:\qt\quant_project\dynamic_exit_runtime_shadow_logger_patch_or_dryrun_no_route_20260430.md`
- `B:\qt\quant_project\dynamic_exit_runtime_shadow_daily_accumulator_no_route_20260430.md`
- `B:\qt\quant_project\9_26推进节奏计划_20260430_v858补充.md`
- `B:\qt\quant_project\9_26推进节奏计划_20260430_v859补充.md`
- `C:\app\models\v858_dynamic_exit_runtime_shadow_logger_patch_or_dryrun_no_route\v858_validation.json`
- `C:\app\models\v859_dynamic_exit_runtime_shadow_daily_accumulator_no_route\v859_validation.json`

## 关键读数

- clean baseline：`v540`
- clean publish：`v590_revalidated_v540_clean_master`
- runtime execution baseline：`v91`
- 最新研究锚点：`v859`
- runtime smoke run：`B:\qt\quant_project\models\sim_trading_runs\20260430_011454`
- smoke trade date：`2026-04-29`
- official_buy_rows：`0`
- action_plan_rows：`0`
- executed_rows：`0`
- `repair_weak` quota：`0`
- v858 dynamic exit sidecar rows：`1`
- v859 accumulator rows：`1`
- v859 distinct trade dates：`1`
- `CASH_RELEASE_PROTECT_REVIEW` rows：`1`
- entry / cost missing rows：`1`
- sector context missing rows：`1`
- v858 acceptance tests：`10/10`
- v859 acceptance tests：`8/8`

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

动态卖点已经进入 runtime logger 和 accumulator，但仍是 evidence-only。

当前 blocker：

- sidecar 只有 1 个 trade_date，需要继续收多日 forward evidence
- raw position 缺 entry / cost，导致 `ret_from_entry_pct` 暂时不能当真钱 gate
- post-trigger sector breadth / sector mean ret 仍为空
- clean `v540/v590` 与 runtime `v91` 仍未完全对齐
- `quant_project` 当前不是 Git 仓库，无法本地 git 提交主线状态

## 下一步候选

首选下一步：

`v860_dynamic_exit_entry_cost_and_sector_context_fill_no_route`

目标：补 entry/cost 和 post-trigger sector context，但仍然只写 sidecar，不改卖点、不下单、不训练。

并行但降优先级：

- holding-day T 继续 evidence-only，重点看 partial sell 与 buyback/keep-reduced 分解
- pre-v485 scout / ranker 继续保持 no-route evidence pool
- runtime alignment 继续推进 `v91 -> v540/v590`

## Codex 提交固定要求

以后每次 Codex 提交或整理主线时，必须同步更新本文档，并至少包含：

- 本轮做了什么
- 新增文件
- 关键读数
- 是否 no-route/no-paper/no-live/no-training
- 当前 blocker
- 下一步候选

