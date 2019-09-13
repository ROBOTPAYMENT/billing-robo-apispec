# 請求元銀行口座パターン登録更新

`/api/v1.0/bs_bank_transfer_pattern/bulk_upsert`

複数の請求元銀行口座パターンを登録または更新する処理。
停止中の請求元銀行口座パターンを指定して送信すると、有効な状態に復活します。

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/v1.0/bs_bank_transfer_pattern/bulk_upsert`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                                                       | 概要                                     | 桁数 | 種別                               | 必須 |
| ---------------------------------------------------------- | ---------------------------------------- | ---- | ---------------------------------- | ---- |
| user_id                                                    | ユーザーID（管理画面へのログインID）     | 100  | [半角英数\*1](/README.md#種別注釈) | 必須 |
| access_key                                                 | アクセスキー                             | 100  | [半角英数\*3](/README.md#種別注釈) | 必須 |
| [bs_bank_transfer_pattern](#bsbanktransferpattern-request) | 請求元銀行口座パターンに属するパラメータ |      | `Array(bs_bank_transfer_pattern)`  |      |

#### bs_bank_transfer_pattern (request)

<!-- 要素が多くないものは detail, summaryタグを使わない (なくても見やすくため) -->
下記のような項目のオブジェクトを持つリスト

| 名前                                                                                               | 概要                                                | 桁数 | 種別                               | 必須 |
| -------------------------------------------------------------------------------------------------- | --------------------------------------------------- | ---- | ---------------------------------- | ---- |
| code                                                                                               | 銀行口座パターンコード <br> ※登録後は変更できません | 20   | [半角英数\*4](/README.md#種別注釈) | 必須 |
| name                                                                                               | 銀行口座パターン管理名                              | 40   | 文字列                             | 必須 |
| text                                                                                               | 請求書表示                                          | 256  | 文字列                             | 必須 |
| [bs_bank_transfer_pattern.link_bs_bank_transfer](#bsbanktransferpatternlinkbsbanktransfer-request) | 請求元銀振口座パターン関連に属するパラメータ        |      | `Array(bs_bank_transfer)`          |      |

#### bs_bank_transfer_pattern.link_bs_bank_transfer (request)

<!-- 要素が多くないものは detail, summaryタグを使わない (なくても見やすくため) -->
下記のような項目のオブジェクトを持つリスト

| 名前 | 概要                                  | 桁数 | 種別                               | 必須 |
| ---- | ------------------------------------- | ---- | ---------------------------------- | ---- |
| code | 銀行口座コード※関連する銀行口座を指定 | 20   | [半角英数\*4](/README.md#種別注釈) | 必須 |


## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                                                        | 概要                                     | 型                                |
| ----------------------------------------------------------- | ---------------------------------------- | --------------------------------- |
| user_id                                                     | ユーザーID（管理画面へのログインID）     | string                            |
| access_key                                                  | アクセスキー                             | string                            |
| [bs_bank_transfer_pattern](#bsbanktransferpattern-response) | 請求元銀行口座パターンに属するパラメータ | `Array(bs_bank_transfer_pattern)` |

#### bs_bank_transfer_pattern (response)

<!-- 要素が多くないものは detail, summaryタグを使わない (なくても見やすくため) -->
下記のような項目のオブジェクトを持つリスト

| 名前                                                                                                | 概要                                         | 型                             |
| --------------------------------------------------------------------------------------------------- | -------------------------------------------- | ------------------------------ |
| error_code                                                                                          | エラーコード※正常時はnull                    | string                         |
| error_message                                                                                       | エラーメッセージ※正常時はnull                | string                         |
| code                                                                                                | 銀行口座パターンコード                       | string                         |
| name                                                                                                | 銀行口座パターン管理名                       | string                         |
| text                                                                                                | 請求書表示                                   | string                         |
| [bs_bank_transfer_pattern.link_bs_bank_transfer](#bsbanktransferpatternlinkbsbanktransfer-response) | 請求元銀振口座パターン関連に属するパラメータ | `Array(link_bs_bank_transfer)` |

#### bs_bank_transfer_pattern.link_bs_bank_transfer (response)

<!-- 要素が多くないものは detail, summaryタグを使わない (なくても見やすくため) -->
下記のような項目のオブジェクトを持つリスト

| 名前 | 概要           | 型     |
| ---- | -------------- | ------ |
| code | 銀行口座コード | string |


## 使用例

### リクエスト

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "bs_bank_transfer_pattern": [
        {
            "code": "sample",
            "name": "サンプル",
            "text": "○○銀行△△支店 普通預金 口座名義 〇〇 口座番号1234567",
            "link_bs_bank_transfer": [
                {
                    "code": "sample-1"
                },
                {
                    "code": "sample-2"
                }
            ]
        }
    ]
}
```

### レスポンス

Status: 200 OK

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "bs_bank_transfer_pattern": [
        {
            "error_code": null,
            "error_message": null,
            "code": "sample",
            "name": "サンプル",
            "text": "○○銀行△△支店 普通預金 口座名義 〇〇 口座番号1234567",
            "link_bs_bank_transfer": [
                {
                    "code": "sample-1"
                },
                {
                    "code": "sample-2"
                }
            ]
        }
    ]
}
```

## エラー

[共通エラー](/README.md#共通エラー)

個別エラー

| エラーコード | 内容                                     |
| ------------ | ---------------------------------------- |
| 2401         | 銀行口座パターンコードが不正             |
| 2402         | 銀行口座パターン管理名が空です           |
| 2403         | 請求書表示が不正です                     |
| 2404         | 更新対象の銀行口座パターンが存在しません |
| 2405         | 更新対象の請求元銀行口座が存在しません   |
