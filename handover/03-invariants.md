# 03-invariants.md

絶対に変更してはいけない設計原則。各InvariantはIDを持ち、
ファイル構成が変わってもIDは変更しない。

## Format

```
ID: INV-XXX
Statement: [原則そのもの]
Reason: [なぜこの原則が必要か]
Priority: Critical / High / Medium
```

## Entries

### INV-001（記入例）
- **Statement**: The AI never decides for the user among multiple
  candidates; a deterministic program selects instead.
- **Reason**: LLMの自己申告・自己判断を最終決定に使うと、
  検証できない判断が紛れ込む（耐ハルシネーション原則）。
- **Priority**: Critical

### INV-002（記入例）
- **Statement**: A fixed phrase is never reused verbatim across responses.
- **Reason**: 定型文の繰り返しは機械的な印象を与え、伴走者としての
  自然さを損なう。
- **Priority**: High

[以降、プロジェクト固有のInvariantを追記]
