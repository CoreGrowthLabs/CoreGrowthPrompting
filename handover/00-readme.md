# 00-readme.md

## Package Version
1.0.0

## Project Name
[FILL IN]

## Last Updated
[FILL IN: YYYY-MM-DD]

## Source LLM
[FILL IN: 例 Claude Sonnet 5]

## Target LLM
[FILL IN: 例 別チャット / 別モデル]

## Overview
[FILL IN: このプロジェクトが何であるかを2〜3行で]

---

## このパッケージについて

このファイル一式は、CGP方式（Core Growth Prompting）で育てたプロジェクトを、
別のLLM・別のチャットセッションへ引き継ぐための標準フォーマットである。
CGP方式プライマー（`CGP方式_改良版プライマー.md` / `cgp-primer-personal_en.md`）
6章で定義されている。

**引継ぎ先LLMは、`Current State`（08）だけを読んで作業を開始してはならない。**
必ず以下の順で読むこと。

## 読む順番

1. 00-readme.md（このファイル）
2. 01-project.md
3. 02-philosophy.md
4. 03-invariants.md
5. 04-concepts.md
6. 05-decisions.md
7. 08-current-state.md
8. 09-next-task.md
9. 06-rejected.md / 07-open-questions.md / 10-model-directive.md（随時参照）

## 運用ルール（CGP方式プライマー3.5〜3.7を参照）

- `05-decisions.md`は決定確定の直後に追記する。まとめて後から生成しない
- ファイル数が多いため、定期的に全ファイルの相互参照（ID・章番号）を
  `grep`等で機械的に確認すること
- 別セッションでの偶発的な閃きを持ち込む場合は、`origin`タグ
  （main_session / external_session）を必ず明記する
