# 請求元担当者停止削除

`/api/v1.0/bs_owner/bulk_stop`

請求元担当者停止・削除をAPIで行う場合に利用します

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/v1.0/bs_owner/bulk_stop`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前       | 概要                                            | 桁数 | 種別       |
| ---------- | ----------------------------------------------- | ---- | ---------- |
| user_id    | ユーザーID（管理画面へのログインID） <br> ※必須 | 100  | 半角英数*1 |
| access_key | アクセスキー <br> ※必須                         | 100  | 半角英数*3 |
| bs_owner   | 請求元担当者に属するパラメータ                  |      |            |
| code       | 担当者コード <br> ※必須                         | 20   | 半角英数*4 |
| del_flg    | 削除フラグ <br> 0:無効 1:有効 <br> ※必須        | 1    | 数値       |


## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前          | 概要                                | 型     |
| ------------- | ----------------------------------- | ------ |
| user_id       | ユーザーID                          | string |
| access_key    | アクセスキー                        | string |
| bs_owner      | 請求元担当者に属するパラメータ      |        |
| error_code    | エラーコード <br> ※正常時はnull     | string |
| error_message | エラーメッセージ <br> ※正常時はnull | string |
| code          | 担当者コード                        | string |
| name          | 担当者名                            | string |
| del_flg       | 削除フラグ <br> 0:無効 1:削除       | int    |


## 使用例

### リクエスト

```
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

### レスポンス

Status: 200 OK

```
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

| エラーコード | 内容                                     |
| ------------ | ---------------------------------------- |
| 3201         | 請求元担当者コードが不正                 |
| 3202         | 削除フラグが不正                         |
| 3203         | 更新対象の請求元担当者が存在しません     |
| 3204         | すでに無効な担当者です                   |
| 3205         | 繰越予約中の請求元担当者は更新できません |
