# Vibe Coding Template

Claude Code設定のテンプレートリポジトリです。効率的で安全な開発環境をすぐに構築できます。

## 概要

このテンプレートは、HTML/CSS、JavaScript/TypeScript（React含む）、Google Apps Script、Go、Astroプロジェクト向けに最適化されたClaude Code環境を提供します。

## 特徴

- **包括的なセキュリティ設定** - 機密情報の漏洩防止、危険コマンドの検出
- **多言語対応** - JavaScript/TypeScript、Go、Astro、Google Apps Script
- **自動コード品質チェック** - Biome、TypeScript、セキュリティスキャンの統合
- **開発効率向上** - カスタムスラッシュコマンド、自動フック実行

## 含まれる設定

### セキュリティ機能
- 機密情報（API キー、パスワード）のハードコーディング検出
- 危険コマンド（`rm -rf`、`sudo rm`等）の実行防止
- 権限変更やシステム操作の確認プロンプト

### 開発支援
- **Biome** - JavaScript/TypeScriptのリンティングとフォーマット
- **TypeScript** - 型チェックと静的解析
- **Go** - フォーマット、静的解析、セキュリティスキャン
- **Astro** - プロジェクト検証とビルドテスト

### フック管理
- `pre_commit` - コミット前の軽量チェック
- `pre_push` - プッシュ前の包括的テスト
- `pre_deploy` - デプロイ前の最終検証

## 使用方法

### 1. テンプレートの利用

```bash
# このリポジトリをテンプレートとして新しいリポジトリを作成
# または既存プロジェクトにコピー
cp -r docs/ your-project/
```

### 2. 必要ツールのインストール

#### JavaScript/TypeScript プロジェクト
```bash
npm install -D @biomejs/biome
npx biome init
```

#### Go プロジェクト
```bash
go install github.com/securecodewarrior/gosec/v2/cmd/gosec@latest
go install golang.org/x/vuln/cmd/govulncheck@latest
```

#### Google Apps Script
```bash
npm install -g @google/clasp
clasp login
```

### 3. Claude Code での利用

```bash
# プロジェクトディレクトリで Claude Code を起動
claude-code

# 設定の確認
/config

# メモリファイルの編集
/memory
```

## ディレクトリ構造

```
docs/
├── claude/
│   ├── HOOKS.md      # フック設定の詳細解説
│   ├── SETTING.md    # 設定ファイルの全行解説
│   └── SLASH.md      # スラッシュコマンドガイド
└── README.md         # このファイル
```

## カスタマイズ

### プロジェクト固有の設定
- `.claude/settings.json` で設定をカスタマイズ
- `.claude/commands/` でカスタムスラッシュコマンドを作成

### チーム向け設定
1. セキュリティレベルの調整
2. 使用言語に応じたフックの有効化/無効化
3. プロジェクト固有のコマンドの追加

## 貢献

設定の改善や新しい言語サポートの追加は歓迎します。Issues や Pull Requests をお待ちしています。

## ライセンス

このテンプレートはMITライセンスの下で公開されています。