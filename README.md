# Everything Claude Code — 日本語版

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
![Markdown](https://img.shields.io/badge/-Markdown-000000?logo=markdown&logoColor=white)
![TypeScript](https://img.shields.io/badge/-TypeScript-3178C6?logo=typescript&logoColor=white)
![Python](https://img.shields.io/badge/-Python-3776AB?logo=python&logoColor=white)

> [everything-claude-code](https://github.com/affaan-m/everything-claude-code) を日本のビジネス環境向けにローカライズしたバージョン。

Claude Code（claude.ai/code）で使える、本番環境対応のエージェント・スキル・フック・コマンド・ルール・MCP 設定のコレクションです。

---

## 特徴

- **全コンポーネントが日本語** — エージェント・スキル・コマンドの指示文がすべて日本語
- **日本向けツール対応** — Backlog・Chatwork・Notion・kintone との MCP 連携
- **日本のビジネス慣習対応** — 稟議フロー・報連相・インボイス制度・電帳法・個人情報保護法
- **厳選コンポーネント** — 日本のビジネスユーザーが実際に使うものだけを収録

---

## クイックスタート

### インストール

```bash
# リポジトリをクローン
git clone https://github.com/uetachavas-collab/everything-claude-code.git
cd everything-claude-code

# 依存関係をインストール
npm install

# macOS / Linux
./install.sh

# Windows
.\install.ps1
```

### すぐに使う

Claude Code を起動してスラッシュコマンドを入力するだけです。

```
/plan          実装計画を作成する
/backlog       Backlog の課題を操作する
/chatwork      Chatwork にメッセージを送る
/notion        Notion のページ・DB を操作する
/jira          Jira チケットを操作する
/learn         セッションからパターンを抽出する
/docs          ライブラリのドキュメントを参照する
/seo           SEO を監査する
```

---

## コンポーネント一覧

### エージェント

| エージェント | 役割 |
|---|---|
| `japanese-business-advisor` | 稟議・インボイス制度・日本ビジネス慣習の相談 |
| `architect` | システム設計・アーキテクチャレビュー |
| `chief-of-staff` | メール・連絡事項の分類・対応優先度付け |
| `planner` | 実装計画の作成と整理 |
| `seo-specialist` | SEO の専門分析・改善提案 |
| `conversation-analyzer` | 会話・コミュニケーションの分析 |
| `doc-updater` | ドキュメントの更新・整理 |
| `docs-lookup` | ライブラリ・API ドキュメントの参照 |

### スキル

#### 日本ビジネス向け

| スキル | 用途 |
|---|---|
| `japanese-business-ops` | 稟議フロー・報連相・消費税計算・インボイス制度・電帳法対応 |
| `backlog-integration` | Backlog の課題管理・マイルストーン操作 |
| `chatwork-ops` | Chatwork のメッセージ送受信・タスク管理 |
| `notion-ja` | Notion の議事録作成・データベース操作 |
| `kintone-ops` | kintone のアプリ・レコード操作 |
| `jira-integration` | Jira のチケット管理・スプリント操作 |

#### コンテンツ・調査

| スキル | 用途 |
|---|---|
| `article-writing` | 記事・ブログの執筆 |
| `brand-voice` | ブランドボイスの統一と管理 |
| `content-engine` | コンテンツ制作の自動化 |
| `crosspost` | 複数プラットフォームへの投稿 |
| `deep-research` | 引用付きの多角的調査・レポート作成 |
| `market-research` | 市場調査・競合分析・意思決定支援 |
| `research-ops` | 調査操作フロー全般 |
| `seo` | SEO 監査・タイトル/メタ/構造化データの最適化 |

#### ナレッジ・コミュニケーション

| スキル | 用途 |
|---|---|
| `documentation-lookup` | ライブラリ・フレームワークのドキュメント参照 |
| `email-ops` | メールの作成・整理・対応 |
| `google-workspace-ops` | Google ドキュメント・スプレッドシート操作 |
| `knowledge-ops` | ナレッジベースの管理・検索 |
| `messages-ops` | メッセージングツール全般の操作 |
| `product-capability` | 製品の機能・ケイパビリティ分析 |
| `product-lens` | 製品視点での課題分析 |

#### ユーティリティ

| スキル | 用途 |
|---|---|
| `strategic-compact` | 長いセッションのコンテキスト管理 |
| `visa-doc-translate` | ビザ申請書類（画像）を英語に翻訳して PDF を生成 |

### コマンド

| コマンド | 機能 |
|---|---|
| `/backlog` | Backlog の課題操作 |
| `/chatwork` | Chatwork のメッセージ・タスク操作 |
| `/notion` | Notion のページ・DB 操作 |
| `/jira` | Jira チケットの操作 |
| `/plan` | 実装計画の作成 |
| `/learn` | セッションからパターンを抽出 |
| `/docs` | ドキュメントの参照 |
| `/aside` | メイン作業を中断せず質問する |
| `/update-docs` | プロジェクトドキュメントの更新 |
| `/projects` | プロジェクト一覧の管理 |

### ルール

| ディレクトリ | 内容 |
|---|---|
| `rules/common/` | 言語共通のコーディング規約 |
| `rules/ja/` | 日本チーム向け追加規約（個人情報・インボイス・稟議・Gitフロー） |
| `rules/typescript/` | TypeScript / JavaScript 規約 |
| `rules/python/` | Python 規約 |
| `rules/golang/` | Go 規約 |
| `rules/java/` | Java 規約 |
| `rules/rust/` | Rust 規約 |
| `rules/web/` | フロントエンド / Web 規約 |
| `rules/php/` | PHP 規約 |

---

## MCP 設定（外部ツール連携）

`mcp-configs/mcp-servers.json` に設定済みのツール一覧。必要なものを `~/.claude.json` にコピーして使います。

### 日本向けツール

| ツール | 用途 | 設定に必要な情報 |
|---|---|---|
| **Backlog** | 課題管理 | スペース ID・API キー |
| **Chatwork** | ビジネスチャット | API トークン |
| **Notion** | ドキュメント・DB | インテグレーショントークン |
| **kintone** | 業務アプリ | URL・ユーザー名・パスワード |
| **Slack** | チームチャット | Bot トークン・チーム ID |

### グローバルツール

| ツール | 用途 |
|---|---|
| **Jira** | プロジェクト管理 |
| **GitHub** | PR・Issue 管理 |
| **Supabase** | データベース操作 |
| **Playwright** | ブラウザ自動化・テスト |
| **Vercel** | デプロイ管理 |
| **sequential-thinking** | 段階的思考 |
| **memory** | セッション間のメモリ永続化 |

### 設定方法

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

> MCP サーバーは 10 個以内を推奨（コンテキストウィンドウを圧迫するため）

---

## API キーの取得方法

### Backlog

1. Backlog にログイン → 個人設定 → API → API キーを発行

### Chatwork

1. Chatwork にログイン → 右上アカウント名 → API トークン

### Notion

1. [notion.so/my-integrations](https://www.notion.so/my-integrations) → 新しいインテグレーション作成
2. 連携したいページ・DB で「接続を追加」

### kintone

1. kintone アプリ設定 → API トークン → トークン生成（必要な権限を設定）

---

## 注意事項

- API キー・認証情報はコードにハードコードしない
- `.env` ファイルは `.gitignore` に必ず含める
- 個人情報（氏名・メールアドレス等）をログに出力しない（個人情報保護法対応）

---

## ライセンス

MIT License — 元リポジトリ [affaan-m/everything-claude-code](https://github.com/affaan-m/everything-claude-code) と同じライセンスに準拠します。
