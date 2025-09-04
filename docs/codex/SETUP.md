## Codex CLI セットアップガイド

このドキュメントは Codex CLI の導入を最短で完了するための手順をまとめたものです。細かな利用方法は `docs/codex/USAGE.md` を参照してください。

### 前提条件

- macOS または Linux 環境（Windows の場合は WSL 推奨）
- ターミナルと Git が利用可能
- パッケージ管理ツール（例: Homebrew）が入っていると簡単

最新のインストール方法は公式リポジトリ（https://github.com/openai/codex）を参照してください。

### インストール

- Homebrew（例）: `brew install codex`
- それ以外: 公式リポジトリの手順に従ってインストール

インストール後、`codex --version` で導入確認ができます。

### 初回起動とサインイン

```bash
codex
```

表示例:

```
> Welcome to Codex, OpenAI's command-line coding agent

> Sign in with ChatGPT to use Codex as part of your paid plan
  or connect an API key for usage-based billing

  1. Sign in with ChatGPT
  2. Provide your own API key

  Press Enter to continue
```

- ChatGPT サインインを選ぶと、ブラウザが開いて認証します。
- API キーを使う場合はプロンプトに従ってキーを入力します。

サインイン後の注意事項表示の例:

```
✓ Signed in with your ChatGPT account

> Before you start:
  Decide how much autonomy you want to grant Codex
  Codex can make mistakes — review generated code and commands

  Press Enter to continue
```

### リポジトリでの初期設定

Codex は現在の作業ディレクトリ（例: このリポジトリ）を検出します。バージョン管理下のフォルダでは、自動実行か都度承認かを選択できます。

```
> You are running Codex in <your/repo/path>

  Since this folder is version controlled, you may wish to allow Codex
  to work in this folder without asking for approval.

  1. Yes, allow Codex to work in this folder without asking for approval
  2. No, ask me to approve edits and commands

  Press Enter to continue
```

推奨: 初期は「都度承認（No, ask me...）」で開始し、運用に慣れてから自動許可を検討します。

### 推奨設定（このリポジトリ）

- 承認ポリシー: on-request（重要操作のみ許可）
- ファイルサンドボックス: workspace-write 以上でドキュメント編集を許可
- ネットワーク: restricted（必要時のみ許可）

これらは CI やチーム方針に合わせて調整してください。安全性を優先し、破壊的操作は常に明示許可します。

### セッションの進め方（概要）

1) 目的を 1 行で伝える（対象・完成条件を明確に）
2) Codex の前置き（preamble）→ 参照ファイルの読み取り
3) 変更提案（差分案）→ `apply_patch` で適用
4) 変更範囲の最小検証（ビルド/テスト）
5) 進捗・次アクションの確認

詳細は `docs/codex/USAGE.md` を参照。

### ヘルスチェック（任意）

- 権限・サンドボックス: 書き込みやネットワークが必要な操作で承認が求められるか確認
- 読み取り上限: 長大ファイルは 200 行単位で読むなど、出力制限への対処を把握

### チーム/CI 向け（非対話）

- API キー方式での起動を利用し、承認モードを保守的に設定
- 破壊的操作は CI 上では実行せず、レビュー後に人間が実行

### トラブルシューティング

- ブラウザが開かない: 端末のデフォルトブラウザ設定を確認。必要なら API キー方式を利用
- 権限不足で失敗: 承認モードを確認し、必要操作は明示承認
- ネットワーク制限で失敗: オフライン前提で進める or 必要な場面だけ許可
- 出力が長すぎて切れる: ファイルは分割して読む（`sed -n '1,200p'` など）

### 参考

- 公式: https://github.com/openai/codex
- 運用: `docs/codex/USAGE.md`, `docs/codex/SETTING.md`, `docs/codex/HOOKS.md`, `docs/codex/SLASH.md`
