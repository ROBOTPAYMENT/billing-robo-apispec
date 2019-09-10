# 商品停止削除(複数)

`/api/v1.0/goods/bulk_stop`

説明

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/v1.0/{}/{}`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前        | 概要                                 | 桁数 | 種別                               | 必須     |
| ----------- | ------------------------------------ | ---- | ---------------------------------- | -------- |
| user_id     | ユーザーID（管理画面へのログインID） | 100  | [半角英数\*1](/README.md#種別注釈) | 必須     |
| access_key  | アクセスキー                         | 100  | [半角英数\*3](/README.md#種別注釈) | 必須     |
| goods       | 商品に属するパラメータ               |      |                                    |          |
| item_number | 商品番号                             | 20   | 数値                               | (必須)^1 |
| item_code   | 商品コード                           | 20   | [半角英数\*4](/README.md#種別注釈) | (必須)^1 |
| del_flg     | 商品削除フラグ <br> 0:停止 1:削除    | 1    | 数値                               | 必須     |


## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前          | 概要                                | 型     |
| ------------- | ----------------------------------- | ------ |
| user_id       | ユーザーID                          | string |
| access_key    | アクセスキー                        | string |
| error_code    | エラーコード <br> ※正常時はnull     | string |
| error_message | エラーメッセージ <br> ※正常時はnull | string |
| goods         | 商品に属するパラメータ              |        |
| item_number   | 請求書番号                          | int    |
| item_code     | 請求先コード                        | string |
| del_flg       | 商品削除フラグ <br> 0:停止 1:削除   | Int    |


## 使用例

### リクエスト

```
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "goods": [
        {
            "item_number": 4,
            "del_flg": 1
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
    "bill":[
        {
            "error_code": null,
            "error_message": null,
            "item_number": 4,
            "item_code": null,
            "del_flg": 1
        }
    ]
}
```

## エラー

| エラーコード | 内容                                             |
| ------------ | ------------------------------------------------ |
| 1901         | 商品番号が不正                                   |
| 1902         | 商品コードが不正                                 |
| 1903         | 更新対象の商品が存在しません                     |
| 1904         | すでに無効な商品です。                           |
| 1905         | 商品無効化に失敗                                 |
| 1906         | 商品番号と商品コードは同時に指定できません。     |
| 1907         | 商品削除フラグが不正                             |
| 1908         | 請求情報で使用されている商品は削除できません。   |
| 1909         | 売上で使用されている商品は削除できません。       |
| 1910         | 請求書明細で使用されている商品は削除できません。 |