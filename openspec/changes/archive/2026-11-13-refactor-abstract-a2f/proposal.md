# Proposal：refactor-abstract-a2f

## Why

來源 Abstract（[原專案 research/proposal_source.md（tag before-dic3-20261112）](https://github.com/benson103081/energy-detective-agent/blob/before-dic3-20261112/research/proposal_source.md) §1）已具 A–F 六段，但 A 段只有全球統計、E 段沒有列出具體 baseline 與 metric 名稱、F 段沒有量化 target，且缺少 300 字中文摘要與 IEEE 英文摘要。本 change 強化 A–F 並新增兩個濃縮版本。

## What Changes

- 新增 `docs/01_abstract_A2F.md`：A–F 六段，每個數字附 `docs/references.md` 編號。
- 新增 `docs/05_abstract_300.md`（約 300 字中文單段）與 `docs/06_abstract_ieee.md`（Title／Abstract／Index Terms）。
- F 段只寫 Expected Outcomes／target，明示尚未實驗。

## Capabilities

### New Capabilities
- `proposal-document`：提案文件本身的驗收契約（結構、證據、不虛構結果）。

### Modified Capabilities
無。

## Impact

只影響 `docs/01`、`docs/05`、`docs/06`、`docs/references.md`；不改系統 specs。
