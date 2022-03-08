# 請求書送付メール

`/api/v1.0/bill/send_bill_by_email`

請求書送付をメールで行う場合に利用します

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Path: `/api/v1.0/bill/send_bill_by_email`
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

| 名前   | 概要       | 桁数 | 種別                                   | 必須 |
| ------ | ---------- | ---- | -------------------------------------- | ---- |
| number | 請求書番号 | 100  | [半角英数 + 記号](../../index.md#種別) |      |


## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                   | 概要                       | 型            |
| ---------------------- | -------------------------- | ------------- |
| user_id                | ユーザーID                 | string        |
| access_key             | アクセスキー               | string        |
| [bill](#bill-response) | 請求書に属するパラメータ   | `array` |

#### bill (response)

下記のような項目のオブジェクトを持つリスト

| 名前               | 概要                                | 型     |
| ------------------ | ----------------------------------- | ------ |
| error_code         | エラーコード <br> ※正常時はnull     | int    |
| error_message      | エラーメッセージ <br> ※正常時はnull | string |
| number             | 請求書番号                          | string |
| email_order_number | メール受付番号                      | int    |


## 使用例

### リクエスト例

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "bill": [
        {
            "number": "201705-billing-1"
        },
        {
            "number": "201705-billing-2"
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
    "bill": [
        {
            "error_code": null,
            "error_message": null,
            "number": "201705-billing-1",
            "email_order_number": 1
        },
        {
            "error_code": 2701,
            "error_message": "Invalid 'billing_number'.",
            "number": "201705-billing-2",
            "email_order_number": null
        }
    ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                       |
| ------------ | -------------------------- |
| 2701         | 請求書メール送信に失敗     |
| 2702         | 請求書番号が不正           |
| 2703         | 無効の請求書は送信できない |
| 2704         | 請求書が承認依頼中         |
| 2705         | 請求先部署が承認依頼中     |

----

[TOPへ戻る](../../index.md)