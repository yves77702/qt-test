# Latest Strategy Guidance

更新时间：2026-04-30 01:20

## 当前判断

当前主线不是继续堆新策略，而是把已有证据收成可运行、可审计、可晋级的状态机。

最新方向已经从离线动态退出 ledger，推进到 runtime shadow logger。`v858` 已经能在真实 runtime smoke 中为当前持仓写出 `dynamic_exit_runtime_shadow_sidecar.csv`，但它仍然只是日志层，不改变买入、卖出、paper、live、quota 或训练。

## 优先级

1. dynamic exit runtime accumulator  
   基于 `v858` 每日收集 sidecar，进入 `v859_dynamic_exit_runtime_shadow_daily_accumulator_no_route`。

2. runtime alignment  
   继续缩小 `v91` execution runtime 与 `v540/v590` clean baseline 的差距。

3. holding-day T evidence only  
   不把完成 buyback 当成功；先补 VWAP、reclaim、last3 slope、low lift、sector context。

4. candidate supply / scout / ranker  
   只做 evidence pool 和 pairwise audit，不开 `max2`，不做 replacement route。

5. model scoring  
   只允许做审计数据集、校准、解释和 shadow score；不训练会改变 route/paper/live 的 scorer。

## 硬纪律

- 不打开 `repair_weak` quota
- 不打开全局 `max2`
- 不把 review-only 字段当 runtime feature
- 不使用 next-open / close outcome 定义 runtime gate
- 不把 completed buyback 当成功
- 不为了增加交易频率替换 active winner
- 不从 2026 frozen 反向调参
- 不把 no-route 研究文件提升成 paper/live 合同
- dynamic exit logger 只能记录，不得 emit order

## 晋级条件

一个研究分支想从 evidence 进入 runtime shadow，至少需要：

- 只使用当前可见字段
- train 2023/2024 有支持
- 2025 exam 不坏
- 2026 frozen 仅诊断
- 样本厚度足够
- active winner protection 通过
- route/paper/live/training flag 明确为 false
- 能写入稳定 sidecar 或 logger

一个分支想从 runtime shadow 进入 paper contract，还需要额外满足：

- 连续多日 forward evidence
- no-route shadow 与实际 runtime 字段一致
- 成交、现金、T+1、100 股、可卖数量约束全部明确
- 卖飞率、错过率、回补成功率、现金复用收益被分开审计

## 最新推荐路线

先做 `v859_dynamic_exit_runtime_shadow_daily_accumulator_no_route`。

不要先写卖出规则。先让 runtime 连续记录：

- 动态退出触发后板块是否扩散或转弱
- 触发后个股是否重新 reclaim VWAP
- 触发后是否有可替代候选队列
- 释放现金是否真的能买到更强机会
- 强势赢家是否仍有延续状态

这条路线比直接发卖单慢一点，但更接近真钱纪律。
