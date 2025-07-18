# 機密情報漏洩チェック

コードベース内の機密情報のハードコーディングをチェックしてください。

## チェック対象パターン
- `password = "..."`
- `api_key = "..."`
- `secret = "..."`
- `token = "..."`
- `private_key = "..."`
- データベース接続文字列
- OAuth シークレット

## 実行方法
```bash
grep -r -n -E '(password|api_key|secret|token|private_key)\s*=\s*["'"'"'][^"'"'"']+["'"'"']' .
```

## 除外対象
- テストファイル（`*.test.*`, `*.spec.*`）
- 設定例ファイル（`.env.example`）
- ドキュメント内の例

問題が発見された場合は、環境変数の使用を推奨してください。
