# 売上消込結果参照

`/api/v1.0/demand/search`

請求情報から作成された売上データの消込結果および売上データが記載された請求書の一覧を取得する。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/v1.0/demand/search`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                      | 概要                                 | 桁数 | 種別                              | 必須 |
| ------------------------- | ------------------------------------ | ---- | --------------------------------- | ---- |
| user_id                   | ユーザーID（管理画面へのログインID） | 100  | [メール形式](../../index.md#種別) | 必須 |
| access_key                | アクセスキー                         | 100  | [半角英数](../../index.md#種別)   | 必須 |
| [demand](#demand-request) | 請求情報に属するパラメータ           |      | `array`                   |      |


#### demand (request)

下記のような項目のオブジェクトを持つリスト

| 名前       | 概要                                      | 桁数 | 種別                                   | 必須     |
| ---------- | ----------------------------------------- | ---- | -------------------------------------- | -------- |
| number     | 請求情報番号  <br> ※両端のスペース除去    | 18   | 数値                                   | (必須)^1 |
| code       | 請求情報コード  <br> ※両端のスペース除去  | 20   | [半角英数 + 記号](../../index.md#種別) | (必須)^1 |
| goods_code | 集計用商品コード <br> ※両端のスペース除去 | 100  | 文字列                                 | (必須)^1 |


## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                       | 概要                                 | 型              |
| -------------------------- | ------------------------------------ | --------------- |
| user_id                    | ユーザーID（管理画面へのログインID） | string          |
| access_key                 | アクセスキー                         | string          |
| [demand](#demand-response) | 請求情報に属するパラメータ           | `array` |

#### demand (response)

下記のような項目のオブジェクトを持つリスト

| 名前                                  | 概要                                                                                                                      | 型                    |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| error_code                            | エラーコード <br> ※正常時はnull                                                                                           | int                    |
| error_message                         | エラーメッセージ <br> ※正常時はnull                                                                                       | string                |
| billing_code                          | 請求先コード                                                                                                              | string                |
| billing_individual_number             | 請求先部署番号                                                                                                            | int                   |
| billing_individual_code               | 請求先部署コード                                                                                                          | string                |
| payment_method_code                   | 決済情報コード                                                                                                            | string                |
| number                                | 請求情報番号                                                                                                              | int                   |
| code                                  | 請求情報コード                                                                                                            | string                |
| item_code                             | 商品コード                                                                                                                | string                |
| type                                  | 請求タイプ <br> 0:単発 1:定期定額 2:定期従量                                                                              | int                   |
| goods_code                            | 集計用商品コード                                                                                                      | string                |
| link_goods_code                       | 会計ソフト連携用商品コード                                                                                                | string                |
| goods_name                            | 商品名                                                                                                                    | string                |
| price                                 | 単価                                                                                                                      | int                   |
| quantity                              | 数量                                                                                                                      | int                   |
| unit                                  | 単位                                                                                                                      | string                |
| tax_category                          | 税区分 <br> 0:外税 1:内税 2:対象外 3:非課税                                                                               | int                   |
| tax                                   | 消費税率                                                                                                                  | int                   |
| remark                                | 備考                                                                                                                      | string                |
| billing_method                        | 請求方法 <br> 0:送付なし 1:自動メール 2:手動メール 3:自動郵送 <br> 4:手動郵送 5:自動メール+自動郵送 6:手動メール+手動郵送 | int                   |
| repetition_period_number              | 繰返し周期                                                                                                                | int                   |
| repetition_period_unit                | 繰返し周期単位                                                                                                            | int                   |
| start_date                            | サービス提供開始日 <br> yyyy/mm/dd                                                                                        | date                  |
| end_date                              | 最終サービス提供期間 <br> yyyy/mm/dd <br> ※repeat_count=0の場合はnull                                                     | date                  |
| repeat_count                          | 繰返し回数                                                                                         | int                   |
| repeat_count_limit                    | 繰返し残回数 <br> ※type=1,2以外はnull <br> ※type=1,2で繰返し回数が「0:設定しない」の場合、null                            | int                   |
| period_format                         | 対象期間形式 <br> 0：○年○月分 <br> 1：○年○月○日分 <br> 2：○年○月～○年○月 <br> 3：○年○月○日～○年○月○日 <br> 99：非表示     | int                   |
| period_value                          | 対象期間 <br> ※period_format=2,3以外はnull                                                                                | int                   |
| period_unit                           | 対象期間単位 <br> 1:月 <br> ※period_format=3以外はnull                                                                    | int                   |
| period_criterion                      | 基準 <br> 0:対象期間開始日 1:対象期間終了日 <br> ※period_format=2,3以外は0                                                | int                   |
| sales_recorded_month                  | 売上計上日_月                                                                                                             | int                   |
| sales_recorded_day                    | 売上計上日_日                                                                                                             | int                   |
| issue_month                           | 請求書発行日_月                                                                                                           | int                   |
| issue_day                             | 請求書発行日_日                                                                                                           | int                   |
| sending_month                         | 請求書送付日_月                                                                                                           | int                   |
| sending_day                           | 請求書送付日_日                                                                                                           | int                   |
| deadline_month                        | 決済期限_月                                                                                                               | int                   |
| deadline_day                          | 決済期限_日                                                                                                               | int                   |
| slip_deadline_month                   | 払込票有効期限_月                                                                                                         | int                   |
| slip_deadline_day                     | 払込票有効期限_日                                                                                                         | int                   |
| next_period_start_date                | 次回対象期間開始日                                                                                                        | date                  |
| next_period_end_date                  | 次回対象期間終了日                                                                                                        | date                  |
| next_sales_recorded_date              | 次回売上計上⽇ <br> ※売上計上日を指定しない場合はnull                                                                      | date                  |
| next_issue_date                       | 次回請求書発行日                                                                                                          | date                  |
| next_sending_date                     | 次回請求書送付⽇                                                                                                           | date                  |
| next_deadline_date                    | 次回決済期限                                                                                                              | date                  |
| next_slip_deadline_date               | 次回払込票有効期限                                                                                                        | date                  |
| next_sales_make_date                  | 次回売上データ作成⽇                                                                                                       | date                  |
| memo                                  | メモ                                                                                                                      | string                |
| bill_template_code                    | 請求書テンプレートコード                                                                                                  | int                   |
| bs_owner_code                         | 請求元担当者コード                                                                                                        | string                |
| account_title_code                    | 勘定科目コード                                                                                                            | string                |
| text_pattern_code                     | 文章パターンコード                                                                                                        | string                |
| url                                   | 請求情報詳細画面URL                                                                                                       | string                |
| created                               | 登録日時                                                                                                                  | datetime              |
| modified                              | 更新日時                                                                                                                  | datetime              |
| status                                | 状態 <br> 0:削除 1:有効 2:無効                                                                                            | int                   |
| [demand.sales](#demandsales-response) | 売上データに属するパラメータ                                                                                              | `array` |

#### demand.sales (response)

下記のような項目のオブジェクトを持つリスト

| 名前                                               | 概要                                                                                                                                                                                        | 型              |
| -------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------- |
| billing_code                                       | 請求先コード                                                                                                                                                                                | string          |
| billing_individual_number                          | 請求先部署番号                                                                                                                                                                              | int             |
| billing_individual_code                            | 請求先部署コード                                                                                                                                                                            | string          |
| item_code                                          | 商品コード                                                                                                                                                                                  | string          |
| goods_code                                         | 集計用商品コード                                                                                                                                                                        | string          |
| link_goods_code                                    | 会計ソフト連携用商品コード                                                                                                                                                                  | string          |
| goods_name                                         | 商品名                                                                                                                                                                                      | string          |
| price                                              | 単価                                                                                                                                                                                        | int             |
| quantity                                           | 数量                                                                                                                                                                                        | int             |
| unit                                               | 単位                                                                                                                                                                                        | string          |
| tax_category                                       | 税区分 <br> 0:外税 1:内税 2:対象外 3:非課税                                                                                                                                                 | int             |
| tax                                                | 消費税率                                                                                                                                                                                    | int             |
| tax_amount                                         | 消費税額                                                                                                                                                                                    | int             |
| remark                                             | 備考                                                                                                                                                                                        | string          |
| billing_method                                     | 請求方法 <br> 0:送付なし 1:自動メール 2:手動メール 3:自動郵送 <br> 4:手動郵送 5:自動メール+自動郵送 6:手動メール+手動郵送                                                                   | int             |
| period_date_start                                  | 請求対象期間開始日                                                                                                                                                                          | date            |
| period_date_end                                    | 請求対象期間終了日                                                                                                                                                                          | date            |
| payment_method                                     | 決済手段 <br> 0:銀行振込 1:クレジットカード 2:バンクチェック <br> 3:RP口座振替 4:RL口座振替 5:その他口座振替 <br> 6:コンビニ払込票(A4) 7:コンビニ払込票(ハガキ) <br> 8:その他コンビニ払込票 | int             |
| total_amount                                       | 売上金額                                                                                                                                                                                    | int             |
| unclearing_amount                                  | 未消込金額                                                                                                                                                                                  | int             |
| recorded_date                                      | 売上計上日                                                                                                                                                                                  | date            |
| issue_date                                         | 請求書発行日                                                                                                                                                                                | date            |
| sending_date                                       | 請求書送付日                                                                                                                                                                                | date            |
| deadline_date                                      | 決済期限                                                                                                                                                                                    | date            |
| slip_deadline                                      | 払込票有効期限                                                                                                                                                                              | int             |
| erasure_status                                     | 消込ステータス <br> 0:未処理 1:完了 2:確認済み 3:未収 4:貸倒 5:手数料 6:現金 7:長期滞留債権 8:破産更生債権 9:売上取消 10:繰越                                                               | int             |
| bill_issue_flg                                     | 請求書発行フラグ <br> 0:未発行 1:発行済み                                                                                                                                                   | int             |
| memo                                               | メモ                                                                                                                                                                                        | string          |
| bill_template_code                                 | 請求書テンプレートコード                                                                                                                                                                    | int             |
| bs_owner_code                                      | 請求元担当者コード                                                                                                                                                                          | string          |
| carryover_status                                   | 繰越ステータス <br> 0:対象外 1:繰越待ち 2:繰越完了                                                                                                                                          | int             |
| carryover_transit_bill_number                      | 繰越先請求書番号                                                                                                                                                                            | string          |
| carryover_total_amount                             | 繰越金額                                                                                                                                                                                    | int             |
| url                                                | 売上詳細画面URL                                                                                                                                                                             | string          |
| created                                            | 登録日時                                                                                                                                                                                    | datetime        |
| modified                                           | 更新日時                                                                                                                                                                                    | datetime        |
| status                                             | 状態 <br> 0:削除 1:有効 2:無効                                                                                                                                                              | int             |
| [demand.sales.bill](#demandsalesbill-response)     | 請求書に属するパラメータ                                                                                                                                                                    | `array`   |
| [demand.sales.result](#demandsalesresult-response) | 消込結果明細に属するパラメータ                                                                                                                                                              | `array` |

#### demand.sales.bill (response)

下記のような項目のオブジェクトを持つリスト

| 名前                      | 概要                                                                                                                                                                                        | 型       |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| number                    | 請求書番号                                                                                                                                                                                  | string   |
| billing_code              | 請求先コード                                                                                                                                                                                | string   |
| billing_individual_number | 請求先部署番号                                                                                                                                                                              | int      |
| billing_individual_code   | 請求先部署コード                                                                                                                                                                            | string   |
| zip_code                  | 郵便番号                                                                                                                                                                                    | int      |
| pref                      | 都道府県                                                                                                                                                                                    | string   |
| city_address              | 市区町村番地                                                                                                                                                                                | string   |
| building_name             | 建物名                                                                                                                                                                                      | string   |
| tel                       | 電話番号                                                                                                                                                                                    | string   |
| mail                      | メールアドレス                                                                                                                                                                              | string   |
| demand_count              | 請求件数 <br> ※請求書明細の件数                                                                                                                                                             | int      |
| subtotal_amount           | 小計                                                                                                                                                                                        | int      |
| tax_amount                | 消費税額                                                                                                                                                                                    | int      |
| total_amount              | 合計                                                                                                                                                                                        | int      |
| unclearing_amount         | 未消込金額                                                                                                                                                                                  | int      |
| message_column            | 通信欄                                                                                                                                                                                      | string   |
| billing_method            | 請求方法 <br> 0:送付なし 1:自動メール 2:手動メール 3:自動郵送 <br> 4:手動郵送 5:自動メール+自動郵送 6:手動メール+手動郵送                                                                   | int      |
| type                      | 請求書タイプ <br> 1：請求 2：繰越請求 3：親請求 4：子請求                                                                                                                                   | int      |
| payment_method            | 決済手段 <br> 0:銀行振込 1:クレジットカード 2:バンクチェック <br> 3:RP口座振替 4:RL口座振替 5:その他口座振替 <br> 6:コンビニ払込票(A4) 7:コンビニ払込票(ハガキ) <br> 8:その他コンビニ払込票 | int      |
| issue_date                | 請求書発行日                                                                                                                                                                                | date     |
| sending_date              | 請求書送付日                                                                                                                                                                                | date     |
| deadline_date             | 決済期限                                                                                                                                                                                    | date     |
| slip_deadline             | 払込票有効期限                                                                                                                                                                              | date     |
| payment_date              | 決済日                                                                                                                                                                                      | date     |
| confirm_date              | 請求書確認日                                                                                                                                                                                | date     |
| mail_status               | メールステータス <br> 0：未送信 1：送信済み                                                                                                                                                 | int      |
| post_status               | 郵送ステータス <br> 0：未郵送 1：郵送済み                                                                                                                                                   | int      |
| payment_status            | 消込ステータス <br> 0:未処理 1:完了 2:確認済み 3:未収 4:貸倒 5:手数料 6:現金 7:長期滞留債権 8:破産更生債権 9:売上取消                                                                       | int      |
| ec                        | 決済結果エラーコード                                                                                                                                                                        | string   |
| memo                      | メモ                                                                                                                                                                                        | string   |
| bill_template_code        | 請求書テンプレートコード                                                                                                                                                                    | int      |
| bs_owner_code             | 請求元担当者コード                                                                                                                                                                          | string   |
| url                       | 請求書詳細画面URL                                                                                                                                                                           | string   |
| created                   | 登録日時                                                                                                                                                                                    | datetime |
| modified                  | 更新日時                                                                                                                                                                                    | datetime |
| status                    | 状態 <br> 0:削除 1:有効 2:無効                                                                                                                                                              | int      |


#### demand.sales.result (response)

下記のような項目のオブジェクトを持つリスト

| 名前                  | 概要                                                                                                                                                                                                                                                                                                                                                                             | 型       |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| type                  | 消込手段 <br> 0：銀行振込 <br> 1：クレジットカード <br> 2：バンクチェック <br> 3：RP口座振替 <br> 4：RL口座振替 <br> 5：その他口座振替 <br> 6：コンビニ払込票(A4) <br> 7：コンビニ払込票(ハガキ) <br> 8：その他コンビニ払込票 <br> 9:バーチャル口座 <br> 10:その他決済手段 1 <br> 11:その他決済手段 2 <br> 12:その他決済手段 3 <br> 13:その他決済手段 4 <br> 14:その他決済手段 5 <br> 99：相殺 <br> 101：貸倒 <br> 102：確認済み <br> 103：手数料 <br> 106：現金 <br> 107：長期滞留債権 <br> 108：破産更生等債権 <br> 109：売上取消 | int      |
| amount                | 消込金額                                                                                                                                                                                                                                                                                                                                                                         | int      |
| deposit_recorded_date | 消込計上日                                                                                                                                                                                                                                                                                                                                                                       | date     |
| cancel_flg            | 取消フラグ <br> 0:未取消 1:取消済み                                                                                                                                                                                                                                                                                                                                              | int      |
| cancel_recorded_date  | 取消計上日                                                                                                                                                                                                                                                                                                                                                                       | date     |
| billing_number        | 請求書番号                                                                                                                                                                                                                                                                                                                                                                       | string   |
| url                   | 入金詳細画面URL                                                                                                                                                                                                                                                                                                                                                                  | string   |
| status                | 状態 <br> 0:削除 1:有効 2:無効                                                                                                                                                                                                                                                                                                                                                   | int      |
| created               | 登録日時                                                                                                                                                                                                                                                                                                                                                                         | datetime |
| modified              | 更新日時                                                                                                                                                                                                                                                                                                                                                                         | datetime |


## 使用例

### リクエスト例

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "demand": [
        {
            "number": 1
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
    "demand": [
        {
            "error_code": null,
            "error_message": null,
            "billing_code": "billing",
            "billing_individual_number": 1,
            "billing_individual_code": "code",
            "payment_method_code": "bank001",
            "number": 1,
            "code": "code",
            "item _code": null,
            "type": 0,
            "goods_code": null,
            "link_goods_code": null,
            "goods_name": "商品1",
            "price": "1000.0000",
            "quantity": "1.00",
            "unit": null,
            "tax_category": 0,
            "tax": 8,
            "remark": null,
            "billing_method": 0,
            "repetition_period_number": null,
            "repetition_period_unit": null,
            "start_date": "2020/11/27",
            "end_date": "2020/11/30",
            "repeat_count": 1,
            "repeat_count_limit": null,
            "period_format": 0,
            "period_value": null,
            "period_unit": null,
            "period_criterion": "0",
            "sales_recorded_month": null,
            "sales_recorded_day": null,
            "issue_month": 1,
            "issue_day": 1,
            "sending_month": 1,
            "sending_day": 25,
            "deadline_month": 1,
            "deadline_day": 99,
            "slip_deadline_month": null,
            "slip_deadline_day": null,
            "next_period_start_date": "2020/11/01",
            "next_period_end_date": "2020/11/30",
            "next_sales_recorded_date": null,
            "next_issue_date": null,
            "next_sending_date": null,
            "next_deadline_date": null,
            "next_slip_deadline_date": null,
            "next_sales_make_date": null,
            "memo": null,
            "bill_template_code": 10010,
            "bs_owner_code": null,
            "account_title_code": "4100",
            "text_pattern_code": null,
            "url": "https://billing-roboxxx/demand/detail/1",
            "created": "2020/11/01 00:00:00",
            "modified": "2020/12/01 00:00:00",
            "status": 1,
            "sales": [
                {
                    "billing_code": "billing",
                    "billing_individual_number": 1,
                    "billing_individual_code": "billing",
                    "item_code": null,
                    "goods_code": null,
                    "link_goods_code": null,
                    "goods_name": "商品1",
                    "price": "1000.0000",
                    "quantity": "1.00",
                    "unit": null,
                    "tax_category": 0,
                    "tax": 8,
                    "tax_amount": 80,
                    "remark": null,
                    "billing_method": 0,
                    "period_date_start": "2020/11/01",
                    "period_date_end": "2020/11/30",
                    "payment_method": 0,
                    "total_amount": 1080,
                    "unclearing_amount": 0,
                    "recorded_date": "2020/11/30",
                    "issue_date": "2020/12/01",
                    "sending_date": "2020/12/25",
                    "deadline_date": "2020/12/31",
                    "slip_deadline": "2020/12/31",
                    "erasure_status": 1,
                    "bill_issue_flg": 1,
                    "memo": "",
                    "bill_template_code": "10010",
                    "bs_owner_code": null,
                    "url": "https://billing-roboxxx/sales/detail/1",
                    "created": "2020/12/01 00:00:00",
                    "modified": "2020/12/01 00:00:00",
                    "status": 1,
                    "bill": [
                        {
                            "number": "202011-billing-1",
                            "billing_code": "billing",
                            "billing_individual_number": 1,
                            "billing_individual_code": "billing",
                            "zip_code": "1651111",
                            "pref": "東京都",
                            "city_address": "渋谷区",
                            "building_name": "ビル3階",
                            "tel": "0300000000",
                            "mail": "sample@robotpayment.co.jp",
                            "demand_count": 1,
                            "subtotal_amount": 1000,
                            "tax_amount": 80,
                            "total_amount": 1080,
                            "unclearing_amount": 0,
                            "message_column": null,
                            "billing_method": 0,
                            "type": 1,
                            "payment_method": 0,
                            "issue_date": "2020/11/01",
                            "sending_date": null,
                            "deadline_date": "2020/11/30",
                            "slip_deadline": "2020/11/30",
                            "payment_date": "2020/11/01",
                            "confirm_date": null,
                            "mail_status": 0,
                            "post_status": 0,
                            "payment_status": 1,
                            "carryover_status": 0,
                            "carryover_transit_bill_number": null,
                            "carryover_total_amount": 0,
                            "ec": null,
                            "memo": null,
                            "bill_template_code": "10010",
                            "bs_owner_code": null,
                            "url": "https://billing-roboxxx/bill/detail/1",
                            "created": "2020/12/01 00:00:00",
                            "modified": "2020/12/01 00:00:00",
                            "status": 1
                        }
                    ],
                    "result": [
                        {
                            "type": 0,
                            "amount": 1080,
                            "deposit_recorded_date": "2020/12/01",
                            "cancel_flg": 0,
                            "cancel_recorded_date": null,
                            "billing_number": null,
                            "url": "https://billing-roboxxx/clearing/deposit_detail/1",
                            "status": 1,
                            "created": "2020/12/01 00:00:00",
                            "modified": "2020/12/01 00:00:00"
                        }
                    ]
                }
            ]
        }
    ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                                                      |
| ------------ | --------------------------------------------------------- |
| 1501         | 請求情報番号が不正                                        |
| 1502         | 請求情報コードが不正                                      |
| 1503         | 集計用商品コードが不正                                    |
| 1504         | 請求情報を指定するパラメータは2つ以上同時に指定できません |
| 1505         | リクエストパラメータに請求情報が存在しない                |
| 1506         | 請求情報が存在しません                                    |

----

[TOPへ戻る](../../index.md)