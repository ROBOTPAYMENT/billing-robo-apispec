# 商品登録更新2

`/api/v1.0/goods/bulk_upsert2`

複数の商品登録・更新処理を実行します。
商品登録更新とは、レスポンスについて単価の型が異なります。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/v1.0/goods/bulk_upsert2`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                    | 概要                                 | 桁数 | 種別                              | 必須 |
| ----------------------- | ------------------------------------ | ---- | --------------------------------- | ---- |
| user_id                 | ユーザーID（管理画面へのログインID） | 100  | [メール形式](../../index.md#種別) | 必須 |
| access_key              | アクセスキー                         | 100  | [半角英数](../../index.md#種別)   | 必須 |
| [goods](#goods-request) | 商品に属するパラメータ               |      | `array`                    |      |

#### goods (request)

下記のような項目のオブジェクトを持つリスト

| 名前                                            | 概要                                                                                                                                                                                                                      | 桁数         | 種別                            | 必須                               |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | ------------------------------- | ---------------------------------- |
| item_number                                     | 商品番号                                                                                                                                                                                                                  | 20           | 半角英数                        | (更新時)^1 <br> (商品コード更新時) |
| item_code                                       | 商品コード                                                                                                                                                                                                                | 20           | 半角英数                        | (登録時) <br> (更新時)^1           |
| code                                            | 集計用商品コード                                                                                                                                                                                                          | 100          | 文字                        |                                    |
| journal_cooperation_goods_code                  | 仕訳連携用商品コード                                                                                                                                                                                                      | 33           | 文字                        |                                    |
| item_name                                       | 商品管理名                                                                                                                                                                                                                | 60           | 文字                            | (登録時)                           |
| name                                            | 商品名                                                                                                                                                                                                                    | 60           | 文字                            | (登録時)                           |
| demand_type                                     | 請求タイプ <br> 0：単発、1：定期定額、2：定期従量                                                                                                                                                                         | 1            | 半角数字                        | (登録時)                           |
| unit_price                                      | 単価  <br> ※クレジットカード決済の場合は桁数上限整数7桁                                                                                                                                                                   | 整数10,小数4 | 数字                            | (demand_type=0,1時)                |
| unit                                            | 単位                                                                                                                                                                                                                      | 3            | 文字                            |                                    |
| tax_category                                    | 税区分  <br> 0:外税 1:内税 2:対象外 3:非課税                                                                                                                                                                              | 1            | 半角数字                        | (登録時)                           |
| tax_rate                                        | 消費税率                                                                                                                                                                                                                  | 2            | 半角数字                        | (登録時) <br> (tax_category=0,1時) |
| remarks_column                                  | 備考欄 <br> ※60文字×17行入力が可能                                                                                                                                                                                        | -            | 文字                            |                                    |
| repetition_period_number                        | 繰り返し周期_数字 <br> 入力可能値:1～60                                                                                                                                                                                   | 2            | 半角数字                        | (demand_type=1,2時)                |
| repetition_period_unit                          | 繰り返し周期_単位 <br> 1：月※請求の繰り返し周期の単位を入力（単発の場合は、値を入力しない）                                                                                                                               | 1            | 半角数字                        |                                    |
| repeat_count_max                                | 繰り返し回数                                                                                                                                                                                                              | 2            | 半角数字                        | (登録時でdemand_type=1,2時)        |
| period_format                                   | 対象期間形式 <br> 0：◯年◯月分1：◯年◯月◯日分2：◯年◯月～△年△月3：◯年◯月◯日～△年△月△日99：表示なし                                                                                                                           | 1            | 半角数字                        | (登録時)                           |
| period_value                                    | 対象期間 <br> 入力可能値：1-60※対象期間形式が2：◯年◯月～△年△月、3：◯年◯月◯日～△年△月△日 の場合に設定                                                                                                                      | 2            | 半角数字                        |                                    |
| period_unit                                     | 対象期間_単位 <br> 1：月※対象期間形式が2：◯年◯月～△年△月、3：◯年◯月◯日～△年△月△日 の場合に設定                                                                                                                            | 1            | 半角数字                        |                                    |
| period_criterion                                | 対象期間_基準 <br> 0：対象期間開始日1：対象期間終了日                                                                                                                                                                     | 1            | 半角数字                        | (period_format=2,3時)              |
| bill_issue_date_month                           | 請求書発行日_月 <br> 入力可能値:-60～60 <br> 例） <br> -1：前月 <br> 0：当月                                                                                                                                              | 3            | 半角数字                        | (bill_issue_date_day入力時)        |
| bill_issue_date_day                             | 請求書発行日_日 <br> 1：1日、2：2日、・・99：末日                                                                                                                                                                         | 2            | 半角数字                        | (bill_issue_date_month入力時)      |
| bill_sending_date_month                         | 請求書送付日_月 <br> 入力可能値:-60～60 <br> 例） <br> -1：前月 <br> 0：当月                                                                                                                                              | 3            | 半角数字                        | (bill_sending_date_day入力時)      |
| bill_sending_date_day                           | 請求書送付日_日 <br> 1：1日、2：2日、・・99：末日                                                                                                                                                                         | 2            | 半角数字                        | (bill_sending_date_month入力時)    |
| transfer_deadline_month                         | 決済期限_月 <br> 入力可能値:-60～60 <br> 例） <br> -1：前月 <br> 0：当月                                                                                                                                                  | 3            | 半角数字                        | (transfer_deadline_day入力時)      |
| transfer_deadline_day                           | 決済期限_日 <br> 1：1日、2：2日、・・99：末日                                                                                                                                                                             | 2            | 半角数字                        | (transfer_deadline_month入力時)    |
| sales_recorded_date_month                       | 売上計上日_月 <br> 入力可能値:-60～60 <br> 例） <br> -1：前月 <br> 0：当月                                                                                                                                                | 3            | 半角数字                        | (sales_recorded_date_day入力時)    |
| sales_recorded_date_day                         | 売上計上日_日 <br> 1：1日、2：2日、・・99：末日                                                                                                                                                                           | 2            | 半角数字                        | (sales_recorded_date_month入力時)  |
| billing_method                                  | 請求方法 <br> 0:請求無し、1:自動メール、2:手動メール、3:自動郵送、4:手動郵送、5:自動メール＋自動郵送、6:手動メール＋手動郵送                                                                                              | 1            | 半角数字                        |                                    |
| bill_template_code                              | 請求書テンプレートコード <br> ※合計請求書はご利用いただけません                                                                                                                                                               | 18           | 半角数字                        |                                    |
| account_title_id                                | 売上高勘定科目コード（勘定科目ID） <br> 売上高 ：4100、売上高2 ：4101、売上高3 ：4102、売上高4 ：4103、売上高5 ：4104、売上高6 ：4105、売上高7 ：4106、売上高8 ：4107、売上高9 ：4108、売上高10 ：4109※入力がなければ4100 | 18           | 半角数字                        |                                    |
| sub_account_title_code                          | 売上高補助科目コード <br> ※補助科目コード設定対象：商品の場合登録可、商品以外の場合登録不可                                                                                                                               | 25           | 文字                       |                                    |
| account_title_id_account_receivable_trade       | 売掛金勘定科目コード <br> ※固定値：1162                                                                                                                                                                                   | 18           | 半角数字                        |                                    |
| sub_account_title_code_account_receivable_trade | 売掛金補助科目コード <br> ※補助科目コード設定対象：商品の場合登録可、商品以外の場合登録不可                                                                                                                               | 25           | 文字                        |                                    |
| account_title_id_advances_received              | 前受金勘定科目コード <br> ※固定値：2111                                                                                                                                                                                   | 18           | 半角数字                        |                                    |
| sub_account_title_code_advances_received        | 前受金補助科目コード <br> ※補助科目コード設定対象：商品の場合登録可、商品以外の場合登録不可                                                                                                                               | 25           | 文字                        |                                    |
| [custom](#custom-request)                       | 商品カスタム項目に属するパラメータ                                                                                                                                                                                     |           | array                                 |                               |

#### custom (request)

下記のような項目のオブジェクトを持つリスト

| 名前                                            | 概要                                                                                                                                                                                                                                                                                                                                                        | 桁数         | 種別                                   | 必須                          |
| ----------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | -------------------------------------- | ----------------------------- |
| number                                          | カスタム項目番号                                                                                                                                                                                                                                                                                                                                                    | 18           | 数値                                   | (必須※1)^1 |
| code                                            | カスタム項目コード                                                                                                                                                                                                                                                                                                                                                  | 20           | [半角英数 + 記号](../../index.md#種別)  | (必須※1)^1 |
| value                                           | カスタム項目値                                                                                                                                                                                                                                                                                                                                                      | 300          | 文字列                                 |                       |

※1 カスタム項目必須フラグONでカスタム項目を登録してる場合必須です。また商品カスタム項目の登録更新時にはnumberもしくはcodeが必須です。



## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                     | 概要                                | 型             |
| ------------------------ | ----------------------------------- | -------------- |
| user_id                  | ユーザーID                          | string         |
| access_key               | アクセスキー                        | string         |
| error_code               | エラーコード <br> ※正常時はnull     | int            |
| error_message            | エラーメッセージ <br> ※正常時はnull | string         |
| [goods](#goods-response) | 商品に属するパラメータ              | `array` |

#### goods (response)

下記のような項目のオブジェクトを持つリスト

| 名前                                            | 概要                                                                                                                                                                                                   | 型     |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------ |
| item_number                                     | 商品番号 <br> ※ミカタ側で発番される番号となります                                                                                                                                                      | int    |
| item_code                                       | 商品コード                                                                                                                                                                                             | string |
| code                                            | 集計用商品コード                                                                                                                                                                                       | string |
| journal_cooperation_goods_code                  | 仕訳連携用商品コード                                                                                                                                                                                   | string |
| item_name                                       | 商品管理名                                                                                                                                                                                             | string |
| name                                            | 商品名                                                                                                                                                                                                 | string |
| demand_type                                     | 請求タイプ <br> 0：単発、1：定期定額、2：定期従量                                                                                                                                                      | int    |
| unit_price                                      | 単価                                                                                                                                                                                                   | string |
| unit                                            | 単位                                                                                                                                                                                                   | string |
| tax_category                                    | 税区分                                                                                                                                                                                                 | int    |
| tax_rate                                        | 消費税率                                                                                                                                                                                               | int    |
| remarks_column                                  | 備考欄                                                                                                                                                                                                 | string |
| repetition_period_number                        | 繰り返し周期_数字                                                                                                                                                                                      | int    |
| repetition_period_unit                          | 繰り返し周期_単位 <br> 1：月                                                                                                                                                                           | int    |
| repeat_count_max                                | 繰り返し回数                                                                                                                                                                                           | int    |
| period_format                                   | 対象期間形式 <br> 0：◯年◯月分1：◯年◯月◯日分2：◯年◯月～△年△月3：◯年◯月◯日～△年△月△日99：表示なし                                                                                                        | int    |
| period_value                                    | 対象期間                                                                                                                                                                                               | int    |
| period_unit                                     | 対象期間_単位 <br> 1：月                                                                                                                                                                               | int    |
| period_criterion                                | 対象期間_基準                                                                                                                                                                                          | int    |
| sales_recorded_date_month                       | 売上計上日_月 <br> -60～60                                                                                                                                                                             | int    |
| sales_recorded_date_day                         | 売上計上日_日 <br> 1：1日、2：2日、・・99：末日                                                                                                                                                        | int    |
| bill_issue_date_month                           | 請求書発行日_月 <br> -60～60                                                                                                                                                                           | int    |
| bill_issue_date_day                             | 請求書発行日_日 <br> 1：1日、2：2日、・・99：末日                                                                                                                                                      | int    |
| bill_sending_date_month                         | 請求書送付日_月 <br> -60～60                                                                                                                                                                           | int    |
| bill_sending_date_day                           | 請求書送付日_日 <br> 1：1日、2：2日、・・99：末日                                                                                                                                                      | int    |
| transfer_deadline_month                         | 決済期限_月 <br> -60～60                                                                                                                                                                               | int    |
| transfer_deadline_day                           | 決済期限_日 <br> 1：1日、2：2日、・・99：末日                                                                                                                                                          | int    |
| billing_method                                  | 請求方法 <br> 0:請求無し、1:自動メール、2:手動メール、3:自動郵送、4:手動郵送、5:自動メール＋自動郵送、6:手動メール＋手動郵送                                                                           | int    |
| bill_template_code                              | 請求書テンプレートID                                                                                                                                                                                   | int |
| account_title_code                              | 売上高勘定科目コード（勘定科目ID） <br> 売上高 ：4100、売上高2 ：4101、売上高3 ：4102、売上高4 ：4103、売上高5 ：4104、売上高6 ：4105、売上高7 ：4106、売上高8 ：4107、売上高9 ：4108、売上高10 ：4109 | int    |
| sub_account_title_code                          | 売上高補助科目コード                                                                                                                                                                                   | string |
| account_title_id_account_receivable_trade       | 売掛金勘定科目コード                                                                                                                                                                                   | int    |
| sub_account_title_code_account_receivable_trade | 売掛金補助科目コード                                                                                                                                                                                   | string |
| account_title_id_advances_received              | 前受金勘定科目コード                                                                                                                                                                                   | int    |
| sub_account_title_code_advances_received        | 前受金補助科目コード                                                                                                                                                                                   | string |
| [custom](#custom-response)                      | 商品カスタム項目に属するパラメータ           | `array` |

#### custom (response)

下記のような項目のオブジェクトを持つリスト

| 名前                      | 概要                                                                                                                      | 型     |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------- | ------ |
| error_code                | エラーコード <br> ※正常時はnull                                                                                            | int    |
| error_message             | エラーメッセージ <br> ※正常時はnull                                                                                        | string |
| number                    | カスタム項目番号                                                                                                            | int    |
| code                      | カスタム項目コード                                                                                                          | string |
| name                      | カスタム項目名                                                                                                              | string |
| value                     | カスタム項目値                                                                                                              | string |

## 使用例

### リクエスト例

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "goods": [
        {
            "item_number": 5,
            "item_code": "1234abc",
            "code": "54",
            "journal_cooperation_goods_code": "1234abc",
            "item_name": "商品管理名",
            "name": "商品A",
            "demand_type": 0,
            "unit_price": 1000,
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
            "bill_issue_date_month": 0,
            "bill_issue_date_day": 1,
            "bill_sending_date_month": 0,
            "bill_sending_date_day": 1,
            "transfer_deadline_month": 0,
            "transfer_deadline_day": 1,
            "sales_recorded_date_month": 0,
            "sales_recorded_date_day": 99,
            "billing_method": 0,
            "bill_template_code": 10010,
            "account_title_id": 4100,
            "sub_account_title_code": "1234abc",
            "account_title_id_account_receivable_trade": 1162,
            "sub_account_title_code_account_receivable_trade": "5678abc",
            "account_title_id_advances_received": 2111,
            "sub_account_title_code_advances_received": "1234cde",
            "custom":[
                {
                    "number": 15,
                    "value": "カスタム項目値登録"
                }
            ]
        }
    ]
}
```

### レスポンス例

Status: 200 OK

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
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
            "tax_rate": "1",
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
            "bill_template_code": "1",
            "account_title_code": 4100,
            "sub_account_title_code": "1234abc",
            "account_title_code_account_receivable_trade": 1162,
            "sub_account_title_code_account_receivable_trade": "5678abc",
            "account_title_code_advances_received": 2111,
            "sub_account_title_code_advances_received": "1234cde",
            "custom":[
                {
                    "error_code": null,
                    "error_message": null,
                    "number": 15,
                    "code": "mst_costom15",
                    "name": "カスタム項目１５",
                    "value": "カスタム項目値登録"
                },
                {
                    "error_code": null,
                    "error_message": null,
                    "number": 16,
                    "code": "mst_costom16",
                    "name": "カスタム項目１６",
                    "value": null
                }
            ]
        }
    ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                             |
| ------------ | -------------------------------- |
| 1801         | 商品番号が不正                   |
| 1802         | 商品コードが不正                 |
| 1803         | 集計用商品コードが不正           |
| 1804         | 仕訳連携用商品コードが不正       |
| 1805         | 商品管理名が不正                 |
| 1806         | 商品名が不正                     |
| 1807         | 請求タイプが不正                 |
| 1808         | 単価が不正                       |
| 1809         | 単位が不正                       |
| 1810         | 税区分が不正                     |
| 1811         | 消費税率が不正                   |
| 1812         | 備考欄が不正                     |
| 1813         | 繰り返し周期_数字が不正          |
| 1814         | 繰り返し周期_単位が不正          |
| 1815         | 繰り返し回数が不正               |
| 1816         | 対象期間形式が不正               |
| 1817         | 対象期間が不正                   |
| 1818         | 対象期間_単位が不正              |
| 1819         | 対象期間_基準が不正              |
| 1820         | 請求書発行日_月が不正            |
| 1821         | 請求書発行日_日が不正            |
| 1822         | 請求書送付日_月が不正            |
| 1823         | 請求書送付日_日が不正            |
| 1824         | 決済期限_月が不正                |
| 1825         | 決済期限_日が不正                |
| 1826         | 売上計上日_月が不正              |
| 1827         | 売上計上日_日が不正              |
| 1828         | 請求方法が不正                   |
| 1829         | 請求書テンプレートIDが不正       |
| 1830         | 売上高勘定科目コードが不正       |
| 1831         | 売上高補助科目コードが不正       |
| 1832         | 売掛金勘定科目コードが不正       |
| 1833         | 売掛金補助科目コードが不正       |
| 1834         | 前受金勘定科目コードが不正       |
| 1835         | 前受金補助科目コードが不正       |
| 1836         | 更新対象の商品が存在しません     |
| 1837         | 商品コードが既に存在しています。 |
| 1838         | カスタム項目情報のデータにエラーがあった場合              |
| 1839         | カスタム項目番号が不正                                  |
| 1840         | カスタム項目コードが不正                                |
| 1841         | カスタム項目値が不正                                   |
| 1842         | カスタム項目番号とカスタム項目コードは同時に指定できません |
| 1843         | 対象のカスタム項目情報が存在しません                    |
| 1844         | カスタム項目リクエスト件数が上限を超えています           |
| 1845         | カスタム項目情報にはarrayを指定してください             |
| 1846         | 商品登録更新に失敗しました                             |

----

[TOPへ戻る](../../index.md)