# 請求先停止削除(複数)

`/api/v1.0/billing/bulk_stop`

複数の請求先、請求先部署、請求先決済手段を停止または削除する処理

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/v1.0/billing/bulk_stop`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                        | 概要                                 | 桁数 | 種別                               | 必須 |
| --------------------------- | ------------------------------------ | ---- | ---------------------------------- | ---- |
| user_id                     | ユーザーID（管理画面へのログインID） | 100  | [半角英数\*1](/README.md#種別注釈) | 必須 |
| access_key                  | アクセスキー                         | 100  | [半角英数\*3](/README.md#種別注釈) | 必須 |
| [billing](#billing-request) | 請求先に属するパラメータ             |      | `Array(billing)`                   |      |


#### billing (request)

<details open>
<summary>クリックして隠す/表示</summary>

下記のような項目のオブジェクトを持つリスト

| 名前                                     | 概要                                                                                                 | 桁数 | 種別                               | 必須 |
| ---------------------------------------- | ---------------------------------------------------------------------------------------------------- | ---- | ---------------------------------- | ---- |
| code                                     | 請求先コード  <br> ※両端のスペース除去                                                               | 20   | [半角英数\*4](/README.md#種別注釈) | 必須 |
| del_flg                                  | 請求先削除フラグ <br> 0:停止 1:削除 <br> ※請求先部署のみまたは請求先決済手段のみを削除する時は、省略 | 1    | 数値                               |      |
| [biling.individual](#individual-request) | 請求先部署に属するパラメータ ※省略可能                                                               |      | `Array(individual)`                |      |
| [billing.payment](#payment-request)      | 請求先決済手段に属するパラメータ ※省略可能                                                           |      | `Array(payment)`                   |      |

#### individual (request)

<details open>
<summary>クリックして隠す/表示</summary>

下記のような項目のオブジェクトを持つリスト

| 名前    | 概要                                     | 桁数 | 種別                               | 必須                 |
| ------- | ---------------------------------------- | ---- | ---------------------------------- | -------------------- |
| number  | 請求先部署番号  <br> ※両端のスペース除去 | 18   | 数値                               | (請求先部署削除時)^1 |
| code    | 請求先部署コード  ※両端のスペース除去    | 20   | [半角英数\*4](/README.md#種別注釈) | (請求先部署削除時)^1 |
| del_flg | 請求先部署削除フラグ <br> 0:停止 1:削除  | 1    | 数値                               | (請求先部署削除時)   |

</details>


#### payment (request)

<details open>
<summary>クリックして隠す/表示</summary>

下記のような項目のオブジェクトを持つリスト

| 名前    | 概要                                  | 桁数 | 種別                               | 必須                   |
| ------- | ------------------------------------- | ---- | ---------------------------------- | ---------------------- |
| code    | 決済情報コード                        | 20   | [半角英数\*4](/README.md#種別注釈) | (請求先決済手段削除時) |
| del_flg | 決済情報削除フラグ <br> 0:停止 1:削除 | 1    | 数値                               | (請求先決済手段削除時) |

</details>
</details>


## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                         | 概要                                 | 型               |
| ---------------------------- | ------------------------------------ | ---------------- |
| user_id                      | ユーザーID（管理画面へのログインID） | string           |
| access_key                   | アクセスキー                         | string           |
| [billing](#billing-response) | 請求先に属するパラメータ             | `Array(billing)` |

#### billing (response)

<details open>
<summary>クリックして隠す/表示</summary>

下記のような項目のオブジェクトを持つリスト

| 名前                                       | 概要                                | 型                  |
| ------------------------------------------ | ----------------------------------- | ------------------- |
| error_code                                 | エラーコード <br> ※正常時はnull     | string              |
| error_message                              | エラーメッセージ <br> ※正常時はnull | string              |
| code                                       | 請求先コード                        | string              |
| del_flg                                    | 請求先削除フラグ                    | int                 |
| [billing.individual](#individual-response) | 請求先部署に属するパラメータ        | `Array(individual)` |
| [billing.payment](#payment-response)       | 請求先決済手段に属するパラメータ    | `Array(payment)`    |

#### individual (response)

<details open>
<summary>クリックして隠す/表示</summary>

下記のような項目のオブジェクトを持つリスト

| 名前          | 概要                                | 型     |
| ------------- | ----------------------------------- | ------ |
| error_code    | エラーコード <br> ※正常時はnull     | string |
| error_message | エラーメッセージ <br> ※正常時はnull | string |
| number        | 請求先部署番号                      | int    |
| code          | 請求先部署コード                    | string |
| del_flg       | 請求先部署削除フラグ                | int    |

</details>

#### payment (response)

<details open>
<summary>クリックして隠す/表示</summary>

下記のような項目のオブジェクトを持つリスト

| 名前          | 概要                                | 型     |
| ------------- | ----------------------------------- | ------ |
| error_code    | エラーコード <br> ※正常時はnull     | string |
| error_message | エラーメッセージ <br> ※正常時はnull | string |
| code          | 決済情報コード                      | string |
| del_flg       | 決済情報削除フラグ                  | int    |

</details>
</details>


## 使用例

### リクエスト

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "billing": [
        {
            "code": "billing",
            "individual": [
                {
                    "code": "abc-12345",
                    "del_lg": 0
                }
            ],
            "payment": [
                {
                    "code": "bank-123",
                    "del_flg": 0
                },
                {
                    "code": "bank-456",
                    "del_flg": 1
                }
            ]
        }
    ]
}
```

### レスポンス

Status: 200 OK

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "billing": [
        {
            "error_code": null,
            "error_message": null,
            "code": "billing",
            "individual": [
                {
                    "error_code": null,
                    "error_message": null,
                    "number": 1,
                    "code": "abc-12345",
                    "del_flg": 0
                }
            ],
            "payment": [
                {
                    "error_code": null,
                    "error_message": null,
                    "code": "bank-123",
                    "del_flg": 0
                },
                {
                    "error_code": null,
                    "error_message": null,
                    "code": "bank-456",
                    "del_flg": 1
                }
            ]
        }
    ]
}
```

## エラー

[共通エラー](/README.md#共通エラー)

個別エラー

| エラーコード | 内容                                     |
| ------------ | ---------------------------------------- |
| 901          | リクエストパラメータに請求先が存在しない |
| 902          | 請求先が存在しない                       |
| 903          | 請求先が既に停止されている               |
| 904          | 請求先コードが不正                       |
| 905          | 登録ユーザーIDが不正                     |
| 906          | 請求先停止に失敗                         |
| 907          | 請求先部署が存在しない                   |
| 908          | 請求先部署が既に停止されている           |
| 909          | 請求先に紐づく請求先部署がなくなる       |
| 910          | 請求先部署停止に失敗                     |
| 911          | 請求先部署コードが不正                   |