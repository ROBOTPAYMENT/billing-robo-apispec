# 債権譲渡申請参照

`/api/v1.0/marunage_bill_credit/search`

指定の請求書番号の債権譲渡申請関連項目参照を実行します。\
債権譲渡申請関連項目に関しては、[請求書参照](/public/bill/search.md)でも取得可能です。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Path: `/api/v1.0/marunage_bill_credit/search`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                                                    | 概要                                                  | 桁数  | 種別                         | 必須 |
|-------------------------------------------------------|-----------------------------------------------------|-----|----------------------------|----|
| user_id                                               | ユーザーID（管理画面へのログインID）                                | 100 | [メール形式](../../index.md#種別) | 必須 |
| access_key                                            | アクセスキー                                              | 100 | [半角英数](../../index.md#種別)  | 必須 |
| limit_count                                           | 請求書取得件数 <br> ※0〜200の数値を設定する。省略した場合、20が設定される         | 3   | 数値                         |    |
| page_count                                            | 請求書取得開始インデックス <br> ※0〜99,999の数値を設定する。省略した場合、0が設定される | 5   | 数値                         |    |
| [sort](#sort-request)                                 | 検索順序に属するパラメータ <br> ※省略時は請求書ID の昇順で返却する              |     | `object`                   |    |
| [marunage_bill_credit](#marunage_bill_credit-request) | 債権譲渡申請に属するパラメータ                                     |     | `object`                   | 必須 |

#### sort (request)

下記のような項目のオブジェクト \
※指定したkeyの値で同一順のレコードが複数あった場合、請求書作成順で返却されます。ソート順はorderによって決まります。

| 名前    | 概要                                                                                                                  | 桁数 | 種別  | 必須 | 一致 |
|-------|---------------------------------------------------------------------------------------------------------------------|----|-----|----|----|
| key   | キー名 <br> ※以下の項目からのみ有効、複数選択可(カンマ区切り) <br> ※左の項目から優先的にソート順序が指定される <br> apply_date: 債権譲渡申請日, <br> answer_date: 債権譲渡回答日 | 31 | 文字列 |    |    |
| order | ソート順 <br> 0: 昇順 <br> 1: 降順                                                                                          | 1  | 数値  |    |    |

#### marunage_bill_credit (request)

下記のような項目のオブジェクトを持つリスト

| 名前               | 概要           | 桁数  | 種別                                  | 必須 |
|------------------|--------------|-----|-------------------------------------|----|
| bill_number      | 請求書番号        | 100 | 配列（ [半角英数 + 記号](../../index.md#種別)） | 必須 |
| status           | 債権譲渡申請ステータス  | 100 | 配列（数値）                              |    |
| apply_date_from  | 債権譲渡申請日（開始日） | 10  | 日付                                  |    |
| apply_date_to    | 債権譲渡申請日（終了日） | 10  | 日付                                  |    |
| answer_date_from | 債権譲渡申請日（開始日） | 10  | 日付                                  |    |
| answer_date_to   | 債権譲渡申請日（終了日） | 10  | 日付                                  |    |


## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                                                     | 概要                                                            | 型        |
|--------------------------------------------------------|---------------------------------------------------------------|----------|
| user_id                                                | ユーザーID                                                        | string   |
| limit_count                                            | 取得件数 <br> ※最大件数は、リクエストで指定された「取得件数」に依存                         | int      |
| page_count                                             | 取得開始インデックス <br> ※取得した開始インデックスを返却する                            | int      |
| total_page_count                                       | 取得開始インデックス合計 <br> ※指定された検索条件によって取得可能な全件数／取得件数によって、算出される値を返却する | int      |
| [sort](#sort-response)                                 | 検索順序に属するパラメータ                                                 | `object` |
| [marunage_bill_credit](#marunage_bill_credit-response) | 債権譲渡申請に属するパラメータ                                               | `array`  |

#### sort (response)

下記のような項目のオブジェクト

| 名前    | 概要                         | 型      |
|-------|----------------------------|--------|
| key   | キー名                        | string |
| order | ソート順 <br> 0: 昇順 <br> 1: 降順 | int    |

#### marunage_bill_credit (response)

下記のような項目のオブジェクトを持つリスト

| 名前            | 概要                                                                            | 型      |
|---------------|-------------------------------------------------------------------------------|--------|
| error_code    | エラーコード<br> ※正常時はnull                                                          | int    |
| error_message | エラーメッセージ<br> ※正常時はnull                                                        | string |
| bill_number   | 請求書番号                                                                         | string |
| status        | 債権譲渡申請ステータス<br> 0: 未申請<br> 1: 申請中<br>2: 承認<br> 3: 却下<br> 4: 回答中<br> 5: 債権譲渡解除 | int    |
| apply_date    | 債権譲渡申請日                                                                       | date   |
| answer_date   | 債権譲渡回答日                                                                       | date   |


## 使用例

### リクエスト例

```json
{
  "user_id": "sample@robotpayment.co.jp",
  "access_key": "xxxxxxxxxxxxxxxx",
  "limit_count": 20,
  "page_count": 0,
  "sort": {
    "key": "apply_date",
    "order": 1
  },
  "marunage_bill_credit": {
    "bill_number": ["202401-sample-1","202401-sample-2"],
    "status":[0,1],
    "apply_date_from": null,
    "apply_date_to": null,
    "answer_date_from": null,
    "answer_date_to": null
  }
}
```

### レスポンス例

Status: 200 OK

```json
{
  "user_id": "sample@robotpayment.co.jp",
  "limit_count": 20,
  "page_count": 0,
  "total_page_count": 1,
  "sort": {
    "key": "apply_date",
    "order": 1
  },
  "marunage_bill_credit": [
    {
      "error_code": null,
      "error_message": null,
      "bill_number": "202401-sample-1",
      "status":1,
      "apply_date":"2024-10-24",
      "answer_date":null
    },
    {
      "error_code": null,
      "error_message": null,
      "bill_number": "202401-sample-2",
      "status":0,
      "apply_date":null,
      "answer_date":null
    }
  ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

[その他の特殊なエラーコード](../../index.md#その他の特殊なエラーコード)

個別エラー

| エラーコード | 内容                |
|--------|-------------------|
| 6115   | 請求書番号が不正          |
| 6116   | 対象のまるなげ請求書が存在しません |
| 6117   | 債権譲渡申請ステータスが不正    |
| 6118   | 債権譲渡申請日の検索開始日が不正  |
| 6119   | 債権譲渡申請日の検索終了日が不正  |
| 6120   | 債権譲渡回答日の検索開始日が不正  |
| 6121   | 債権譲渡回答日の検索終了日が不正  |
| 6122   | 取得件数が不正           |
| 6123   | 取得開始インデックスが不正     |
| 6124   | キー名が不正            |
| 6125   | ソート順が不正           |

----

[TOPへ戻る](../../index.md)