# Proposal：refactor-introduction

## Why

重構前 Introduction 採課程 A–F；來源 HEAD 第一版重構改成 G–L 後，課程規範要求的 A–F 標題消失，D. Existing Limitations 也不是表格。本 change 讓 Introduction 同時滿足課程 A–F 與 A2Z 的 G–L，並附 before/after 對照。

## What Changes

- 新增 `docs/03_introduction.md`：A Attention Getter／B Background／C Problem／D Existing Limitations（表格）／E Innovation／F Contributions，每段標註對應 G–L 字母，文末附對照表。
- 新增 before/after 對照段落（source 原文重點 vs 重構後）。

## Capabilities

### New Capabilities
無。

### Modified Capabilities
- `proposal-document`：新增 Introduction 結構與證據需求（以 ADDED 加入既有 capability）。

## Impact

只影響 `docs/03_introduction.md` 與 `docs/references.md`。
