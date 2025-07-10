# Go プロジェクト総合チェック

Go プロジェクトの品質とセキュリティを総合的にチェックしてください。

## 実行項目

1. **フォーマットとスタイル**
   ```bash
   go fmt ./...
   go vet ./...
   ```

2. **静的解析**
   ```bash
   staticcheck ./...
   golint ./...
   ```

3. **セキュリティスキャン**
   ```bash
   gosec ./...
   ```

4. **脆弱性チェック**
   ```bash
   govulncheck ./...
   ```

5. **依存関係管理**
   ```bash
   go mod tidy
   go mod verify
   ```

6. **テスト実行**
   ```bash
   go test -v -race ./...
   ```

## 品質指標
- テストカバレッジ（80%以上推奨）
- Cyclomatic Complexity
- セキュリティ問題の有無

## 改善提案
発見された問題に対する具体的な修正方法を提示してください。
