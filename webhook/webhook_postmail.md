# Webhook郵送通知

`Webhook URLはWebhook管理画面から設定が可能です。`

郵送処理する際、そのステータスをWebhookとして通知します。

## アウトライン

- [リクエスト](#リクエスト)
  - [共通リクエストパラメータ](#共通リクエストパラメータ)
    - [event_detail (request)](#event_detail-request)
  - [サンプル](#サンプル)

## リクエスト
- HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### 共通リクエストパラメータ

下記の項目を持つJSONオブジェクトです。

| 名前                                  | 型        | 概要                                              |
|-------------------------------------- | --------- | ------------------------------------------------- |
| BillingRoboSignaturekey               | string    | ロボで自動生成する <br> Webhookページで更新可能   |
| org                                   | string    | BillingRobo_RobotPayment                          |
| event_name                            | string    | ウェブフックイベントの名称 <br> bill_issue        |
| regist_time                           | string    | イベント発生時刻                                  |
| notification_time                     | string    | イベント通知時刻                                  |
| billing_source_id                     | int       | 請求元ID                                          |
| [event_detail](#event_detail-request) | array     | イベント詳細                                      |


#### event_detail (request)

共通リクエストパラメータのevent_detailの中身

| 名前                  | 型        |  概要                                             |
| --------------------- | --------- | ------------------------------------------------- |
| type                  | string    | 郵送タイプ <br> 郵送発注：order、郵送発注失敗：order_fail、郵送発送済み：sent、郵送失敗：fail、郵送完了：complete、郵送取消：cancel、郵送差止：injunction |
| [mail](#mail-request) | array     | 詳細内容                                          |

##### mail (request)
event_detailパラメーターのmailの中身

| 名前                  | 型        |  概要                                         |
| --------------------- | --------- | --------------------------------------------- |
| number                | int       | 郵送コード                                    |
| bill_number           | string    | 請求書番号                                    |
| status                | int       | 郵送ステータス<br> 0:受付中,1:発送済,2:完了,3:失敗,4:取り消し,5:差止 |
| error_type            | int       | 郵送エラータイプ<br> 0:その他エラー,1:枚数超過,2:ロゴファイルエラー |
| order_date            | string    | 郵送注文日                                    |
| mail_date             | string    | 郵送発送日                                    |
| nondelivery_flg       | int       | 郵送不達フラグ                                |
| nondelivery_date      | string    | 郵送不達処理日                                |
| update_date           | string    | 更新日時                                      |

### サンプル
```json
{
  "BillingRoboSignaturekey": "$2y$10$ee1ZuZ6XSxEkzq9egVTLL.OlU7wb/WCgY0ORQyCZpfiDnhoPH2rXu",
  "org": "BillingRobo_RobotPayment",
  "id": "26",
  "event_name": "credit_status_issue",
  "regist_time": "2019-12-06 08:31:33",
  "notification_time": "2019-12-06 06:39:50",
  "billing_source_id": "1",
  "event_detail": {
    "type": "complete",
    "mail": {
      "number": "11",
      "bill_number": "201811-SELF-18",
      "status": "2",
      "order_date": "2018-11-16",
      "mail_date": "2018-11-16",
      "update_date": "2018-12-12 16:58:40"
    }
  }
}
```
