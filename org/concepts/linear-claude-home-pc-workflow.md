---
status: intake # intake | published | skipped
article: null
---

# スマホのLinearから家のPCのClaudeに開発してもらう仕組み

スマホでLinearに指示を出し、家のPCのClaudeに開発してもらう、現在このPCにある仕組みを
わかりやすく説明する記事を書いてほしい。ただし、他にも同じような記事がすでにある場合は
書くのをやめてほしい。

## 調査のヒント

- このマシン上に実装の手がかりがある: `~/linear-claude-bridge`, `~/linear-claude-workspace`,
  `~/linear-ios-build-trigger`(README.md、server.py / webhook_server.py、launchdの
  plistなど)。これらを読んで仕組みの概要(Linear Webhook受信 → Claude Codeの起動 →
  実行結果の反映、といった全体の流れ)を把握したうえで、一般化して説明する記事にする。
- `.env`、`tunnel_url.txt`、`state.json` 等に含まれるトークン・URL・個人情報・内部パスは
  絶対に記事に書かない(CLAUDE.mdの注意事項を参照)。
- 執筆前に、Zenn検索・Web検索で「Linear + Claude Code」「スマホから指示してAIに開発させる」
  等の類似記事が既に十分な数あるか確認すること。同種の記事が既にありオリジナリティが
  乏しいと判断したら、執筆を中止して `status: skipped` にし理由を `log` に書く。

## log
(まだ何もしていない)
