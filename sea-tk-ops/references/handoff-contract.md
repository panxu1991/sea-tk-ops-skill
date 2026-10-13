# 双 SKILL 交接契约（panTK C 交付 → tk-image-studio）

> 目的：panTK 负责「调研/选品/定价/盈亏」，tk-image-studio 负责「出图」。两者之间用一份
> 结构化契约文件 `evidence_input.json` 解耦，避免图 skill 重新推导产品事实导致口径漂移，
> 也避免把定价/卖点从文档里人肉搬一遍。

## 触发

- panTK **C 上架交付**阶段产出 `evidence_input.json`（见 `subskills/delivery/SKILL.md` 步骤）。
- tk-image-studio 用 `python pipeline.py --input <真实产品图> --handoff evidence_input.json` 消费。

## evidence_input.json 契约（schema v1.0）

```jsonc
{
  "schema_version": "1.0",
  "source_skill": "sea-tk-ops",
  "product": {
    "name": "十二生肖八大守护神钥匙扣",     // 必填
    "category": "Keychains",
    "brand": "(待定)",                      // 产品本体文字，AI 不出/不翻译/不擦除
    "material": "锌合金",                    // 必填
    "weight_g": 26,
    "size": "约 3cm",
    "color": "多色",
    "package": "opp 袋",
    "origin_printed_text": []               // 产品自带印刷文字白名单（OCR 放行，不翻译）
  },
  "market": {
    "country": "VN",                         // 必填
    "currency": "VND",
    "exchange_rate_cny_to_target": 3874.7,
    "locale_text_rules": {                   // 图 skill 本地化铁律
      "language": "vi",
      "no_cjk_font": true,                   // arial 无 CJK 字形，中越混排会豆腐块
      "rabbit_is_meo": true                  // 越南生肖 兔=Mèo（猫）
    }
  },
  "pricing": {                               // 双币种已在本 skill 算好，直接带过去
    "selling_price": 199000, "currency": "VND",
    "break_even_price": 36096, "net_margin_pct": 43.52
  },
  "cost": {
    "cogs": 50000, "currency": "VND",
    "fulfillment": "direct", "sample_unit_cost": 38885
  },
  "claims_to_illustrate": [                  // 待图解/视觉强化的卖点（图 skill 直接消费）
    "十二生肖对应八大守护神（佛教/道教本位）",
    "锌合金精雕 + 电镀不掉色",
    "钥匙扣日常实用 + 生肖守护寓意（送礼/自留）"
  ],
  "visual_constraints": {                     // 视觉孪生铁律（图 skill 必遵守）
    "product_identity_lock": true,            // 产品本体不交 AI，白底/尺寸图用 PIL 合成
    "no_ai_text": true,                       // 文字一律 PIL 程序化叠，越南语先分词校验
    "white_background_required": true
  },
  "source_documents": ["A-调研/01-产品简报.md", "B-选品/单位经济变量模型.md"]
}
```

## 字段规则

- `product.origin_printed_text`：产品本体印刷文字（品牌/容量/材质/警示语）属产品本体，
  **不翻译、不替换、不擦除**；图 skill 的 OCR 机检按白名单放行。
- `market.locale_text_rules`：越南站只用目标语言字体（`arial.ttf` 无 CJK 字形），中文与越南语
  混排处会渲染成豆腐块 → 统一半角；本地化译名按当地习俗（兔=Mèo）。
- `claims_to_illustrate`：panTK 已锁方法论+来源+WHY 的卖点，图 skill 不重新编造，只做视觉强化。
- `visual_constraints`：与 tk-image-studio 锁①⑥（产品身份锁/证据图谱+视觉孪生）对齐。

## 调用示例

```bash
# panTK C 交付后，把 evidence_input.json 交给图 skill（真实产品图由用户/1688 提供）
python pipeline.py --input 真实产品图.jpg --handoff evidence_input.json
```

> 契约变更：任何字段增减，先改本文件 schema + 两 skill 的读写方，并在 `workflow-freeze.md` 追加一行。

---

## ✅ 端到端首跑验证记录（2026-10-10）

**结论：契约 v1.0 全链路实测走通，十二把锁全过。**

- **输入**：`_handoff_e2e/evidence_input.json`（生肖守护神钥匙扣，schema=panTK-handoff/v1.0）+ 真实产品图 `01_O1CN01NTm0nz_whitebg.png`（1688 抠白图）。
- **命令**：`python scripts/pipeline.py --input <产品图> --product 生肖守护神钥匙扣 --handoff evidence_input.json --budget 250`
- **handoff 注入验证**：日志输出 `[handoff] 已载入 panTK 证据：国家=VN 币种=VND 语言=vi；产品事实+市场规则已注入，跳过交互确认` ✅ facts.json 生成（title/body_snippet/product_printed_text/market/claims/visual_constraints）✅ info 注入（brand/material/size/color/weight/package/selling_point）✅
- **产物**：12 张合成图（白底主图/场景/人物/促销/功能/卖点/尺寸 + 4 详情屏）+ evidence.json + 视觉孪生抠白图，输出于 `tk-image-studio/scripts/data/jobs/生肖守护神钥匙扣/输出/`（v1 快照）。
- **首跑暴露并修复的 2 个真 BUG**（不在 panTK，在 tk-image-studio 消费端——这正是端到端实测的价值）：
  1. `completeness_check.py` `list_files` 用**全路径子串**匹配跳过 `.workbuddy`，作业目录在 `~/.workbuddy` 下时检出 0 张 → 改为按相对路径段匹配。锁⑪修复后 14 张检出 PASS。
  2. `pipeline.py` `GATES` 字典缺锁 11/12 条目 → 锁⑪通过后打印门禁名 KeyError 崩溃 → 补齐（11=全套清单机检，12=结构保真IoU）。
- **注意点（非 BUG）**：
  - `--budget` 熔断按设计工作：storyboard 11 张 Sunburst/Flare 预估 220 credits，预算 100 时锁⑧正确中止。
  - storyboard 是**产品级**资产（当前为运动水杯文案）——handoff 只注入产品事实/市场规则，不自动改写 storyboard 文案；正式交付前需按新品改写规划表（这属 C→出图流程的人工确认环节，符合锁③设计）。
  - `product_printed_text` 由 OCR 自动采集（本例白底图无印刷文字 → 提示手工填 evidence.json），handoff 注入的 `origin_printed_text` 落在 facts.json 供人工核对。
- **复跑口径**：任意新机器只需 ① deps_check 全绿（pillow/numpy/rembg 必需）② rembg bria 模型缓存（首跑自动下载）③ AGNES 失败自动回退渐变背景，不阻塞链路。
