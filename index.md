# billing-robo-apispec

## API一覧

- [請求先登録更新(複数) v1.0/billing/bulk_upsert](/public/billing/bulk_upsert.md)
- [請求先停止削除(複数) v1.0/billing/bulk_stop](/public/billing/bulk_stop.md)
- [請求情報等登録更新(複数) v1.0/demand/bulk_upsert](/public/demand/bulk_upsert.md)
- [請求情報停止削除(複数) v1.0/demand/bulk_stop](/public/demand/bulk_stop.md)
- [即時決済 請求書合算 v1.0/demand/bulk_register](/public/demand/bulk_register.md)
- [売上消込結果参照 v1.0/demand/search](/public/demand/search.md)
- [請求書更新 v1.0/bill/update](/public/bill/update.md)
- [請求書無効 v1.0/bill/stop](/public/bill/stop.md)
- [請求書参照2 v1.0/bill/search_list2](/public/bill/search_list2.md)
- [商品登録更新(複数)2 v1.0/goods/bulk_upsert2](/public/goods/bulk_upsert2.md)
- [商品停止削除(複数) v1.0/goods/bulk_stop](/public/goods/bulk_stop.md)
- [口座振替依頼書発行 v1.0/billing/bulk_download_pdf](/public/billing/bulk_download_pdf.md)
- [クレジットカード登録(トークン決済方式)](/public/j-payment/billgate_token.md)

[非推奨のAPI一覧](/deprecated/index.md)

[開発中のAPI一覧](/dev/index.md)

## 注釈

### 種別

| 種別            | 入力可能値                                                                                |
| --------------- | ----------------------------------------------------------------------------------------- |
| メール形式      | 正規表現 : `/^([a-z0-9\+_\-]+)(\.[a-z0-9\+_\-]+)*@([a-z0-9\-]+\.)+[a-z]{2,6}$/ix`         |
| アルファベット  | 正規表現 : `^[a-zA-Z]*$`                                                                  |
| 半角英数        | 正規表現 : `^[a-zA-Z0-9]*$`                                                               |
| 半角英数 + 記号 | 正規表現 : `^[a-zA-Z0-9!"#$%&'()*,\-.\/:;<>?@\[\\\]\^_{|}~]*$`                            |
| 口座名義        | 半角英数 + 半角カタカナ + `,.()\/｢｣-` <br> 正規表現 : `^[a-zA-Z0-9ｱ-ﾝｦ-ｯ,\.\\\/()\-｢｣]*$` |
| 銀行名等        | 半角英数 + 半角カタカナ <br> 正規表現 : `^[a-zA-Z0-9ｱ-ﾝｦ-ｯ]*$`                            |

### 必須

| 種別       | 説明                                                              |
| ---------- | ----------------------------------------------------------------- |
| 必須       | 全てのリクエストに対して必要                                      |
| (追加時)   | 登録・更新両方ができるAPIに存在し、追加時のリクエストに対して必要 |
| (更新時)   | 登録・更新両方ができるAPIに存在し、更新時のリクエストに対して必要 |
| ({条件})^n | {条件}時に、同じnの必須条件があるパラメータのなかでいずれかが必要 |


## 共通エラー
APIによる操作が失敗した場合、サーバは可能な限りエラーについての情報を `application/json` 形式でレスポンスします。その際、HTTPステータスは `400 Bad Request` が使われます。ログインIDやアクセスキー、接続IPに不正があった場合は、HTTPステータスは、 `401 Unauthorized` で返却します。

エラー情報のレスポンスができないケースの例
- APIサーバへの接続エラー
- APIアクセスであると判断できないURIでのアクセス
- サーバ内部エラー

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

- [ER系](https://keirinomikata.zendesk.com/hc/ja/articles/360000077461-クレジットカードエラーコード表-ER系)
- [G系](https://keirinomikata.zendesk.com/hc/ja/articles/360000077441-%E3%82%AF%E3%83%AC%E3%82%B8%E3%83%83%E3%83%88%E3%82%AB%E3%83%BC%E3%83%89%E3%82%A8%E3%83%A9%E3%83%BC%E3%82%B3%E3%83%BC%E3%83%89%E8%A1%A8-G%E7%B3%BB)

## 推奨SSL/TLSバージョン

請求管理ロボAPIを、正常化つ快適にご利用いただくために、以下を推奨しております。

`TLS 1.2`

SSL/TLSバージョンんをご確認いただき、推奨バージョンでのご利用をお願いいたします。