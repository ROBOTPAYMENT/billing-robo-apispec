# Webhookクレジットカード登録状況

`Webhook URLはWebhook管理画面から設定が可能です。`

クレジットカードの登録状況が変更された際に、そのステータスをWebhookとして通知します。

## アウトライン

- [リクエスト](#リクエスト)
  - [共通リクエストパラメータ](#共通リクエストパラメータ)
    - [イベントデーター](#イベントデーター)
  - [サンプル](#サンプル)

## リクエスト
- HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### 共通リクエストパラメータ

下記の項目を持つJSONオブジェクトです。

| 名前                                 | 型   | 概要                                      |
|------------------------------------- | ------ | --------------------------------------------- |
| BillingRoboSignaturekey              | string | ロボで自動生成する <br> Webhookページで更新可能 |
| org                                  | string | BillingRobo_RobotPayment                     |
| event_name                           | string | ウェブフックイベントの名称 <br> credit_status_issue |
| regist_time                          | string | イベント発生時刻                             |
| notification_time                    | string | イベント通知時刻                             |
| billing_source_id                    | int   | 請求元ID                                     |
| [event_detail](#event_detail-request)                         | array   | イベント詳細                                 |


#### event_detail (request)

共通リクエストパラメータのevent_detailの中身

| 名前                  | 型 |  概要                                      |
| --------------------- | ---- | --------------------------------------------- |

### サンプル
```json
```
