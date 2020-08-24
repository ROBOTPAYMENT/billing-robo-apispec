# 請求書明細参照

`/api/v1.0/bill_detail/search`

請求書明細の参照処理を実行します。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/v1.0/bill_detail/search`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                  | 概要                                                                                | 桁数 | 種別                              | 必須 |
| --------------------- | ----------------------------------------------------------------------------------- | ---- | --------------------------------- | ---- |
| user_id               | ユーザーID（管理画面へのログインID）                                                | 100  | [メール形式](../../index.md#種別) | 必須 |
| access_key            | アクセスキー                                                                        | 100  | [半角英数](../../index.md#種別) | 必須 |
| limit_count           | 請求書明細取得件数 <br> ※0〜200の数値を設定する。省略した場合、20が設定される           | 3    | 数値                              |      |
| page_count            | 請求書明細取得開始インデックス <br> ※0～24,9999の数値を設定する。省略した場合、0が設定される | 5    | 数値                              |      |
| [sort](#sort-request)         | 検索順序に属するパラメータ <br> ※省略時は消込結果 ID、消込結果明細 ID の昇順で返却する            |      | `object`                          |      |
| [bill_detail](#bill_detail-request) | 請求書明細に属するパラメータ                                                            |      | `object`                     |      |

#### sort (request)

下記のような項目のオブジェクト

| 名前  | 概要                                                                                                                                                                                                                                         | 桁数 | 種別   | 必須 | 一致 |
| ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---- | ------ | ---- | ---- |
| key   | キー名 <br> ※以下の項目からのみ有効、複数選択可(カンマ区切り) <br> ※左の項目から優先的にソート順序が指定される <br> bill_id: 請求書 ID, <br> bill_detail_id: 請求書明細 ID, <br> regist_date: 登録日時, <br> update_date: 更新日時 | 49   | 文字列 |      |      |
| order | ソート順 <br> 0: 昇順 <br> 1: 降順                                                                                                                                                                                                           | 1    | 数値   |      |      |


#### bill_detail (request)

下記のような項目のオブジェクトを持つリスト

| 名前                          | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;概要&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | 桁数 | 種別                                   | 必須                                              | 一致 |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---- | ------------------------------------- | ------------------------------------------------ |----------|
| number                        | 請求書明細番号                                                                                                                                                                                                                                                                                                                                                                | 100  | [半角英数 + 記号](../../index.md#種別) |                                                   | 完全一致 |
| billing_code                  | 請求先コード                                                                                                                                                                                                                                                                                                                                                                  | 20   | [半角英数 + 記号](../../index.md#種別) | (請求先部署番号指定時) <br> (請求先部署コード指定時) | 完全一致  |
| billing_individual_number     | 請求先部署番号                                                                                                                                                                                                                                                                                                                                                                | 20   | 数値                                  |                                                   | 完全一致 |
| billing_individual_code       | 請求先部署コード                                                                                                                                                                                                                                                                                                                                                              | 20   | [半角英数 + 記号](../../index.md#種別) |                                                   | 完全一致 |
| issue_date_from               | 発行日の検索開始日                                                                                                                                                                                                                                                                                                                                                            | 10   | 日付                                  |                                                   |          |
| issue_date_to                 | 発行日の検索終了日                                                                                                                                                                                                                                                                                                                                                            | 10   | 日付                                  |                                                   |          |
| deadline_date_from            | 決済期限の検索開始日                                                                                                                                                                                                                                                                                                                                                          | 10   | 日付                                  |                                                   |          |
| deadline_date_to              | 決済期限の検索終了日                                                                                                                                                                                                                                                                                                                                                          | 10   | 日付                                  |                                                   |          |
| payment_method                | 決済手段 <br> 0:銀行振込 1:クレジットカード 2:バンクチェック <br> 3:RP口座振替 4:RL口座振替 5:その他口座振替 <br> 6:コンビニ払込票(A4) <br> 7:コンビニ払込票(ハガキ) <br> 8:その他コンビニ払込票 <br> 9:バーチャル口座 <br> 10:その他決済1 <br> 11:その他決済2 <br> 12:その他決済3 <br> 13:その他決済4 <br> 14:その他決済5                                                                    | 2    | 数値                                  |                                                   |          |
| clearing_status               | 消込ステータス <br> 0:未処理 <br> 1:完了 <br> 2:確認済み <br> 3:未収 <br> 4:貸倒 <br> 5:手数料 <br> 6:現金 <br> 7:長期滞留債権 <br> 8:破産更生債権 <br> 9:売上取消 <br> 10:繰越                                                                                                                                                                                                     | 2    | 数値                                  |                                                   |          |
| goods_code                    | 集計用商品コード                                                                                                                                                                                                                                                                                                                                                              | 33   | 文字列                                |                                                   | 完全一致 |
| bs_department_code            | 請求元部署コード <br>                                                                                                                                                                                                                                                                                                                                                         | 40   | [半角英数 + 記号](../../index.md#種別) |                                                   | 完全一致 |
| bs_owner_code                 | 請求元担当者コード <br>                                                                                                                                                                                                                                                                                                                                                       | 20   | [半角英数 + 記号](../../index.md#種別) |                                                   | 完全一致 |
| carryover_status              | 繰越ステータス <br> 0:対象外 <br> 1:繰越待ち <br> 2:繰越完了                                                                                                                                                                                                                                                                                                                    | 2    | 数値                                  |                                                   |         |
| carryover_transit_bill_number | 繰越先請求書番号                                                                                                                                                                                                                                                                                                                                                              | 100  | [半角英数 + 記号](../../index.md#種別) |                                                   | 完全一致 |
| regist_date_from              | 登録日時(開始日) <br> ※時刻省略時 00:00:00 で検索します                                                                                                                                                                                                                                                                                                                          | 19   | 日時                                 |                                                    |          |
| regist_date_to                | 登録日時(終了日) <br> ※時刻省略時 23:59:59 で検索します                                                                                                                                                                                                                                                                                                                          | 19   | 日時                                 |                                                    |          |
| update_date_from              | 更新日時(開始日) <br> ※時刻省略時 00:00:00 で検索します                                                                                                                                                                                                                                                                                                                          | 19   | 日時                                 |                                                    |          |
| update_date_to                | 更新日時(終了日) <br> ※時刻省略時 23:59:59 で検索します                                                                                                                                                                                                                                                                                                                          | 19   | 日時                                 |                                                    |          |
| valid_flg                     | 状態 <br> 0:無効 <br> 1:有効                                                                                                                                                                                                                                                                                                                                                  | 1    | 数値                                 |                                                    |          |

## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                                             | 概要                                                                                                                                         | 型            |
| ------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| user_id                                          | ユーザーID                                                                                                                                    | string        |
| access_key                                       | アクセスキー                                                                                                                                  | string        |
| limit_count                                      | 請求書明細取得件数 <br> ※最大件数は、リクエストで指定された「請求書明細情報取得件数」に依存                                                         | int           |
| page_count                                       | 請求書明細取得開始インデックス <br> ※取得した請求書明細情報の開始インデックスを返却する                                                             | int           |
| total_page_count                                 | 請求書明細取得開始インデックス合計 <br> ※指定された検索条件によって取得可能な請求書明細情報の全件数／請求書明細情報取得件数によって、算出される値を返却する | int           |
| [sort](#sort-response)                           | 検索順序に属するパラメータ                                                                                                                        | `object` |
| [bill_detail](#bill_detail-response)             | 請求書明細に属するパラメータ                                                                                                                      | `array` |
| [count_update_date](#count_update_date-response) | 更新日時に属するパラメータ                                                                                                                        | `array` |

#### sort (response)

下記のような項目のオブジェクト

| 名前  | 概要                               | 型     |
| ----- | ---------------------------------- | ------ |
| key   | キー名                             | string |
| order | ソート順 <br> 0: 昇順 <br> 1: 降順 | int    |

#### bill_detail (response)

下記のような項目のオブジェクトを持つリスト

| 名前                           | 概要                                                                                                                  | 型     |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------- | ------ |
| error_code                     | エラーコード <br> ※正常時はnull                                                                                        | string |
| error_message                  | エラーメッセージ <br> ※正常時はnull                                                                                    | string |
| number                         | 請求書番号                                                                                                             | string |
| billing_code                   | 請求先コード                                                                                                           | string |
| billing_individual_number      | 請求先部署番号                                                                                                         | int |
| billing_individual_code        | 請求先部署コード                                                                                                       | string |
| bill_detail_id                 | 請求書明細 ID                                                                                                         | int |
| demand_number                  | 請求情報番号                                                                                                          | int |
| demand_code                    | 請求情報コード                                                                                                        | string |
| sales_id                       | 売上 ID                                                                                                              | int |
| item_number                    | 商品番号                                                                                                              | string |
| item_code                      | 商品コード                                                                                                            | string |
| goods_name                     | 商品名                                                                                                                | string |
| pattern_period_format          | 対象期間形式 <br> 0：○年○月分 <br> 1：○年○月○日分 <br> 2：○年○月～○年○月 <br> 3：○年○月○日～○年○月○日 <br> 99：非表示      | string |
| demand_start_date              | 対象期間_開始                                                                                                         | date |
| demand_end_date                | 対象期間_終了                                                                                                         | date |
| criterion_date                 | 基準日                                                                                                                | date |
| link_customer_code             | 仕訳連携用取引先コード                                                                                                 | string |
| goods_code                     | 集計用商品コード                                                                                                       | string |
| link_goods_code                | 仕訳連携用商品コード                                                                                                   | string   |
| unit_price                     | 単価                                                                                                                  | string |
| quantity                       | 数量                                                                                                                  | string |
| unit                           | 単位                                                                                                                  | string |
| tax_category                   | 税区分 <br> 0:外税 1:内税 2:対象外 3:非課税                                                                             | int    |
| consumption_tax                | 消費税率                                                                                                              | int    |
| subtotal_amount_billed         | 請求金額小計                                                                                                           | int    |
| consumption_tax_amount         | 消費税額                                                                                                               | int    |
| total_amount_billed            | 請求金額合計                                                                                                           | int    |
| unclearing_amount              | 未消込金額                                                                                                             | int |
| remark                         | 備考                                                                                                                  | string |
| recorded_date                  | 売上計上日                                                                                                             | date |
| carryover_flg                  | 繰越フラグ <br> 0:通常請求書明細 1:繰越請求書明細                                                                         | int    |
| carryover_original_bill_number | 繰越元請求書番号                                                                                                        | string |
| regist_date                    | 登録日時                                                                                                               | datetime |
| update_date                    | 更新日時                                                                                                               | datetime |
| [custom](#custom-response)     | カスタム項目に属するパラメータ                                                                                           | array |

#### custom (response)

下記のような項目のオブジェクトを持つリスト

| 名前  | 概要                      | 型     |
| ----- | ------------------------ | ------ |
| code  | カスタム項目コード        | string |
| name  | カスタム項目名            | string |
| value | カスタム項目値            | string |


#### count_update_date (response)

下記のような項目のオブジェクトを持つリスト

| 名前        | 概要             | 型       |
| ----------- | ---------------- | -------- |
| update_date | 更新日時         | datetime |
| count       | 更新日時毎の件数  | int      |

## 使用例

### リクエスト例

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "limit_count": 20,
    "page_count": 1,
    "sort": {
        "key": "update_date,bill_id,bill_detail_id",
        "order": 0
    },
    "bill_detail": {
        "number": "202001-sample-1",
        "billing_code": "sample",
        "billing_individual_number": 1,
        "billing_individual_code": "",
        "issue_date_from": "2020/07/01",
        "issue_date_to": "2020/07/30",
        "deadline_date_from": "2020/07/01",
        "deadline_date_to": "2020/07/30",
        "payment_method": 0,
        "clearing_status": 3,
        "goods_code": "goods_code",
        "bs_department_code": "department",
        "bs_owner_code": "bs_owner_code",
        "carryover_status": 0,
        "carryover_transit_bill_number": "",
        "regist_date_from": "2020/07/01 10:00:00",
        "regist_date_to": "2020/07/31 10:00:00",
        "update_date_from": "2020/07/01 10:00:00",
        "update_date_to": "2020/07/30 10:00:00",
        "valid_flg": 1
    }
}

```

### レスポンス例

Status: 200 OK

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "limit_count": 20,
    "page_count": 0,
    "total_page_count": 1,
    "sort": {
        "key": "update_date,bill_id,bill_detail_id",
        "order": 0
    },
    "bill_detail": [
        {
            "error_code": null,
            "error_message": null,
            "number": "202007-sample-1",
            "billing_code": "sample",
            "billing_individual_number": 1,
            "billing_individual_code": "individual_code",
            "issue_date": "2020/06/01",
            "transfer_deadline": "2020/06/30",
            "bill_detail_id": 40,
            "demand_number": 6,
            "demand_code": "demand_code",
            "sales_id": 5,
            "item_number": 6,
            "item_code": "item_code",
            "goods_name": "goods_name",
            "pattern_period_format": 0,
            "demand_date_from": "2020/05/01",
            "demand_end_date": "2020/05/31",
            "criterion_date": "2020/05/15",
            "link_customer_code": "customer_code",
            "goods_code": "goods_code",
            "link_goods_code": "link_goods_code",
            "unit_price": "100.00",
            "quantity": "20.00",
            "unit": "ml",
            "tax_category": 0,
            "consumption_tax": 10,
            "subtotal_amount_billed": 2000,
            "consumption_tax_amount": 200,
            "total_amount_billed": 2200,
            "unclearing_amount": 2200,
            "remark": "これは備考です",
            "recorded_date": "2020/05/31",
            "carryover_flg": 0,
            "carryover_original_bill_number": "202007-sample-2",
            "regist_date": "2020/07/01 10:11:12",
            "update_date": "2020/07/01 16:17:18",
            "custom": [
                {
                    "code": "custom_1",
                    "name": "カスタム項目名",
                    "value": "カスタム項目値"
                },
                {
                    "code": "custom_2",
                    "name": "カスタム項目名2",
                    "value": "カスタム項目値2"
                }
            ]
        }
    ],
    "count_update_date": [
        {
            "update_time": "2020/07/21 10:00:00",
            "count": 4
        }
    ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード  | 内容                                      |
| ------------ | ---------------------------------------- |
| 4301         | 請求書番号が不正                           |
| 4302         | 請求先コードが不正                         |
| 4303         | 請求先部署番号が不正                       |
| 4304         | 請求先部署コードが不正                     |
| 4305         | 発行日の検索開始日が不正                   |
| 4306         | 発行日の検索終了日が不正                   |
| 4307         | 決済期限の検索開始日が不正                 |
| 4308         | 決済期限の検索終了日が不正                 |
| 4309         | 決済手段が不正                            |
| 4310         | 消込ステータスが不正                       |
| 4311         | 集計用商品コードが不正                      |
| 4312         | 請求元部署コードが不正                      |
| 4313         | 請求元担当者コードが不正                    |
| 4314         | 繰越ステータスが不正                        |
| 4315         | 繰越先請求書番号が不正                      |
| 4316         | 登録日時の検索開始日が不正                  |
| 4317         | 登録日時の検索終了日が不正                  |
| 4318         | 更新日時の検索開始日が不正                  |
| 4319         | 更新日時の検索終了日が不正                  |
| 4320         | 状態が不正                                 |
| 4321         | 請求書明細取得件数が不正                    |
| 4322         | 請求書明細取得開始インデックスが不正         |
| 4323         | キー名が不正                               |
| 4324         | ソート順が不正                             |
| 4325         | 請求書明細件数が25,000件に到達しました       |
| 4326         | 請求書明細参照に失敗しました                 |

----

[TOPへ戻る](../../index.md)