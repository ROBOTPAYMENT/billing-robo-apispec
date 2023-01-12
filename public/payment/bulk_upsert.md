# 入金登録更新

`/api/v1.0/payment/bulk_upsert`

複数の入金情報を登録または更新します。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト

- Path: `/api/v1.0/payment/bulk_upsert`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                                                    | 概要                                         | 桁数 | 種別                              | 必須     |
| ------------------------------------------------------- | -------------------------------------------- | ---- | --------------------------------- | -------- |
| user_id                                                 | ユーザー ID <br> （管理画面へのログイン ID） | 100  | [メール形式](../../index.md#種別) | 必須     |
| access_key                                              | アクセスキー                                 | 100  | [半角英数](../../index.md#種別)   | 必須     |
| [payment_bank_transfer](#payment_bank_transfer-request) | 銀行振込形式に属するパラメータ               |      | `array`                           | (必須)^1 |
| [payment_other](#payment_other-request)                 | その他形式に属するパラメータ                 |      | `array`                           | (必須)^1 |

#### payment_bank_transfer (request)

下記のような項目のオブジェクトを持つリスト

| 名前                      | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;概要&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | 桁数 | 種別                                   | 必須           |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---- | -------------------------------------- | -------------- |
| payment_id                | 入金 ID <br> ※値ありの場合は更新                                                                                                                                                                                                                                                                                                                                             | 18   | 数値                                   | (更新時)       |
| payment_transfer_date     | 入金日                                                                                                                                                                                                                                                                                                                                                                       | 10   | 日付                                   | 必須           |
| amount                    | 金額                                                                                                                                                                                                                                                                                                                                                                         | 10   | 数値                                   | 必須           |
| account_name              | 口座名義                                                                                                                                                                                                                                                                                                                                                                     | 48   | [口座名義](../../index.md#種別)        | 必須           |
| bs_bank_transfer_code     | 請求元銀行口座コード <br> ※追加省略時、null で登録される                                                                                                                                                                                                                                                                                                                     | 20   | [半角英数 + 記号](../../index.md#種別) |                |
| suspense_received_flg     | 仮受金フラグ <br> 0:仮受金として登録しない <br> 1:仮受金として登録する <br> ※追加省略時、0 で登録される                                                                                                                                                                                                                                                                      | 1    | 数値                                   |                |
| billing_code              | 請求先コード <br> ※仮受⾦として登録する場合のみ入力 <br> ※追加省略時、null で登録される                                                                                                                                                                                                                                                                                      | 20   | [半角英数 + 記号](../../index.md#種別) | (仮受金登録時) |
| billing_individual_number | 請求先部署番号 <br> ※仮受⾦として登録する場合のみ入力 <br> ※追加省略時、null で登録される                                                                                                                                                                                                                                                                                    | 18   | 数値                                   | (仮受金登録時) |
| clearing_method           | 消込方法 <br> 0:自動消込 <br> 1:手動消込 <br> ※追加省略時、1 で登録される                                                                                                                                                                                                                                                                                                    | 1    | 数値                                   |                |
| clearing_status           | 消込ステータス <br> 0:未消込 <br> 2:確認済み <br> ※追加省略時、0 で登録される                                                                                                                                                                                                                                                                                                | 1    | 数値                                   |                |
| accrued_clearing_auto_flg | 未収自動消込フラグ <br> 0:未収を自動消込の対象に含めない <br> 1:未収を自動消込の対象に含める <br> ※追加省略時、または消込方法:手動消込の場合 0 で登録される                                                                                                                                                                                                                  | 1    | 数値                                   |                |
| memo                      | メモ <br> ※省略時、nullで登録される <br> ※更新時、空文字指定で空白にできます。                                                                                                                                                                                                                                                                                                                        | 300          | 文字列                                 |                               |

#### payment_other (request)

下記のような項目のオブジェクトを持つリスト

| 名前                      | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;概要&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | 桁数 | 種別                                   | 必須           |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---- | -------------------------------------- | -------------- |
| payment_id                | 入金 ID <br> ※値ありの場合は更新                                                                                                                                                                                                                                                                                                                                             | 18   | 数値                                   | (更新時)       |
| payment_transfer_date     | 入金日                                                                                                                                                                                                                                                                                                                                                                       | 10   | 日付                                   | 必須           |
| amount                    | 金額                                                                                                                                                                                                                                                                                                                                                                         | 10   | 数値                                   | 必須           |
| bs_bank_transfer_code     | 請求元銀行口座コード <br> ※追加省略時、null で登録される                                                                                                                                                                                                                                                                                                                     | 20   | [半角英数 + 記号](../../index.md#種別) |                |
| suspense_received_flg     | 仮受金フラグ <br> 0:仮受金として登録しない <br> 1:仮受金として登録する <br> ※追加省略時、0 で登録される                                                                                                                                                                                                                                                                      | 1    | 数値                                   |                |
| billing_code              | 請求先コード <br> ※仮受⾦として登録する場合のみ入力 <br> ※追加省略時、null で登録される                                                                                                                                                                                                                                                                                      | 20   | [半角英数 + 記号](../../index.md#種別) | (仮受金登録時) |
| billing_individual_number | 請求先部署番号 <br> ※仮受⾦として登録する場合のみ入力 <br> ※追加省略時、null で登録される                                                                                                                                                                                                                                                                                    | 18   | 数値                                   | (仮受金登録時) |
| payment_method            | 決済手段 <br> 10:その他決済⼿段 1 <br> 11:その他決済⼿段 2 <br> 12:その他決済⼿段 3 <br> 13:その他決済⼿段 4 <br> 14:その他決済⼿段 5                                                                                                                                                                                                                                        | 2    | 数値                                   | 必須           |
| clearing_key              | 消込キー <br> ※追加省略時、null で登録される                                                                                                                                                                                                                                                                                                                                 | 33   | 文字列                                 |                |
| clearing_method           | 消込方法 <br> 0:自動消込 <br> 1:手動消込 <br> ※追加省略時、1 で登録される                                                                                                                                                                                                                                                                                                    | 1    | 数値                                   |                |
| clearing_status           | 消込ステータス <br> 0:未消込 <br> 2:確認済み <br> ※追加省略時、0 で登録される                                                                                                                                                                                                                                                                                                | 1    | 数値                                   |                |
| accrued_clearing_auto_flg | 未収自動消込フラグ <br> 0:未収を自動消込の対象に含めない <br> 1:未収を自動消込の対象に含める <br> ※追加省略時、または消込方法:手動消込の場合 0 で登録される                                                                                                                                                                                                                  | 1    | 数値                                   |                |
| memo                      | メモ <br> ※省略時、nullで登録される <br> ※更新時、空文字指定で空白にできます。                                                                                                                                                                                                                                                                                                                        | 300          | 文字列                                 |                               |

## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                                                     | 概要                                   | 型      |
| -------------------------------------------------------- | -------------------------------------- | ------- |
| user_id                                                  | ユーザー ID（管理画面へのログイン ID） | string  |
| [payment_bank_transfer](#payment_bank_transfer-response) | 銀行振込形式に属するパラメータ         | `array` |
| [payment_other](#payment_other-response)                 | その他形式に属するパラメータ           | `array` |

#### payment_bank_transfer (response)

下記のような項目のオブジェクトを持つリスト

| 名前                      | 概要                                                                                         | 型     |
| ------------------------- | -------------------------------------------------------------------------------------------- | ------ |
| error_code                | エラーコード <br> ※正常時は null                                                             | int     |
| error_message             | エラーメッセージ <br> ※正常時は null                                                         | string  |
| payment_id                | 入金 ID                                                                                      | int    |
| payment_transfer_date     | 入金日                                                                                       | string |
| amount                    | 金額                                                                                         | int    |
| account_name              | 口座名義                                                                                     | string |
| bs_bank_transfer_code     | 請求元銀行口座コード                                                                         | string |
| suspense_received_flg     | 仮受金フラグ <br> 0:仮受金として登録しない <br> 1:仮受金として登録する                       | int    |
| billing_code              | 請求先コード                                                                                 | string |
| billing_individual_number | 請求先部署番号                                                                               | int    |
| clearing_method           | 消込方法 <br> 0:自動消込 <br> 1:手動消込                                                     | int    |
| clearing_status           | 消込ステータス <br> 0:未消込 <br> 2:確認済み                                                 | int    |
| accrued_clearing_auto_flg | 未収自動消込フラグ <br> 0:未収を自動消込の対象に含めない <br> 1:未収を自動消込の対象に含める | int    |
| memo                      | メモ                                                                                       | string |

#### payment_other (response)

下記のような項目のオブジェクトを持つリスト

| 名前                      | 概要                                                                                                                                  | 型     |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| error_code                | エラーコード <br> ※正常時は null                                                                                                      | int    |
| error_message             | エラーメッセージ <br> ※正常時は null                                                                                                  | string |
| payment_id                | 入金 ID                                                                                                                               | int    |
| payment_transfer_date     | 入金日                                                                                                                                | string |
| amount                    | 金額                                                                                                                                  | int    |
| bs_bank_transfer_code     | 請求元銀行口座コード                                                                                                                  | string |
| suspense_received_flg     | 仮受金フラグ <br> 0:仮受金として登録しない <br> 1:仮受金として登録する                                                                | int    |
| billing_code              | 請求先コード                                                                                                                          | string |
| billing_individual_number | 請求先部署番号                                                                                                                        | int    |
| payment_method            | 決済手段 <br> 10:その他決済⼿段 1 <br> 11:その他決済⼿段 2 <br> 12:その他決済⼿段 3 <br> 13:その他決済⼿段 4 <br> 14:その他決済⼿段 5      | int    |
| clearing_key              | 消込キー                                                                                                                              | string |
| clearing_method           | 消込方法 <br> 0:自動消込 <br> 1:手動消込                                                                                              | int    |
| clearing_status           | 消込ステータス <br> 0:未消込 <br> 2:確認済み                                                                                          | int    |
| accrued_clearing_auto_flg | 未収自動消込フラグ <br> 0:未収を自動消込の対象に含めない <br> 1:未収を自動消込の対象に含める                                          | int    |
| memo                      | メモ                                                                                                                               | string |

## 使用例

### リクエスト例

２ケースを同時の登録する際のリクエスト例

ケース１

- 入金 ID を参照できる項目を事前登録していない

ケース２

- 入金 ID を参照できる項目を事前登録している

```json
{
  "user_id": "sample@robotpayment.co.jp",
  "access_key": "xxxxxxxxxxxxxxxx",
  "payment_bank_transfer": [
    {
      "payment_id": "",
      "payment_transfer_date": "2020/04/01",
      "amount": 10000,
      "account_name": "ｺｳｻﾞﾒｲｷﾞ",
      "bs_bank_transfer_code": 246,
      "suspense_received_flg": 1,
      "billing_code": "billing_code",
      "billing_individual_number": 123456,
      "clearing_method": 0,
      "clearing_status": 0,
      "accrued_clearing_auto_flg": 0,
      "memo": "メモ1",
    },
    {
      "payment_id": 2,
      "payment_transfer_date": "2020/04/03",
      "amount": 20000,
      "account_name": "ｺｳｻﾞﾒｲｷﾞｿﾉﾆ",
      "bs_bank_transfer_code": null,
      "suspense_received_flg": null,
      "billing_code": null,
      "billing_individual_number": null,
      "clearing_method": null,
      "clearing_status": 2,
      "accrued_clearing_auto_flg": 1,
      "memo": "メモ2",
    }
  ],
  "payment_other": []
}
```

### レスポンス例

Status: 200 OK

```json
{
  "user_id": "sample@robotpayment.co.jp",
  "payment_bank_transfer": [
    {
      "error_code": null,
      "error_message": null,
      "payment_id": 3,
      "payment_transfer_date": "2020/04/01",
      "amount": 10000,
      "account_name": "ｺｳｻﾞﾒｲｷﾞ",
      "bs_bank_transfer_code": 246,
      "suspense_received_flg": 1,
      "billing_code": "billing_code",
      "billing_individual_number": 123456,
      "clearing_method": 0,
      "clearing_status": 0,
      "accrued_clearing_auto_flg": 0,
      "memo": "メモ1",
    },
    {
      "error_code": null,
      "error_message": null,
      "payment_id": 2,
      "payment_transfer_date": "2020/04/03",
      "amount": 20000,
      "account_name": "ｺｳｻﾞﾒｲｷﾞｿﾉﾆ",
      "bs_bank_transfer_code": 468,
      "suspense_received_flg": 1,
      "billing_code": "billing_code",
      "billing_individual_number": 123456,
      "clearing_method": 0,
      "clearing_status": 2,
      "accrued_clearing_auto_flg": 1,
      "memo": "メモ2",
    }
  ],
  "payment_other": []
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                                                                |
| ------------ | ------------------------------------------------------------------ |
| 3601         | リクエストパラメータに入金情報が存在しません。                        |
| 3602         | 銀行振込、その他のリクエストパラメータは同時に登録更新できません。     |
| 3603         | 入金 ID が不正                                                      |
| 3604         | 入金日が不正                                                        |
| 3605         | 金額が不正                                                          |
| 3606         | 口座名義が不正                                                      |
| 3607         | 請求元銀行口座コードが不正                                           |
| 3608         | 仮受金フラグが不正                                                  |
| 3609         | 請求先コードが不正                                                  |
| 3610         | 請求先部署番号が不正                                                |
| 3611         | 決済手段が不正                                                      |
| 3612         | 消込方法が不正                                                      |
| 3613         | 消込ステータスが不正                                                |
| 3614         | 未収自動消込フラグが不正                                             |
| 3615         | 消込キーが不正                                                      |
| 3616         | 更新対象の入金情報が存在しません。                                   |
| 3617         | 請求先に紐づく請求先部署が存在しません。                             |
| 3618         | 入金消込ステータスが完了しています。                                 |
| 3619         | 処理中の入金情報は変更できません。                                   |
| 3620         | 承認中の入金情報は変更できません。                                   |
| 3621         | 入金情報は既に削除しています。                                       |
| 3622         | 入金情報は既に無効化しています。                                     |
| 3623         | 一部または全て消込済みの入金情報は変更できません。                    |
| 3624         | 既に締められた入金情報は登録更新できません。                          |
| 3625         | 振込先銀行口座が存在しません。                                       |
| 3626         | 入金情報の登録更新に失敗しました。                                   |
| 3627         | 確認済みの入金情報は仮受金フラグを変更できません。                    |
| 3628         | リクエスト件数が上限を超えています。                                 |
| 3629         | メモが不正                                                         |

---

[TOP へ戻る](../../index.md)
