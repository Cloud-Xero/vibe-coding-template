# パフォーマンス監査

アプリケーションのパフォーマンスを包括的に分析してください。

## 分析項目

1. **バンドルサイズ分析**
   ```bash
   # Webpack Bundle Analyzer
   npx webpack-bundle-analyzer dist/static/js/*.js

   # Vite Bundle Analyzer
   npx vite-bundle-analyzer
   ```

2. **ロード時間測定**
   - First Contentful Paint (FCP)
   - Largest Contentful Paint (LCP)
   - Cumulative Layout Shift (CLS)

3. **Lighthouse 監査**
   ```bash
   npx lighthouse http://localhost:3000 --output=json
   ```

4. **依存関係の最適化**
   - 重複ライブラリの検出
   - Tree shaking の効果測定
   - 動的インポートの機会

## 改善提案
- コード分割の推奨箇所
- 画像最適化の提案
- キャッシュ戦略の改善
- 不要な依存関係の削除

スコアが90点未満の項目については具体的な改善策を提示してください。
