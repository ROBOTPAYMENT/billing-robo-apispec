# 入金無効削除(複数)

`/api/v1.0/payment/bulk_stop`

入金情報の無効、削除処理を実行します。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト

- Method URL: `https://billing-robo.jp:10443/api/v1.0/payment/bulk_stop`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                        | 概要                                   | 桁数 | 種別                              | 必須 |
| --------------------------- | -------------------------------------- | ---- | --------------------------------- | ---- |
| user_id                     | ユーザー ID（管理画面へのログイン ID） | 100  | [メール形式](../../index.md#種別) | 必須 |
| access_key                  | アクセスキー                           | 100  | [半角英数](../../index.md#種別)   | 必須 |
| [payment](#payment-request) | 入金情報に属するパラメータ             |      | `array`                           |      |

#### payment (request)

下記のような項目のオブジェクトを持つリスト

| 名前       | 概要                                       | 桁数 | 種別 | 必須 |
| ---------- | ------------------------------------------ | ---- | ---- | ---- |
| payment_id | 入金 ID                                    | 18   | 数値 | 必須 |
| del_flg    | 入金情報削除フラグ <br> 0:停止 <br> 1:削除 | 1    | 数値 | 必須 |

## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                         | 概要                       | 型      |
| ---------------------------- | -------------------------- | ------- |
| user_id                      | ユーザー ID                | string  |
| access_key                   | アクセスキー               | string  |
| [payment](#payment-response) | 入金情報に属するパラメータ | `array` |

#### payment (response)

下記のような項目のオブジェクトを持つリスト

| 名前          | 概要                                       | 型     |
| ------------- | ------------------------------------------ | ------ |
| error_code    | エラーコード <br> ※正常時は null           | string |
| error_message | エラーメッセージ <br> ※正常時は null       | string |
| payment_id    | 入金 ID                                    | int    |
| del_flg       | 入金情報削除フラグ <br> 0:停止 <br> 1:削除 | int    |

## 使用例

### リクエスト例

```json
{
  "user_id": "sample@robotpayment.co.jp",
  "access_key": "xxxxxxxxxxxxxxxx",
  "payment": [
    {
      "payment_id": 1,
      "del_flg": 0
    },
    {
      "payment_id": 2,
      "del_flg": 1
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
  "payment": [
    {
      "error_code": null,
      "error_message": null,
      "payment_id": 1,
      "del_flg": 0
    },
    {
      "error_code": null,
      "error_message": null,
      "payment_id": 2,
      "del_flg": 1
    }
  ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                                               |
| ------------ | -------------------------------------------------- |
| 3701         | 入金 ID が不正                                     |
| 3702         | 入金情報削除フラグが不正                           |
| 3703         | リクエストパラメータに入金情報が存在しません。     |
| 3704         | 入金情報が存在しません。                           |
| 3705         | 入金情報は既に無効化しています。                   |
| 3706         | 入金情報は既に削除しています。                     |
| 3707         | 入金消込ステータスが完了しています。               |
| 3708         | その他口座振替は無効に変更できません。             |
| 3709         | 一部または全て消込済みの入金情報は変更できません。 |
| 3710         | 仮受にした入金情報は変更できません。               |
| 3711         | 処理中の入金情報は変更できません。                 |
| 3712         | 承認中の入金情報は変更できません。                 |
| 3713         | 入金情報更新に失敗しました。                       |

---

[TOP へ戻る](../../index.md)
