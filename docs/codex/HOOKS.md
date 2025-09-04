## Codex CLI ワークフロー（Hooks 相当の設計）

Claude の Hooks のようなイベント駆動の実行ポイントは Codex CLI にはありません。ただし、以下の運用ルールにより同等以上の再現性と安全性を実現できます。

### 実行段階

- **Pre-Change（変更前）**: 読み取り・影響範囲の特定・軽量チェック
- **Apply（適用）**: `apply_patch` による最小差分の適用
- **Post-Change（変更後）**: テスト/ビルド/リンティング等の検証
- **Report（報告）**: 失敗/成功と次のアクションを短く共有

### 条件付き実行（目安）

- JS/TS/React: `biome check --apply .` または既存リンターで範囲限定チェック
- TypeScript: `tsc --noEmit`（`tsconfig.json` がある場合）
- Go: `go fmt ./...`, `go vet ./...`（`go.mod` がある場合）
- Astro: `npx astro check`（`astro.config.*` がある場合）
- GAS: `npx @google/clasp push --dry-run`（`.clasp.json` がある場合）
- HTML/CSS: `npx html-validate`, `stylelint`（設定がある場合）

必要なツールがない場合は導入手順の提案のみ行い、無理に実行しない方針を推奨します。

### セキュリティチェック（推奨）

- 危険コマンド検出: `grep -r -n -E '(rm\s+-rf|sudo\s+rm|del\s+/f|format\s+[A-Z]:)' .`
- 機密情報検出: `grep -r -n -E '(password|api_key|secret|token)\s*=\s*["\'][^"\']+["\']' .`
- Git 履歴スキャン: `gitleaks` / `truffleHog` があれば活用（なければ提案のみ）

### タイムアウトと失敗方針

- 迅速チェック: 30–120 秒
- ビルド/テスト: 300–600 秒
- 依存解決: 300 秒（ネットワーク制限に注意）

失敗を致命（停止）か警告（継続）に分離し、段階的に厳格化するのが実務的です。

### 実装上の原則

- 変更はパッチで明示／最小化（巻き込み修正を避ける）
- 既存スタイル・命名・構成を尊重
- テストは変更範囲から外へ順に広げる

### 例: JS/TS プロジェクト

1) 変更ファイルのみ `biome check --apply` または ESLint/Prettier を適用
2) 型が関与する場合のみ `tsc --noEmit`
3) 重要画面のスモークテスト（スクリプトがある場合）

### 例: Go プロジェクト

1) `go fmt ./...` → `go vet ./...`
2) ユニットテストがあれば `go test ./... -run <対象>` など範囲限定

### まとめ

Codex では「計画→最小差分→限定検証→進捗共有」という運用で、Claude Hooks と同等の安全性と再現性を実現します。プロジェクトに即したチェックセットを決め、段階的に厳格化していくのが最も効果的です。

