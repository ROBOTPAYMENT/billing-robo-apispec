# 口座振替依頼書発行

`/api/v1.0/billing/bulk_download_pdf`

複数の請求先の決済情報の口座振替依頼書を発行します。
戻り値で受け取ったurlに再度アクセスすればダウンロードできます。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Path: `/api/v1.0/billing/bulk_download_pdf`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                        | 概要                               | 桁数 | 種別                              | 必須 |
| --------------------------- | ---------------------------------- | ---- | --------------------------------- | ---- |
| user_id                     | ユーザID（管理画面へのログインID） | 100  | [メール形式](../../index.md#種別) | 必須 |
| access_key                  | アクセスキー                       | 10   | [半角英数](../../index.md#種別)   | 必須 |
| [billing](#billing-request) | 請求先に属するパラメータ           |      | `array`                  |      |

#### billing (request)

下記のような項目のオブジェクトを持つリスト

| 名前                                       | 概要                       | 桁数 | 種別                                   | 必須 |
| ------------------------------------------ | -------------------------- | ---- | -------------------------------------- | ---- |
| code                                       | 請求先コード               | 20   | [半角英数 + 記号](../../index.md#種別) | 必須 |
| [billing.payment](#billingpayment-request) | 決済情報に属するパラメータ |      | `array`                       |      |

#### billing.payment (request)

下記のような項目のオブジェクトを持つリスト

| 名前   | 概要           | 桁数 | 種別                                   | 必須     |
| ------ | -------------- | ---- | -------------------------------------- | -------- |
| number | 決済情報番号   | 20   | 数字                                   | [(必須)^1](../../index.md#必須) |
| code   | 決済情報コード | 20   | [半角英数 + 記号](../../index.md#種別) | [(必須)^1](../../index.md#必須) |


## レスポンス
- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                         | 概要                     | 型               |
| ---------------------------- | ------------------------ | ---------------- |
| user_id                      | ユーザーID               | string           |
| access_key                   | アクセスキー             | string           |
| [billing](#billing-response) | 請求先に属するパラメータ | `array` |

#### billing (response)

下記のような項目のオブジェクトを持つリスト

| 名前                                        | 概要                         | 型                         |
| ------------------------------------------- | ---------------------------- | -------------------------- |
| error_code                                  | エラーコード                 | int <br> (正常時はnull)    |
| error_message                               | エラーメッセージ             | string <br> (正常時はnull) |
| code                                        | 請求先コード                 | string                     |
| name                                        | 請求先名                     | string                     |
| [billing.payment](#billingpayment-response) | 請求先決済情報に属するデータ | `array`           |

#### billing.payment (response)

下記のような項目のオブジェクトを持つリスト

| 名前          | 概要                          | 型                         |
| ------------- | ----------------------------- | -------------------------- |
| error_code    | エラーコード                  | int <br> (正常時はnull)    |
| error_message | エラーメッセージ              | string <br> (正常時はnull) |
| number        | 決済情報番号                  | int                        |
| code          | 決済情報コード                | string                     |
| name          | 決済情報名                    | string                     |
| url           | 口座振替依頼書ダウンロードURL | int                        |


## 使用例

### リクエスト例

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "billing" : [
        {
            "code": "billing_code",
            "payment": [
                {
                    "number": 12,
                    "code": "payment_code"
                }
            ]
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
    "billing" : [
        {
            "error_code": null,
            "error_message": null,
            "billing_code": "billing_code",
            "billing_name": "請求先名",
            "payment" : [
                {
                    "error_code": null,
                    "error_message": null,
                    "number": 12,
                    "code": "payment_code",
                    "url": "https://billing-robo.jp/api/bulk_download_request_form?id=xxx"
                }
            ]
        }
    ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                                 |
| ------------ | ------------------------------------ |
| 2101         | 請求先コードが不正                   |
| 2102         | 決済情報番号が不正                   |
| 2103         | 決済情報コードが不正                 |
| 2104         | 決済手段がRP口座振替以外             |
| 2105         | 顧客番号が空です                     |
| 2106         | 決済情報番号と決済情報コードが空です |
| 2107         | 決済情報が存在しません               |

----

[TOPへ戻る](../../index.md)