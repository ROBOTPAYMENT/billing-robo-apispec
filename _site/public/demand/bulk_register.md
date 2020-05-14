# 即時決済 請求書合算

`/api/demand/bulk_register`

複数の請求情報登録、合算請求書発行、クレジット決済処理まで行います。
決済手段がクレジットカードの場合のみに限られます。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/demand/bulk_register`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                  | 概要                                 | 桁数 | 種別                              | 必須 |
| --------------------- | ------------------------------------ | ---- | --------------------------------- | ---- |
| user_id               | ユーザーID（管理画面へのログインID） | 100  | [メール形式](../../index.md#種別) | 必須 |
| access_key            | アクセスキー                         | 100  | [半角英数](../../index.md#種別)   | 必須 |
| [bill](#bill-request) | 請求書に属するパラメータ             |      | `array`                           |      |

#### bill (request)

下記のような項目のオブジェクトを持つリスト

| 名前                               | 概要                                                                                                                      | 桁数 | 種別                                   | 必須     |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------- | ---- | -------------------------------------- | -------- |
| billing_code                       | 請求先コード                                                                                                              | 20   | [半角英数 + 記号](../../index.md#種別) | 必須     |
| billing_individual_number          | 請求先部署番号                                                                                                            | 20   | 数値                                   | (必須)^1 |
| billing_individual_code            | 請求先部署コード                                                                                                          | 20   | [半角英数 + 記号](../../index.md#種別) | (必須)^1 |
| billing_method                     | 請求方法 <br> 0:送付なし 1:自動メール 2:手動メール 3:自動郵送 <br> 4:手動郵送 5:自動メール+自動郵送 6:手動メール+手動郵送 | 1    | 数値                                   | 必須     |
| bill_template_code                 | 請求書テンプレートコード <br> 10000:基本テンプレート <br> 10010:シンプル  <br> ※合計請求書はご利用いただけません          | 20   | 数値                                   | 必須     |
| tax                                | 消費税率 <br> 画面上で選択できる消費税率のみ入力可能                                                                      | 2    | 数値                                   | 必須     |
| issue_date                         | 初回請求書発行日 <br> ※本日以前のみ有効                                                                                   | 10   | [日付形式](../../index.md#種別)        | 必須     |
| sending_date                       | 初回請求書送付日                                                                                                          | 10   | [日付形式](../../index.md#種別)        | 必須     |
| deadline_date                      | 初回決済期限 <br> ※本日以前のみ有効                                                                                       | 10   | [日付形式](../../index.md#種別)        | 必須     |
| bs_owner_code                      | 請求元担当者コード <br> ※両端のスペース除去                                                                               | 20   | [半角英数 + 記号](../../index.md#種別) |          |
| jb                                 | 決済処理方法 <br> 仮実同時売上:CAPTURE                                                                                    | 7    | 文字列                                 | 必須     |
| [bill.detail](#billdetail-request) | 請求書詳細に属するパラメータ                                                                                              |      | `array`                                |          |

#### bill.detail (request)

下記のような項目のオブジェクトを持つリスト

| 名前                     | 概要                                                                                                                              | 桁数        | 種別   | 必須                  |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------- | ----------- | ------ | --------------------- |
| demand_type              | 請求タイプ <br> 0:単発 1:定期定額 2:定期従量                                                                                      | 1           | 数値   | 必須                  |
| goods_code               | 商品コード                                                                                                                        | 33          | 文字列 |                       |
| link_goods_code          | 会計ソフト連携用商品コード                                                                                                        | 33          | 文字列 |                       |
| goods_name               | 商品名                                                                                                                            | 60          | 文字列 | 必須                  |
| price                    | 単価  <br> 両端のスペース除去 <br> ※クレジットカード決済のため桁数上限整数7桁                                                     | 整数7,小数4 | 数値   | (demand_type=0,1時)   |
| quantity                 | 数量  <br> 両端のスペース除去                                                                                                     | 整数6,小数2 | 数値   | (demand_type=0,1時)   |
| unit                     | 単位                                                                                                                              | 3           | 文字列 |                       |
| tax_category             | 税区分 <br> 0:外税 1:内税 2:対象外 3:非課税                                                                                       | 1           | 数値   | 必須                  |
| tax                      | 消費税率 <br> 画面上で選択できる消費税率のみ入力可能 <br> tax_category=0,1の場合のみ有効 <br> ※指定しない場合はbill.taxの値を参照 | 2           | 数値   |                       |
| remark                   | 備考                                                                                                                              | 60×17行     | 文字列 |                       |
| repetition_period_number | 繰返し周期 <br> 1～60                                                                                                            | 2           | 数値   | (demand_type=1,2時)   |
| repetition_period_unit   | 繰返し周期単位 <br> 1:月                                                                                                          | 1           | 数値   | (demand_type=1,2時)   |
| start_date               | サービス提供開始日 <br> yyyy/mm/dd                                                                                                | 10          | 日付   | 必須                  |
| repeat_count             | 繰返し回数 <br> 0:設定しない または1～60                                                                                          | 2           | 数値   | (demand_type=1,2時)   |
| period_format            | 対象期間形式 <br> 0：○年○月分 <br> 1：○年○月○日分 <br> 2：○年○月～○年○月 <br> 3：○年○月○日～○年○月○日 <br> 99：非表示             | 2           | 数値   | 必須                  |
| period_value             | 対象期間 <br> 入力可能値:1～60                                                                                                    | 2           | 数値   | (period_format=2,3時) |
| period_unit              | 対象期間単位 <br> 1:月                                                                                                            | 1           | 数値   | (period_format=3時)   |
| period_criterion         | 基準 <br> 0:対象期間開始日 1:対象期間終了日                                                                                       | 1           | 数値   | (period_format=2,3時) |



## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前 | 概要                     | 型       |
| ---- | ------------------------ | -------- |
| user | ユーザに属するパラメータ | `object` |


#### user


| 名前                      | 概要                     | 型      |
| ------------------------- | ------------------------ | ------- |
| user_id                   | ユーザID                 | string  |
| access_key                | アクセスキー             | string  |
| [demand](#demand-request) | 請求書に属するパラメータ | `array` |


#### demand (response)

下記のような項目のオブジェクトを持つリスト

| 名前                      | 概要                                                                                                                                                            | 型     |
| ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| demand                    | 請求情報に属するパラメータ                                                                                                                                      |        |
| billing_code              | 請求先コード                                                                                                                                                    | string |
| billing_name              | 請求先名                                                                                                                                                        | string |
| billing_individual_number | 請求先部署番号                                                                                                                                                  | int    |
| billing_individual_code   | 請求先部署コード                                                                                                                                                | string |
| billing_individual_name   | 請求先部署名                                                                                                                                                    | string |
| payment_method            | 決済手段 <br> 0:銀行振込 1:クレジットカード 2:バンクチェック <br> 3:RP口座振替 4:RL口座振替 5:その他口座振替 <br> 6:コンビニ払込票(A4) 7:コンビニ払込票(ハガキ) | int    |
| code                      | 請求情報番号                                                                                                                                                    | int    |
| type                      | 請求タイプ <br> 0:単発 1:定期定額 2:定期従量                                                                                                                    | int    |
| goods_code                | 商品コード                                                                                                                                                      | string |
| link_goods_code           | 会計ソフト連携用商品コード                                                                                                                                      | string |
| goods_name                | 商品名                                                                                                                                                          | string |
| price                     | 単価                                                                                                                                                            | string |
| quantity                  | 数量                                                                                                                                                            | string |
| unit                      | 単位                                                                                                                                                            | string |
| tax_category              | 税区分 <br> 0:外税 1:内税 2:対象外 3:非課税                                                                                                                     | int    |
| tax                       | 消費税率 <br> 画面上で選択できる消費税率のみ入力可能 <br> ※「tax_category = 3:対象外、4:非課税」の場合、空白                                                    | int    |
| withholding_tax           | 源泉所得税設定 <br> 0:無し 1:有り <br> ※請求情報登録画面上にこの項目が表示されていない場合は0固定。                                                             | int    |
| remark                    | 備考                                                                                                                                                            | string |
| billing_method            | 請求方法 <br> 0:送付なし 1:自動メール 2:手動メール 3:自動郵送 <br> 4:手動郵送 5:自動メール+自動郵送 6:手動メール+手動郵送                                       | int    |
| repetition_period_number  | 繰返し周期 <br> 1～60                                                                                                                                           | string |
| repetition_period_unit    | 繰返し周期単位 <br> 1:月                                                                                                                                        | string |
| start_date                | サービス提供開始日 <br> yyyy/mm/dd                                                                                                                              | date   |
| end_date                  | 最終サービス提供期間 <br> yyyy/mm/dd <br> ※repeat_count=0の場合はNULL                                                                                           | date   |
| repeat_count              | 繰返し回数 <br> 0:設定しない または1～60 <br> ※demand_type=1,2以外は"1"固定                                                                                     | string |
| period_format             | 対象期間形式 <br> 0：○年○月分 <br> 1：○年○月○日分 <br> 2：○年○月～○年○月 <br> 3：○年○月○日～○年○月○日 <br> 99：非表示                                           | int    |
| period_value              | 対象期間 <br> ※period_format=2,3以外はNULL                                                                                                                      | int    |
| period_unit               | 対象期間単位 <br> 1:月 <br> ※period_format=3以外はNULL                                                                                                          | string |
| period_criterion          | 基準 <br> 0:対象期間開始日 1:対象期間終了日 <br> ※period_format=2,3以外は0                                                                                      | string |
| issue_month               | 請求書発行日_月                                                                                                                                                 | int    |
| issue_day                 | 請求書発行日_日                                                                                                                                                 | int    |
| sending_month             | 請求書送付日_月                                                                                                                                                 | int    |
| sending_day               | 請求書送付日_日                                                                                                                                                 | int    |
| deadline_month            | 決済期限_月                                                                                                                                                     | int    |
| deadline_day              | 決済期限_日                                                                                                                                                     | int    |
| next_issue_date           | 次回請求書発行日 <br> yyyy/mm/dd <br> ※次回請求書が存在しない場合はNULL                                                                                         | date   |
| bill_template_code        | 請求書テンプレートコード                                                                                                                                        | int    |
| jb                        | 決済処理方法                                                                                                                                                    | string |
| bs_owner_code             | 請求元担当者コード                                                                                                                                              | string |



## 使用例

### リクエスト例

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "bill": [
        {
            "billing_code": "billing",
            "billing_individual_number": 1,
            "billing_method": 0,
            "bill_template_code": 10010,
            "tax": 8,
            "issue_date": "2014/11/11",
            "sending_date": "2014/11/12",
            "deadline_date": "2014/11/13",
            "bs_owner_code": "bs_owner_code",
            "jb": "CAPTURE",
            "bill_detail": [
                {
                    "demand_type": 0,
                    "goods_code": "goods",
                    "link_goods_code": "link_goods",
                    "goods_name": "商品",
                    "price": 1000,
                    "quantity": 1,
                    "unit": "円",
                    "tax_category": 0,
                    "remark": "備考",
                    "repetition_period_number": null,
                    "repetition_period_unit": null,
                    "start_date": "2014/11/11",
                    "repeat_count": null,
                    "period_format": 0,
                    "period_value": null,
                    "period_unit": null,
                    "period_criterion": 0
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
    "user": {
        "user_id": "sample@robotpayment.co.jp",
        "access_key": "xxxxxxxxxxxxxxxx",
        "demand": {
            "billing_code": "billing",
            "billing_name": "請求先名",
            "billing_individual_number": 1,
            "billing_individual_code": "bicd0001",
            "billing_individual_name": "請求先部署名",
            "payment_method": 0,
            "code": 1,
            "type": 0,
            "goods_code": "goods",
            "link_goods_code": "link_goods",
            "goods_name": "商品",
            "price": "1000.0000",
            "quantity": "1.00",
            "unit": "円",
            "tax_category": 0,
            "tax": 8,
            "withholding_tax": 0,
            "remark": "備考",
            "billing_method": 0,
            "repetition_period_number": null,
            "repetition_period_unit": null,
            "start_date": "2014/11/11",
            "end_date": "2014/11/30",
            "repeat_count": null,
            "period_format": 0,
            "period_value": null,
            "period_unit": null,
            "period_criterion": "0",
            "issue_month": 0,
            "issue_day": 1,
            "sending_month": 0,
            "sending_day": 1,
            "deadline_month": 0,
            "deadline_day": 1,
            "next_issue_date": "2014/11/01",
            "bill_template_code": 10010,
            "jb": "CAPTURE",
            "bs_owner_code": "bs_owner_code"
        }
    }
}

```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                                                                                                                                                                                                  |
| ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 201          | 請求先部署が存在しない                                                                                                                                                                                |
| 202          | 請求先部署の登録ステータスが不正                                                                                                                                                                      |
| 203          | 請求タイプが不正                                                                                                                                                                                      |
| 204          | 商品コードが不正                                                                                                                                                                                      |
| 205          | 会計ソフト連携用商品コードが不正                                                                                                                                                                      |
| 206          | 商品名が不正                                                                                                                                                                                          |
| 207          | 単価が不正                                                                                                                                                                                            |
| 208          | 数量が不正                                                                                                                                                                                            |
| 209          | 単位が不正                                                                                                                                                                                            |
| 210          | 税区分が不正                                                                                                                                                                                          |
| 211          | 消費税率が不正                                                                                                                                                                                        |
| 212          | 源泉所得税設定が不正                                                                                                                                                                                  |
| 213          | 備考が不正                                                                                                                                                                                            |
| 214          | 請求方法が不正                                                                                                                                                                                        |
| 215          | 繰返し周期が不正                                                                                                                                                                                      |
| 216          | 繰返し周期単位が不正                                                                                                                                                                                  |
| 217          | サービス提供開始日が不正                                                                                                                                                                              |
| 218          | 繰返し回数が不正                                                                                                                                                                                      |
| 219          | 対象期間形式が不正                                                                                                                                                                                    |
| 220          | 対象期間が不正                                                                                                                                                                                        |
| 221          | 対象期間単位が不正                                                                                                                                                                                    |
| 222          | 基準が不正                                                                                                                                                                                            |
| 223          | 請求書発行日_月が不正                                                                                                                                                                                 |
| 224          | 請求書発行日_日が不正                                                                                                                                                                                 |
| 225          | 請求書送付日_月が不正                                                                                                                                                                                 |
| 226          | 請求書送付日_日が不正                                                                                                                                                                                 |
| 227          | 決済期限_月が不正                                                                                                                                                                                     |
| 228          | 決済期限_日が不正                                                                                                                                                                                     |
| 229          | 請求書テンプレートコードが不正                                                                                                                                                                        |
| 230          | 請求情報登録に失敗                                                                                                                                                                                    |
| 231          | 請求先部署の決済手段がクレジット以外(即時決済APIのみ)                                                                                                                                                 |
| 232          | jbが不正(即時決済APIのみ)                                                                                                                                                                             |
| 233          | 請求書発行に失敗(即時決済APIのみ)                                                                                                                                                                     |
| 234          | クレジット決済に失敗(即時決済APIのみ)                                                                                                                                                                 |
| 235          | 請求元担当者コードが不正                                                                                                                                                                              |
| 236          | 仕訳作成に失敗(即時決済APIのみ)                                                                                                                                                                       |
| 237          | 請求先コードが不正                                                                                                                                                                                    |
| 238          | 請求先部署番号または請求先部署コードが不正                                                                                                                                                            |
| 239          | 請求書発行日が不正 <br> - 入力がない場合 <br> - [日付形式](../../index.md#種別)でない場合 <br> - リクエスト日より未来の場合 <br> - サービス提供開始月と次回請求書発行月の差の絶対値が60より大きい場合 <br> - 請求書が本日以前に2回以上発行される場合 |
| 240          | 請求書送付日が不正 <br> - 入力がない場合 <br> - [日付形式](../../index.md#種別)でない場合 <br> - サービス提供開始月と次回請求書送付月の差の絶対値が60より大きい場合                                   |
| 241          | 決済期限日が不正 <br> - 入力がない場合 <br> - [日付形式](../../index.md#種別)でない場合 <br> - リクエスト日より未来の場合 <br> - サービス提供開始月と次回決済期限月の差の絶対値が60より大きい場合     |
| 242          | 請求書発行件数が不正 <br> リクエストした請求書が1件より多い場合                                                                                                                                       |
| 243          | 既に締められた期間への請求書発行日は設定できません                                                                                                                                                    |
| 244          | 既に締められた期間への売上計上日の指定はできません                                                                                                                                                    |

----

[TOPへ戻る](../../index.md)