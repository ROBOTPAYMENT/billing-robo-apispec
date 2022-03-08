# 請求元担当者登録更新

`/api/v1.0/bs_owner/bulk_upsert`

請求元担当者登録・更新をAPIで行う場合に利用します

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Path: `/api/v1.0/bs_owner/bulk_upsert`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                         | 概要                                 | 桁数 | 種別                              | 必須 |
| ---------------------------- | ------------------------------------ | ---- | --------------------------------- | ---- |
| user_id                      | ユーザーID（管理画面へのログインID） | 100  | [メール形式](../../index.md#種別) | 必須 |
| access_key                   | アクセスキー                         | 100  | [半角英数](../../index.md#種別)   | 必須 |
| [bs_owner](#bsowner-request) | 請求元担当者に属するパラメータ       |      | `array`                 |      |

#### bs_owner (request)

下記のような項目のオブジェクトを持つリスト

| 名前            | 概要                                                                  | 桁数 | 種別                                   | 必須 |
| --------------- | --------------------------------------------------------------------- | ---- | -------------------------------------- | ---- |
| code            | 担当者コード  ※登録後は変更できません                                 | 20   | [半角英数 + 記号](../../index.md#種別) | 必須 |
| name            | 担当者名                                                              | 40   | 文字列                                 | 必須 |
| post_name       | 役職名                                                                | 40   | 文字列                                 |      |
| department_code | 請求元部署コード <br> ※設定しない場合、「未所属」0000が設定されます。 | 40   | [半角英数 + 記号](../../index.md#種別) |      |


## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                          | 概要                           | 型                |
| ----------------------------- | ------------------------------ | ----------------- |
| user_id                       | ユーザーID                     | string            |
| access_key                    | アクセスキー                   | string            |
| [bs_owner](#bsowner-response) | 請求元担当者に属するパラメータ | `array` |

#### bs_owner (response)

下記のような項目のオブジェクトを持つリスト

| 名前            | 概要                                | 型     |
| --------------- | ----------------------------------- | ------ |
| error_code      | エラーコード <br> ※正常時はnull     | int    |
| error_message   | エラーメッセージ <br> ※正常時はnull | string |
| code            | 担当者コード                        | string |
| name            | 担当者名                            | string |
| post_name       | 役職名                              | string |
| department_code | 請求元部署コード                    | string |


## 使用例

### リクエスト例

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "bs_owner": [
        {
            "code": "1001",
            "name": "元担当者1001",
            "post_name": "部長",
            "department_code": "1001"
        },
        {
            "code": "1001",
            "name": "請求元担当者1001",
            "post_name": "部長",
            "department_code": "1001"
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
    "bs_owner": [
        {
            "error_code": null,
            "error_message": null,
            "code": "1001",
            "name": "元担当者1001",
            "post_name": "部長",
            "department_code": "1001"
        },
        {
            "error_code": null,
            "error_message": null,
            "code": "1001",
            "name": "請求元担当者1001",
            "post_name": "部長",
            "department_code": "1001"
        }
    ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                                      |
| ------------ | ---------------------------------------- |
| 3101         | 請求元担当者コードが不正                  |
| 3102         | 請求元担当者名が不正                      |
| 3103         | 役職名が不正                             |
| 3104         | 請求元部署コードが不正                    |

----

[TOPへ戻る](../../index.md)