# Latest Strategy Progress

更新时间：2026-04-30

## 本轮做了什么

本轮主线已经更新到 `v856_dynamic_exit_current_visible_gate_contract_no_route`。

最新工作把 `v855` 的动态退出 ledger 收成一个 no-route、current-visible 的 gate contract。核心不是发出卖单，而是把动态退出拆成可审计状态：

- `NO_CANDIDATE_KEEP_BASELINE`
- `BLOCK_ACTIVE_WINNER_REVIEW`
- `CASH_RELEASE_PROTECT_REVIEW`
- `PROTECT_WEAK_OR_FADE_REVIEW`
- `SMALL_EDGE_OBSERVE`

这一轮也把持仓日内 T 的当前小循环收口：`v852` 证明“完成回补不等于成功”，`v853` 证明粗粒度可见代理还不足以 route，因此 `v854-v856` 将 9.26 主攻方向重新拨回动态退出质量。

## 新增文件

- `9_26推进节奏计划_20260430_v856补充.md`
- `dynamic_exit_current_visible_gate_contract_no_route_20260430.md`
- `build_v856_dynamic_exit_current_visible_gate_contract_no_route_v1.py`
- `mainline_runtime_manifest_v1.json`
- `docs/strategy/latest_progress.md`
- `docs/strategy/latest_guidance.md`

## 关键读数

- clean baseline：`v540`
- clean publish：`v590_revalidated_v540_clean_master`
- runtime execution baseline：`v91`
- 最新研究锚点：`v856`
- v856 application rows：`232`
- v856 dynamic candidate rows：`32`
- `CASH_RELEASE_PROTECT_REVIEW` rows：`26`
- `BLOCK_ACTIVE_WINNER_REVIEW` rows：`6`
- missing runtime context rows：`6`
- acceptance tests：`10/10`
- 最新 runtime preview：`official_buy_rows = 0`
- 最新 runtime block reason：`subphase_total_quota_zero`
- 最新 runtime 状态：`repair / repair_weak / quota=0`

## 是否 no-route/no-paper/no-live/no-training

- no-route：是
- no-paper：是
- no-live：是
- no-training：是
- `repair_weak` quota：未打开
- `global max2`：未打开
- 2026 frozen：仍然只做 diagnostic，不反向调参

## 当前 blocker

动态退出方向当前 blocker 是 runtime 缺 6 类 post-trigger 字段：

- post-trigger sector breadth
- post-trigger sector mean return
- post-trigger reclaim count
- post-trigger VWAP reclaim hold bars
- same-day candidate queue after exit
- cash reuse fillable lot

持仓日内 T 当前 blocker 是微观上下文不完整：

- VWAP distance
- reclaim bars
- last-3 amount slope
- last-3 low lift
- sector context
- sellable quantity / cash reuse context

工程层 blocker：

- 本地 `quant_project` 当前不是 Git 仓库
- clean `v540/v590` 与 runtime `v91` 仍未完全对齐
- 本机 HTTPS git push 缺少有效 GitHub token，当前使用 GitHub 插件写入远端

## 下一步候选

首选下一步：

`v857_dynamic_exit_runtime_shadow_logger_design_no_route`

目标：设计 log-only runtime shadow logger，记录动态退出需要的 post-trigger sector / reclaim / cash queue 字段，但不改变退出、不下单。

并行但降优先级：

- holding-day T 继续补微观上下文字段，只做 evidence-only
- pre-v485 scout / ranker 继续保持 no-route evidence pool
- runtime alignment 继续推进 `v91 -> v540/v590`

## Codex 提交固定要求

以后每次 Codex 提交或整理主线时，必须同步更新本文件，并至少包含：

- 本轮做了什么
- 新增文件
- 关键读数
- 是否 no-route/no-paper/no-live/no-training
- 当前 blocker
- 下一步候选
