# 請求元担当者停止削除

`/api/v1.0/bs_owner/bulk_stop`

請求元担当者停止・削除をAPIで行う場合に利用します

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/v1.0/bs_owner/bulk_stop`
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

| 名前    | 概要                          | 桁数 | 種別                                   | 必須 |
| ------- | ----------------------------- | ---- | -------------------------------------- | ---- |
| code    | 担当者コード                  | 20   | [半角英数 + 記号](../../index.md#種別) | 必須 |
| del_flg | 削除フラグ <br> 0:無効 1:有効 | 1    | 数値                                   | 必須 |


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

| 名前          | 概要                                | 型     |
| ------------- | ----------------------------------- | ------ |
| error_code    | エラーコード <br> ※正常時はnull     | string |
| error_message | エラーメッセージ <br> ※正常時はnull | int    |
| code          | 担当者コード                        | string |
| name          | 担当者名                            | string |
| del_flg       | 削除フラグ <br> 0:無効 1:削除       | int    |


## 使用例

### リクエスト例

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "bs_owner": [
        {
            "code": "1001",
            "del_flg": "0"
        },
        {
            "code": "1001",
            "del_flg": "1"
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
            "name": "請求元担当者1001",
            "del_flg": 0
        },
        {
            "error_code": null,
            "error_message": null,
            "code": "1001",
            "name": "請求元担当者1001",
            "del_flg": 1
        }
    ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                                     |
| ------------ | ---------------------------------------- |
| 3201         | 請求元担当者コードが不正                 |
| 3202         | 削除フラグが不正                         |
| 3203         | 更新対象の請求元担当者が存在しません     |
| 3204         | すでに無効な担当者です                   |
| 3205         | 繰越予約中の請求元担当者は更新できません |

----

[TOPへ戻る](../../index.md)