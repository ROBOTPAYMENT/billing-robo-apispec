# billing-robo-apispec

## 目次

- [概要](#概要)
- [API一覧](#api一覧)
- [Webhook一覧](#webhook一覧)
- [注釈](#注釈)
  - [APIの仕様公開・仕様変更の方針](#apiの仕様公開仕様変更の方針)
  - [APIアクセス制限](#apiアクセス制限)
    - [リクエスト数の制限](#リクエスト数の制限)
    - [リクエストサイズの制限](#リクエストサイズの制限)
  - [種別](#種別)
  - [決済手段](#決済手段)
  - [消込手段](#消込手段)
  - [必須](#必須)
- [共通エラー](#共通エラー)
  - [エラーコード](#エラーコード)
  - [決済システムエラーコード](#決済システムエラーコード)
  - [その他の特殊なエラーコード](#その他の特殊なエラーコード)
- [推奨SSL/TLSバージョン](#推奨ssltlsバージョン)

## 概要

- Base URL:
    - 本番環境: `https://billing-robo.jp:10443`
    - デモ環境: `https://demo.billing-robo.jp:10443`
- Accepted content types: `application/json`
- Encode: `UTF-8`

## API一覧

| API | Method | Path |
| --- | ------ | ---- |
| [請求先登録更新](/public/billing/bulk_upsert.md) | POST | /api/v1.0/billing/bulk_upsert |
| [請求先停止削除](/public/billing/bulk_stop.md) | POST | /api/v1.0/billing/bulk_stop |
| [口座振替依頼書発行](/public/billing/bulk_download_pdf.md) | POST | /api/v1.0/billing/bulk_download_pdf |
| [請求先部署参照](/public/billing_individual/search.md) | POST | /api/v1.0/billing_individual/search |
| [クレジットカード登録(トークン方式)](/public/billing_payment_method/credit_card_token.md) | POST | /api/v1.0/billing_payment_method/credit_card_token |
| [決済情報参照](/public/billing_payment_method/search.md) | POST | /api/v1.0/billing_payment_method/search |
| [請求情報登録更新](/public/demand/bulk_upsert.md) | POST | /api/v1.0/demand/bulk_upsert |
| [請求情報停止削除](/public/demand/bulk_stop.md) | POST | /api/v1.0/demand/bulk_stop |
| [売上消込結果参照](/public/demand/search.md) | POST | /api/v1.0/demand/search |
| [請求情報参照](/public/demand/search2.md) | POST | /api/v1.0/demand/search2 |
| [請求書発行](/public/demand/bulk_issue_bill_select.md) | POST | /api/v1.0/demand/bulk_issue_bill_select |
| [即時決済 請求書合算](/public/demand/bulk_register.md) | POST | /api/demand/bulk_register |
| [請求書送付メール](/public/bill/send_bill_by_email.md) | POST | /api/v1.0/bill/send_bill_by_email |
| [請求書送付郵送](/public/bill/send_bill_by_mail.md) | POST | /api/v1.0/bill/send_bill_by_mail |
| [繰越予約](/public/bill/update_carryover.md) | POST | /api/v1.0/bill/update_carryover |
| [請求書参照](/public/bill/search.md) | POST | /api/v1.0/bill/search |
| [請求書更新](/public/bill/update.md) | POST | /api/v1.0/bill/update |
| [請求書無効](/public/bill/stop.md) | POST | /api/v1.0/bill/stop |
| [請求書明細参照](/public/bill_detail/search.md) | POST | /api/v1.0/bill_detail/search |
| [入金登録更新](/public/payment/bulk_upsert.md) | POST | /api/v1.0/payment/bulk_upsert |
| [入金無効削除](/public/payment/bulk_stop.md) | POST | /api/v1.0/payment/bulk_stop |
| [入金参照](/public/payment/search.md) | POST | /api/v1.0/payment/search |
| [消込](/public/clearing/exec.md) | POST | /api/v1.0/clearing/exec |
| [消込結果参照](/public/clearing/search.md) | POST | /api/v1.0/clearing/search |
| [消込取消](/public/clearing/bulk_cancel.md) | POST | /api/v1.0/clearing/bulk_cancel |
| [消込結果明細参照](/public/clearing_detail/search.md) | POST | /api/v1.0/clearing_detail/search |
| [商品登録更新2](/public/goods/bulk_upsert2.md) | POST | /api/v1.0/goods/bulk_upsert2 |
| [商品停止削除](/public/goods/bulk_stop.md) | POST | /api/v1.0/goods/bulk_stop |
| [商品参照](/public/goods/search.md) | POST | /api/v1.0/goods/search |
| [カスタム項目登録更新](/public/mst_custom_field/bulk_upsert.md) | POST | /api/v1.0/custom_field/bulk_upsert |
| [カスタム項目削除](/public/mst_custom_field/bulk_stop.md) | POST | /api/v1.0/custom_field/bulk_stop |
| [カスタム項目参照](/public/mst_custom_field/search.md) | POST | /api/v1.0/custom_field/search |
| [請求元銀行口座登録更新](/public/bs_bank_transfer/bulk_upsert.md) | POST | /api/v1.0/bs_bank_transfer/bulk_upsert |
| [請求元銀行口座停止削除](/public/bs_bank_transfer/bulk_stop.md) | POST | /api/v1.0/bs_bank_transfer/bulk_stop |
| [請求元銀行口座パターン登録更新](/public/bs_bank_transfer_pattern/bulk_upsert.md) | POST | /api/v1.0/bs_bank_transfer_pattern/bulk_upsert |
| [請求元銀行口座パターン停止削除](/public/bs_bank_transfer_pattern/bulk_stop.md) | POST | /api/v1.0/bs_bank_transfer_pattern/bulk_stop |
| [請求元部署登録更新](/public/bs_department/bulk_upsert.md) | POST | /api/v1.0/bs_department/bulk_upsert |
| [請求元部署停止削除](/public/bs_department/bulk_stop.md) | POST | /api/v1.0/bs_department/bulk_stop |
| [請求元担当者登録更新](/public/bs_owner/bulk_upsert.md) | POST | /api/v1.0/bs_owner/bulk_upsert |
| [請求元担当者停止削除](/public/bs_owner/bulk_stop.md) | POST | /api/v1.0/bs_owner/bulk_stop |
| [まるなげ与信申請登録](/public/request_marunage_credit/bulk_register.md) |  POST  | /api/v1.0/request_marunage_credit/bulk_register |
| [まるなげ与信申請参照](/public/request_marunage_credit/search.md)        |  POST  | /api/v1.0/request_marunage_credit/search        |
| [まるなげ与信解除](/public/marunage_credit/bulk_stop.md)                |  POST  | /api/v1.0/marunage_credit/bulk_stop             |
| [まるなげ与信参照](/public/marunage_credit/search.md)                   |  POST  | /api/v1.0/marunage_credit/search                |

<br>
- [非推奨のAPI一覧](/deprecated/index.md)

## Webhook一覧
- [Webhook請求書発行イベント](/webhook/webhook_bill.md)
- [Webhook郵送通知](/webhook/webhook_postmail.md)
- [Webhookクレジットカード登録状況通知](/webhook/webhook_credit_status.md)

## 注釈

### APIの仕様公開・仕様変更の方針

請求管理ロボでは機能の拡張に応じてAPIのサポートを終了し、最新機能に対応した新しいバージョンのAPIを提供する場合がございますが、<br>
最新の機能が利用できるよう既存のAPIに対して後方互換性のある拡張を随時実施します。<br>
後方互換性のある拡張例としては以下があります。

- リクエストのParameterに任意入力フィールドを追加
- レスポンスにフィールドを追加
- エラーコードの追加
- バリデーションの緩和

サポートを終了する場合にはご利用企業ご担当者様とのご相談の上、十分な移行期間を設けさせていただく方針
となっております。

### APIアクセス制限

2021/11/17(水)よりAPIの利用に下記の制限を設けます。下記の制限を予防する実装例を[こちら](https://keirinomikata.zendesk.com/hc/ja/articles/4404549985945)で公開しています。

#### リクエスト数の制限
- 同一IPアドレスによるリクエスト数を5分間で300回迄に制限します。
- 制限数を超過した場合、制限状態となりリクエスト数が上限値を下回るまでステータスコード `429 Too Many Requests` のエラーを返却します。
- リクエスト数が上限値を超過または下回っても、制限または制限解除の状態となる迄に2分程度のタイムラグが発生するためご注意ください。
- 制限中のリクエストも制限対象のリクエスト数としてカウントされますのでご注意ください。

#### リクエストサイズの制限
- Request Bodyのサイズを9Mbyte未満に制限します。
- Request Bodyのサイズの制限を超過した場合、ステータスコード `413 Request Entity Too Large` のエラーを返却します。

### 種別

| 種別                 | 入力可能値                                                                                                                                                                                             |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| メール形式           | 正規表現 : `/^([a-z0-9\+_\-]+)(\.[a-z0-9\+_\-]+)*@([a-z0-9\-]+\.)+[a-z]+$/ix` <br> (例) robot_payment@example.com                                                                                      |
| アルファベット       | `ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz`                                                                                                                                                 |
| 半角英数             | `ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789`                                                                                                                                       |
| 半角英数 + 記号      | ``ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789!"#$%&'()*+,-./:;<=>?@[\]^_`{|}~``                                                                                                     |
| 口座名義             | 半角英数 + 半角カタカナ + `,.()\/｢｣-` <br> `ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789ｦｧｨｩｪｫｬｭｮｯ-ｱｲｳｴｵｶｷｸｹｺｻｼｽｾｿﾀﾁﾂﾃﾄﾅﾆﾇﾈﾉﾊﾋﾂﾍﾎﾏﾐﾑﾒﾓﾔﾕﾖﾗﾘﾙﾚﾛﾜﾝｶﾞｷﾞｸﾞｹﾞｺﾞｻﾞｼﾞｽﾞｾﾞｿﾞﾀﾞﾁﾞﾂﾞﾃﾞﾄﾞﾊﾞﾋﾞﾌﾞﾍﾞﾎﾞﾊﾟﾋﾟﾌﾟﾍﾟﾎﾟ,. )(\/｢｣-` |
| 口座名義(RP口座振替) | 半角英数 + 半角カタカナ + `().-/` <br> [(RP口座振替)口座名義使用可能文字について](https://keirinomikata.zendesk.com/hc/ja/articles/900000986386)                                                       |
| 銀行名等             | 半角英数(半角英小文字除く) + 半角カタカナ <br>`ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789ｦｧｨｩｪｫｬｭｮｯ-ｱｲｳｴｵｶｷｸｹｺｻｼｽｾｿﾀﾁﾂﾃﾄﾅﾆﾇﾈﾉﾊﾋﾂﾍﾎﾏﾐﾑﾒﾓﾔﾕﾖﾗﾘﾙﾚﾛﾜﾝｶﾞｷﾞｸﾞｹﾞｺﾞｻﾞｼﾞｽﾞｾﾞｿﾞﾀﾞﾁﾞﾂﾞﾃﾞﾄﾞﾊﾞﾋﾞﾌﾞﾍﾞﾎﾞﾊﾟﾋﾟﾌﾟﾍﾟﾎﾟ`                                  |
| 日付形式             | 正規表現 : `@^([0-9]{4}(-|/)(0?[1-9]|1[012])(-|/)(0?[1-9]|[12][0-9]|3[01]))$@` <br> (例) 2020-01-01 または 2020/01/01 など                                                                             |

### 決済手段

| 数値 | 決済手段名             |
| ---- | ---------------------- |
| 0    | 銀行振込               |
| 1    | クレジットカード       |
| 2    | バンクチェック         |
| 3    | RP口座振替             |
| 4    | RL口座振替             |
| 5    | その他口座振替         |
| 6    | コンビニ払込票(A4)     |
| 7    | コンビニ払込票(ハガキ) |
| 8    | その他コンビニ払込票   |
| 9    | バーチャル口座         |
| 10   | その他決済1            |
| 11   | その他決済2            |
| 12   | その他決済3            |
| 13   | その他決済4            |
| 14   | その他決済5            |
| 15   | まるなげ口座振替       |
| 16   | まるなげバンクチェック  |

### 消込手段

| 数値 | 消込手段名             |
| ---- | ---------------------- |
| 0    | 銀行振込               |
| 1    | クレジットカード       |
| 2    | バンクチェック         |
| 3    | RP口座振替             |
| 4    | RL口座振替             |
| 5    | その他口座振替         |
| 6    | コンビニ払込票(A4)     |
| 7    | コンビニ払込票(ハガキ) |
| 8    | その他コンビニ払込票   |
| 9    | バーチャル口座         |
| 10   | その他決済手段1        |
| 11   | その他決済手段2        |
| 12   | その他決済手段3        |
| 13   | その他決済手段4        |
| 14   | その他決済手段5        |
| 15   | まるなげ口座振替       |
| 16   | まるなげバンクチェック  |
| 98   | 相殺                  |
| 101  | 貸倒                  |
| 102  | 確認済み               |
| 103  | 手数料                 |
| 104  | 請求書明細相殺           |
| 106  | 現金                   |
| 107  | 長期滞留債権           |
| 108  | 破産更生等債権         |
| 109  | 売上取消               |
| 110  | 繰越                   |
| 201  | 入金確認済み               |

### 必須

| 種別       | 説明                                                              |
| ---------- | ----------------------------------------------------------------- |
| 必須       | 全てのリクエストに対して必要                                      |
| (追加時)   | 登録・更新両方ができるAPIに存在し、追加時のリクエストに対して必要 |
| (更新時)   | 登録・更新両方ができるAPIに存在し、更新時のリクエストに対して必要 |
| ({条件})^n | {条件}時に、同じnの必須条件があるパラメータのなかでいずれかが必要 |

## 共通エラー
APIによる操作が失敗した場合、サーバは可能な限りエラーについての情報を `application/json` 形式でレスポンスします。その際、HTTPステータスは `400 Bad Request` が使われます。ログインIDやアクセスキー、接続IPに不正があった場合は、HTTPステータスは、 `401 Unauthorized` が使われます。メンテナンス中の場合は、HTTPステータスは、 `503 Service Unavailable` で返却します。一定時間に大量のリクエストを検知した場合は `429 Too Many Requests` を返却します。リクエストサイズが上限値を超えた場合 `413 Request Entity Too Large` を返却します。

エラー情報のレスポンスができないケースの例
- APIサーバへの接続エラー
- APIアクセスであると判断できないURIでのアクセス
- サーバ内部エラー
- アクセス制限によるエラー

### エラーコード

| エラーコード | 内容                                        |
| ----------- | ------------------------------------------ |
| 1           | 内部エラー                                  |
| 10          | 不明なURI                                  |
| 11          | ログインIDが不正                            |
| 12          | アクセスキーが不正                          |
| 13          | 接続IPが不正                                |
| 14          | 店舗IDが不正                                |
| 16          | ログイン失敗                                |
| 17          | 権限が不正                                  |
| 18          | 利用企業が不正                              |
| 19          | メンテナンス中                              |
| 20          | リクエスト数が不正                          |

### レスポンス例

```json
{
    "error": {
        "code": 11,
        "message": "Invalid 'user_id'."
    }
}
```

### 決済システムエラーコード

[決済システムエラーコード一覧](https://keirinomikata.zendesk.com/hc/ja/sections/200486505)

### その他の特殊なエラーコード

| エラーコード | 内容                     |
| ----------- | ------------------------ |
| 51         | まるなげ請求書編集不可     |
| 52         | まるなげオプション利用不可 |


## 推奨SSL/TLSバージョン

請求管理ロボAPIを、正常かつ快適にご利用いただくために、以下を推奨しております。

`TLS 1.2`

SSL/TLSバージョンをご確認いただき、推奨バージョンでのご利用をお願いいたします。

---

[改訂履歴](https://github.com/ROBOTPAYMENT/billing-robo-apispec/releases)
