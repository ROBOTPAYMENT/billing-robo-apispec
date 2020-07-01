# 消込取消

消込結果の取消処理を実行します。

`/api/v1.0/clearing/bulk_cancel`

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/v1.0/clearing/bulk_cancel`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                               | 概要                             | 桁数 | 種別                              | 必須     |
| ----------------------------- | ------------------------------------ | ---- | -------------------------------- | -------- |
| user_id                       | ユーザーID（管理画面へのログインID）    | 100  | [メール形式](../../index.md#種別) | 必須     |
| access_key                    | アクセスキー                          | 100  | [半角英数](../../index.md#種別)   | 必須     |
| [clearing](#clearing-request) | その他形式に属するパラメータ           |      | `array`                          | 必須      |

#### clearing (request)

取消計上日は請求書の消込ステータス変更による消込のみ指定できます。 <br> 取消計上日が指定無しの場合、消込計上日と同日になります。

下記のような項目のオブジェクトを持つリスト

| 名前                         | 概要       | 桁数        | 種別       | 必須   |
| ---------------------------- | --------- | ----------- | --------- | ------ |
| erasure_id                   | 消込結果ID | 18          | 数値      | 必須   |
| erasure_cancel_recorded_date | 取消計上日 | 10          | 日付      |        |

## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                         | 概要                                  | 型      |
| ---------------------------- | ------------------------------------ | ------- |
| user_id                      | ユーザーID                            | string  |
| access_key                   | アクセスキー                          | string  |
| [erasure](#erasure-response) | 消込結果に属するパラメータ             | `array` |

#### erasure (response)

下記のような項目のオブジェクトを持つリスト

| 名前                      | 概要                                                                                    | 型     |
| ------------------------- | --------------------------------------------------------------------------------------- | ------ |
| error_code                | エラーコード <br> ※正常時はnull                                                          | string |
| error_message             | エラーメッセージ <br> ※正常時はnull                                                      | string  |
| erasure_id                | 消込結果ID                                                                              | int     |
| payment_id                | 入金ID                                                                                  | int     |
| [bill](#bill-response)    | 請求書に関するパラメータ                                                                  | `array` |

#### bill (response)

下記のような項目のオブジェクトを持つリスト

| 名前   | 概要            | 型     |
| ------ | -------------- | ------ |
| number | 請求書番号      | string |

## 使用例

### リクエスト例

```json

{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "clearing": [
        {
            "erasure_id":12345,
            "erasure_cancel_recorded_date":"2020/06/01"
        },
        {
            "erasure_id":6789,
            "erasure_cancel_recorded_date":"2020/06/02"
        }
    ]
}
```

### レスポンス例
決済と請求書の消込取消の場合
Status: 200 OK

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "erusure": [
        {
            "error_code":null,
            "error_message":null,
            "erasure_id":12345,
            "payment_id":22345,
            "bill":[
                {
                  "number":"202005-sample-1"
                }
            ]
        },
        {
            "error_code":null,
            "error_message":null,
            "erasure_id":67890,
            "payment_id":77890,
            "bill":[
                {
                  "number":"202005-sample-2"
                }
            ]
        }
    ]
}
```
請求書同士（マイナス明細請求書との相殺）の消込取消の場合
```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "erusure": [
        {
            "error_code":null,
            "error_message":null,
            "erasure_id":12345,
            "payment_id":null,
            "bill":[
                {
                  "number":"202005-sample-1"
                },
                {
                  "number":"202005-sample-2"
                }
            ]
        }
    ]
}
```

請求書の消込ステータス変更による消込の場合
```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "erusure": [
        {
            "error_code":null,
            "error_message":null,
            "erasure_id":12345,
            "payment_id":null,
            "bill":[
                {
                  "number":"202005-sample-1"
                }
            ]
        }
    ]
}
```
## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                                                    |
| ------------ | ------------------------------------------------------ |
| 4101         | 消込結果IDが不正                                        |
| 4102         | 取消計上日が不正                                        |
| 4103         | 取消対象の消込結果が存在しません                         |
| 4104         | すでに取消済みの消込結果のため取消できません              |
| 4105         | 削除済みの消込結果です                                  |
| 4106         | マイナス売上相殺による消込は取消できません                |
| 4107         | 請求書が繰越済みです                                    |
| 4108         | 請求書が繰越予約中です                                  |
| 4109         | 消込取消タスクが発生している消込は取消できません          |
| 4110         | 承認依頼中です。承認が完了するまで取消できません          |
| 4111         | 入金消込ステータスが確認済みの消込は取消できません        |
| 4112         | 入金の未消込金額がマイナスになるため消込取消はできません   |
| 4113         | 既に締められた期間での消込取消はできません               |
| 4114         | 指定された取消計上日は締め日を過ぎています               |
| 4115         | 取消計上日は、消込計上日より前の日付を指定できません      |
| 4116         | 対象の消込は消込計上日を指定できません                   |
| 4117         | リクエストにclearingが存在しない                        |
| 4118         | リクエスト件数が上限を超えています。                     |
| 4119         | 消込取消に失敗しました                                  |

----

[TOPへ戻る](../../index.md)