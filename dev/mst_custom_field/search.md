# カスタム項目参照API

`/api/v1.0/custom_field/search`

カスタム項目情報の参照処理を実行します

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/v1.0/custom_field/search`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

下記のような項目のオブジェクトを持つリスト

### Parameters

| 名前                                              | 概要                                                                                  | 桁数 | 種別                              | 必須 |
| ------------------------------------------------- | ------------------------------------------------------------------------------------- | ---- | -------------------------------- | ---- |
| user_id                                           | ユーザーID                                                                             | 100  | [メール形式](../../index.md#種別) | 必須 |
| access_key                                        | アクセスキー                                                                           | 100  | [半角英数](../../index.md#種別)   | 必須 |
| limit_count                                       | カスタム項目情報取得件数 <br> ※0〜200の数値を設定する。省略した場合、20が設定される          | 3    | 数値                              |     |
| page_count                                        | カスタム項目情報取得開始インデックス <br> ※0～99の数値を設定する。省略した場合、0が設定される | 2    | 数値                              |     |
| [custom_field](#custom_field-request) | カスタム項目情報に属するパラメータ                                                                |      | `object`                         |      |


#### custom_field (request)

| 名前                | 概要                                                                                                                                                                                                                                                                                                                          | 桁数 | 種別                                    | 必須                                             | 一致 |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---- | -------------------------------------- | ------------------------------------------------ | ---- |
| number              | カスタム項目番号                                                                                                                                                                                                                                                                                                                | 18   | 数値                                   |                                                  | 完全 |
| code                | カスタム項目コード                                                                                                                                                                                                                                                                                                              | 20   | 半角英数 + 記号                         |                                                  | 完全 |
| name                | カスタム項目名                                                                                                                                                                                                                                                                                                                  | 60  | 文字列                                 |                                                  | 部分 |
| target              | カスタム項目設定対象 <br> 2:請求情報・商品                                                                                                                                                                                                                                                                                       | 1  | 数値                                     |                                                  |     |
| type                | カスタム項目種別 <br> 1:テキストボックス                                                                                                                                                                                                                                                                                         | 1  | 数値                                     |                                                  |     |
| required            | カスタム項目必須 <br> 0:任意 <br> 1:必須                                                                                                                                                                                                                                                                                        | 1  | 数値                                     |                                                  |     |
| regist_date_from    | 登録日時の検索開始日                                                                                                                                                                                                                                                                                                            | 19   | 日時                                   |                                                 |     |
| regist_date_to      | 登録日時の検索終了日                                                                                                                                                                                                                                                                                                            | 19   | 日時                                   |                                                 |     |
| update_date_from    | 更新日時の検索開始日                                                                                                                                                                                                                                                                                                            | 19   | 日時                                   |                                                 |     |
| update_date_to      | 更新日時の検索終了日                                                                                                                                                                                                                                                                                                            | 19   | 日時                                   |                                                 |     |



## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                       | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;概要&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | 型      |
| ------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------- |
| user_id                                           | ユーザーID（管理画面へのログインID）                                                                                                                  | string                           |
| access_key                                        | アクセスキー                                                                                                                                        | string                           |
| limit_count                                       | カスタム項目情報取得件数 <br> ※最大件数は、リクエストで指定された「カスタム項目情報取得件数」に依存                                                           | int                              |
| page_count                                        | カスタム項目情報取得開始インデックス <br> ※取得したカスタム項目情報の開始インデックスを返却する                                                               | int                              |
| total_page_count                                  | カスタム項目情報取得開始インデックス合計 <br> ※指定された検索条件によって取得可能なカスタム項目情報の全件数／カスタム項目情報取得件数によって、算出される値を返却する | int                              |
| [custom_field](#custom_field-response)| カスタム項目情報に属するパラメータ                                                                                                                      | `array`                          |

#### custom_field (response)

| 名前                                    | 概要                                          | 型        |
| --------------------------------------- | -------------------------------------------- | --------- |
| error_code                              | エラーコード <br> ※正常時はnull               | string    |
| error_message                           | エラーメッセージ <br> ※正常時はnull            | string   |
| number                                  | カスタム項目番号                                | int     |
| code                                    | カスタム項目コード                              | string  |
| name                                    | カスタム項目名                                  | string  |
| target                                  | カスタム項目設定対象                            | int     |
| type                                    | カスタム項目種別                                | int     |
| required                                | カスタム項目必須                                | int     |
| description                             | カスタム項目説明                                | int     |
| regist_date                             | 登録日時                                      | datetime   |
| update_date                             | 更新日時                                      | datetime   |

## 使用例

### リクエスト例

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "limit_count": 20,
    "page_count": 1,
    "custom_field": {
        "number": 10,
        "code": "sample_code",
        "name": "カスタム項目名１",
        "target": 2,
        "type": 1,
        "required": 1,
        "regist_date_from": "2020/07/01 10:00:00",
        "regist_date_to": "2020/07/31 10:00:00",
        "update_date_from": "2020/07/01 10:00:00",
        "update_date_to": "2020/07/30 10:00:00"
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
    "custom_field": [
        {
            "error_code": null,
            "error_message": null,
            "number": 10,
            "code": "sample_code",
            "name": "カスタム項目名１",
            "target": 2,
            "type": 1,
            "required": 1,
            "description": "カスタム項目説明",
            "regist_date": "2020/07/01 10:11:12",
            "update_date": "2020/07/01 16:17:18"
        }
    ]
}
```

## エラー

[共通エラー](../../index.md#共通エラー)

個別エラー

| エラーコード | 内容                                     |
| ------------ | -------------------------------------   |
| 5001         | カスタム項目番号が不正                    |
| 5002         | カスタム項目コードが不正                  |
| 5003         | カスタム項目名が不正                      |
| 5004         | カスタム項目設定対象が不正                |
| 5005         | カスタム項目種別が不正                    |
| 5006         | カスタム項目必須が不正                    |
| 5007         | 登録日時の検索開始日が不正                |
| 5008         | 登録日時の検索終了日が不正                |
| 5009         | 更新日時の検索開始日が不正                |
| 5010         | 更新日時の検索終了日が不正                |
| 5011         | カスタム項目情報取得件数が不正            |
| 5012         | カスタム項目情報取得開始インデックスが不正 |
| 5013         | カスタム項目情報にはarrayを指定できません |
| 5014         | カスタム項目参照に失敗しました            |

----

[TOPへ戻る](../../index.md)