# まるなげ与信申請解除API

`/api/v1.0/request_marunage_credit/bulk_stop`

指定の請求先のまるなげ与信を解除（停止）します。既に与信が有効な請求先に対してのみ行えます。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/v1.0/request_marunage_credit/bulk_stop`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前            | 概要                               | 桁数 | 種別                             | 必須 |
| ----------------| ----------------------------------| ---- | -------------------------------- | --- |
| user_id         | ユーザーID（管理画面へのログインID） | 100  | [メール形式](../../index.md#種別) | 必須 |
| access_key      | アクセスキー                       | 100  | [半角英数](../../index.md#種別)   | 必須 |
| marunage_credit | まるなげ与信解除に属するパラメータ   |      | `array`                          |      |

#### marunage_credit (request)

下記のような項目のオブジェクトを持つリスト

| 名前         | 概要         | 桁数 | 種別                                  | 必須 |
| -------------| ------------| -----| ------------------------------------- | --- |
| billing_code | 請求先コード | 20   | [半角英数 + 記号](../../index.md#種別) | 必須 |



## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前            | 概要                             | 型      |
| ----------------| ------------------------------- | ------- |
| user_id         | ユーザーID                       | string  |
| access_key      | アクセスキー                     | string  |
| marunage_credit | まるなげ与信解除に属するパラメータ | `array` |

#### marunage_credit (response)

下記のような項目のオブジェクトを持つリスト

| 名前           | 概要                              | 型      |
| --------------| ----------------------------------| ------- |
| error_code    | エラーコード<br> ※正常時はnull     | int     |
| error_message | エラーメッセージ<br> ※正常時はnull | string  |
| billing_code  | 請求先コード                       | string  |


## 使用例

### リクエスト例

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "marunage_credit": [
        {
            "billing_code": "billing_code_001"
        },
	{
            "billing_code": "billing_code_002"
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
    "marunage_credit": [
        {
            "error_code": null,
            "error_message": null,
	    "billing_code": "billing_code_001"
        },
        {
            "error_code": null,
            "error_message": null,
            "billing_code": "billing_code_002"
        }
    ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                                                       |
| ------------ | --------------------------------------------------------- |
| 5501         | まるなげオプション利用不可                                  |
| 5502         | 請求先が存在しません                                       |
| 5503         | 請求先コードが不正                                         |
| 5504         | 対象のまるなげ与信が申請中又は停止中の為解除できません        |
| 5505         | まるなげ与信のデータが存在しません                          |
| 5506         | 同じ請求先に対して、２件以上のリクエストは出来ません          |
| 5507         | リクエストパラメータにmarunage_creditオブジェクトが存在しない |
| 5508         | 他のまるなげ与信の解除に失敗しました                         |

----

[TOPへ戻る](../../index.md)