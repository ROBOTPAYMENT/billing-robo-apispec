# Webhookクレジットカード登録状況

`Webhook URLはWebhook管理画面から設定が可能です。`

クレジットカードの登録状況をWebhookとして通知します。

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

| 名前                                 | 種別   | 値/説明                                      |
|------------------------------------- | ------ | --------------------------------------------- |
| BillingRoboSignaturekey              | 文字列 | ロボで自動生成する <br> 受け取り側で確かなものであると判定する |
| org                                  | 文字列 | BillingRobo_RobotPayment                     |
| event_name                           | 文字列 | ウェブフックイベントの名称 <br> credit_status_issue |
| regist_time                          | 文字列 | イベント発生時刻                             |
| notification_time                    | 文字列 | イベント通知時刻                             |
| billing_source_id                    | 数値   | 請求元ID                                     |
| [event_detail](#event_detail-request)                         | 配列   | イベント詳細                                 |


#### event_detail (request)

共通リクエストパラメータのevent_detailの中身

| 名前                  | 種別 |  値/説明                                      |
| --------------------- | ---- | --------------------------------------------- |
| billing_id            | 数値 | 請求先ID                                      |
| billing_code          | 文字列 | 請求先コード                                  |
| payment_number        | 数値 | 決済情報番号 <br> 決済情報一覧か請求先詳細ページで確認できる |
| payment_code          | 文字列 | 決済情報コード                                |
| payment_status        | 数値 | 決済情報登録状況 <br> 0:未登録,1:登録待ち,2:メール送信済,3:申請中,4:登録情報送信エラー,5:登録完了,6:登録失敗 |
| credit_status         | 数値 | クレジットカード登録状況 <br> 0:未処理,1:メール送信済,2:完了,3:エラー |
| credit_error_code     | 文字列 | 失敗時のエラーコード |
| credit_update_time    | 文字列 | credit情報更新時の時刻 |

### サンプル
```json
{
  "BillingRoboSignaturekey": "$2y$10$RFFenSWN0U8N9Ik1xf0Nj.pQJd/mSasB9ucofm1qTdYeTc8Ag9OLC",
  "org": "BillingRobo_RobotPayment",
  "id": "17",
  "event_name": "credit_status_issue",
  "regist_time": "2019-11-28 04:35:15",
  "notification_time": "2019-11-28 01:35:40",
  "billing_source_id": "1",
  "event_detail": {
    "billing_id": "5",
    "billing_code": "izuxab",
    "payment_number": "3",
    "payment_code": "bank123",
    "payment_status": "1",
    "credit_status": "",
    "credit_error_code": null,
    "credit_update_time": "2019-11-28 01:35:15"
  }
}
```