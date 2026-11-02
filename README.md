# cache_savings_estimate_budgeted_eval

（缓存省钱测算场景）缓存省钱测算的测量噪声主要来自哪里？用成本与资源核算估计噪声水平，并求出结论仍然稳定的最小样本量。

## 要回答的问题

- 子问题：缓存省钱测算
- 提问角度：noise
- 测量方式：成本与资源核算（指标字段 `cost_ratio`，单位 倍）
- 实验协议：预算内评测 —— 在给定预算内挑选评测样本，报告预算消耗与覆盖程度
- 重点排查的失败模式：评测泄漏

## 快速开始

仅依赖 Python 标准库。

```bash
python cache_savings_estimate_budgeted_eval.py --help
python cache_savings_estimate_budgeted_eval.py --input sample.txt
python cache_savings_estimate_budgeted_eval.py --input sample.txt --json
```

输入文件按行解析。协议「预算内评测」使用分隔符 `,` 区分标签与载荷。

## 用法

| 参数 | 说明 | 默认值 |
| --- | --- | --- |
| `--input` | 输入文件路径 | 无（必填） |
| `--limit` | 只处理前 N 行 | 0 表示不限 |
| `--json` | 以 JSON 输出结果 | 关闭 |
| `--threshold` | 判定阈值 | 0 |
| `--budget` | 预算条数 | 50 |
| `--bins` | 分桶数量 | 10 |
| `--components` | 组件列表（逗号分隔） | 无 |

## 输出

```text
读取 1 个文件，共 3 条记录
处理完成，耗时 0.002s
```

## 判定规则

成本增幅超过效果增幅的 1/3 即判定为不划算。

按单位效果成本（cost / effect）排序，而非单看成本。

## 复现步骤

1. 按上面的分隔符准备输入文件，每行一条样本；
2. 运行 `python cache_savings_estimate_budgeted_eval.py --input sample.txt --json`；
3. 记录运行环境、输入文件与完整输出；
4. **未运行的指标不要引用**：本仓库只提供测量方法与判定规则，
   不预填任何实测数字。填入实测值前请确认对应命令与输入一致。

## 适用与不适用

- 适用：需要可脚本化、可离线复核的「缓存省钱测算」测量。
- 不适用：需要真实线上流量回放或分布式压测的场景。

## 与同类方案的区别

同类脚本通常只输出一个总数，无法回答「哪一段出的问题」。
这个实现把 cost_ratio 拆成可核对的分量，并明确给出判定规则与反例，
让人能追问结论是怎么来的，而不是接受一个无法追问来源的数字。

## 来源与参考

- Python 官方文档 argparse：<https://docs.python.org/3/library/argparse.html>
- Python 官方文档 statistics：<https://docs.python.org/3/library/statistics.html>
