# Computer Usage Summary Skill

[![CI](https://github.com/liuyewang/computer-usage-summary-skill/actions/workflows/test.yml/badge.svg)](https://github.com/liuyewang/computer-usage-summary-skill/actions/workflows/test.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-0f766e.svg)](LICENSE)
[![Local first](https://img.shields.io/badge/privacy-local--first-0f766e.svg)](PRIVACY.md)

**See where your time went, without sending it anywhere.**

A privacy-first [Codex skill-only plugin](https://developers.openai.com/codex/plugins/build) that
turns local [ActivityWatch](https://activitywatch.net/) data into clear app,
project, category, billable-time, AFK, and sanitized timeline reports. It runs
on macOS, Windows, and Linux wherever ActivityWatch and Python are available.

中文简介：这是一个基于本机 ActivityWatch 的跨平台电脑使用情况汇总技能。它不会上传
活动数据，不安装浏览器扩展，也不会展示完整网址。

## Requirements

- macOS, Windows, or Linux
- Python 3.9 or newer
- ActivityWatch running locally with the window and AFK watchers enabled

On Windows, install the small IANA time-zone data dependency before using an
explicit `--timezone Area/City` value:

```bash
python -m pip install -r requirements.txt
```

Install ActivityWatch from an official source of your choice. Before enabling
automatic startup, verify its publisher and operating-system security prompt.
On macOS, verify its code signature and Gatekeeper status. This
repository never downloads, installs, or starts ActivityWatch automatically.

## Install In Codex

Install the repository as a skill-only Codex plugin, or install the
`plugins/computer-usage-summary-skill/skills/computer-usage-summary` directory
directly with Codex's skill installer.
For a local clone, the skill directory can also be copied into
`~/.codex/skills/computer-usage-summary/`.

After this repository is published, a GitHub-directory install looks like:

```text
$skill-installer install https://github.com/liuyewang/computer-usage-summary-skill/tree/main/plugins/computer-usage-summary-skill/skills/computer-usage-summary
```

## Usage

Ask Codex: `Summarize what I did today on my Mac.`

Or run the bundled script directly:

```bash
# Default JSON report for an agent or another program
python3 plugins/computer-usage-summary-skill/skills/computer-usage-summary/scripts/activitywatch_summary.py --date today

# Paste an application table into Excel, Numbers, or Feishu Sheets
python3 plugins/computer-usage-summary-skill/skills/computer-usage-summary/scripts/activitywatch_summary.py --date today --format tsv --table apps

# Review the Monday-Sunday week containing the anchor date
python3 plugins/computer-usage-summary-skill/skills/computer-usage-summary/scripts/activitywatch_summary.py --date 2026-07-26 --period week --format markdown --table trend

# Review the month containing the anchor date
python3 plugins/computer-usage-summary-skill/skills/computer-usage-summary/scripts/activitywatch_summary.py --date 2026-07-26 --period month --format tsv --table categories

# A readable timeline in Markdown
python3 plugins/computer-usage-summary-skill/skills/computer-usage-summary/scripts/activitywatch_summary.py --date today --format markdown --table timeline

# Keep timeline titles out of the output
python3 plugins/computer-usage-summary-skill/skills/computer-usage-summary/scripts/activitywatch_summary.py --date today --format tsv --table timeline --hide-titles

# An Excel-friendly CSV file with a UTF-8 BOM
python3 plugins/computer-usage-summary-skill/skills/computer-usage-summary/scripts/activitywatch_summary.py --start 2026-07-20 --end 2026-07-26 --format csv --table apps --csv-bom --output weekly-apps.csv

# Create a client-ready timesheet from local app/title mapping rules
python3 plugins/computer-usage-summary-skill/skills/computer-usage-summary/scripts/activitywatch_summary.py --date today --period week --rules examples/rules.json --format csv --report client-timesheet --csv-bom --output client-timesheet.csv
```

Tables are available as `summary`, `apps`, `projects`, `categories`, `trend`,
and `timeline`. Report templates are `client-timesheet`, `weekly-review`, and
`app-trend`. User-facing times are emitted in the selected local time zone; use
`--timezone Asia/Singapore` to make a report reproducible across machines.
`--api-url` defaults to the local ActivityWatch API and should remain on
loopback addresses.

## Local Project Rules

Copy [examples/rules.json](examples/rules.json) and make it yours. Rules match
application names, application regular expressions, or **already-sanitized**
window-title patterns. The first matching rule wins. Rules stay on your device
and are never uploaded.

```json
{
  "defaults": {"category": "Admin", "billable": false},
  "rules": [
    {"app": "Code", "project": "Website refresh", "client": "Northwind Studio", "category": "Client work", "billable": true}
  ]
}
```

See the [local-first landing page](https://liuyewang.github.io/computer-usage-summary-skill/)
for a synthetic report preview and the [design-partner guide](docs/DESIGN_PARTNERS.md)
to help shape future reports without sharing activity data.

## Limitations

Accurate historical foreground and AFK time begins only after ActivityWatch
starts recording. Without ActivityWatch, macOS has no reliable public history
for foreground-app time or application launches. The skill can provide a
clearly labeled limited-evidence report but will not invent time totals.

See [PRIVACY.md](PRIVACY.md) for the data-handling rules and
[CONTRIBUTING.md](CONTRIBUTING.md) for development checks, and
[SUPPORT.md](SUPPORT.md) for safe support requests. Planned improvements are
listed in [ROADMAP.md](ROADMAP.md). Maintainers can reuse the
[launch kit](PROMOTION.md) for privacy-accurate project announcements. Public
product updates are recorded in [docs/CHANGELOG.md](docs/CHANGELOG.md).

## License

[MIT](LICENSE)
