# Everything Claude Code — 日本語版

Claude Code（claude.ai/code）向けの本番環境対応プラグイン集を、日本のビジネス環境に合わせてローカライズしたバージョンです。

---

## このリポジトリについて

[everything-claude-code](https://github.com/affaan-m/everything-claude-code) を日本向けに最適化したフォーク版です。

**追加・変更した主な内容：**

- 全コンポーネントを日本語化（エージェント・スキル・コマンド・CLAUDE.md）
- 日本でよく使われるツール（Backlog・Chatwork・Notion・kintone）の MCP 連携を追加
- 日本固有のビジネスルール（稟議・報連相・インボイス制度・電帳法・個人情報保護法）を追加
- 非エンジニアや日本の一般ビジネスユーザーが使いやすいコンポーネントのみ厳選

---

## 収録コンポーネント一覧

### エージェント（agents/）

| ファイル | 役割 |
|---|---|
| `architect.md` | システム設計・アーキテクチャ専門エージェント |
| `chief-of-staff.md` | 個人通信参謀（メール・資料の分類・対応） |
| `planner.md` | 実装計画作成エージェント |
| `seo-specialist.md` | SEO 専門エージェント |
| `conversation-analyzer.md` | 会話分析エージェント |
| `doc-updater.md` | ドキュメント更新エージェント |
| `docs-lookup.md` | ドキュメント参照エージェント |
| `japanese-business-advisor.md` | 日本ビジネス慣習アドバイザー（稟議・インボイス等） |

### スキル（skills/）

| スキル | 用途 |
|---|---|
| `japanese-business-ops` | 稟議フロー・消費税計算・インボイス制度 |
| `backlog-integration` | Backlog 課題管理 |
| `chatwork-ops` | Chatwork メッセージ・タスク操作 |
| `notion-ja` | Notion 日本語環境での操作 |
| `jira-integration` | Jira チケット管理 |
| `seo` | SEO 監査と最適化 |
| `market-research` | 市場調査・競合分析 |
| `deep-research` | 引用付き多角的調査 |
| `article-writing` | 記事・ブログ執筆 |
| `brand-voice` | ブランドボイス統一 |
| `knowledge-ops` | ナレッジ管理 |
| `email-ops` | メール操作 |
| `google-workspace-ops` | Google Workspace 操作 |
| `research-ops` | 調査操作フロー |
| `strategic-compact` | コンテキスト管理 |
| `visa-doc-translate` | ビザ申請書類の翻訳・PDF 生成 |

### コマンド（commands/）

| コマンド | 機能 |
|---|---|
| `/backlog` | Backlog 課題の操作 |
| `/chatwork` | Chatwork メッセージ・タスク操作 |
| `/jira` | Jira チケット操作 |
| `/plan` | 実装計画の作成 |
| `/learn` | セッションからパターンを抽出 |
| `/docs` | ドキュメント参照 |
| `/aside` | メイン作業を中断せず質問 |

### ルール（rules/）

| ディレクトリ | 内容 |
|---|---|
| `common/` | 言語共通のコーディング規約 |
| `ja/` | 日本チーム向け追加規約（個人情報・インボイス・稟議） |
| `typescript/` | TypeScript/JavaScript 規約 |
| `python/` | Python 規約 |
| `golang/` | Go 規約 |
| `java/` | Java 規約 |
| `rust/` | Rust 規約 |
| `web/` | フロントエンド/Web 規約 |
| `php/` | PHP 規約 |

---

## セットアップ

### 前提条件

- [Claude Code](https://claude.ai/code) がインストール済みであること
- Node.js 18 以上
- npm または npx が使えること

### インストール手順

**1. リポジトリをクローンする**

```bash
git clone https://github.com/havalabs/everything-claude-code-japanese.git
cd everything-claude-code-japanese
```

**2. インストールスクリプトを実行する**

```bash
# macOS / Linux
bash install.sh

# Windows（PowerShell）
.\install.ps1
```

**3. MCP サーバーの設定（任意）**

`mcp-configs/mcp-servers.json` を参考に、使いたいサービスの設定を `~/.claude.json` に追加する。

```json
// ~/.claude.json に追加する例
{
  "mcpServers": {
    "backlog": {
      "command": "npx",
      "args": ["-y", "backlog-mcp-server"],
      "env": {
        "BACKLOG_SPACE_ID": "あなたのスペースID",
        "BACKLOG_API_KEY": "あなたのAPIキー"
      }
    },
    "chatwork": {
      "command": "npx",
      "args": ["-y", "chatwork-mcp-server"],
      "env": {
        "CHATWORK_API_TOKEN": "あなたのAPIトークン"
      }
    },
    "notion": {
      "command": "npx",
      "args": ["-y", "@notionhq/notion-mcp-server"],
      "env": {
        "OPENAPI_MCP_HEADERS": "{\"Authorization\": \"Bearer あなたのトークン\", \"Notion-Version\": \"2022-06-28\"}"
      }
    }
  }
}
```

---

## 使い方

Claude Code を起動したら、スラッシュコマンドで操作できます。

```bash
# 実装計画を作成する
/plan

# Backlog の担当課題を確認する
/backlog 担当課題を表示して

# Chatwork で日報を送信する
/chatwork 開発チームルームに今日の日報を送って

# 日本ビジネス系の相談（稟議・インボイス等）
@japanese-business-advisor 稟議フローを実装したい

# SEO 監査
/seo

# 市場調査
/market-research
```

---

## MCP ツールの API キー取得方法

### Backlog

1. Backlog にログイン → 個人設定 → API → API キーを発行

### Chatwork

1. Chatwork にログイン → 右上アカウント名 → API トークン → トークン発行

### Notion

1. [notion.so/my-integrations](https://www.notion.so/my-integrations) → 新しいインテグレーション作成
2. 連携したいページで「接続を追加」

### kintone

1. kintone 管理者にアクセス権限を依頼
2. または `Password Authentication` でユーザー名・パスワードを使用

---

## 注意事項

- API キーや認証情報をコードにハードコードしないこと
- `.env` ファイルは `.gitignore` に含まれていることを確認すること
- MCP サーバーは必要なものだけ有効化すること（10個以内を推奨）

---

## ライセンス

MIT License — 元リポジトリ [affaan-m/everything-claude-code](https://github.com/affaan-m/everything-claude-code) と同じライセンスに準拠します。
