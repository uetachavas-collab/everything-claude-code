# Git ワークフロー（日本チーム向け）

> [common/git-workflow.md](../common/git-workflow.md) の共通ルールを拡張した、日本チームの開発フロー対応規約。

## ブランチ命名規則

| 種別 | 形式 | 例 |
|---|---|---|
| 新機能 | `feature/機能名` | `feature/user-profile-edit` |
| バグ修正 | `fix/バグ名` | `fix/login-error-handling` |
| 緊急修正 | `hotfix/内容` | `hotfix/payment-api-timeout` |
| ドキュメント | `docs/内容` | `docs/api-reference-update` |
| リリース | `release/バージョン` | `release/v2.3.0` |

## コミットメッセージ

Conventional Commits 形式を使用する。プレフィックス一覧：

| プレフィックス | 用途 |
|---|---|
| `feat:` | 新機能 |
| `fix:` | バグ修正 |
| `docs:` | ドキュメントのみの変更 |
| `refactor:` | リファクタリング（機能変更なし） |
| `test:` | テストの追加・修正 |
| `chore:` | ビルド・依存関係・設定の変更 |
| `perf:` | パフォーマンス改善 |
| `style:` | フォーマット変更（ロジック変更なし） |

```bash
# 例
git commit -m "feat(user-profile): プロフィール画像のアップロード機能を追加"
git commit -m "fix(auth): ログイントークンの有効期限切れ処理を修正"
git commit -m "docs(api): 注文 API のリクエスト仕様を更新"
```

## プルリクエスト（PR）のルール

- **レビュアーは最低2名**の承認を得てからマージする（セルフマージ禁止）
- PR のタイトルもコミットメッセージと同じ形式にする
- レビュー依頼前にセルフレビューを必ず実施する
- 大きな変更は事前に設計レビュー（デザインドク）を行う

### PR テンプレート

```markdown
## 変更内容

- 何を変更したか（箇条書き）

## 変更の理由

なぜこの変更が必要か

## テスト方法

どのようにテストしたか

## 関連チケット

Backlog: #123 / Jira: PROJ-456
```

## マージ戦略

- `main` / `master` への直接プッシュは禁止
- マージ方法は **Squash merge** を基本とする（コミット履歴をきれいに保つ）
- リリースブランチは **Merge commit** を使用する（履歴を保持するため）

## リリースフロー

1. `feature/*` ブランチで開発
2. `develop` ブランチに PR を出してマージ
3. リリース前に `release/vX.X.X` ブランチを切る
4. `release/*` ブランチでバグ修正・バージョン更新
5. `main` と `develop` の両方にマージ
6. `main` にタグ（`vX.X.X`）を打つ
