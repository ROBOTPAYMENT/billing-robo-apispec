# 入金繰越予約

`/api/v1.0/payment/carryover_suspense`

入金の繰越予約処理を実行します。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト

- Path: `/api/v1.0/payment/carryover_suspense`
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
| bs_residence_code | 請求元差出人コード | 20  | 半角英数 + 記号    | 必須     |      |
| bs_owner_code     | 請求元担当者コード | 20  | 半角英数 + 記号    |     |      |


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
| bs_residence_code | 請求元差出人コード                                      | string   |
| bs_owner_code     | 請求元担当者コード <br> ※請求元担当者が設定しない場合、nullで表示される      | string   |

## 使用例

### リクエスト例

```json
{
  "user_id": "sample@robotpayment.co.jp",
  "access_key": "xxxxxxxxxxxxxxxx",
  "payment":{
    "payment_id": 1
    "bs_residence_code": "sample",
    "bs_owner_code": "sample"
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
      "bs_residence_code": "sample",
      "bs_owner_code": "sample"
    }
  ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                        |
|----|---------------------------|
| 6001 | 入金 ID が不正                 |
| 6002 | 請求元差出人コードが不正              |
| 6003 | 請求元担当者コードが不正              |
| 6004 | 既に繰越予約処理済の入金です            |
| 6005 | 対象の仮受金で部署が指定されていない        |
| 6006 | 更新対象の入金情報が存在しません          |
| 6007 | リクエストパラメータに入金繰越情報が存在しません  |
| 6008 | 承認中の入金情報は変更できません          |
| 6009 | 全て消込済みの入金情報は変更できません       |
| 6010 | 合算条件が同じ繰越予約中の請求書が存在します    |
| 6011 | 更新対象の入金情報が仮受金ではありません      |
| 6012 | 入金繰越予約の更新に失敗しました          |
| 6013 | 繰越オプションが利用可能ではありません       |

---

[TOP へ戻る](../../index.md)
