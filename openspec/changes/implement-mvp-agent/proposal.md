# Proposal：implement-mvp-agent

## Why

`openspec/specs/` 已定義 Energy Detective 的預定行為（9 個 capability），但程式與實驗尚未開始。本 change 承接期末實作，刻意保持**未封存**，用來表示「規格已定、系統未完成」。

## What Changes

- 依 specs 實作 `src/`（agent、data、quality、analysis、rules、evidence、evaluate、main）。
- 執行 offline-evaluation 規格中的 B0／B1、合成注入、C01–C10 × 3 組 × 3 次評估。
- 本 change 不修改規格行為；若實作中發現規格需調整，另開 change 以 MODIFIED delta 處理。

## Capabilities

### New Capabilities
無。

### Modified Capabilities
無（`skip_specs: true`：純實作，不改變規格行為）。

## Impact

新增程式碼、設定、依賴清單與 outputs/；期末 Final Handover 使用。
