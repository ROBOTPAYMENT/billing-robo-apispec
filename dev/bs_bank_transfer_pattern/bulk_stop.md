# 請求元銀行口座パターン登録更新

`/api/v1.0/bs_bank_transfer_pattern/bulk_stop`

複数の請求元銀行口座パターンを停止または削除する処理。

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/v1.0/bs_bank_transfer_pattern/bulk_stop`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                     | 概要                                     | 桁数 | 種別       | 必須 |
| ------------------------ | ---------------------------------------- | ---- | ---------- | ---- |
| user_id                  | ユーザーID（管理画面へのログインID）     | 100  | 半角英数*1 | 必須 |
| access_key               | アクセスキー                             | 100  | 半角英数*3 | 必須 |
| bs_bank_transfer_pattern | 請求元銀行口座パターンに属するパラメータ |      |            |      |
| code                     | 銀行口座パターンコード                   | 20   | 半角英数*4 | 必須 |
| del_flg                  | 削除フラグ0: 停止 1:削除                 | 1    | 数値       | 必須 |


## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                     | 概要                                     | 型     |
| ------------------------ | ---------------------------------------- | ------ |
| user_id                  | ユーザーID（管理画面へのログインID）     | string |
| access_key               | アクセスキー                             | string |
| bs_bank_transfer_pattern | 請求元銀行口座パターンに属するパラメータ |        |
| error_code               | エラーコード※正常時はnull                | string |
| error_message            | エラーメッセージ※正常時はnull            | string |
| code                     | 銀行口座パターンコード                   | string |
| name                     | 銀行口座パターン管理名                   | string |
| del_flg                  | 削除フラグ0: 停止 1:削除                 | int    |


## 使用例

### リクエスト

```
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "bs_bank_transfer_pattern": [
        {
            "code": "sample",
            "del_flg": 0
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
    "bs_bank_transfer_pattern": [
        {
            "error_code": null,
            "error_message": null,
            "code": "sample",
            "name": "サンプル",
            "del_flg": 0
        }
    ]
}
```

## エラー

| エラーコード | 内容                                                                                     |
| ------------ | ---------------------------------------------------------------------------------------- |
| 2501         | 銀行口座パターンコードが不正                                                             |
| 2502         | 銀行口座パターン削除フラグが不正                                                         |
| 2504         | 更新対象の請求元銀行口座パターンが存在しません                                           |
| 2505         | 銀行口座パターンはすでに停止済です                                                       |
| 2506         | 銀行口座パターンを全て無効にできません                                                   |
| 2507         | 請求先利用可能決済手段で使用されている銀行口座パターンは無効にできません                 |
| 2508         | 承認中の請求先利用可能決済手段で使用されている請求元銀行口座パターンは無効にできません。 |
