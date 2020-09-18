# 繰越予約

`/api/v1.0/bill/update_carryover`

請求書の繰越予約および繰越予約取消を実行します。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/v1.0/bill/update_carryover`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                  | 概要                                 | 桁数 | 種別                              | 必須 |
| --------------------- | ------------------------------------ | ---- | --------------------------------- | ---- |
| user_id               | ユーザーID（管理画面へのログインID） | 100  | [メール形式](../../index.md#種別) | 必須 |
| access_key            | アクセスキー                         | 100  | [半角英数](../../index.md#種別)   | 必須 |
| [bill](#bill-request) | 請求書に属するパラメータ             |      | `array`                     |      |

#### bill (request)

下記のような項目のオブジェクトを持つリスト

| 名前                      | 概要                                                                                                                                                                               | 桁数 | 種別                                   | 必須                                                         |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---- | -------------------------------------- | ------------------------------------------------------------ |
| number                    | 請求書番号                                                                                                                                                                         | 100  | [半角英数 + 記号](../../index.md#種別) |                                                              |
| billing_code              | 請求先コード                                                                                                                                                                       | 20   | [半角英数 + 記号](../../index.md#種別) | (billing_individual_number or billing_individual_code入力時) |
| billing_individual_number | 請求先部署番号                                                                                                                                                                     | 20   | 数値                                   |                                                              |
| billing_individual_code   | 請求先部署コード                                                                                                                                                                   | 20   | [半角英数 + 記号](../../index.md#種別) |                                                              |
| bs_owner_code             | 請求元担当者コード <br> ※両端のスペース除去                                                                                                                                        | 20   | [半角英数 + 記号](../../index.md#種別) |                                                              |
| payment_method            | 決済手段 <br> 0:銀行振込1:クレジットカード2:バンクチェック <br> 3:RP口座振替4:RL口座振替5:その他口座振替 <br> 6:コンビニ払込票(A4) 7:コンビニ払込票(ハガキ) 8:その他コンビニ払込票 | 2    | 数値                                   |                                                              |
| payment_status            | 消込ステータス <br> 0:未処理 3:未収                                                                                                                                                | 2    | 数値                                   |                                                              |
| issue_start_date          | 発行日の検索開始日                                                                                                                                                                 | 10   | 日付                                   |                                                              |
| issue_stop_date           | 発行日の検索終了日                                                                                                                                                                 | 10   | 日付                                   |                                                              |
| deadline_start_date       | 決済期限の検索開始日 <br> ※繰越予約は決済期限が過ぎている請求書が対象です                                                                                                          | 10   | 日付                                   |                                                              |
| deadline_stop_date        | 決済期限の検索終了日 <br> ※繰越予約は決済期限が過ぎている請求書が対象です                                                                                                          | 10   | 日付                                   |                                                              |
| carryover_flg             | 繰越予約フラグ <br> 1:繰越予約 2:予約取消  <br> ※繰越予約指定の場合は、繰越ステータスが0:対象外を対象とする <br> ※繰越予約取消指定の場合は、繰越ステータスが1:繰越待ちを対象とする | 2    | 数値                                   | 必須                                                         |



## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                   | 概要                                | 型            |
| ---------------------- | ----------------------------------- | ------------- |
| user_id                | ユーザーID                          | string        |
| access_key             | アクセスキー                        | string        |
| error_code             | エラーコード <br> ※正常時はnull     | string        |
| error_message          | エラーメッセージ <br> ※正常時はnull | int           |
| [bill](#bill-response) | 請求書に属するパラメータ            | `array` |

#### bill (response)

下記のような項目のオブジェクトを持つリスト

| 名前                          | 概要                                                                                                                                                                                   | 型     |
| ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| number                        | 請求書番号                                                                                                                                                                             | string |
| bs_residence_code             | 請求元差出人コード                                                                                                                                                                     | string |
| billing_code                  | 請求先コード                                                                                                                                                                           | string |
| billing_name                  | 請求先名                                                                                                                                                                               | string |
| billing_individual_number     | 請求先部署番号                                                                                                                                                                         | string |
| billing_individual_code       | 請求先部署コード                                                                                                                                                                       | string |
| billing_individual_name       | 請求先部署名                                                                                                                                                                           | string |
| demand_number                 | 請求件数                                                                                                                                                                               | string |
| subtotal_amount_billed        | 請求金額小計                                                                                                                                                                           | int    |
| consumption_tax_amount        | 消費税額                                                                                                                                                                               | int    |
| total_amount_billed           | 請求金額合計                                                                                                                                                                           | int    |
| unclearing_amount             | 未消込金額                                                                                                                                                                             | int    |
| message_column                | 通信欄                                                                                                                                                                                 | string |
| billing_method                | 請求方法 <br> 0：送付なし、1：自動メール、2：手動メール、3：自動郵送、4：手動郵送、5：自動メール+自動郵送、6：手動メール+手動郵送                                                      | int    |
| issue_date                    | 請求書発行日                                                                                                                                                                           | string |
| make_date                     | 請求書作成日                                                                                                                                                                           | string |
| sending_scheduled_date        | 請求書送付予定日                                                                                                                                                                       | string |
| sending_date                  | 請求書送付日                                                                                                                                                                           | string |
| update_date                   | 請求書更新日                                                                                                                                                                           | string |
| confirm_date                  | 請求書確認日                                                                                                                                                                           | string |
| mail_send_flg                 | メールステータス <br> 0：未送信 1：送信済み                                                                                                                                            | int    |
| post_send_flg                 | 郵送ステータス <br> 0：未送信 1：送信済み                                                                                                                                              | int    |
| payment_method                | 決済手段 <br> 0:銀行振込 1:クレジットカード 2:バンクチェック <br> 3:RP口座振替 4:RL口座振替 5:その他口座振替 <br> 6:コンビニ払込票(A4) 7:コンビニ払込票(ハガキ) 8:その他コンビニ払込票 | int    |
| payment_method_number         | 決済情報番号                                                                                                                                                                           | string |
| payment_method_code           | 決済情報コード                                                                                                                                                                         | string |
| payment_method_name           | 決済情報名                                                                                                                                                                             | string |
| settlement_result             | 決済連携ステータス <br> 0：申請中、1：送信失敗、2：決済成功、3：決済失敗                                                                                                               | int    |
| transfer_deadline             | 決済期限                                                                                                                                                                               | string |
| slip_deadline                 | 払込票有効期限                                                                                                                                                                         | string |
| transfer_date                 | 決済日                                                                                                                                                                                 | string |
| payment_status                | 消込ステータス <br> 0:未処理 1:完了 2:確認済み 3:未収 4:貸倒 5:手数料 6:現金 7:長期滞留債権 8:破産更生債権 9:売上取消 10:繰越                                                          | int    |
| memo                          | メモ                                                                                                                                                                                   | string |
| template_code                 | 請求書テンプレート                                                                                                                                                                     | string |
| bs_department_code            | 請求元部署コード                                                                                                                                                                       | string |
| bs_department_name            | 請求元部署名                                                                                                                                                                           | string |
| bs_owner_code                 | 請求元担当者コード                                                                                                                                                                     | string |
| bs_owner_name                 | 請求元担当者名                                                                                                                                                                         | string |
| valid_flg                     | 請求書有効フラグ <br> 0:無効1:有効                                                                                                                                                     | int    |
| delete_flg                    | 請求書削除フラグ <br> 0:未削除 1:削除 <br> ※現行削除機能は未実装のため未削除が返却されます。                                                                                           | int    |
| gid                           | 決済ID                                                                                                                                                                                 | int    |
| carryover_status              | 繰越ステータス <br> 0:対象外 1:繰越待ち                                                                                                                                                | int    |
| carryover_transit_bill_number | 繰越先請求書番号                                                                                                                                                                       | string |
| carryover_total_amount        | 繰越金額                                                                                                                                                                               | int    |



## 使用例

### リクエスト例

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "bill": [
        {
            "number": "",
            "billing_code": "",
            "billing_individual_number": "",
            "billing_individual_code": "",
            "bs_owner_code": "",
            "payment_method": "0",
            "issue_start_date": "2016/04/01",
            "issue_stop_date": "2017/07/31",
            "deadline_start_date": "",
            "deadline_stop_date": "",
            "carryover_flg": "1"
        }
    ]
}
```

### レスポンス例

Status: 200 OK

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "error_code": null,
    "error_message": null,
    "bill": [
        {
            "error_code": null,
            "error_message": null,
            "number": "201705-banche-1",
            "bs_residence_code": "1",
            "billing_code": "banche",
            "billing_name": "株式001",
            "billing_individual_number": 1,
            "billing_individual_code": "",
            "billing_individual_name": "経理",
            "demand_number": "1",
            "subtotal_amount_billed": 5000,
            "consumption_tax_amount": 0,
            "total_amount_billed": 1080,
            "unclearing_amount": 0,
            "message_column": null,
            "billing_method": 0,
            "issue_date": "2017/03/31",
            "make_date": "2017/05/16",
            "sending_scheduled_date": "2017/04/01",
            "sending_date": null,
            "update_date": "2017/05/16",
            "confirm_date": null,
            "mail_send_flg": 0,
            "post_send_flg": 0,
            "payment_method": 2,
            "payment_method_number": "1",
            "payment_method_code": "",
            "payment_method_name": "バンクチェック",
            "settlement_result": 2,
            "transfer_deadline": "2017/04/30",
            "slip_deadline": "2017/04/30",
            "transfer_date": "2016/07/06",
            "payment_status": 1,
            "memo": null,
            "template_code": "10000",
            "bs_department_code": null,
            "bs_department_name": null,
            "bs_owner_code": null,
            "bs_owner_name": null,
            "valid_flg": 1,
            "delete_flg": 0,
            "gid": "1234567",
            "carryover_status": 1,
            "carryover_transit_bill_number": null,
            "carryover_total_amount": ""
        }
    ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                       |
| ------------ | -------------------------- |
| 2601         | 請求書番号が不正           |
| 2602         | 請求先コードが不正         |
| 2603         | 請求先部署番号が不正       |
| 2604         | 請求先部署コードが不正     |
| 2605         | 請求元担当者が不正         |
| 2606         | 集計用商品コードが不正     |
| 2607         | 決済手段が不正             |
| 2608         | 消込ステータスが不正       |
| 2609         | 発行日の検索開始日が不正   |
| 2610         | 発行日の検索終了日が不正   |
| 2611         | 決済期限の検索開始日が不正 |
| 2612         | 決済期限の検索終了日が不正 |
| 2613         | 繰越予約が不正             |
| 2614         | 繰越ステータス更新に失敗   |
| 2615         | 発行日の指定が不正         |
| 2616         | 決済期限の指定が不正       |
| 2617         | 繰越請求が利用可能でない   |

----

[TOPへ戻る](../../index.md)