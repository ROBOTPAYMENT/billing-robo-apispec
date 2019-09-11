# 請求先登録更新(複数)

`/api/v1.0/billing/bulk_upsert`

複数の請求先、請求先部署、請求先決済手段、請求先補助科目を登録または更新します。
停止中の請求先、請求先部署、決済情報を指定して送信すると、有効な状態に復元します。

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/v1.0/billing/bulk_upsert`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                | 概要                                 | 桁数 | 種別                               | 必須 |
| ------------------- | ------------------------------------ | ---- | ---------------------------------- | ---- |
| user_id             | ユーザーID（管理画面へのログインID） | 100  | [半角英数\*1](/README.md#種別注釈) | 必須 |
| access_key          | アクセスキー                         | 100  | [半角英数\*3](/README.md#種別注釈) | 必須 |
| [billing](#billing) | 請求先に属するパラメータ             |      | list                               |      |

#### billing


<details>
<summary>show child arguments</summary>

下記のような項目のオブジェクトを持つリスト

| 名前                                         | 概要                                                                            | 桁数 | 種別                               | 必須     |
| -------------------------------------------- | ------------------------------------------------------------------------------- | ---- | ---------------------------------- | -------- |
| code                                         | 請求先コード  <br> ※両端のスペース除去 <br> ※登録後は変更できません             | 20   | [半角英数\*4](/README.md#種別注釈) | 必須     |
| name                                         | 請求先名  <br> ※両端のスペース除去                                              | 100  | 文字列                             | (追加時) |
| [individual](#individual)                    | 請求先部署に属するパラメータ                                                    |      | list                               |          |
| [payment](#payment)                          | 請求先決済手段に属するパラメータ ※省略可能                                      |      | list                               |          |
| [sub_account_title](#billingsubaccounttitle) | 請求先補助科目に属するパラメータ <br> ※今後拡張予定です。現在利用はできません。 |      | list                               |          |

#### individual

<details>
<summary>show child arguments</summary>

下記のような項目のオブジェクトを持つリスト


| 名前                                                                      | 概要                                                                                                                                                                                                                                        | 桁数 | 種別                               | 必須                                 |
| ------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---- | ---------------------------------- | ------------------------------------ |
| number                                                                    | 請求先部署番号 <br> ※両端のスペース除去 <br> ※登録後は変更できません                                                                                                                                                                        | 18   | 数値                               | (更新時)^1                           |
| code                                                                      | 請求先部署コード <br> ※両端のスペース除去 <br> ※登録後は変更できません                                                                                                                                                                      | 20   | [半角英数\*4](/README.md#種別注釈) | (更新時)^1                           |
| name                                                                      | 請求先部署名  <br> ※両端のスペース除去                                                                                                                                                                                                      | 100  | 文字列                             |                                      |
| link_customer_code                                                        | 会計ソフト連携用取引先コード <br> ※追加省略時、nullで登録される                                                                                                                                                                             | 20   | [半角英数\*3](/README.md#種別注釈) |                                      |
| address1                                                                  | 宛名1  <br> ※両端のスペース除去                                                                                                                                                                                                             | 60   | 文字列                             | (追加時)                             |
| address2                                                                  | 宛名2 <br> ※両端のスペース除去 <br> ※追加省略時、nullで登録される                                                                                                                                                                           | 60   | 文字列                             |                                      |
| address3                                                                  | 宛名3 <br> ※両端のスペース除去 <br> ※追加省略時、nullで登録される                                                                                                                                                                           | 60   | 文字列                             |                                      |
| zip_code                                                                  | 郵便番号  <br> ※両端のスペース除去 <br> ※ハイフンありなしどちらでも可                                                                                                                                                                       | 7    | 文字列                             | (追加時)                             |
| pref                                                                      | 都道府県                                                                                                                                                                                                                                    | 4    | 文字列                             | (追加時)                             |
| city_address                                                              | 市区町村番地  <br> ※両端のスペース除去                                                                                                                                                                                                      | 56   | 文字列                             | (追加時)                             |
| building_name                                                             | 建物名 <br> ※両端のスペース除去 <br> ※追加省略時、nullで登録される                                                                                                                                                                          | 60   | 文字列                             |                                      |
| set_post_address                                                          | 郵送宛先情報 <br> 0:使用しない 1:使用する <br> ※省略時は、0と同義                                                                                                                                                                           | 1    | 数値                               |                                      |
| post_address1                                                             | 郵送先宛名1  <br> ※両端のスペース除去 <br> ※追加省略時、nullで登録される                                                                                                                                                                    | 60   | 文字列                             | (追加時でset_post_address=1時)       |
| post_address2                                                             | 郵送先宛名2 <br> ※両端のスペース除去 <br> ※追加省略時、nullで登録される                                                                                                                                                                     | 60   | 文字列                             |                                      |
| post_address3                                                             | 郵送先宛名3 <br> ※両端のスペース除去 <br> ※追加省略時、nullで登録される                                                                                                                                                                     | 60   | 文字列                             |                                      |
| post_zip_code                                                             | 郵送先郵便番号  <br> ※両端のスペース除去 <br> ※ハイフンありなしどちらでも可                                                                                                                                                                 | 7    | 文字列                             | (追加時でset_post_address=1時)       |
| post_pref                                                                 | 郵送先都道府県  <br> ※追加省略時、nullで登録される                                                                                                                                                                                          | 4    | 文字列                             | (追加時でset_post_address=1時)       |
| post_city_address                                                         | 郵送先市区町村番地  <br> ※両端のスペース除去 <br> ※追加省略時、nullで登録される                                                                                                                                                             | 56   | 文字列                             | (追加時でset_post_address=1時)       |
| post_building_name                                                        | 郵送先建物名 <br> ※両端のスペース除去 <br> ※追加省略時                                                                                                                                                                                      | 60   | 文字列                             |                                      |
| tel                                                                       | 電話番号  <br> ※両端のスペース除去 <br> ※追加省略時、nullで登録される                                                                                                                                                                       | 15   | 数値、半角ハイフン                 | (追加時でpayment_method=1,2,6,7,8時) |
| email                                                                     | メールアドレス  <br> ※両端のスペース除去                                                                                                                                                                                                    | 100  | [半角英数\*1](/README.md#種別注釈) | (追加時)                             |
| cc_email                                                                  | CC送信先メールアドレス <br> ※両端のスペース除去 <br> ※追加省略時、nullで登録される                                                                                                                                                          | 256  | [半角英数\*1](/README.md#種別注釈) |                                      |
| memo                                                                      | メモ <br> ※追加省略時、nullで登録される                                                                                                                                                                                                     | 300  | 文字列                             |                                      |
| billing_method                                                            | 請求方法 <br> 0:送付なし 1:自動メール 2:手動メール 3:自動郵送 <br> 4:手動郵送 5:自動メール+自動郵送 6:手動メール+手動郵送 <br> ※追加省略時、nullで登録される <br> ※請求情報を登録する際に利用する値です。事前に登録しておくことができます。 | 1    | 数値                               |                                      |
| issue_month                                                               | 請求書発行日_月 <br> 入力可能値:-36～36 <br> 例） <br> -1：前月 <br> 0：当月 <br> 36：36ヵ月後 <br> ※追加省略時、nullで登録される <br> ※請求情報を登録する際に利用する値です。事前に登録しておくことができます。                            | 2    | 数値                               |                                      |
| issue_day                                                                 | 請求書発行日_日 <br> 入力可能値:1～30、99(末日) <br> ※追加省略時、nullで登録される <br> ※請求情報を登録する際に利用する値です。事前に登録しておくことができます。                                                                           | 2    | 数値                               |                                      |
| sending_month                                                             | 請求書送付日_月 <br> 入力可能値:-36～36 <br> 例） <br> -1：前月 <br> 0：当月 <br> 36：36ヵ月後 <br> ※追加省略時、nullで登録される <br> ※請求情報を登録する際に利用する値です。事前に登録しておくことができます。                            | 2    | 数値                               |                                      |
| sending_day                                                               | 請求書送付日_日 <br> 入力可能値:1～30、99(末日) <br> ※追加省略時、nullで登録される <br> ※請求情報を登録する際に利用する値です。事前に登録しておくことができます。                                                                           | 2    | 数値                               |                                      |
| deadline_month                                                            | 決済期限_月 <br> 入力可能値:-36～36 <br> 例） <br> -1：前月 <br> 0：当月 <br> 36：36ヵ月後 <br> ※追加省略時、nullで登録される <br> ※請求情報を登録する際に利用する値です。事前に登録しておくことができます。                                | 2    | 数値                               |                                      |
| deadline_day                                                              | 決済期限_日 <br> 入力可能値:1～30、99(末日) <br> ※追加省略時、nullで登録される <br> ※請求情報を登録する際に利用する値です。事前に登録しておくことができます。                                                                               | 2    | 数値                               |                                      |
| payment_method_code                                                       | 決済情報コード <br> ※追加省略時、nullで登録される                                                                                                                                                                                           | 20   | [半角英数\*4](/README.md#種別注釈) |                                      |
| bs_owner_code                                                             | 請求元担当者コード <br> ※追加省略時、nullで登録される                                                                                                                                                                                       | 20   | [半角英数\*4](/README.md#種別注釈) |                                      |
| ref_billing_code                                                          | 合計請求書用請求先コード <br> ※追加省略時、nullで登録される                                                                                                                                                                                 | 20   | [半角英数\*4](/README.md#種別注釈) |                                      |
| ref_individual_number                                                     | 合計請求書用請求先部署番号 <br> ※追加省略時、nullで登録される                                                                                                                                                                               | 18   | 数値                               | (合計請求書時)^2                     |
| ref_individual_code                                                       | 合計請求書用請求先部署コード <br> ※追加省略時、nullで登録される                                                                                                                                                                             | 20   | [半角英数\*4](/README.md#種別注釈) | (合計請求書時)^2                     |
| bill_template_code                                                        | 請求書テンプレートコード <br> ※合計請求書をご利用される場合は、お手数ですがお問い合わせください <br> ※追加省略時、nullで登録される                                                                                                          | 18   | 数値                               |                                      |
| auto_erase_commission_amount                                              | 手数料自動消込許容金額 <br> ※追加省略時、0で登録される                                                                                                                                                                                      | 9    | 数値                               |                                      |
| [billing.individual.sub_account_title](#billingindividualsubaccounttitle) | 請求先部署補助科目に属するパラメータ ※省略可能                                                                                                                                                                                              |      | list                               |                                      |

#### billing.individual.sub_account_title

<details>
<summary>show child arguments</summary>

下記のような項目のオブジェクトを持つリスト

| 名前                    | 概要                                                              | 桁数 | 種別   | 必須 |
| ----------------------- | ----------------------------------------------------------------- | ---- | ------ | ---- |
| account_receivable_code | 請求先部署売掛金補助科目コード <br> ※追加省略時、nullで登録される | 25   | 文字列 |      |
| advances_received_code  | 請求先部署前受金補助科目コード <br> ※追加省略時、nullで登録される | 25   | 文字列 |      |
| suspense_received_code  | 請求先部署仮受金補助科目コード <br> ※追加省略時、nullで登録される | 25   | 文字列 |      |

<details>

</details>

#### payment

<details>
<summary>show child arguments</summary>

下記のような項目のオブジェクトを持つリスト

| 名前                       | 概要                                                                                                                                                                                     | 桁数     | 種別                               | 必須                               |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ---------------------------------- | ---------------------------------- |
| number                     | 決済情報番号  <br> ※両端のスペース除去 <br> ※登録後は変更できません                                                                                                                      | 18       | 半角数字                           | (更新時)^3                         |
| code                       | 決済情報コード <br> ※両端のスペース除去 <br> ※登録後は変更できません                                                                                                                     | 20       | [半角英数\*4](/README.md#種別注釈) | (更新時)^3                         |
| name                       | 決済情報名                                                                                                                                                                               | 100      | 文字列                             | (追加時)                           |
| bank_transfer_pattern_code | 請求元銀行口座パターンコード <br> ※payment_method=0の場合に必要(入力は任意) <br> ※追加省略時、1で登録される                                                                              | 20       | [半角英数\*4](/README.md#種別注釈) |                                    |
| payment_method             | 決済手段 <br> 0:銀行振込 1:クレジットカード 2:バンクチェック <br> 3:RP口座振替 4:RL口座振替 5:その他口座振替 <br> 6:コンビニ払込票(A4) 7:コンビニ払込票(ハガキ) 9:バーチャル口座         | 1        | 数値                               | (追加時)                           |
| source_bank_account_name   | 振込元口座名義 <br> ※payment_method=0,2,9の場合に必要(入力は任意) <br> ※追加省略時、空で登録される <br> ※エンドユーザーの口座名義になります                                              | 48       | [半角英数\*5](/README.md#種別注釈) |                                    |
| customer_number            | 顧客番号 <br> ※payment_method=5の場合に必要(入力は任意) <br> ※追加省略時、nullで登録される                                                                                               | 20       | [半角英数\*3](/README.md#種別注釈) |                                    |
| bank_code                  | 銀行コード <br> ※payment_method=3,4,5,9の場合に必要(入力は任意) <br> ※追加省略時、空で登録される                                                                                         | 4        | 数値                               |                                    |
| bank_name                  | 銀行名 <br> ※payment_method=5の場合に必要(入力は任意) <br> ※ゆうちょの場合は「ﾕｳﾁﾖ」 <br> ※追加省略時、nullで登録される                                                                  | 15       | [半角英数\*6](/README.md#種別注釈) |                                    |
| branch_code                | 支店コード <br> ※payment_method=3,4,5,9の場合に必要(入力は任意) <br> ※追加省略時、空で登録される <br> ※payment_method=3でゆうちょの時の桁数は5桁                                         | 3または5 | 数値                               |                                    |
| branch_name                | 支店名 <br> ※payment_method=5の場合に必要(入力は任意) <br> ※追加省略時、nullで登録される                                                                                                 | 15       | [半角英数\*6](/README.md#種別注釈) |                                    |
| bank_account_type          | 預金種目 <br> 1:普通 2:当座                                                                                                                                                              | 1        | 数値                               | (追加時でpayment_method=3,4,5,9時) |
| bank_account_number        | 口座番号 <br> ※payment_method=3,4,5,9の場合に必要(入力は任意) <br> ※payment_method=3でゆうちょの時の桁数は8桁 <br> ※追加省略時、空で登録される                                           | 7または8 | 数値                               |                                    |
| bank_account_name          | 口座名義 <br> ※payment_method=3,4,5の場合に必要(入力は任意) <br> ※payment_method=3の場合の桁数は30桁 <br> ※payment_method=3の場合は半角スペース入力不可 <br> ※追加省略時、空で登録される | 30       | [半角英数\*5](/README.md#種別注釈) |                                    |
| credit_card_regist_kind    | クレジットカード登録方法 <br> 1:あとで登録 2:登録用メールを送付 <br> ※payment_method=1の場合に必要(入力は任意) <br> ※追加省略時、1で登録される                                           | 1        | 数値                               |                                    |
| credit_card_email          | クレジットカード登録メール送信先 <br> ※credit_card_regist_kind=2の場合に必要(入力は任意) <br> ※追加省略時、emailで入力したメールアドレスに送信される                                     | 100      | [半角英数\*1](/README.md#種別注釈) |                                    |

</details>

#### billing.sub_account_title

<details>
<summary>show child arguments</summary>

下記のような項目のオブジェクトを持つリスト

| 名前                     | 概要                                                      | 桁数 | 種別                               | 必須 |
| ------------------------ | --------------------------------------------------------- | ---- | ---------------------------------- | ---- |
| account_title_code       | 勘定科目コード                                            | 20   | [半角英数\*4](/README.md#種別注釈) | 必須 |
| journal_cooperation_code | 仕訳連携用補助科目コード <br> ※追加省略時、空で登録される | 25   | 文字列                             |      |

</details>

</details>


## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                                 | 概要                                                                                                                                                                             | 型     |
| ------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| user_id                              | ユーザーID（管理画面へのログインID）                                                                                                                                             | string |
| access_key                           | アクセスキー                                                                                                                                                                     | string |
| billing                              | 請求先に属するパラメータ                                                                                                                                                         |        |
| error_code                           | エラーコード <br> ※正常時はnull                                                                                                                                                  | string |
| error_message                        | エラーメッセージ <br> ※正常時はnull                                                                                                                                              | string |
| code                                 | 請求先コード                                                                                                                                                                     | string |
| name                                 | 請求先名                                                                                                                                                                         | string |
| billing.individual                   | 請求先部署に属するパラメータ                                                                                                                                                     |        |
| error_code                           | エラーコード <br> ※正常時はnull                                                                                                                                                  | string |
| error_message                        | エラーメッセージ <br> ※正常時はnull                                                                                                                                              | string |
| number                               | 請求先部署番号 <br> ※登録時に請求管理ロボ側で発番される番号となります                                                                                                            | int    |
| code                                 | 請求先部署コード                                                                                                                                                                 | string |
| name                                 | 請求先部署名                                                                                                                                                                     | string |
| link_customer_code                   | 会計ソフト連携用取引先コード                                                                                                                                                     | string |
| address1                             | 宛名1                                                                                                                                                                            | string |
| address2                             | 宛名2                                                                                                                                                                            | string |
| address3                             | 宛名3                                                                                                                                                                            | string |
| zip_code                             | 郵便番号                                                                                                                                                                         | string |
| pref                                 | 都道府県                                                                                                                                                                         | string |
| city_address                         | 市区町村番地                                                                                                                                                                     | string |
| building_name                        | 建物名                                                                                                                                                                           | string |
| set_post_address                     | 郵送宛先情報 <br> 0:使用しない 1:使用する                                                                                                                                        | int    |
| post_address1                        | 郵送先宛名1                                                                                                                                                                      | string |
| post_address2                        | 郵送先宛名2                                                                                                                                                                      | string |
| post_address3                        | 郵送先宛名3                                                                                                                                                                      | string |
| post_zip_code                        | 郵送先郵便番号                                                                                                                                                                   | string |
| post_pref                            | 郵送先都道府県                                                                                                                                                                   | string |
| post_city_address                    | 郵送先市区町村番地                                                                                                                                                               | string |
| post_building_name                   | 郵送先建物名                                                                                                                                                                     | string |
| tel                                  | 電話番号                                                                                                                                                                         | string |
| email                                | メールアドレス                                                                                                                                                                   | string |
| cc_email                             | CC送信先メールアドレス                                                                                                                                                           | string |
| memo                                 | メモ                                                                                                                                                                             | string |
| auto_erase_commission_amount         | 手数料自動消込許容金額                                                                                                                                                           | int    |
| billing_method                       | 請求方法                                                                                                                                                                         | int    |
| issue_month                          | 請求書発行日_月                                                                                                                                                                  | int    |
| issue_day                            | 請求書発行日_日                                                                                                                                                                  | int    |
| sending_month                        | 請求書送付日_月                                                                                                                                                                  | int    |
| sending_day                          | 請求書送付日_日                                                                                                                                                                  | int    |
| deadline_month                       | 決済期限_月                                                                                                                                                                      | int    |
| deadline_day                         | 決済期限_日                                                                                                                                                                      | int    |
| payment_method_code                  | 決済情報コード                                                                                                                                                                   | string |
| bs_owner_code                        | 請求元担当者コード                                                                                                                                                               | string |
| ref_billing_code                     | 合計請求書用請求先コード                                                                                                                                                         | string |
| ref_individual_number                | 合計請求書用請求先部署番号                                                                                                                                                       | int    |
| ref_individual_code                  | 合計請求書用請求先部署コード                                                                                                                                                     | string |
| bill_template_code                   | 請求書テンプレートコード                                                                                                                                                         | int    |
| billing.individual.sub_account_title | 請求先部署補助科目に属するパラメータ                                                                                                                                             |        |
| account_receivable_code              | 請求先部署売掛金補助科目コード                                                                                                                                                   | string |
| advances_received_code               | 請求先部署前受金補助科目コード                                                                                                                                                   | string |
| suspense_received_code               | 請求先部署仮受金補助科目コード                                                                                                                                                   | string |
| billing.payment                      | 請求先決済手段に属するパラメータ                                                                                                                                                 |        |
| error_code                           | エラーコード <br> ※正常時はnull                                                                                                                                                  | string |
| error_message                        | エラーメッセージ <br> ※正常時はnull                                                                                                                                              | string |
| number                               | 決済情報番号                                                                                                                                                                     | int    |
| code                                 | 決済情報コード                                                                                                                                                                   | string |
| name                                 | 決済情報名                                                                                                                                                                       | string |
| bank_transfer_pattern_code           | 請求元銀行口座パターンコード                                                                                                                                                     | string |
| payment_method                       | 決済手段 <br> 0:銀行振込 1:クレジットカード 2:バンクチェック <br> 3:RP口座振替 4:RL口座振替 5:その他口座振替 <br> 6:コンビニ払込票(A4) 7:コンビニ払込票(ハガキ) 9:バーチャル口座 | int    |
| register_status                      | 登録ステータス <br> 0:未処理 1:登録待ち 2:メール送信済み 3:申請中 <br> 4:登録情報_送信エラー 5:登録完了 6:登録失敗                                                               | int    |
| source_bank_account_name             | 振込元口座名義 <br> ※payment_method=0,2,9以外は空文字                                                                                                                            | string |
| customer_number                      | 顧客番号 <br> ※payment_method=3,5以外は空文字                                                                                                                                    | string |
| bank_code                            | 銀行コード <br> ※payment_method=3,4,5,9以外は空文字                                                                                                                              | string |
| bank_name                            | 銀行名 <br> ※payment_method=5,9以外は空文字                                                                                                                                      | string |
| branch_code                          | 支店コード <br> ※payment_method=3,4,5,9以外は空文字                                                                                                                              | string |
| branch_name                          | 支店名 <br> ※payment_method=5,9以外は空文字                                                                                                                                      | string |
| bank_account_type                    | 預金種目 <br> 1:普通 2:当座 <br> ※payment_method=3,4,5,9以外は空文字                                                                                                             | string |
| bank_account_number                  | 口座番号 <br> ※payment_method=3,4,5,9以外は空文字                                                                                                                                | string |
| bank_account_name                    | 口座名義 <br> ※payment_method=3,4,5,9以外は空文字                                                                                                                                | string |
| cod                                  | 店舗オーダー番号 <br> ※payment_method=1以外は空文字                                                                                                                              | String |
| bank_check_bank_code                 | バンクチェック銀行コード <br> ※payment_method=2以外は空文字                                                                                                                      | string |
| bank_check_bank_name                 | バンクチェック銀行名 <br> ※payment_method=2以外は空文字                                                                                                                          | string |
| bank_check_branch_code               | バンクチェック支店コード <br> ※payment_method=2以外は空文字                                                                                                                      | string |
| bank_check_branch_name               | バンクチェック支店名 <br> ※payment_method=2以外は空文字                                                                                                                          | string |
| bank_check_kind                      | バンクチェック口座種別 <br> 1:普通 2:当座 <br> ※payment_method=2以外は空文字                                                                                                     | string |
| bank_check_bank_account_number       | バンクチェック口座番号 <br> ※payment_method=2以外は空文字                                                                                                                        | string |
| credit_card_regist_kind              | クレジットカード登録方法                                                                                                                                                         | int    |
| credit_card_email                    | クレジットカード登録メール送信先                                                                                                                                                 | string |
| billing.sub_account_title            | 請求先補助科目に属するパラメータ                                                                                                                                                 |        |
| error_code                           | エラーコード <br> ※正常時はnull                                                                                                                                                  | string |
| error_message                        | エラーメッセージ <br> ※正常時はnull                                                                                                                                              | string |
| account_title_code                   | 勘定科目コード                                                                                                                                                                   | string |
| journal_cooperation_code             | 仕訳連携用補助科目コード                                                                                                                                                         | string |


## 使用例

### リクエスト

```
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "billing": [
        {
            "code": "billing_code",
            "name": "請求先名",
            "individual": [
                {
                    "code": "abc-12345",
                    "name": "請求先部署名",
                    "address1": "宛名1",
                    "zip_code": "1651111",
                    "pref": "東京都",
                    "city_address": "渋谷区",
                    "email": "email@robotpayment.co.jp",
                    "sub_account_title": [
                        {
                            "account_receivable_code": null,
                            "advances_received_code": null,
                            "suspense_received_code": null
                        }
                    ]
                }
            ],
            "payment": [
                {
                    "number": 123,
                    "code": "bank-123",
                    "name": "銀行振込123",
                    "bank_transfer_pattern_code": "bank-123",
                    "payment_method": 0
                },
                {
                    "number": 456,
                    "code": "bank-456",
                    "name": "銀行振込456",
                    "bank_transfer_pattern_code": "bank-456",
                    "payment_method": 0
                }
            ],
            "sub_account_title": [
                {
                    "account_title_code": "abc1234",
                    "journal_cooperation_code": "abc1234"
                },
                {
                    "account_title_code": "abc5678",
                    "journal_cooperation_code": "abc5678"
                }
            ]
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
    "billing": [
        {
            "error_code": null,
            "error_message": null,
            "code": "billing",
            "name": "請求先名",
            "individual": [
                {
                    "error_code": null,
                    "error_message": null,
                    "number": 1,
                    "code": "abc-12345",
                    "name": "請求先部署名",
                    "link_customer_code": "",
                    "address1": "宛名1",
                    "address2": "",
                    "address3": "",
                    "zip_code": "1651111",
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
                    "auto_erase_commission_amount": 0,
                    "billing_method": null,
                    "issue_month": null,
                    "issue_day": null,
                    "sending_month": null,
                    "sending_day": null,
                    "deadline_month": null,
                    "deadline_day": null,
                    "payment_method_code": "",
                    "bs_owner_code": "",
                    "ref_billing_code": "",
                    "ref_individual_number": null,
                    "ref_individual_code": "",
                    "bill_template_code": null,
                    "sub_account_title": [
                        {
                            "account_receivable_code": null,
                            "advances_received_code": null,
                            "suspense_received_code": null
                        }
                    ]
                }
            ],
            "payment": [
                {
                    "error_code": null,
                    "error_message": null,
                    "number": 123,
                    "code": "bank-123",
                    "name": "銀行振込123",
                    "bank_transfer_pattern_code": "bank-123",
                    "payment_method": 0,
                    "register_status": 5,
                    "source_bank_account_name": "",
                    "customer_number": "",
                    "bank_code": "",
                    "bank_name": "",
                    "branch_code": "",
                    "branch_name": "",
                    "bank_account_type": "",
                    "bank_account_number": "",
                    "bank_account_name": "",
                    "payment_type ": "",
                    "cod": "",
                    "credit_card_regist_kind": ""
                },
                {
                    "error_code": null,
                    "error_message": null,
                    "number": 456,
                    "code": "bank-456",
                    "name": "銀行振込456",
                    "bank_transfer_pattern_code": "bank-456",
                    "register_status": 5,
                    "payment_method": 0,
                    "source_bank_account_name": "",
                    "customer_number": "",
                    "bank_code": "",
                    "bank_name": "",
                    "branch_code": "",
                    "branch_name": "",
                    "bank_account_type": "",
                    "bank_account_number": "",
                    "bank_account_name": "",
                    "payment_type ": "",
                    "cod": "",
                    "bank_check_bank_code": "",
                    "bank_check_bank_name": "",
                    "bank_check_branch_code": "",
                    "bank_check_branch_name": "",
                    "bank_check_kind": "",
                    "bank_check_bank_account_number": "",
                    "credit_card_regist_kind": ""
                }
            ],
            "sub_account_title": [
                {
                    "error_code": null,
                    "error_message": null,
                    "account_title_code": "abc1234",
                    "journal_cooperation_code": "abc1234"
                },
                {
                    "error_code": null,
                    "error_message": null,
                    "account_title_code": "abc5678",
                    "journal_cooperation_code": "abc5678"
                }
            ]
        }
    ]
}
```

## エラー

[共通エラー](/README.md#共通エラー)

個別エラー

| エラーコード | 内容                                                                           |
| ------------ | ------------------------------------------------------------------------------ |
| 801          | リクエストパラメータに請求先が存在しない                                       |
| 802          | 請求先コードが不正                                                             |
| 803          | 請求先名が不正                                                                 |
| 804          | 登録ユーザーIDが不正                                                           |
| 805          | 請求先部署番号が不正                                                           |
| 806          | 請求先部署名が不正                                                             |
| 807          | 会計ソフト連携用取引先コードが不正                                             |
| 808          | 宛名１が不正                                                                   |
| 809          | 宛名２が不正                                                                   |
| 810          | 宛名３が不正                                                                   |
| 811          | 郵便番号が不正                                                                 |
| 812          | 都道府県名が不正                                                               |
| 813          | 市区町村番地が不正                                                             |
| 814          | 建物名が不正                                                                   |
| 815          | 電話番号が不正                                                                 |
| 816          | メールアドレスが不正                                                           |
| 817          | 決済手段が不正                                                                 |
| 818          | 振込元口座名義が不正                                                           |
| 819          | 銀行コードが不正                                                               |
| 820          | 支店コード                                                                     |
| 821          | 預金種目が不正                                                                 |
| 822          | 口座番号が不正                                                                 |
| 823          | 口座名義が不正                                                                 |
| 824          | CC送信先メールアドレスが不正                                                   |
| 825          | 郵送宛先情報が不正                                                             |
| 826          | 郵送先宛名１が不正                                                             |
| 827          | 郵送先宛先２が不正                                                             |
| 828          | 郵送先宛名３が不正                                                             |
| 829          | 郵送先郵便番号が不正                                                           |
| 830          | 郵送先都道府県名が不正                                                         |
| 831          | 郵送先市区町村番地が不正                                                       |
| 832          | 郵送先建物名が不正                                                             |
| 833          | 登録ユーザーIDが不正                                                           |
| 834          | 請求先登録に失敗                                                               |
| 835          | 請求先部署登録に失敗                                                           |
| 836          | 請求先に紐づく請求先部署が存在しない                                           |
| 837          | 請求先に紐づく請求先部署の登録に失敗                                           |
| 838          | 顧客番号が不正                                                                 |
| 839          | 銀行名が不正                                                                   |
| 840          | 支店名が不正                                                                   |
| 841          | 請求先部署コードが不正                                                         |
| 842          | 指定した合計請求書用請求先コードが不正                                         |
| 843          | 指定した合計請求書用請求先部署番号が不正                                       |
| 844          | 指定した合計請求書用請求先部署コードが不正                                     |
| 845          | 指定した合計請求書用テンプレートコードが不正                                   |
| 846          | 指定した合計請求書用請求先部署が存在しない                                     |
| 847          | 指定した合計請求書用請求先部署が、既に合計請求書用請求先部署を持っている       |
| 848          | 指定した合計請求書用請求先部署の登録ステータスが不正                           |
| 850          | 請求先部署が、既に他の合計請求書用請求先部署として登録されている               |
| 851          | 合計請求書用請求先部署番号と、合計請求書用請求先部署コードは同時に指定できない |
| 852          | 指定した合計請求書用請求先部署が承認中                                         |
| 853          | 指定した合計請求書用テンプレートコードが不正                                   |
| 854          | 指定した合計請求書用請求先部署の決済手段が不正                                 |
| 855          | 請求先決済情報番号が不正                                                       |
| 856          | 請求先決済情報コードが不正                                                     |
| 857          | 請求先決済情報名が不正                                                         |
| 858          | 決済情報番号と決済情報コードは同時に指定できない                               |
| 859          | 請求先部署売掛金補助科目コードが不正                                           |
| 860          | 請求先部署前受金補助科目コードが不正                                           |
| 861          | 請求先部署仮受金補助科目コードが不正                                           |
| 862          | 合計請求書の設定で登録済みの請求情報があります                                 |