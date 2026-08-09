# AI組織: Zenn記事制作ライン

このリポジトリは「CEOがコンセプトを出し、独立したエージェントがそれぞれ1コンセプトを
担当して執筆・公開・反応観測・改善のサイクルを自律的に回す」組織のワークスペース。

## 役割

- **CEO(人間)**: `org/concepts/` にコンセプトを追加する(または追加を依頼する)。
  コンテンツの可否判断はしない — 各エージェントの裁量に委ねる。CEOが担うのは支払いが
  発生する意思決定(有料API、外部サービス契約など)のみ。
- **エージェント(cronで定期起動、1コンセプト=1エージェント)**: 担当コンセプトについて
  完全に自律で意思決定・実行する。人間の承認を待たずに commit & push まで完了させる。

## 1サイクルの動き

エージェントは起動するたびに、自分が担当する `org/concepts/<slug>.md` を読み、
frontmatterの `status` に応じて次のいずれかを行う。

1. `status: intake` → 記事を1本執筆し `articles/` 配下に新規ファイルを作成
   (`npx zenn new:article` でスラッグを生成)、`published: true` にしてcommit & push。
   コンセプトファイルの `status` を `published` に更新し、`article` フィールドに
   記事のスラッグを記録する。
2. `status: published` → Zenn公開APIで反応を取得し、記事を改善すべきか判断する。
   - `https://zenn.dev/api/articles?username=obo_koki` (該当記事の `liked_count` /
     `comments_count` / `bookmarked_count` を確認)
   - 反応が乏しい、または改善余地がある場合は、タイトル・導入・構成などを見直して
     articles配下のファイルを直接編集しcommit & push。
   - コンセプトファイルの `log` に、いつ何を変えたか(日付 + 要約)を追記する。
   - 明確な改善判断ができない場合は何もせず終了してよい(無理に変更を加えない)。

## コンセプトファイルの形式 (`org/concepts/<slug>.md`)

```markdown
---
status: intake # intake | published
article: null  # 公開後は articles/ のスラッグ(例: "abcdef1234567")
---

# コンセプト名

(CEOが提示した元の依頼文をそのまま書く)

## log
(published後、変更のたびに追記する。例: "2026-08-10: タイトルを変更、反応がなかったため")
```

新規コンセプトのテンプレートは `org/concepts/_template.md` を参照。

## Zenn記事のfrontmatter規約

```yaml
title: "..."
emoji: "適切な絵文字1つ"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [関連トピックを2〜5個]
published: true
```

## 注意事項

- 1エージェント1コンセプトを厳守し、他のコンセプトの記事には触れない
  (並行稼働時のコンフリクト防止)。
- push前に必ず `git pull --rebase` して他エージェントの変更と衝突しないようにする。
- 記事は日本語で書く(このリポジトリの既存記事に合わせる)。
- 内容に事実確認が必要な技術的主張を含める場合は、断定を避け根拠を示すか、
  検証可能な形で書く。
