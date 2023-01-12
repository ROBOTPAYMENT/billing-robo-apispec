# 入金参照

`/api/v1.0/payment/search`

入金情報の参照処理を実行します。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト

- Path: `/api/v1.0/payment/search`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                        | 概要                                                                                          | 桁数 | 種別                              | 必須 |
| --------------------------- | --------------------------------------------------------------------------------------------- | ---- | --------------------------------- | ---- |
| user_id                     | ユーザー ID（管理画面へのログイン ID）                                                        | 100  | [メール形式](../../index.md#種別) | 必須 |
| access_key                  | アクセスキー                                                                                  | 100  | [半角英数](../../index.md#種別)   | 必須 |
| limit_count                 | 入金情報取得件数 <br> ※0〜200 の数値を設定する。 <br> 省略した場合、20 が設定される           | 3    | 数値                              |      |
| page_count                  | 入金情報取得開始インデックス <br> ※0〜99,999 の数値を設定する。 <br> 省略した場合、0 が設定される | 5    | 数値                              |      |
| [sort](#sort-request)       | 検索順序に属するパラメータ <br> ※省略時は入金 ID の昇順で返却する                                  |      | `object`                          |      |
| [payment](#payment-request) | 入金情報に属するパラメータ                                                                    |      | `array`                           |      |

#### sort (request)

下記のような項目のオブジェクト

| 名前  | 概要                                                                                                                                                                                                                                         | 桁数 | 種別   | 必須 | 一致 |
| ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---- | ------ | ---- | ---- |
| key   | キー名 <br> ※以下の項目からのみ有効、複数選択可(カンマ区切り) <br> ※左の項目から優先的にソート順序が指定される <br> payment_id:入金ID, <br> regist_date: 登録日時, <br> update_date: 更新日時                                                                   | 52   | 文字列 |      |      |
| order | ソート順 <br> 0: 昇順 <br> 1: 降順                                                                                                                                                                                                           | 1    | 数値   |      |      |


#### payment (request)

下記のような項目のオブジェクトを持つリスト

| 名前                       | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;概要&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;                                                                                                          | 桁数 | 種別   | 必須 | 一致     |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---- | ------ | ---- | -------- |
| payment_id                 | 入金 ID                                                                                                                                                                                                                                                                                                                                                                   | 18   | 数値   |      |          |
| payment_method             | [決済方法](../../index.md#決済手段) | 2    | 数値   |      |          |
| payment_transfer_date_from | 入金日(開始日)                                                                                                                                                                                                                                                                                                                                                            | 10   | 日付   |      |          |
| payment_transfer_date_to   | 入金日(終了日)                                                                                                                                                                                                                                                                                                                                                            | 10   | 日付   |      |          |
| clearing_status            | 入金消込ステータス <br> 0:未消込み <br> 1:完了 <br> 2:確認済み                                                                                                                                                                                                                                                                                                            | 1    | 数値   |      |          |
| clearing_method            | 消込方法 <br> 0:自動消込 <br> 1:手動消込                                                                                                                                                                                                                                                                                                                                  | 1    | 数値   |      |          |
| import_date_from           | 取込日(開始日)                                                                                                                                                                                                                                                                                                                                                            | 10   | 日付   |      |          |
| import_date_to             | 取込日(終了日)                                                                                                                                                                                                                                                                                                                                                            | 10   | 日付   |      |          |
| import_format              | 取込形式 <br> 1:全銀 <br> 2:CSV-銀行振込 <br> 3:金融機関連携 <br> 5:CSV-その他 <br> 6:API                                                                                                                                                                                                                                                                                 | 1    | 数値   |      |          |
| transfer_status            | 振替区分 <br> 0:仮受 <br> 1:入金                                                                                                                                                                                                                                                                                                                                          | 1    | 数値   |      |          |
| suspense_name              | 仮受先名                                                                                                                                                                                                                                                                                                                                                                  | 100  | 文字列 |      | 部分一致 |
| valid_flg                  | 状態 <br> 0:無効 <br> 1:有効                                                                                                                                                                                                                                                                                                                                              | 1    | 数値   |      |          |
| bs_bank_transfer_code      | 請求元銀行口座コード <br> ※追加省略時、null で登録される                                                                                                                                                                                                                                                                                                                        | 20    | [半角英数 + 記号](../../index.md#種別)   |      | 完全一致 |
| regist_date_from           | 入金登録日時(開始日時) <br> ＊yyyy/MM/dd HH:mm:ss 形式 <br> ＊時刻省略時 00:00:00 で検索します                                                                                                                                                                                                                                                                                      | 19   | 日時   |      |          |
| regist_date_to             | 入金登録日時(終了日時) <br> ＊yyyy/MM/dd HH:mm:ss 形式 <br> ＊時刻省略時 23:59:59 で検索します                                                                                                                                                                                                                                                                                      | 19   | 日時   |      |          |
| update_date_from           | 入金更新日時(開始日時) <br> ＊yyyy/MM/dd HH:mm:ss 形式 <br> ＊時刻省略時 00:00:00 で検索します                                                                                                                                                                                                                                                                                       | 19   | 日時   |      |          |
| update_date_to             | 入金更新日時(終了日時) <br> ＊yyyy/MM/dd HH:mm:ss 形式 <br> ＊時刻省略時 23:59:59 で検索します                                                                                                                                                                                                                                                                                       | 19   | 日時   |      |          |

## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                         | 概要                                                                                                                                          | 型      |
| ---------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| user_id                      | ユーザー ID                                                                                                                                   | string  |
| limit_count                  | 入金情報取得件数 <br> ※最大件数は、リクエストで指定された「入金情報取得件数」に依存                                                           | int     |
| page_count                   | 入金情報取得開始インデックス <br> ※取得した入金情報の開始インデックスを返却する                                                               | int     |
| total_page_count             | 入金情報取得開始インデックス合計 <br> ※指定された検索条件によって取得可能な入金情報の全件数／入金情報取得件数によって、算出される値を返却する | int     |
| [sort](#sort-response)       | 検索順序に属するパラメータ                                                                                                                    | `object` |
| [payment](#payment-response) | 入金情報に属するパラメータ                                                                                                                    | `array` |
| [count_update_date](#count_update_date-response) | 更新日時に属するパラメータ                                                                                                                    | `array` |

#### sort (response)

下記のような項目のオブジェクト

| 名前  | 概要                               | 型     |
| ----- | ---------------------------------- | ------ |
| key   | キー名                             | string |
| order | ソート順 <br> 0: 昇順 <br> 1: 降順 | int    |

#### payment (response)

下記のような項目のオブジェクトを持つリスト

| 名前                  | 概要                                                                                                                                                                                                                                                                                                                                                                      | 型     |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| error_code            | エラーコード <br> ※正常時は null                                                                                                                                                                                                                                                                                                                                          | int    |
| error_message         | エラーメッセージ <br> ※正常時は null                                                                                                                                                                                                                                                                                                                                      | string |
| payment_id            | 入金 ID                                                                                                                                                                                                                                                                                                                                                                   | int    |
| payment_method        | [決済方法](../../index.md#決済手段) | int    |
| payment_transfer_date | 入金日                                                                                                                                                                                                                                                                                                                                                                    | string |
| account_number        | 口座番号                                                                                                                                                                                                                                                                                                                                                                  | string |
| account_name          | 振込名義                                                                                                                                                                                                                                                                                                                                                                  | string |
| amount                | 金額                                                                                                                                                                                                                                                                                                                                                                      | int    |
| unclearing_amount     | 未消込金額                                                                                                                                                                                                                                                                                                                                                                | int    |
| clearing_status       | 入金消込ステータス <br> 0:未消込み <br> 1:完了 <br> 2:確認済み                                                                                                                                                                                                                                                                                                            | int    |
| clearing_method       | 消込方法 <br> 0:自動消込 <br> 1:手動消込                                                                                                                                                                                                                                                                                                                                  | int    |
| clearing_key          | 消込キー                                                                                                                                                                                                                                                                                                                                                                  | string |
| import_date           | 取込日 <br> ※バンクチェックと入金インポートした入金以外はnull                                                                                                                                                                                                                                                                                                                                                                 | string |
| import_format         | 取込形式 <br> 1:全銀 <br> 2:CSV-銀行振込 <br> 3:金融機関連携 <br> 5:CSV-その他 <br> 6:API                                                                                                                                                                                                                                                                                 | int    |
| transfer_status       | 振替区分 <br> 0:仮受 <br> 1:入金                                                                                                                                                                                                                                                                                                                                          | int    |
| suspense_name         | 仮受先名                                                                                                                                                                                                                                                                                                                                                                  | string |
| memo                      | メモ                                                                                                                                   | string |
| valid_flg             | 状態 <br> 0:無効 <br> 1:有効                                                                                                                                                                                                                                                                                                                                              | string |
| bs_bank_transfer_code | 請求元銀行口座コード                                                                                                                                                                                                                                                                                                                                                       | string |
| regist_date           | 登録日時                                                                                                                                                                                                                                                                                                                                                                  | datetime |
| update_date           | 更新日時                                                                                                                                                                                                                                                                                                                                                                  | datetime |

#### count_update_date (response)

下記のような項目のオブジェクトを持つリスト

| 名前        | 概要             | 型       |
| ----------- | ---------------- | -------- |
| update_date | 更新日時         | datetime |
| count       | 更新日時毎の件数 | int      |

## 使用例

### リクエスト例

```json
{
  "user_id": "sample@robotpayment.co.jp",
  "access_key": "xxxxxxxxxxxxxxxx",
  "limit_count": 20,
  "page_count": 0,
  "sort": {
    "key": "regist_date,payment_id",
    "order": 0
  },
  "payment": {
    "payment_id": "123456",
    "payment_method": 1,
    "payment_transfer_date_from": "2020/03/01",
    "payment_transfer_date_to": "2020/04/11",
    "clearing_status": 0,
    "clearing_method": 0,
    "import_date_from": "2020/02/01",
    "import_date_to": "2020/03/31",
    "import_format": 1,
    "transfer_status": 1,
    "suspense_name": "sample株式会社",
    "valid_flg": 1,
    "regist_date_from": "2019/01/01 00:00:00",
    "regist_date_to": "2019/08/01 23:59:59",
    "update_date_from": "2019/01/01 00:00:00",
    "update_date_to": "2019/08/01 23:59:59",
    "bs_bank_transfer_code": "1234"
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
  "total_page_count": 0,
  "sort": {
    "key": "regist_date,payment_id",
    "order": 0
  },
  "payment": [
    {
      "error_code": null,
      "error_message": null,
      "payment_id": 123456,
      "payment_method": 1,
      "payment_transfer_date": "2020/04/01",
      "account_number": "1234abc",
      "account_name": "振込名義",
      "amount": 5000,
      "unclearing_amount": 0,
      "clearing_status": 0,
      "clearing_method": 0,
      "clearing_key": "",
      "import_date": "2020/04/01",
      "import_format": 1,
      "transfer_status": 0,
      "suspense_name": "sample株式会社",
      "memo": "メモ",
      "valid_flg": 1,
      "regist_date": "2019/06/21 13:57:01",
      "update_date": "2019/06/21 13:57:02",
      "bs_bank_transfer_code": "1234"
    }
  ],
  "count_update_date": [
    {
      "update_date": "2019/06/21 13:57:02",
      "update_count": 1
    }
  ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                               |
| ------------ | ---------------------------------- |
| 3501         | 入金 ID が不正                     |
| 3502         | 仮受先名が不正                     |
| 3503         | 決済方法が不正                     |
| 3504         | 決済日の開始日が不正               |
| 3505         | 決済日の終了日が不正               |
| 3506         | 入金消込ステータスが不正           |
| 3507         | 振替区分が不正                     |
| 3508         | 取込日の開始日が不正               |
| 3509         | 取込日の終了日が不正               |
| 3510         | 取込形式が不正                     |
| 3511         | 消込方法が不正                     |
| 3512         | 状態が不正                         |
| 3513         | 入金情報取得件数が不正             |
| 3514         | 入金情報取得開始インデックスが不正 |
| 3515         | 入金参照に失敗                     |
| 3516         | 入金登録日時の開始日時が不正                     |
| 3517         | 入金登録日時の終了日時が不正                     |
| 3518         | 入金更新日時の開始日時が不正                     |
| 3519         | 入金更新日時の終了日時が不正                     |
| 3520         | 請求元銀行口座コードが不正                     |
| 3521         | 振込先銀行口座が存在しません                     |
| 3522         | 入金件数が100,000件に到達しました                     |
| 3523         | キー名が不正                     |
| 3524         | ソート順が不正                     |

---

[TOP へ戻る](../../index.md)
