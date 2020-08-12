# 消込

`/api/v1.0/clearing/exec`

消込処理を実行します。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト

- Method URL: `https://billing-robo.jp:10443/api/v1.0/clearing/exec`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                          | 概要                                   | 桁数 | 種別                               | 必須 |
| ----------------------------- | -------------------------------------- | ---- | ---------------------------------- | ---- |
| user_id                       | ユーザー ID（管理画面へのログイン ID） | 100  | [メール形式](../../index.md#種別)  | 必須 |
| access_key                    | アクセスキー                           | 100  | [半角英数](../../index.md#種別)    | 必須 |
| [clearing](#clearing-request) | 消込に属するパラメータ                 |      | `object`                           | 必須 |

#### clearing (request)

下記のような項目のオブジェクトを持つリスト

| 名前                        | 概要                     | 桁数 | 種別     | 必須 |
| --------------------------- | ------------------------ | ---- | -------- | ---- |
| [payment](#payment-request) | 入金に属するパラメータ   |      | `object` |      |
| [bill](#bill-request)       | 請求書に属するパラメータ |      | `array`  | 必須 |

#### payment (request)

下記のような項目のオブジェクトを持つリスト

| 名前          | 概要                                                                                 | 桁数 | 種別 | 必須             |
| ------------- | ------------------------------------------------------------------------------------ | ---- | ---- | ---------------- |
| payment_id    | 入金 ID                                                                              | 18   | 数値 | (payment 指定時) |
| bank_save_flg | 口座名義学習フラグ <br> 0:学習なし <br> 1:学習あり <br> ※追加省略時、0 で登録される | 1    | 数値 |                  |

#### bill (request)

下記のような項目のオブジェクトを持つリスト

| 名前           | 概要       | 桁数 | 種別                                   | 必須 |
| -------------- | ---------- | ---- | -------------------------------------- | ---- |
| number        | 請求書番号 | 100  | [半角英数 + 記号](../../index.md#種別) | 必須 |


## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                           | 概要                                   | 型       |
| ------------------------------ | -------------------------------------- | -------- |
| user_id                        | ユーザー ID（管理画面へのログイン ID） | string   |
| access_key                     | アクセスキー                           | string   |
| [clearing](#clearing-response) | 消込に属するパラメータ                 | `object` |

#### clearing (response)

下記のような項目のオブジェクトを持つリスト

| 名前                         | 概要                     | 型       |
| ---------------------------- | ------------------------ | -------- |
| [payment](#payment-response) | 入金に属するパラメータ   | `object` |
| [bill](#bill-response)       | 請求書に属するパラメータ | `array`  |

#### payment (response)

下記のような項目のオブジェクトを持つリスト

| 名前                         | 概要                                               | 型      |
| ---------------------------- | -------------------------------------------------- | ------- |
| error_code                   | エラーコード <br> ※正常時は null                  | string  |
| error_message                | エラーメッセージ <br> ※正常時は null              | string  |
| payment_id                   | 入金 ID                                            | int     |
| bank_save_flg                | 口座名義学習フラグ <br> 0:学習なし <br> 1:学習あり | int     |
| clearing_amount              | 消込金額                                           | int     |
| unclearing_amount            | 未消込金額                                         | int     |
| [erasure](#erasure-response) | 消込結果に属するパラメータ                         | `array` |

#### bill (response)

下記のような項目のオブジェクトを持つリスト

| 名前                         | 概要                                  | 型      |
| ---------------------------- | ------------------------------------- | ------- |
| error_code                   | エラーコード <br> ※正常時は null     | string  |
| error_message                | エラーメッセージ <br> ※正常時は null | string  |
| number                       | 請求書番号                            | string   |
| clearing_amount              | 消込金額                              | int     |
| unclearing_amount            | 未消込金額                            | int     |
| [erasure](#erasure-response) | 消込結果に属するパラメータ            | `array` |

#### erasure (response)

下記のような項目のオブジェクトを持つリスト

| 名前       | 概要        | 型  |
| ---------- | ----------- | --- |
| erasure_id | 消込結果 ID | int |

## 使用例

### リクエスト例
入金と請求書の消込の場合
```json
{
  "user_id": "sample@robotpayment.co.jp",
  "access_key": "xxxxxxxxxxxxxxxx",
  "clearing": {
    "payment": {
      "payment_id": 1,
      "bank_save_flg": 1
    },
    "bill": [
      {
        "number": "202005-sample-1"
      },
      {
        "number": "202005-sample-2"
      }
    ]
  }
}
```

請求書同士（マイナス明細請求書との相殺）の消込の場合
```json
{
  "user_id": "sample@robotpayment.co.jp",
  "access_key": "xxxxxxxxxxxxxxxx",
  "clearing": {
    "bill": [
      {
        "number": "202005-sample-3"
      },
      {
        "number": "202005-sample-4"
      }
    ]
  }
}
```

### レスポンス例

Status: 200 OK<br>入金と請求書の消込の場合
```json
{
  "user_id": "sample@robotpayment.co.jp",
  "access_key": "xxxxxxxxxxxxxxxx",
  "clearing": {
    "error_code": null,
    "error_message": null,
    "payment": {
      "error_code": null,
      "error_message": null,
      "payment_id": 1,
      "bank_save_flg": 1,
      "clearing_amount": 1000,
      "unclearing_amount": 0,
      "erasure": [
        {
          "erasure_id": 1
        },
        {
          "erasure_id": 2
        }
      ]
    },
    "bill": [
      {
        "error_code": null,
        "error_message": null,
        "number": "202005-sample-1",
        "clearing_amount": 600,
        "unclearing_amount": 0,
        "erasure": [
          {
            "erasure_id": 1
          }
        ]
      },
      {
        "error_code": null,
        "error_message": null,
        "number": "202005-sample-2",
        "clearing_amount": 400,
        "unclearing_amount": 0,
        "erasure": [
          {
            "erasure_id": 2
          }
        ]
      }
    ]
  }
}
```

請求書同士（マイナス明細請求書との相殺）の消込の場合
```json
{
  "user_id": "sample@robotpayment.co.jp",
  "access_key": "xxxxxxxxxxxxxxxx",
  "clearing": {
    "error_code": null,
    "error_message": null,
    "payment": null,
    "bill": [
      {
        "error_code": null,
        "error_message": null,
        "number": "202005-sample-3",
        "clearing_amount": -600,
        "unclearing_amount": 0,
        "erasure": [
          {
            "erasure_id": 1
          }
        ]
      },
      {
        "error_code": null,
        "error_message": null,
        "number": "202005-sample-4",
        "clearing_amount": 600,
        "unclearing_amount": 0,
        "erasure": [
          {
            "erasure_id": 1
          }
        ]
      }
    ]
  }
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                                                       |
| ------------ | ---------------------------------------------------------- |
| 3801         | 請求書番号が不正                                           |
| 3802         | 請求書が存在しません。                                     |
| 3803         | この請求書は繰越済みです。                                 |
| 3804         | この請求書は繰越予約中です。                               |
| 3805         | 既に消込済み請求書のため消込はできません。                 |
| 3806         | 既に無効な請求書です。                                     |
| 3807         | 承認依頼中の請求書があります。                             |
| 3808         | 既に締められた売上が存在するため消込できません。           |
| 3809         | マイナス明細請求書もしくは入金データがありません。         |
| 3810         | プラス請求書もしくは入金データがありません。               |
| 3811         | 入金 ID が不正                                             |
| 3812         | 口座名義学習フラグが不正                                   |
| 3813         | 入金情報が存在しません。                                   |
| 3814         | 入金情報が複数あります。                                   |
| 3815         | 入金消込ステータスが完了もしくは確認済みの入金があります。 |
| 3816         | 既に無効な入金情報です。                                   |
| 3817         | 既に削除済みの入金情報です。                               |
| 3818         | 承認依頼中の入金情報があります。                           |
| 3819         | 既に締められた期間内の入金のため消込できません。           |
| 3820         | 処理中の入金のため変更できません。                         |
| 3821         | リクエスト件数が上限を超えています                          |
| 3822         | 消込に失敗しました。                                       |
| 3823         | 消込情報が存在しません。                                   |
| 3824         | 銀行振込とバンクチェック以外の入金で口座名義学習フラグ1:学習ありを指定しています。 |

---

[TOP へ戻る](../../index.md)
