# テストガイドライン（日本チーム向け）

> [common/testing.md](../common/testing.md) の共通ルールを拡張した、日本チーム向けの追加規約。

## テストの基本方針

- 新機能は必ずテストを書く（テストなしのマージは原則禁止）
- テストのコメントや説明文は**日本語**で記載する
- テストファイルの命名: `*.test.ts` / `*.spec.ts`

## テストの構造

日本語のテスト説明文を使って、何をテストしているかを明確にする。

```typescript
describe('UserService', () => {
  describe('getUserById', () => {
    it('存在するユーザーIDを渡すとユーザー情報を返す', async () => {
      // ...
    });

    it('存在しないユーザーIDを渡すと UserNotFoundError をスローする', async () => {
      // ...
    });

    it('ユーザーIDが空文字の場合 ValidationError をスローする', async () => {
      // ...
    });
  });
});
```

## テストの種別と優先度

| 種別 | 目的 | 優先度 |
|---|---|---|
| 単体テスト | 個々の関数・クラスの動作確認 | 高 |
| 統合テスト | モジュール間の連携確認 | 高 |
| E2E テスト | ユーザー操作フローの確認 | 中 |
| パフォーマンステスト | 負荷・速度の確認 | 低（本番前に必須） |

## テストカバレッジ

- 新規コードのカバレッジ目標: **80%以上**
- クリティカルなビジネスロジック: **95%以上**
- カバレッジレポートは CI で自動生成する

## モックの使い方

- 外部 API・データベースへの呼び出しはモック化する
- 日本特有の API（e-Tax・gBizID・マイナポータル等）のモックデータを用意する

```typescript
// 日本の郵便番号 API のモック例
jest.mock('../services/postal-code-api', () => ({
  lookup: jest.fn().mockResolvedValue({
    postalCode: '100-0001',
    prefecture: '東京都',
    city: '千代田区',
    street: '千代田',
  }),
}));
```

## 日本特有のテストケース

### 文字コード・文字列

```typescript
it('全角文字を含む氏名を正しく処理できる', () => {
  expect(formatName('田中　太郎')).toBe('田中 太郎'); // 全角スペースを半角に変換
});

it('半角カナを含む入力を正規化できる', () => {
  expect(normalizeKana('ｱｲｳｴｵ')).toBe('アイウエオ');
});
```

### 日付・和暦

```typescript
it('西暦を和暦に変換できる', () => {
  expect(toJapaneseEra(new Date('2026-04-10'))).toBe('令和8年4月10日');
});

it('元号をまたぐ日付計算が正しい', () => {
  // 平成から令和への移行（2019-05-01）
  expect(calculateAge(new Date('1989-01-08'), new Date('2026-04-10'))).toBe(37);
});
```

### 金額・数値

```typescript
it('消費税（10%）を正しく計算できる', () => {
  expect(calculateTax(1000)).toBe(100);
  expect(totalWithTax(1000)).toBe(1100);
});

it('軽減税率（8%）が適用される商品を判定できる', () => {
  expect(getTaxRate('食料品')).toBe(0.08);
  expect(getTaxRate('電化製品')).toBe(0.10);
});
```
