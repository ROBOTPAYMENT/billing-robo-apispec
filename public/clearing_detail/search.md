# 消込結果明細参照

`/api/v1.0/clearing_detail/search`

消込結果の明細を、検索順序のパラメータに沿って取得します。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト

- Method URL: `https://billing-robo.jp:10443/api/v1.0/clearing_detail/search`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                          | 概要                                                                                              | 桁数 | 種別                              | 必須 |
| ----------------------------- | ------------------------------------------------------------------------------------------------- | ---- | --------------------------------- | ---- |
| user_id                       | ユーザー ID（管理画面へのログイン ID）                                                            | 100  | [メール形式](../../index.md#種別) | 必須 |
| access_key                    | アクセスキー                                                                                      | 100  | [半角英数](../../index.md#種別)   | 必須 |
| limit_count                   | 消込情報取得件数 <br> ※0〜200 の数値を設定する。 <br> 省略した場合、20 が設定される               | 3    | 数値                              |      |
| page_count                    | 消込情報取得開始インデックス <br> ※0〜24,999 の数値を設定する。 <br> 省略した場合、0 が設定される | 2    | 数値                              |      |
| [sort](#sort-request)         | 検索順序に属するパラメータ <br> ※省略時は消込結果 ID、消込結果明細 ID の昇順で返却する            |      | `object`                          |      |
| [clearing](#clearing-request) | 消込情報に属するパラメータ                                                                        |      | `object`                          |      |

#### sort (request)

下記のような項目のオブジェクトを持つリスト

| 名前  | 概要                                                                                                                                                                                                                                         | 桁数 | 種別   | 必須 | 一致 |
| ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---- | ------ | ---- | ---- |
| key   | キー名 <br> ※以下の項目からのみ有効、複数選択可(カンマ区切り) <br> ※左の項目から優先的にソート順序が指定される <br> erasure_id: 消込結果 ID, <br> erasure_detail_id: 消込結果明細 ID, <br> regist_date: 登録日時, <br> update_date: 更新日時 | 52   | 文字列 |      |      |
| order | ソート順 <br> 0: 昇順 <br> 1: 降順                                                                                                                                                                                                           | 1    | 数値   |      |      |

#### clearing (request)

下記のような項目のオブジェクトを持つリスト

| 名前                                  | 概要                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | 桁数 | 種別    | 必須 | 一致 |
| ------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---- | ------- | ---- | ---- |
| [payment](#payment-request)           | 入金情報に属するパラメータ                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |      | `array` |      |      |
| [bill](#bill-request)                 | 請求書に属するパラメータ                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |      | `array` |      |      |
| [erasure](#erasure-request)           | 消込結果に属するパラメータ                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |      | `array` |      |      |
| [erasure_bill](#erasure_bill-request) | 消込請求書に属するパラメータ                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |      | `array` |      |      |
| clearing_method                       | 消込手段 <br> 0:銀行振込 <br> 1:クレジットカード <br> 2:バンクチェック <br> 3:RP 口座振替 <br> 4:RL 口座振替 <br> 5:その他口座振替 <br> 6:コンビニ払込票(A4) <br> 7:コンビニ払込票(ハガキ) <br> 8:その他コンビニ払込票 <br> 9:バーチャル口座 <br> 10:その他決済手段 1 <br> 11:その他決済手段 2 <br> 12:その他決済手段 3 <br> 13:その他決済手段 4 <br> 14:その他決済手段 5 <br> 98:相殺 <br> 101:貸倒 <br> 102:確認済み <br> 103:手数料 <br> 106:現金 <br> 107:長期滞留債権 <br> 108:破産更生等債権 <br> 109:売上取消 <br> 110:繰越 | 3    | 数値    |      |      |
| clearing_date_from                    | 消込処理日(開始日)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | 10   | 日付    |      |      |
| clearing_date_to                      | 消込処理日(終了日)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | 10   | 日付    |      |      |
| erasure_deposit_recorded_date_from    | 消込計上日(開始日)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | 10   | 日付    |      |      |
| erasure_deposit_recorded_date_to      | 消込計上日(終了日)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | 10   | 日付    |      |      |
| erasure_cancel_recorded_date_from     | 消込取消計上日(開始日)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | 10   | 日付    |      |      |
| erasure_cancel_recorded_date_to       | 消込取消計上日(終了日)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | 10   | 日付    |      |      |
| payment_transfer_date_from            | 入金日(開始日)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | 10   | 日付    |      |      |
| payment_transfer_date_to              | 入金日(終了日)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | 10   | 日付    |      |      |
| regist_date_from                      | 登録日(開始日) <br> ※時刻省略時 00:00:00 で検索します                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | 19   | 日時    |      |      |
| regist_date_to                        | 登録日(終了日) <br> ※時刻省略時 23:59:59 で検索します                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | 19   | 日時    |      |      |
| update_date_from                      | 更新日(開始日) <br> ※時刻省略時 00:00:00 で検索します                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | 19   | 日時    |      |      |
| update_date_to                        | 更新日(終了日) <br> ※時刻省略時 23:59:59 で検索します                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | 19   | 日時    |      |      |

#### payment (request)

下記のような項目のオブジェクトを持つリスト

| 名前       | 概要    | 桁数 | 種別 | 必須 | 一致 |
| ---------- | ------- | ---- | ---- | ---- | ---- |
| payment_id | 入金 ID | 18   | 数値 |      |      |

#### bill (request)

下記のような項目のオブジェクトを持つリスト

| 名前   | 概要       | 桁数 | 種別   | 必須 | 一致     |
| ------ | ---------- | ---- | ------ | ---- | -------- |
| number | 請求書番号 | 100  | 文字列 |      | 完全一致 |

#### erasure (request)

下記のような項目のオブジェクトを持つリスト

| 名前       | 概要        | 桁数 | 種別 | 必須 | 一致 |
| ---------- | ----------- | ---- | ---- | ---- | ---- |
| erasure_id | 消込結果 ID | 18   | 数値 |      |      |

#### erasure_bill (request)

下記のような項目のオブジェクトを持つリスト

| 名前   | 概要           | 桁数 | 種別   | 必須 | 一致     |
| ------ | -------------- | ---- | ------ | ---- | -------- |
| number | 消込請求書番号 | 100  | 文字列 |      | 完全一致 |

## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                                             | 概要                                                                                                                                          | 型      |
| ------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| user_id                                          | ユーザー ID                                                                                                                                   | string  |
| access_key                                       | アクセスキー                                                                                                                                  | string  |
| limit_count                                      | 消込情報取得件数 <br> ※最大件数は、リクエストで指定された「消込情報取得件数」に依存                                                           | int     |
| page_count                                       | 消込情報取得開始インデックス <br> ※取得した消込情報の開始インデックスを返却する                                                               | int     |
| total_page_count                                 | 消込情報取得開始インデックス合計 <br> ※指定された検索条件によって取得可能な消込情報の全件数／消込情報取得件数によって、算出される値を返却する | int     |
| [sort](#sort-response)                           | 検索順序に属するパラメータ                                                                                                                    | `array` |
| [clearing](#clearing-response)                   | 消込結果明細に属するパラメータ                                                                                                                | `array` |
| [count_update_date](#count_update_date-response) | 更新日時に属するパラメータ                                                                                                                    | `array` |

#### sort (response)

下記のような項目のオブジェクトを持つリスト

| 名前  | 概要                               | 型     |
| ----- | ---------------------------------- | ------ |
| key   | キー名                             | string |
| order | ソート順 <br> 0: 昇順 <br> 1: 降順 | int    |

#### clearing (response)

下記のような項目のオブジェクトを持つリスト

| 名前                          | 概要                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | 型       |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| error_code                    | エラーコード <br> ※正常時は null                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | string   |
| error_message                 | エラーメッセージ <br> ※正常時は null                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | string   |
| erasure_id                    | 消込結果 ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | int      |
| erasure_detail_id             | 消込結果明細 ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | int      |
| sales_id                      | 売上 ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | int      |
| clearing_date                 | 消込処理日                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | date     |
| erasure_deposit_recorded_date | 消込計上日                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | date     |
| clearing_method               | 消込手段 <br> 0:銀行振込 <br> 1:クレジットカード <br> 2:バンクチェック <br> 3:RP 口座振替 <br> 4:RL 口座振替 <br> 5:その他口座振替 <br> 6:コンビニ払込票(A4) <br> 7:コンビニ払込票(ハガキ) <br> 8:その他コンビニ払込票 <br> 9:バーチャル口座 <br> 10:その他決済手段 1 <br> 11:その他決済手段 2 <br> 12:その他決済手段 3 <br> 13:その他決済手段 4 <br> 14:その他決済手段 5 <br> 98:相殺 <br> 101:貸倒 <br> 102:確認済み <br> 103:手数料 <br> 106:現金 <br> 107:長期滞留債権 <br> 108:破産更生等債権 <br> 109:売上取消 <br> 110:繰越 | int      |
| clearing_amount               | 消込金額                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | int      |
| bill_billing_number           | 請求書番号                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | string   |
| bill_amount                   | 請求金額                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | int      |
| clearing_bill_billing_number  | 消込請求書番号                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | string   |
| payment_id                    | 入金 ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | int      |
| payment_transfer_date         | 入金日                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | date     |
| account_name                  | 振込名義                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | string   |
| payment_amount                | 入金金額                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | int      |
| erasure_cancel_flg            | 取消フラグ <br> 0:未取消 <br> 1:取消済み                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | int      |
| erasure_cancel_recorded_date  | 消込取消計上日                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | string   |
| regist_date                   | 登録日                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | datetime |
| update_date                   | 更新日                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | datetime |

#### count_update_date (response)

下記のような項目のオブジェクトを持つリスト

| 名前        | 概要             | 型       |
| ----------- | ---------------- | -------- |
| update_date | 更新日時         | datetime |
| count       | 更新日時毎の件数 | int      |

## 使用例

### リクエスト例

```json
{
  "user_id": "sample@robotpayment.co.jp",
  "access_key": "xxxxxxxxxxxxxxxx",
  "limit_count": "",
  "page_count": "",
  "sort": {
    "key": "regist_date,erasure_id,erasure_detail_id",
    "order": 0
  },
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
    "erasure_cancel_recorded_date_from": null,
    "erasure_cancel_recorded_date_to": null,
    "payment_transfer_date_from": "2020/03/01",
    "payment_transfer_date_to": "2020/04/11",
    "regist_date_from": "2020/03/01 00:12:00",
    "regist_date_to": "2020/03/01 23:59:59",
    "update_date_from": "2020/03/01 00:00:00",
    "update_date_to": "2020/03/01 23:59:59"
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
  "sort": {
    "key": "regist_date,erasure_id,erasure_detail_id",
    "order": 0
  },
  "erasure_detail": [
    {
      "error_code": null,
      "error_message": null,
      "erasure_id": 1,
      "erasure_detail_id": 1,
      "sales_id": 1,
      "clearing_date": "2020/04/01",
      "erasure_deposit_recorded_date": "2020/04/01",
      "clearing_method": 0,
      "clearing_amount": 1080,
      "bill_billing_number": "202005-sample-1",
      "bill_amount": 1080,
      "clearing_bill_billing_number": null,
      "payment_id": 12,
      "payment_transfer_date": "2020/04/01",
      "account_name": "振込名義",
      "payment_amount": 1080,
      "erasure_cancel_flg": 1,
      "erasure_cancel_recorded_date": "2020/04/01",
      "regist_date": "2020/03/01 12:15:00",
      "update_date": "2020/03/01 12:15:00"
    },
    {
      "error_code": null,
      "error_message": null,
      "erasure_id": 1,
      "erasure_detail_id": 2,
      "sales_id": 2,
      "clearing_date": "2020/04/01",
      "erasure_deposit_recorded_date": "2020/04/01",
      "clearing_method": 0,
      "clearing_amount": 1080,
      "bill_billing_number": "202005-sample-1",
      "bill_amount": 1080,
      "clearing_bill_billing_number": null,
      "payment_id": 12,
      "payment_transfer_date": "2020/04/01",
      "account_name": "振込名義",
      "payment_amount": 1080,
      "erasure_cancel_flg": 1,
      "erasure_cancel_recorded_date": "2020/04/01",
      "regist_date": "2020/03/01 12:10:00",
      "update_date": "2020/03/01 12:10:00"
    },
    {
      "error_code": null,
      "error_message": null,
      "erasure_id": 2,
      "erasure_detail_id": 3,
      "sales_id": 3,
      "clearing_date": "2020/04/01",
      "erasure_deposit_recorded_date": "2020/04/01",
      "clearing_method": 0,
      "clearing_amount": 1080,
      "bill_billing_number": "202005-sample-1",
      "bill_amount": 1080,
      "clearing_bill_billing_number": "202005-sample-5",
      "payment_id": null,
      "payment_transfer_date": "2020/04/01",
      "account_name": "振込名義",
      "payment_amount": 1080,
      "erasure_cancel_flg": 1,
      "erasure_cancel_recorded_date": "2020/04/01",
      "regist_date": "2020/03/01 12:00:00",
      "update_date": "2020/03/01 12:00:00"
    }
  ],
  "count_update_date": [
    {
      "update_date": "2020/03/01 12:15:00",
      "count": 1
    },
    {
      "update_date": "2020/03/01 12:00:00",
      "count": 2
    }
  ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                                       |
| ------------ | ------------------------------------------ |
| 4001         | 入金 ID が不正                             |
| 4002         | 請求書番号が不正                           |
| 4003         | 消込結果 ID が不正                         |
| 4004         | 消込手段が不正                             |
| 4005         | 消込処理日の開始日が不正                   |
| 4006         | 消込処理日の終了日が不正                   |
| 4007         | 消込計上日の開始日が不正                   |
| 4008         | 消込計上日の終了日が不正                   |
| 4009         | 消込取消計上日の開始日が不正               |
| 4010         | 消込取消計上日の終了日が不正               |
| 4011         | 入金日の開始日が不正                       |
| 4012         | 入金日の終了日が不正                       |
| 4013         | 登録日の開始日が不正                       |
| 4014         | 登録日の終了日が不正                       |
| 4015         | 更新日の開始日が不正                       |
| 4016         | 更新日の終了日が不正                       |
| 4017         | キー名が不正です                           |
| 4018         | ソート順が不正です                         |
| 4019         | 消込結果明細情報取得件数が不正             |
| 4020         | 消込結果明細情報取得開始インデックスが不正 |
| 4021         | 消込結果明細件数が 25,000 件に到達しました |
| 4022         | 消込結果明細参照に失敗しました             |

---

[TOP へ戻る](../../index.md)
