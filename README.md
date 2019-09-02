# billing-robo-apispec

## 基本

## API一覧

- [口座振替依頼書発行](/public/billing/bulk_download_pdf.md)
- [請求先登録更新(複数)](/public/billing/bulk_upsert.md)
- [請求先停止削除(複数)](/public/billing/bulk_stop.md)
- [請求情報等登録更新(複数)](/public/demand/bulk_upsert.md)
- [請求情報停止削除(複数)](/public/demand/bulk_stop.md)
- [即時決済 請求書合算](/public/demand/bulk_register.md)
- [売上消込結果参照](/public/demand/search.md)
- [請求書更新](/public/bill/update.md)
- [請求書無効](/public/bill/stop.md)
- [請求書参照2](/public/bill/search_list2.md)
- [商品登録更新(複数)2](/public/goods/bulk_upsert2.md)
- [商品停止削除(複数)](/public/goods/bulk_stop.md)
- [クレジットカード登録(トークン決済方式)](/public/j-payment/billgate_token.md)

[非推奨のAPI一覧](/deprecated/index.md)

[開発中のAPI一覧](/dev/index.md)

## 種別注釈

|種別|入力可能値|
|---|---|
|半角英数*1|`/^([a-z0-9\+_\-]+)(\.[a-z0-9\+_\-]+)*@([a-z0-9\-]+\.)+[a-z]{2,6}$/ix`|
|半角英数*2|ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz|
|半角英数*3|ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789|
|半角英数*4|ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789!"#$%&'()*+,-./:;<=>?@[\]^_\`{\|}~|
|半角英数*5|ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789ｦｧｨｩｪｫｬｭｮｯ-ｱｲｳｴｵｶｷｸｹｺｻｼｽｾｿﾀﾁﾂﾃﾄﾅﾆﾇﾈﾉﾊﾋﾂﾍﾎﾏﾐﾑﾒﾓﾔﾕﾖﾗﾘﾙﾚﾛﾜﾝｶﾞｷﾞｸﾞｹﾞｺﾞｻﾞｼﾞｽﾞｾﾞｿﾞﾀﾞﾁﾞﾂﾞﾃﾞﾄﾞﾊﾞﾋﾞﾌﾞﾍﾞﾎﾞﾊﾟﾋﾟﾌﾟﾍﾟﾎﾟ,. )(\/｢｣-|
|半角英数*6|ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789ｦｧｨｩｪｫｬｭｮｯ-ｱｲｳｴｵｶｷｸｹｺｻｼｽｾｿﾀﾁﾂﾃﾄﾅﾆﾇﾈﾉﾊﾋﾂﾍﾎﾏﾐﾑﾒﾓﾔﾕﾖﾗﾘﾙﾚﾛﾜﾝｶﾞｷﾞｸﾞｹﾞｺﾞｻﾞｼﾞｽﾞｾﾞｿﾞﾀﾞﾁﾞﾂﾞﾃﾞﾄﾞﾊﾞﾋﾞﾌﾞﾍﾞﾎﾞﾊﾟﾋﾟﾌﾟﾍﾟﾎﾟ|


## 共通エラー
APIによる操作が失敗した場合、サーバは可能な限りエラーについての情報を `application/json` 形式でレスポンスします。その際、HTTPステータスは `400 Bad Request` が使われます。ログインIDやアクセスキー、接続IPに不正があった場合は、HTTPステータスは、 `401 Unauthorized` で返却します。

エラー情報のレスポンスができないケースの例
- APIサーバへの接続エラー
- APIアクセスであると判断できないURIでのアクセス
- サーバ内部エラー

### エラーコード

| エラーコード | 内容                                 |
| ------------ | ------------------------------------ |
| 1            | 内部エラー                           |
| 10           | 不明なURI                            |
| 11           | ログインIDが不正                     |
| 12           | アクセスキーが不正                   |
| 13           | 接続IPが不正                         |
| 14           | 店舗IDが不正                         |
| 15           | 外部システムとの連携に失敗           |
| 16           | ログイン失敗                         |
| 17           | 権限が不正                           |
| 18           | 利用企業が不正                       |
| 19           | メンテナンス中                       |

### レスポンス

```
{
    "error": {
        "code": 11,
        "message": "Invalid 'user_id'."
    }
}
```

### 決済システムエラーコード一覧

[決済システムエラーコード一覧](/public/j-payment/ec.md)

## 推奨SSL/TLSバージョン

請求管理ロボAPIを、正常化つ快適にご利用いただくために、以下を推奨しております。

`TLS 1.2`

SSL/TLSバージョンんをご確認いただき、推奨バージョンでのご利用をお願いいたします。