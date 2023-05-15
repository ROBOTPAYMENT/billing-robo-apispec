# 請求情報登録更新

`/api/v1.0/demand/bulk_upsert`

複数の請求情報を登録または更新する処理。
停止中の請求情報を指定して送信すると、有効な状態に復活します。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Path: `/api/v1.0/demand/bulk_upsert`
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

| 名前                      | 概要                                                                                                                                                                                                                                                                                                                                                        | 桁数         | 種別                                   | 必須                          |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | -------------------------------------- | ----------------------------- |
| billing_code              | 請求先コード  <br> ※両端のスペース除去 <br> ※登録後は変更できません                                                                                                                                                                                                                                                                                         | 20           | [半角英数 + 記号](../../index.md#種別) | (追加時)                      |
| billing_individual_number | 請求先部署番号  <br> ※両端のスペース除去 <br> ※登録後は変更できません                                                                                                                                                                                                                                                                                       | 18           | 数値                                   | (追加時)^1                    |
| billing_individual_code   | 請求先部署コード  <br> ※両端のスペース除去 <br> ※登録後は変更できません                                                                                                                                                                                                                                                                                     | 20           | [半角英数 + 記号](../../index.md#種別) | (追加時)^1                    |
| payment_method_code       | 決済情報コード  <br> ※請求先部署に登録されている場合は任意 <br> ※両端のスペース除去                                                                                                                                                                                                                                                                         | 20           | [半角英数 + 記号](../../index.md#種別) | (追加時)                      |
| number                    | 請求情報番号  <br> ※両端のスペース除去 <br> ※登録後は変更できません                                                                                                                                                                                                                                                                                         | 18           | 数値                                   | (更新時)^2                    |
| code                      | 請求情報コード  <br> ※両端のスペース除去 <br> ※登録後は変更できません                                                                                                                                                                                                                                                                                       | 20           | [半角英数 + 記号](../../index.md#種別) | (更新時)^2                    |
| item_code                 | 商品コード <br> ※商品マスタに登録されているコード <br> ※追加省略時、nullで登録される <br> ※登録後は変更できません                                                                                                                                                                                                                                           | 20           | [半角英数 + 記号](../../index.md#種別) |                               |
| type                      | 請求タイプ <br> 0:単発 <br> 1:定期定額 <br> 2:定期従量  <br> ※商品マスタに登録されている場合は任意 <br> ※登録後は変更できません                                                                                                                                                                                                                                       | 1            | 数値                                   | (追加時)                      |
| goods_code                | 集計用商品コード <br> ※項目未指定の場合、商品マスタの値をセット <br> ※追加省略時、nullで登録される <br> ※更新時、空文字指定で空白にできます。                                                                                                                                                                                                                                                         | 100           | 文字列                                 |                               |
| link_goods_code           | 会計ソフト連携用商品コード <br> ※項目未指定の場合、商品マスタの値をセット <br> ※追加省略時、nullで登録される <br> ※更新時、空文字指定で空白にできます。                                                                                                                                                                                                                                               | 33           | 文字列                                 |                               |
| goods_name                | 商品名  <br> ※商品マスタに登録されている場合は任意 <br> ※両端のスペース除去                                                                                                                                                                                                                                                                                 | 60           | 文字列                                 | (追加時)                      |
| price                     | 単価 <br> ※商品マスタに登録されている場合は任意 <br> ※両端のスペース除去 <br> ※クレジットカード決済の場合は桁数上限整数7桁                                                                                                                                                                                                                                  | 整数10,小数4 | 数値                                   | (追加時でtype=0,1時)          |
| quantity                  | 数量 <br> ※両端のスペース除去                                                                                                                                                                                                                                                                                                                               | 整数6,小数2  | 数値                                   | (追加時でtype=0,1時)          |
| unit                      | 単位 <br> ※追加省略時、nullで登録される <br> ※更新時、空文字指定で空白にできます。                                                                                                                                                                                                                                                                                                                   | 3            | 文字列                                 |                               |
| tax_category              | 税区分 <br> 0:外税 <br> 1:内税 <br> 2:対象外 <br> 3:非課税  <br> ※商品マスタに登録されている場合は任意                                                                                                                                                                                                                                                                     | 1            | 数値                                   | (追加時)                      |
| tax                       | 消費税率 <br> 5:5% <br> 8:8% <br> 10:10% <br> ※画面上で選択できる消費税のみ入力可能 <br> その他はNULL固定。 <br> ※商品マスタに登録されている場合は任意                                                                                                                                                                                                                       | 2            | 数値                                   | (追加時でtax_category=0.1時)  |
| remark                    | 備考 <br> ※追加省略時、nullで登録される <br> ※更新時、空文字指定で空白にできます。                                                                                                                                                                                                                                                                                                                    | 60×17行      | 文字列                                 |                               |
| billing_method            | 請求方法 <br> 0:送付なし <br> 1:自動メール <br> 2:手動メール <br> 3:自動郵送 <br> 4:手動郵送 <br> 5:自動メール+自動郵送 <br> 6:手動メール+手動郵送 <br> 7:手動マイページ配信 (※マイページ機能を利用可能な方のみ設定できます) <br> 8:自動マイページ配信 (※マイページ機能を利用可能な方のみ設定できます) <br> 以下の承認を利用している場合、対応する請求方法を指定するとエラー <br> 請求書メール送付承認:自動メール <br> 請求書郵送承認:自動郵送 <br> 請求書メール+郵送:自動メール+自動郵送 <br> ※請求先部署に登録されている場合は任意 | 1            | 数値                                   | (追加時)                      |
| repetition_period_number  | 繰返し周期 <br> 1～60 <br> ※商品マスタに登録されている場合は任意                                                                                                                                                                                                                                                                                            | 2            | 数値                                   | (追加時でtype=1,2時)          |
| repetition_period_unit    | 繰返し周期単位 <br> 1:月  <br> ※商品マスタに登録されている場合は任意                                                                                                                                                                                                                                                                                        | 1            | 数値                                   | (追加時でtype=1,2時)          |
| start_date                | サービス提供開始日 <br> yyyy/mm/dd  <br> ※登録後は変更できません                                                                                                                                                                                                                                                                                            | 10           | 日付                                   | (追加時)                      |
| repeat_count              | 繰返し回数 <br> 0:設定しない または1～60 <br> ※商品マスタに登録されている場合は任意 <br> ※追加省略時、上記type以外は1で登録される                                                                                                                                                                                                                           | 2            | 数値                                   | (追加時でtype=1,2時)          |
| period_format             | 対象期間形式 <br> 0:○年○月分 <br> 1:○年○月○日分 <br> 2:○年○月～○年○月 <br> 3:○年○月○日～○年○月○日 <br> 99:非表示  <br> ※商品マスタに登録されている場合は任意                                                                                                                                                                                           | 2            | 数値                                   | (追加時)                      |
| period_value              | 対象期間  <br> ※商品マスタに登録されている場合は任意                                                                                                                                                                                                                                                                                                        | 2            | 数値                                   | (追加時でperiod_format=2,3時) |
| period_unit               | 対象期間単位 <br> 1:月  <br> ※商品マスタに登録されている場合は任意                                                                                                                                                                                                                                                                                          | 1            | 数値                                   | (追加時でperiod_format=3時)   |
| period_criterion          | 基準 <br> 0:対象期間開始日 1:対象期間終了日  <br> ※商品マスタに登録されている場合は任意 <br> ※追加省略時、0で登録される <br> ※登録後は変更できません                                                                                                                                                                                                        | 1            | 数値                                   | (追加時でperiod_format=2,3時)   |
| sales_recorded_month      | 売上計上日_月 <br> 入力可能値:-60～60 <br> 例） <br> -1:前月 <br> 0:当月 <br> 60:60ヵ月後 <br> ※売上計上日を設定する場合に指定 <br> ※追加省略時、nullで登録される                                                                                                                                                                                        | 2            | 数値                                   |                               |
| sales_recorded_day        | 売上計上日_日 <br> 入力可能値:1～30、99(末日) <br> ※売上計上日を設定する場合に指定 <br> ※追加省略時、nullで登録される                                                                                                                                                                                                                                       | 2            | 数値                                   |                               |
| issue_month               | 請求書発行日_月 <br> 入力可能値:-60～60 <br> 例） <br> -1:前月 <br> 0:当月 <br> 60:60ヵ月後  <br> ※請求先部署に登録されている場合は任意                                                                                                                                                                                                                  | 2            | 数値                                   | (追加時)                      |
| issue_day                 | 請求書発行日_日 <br> 入力可能値:1～30、99(末日)  <br> ※請求先部署に登録されている場合は任意                                                                                                                                                                                                                                                                 | 2            | 数値                                   | (追加時)                      |
| sending_month             | 請求書送付日_月 <br> 入力可能値:-60～60 <br> 例） <br> -1:前月 <br> 0:当月 <br> 60:60ヵ月後  <br> ※請求先部署に登録されている場合は任意                                                                                                                                                                                                                  | 2            | 数値                                   | (追加時)                      |
| sending_day               | 請求書送付日_日 <br> 入力可能値:1～30、99(末日)  <br> ※請求先部署に登録されている場合は任意                                                                                                                                                                                                                                                                 | 2            | 数値                                   | (追加時)                      |
| deadline_month            | 決済期限_月 <br> 入力可能値:-60～60 <br> 例） <br> -1:前月 <br> 0:当月 <br> 60:60ヵ月後  <br> ※請求先部署に登録されている場合は任意                                                                                                                                                                                                                      | 2            | 数値                                   | (追加時)                      |
| deadline_day              | 決済期限_日 <br> 入力可能値:1～30、99(末日)  <br> ※請求先部署に登録されている場合は任意 <br> ※決済手段がRP口座振替の場合、振替日（10or26）を指定                                                                                                                                                                                                            | 2            | 数値                                   | (追加時)                      |
| slip_deadline_month       | 払込票有効期限_月 <br> 入力可能値:-60～60 <br> 例） <br> -1:前月 <br> 0:当月 <br> 60:60ヵ月後 <br> ※決済手段が、コンビニ払込票（ハガキ）で、払込票有効期限を設定する場合に指定 <br> ※追加省略時、nullで登録される                                                                                                                                        | 2            | 数値                                   |                               |
| slip_deadline_day         | 払込票有効期限_日 <br> 入力可能値:1～30、99(末日) <br> ※決済手段が、コンビニ払込票（ハガキ）で、払込票有効期限を設定する場合に指定 <br> ※追加省略時、nullで登録される                                                                                                                                                                                       | 2            | 数値                                   |                               |
| memo                      | メモ <br> ※省略時、nullで登録される <br> ※更新時、空文字指定で空白にできます。                                                                                                                                                                                                                                                                                                                        | 300          | 文字列                                 |                               |
| bill_template_code        | 請求書テンプレートコード <br> 10000:基本テンプレート <br> 10010:シンプル  <br> ※請求先部署に登録されている場合は任意 <br> ※合計請求書をご利用される場合は、お手数ですがお問い合わせください                                                                                                                                                                 | 18           | 数値                                   | (追加時)                      |
| bs_residence_code         | 請求元差出人コード <br> ※両端のスペース除去 <br> ※（追加時）空文字・省略・NULLの場合、請求元設定で登録しているデフォルト差出人が適用されます <br> ※（更新時）空文字、省略、nullの場合は更新しません                                                                                                                                                                                                                                  | 20           | [半角英数 + 記号](../../index.md#種別) |                               |
| bs_owner_code             | 請求元担当者コード <br> ※両端のスペース除去 <br> ※請求先部署に登録されている場合は任意 <br> ※追加省略時、nullで登録される                                                                                                                                                                                                                                   | 20           | [半角英数 + 記号](../../index.md#種別) |                               |
| account_title_code        | 勘定科目コード <br> ※両端のスペース除去 <br> ※追加省略時、4100で登録される                                                                                                                                                                                                                                                                                  | 20           | [半角英数 + 記号](../../index.md#種別) |                               |
| bill_group_key            | 請求書合算キー                                                                                                                                                                                                                                                                                                                                              | 256          | 文字列                                 |                               |
| outside_billing_number | 外部連携用請求書番号 <br>指定しない・null・`""`(空文字)の場合、通常通り請求書番号が発番されます<br>更新時、`""`を指定すると通常通り請求書番号を発番できます　| 32 | [半角英数 + 記号](../../index.md#種別) | |
| [custom](#custom-request) | 請求情報カスタム項目に属するパラメータ                                                                                                                                                                                                                                                                                                                        |           | array                                 |                               |

- 更新時、パラメータ名=””というように値を未指定（空）で送信した場合、空文字で更新されます。
- 更新時、パラメータを送信しなかった場合、更新はされず、既存の値が保持されます。
- 更新時、商品を指定して請求情報を登録していた場合、商品の変更はできません。
- 商品を指定して請求情報を登録した後、商品情報を変更し、請求情報更新で同商品を選択し更新した場合、請求情報の既存の値が保持されます。

#### custom (request)

下記のような項目のオブジェクトを持つリスト<br>
商品を指定して請求情報を登録する際、カスタム項目値を未入力でリクエストすると、商品のカスタム項目値を反映します。

| 名前                      | 概要                                                                                                                                                                                                                                                                                                                                                        | 桁数         | 種別                                   | 必須                          |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | -------------------------------------- | ----------------------------- |
| number              | カスタム項目番号                                                                                                                                                                                                                                                                                                                                                    | 18           | 数値                                   | (必須※1)^1 |
| code                | カスタム項目コード                                                                                                                                                                                                                                                                                                                                                  | 20           | [半角英数 + 記号](../../index.md#種別)  | (必須※1)^1 |
| value               | カスタム項目値                                                                                                                                                                                                                                                                                                                                                      | 300          | 文字列                                 |                       |

※1 カスタム項目必須フラグONでカスタム項目を登録してる場合必須です。また請求情報カスタム項目の登録更新時にはnumberもしくはcodeが必須です。




## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                       | 概要                                 | 型              |
| -------------------------- | ------------------------------------ | --------------- |
| user_id                    | ユーザーID（管理画面へのログインID） | string          |
| [demand](#demand-response) | 請求情報に属するパラメータ           | `array` |

#### demand (response)

下記のような項目のオブジェクトを持つリスト

| 名前                      | 概要                                                                                                                      | 型     |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------- | ------ |
| error_code                | エラーコード <br> ※正常時はnull                                                                                                        | int    |
| error_message             | エラーメッセージ <br> ※正常時はnull                                                                                                    | string |
| billing_code              | 請求先コード                                                                                                                           | string |
| billing_individual_number | 請求先部署番号                                                                                                                         | int    |
| billing_individual_code   | 請求先部署コード                                                                                                                       | string |
| payment_method_code       | 決済情報コード                                                                                                                         | string |
| number                    | 請求情報番号 <br> ※登録時、請求管理ロボ側で発番される番号となります                                                                    | int    |
| code                      | 請求情報コード                                                                                                                         | string |
| item _code                | 商品コード                                                                                                                             | string |
| type                      | 請求タイプ <br> 0:単発 <br> 1:定期定額 <br> 2:定期従量                                                                                   | int    |
| goods_code                | 集計用商品コード                                                                                                                       | string |
| link_goods_code           | 会計ソフト連携用商品コード                                                                                                             | string |
| goods_name                | 商品名                                                                                                                                 | string |
| price                     | 単価                                                                                                                                   | string |
| quantity                  | 数量                                                                                                                                   | string |
| unit                      | 単位                                                                                                                                   | string |
| tax_category              | 税区分 <br> 0:外税 <br> 1:内税 <br> 2:対象外 <br> 3:非課税                                                                               | int    |
| tax                       | 消費税率                                                                                                                               | int    |
| remark                    | 備考                                                                                                                                   | string |
| billing_method            | 請求方法 <br> 0:送付なし <br> 1:自動メール <br> 2:手動メール 3:自動郵送 <br> 4:手動郵送 <br> 5:自動メール+自動郵送 <br> 6:手動メール+手動郵送 <br> 7:手動マイページ配信 (※マイページ機能を利用可能な方のみ設定できます) <br> 8:自動マイページ配信 | int    |
| repetition_period_number  | 繰返し周期                                                                                                                             | int    |
| repetition_period_unit    | 繰返し周期単位                                                                                                                         | int    |
| start_date                | サービス提供開始日 <br> yyyy/mm/dd                                                                                                     | date   |
| end_date                  | 最終サービス提供期間 <br> yyyy/mm/dd <br> ※repeat_count=0の場合はnull                                                                  | date   |
| repeat_count              | 繰返し回数 <br> ※type=1,2以外はnull                                                                                                    | int    |
| period_format             | 対象期間形式 <br> 0:○年○月分 <br> 1:○年○月○日分 <br> 2:○年○月～○年○月 <br> 3:○年○月○日～○年○月○日 <br> 99:非表示                            | int    |
| period_value              | 対象期間 <br> ※period_format=2,3以外はnull                                                                                             | int    |
| period_unit               | 対象期間単位 <br> ※period_format=3以外はnull                                                                                           | int    |
| period_criterion          | 基準 <br> 0:対象期間開始日 <br> 1:対象期間終了日 <br> ※period_format=2,3以外は0                                                          | int    |
| sales_recorded_month      | 売上計上日_月                                                                                                                          | int    |
| sales_recorded_day        | 売上計上日_日                                                                                                                          | int    |
| issue_month               | 請求書発行日_月                                                                                                                        | int    |
| issue_day                 | 請求書発行日_日                                                                                                                        | int    |
| sending_month             | 請求書送付日_月                                                                                                                        | int    |
| sending_day               | 請求書送付日_日                                                                                                                        | int    |
| deadline_month            | 決済期限_月                                                                                                                            | int    |
| deadline_day              | 決済期限_日                                                                                                                            | int    |
| slip_deadline_month       | 払込票有効期限_月                                                                                                                      | int    |
| slip_deadline_day         | 払込票有効期限_日                                                                                                                      | int    |
| next_issue_date           | 次回請求書発行日 <br> ※次回請求書が存在しない場合はnull                                                                                | date   |
| memo                      | メモ                                                                                                                                   | string |
| bill_template_code        | 請求書テンプレートコード                                                                                                               | int    |
| bs_residence_code         | 請求元差出人コード                                                                                                                     | string |
| bs_owner_code             | 請求元担当者コード                                                                                                                     | string |
| account_title_code        | 勘定科目コード                                                                                                                         | string |
| text_pattern_code         | 文章パターンコード                                                                                                                     | string |
| bill_group_key            | 請求書合算キー                                                                                                                         | string |
| outside_billing_number | 外部連携用請求書番号 | string |
| [custom](#custom-response) | 請求情報カスタム項目に属するパラメータ           | `array` |

#### custom (response)

下記のような項目のオブジェクトを持つリスト

| 名前                      | 概要                                                                                                                      | 型     |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------- | ------ |
| error_code                | エラーコード <br> ※正常時はnull                                                                                            | int    |
| error_message             | エラーメッセージ <br> ※正常時はnull                                                                                        | string |
| number                    | カスタム項目番号                                                                                                            | int    |
| code                      | カスタム項目コード                                                                                                          | string |
| name                      | カスタム項目名                                                                                                              | string |
| value                     | カスタム項目値                                                                                                              | string |

## 使用例

### リクエスト例

２ケースを同時の登録する際のリクエスト例

ケース１
- 商品指定あり
- 請求先部署を参照できる項目を事前登録している
- カスタム項目を2つ設定している

ケース２
- 商品指定なし
- 請求先部署を参照できる項目を事前登録していない
- カスタム項目を2つ設定している

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "demand": [
        {
            "billing_code": "billing1",
            "billing_individual_code": "abc1234",
            "item_code": "item1",
            "quantity": 5,
            "start_date": "2016/05/01",
            "custom":[
                {
                    "number": 15,
                    "value": "カスタム項目値登録1"
                }
            ]
        },
        {
            "billing_code": "billing2",
            "billing_individual_code": "abc1234",
            "payment_method_code": "payment1234",
            "type": 0,
            "goods_name": "商品2",
            "price": 2000,
            "quantity": 10,
            "tax_category": 0,
            "tax": 8,
            "billing_method": 1,
            "start_date": "2016/04/01",
            "period_format": 0,
            "issue_month": 0,
            "issue_day": 1,
            "sending_month": 0,
            "sending_day": 1,
            "deadline_month": 0,
            "deadline_day": 1,
            "bill_template_code": 10010,
            "custom":[
                {
                    "code": "custom16",
                    "value": "カスタム項目値登録2"
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
    "demand": [
        {
            "error_code": null,
            "error_message": null,
            "billing_code": "billing1",
            "billing_individual_number": 1,
            "billing_individual_code": "abc1234",
            "payment_method_code": "payment1234",
            "number": 1,
            "code": "",
            "item_code": "item1",
            "type": 0,
            "goods_code": "",
            "link_goods_code": "",
            "goods_name": "商品１",
            "price": 1000,
            "quantity": 5,
            "unit": "",
            "tax_category": 0,
            "tax": 8,
            "remark": "",
            "billing_method": 1,
            "repetition_period_number": null,
            "repetition_period_unit": null,
            "start_date": "2016/05/01",
            "end_date": null,
            "repeat_count": null,
            "period_format": 0,
            "period_value": null,
            "period_unit": null,
            "period_criterion": 0,
            "issue_month": 0,
            "issue_day": 1,
            "sending_month": 0,
            "sending_day": 1,
            "deadline_month": 0,
            "deadline_day": 1,
            "slip_deadline_month": null,
            "slip_deadline_day": null,
            "next_issue_date": null,
            "memo": "",
            "bill_template_code": 10010,
            "bs_residence_code": "bs_residence_code",
            "bs_owner_code": "bs_owner_code",
            "account_title_code": "4100",
            "text_pattern_code": null,
            "bill_group_key" : null,
            "outside_billing_number" : null,
            "custom":[
                {
                    "error_code": null,
                    "error_message": null,
                    "number": 15,
                    "code": "mst_costom15",
                    "name": "カスタム項目１５",
                    "value": "カスタム項目値登録1"
                },
                {
                    "error_code": null,
                    "error_message": null,
                    "number": 16,
                    "code": "mst_costom16",
                    "name": "カスタム項目１６",
                    "value": null
                }
            ]
        },
        {
            "error_code": null,
            "error_message": null,
            "billing_code": "billing2",
            "billing_individual_number": 2,
            "billing_individual_code": "abc1234",
            "payment_method_code": "payment1234",
            "number": 2,
            "code": "",
            "item_code": "",
            "type": 0,
            "goods_code": "",
            "link_goods_code": "",
            "goods_name": "商品2",
            "price": 2000,
            "quantity": 10,
            "unit": "",
            "tax_category": 0,
            "tax": 8,
            "remark": "",
            "billing_method": 1,
            "repetition_period_number": null,
            "repetition_period_unit": null,
            "start_date": "2016/04/01",
            "end_date": null,
            "repeat_count": null,
            "period_format": 0,
            "period_value": null,
            "period_unit": null,
            "period_criterion": 0,
            "issue_month": 0,
            "issue_day": 1,
            "sending_month": 0,
            "sending_day": 1,
            "deadline_month": 0,
            "deadline_day": 1,
            "slip_deadline_month": null,
            "slip_deadline_day": null,
            "next_issue_date": null,
            "memo": "",
            "bill_template_code": 10010,
            "bs_owner_code": "bs_owner_code",
            "account_title_code": "4100",
            "text_pattern_code": null,
            "bill_group_key" : null,
            "outside_billing_number" : null,
            "custom":[
                {
                    "error_code": null,
                    "error_message": null,
                    "number": 15,
                    "code": "mst_costom15",
                    "name": "カスタム項目１５",
                    "value": null 
                },
                {
                    "error_code": null,
                    "error_message": null,
                    "number": 16,
                    "code": "mst_costom16",
                    "name": "カスタム項目１６",
                    "value": "カスタム項目値登録2"
                }
            ]
        }
    ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                                                                              |
|--------|---------------------------------------------------------------------------------|
| 1301   | 請求先コードが不正                                                                       |
| 1302   | 請求先部署番号が不正                                                                      |
| 1303   | 請求先部署コードが不正                                                                     |
| 1304   | 決済情報コードが不正                                                                      |
| 1305   | 請求情報番号が不正                                                                       |
| 1306   | 請求情報コードが不正                                                                      |
| 1307   | 商品コードが不正                                                                        |
| 1308   | 請求タイプが不正                                                                        |
| 1309   | 集計用商品コードが不正                                                                     |
| 1310   | 会計ソフト連携用商品コードが不正                                                                |
| 1311   | 商品名が不正                                                                          |
| 1312   | 単価が不正                                                                           |
| 1313   | 数量が不正                                                                           |
| 1314   | 単位が不正                                                                           |
| 1315   | 税区分が不正                                                                          |
| 1316   | 消費税率が不正                                                                         |
| 1317   | 備考が不正                                                                           |
| 1318   | 請求方法が不正                                                                         |
| 1319   | 繰返し周期が不正                                                                        |
| 1320   | 繰返し周期単位が不正                                                                      |
| 1321   | サービス提供開始日が不正                                                                    |
| 1322   | 繰返し回数が不正                                                                        |
| 1323   | 対象期間形式が不正                                                                       |
| 1324   | 対象期間が不正                                                                         |
| 1325   | 対象期間単位が不正                                                                       |
| 1326   | 基準が不正                                                                           |
| 1327   | 請求書発行日_月が不正                                                                     |
| 1328   | 請求書発行日_日が不正                                                                     |
| 1329   | 請求書送付日_月が不正                                                                     |
| 1330   | 請求書送付日_日が不正                                                                     |
| 1331   | 決済期限_月が不正                                                                       |
| 1332   | 決済期限_日が不正                                                                       |
| 1333   | メモが不正                                                                           |
| 1334   | 請求書テンプレートコードが不正                                                                 |
| 1335   | 請求元担当者コードが不正                                                                    |
| 1336   | 勘定科目コードが不正                                                                      |
| 1338   | 請求先部署番号と請求先部署コードは同時に指定できません                                                     |
| 1339   | 請求先部署が存在しません                                                                    |
| 1340   | 決済情報コードが存在しません                                                                  |
| 1341   | 請求先部署の登録ステータスが不正                                                                |
| 1342   | 請求情報番号と請求情報コードは同時に指定できません                                                       |
| 1343   | 請求情報が存在しない                                                                      |
| 1344   | 商品が存在しない                                                                        |
| 1345   | 請求情報登録に失敗                                                                       |
| 1346   | 文章パターンコードが不正                                                                    |
| 1347   | 文章パターンコードが存在しません                                                                |
| 1348   | 決済タイプが不正                                                                        |
| 1349   | 売上計上日_月が不正                                                                      |
| 1350   | 売上計上日_日が不正                                                                      |
| 1351   | 払込票有効期限_月が不正                                                                    |
| 1352   | 払込票有効期限_日が不正                                                                    |
| 1353   | 親部署のデフォルト利用可能決済手段<font size="2" color="#ff0000">*注１</font>とは異なる決済手段は指定できません。<br> |
| 1354   | 既に締められた期間への請求書発行日は設定できません                                                       |
| 1355   | 既に締められた期間への売上計上日の指定はできません                                                       |
| 1356   | 請求書合算キーが不正                                                                      |
| 1357   | 外部連携用請求書番号が不正<br>桁数や入力可能文字をご確認ください                                              |
| 1358   | カスタム項目情報のデータにエラーがあった場合                                                          |
| 1359   | カスタム項目番号が不正                                                                     |
| 1360   | カスタム項目コードが不正                                                                    |
| 1361   | カスタム項目値が不正                                                                      |
| 1362   | カスタム項目番号とカスタム項目コードは同時に指定できません                                                   |
| 1363   | 対象のカスタム項目情報が存在しません                                                              |
| 1364   | カスタム項目リクエスト件数が上限を超えています                                                         |
| 1365   | カスタム項目情報にはarrayを指定してください                                                        |
| 1366   | 請求元差出人コードが不正                                                                    |
| 1367   | 請求元差出人が存在しません                                                                   |
| 1368   | 選択された消費税率は利用不可です                                                                |
| 1369   | 請求情報配列内のリクエスト形式が不正です。<br/> - 請求情報の配列内でオブジェクトを持つリスト以外の形式が含まれている場合                |

- 注１.このデフォルト利用可能決済手段は「コンビニ払込票（ハガキ）、その他コンビニ払込票」を指します


----

[TOPへ戻る](../../index.md)
