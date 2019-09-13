# 請求書無効

`/api/v1.0/bill/stop`

請求書の無効化処理を実行します。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/v1.0/bill/stop`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                  | 概要                                 | 桁数 | 種別                          | 必須 |
| --------------------- | ------------------------------------ | ---- | ----------------------------- | ---- |
| user_id               | ユーザーID（管理画面へのログインID） | 100  | [メール形式](/README.md#種別) | 必須 |
| access_key            | アクセスキー                         | 100  | [半角英数](/README.md#種別)   | 必須 |
| [bill](#bill-request) | 請求書に属するパラメータ             |      | `array`                 |      |

#### bill (request)

下記のような項目のオブジェクトを持つリスト

| 名前         | 概要         | 桁数 | 種別                               | 必須 |
| ------------ | ------------ | ---- | ---------------------------------- | ---- |
| number       | 請求書番号   | 100  | [半角英数 + 記号](/README.md#種別) | 必須 |
| billing_code | 請求先コード | 20   | [半角英数 + 記号](/README.md#種別) | 必須 |


## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                   | 概要                     | 型            |
| ---------------------- | ------------------------ | ------------- |
| user_id                | ユーザーID               | string        |
| access_key             | アクセスキー             | string        |
| [bill](#bill-response) | 請求書に属するパラメータ | `array` |

#### bill (response)

下記のような項目のオブジェクトを持つリスト

| 名前          | 概要                                | 型     |
| ------------- | ----------------------------------- | ------ |
| error_code    | エラーコード <br> ※正常時はnull     | string |
| error_message | エラーメッセージ <br> ※正常時はnull | string |
| number        | 請求書番号                          | string |
| billing_code  | 請求先コード                        | string |


## 使用例

### リクエスト例

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "bill": [
        {
            "number": "201705-billing-1",
            "billing_code": "billing"
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
    "bill":[
        {
            "error_code": null,
            "error_message": null,
            "number": "201705-billing-1",
            "billing_code": "billing"
        }
    ]
}

```

## エラー

[共通エラー](/README.md#共通エラー)

個別エラー

| エラーコード | 内容                                                                   |
| ------------ | ---------------------------------------------------------------------- |
| 1701         | 請求書番号が不正                                                       |
| 1702         | 請求先コードが不正                                                     |
| 1703         | 更新対象の請求書が存在しません                                         |
| 1704         | この請求書の無効化はできません。(請求書が子請求書又は繰越請求書の場合) |
| 1705         | 一部または全て消込済みのため、無効に変更できません。                   |
| 1706         | すでに無効な請求書です。                                               |
| 1707         | 承認依頼中の請求書は変更できません。                                   |
| 1708         | 既に締められた売上が存在するため編集できません。                       |
| 1709         | 請求書無効化に失敗                                                     |