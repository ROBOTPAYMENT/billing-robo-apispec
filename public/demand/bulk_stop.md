# 請求情報停止削除

`/api/v1.0/demand/bulk_stop`

複数の請求情報を停止または削除する処理。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Path: `/api/v1.0/demand/bulk_stop`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                      | 概要                                 | 桁数 | 種別                              | 必須 |
| ------------------------- | ------------------------------------ | ---- | --------------------------------- | ---- |
| user_id                   | ユーザーID（管理画面へのログインID） | 100  | [メール形式](../../index.md#種別) | 必須 |
| access_key                | アクセスキー                         | 100  | [半角英数](../../index.md#種別)   | 必須 |
| [demand](#demand-request) | 請求情報に属するパラメータ           |      | `array`                   |      |

#### demand (request)

下記のような項目のオブジェクトを持つリスト

| 名前    | 概要                                      | 桁数 | 種別                                   | 必須     |
| ------- | ---------------------------------------- | ---- | -------------------------------------- | -------- |
| number  | 請求情報番号  <br> ※両端のスペース除去     | 18   | 数値                                   | (必須)^1 |
| code    | 請求情報コード  <br> ※両端のスペース除去   | 20   | [半角英数 + 記号](../../index.md#種別)  | (必須)^1 |
| del_flg | 請求情報削除フラグ <br> 0:停止 <br> 1:削除 | 1    | 数値                                   | 必須     |


## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                       | 概要                                 | 型              |
| -------------------------- | ------------------------------------ | --------------- |
| user_id                    | ユーザーID（管理画面へのログインID） | string          |
| access_key                 | アクセスキー                         | string          |
| [demand](#demand-response) | 請求情報に属するパラメータ           | `array` |

#### demand (response)

下記のような項目のオブジェクトを持つリスト

| 名前          | 概要                                | 型     |
| ------------- | ----------------------------------- | ------ |
| error_code    | エラーコード <br> ※正常時はnull     | int    |
| error_message | エラーメッセージ <br> ※正常時はnull | string |
| number        | 請求情報番号                        | int    |
| code          | 請求情報コード                      | string |
| del_flg       | 請求情報削除フラグ                  | int    |


## 使用例

### リクエスト例

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "demand": [
        {
            "number": 1,
            "del_flg": 0
        },
        {
            "number": 2,
            "del_flg": 1
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
    "demand": [
        {
            "error_code": null,
            "error_message": null,
            "number": 1,
            "code": "",
            "del_flg": 0
        },
        {
            "error_code": null,
            "error_message": null,
            "number": 2,
            "code": "",
            "del_flg": 1
        }
    ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                                               |
| ------------ | -------------------------------------------------- |
| 1401         | 請求情報番号が不正                                 |
| 1402         | 請求情報削除フラグが不正                           |
| 1403         | 請求情報コードが不正                               |
| 1404         | 請求情報番号と請求情報コードは同時に指定できません |
| 1405         | リクエストパラメータに請求情報が存在しない         |
| 1406         | 請求情報が存在しません                             |
| 1407         | 請求情報は既に停止しています                       |
| 1408         | 請求情報更新に失敗                                 |

----

[TOPへ戻る](../../index.md)