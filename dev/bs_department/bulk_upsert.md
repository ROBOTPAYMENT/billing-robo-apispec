# 請求元部署登録更新

`/api/v1.0/bs_department/bulk_upsert`

請求元部署登録・更新をAPIで行う場合に利用します

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/v1.0/bs_department/bulk_upsert`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

|名前|概要|桁数|種別|
|---|---|---|---|
|user_id|ユーザーID（管理画面へのログインID） <br> ※必須|100|半角英数*1|
|access_key|アクセスキー <br> ※必須|100|半角英数*3|
|bs_department|請求元部署に属するパラメータ|||
|code|部署コード <br> ※必須 ※登録後は変更できません|40|半角英数*4|
|name|部署名 <br> ※必須|40|文字列|
|journal_cooperation_code|会計ソフト連携用部署コード <br> ※仕訳オプションがONのときのみ設定可能|25|文字列|


## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

|名前|概要|型|
|---|---|---|
|user_id|ユーザーID|string|
|access_key|アクセスキー|string|
|bs_department|請求元部署に属するパラメータ||
|error_code|エラーコード <br> ※正常時はnull|string|
|error_message|エラーメッセージ <br> ※正常時はnull|string|
|code|部署コード|string|
|name|部署名|string|
|journal_cooperation_code|仕訳連携用部署コード|string|


## 使用例

### リクエスト

```
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "bs_department": [
        {
            "code": "1001",
            "name": "元部署1001",
            "journal_cooperation_code": "1001"
        },
        {
            "code": "1001",
            "name": "請求元部署1001",
            "journal_cooperation_code": "1001"
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
    "bs_department": [
        {
            "error_code": null,
            "error_message": null,
            "code": "1001",
            "name": "元部署1001",
            "journal_cooperation_code": "1001"
        },
        {
            "error_code": null,
            "error_message": null,
            "code": "1001",
            "name": "請求元部署1001",
            "journal_cooperation_code": "1001"
        }
    ]
}
```

## エラー

| エラーコード | 内容                               |
| ------------ | ---------------------------------- |
| 2901         | 請求元部署コードが不正             |
| 2902         | 請求元部署名が不正                 |
| 2903         | 会計ソフト連携用部署コードが不正   |
| 2904         | 未所属の請求元部署は更新できません |
| 2905         | 仕訳オプションがオフになっています |