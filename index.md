# billing-robo-apispec

## API一覧

- [請求先登録更新 v1.0/billing/bulk_upsert](/public/billing/bulk_upsert.md)
- [請求先停止削除 v1.0/billing/bulk_stop](/public/billing/bulk_stop.md)
- [請求情報登録更新 v1.0/demand/bulk_upsert](/public/demand/bulk_upsert.md)
- [請求情報停止削除 v1.0/demand/bulk_stop](/public/demand/bulk_stop.md)
- [即時決済 請求書合算 demand/bulk_register](/public/demand/bulk_register.md)
- [売上消込結果参照 v1.0/demand/search](/public/demand/search.md)
- [請求書更新 v1.0/bill/update](/public/bill/update.md)
- [請求書無効 v1.0/bill/stop](/public/bill/stop.md)
- [請求書参照2 v1.0/bill/search_list2](/public/bill/search_list2.md)
- [商品登録更新2 v1.0/goods/bulk_upsert2](/public/goods/bulk_upsert2.md)
- [商品停止削除 v1.0/goods/bulk_stop](/public/goods/bulk_stop.md)
- [口座振替依頼書発行 v1.0/billing/bulk_download_pdf](/public/billing/bulk_download_pdf.md)
- [クレジットカード登録(トークン方式) v1.0/billing_payment_method/credit_card_token](/public/billing_payment_method/credit_card_token.md)
- [請求元銀行口座登録更新 bs_bank_transfer/bulk_upsert](/public/bs_bank_transfer/bulk_upsert.md)
- [請求元銀行口座停止削除 bs_bank_transfer/bulk_stop](/public/bs_bank_transfer/bulk_stop.md)
- [請求元銀行口座パターン登録更新 bs_bank_transfer_pattern/bulk_upsert](/public/bs_bank_transfer_pattern/bulk_upsert.md)
- [請求元銀行口座パターン停止削除 bs_bank_transfer_pattern/bulk_stop](/public/bs_bank_transfer_pattern/bulk_stop.md)
- [繰越予約 bill/update_carryover](/public/bill/update_carryover.md)
- [請求書送付メール bill/send_bill_by_email](/public/bill/send_bill_by_email.md)
- [請求書送付郵送 bill/send_bill_by_mail](/public/bill/send_bill_by_mail.md)
- [請求元部署登録更新 bs_department/bulk_upsert](/public/bs_department/bulk_upsert.md)
- [請求元部署停止削除 bs_department/bulk_stop](/public/bs_department/bulk_stop.md)
- [請求元担当者登録更新 bs_owner/bulk_upsert](/public/bs_owner/bulk_upsert.md)
- [請求元担当者停止削除 bs_owner/bulk_stop](/public/bs_owner/bulk_stop.md)
- [請求書発行 demand/bulk_issue_bill_select](/public/demand/bulk_issue_bill_select.md)
- [入金参照 payment/search](/public/payment/search.md)
- [入金登録更新 payment/bulk_upsert](/public/payment/bulk_upsert.md)
- [入金無効削除 payment/bulk_stop](/public/payment/bulk_stop.md)
- [消込 clearing/exec](/public/clearing/exec.md)
- [消込結果参照 clearing/search](/public/clearing/search.md)
- [消込結果明細参照 clearing_detail/search](/public/clearing_detail/search.md)
- [消込取消 clearing/bulk_cancel](/public/clearing/bulk_cancel.md)
- [商品参照 goods/search](/public/goods/search.md)
- [決済情報参照 billing_payment_method/search](/public/billing_payment_method/search.md)
- [請求先部署参照 billing_individual/search](/public/billing_individual/search.md)
- [請求書明細参照 bill_detail/search](/public/bill_detail/search.md)
- [請求書参照 bill/search](/public/bill/search.md)
- [請求情報参照 demand/search2](/public/demand/search2.md)
- [カスタム項目登録更新 custom_field/bulk_upsert](/public/mst_custom_field/bulk_upsert.md)
- [カスタム項目削除 custom_field/bulk_stop](/public/mst_custom_field/bulk_stop.md)
- [カスタム項目参照 custom_field/search](/public/mst_custom_field/search.md)



[開発中のAPI一覧](/dev/index.md)

[非推奨のAPI一覧](/deprecated/index.md)

## Webhook一覧
- [Webhook請求書発行イベント](/webhook/webhook_bill.md)
- [Webhook郵送通知](/webhook/webhook_postmail.md)
- [Webhookクレジットカード登録状況通知](/webhook/webhook_credit_status.md)

## 注釈

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
| 98   | 相殺                   |
| 101  | 貸倒                   |
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

| エラーコード | 内容                       |
| ------------ | -------------------------- |
| 1            | 内部エラー                 |
| 10           | 不明なURI                  |
| 11           | ログインIDが不正           |
| 12           | アクセスキーが不正         |
| 13           | 接続IPが不正               |
| 14           | 店舗IDが不正               |
| 15           | 外部システムとの連携に失敗 |
| 16           | ログイン失敗               |
| 17           | 権限が不正                 |
| 18           | 利用企業が不正             |
| 19           | メンテナンス中             |
| 20           | リクエスト数が不正         |

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

| エラーコード | 内容                   |
| ------------ | ---------------------- |
| 21           | まるなげ請求書編集不可 |


## 推奨SSL/TLSバージョン

請求管理ロボAPIを、正常かつ快適にご利用いただくために、以下を推奨しております。

`TLS 1.2`

SSL/TLSバージョンをご確認いただき、推奨バージョンでのご利用をお願いいたします。

---

[改訂履歴](https://github.com/ROBOTPAYMENT/billing-robo-apispec/releases)
