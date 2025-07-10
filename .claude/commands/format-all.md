# プロジェクト全体フォーマット

プロジェクト内のすべてのファイルを適切な形式でフォーマットしてください。

## フォーマット対象

### JavaScript/TypeScript
```bash
npx biome format --write .
```

### Go
```bash
go fmt ./...
```

### HTML
```bash
npx prettier --write "**/*.html"
```

### CSS
```bash
npx prettier --write "**/*.css"
```

### JSON
```bash
npx biome format --write "**/*.json"
```

## 除外対象
- `node_modules/`
- `dist/`, `build/`
- `*.min.js`, `*.min.css`
- バイナリファイル

## 実行後の確認
- Git diff での変更内容確認
- フォーマットエラーの有無
- ビルドの正常性確認
