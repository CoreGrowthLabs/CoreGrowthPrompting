# 05-decisions.md

採用された設計判断を記録する。各Decisionは変更履歴ではなく、
**現在の正**として扱う（Statusで無効化する。書き換えて消さない）。

**記録タイミング（CGP方式プライマー3.5）**：決定が確定した直後に追記する。
セクション完成時やまとめての一括生成は行わない。

## Format

```
Decision ID: DEC-XXX
Category: [分類]
Status: Accepted / Deprecated / Superseded (by DEC-YYY)
Decision: [採用した設計]
Reason: [採用理由]
Alternatives: [検討した代替案]
Rejected Because: [代替案を却下した理由]
Consequences: [この判断で得られた利点と受け入れた制約]
Evidence: [根拠となる引用・実行結果。internal_reflectionの場合は
  「セッション全体の流れに由来する」等、正直に記述する]
Counter Evidence: [この決定に反する証拠。なければ「なし」と明記]
Confidence: [confirmation_sourceから導出。下記参照]
Tension Axes: [複数軸を同時に釣り合わせた場合のみ記載。該当なしなら省略可]
Origin: main_session / external_session
Trigger Type: external_stimulus / internal_reflection（origin: external_sessionの場合のみ記載）
Confirmation Source: log_verified / user_recollection / ai_reconstructed
```

## Confidenceの導出ルール

```
log_verified      → Confidence: High
user_recollection → Confidence: Medium
ai_reconstructed  → Confidence: Low
```

## Entries

[決定が確定するたびに、ここに追記していく]
