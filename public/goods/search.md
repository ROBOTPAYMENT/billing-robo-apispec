# 商品参照

`/api/v1.0/goods/search`

商品情報の詳細を取得します

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト

- Path: `/api/v1.0/goods/search`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                    | 概要                                                                                          | 桁数 | 種別                              | 必須 |
| ----------------------- | --------------------------------------------------------------------------------------------- | ---- | --------------------------------- | ---- |
| user_id                 | ユーザー ID                                                                                   | 100  | [メール形式](../../index.md#種別) | 必須 |
| access_key              | アクセスキー                                                                                  | 100  | [半角英数](../../index.md#種別)   | 必須 |
| limit_count             | 商品情報取得件数 <br> ※0〜200 の数値を設定する。 <br> 省略した場合、20 が設定される           | 3    | 数値                              |      |
| page_count              | 商品情報取得開始インデックス <br> ※0〜99 の数値を設定する。 <br> 省略した場合、0 が設定される | 2    | 数値                              |      |
| [goods](#goods-request) | 商品に属するパラメータ                                                                        |      | `object`                          |      |

#### goods (request)

下記のような項目のオブジェクト

| 名前        | 概要                                                  | 桁数 | 種別          | 一致     |
| ----------- | ----------------------------------------------------- | ---- | ------------- | -------- |
| item_number | 商品番号                                              | 18   | 数値          |          |
| item_code   | 商品コード                                            | 20   | 半角英数+記号 | 完全一致 |
| code        | 集計用商品コード                                      | 100  | 文字列        | 完全一致 |
| item_name   | 商品管理名                                            | 60   | 文字列        | 部分一致 |
| demand_type | 請求タイプ <br> 0:単発 <br> 1:定期定額 <br>2:定期従量 | 1    | 数値          |          |
| valid_flg   | 状態 <br> 0:無効 <br> 1:有効                          | 1    | 数値          |          |

## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                     | 概要                                                                                                                                          | 型      |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| user_id                  | ユーザー ID                                                                                                                                   | string  |
| limit_count              | 商品情報取得件数 <br> ※最大件数は、リクエストで指定された「商品情報取得件数」に依存                                                           | int     |
| page_count               | 商品情報取得開始インデックス <br> ※取得した商品情報の開始インデックスを返却する                                                               | int     |
| total_page_count         | 商品情報取得開始インデックス合計 <br> ※指定された検索条件によって取得可能な商品情報の全件数／商品情報取得件数によって、算出される値を返却する | int     |
| [goods](#goods-response) | 商品情報に属するパラメータ                                                                                                                    | `array` |

#### goods (response)

下記のような項目のオブジェクトを持つリスト

| 名前                                            | 概要                                                                                                                                                                                                                                       | 型     |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------ |
| error_code                                      | エラーコード <br> ※正常時は null                                                                                                                                                                                                           | int    |
| error_message                                   | エラーメッセージ <br> ※正常時は null                                                                                                                                                                                                       | string |
| item_number                                     | 商品番号                                                                                                                                                                                                                                   | int    |
| item_code                                       | 商品コード                                                                                                                                                                                                                                 | string |
| code                                            | 集計用商品コード                                                                                                                                                                                                                           | string |
| journal_cooperation_goods_code                  | 仕訳連携用商品コード                                                                                                                                                                                                                       | string |
| item_name                                       | 商品管理名                                                                                                                                                                                                                                 | string |
| name                                            | 商品名                                                                                                                                                                                                                                     | string |
| demand_type                                     | 請求タイプ <br> 0:単発 <br> 1:定期定額 <br>2:定期従量                                                                                                                                                                                      | int    |
| unit_price                                      | 単価                                                                                                                                                                                                                                       | string |
| unit                                            | 単位                                                                                                                                                                                                                                       | string |
| tax_category                                    | 税区分 <br> 0:外税 <br> 1:内税 <br> 2:対象外 <br> 3:非課税                                                                                                                                                                                 | int    |
| tax_rate                                        | 消費税率                                                                                                                                                                                                                                   | string |
| remarks_column                                  | 備考欄                                                                                                                                                                                                                                     | string |
| repetition_period_number                        | 繰返し周期\_数字 <br> 値:1-60                                                                                                                                                                                                         | int    |
| repetition_period_unit                          | 繰返し周期\_単位 <br> 1:月                                                                                                                                                                                                             | int    |
| repeat_count_max                                | 繰返し回数                                                                                                                                                                                                                               | int    |
| period_format                                   | 対象期間形式 <br> 0:◯ 年 ◯ 月分 <br> 1:◯ 年 ◯ 月 ◯ 日分 <br> 2:◯ 年 ◯ 月～ △ 年 △ 月 <br> 3:◯ 年 ◯ 月 ◯ 日～ △ 年 △ 月 △ 日 <br> 99:表示なし                                                                                               | int    |
| period_value                                    | 対象期間\_数字 値:1-60                                                                                                                                                                                                                     | int    |
| period_unit                                     | 対象期間\_単位 1:月                                                                                                                                                                                                                        | int    |
| period_criterion                                | 対象期間\_基準 <br> 0:対象期間開始日 <br> 1:対象期間終了日                                                                                                                                                                                 | int    |
| sales_recorded_date_month                       | 売上計上日\_月 <br> -60:60 ヶ月前 ～ 60:60 ヶ月後 <br> 当月は 0                                                                                                                                             | int    |
| sales_recorded_date_day                         | 売上計上日\_日 <br> 1:1 日、2:2 日、・・99:末日                                                                                                                                                                                            | int    |
| bill_issue_date_month                           | 請求書発行日\_月 <br> -60:60 ヶ月前 ～ 60:60 ヶ月後 <br> 当月は 0                                                                                                                                           | int    |
| bill_issue_date_day                             | 請求書発行日\_日 <br> 1:1 日、2:2 日、・・99:末日                                                                                                                                                                                          | int    |
| bill_sending_date_month                         | 請求書送付日\_月 <br> -60:60 ヶ月前 ～ 60:60 ヶ月後 <br> 当月は 0                                                                                                                                           | int    |
| bill_sending_date_day                           | 請求書送付日\_日 <br> 1:1 日、2:2 日、・・99:末日                                                                                                                                                                                          | int    |
| transfer_deadline_month                         | 決済期限\_月 <br> -60:60 ヶ月前 ～ 60:60 ヶ月後 <br> 当月は 0                                                                                                                                               | int    |
| transfer_deadline_day                           | 決済期限\_日 <br> 1:1 日、2:2 日、・・99:末日                                                                                                                                                                                              | int    |
| billing_method                                  | 請求方法 <br> 0:請求無し <br> 1:自動メール <br> 2:手動メール <br> 3:自動郵送 <br> 4:手動郵送 <br> 5:自動メール＋自動郵送 <br> 6:手動メール＋手動郵送 <br> 7:手動マイページ配信 <br> 8:自動マイページ配信                                                                                    | int    |
| bill_template_code                              | 請求書テンプレートコード                                                                                                                                                                                                                   | int    |
| bill_template_name                              | 請求書テンプレート名                                                                                                                                                                                                                       | string |
| account_title_code                              | 売上高勘定科目コード（勘定科目 ID） <br> 売上高 :4100 <br> 売上高 2 :4101 <br> 売上高 3 :4102 <br> 売上高 4 :4103 <br> 売上高 5 :4104 <br> 売上高 6 :4105 <br> 売上高 7 :4106 <br> 売上高 8 :4107 <br> 売上高 9 :4108 <br> 売上高 10 :4109 | int    |
| sub_account_title_code                          | 売上高補助科目コード                                                                                                                                                                                                                       | string |
| account_title_id_account_receivable_trade       | 売掛金勘定科目コード <br> ※固定値:1162                                                                                                                                                                                                     | int    |
| sub_account_title_code_account_receivable_trade | 売掛金補助科目コード                                                                                                                                                                                                                       | string |
| account_title_id_advances_received              | 前受金勘定科目コード <br> ※固定値:2111                                                                                                                                                                                                     | int    |
| sub_account_title_code_advances_received        | 前受金補助科目コード                                                                                                                                                                                                                       | string |
| [custom](#custom-response)                      | カスタム項目に属するパラメータ                                                                                                                                                                                                             | array  |

### custom (response)

下記のような項目のオブジェクトを持つリスト

| 名前 | 概要               | 型     |
| ---- | ------------------ | ------ |
| number | カスタム項目番号 | int |
| code | カスタム項目コード | string |
| name | カスタム項目名     | string |
| value | カスタム項目値    | string |

## 使用例

### リクエスト例

```json
{
  "user_id": "sample@robotpayment.co.jp",
  "access_key": "xxxxxxxxxxxxxxxx",
  "limit_count": "",
  "page_count": "",
  "goods": {
    "item_number": 5,
    "item_code": "1234abc",
    "code": "",
    "item_name": "商品管理名",
    "demand_type": 0,
    "valid_flg": 1
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
  "goods": [
    {
      "error_code": null,
      "error_message": null,
      "item_number": 5,
      "item_code": "1234abc",
      "code": "54",
      "journal_cooperation_goods_code": "1234abc",
      "item_name": "商品管理名",
      "name": "商品A",
      "demand_type": 0,
      "unit_price": "1000.0000",
      "unit": "個",
      "tax_category": 0,
      "tax_rate": 8,
      "remarks_column": "備考",
      "repetition_period_number": 1,
      "repetition_period_unit": 1,
      "repeat_count_max": 1,
      "period_format": 2,
      "period_value": 1,
      "period_unit": 1,
      "period_criterion": 1,
      "sales_recorded_date_month": 0,
      "sales_recorded_date_day": 99,
      "bill_issue_date_month": 0,
      "bill_issue_date_day": 1,
      "bill_sending_date_month": 0,
      "bill_sending_date_day": 1,
      "transfer_deadline_month": 0,
      "transfer_deadline_day": 1,
      "billing_method": 0,
      "bill_template_code": 10010,
      "bill_template_name": "シンプル",
      "account_title_code": 4100,
      "sub_account_title_code": "1234abc",
      "account_title_code_account_receivable_trade": 1162,
      "sub_account_title_code_account_receivable_trade": "5678abc",
      "account_title_code_advances_received": 2111,
      "sub_account_title_code_advances_received": "1234cde",
      "custom": [
        {
          "number": 15,
          "code": "custom_1",
          "name": "カスタム項目名",
          "value": "カスタム項目値"
        },
        {
          "number": 16,
          "code": "custom_2",
          "name": "カスタム項目名2",
          "value": "カスタム項目値2"
        }
      ]
    }
  ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                               |
| ------------ | ---------------------------------- |
| 4201         | 商品番号が不正                     |
| 4202         | 商品コードが不正                   |
| 4203         | 集計用商品コードが不正             |
| 4204         | 商品管理名が不正                   |
| 4205         | 請求タイプが不正                   |
| 4206         | 状態が不正                         |
| 4207         | 商品情報取得件数が不正             |
| 4208         | 商品情報取得開始インデックスが不正 |
| 4209         | 商品参照に失敗                     |

---

[TOP へ戻る](../../index.md)
