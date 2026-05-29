# Codex Finance Skills

A public-ready collection of Codex skills for China/A-share finance research workflows.

This repository packages Eastmoney MX data/search tools together with AlphaEar-style finance reasoning skills, so Codex can route common finance tasks to the right workflow: data lookup, news research, stock screening, watchlist management, simulated trading, signal tracking, and logic visualization.

> These skills are research aids only. They do not provide investment advice.

## What Is Included

| Skill | Purpose | Typical Request |
| --- | --- | --- |
| `mx-data` | Query authoritative financial data from Eastmoney MX, including quotes, valuation, financial statements, company basics, shareholders, and entity relationships. | "查一下澜起科技近三年收入、净利润和毛利率" |
| `mx-search` | Search finance-specific news, announcements, research reports, policy, trading rules, and event context. | "宁德时代最近有什么重大公告？" |
| `mx-xuangu` | Screen stocks by market, valuation, financial, industry, concept, or ranking conditions. | "筛选市盈率小于20且ROE大于15%的A股" |
| `mx-zixuan` | Query, add, or delete Eastmoney self-selected watchlist stocks. | "把300059加入自选" |
| `mx-moni` | Query and operate an Eastmoney simulated stock portfolio. | "模拟盘市价买入000001 100股" |
| `alphaear-logic-visualizer` | Convert finance logic and investment transmission chains into Draw.io-compatible diagrams. | "把AI服务器需求如何传导到内存接口芯片画成图" |
| `alphaear-signal-tracker` | Track whether a previous investment signal is strengthened, weakened, realized, falsified, or unchanged. | "跟踪上周的光模块信号有没有被证伪" |

## Repository Layout

```text
.
├── README.md
├── SECURITY.md
├── MANIFEST.txt
└── skills/
    ├── mx-data/
    ├── mx-search/
    ├── mx-xuangu/
    ├── mx-zixuan/
    ├── mx-moni/
    ├── alphaear-logic-visualizer/
    └── alphaear-signal-tracker/
```

Each skill folder contains a `SKILL.md`. Some skills also include:

- `scripts/`: reusable execution code
- `references/`: detailed field or API notes loaded only when needed
- `evals/`: lightweight routing and behavior examples
- `tests/`: local tests where available

## Installation

Copy the skill folders into your Codex skills directory:

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -R skills/* "${CODEX_HOME:-$HOME/.codex}/skills/"
```

Restart or refresh Codex after copying so the skills are rediscovered.

## Configuration

The `mx-*` skills require an Eastmoney MX API key at runtime:

```bash
export MX_APIKEY="your_eastmoney_mx_api_key"
```

Optional output directory:

```bash
export MX_OUTPUT_DIR="$HOME/.codex/skills-output/mx_data/output"
```

If `MX_OUTPUT_DIR` is not set, outputs default to:

```text
~/.codex/skills-output/mx_data/output
```

## Security

Never commit a real `MX_APIKEY` value to GitHub.

This repository should contain only environment variable names and placeholder examples. Keep real keys in your local shell, private environment manager, or CI secret store.

Before publishing, you can scan for your current key:

```bash
if [ -n "$MX_APIKEY" ]; then
  rg -F "$MX_APIKEY" .
fi
```

No output means the current key is not present in the repository.

See [`SECURITY.md`](SECURITY.md) for the publishing checklist.

## Quick Examples

Run a financial data query:

```bash
python skills/mx-data/mx_data.py "澜起科技 688008 近三年 营业收入 净利润 毛利率"
```

Search finance news and reports:

```bash
python skills/mx-search/mx_search.py "澜起科技 最新研报 DDR5 PCIe Retimer CXL"
```

Screen stocks:

```bash
python skills/mx-xuangu/mx_xuangu.py --query "市盈率小于20且ROE大于15%的银行股"
```

Query watchlist:

```bash
python skills/mx-zixuan/mx_zixuan.py query
```

Query simulated portfolio holdings:

```bash
python skills/mx-moni/mx_moni.py "我的持仓"
```

## Validate Skills

If the Codex system `skill-creator` validator is available locally:

```bash
for d in skills/*; do
  python "$HOME/.codex/skills/.system/skill-creator/scripts/quick_validate.py" "$d"
done
```

You can also validate the bundled eval JSON files:

```bash
find skills -path "*/evals/evals.json" -print0 |
  xargs -0 -n1 python -m json.tool >/dev/null
```

## Publishing Checklist

- [ ] No real API keys, tokens, cookies, `.env` files, generated outputs, or caches are committed.
- [ ] `SECURITY.md` is included.
- [ ] `_meta.json` or other local marketplace metadata is not included unless intentionally published.
- [ ] All `SKILL.md` files pass `quick_validate.py`.
- [ ] Account-mutating skills (`mx-zixuan`, `mx-moni`) are reviewed before public use.

## Disclaimer

Finance data and generated analysis can be incomplete, delayed, or interpreted incorrectly. Users should verify important facts with official announcements, filings, exchanges, and professional judgment. Nothing in this repository is investment advice or a recommendation to buy, sell, or hold securities.
