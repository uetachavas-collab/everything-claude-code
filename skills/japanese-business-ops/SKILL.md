---
name: japanese-business-ops
description: 日本のビジネス業務（稟議・報連相・会計・インボイス制度）をシステムに実装するためのガイド。承認フロー設計・通知設計・税務対応に使用してください。
origin: ECC
---

# 日本ビジネス業務スキル

日本固有のビジネス慣習をシステム実装に反映するためのガイドライン。

## 使いどき

- 稟議（承認フロー）機能を実装するとき
- 報告・連絡・相談（報連相）の通知設計をするとき
- 消費税計算・インボイス制度対応をするとき
- 電子帳簿保存法（電帳法）対応のシステムを構築するとき
- 日本語での業務フロー設計・レビューをするとき

## 仕組み

### 稟議（承認フロー）の実装

#### 基本フロー

```
起案 → 回覧 → 承認（直列） → 完了 / 否決
```

承認は**順番に**行う（上位者が先に承認する）。承認者リストを配列で管理し、現在の承認待ちインデックスを追跡する。

#### データモデル

```typescript
interface RingiRequest {
  id: string;
  title: string;           // 件名
  requester: UserId;       // 申請者
  requestDate: Date;       // 申請日
  amount?: number;         // 金額（費用稟議の場合）
  purpose: string;         // 目的・理由
  approvers: ApproverStep[];
  currentStep: number;     // 現在の承認ステップ（0始まり）
  status: RingiStatus;
  comments: Comment[];     // 差し戻し理由・コメント
}

interface ApproverStep {
  userId: UserId;
  role: string;            // 例: '部長', '経理担当'
  status: 'pending' | 'approved' | 'rejected';
  approvedAt?: Date;
  comment?: string;
}
```

#### ステータス遷移

```
draft → pending（申請）
pending → approved（全承認者が承認）
pending → rejected（いずれかの承認者が否決）
pending → draft（差し戻し）
pending → withdrawn（申請者が取り下げ）
```

### 報連相の通知設計

報告・連絡・相談の種別に応じて通知先とチャンネルを分ける。

| 種別 | 緊急度 | 通知先 | チャンネル |
|---|---|---|---|
| 報告 | 低 | 直属の上長 | Slack / メール |
| 連絡 | 中 | 関係者全員 | Slack チャンネル |
| 相談 | 状況による | 相談相手 | ダイレクトメッセージ |
| 障害報告 | 高 | 全関係者 | Slack @here + メール |

### 消費税計算

```typescript
const TAX_RATES = {
  STANDARD: 0.10,   // 標準税率（電化製品・衣料品等）
  REDUCED: 0.08,    // 軽減税率（食料品・週2回以上発行の新聞）
} as const;

// 標準的な税額計算（税抜価格から）
function calculateTax(priceExTax: number, rate = TAX_RATES.STANDARD): number {
  return Math.floor(priceExTax * rate);
}

// 税込価格の計算
function priceIncludingTax(priceExTax: number, rate = TAX_RATES.STANDARD): number {
  return priceExTax + calculateTax(priceExTax, rate);
}

// 税込価格から税額を逆算（内税）
function extractTax(priceIncTax: number, rate = TAX_RATES.STANDARD): number {
  return Math.floor(priceIncTax * rate / (1 + rate));
}
```

### インボイス制度（適格請求書等保存方式）

2023年10月1日から施行。請求書に以下を必ず含める。

```typescript
interface Invoice {
  invoiceNumber: string;          // 請求書番号
  issuerRegistrationNumber: string; // 登録番号（T + 13桁）
  issueDate: Date;                // 発行日
  items: InvoiceItem[];
  subtotal: number;               // 税抜合計
  taxBreakdown: TaxBreakdown[];   // 税率別の税額内訳
  total: number;                  // 税込合計
}

interface TaxBreakdown {
  rate: number;          // 税率（0.10 または 0.08）
  targetAmount: number;  // 対象金額（税抜）
  taxAmount: number;     // 税額
}
```

### 電子帳簿保存法（電帳法）対応

電子取引のデータ保存に必要な要件：

1. **真実性の確保** — タイムスタンプを付与するか、訂正削除の履歴を残す
2. **可視性の確保** — 日付・金額・取引先でのキーワード検索が可能
3. **保存期間** — 法人は原則7年（欠損金あり：10年）

```typescript
interface ElectronicRecord {
  id: string;
  createdAt: Date;
  timestamp?: string;    // 認定タイムスタンプ（TSA）
  hash: string;          // SHA-256 ハッシュ（改ざん検知）
  fileUrl: string;       // 保存先URL
  metadata: {
    date: Date;          // 取引日
    amount: number;      // 金額
    counterparty: string; // 取引先
    category: string;    // 勘定科目
  };
}
```

## 出力フォーマット

### 稟議書の出力例

```
件名: クラウドサービス費用の承認申請
申請者: 山田太郎（開発部）
申請日: 2026年04月10日
金額: ¥528,000（税込）

【目的・理由】
開発環境の CI/CD パイプライン強化のため、
GitHub Actions の有料プランへの移行を申請します。

【承認フロー】
1. 田中部長 ← 現在待ち
2. 鈴木経理部長
3. 佐藤取締役

関連資料: [見積書](https://...)
```

## アンチパターン

| 避けること | 理由 |
|---|---|
| 承認を並列で処理する | 日本の稟議は直列が基本 |
| 差し戻し理由を記録しない | 修正・再申請の際に必要 |
| 税率を固定値でハードコードする | 税率変更に対応できない |
| タイムスタンプなしで電子データを保存する | 電帳法の真実性要件を満たせない |
