# 請求書送付郵送

`/api/v1.0/bill/send_bill_by_mail`

請求書郵送をAPIで行う場合に利用します

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/v1.0/bill/send_bill_by_mail`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前       | 概要                                 | 桁数 | 種別       | 必須 |
| ---------- | ------------------------------------ | ---- | ---------- | ---- |
| user_id    | ユーザーID（管理画面へのログインID） | 100  | 半角英数*1 | 必須 |
| access_key | アクセスキー                         | 100  | 半角英数*3 | 必須 |
| bill       | 請求書に属するパラメータ             |      |            |      |
| number     | 請求書番号                           | 100  | 半角英数*4 |      |


## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前              | 概要                                | 型     |
| ----------------- | ----------------------------------- | ------ |
| user_id           | ユーザーID                          | string |
| access_key        | アクセスキー                        | string |
| bill              | 請求書に属するパラメータ            |        |
| error_code        | エラーコード <br> ※正常時はnull     | string |
| error_message     | エラーメッセージ <br> ※正常時はnull | string |
| number            | 請求書番号                          | string |
| mail_order_number | 郵送受付番号                        | int    |


## 使用例

### リクエスト

```
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "bill": [
        {
            "number": "201705-billing-1"
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
    "bill": [
        {
            "error_code": null,
            "error_message": null,
            "number": "201705-billing-1",
            "mail_order_number": 1
        }
    ]
}
```

## エラー

| エラーコード | 内容                           |
| ------------ | ------------------------------ |
| 2801         | 請求書郵送に失敗               |
| 2802         | 郵送済みの請求書は郵送できない |
| 2803         | 郵送機能が利用可能でない       |
| 2804         | 請求書番号が不正               |
| 2805         | 無効の請求書は送信できない     |
| 2806         | 請求書が承認依頼中             |
| 2807         | 請求先部署が承認依頼中         |
| 2808         | 枚数制限(6枚)オーバー          |
