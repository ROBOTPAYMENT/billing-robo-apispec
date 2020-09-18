# 決済情報参照

`/api/v1.0/billing_payment_method/search`

決済情報の参照処理を実行します。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト

- Method URL: `https://billing-robo.jp:10443/api/v1.0/billing_payment_method/search`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                    | 概要                                                                                    | 桁数 | 種別                              | 必須 |
| ----------------------- | --------------------------------------------------------------------------------------- | ---- | --------------------------------- | ---- |
| user_id                 | ユーザー ID                                                                             | 100  | [メール形式](../../index.md#種別) | 必須 |
| access_key              | アクセスキー                                                                            | 100  | [半角英数](../../index.md#種別)   | 必須 |
| limit_count             | 決済情報取得件数 <br> ※0〜200の数値を設定する。省略した場合、20 が設定される                   | 3    | 数値                              |      |
| page_count              | 決済情報取得開始インデックス <br> ※0〜99の数値を設定する。省略した場合、0 が設定される           | 2    | 数値                              |      |
| [billing_payment_method](#billing_payment_method-request) | 決済情報に属するパラメータ                                                                  |      | `object`                          |      |

#### billing_payment_method (request)

| 名前        | 概要                                                  | 桁数 | 種別          | 一致     |
| ----------- | ----------------------------------------------------- | ---- | ------------- | -------- |
| billing_code     | 請求先コード                                              | 20   | 半角英数+記号  | 完全一致 |
| billing_name     | 請求先名                                                  | 100   | 文字列       |  部分一致|
| number           | 決済情報番号                                              | 18   | 数値          | 完全一致 |
| code             | 決済情報コード                                            | 20   | 半角英数+記号 | 完全一致 |
| name             | 決済情報名                                               | 100   | 文字列        | 部分一致 |
| payment_method   | 決済手段 <br> 0:銀行振込 <br>1:クレジットカード <br>2:バンクチェック <br> 3:RP口座振替 <br>4:RL口座振替 <br>5:その他口座振替 <br>6:コンビニ払込票(A4) <br>7:コンビニ払込票(ハガキ) <br>8:その他コンビニ払込票 <br>9:バーチャル口座 <br>10:その他決済1<br>11:その他決済2<br>12:その他決済3<br>13:その他決済4<br>14:その他決済5 | 2    | 数値          |          |
| register_status  | 登録ステータス <br> 0:未処理 <br>1:登録待ち <br>2:メール送信済み <br>3:申請中 <br> 4:登録情報_送信エラー <br>5:登録完了 <br>6:登録失敗  | 1    | 数値          |          |
| regist_date_from | 登録日の検索開始日                                     | 10    | 日付          |          |
| regist_date_to   | 登録日の検索終了日                                     | 10    | 日付          |          |
| update_date_from | 更新日の検索開始日                                     | 10    | 日付          |          |
| update_date_to   | 更新日の検索終了日                                     | 10    | 日付          |          |
| valid_flg        | 状態 <br> 0:無効 <br> 1:有効                          | 1     | 数値          |          |

## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                     | 概要                                                                                                                                          | 型      |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------- | ------- |
| user_id                  | ユーザー ID                                                                                                                                   | string  |
| access_key               | アクセスキー                                                                                                                                  | string  |
| limit_count              | 決済情報取得件数 <br> ※最大件数は、リクエストで指定された「決済情報取得件数」に依存                                                                    | int     |
| page_count               | 決済情報取得開始インデックス <br> ※取得した決済情報の開始インデックスを返却する                                                                        | int     |
| total_page_count         | 決済情報取得開始インデックス合計 <br> ※指定された検索条件によって取得可能な決済情報の全件数／決済情報取得件数によって、算出される値を返却する | int             |
| [billing_payment_method](#billing_payment_method-response) | 決済情報に属するパラメータ                                                                                                                    | `array` |

#### billing_payment_method (response)

下記のような項目のオブジェクトを持つリスト

| 名前                           | 概要                                                                                                                                                                                                                                                             | 型     |
| ------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| error_code                     | エラーコード <br> ※正常時はnull                                                                                                                                                                                                                                  | string |
| error_message                  | エラーメッセージ <br> ※正常時はnull                                                                                                                                                                                                                              | int    |
| billing_code                   | 請求先コード                                                                                                                                                                                                                                                  | string    |
| billing_name                   | 請求先名                                                                                                                                                                                                                                                     | string    |
| number                         | 決済情報番号                                                                                                                                                                                                                                                     | int    |
| code                           | 決済情報コード                                                                                                                                                                                                                                                   | string |
| name                           | 決済情報名                                                                                                                                                                                                                                                       | string |
| bank_transfer_pattern_code     | 請求元銀行口座パターンコード                                                                                                                                                                                                                                     | string |
| payment_method                 | 決済手段 <br> 0:銀行振込 <br> 1:クレジットカード <br> 2:バンクチェック <br> 3:RP口座振替 <br> 4:RL口座振替 <br> 5:その他口座振替 <br> 6:コンビニ払込票(A4) <br> 7:コンビニ払込票(ハガキ) <br> 8:その他コンビニ払込票 <br> 9:バーチャル口座 <br> 10:その他決済1<br> 11:その他決済2<br> 12:その他決済3<br> 13:その他決済4<br> 14:その他決済5 | int    |
| register_status                | 登録ステータス <br> 0:未処理 <br> 1:登録待ち <br> 2:メール送信済み <br> 3:申請中 <br> 4:登録情報_送信エラー <br> 5:登録完了 <br> 6:登録失敗                                                                                                                                               | int    |
| source_bank_account_name       | 振込元口座名義 <br> ※payment_method=0,2,9以外はnull                                                                                                                                                                                                            | string |
| customer_number                | 顧客番号 <br> ※payment_method=3,4,5以外はnull                                                                                                                                                                                                                    | string |
| bank_code                      | 銀行コード <br> ※payment_method=3,4,5,9以外はnull                                                                                                                                                                                                              | string |
| bank_name                      | 銀行名 <br> ※payment_method=5,9以外はnull                                                                                                                                                                                                                      | string |
| branch_code                    | 支店コード <br> ※payment_method=3,4,5,9以外はnull                                                                                                                                                                                                              | string |
| branch_name                    | 支店名 <br> ※payment_method=5,9以外はnull                                                                                                                                                                                                                      | string |
| bank_account_type              | 預金種目 <br> 1:普通 2:当座 <br> ※payment_method=3,4,5,9以外はnull                                                                                                                                                                                             | string |
| bank_account_number            | 口座番号 <br> ※payment_method=3,4,5,9以外はnull                                                                                                                                                                                                                | string |
| bank_account_name              | 口座名義 <br> ※payment_method=3,4,5,9以外はnull                                                                                                                                                                                                                | string |
| payment_type                   | 決済タイプ <br> ※payment_method=8以外はnull <br>0：ハガキ・マルチレコードタイプ 1：B5督促用                                                                                                                                                                           | string |
| cod                            | 店舗オーダー番号 <br> ※payment_method=1以外はnull                                                                                                                                                                                                              | string |
| bank_check_bank_code           | バンクチェック銀行コード <br> ※payment_method=2以外はnull                                                                                                                                                                                                      | string |
| bank_check_bank_name           | バンクチェック銀行名 <br> ※payment_method=2以外はnull                                                                                                                                                                                                          | string |
| bank_check_branch_code         | バンクチェック支店コード <br> ※payment_method=2以外はnull                                                                                                                                                                                                      | string |
| bank_check_branch_name         | バンクチェック支店名 <br> ※payment_method=2以外はnull                                                                                                                                                                                                          | string |
| bank_check_kind                | バンクチェック口座種別 <br> 1:普通 2:当座 <br> ※payment_method=2以外はnull                                                                                                                                                                                     | string |
| bank_check_bank_account_number | バンクチェック口座番号 <br> ※payment_method=2以外はnull                                                                                                                                                                                                        | string |
| clearing_key                   | 消込キー <br> ※payment_method=10-14以外はnull                                                                                                                                                                                                                    | string | 
| regist_date                    | 登録日                                                                                                                                                                                                                                                      | date | 
| update_date                    | 更新日                                                                                                                                                                                                                                                      | date |
| valid_flg                      | 状態                                                                                                                                                                                                                                                      | int |
| [bank_save](#bank_save-response)              | 学習した口座に属するパラメータ <br>※payment_method=0,2以外はnull                                                                                                                                                                         | `array`  |

### bank_save (response)

| 名前 | 概要               | 型     |
| ---- | ------------------ | ------ |
| name | 口座名義            | string |

## 使用例

### リクエスト例

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "limit_count": "",
    "page_count": "",
    "billing_payment_method":{
        "billing_code": "billing_code",
        "billing_name": "請求先名",
        "number": 123,
        "code": "bank-123",
        "name": "銀行振込123",
        "payment_method": 0,
        "register_status": 5,
        "regist_date_from": "",
        "regist_date_to": "",
        "update_date_from": "",
        "update_date_to": "",
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
    "billing_payment_method": [
        {
            "error_code": null,
            "error_message": null,
            "billing_code": "billing_code",
            "billing_name": "請求先名",
            "number": 123,
            "code": "bank-123",
            "name": "銀行振込123",
            "bank_transfer_pattern_code": "bank-123",
            "payment_method": 0,
            "register_status": 5,
            "source_bank_account_name": "satou",
            "customer_number": null,
            "bank_code": null,
            "bank_name": null,
            "branch_code": null,
            "branch_name": null,
            "bank_account_type": null,
            "bank_account_number": null,
            "bank_account_name": null,
            "payment_type": null,
            "cod": null,
            "bank_check_bank_code": null,
            "bank_check_bank_name": null,
            "bank_check_branch_code": null,
            "bank_check_branch_name": null,
            "bank_check_kind": null,
            "bank_check_bank_account_number": null,
            "clearing_key": null,
            "regist_date": "2019/06/20",
            "update_date": "2019/06/20",
            "valid_flg": 1,
            "bank_save": [
                {
                    "name": "ﾌﾘｺﾐｻｷｺｳｻﾞﾒｲｷﾞ"
                },{
                    "name": "ﾌﾘｺﾐｻｷｺｳｻﾞﾒｲｷﾞ"
                }
            ]
        }
    ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                               |
| ------------ | ---------------------------------- |
| 4501         | 請求先番号が不正                     |
| 4502         | 請求先名が不正                     |
| 4503         | 決済情報番号が不正                  |
| 4504         | 決済情報コードが不正                |
| 4505         | 決済情報名が不正                    |
| 4506         | 決済手段が不正                      |
| 4507         | 登録ステータスが不正                 |
| 4508         | 登録日の検索開始日                   |
| 4509         | 登録日の検索終了日                   |
| 4510         | 更新日の検索開始日                   |
| 4511         | 更新日の検索終了日                   |
| 4512         | 状態が不正                          |
| 4513         | 決済情報取得件数が不正            |
| 4514         | 決済情報取得開始インデックスが不正  |
| 4515         | 決済情報参照に失敗                   |

---

[TOP へ戻る](../../index.md)
