# クレジットカード登録(トークン方式)

`/api/v1.0/billing/credit_card_token`

請求先登録更新後、レスポンスで返却された店舗オーダー番号をリクエストに加え、ROBOT PAYMENT決済システムにリクエスト送信する際に、ユーザーが入力するクレジットカード番号を、別の文字列（トークン）に置き換えて通信を行うことで、情報漏洩リスクを軽減しつつ請求先の決済情報のクレジットカード情報を登録することができます。

決済方式には、ポップアップ方式・カスタマイズ方式の2種類があります。
ポップアップ方式とは、JavaScriptでユーザーのブラウザ内に弊社で用意しました決済画面をポップアップで呼び出し、カード情報をトークンに置き換えて加盟店様へ返却しクレジットカード登録を行う方式です。
カスタマイズ方式とは、加盟店様サイト内でカード情報を入力後、JavaScriptで弊社サーバーへ通信しカード情報をトークンに置き換えて、加盟店様へ返却しクレジットカード登録を行う方式です。

**トークン決済をご利用される場合、弊社側の設定が必要となりますので、ご希望の際は下記カスタマーサポートまでご連絡をお願い致します。**

＜カスタマーサポート＞
- TEL：03-5469-5783(平日9:00-18:00)
- Mail：support@billing-robo.jp

設定が完了後、コントロールパネルより以下の設定をお願い致します。
1. 「設定」→「決済システム」
2. 「決済ゲートウェイ＆CTI決済設定」→「決済データ送信元IP」
3. 「PC用決済フォーム設定」→「決済データ送信元URL」

トークンはJavascriptの非同期通信により生成されるため、対応ブラウザは下記の通りになります。
- internet Explorer(Microsoftがサポート対象としているバージョンに限る)
- Microsoft Edge 最新版
- Google Chrome 最新版
- Firefox 最新版
- Safari 最新版

## アウトライン

- [トークンの取得](#トークンの取得)
  - [実装方式サンプル1](#ポップアップ方式)
  - [実装方式サンプル2](#カスタマイズ方式)
- [クレジットカード登録](#クレジットカード登録)
  - [リクエスト](#リクエスト)
  - [レスポンス](#レスポンス)
  - [エラー](#エラー)

## トークンの取得

**※サイト内でユーザー様が入力したカード情報は必ず消去するように構築お願い致します。**

### 実装方式サンプル1(ポップアップ方式)

1. 決済システムにて提供する JavaScript をインポート

    ``` HTML
    <head>
        <script type="text/javascript"
                src="https://credit.j-payment.co.jp/gateway/js/jquery.js"></script>
        <script type="text/javascript"
                src="https://credit.j-payment.co.jp/gateway/js/CPToken.js"></script>
    </head>
    ```

2. 受け取ったトークンを格納するフィールドとポップアップ表示用の `<div>` タグを設置

    ``` HTML
    <form id="mainform">
        <input id="tkn" name="tkn" type="hidden" value="">
        <div id="CARD_INPUT_FORM" />
        <input type="button" value="購入する" onclick="doPurchase()"/>
    </form>
    ```

3. カード情報入力フォーム表示の JavaScript を実装

    ``` JavaScript
    // カード情報入力フォーム表示
    function doPurchase() {
        //CP非同期通信よりカード番号入力画面を表示する
        CPToken.CardInfo (
            {
                aid: 'xxxxxx'
            },
            execPurchase
        );
    }
    ```

4. 受け取ったトークン情報を設定し、フォームの送信処理を行う JavaScript を実装

    ``` JavaScript
    // コールバック関数
    function execPurchase(resultCode, errMsg) {
        if (resultCode != "Success") {
            // 戻り値がSuccess以外の場合はエラーメッセージを表示
            window.alert(errMsg);
        } else {
            // スクリプトからフォームをsubmit
            $("#mainform").submit();
        }
    }
    ```

### ポップアップ方式のトークン生成用関数

``` JavaScript
CPToken.CardInfo (
    { 送信するJSONパラメータ },
    コールバック関数
);
```

パラメータ

| 名前 | 概要                                                | 桁数 | 種別 | 必須 |
| ---- | --------------------------------------------------- | ---- | ---- | ---- |
| aid  | 店舗ID <br> ※契約時に発行された決済システムの店舗ID | 6    | 数値 | 必須 |


1. 決済システムにて提供する JavaScript をインポート

    ``` HTML
    <head>
        <script type="text/javascript"
                src="https://credit.j-payment.co.jp/gateway/js/jquery.js"></script>
        <script type="text/javascript"
                src="https://credit.j-payment.co.jp/gateway/js/CPToken.js"></script>
    </head>
    ```

2. 受け取ったトークンを格納するフィールドを設置

    ``` HTML
        <form id="mainform">
            カード番号：
                <input type="text" value="" name="cn" id="cn" />
            カード有効期限：
                <input type="text" value="" name="ed_year" id="ed_year" /> /
                <input type="text" value="" name="ed_month" id="ed_month" />
            カード名義人：
                <input type="text" value="" name="fn" id="fn" />
                <input type="text" value="" name="ln" id="ln" />
                <input id="tkn" name="tkn" type="hidden" value="">
            <input type="button" value="購入する" onclick="doPurchase()"/>
        </form>
    ```

3. トークンを生成するための JavaScript を実装

    ``` JavaScript
    function doPurchase() {
        // CP非同期通信よりトークンを生成する
        CPToken.TokenCreate (
            {
                aid: 'xxxxxx',
                cn: $("#cn").val(),
                ed: $("#ed_year").val() + $("#ed_month").val(),
                fn: $("#fn").val(),
                ln: $("#ln").val()
            },
            execPurchase
        );
    }
    ```

4. 受け取ったトークン情報を送信するための JavaScript を実装

    ``` JavaScript
    // コールバック関数
    function execPurchase(resultCode, errMsg) {
        if (resultCode != "Success") {
            window.alert(errMsg);
        } else {
            // カード情報を消去
            $("#cn").val("");
            $("#ed_year").val("");
            $("#ed_month").val("");
            $("#fn").val("");
            $("#ln").val("");
            //スクリプトからフォームをsubmit
            $("#mainform").submit();
        }
    }
    ```

### 実装方式サンプル1(カスタマイズ方式)

``` JavaScript
CPToken.TokenCreate (
    { 送信するJSONパラメータ },
    コールバック関数
);
```

パラメータ

| 名前 | 概要                                                | 桁数     | 種別   | 必須                                         |
| ---- | --------------------------------------------------- | -------- | ------ | -------------------------------------------- |
| aid  | 店舗ID <br> ※契約時に発行された決済システムの店舗ID | 6        | 数値   | 必須                                         |
| cn   | カード番号                                          | 16       | 数値   | 必須                                         |
| ed   | カード有効期限（YYMM形式）                          | 4        | 数値   | 必須                                         |
| fn   | カード名義（姓）                                    | 50       | 文字列 | 必須                                         |
| ln   | カード名義（名）                                    | 50       | 文字列 | 必須                                         |
| cvv  | セキュリティコード                                  | 3または4 | 数値   | (セキュリティコード利用オプションをご利用時) |
| md   | 支払い方法 <br> 「10」一括払い                      | 2        | 定型   |                                              |


## クレジットカード登録

### リクエスト

- Method URL: `https://billing-robo.jp:10443/api/v1.0/billing/credit_card_token`
- Preferred HTTP method: `POST`
- Accepted content types: `application/json`
- Encode: `UTF-8`

#### Parameters

| 名前                          | 概要                               | 桁数 | 種別                                   | 必須     |
| ----------------------------- | ---------------------------------- | ---- | -------------------------------------- | -------- |
| user_id                       | ユーザID（管理画面へのログインID） | 100  | [メール形式](../../index.md#種別)      | 必須     |
| access_key                    | アクセスキー                       | 100  | [半角英数](../../index.md#種別)        | 必須     |
| billing_code                  | 請求先コード                       | 20   | [半角英数 + 記号](../../index.md#種別) | 必須     |
| billing_payment_method_number | 決済情報番号                       | 18   | 数値                                   | (必須)^1 |
| billing_payment_method_code   | 決済情報コード                     | 20   | [半角英数 + 記号](../../index.md#種別) | (必須)^1 |
| token                         | トークン情報                       |      | [半角英数 ](../../index.md#種別)       | 必須     |

### レスポンス

- Type: `application/json`
- Encode: `UTF-8`

#### Fields

| 名前                     | 概要                      | 型       |
| ------------------------ | ------------------------- | -------- |
| [error](#error-response) | エラー <br> ※正常時はnull | `object` |

##### error (response)

| 名前    | 概要             | 型     |
| ------- | ---------------- | ------ |
| code    | エラーコード     | string |
| message | エラーメッセージ | string |


### 使用例

#### リクエスト例

```json
{
    "user_id": "sample@robotpayment.co.jp",
    "access_key": "xxxxxxxxxxxxxxxx",
    "billing_code" : "billing_code",
    "billing_payment_method_number" : 1,
    "token" : "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
}
```

#### レスポンス例

Status: 200 OK

```json
{
    "error" : {
        "code" : "ER000",
        "message" : "Credit card payment error."
    }
}
```

### エラー

[共通エラー](../../index.md#共通エラー)

[決済システムエラーコード](../../index.md#決済システムエラーコード)

個別エラー

| エラーコード | 内容                                                 |
| ------------ | ---------------------------------------------------- |
| 3401         | 請求先コードが不正                                   |
| 3402         | 決済情報番号が不正                                   |
| 3403         | 決済情報コードが不正                                 |
| 3404         | 請求先決済番号と請求先決済コードは同時に指定できない |
| 3405         | トークン情報が不正                                   |
