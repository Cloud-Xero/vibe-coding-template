# 使用量表示

Claude API使用量とコストをリアルタイムで表示します。

## 実行コマンド

```bash
npx ccusage@latest
```

## 出力形式

実行結果はccusageツールの標準出力と同じ形式で表示されます：

- 日別のトークン使用量統計テーブル
- Input/Output/Cache トークン数
- 累計コストとトークン数
- 使用モデル情報

## 説明

[ccusage](https://github.com/ryoppippi/ccusage)を使用してClaude APIの使用量とコストを表示します。

## 使用方法

`/usage` と入力することで、現在のClaude API使用量とコストがターミナルに表示されます。

## 表示内容

- API使用量の統計
- 累計コスト
- リクエスト数
- トークン使用量