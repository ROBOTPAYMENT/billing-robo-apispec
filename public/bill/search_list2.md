# 請求書参照2

`/api/v1.0/bill/search_list2`

請求書の参照処理を実行します。
請求書参照とは、レスポンスについて単価・数量・消費税の型が異なります。

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/v1.0/bill/search_list2`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                          | 概要                                                                                                                                                            | 桁数 | 種別                              | 必須                                                 |
| ----------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---- | --------------------------------- | ---------------------------------------------------- |
| user_id                       | ユーザーID（管理画面へのログインID）                                                                                                                            | 100  | [半角英数*1](/README.md#種別注釈) | 必須                                                 |
| access_key                    | アクセスキー                                                                                                                                                    | 100  | [半角英数*3](/README.md#種別注釈) | 必須                                                 |
| limit_count                   | 請求書取得件数 <br> ※0〜200の数値を設定する。省略した場合、20が設定される                                                                                       | 3    | 数値                              |                                                      |
| page_count                    | 請求書取得開始インデックス <br> ※0以上の数値を設定する。省略した場合、0が設定される                                                                             | 2    | 数値                              |                                                      |
| bill                          | 請求書に属するパラメータ                                                                                                                                        |      |                                   |                                                      |
| number                        | 請求書番号                                                                                                                                                      | 100  | [半角英数*4](/README.md#種別注釈) |                                                      |
| billing_code                  | 請求先コード                                                                                                                                                    | 20   | [半角英数*4](/README.md#種別注釈) | (請求先部署番号指定時) <br> (請求先部署コード指定時) |
| billing_individual_number     | 請求先部署番号                                                                                                                                                  | 20   | 数値                              |                                                      |
| billing_individual_code       | 請求先部署コード                                                                                                                                                | 20   | [半角英数*4](/README.md#種別注釈) |                                                      |
| issue_start_date              | 発行日の検索開始日                                                                                                                                              | 10   | 日付                              | [(必須)^1](/README.md#必須)                          |
| issue_stop_date               | 発行日の検索終了日                                                                                                                                              | 10   | 日付                              | [(必須)^1](/README.md#必須)                          |
| update_start_date             | 更新日の検索開始日                                                                                                                                              | 10   | 日付                              | [(必須)^1](/README.md#必須)                          |
| update_stop_date              | 更新日の検索終了日                                                                                                                                              | 10   | 日付                              | [(必須)^1](/README.md#必須)                          |
| deadline_start_date           | 決済期限の検索開始日                                                                                                                                            | 10   | 日付                              | [(必須)^1](/README.md#必須)                          |
| deadline_stop_date            | 決済期限の検索終了日                                                                                                                                            | 10   | 日付                              | [(必須)^1](/README.md#必須)                          |
| payment_method                | 決済手段 <br> 0:銀行振込 1:クレジットカード 2:バンクチェック <br> 3:RP口座振替 4:RL口座振替 5:その他口座振替 <br> 6:コンビニ払込票(A4) 7:コンビニ払込票(ハガキ) | 2    | 数値                              |                                                      |
| goods_code                    | 集計用商品区分コード                                                                                                                                            | 33   | 文字列                            |                                                      |
| bs_owner_code                 | 請求元担当者コード <br> ※両端のスペース除去                                                                                                                     | 20   | [半角英数*4](/README.md#種別注釈) |                                                      |
| payment_status                | 消込ステータス <br> 0:未処理 1:完了 2:確認済み 3:未収 4:貸倒 5:手数料 6:現金 7:長期滞留債権 8:破産更生債権 9:売上取消 10:繰越                                   | 2    | 数値                              |                                                      |
| carryover_status              | 繰越ステータス <br> 0:対象外 1:繰越待ち 2:繰越完了                                                                                                              | 2    | 数値                              |                                                      |
| carryover_transit_bill_number | 繰越先請求書番号                                                                                                                                                | 100  | [半角英数*4](/README.md#種別注釈) |                                                      |

## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                           | 概要                                                                                                                                                            | 型     |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| user_id                        | ユーザーID                                                                                                                                                      | string |
| access_key                     | アクセスキー                                                                                                                                                    | string |
| error_code                     | エラーコード <br> ※正常時はnull                                                                                                                                 | string |
| error_message                  | エラーメッセージ <br> ※正常時はnull                                                                                                                             | string |
| limit_count                    | 請求書取得件数 <br> ※最大件数は、リクエストで指定された「請求書取得件数」に依存                                                                                 | int    |
| page_count                     | 請求書取得開始インデックス <br> ※取得した請求書の開始インデックスを返却する                                                                                     | int    |
| total_page_count               | 請求書取得開始インデックス合計 <br> ※指定された検索条件によって取得可能な請求書の全件数／請求書取得件数によって、算出される値を返却する                         | int    |
| bill                           | 請求書に属するパラメータ                                                                                                                                        |        |
| number                         | 請求書番号                                                                                                                                                      | string |
| bs_residence_code              | 請求元差出人コード                                                                                                                                              | string |
| billing_code                   | 請求先コード                                                                                                                                                    | string |
| billing_name                   | 請求先名                                                                                                                                                        | string |
| billing_individual_number      | 請求先部署番号                                                                                                                                                  | string |
| billing_individual_code        | 請求先部署コード                                                                                                                                                | string |
| billing_individual_name        | 請求先部署名                                                                                                                                                    | string |
| demand_number                  | 請求件数                                                                                                                                                        | string |
| subtotal_amount_billed         | 請求金額小計                                                                                                                                                    | int    |
| consumption_tax_amount         | 消費税額                                                                                                                                                        | int    |
| total_amount_billed            | 請求金額合計                                                                                                                                                    | int    |
| unclearing_amount              | 未消込金額                                                                                                                                                      | int    |
| message_column                 | 通信欄                                                                                                                                                          | string |
| billing_method                 | 請求方法 <br> 0：送付なし、1：自動メール、2：手動メール、3：自動郵送、4：手動郵送、5：自動メール+自動郵送、6：手動メール+手動郵送                               | int    |
| issue_date                     | 請求書発行日                                                                                                                                                    | string |
| make_date                      | 請求書作成日                                                                                                                                                    | string |
| sending_scheduled_date         | 請求書送付予定日                                                                                                                                                | string |
| sending_date                   | 請求書送付日                                                                                                                                                    | string |
| update_date                    | 請求書更新日                                                                                                                                                    | string |
| confirm_date                   | 請求書確認日                                                                                                                                                    | string |
| mail_send_flg                  | メールステータス <br> 0：未送信 1：送信済み                                                                                                                     | int    |
| post_send_flg                  | 郵送ステータス <br> 0：未送信 1：送信済み                                                                                                                       | int    |
| payment_method                 | 決済手段 <br> 0:銀行振込 1:クレジットカード 2:バンクチェック <br> 3:RP口座振替 4:RL口座振替 5:その他口座振替 <br> 6:コンビニ払込票(A4) 7:コンビニ払込票(ハガキ) | int    |
| payment_method_number          | 決済情報番号                                                                                                                                                    | string |
| payment_method_code            | 決済情報コード                                                                                                                                                  | string |
| payment_method_name            | 決済情報名                                                                                                                                                      | string |
| settlement_result              | 決済連携ステータス <br> 0：申請中、1：送信失敗、2：決済成功、3：決済失敗                                                                                        | int    |
| transfer_deadline              | 決済期限                                                                                                                                                        | string |
| slip_deadline                  | 払込票有効期限                                                                                                                                                  | string |
| transfer_date                  | 決済日                                                                                                                                                          | string |
| payment_status                 | 消込ステータス <br> 0:未処理 1:完了 2:確認済み 3:未収 4:貸倒 5:手数料 6:現金 7:長期滞留債権 8:破産更生債権 9:売上取消 10:繰越                                   | int    |
| memo                           | メモ                                                                                                                                                            | string |
| template_code                  | 請求書テンプレート                                                                                                                                              | string |
| bs_department_code             | 請求元部署コード                                                                                                                                                | string |
| bs_department_name             | 請求元部署名                                                                                                                                                    | string |
| bs_owner_code                  | 請求元担当者コード                                                                                                                                              | string |
| bs_owner_name                  | 請求元担当者名                                                                                                                                                  | string |
| valid_flg                      | 請求書有効フラグ <br> 0:無効1:有効                                                                                                                              | int    |
| delete_flg                     | 請求書削除フラグ <br> 0:未削除 1:削除 <br> ※現行削除機能は未実装のため未削除が返却されます。                                                                    | int    |
| gid                            | 決済ID                                                                                                                                                          | int    |
| carryover_status               | 繰越ステータス <br> 0:対象外 1:繰越待ち 2:繰越完了                                                                                                              | int    |
| carryover_transit_bill_number  | 繰越先請求書番号                                                                                                                                                | string |
| carryover_total_amount         | 繰越金額                                                                                                                                                        | int    |
| download_url                   | 請求書ダウンロードURL                                                                                                                                           | string |
| bill_group_key                 | 請求書合算キー                                                                                                                                                  | string |
| bill.bill_detail               | 請求書詳細に属するパラメータ                                                                                                                                    |        |
| demand_number                  | 請求情報番号                                                                                                                                                    | string |
| demand_code                    | 請求情報コード                                                                                                                                                  | string |
| item_number                    | 商品番号                                                                                                                                                        | string |
| item_code                      | 商品コード                                                                                                                                                      | string |
| goods_name                     | 商品名                                                                                                                                                          | int    |
| pattern_period_format          | 対象期間形式 <br> 0：○年○月分 <br> 1：○年○月○日分 <br> 2：○年○月～○年○月 <br> 3：○年○月○日～○年○月○日 <br> 99：非表示                                           | string |
| demand_start_date              | 対象期間_開始                                                                                                                                                   | string |
| demand_end_date                | 対象期間_終了                                                                                                                                                   | string |
| criterion_date                 | 基準日                                                                                                                                                          | string |
| link_customer_code             | 仕訳連携用取引先コード                                                                                                                                          | string |
| goods_code                     | 集計用商品区分コード                                                                                                                                            | string |
| link_goods_code                | 仕訳連携用商品コード                                                                                                                                            | int    |
| unit_price                     | 単価                                                                                                                                                            | string |
| quantity                       | 数量                                                                                                                                                            | string |
| unit                           | 単位                                                                                                                                                            | string |
| tax_category                   | 税区分 <br> 0:外税 1:内税 2:対象外 3:非課税                                                                                                                     | int    |
| consumption_tax                | 消費税                                                                                                                                                          | int    |
| subtotal_amount_billed         | 請求金額小計                                                                                                                                                    | int    |
| consumption_tax_amount         | 消費税額                                                                                                                                                        | int    |
| total_amount_billed            | 請求金額合計                                                                                                                                                    | int    |
| unclearing_amount              | 未消込金額                                                                                                                                                      | string |
| remark                         | 備考                                                                                                                                                            | string |
| recorded_date                  | 売上計上日                                                                                                                                                      | string |
| payment_method                 | 決済情報名                                                                                                                                                      | string |
| payment_status                 | 消込ステータス                                                                                                                                                  | int    |
| carryover_flg                  | 繰越フラグ <br> 0:通常請求書明細 1:繰越請求書明細                                                                                                               | int    |
| carryover_original_bill_number | 繰越元請求書番号                                                                                                                                                | string |
| valid_flg                      | 売上有効フラグ <br> 0:無効1:有効                                                                                                                                | int    |
| delete_flg                     | 売上削除フラグ <br> 0:未削除 1:削除 <br> ※現行削除機能は未実装のため未削除が返却されます。                                                                      | string |


## 使用例

### リクエスト

```
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "payment_status": "",
    "limit_count": "",
    "page_count": "",
    "bill": [
        {
            "number": "",
            "billing_code": "",
            "billing_individual_number": "",
            "billing_individual_code": "",
            "issue_start_date": "2016/04/01",
            "issue_stop_date": "2017/07/31",
            "update_start_date": "",
            "update_stop_date": "",
            "deadline_start_date": "",
            "deadline_stop_date": "",
            "payment_method": "",
            "goods_code": "",
            "bs_owner_code": ""
        }
    ]
}
```

### レスポンス

Status: 200 OK

```
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "limit_count": 20,
    "page_count": 0,
    "total_page_count": 0,
    "bill": [
        {
            "error_code": null,
            "error_message": null,
            "number": "201705-banche-1",
            "bs_residence_code": "1",
            "billing_code": "banche",
            "billing_name": "株式001",
            "billing_individual_number": 1,
            "billing_individual_code": "",
            "billing_individual_name": "経理",
            "demand_number": "1",
            "subtotal_amount_billed": 5000,
            "consumption_tax_amount": 0,
            "total_amount_billed": 5000,
            "unclearing_amount": 0,
            "message_column": null,
            "billing_method": 0,
            "issue_date": "2017/03/31",
            "make_date": "2017/05/16",
            "sending_scheduled_date": "2017/04/01",
            "sending_date": null,
            "update_date": "2017/05/16",
            "confirm_date": null,
            "mail_send_flg": 0,
            "post_send_flg": 0,
            "payment_method": 2,
            "payment_method_number": "1",
            "payment_method_code": "",
            "payment_method_name": "バンクチェック",
            "settlement_result": 2,
            "transfer_deadline": "2017/04/30",
            "slip_deadline": "2017/04/30",
            "transfer_date": "2016/07/06",
            "payment_status": 1,
            "memo": null,
            "template_code": "10010",
            "bs_department_code": null,
            "bs_department_name": null,
            "bs_owner_code": null,
            "bs_owner_name": null,
            "valid_flg": 1,
            "delete_flg": 0,
            "gid": "1234567",
            "bill_detail": [
                {
                    "demand_number": "97",
                    "demand_code": null,
                    "item_number": null,
                    "item_code": null,
                    "goods_name": "商品名",
                    "pattern_period_format": 0,
                    "demand_start_date": "2017-03-01",
                    "demand_end_date": "2017-03-31",
                    "criterion_date": "2017-03-31",
                    "link_customer_code": "",
                    "goods_code": null,
                    "link_goods_code": null,
                    "unit_price": "5000.0000",
                    "quantity": "1.00",
                    "unit": null,
                    "tax_category": 0,
                    "consumption_tax": 8,
                    "subtotal_amount_billed": 5000,
                    "consumption_tax_amount": 370,
                    "total_amount_billed": 5000,
                    "unclearing_amount": 0,
                    "remark": null,
                    "recorded_date": "2017/03/31",
                    "payment_method": "決済情報名",
                    "payment_status": 1,
                    "valid_flg": 1,
                    "delete_flg": 0
                }
            ]
        }
    ]
}
```

## エラー

| エラーコード | 内容                                     |
| ------------ | ---------------------------------------- |
| 2001         | 請求書番号が不正                         |
| 2002         | 請求先コードが不正                       |
| 2003         | 請求先部署番号が不正                     |
| 2004         | 請求先部署コードが不正                   |
| 2005         | 発行日の検索開始日が不正                 |
| 2006         | 発行日の検索終了日が不正                 |
| 2007         | 更新日の検索開始日が不正                 |
| 2008         | 更新日の検索終了日が不正                 |
| 2009         | 決済期限の検索開始日が不正               |
| 2010         | 決済期限の検索終了日が不正               |
| 2011         | 決済手段が不正                           |
| 2012         | 集計用商品区分コードが不正               |
| 2013         | 請求元担当者コードが不正                 |
| 2014         | 消込ステータスが不正                     |
| 2015         | 請求書取得件数が不正                     |
| 2016         | 請求書取得開始インデックスが不正         |
| 2017         | 請求書参照に失敗                         |
| 2018         | 発行日、更新日、決済期限のいずれか必須。 |