# 請求元銀行口座停止削除

`/api/v1.0/bs_bank_transfer/bulk_stop`

複数の請求元銀行口座を停止または削除する処理。

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/v1.0/bs_bank_transfer/bulk_stop`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                                        | 概要                                 | 桁数 | 種別                               | 必須 |
| ------------------------------------------- | ------------------------------------ | ---- | ---------------------------------- | ---- |
| user_id                                     | ユーザーID（管理画面へのログインID） | 100  | [半角英数\*1](/README.md#種別注釈) | 必須 |
| access_key                                  | アクセスキー                         | 100  | [半角英数\*3](/README.md#種別注釈) | 必須 |
| [bs_bank_transfer](#bsbanktransfer-request) | 請求元銀行口座に属するパラメータ     |      | `Array(bs_bank_transfer)`          |      |

#### bs_bank_transfer (request)

<!-- 要素が多くないものは detail, summaryタグを使わない (なくても見やすくため) -->

下記のような項目のオブジェクトを持つリスト

| 名前    | 概要                     | 桁数 | 種別                               | 必須 |
| ------- | ------------------------ | ---- | ---------------------------------- | ---- |
| code    | 銀行口座コード           | 20   | [半角英数\*4](/README.md#種別注釈) | 必須 |
| del_flg | 削除フラグ0: 停止 1:削除 | 1    | 数値                               | 必須 |


## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                                         | 概要                                 | 型                        |
| -------------------------------------------- | ------------------------------------ | ------------------------- |
| user_id                                      | ユーザーID（管理画面へのログインID） | string                    |
| access_key                                   | アクセスキー                         | string                    |
| [bs_bank_transfer](#bsbanktransfer-response) | 請求元銀行口座に属するパラメータ     | `Array(bs_bank_transfer)` |

#### bs_bank_transfer (response)

<!-- 要素が多くないものは detail, summaryタグを使わない (なくても見やすくため) -->

下記のような項目のオブジェクトを持つリスト

| 名前          | 概要                          | 型     |
| ------------- | ----------------------------- | ------ |
| error_code    | エラーコード※正常時はnull     | string |
| error_message | エラーメッセージ※正常時はnull | string |
| code          | 銀行口座コード                | string |
| title         | 銀行口座名                    | string |
| del_flg       | 削除フラグ0: 無効 1:削除      | int    |


## 使用例

### リクエスト

```
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    " bs_bank_transfer ": [
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
    "bs_bank_transfer": [
        {
            "error_code": null,
            "error_message": null,
            "code": "sample",
            "title": "サンプル",
            "del_flg": 0
        }
    ]
}
```

## エラー

| エラーコード | 内容                                                                   |
| ------------ | ---------------------------------------------------------------------- |
| 2301         | 銀行口座コードが不正                                                   |
| 2302         | 更新対象の銀行口座が存在しません                                       |
| 2303         | 削除フラグが不正                                                       |
| 2304         | すでに無効な口座です                                                   |
| 2305         | 請求元銀行口座を全て無効にできません                                   |
| 2306         | 請求元銀行口座を無効にできません。                                     |
| 2307         | 請求元銀行口座パターンで使用されている請求元銀行口座は無効にできません |