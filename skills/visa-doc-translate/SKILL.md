---
name: visa-doc-translate
description: ビザ申請書類（画像）を英語に翻訳し、元の書類と翻訳を含むバイリンガル PDF を作成します。
---

ビザ申請のための書類翻訳をサポートします。

## 手順

ユーザーが画像ファイルのパスを提供した場合、確認を求めずに以下のステップを**自動的に実行**してください：

1. **画像変換**: ファイルが HEIC の場合、`sips -s format png <input> --out <output>` で PNG に変換する

2. **画像の回転**:
   - EXIF の向きデータを確認する
   - EXIF データに基づいて自動的に画像を回転する
   - EXIF の向きが 6 の場合は反時計回りに 90 度回転する

3. **OCR テキスト抽出**:
   - 以下の OCR 方法を自動的に試みる：
     - macOS Vision フレームワーク（macOS では推奨）
     - EasyOCR（クロスプラットフォーム、tesseract 不要）
     - Tesseract OCR（利用可能な場合）
   - 書類からすべてのテキスト情報を抽出する
   - 書類の種類を特定する（預金証明書・在職証明書・退職証明書など）

4. **翻訳**:
   - すべてのテキストコンテンツを専門的な英語に翻訳する
   - 元の書類の構造とフォーマットを維持する
   - ビザ申請に適した専門用語を使用する
   - 固有名詞は元の言語を括弧内の英語とともに保持する
   - 中国語の名前はピンイン形式を使用する（例: WU Zhengye）
   - すべての数値・日付・金額を正確に保持する

5. **PDF 生成**:
   - PIL と reportlab ライブラリを使用した Python スクリプトを作成する
   - 1ページ目: A4 ページに収まるようにスケーリングして回転済みの元画像を中央に表示する
   - 2ページ目: 英語翻訳を適切なフォーマットで表示する：
     - タイトルは中央揃えで太字
     - 本文は適切な間隔で左揃え
     - 公式書類に適したプロフェッショナルなレイアウト
   - 下部に注記を追加する: "This is a certified English translation of the original document"
   - スクリプトを実行して PDF を生成する

6. **アウトプット**: 同じディレクトリに `<元のファイル名>_Translated.pdf` という名前の PDF ファイルを作成する

## 対応書類

- 銀行預金証明書
- 収入証明書
- 在職証明書
- 退職証明書
- 不動産証明書
- 事業許可証
- 身分証明書・パスポート
- その他の公式書類

## 技術的な実装

### OCR 方法（試行順）

1. **macOS Vision フレームワーク**（macOS のみ）:
   ```python
   import Vision
   from Foundation import NSURL
   ```

2. **EasyOCR**（クロスプラットフォーム）:
   ```bash
   pip install easyocr
   ```

3. **Tesseract OCR**（利用可能な場合）:
   ```bash
   brew install tesseract tesseract-lang
   pip install pytesseract
   ```

### 必要な Python ライブラリ

```bash
pip install pillow reportlab
```

macOS Vision フレームワーク用:
```bash
pip install pyobjc-framework-Vision pyobjc-framework-Quartz
```

## 重要なガイドライン

- 各ステップでユーザーの確認を求めない
- 最適な回転角度を自動的に判断する
- 一つの方法が失敗した場合は複数の OCR 方法を試みる
- すべての数値・日付・金額が正確に翻訳されていることを確認する
- クリーンでプロフェッショナルなフォーマットを使用する
- プロセス全体を完了して最終的な PDF の場所を報告する

## 使用例

```bash
/visa-doc-translate RetirementCertificate.PNG
/visa-doc-translate BankStatement.HEIC
/visa-doc-translate EmploymentLetter.jpg
```

## アウトプット例

スキルは以下を実行します：
1. 利用可能な OCR 方法でテキストを抽出する
2. プロフェッショナルな英語に翻訳する
3. 以下を含む `<ファイル名>_Translated.pdf` を生成する：
   - 1ページ目: 元の書類画像
   - 2ページ目: プロフェッショナルな英語翻訳

オーストラリア・アメリカ・カナダ・イギリスなど翻訳書類を必要とする国へのビザ申請に最適。
