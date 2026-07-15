# 10-model-directive.md

引継ぎ先LLMへの指示。CGP方式プライマー（1〜2章）の要約をここに
埋め込み、引継ぎ先が本プライマー自体を読んでいない場合の保険とする。

## CGP方式の核（要約）

- 人格は一つ、知能は複数。ユーザーと対話するのは唯一のコアAIだけ。
- 優先順位は「耐ハルシネーション性 ＞ 堅牢性 ＞ 低レイテンシー」で固定。
- LLMの自己申告（確信度・一貫性の自己評価）を信用しない。検証は
  決定論的な処理で行う。
- 意味的類似度は「承認」には使わない（棄却の格下げ・重複排除のみ）。
- 固定文言を繰り返さない。

## DO

- Preserve terminology（04-concepts.mdの用語をそのまま使う）
- Preserve philosophy（02-philosophy.mdの方針を維持する）
- Ask when uncertain（03-invariants.md・05-decisions.mdに明記されて
  いない仕様判断が必要な場合、勝手に補わず確認する）
- Reuse previous concepts

## DON'T

- Rename concepts（IDや用語名を変更しない）
- Ignore rejected ideas（06-rejected.mdの却下理由を無視して蒸し返さない）
- Replace architecture without discussion
- Optimize before understanding

## Conflict Resolution

複数のファイルに矛盾がある場合の優先順位：

1. 03-invariants.md（Invariants）
2. 05-decisions.md（Status: Acceptedの決定）
3. 02-philosophy.md（Philosophy）
4. 08-current-state.md（Current State）
5. その他の提案・示唆
