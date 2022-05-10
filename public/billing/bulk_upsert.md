# 請求先登録更新

`/api/v1.0/billing/bulk_upsert`

複数の請求先、請求先部署、請求先決済手段、請求先補助科目を登録または更新します。
停止中の請求先、請求先部署、決済情報を指定して送信すると、有効な状態に復元します。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Path: `/api/v1.0/billing/bulk_upsert`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                        | 概要                                 | 桁数 | 種別                              | 必須 |
| --------------------------- | ------------------------------------ | ---- | --------------------------------- | ---- |
| user_id                     | ユーザーID（管理画面へのログインID） | 100  | [メール形式](../../index.md#種別) | 必須 |
| access_key                  | アクセスキー                         | 100  | [半角英数](../../index.md#種別)   | 必須 |
| [billing](#billing-request) | 請求先に属するパラメータ             |      | `array`                           |      |

#### billing (request)

下記のような項目のオブジェクトを持つリスト

| 名前                                                         | 概要                                                                            | 桁数 | 種別                                   | 必須     |
| ------------------------------------------------------------ | ------------------------------------------------------------------------------- | ---- | -------------------------------------- | -------- |
| code                                                         | 請求先コード  <br> ※両端のスペース除去 <br> ※登録後は変更できません             | 20   | [半角英数 + 記号](../../index.md#種別) | 必須     |
| name                                                         | 請求先名  <br> ※両端のスペース除去                                              | 100  | 文字列                                 | (追加時) |
| [individual](#individual-request)                            | 請求先部署に属するパラメータ                                                    |      | `array`                                |          |
| [payment](#payment-request)                                  | 請求先決済手段に属するパラメータ ※省略可能                                      |      | `array`                                |          |
| [billing.sub_account_title](#billingsubaccounttitle-request) | 請求先補助科目に属するパラメータ <br> ※今後拡張予定です。現在利用はできません。 |      | `array`                                |          |

#### individual (request)

下記のような項目のオブジェクトを持つリスト

| 名前                                                                              | 概要                                                                                                                                                                                                                                                 | 桁数 | 種別                                   | 必須                                 |
| --------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---- | -------------------------------------- | ------------------------------------ |
| number                                                                            | 請求先部署番号 <br> ※両端のスペース除去 <br> ※登録後は変更できません                                                                                                                                                                                    | 18   | 数値                                   | (更新時)^1                           |
| code                                                                              | 請求先部署コード <br> ※両端のスペース除去 <br> ※登録後は変更できません                                                                                                                                                                                  | 20   | [半角英数 + 記号](../../index.md#種別) | (更新時)^1                           |
| name                                                                              | 請求先部署名  <br> ※両端のスペース除去                                                                                                                                                                                                                  | 100  | 文字列                                 | (追加時)                             |
| link_customer_code                                                                | 会計ソフト連携用取引先コード <br> ※追加省略時、nullで登録される                                                                                                                                                                                         | 20   | [半角英数](../../index.md#種別)        |                                      |
| address1                                                                          | 宛名1  <br> ※両端のスペース除去                                                                                                                                                                                                                         | 60   | 文字列                                 | (追加時)                             |
| address2                                                                          | 宛名2 <br> ※両端のスペース除去 <br> ※追加省略時、nullで登録される                                                                                                                                                                                       | 60   | 文字列                                 |                                      |
| address3                                                                          | 宛名3 <br> ※両端のスペース除去 <br> ※追加省略時、nullで登録される                                                                                                                                                                                       | 60   | 文字列                                 |                                      |
| zip_code                                                                          | 郵便番号  <br> ※両端のスペース除去 <br> ※ハイフンを除去した時に7桁の数字であれば可                                                                                                                                                                      | 7~   | 文字列                                 | (追加時)                             |
| pref                                                                              | 都道府県名 or 「その他」                                                                                                                                                                                                                                | 3~4  | 文字列                                 | (追加時)                             |
| city_address                                                                      | 市区町村番地  <br> ※両端のスペース除去                                                                                                                                                                                                                  | 56   | 文字列                                 | (追加時)                             |
| building_name                                                                     | 建物名 <br> ※両端のスペース除去 <br> ※追加省略時、nullで登録される                                                                                                                                                                                      | 60   | 文字列                                 |                                      |
| set_post_address                                                                  | 郵送宛先情報 <br> 0:使用しない <br> 1:使用する <br> ※省略時は、0と同義                                                                                                                                                                                       | 1    | 数値                                   |                                      |
| post_address1                                                                     | 郵送先宛名1  <br> ※両端のスペース除去 <br> ※追加省略時、nullで登録される                                                                                                                                                                                | 60   | 文字列                                 | (追加時でset_post_address=1時)       |
| post_address2                                                                     | 郵送先宛名2 <br> ※両端のスペース除去 <br> ※追加省略時、nullで登録される                                                                                                                                                                                 | 60   | 文字列                                 |                                      |
| post_address3                                                                     | 郵送先宛名3 <br> ※両端のスペース除去 <br> ※追加省略時、nullで登録される                                                                                                                                                                                 | 60   | 文字列                                 |                                      |
| post_zip_code                                                                     | 郵送先郵便番号  <br> ※両端のスペース除去 <br> ※ハイフンありなしどちらでも可                                                                                                                                                                             | 7    | 文字列                                 | (追加時でset_post_address=1時)       |
| post_pref                                                                         | 郵送先都道府県  <br> ※追加省略時、nullで登録される                                                                                                                                                                                                      | 4    | 文字列                                 | (追加時でset_post_address=1時)       |
| post_city_address                                                                 | 郵送先市区町村番地  <br> ※両端のスペース除去 <br> ※追加省略時、nullで登録される                                                                                                                                                                         | 56   | 文字列                                 | (追加時でset_post_address=1時)       |
| post_building_name                                                                | 郵送先建物名 <br> ※両端のスペース除去 <br> ※追加省略時                                                                                                                                                                                                  | 60   | 文字列                                 |                                      |
| tel                                                                               | 電話番号  <br> ※両端のスペース除去 <br> ※追加省略時、nullで登録される                                                                                                                                                                                   | 15   | 数値、半角ハイフン                     | (追加時でpayment_method=1,2,6,7,8時) |
| email                                                                             | メールアドレス  <br> ※両端のスペース除去                                                                                                                                                                                                                | 100  | [メール形式](../../index.md#種別)      | (追加時)                             |
| cc_email                                                                          | CC送信先メールアドレス <br> ※両端のスペース除去 <br> ※追加省略時、nullで登録される <br> ,(カンマ)区切りで複数指定可                                                                                                                                     | 256  | [メール形式](../../index.md#種別)      |                                      |
| memo                                                                              | メモ <br> ※追加省略時、nullで登録される                                                                                                                                                                                                                 | 300  | 文字列                                 |                                      |
| billing_method                                                                    | 請求方法 <br> 0:送付なし <br> 1:自動メール <br> 2:手動メール <br> 3:自動郵送 <br> 4:手動郵送 <br> 5:自動メール+自動郵送 <br> 6:手動メール+手動郵送 <br> ※追加省略時、nullで登録される <br> ※請求情報を登録する際に利用する値です。事前に登録しておくことができます。 | 1    | 数値                                   |                                      |
| issue_month                                                                       | 請求書発行日_月 <br> 入力可能値:-60～60 <br> 例） <br> -1:前月 <br> 0:当月 <br> 60:60ヵ月後 <br> ※追加省略時、nullで登録される <br> ※請求情報を登録する際に利用する値です。事前に登録しておくことができます。                                                  | 2    | 数値                                   |                                      |
| issue_day                                                                         | 請求書発行日_日 <br> 入力可能値:1～30、99(末日) <br> ※追加省略時、nullで登録される <br> ※請求情報を登録する際に利用する値です。事前に登録しておくことができます。                                                                                                 | 2    | 数値                                   |                                      |
| sending_month                                                                     | 請求書送付日_月 <br> 入力可能値:-60～60 <br> 例） <br> -1:前月 <br> 0:当月 <br> 60:60ヵ月後 <br> ※追加省略時、nullで登録される <br> ※請求情報を登録する際に利用する値です。事前に登録しておくことができます。                                                  | 2    | 数値                                   |                                      |
| sending_day                                                                       | 請求書送付日_日 <br> 入力可能値:1～30、99(末日) <br> ※追加省略時、nullで登録される <br> ※請求情報を登録する際に利用する値です。事前に登録しておくことができます。                                                                                                 | 2    | 数値                                   |                                      |
| deadline_month                                                                    | 決済期限_月 <br> 入力可能値:-60～60 <br> 例） <br> -1:前月 <br> 0:当月 <br> 60:60ヵ月後 <br> ※追加省略時、nullで登録される <br> ※請求情報を登録する際に利用する値です。事前に登録しておくことができます。                                                      | 2    | 数値                                   |                                      |
| deadline_day                                                                      | 決済期限_日 <br> 入力可能値:1～30、99(末日) <br> ※追加省略時、nullで登録される <br> ※請求情報を登録する際に利用する値です。事前に登録しておくことができます。                                                                                                     | 2    | 数値                                   |                                      |
| payment_method_code                                                               | 決済情報コード <br> ※追加省略時、nullで登録される                                                                                                                                                                                                           | 20   | [半角英数 + 記号](../../index.md#種別) |                                      |
| bs_owner_code                                                                     | 請求元担当者コード <br> ※追加省略時、nullで登録される                                                                                                                                                                                                       | 20   | [半角英数 + 記号](../../index.md#種別) |                                      |
| ref_billing_code                                                                  | 合計請求書用請求先コード <br> ※追加省略時、nullで登録される                                                                                                                                                                                                 | 20   | [半角英数 + 記号](../../index.md#種別) |                                      |
| ref_individual_number                                                             | 合計請求書用請求先部署番号 <br> ※追加省略時、nullで登録される                                                                                                                                                                                               | 18   | 数値                                   | (合計請求書時)^2                     |
| ref_individual_code                                                               | 合計請求書用請求先部署コード <br> ※追加省略時、nullで登録される                                                                                                                                                                                             | 20   | [半角英数 + 記号](../../index.md#種別) | (合計請求書時)^2                     |
| bill_template_code                                                                | 請求書テンプレートコード <br> ※合計請求書をご利用される場合は、お手数ですがお問い合わせください <br> ※追加省略時、nullで登録される                                                                                                                                | 18   | 数値                                   |                                      |
| auto_erase_commission_amount                                                      | 手数料自動消込許容金額 <br> ※追加省略時、0で登録される                                                                                                                                                                                                       | 9    | 数値                                   |                                      |
| [billing.individual.sub_account_title](#billingindividualsubaccounttitle-request) | 請求先部署補助科目に属するパラメータ ※省略可能                                                                                                                                                                                                               |      | `object`                                |                                      |

#### billing.individual.sub_account_title (request)

下記のような項目を持つオブジェクト

| 名前                    | 概要                                                              | 桁数 | 種別   | 必須 |
| ----------------------- | ----------------------------------------------------------------- | ---- | ------ | ---- |
| account_receivable_code | 請求先部署売掛金補助科目コード <br> ※追加省略時、nullで登録される | 25   | 文字列 |      |
| advances_received_code  | 請求先部署前受金補助科目コード <br> ※追加省略時、nullで登録される | 25   | 文字列 |      |
| suspense_received_code  | 請求先部署仮受金補助科目コード <br> ※追加省略時、nullで登録される | 25   | 文字列 |      |


#### payment (request)

下記のような項目のオブジェクトを持つリスト

| 名前                       | 概要                                                                                                                                                                                                                                    | 桁数     | 種別                                   | 必須                               |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | -------------------------------------- | ---------------------------------- |
| number                     | 決済情報番号  <br> ※両端のスペース除去 <br> ※登録後は変更できません                                                                                                                                                                     | 18       | 半角数字                               | (更新時)^3                         |
| code                       | 決済情報コード <br> ※両端のスペース除去 <br> ※登録後は変更できません                                                                                                                                                                    | 20       | [半角英数 + 記号](../../index.md#種別) | (更新時)^3                         |
| name                       | 決済情報名                                                                                                                                                                                                                              | 100      | 文字列                                 | (追加時)                           |
| bank_transfer_pattern_code | 請求元銀行口座パターンコード <br> ※payment_method=0の場合に必要(入力は任意) <br> ※追加省略時、1で登録される                                                                                                                             | 20       | [半角英数 + 記号](../../index.md#種別) |                                    |
| payment_method             | [決済手段](../../index.md#決済手段)                                                                                                                                                                                                     | 2        | 数値                                   | (追加時)                           |
| source_bank_account_name   | 振込元口座名義 <br> ※payment_method=0,2,9の場合に必要(入力は任意) <br> ※追加省略時、空で登録される <br> ※エンドユーザーの口座名義になります                                                                                             | 48       | [口座名義](../../index.md#種別)        |                                    |
| customer_number            | 顧客番号 <br> ※payment_method=5の場合に必要(入力は任意) <br> ※追加省略時、nullで登録される <br> ※payment_method=3の場合入力の有無に関わらず顧客番号が自動発番され、入力指定しての登録/更新はできません                                  | 20       | [半角英数](../../index.md#種別)        |                                    |
| bank_code                  | 銀行コード <br> ※payment_method=3,4,5,9の場合に必要(入力は任意) <br> ※追加省略時、空で登録される <br> ※payment_method=9の場合、省略時は自動でバーチャル口座の銀行コードが割り当てられる                                                 | 4        | 数値                                   |                                    |
| bank_name                  | 銀行名 <br> ※payment_method=5の場合に必要(入力は任意) <br> ※ゆうちょの場合は「ﾕｳﾁﾖ」 <br> ※追加省略時、nullで登録される <br> ※payment_method=9の場合、省略時は自動でバーチャル口座の銀行名が割り当てられる                              | 15       | [銀行名等](../../index.md#種別)        |                                    |
| branch_code                | 支店コード <br> ※payment_method=3,4,5,9の場合に必要(入力は任意) <br> ※追加省略時、空で登録される <br> ※payment_method=3でゆうちょの時の桁数は5桁 <br> ※payment_method=9の場合、省略時は自動でバーチャル口座の支店コードが割り当てられる | 3または5 | 数値                                   |                                    |
| branch_name                | 支店名 <br> ※payment_method=5の場合に必要(入力は任意) <br> ※追加省略時、nullで登録される <br> ※payment_method=9の場合、省略時は自動でバーチャル口座の支店名が割り当てられる                                                             | 15       | [銀行名等](../../index.md#種別)        |                                    |
| bank_account_type          | 預金種目 <br> 1:普通 <br> 2:当座 <br> ※payment_method=9の場合、省略時は自動でバーチャル口座の預金種目が割り当てられる                                                                                                                        | 1        | 数値                                   | (追加時でpayment_method=3,4,5,9時) |
| bank_account_number        | 口座番号 <br> ※payment_method=3,4,5,9の場合に必要(入力は任意) <br> ※payment_method=3でゆうちょの時の桁数は8桁 <br> ※追加省略時、空で登録される <br> ※payment_method=9の場合、省略時は自動でバーチャル口座の口座番号が割り当てられる     | 7または8 | 数値                                   |                                    |
| bank_account_name          | 口座名義 <br> ※payment_method=3,4,5の場合に必要(入力は任意) <br> ※payment_method=3の場合の桁数は30桁 <br> ※追加省略時、空で登録される<br> ※payment_method=9の場合、省略時は自動でバーチャル口座の口座名義が割り当てられる               | 30       | [口座名義](../../index.md#種別)        |                                    |
| credit_card_regist_kind    | クレジットカード登録方法 <br> 1:あとで登録 <br> 2:登録用メールを送付 <br> ※payment_method=1の場合に必要(入力は任意) <br> ※追加省略時、1で登録される                                                                                          | 1        | 数値                                   |                                    |
| credit_card_email          | クレジットカード登録メール送信先 <br> ※credit_card_regist_kind=2の場合に必要(入力は任意) <br> ※追加省略時、emailで入力したメールアドレスに送信される                                                                                    | 100      | [メール形式](../../index.md#種別)      |                                    |
| clearing_key               | 消込キー <br> ※payment_method=その他の場合に必要（入力は任意）<br> ※追加省略時 `""` (空文字)で登録される                                                                                                                                | 33       | 文字列                                 |                                    |


#### billing.sub_account_title (request)

下記のような項目のオブジェクトを持つリスト

| 名前                     | 概要                                                      | 桁数 | 種別                                   | 必須 |
| ------------------------ | --------------------------------------------------------- | ---- | -------------------------------------- | ---- |
| account_title_code       | 勘定科目コード                                            | 20   | [半角英数 + 記号](../../index.md#種別) | 必須 |
| journal_cooperation_code | 仕訳連携用補助科目コード <br> ※追加省略時、空で登録される | 25   | 文字列                                 |      |



## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                | 概要                                 | 型             |
| ------------------- | ------------------------------------ | -------------- |
| user_id             | ユーザーID（管理画面へのログインID） | string         |
| access_key          | アクセスキー                         | string         |
| [billing](#billing) | 請求先に属するパラメータ             | Array(billing) |

#### billing (response)

下記のような項目のオブジェクトを持つリスト

| 名前                                                          | 概要                                | 型      |
| ------------------------------------------------------------- | ----------------------------------- | ------- |
| error_code                                                    | エラーコード <br> ※正常時はnull     | int     |
| error_message                                                 | エラーメッセージ <br> ※正常時はnull | string  |
| code                                                          | 請求先コード                        | string  |
| name                                                          | 請求先名                            | string  |
| [individual](#individual-response)                            | 請求先部署に属するパラメータ        | `array` |
| [payment](#payment-response)                                  | 請求先決済手段に属するパラメータ    | `array` |
| [billing.sub_account_title](#billingsubaccounttitle-response) | 請求先補助科目に属するパラメータ    | `array` |

#### individual (response)

下記のような項目のオブジェクトを持つリスト

| 名前                                 | 概要                                                                  | 型     |
| ------------------------------------ | --------------------------------------------------------------------- | ------ |
| error_code                           | エラーコード <br> ※正常時はnull                                       | int    |
| error_message                        | エラーメッセージ <br> ※正常時はnull                                   | string |
| number                               | 請求先部署番号 <br> ※登録時に請求管理ロボ側で発番される番号となります | int |
| code                                 | 請求先部署コード                                                      | string |
| name                                 | 請求先部署名                                                          | string |
| link_customer_code                   | 会計ソフト連携用取引先コード                                          | string |
| address1                             | 宛名1                                                                 | string |
| address2                             | 宛名2                                                                 | string |
| address3                             | 宛名3                                                                 | string |
| zip_code                             | 郵便番号                                                              | string |
| pref                                 | 都道府県                                                              | string |
| city_address                         | 市区町村番地                                                          | string |
| building_name                        | 建物名                                                                | string |
| set_post_address                     | 郵送宛先情報 <br> 0:使用しない <br> 1:使用する                          | int    |
| post_address1                        | 郵送先宛名1                                                           | string |
| post_address2                        | 郵送先宛名2                                                           | string |
| post_address3                        | 郵送先宛名3                                                           | string |
| post_zip_code                        | 郵送先郵便番号                                                        | string |
| post_pref                            | 郵送先都道府県                                                        | string |
| post_city_address                    | 郵送先市区町村番地                                                    | string |
| post_building_name                   | 郵送先建物名                                                          | string |
| tel                                  | 電話番号                                                              | string |
| email                                | メールアドレス                                                        | string |
| cc_email                             | CC送信先メールアドレス                                                | string |
| memo                                 | メモ                                                                  | string |
| auto_erase_commission_amount         | 手数料自動消込許容金額                                                | int    |
| billing_method                       | 請求方法                                                              | int |
| issue_month                          | 請求書発行日_月                                                       | int |
| issue_day                            | 請求書発行日_日                                                       | int |
| sending_month                        | 請求書送付日_月                                                       | int |
| sending_day                          | 請求書送付日_日                                                       | rint |
| deadline_month                       | 決済期限_月                                                           | int |
| deadline_day                         | 決済期限_日                                                           | int |
| payment_method_code                  | 決済情報コード                                                        | string |
| bs_owner_code                        | 請求元担当者コード                                                    | string |
| ref_billing_code                     | 合計請求書用請求先コード                                              | string |
| ref_individual_number                | 合計請求書用請求先部署番号                                            | int |
| ref_individual_code                  | 合計請求書用請求先部署コード                                          | string |
| bill_template_code                   | 請求書テンプレートコード                                              | int |
| billing.individual.sub_account_title | 請求先部署補助科目に属するパラメータ                                  | object  |

#### billing.individual.sub_account_title (response)

下記のような項目を持つオブジェクト

| 名前                    | 概要                           | 型     |
| ----------------------- | ------------------------------ | ------ |
| account_receivable_code | 請求先部署売掛金補助科目コード | string |
| advances_received_code  | 請求先部署前受金補助科目コード | string |
| suspense_received_code  | 請求先部署仮受金補助科目コード | string |


#### payment (response)

下記のような項目のオブジェクトを持つリスト

| 名前                           | 概要                                                                                                                                  | 型     |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| error_code                     | エラーコード <br> ※正常時はnull                                                                                                        | int    |
| error_message                  | エラーメッセージ <br> ※正常時はnull                                                                                                    | string |
| number                         | 決済情報番号                                                                                                                           | int |
| code                           | 決済情報コード                                                                                                                         | string |
| name                           | 決済情報名                                                                                                                             | string |
| bank_transfer_pattern_code     | 請求元銀行口座パターンコード                                                                                                            | string |
| payment_method                 | [決済手段](../../index.md#決済手段)                                                                                                    | int    |
| register_status                | 登録ステータス <br> 0:未処理 <br> 1:登録待ち <br> 2:メール送信済み <br> 3:申請中 <br> 4:登録情報_送信エラー <br> 5:登録完了 <br> 6:登録失敗  | int    |
| source_bank_account_name       | 振込元口座名義 <br> ※payment_method=0,2,9以外はnull                                                                                  | string |
| customer_number                | 顧客番号 <br> ※payment_method=3,5以外はnull                                                                                          | string |
| bank_code                      | 銀行コード <br> ※payment_method=3,4,5,9以外はnull                                                                                    | string |
| bank_name                      | 銀行名 <br> ※payment_method=5,9以外はnull                                                                                            | string |
| branch_code                    | 支店コード <br> ※payment_method=3,4,5,9以外はnull                                                                                    | string |
| branch_name                    | 支店名 <br> ※payment_method=5,9以外はnull                                                                                            | string |
| bank_account_type              | 預金種目 <br> 1:普通 <br> 2:当座 <br> ※payment_method=3,4,5,9以外はnull                                                                   | string |
| bank_account_number            | 口座番号 <br> ※payment_method=3,4,5,9以外はnull                                                                                      | string |
| bank_account_name              | 口座名義 <br> ※payment_method=3,4,5,9以外はnull                                                                                      | string |
| cod                            | 店舗オーダー番号 <br> ※payment_method=1以外はnull                                                                                    | string |
| bank_check_bank_code           | バンクチェック銀行コード <br> ※payment_method=2以外はnull                                                                            | string |
| bank_check_bank_name           | バンクチェック銀行名 <br> ※payment_method=2以外はnull                                                                                | string |
| bank_check_branch_code         | バンクチェック支店コード <br> ※payment_method=2以外はnull                                                                            | string |
| bank_check_branch_name         | バンクチェック支店名 <br> ※payment_method=2以外はnull                                                                                | string |
| bank_check_kind                | バンクチェック口座種別 <br> 1:普通 <br> 2:当座 <br> ※payment_method=2以外はnull                                                            | string |
| bank_check_bank_account_number | バンクチェック口座番号 <br> ※payment_method=2以外はnull                                                                              | string |
| credit_card_regist_kind        | クレジットカード登録方法                                                                                                               | int    |
| credit_card_email              | クレジットカード登録メール送信先                                                                                                        | string |
| clearing_key                   | 消込キー                                                                                                                              | string |


### billing.sub_account_title (response)

下記のような項目のオブジェクトを持つリスト

| 名前                     | 概要                                | 型     |
| ------------------------ | ----------------------------------- | ------ |
| error_code               | エラーコード <br> ※正常時はnull     | int    |
| error_message            | エラーメッセージ <br> ※正常時はnull | string |
| account_title_code       | 勘定科目コード                      | string |
| journal_cooperation_code | 仕訳連携用補助科目コード            | string |



## 使用例

### リクエスト例

```json
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
                    "sub_account_title": {
                        "account_receivable_code": null,
                        "advances_received_code": null,
                        "suspense_received_code": null
                    }
                }
            ],
            "payment": [
                {
                    "number": 123,
                    "code": null,
                    "name": "銀行振込123",
                    "bank_transfer_pattern_code": "bank-123",
                    "payment_method": 0
                },
                {
                    "number": null,
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

### レスポンス例

Status: 200 OK

```json
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
                    "sub_account_title": {
                        "account_receivable_code": null,
                        "advances_received_code": null,
                        "suspense_received_code": null
                    }
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

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                                                                                                                                    |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| 1101         | リクエストパラメータに請求先が存在しない                                                                                                  |
| 1102         | 請求先コードが不正                                                                                                                      |
| 1103         | 請求先名が不正 <br> 新規登録時に空白、または101桁以上で発生                                                                               |
| 1104         | 請求先部署番号が不正                                                                                                                    |
| 1105         | 請求先部署コードが不正                                                                                                                  |
| 1106         | 請求先部署名が不正 <br> 新規登録時に空白、または101桁以上で発生                                                                           |
| 1107         | 会計ソフト連携用取引先コードが不正                                                                                                       |
| 1108         | 宛名１が不正 <br> 新規登録時に空白、または61桁以上で発生                                                                                  |
| 1109         | 宛名２が不正                                                                                                                           |
| 1110         | 宛名３が不正                                                                                                                           |
| 1111         | 郵便番号が不正 <br> 新規登録時に空白、またはハイフン(-)を除いた文字が7桁の数字でなければ発生                                                |
| 1112         | 都道府県名が不正 <br> 新規登録時に空白、または都道府県名か「その他」以外の値で発生                                                          |
| 1113         | 市区町村番地が不正 <br> 57桁以上で発生                                                                                                   |
| 1114         | 建物名が不正                                                                                                                           |
| 1115         | 郵送宛先情報が不正                                                                                                                     |
| 1116         | 郵送先宛名１が不正                                                                                                                     |
| 1117         | 郵送先宛先２が不正                                                                                                                     |
| 1118         | 郵送先宛名３が不正                                                                                                                     |
| 1119         | 郵送先郵便番号が不正                                                                                                                   |
| 1120         | 郵送先都道府県名が不正                                                                                                                 |
| 1121         | 郵送先市区町村番地が不正                                                                                                               |
| 1122         | 郵送先建物名が不正                                                                                                                     |
| 1123         | 電話番号が不正                                                                                                                         |
| 1124         | メールアドレスが不正 <br> 新規登録時に空白、101桁以上またはメール形式以外の値で発生 <br> メールアドレスは,(カンマ)区切りで複数指定不可 |
| 1125         | CC送信先メールアドレスが不正                                                                                                           |
| 1126         | メモが不正                                                                                                                             |
| 1127         | 請求方法が不正                                                                                                                         |
| 1128         | 請求書発行日_月が不正                                                                                                                  |
| 1129         | 請求書発行日_日が不正                                                                                                                  |
| 1130         | 請求書送付日_月が不正                                                                                                                  |
| 1131         | 請求書送付日_日が不正                                                                                                                  |
| 1132         | 決済期限_月が不正                                                                                                                      |
| 1133         | 決済期限_日が不正                                                                                                                      |
| 1134         | 請求先決済情報コードが不正                                                                                                             |
| 1135         | 請求元担当者コードが不正                                                                                                               |
| 1136         | 合計請求書用請求先コードが不正                                                                                                         |
| 1137         | 合計請求書用請求先部署番号が不正                                                                                                       |
| 1138         | 合計請求書用請求先部署コードが不正                                                                                                     |
| 1139         | 請求書テンプレートコードが不正                                                                                                         |
| 1140         | 請求先決済情報コードが不正                                                                                                             |
| 1141         | 請求先決済情報名が不正                                                                                                                 |
| 1142         | 請求元銀行口座パターンコードが不正                                                                                                     |
| 1143         | 決済手段が不正                                                                                                                         |
| 1144         | 振込元口座名義が不正                                                                                                                   |
| 1145         | 顧客番号が不正                                                                                                                         |
| 1146         | 銀行コードが不正                                                                                                                       |
| 1147         | 銀行名が不正                                                                                                                           |
| 1148         | 支店コード                                                                                                                             |
| 1149         | 支店名が不正                                                                                                                           |
| 1150         | 預金種目が不正                                                                                                                         |
| 1151         | 口座番号が不正                                                                                                                         |
| 1152         | 口座名義が不正                                                                                                                         |
| 1153         | 勘定科目コードが不正                                                                                                                   |
| 1154         | 仕訳連携要補助科目コードが不正                                                                                                         |
| 1155         | 請求先登録に失敗                                                                                                                       |
| 1156         | 請求先部署登録に失敗                                                                                                                   |
| 1157         | 請求先決済手段登録に失敗                                                                                                               |
| 1158         | 請求先決済手段にひもづくBI系の登録に失敗                                                                                                |
| 1159         | 請求先補助科目登録に失敗                                                                                                               |
| 1160         | 親請求先部署が存在しない                                                                                                               |
| 1161         | 指定した親部署が、既に他の親部署を持っている                                                                                             |
| 1163         | 合計請求書用請求先部署番号と合計請求書用請求先部署コードは両方指定できない                                                                |
| 1164         | 請求先部署の登録ステータスが不正                                                                                                       |
| 1165         | 親部署のデフォルト利用可能決済手段が、請求書テンプレートで利用可能になっていません。                                                       |
| 1166         | 請求先に紐づく請求先部署が存在しない                                                                                                   |
| 1167         | 請求先部署番号と請求先部署コードは同時に指定できません。                                                                                 |
| 1168         | クレジットカード登録方法が不正                                                                                                         |
| 1169         | 親請求先部署は承認依頼中のため使用できません。                                                                                          |
| 1170         | 合計請求書テンプレートを指定する場合、合計請求書_請求先部署を指定してください。                                                           |
| 1171         | 決済タイプが不正                                                                                                                       |
| 1172         | 請求先決済番号が不正                                                                                                                   |
| 1173         | 請求先決済番号と請求先決済コードは同時に指定できません。                                                                                 |
| 1174         | 請求先部署売掛金補助科目コードが不正                                                                                                   |
| 1175         | 請求先部署前受金補助科目コードが不正                                                                                                   |
| 1176         | 請求先部署仮受金補助科目コードが不正                                                                                                   |
| 1177         | 合計請求書の設定で登録済みの請求情報があります。                                                                                        |
| 1178         | クレジットカード情報登録メール送信に失敗しました。                                                                                      |
| 1179         | 手数料自動消込許容金額が不正                                                                                                           |
| 1180         | バーチャル口座割当てに失敗                                                                                                             |
| 1181         | バーチャル口座が使用中                                                                                                                 |
| 1182         | 利用可能なバーチャル口座番号がない                                                                                                     |
| 1183         | クレジットカード登録メール送信先が不正 <br> 101桁以上またはメール形式以外の値で発生 <br> メールアドレスは,(カンマ)区切りで複数指定不可       |
| 1184         | 請求先以下の内部データにエラーがあった場合                                                                                              |
| 1185         | 請求先以下の同列のデータにエラーがあった場合                                                                                            |
| 1186         | 有効かつ未削除の親請求先部署が存在していない場合                                                                                        |
| 1187         | バーチャル口座は変更不可能                                                                                                             |
| 1188         | 消込キーが不正                                                                                                                         |
| 1189         | まるなげ与信が申請中又は有効のため請求先の更新に失敗                                                                                     |

----

[TOPへ戻る](../../index.md)
