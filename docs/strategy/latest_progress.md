# Latest Strategy Progress

更新时间：2026-04-30 01:20

## 本轮做了什么

本轮主线已经更新到 `v858_dynamic_exit_runtime_shadow_logger_patch_or_dryrun_no_route`。

本轮把 `v857` 的动态卖点 runtime shadow logger 设计正式接入 `run_ths_sim_once_v1.py`，并用 `read_only` 跑了一轮真实 runtime smoke。核心不是发卖单，而是让 runtime 每天稳定产出：

- `dynamic_exit_runtime_shadow_sidecar.csv`
- `run_summary.json` 内的 `dynamic_exit_runtime_*` 命名空间字段
- `runtime_state_v1.json` 内的 `last_dynamic_exit_runtime_shadow_sidecar`

这表示动态卖点主线已经从离线 ledger / gate contract，推进到 runtime 日志层。当前仍然是 evidence collection，不是卖出规则。

## 新增文件

- `9_26推进节奏计划_20260430_v858补充.md`
- `dynamic_exit_runtime_shadow_logger_patch_or_dryrun_no_route_20260430.md`
- `build_v858_dynamic_exit_runtime_shadow_logger_patch_or_dryrun_no_route_v1.py`
- `mainline_runtime_manifest_v1.json`
- `docs/strategy/latest_progress.md`
- `docs/strategy/latest_guidance.md`

## 关键读数

- clean baseline：`v540`
- clean publish：`v590`
- runtime baseline：`v91`
- 最新研究锚点：`v858`
- runtime smoke run：`B:\qt\quant_project\models\sim_trading_runs\20260430_011454`
- trade_date：`2026-04-29`
- official_buy_rows：`0`
- action_plan_rows：`0`
- executed_rows：`0`
- repair_weak quota：`0`
- dynamic exit sidecar rows：`1`
- dynamic exit candidate rows：`1`
- `CASH_RELEASE_PROTECT_REVIEW` rows：`1`
- `BLOCK_ACTIVE_WINNER_REVIEW` rows：`0`
- acceptance tests：`10/10`

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

动态退出方向已经接入 runtime 日志，但仍不能 route。当前 blocker：

- 需要连续多日 sidecar 证据，而不是单日 smoke
- post-trigger sector breadth / sector mean return 仍有缺口
- same-day candidate queue after exit 仍需稳定记录
- cash reuse fillable lot 需要和真实现金、100 股、T+1 约束对齐
- 动态退出事件与实际收盘、次日表现只能进入 review-only 区，不能进入 runtime gate

工程层 blocker：

- 本地 `quant_project` 当前不是 Git 仓库
- clean `v540/v590` 与 runtime `v91` 仍未完全对齐
- 本机 HTTPS git push 缺少有效 GitHub token，当前远端更新通过 GitHub 插件完成

## 下一步候选

首选下一步：

`v859_dynamic_exit_runtime_shadow_daily_accumulator_no_route`

目标：连续收集动态退出 sidecar，做 daily accumulator，把 runtime 当下看到的动态退出候选与后续 review-only outcome 分开审计。

验收重点：

- 多日 `dynamic_exit_runtime_shadow_sidecar.csv` 能稳定产出
- 不要求每天有候选，但要求有持仓时不漏日志
- route/live/paper/training flags 始终为 false
- 与实际收盘、次日表现的比较只能进入 review-only 区
- 继续观察 `CASH_RELEASE_PROTECT_REVIEW` 是否真正对应现金释放价值

## Codex 提交固定要求

以后每次 Codex 提交或整理主线时，必须同步更新本文件，并至少包含：

- 本轮做了什么
- 新增文件
- 关键读数
- 是否 no-route/no-paper/no-live/no-training
- 当前 blocker
- 下一步候选
