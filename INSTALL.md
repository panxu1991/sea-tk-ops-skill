# sea-tk-ops skill pack

Python 3.9+ required.

## Dependencies

```
# Required (spreadsheet generation)
pip install openpyxl

# Optional (official-data scraping / PDF parsing)
pip install requests pdfplumber beautifulsoup4
```

Quick check (installs whatever is missing):

```bash
python scripts/deps_check.py --install-cmd
```

## Quick start

```bash
cd sea-tk-ops
python scripts/preflight.py          # machine-generated preflight checklist; non-zero exit = fix first
python scripts/restore_skill.py --seal   # one-time: seal integrity baseline after first install
```

Full install guide: `sea-tk-ops/安装说明.md` (中文).

## Structure

```
sea-tk-ops/
├── SKILL.md            # main skill definition (Codex / Claude / WorkBuddy / Doubao)
├── 安装说明.md          # install & self-check guide
├── 交付清单.md          # frozen deliverable spec (14 docs + 8 live sheets + images + _build)
├── 运营测机制说明.md     # pipeline & gate mechanism explanation
├── references/         # methodology, fee/FX snapshots, quality locks, knowledge-base
├── subskills/          # 4 sub-skills: research / selection / delivery / growth
└── scripts/            # deterministic Python scripts (preflight, verify, economics, sync)
```

## Install into your AI client

| Client   | Skills dir                        |
|----------|-----------------------------------|
| Codex    | `~/.codex/skills/sea-tk-ops`      |
| Claude   | `~/.claude/skills/sea-tk-ops`     |
| WorkBuddy| `~/.workbuddy/skills/sea-tk-ops`  |
| Doubao   | `~/Doubao/skills/sea-tk-ops`      |

Copy or symlink this `sea-tk-ops/` folder into the client's skills dir, then run
`python scripts/deps_check.py --install-cmd` once.
