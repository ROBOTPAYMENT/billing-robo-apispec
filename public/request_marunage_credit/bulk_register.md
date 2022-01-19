# まるなげ与信申請登録API

`/api/v1.0/request_marunage_credit/bulk_register`

指定の請求先に対してまるなげ与信申請登録を実行します。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/v1.0/request_marunage_credit/bulk_register`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                  | 概要                                                                             | 桁数 | 種別                             | 必須 |
| --------------------- | ------------------------------------------------------------------------------- | ---- | -------------------------------- | --- |
| user_id               | ユーザーID（管理画面へのログインID）                                               | 100  | [メール形式](../../index.md#種別) | 必須 |
| access_key            | アクセスキー                                                                     | 100  | [半角英数](../../index.md#種別)   | 必須 |
| marunage_credit       | まるなげ与信申請情報に属するパラメータ                                             |      | `array`                         |      |

#### marunage_credit (request)

下記のような項目のオブジェクトを持つリスト

| 名前                | 概要                                                | 桁数  | 種別                                  | 必須 |
| ------------------- | -------------------------------------------------- | ---- | -------------------------------------- | ---- |
| billing_code        | 請求先コード                                        | 20   | [半角英数 + 記号](../../index.md#種別)  | 必須  |
| representative_name | 代表者名                                            | 40   | 文字列                                 | 必須  |
| contract_date       | 契約日                                              | 10   | 日付                                   | 必須  |
| transaction_count   | 取引回数                                            | 4    | 数値                                   | 必須  |
| company_url         | 企業URL                                             | 200  | 文字列                                 | 必須  |



## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前              | 概要                                                                                                                               | 型       |
| ----------------- | --------------------------------------------------------------------------------------------------------------------------------- | -------- |
| user_id           | ユーザーID                                                                                                                         | string   |
| access_key        | アクセスキー                                                                                                                       | string   |
| marunage_credit   | まるなげ与信申請情報に属するパラメータ                                                                                               | `array`  |

#### marunage_credit (response)

下記のような項目のオブジェクトを持つリスト

| 名前                | 概要                                                | 型      |
| ------------------- | -------------------------------------------------- | ------- |
| error_code          | エラーコード<br> ※正常時はnull                      | int     |
| error_message       | エラーメッセージ<br> ※正常時はnull                  | string  |
| id                  | 申請ID                                             | int     |
| billing_code        | 請求先コード                                        | string  |
| representative_name | 代表者名                                            | string  |
| contract_date       | 契約日                                              | date    |
| transaction_count   | 取引回数                                            | int     |
| company_url         | 企業URL                                             | string  |
| status              | ステータス<br> 0: 却下<br> 1: 承認<br> 2: 申請中      | int     |
| requested_date      | 申請日時                                            | datetime |


## 使用例

### リクエスト例

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "marunage_credit": [
        {
            "billing_code": "billing_code_001",
            "representative_name": "代表者名1",
            "contract_date": "2022/01/01",
            "transaction_count": 12,
            "company_url": "https://www.sample1.co.jp"
        },
	{
            "billing_code": "billing_code_002",
            "representative_name": "代表者名2",
            "contract_date": "2022/01/01",
            "transaction_count": 24,
            "company_url": "https://www.sample2.co.jp"
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
            "id": 1,
	    "billing_code": "billing_code_001",
            "representative_name": "代表者名1",
            "contract_date": "2022/01/01",
            "transaction_count": 12,
            "company_url": "https://www.sample1.co.jp",
            "status": 2,
            "requested_date": "2022/01/02 00:00:00"
        },
        {
            "error_code": null,
            "error_message": null,
            "id": 2,
            "billing_code": "billing_code_002",
            "representative_name": "代表者名2",
            "contract_date": "2022/01/01",
            "transaction_count": 24,
            "company_url": "https://www.sample2.co.jp",
            "status": 2,
            "requested_date": "2022/01/02 00:00:00"
        }
    ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                                                      |
| ------------ | -------------------------------------------------------- |
| 5401         | 請求先が存在しません                                      |
| 5402         | まるなげオプション利用不可                                 |
| 5403         | 請求先コードが不正                                        |
| 5404         | 代表者名が不正                                            |
| 5405         | 契約日が不正                                              |
| 5406         | 取引回数が不正                                            |
| 5407         | 企業URLが不正                                             |
| 5408         | 契約日が未来の日付の為与信申請できません                     |
| 5409         | 電話番号が未登録の請求先部署がある為与信申請できません        |
| 5410         | 請求先部署が未承認の為与信申請できません                     |
| 5411         | 既に与信申請中又は請求先与信が有効になっています              |
| 5412         | 他の請求先与信の申請に失敗しました                           |
| 5413         | 同じ請求先に対して、２件以上のリクエストは出来ません          |
| 5414         | リクエストパラメータにmarunage_creditオブジェクトが存在しない |

----

[TOPへ戻る](../../index.md)