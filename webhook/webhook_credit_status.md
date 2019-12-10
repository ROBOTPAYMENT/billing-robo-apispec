# Webhookクレジットカード登録状況

`Webhook URLはWebhook管理画面から設定が可能です。`

クレジットカードの登録状況が変更された際に、そのステータスをWebhookとして通知します。

## アウトライン

- [リクエスト](#リクエスト)
  - [共通リクエストパラメータ](#共通リクエストパラメータ)
    - [event_detail (request)](#event_detail-request)
  - [サンプル](#サンプル)
- [webhookされるイベント一覧](#webhookされるイベント一覧)
  - [請求管理ロボ画面](#請求管理ロボ画面)
  - [請求管理ロボAPI](#請求管理ロボAPI)

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
| billing_id            | int | 請求先ID                                      |
| billing_code          | string | 請求先コード                                  |
| payment_number        | int | 決済情報番号 <br> 決済情報一覧か請求先詳細ページで確認できる |
| payment_code          | string | 決済情報コード                                |
| payment_status        | int | 決済情報登録状況 <br> 0:未登録 1:登録待ち 2:メール送信済 3:申請中 4:登録情報送信エラー 5:登録完了 6:登録失敗 |
| credit_status         | int | クレジットカード登録状況 <br> 0:未処理 1:メール送信済 2:完了 3:エラー |
| credit_error_code     | string | 失敗時のエラーコード |
| credit_update_time    | string | credit情報更新時の時刻 |

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

## webhookされるイベント一覧

### 請求管理ロボ画面

| 画面名                            | URL                                           | 条件                                                          |
| --------------------------------- | --------------------------------------------- | ------------------------------------------------------------- |
| 請求先登録                        | /billing/add                                  | デフォルト決済手段を「クレジットカード」を選択して登録        |
| 決済情報追加                      | /billing_payment_method/add/{請求先ID}        | 決済手段を「クレジットカード」にて選択して登録                |
| 請求先承認                        | /approve_billing/task                         | デフォルト決済手段を「クレジットカード」を選択して登録        |
| 請求先インポート                  | /billing/import                               | 決済手段を「クレジットカード」にて選択して登録                |
| クレジットカード情報入力ポップアップ<br>（決済連携側）| https://credit.j-payment.co.jp/gateway/form/type1/ja/payform.aspx | ・請求先登録<br>・決済情報登録<br>・決済情報詳細内→カード情報更新ボタン押下時<br>などに表示されるクレジットカード情報入力ポップアップ→有効性チェックリターン取得時<br> 「カード情報メール送信」等はステータス変更が行われないので、webhook送信しない |

### 請求管理ロボAPI

| API名                                         | URL                                                   | 条件                                              |
| --------------------------------------------- | ----------------------------------------------------- | ------------------------------------------------- |
| 請求先登録更新(複数) v1.0/billing/bulk_upsert | /api/v1.0/billing/bulk_upsert                         | 決済手段を「クレジットカード」にて決済情報を登録  |
| 請求先登録 billing/register                   | /api/billing/register                                 | 決済手段を「クレジットカード」にて決済情報を登録  |
| 請求先登録 billing/add                        | /api/billing/add                                      | 決済手段を「クレジットカード」にて決済情報を登録  |
| 請求先部署登録 billing_individual/add         | /api/billing_individual/add                           | 決済手段を「クレジットカード」にて決済情報を登録  |
| 請求先部署編集 billing_individual/edit        | /api/billing_individual/edit                          | 決済手段を「クレジットカード」にて決済情報を登録  |
| 請求先登録更新(複数) billing/bulk_upsert      | /api/billing/bulk_upsert                              | 決済手段を「クレジットカード」にて決済情報を登録  |
| クレジットカード登録(トークン方式)            | /api/v1.0/billing_payment_method/credit_card_token    | 決済手段を「クレジットカード」にて決済情報を登録  |

※以下廃止予定のAPIのクレジットカード有効性チェックリターン時のwebhook送信は対象外
- 請求先登録：api/billing/register
- 請求先部署編集：api/billing_individual/edit
- 請求先登録更新(複数)：api/bulk_upsert
