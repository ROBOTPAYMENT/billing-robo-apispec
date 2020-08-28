# 請求先部署参照API

`/api/v1.0/billing_individual/search`

請求先部署情報の参照処理を実行します

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/v1.0/billing_individual/search`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

下記のような項目のオブジェクトを持つリスト

### Parameters

| 名前                                              | 概要                                                                                  | 桁数 | 種別                              | 必須 |
| ------------------------------------------------- | ------------------------------------------------------------------------------------- | ---- | -------------------------------- | ---- |
| user_id                                           | ユーザーID                                                                             | 100  | [メール形式](../../index.md#種別) | 必須 |
| access_key                                        | アクセスキー                                                                           | 100  | [半角英数](../../index.md#種別)   | 必須 |
| limit_count                                       | 請求先部署情報取得件数 <br> ※0〜200の数値を設定する。省略した場合、20が設定される          | 3    | 数値                              |     |
| page_count                                        | 請求先部署情報取得開始インデックス <br> ※0～99の数値を設定する。省略した場合、0が設定される | 2    | 数値                              |     |
| [billing_individual](#billing-individual-request) | 請求先部署情報に属するパラメータ                                                                |      | `object`                         |      |


#### billing-individual (request)

| 名前                | 概要                                                                                                                                                                                                                                                                                                                          | 桁数 | 種別                                    | 必須                                             | 一致 |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---- | -------------------------------------- | ------------------------------------------------ | ---- |
| billing_code        | 請求先コード                                                                                                                                                                                                                                                                                                                  | 20   | 半角英数 + 記号                         | (請求先部署番号指定時) <br> (請求先部署コード指定時) | 完全 |
| billing_name        | 請求先名                                                                                                                                                                                                                                                                                                                      | 100  | 文字列                                 |                                                  | 部分 |
| number              | 請求先部署番号                                                                                                                                                                                                                                                                                                                | 18   | 数値                                   |                                                  |     |
| code                | 請求先部署コード                                                                                                                                                                                                                                                                                                              | 20   | 半角英数 + 記号                         |                                                  | 完全 |
| name                | 請求先部署名                                                                                                                                                                                                                                                                                                                  | 100  | 文字列                                 |                                                  | 部分 |
| email               | メールアドレス                                                                                                                                                                                                                                                                                                                | 100  | メール形式                              |                                                  | 完全 |
| payment_method_code | 決済情報コード                                                                                                                                                                                                                                                                                                                 | 20   | 半角英数 + 記号                         |                                                 | 完全 |
| payment_method      | 決済手段 <br> 0:銀行振込 <br> 1:クレジットカード <br> 2:バンクチェック <br> 3:RP口座振替 <br> 4:RL口座振替 <br> 5:その他口座振替 <br> 6:コンビニ払込票(A4) <br> 7:コンビニ払込票(ハガキ) <br> 8:その他コンビニ払込票 <br> 9:バーチャル口座 <br> 10:その他決済1 <br> 11:その他決済2 <br> 12:その他決済3 <br> 13:その他決済4 <br> 14:その他決済5 | 2     | 数値                                   |                                                 |     |
| register_status     | 決済手段状況 <br> 0:未登録 <br> 1:登録待ち <br> 2:メール送信済み <br> 3:申請中 <br> 4:登録情報送信エラー <br> 5:登録完了 <br>6:登録失敗                                                                                                                                                                                              | 1    | 数値                                   |                                                  |     |
| bs_department_code  | 請求元部署コード                                                                                                                                                                                                                                                                                                               | 40   | 半角英数 + 記号                         |                                                  | 完全 |
| bs_owner_code       | 請求元担当者コード                                                                                                                                                                                                                                                                                                             | 20   | 半角英数 + 記号                         |                                                  | 完全 |
| regist_date_from    | 登録日時の検索開始日                                                                                                                                                                                                                                                                                                            | 19   | 日時                                   |                                                 |     |
| regist_date_to      | 登録日時の検索終了日                                                                                                                                                                                                                                                                                                            | 19   | 日時                                   |                                                 |     |
| update_date_from    | 更新日時の検索開始日                                                                                                                                                                                                                                                                                                            | 19   | 日時                                   |                                                 |     |
| update_date_to      | 更新日時の検索終了日                                                                                                                                                                                                                                                                                                            | 19   | 日時                                   |                                                 |     |
| valid_flg           | 状態 <br> 0: 停止 <br> 1: 有効                                                                                                                                                                                                                                                                                                 | 1    | 数値                                   |                                                 |     |



## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                                              | 概要                                                                                                                                               | 型                                |
| ------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------- |
| user_id                                           | ユーザーID（管理画面へのログインID）                                                                                                                  | [メール形式](../../index.md#種別) |
| access_key                                        | アクセスキー                                                                                                                                        | [半角英数](../../index.md#種別)   |
| limit_count                                       | 請求先部署情報取得件数 <br> ※最大件数は、リクエストで指定された「請求先部署情報取得件数」に依存                                                           | 数値                              |
| page_count                                        | 請求先部署情報取得開始インデックス <br> ※取得した請求先部署情報の開始インデックスを返却する                                                               | 数値                              |
| total_page_count                                  | 請求先部署情報取得開始インデックス合計 <br> ※指定された検索条件によって取得可能な請求先部署情報の全件数／請求先部署情報取得件数によって、算出される値を返却する | 数値                              |
| [billing_individual](#billing-individual-response)| 請求先部署情報に属するパラメータ                                                                                                                      | `object`                         |

#### billing-individual (response)

| 名前                         | 概要                                          | 型        |
| ---------------------------- | -------------------------------------------- | --------- |
| error_code                   | エラーコード <br> ※正常時はnull               | string    |
| error_message                | エラーメッセージ <br> ※正常時はnull            | string    |
| billing_code                 | 請求先コード                                  | string    |
| billing_name                 | 請求先名                                      | string    |
| number                       | 請求先部署番号                                | int       |
| code                         | 請求先部署コード                              | string    |
| name                         | 請求先部署名                                  | string    |
| link_customer_code           | 会計ソフト連携用取引先コード                   | string    |
| address1                     | 宛名1                                        | string    |
| address2                     | 宛名2                                        | string    |
| address3                     | 宛名3                                        | string    |
| zip_code                     | 郵便番号                                     | string    |
| pref                         | 都道府県                                     | string    |
| city_address                 | 市区町村番地                                 | string    |
| building_name                | 建物名                                       | string    |
| set_post_address             | 郵送宛先情報 <br> 0:使用しない <br> 1:使用する | int       |
| post_address1                | 郵送先宛名1                                  | string    |
| post_address2                | 郵送先宛名2                                  | string    |
| post_address3                | 郵送先宛名3                                  | string    |
| post_zip_code                | 郵送先郵便番号                               | string    |
| post_pref                    | 郵送先都道府県                               | string    |
| post_city_address            | 郵送先市区町村番地                           | string    |
| post_building_name           | 郵送先建物名                                | string    |
| tel                          | 電話番号                                    | string    |
| email                        | メールアドレス                               | string    |
| cc_email                     | CC送信先メールアドレス                       | string    |
| memo                         | メモ                                       | string    |
| auto_erase_commission_amount | 手数料自動消込許容金額                       | int       |
| billing_method               | 請求方法                                    | int       |
| issue_month                  | 請求書発行日_月                             | int       |
| issue_day                    | 請求書発行日_日                             | int       |
| sending_month                | 請求書送付日_月                             | int       |
| sending_day                  | 請求書送付日_日                             | int       |
| deadline_month               | 決済期限_月                                 | int       |
| deadline_day                 | 決済期限_日                                 | int       |
| payment_method_number        | 決済情報番号                                | int       |
| payment_method_code          | 決済情報コード                              | string    |
| register_status              | 決済手段状況                                | int       |
| bs_department_code           | 請求元部署コード                            | string    |
| bs_owner_code                | 請求元担当者コード                          | string    |
| ref_billing_code             | 合計請求書用請求先コード                     | string    |
| ref_individual_number        | 合計請求書用請求先部署番号                   | int       |
| ref_individual_code          | 合計請求書用請求先部署コード                 | string    |
| bill_template_code           | 請求書テンプレートコード                     | int       |
| regist_date                  | 登録日時                                   | datetime   |
| update_date                  | 更新日時                                   | datetime   |
| valid_flg                    | 状態                                       | int       |
| sub_account_title            | 請求先部署補助科目に属するパラメータ          | `array`   |

#### sub_account_title

| 名前                    | 概要                         | 型      |
| ----------------------- | --------------------------- | ------- |
| account_receivable_code | 請求先部署売掛金補助科目コード | string  |
| advances_received_code  | 請求先部署前受金補助科目コード | string  |
| suspense_received_code  | 請求先部署仮受金補助科目コード | string  |


## 使用例

### リクエスト例

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "limit_count": 20,
    "page_count": 0,
    "billing_individual":{
        "billing_code": "billing_code",
        "billing_name": "請求先名",
        "number": 2,
        "code": "keiri",
        "name": "経理部",
        "email": "xxx@robotpayment.co.jp",
        "payment_method_code": "payment1",
        "payment_method": 0,
        "register_status": 5,
        "bs_department_code": "0001",
        "bs_owner_code": "bs_owner",
        "regist_date_from": "2020/07/01 10:00:00",
        "regist_date_to": "2020/07/30 20:00:00",
        "update_date_from": "2020/07/01 10:00:00",
        "update_date_to": "2020/07/30 20:00:00",
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
    "billing_individual": [
        {
            "error_code": null,
            "error_message": null,
            "billing_code": "sample",
            "billing_name": "株式会社サンプル",
            "number": 2,
            "code": "individualcode",
            "name": "サンプル部",
            "link_customer_code": "link_customer_code",
            "address1": "宛名１",
            "address2": "",
            "address3": "",
            "zip_code": 1651111,
            "pref": "東京都",
            "city_address": "渋谷区",
            "building_name": "",
            "set_post_address": 0,
            "post_address1": "",
            "post_address2": "",
            "post_address3": "",
            "post_zip_code": "",
            "post_pref": "",
            "post_city_address": "",
            "post_building_name": "",
            "tel": "",
            "email": "email@robotpayment.co.jp",
            "cc_email": "",
            "memo": "",
            "auto_erase_commission_amount": null,
            "billing_method": null,
            "issue_month": null,
            "issue_day": null,
            "sending_month": null,
            "sending_day": null,
            "deadline_month": null,
            "deadline_day": null,
            "payment_method_number": 111,
            "payment_method_code": "",
            "register_status": 5,
            "bs_department_code": "bs_department_code",
            "bs_owner_code": "",
            "ref_billing_code": "",
            "ref_individual_number": null,
            "ref_individual_code": "",
            "bill_template_code": null,
            "regist_date": "2020/08/01 10:00:00",
            "update_date": "2020/08/01 10:00:00",
            "valid_flg": 1,
            "sub_account_title": [
                {
                  "account_receivable_code": "null",
                  "advances_received_code": "null",
                  "suspense_received_code": "null"
                }
            ]
        }
    ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                                   |
| ------------ | ------------------------------------- |
| 4401         | 請求先コードが不正                     |
| 4402         | 請求先名が不正                         |
| 4403         | 請求先部署番号が不正                    |
| 4404         | 請求先部署コードが不正                  |
| 4405         | 請求先部署名が不正                      |
| 4406         | メールアドレスが不正                    |
| 4407         | 決済情報コードが不正                    |
| 4408         | 決済手段が不正                         |
| 4409         | 決済手段状況が不正                     |
| 4410         | 請求元部署コードが不正                 |
| 4411         | 請求元担当者コードが不正               |
| 4412         | 登録日時の検索開始日が不正             |
| 4413         | 登録日時の検索終了日が不正             |
| 4414         | 更新日時の検索開始日が不正             |
| 4415         | 更新日時の検索終了日が不正             |
| 4416         | 状態が不正                            |
| 4417         | 請求先部署情報取得開始インデックスが不正 |
| 4418         | 請求先部署情報取得件数が不正            |
| 4419         | 請求先部署参照に失敗しました            |

----

[TOPへ戻る](../../index.md)