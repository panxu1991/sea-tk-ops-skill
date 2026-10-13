# SEA TK Ops Skill — 东南亚 TikTok 跨境电商运营全域交付引擎

> **SEA TikTok Cross-Border E-Commerce Ops Skill — Full-Pipeline Delivery Engine for Southeast Asia**

本仓库是一个**自包含、可移植的 AI 技能包**（Codex / Claude Code / WorkBuddy / 豆包 等任意支持 SKILL.md 的客户端均可安装）：
**一张产品图 / 产品链接 → 自动跑完调研、选品、定价盈亏、全阶段 SOP、持续出单引擎与复盘内循环，交付一套数量·质量·流程全固化的运营包。**
（本包不含文案/主图/详情页/短视频生产，聚焦运营与经营决策层。）

This repository is a **self-contained, portable AI skill pack** (installable in any SKILL.md-capable client: Codex, Claude Code, WorkBuddy, Doubao, etc.):
**One product image / product link → the pipeline automatically runs research, selection, pricing & break-even economics, full-stage SOPs, a continuous sales engine, and a review/feedback loop — delivering a fixed-spec ops package with consistent quantity, quality, and process.**
(This pack excludes copywriting / main images / detail pages / short-video production; it focuses on operations and business-decision layers.)

---

## 一、简介（中文版）

### 1. 这是什么

**sea-tk-ops** 是「TikTok 跨境电商运营引擎」的独立可移植版。修复了原版依赖外部维护包、表格构建器缺失、元技能缺失、硬编码本机路径等问题，**换电脑 / 换 AI 客户端后，交付数量、质量、流程保持一致**。

- 一张图 / 链接进来，不逐环节追问（只在**第一步履约门禁**、货源、卖家中心处问一次，且集中前置收齐）。
- 4 个子 SKILL 分工：`A 产品调研 → B 选品决策 → C 上架交付 → D 运营引擎（持续循环）`，每步有门禁，跳过即重做。
- 所有确定性计算（选品打分 / 单位经济 / 盈亏 / 看板 / 官方数据蒸馏）都由 Python 脚本执行，AI 不得手算脑补。
- 输出固化：14 文档 + 8 活表 + 图片 + _build，缺一不可；交付前机检 `verify_baseline.py` 全绿才放行。
- 内置复盘反馈手册 + 自循环进化：你回填数据 → 定位漏损 → 单变量优化 → 回写经验库 → 下次用新口径，越用越强。

### 2. 核心特性

| 特性 | 说明 |
|---|---|
| 官方优先取数 | ① 本地官方文件 → ② 官方下载 → ③ 权威二手（必标来源+日期+置信度）；费率/物流价卡只用官方值 |
| 双币种铁律 | 凡出现金额，¥(CNY) 与目标国币种必须相邻列/并列，不得单币种 |
| 两套互斥计价 | 直邮 vs 京东边境仓，脚本按 `fulfillment` 自动匹配成本项，不混算、不清零输入 |
| 国家门禁 | 开工先定目标国（VN/TH/MY/PH/SG 或 ALL-SEA，默认 VN）；ALL-SEA 必须给「建议国家排序（首站/次站/观望）」 |
| 唯一入口 | 开工第一条命令 `python scripts/preflight.py`，机器生成四勾表，退出码≠0 不得进 A |
| 交付拒收闸 | 宣布交付前 `python scripts/verify_baseline.py <包目录>`，非全绿不得交付 |
| 断点续跑 | 先扫已有策略包，已完成跳过、部分续跑、缺失补做，不覆盖用户手改 |
| 多端同步 | `sync_to_backends.py` + `verify_backends.py` 把源 SKILL 干净镜像到本机所有 AI 客户端 |
| 防漂移 | `restore_skill.py` 比对 SHA256 完整性清单，防止客户端自动还原回 BUG 版 |

### 3. 目录结构

```
sea-tk-ops/
├── SKILL.md                  # 主技能：使命/门禁/15条锁/执行协议/交付清单
├── 安装说明.md               # 任意电脑/客户端安装 + 自检步骤
├── 交付清单.md               # 冻结的交付物清单（14 文档 + 8 活表 + 图片 + _build）
├── 运营测机制说明.md         # A→B→C→D 流水线 + 脚本门禁 + 机检机制详解
├── references/               # 方法论 / 费用汇率 / 质量锁 / 输出标准 / 官方资料库
│   ├── knowledge-base/       # 各国官方费率·物流·政策快照（VN/TH/MY/PH/SG + ALL-SEA）
│   ├── meta/                 # 元技能：research / nuwa / grilling / cangjie 蒸馏
│   └── review/              # 复盘方法论 + 历史复盘案例
├── subskills/                # 4 个子 SKILL
│   ├── research/             # A 产品调研
│   ├── selection/            # B 选品决策
│   ├── delivery/             # C 上架交付
│   └── growth/               # D 运营引擎
└── scripts/                  # 确定性脚本（选品打分/单位经济/看板/蒸馏/机检/同步）
    ├── preflight.py          # 开工唯一入口（五检合一 + 机器生成四勾表）
    ├── verify_baseline.py    # 交付拒收闸（机检全绿才放行）
    ├── unit_economics.py     # 双体系盈亏 + 保本ROI + 五档
    ├── selection_score.py    # 七维选品打分
    ├── tk_distill.py         # TK 官方资料蒸馏
    ├── sync_subskills.py     # 主 SKILL 门禁同步到 4 个子 skill（幂等）
    ├── sync_to_backends.py   # 多客户端镜像
    ├── verify_backends.py    # 多端语义一致性校验
    ├── restore_skill.py      # 完整性清单防漂移
    └── ...
```

### 4. 安装

**前置**：Python 3.9+ 与 `openpyxl`（表格必需）；可选 `requests` / `pdfplumber` / `bs4`（官方取数）。

```bash
# 1) 安装到客户端技能目录（以 Codex 为例）
git clone https://github.com/panxu1991/sea-tk-ops-skill.git
# 将 sea-tk-ops-skill/sea-tk-ops 拷贝/软链到:
#   Codex:      ~/.codex/skills/sea-tk-ops
#   Claude Code: ~/.claude/skills/sea-tk-ops
#   WorkBuddy:  ~/.workbuddy/skills/sea-tk-ops
#   豆包:       ~/Doubao/skills/sea-tk-ops

# 2) 依赖自检（缺什么装什么）
cd <你的技能根目录>/sea-tk-ops
python scripts/deps_check.py --install-cmd

# 3) 开工自检（机器生成四勾表；退出码≠0 先按提示补）
python scripts/preflight.py

# 4) 首次安装后建立完整性基线（防客户端自动还原）
python scripts/restore_skill.py --seal
```

详细说明见 `安装说明.md`（含各客户端路径约定与换机首跑通路 T38-P5）。

### 5. 触发方式

直接对 AI 说（任意客户端）：

- 「发一个产品链接/一张图，做全套 TK 运营」
- 「TK 运营 / 运营测款 / 复盘 / 提升」
- 「东南亚跨境，目标越南（或 ALL-SEA），POP + 京东边境仓」

AI 会按 `SKILL.md` 的开工前置（STEP 0→3）集中收集一次性输入，然后自动跑 A→B→C→D。

---

## II. Introduction (English)

### What Is This

**sea-tk-ops** is the standalone, portable edition of a "TikTok cross-border e-commerce operations engine." It fixes the original's dependence on an external maintenance pack, missing table builders, missing meta-skills, and hardcoded local paths — so **the delivered package keeps identical quantity, quality, and process across machines and AI clients.**

- Drop in one product image or link; no step-by-step interruptions (inputs are collected **once**, up front: fulfillment gate, sourcing, seller-center data).
- Four sub-skills in a strict gate-locked pipeline: `A Research → B Selection → C Listing Delivery → D Growth Engine (continuous loop)`; skipping a gate means redoing that step.
- All deterministic math (selection scoring / unit economics / break-even / dashboards / official-data distillation) runs via Python scripts — the AI is forbidden from hand-waving numbers.
- Fixed output spec: 14 documents + 8 live spreadsheets + images + _build, none skippable; `verify_baseline.py` must pass (all green) before a package may be declared delivered.
- Built-in review feedback manual + self-improving loop: you backfill real data → loss localization → single-variable optimization → experience library write-back → next cycle uses the updated baseline. It gets better the more you use it.

### Core Features

| Feature | Description |
|---|---|
| Official-first data sourcing | ① local official files → ② official downloads → ③ authoritative secondary sources (always cite source + date + confidence); fee rates & logistics cards use official values only |
| Dual-currency rule | Every monetary value appears as adjacent CNY ¥ + target-country currency columns — single-currency output is forbidden |
| Two exclusive costing modes | Direct-ship vs JD border-warehouse; scripts auto-match cost items by the `fulfillment` field, no mixing, no zeroing inputs |
| Country gate | Target country confirmed first (VN/TH/MY/PH/SG or ALL-SEA, default VN); ALL-SEA requires a recommended country ranking (1st/2nd/watch) |
| Single entry point | First command is `python scripts/preflight.py` — five-in-one checks + machine-generated 4-item checklist; non-zero exit blocks entry into phase A |
| Delivery rejection gate | Before declaring delivery, `python scripts/verify_baseline.py <package-dir>` must be all-green |
| Resume support | Scans existing strategy packs first: skip completed, resume partial, fill gaps, never overwrite user edits |
| Multi-client sync | `sync_to_backends.py` + `verify_backends.py` mirror the source skill cleanly into every AI client on the machine |
| Drift protection | `restore_skill.py` checks a SHA256 integrity manifest against client auto-restores to buggy versions |

### Installation

**Prereqs**: Python 3.9+ with `openpyxl` (required for spreadsheets); optional `requests` / `pdfplumber` / `bs4` for official-data scraping.

```bash
# 1) Clone and place into your client's skills dir (Codex example)
git clone https://github.com/panxu1991/sea-tk-ops-skill.git
# Copy/link sea-tk-ops-skill/sea-tk-ops to:
#   Codex:      ~/.codex/skills/sea-tk-ops
#   Claude Code: ~/.claude/skills/sea-tk-ops
#   WorkBuddy:  ~/.workbuddy/skills/sea-tk-ops
#   Doubao:     ~/Doubao/skills/sea-tk-ops

# 2) Dependency self-check (installs whatever is missing)
cd <your-skill-root>/sea-tk-ops
python scripts/deps_check.py --install-cmd

# 3) Preflight self-check (machine-generated checklist; fix any non-zero exit)
python scripts/preflight.py

# 4) After first install, seal the integrity baseline
python scripts/restore_skill.py --seal
```

Full install & per-client paths: see `安装说明.md` (Install Guide).

### Triggering

Tell the AI (in any client):

- "Here is a product link / image — run the full TikTok Shop ops pipeline"
- "TK ops / test-run / review / improve"
- "SEA cross-border, target Vietnam (or ALL-SEA), POP + JD border-warehouse"

The AI follows the preflight steps (STEP 0→3), collects all one-shot inputs, then auto-runs A→B→C→D.

---

## 三、License / 许可

技能包内含方法论与参考数据快照（费率/汇率/政策来自各平台公开渠道，均带来源与日期标注）。具体复用许可见各 `references/` 文件的来源标注；脚本可自由使用与修改。

The pack ships methodology plus reference data snapshots (fee/FX/policy values from public platform sources, each cited with source and date). Reuse terms follow the per-file source notes in `references/`; scripts are free to use and modify.
