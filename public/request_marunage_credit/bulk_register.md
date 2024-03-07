# まるなげ与信申請登録

`/api/v1.0/request_marunage_credit/bulk_register`

指定の請求先に対してまるなげ与信申請登録を実行します。リクエスト上限は3000件です。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Path: `/api/v1.0/request_marunage_credit/bulk_register`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                    | 概要                                                                             | 桁数 | 種別                             | 必須 |
| ----------------------- | ------------------------------------------------------------------------------- | ---- | -------------------------------- | --- |
| user_id                 | ユーザーID（管理画面へのログインID）                                               | 100  | [メール形式](../../index.md#種別) | 必須 |
| access_key              | アクセスキー                                                                     | 100  | [半角英数](../../index.md#種別)   | 必須 |
| request_marunage_credit | まるなげ与信申請情報に属するパラメータ                                             |      | `array`                         |      |

#### request_marunage_credit (request)

下記のような項目のオブジェクトを持つリスト

| 名前                     | 概要                                                | 桁数  | 種別                                 | 必須 |
| ------------------------ | -------------------------------------------------- | ---- | ------------------------------------- | ---- |
| billing_code             | 請求先コード                                        | 20   | [半角英数 + 記号](../../index.md#種別) | 必須  |
| business_type            | 事業体区分                                           | 1   | 数値                                  | 必須  |
| corporate_number         | 法人番号<br> 1: 法人<br> 2: 個人       　            | 13   | 数値                                  | (申請時でbusiness_type=1時)  |
| representative_name      | 代表者名                                            | 40   | 文字列                                | 必須  |
| contract_date            | 契約日                                              | 10   | 日付                                  | 必須  |
| transaction_count        | 取引回数                                            | 4    | 数値                                  |       |
| company_url              | 企業URL                                             | 200  | 文字列                                | 必須  |
| estimated_billing_amount | 請求予定金額                                         | 15   | 数値                                  | 必須  |



## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                    | 概要                                                                                                                               | 型       |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- | -------- |
| user_id                 | ユーザーID                                                                                                                         | string   |
| request_marunage_credit | まるなげ与信申請情報に属するパラメータ                                                                                               | `array`  |

#### request_marunage_credit (response)

下記のような項目のオブジェクトを持つリスト

| 名前                     | 概要                                                    | 型       |
| ------------------------ | ------------------------------------------------------ | -------- |
| error_code               | エラーコード<br> ※正常時はnull                          | int      |
| error_message            | エラーメッセージ<br> ※正常時はnull                      | string   |
| id                       | 申請ID                                                 | int      |
| billing_code             | 請求先コード                                            | string   |
| business_type            | 事業体区分<br> 1: 法人<br> 2: 個人                        | int   |
| corporate_number         | 法人番号                                                | int   |
| representative_name      | 代表者名                                                | string   |
| contract_date            | 契約日                                                  | date     |
| transaction_count        | 取引回数                                                | int      |
| company_url              | 企業URL                                                 | string   |
| estimated_billing_amount | 請求予定金額                                             | int      |
| status                   | まるなげ与信申請状態<br> 0: 却下<br> 1: 承認<br> 2: 申請中 | int      |
| requested_date           | 申請日時                                                 | datetime |


## 使用例

### リクエスト例

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "request_marunage_credit": [
        {
            "billing_code": "billing_code_001",
			"business_type": 1,
			"corporate_number":1234567891234, 
            "representative_name": "代表者名1",
            "contract_date": "2022/01/01",
            "transaction_count": 12,
            "company_url": "https://www.sample1.co.jp",
            "estimated_billing_amount": 10000
        },
	{
            "billing_code": "billing_code_002",
			"business_type": 2,
			"corporate_number":"", 
            "representative_name": "代表者名2",
            "contract_date": "2022/01/01",
            "transaction_count": 24,
            "company_url": "https://www.sample2.co.jp",
	    "estimated_billing_amount": 50000
        }
    ]
}
```

### レスポンス例

Status: 200 OK

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "request_marunage_credit": [
        {
            "error_code": null,
            "error_message": null,
            "id": 1,
	    "billing_code": "billing_code_001",
            "business_type": 1,
            "corporate_number":1234567891234, 
            "representative_name": "代表者名1",
            "contract_date": "2022/01/01",
            "transaction_count": 12,
            "company_url": "https://www.sample1.co.jp",
            "estimated_billing_amount": 10000,
            "status": 2,
            "requested_date": "2022/01/02 00:00:00"
        },
        {
            "error_code": null,
            "error_message": null,
            "id": 2,
            "billing_code": "billing_code_002",
            "business_type": 2,
            "corporate_number":"", 
            "representative_name": "代表者名2",
            "contract_date": "2022/01/01",
            "transaction_count": 24,
            "company_url": "https://www.sample2.co.jp",
            "estimated_billing_amount": 50000,
            "status": 2,
            "requested_date": "2022/01/02 00:00:00"
        }
    ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

[その他の特殊なエラーコード](../../index.md#その他の特殊なエラーコード)

個別エラー

| エラーコード | 内容                                                           |
| ------------ | ------------------------------------------------------------ |
| 5401         | 請求先が存在しません                                           |
| 5402         | 請求先コードが不正                                             |
| 5403         | 代表者名が不正                                                |
| 5404         | 契約日が不正                                                  |
| 5405         | 取引回数が不正                                                |
| 5406         | 企業URLが不正                                                 |
| 5408         | 電話番号が未登録の請求先部署がある為まるなげ与信申請登録できません |
| 5409         | 請求先部署が未承認の為まるなげ与信申請登録できません              |
| 5410         | 既に与信申請中又はまるなげ与信が有効になっています                |
| 5411         | 他のまるなげ与信申請の登録に失敗しました                         |
| 5412         | 同じ請求先に対して２件以上のリクエストはできません                |
| 5413         | リクエストパラメータにまるなげ与信申請オブジェクトがない          |
| 5414         | 請求予定金額が不正                                             |

----

[TOPへ戻る](../../index.md)