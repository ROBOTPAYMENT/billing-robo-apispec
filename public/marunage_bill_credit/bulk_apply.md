# 債権譲渡申請

`/api/v1.0/marunage_bill_credit/bulk_apply`

指定の請求書番号に対して債権譲渡申請を実行します。\
債権譲渡申請関連項目に関しては、[請求書参照](/public/bill/search.md)でも取得可能です。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Path: `/api/v1.0/marunage_bill_credit/bulk_apply`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                                                    | 概要                                                               | 桁数 | 種別               | 必須 |
|-------------------------------------------------------| ----------------------------------------------------------------- | ---- |-------------------| --- |
| user_id                                               | ユーザーID（管理画面へのログインID）                                    | 100  | [メール形式](../../index.md#種別)  | 必須 |
| access_key                                            | アクセスキー                                                        | 100  | [半角英数](../../index.md#種別)    | 必須 |
| [marunage_bill_credit](#marunage_bill_credit-request) | 債権譲渡申請に属するパラメータ                                            |      | `array`          | 必須 |

#### marunage_bill_credit (request)

下記のような項目のオブジェクトを持つリスト

| 名前             | 概要                                                | 桁数  | 種別                                 | 必須 |
| -----------------| -------------------------------------------------- | ---- | ----------------------------------- | ---- |
| bill_number      | 請求書番号                                           |100 | [半角英数 + 記号](../../index.md#種別)   | 必須  |



## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                                                     | 概要                                                                                               | 型       |
|--------------------------------------------------------|---------------------------------------------------------------------------------------------------| -------- |
| user_id                                                | ユーザーID                                                                                         | string   |
| [marunage_bill_credit](#marunage_bill_credit-response) | 債権譲渡申請に属するパラメータ                                                                            | `array`  |

#### marunage_bill_credit (response)

下記のような項目のオブジェクトを持つリスト

| 名前                   | 概要                                | 型      |
|-----------------------|-------------------------------------|--------|
| error_code            | エラーコード<br> ※正常時はnull         | int    |
| error_message         | エラーメッセージ<br> ※正常時はnull      | string |
| bill_number           | 請求書番号                           | string  |
| status                | 債権譲渡申請ステータス                 | int    |
| apply_date            | 債権譲渡申請日                        | date   |


## 使用例

### リクエスト例

```json
{
  "user_id": "sample@robotpayment.co.jp",
  "access_key": "xxxxxxxxxxxxxxxx",
  "marunage_bill_credit": [
    {
      "bill_number": "202401-sample-1"
    },
    {
      "bill_number": "202401-sample-2"
    }
  ]
}
```

### レスポンス例

Status: 200 OK

```json
{
  "user_id": "sample@robotpayment.co.jp",
  "request_marunage_credit": [
    {
      "error_code": null,
      "error_message": null,
      "bill_number": "202401-sample-1",
      "status":1,
      "apply_date":"2024-10-24"
    },
    {
      "error_code": 9999,
      "error_message": "error_text",
      "bill_number": "202401-sample-2",
      "status":null,
      "apply_date":null
    }
  ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

[その他の特殊なエラーコード](../../index.md#その他の特殊なエラーコード)

個別エラー

| エラーコード | 内容                                                       |
| ------------ |---------------------------------------------------------|
| 6110         | 請求書番号が不正                                           |
| 6111         | 対象の請求書が存在しません                                  |
| 6112         | 対象の請求書が有効ではありません                             |
| 6113         | まるなげ請求書ではない請求書が選択されています                 |
| 6114         | 債権譲渡申請ステータスが「未申請」でない請求書が選択されています  |


----

[TOPへ戻る](../../index.md)