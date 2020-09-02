# 請求情報参照

`/api/v1.0/demand/search2`

請求情報の参照処理を実行します。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト

- Method URL: `https://billing-robo.jp:10443/api/v1.0/demand/search2`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                      | 概要                                                                                          | 桁数 | 種別                              | 必須 |
| ------------------------- | --------------------------------------------------------------------------------------------- | ---- | --------------------------------- | ---- |
| user_id                   | ユーザー ID（管理画面へのログイン ID）                                                        | 100  | [メール形式](../../index.md#種別) | 必須 |
| access_key                | アクセスキー                                                                                  | 100  | [半角英数](../../index.md#種別)   | 必須 |
| limit_count               | 請求情報取得件数 <br> ※0〜200 の数値を設定する。 <br> 省略した場合、20 が設定される           | 3    | 数値                              |      |
| page_count                | 請求情報取得開始インデックス <br> ※0〜99 の数値を設定する。 <br> 省略した場合、0 が設定される | 2    | 数値                              |      |
| [demand](#demand-request) | 請求情報に属するパラメータ                                                                    |      | `array`                           |      |

#### demand (request)

下記のような項目のオブジェクトを持つリスト

| 名前                      | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;概要&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | 桁数 | 種別                            | 一致     |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---- | ------------------------------- | -------- |
| billing_code              | 請求先コード                                                                                                                                                                                                                                                                                                                                                                 | 20   | [半角英数](../../index.md#種別) | 完全一致 |
| billing_individual_number | 請求先部署番号                                                                                                                                                                                                                                                                                                                                                               | 18   | 数値                            |          |
| billing_individual_code   | 請求先部署コード                                                                                                                                                                                                                                                                                                                                                             | 20   | [半角英数](../../index.md#種別) | 完全一致 |
| number                    | 請求情報番号                                                                                                                                                                                                                                                                                                                                                                 | 18   | 数値                            |          |
| code                      | 請求情報コード                                                                                                                                                                                                                                                                                                                                                               | 20   | [半角英数](../../index.md#種別) | 完全一致 |
| outside_billing_number    | 外部連携用請求書番号                                                                                                                                                                                                                                                                                                                                                         | 20   | [半角英数](../../index.md#種別) |          |
| item_code                 | 商品コード                                                                                                                                                                                                                                                                                                                                                                   | 20   | [半角英数](../../index.md#種別) | 完全一致 |
| goods_code                | 集計用商品コード                                                                                                                                                                                                                                                                                                                                                             | 33   | 文字列                          | 部分一致 |
| type                      | 請求タイプ <br> 0:単発 <br> 1:定期定額 <br> 2:定期従量                                                                                                                                                                                                                                                                                                                       | 1    | 数値                            |          |
| repetition_period_number  | 繰り返し周期 <br> 値:1~60月                                                                                                                                                                                                                                                                                                                                                 | 2    | 数値                            |          |
| next_issue_date_check     | 次回請求書発行                                                                                                                                                                                                                                                                                                                                                               | 1    | 数値                            |          |
| next_issue_date_from      | 次回請求書発行日（開始日）                                                                                                                                                                                                                                                                                                                                                   | 10   | 日付                            |          |
| next_issue_date_to        | 次回請求書発行日（終了日）                                                                                                                                                                                                                                                                                                                                                   | 10   | 日付                            |          |
| end_date_from             | 最終サービス提供期間（開始日）                                                                                                                                                                                                                                                                                                                                               | 10   | 日付                            |          |
| end_date_to               | 最終サービス提供期間（終了日）                                                                                                                                                                                                                                                                                                                                               | 10   | 日付                            |          |
| payment_method_code       | 決済情報コード                                                                                                                                                                                                                                                                                                                                                               | 20   | [半角英数](../../index.md#種別) | 完全一致 |
| payment_method            | 決済手段 <br> 0:銀行振込 <br> 1:クレジットカード <br> 2:バンクチェック <br> 3:RP 口座振替 <br> 4:RL 口座振替 <br> 5:その他口座振替 <br> 6:コンビニ払込票(A4) <br> 7:コンビニ払込票(ハガキ) <br> 8:その他コンビニ払込票 <br> 9:バーチャル口座 <br> 10:その他決済手段1 <br> 11:その他決済手段2 <br> 12:その他決済手段3 <br> 13:その他決済手段4 <br> 14:その他決済手段5         | 2    | 数値                            |          |
| billing_method            | 請求方法 <br> 0:送付なし <br> 1:自動メール <br> 2:手動メール <br> 3:自動郵送 <br> 4:手動郵送 <br> 5:自動メール+自動郵送 <br> 6:手動メール+手動郵送                                                                                                                                                                                                                           | 1    | 数値                            |          |
| regist_user_code          | 登録ユーザーコード                                                                                                                                                                                                                                                                                                                                                           | 20   | [半角英数](../../index.md#種別) | 完全一致 |
| bs_department_code        | 請求元部署コード                                                                                                                                                                                                                                                                                                                                                             | 40   | [半角英数](../../index.md#種別) | 完全一致 |
| bs_owner_code             | 請求元担当者コード                                                                                                                                                                                                                                                                                                                                                           | 20   | [半角英数](../../index.md#種別) | 完全一致 |
| regist_date_from          | 登録日時の検索開始日 <br> ※時刻省略時 00:00:00 で検索します                                                                                                                                                                                                                                                                                                                  | 19   | 日時                            |          |
| regist_date_to            | 登録日時の検索終了日 <br> ※時刻省略時 23:59:59 で検索します                                                                                                                                                                                                                                                                                                                  | 19   | 日時                            |          |
| update_date_from          | 更新日時の検索開始日 <br> ※時刻省略時 00:00:00 で検索します                                                                                                                                                                                                                                                                                                                  | 19   | 日時                            |          |
| update_date_to            | 更新日時の検索終了日 <br> ※時刻省略時 23:59:59 で検索します                                                                                                                                                                                                                                                                                                                  | 19   | 日時                            |          |
| valid_flg                 | 状態 <br> 0:無効 <br> 1:有効                                                                                                                                                                                                                                                                                                                                                 | 1    | 数値                            |          |

## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                       | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;概要&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | 型      |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| user_id                    | ユーザー ID（管理画面へのログイン ID）                                                                                                                                                                                                                                                                                                                                       | string  |
| access_key                 | アクセスキー                                                                                                                                                                                                                                                                                                                                                                 | string  |
| limit_count                | 請求情報取得件数 <br> ※最大件数は、リクエストで指定された「請求書取得件数」に依存                                                                                                                                                                                                                                                                                              | int     |
| page_count                 | 請求情報取得開始インデックス <br> ※取得した請求書の開始インデックスを返却する                                                                                                                                                                                                                                                                                                  | int     |
| total_page_count           | 請求情報取得開始インデックス合計 <br> ※指定された検索条件によって取得可能な請求書の全件数／請求書取得件数によって、算出される値を返却する                                                                                                                                                                                                                                      | int     |
| [demand](#demand-response) | 請求情報に属するパラメータ                                                                                                                                                                                                                                                                                                                                                   | `array` |

#### demand (response)

下記のような項目のオブジェクトを持つリスト

| 名前                       | 概要                                                                                                                                                                                                                                                                                                                                                                      | 型       |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| error_code                 | エラーコード <br> ※正常時は null                                                                                                                                                                                                                                                                                                                                          | string   |
| error_message              | エラーメッセージ <br> ※正常時は null                                                                                                                                                                                                                                                                                                                                      | string   |
| billing_code               | 請求先コード                                                                                                                                                                                                                                                                                                                                                              | string   |
| billing_individual_number  | 請求先部署番号                                                                                                                                                                                                                                                                                                                                                            | int      |
| billing_individual_code    | 請求先部署コード                                                                                                                                                                                                                                                                                                                                                          | string   |
| number                     | 請求情報番号                                                                                                                                                                                                                                                                                                                                                              | int      |
| code                       | 請求情報コード                                                                                                                                                                                                                                                                                                                                                            | string   |
| outside_billing_number     | 外部連携用請求書番号                                                                                                                                                                                                                                                                                                                                                      | string   |
| item_code                  | 商品コード                                                                                                                                                                                                                                                                                                                                                                | string   |
| goods_code                 | 集計用商品コード                                                                                                                                                                                                                                                                                                                                                          | string   |
| link_goods_code            | 会計ソフト連携用商品コード                                                                                                                                                                                                                                                                                                                                                | string   |
| goods_name                 | 商品名                                                                                                                                                                                                                                                                                                                                                                    | string   |
| type                       | 請求タイプ <br> 0:単発 <br> 1:定期定額 <br> 2:定期従量                                                                                                                                                                                                                                                                                                                    | int      |
| price                      | 単価                                                                                                                                                                                                                                                                                                                                                                      | string   |
| quantity                   | 数量                                                                                                                                                                                                                                                                                                                                                                      | string   |
| unit                       | 単位                                                                                                                                                                                                                                                                                                                                                                      | string   |
| tax_category               | 税区分 <br> 0:外税 <br> 1:内税 <br> 2:対象外 <br> 3:非課税                                                                                                                                                                                                                                                                                                                | int      |
| tax                        | 消費税率                                                                                                                                                                                                                                                                                                                                                                  | int      |
| remark                     | 備考                                                                                                                                                                                                                                                                                                                                                                      | string   |
| payment_method_code        | 決済情報コード                                                                                                                                                                                                                                                                                                                                                            | string   |
| payment_method             | 決済手段 <br> 0:銀行振込 <br> 1:クレジットカード <br> 2:バンクチェック <br> 3:RP 口座振替 <br> 4:RL 口座振替 <br> 5:その他口座振替 <br> 6:コンビニ払込票(A4) <br> 7:コンビニ払込票(ハガキ) <br> 8:その他コンビニ払込票 <br> 9:バーチャル口座 <br> 10:その他決済手段1 <br> 11:その他決済手段2 <br> 12:その他決済手段3 <br> 13:その他決済手段4 <br> 14:その他決済手段5       | int      |
| billing_method             | 請求方法 <br> 0:送付なし <br> 1:自動メール <br> 2:手動メール <br> 3:自動郵送 <br> 4:手動郵送 <br> 5:自動メール+自動郵送 <br> 6:手動メール+手動郵送                                                                                                                                                                                                                        | int      |
| repetition_period_number   | 繰返し周期 <br> 値:1-60                                                                                                                                                                                                                                                                                                                                                   | int      |
| repetition_period_unit     | 繰返し周期単位 <br> 1:月                                                                                                                                                                                                                                                                                                                                                  | int      |
| start_date                 | サービス提供開始日                                                                                                                                                                                                                                                                                                                                                        | date     |
| end_date                   | 最終サービス提供期間                                                                                                                                                                                                                                                                                                                                                      | date     |
| repeat_count               | 繰返し回数 <br> ※type=1,2 以外は null                                                                                                                                                                                                                                                                                                                                     | int      |
| period_format              | 対象期間形式 <br> 0：○年 ○月分 <br> 1：○年 ○月 ○日分 <br> 2：○年 ○月 ～ ○年 ○月 <br> 3：○年 ○月 ○日 ～ ○年 ○月 ○日 <br> 99：非表示                                                                                                                                                                                                                                        | int      |
| period_value               | 対象期間 <br> ※period_format=2,3 以外はnull                                                                                                                                                                                                                                                                                                                              | int      |
| period_unit                | 対象期間単位 <br> ※period_format=3 以外はnull                                                                                                                                                                                                                                                                                                                            | int      |
| period_criterion           | 基準 <br> 0:対象期間開始日 <br> 1:対象期間終了日 <br> ※period_format=2,3 以外は0                                                                                                                                                                                                                                                                                         | int      |
| sales_recorded_month       | 売上計上日\_月                                                                                                                                                                                                                                                                                                                                                            | int      |
| sales_recorded_day         | 売上計上日\_日                                                                                                                                                                                                                                                                                                                                                            | int      |
| issue_month                | 請求書発行日\_月                                                                                                                                                                                                                                                                                                                                                          | int      |
| issue_day                  | 請求書発行日\_日                                                                                                                                                                                                                                                                                                                                                          | int      |
| sending_month              | 請求書送付日\_月                                                                                                                                                                                                                                                                                                                                                          | int      |
| sending_day                | 請求書送付日\_日                                                                                                                                                                                                                                                                                                                                                          | int      |
| deadline_month             | 決済期限\_月                                                                                                                                                                                                                                                                                                                                                              | int      |
| deadline_day               | 決済期限\_日                                                                                                                                                                                                                                                                                                                                                              | int      |
| slip_deadline_month        | 払込票有効期限\_月                                                                                                                                                                                                                                                                                                                                                        | int      |
| slip_deadline_day          | 払込票有効期限\_日                                                                                                                                                                                                                                                                                                                                                        | int      |
| next_period_start_date     | 次回対象期間開始日                                                                                                                                                                                                                                                                                                                                                        | date     |
| next_period_end_date       | 次回対象期間終了日                                                                                                                                                                                                                                                                                                                                                        | date     |
| next_sales_recorded_date   | 次回売上計上日                                                                                                                                                                                                                                                                                                                                                            | date     |
| next_issue_date            | 次回請求発行日                                                                                                                                                                                                                                                                                                                                                            | date     |
| next_sending_date          | 次回請求送付日                                                                                                                                                                                                                                                                                                                                                            | date     |
| next_deadline_date         | 次回振込期限                                                                                                                                                                                                                                                                                                                                                              | date     |
| next_slip_deadline_date    | 次回払込票有効期限                                                                                                                                                                                                                                                                                                                                                        | date     |
| next_sales_make_date       | 次回売上データ作成⽇                                                                                                                                                                                                                                                                                                                                                      | date     |
| memo                       | メモ                                                                                                                                                                                                                                                                                                                                                                      | string   |
| bill_template_code         | 請求書テンプレートコード                                                                                                                                                                                                                                                                                                                                                  | int      |
| account_title_code         | 勘定科目コード                                                                                                                                                                                                                                                                                                                                                            | string   |
| text_pattern_code          | 文書パターンコード                                                                                                                                                                                                                                                                                                                                                        | string   |
| bill_group_key             | 請求書合算キー                                                                                                                                                                                                                                                                                                                                                            | string   |
| regist_user_code           | 登録ユーザーコード                                                                                                                                                                                                                                                                                                                                                        | string   |
| bs_owner_code              | 請求元担当者コード                                                                                                                                                                                                                                                                                                                                                        | string   |
| bs_department_code         | 請求元部署コード                                                                                                                                                                                                                                                                                                                                                          | string   |
| regist_date                | 登録日時                                                                                                                                                                                                                                                                                                                                                                  | datetime |
| update_date                | 更新日時                                                                                                                                                                                                                                                                                                                                                                  | datetime |
| valid_flg                  | 状態                                                                                                                                                                                                                                                                                                                                                                      | int      |
| [custom](#custom-response) | カスタム項目に属するパラメータ                                                                                                                                                                                                                                                                                                                                            | array    |

#### custom (response)

下記のような項目のオブジェクトを持つリスト

| 名前  | 概要               | 型     |
| ----- | ------------------ | ------ |
| code  | カスタム項目コード | string |
| name  | カスタム項目名     | string |
| value | カスタム項目値     | string |

## 使用例

### リクエスト例

```json
{
  "user_id": "sample@robotpayment.co.jp",
  "access_key": "xxxxxxxxxxxxxxxx",
  "limit_count": 20,
  "page_count": 0,
  "demand": {
    "billing_code": "sample",
    "billing_individual_number": 1,
    "billing_individual_code": "test",
    "number": 1,
    "code": "123",
    "item _code": "sample001",
    "goods_code": "sample001",
    "type": 0,
    "repetition_period_number": 1,
    "next_issue_date_check": 1,
    "next_issue_date_from": "2020/8/1",
    "next_issue_date_to": "2020/8/2",
    "end_date_from": "2020/8/30",
    "end_date_to": "2020/8/31",
    "payment_method_code": "bank001",
    "payment_method": 0,
    "billing_method": 1,
    "regist_user_code": "sample_account_code",
    "bs_department_code": "sample_bs_code",
    "bs_owner_code": "sample_owner_code",
    "regist_date_from": "2020/08/01 10:11:12",
    "regist_date_to": "2020/08/02 10:11:12",
    "update_date_from": "2020/08/01 10:11:12",
    "update_date_to": "2020/08/02 10:11:12",
    "valid_flg": 0
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
  "total_page_count": 0,
  "demand": [
    {
      "error_code": null,
      "error_message": null,
      "billing_code": "sample",
      "billing_individual_number": "test",
      "billing_individual_code": "test",
      "number": 1,
      "code": "123",
      "item _code": "dammy",
      "goods_code": "dammy",
      "link_goods_code": "dammy",
      "goods_name": "ダミー商品",
      "type": 0,
      "price": "10000.0000",
      "quantity": "1.0000",
      "unit": "式",
      "tax_category": 0,
      "tax": 10,
      "remark": "備考",
      "payment_method_code": "code0",
      "payment_method": 0,
      "billing_method": 1,
      "repetition_period_number": 1,
      "repetition_period_unit": 1,
      "start_date": "2020/8/1",
      "end_date": "2020/8/31",
      "repeat_count": 1,
      "period_format": 3,
      "period_value": 1,
      "period_unit": 1,
      "period_criterion": 0,
      "sales_recorded_month": 8,
      "sales_recorded_day": 31,
      "issue_month": 8,
      "issue_day": 1,
      "sending_month": 8,
      "sending_day": 1,
      "deadline_month": 8,
      "deadline_day": 31,
      "slip_deadline_month": 8,
      "slip_deadline_day": 31,
      "next_issue_date": "2020/8/1",
      "next_period_start_date": "2020/8/1",
      "next_period_end_date": "2020/8/31",
      "next_sales_recorded_date": "2020/8/1",
      "next_bill_issue_date": "2020/8/1",
      "next_bill_sending_date": "2020/8/1",
      "next_transfer_deadline_date": "2020/8/31",
      "next_payment_slip_deadline_date": "2020/8/31",
      "memo": "メモ",
      "bill_template_code": 1,
      "account_title_code": "title_code",
      "text_pattern_code": "pattern_code",
      "bill_group_key": "group_key",
      "owner_account_code": "account_owner_code",
      "bs_owner_code": "owner_code",
      "bs_department_code": "department_code",
      "regist_date": "2020/08/01 10:11:12",
      "update_date": "2020/08/01 10:11:12",
      "valid_flg": 0,
      "custom": [
        {
          "code": 1,
          "name": "カスタム１",
          "value": "カスタム１"
        }
      ]
    }
  ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                                 |
| ------------ | ------------------------------------ |
| 4701         | 請求先コードが不正                   |
| 4702         | 請求先部署番号が不正                 |
| 4703         | 請求先部署コードが不正               |
| 4704         | 請求情報番号が不正                   |
| 4705         | 請求情報コードが不正                 |
| 4706         | 外部連携用請求書番号が不正           |
| 4707         | 商品コードが不正                     |
| 4708         | 集計用商品コードが不正               |
| 4709         | 請求タイプが不正                     |
| 4710         | 繰り返し周期が不正                   |
| 4711         | 次回請求書発行が不正                 |
| 4712         | 次回請求書発行日（開始日）が不正     |
| 4713         | 次回請求書発行日（終了日）が不正     |
| 4714         | 最終サービス提供期間（開始日）が不正 |
| 4715         | 最終サービス提供期間（終了日）が不正 |
| 4716         | 決済情報コードが不正                 |
| 4717         | 決済手段が不正                       |
| 4718         | 請求方法が不正                       |
| 4719         | 登録ユーザーコードが不正             |
| 4720         | 請求元部署コードが不正               |
| 4721         | 請求元担当者コードが不正             |
| 4722         | 登録日時の検索開始日が不正           |
| 4723         | 登録日時の検索終了日が不正           |
| 4724         | 更新日時の検索開始日が不正           |
| 4725         | 更新日時の検索終了日が不正           |
| 4726         | 状態が不正                           |
| 4727         | 請求情報取得件数が不正               |
| 4728         | 請求情報取得開始インデックスが不正   |
| 4729         | 請求情報参照に失敗                   |

---

[TOP へ戻る](../../index.md)
