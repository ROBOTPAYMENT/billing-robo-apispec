# 与信参照

`/api/v1.0/marunage_credit/search`

与信情報を返却します。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Path: `/api/v1.0/marunage_credit/search`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前              | 概要                                                                     | 桁数 | 種別                           | 必須 |
| ---------------- | ----------------------------------------------------------------------- | --- | ----------------------------- | --- |
| user_id          | ユーザーID（管理画面へのログインID）                                            | 100 | [メール形式](../../index.md#種別) | 必須 |
| access_key       | アクセスキー                                                               | 100 | [半角英数](../../index.md#種別)  | 必須 |
| limit_count      | 与信情報取得件数<br> ※0〜200の数値を設定する。省略した場合、20が設定される        |  3  | int                           |    |
| page_count       | 与信情報取得開始インデックス<br> ※0～99の数値を設定する。省略した場合、0が設定される |  2  | int                           |    |
| marunage_credit  | 与信情報に属するパラメータ                                               |     | `object`                      |    |

#### marunage_credit (request)

下記のような項目のオブジェクト

| 名前               | 概要                                                    | 桁数  | 種別                                | 必須 |
|------------------| ------------------------------------------------------ | ---- | ---------------------------------- | --- |
| id               | 与信ID                                            | 18   | 数値                                |     |
| billing_code     | 請求先コード                                              | 20   | [半角英数 + 記号](../../index.md#種別) |     |
| claim_type       | 債権区分<br> 1: まるなげ<br> 2: ファクタリング         | 1    | 数値                                |     |
| status           | 与信状態<br> 0: 停止<br> 1: 有効<br> 2: 申請中<br> 3: 未申請<br> 4: 有効（停止申請中）         | 1    | 数値                                |     |
| regist_date_from | 登録日時の検索開始日時<br> ※時刻省略時 00:00:00 で検索します | 19   | 日時                                |     |
| regist_date_to   | 登録日時の検索終了日時<br> ※時刻省略時 23:59:59 で検索します | 19   | 日時                                |     |
| update_date_from | 更新日時の検索開始日時<br> ※時刻省略時 00:00:00 で検索します | 19   | 日時                                |     |
| update_date_to   | 更新日時の検索終了日時<br> ※時刻省略時 23:59:59 で検索します | 19   | 日時                                |     |



## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前              | 概要                                                                                                              | 型      |
| ---------------- | ---------------------------------------------------------------------------------------------------------------- | ------- |
| user_id          | ユーザーID                                                                                                         | string  |
| limit_count      | 与信情報取得件数<br> ※最大件数は、リクエストで指定された「与信情報取得件数」に依存                                            | int     |
| page_count       | 与信情報取得開始インデックス<br> ※取得した与信情報の開始インデックスを返却する                                               | int     |
| total_page_count | 与信情報取得開始インデックス合計<br> ※指定された検索条件によって取得可能な与信情報の全件数／与信情報取得件数によって、算出される値を返却する | int     |
| marunage_credit  | 与信情報に属するパラメータ                                                                                        | `array` |

#### marunage_credit (response)

下記のような項目のオブジェクトを持つリスト

| 名前            | 概要                                                | 型       |
|---------------| -------------------------------------------------  | ------  |
| error_code    | エラーコード<br> ※正常時はnull                         | int      |
| error_message | エラーメッセージ<br> ※正常時はnull                      | string   |
| id            | 与信ID                                        | int      |
| billing_code  | 請求先コード                                          | string   |
| claim_type    | 債権区分<br> 1: まるなげ<br> 2: ファクタリング    | int      |
| status        | 与信状態<br> 0: 停止<br> 1: 有効<br> 2: 申請中<br> 3: 未申請    | int      |
| regist_date   | 登録日時                                             | datetime |
| update_date   | 更新日時                                             | datetime |


## 使用例

### リクエスト例

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "limit_count": 100,
    "page_count": 0,
    "marunage_credit": {
        "id": null,
        "billing_code": "billing_code",
        "claim_type": 1,
        "status": 1,
        "regist_date_from":"2022/05/12 10:00:00",
        "regist_date_to":"2022/05/15 10:00:00",
        "update_date_from":"2022/05/13",
        "update_date_to":"2022/05/14"
    }
}
```

### レスポンス例

Status: 200 OK

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "limit_count": 100,
    "page_count": 0,
    "total_page_count": 1,
    "marunage_credit": [
        {
            "error_code": null,
            "error_message": null,
            "id": 2,
            "billing_code": "billing_code_2",
            "claim_type": 1,
            "status": 1,
            "regist_date":"2022/05/14 23:59:59",
            "update_date":"2022/05/14 23:59:59"
        },
        {
            "error_code": null,
            "error_message": null,
            "id": 1,
            "billing_code": "billing_code_1",
            "claim_type": 1,
            "status": 1,
            "regist_date":"2022/05/13 00:00:00",
            "update_date":"2022/05/13 00:00:00"
        }
    ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

[その他の特殊なエラーコード](../../index.md#その他の特殊なエラーコード)

個別エラー

| エラーコード    | 内容                                |
| ------------ | ---------------------------------- |
| 5701         | 与信IDが不正                    |
| 5702         | 請求先コードが不正                      |
| 5703         | 与信状態が不正                   |
| 5704         | 登録日時の検索開始日時が不正              |
| 5705         | 登録日時の検索終了日時が不正              |
| 5706         | 更新日時の検索開始日時が不正              |
| 5707         | 更新日時の検索終了日時が不正              |
| 5708         | 与信情報取得件数が不正             |
| 5709         | 与信情報取得開始インデックス件数が不正 |
| 5610   | 債権区分が不正                   |

----

[TOPへ戻る](../../index.md)