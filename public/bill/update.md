# 請求書更新

`/api/v1.0/bill/update`

請求書の更新処理を実行します。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Path: `/api/v1.0/bill/update`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                  | 概要                                 | 桁数 | 種別                              | 必須 |
| --------------------- | ------------------------------------ | ---- | --------------------------------- | ---- |
| user_id               | ユーザーID（管理画面へのログインID） | 100  | [メール形式](../../index.md#種別) | 必須 |
| access_key            | アクセスキー                         | 100  | [半角英数](../../index.md#種別)   | 必須 |
| [bill](#bill-request) | 請求書に属するパラメータ             |      | `array`                           |      |

#### bill (request)

下記のような項目のオブジェクトを持つリスト

| 名前                   | 概要                                                                                                                  | 桁数 | 種別                                   | 必須                            |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------- | ---- | -------------------------------------- | ------------------------------- |
| number                 | 請求書番号                                                                                                            | 100  | [半角英数 + 記号](../../index.md#種別) | 必須                            |
| billing_code           | 請求先コード                                                                                                          | 20   | [半角英数 + 記号](../../index.md#種別) | 必須                            |
| message_column         | 通信欄                                                                                                                | 300  | 文字列                                 |                                 |
| sending_scheduled_date | 請求書送付予定日                                                                                                      | 10   | 日付                                   |                                 |
| sending_date           | 請求書送付日                                                                                                          | 10   | 日付                                   |                                 |
| transfer_deadline      | 決済期限                                                                                                              | 10   | 日付                                   |                                 |
| payment_status         | 消込ステータス <br> 0:未処理 <br> 2:確認済み <br> 3:未収 <br> 4:貸倒 <br> 5:手数料 <br> 6:現金 <br> 7:長期滞留債権 <br> 8:破産更生債権 <br> 9:売上取消 | 2    | 数値                                   |                                 |
| erasure_deposit_date   | 消込計上日                                                                                                            | 10   | 日付                                   | [(消込時)](#必須条件詳細)       |
| erasure_cancel_date    | キャンセル計上日                                                                                                      | 10   | 日付                                   | [(消込取消時)](#必須条件詳細)   |
| memo                   | メモ                                                                                                                  | 300  | 文字列                                 |                                 |

#### 必須条件詳細

| 必須条件       | 説明                                                                         |
| -------------- | ---------------------------------------------------------------------------- |
| (消込未完了時) | 消込ステータスが完了になっていない場合に必須                                 |
| (消込時)       | 消込ステータスを「未処理・未収」から「未処理・未収以外」へ変更する場合に必須 |
| (消込取消時)   | 消込ステータスを「未処理・未収以外」から「未処理・未収」へ変更する場合に必須 |

## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                   | 概要                                | 型      |
| ---------------------- | ----------------------------------- | ------- |
| user_id                | ユーザーID                          | string  |
| access_key             | アクセスキー                        | string  |
| error_code             | エラーコード <br> ※正常時はnull     | int     |
| error_message          | エラーメッセージ <br> ※正常時はnull | string  |
| [bill](#bill-response) | 請求書に属するパラメータ            | `array` |

#### bill (response)

下記のような項目のオブジェクトを持つリスト

| 名前                   | 概要             | 型     |
| ---------------------- | ---------------- | ------ |
| number                 | 請求書番号       | string |
| billing_code           | 請求先コード     | string |
| message_column         | 通信欄           | string |
| sending_scheduled_date | 請求書送付予定日 | string |
| sending_date           | 請求書送付日     | string |
| transfer_deadline      | 決済期限         | string |
| payment_status         | 消込ステータス   | int    |
| erasure_deposit_date   | 消込計上日       | string |
| erasure_cancel_date    | キャンセル計上日 | string |
| memo                   | メモ             | string |


## 使用例

### リクエスト例

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "bill": [
        {
            "number": "201705-billing-1",
            "billing_code": "billing",
            "message_column": "",
            "sending_scheduled_date": "",
            "sending_date": "",
            "transfer_deadline": "",
            "payment_status": "",
            "erasure_deposit_date": "",
            "erasure_cancel_date": "",
            "memo": ""
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
    "bill":[
        {
            "error_code": null,
            "error_message": null,
            "number": "201705-billing-1",
            "billing_code": "billing",
            "message_column": "",
            "sending_scheduled_date": "2017/05/01",
            "sending_date": null,
            "transfer_deadline": "2017/05/31",
            "payment_status": 0,
            "erasure_deposit_date": null,
            "erasure_cancel_date": null,
            "memo": null
        }
    ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                                                                 |
| ------------ | -------------------------------------------------------------------- |
| 1601         | 請求書番号が不正                                                     |
| 1602         | 請求先コードが不正                                                   |
| 1603         | 通信欄が不正                                                         |
| 1604         | 請求書送付予定日が不正                                               |
| 1605         | 請求書送付日が不正                                                   |
| 1606         | 決済期限が不正                                                       |
| 1607         | 消込ステータスが不正                                                 |
| 1608         | 消込計上日が不正 <br> - yyyy/mm/dd形式でない場合 <br> - グレゴリオ暦の日付として妥当でない場合 <br> - 必須時に入力がない場合 <br> - 消込ステータスを貸倒(4)への変更時に、請求書に紐付く売上の計上日より前の日付の場合 <br> - 消込ステータスを確認済み/貸倒/手数料/現金/長期滞留債権/破産更生債権(2,4~8)への変更時に、請求書発行日より前の日付の場合 |
| 1609         | キャンセル計上日が不正                                               |
| 1610         | 請求書メモが不正                                                     |
| 1611         | 既に締められた請求書発行日の場合、通信欄の編集できません。           |
| 1612         | 既に締められた売上が存在するため編集できません。                     |
| 1613         | 既に締められた期間への計上日となるため編集できません。               |
| 1614         | 請求書更新に失敗                                                     |
| 1615         | 請求書が存在しない                                                   |
| 1616         | 決済手段がコンビニ払込票の場合は決済期限の編集不可                   |
| 1617         | 消込ステータスが完了の場合メモ以外の任意項目の値は指定不可           |
| 1618         | 請求書が承認依頼中                                                   |
| 1619         | 消込計上日とキャンセル計上日は同時に指定できません。                 |
| 1620         | 再発行により無効になった請求書は有効にできません                     |
| 1621         | 繰越請求は変更できません。                                           |
| 1622         | 消込が繰越の繰越ステータスは変更できません。                         |
| 1623         | 請求先部署が無効または削除されている場合、決済期限は編集できません。 |

----

[TOPへ戻る](../../index.md)
