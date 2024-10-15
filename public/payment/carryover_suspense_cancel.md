# 入金繰越予約取消

`/api/v1.0/payment/carryover_suspense_cancel`

入金の繰越予約取消処理を実行します。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト

- Path: `/api/v1.0/payment/carryover_suspense_cancel`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                            | 概要                                       | 桁数 | 種別                              | 必須 |
|-------------------------------|------------------------------------------| ---- | --------------------------------- | --- |
| user_id                       | ユーザー ID（管理画面へのログイン ID）                   | 100  | [メール形式](../../index.md#種別) | 必須 |
| access_key                    | アクセスキー                                   | 100  | [半角英数](../../index.md#種別)   | 必須 |
| [payment](#payment-request)   | 入金繰越に属するパラメータ                            |      | `object`                          | 必須 |

#### payment (request)

下記のような項目のオブジェクト

| 名前               | 概要        | 桁数  | 種別    | 必須 | 一致 |
|-------------------|-----------|-----| ------- | ---- | ---- |
| payment_id        | 入金 ID     | 18  | 数値    | 必須     |      |

## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                           | 概要                      | 型      |
| ------------------------------ |-------------------------| ------- |
| user_id                        | ユーザー ID                 | string  |
| [payment](#payment-response) | 入金繰越に属するパラメータ           | `array` |

#### payment (response)

下記のような項目のオブジェクトを持つリスト

| 名前                | 概要                                             | 型     |
|-------------------|------------------------------------------------| ------ |
| error_code        | エラーコード <br> ※正常時は null                         | int    |
| error_message     | エラーメッセージ <br> ※正常時は null                       | string  |
| payment_id        | 入金 ID                                          | int    |

## 使用例

### リクエスト例

```json
{
  "user_id": "sample@robotpayment.co.jp",
  "access_key": "xxxxxxxxxxxxxxxx",
  "payment":{
    "payment_id": 1
  }
}
```

### レスポンス例

Status: 200 OK

```json
{
  "user_id": "sample@robotpayment.co.jp",
  "payment": [
    {
      "error_code": null,
      "error_message": null,
      "payment_id": 1
    }
  ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                       |
|--------|--------------------------|
| 6101   | 入金 ID が不正                |
| 6102   | 既に繰越予約取消済の入金です           |
| 6103   | 更新対象の入金情報が存在しません         |
| 6104   | リクエストパラメータに入金繰越情報が存在しません |
| 6105   | 承認中の入金情報は変更できません         |
| 6106   | 全て消込済みの入金情報は変更できません      |
| 6107   | 更新対象の入金情報が仮受金ではありません     |
| 6108   | 入金繰越予約取消の更新に失敗しました       |
| 6109   | 繰越オプションが利用可能ではありません      |

---

[TOP へ戻る](../../index.md)