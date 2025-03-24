# RSS feed通知アプリケーション要件

## 技術スタック
- バックエンド: Hono (TypeScript)
- インフラ: Cloudflare Workers + R2
- 定期実行: Cloudflare Cron Triggers

## 機能要件

### 1. RSS feed取得機能
- 複数のRSS feedを定期的に取得
- 取得頻度: 1日1回
- 取得した記事の重複チェック
  - 前回実行から今回実行までの間で更新された記事を対象
  - 重複する場合は通知対象外

### 2. 記事情報管理機能
- 取得した記事の情報をR2に保存
- 保存する情報
  - 記事タイトル
  - 記事URL
  - 公開日時

### 3. Slack通知機能
- 新規記事の情報をSlackに通知
- 通知フォーマット
  ```
  🔔 新着記事のお知らせ

  [タイトル]
  [URL]
  [公開日時]
  ---
  ```

## 非機能要件

### 1. パフォーマンス
- RSS feed取得のタイムアウト: 30秒
- 1回の実行で処理する feed数: 最大10件

### 2. エラー処理
- RSS feed取得失敗時のリトライ: 最大3回
- Slack通知失敗時のリトライ: 最大3回
- エラーログの保存

### 3. セキュリティ
- Slack Webhook URLの安全な管理
- RSS feedURLの検証

## 設定項目
### 環境変数
- `SLACK_WEBHOOK_URL`: Slack通知用のWebhook URL

### 設定ファイル
- `feeds.yaml`: RSS feedのURLリスト
  ```yaml
  feeds:
    - url: https://example1.com/feed
      name: Example Blog 1
    - url: https://example2.com/feed
      name: Example Blog 2
  ```

## 今後の拡張性
- feed元の追加・削除機能
- 通知フォーマットのカスタマイズ機能
- 通知頻度の調整機能
- 記事のフィルタリング機能
