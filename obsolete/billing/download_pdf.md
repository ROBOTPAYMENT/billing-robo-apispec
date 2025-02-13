> このAPIは廃止されたAPIとなっております。代替APIを下記の通りご用意させていただいております。
- [口座振替依頼書発行 v1.0/billing/bulk_download_pdf](/public/billing/bulk_download_pdf.html)

# 口座振替依頼書発行

`/api/billing/download_pdf`

口座振替依頼書を発行します。
戻り値で受け取ったurlに再度アクセスすればダウンロードできます。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Path: `/api/billing/download_pdf`
- Preferred HTTP method: `POST`
- Accepted content types: `application/x-www-form-urulencoded`
- Encode: `UTF-8`

### Parameters

| 名前                      | 概要                                 | 桁数 | 型                                     | 必須     |
| ------------------------- | ------------------------------------ | ---- | -------------------------------------- | -------- |
| user_id                   | ユーザーID（管理画面へのログインID） | 100  | [メール形式](../../index.md#種別)      | 必須     |
| access_key                | アクセスキー                         | 100  | [半角英数](../../index.md#種別)        | 必須     |
| billing_code              | 請求先コード                         | 20   | [半角英数 + 記号](../../index.md#種別) | 必須     |
| billing_individual_number | 請求先部署番号                       | 20   | 数値                                   | (必須)^1 |
| billing_individual_code   | 請求先部署コード                     | 20   | [半角英数 + 記号](../../index.md#種別) | (必須)^1 |


## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前 | 概要                            | 型     |
| ---- | ------------------------------- | ------ |
| url  | 口座振替依頼書ダウンロード用URL | string |


## 使用例

### リクエスト例

```
user_id: "sample@robotpayment.co.jp"
access_key: "xxxxxxxxxxxxxxxx"
billing_code: "billing"
billing_individual_code: "bicd0001"
```

### レスポンス例

Status: 200 OK

```json
{
  "url":" https://keirinomikata.jp/download_request_form?id=HyNnajensdalineavllie",
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                                                        |
| ------------ | ---------------------------------------------------------- |
| 301          | 振替日が不正                                                |
| 302          | 請求先コード、請求先部署番号、請求先部署コードのいずれかが不正 |
| 303          | 口座振替依頼書発行に失敗                                     |
| 304          | 部署の決済手段がRP口座振替以外                               |
| 305          | 顧客番号が空                                                |

----

[TOPへ戻る](../../index.md)