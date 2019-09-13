# 請求先部署編集

`/api/billing_individual/update`

請求先部署情報の編集処理を実行します。
既存の請求先部署編集APIとの相違点はクレジットカード登録の扱いが異なる点です。

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/billing_individual/update`
- Preferred HTTP method: `POST`
- Accepted content types: `application/x-www-form-urulencoded`
- Encode: `UTF-8`

### Parameters

| 名前                      | 概要                                                                                                         | 桁数 | 種別                               | 必須                     |
| ------------------------- | ------------------------------------------------------------------------------------------------------------ | ---- | ---------------------------------- | ------------------------ |
| user_id                   | ユーザーID（管理画面へのログインID）                                                                         | 100  | [半角英数\*1](/README.md#種別注釈) | 必須                     |
| access_key                | アクセスキー                                                                                                 | 100  | [半角英数\*3](/README.md#種別注釈) | 必須                     |
| billing_code              | 請求先コード                                                                                                 | 20   | [半角英数\*4](/README.md#種別注釈) | 必須                     |
| billing_individual_number | 請求先部署番号                                                                                               | 20   | 数値                               | 必須                     |
| billing_individual_name   | 請求先部署名                                                                                                 | 100  | 文字列                             | 必須                     |
| link_customer_code        | 会計ソフト連携用取引先コード                                                                                 | 20   | [半角英数\*3](/README.md#種別注釈) |                          |
| address1                  | 宛名1                                                                                                        | 60   | 文字列                             | 必須                     |
| address2                  | 宛名2                                                                                                        | 60   | 文字列                             |                          |
| address3                  | 宛名3                                                                                                        | 60   | 文字列                             |                          |
| zip_code                  | 郵便番号                                                                                                     | 7    | 数値                               | 必須                     |
| pref                      | 都道府県                                                                                                     | 4    | 文字列                             | 必須                     |
| city_address              | 市区町村番地                                                                                                 | 56   | 文字列                             | 必須                     |
| building_name             | 建物名                                                                                                       | 60   | 文字列                             |                          |
| tel                       | 電話番号                                                                                                     | 15   | 数値、半角ハイフン                 | (payment_method=1,2時)   |
| email                     | メールアドレス                                                                                               | 100  | [半角英数\*1](/README.md#種別注釈) | 必須                     |
| cc_email                  | CC送信先メールアドレス                                                                                       | 256  | [半角英数\*1](/README.md#種別注釈) |                          |
| bs_owner_code             | 請求元担当者コード                                                                                           | 20   | [半角英数\*4](/README.md#種別注釈) |                          |
| payment_method            | 決済手段 <br> 0:銀行振込 1:クレジットカード 2:バンクチェック <br> 3:RP口座振替 4:RL口座振替 5:その他口座振替 | 1    | 数値                               | 必須                     |
| source_bank_account_name  | 振込元口座名義 <br> ※payment_method=0,2の場合のみ必要(入力は任意)                                            | 48   | [半角英数\*3](/README.md#種別注釈) |                          |
| customer_number           | 顧客番号 <br> ※payment_method=5の場合に必要(入力は任意)                                                      | 15   | [半角英数\*2](/README.md#種別注釈) |                          |
| bank_code                 | 銀行コード <br> ※payment_method=3,4,5の場合に必要(入力は任意)                                                | 4    | 数値                               |                          |
| bank_name                 | 銀行名 <br> ※payment_method=5の場合に必要(入力は任意) <br> ※ゆうちょの場合は「ﾕｳﾁﾖ」                         | 15   | [半角英数\*6](/README.md#種別注釈) |                          |
| branch_code               | 支店コード <br> ※payment_method=3,4,5の場合に必要(入力は任意)                                                | 3    | 数値                               |                          |
| branch_name               | 支店名 <br> ※payment_method=5の場合に必要(入力は任意)                                                        | 15   | [半角英数\*6](/README.md#種別注釈) |                          |
| bank_account_type         | 貯金種目 <br> 1:普通 2:当座                                                                                  | 1    | 数値                               | (payment_method=3,4,5時) |
| bank_account_number       | 口座番号 <br> ※payment_method=3,4,5の場合に必要(入力は任意)                                                  | 7    | 数値                               |                          |
| bank_account_name         | 口座名義 <br> ※payment_method=3,4,5の場合に必要(入力は任意)                                                  | 100  | [半角英数\*5](/README.md#種別注釈) |                          |


## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                                              | 概要                         | 型                          |
| ------------------------------------------------- | ---------------------------- | --------------------------- |
| [billing_individual](#billingindividual-response) | 請求先部署に属するパラメータ | `Array(billing_individual)` |

#### billing_individual (response)

<!-- ネストが単純なので detail, summaryタグを使わない (なくても見やすくため) -->
下記のような項目のオブジェクトを持つリスト

| 名前                     | 概要                                                                                                               | 型     |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------ | ------ |
| number                   | 請求先部署番号                                                                                                     | Int    |
| name                     | 請求先部署名                                                                                                       | string |
| link_customer_code       | 会計ソフト連携用取引先コード                                                                                       | string |
| address1                 | 宛名1                                                                                                              | string |
| address2                 | 宛名2                                                                                                              | string |
| address3                 | 宛名3                                                                                                              | string |
| zip_code                 | 郵便番号                                                                                                           | string |
| pref                     | 都道府県                                                                                                           | string |
| city_address             | 市区町村番地                                                                                                       | string |
| building_name            | 建物名                                                                                                             | string |
| tel                      | 電話番号                                                                                                           | string |
| email                    | メールアドレス                                                                                                     | string |
| cc_email                 | CC送信先メールアドレス                                                                                             | string |
| bs_owner_code            | 請求元担当者コード                                                                                                 | string |
| payment_method           | 決済手段 <br> 0:銀行振込 1:クレジットカード 2:バンクチェック <br> 3:RP口座振替 4:RL口座振替 5:その他口座振替       | Int    |
| register_status          | 登録ステータス <br> 0:未登録 1:登録待ち 2:メール送信済み 3:申請中 <br> 4:登録情報_送信エラー 5:登録完了 6:登録失敗 | Int    |
| source_bank_account_name | 振込元口座名義 <br> ※payment_method=0,2以外はNULL                                                                  | string |
| customer_number          | 顧客番号 <br> ※payment_method=3以外はNULL                                                                          | string |
| bank_code                | 銀行コード <br> ※payment_method=3,4,5以外はNULL                                                                    | string |
| bank_name                | 銀行名 <br> ※payment_method=5以外はNULL                                                                            | string |
| branch_code              | 支店コード <br> ※payment_method=3,4,5以外はNULL                                                                    | string |
| branch_name              | 支店名 <br> ※payment_method=5以外はNULL                                                                            | string |
| bank_account_type        | 貯金種目 <br> 1:普通 2:当座 <br> ※payment_method=3,4,5以外はNULL                                                   | string |
| bank_account_number      | 口座番号 <br> ※payment_method=3,4,5以外はNULL                                                                      | string |
| bank_account_name        | 口座名義 <br> ※payment_method=3,4,5以外はNULL                                                                      | string |
| cod                      | 店舗オーダー番号 <br> ※payment_method=1以外はNULL                                                                  | string |


## 使用例

### リクエスト

```
user_id: "sample@robotpayment.co.jp"
access_key: "xxxxxxxxxxxxxxxx"
billing_code: "billing"
billing_individual_number: "請求先部署番号"
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

```json
{
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
        "cod": null
    }
}
```

## エラー

[共通エラー](/README.md#共通エラー)

個別エラー

| エラーコード | 内容                                                                         |
| ------------ | ---------------------------------------------------------------------------- |
| 601          | 請求先コードが不正                                                           |
| 602          | 請求先部署番号が不正                                                         |
| 603          | 請求先部署名が不正                                                           |
| 604          | 会計ソフト連携用取引先コードが不正                                           |
| 605          | 宛名1が不正                                                                  |
| 606          | 宛名2が不正                                                                  |
| 607          | 宛名3が不正                                                                  |
| 608          | 郵便番号が不正                                                               |
| 609          | 都道府県が不正                                                               |
| 610          | 市区町村番地が不正                                                           |
| 611          | 建物名が不正                                                                 |
| 612          | 電話番号が不正                                                               |
| 613          | メールアドレスが不正                                                         |
| 614          | 決済手段が不正                                                               |
| 615          | 振込元口座名義が不正                                                         |
| 616          | クレジットカード番号が不正                                                   |
| 617          | クレジットカード有効期限が不正                                               |
| 618          | クレジットカード名義(名)が不正                                               |
| 619          | クレジットカード名義(姓)が不正                                               |
| 620          | 銀行コードが不正                                                             |
| 621          | 支店コードが不正                                                             |
| 622          | 預金種目が不正                                                               |
| 623          | 口座番号が不正                                                               |
| 624          | 口座名義が不正                                                               |
| 625          | 請求先編集に失敗                                                             |
| 626          | 請求先部署が存在しない                                                       |
| 627          | 顧客番号が不正                                                               |
| 628          | 銀行名が不正                                                                 |
| 629          | 支店名が不正                                                                 |
| 630          | 請求元担当者コードが不正                                                     |
| 631          | CC送信先メールアドレスが不正                                                 |
| 632          | 郵送先宛名1が不正                                                            |
| 633          | 郵送先宛名2が不正                                                            |
| 634          | 郵送先宛名3が不正                                                            |
| 635          | 郵送先郵便番号が不正                                                         |
| 636          | 郵送先都道府県が不正                                                         |
| 637          | 郵送先市区町村番地が不正                                                     |
| 638          | 郵送先建物名が不正                                                           |
| 639          | 郵送宛先利用フラグが不正                                                     |
| 640          | 請求先部署コードが不正                                                       |
| 641          | 指定した合計請求書用請求先コードが不正                                       |
| 642          | 指定した合計請求書用請求先部署番号が不正                                     |
| 643          | 指定した合計請求書用請求先部署コードが不正                                   |
| 644          | 指定した合計請求書用請求書テンプレートコードが不正                           |
| 645          | 指定した合計請求書用請求先部署が存在しない                                   |
| 646          | 指定した合計請求書用請求先部署が、既に合計請求書用請求先部署を持っている     |
| 647          | 指定した合計請求書用請求先部署の登録ステータスが不正                         |
| 648          | 指定した合計請求書用請求先部署の決済手段が不正                               |
| 649          | 請求先部署が、既に他の合計請求書用請求先部署として登録されている             |
| 650          | 指定した合計請求書用請求先部署が承認中                                       |
| 651          | 合計請求書用請求先部署番号と合計請求書用請求先部署コードは同時に指定できない |
| 653          | 請求先決済情報番号が不正                                                     |
| 654          | 請求先決済情報コードが不正                                                   |
| 655          | 請求先決済情報名が不正                                                       |
| 656          | 請求先部署売掛金補助科目コード                                               |
| 657          | 請求先部署前受金補助科目コード                                               |
| 658          | 請求先部署仮受金補助科目コード                                               |
