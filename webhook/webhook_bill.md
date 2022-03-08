# Webhook請求書発行イベント

`Webhook URLはWebhook管理画面から設定が可能です。`

請求書発行する際、その情報をWebhookとして通知します。
(手動請求書発行、自動請求書発行、請求書再発行)

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

| 名前                                      | 型        |  概要                                         |
| ----------------------------------------- | --------- | --------------------------------------------- |
| billing_number                            | string    | 請求書番号                                    |
| type                                      | int       | 請求書タイプ<br> 1:請求 <br> 2:繰越請求 <br> 3:親請求書 <br> 4:子請求書 |
| bill_issue_date                           | string    | 請求書発行日<br>                              |
| make_date                                 | string    | 請求書作成日                                  |
| billing_individual_number                 | int       | 請求先部署番号                                |
| billing_method                            | int       | 請求方法<br> 0:送付なし <br> 1:自動メール <br> 2:手動メール <br> 3:自動郵送 <br> 4:手動郵送 <br> 5:自動メール＋手動郵送 <br> 6:手動メール+手動郵送 |
| bill_sending_scheduled_date               | string    | 請求送付予定日                                |
| payment_method                            | int       | [決済手段](../../index.md#決済手段) | 
| demand_number                             | int       | 請求件数                                      |
| subtotal_amount_billed                    | int       | 請求金額小計                                  |
| consumption_tax_amount                    | int       | 消費税額                                      |
| total_bill_detail_consumption_tax_amount  | int       | 明細毎消費税合計                              |
| withholding_tax_amount                    | int       | 源泉所得税額                                  |
| total_amount_billed                       | int       | 請求金額合計                                  |

### サンプル
```json
{
  "BillingRoboSignaturekey": "$2y$10$ee1ZuZ6XSxEkzq9egVTLL.OlU7wb/WCgY0ORQyCZpfiDnhoPH2rXu",
  "org": "BillingRobo_RobotPayment",
  "id": "25",
  "event_name": "bill_issue",
  "regist_time": "2019-12-06 08:09:26",
  "notification_time": "2019-12-06 05:09:32",
  "billing_source_id": "1",
  "event_detail": {
    "bill": {
      "billing_number": "201912-sample-3",
      "type": 1,
      "bill_issue_date": "2019-12-01",
      "make_date": "2019/12/06",
      "billing_individual_number": "1",
      "billing_method": "0",
      "bill_sending_scheduled_date": "2019-12-25",
      "payment_method": "0",
      "demand_number": 1,
      "subtotal_amount_billed": 10000,
      "consumption_tax_amount": 800,
      "total_bill_detail_consumption_tax_amount": 800,
      "withholding_tax_amount": 0,
      "total_amount_billed": 10800
    }
  }
}
```
[TOPへ戻る](../index.md)