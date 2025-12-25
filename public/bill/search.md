# 請求書参照

`/api/v1.0/bill/search`

請求書の参照処理を実行します。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Path: `/api/v1.0/bill/search`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                  | 概要                                                                                 | 桁数 | 種別                               | 必須 |
| --------------------- | ------------------------------------------------------------------------------------ | ---- | --------------------------------- | ---- |
| user_id               | ユーザーID（管理画面へのログインID）                                                   | 100  | [メール形式](../../index.md#種別)  | 必須 |
| access_key            | アクセスキー                                                                          | 100  | [半角英数](../../index.md#種別)    | 必須 |
| limit_count           | 請求書取得件数 <br> ※0〜200の数値を設定する。省略した場合、20が設定される               | 3    | 数値                              |      |
| page_count            | 請求書取得開始インデックス <br> ※0〜99,999の数値を設定する。省略した場合、0が設定される  | 5    | 数値                              |      |
| [sort](#sort-request) | 検索順序に属するパラメータ <br> ※省略時は請求書ID の昇順で返却する                      |      | `object`                          |      |
| [bill](#bill-request) | 請求書に属するパラメータ                                                               |      | `object`                          |      |

#### sort (request)

下記のような項目のオブジェクト

| 名前  | 概要                                                                                                                                                                                                                                         | 桁数 | 種別   | 必須 | 一致 |
| ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---- | ------ | ---- | ---- |
| key   | キー名 <br> ※以下の項目からのみ有効、複数選択可(カンマ区切り) <br> ※左の項目から優先的にソート順序が指定される <br> bill_id: 請求書 ID, <br>  regist_date: 登録日時, <br> update_date: 更新日時 | 31   | 文字列 |      |      |
| order | ソート順 <br> 0: 昇順 <br> 1: 降順                                                                                                                                                                                                            | 1    | 数値   |      |      |


#### bill (request)

下記のような項目のオブジェクト

| 名前                          | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;概要&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | 桁数  | 種別                               | 必須                                              | 一致 |
| ----------------------------- |----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----| --------------------------------- | ------------------------------------------------ |----------|
| number                        | 請求書番号                                                                                                                                                                                                                                                                                                                                                                      | 100 | [半角英数 + 記号](../../index.md#種別) |                                                   | 完全一致 |
| billing_code                  | 請求先コード                                                                                                                                                                                                                                                                                                                                                                     | 20  | [半角英数 + 記号](../../index.md#種別) | (請求先部署番号指定時) <br> (請求先部署コード指定時) | 完全一致  |
| billing_individual_number     | 請求先部署番号                                                                                                                                                                                                                                                                                                                                                                    | 18  | 数値                              |                                                   | 完全一致 |
| billing_individual_code       | 請求先部署コード                                                                                                                                                                                                                                                                                                                                                                   | 20  | [半角英数 + 記号](../../index.md#種別) |                                                   | 完全一致 |
| issue_date_from               | 発行日の検索開始日                                                                                                                                                                                                                                                                                                                                                                  | 10  | 日付                              |                                                   |          |
| issue_date_to                 | 発行日の検索終了日                                                                                                                                                                                                                                                                                                                                                                  | 10  | 日付                              |                                                   |          |
| deadline_date_from            | 決済期限の検索開始日                                                                                                                                                                                                                                                                                                                                                                 | 10  | 日付                              |                                                   |          |
| deadline_date_to              | 決済期限の検索終了日                                                                                                                                                                                                                                                                                                                                                                 | 10  | 日付                              |                                                   |          |
| payment_method                | [決済手段](../../index.md#決済手段)                                                                                                                                                                                                                                                                                                                                                | 2   | 数値                              |                                                   |          |
| clearing_status               | 消込ステータス <br> 0:未処理 <br> 1:完了 <br> 2:確認済み <br> 3:未収 <br> 4:貸倒 <br> 5:手数料 <br> 6:現金 <br> 7:長期滞留債権 <br> 8:破産更生債権 <br> 9:売上取消 <br> 10:繰越 <br> 11:まるなげ<br> 12:ファクタリング                                                                                                                                                                                                           | 2   | 数値                              |                                                   |          |
| marunage_clearing_status      | 債権譲渡消込ステータス <br> 0:未処理 <br> 1:完了 <br> 2:確認済み <br> 3:未収 <br> 4:貸倒 <br> 5:手数料 <br> 6:現金 <br> 7:長期滞留債権 <br> 8:破産更生債権 <br> 9:売上取消 <br> 10:繰越                                                                                                                                                                                                                                   | 2   | 数値                              |                                                   |          |
| bs_residence_code             | 請求元差出人コード <br>                                                                                                                                                                                                                                                                                                                                                             | 20  | [半角英数 + 記号](../../index.md#種別) |                                                   | 完全一致 |
| bs_department_code            | 請求元部署コード <br>                                                                                                                                                                                                                                                                                                                                                              | 40  | [半角英数 + 記号](../../index.md#種別) |                                                   | 完全一致 |
| bs_owner_code                 | 請求元担当者コード <br>                                                                                                                                                                                                                                                                                                                                                             | 20  | [半角英数 + 記号](../../index.md#種別) |                                                   | 完全一致 |
| attachment_planned_flg        | ファイル添付予定フラグ <br> 0:ファイル添付予定なし <br> 1:ファイル添付予定あり <br> ※ファイル添付機能利用中のみ利用可能                                                                                                                                                                                                                                                                                                    | 1   | 数値 |                                                   |  |
| attachment_flg                | 添付ファイルフラグ <br> 0:添付ファイルなし <br> 1:添付ファイルあり <br> ※ファイル添付機能利用中のみ利用可能                                                                                                                                                                                                                                                                                                          | 1   | 数値 |                                                   |  |
| carryover_status              | 繰越ステータス <br> 0:対象外 <br> 1:繰越待ち <br> 2:繰越完了                                                                                                                                                                                                                                                                                                                                 | 1   | 数値                              |                                                   |         |
| carryover_transit_bill_number | 繰越先請求書番号                                                                                                                                                                                                                                                                                                                                                                   | 100 | [半角英数 + 記号](../../index.md#種別) |                                                   | 完全一致 |
| marunage_bill_credit_status           | 債権譲渡申請ステータス                                                                                                                                                                                                                                                                                                                                                                | 1   | 数値                              |    |
| marunage_bill_credit_apply_date_from  | 債権譲渡申請日（開始日）                                                                                                                                                                                                                                                                                                                                                               | 10  | 日付                              |    |
| marunage_bill_credit_apply_date_to    | 債権譲渡申請日（終了日）                                                                                                                                                                                                                                                                                                                                                               | 10  | 日付                              |    |
| marunage_bill_credit_answer_date_from | 債権譲渡申請日（開始日）                                                                                                                                                                                                                                                                                                                                                               | 10  | 日付                              |    |
| marunage_bill_credit_answer_date_to   | 債権譲渡申請日（終了日）                                                                                                                                                                                                                                                                                                                                                               | 10  | 日付                              |    |
| regist_date_from              | 登録日時(開始日) <br> ※時刻省略時 00:00:00 で検索します                                                                                                                                                                                                                                                                                                                                      | 19  | 日時                               |                                                   |          |
| regist_date_to                | 登録日時(終了日) <br> ※時刻省略時 23:59:59 で検索します                                                                                                                                                                                                                                                                                                                                      | 19  | 日時                               |                                                   |          |
| update_date_from              | 更新日時(開始日) <br> ※時刻省略時 00:00:00 で検索します                                                                                                                                                                                                                                                                                                                                      | 19  | 日時                               |                                                   |          |
| update_date_to                | 更新日時(終了日) <br> ※時刻省略時 23:59:59 で検索します                                                                                                                                                                                                                                                                                                                                      | 19  | 日時                               |                                                   |          |
| valid_flg                     | 状態 <br> 0:無効 <br> 1:有効                                                                                                                                                                                                                                                                                                                                                     | 1   | 数値                               |                                                   |          |

## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                                             | 概要                                                                                                                                          | 型            |
| ------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| user_id                                          | ユーザーID                                                                                                                                    | string        |
| limit_count                                      | 請求書取得件数 <br> ※最大件数は、リクエストで指定された「請求書取得件数」に依存                                                                  | int           |
| page_count                                       | 請求書取得開始インデックス <br> ※取得した請求書の開始インデックスを返却する                                                                      | int           |
| total_page_count                                 | 請求書取得開始インデックス合計 <br> ※指定された検索条件によって取得可能な請求書の全件数／請求書取得件数によって、算出される値を返却する              | int           |
| [sort](#sort-response)                           | 検索順序に属するパラメータ                                                                                                                      | `object`     |
| [bill](#bill-response)                           | 請求書に属するパラメータ                                                                                                                        | `array`      |
| [count_update_date](#count_update_date-response) | 更新日時に属するパラメータ                                                                                                                      | `array`      |

#### sort (response)

下記のような項目のオブジェクト

| 名前  | 概要                               | 型     |
| ----- | ---------------------------------- | ------ |
| key   | キー名                             | string |
| order | ソート順 <br> 0: 昇順 <br> 1: 降順 | int    |

#### bill (response)

下記のような項目のオブジェクトを持つリスト

| 名前                                           | 概要                                                                                                                                                                   | 型        | 備考                      |
|----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------| ------------------------ |
| error_code                                   | エラーコード <br> ※正常時はnull                                                                                                                                                | int      |                          |
| error_message                                | エラーメッセージ <br> ※正常時はnull                                                                                                                                              | string   |                          |
| number                                       | 請求書番号                                                                                                                                                                | string   |                          |
| billing_code                                 | 請求先コード                                                                                                                                                               | string   |                          |
| billing_name                                 | 請求先名                                                                                                                                                                 | string   | 発行時の情報を返却します  |
| billing_individual_number                    | 請求先部署番号                                                                                                                                                              | int      |                         |
| billing_individual_code                      | 請求先部署コード                                                                                                                                                             | string   |                         |
| billing_individual_name                      | 請求先部署名                                                                                                                                                               | string   | 発行時の情報を返却します  |
| billing_individual_address1                  | 請求先部署宛名1                                                                                                                                                             | string   | 発行時の情報を返却します  |
| billing_individual_address2                  | 請求先部署宛名2                                                                                                                                                             | string   | 発行時の情報を返却します  |
| billing_individual_address3                  | 請求先部署宛名3                                                                                                                                                             | string   | 発行時の情報を返却します  |
| billing_individual_zip_code                  | 請求先部署郵便番号                                                                                                                                                            | string   | 発行時の情報を返却します  |
| billing_individual_pref                      | 請求先部署都道府県名                                                                                                                                                           | string   | 発行時の情報を返却します  |
| billing_individual_city_address              | 請求先部署市区町村番地                                                                                                                                                          | string   | 発行時の情報を返却します  |
| billing_individual_building_name             | 請求先部署建物名                                                                                                                                                             | string   | 発行時の情報を返却します  |
| billing_individual_tel                       | 請求先部署電話番号                                                                                                                                                            | string   | 発行時の情報を返却します  |
| bill_detail_count                            | 請求書明細件数                                                                                                                                                              | int      |                         |
| subtotal_amount_billed                       | 請求金額小計                                                                                                                                                               | int      |                         |
| consumption_tax_amount                       | 消費税額                                                                                                                                                                 | int      |                         |
| bill_carryover_suspense_total_amount_billed  | 入金繰越額                                                                                                                                                                | int      |                         |
| total_amount_billed                          | 請求金額合計                                                                                                                                                               | int      |                         |
| bill_total_amount_billed_before_suspense_carryover | 合計（入金繰越前）                                                                                                                                                            | int      |                         |
| [consumption_tax_items](#consumption_tax_items-response) | 消費税内訳                                                                                                                                                                | array    | 税率がない（対象外、非課税）場合はnullを返却 |
| unclearing_amount                            | 未消込金額                                                                                                                                                                | int      |                         |
| message_column                               | 通信欄                                                                                                                                                                  | string   |                          |
| billing_method                               | 請求方法 <br> 0:請求無し <br> 1:自動メール <br> 2:手動メール <br> 3:自動郵送 <br> 4:手動郵送 <br> 5:自動メール＋自動郵送 <br> 6:手動メール＋手動郵送 <br> 7:自動マイページ配信 <br> 8:手動マイページ配信配信                           | int      |                         |
| issue_date                                   | 請求書発行日                                                                                                                                                               | date     |                         |
| make_date                                    | 請求書作成日                                                                                                                                                               | date     |                         |
| sending_scheduled_date                       | 請求書送付予定日                                                                                                                                                             | date     |                         |
| sending_date                                 | 請求書送付日                                                                                                                                                               | date     |                         |
| confirm_date                                 | 請求書確認日                                                                                                                                                               | date     |                         |
| mail_send_flg                                | メールステータス <br> 0:未送信  <br> 1:送信済み                                                                                                                                     | int      |                         |
| post_send_flg                                | 郵送ステータス   <br> 0:未送信  <br> 1:送信済み                                                                                                                                    | int      |                         |
| marunage_mail_status                         | 債権譲渡メールステータス <br> 0:未送信  <br> 1:送信済み                                                                                                                                 | int      |                        |
| marunage_post_status                         | 債権譲渡郵送ステータス <br> 0:注文済 <br> 1:発送済 <br> 2:完了 <br> 3:注文失敗 <br> 4:注文取り消し <br> 5:注文差止 <br> 6:未送信                                                                         | int      |                        |
| payment_method                               | [決済手段](../../index.md#決済手段)                                                                                                                                          | int      |                         |
| payment_method_number                        | 決済情報番号                                                                                                                                                               | int      |                         |
| payment_method_code                          | 決済情報コード                                                                                                                                                              | string   |                         |
| settlement_result                            | 決済連携ステータス  <br> 0:申請中  <br> 1:送信失敗   <br> 2:決済成功   <br> 3:決済失敗                                                                                                       | int      |                         |
| settlement_linkage_status                    | 決済データ連携ステータス  <br> 0:未連携   <br> 1:連携失敗   <br> 2:決済成功   <br> 3:決済失敗   <br> 4:連携なし   <br> 5:連携済み   <br> 6:再連携待ち                                                        | int      |                         |
| transfer_deadline                            | 決済期限                                                                                                                                                                 | date     |                          |
| slip_deadline                                | 払込票有効期限                                                                                                                                                              | date     |                         |
| transfer_date                                | 決済日                                                                                                                                                                  | date     |                         |
| gid                                          | 決済ID                                                                                                                                                                 | string   |                         |
| ec                                           | 決済エラーコード                                                                                                                                                             | string   |                         |
| clearing_status                              | 消込ステータス <br> 0:未処理 <br> 1:完了 <br> 2:確認済み <br> 3:未収 <br> 4:貸倒 <br> 5:手数料 <br> 6:現金 <br> 7:長期滞留債権 <br> 8:破産更生債権 <br> 9:売上取消 <br> 10:繰越 <br> 11:まるなげ<br> 12:ファクタリング     | int      |                         |
| marunage_clearing_status                     | 債権譲渡消込ステータス <br> 0:未処理 <br> 1:完了 <br> 2:確認済み <br> 3:未収 <br> 4:貸倒 <br> 5:手数料 <br> 6:現金 <br> 7:長期滞留債権 <br> 8:破産更生債権 <br> 9:売上取消 <br> 10:繰越                             | int      |                         |
| memo                                         | メモ                                                                                                                                                                   | string   |                         |
| template_code                                | 請求書テンプレート                                                                                                                                                            | string   |                        |
| bs_residence_code                            | 請求元差出人コード                                                                                                                                                            | string   |                        |
| bs_residence_name                            | 請求元差出人名                                                                                                                                                              | string   |                        |
| bs_invoice_supplier_code                     | 適格請求書発行事業者 登録番号                                                                                                                                                      | string   | 発行時の情報を返却します  |
| bs_residence_zip_code                        | 請求元差出人郵便番号                                                                                                                                                           | string   | 発行時の情報を返却します                       |
| bs_residence_pref                            | 請求元差出人都道府県                                                                                                                                                           | string   | 発行時の情報を返却します                       |
| bs_residence_address                         | 請求元差出人市区町村番地                                                                                                                                                         | string   | 発行時の情報を返却します                       |
| bs_residence_building_name                   | 請求元差出人建物名                                                                                                                                                            | string   | 発行時の情報を返却します                       |
| bs_residence_phone_number                    | 請求元差出人電話番号                                                                                                                                                           | string   | 発行時の情報を返却します                       |
| bs_department_code                           | 請求元部署コード                                                                                                                                                             | string   |                        |
| bs_department_name                           | 請求元部署名                                                                                                                                                               | string   |                        |
| bs_owner_code                                | 請求元担当者コード                                                                                                                                                            | string   |                        |
| bs_owner_name                                | 請求元担当者名                                                                                                                                                              | string   |                        |
| bs_owner_post_name                           | 請求元担当者役職名                                                                                                                                                            | string   |                         |
| carryover_status                             | 繰越ステータス  <br> 0:対象外  <br> 1:繰越待ち   <br> 2:繰越完了                                                                                                                       | int      |                        |
| carryover_transit_bill_number                | 繰越先請求書番号                                                                                                                                                             | string   |                        |
| carryover_total_amount                       | 繰越金額                                                                                                                                                                 | int      |                       |
| attachment_planned_flg                       | ファイル添付予定フラグ <br> 0:ファイル添付予定なし <br> 1:ファイル添付予定あり                                                                                                                      | int      |                       |
| attachment_flg                               | 添付ファイルフラグ <br> 0:添付ファイルなし <br> 1:添付ファイルあり                                                                                                                            | int      |                       |
| marunage_bill_credit_status     | 債権譲渡申請ステータス<br> 0:未申請 <br> 1:申請中 <br> 2:承認 <br> 3:却下 <br> 4:回答中 <br> 5:債権譲渡解除                                                                                        | int      |                               |
| marunage_bill_credit_apply_date | 債権譲渡申請日                                                                                                                                                              | date     |                                   |
| marunage_bill_credit_answer_date| 債権譲渡回答日                                                                                                                                                              | date       |                                   |
| download_url                                 | 請求書ダウンロードURL                                                                                                                                                         | string   |                        |
| bill_group_key                               | 請求書合算キー                                                                                                                                                              | string   |                        |
| regist_date                                  | 登録日時                                                                                                                                                                 | datetime |                      |
| update_date                                  | 更新日時                                                                                                                                                                 | datetime |                      |
| valid_flg                                    | 有効フラグ  <br> 0:無効  <br> 1:有効                                                                                                                                          | int      |                       |

#### consumption_tax_items (response)

下記のような項目のオブジェクトを持つリスト

| 名前             | 概要       | 型     | 備考   |
| --------------- | --------- | ------ | ----- |
| taxable_amount  | 課税基準額   | int    | 税抜額 |
| tax_amount      | 消費税額    | int    | |
| tax_rate        | 税率       | int    | |

#### count_update_date (response)

下記のような項目のオブジェクトを持つリスト

| 名前         | 概要              | 型       |
| -----------  | ---------------- | -------- |
| update_date  | 更新日時          | datetime |
| update_count | 更新日時毎の件数   | int      |

## 使用例

### リクエスト例

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "limit_count": 20,
    "page_count": 1,
    "sort": {
        "key": "update_date,bill_id",
        "order": 0
    },
    "bill": {
        "number": "202001-sample-1",
        "billing_code": "sample",
        "billing_individual_number": 1,
        "billing_individual_code": "sample",
        "issue_date_from": "2020/06/01",
        "issue_date_to": "2020/07/30",
        "deadline_date_from": "2020/06/01",
        "deadline_date_to": "2020/07/30",
        "payment_method": 0,
        "clearing_status": 1,
        "marunage_clearing_status": "",
        "bs_residence_code": "bs_residence001",
        "bs_department_code": "bs_department001",
        "bs_owner_code": "bs_owner001",
        "attachment_planned_flg": 0,
        "attachment_flg": 0,
        "carryover_status": 0,
        "marunage_bill_credit_status": 1,
        "marunage_bill_credit_apply_date_from": "2020/07/01",
        "marunage_bill_credit_apply_date_to": "2020/07/31",
        "marunage_bill_credit_answer_date_from": "2020/07/01",
        "marunage_bill_credit_answer_date_to": "2020/07/31",
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
    "limit_count": 20,
    "page_count": 0,
    "total_page_count": 1,
    "sort": {
        "key": "update_date,bill_id",
        "order": 0
    },
    "bill": [
        {
            "error_code": null,
            "error_message": null,
            "number": "202001-sample-1",
            "billing_code": "sample",
            "billing_name": "株式会社sample",
            "billing_individual_number": 1,
            "billing_individual_code": "sample",
            "billing_individual_name": "株式会社sample経理部",
            "billing_individual_address1": "株式会社sample",
            "billing_individual_address2": "経理部",
            "billing_individual_address3": "代表取締役　サンプル　様",
            "billing_individual_zip_code": "0000000",
            "billing_individual_pref": "東京都",
            "billing_individual_city_address": "渋谷区1-1-1",
            "billing_individual_building_name": "渋谷ビル3階",
            "billing_individual_tel": "00000000000",
            "bill_detail_count": 1,
            "subtotal_amount_billed": 10000,
            "consumption_tax_amount": 800,
            "bill_carryover_suspense_total_amount_billed": 0,
            "total_amount_billed": 10800,
            "bill_total_amount_billed_before_suspense_carryover": 0,
            "consumption_tax_items":[
              {
                "taxable_amount": 10000,
                "tax_amount": 800,
                "tax_rate": 8
              }
            ],
            "unclearing_amount": 0,
            "message_column": "message_column",
            "billing_method": 6,
            "issue_date": "2020/06/01",
            "make_date": "2020/06/01",
            "sending_scheduled_date": "2020/06/01",
            "sending_date": "2020/06/01",
            "confirm_date": "2020/06/01",
            "mail_send_flg": 1,
            "post_send_flg": 1,
            "marunage_mail_status": null,
            "marunage_post_status": null,
            "payment_method": 0,
            "payment_method_number": 1,
            "payment_method_code": "bank001",
            "settlement_result": 0,
            "settlement_linkage_status": 0,
            "transfer_deadline": "2020/07/30",
            "slip_deadline": null,
            "transfer_date": "2020/07/15",
            "gid": null,
            "ec": null,
            "clearing_status": 1,
            "marunage_clearing_status": null,
            "memo": "メモ",
            "template_code": "10010",
            "bs_residence_code": "bs_residence001",
            "bs_residence_name": "差出人1",
            "bs_invoice_supplier_code": "T1234567890123",
            "bs_residence_zip_code": "1234567",
            "bs_residence_pref": "北海道",
            "bs_residence_address": "札幌市1-1",
            "bs_residence_building_name": "札幌ビル1F",
            "bs_residence_phone_number": "08012345678",
            "bs_department_code": "bs_department001",
            "bs_department_name": "請求元財務部",
            "bs_owner_code": "bs_owner001",
            "bs_owner_name": "山田太郎",
            "bs_owner_post_name": "課長",
            "carryover_status": 0,
            "carryover_transit_bill_number": null,
            "carryover_total_amount": 0,
            "attachment_planned_flg": 0,
            "attachment_flg": 0,
            "marunage_bill_credit_status": 1,
            "marunage_bill_credit_apply_date": "2020/07/01",
            "marunage_bill_credit_answer_date": "2020/07/01",
            "download_url": "http:://",
            "bill_group_key": null,
            "regist_date": "2020/07/01 10:11:12",
            "update_date": "2020/07/01 16:17:18",
            "valid_flg": 1
        }
    ],
    "count_update_date": [
        {
            "update_date": "2020/07/01 16:17:18",
            "update_count": 1
        }
    ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                    |
|--------|-----------------------|
| 4601   | 請求書番号が不正              |
| 4602   | 請求先コードが不正             |
| 4603   | 請求先部署番号が不正            |
| 4604   | 請求先部署コードが不正           |
| 4605   | 発行日の検索開始日が不正          |
| 4606   | 発行日の検索終了日が不正          |
| 4607   | 決済期限の検索開始日が不正         |
| 4608   | 決済期限の検索終了日が不正         |
| 4609   | 決済手段が不正               |
| 4610   | 消込ステータスが不正            |
| 4611   | 請求元部署コードが不正           |
| 4612   | 請求元担当者コードが不正          |
| 4613   | 繰越ステータスが不正            |
| 4614   | 繰越先請求書番号が不正           |
| 4615   | 登録日時の検索開始日が不正         |
| 4616   | 登録日時の検索終了日が不正         |
| 4617   | 更新日時の検索開始日が不正         |
| 4618   | 更新日時の検索終了日が不正         |
| 4619   | 状態が不正                 |
| 4620   | 請求書取得件数が不正            |
| 4621   | 請求書取得開始インデックスが不正      |
| 4622   | キー名が不正                |
| 4623   | ソート順が不正               |
| 4624   | 請求書件数が100,000件に到達しました |
| 4625   | 請求書参照に失敗しました          |
| 4626   | 債権譲渡消込ステータスが不正        |
| 4627   | 請求元差出人コードが不正          |
| 4628   | ファイル添付予定フラグが不正        |
| 4629   | ファイル添付予定フラグ利用不可       |
| 4630   | 添付ファイルフラグが不正          |
| 4631   | 添付ファイルフラグ利用不可         |
| 4632   | 債権譲渡申請ステータスが不正          |
| 4633   | 債権譲渡申請日の検索開始日が不正        |
| 4634   | 債権譲渡申請日の検索終了日が不正       |
| 4635   | 債権譲渡回答日の検索開始日が不正          |
| 4636   | 債権譲渡回答日の検索終了日が不正         |

----

[TOPへ戻る](../../index.md)