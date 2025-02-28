# 債権譲渡申請参照

`/api/v1.0/marunage_bill_credit/search`

指定の請求書番号の債権譲渡申請ステータス参照を実行します。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Path: `/api/v1.0/marunage_bill_credit/search`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                    | 概要                                | 桁数 | 種別             | 必須 |
| ----------------------- | ----------------------------------| ---- |------------------| --- |
| user_id                 | ユーザーID（管理画面へのログインID）    | 100  | [メール形式](../../index.md#種別) | 必須 |
| access_key              | アクセスキー                         | 100  | [半角英数](../../index.md#種別) | 必須 |
| marunage_bill_credit    | 債権譲渡申請に属するパラメータ          |   3  | `array`          |     |


#### request_marunage_credit (request)

下記のような項目のオブジェクトを持つリスト

| 名前                     | 概要                                                | 桁数  | 種別                               | 必須 |
| ------------------------ | -------------------------------------------------- | ---- | --------------------------------- | ---- |
| bill_number      | 請求書番号                                            |100 | [半角英数 + 記号](../../index.md#種別)       | 必須  |



## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                    | 概要                                                                                               | 型       |
| ----------------------- |---------------------------------------------------------------------------------------------------| -------- |
| user_id                 | ユーザーID                                                                                         | string   |
| marunage_bill_credit | 債権譲渡申請に属するパラメータ                                                                            | `array`  |

#### request_marunage_credit (response)

下記のような項目のオブジェクトを持つリスト

| 名前           | 概要                                                                                               | 型      |
| -------------- |---------------------------------------------------------------------------------------------------| ------- |
| error_code     | エラーコード<br> ※正常時はnull                                                                       | int     |
| error_message  | エラーメッセージ<br> ※正常時はnull                                                                    | string  |
| bill_number   | 請求書番号                                                                                           | string  |
| status         | 債権譲渡申請ステータス<br> 0: 未申請<br> 1: 申請中<br>2: 承認<br> 3: 却下<br> 4: 回答中<br> 5: 債権譲渡解除 | int      |
| apply_date | 債権譲渡申請日                                                                                           | date |
| answer_date | 債権譲渡回答日                                                                                          | date |


## 使用例

### リクエスト例

```json
{
  "user_id": "sample@robotpayment.co.jp",
  "access_key": "xxxxxxxxxxxxxxxx",
  "limit_count": 20,
  "page_count": 0,
  "marunage_bill_credit":
    {
      "bill_number": "202401-sample-1"
    }
}
```

### レスポンス例

Status: 200 OK

```json
{
  "user_id": "sample@robotpayment.co.jp",
  "limit_count": 20,
  "page_count": 0,
  "total_page_count": 1,
  "request_marunage_credit": [
    {
      "error_code": null,
      "error_message": null,
      "bill_number": "202401-sample-1"
      "status":1,
      "apply_date":"2024-10-24",
      "answer_date":null
    }
  ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

[その他の特殊なエラーコード](../../index.md#その他の特殊なエラーコード)

個別エラー

| エラーコード | 内容                                               |
|--------| ----------------------------------------------------- |
| 6115   | 請求書番号が不正                                         |
| 6116   | 対象のまるなげ請求書が存在しません                          |

----

[TOPへ戻る](../../index.md)