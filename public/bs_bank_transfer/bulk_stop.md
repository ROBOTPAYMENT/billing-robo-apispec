# 請求元銀行口座停止削除

`/api/v1.0/bs_bank_transfer/bulk_stop`

複数の請求元銀行口座を停止または削除する処理。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Path: `/api/v1.0/bs_bank_transfer/bulk_stop`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                                        | 概要                                 | 桁数 | 種別                              | 必須 |
| ------------------------------------------- | ------------------------------------ | ---- | --------------------------------- | ---- |
| user_id                                     | ユーザーID（管理画面へのログインID） | 100  | [メール形式](../../index.md#種別) | 必須 |
| access_key                                  | アクセスキー                         | 100  | [半角英数](../../index.md#種別)   | 必須 |
| [bs_bank_transfer](#bsbanktransfer-request) | 請求元銀行口座に属するパラメータ     |      | `array`         |      |

#### bs_bank_transfer (request)

下記のような項目のオブジェクトを持つリスト

| 名前    | 概要                     | 桁数 | 種別                                   | 必須 |
| ------- | ------------------------ | ---- | -------------------------------------- | ---- |
| code    | 銀行口座コード           | 20   | [半角英数 + 記号](../../index.md#種別) | 必須 |
| del_flg | 削除フラグ <br> 0: 停止 <br> 1:削除 | 1    | 数値                                   | 必須 |


## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                                         | 概要                                 | 型                        |
| -------------------------------------------- | ------------------------------------ | ------------------------- |
| user_id                                      | ユーザーID（管理画面へのログインID） | string                    |
| [bs_bank_transfer](#bsbanktransfer-response) | 請求元銀行口座に属するパラメータ     | `array` |

#### bs_bank_transfer (response)

下記のような項目のオブジェクトを持つリスト

| 名前          | 概要                          | 型     |
| ------------- | ----------------------------- | ------ |
| error_code    | エラーコード※正常時はnull     | int    |
| error_message | エラーメッセージ※正常時はnull | string |
| code          | 銀行口座コード                | string |
| title         | 銀行口座名                    | string |
| del_flg       | 削除フラグ <br> 0: 無効 <br> 1:削除      | int    |


## 使用例

### リクエスト例

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    " bs_bank_transfer ": [
        {
            "code": "sample",
            "del_flg": 0
        }
    ]
}
```

### レスポンス例

Status: 200 OK

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "bs_bank_transfer": [
        {
            "error_code": null,
            "error_message": null,
            "code": "sample",
            "title": "サンプル",
            "del_flg": 0
        }
    ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                                                                     |
| ------------ | ---------------------------------------------------------------------- |
| 2301         | 銀行口座コードが不正                                                     |
| 2302         | 更新対象の銀行口座が存在しません                                          |
| 2303         | 削除フラグが不正                                                         |
| 2304         | すでに無効な口座です                                                     |
| 2305         | 請求元銀行口座を全て無効にできません                                       |
| 2306         | 請求元銀行口座を無効にできません                                          |
| 2307         | 請求元銀行口座パターンで使用されている請求元銀行口座は無効にできません        |
| 2308         | 金融機関連携済みの請求元銀行口座は無効または削除できません                  |

----

[TOPへ戻る](../../index.md)