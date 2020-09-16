# 請求先停止(複数)

`/api/v1.0/custom_field/bulk_stop`

複数のカスタム項目削除処理を実行します。 <br> 削除後の取消は出来ません。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト

- Method URL: `https://billing-robo.jp:10443/api/v1.0/custom_field/bulk_stop`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                                  | 概要                                   | 桁数 | 種別                              | 必須 |
| ------------------------------------- | -------------------------------------- | ---- | --------------------------------- | ---- |
| user_id                               | ユーザー ID（管理画面へのログイン ID） | 100  | [メール形式](../../index.md#種別) | 必須 |
| access_key                            | アクセスキー                           | 100  | [半角英数](../../index.md#種別)   | 必須 |
| [custom_field](#custom_field-request) | カスタム項目に属するパラメータ         |      | `array`                           |      |

#### custom_field (request)

下記のような項目のオブジェクトを持つリスト

| 名前   | 概要               | 桁数 | 種別                                   | 必須     | 一致     |
| ------ | ------------------ | ---- | -------------------------------------- | -------- | -------- |
| number | カスタム項目番号   | 18   | 数値                                   | (必須)^1 | 完全一致 |
| code   | カスタム項目コード | 20   | [半角英数 + 記号](../../index.md#種別) | (必須)^1 | 完全一致 |

## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                                   | 概要                                   | 型      |
| -------------------------------------- | -------------------------------------- | ------- |
| user_id                                | ユーザー ID（管理画面へのログイン ID） | string  |
| access_key                             | アクセスキー                           | string  |
| [custom_field](#custom_field-response) | カスタム項目に属するパラメータ         | `array` |

#### custom_field (response)

下記のような項目のオブジェクトを持つリスト

| 名前          | 概要                                       | 型     |
| ------------- | ------------------------------------------ | ------ |
| error_code    | エラーコード <br> ※エラー時以外は NULL     | string |
| error_message | エラーメッセージ <br> ※エラー時以外は NULL | string |
| number        | カスタム項目番号                           | int    |
| code          | カスタム項目コード                         | string |

## 使用例

### リクエスト例

```json
{
  "user_id": "sample@robotpayment.co.jp",
  "access_key": "xxxxxxxxxxxxxxxx",
  "custom_field": [
    {
      "number": 1
    },
    {
      "code": "sample_code2"
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
    "custom_field": [
      {
        "error_code": null,
        "error_message": null,
        "number": 1,
        "code": "sample_code1"
      },
      {
        "error_code": null,
        "error_message": null,
        "number": 2,
        "code": "sample_code2"
      }
    ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                                                             |
| ------------ | ---------------------------------------------------------------- |
| 4901         | カスタム項目番号が不正                                           |
| 4902         | カスタム項目コードが不正                                         |
| 4903         | カスタム項目番号とカスタム項目コードを同時に指定できません        |
| 4904         | リクエストパラメータにがカスタム項目情報が存在しません           |
| 4905         | カスタム項目が存在しません                                       |
| 4906         | 請求書未発行の登録済みカスタム項目は削除できません               |
| 4907         | 承認中の請求情報にカスタム項目が登録されているので削除できません |
| 4908         | リクエスト件数が上限を超えています                               |
| 4909         | カスタム項目情報には arrayを指定してください                    |
| 4910         | カスタム項目の削除に失敗しました                                 |

---

[TOP へ戻る](../../index.md)
