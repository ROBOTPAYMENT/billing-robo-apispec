# 請求元銀行口座登録更新

`/api/v1.0/bs_bank_transfer/bulk_upsert`

複数の請求元銀行口座を登録または更新する処理。
停止中の請求元銀行口座を指定して送信すると、有効な状態に復活します。

## アウトライン

- [リクエスト](#リクエスト)
- [レスポンス](#レスポンス)
- [使用例](#使用例)
  - [リクエスト例](#リクエスト例)
  - [レスポンス例](#レスポンス例)
- [エラー](#エラー)

## リクエスト
- Method URL: `https://billing-robo.jp:10443/api/v1.0/bs_bank_transfer/bulk_upsert`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

### Parameters

| 名前                                        | 概要                                 | 桁数 | 種別                              | 必須 |
| ------------------------------------------- | ------------------------------------ | ---- | --------------------------------- | ---- |
| user_id                                     | ユーザーID（管理画面へのログインID） | 100  | [メール形式](/README.md#種別) | 必須 |
| access_key                                  | アクセスキー                         | 100  | [半角英数](/README.md#種別)   | 必須 |
| [bs_bank_transfer](#bsbanktransfer-request) | 請求元銀行口座に属するパラメータ     |      | `Array(bs_bank_transfer)`         |      |

#### bs_bank_transfer (request)

<!-- 要素が多くないものは detail, summaryタグを使わない (なくても見やすくため) -->

下記のような項目のオブジェクトを持つリスト

| 名前                          | 概要                                                 | 桁数 | 種別                                   | 必須     |
| ----------------------------- | ---------------------------------------------------- | ---- | -------------------------------------- | -------- |
| code                          | 銀行口座コード <br> ※登録後は変更できません          | 20   | [半角英数 + 記号](/README.md#種別) | 必須     |
| title                         | 銀行口座名                                           | 100  | 文字列                                 | (登録時  |
| bank_code                     | 銀行コード                                           | 4    | 数値                                   |          |
| bank_name                     | 銀行名                                               | 15   | 文字列                                 | (登録時) |
| branch_code                   | 支店コード                                           | 3    | 数値                                   |          |
| branch_name                   | 支店名                                               | 15   | 文字列                                 | (登録時) |
| bank_account_type             | 預金種目1:普通 2:当座                                | 1    | 数値                                   | (登録時) |
| bank_account_number           | 口座番号                                             | 7    | 数値                                   | (登録時) |
| bank_account_name             | 口座名義                                             | 30   | [口座名義](/README.md#種別)        | (登録時) |
| bs_department_code            | 請求元部署コード  <br> ※未所属の場合は「0000」を指定 | 20   | [半角英数 + 記号](/README.md#種別) | 必須     |
| journal_cooperation_bank_code | 会計ソフト連携用銀行コード                           | 33   | [半角英数](/README.md#種別)        |          |
| account_title_id              | 勘定科目ID <br> 普通預金：1120（固定値）             | 20   | 半角数字                               | 必須     |
| sub_account_title_code        | 補助科目コード                                       | 25   | [半角英数](/README.md#種別)        |          |

</details>


## レスポンス

- Type: `application/json`
- Encode: `UTF-8`

### Fields

| 名前                                         | 概要                                 | 型                        |
| -------------------------------------------- | ------------------------------------ | ------------------------- |
| user_id                                      | ユーザーID（管理画面へのログインID） | string                    |
| access_key                                   | アクセスキー                         | string                    |
| [bs_bank_transfer](#bsbanktransfer-response) | 請求元銀行口座に属するパラメータ     | `Array(bs_bank_transfer)` |

#### bs_bank_transfer (response)

<!-- 要素が多くないものは detail, summaryタグを使わない (なくても見やすくため) -->

下記のような項目のオブジェクトを持つリスト

| 名前                          | 概要                          | 型     |
| ----------------------------- | ----------------------------- | ------ |
| error_code                    | エラーコード※正常時はnull     | string |
| error_message                 | エラーメッセージ※正常時はnull | string |
| code                          | 銀行口座コード                | string |
| title                         | 銀行口座名                    | string |
| bank_code                     | 銀行コード                    | string |
| bank_name                     | 銀行名                        | string |
| branch_code                   | 支店コード                    | string |
| branch_name                   | 支店名                        | string |
| bank_account_type             | 預金種目                      | int    |
| bank_account_number           | 口座番号                      | string |
| bank_account_name             | 口座名義                      | string |
| bs_department_code            | 請求元部署コード              | string |
| journal_cooperation_bank_code | 会計ソフト連携用銀行コード    | string |
| account_title_id              | 勘定科目ID                    | int    |
| sub_account_title_code        | 補助科目コード                | string |


## 使用例

### リクエスト例

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    " bs_bank_transfer ": [
        {
            "code": "code",
            "title": "銀行口座名",
            "bank_code": "bank",
            "bank_name": "銀行",
            "branch_code": "branch",
            "branch_name": "支店",
            "bank_account_type": 1,
            "bank_account_number": "1234567",
            "bank_account_name": "口座名義",
            "bs_department_code": "",
            "journal_cooperation_bank_code": "",
            "account_title_id": null,
            "sub_account_title_code": ""
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
    "bs_bank_transfer": [
        {
            "error_code": null,
            "error_message": null,
            "code": "code",
            "title": "銀行口座名",
            "bank_code": "bank",
            "banl_name": "銀行",
            "branch_code": "branch",
            "branch_name": "支店",
            "bank_account_type": 1,
            "bank_account_number": "1234567",
            "bank_account_name": "口座名義",
            "bs_department_code": "",
            "journal_cooperation_bank_code": "",
            "account_title_id": null,
            "sub_account_title_code": ""
        }
    ]
}
```

## エラー

[共通エラー](/README.md#共通エラー)

個別エラー

| エラーコード | 内容                             |
| ------------ | -------------------------------- |
| 2201         | 銀行口座コードが不正             |
| 2202         | 銀行口座名が不正                 |
| 2203         | 銀行コードが不正                 |
| 2204         | 銀行名が不正                     |
| 2205         | 支店コードが不正                 |
| 2206         | 支店名が不正                     |
| 2207         | 預金種目が不正                   |
| 2208         | 口座番号が不正                   |
| 2209         | 口座名義が不正                   |
| 2210         | 請求元部署コードが不正           |
| 2211         | 会計ソフト連携用銀行コードが不正 |
| 2212         | 勘定科目IDが不正                 |
| 2213         | 更新対象の銀行口座が存在しません |