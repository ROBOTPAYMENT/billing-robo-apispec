# 請求元部署停止削除

`/api/v1.0/bs_department/bulk_stop`

請求元部署停止・削除をAPIで行う場合に利用します

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/v1.0/bs_department/bulk_stop`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                                   | 概要                                 | 桁数 | 種別                              | 必須 |
| -------------------------------------- | ------------------------------------ | ---- | --------------------------------- | ---- |
| user_id                                | ユーザーID（管理画面へのログインID） | 100  | [メール形式](../../index.md#種別) | 必須 |
| access_key                             | アクセスキー                         | 100  | [半角英数](../../index.md#種別)   | 必須 |
| [bs_department](#bsdepartment-request) | 請求元部署に属するパラメータ         |      | `array`            |      |

#### bs_department (request)

下記のような項目のオブジェクトを持つリスト

| 名前    | 概要                          | 桁数 | 種別                                   | 必須 |
| ------- | ----------------------------- | ---- | -------------------------------------- | ---- |
| code    | 部署コード                    | 40   | [半角英数 + 記号](../../index.md#種別) | 必須 |
| del_flg | 削除フラグ <br> 0:無効 1:削除 | 1    | 数値                                   | 必須 |


## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                                    | 概要                         | 型                     |
| --------------------------------------- | ---------------------------- | ---------------------- |
| user_id                                 | ユーザーID                   | string                 |
| access_key                              | アクセスキー                 | string                 |
| [bs_department](#bsdepartment-response) | 請求元部署に属するパラメータ | `array` |

#### bs_department (response)

| 名前          | 概要                                | 型     |
| ------------- | ----------------------------------- | ------ |
| error_code    | エラーコード <br> ※正常時はnull     | string |
| error_message | エラーメッセージ <br> ※正常時はnull | string |
| code          | 部署コード                          | string |
| name          | 部署名                              | string |
| del_flg       | 削除フラグ <br> 0:無効 1:削除       | int    |


## 使用例

### リクエスト例

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "bs_department": [
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
    "bs_departmentl": [
        {
            "error_code": null,
            "error_message": null,
            "code": "1001",
            "name": "元部署1001",
            "del_flg": 0
        },
        {
            "error_code": null,
            "error_message": null,
            "code": "1001",
            "name": "元部署1001",
            "del_flg": 1
        }
    ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                               |
| ------------ | ---------------------------------- |
| 3001         | 請求元部署コードが不正             |
| 3002         | 削除フラグが不正                   |
| 3003         | 更新対象の請求元部署が存在しません |
| 3004         | 未所属の請求元部署は更新できません |
| 3005         | すでに無効な部署です               |

----

[TOPへ戻る](../../index.md)