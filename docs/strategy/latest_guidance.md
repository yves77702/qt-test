# Latest Strategy Guidance

更新时间：2026-04-30

## 当前判断

当前主线不是继续堆新策略，而是把已有证据收成可运行、可审计、可晋级的状态机。

最新方向已经从持仓日内 T 暂时转回动态退出质量。理由很明确：holding-day T 有动作频率，但当前 weekly delta 小；动态退出虽然也不能 route，但在 v364/v462/v855/v856 上显示出更像主缺口的结构。

## 优先级

1. runtime alignment  
   继续缩小 `v91` execution runtime 与 `v540/v590` clean baseline 的差距。

2. dynamic exit runtime shadow logger  
   按 `v856` 的 gate contract 设计 log-only 记录器，补 post-trigger sector、reclaim、cash queue 字段。

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

先做 `v857_dynamic_exit_runtime_shadow_logger_design_no_route`。

不要先写卖出规则。先让 runtime 记录：

- 动态退出触发后板块是否扩散或转弱
- 触发后个股是否重新 reclaim VWAP
- 触发后是否有可替代候选队列
- 释放现金是否真的能买到更强机会
- 强势赢家是否仍有延续状态

这条路线比直接发卖单慢一点，但更接近真钱纪律。
