# Finance Skills 中文版

这是一组面向中国 A 股 / 金融研究场景的 Codex Skills，适合公开发布到 GitHub。

项目将东方财富妙想（Eastmoney MX）的数据查询、资讯检索、条件选股、自选股管理、模拟组合操作，与 AlphaEar 风格的投资信号跟踪和逻辑可视化能力打包在一起，让 Codex 在处理金融任务时能够更稳定地选择合适 workflow。

> 本项目仅用于研究辅助，不构成任何投资建议。

## 包含哪些 Skills

| Skill | 用途 | 典型请求 |
| --- | --- | --- |
| `mx-data` | 查询东方财富妙想金融数据，包括行情、估值、财务报表、公司资料、股东、高管和关联关系等。 | “查一下澜起科技近三年收入、净利润和毛利率” |
| `mx-search` | 检索金融资讯、公告、研报、政策、交易规则和事件背景。 | “宁德时代最近有什么重大公告？” |
| `mx-xuangu` | 根据行情、估值、财务、行业、概念等条件进行选股或成分股查询。 | “筛选市盈率小于20且ROE大于15%的A股” |
| `mx-zixuan` | 查询、添加或删除东方财富自选股。 | “把300059加入自选” |
| `mx-moni` | 查询和操作东方财富模拟组合，包括持仓、资金、委托、模拟买卖和撤单。 | “模拟盘市价买入000001 100股” |
| `alphaear-logic-visualizer` | 将投资逻辑、产业链传导、风险收益路径转换为 Draw.io 兼容图表。 | “把AI服务器需求如何传导到内存接口芯片画成图” |
| `alphaear-signal-tracker` | 跟踪已有投资信号是否被增强、削弱、兑现、证伪或保持不变。 | “跟踪上周的光模块信号有没有被证伪” |

## 目录结构

```text
.
├── README.md
├── README.zh-CN.md
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

每个 Skill 目录都包含一个 `SKILL.md`。部分 Skill 还包含：

- `scripts/`：可复用执行脚本
- `references/`：字段释义、API 说明或长参考资料
- `evals/`：用于检查触发边界的正例和反例
- `tests/`：可选的本地测试文件

## 安装方式

将 `skills/` 下的 Skill 目录复制到 Codex 的 skills 目录：

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -R skills/* "${CODEX_HOME:-$HOME/.codex}/skills/"
```

复制后重启或刷新 Codex，让它重新发现这些 Skills。

## 环境变量配置

`mx-*` 系列 Skill 需要东方财富妙想 API Key，在运行时通过环境变量读取：

```bash
export MX_APIKEY="your_eastmoney_mx_api_key"
```

可选：自定义输出目录。

```bash
export MX_OUTPUT_DIR="$HOME/.codex/skills-output/mx_data/output"
```

如果不设置 `MX_OUTPUT_DIR`，默认输出到：

```text
~/.codex/skills-output/mx_data/output
```

## 密钥安全

不要把真实的 `MX_APIKEY` 提交到 GitHub。

仓库中只应该出现环境变量名和占位符示例。真实密钥应保存在你的本地 shell、私有环境变量管理工具或 CI/CD Secret 中。

发布前可以用下面的命令检查当前环境变量值是否被写进仓库：

```bash
if [ -n "$MX_APIKEY" ]; then
  rg -F "$MX_APIKEY" .
fi
```

如果没有任何输出，说明当前密钥没有出现在仓库文件里。

更多安全说明见 [`SECURITY.md`](SECURITY.md)。

## 快速使用示例

查询财务数据：

```bash
python skills/mx-data/mx_data.py "澜起科技 688008 近三年 营业收入 净利润 毛利率"
```

检索资讯和研报：

```bash
python skills/mx-search/mx_search.py "澜起科技 最新研报 DDR5 PCIe Retimer CXL"
```

条件选股：

```bash
python skills/mx-xuangu/mx_xuangu.py --query "市盈率小于20且ROE大于15%的银行股"
```

查询自选股：

```bash
python skills/mx-zixuan/mx_zixuan.py query
```

查询模拟组合持仓：

```bash
python skills/mx-moni/mx_moni.py "我的持仓"
```

## 校验 Skills

如果本地存在 Codex 系统自带的 `skill-creator` 校验脚本，可以运行：

```bash
for d in skills/*; do
  python "$HOME/.codex/skills/.system/skill-creator/scripts/quick_validate.py" "$d"
done
```

也可以检查所有 eval JSON 是否格式正确：

```bash
find skills -path "*/evals/evals.json" -print0 |
  xargs -0 -n1 python -m json.tool >/dev/null
```

## 发布前检查清单

- [ ] 没有提交真实 API Key、Token、Cookie、`.env` 文件、本地输出结果或缓存。
- [ ] 已包含 `SECURITY.md`。
- [ ] 没有提交 `_meta.json` 等本地 marketplace 元数据，除非你明确希望公开它们。
- [ ] 所有 `SKILL.md` 都通过 `quick_validate.py`。
- [ ] 已检查 `mx-zixuan` 和 `mx-moni` 这类会修改账户数据或模拟账户数据的 Skill。

## 免责声明

金融数据和自动生成的分析可能存在延迟、不完整或解释偏差。重要结论应以官方公告、交易所披露、上市公司文件和专业判断为准。本仓库中的任何内容均不构成买入、卖出或持有证券的建议。
