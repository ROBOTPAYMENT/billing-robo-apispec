# クレジットカード登録(トークン方式 3Dセキュア利用)

### [※3Dセキュア未対応のAPI仕様書はこちらをご参照ください。](https://apispec.billing-robo.jp/public/billing_payment_method/credit_card_token_old.html)

`/api/v1.0/billing_payment_method/credit_card_token`

請求先登録更新後、レスポンスで返却された店舗オーダー番号をリクエストに加え、ROBOT PAYMENT決済システムにリクエスト送信する際に、ユーザーが入力するクレジットカード番号を、別の文字列（トークン）に置き換えて通信を行うことで、情報漏洩リスクを軽減しつつ請求先の決済情報のクレジットカード情報を登録することができます。

決済方式には、ポップアップ方式・カスタマイズ方式の2種類があります。
ポップアップ方式とは、JavaScriptでユーザーのブラウザ内に弊社で用意しました決済画面をポップアップで呼び出し、カード情報をトークンに置き換えて加盟店様へ返却しクレジットカード登録を行う方式です。
カスタマイズ方式とは、加盟店様サイト内でカード情報を入力後、JavaScriptで弊社サーバーへ通信しカード情報をトークンに置き換えて、加盟店様へ返却しクレジットカード登録を行う方式です。

**トークン決済をご利用される場合、弊社側の設定が必要となりますので、ご希望の際は下記カスタマーサポートまでご連絡をお願い致します。**

- **3Dセキュア導入に関する留意点**
  - **原則2025年3月末までに実装が必要なものとなります。**
    - **カード会社からは3Dセキュア未導入企業様につきましては、カード契約を強制解約する方針であるとのご連絡をいただいておりますため、ご対応をお願いいたします。**
  - **仕様上、検証環境での検証を行うことはできかねますのでご了承ください。**
  - **既にトークン決済をご利用されている方でも3Dセキュアを導入する場合は事前に弊社側での設定が必要となりますので、ご希望の方は下記カスタマーサポートへご連絡ください。**

## 3Dセキュア2.0 概要
3Dセキュア2.0はEMVCo（国際カードブランド6社によるカード決済の安全、促進のための団体）によって作成された3Dセキュアの改良版となります。
従来どおり本人認証はしつつ、不正利用のリスクが低ければ本人認証画面をスキップするフリクションレスフローが導入されました。
3Dセキュアの最大のメリットとして、ネット決済でも実店舗での決済と同様に本人確認をすることができるために、加盟店様で「なりすまし被害」等によるチャージバックを防ぐことができる点があげられます。

＜カスタマーサポート＞
- TEL：03-4405-0665(平日9:00-18:00)
- Mail：support@billing-robo.jp

設定が完了後、コントロールパネルより以下の設定をお願い致します。
1. 「設定」→「決済システム」
2. 「決済ゲートウェイ＆CTI決済設定」→「決済データ送信元IP」
3. 「PC用決済フォーム設定」→「決済データ送信元URL」

トークンはJavascriptの非同期通信により生成されるため、対応ブラウザは下記の通りになります。
- Microsoft Edge 最新版
- Google Chrome 最新版
- Firefox 最新版
- Safari 最新版

## アウトライン

- [トークンの取得](#トークンの取得)
  - [実装方式サンプル1(ポップアップ方式)](#実装方式サンプル1(ポップアップ方式))
  - [実装方式サンプル2(カスタマイズ方式)](#実装方式サンプル2(カスタマイズ方式))
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
        <meta charset="utf-8" />
        <script type="text/javascript"
                src="https://credit.j-payment.co.jp/gateway/js/jquery.js"></script> <!--※1-->
        <script type="text/javascript"
                src="https://credit.j-payment.co.jp/gateway/js/CPToken.js"></script><!--※2-->
        <script type="text/javascript"
                src="https://credit.j-payment.co.jp/gateway/js/EMV3DSAdapter.js"></script><!--※3-->
    </head>
    ```

   ※1. 加盟店様側でjQueryの読み込み部分を実装されている場合は以下jQuery読み込みは不要です。<br>
   ※2 トークン決済に必要なjavascriptです。<br>
   ※3 3Dセキュア2.0に必要なjavascriptです。


2. 受け取ったトークンを格納するフィールドとポップアップ表示用の `<div>` タグを設置
    (`CPToken.CardInfo()` を呼び出すと自動で `<form id="mainform">` の `<input id="tkn" ..>` に値が格納されます)

    ``` HTML
    <form id="mainform">
        <!-- input要素としてtknの追加をします -->
        <input id="tkn" name="tkn" type="hidden" value="">
        <!-- ポップアップ表示用の要素としてCARD_INPUT_FORMを追加をします -->
        <div id="CARD_INPUT_FORM"></div>
        <!-- 3Dセキュアポップアップ表示用としてEMV3DS_INPUT_FORMの追加をします -->
        <div id="EMV3DS_INPUT_FORM"></div>
        <input type="button" value="購入する" onclick="doPurchase()"/>
    </form>
    ```

3. クレジットカードトークンの作成を行う JavaScript を実装

    ``` JavaScript
    function doPurchase() {
        // トークン作成処理を呼び出します。
        CPToken.CardInfo({
            aid: '000000' // 店舗IDを設定します。
        }, execAuth);     // 3Dセキュア2.0認証用関数をコールバックにセットします。
    }
    ```

4. 3Dセキュア2.0の認証を実行を行う JavaScript を実装

    ``` JavaScript
    // コールバック関数
    function execAuth(resultCode, errMsg) {
        if (resultCode != "Success") {
            // 戻り値がSuccess以外の場合はエラーメッセージを表示
            window.alert(errMsg);
        } else {
            // 3Dセキュア2.0認証処理（商品登録なし）を呼び出します。
            ThreeDSAdapter.authenticate({ 
                tkn: $("#tkn").val(), // トークン作成後にtkn要素に値が入力されます
                aid: '000000', // 店舗IDを設定します。
                am: 0, // 「am」「tx」「sf」 の値はいずれも0固定で設定します。
                tx: 0,        
                sf: 0,
                em: 'sample@sample.com', // メールアドレス、電話番号のどちらかが必須となります。
                pn: '0300000000', // メールアドレス、電話番号のどちらかが必須となります。
            }, execPurchase);  // 決済実行用の関数をコールバックにセットします。
        }
    }
    ```

5. 決済処理の実行を行う JavaScript を実装

    ``` JavaScript
    // コールバック関数
    function execPurchase(resultCode, errMsg) {
        if (resultCode != "Success") {
            // 戻り値がSuccess以外の場合はエラーメッセージを表示
            window.alert(errMsg);
        } else {
            window.alert("PAYMENT SUCCESS!!");
            // 加盟店様サーバーに決済リクエストを実行する処理の実装をします。
            // 以下はサンプルです。
            $("#mainform").submit();
        }
    }
    ```

#### ポップアップ方式のトークン生成用関数

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


### 実装方式サンプル2(カスタマイズ方式)

1. 決済システムにて提供する JavaScript をインポート

    ``` HTML
    <head>
        <meta charset="utf-8" />
        <script type="text/javascript"
                src="https://credit.j-payment.co.jp/gateway/js/jquery.js"></script> <!--※1-->
        <script type="text/javascript"
                src="https://credit.j-payment.co.jp/gateway/js/CPToken.js"></script><!--※2-->
        <script type="text/javascript"
                src="https://credit.j-payment.co.jp/gateway/js/EMV3DSAdapter.js"></script><!--※3-->
    </head>
    ```

   ※1. 加盟店様側でjQueryの読み込み部分を実装されている場合は以下jQuery読み込みは不要です。<br>
   ※2 トークン決済に必要なjavascriptです。<br>
   ※3 3Dセキュア2.0に必要なjavascriptです。


2. 受け取ったトークンを格納するフィールドを設置
    (`CPToken.TokenCreate()` を呼び出すと自動で `<form id="mainform">` の `<input id="tkn" ..>` に値が格納されます)

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
        <!-- input要素としてtknの追加をします -->
        <input id="tkn" name="tkn" type="hidden" value="">
        <!-- 3Dセキュアポップアップ表示用としてEMV3DS_INPUT_FORMの追加をします -->
        <div id="EMV3DS_INPUT_FORM"></div>
        <input type="button" value="購入する" onclick="doPurchase()"/>
     </form>
    ```

3. クレジットカードトークンの作成を行う JavaScript を実装

    ``` JavaScript
    function doPurchase() {
        // トークン作成処理を呼び出します。
        CPToken.TokenCreate ({
            aid: '000000', // 店舗IDを設定します。
            cn: $("#cn").val(),
            ed: $("#ed_year").val() + $("#ed_month").val(),
            fn: $("#fn").val(),
            ln: $("#ln").val()
        }, execAuth); // 3Dセキュア2.0認証用関数をコールバックにセットします。
    }
    ```

4. 3Dセキュア2.0の認証を行う JavaScript を実装

    ``` JavaScript
    // コールバック関数
    function execAuth(resultCode, errMsg) {
        if (resultCode != "Success") {
            // 戻り値がSuccess以外の場合はエラーメッセージを表示します
            window.alert(errMsg);
        } else {
            // 3Dセキュア2.0認証処理（商品登録なし）を呼び出します。
            ThreeDSAdapter.authenticate({
                tkn: $("#tkn").val(), // トークン作成後にtkn要素に値が入力されます
                aid: '000000', // 店舗IDを設定します。
                am: 0, // 「am」「tx」「sf」 の値はいずれも0固定で設定します。
                tx: 0,
                sf: 0,
                em: 'sample@sample.com', // メールアドレス、電話番号のどちらかが必須となります。
                pn: '0300000000', // メールアドレス、電話番号のどちらかが必須となります。
            }, execPurchase);  // 決済実行用の関数をコールバックにセットします。
        }
   }
    ```

5. 決済処理の実行を行う JavaScript を実装

    ``` JavaScript
    // コールバック関数
    function execPurchase(resultCode, errMsg) {
        if (resultCode != "Success") {
            // 戻り値がSuccess以外の場合はエラーメッセージを表示します
            window.alert(errMsg);
        } else {
            // カード情報を消去
            $("#cn").val("");
            $("#ed_year").val("");
            $("#ed_month").val("");
            $("#fn").val("");
            $("#ln").val("");
            // 加盟店様サーバーに決済リクエストを実行する処理の実装をします。
            // 以下はサンプルです。
            $("#mainform").submit();
        }
    }
    ```
   ※ お客様の入力されたカード情報の削除をお願いします。<br>
   ※ お客様の入力されたカード情報の加盟店様サーバへ送信をしないようにお願いします。

#### カスタマイズ方式のトークン生成用関数

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
| md   | 支払い方法 <br> 「10」一括払い                      | 2        | 定型   | 必須                                         |


## クレジットカード登録

### リクエスト

- Path: `/api/v1.0/billing_payment_method/credit_card_token`
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
| code    | エラーコード     | int     |
| message | エラーメッセージ | string  |


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

[決済システムエラーコード](https://keirinomikata.zendesk.com/hc/ja/sections/200486505)

個別エラー

| エラーコード | 内容                                                 |
| ------------ | ---------------------------------------------------- |
| 3401         | 請求先コードが不正                                   |
| 3402         | 決済情報番号が不正                                   |
| 3403         | 決済情報コードが不正                                 |
| 3404         | 請求先決済番号と請求先決済コードは同時に指定できない |
| 3405         | トークン情報が不正                                   |
