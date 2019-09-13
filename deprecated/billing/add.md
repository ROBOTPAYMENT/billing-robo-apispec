# 請求先登録

`/api/billing/add`

請求先情報処理を実行します。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/billing/add`
- Preferred HTTP method: `POST`
- Accepted content types: `application/x-www-form-urulencoded`
- Encode: `UTF-8`

### Parameters

| 名前                     | 概要                                                                                                                                                            | 桁数 | 種別                                   | 必須                                                 |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---- | -------------------------------------- | ---------------------------------------------------- |
| user_id                  | ユーザーID（管理画面へのログインID）                                                                                                                            | 100  | [メール形式](../../index.md#種別)      | 必須                                                 |
| access_key               | アクセスキー                                                                                                                                                    | 100  | [半角英数 + 記号](../../index.md#種別) | 必須                                                 |
| billing_code             | 請求先コード  <br> 両端のスペース除去                                                                                                                           | 20   | [半角英数 + 記号](../../index.md#種別) | 必須                                                 |
| billing_name             | 請求先名  <br> 両端のスペース除去                                                                                                                               | 100  | 文字列                                 | 必須                                                 |
| billing_individual_code  | 請求先部署コード <br> 両端のスペース除去                                                                                                                        | 20   | [半角英数 + 記号](../../index.md#種別) |                                                      |
| billing_individual_name  | 請求先部署名 <br> 両端のスペース除去                                                                                                                            | 100  | 文字列                                 | 必須                                                 |
| link_customer_code       | 会計ソフト連携用取引先コード                                                                                                                                    | 20   | [半角英数](../../index.md#種別)        | 必須                                                 |
| address1                 | 宛名1 <br> 両端のスペース除去                                                                                                                                   | 60   | 文字列                                 |                                                      |
| address2                 | 宛名2 <br> ※両端のスペース除去                                                                                                                                  | 60   | 文字列                                 |                                                      |
| address3                 | 宛名3 <br> ※両端のスペース除去                                                                                                                                  | 60   | 文字列                                 |                                                      |
| zip_code                 | 郵便番号 <br>  両端のスペース除去                                                                                                                               | 7    | 数値                                   |                                                      |
| pref                     | 都道府県                                                                                                                                                        | 4    | 文字列                                 | 必須                                                 |
| city_address             | 市区町村番地 <br> 両端のスペース除去                                                                                                                            | 56   | 文字列                                 | 必須                                                 |
| building_name            | 建物名 <br> ※両端のスペース除去                                                                                                                                 | 60   | 文字列                                 |                                                      |
| set_post_address         | 郵送宛先情報 <br> 0:使用しない 1:使用する <br> ※両端のスペース除去                                                                                              | 1    | 数値                                   |                                                      |
| post_address1            | 郵送先宛名1  <br> 両端のスペース除去                                                                                                                            | 60   | 文字列                                 | (set_post_address=1時)                               |
| post_address2            | 郵送先宛名2 <br> ※両端のスペース除去                                                                                                                            | 60   | 文字列                                 |                                                      |
| post_address3            | 郵送先宛名3 <br> ※両端のスペース除去                                                                                                                            | 60   | 文字列                                 |                                                      |
| post_zip_code            | 郵送先郵便番号  <br> 両端のスペース除去                                                                                                                         | 7    | 数値                                   | (set_post_address=1時)                               |
| post_pref                | 郵送先都道府県                                                                                                                                                  | 4    | 文字列                                 | (set_post_address=1時)                               |
| post_city_address        | 郵送先市区町村番地                                                                                                                                              | 56   | 文字列                                 | (set_post_address=1時)                               |
| post_building_name       | 郵送先建物名 <br> ※両端のスペース除去                                                                                                                           | 60   | 文字列                                 |                                                      |
| tel                      | 電話番号  <br> 両端のスペース除去                                                                                                                               | 15   | 数値、半角ハイフン                     | (payment_method=1,2,6,7時)                           |
| email                    | メールアドレス <br> 両端のスペース除去                                                                                                                          | 100  | [メール形式](../../index.md#種別)      | 必須                                                 |
| cc_email                 | CC送信先メールアドレス <br> ※両端のスペース除去                                                                                                                 | 256  | [メール形式](../../index.md#種別)      |                                                      |
| bs_owner_code            | 請求元担当者コード <br> ※両端のスペース除去                                                                                                                     | 20   | [半角英数 + 記号](../../index.md#種別) |                                                      |
| payment_method           | 決済手段 <br> 0:銀行振込 1:クレジットカード 2:バンクチェック <br> 3:RP口座振替 4:RL口座振替 5:その他口座振替 <br> 6:コンビニ払込票(A4) 7:コンビニ払込票(ハガキ) | 1    | 数値                                   | 必須                                                 |
| source_bank_account_name | 振込元口座名義 <br> ※payment_method=0,2の場合に必要(入力は任意)                                                                                                 | 48   | [口座名義](../../index.md#種別)        |                                                      |
| credit_number            | クレジットカード番号(ハイフンはつけない)  <br> 両端のスペース除去                                                                                               | 20   | 数値                                   | (payment_method=1時)                                 |
| credit_expiration_date   | クレジットカード有効期限(YYMM形式)  <br> 両端のスペース除去                                                                                                     | 4    | 数値                                   | (payment_method=1時)                                 |
| credit_first_name        | クレジットカード名義(名)  <br> 両端のスペース除去                                                                                                               | 20   | [アルファベット](../../index.md#種別)  | (payment_method=1時)                                 |
| credit_last_name         | クレジットカード名義(姓)  <br> 両端のスペース除去                                                                                                               | 20   | [アルファベット](../../index.md#種別)  | (payment_method=1時)                                 |
| customer_number          | 顧客番号 <br> ※payment_method=5の場合に必要(入力は任意)                                                                                                         | 20   | [半角英数](../../index.md#種別)        |                                                      |
| bank_code                | 銀行コード <br> ※payment_method=3,4,5の場合に必要(入力は任意)                                                                                                   | 4    | 数値                                   |                                                      |
| bank_name                | 銀行名 <br> ※payment_method=5の場合に必要(入力は任意) <br> ※ゆうちょの場合は「ﾕｳﾁﾖ」                                                                            | 15   | [銀行名等](../../index.md#種別)        |                                                      |
| branch_code              | 支店コード <br> ※payment_method=3,4,5の場合に必要(入力は任意)                                                                                                   | 3    | 数値                                   |                                                      |
| branch_name              | 支店名 <br> ※payment_method=5の場合に必要(入力は任意)                                                                                                           | 15   | [銀行名等](../../index.md#種別)        |                                                      |
| bank_account_type        | 預金種目 <br> 1:普通 2:当座                                                                                                                                     | 1    | 数値                                   | (payment_method=3,4,5時)                             |
| bank_account_number      | 口座番号 <br> ※payment_method=3,4,5の場合に必要(入力は任意) <br> ※payment_method=3でゆうちょの時の桁数は8桁固定                                                 | 7    | 数値                                   |                                                      |
| bank_account_name        | 口座名義 <br> ※payment_method=3,4,5の場合に必要(入力は任意) <br> ※payment_method=3時の桁数は30桁                                                                | 100  | [口座名義](../../index.md#種別)        |                                                      |
| ref_billing_code         | 合計請求書用請求先コード                                                                                                                                        | 20   | [半角英数 + 記号](../../index.md#種別) | (ref_individual_number or ref_individual_code指定時) |
| ref_individual_number    | 合計請求書用請求先部署番号                                                                                                                                      | 20   | 数値                                   | (合計請求書時)^1                                     |
| ref_individual_code      | 合計請求書用請求先部署コード                                                                                                                                    | 20   | [半角英数 + 記号](../../index.md#種別) | (合計請求書時)^1                                     |
| bill_template_code       | 請求書テンプレートコード <br> ※合計請求書をご利用される場合は、お手数ですがお問い合わせください                                                                 |      | 数値                                   |                                                      |
| payment_method_code      | 決済情報コード                                                                                                                                                  | 20   | [半角英数 + 記号](../../index.md#種別) |                                                      |
| payment_method_name      | 決済情報名                                                                                                                                                      | 100  | 文字列                                 |                                                      |
| account_receivable_code  | 請求先部署売掛金補助科目コード <br> ※追加省略時、nullで登録される                                                                                               | 25   | 文字列                                 |                                                      |
| advances_received_code   | 請求先部署前受金補助科目コード <br> ※追加省略時、nullで登録される                                                                                               | 25   | 文字列                                 |                                                      |
| suspense_received_code   | 請求先部署仮受金補助科目コード <br> ※追加省略時、nullで登録される                                                                                               | 25   | 文字列                                 |                                                      |


## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                         | 概要                     | 型               |
| ---------------------------- | ------------------------ | ---------------- |
| [billing](#billing-response) | 請求先に属するパラメータ | `array` |

#### billing (response)

<!-- 要素が多くないものは detail, summaryタグを使わない (なくても見やすくため) -->
下記のような項目のオブジェクトを持つリスト

| 名前                                              | 概要                         | 型                          |
| ------------------------------------------------- | ---------------------------- | --------------------------- |
| code                                              | 請求先コード                 | string                      |
| name                                              | 請求先名                     | string                      |
| [billing_individual](#billingindividual-response) | 請求先部署に属するパラメータ | `array` |

#### billing_individual (response)

<details open>
<summary>クリックして隠す/表示</summary>

下記のような項目のオブジェクトを持つリスト

| 名前                           | 概要                                                                                                               | 型     |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------ | ------ |
| number                         | 請求先部署番号                                                                                                     | int    |
| code                           | 請求先部署コード                                                                                                   | string |
| name                           | 請求先部署名                                                                                                       | string |
| link_customer_code             | 会計ソフト連携用取引先コード                                                                                       | string |
| address1                       | 宛名1                                                                                                              | string |
| address2                       | 宛名2                                                                                                              | string |
| address3                       | 宛名3                                                                                                              | string |
| zip_code                       | 郵便番号                                                                                                           | string |
| pref                           | 都道府県                                                                                                           | string |
| city_address                   | 市区町村番地                                                                                                       | string |
| building_name                  | 建物名                                                                                                             | string |
| set_post_address               | 郵送宛先情報 <br> 0:使用しない 1:使用する                                                                          | int    |
| post_address1                  | 郵送先宛名1                                                                                                        | string |
| post_address2                  | 郵送先宛名2                                                                                                        | string |
| post_address3                  | 郵送先宛名3                                                                                                        | string |
| post_zip_code                  | 郵送先郵便番号                                                                                                     | string |
| post_pref                      | 郵送先都道府県                                                                                                     | string |
| post_city_address              | 郵送先市区町村番地                                                                                                 | string |
| post_building_name             | 郵送先建物名                                                                                                       | string |
| tel                            | 電話番号                                                                                                           | string |
| email                          | メールアドレス                                                                                                     | string |
| cc_email                       | CC送信先メールアドレス                                                                                             | string |
| bs_owner_code                  | 請求元担当者コード                                                                                                 | string |
| payment_method                 | 決済手段 <br> 0:銀行振込 1:クレジットカード 2:バンクチェック <br> 3:RP口座振替 4:RL口座振替 5:その他口座振替       | int    |
| register_status                | 登録ステータス <br> 0:未登録 1:登録待ち 2:メール送信済み 3:申請中 <br> 4:登録情報_送信エラー 5:登録完了 6:登録失敗 | int    |
| source_bank_account_name       | 振込元口座名義 <br> ※payment_method=0,2以外はNULL                                                                  | string |
| customer_number                | 顧客番号 <br> ※payment_method=5以外はNULL                                                                          | string |
| bank_code                      | 銀行コード <br> ※payment_method=3,4,5以外はNULL                                                                    | string |
| bank_name                      | 銀行名 <br> ※payment_method=5以外はNULL                                                                            | string |
| branch_code                    | 支店コード <br> ※payment_method=3,4,5以外はNULL                                                                    | string |
| branch_name                    | 支店名 <br> ※payment_method=5以外はNULL                                                                            | string |
| bank_account_type              | 預金種目 <br> 1:普通 2:当座 <br> ※payment_method=3,4,5以外はNULL                                                   | string |
| bank_account_number            | 口座番号 <br> ※payment_method=3,4,5以外はNULL                                                                      | string |
| bank_account_name              | 口座名義 <br> ※payment_method=3,4,5以外はNULL                                                                      | string |
| gid                            | 決済番号 <br> ※payment_method=1以外はNULL                                                                          | string |
| bank_check_bank_code           | バンクチェック銀行コード <br> ※payment_method=2以外はNULL                                                          | string |
| bank_check_bank_name           | バンクチェック銀行名 <br> ※payment_method=2以外はNULL                                                              | string |
| bank_check_branch_code         | バンクチェック支店コード <br> ※payment_method=2以外はNULL                                                          | string |
| bank_check_branch_name         | バンクチェック支店名 <br> ※payment_method=2以外はNULL                                                              | string |
| bank_check_kind                | バンクチェック口座種別 <br> ※NULL string                                                                           | string |
| bank_check_bank_account_number | バンクチェック口座番号 <br> ※payment_method=2以外はNULL                                                            | string |
| ref_billing_code               | 合計請求書用請求先コード                                                                                           | string |
| ref_individual_number          | 合計請求書用請求先部署番号                                                                                         | string |
| ref_individual_code            | 合計請求書用請求先部署コード                                                                                       | string |
| bill_template_code             | 請求書テンプレートコード                                                                                           | string |
| payment_method_number          | 決済情報番号                                                                                                       | int    |
| payment_method_code            | 決済情報コード                                                                                                     | string |
| payment_method_name            | 決済情報名                                                                                                         | string |
| account_receivable_code        | 請求先部署売掛金補助科目コード                                                                                     | string |
| advances_received_code         | 請求先部署前受金補助科目コード                                                                                     | string |
| suspense_received_code         | 請求先部署仮受金補助科目コード                                                                                     | string |

</details>


## 使用例

### リクエスト例

```
user_id: "sample@robotpayment.co.jp"
access_key: "xxxxxxxxxxxxxxxx"
billing_code: "billing"
billing_name: "請求先名"
billing_individual_code:"bicd0001"
billing_individual_name: "請求先部署名"
link_customer_code: "1234"
address1: "宛名1"
address2: "宛名2"
address3: "宛名3"
zip_code: "1651111"
pref: "東京都"
city_address: "渋谷区"
building_name: "建物名"
set_post_address: "1"
post_address1: "郵送先宛名1"
post_address2: "郵送先宛名2"
post_address3: "郵送先宛名3"
post_zip_code: "1651110"
post_pref: "東京都"
post_city_address: "港区"
post_building_name: "建物名"
tel: "0811112222"
email: email@robotpayment.co.jp
cc_email:sub1email@robotpayment.co.jp,sub2email@robotpayment.co.jp
bs_owner_code:"bs_owner_code"
payment_method: 0
source_bank_account_name: "ｹｲﾘﾉﾐｶﾀ"
credit_number: null
credit_expiration_date: null
credit_first_name: null
credit_last_name: null
customer_number: null
bank_code: null
bank_name: null
branch_code: null
branch_name: null
bank_account_type: null
bank_account_number: null
bank_account_name: null
ref_billing_code: null
ref_individual_number: null
ref_individual_code: null
bill_template_code: null
payment_method_number: 123
payment_method_code: "bank-123"
account_receivable_code: null
advances_received_code: null
suspense_received_code: null
```

### レスポンス例

Status: 200 OK

```json
{
    "billing": {
        "code": "billing",
        "name": "請求先名",
        "billing_individual": {
            "number": 1,
            "code": "bicd0001",
            "name": "請求先部署名",
            "link_customer_code": "1234",
            "address1": "宛名1",
            "address2": "宛名2",
            "address3": "宛名3",
            "zip_code": "1651111",
            "pref": "東京都",
            "city_address": "渋谷区",
            "building_name": "建物名",
            "set_post_address": 1,
            "post_address1": "郵送先宛名1",
            "post_address2": "郵送先宛名2",
            "post_address3": "郵送先宛名3",
            "post_zip_code": "1651110",
            "post_pref": "東京都",
            "post_city_address": "港区",
            "post_building_name": "建物名",
            "tel": "0811112222",
            "email": "email@gmail.com",
            "cc_email": " sub1email@gmail.com,sub2email@gmail.com",
            "bs_owner_code": "bs_owner_code",
            "payment_method": 0,
            "register_status": 5,
            "source_bank_account_name": " ｹｲﾘﾉﾐｶﾀ",
            "customer_number": null,
            "bank_code": null,
            "bank_name": null,
            "branch_code": null,
            "branch_name": null,
            "bank_account_type": null,
            "bank_account_number": null,
            "bank_account_name": null,
            "gid": null,
            "bank_check_bank_code": null,
            "bank_check_bank_name": null,
            "bank_check_branch_code": null,
            "bank_check_branch_name": null,
            "bank_check_kind": null,
            "bank_check_bank_account_number": null,
            "ref_billing_code": null,
            "ref_individual_number": null,
            "ref_individual_code": null,
            "bill_template_code": null,
            "payment_method_number": 123,
            "payment_method_code": "bank-123",
            "payment_method_name": " 銀行振込123",
            "account_receivable_code": null,
            "advances_received_code": null,
            "suspense_received_code": null
        }
    }
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                                                 |
| ------------ | ---------------------------------------------------- |
| 101          | 請求先コードが不正                                   |
| 102          | 請求先名が不正                                       |
| 103          | 請求先部署名が不正                                   |
| 104          | 会計ソフト連携用取引先コードが不正                   |
| 105          | 宛名1が不正                                          |
| 106          | 宛名2が不正                                          |
| 107          | 宛名3が不正                                          |
| 108          | 郵便番号が不正                                       |
| 109          | 都道府県が不正                                       |
| 110          | 市区町村番地が不正                                   |
| 111          | 建物名が不正                                         |
| 112          | 電話番号が不正                                       |
| 113          | メールアドレスが不正                                 |
| 114          | 決済手段が不正                                       |
| 115          | 振込元口座名義が不正                                 |
| 116          | クレジットカード番号が不正                           |
| 117          | クレジットカード有効期限が不正                       |
| 118          | クレジットカード名義(名)が不正                       |
| 119          | クレジットカード名義(姓)が不正                       |
| 120          | 銀行コードが不正                                     |
| 121          | 支店コードが不正                                     |
| 122          | 預金種目が不正                                       |
| 123          | 口座番号が不正                                       |
| 124          | 口座名義が不正                                       |
| 125          | 請求先登録に失敗                                     |
| 126          | CC送信先メールアドレスが不正                         |
| 127          | 顧客番号が不正                                       |
| 128          | 銀行名が不正                                         |
| 129          | 支店名が不正                                         |
| 130          | 請求元担当者コードが不正                             |
| 131          | 郵送先宛名1が不正                                    |
| 132          | 郵送先宛名2が不正                                    |
| 133          | 郵送先宛名3が不正                                    |
| 134          | 郵送先郵便番号が不正                                 |
| 135          | 郵送先都道府県が不正                                 |
| 136          | 郵送先市区町村番地が不正                             |
| 137          | 郵送先建物名が不正                                   |
| 138          | 郵送宛先利用フラグが不正                             |
| 139          | 請求先部署コードが不正                               |
| 140          | 請求先部署番号と請求先部署コードは同時に指定できない |
| 152          | 請求先決済情報番号が不正                             |
| 153          | 請求先決済情報コードが不正                           |
| 154          | 請求先決済情報名が不正                               |
| 155          | 請求先部署売掛金補助科目コード                       |
| 156          | 請求先部署前受金補助科目コード                       |
| 157          | 請求先部署仮受金補助科目コード                       |