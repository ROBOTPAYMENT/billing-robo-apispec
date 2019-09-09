# 請求書参照

`/api/bill/list`

請求書、請求書明細の一覧を取得します。
レスポンスは請求書登録日時の降順でソートし返却されます。

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/bill/list`
- Preferred HTTP method: `POST`
- Accepted content types: `application/x-www-form-urulencoded`
- Encode: `UTF-8`

### Parameters

| 名前                                  | 概要                                                                                                                                                            | 桁数 | 種別                              | 必須 |
| ------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---- | --------------------------------- | ---- |
| user_id                               | ユーザーID（管理画面へのログインID）                                                                                                                            | 100  | [半角英数*1](/README.md#種別注釈) | 必須 |
| access_key                            | アクセスキー                                                                                                                                                    | 100  | [半角英数*3](/README.md#種別注釈) | 必須 |
| demand_code                           | 請求情報番号                                                                                                                                                    | 20   | 数値                              |      |
| billing_code                          | 請求先コード                                                                                                                                                    | 20   | [半角英数*4](/README.md#種別注釈) |      |
| billing_individual_number             | 請求先部署番号 <br> ※指定する場合billing_codeが必要                                                                                                             | 20   | 数値                              |      |
| billing_individual_code               | 請求先部署コード <br> ※指定する場合billing_codeが必要                                                                                                           | 20   | [半角英数*4](/README.md#種別注釈) |      |
| issue_start_date                      | 発行日の検索開始日 yyyy/mm/dd形式                                                                                                                               | 10   | 日付                              |      |
| issue_stop_date                       | 発行日の検索終了日yyyy/mm/dd形式                                                                                                                                | 10   | 日付                              |      |
| deadline_start_date                   | 決済期限の検索開始日yyyy/mm/dd形式                                                                                                                              | 10   | 日付                              |      |
| deadline_stop_date                    | 決済期限の検索終了日yyyy/mm/dd形式                                                                                                                              | 10   | 日付                              |      |
| payment_method                        | 決済手段 <br> 0:銀行振込 1:クレジットカード 2:バンクチェック <br> 3:RP口座振替 4:RL口座振替 5:その他口座振替 <br> 6:コンビニ払込票(A4) 7:コンビニ払込票(ハガキ) | 1    | 数値                              |      |
| goods_code                            | 商品コード                                                                                                                                                      | 33   | 文字列                            |      |
| carryover_payment_status              | 末端の消込ステータス <br> 0:未処理 1:完了 2:確認済み 3:未収 4:貸倒 5:手数料 6:現金 7:長期滞留債権 8:破産更生債権 9:売上取消                                     | 1    | 数値                              |      |
| bs_owner_code                         | 請求元担当者コード <br> ※両端のスペース除去                                                                                                                     | 20   | [半角英数*4](/README.md#種別注釈) |      |
| carryover_payment_complete_start_date | 末端の消込ステータス完了日時の検索開始日 <br> yyyy/mm/dd hh:ii:ss 形式                                                                                          | 19   | 日時                              |      |
| carryover_payment_complete_stop_date  | 末端の消込ステータス完了日時の検索終了日 <br> yyyy/mm/dd hh:ii:ss 形式                                                                                          | 19   | 日時                              |      |
| transfer_start_date                   | 決済日の検索開始日yyyy/mm/dd 形式                                                                                                                               | 10   | 日付                              |      |
| transfer_stop_date                    | 決済日の検索開始日yyyy/mm/dd 形式                                                                                                                               | 10   | 日付                              |      |
| update_start_date                     | データ更新日時の検索開始日yyyy/mm/dd hh:ii;ss形式                                                                                                               | 19   | 日時                              |      |
| update_stop_date                      | データ更新日時の検索開始日yyyy/mm/dd hh:ii:ss 形式                                                                                                              | 19   | 日時                              |      |


## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                                     | 概要                                                                                                                                                                           | 型     |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------ |
| bill                                     | 請求書に属するパラメータ                                                                                                                                                       |        |
| number                                   | 請求書番号                                                                                                                                                                     | string |
| billing_code                             | 請求先コード                                                                                                                                                                   | string |
| billing_name                             | 請求先名                                                                                                                                                                       | string |
| billing_individual_number                | 請求先部署番号                                                                                                                                                                 | string |
| billing_individual_code                  | 請求先部署コード                                                                                                                                                               | string |
| billing_individual_name                  | 請求先部署名                                                                                                                                                                   | string |
| issue_date                               | 請求書発行日                                                                                                                                                                   | string |
| sending_date                             | 請求書送付日                                                                                                                                                                   | string |
| payment_status                           | 消込ステータス <br> 0:未処理 1:完了 2:確認済み 3:未収 4:貸倒 5:手数料 6:現金 7:長期滞留債権 8:破産更生債権 9:売上取消 10:繰越                                                  | int    |
| bill_carryover_payment_status            | 末端の消込ステータス <br> 0:未処理 1:完了 2:確認済み 3:未収 4:貸倒 5:手数料 6:現金 7:長期滞留債権 8:破産更生債権 9:売上取消 10:繰越 <br> ※2016年2月6日よりpayment_statusと同義 | int    |
| deadline_date                            | 決済期限                                                                                                                                                                       | string |
| payment_method                           | 決済手段 <br> 0:銀行振込 1:クレジットカード 2:バンクチェック <br> 3:RP口座振替 4:RL口座振替 5:その他口座振替                                                                   | int    |
| demand_number                            | 請求件数 <br> ※請求書明細の件数                                                                                                                                                | int    |
| subtotal_amount_billed                   | 小計                                                                                                                                                                           | int    |
| consumption_tax_amount                   | 消費税額                                                                                                                                                                       | int    |
| total_bill_detail_consumption_tax_amount | 明細毎消費税額合計                                                                                                                                                             | int    |
| withholding_tax_amount                   | 源泉所得税額                                                                                                                                                                   | int    |
| total_amount_billed                      | 合計                                                                                                                                                                           | int    |
| billing_method                           | 請求方法 <br> 0:送付なし 1:自動メール 2:手動メール 3:自動郵送 <br> 4:手動郵送 5:自動メール+自動郵送 6:手動メール+手動郵送                                                      | int    |
| carryover_total_amount_billed            | 未消込金額                                                                                                                                                                     | int    |
| ec                                       | 決済結果エラーコード <br> ※決済システム側でのエラーコードが入ります。 エラーコードの詳細につきましては下記リンクよりご確認ください。 <br> https://jpayment.zendesk.com/hc/ja   | string |
| bs_owner_code                            | 請求元担当者コード                                                                                                                                                             | string |
| carryover_payment_complete_date          | 消込ステータス完了日時                                                                                                                                                         | string |
| transfer_date                            | 決済日                                                                                                                                                                         | string |
| update_date                              | データ更新日時                                                                                                                                                                 | string |
| bill.bill_detail                         | 請求書明細に属するパラメータ                                                                                                                                                   |        |
| goods_code                               | 商品コード                                                                                                                                                                     | string |
| goods_name                               | 商品名                                                                                                                                                                         | string |
| unit_price                               | 単価                                                                                                                                                                           | string |
| quantity                                 | 数量                                                                                                                                                                           | string |
| unit                                     | 単位                                                                                                                                                                           | string |
| subtotal_amount_billed                   | 合計金額                                                                                                                                                                       | int    |
| consumption_tax_amount                   | 消費税金額                                                                                                                                                                     | int    |
| total_amount_billed                      | 請求金額                                                                                                                                                                       | int    |


## 使用例

### リクエスト

```
user_id: "sample@robotpayment.co.jp"
access_key: "xxxxxxxxxxxxxxxx"
demand_code: 1
billing_code:billing_code
billing_individual_number: 1
billing_individual_code: "bicd0001"
issue_start_date: "2015/08/01"
issue_stop_date:"2015/08/31"
deadline_start_date: "2015/09/01"
deadline_stop_date:"2015/09/30"
payment_method: 0
goods_code:"goods_code"
carryover_payment_status:0
bs_owner_code:"0001"
carryover_payment_complete_start_date:"2015/09/01 00:00:00"
carryover_payment_complete_start_date:"2015/09/30 00:00:00"
transfer_start_date:"2015/09/01"
transfer_stop_date: "2015/09/30"
update_start_date: "2015/09/01 00:00:00"
update_stop_date: "2015/09/30 00:00:00"
```

### レスポンス

Status: 200 OK

```
{
    "bill": [
        {
            "number": "201508-billing_code-1",
            "billing_code": "billing_code",
            "billing_name": "請求先名",
            "billing_individual_number": "1",
            "billing_individual_code": "bicd0001",
            "billing_individual_name": "請求先部署名",
            "issue_date": "2015/08/01",
            "sending_date": "2015/08/10",
            "payment_status": 0,
            "bill_carryover_payment_status": 0,
            "deadline_date": "2015/09/20",
            "payment_method": 0,
            "demand_number": 1,
            "subtotal_amount_billed": 1000,
            "consumption_tax_amount": 80,
            "total_bill_detail_consumption_tax_amount": 80,
            "withholding_tax_amount": 102,
            "total_amount_billed": 978,
            "billing_method": 0,
            "carryover_total_amount_billed": 978,
            "ec": null,
            "bs_owner_code": "0001",
            "carryover_payment_complete_date": "2015/09/20 10:00:00",
            "transfer_date": "2015/09/20",
            "update_date": "2015/09/20 10:00:00",
            "bill_detail": [
                {
                    "goods_code": "goods_code",
                    "goods_name": "商品名",
                    "unit_price": "1000",
                    "quantity": "1",
                    "unit": null,
                    "subtotal_amount_billed": 1000,
                    "consumption_tax_amount": 80,
                    "total_amount_billed": 1080
                }
            ]
        }
    ]
}
```

## エラー

| エラーコード | 内容                                                     |
| ------------ | -------------------------------------------------------- |
| 701          | 請求情報コードが不正                                     |
| 702          | 請求先コードが不正                                       |
| 703          | 請求先部署番号が不正                                     |
| 704          | 請求先コードか請求先部署番号が不正                       |
| 705          | 発行日の検索開始日が不正                                 |
| 706          | 発行日の検索終了日が不正                                 |
| 707          | 決済期限の検索開始日                                     |
| 708          | 決済期限の検索終了日                                     |
| 709          | 決済手段が不正                                           |
| 710          | 商品コードが不正                                         |
| 711          | 末端の繰越請求のステータスが不正                         |
| 712          | 請求元担当者コードが不正                                 |
| 713          | 末端の繰越請求の消込ステータス完了日時の検索開始日が不正 |
| 714          | 末端の繰越請求の消込ステータス完了日時の検索終了日が不正 |
| 715          | 決済日の検索開始日が不正                                 |
| 716          | 決済日の検索終了日が不正                                 |
| 717          | データ更新日時の検索開始日が不正                         |
| 718          | データ更新日時の検索終了日が不正                         |
| 719          | 請求先部署コードが不正                                   |