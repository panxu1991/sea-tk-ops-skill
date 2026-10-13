# 核心需求 13 项（交付前逐项对齐）
> 现实际 15 项（#14 压力测试、#15 盈亏平衡档，2026-10-10 增补）。

> 这是用户还原的核心要求清单。每项都锁到具体文件；未覆盖=重做。

| # | 单项要求 | 锁到 |
|---|---|---|
> 口径锁（2026-10-10）：保本=纯商品卡盈亏平衡下限（不算达人/投流）；每个定价输出 保本ROAS_商品卡 / 保本ROAS_达人 两值；投流费不计入成本。详见 `break-even-roi-rule.md`。
> 补充（2026-10-10）：每个运营模式（商品卡/达人A/达人B/达人0）额外输出「盈亏平衡档」（反解不亏的关键变量，#15）与「压力档」（最差情形，#14）；达人出单率按 A（中位数）/B（最低20%）/0（全不出单）三档计算。
| 1 | 达人出单率 + 双体系：中位数A/最低20%B；商品卡无达人佣、达人另计样品；保本ROI不含投流 | unit-economics.md + selection/SKILL.md + scripts/unit_economics.py |
| 2 | 达人样品成本 + 越南尾程 2 万盾包邮 | selection/SKILL.md + unit-economics.md |
| 3 | 竞品时间维度：开店/上架/总销量/首单/180天月周日均+环比/增长下降原因/可复制打法 | selection-and-sourcing.md |
| 4 | 边境仓纸箱独立变量，按配送条件分档（布类=0），不默认统一 | unit-economics.md + selection/SKILL.md |
| 5 | 交易费+税：6%基数=实付+补贴+尾程；VAT默认0；所得税1%；佣金按官方档 | policy-snapshot.md + unit-economics.md |
| 6 | 成本优化：1688/义乌购白底图搜同款，成本高则采规格/起订量/供应商写报告，换不换用户定 | selection-and-sourcing.md + research/SKILL.md |
| 7 | 运营数据表深度：观测/上架初期/热卖/放量/复盘/监控多阶段看板，复盘表可填+目标值+达标公式 | growth-engine.md + growth/SKILL.md + scripts/growth_tables.py |
| 8 | 打法分析：头部+崛起卖家，逐产品/价格/规格/时间维度/增长下降原因 | selection-and-sourcing.md |
| 9 | 标题公式：核心词+品牌+属性+场景+卖点+情绪，60-80字符越南语 | title-formula.md（方法论库） |
| 10 | 流量入口：四渠道归因，商品卡自然流基本盘 | traffic-sources.md + growth-engine.md |
| 11 | 数据复盘自循环：反馈数据→漏斗定位→单变量优化→沉淀→新口径闭环 | evolution.md + growth/SKILL.md |
| 12 | FastMoss 纪律：非必须+需登录，无法登录反馈+建议+询问 | research/SKILL.md + quality-locks.md |
| 15 | 盈亏平衡档（全运营模式）：每个运营模式反解「净利=0」的关键变量值（商品卡=最低日均单量；达人=最低出单率×最低出单数），作为扩量/收缩分界线 | break-even-roi-rule.md + unit-economics.md + selection/SKILL.md + scripts/unit_economics.py |
| 14 | 压力测试（全运营模式）：每个运营模式（商品卡/达人A/达人B/达人0）都输出最差情形压力值；达人0档=全不出单只报总损失；商品卡档=退货率上限+汇率最差+补货断档叠加 | break-even-roi-rule.md + unit-economics.md + selection/SKILL.md + scripts/unit_economics.py |
| 13 | 内容方法论深度：方法论优先，蒸馏分析目标品+竞品/视频/商品卡综合分析再输出 | content-methodology.md（方法论库） |

## 说明

- #9、#13 是「方法论库」，锁方法/公式/分析路径，供分析与决策调用；**不恢复自动产出文案/主图/详情页/短视频**（该产出环节仍保持移除）。
- #7 复盘表必须是「可填写 + 目标值 + 达标公式」，不是只读文档；多阶段看板=观测期/上架初期/热卖期/放量期 + 复盘 + 复盘后监控。
