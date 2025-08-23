## Claude Code Hooks 設定ガイド

このドキュメントでは、Claude Code Hooks の設定と使用方法について詳しく解説します。HTML/CSS、JavaScript/TypeScript（React含む）、Google Apps Script、Go、Astro プロジェクト向けの実用的な設定例も含みます。

## 概要

Claude Code Hooks は、Claude Code の動作を様々なタイミングでカスタマイズできる強力な機能です。ツール使用前後、プロンプト送信時、セッション開始時など、特定のイベントで自動的にコマンドを実行し、検証やコンテキストの追加を行うことができます。

**注意**: フックは強力な機能ですが、セキュリティリスクも伴います。設定するコマンドは慎重に検証してください。

## フック種別とタイミング

### イベント種別

- **PreToolUse**: ツール使用前に実行
- **PostToolUse**: ツール使用後に実行  
- **UserPromptSubmit**: ユーザープロンプト送信時に実行
- **SessionStart**: セッション開始時に実行

### 対応技術スタック

この設定では以下の技術スタックに対応しています：

- **フロントエンド**: HTML, CSS, JavaScript, TypeScript, React, Astro
- **バックエンド**: Go
- **自動化**: Google Apps Script
- **ツール**: Biome（リンター・フォーマッター）

### フック設定形式

フックは設定ファイル（`~/.claude/settings.json`や`.claude/settings.json`）で定義します：

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write",
        "hooks": [
          {
            "type": "command",
            "command": "./scripts/validate-code.sh"
          }
        ]
      }
    ],
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command", 
            "command": "echo 'Prompt submitted at: $(date)'"
          }
        ]
      }
    ]
  }
}
```

### 実行段階とタイミング

#### PreToolUse (ツール使用前)
軽量なチェックや検証を実行。開発者の生産性を損なわない範囲で基本的な品質を確保します。

#### PostToolUse (ツール使用後)
包括的なテスト・セキュリティチェック。時間はかかりますが、より徹底した検証を行います。

#### UserPromptSubmit (プロンプト送信時)
ユーザー入力に対する追加コンテキストの提供や前処理を実行します。

#### SessionStart (セッション開始時)
環境セットアップや初期化処理を実行。依存関係の確認や設定ファイルの準備を自動化します。

## 言語別設定詳細

### JavaScript/TypeScript プロジェクト

#### Biome による静的解析
```json
"biome_linting_and_formatting": {
  "command": "npx biome check --apply .",
  "description": "Biome による JavaScript/TypeScript の静的解析とフォーマット"
}
```

**実行条件:**
- `biome.json` または `biome.jsonc` ファイルが存在
- `.js`, `.ts`, `.jsx`, `.tsx`, `.mjs`, `.cjs` ファイルが存在

**機能:**
- 構文エラーの検出
- コードスタイルの統一
- 自動フォーマット適用
- パフォーマンス問題の指摘

#### TypeScript 型チェック
```json
"typescript_check": {
  "command": "npx tsc --noEmit",
  "description": "TypeScript の型チェック"
}
```

**実行条件:**
- `tsconfig.json` ファイルが存在
- TypeScript ファイル（`.ts`, `.tsx`）が存在

**機能:**
- 型安全性の検証
- 未使用変数の検出
- インターフェース準拠の確認

#### パッケージマネージャー対応
システムは以下の優先順位でパッケージマネージャーを自動検出します：

1. **pnpm** - `pnpm-lock.yaml` の存在で判定
2. **yarn** - `yarn.lock` の存在で判定
3. **npm** - `package-lock.json` の存在で判定

それぞれに対応したセキュリティ監査とインストールコマンドが実行されます。

### Go プロジェクト

#### コードフォーマットと静的解析
```json
"go_formatting": {
  "command": "go fmt ./...",
  "description": "Go コードのフォーマット"
},
"go_vet": {
  "command": "go vet ./...",
  "description": "Go コードの静的解析"
}
```

**実行条件:**
- `go.mod` ファイルが存在
- `.go` ファイルが存在
- `go` コマンドが利用可能

#### セキュリティスキャン
```json
"go_security_scan": {
  "command": "gosec ./...",
  "description": "Go セキュリティスキャン"
},
"go_vulnerability_check": {
  "command": "govulncheck ./...",
  "description": "Go 脆弱性チェック"
}
```

**機能:**
- セキュリティ脆弱性の検出
- 既知の脆弱な依存関係の確認
- 危険なコーディングパターンの指摘

### Astro プロジェクト

#### プロジェクト検証
```json
"astro_check": {
  "command": "npx astro check",
  "description": "Astro プロジェクトの型チェックと検証"
}
```

**実行条件:**
- `astro.config.mjs`, `astro.config.js`, または `astro.config.ts` が存在
- `.astro` ファイルが存在

**機能:**
- Astro コンポーネントの構文チェック
- TypeScript 型検証
- 設定ファイルの妥当性確認

#### ビルドテスト
```json
"astro_build_test": {
  "command": "npx astro build",
  "description": "Astro ビルドテスト"
}
```

本番環境での正常な動作を保証するため、ビルドプロセスを事前に検証します。

### Google Apps Script

#### プロジェクト検証
```json
"google_apps_script_validation": {
  "command": "npx @google/clasp push --dry-run",
  "description": "Google Apps Script プッシュ検証"
}
```

**実行条件:**
- `.clasp.json` ファイルが存在
- `appsscript.json` ファイルが存在

**機能:**
- Apps Script プロジェクトの構文チェック
- 権限設定の検証
- デプロイ前の事前確認

#### 認証状態確認
```json
"google_apps_script_setup": {
  "command": "npx @google/clasp login --check",
  "description": "Google Apps Script 認証状態の確認"
}
```

Google Apps Script の操作に必要な認証が正常に設定されているかを確認します。

### HTML/CSS

#### HTML 検証
```json
"html_validation": {
  "command": "npx html-validate *.html **/*.html",
  "description": "HTML ファイルの構文検証"
}
```

**機能:**
- HTML5 仕様への準拠チェック
- アクセシビリティ問題の検出
- SEO 観点での問題指摘

#### CSS 検証
```json
"css_validation": {
  "command": "npx stylelint '**/*.css' --fix",
  "description": "CSS ファイルの検証とフォーマット"
}
```

**実行条件:**
- `.stylelintrc.json`, `.stylelintrc.js`, または `stylelint.config.js` が存在

**機能:**
- CSS 構文エラーの検出
- ベストプラクティスの強制
- 自動修正の適用

## セキュリティ機能

### 機密情報検出
```json
"security_scan": {
  "command": "grep -r -n -E '(password|api_key|secret|token)\\s*=\\s*[\"\\'][^\"\\'']+[\"\\']' .",
  "description": "機密情報のハードコーディングをチェック"
}
```

以下のパターンを検出し、機密情報の漏洩を防止します：
- パスワード
- API キー
- シークレットキー
- 認証トークン
- 秘密鍵

### 危険コマンド検出
```json
"dangerous_commands_check": {
  "command": "grep -r -n -E '(rm\\s+-rf|sudo\\s+rm|del\\s+/f|format\\s+[A-Z]:)' .",
  "description": "危険なコマンドの使用をチェック"
}
```

以下の破壊的なコマンドの使用を防止します：
- `rm -rf` - Linux での強制削除
- `sudo rm` - 管理者権限での削除
- `del /f /q` - Windows での強制削除
- `format` - ディスクフォーマット

### 機密情報漏洩スキャン
```json
"secrets_scan": {
  "command": "gitleaks detect --source . || truffleHog . || true",
  "description": "機密情報の漏洩スキャン"
}
```

GitLeaks や TruffleHog を使用して、Git 履歴に含まれる機密情報を検出します。

## 条件付き実行システム

### 実行条件の種類

#### ファイル存在チェック
```json
"file_exists": ["package.json", "go.mod", "astro.config.mjs"]
```
指定されたファイルが存在する場合のみ実行されます。

#### ファイルパターンマッチング
```json
"file_patterns": ["*.js", "*.ts", "*.go", "*.astro"]
```
指定されたパターンにマッチするファイルが存在する場合のみ実行されます。

#### コマンド利用可能性チェック
```json
"command_available": ["npm", "go", "npx"]
```
指定されたコマンドがシステムで利用可能な場合のみ実行されます。

#### スクリプト存在チェック
```json
"script_exists": ["test", "build", "dev"]
```
`package.json` 内に指定されたスクリプトが存在する場合のみ実行されます。

### プロジェクト自動検出

システムは以下の指標でプロジェクトタイプを自動検出し、適切なフックを有効化します：

#### Node.js プロジェクト
- **npm**: `package.json` + `package-lock.json`
- **pnpm**: `package.json` + `pnpm-lock.yaml`
- **yarn**: `package.json` + `yarn.lock`

#### TypeScript プロジェクト
- **指標**: `tsconfig.json`, `*.ts`, `*.tsx`
- **有効フック**: TypeScript チェック, Biome

#### React プロジェクト
- **指標**: `*.jsx`, `*.tsx`, package.json 内の react 依存関係
- **有効フック**: Biome, TypeScript チェック

#### Go プロジェクト
- **指標**: `go.mod`, `*.go`
- **有効フック**: Go フォーマット, セキュリティスキャン, テスト

#### Astro プロジェクト
- **指標**: `astro.config.*`, `*.astro`
- **有効フック**: Astro チェック, ビルドテスト

#### Google Apps Script
- **指標**: `.clasp.json`, `appsscript.json`
- **有効フック**: clasp 検証, デプロイチェック

## エラーハンドリング

### 失敗時の動作制御

#### 致命的エラー (`fail_on_error: true`)
以下のチェックで失敗した場合、処理を停止します：
- セキュリティスキャン
- 危険コマンドチェック
- 脆弱性スキャン
- テスト実行

#### 警告レベル (`fail_on_error: false`)
以下のチェックで失敗しても処理を継続します：
- コードフォーマット
- リンティング
- パフォーマンス監査

### タイムアウト設定
各コマンドには適切なタイムアウトが設定されており、無限ループや長時間実行を防止します：

- **軽量チェック**: 30-120秒
- **テスト実行**: 600秒
- **ビルド処理**: 300-600秒
- **依存関係インストール**: 300秒

### 再試行機能
```json
"retry_on_failure": {
  "enabled": true,
  "max_attempts": 2,
  "delay_seconds": 5
}
```

ネットワーク障害などの一時的な問題に対して自動的に再試行を行います。

## 環境設定

### 開発環境向け設定
```json
"development": {
  "skip_heavy_checks": true,
  "reduced_timeouts": true,
  "warning_only_mode": true
}
```

開発効率を優先し、軽量なチェックのみを実行します。

### 本番環境向け設定
```json
"production": {
  "strict_mode": true,
  "comprehensive_auditing": true,
  "zero_tolerance_policy": true
}
```

品質を最優先とし、すべてのチェックを厳格に実行します。

## 導入手順

### 1. 設定ファイルの配置
フック設定を適切な設定ファイルに配置します：

- **ユーザーレベル**: `~/.claude/settings.json`
- **プロジェクトレベル**: `.claude/settings.json`
- **ローカル設定**: `.claude/settings.local.json`

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

#### HTML/CSS
```bash
npm install -D html-validate stylelint
```

### 3. 段階的導入

#### 第1段階: 基本セキュリティ
- 機密情報スキャン
- 危険コマンドチェック

#### 第2段階: 言語固有チェック
- 使用言語に応じたリンティング
- 基本的なテスト実行

#### 第3段階: 包括的品質管理
- セキュリティ監査
- パフォーマンステスト
- デプロイ前検証

## トラブルシューティング

### よくある問題

#### コマンドが見つからない
**症状**: `command not found` エラー
**解決策**:
1. 必要なツールがインストールされているか確認
2. PATH環境変数が正しく設定されているか確認
3. `command_available` 条件を調整

#### タイムアウトエラー
**症状**: コマンドがタイムアウトで失敗
**解決策**:
1. `timeout` 値を増加
2. 処理対象ファイルを減らす
3. パフォーマンスの良いマシンで実行

#### 権限エラー
**症状**: ファイルアクセス権限エラー
**解決策**:
1. ファイル権限を確認・修正
2. 実行ユーザーを変更
3. `sudo` の使用（セキュリティに注意）

### ログとデバッグ

#### 詳細ログの有効化
```json
"log_level": "debug",
"log_skipped_commands": true,
"show_skip_reason": true
```

#### 実行状況の確認
各フックの実行状況は以下で確認できます：
- 実行されたコマンド
- スキップされたコマンドとその理由
- エラーメッセージと解決策

## ベストプラクティス

### 設定のカスタマイズ
1. **プロジェクトに応じた調整**: 使用していない言語のフックは無効化
2. **チーム標準の統一**: 共通の設定ファイルを共有
3. **段階的な厳格化**: 開発フェーズに応じて設定を調整

### パフォーマンス最適化
1. **並列実行の活用**: 関連性のないチェックは並列で実行
2. **条件の最適化**: 不要なチェックを避けるための適切な条件設定
3. **キャッシュの活用**: 条件評価結果のキャッシュ機能を有効化

### セキュリティ強化
1. **定期的な更新**: セキュリティツールの定期更新
2. **新しい脅威への対応**: 設定の定期見直し
3. **チーム教育**: セキュリティ意識の向上

## セキュリティ考慮事項

### 重要な注意点
- フックは任意のシステムコマンドを実行できるため、設定するコマンドは慎重に検証してください
- 信頼できないソースからのフック設定はセキュリティリスクとなります
- フックの実行はClaude Codeの動作を大きく変更する可能性があります

### セキュリティベストプラクティス
- フックコマンドは最小権限で実行する
- 外部スクリプトの実行時は内容を事前に確認
- 機密情報を含む環境でのフック使用は特に注意

## ベストプラクティス

### 設定のカスタマイズ
1. **プロジェクトに応じた調整**: 使用していない言語のフックは無効化
2. **チーム標準の統一**: 共通の設定ファイルを共有
3. **段階的な厳格化**: 開発フェーズに応じて設定を調整
4. **フック設定のバージョン管理**: プロジェクト設定はGitで管理
5. **設定の継承理解**: ユーザー設定とプロジェクト設定の優先順位を把握

この設定により、指定された技術スタックでの安全で効率的な開発ワークフローが実現できます。
