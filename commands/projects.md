---
name: projects
description: 既知のプロジェクトとその統計情報を一覧表示する
command: true
---

# Projects コマンド

プロジェクトレジストリのエントリと、継続学習の統計情報を一覧表示します。

## 実装

プラグインルートパスを使って CLI を実行する：

```bash
python3 "${CLAUDE_PLUGIN_ROOT}/skills/continuous-learning-v2/scripts/instinct-cli.py" projects
```

`CLAUDE_PLUGIN_ROOT` が設定されていない場合（手動インストール時）：

```bash
python3 ~/.claude/skills/continuous-learning-v2/scripts/instinct-cli.py projects
```

## 使い方

```bash
/projects
```

## 表示内容

1. `~/.claude/homunculus/projects.json` を読む
2. 各プロジェクトについて以下を表示：
   - プロジェクト名・ID・ルートパス・リモート
   - 個人・継承の統計件数
   - 観察イベント件数
   - 最終確認タイムスタンプ
3. 全体の合計統計も表示
