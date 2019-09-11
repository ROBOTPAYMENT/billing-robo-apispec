# 請求先登録

`/api/billing/register`

請求先情報処理を実行します。
既存の請求先登録APIとの相違点はクレジットカード登録の扱いが異なる点です。

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/billing/register`
- Preferred HTTP method: `POST`
- Accepted content types: `application/x-www-form-urulencoded`
- Encode: `UTF-8`

### Parameters

| 名前                     | 概要                                                                                                         | 桁数 | 種別                               | 必須                     |
| ------------------------ | ------------------------------------------------------------------------------------------------------------ | ---- | ---------------------------------- | ------------------------ |
| user_id                  | ユーザーID（管理画面へのログインID）                                                                         | 100  | [半角英数\*1](/README.md#種別注釈) | 必須                     |
| access_key               | アクセスキー                                                                                                 | 100  | [半角英数\*4](/README.md#種別注釈) | 必須                     |
| billing_code             | 請求先コード                                                                                                 | 20   | [半角英数\*3](/README.md#種別注釈) | 必須                     |
| billing_name             | 請求先名                                                                                                     | 100  | 文字列                             | 必須                     |
| billing_individual_name  | 請求先部署名                                                                                                 | 100  | 文字列                             | 必須                     |
| link_customer_code       | 会計ソフト連携用取引先コード                                                                                 | 20   | [半角英数\*3](/README.md#種別注釈) |                          |
| address1                 | 宛名1                                                                                                        | 60   | 文字列                             | 必須                     |
| address2                 | 宛名2                                                                                                        | 60   | 文字列                             |                          |
| address3                 | 宛名3                                                                                                        | 60   | 文字列                             |                          |
| zip_code                 | 郵便番号                                                                                                     | 7    | 数値                               | 必須                     |
| pref                     | 都道府県                                                                                                     | 4    | 文字列                             | 必須                     |
| city_address             | 市区町村番地                                                                                                 | 56   | 文字列                             | 必須                     |
| building_name            | 建物名                                                                                                       | 60   | 文字列                             |                          |
| tel                      | 電話番号                                                                                                     | 15   | 数値、半角ハイフン                 | (payment_method=1,2時)   |
| email                    | メールアドレス                                                                                               | 100  | [半角英数\*1](/README.md#種別注釈) | 必須                     |
| cc_email                 | CC送信先メールアドレス                                                                                       | 256  | [半角英数\*1](/README.md#種別注釈) |                          |
| bs_owner_code            | 請求元担当者コード                                                                                           | 20   | [半角英数\*4](/README.md#種別注釈) |                          |
| payment_method           | 決済手段 <br> 0:銀行振込 1:クレジットカード 2:バンクチェック <br> 3:RP口座振替 4:RL口座振替 5:その他口座振替 | 1    | 数値                               | 必須                     |
| source_bank_account_name | 振込元口座名義 <br> ※payment_method=0,2の場合のみ必要(入力は任意)                                            | 48   | [半角英数\*3](/README.md#種別注釈) |                          |
| customer_number          | 顧客番号 <br> ※payment_method=5の場合に必要(入力は任意)                                                      | 15   | [半角英数\*2](/README.md#種別注釈) |                          |
| bank_code                | 銀行コード <br> ※payment_method=3,4,5の場合に必要(入力は任意)                                                | 4    | 数値                               |                          |
| bank_name                | 銀行名 <br> ※payment_method=5の場合に必要(入力は任意) <br> ※ゆうちょの場合は「ﾕｳﾁﾖ」                         | 15   | [半角英数\*6](/README.md#種別注釈) |                          |
| branch_code              | 支店コード <br> ※payment_method=3,4,5の場合に必要(入力は任意)                                                | 3    | 数値                               |                          |
| branch_name              | 支店名 <br> ※payment_method=5の場合に必要(入力は任意)                                                        | 15   | [半角英数\*6](/README.md#種別注釈) |                          |
| bank_account_type        | 貯金種目 <br> 1:普通 2:当座                                                                                  | 1    | 数値                               | (payment_method=3,4,5時) |
| bank_account_number      | 口座番号 <br> ※payment_method=3,4,5の場合に必要(入力は任意)                                                  | 7    | 数値                               |                          |
| bank_account_name        | 口座名義 <br> ※payment_method=3,4,5の場合に必要(入力は任意)                                                  | 100  | [半角英数\*5](/README.md#種別注釈) |                          |


## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                                        | 概要                                                                                                               | 型     |
| ------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ | ------ |
| code                                        | 請求先コード                                                                                                       | string |
| name                                        | 請求先名                                                                                                           | string |
| billing_individual.number                   | 請求先部署番号                                                                                                     | Int    |
| billing_individual.name                     | 請求先部署名                                                                                                       | string |
| billing_individual. link_customer_code      | 会計ソフト連携用取引先コード                                                                                       | string |
| billing_individual.address1                 | 宛名1                                                                                                              | string |
| billing_individual.address2                 | 宛名2                                                                                                              | string |
| billing_individual.address3                 | 宛名3                                                                                                              | string |
| billing_individual.zip_code                 | 郵便番号                                                                                                           | string |
| billing_individual.pref                     | 都道府県                                                                                                           | string |
| billing_individual.city_address             | 市区町村番地                                                                                                       | string |
| billing_individual.building_name            | 建物名                                                                                                             | string |
| billing_individual.tel                      | 電話番号                                                                                                           | string |
| billing_individual.email                    | メールアドレス                                                                                                     | string |
| billing_individual. cc_email                | CC送信先メールアドレス                                                                                             | string |
| billing_individual.bs_owner_code            | 請求元担当者コード                                                                                                 | string |
| billing_individual.payment_method           | 決済手段 <br> 0:銀行振込 1:クレジットカード 2:バンクチェック <br> 3:RP口座振替 4:RL口座振替 5:その他口座振替       | Int    |
| billing_individual. register_status         | 登録ステータス <br> 0:未登録 1:登録待ち 2:メール送信済み 3:申請中 <br> 4:登録情報_送信エラー 5:登録完了 6:登録失敗 | Int    |
| billing_individual.source_bank_account_name | 振込元口座名義 <br> ※payment_method=0,2以外はNULL                                                                  | string |
| billing_individual.customer_number          | 顧客番号 <br> ※payment_method=3以外はNULL                                                                          | string |
| billing_individual.bank_code                | 銀行コード <br> ※payment_method=3,4,5以外はNULL                                                                    | string |
| billing_individual.bank_name                | 銀行名 <br> ※payment_method=5以外はNULL                                                                            | string |
| billing_individual.branch_code              | 支店コード <br> ※payment_method=3,4,5以外はNULL                                                                    | string |
| billing_individual.branch_name              | 支店名 <br> ※payment_method=5以外はNULL                                                                            | string |
| billing_individual.bank_account_type        | 貯金種目 <br> 1:普通 2:当座 <br> ※payment_method=3,4,5以外はNULL                                                   | string |
| billing_individual.bank_account_number      | 口座番号 <br> ※payment_method=3,4,5以外はNULL                                                                      | string |
| billing_individual.bank_account_name        | 口座名義 <br> ※payment_method=3,4,5以外はNULL                                                                      | string |
| billing_individual.cod                      | 店舗オーダー番号 <br> ※payment_method=1以外はNULL                                                                  | string |


## 使用例

### リクエスト

```
user_id: "sample@robotpayment.co.jp"
access_key: "xxxxxxxxxxxxxxxx"
billing_code: "billing"
billing_name: "請求先名"
billing_individual_name: "請求先部署名"
link_customer_code: "1234"
address1: "宛名1"
address2: "宛名2"
address3: "宛名3"
zip_code: "1651111"
pref: "東京都"
city_address: "渋谷区"
building_name: "建物名"
tel: "0811112222"
email: email@robotpayment.co.jp
cc_email:sub1email@robotpayment.co.jp,sub2email@robotpayment.co.jp
bs_owner_code:"bs_owner_code"
payment_method: 0
source_bank_account_name: "ｹｲﾘﾉﾐｶﾀ"
customer_number: null
bank_code: null
bank_name: null
branch_code: null
branch_name: null
bank_account_type: null
bank_account_number: null
bank_account_name: null
```

### レスポンス

Status: 200 OK

```
{
    "billing": {
        "code": "billing",
        "name": "請求先名",
        "billing_individual": {
            "number": 1,
            "name": "請求先部署名",
            "link_customer_code": "1234",
            "address1": "宛名1",
            "address2": "宛名2",
            "address3": "宛名3",
            "zip_code": "1651111",
            "pref": "東京都",
            "city_address": "渋谷区",
            "building_name": "建物名",
            "tel": "0811112222",
            "email": "email@robotpayment.co.jp",
            "cc_email": " sub1email@robotpayment.co.jp,sub2email@robotpayment.co.jp",
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
            "cod": null
        }
    }
}
```

## エラー

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