# スラッシュコマンド

> インタラクティブセッション中にスラッシュコマンドでClaudeの動作を制御します。

## 組み込みスラッシュコマンド

| コマンド                      | 目的                                                             |
| :------------------------ | :------------------------------------------------------------- |
| `/add-dir`                | 追加の作業ディレクトリを追加                                                 |
| `/bug`                    | バグを報告（会話をAnthropicに送信）                                         |
| `/clear`                  | 会話履歴をクリア                                                       |
| `/compact [instructions]` | オプションのフォーカス指示で会話をコンパクト化                                        |
| `/config`                 | 設定の表示/変更                                                       |
| `/cost`                   | トークン使用統計を表示                                                    |
| `/doctor`                 | Claude Codeインストールの健全性をチェック                                     |
| `/help`                   | 使用方法のヘルプを取得                                                    |
| `/init`                   | CLAUDE.mdガイドでプロジェクトを初期化                                        |
| `/login`                  | Anthropicアカウントを切り替え                                            |
| `/logout`                 | Anthropicアカウントからサインアウト                                         |
| `/mcp`                    | MCPサーバー接続とOAuth認証を管理                                           |
| `/memory`                 | CLAUDE.mdメモリファイルを編集                                            |
| `/model`                  | AIモデルを選択または変更                                                  |
| `/permissions`            | [権限](/ja/docs/claude-code/iam#configuring-permissions)を表示または更新 |
| `/pr_comments`            | プルリクエストコメントを表示                                                 |
| `/review`                 | コードレビューをリクエスト                                                  |
| `/status`                 | アカウントとシステムのステータスを表示                                            |
| `/terminal-setup`         | 改行用のShift+Enterキーバインドをインストール（iTerm2とVSCodeのみ）                  |
| `/vim`                    | 挿入モードとコマンドモードを交互に切り替えるvimモードに入る                                |

## カスタムスラッシュコマンド

カスタムスラッシュコマンドを使用すると、Claude Codeが実行できる頻繁に使用されるプロンプトをMarkdownファイルとして定義できます。コマンドはスコープ（プロジェクト固有または個人）で整理され、ディレクトリ構造を通じて名前空間をサポートします。

### 構文

```
/<prefix>:<command-name> [arguments]
```

#### パラメータ

| パラメータ            | 説明                                           |
| :--------------- | :------------------------------------------- |
| `<prefix>`       | コマンドスコープ（プロジェクト固有の場合は`project`、個人の場合は`user`） |
| `<command-name>` | Markdownファイル名から派生した名前（`.md`拡張子なし）            |
| `[arguments]`    | コマンドに渡されるオプションの引数                            |

### コマンドタイプ

#### プロジェクトコマンド

リポジトリに保存され、チームと共有されるコマンド。

**場所**: `.claude/commands/`\
**プレフィックス**: `/project:`

次の例では、`/project:optimize`コマンドを作成します：

```bash
# プロジェクトコマンドを作成
mkdir -p .claude/commands
echo "このコードのパフォーマンスの問題を分析し、最適化を提案してください：" > .claude/commands/optimize.md
```

#### 個人コマンド

すべてのプロジェクトで利用可能なコマンド。

**場所**: `~/.claude/commands/`\
**プレフィックス**: `/user:`

次の例では、`/user:security-review`コマンドを作成します：

```bash
# 個人コマンドを作成
mkdir -p ~/.claude/commands
echo "このコードのセキュリティ脆弱性をレビューしてください：" > ~/.claude/commands/security-review.md
```

### 機能

#### 名前空間

サブディレクトリでコマンドを整理して、名前空間付きコマンドを作成します。

**構造**: `<prefix>:<namespace>:<command>`

例えば、`.claude/commands/frontend/component.md`にあるファイルは`/project:frontend:component`コマンドを作成します

#### 引数

`$ARGUMENTS`プレースホルダーを使用してコマンドに動的な値を渡します。

例：

```bash
# コマンド定義
echo "コーディング標準に従って問題 #$ARGUMENTS を修正してください" > .claude/commands/fix-issue.md

# 使用方法
> /project:fix-issue 123
```

#### Bashコマンド実行

`!`プレフィックスを使用してスラッシュコマンドが実行される前にbashコマンドを実行します。出力はコマンドコンテキストに含まれます。

例：

```markdown
---
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*)
description: gitコミットを作成
---

## コンテキスト

- 現在のgitステータス: !`git status`
- 現在のgit diff（ステージされた変更とステージされていない変更）: !`git diff HEAD`
- 現在のブランチ: !`git branch --show-current`
- 最近のコミット: !`git log --oneline -10`

## あなたのタスク

上記の変更に基づいて、単一のgitコミットを作成してください。
```

#### ファイル参照

`@`プレフィックスを使用して[ファイルを参照](/ja/docs/claude-code/common-workflows#reference-files-and-directories)し、コマンドにファイル内容を含めます。

例：

```markdown
# 特定のファイルを参照
@src/utils/helpers.jsの実装をレビューしてください

# 複数のファイルを参照
@src/old-version.jsと@src/new-version.jsを比較してください
```

#### 思考モード

スラッシュコマンドは[拡張思考キーワード](/ja/docs/claude-code/common-workflows#use-extended-thinking)を含めることで拡張思考をトリガーできます。

### ファイル形式

コマンドファイルは以下をサポートします：

* **Markdown形式**（`.md`拡張子）
* **YAMLフロントマター**（メタデータ用）：
  * `allowed-tools`: コマンドが使用できるツールのリスト
  * `description`: コマンドの簡潔な説明
* **動的コンテンツ**（bashコマンド（`!`）とファイル参照（`@`）付き）
* **プロンプト指示**（メインコンテンツとして）

## MCPスラッシュコマンド

MCPサーバーは、Claude Codeで利用可能になるスラッシュコマンドとしてプロンプトを公開できます。これらのコマンドは接続されたMCPサーバーから動的に発見されます。

### コマンド形式

MCPコマンドは次のパターンに従います：

```
/mcp__<server-name>__<prompt-name> [arguments]
```

### 機能

#### 動的発見

MCPコマンドは以下の場合に自動的に利用可能になります：

* MCPサーバーが接続されアクティブである
* サーバーがMCPプロトコルを通じてプロンプトを公開している
* 接続中にプロンプトが正常に取得される

#### 引数

MCPプロンプトはサーバーによって定義された引数を受け取ることができます：

```
# 引数なし
> /mcp__github__list_prs

# 引数あり
> /mcp__github__pr_review 456
> /mcp__jira__create_issue "バグタイトル" high
```

#### 命名規則

* サーバーとプロンプト名は正規化される
* スペースと特殊文字はアンダースコアになる
* 一貫性のために名前は小文字になる

### MCP接続の管理

`/mcp`コマンドを使用して：

* 設定されたすべてのMCPサーバーを表示
* 接続ステータスをチェック
* OAuth対応サーバーで認証
* 認証トークンをクリア
* 各サーバーから利用可能なツールとプロンプトを表示

## 関連項目

* [インタラクティブモード](/ja/docs/claude-code/interactive-mode) - ショートカット、入力モード、インタラクティブ機能
* [CLIリファレンス](/ja/docs/claude-code/cli-reference) - コマンドラインフラグとオプション
* [設定](/ja/docs/claude-code/settings) - 設定オプション
* [メモリ管理](/ja/docs/claude-code/memory) - セッション間でのClaudeのメモリ管理
