# プルリクエスト作成

現在の変更内容を分析して、包括的なプルリクエストを作成してください。

## PR作成手順

1. **変更内容の分析**
   ```bash
   git diff --name-only
   git diff --cached --name-only
   git log --oneline -10
   ```

2. **変更されたファイルの詳細確認**
   - 変更の種類（機能追加、バグ修正、リファクタリング）
   - 影響範囲の特定
   - 破壊的変更の有無

3. **PR情報の生成**
   - **タイトル**: 簡潔で説明的なタイトル
   - **概要**: 変更の目的と背景
   - **変更詳細**: 主要な変更点のリスト
   - **テスト**: 追加・変更されたテストの説明
   - **影響**: パフォーマンスやAPIへの影響
   - **チェックリスト**: レビュアー向けの確認事項

## PR作成前チェック

### コード品質
- [ ] Linting エラーなし
- [ ] 型チェック完了
- [ ] テストが通過
- [ ] コミットメッセージが適切

### セキュリティ
- [ ] 機密情報の漏洩なし
- [ ] 新しい依存関係の安全性確認
- [ ] 権限設定の適切性

### ドキュメント
- [ ] README の更新（必要に応じて）
- [ ] API ドキュメントの更新
- [ ] CHANGELOG の追記

## 出力形式

以下の形式でPR情報を生成してください：

```
## 概要
[変更の目的と背景を簡潔に説明]

## 変更内容
- [主要な変更点1]
- [主要な変更点2]
- [主要な変更点3]

## 技術的詳細
[実装の詳細や技術的な決定事項]

## テスト
- [追加されたテスト]
- [変更されたテスト]
- [テストカバレッジの状況]

## スクリーンショット（該当する場合）
[UI変更がある場合はスクリーンショットを追加]

## 影響範囲
- **破壊的変更**: [あり/なし]
- **パフォーマンス**: [影響の説明]
- **セキュリティ**: [考慮事項]

## レビューポイント
- [ ] [重点的にレビューしてほしい箇所1]
- [ ] [重点的にレビューしてほしい箇所2]
- [ ] [特に注意が必要な変更]

## 関連Issue
Closes #[issue番号]
```

## 最終確認
- Git コミットの状態確認
- ブランチ名の適切性
- リモートブランチとの同期状況
