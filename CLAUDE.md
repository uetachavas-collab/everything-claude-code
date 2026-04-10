# CLAUDE.md

このファイルは、リポジトリ内で作業する Claude Code（claude.ai/code）へのガイドを提供します。

## プロジェクト概要

これは **Claude Code プラグイン** です。本番環境で使えるエージェント・スキル・フック・コマンド・ルール・MCP 設定のコレクションで、Claude Code を使ったソフトウェア開発に向けた実績あるワークフローを提供します。

## テストの実行

```bash
# 全テストの実行
node tests/run-all.js

# 個別テストファイルの実行
node tests/lib/utils.test.js
node tests/lib/package-manager.test.js
node tests/hooks/hooks.test.js
```

## アーキテクチャ

プロジェクトは以下のコアコンポーネントで構成されています。

- **agents/** — 委譲先の専門サブエージェント（planner、seo-specialist など）
- **skills/** — ワークフロー定義とドメイン知識
- **commands/** — ユーザーが呼び出すスラッシュコマンド（/plan、/learn など）
- **hooks/** — トリガーベースの自動処理（セッション永続化、ツール前後フックなど）
- **rules/** — 常時適用ガイドライン（セキュリティ、コーディングスタイル、テスト要件など）
- **mcp-configs/** — 外部連携向けの MCP サーバー設定
- **scripts/** — フック・セットアップ用の Node.js ユーティリティ
- **tests/** — スクリプトとユーティリティのテストスイート

## 主要コマンド

- `/plan` — 実装計画の作成
- `/learn` — セッションからパターンを抽出
- `/docs` — ドキュメント参照
- `/jira` — Jira チケットの操作

## 開発メモ

- パッケージマネージャー検出: npm、pnpm、yarn、bun（`CLAUDE_PACKAGE_MANAGER` 環境変数またはプロジェクト設定で変更可能）
- クロスプラットフォーム対応: Node.js スクリプトにより Windows・macOS・Linux をサポート
- エージェント形式: YAML フロントマター付き Markdown（name・description・tools・model）
- スキル形式: 明確なセクション分けの Markdown（いつ使うか・仕組み・例）
- スキル配置ポリシー: キュレーション済みは skills/、生成・インポート済みは ~/.claude/skills/ に配置
- フック形式: matcher 条件と command/notification フックによる JSON

## コントリビュート

CONTRIBUTING.md のフォーマットに従ってください。
- エージェント: フロントマター付き Markdown（name・description・tools・model）
- スキル: 明確なセクション（いつ使うか・仕組み・例）
- コマンド: description フロントマター必須の Markdown
- フック: matcher と hooks 配列による JSON

ファイル名: ハイフン区切りの小文字（例: `seo-specialist.md`、`planner.md`）

## 出力言語

- すべての応答・レポート・コメントは日本語で出力する
- コードやコマンドは英語のまま維持する
- エラーメッセージの解説は日本語で補足する
- 日付フォーマット：YYYY年MM月DD日
- 数値フォーマット：日本式（カンマ区切り）

## スキル

関連ファイルで作業する際は、以下のスキルを使用してください。

| ファイル | スキル |
|---------|-------|
| `README.md` | `/readme` |
| `.github/workflows/*.yml` | `/ci-workflow` |

サブエージェントを起動する際は、該当スキルの規約を必ずエージェントのプロンプトに渡してください。
