# 消込結果参照

`api/v1.0/clearing/search`

消込結果の参照処理を実行します。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/v1.0/clearing/search`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                        | 概要                                                                                | 桁数 | 種別                               | 必須 |
| --------------------------- | ----------------------------------------------------------------------------------- | ---- | --------------------------------- | ---- |
| user_id                     | ユーザーID（管理画面へのログインID）                                                   | 100  | [メール形式](../../index.md#種別)  | 必須 |
| access_key                  | アクセスキー                                                                         | 100  | [半角英数](../../index.md#種別)    | 必須 |
| limit_count                 | 消込情報取得件数 <br> ※0〜200の数値を設定する。省略した場合、20が設定される              | 3    | 数値                              |      |
| page_count                  | 消込情報取得開始インデックス <br> ※0〜99の数値を設定する。省略した場合、0が設定される     | 2    | 数値                              |      |
| [clearing](#clearing-request) | 消込情報に属するパラメータ                                                          |      | `array`                           |      |

#### clearing (request)

下記のような項目のオブジェクトを持つリスト

| 名前                                 | 概要                                                                                                                                                                                                                                                                                                                                                      | 桁数 | 種別            | 必須                |
| ------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---- | --------------- | ------------------- |
| [payment](#payment-request)          | 入金情報に属するパラメータ                                                                                                                                                                                                                                                                                                                                  |      | `array`        |                     |
| [bill](#bill-reqest)                 | 請求書に属するパラメータ                                                                                                                                                                                                                                                                                                                                    |      | `array`        |                     |
| [erasure](#erasure-reqest)           | 消込結果に属するパラメータ                                                                                                                                                                                                                                                                                                                                  |      | `array`        |                     |
| [erasure_bill](#erasure_bill-reqest) | 消込請求書に属するパラメータ                                                                                                                                                                                                                                                                                                                                |      | `array`        |                     |
| clearing_method                      | 消込手段 <br> 0:銀行振込 <br> 1:クレジットカード <br> 2:バンクチェック <br> 3:RP口座振替 <br> 4:RL口座振替 <br> 5:その他口座振替 <br> 6:コンビニ払込票(A4) <br> 7:コンビニ払込票(ハガキ) <br> 8:その他コンビニ払込票 <br> 9:バーチャル口座 <br> 10:その他決済手段1 <br> 11:その他決済手段2 <br> 12:その他決済手段3 <br> 13:その他決済手段4 <br> 14:その他決済手段5 <br> 98:相殺 <br> 101:貸倒 <br> 102:確認済み <br> 103:手数料 <br> 106:現金 <br> 107:長期滞留債権 <br> 108:破産更生等債権 <br> 109:売上取消 <br> 110:繰越                                   | 3     | 数値         |                     |
| clearing_date_from                   | 消込処理日(開始日)                                                                                                                                                                                                                                                                                                                                         | 10  | 日付           |                     |
| clearing_date_to                     | 消込処理日(終了日)                                                                                                                                                                                                                                                                                                                                         | 10  | 日付           |                     |
| erasure_deposit_recorded_date_from   | 消込計上日(開始日)                                                                                                                                                                                                                                                                                                                                         | 10  | 日付            |                    |
| erasure_deposit_recorded_date_to     | 消込計上日(終了日)                                                                                                                                                                                                                                                                                                                                         | 10  | 日付            |                     |
| erasure_cancel_recorded_date_from    | 消込取消計上日(開始日)                                                                                                                                                                                                                                                                                                                                     | 10  | 日付            |                     |
| erasure_cancel_recorded_date_to      | 消込取消計上日(終了日)                                                                                                                                                                                                                                                                                                                                     | 10  | 日付            |                     |
| payment_transfer_date_from           | 入金日(開始日)                                                                                                                                                                                                                                                                                                                                            | 10  | 日付            |                     |
| payment_transfer_date_to             | 入金日(終了日)                                                                                                                                                                                                                                                                                                                                            | 10  | 日付|           |                     |


#### payment (request)

下記のような項目のオブジェクトを持つリスト

| 名前            | 概要         | 桁数 | 種別         | 必須                |
| --------------- |-------------|------| ----------- | --------------------|
| payment_id      | 入金ID       | 18  | 数値         |                     |

#### bill (request)

下記のような項目のオブジェクトを持つリスト

| 名前             | 概要         | 桁数 | 種別         | 必須                |
| ---------------| ------------- | ---- | ----------- | ------------------- |
| number          | 請求書番号    | 100  | 文字列      |                     |

#### erasure (request)

下記のような項目のオブジェクトを持つリスト

| 名前             | 概要        | 桁数  | 種別         | 必須               |
| ---------------- | ---------- | ----- | ----------- | ------------------ |
| erasure_id       | 消込結果ID | 18    | 数値         |                     |

#### erasure_bill (request)

下記のような項目のオブジェクトを持つリスト

| 名前             | 概要           | 桁数   | 種別          | 必須              |
| ---------------- | --------------| ----- | ------------ | ------------------ |
| number           | 消込請求書番号 | 100     | 文字列         |                 |



## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                           | 概要                                                                                                                                    | 型            |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| user_id                        | ユーザーID                                                                                                                               | string        |
| access_key                     | アクセスキー                                                                                                                             | string        |
| limit_count                    | 消込情報取得件数 <br> ※最大件数は、リクエストで指定された「消込情報取得件数」に依存                                                           | int           |
| page_count                     | 消込情報取得開始インデックス <br> ※取得した消込情報の開始インデックスを返却する                                                              | int           |
| total_page_count               | 消込情報取得開始インデックス合計 <br> ※指定された検索条件によって取得可能な消込情報の全件数／消込情報取得件数によって、算出される値を返却する      | int           |
| [clearing](#clearing-response) | 消込情報に属するパラメータ                                                                                                                | `array` |

#### clearing (response)

下記のような項目のオブジェクトを持つリスト

| 名前                                  | 概要                                                                                                                                                                                                                                                                                                                                                | 型             |
| ------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------- |
| error_code                            | エラーコード <br> ※正常時はnull                                                                                                                                                                                                                                                                                                                      | string        |
| error_message                         | エラーメッセージ <br> ※正常時はnull                                                                                                                                                                                                                                                                                                                  | string         |
| erasure_id                            | 消込結果ID                                                                                                                                                                                                                                                                                                                                         | int      |
| clearing_date                         | 消込処理日                                                                                                                                                                                                                                                                                                                                         | string         |
| error_message                         | 消込計上日                                                                                                                                                                                                                                                                                                                                         | string        |
| clearing_method                       | 消込手段 <br> 0:銀行振込 <br> 1:クレジットカード <br> 2:バンクチェック <br> 3:RP口座振替 <br> 4:RL口座振替 <br> 5:その他口座振替 <br> 6:コンビニ払込票(A4) <br> 7:コンビニ払込票(ハガキ) <br> 8:その他コンビニ払込票 <br> 9:バーチャル口座 <br> 10:その他決済手段1 <br> 11:その他決済手段2 <br> 12:その他決済手段3 <br> 13:その他決済手段4 <br> 14:その他決済手段5 <br> 98:相殺 <br> 101:貸倒 <br> 102:確認済み <br> 103:手数料 <br> 106:現金 <br> 107:長期滞留債権 <br> 108:破産更生等債権 <br> 109:売上取消 <br> 110:繰越                          | int     |
| clearing_amount                       | 消込金額                                                                                                                                                                                                                                                                                                                                          | int        |
| bill_billing_number                   | 請求書番号                                                                                                                                                                                                                                                                                                                                         | string        |
| bill_amount                           | 請求金額                                                                                                                                                                                                                                                                                                                                            | int        |
| clearing_bill_billing_number          | 消込請求書番号                                                                                                                                                                                                                                                                                                                                       | string           |
| payment_id                            | 入金ID                                                                                                                                                                                                                                                                                                                                               | int           |
| payment_transfer_date                 | 入金日                                                                                                                                                                                                                                                                                                                                               | string        |
| account_namne                         | 振込名義                                                                                                                                                                                                                                                                                                                                             | string           |
| payment_amount                        | 入金金額                                                                                                                                                                                                                                                                                                                                             | int           |
| erasure_cancel_flg                    | 取消フラグ <br> 0:未取消 <br> 1:取消済み                                                                                                                                                                                                                                                                                                               | int           |
| erasure_cancel_recorded_date          | 消込取消計上日                                                                                                                                                                                                                                                                                                                                        | string           |



## 使用例

### リクエスト例

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "limit_count": "",
    "page_count": "",
    "clearing": {
        "payment": [
            {
                "payment_id": 12
            }
        ],
        "bill": [
            {
                "number": "202005-sample-1"
            }
        ],
        "erasure": [
            {
                "erasure_id": 1
            },
            {
                "erasure_id": 2
            }
        ],
        "erasure_bill": [
            {
                "number": "202005-sample-2"
            }
        ],
        "clearing_method": 0,
        "clearing_date_from": "2020/03/01",
        "clearing_date_to": "2020/04/11",
        "erasure_deposit_recorded_date_from": "2020/03/01",
        "erasure_deposit_recorded_date_to": "2020/04/11",
        "erasure_cancel_recorded_date_from": "",
        "erasure_cancel_recorded_date_to": null,
        "payment_transfer_date_from": "2020/03/01",
        "payment_transfer_date_to": "2020/04/11"
    }
}
```

### レスポンス例

Status: 200 OK

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "limit_count": 20,
    "page_count": 0,
    "total_page_count": 1,
    "clearing": [
        {
            "error_code": null,
            "error_message": null,
            "erasure_id": 1,
            "clearing_date": "2020/04/01",
            "erasure_deposit_recorded_date": "2020/04/01",
            "clearing_method": 0,
            "clearing_amount": 1080,
            "bill_billing_number": "202005-sample-1",
            "bill_amount": 1080,
            "clearing_bill_billing_number":"",
            "payment_id": 12,
            "payment_transfer_date": "2020/04/01",
            "account_name": "振込名義",
            "payment_amount": 1080,
            "erasure_cancel_flg": 0,
            "erasure_cancel_recorded_date": null
        },
        {
            "error_code": null,
            "error_message": null,
            "erasure_id": 2,
            "clearing_date": "2020/04/01",
            "erasure_deposit_recorded_date": "2020/04/01",
            "clearing_method": 0,
            "clearing_amount": 1080,
            "bill_billing_number": "202005-sample-2",
            "bill_amount": 1080,
            "clearing_bill_billing_number":null,
            "payment_id": 13,
            "payment_transfer_date": "2020/04/01",
            "account_name": "振込名義",
            "payment_amount": 1080,
            "erasure_cancel_flg": 1,
            "erasure_cancel_recorded_date": "2020/04/01"
        }
    ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード  | 内容                                      |
| ------------ | ------------------------------------------|
| 3501         | 入金IDが不正                               |
| 3502         | 請求書番号が不正                           |
| 3503         | 消込結果IDが不正                           |
| 3504         | 消込手段が不正                             |
| 3505         | 消込処理日の開始日が不正                    |
| 3506         | 消込処理日の終了日が不正                    |
| 3507         | 消込計上日の開始日が不正                    |
| 3508         | 消込計上日の終了日が不正                    |
| 3509         | 消込取消計上日の開始日が不正                 |
| 3510         | 消込取消計上日の終了日が不正                 |
| 3511         | 入金日の開始日が不正                        |
| 3512         | 入金日の終了日が不正                        |
| 3513         | 消込結果参照に失敗しました                  |
| 3514         | 入金情報取得件数が不正                      |
| 3515         | 入金情報取得開始インデックスが不正           |



----

[TOPへ戻る](../../index.md)