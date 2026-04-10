---
paths:
  - "**/*.ts"
  - "**/*.tsx"
  - "**/*.js"
  - "**/*.jsx"
  - "**/*.py"
---
# 日本語コーディングスタイルガイドライン

> [common/coding-style.md](../common/coding-style.md) の共通ルールを拡張した、日本チーム向けの追加規約。

## コメントの書き方

### 基本方針

- コメントはすべて**日本語**で記載する
- コードが「何をしているか」より「なぜそうしているか」を説明する
- 自明な処理にコメントを付けない

```typescript
// NG: 自明なコメント
// ユーザーIDを取得する
const userId = user.id;

// OK: 理由を説明するコメント
// キャッシュキーに userId を含めることで、ユーザーごとのデータ競合を防ぐ
const cacheKey = `user:${user.id}:profile`;
```

### コメントスタイル

- 箇条書きは句点（。）を省略してよい
- 長い説明は複数行に分ける
- TODO は `// TODO(担当者名): 内容` の形式で記載する

```typescript
// TODO(tanaka): 2026年Q3 に廃止予定の旧 API 対応を削除する
```

## 命名規則

共通ルール（[common/coding-style.md](../common/coding-style.md)）の命名規則に従いつつ、以下を追加する。

| 対象 | 形式 | 例 |
|---|---|---|
| 変数・関数名 | camelCase | `getUserProfile`, `isLoggedIn` |
| 定数 | SCREAMING_SNAKE_CASE | `MAX_RETRY_COUNT`, `API_BASE_URL` |
| コンポーネント | PascalCase | `UserProfileCard`, `LoginForm` |
| ファイル名 | kebab-case | `user-profile.tsx`, `auth-service.ts` |

### 日本語ドメイン用語のマッピング

日本固有のビジネス用語は英語名に統一して使用する。

| 日本語 | 英語変数名 |
|---|---|
| 稟議 | `ringi` / `approvalRequest` |
| 担当者 | `assignee` / `responsible` |
| 得意先 | `client` / `customer` |
| 仕入先 | `supplier` / `vendor` |
| 締め日 | `cutoffDate` |
| 支払サイト | `paymentTerms` |

## 日付・数値のフォーマット

```typescript
// 日付フォーマット（表示用）
const formattedDate = new Intl.DateTimeFormat('ja-JP', {
  year: 'numeric',
  month: '2-digit',
  day: '2-digit',
  timeZone: 'Asia/Tokyo',
}).format(date);
// → 2026年04月10日

// 通貨フォーマット（円）
const formattedPrice = new Intl.NumberFormat('ja-JP', {
  style: 'currency',
  currency: 'JPY',
}).format(amount);
// → ¥1,234,567
```

## エラーメッセージの書き方

ユーザー向けエラーメッセージは日本語で、技術的詳細はログに出力する。

```typescript
// NG: 英語のエラーメッセージ
throw new Error('User not found');

// OK: 日本語のユーザー向けメッセージ + ログ
logger.error('User lookup failed', { userId, error });
throw new UserFacingError('ユーザーが見つかりませんでした。IDを確認してください。');
```

## ファイル構成

- 1ファイル 200〜400 行が目安、800 行以上は分割を検討する
- 機能・ドメインごとにディレクトリを切る（型別ではなく）

## コード品質チェックリスト

コミット前に確認する。

- [ ] コメントは日本語で書かれているか
- [ ] 変数名・関数名が英語で意味が通るか
- [ ] 日付・数値のフォーマットが日本式か
- [ ] エラーメッセージが日本語でユーザーフレンドリーか
- [ ] 共通ルールのチェックリストも通過しているか
