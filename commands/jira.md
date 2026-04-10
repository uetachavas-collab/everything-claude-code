---
description: Jira チケットの取得・要件分析・ステータス更新・コメント追加を行います。jira-integration スキルと MCP または REST API を使用します。
---

# Jira コマンド

ワークフローから直接 Jira チケットを操作する — チケットの取得・要件分析・コメント追加・ステータス遷移。

## 使い方

```
/jira get <TICKET-KEY>          # チケットを取得して分析
/jira comment <TICKET-KEY>      # 進捗コメントを追加
/jira transition <TICKET-KEY>   # チケットのステータスを変更
/jira search <JQL>              # JQL でイシューを検索
```

## このコマンドがすること

1. **取得 & 分析** — Jira チケットを取得し、要件・受け入れ基準・テストシナリオ・依存関係を抽出する
2. **コメント** — 構造化された進捗更新をチケットに追加する
3. **遷移** — チケットをワークフロー状態間で移動する（作業予定 → 進行中 → 完了）
4. **検索** — JQL クエリを使ってイシューを検索する

## 仕組み

### `/jira get <TICKET-KEY>`

1. Jira からチケットを取得する（MCP `jira_get_issue` または REST API 経由）
2. 全フィールドを抽出: サマリー・説明・受け入れ基準・優先度・ラベル・リンクイシュー
3. 必要に応じてコメントも取得してコンテキストを補完する
4. 構造化された分析を出力する：

```
チケット: PROJ-1234
サマリー: [タイトル]
ステータス: [状態]
優先度: [優先度]
種別: [ストーリー/バグ/タスク]

要件:
1. [抽出された要件]
2. [抽出された要件]

受け入れ基準:
- [ ] [チケットからの基準]

テストシナリオ:
- ハッピーパス: [説明]
- エラーケース: [説明]
- エッジケース: [説明]

依存関係:
- [リンクイシュー・API・サービス]

推奨される次のステップ:
- /plan で実装計画を作成
```

### `/jira comment <TICKET-KEY>`

1. 現在のセッション進捗をまとめる（構築したもの・テスト・コミット）
2. 構造化コメントとしてフォーマットする
3. Jira チケットに投稿する

### `/jira transition <TICKET-KEY>`

1. チケットの利用可能な遷移を取得する
2. ユーザーに選択肢を表示する
3. 選択された遷移を実行する

### `/jira search <JQL>`

1. Jira に対して JQL クエリを実行する
2. マッチするイシューのサマリーテーブルを返す

## 前提条件

このコマンドには Jira 認証情報が必要です。いずれかを選択：

**オプション A — MCP サーバー（推奨）:**
`mcpServers` 設定に `jira` を追加する（テンプレートは `mcp-configs/mcp-servers.json` を参照）。

**オプション B — 環境変数:**
```bash
export JIRA_URL="https://yourorg.atlassian.net"
export JIRA_EMAIL="your.email@example.com"
export JIRA_API_TOKEN="your-api-token"
```

認証情報がない場合は停止して、設定手順をユーザーに案内してください。

## 関連

- **スキル:** `skills/jira-integration/`
- **MCP 設定:** `mcp-configs/mcp-servers.json` → `jira`
