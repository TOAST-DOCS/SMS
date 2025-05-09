<style>
    .custom-table thead {
        background-color: #FAFAFA;
    }
    
    .custom-table tbody tr {
        background-color: white;
    }
    
    .custom-table td {
        vertical-align: middle;
    }
</style>

## Notification > SMS > コンソール使用ガイド

> SMSサービスを利用するには、[コンソール > SMS > 発信番号事前登録 > 発信番号登録および名義人認証]で発信番号を事前に登録した後、使用できます(電気通信事業法)。

## 本人認証
* 電気通信事業法関連改定告示を遵守するために、SMSサービスに強化された発信番号事前登録制が適用されました。
    * 2022年3月2日以降に加入した会員に限る
* 個人会員はサービスを利用できません。(2023年12月15日時点で個人会員サービス利用ポリシーが変更されました。)
    * 2023年12月15日以降に加入した会員に限る。
* SMSサービスを利用するには、本人認証手続きを行う必要があります。本人認証は基本的に携帯電話本人認証と会員タイプ別の追加書類審査が必要です。
    * 本人認証を行っていいない場合、**発信番号事前登録タブ**以外の機能はすべて無効になります。
* 会員登録時に入力した名前と携帯電話番号が本人認証時に入力する情報と一致すれば本人認証が承認されます。
* 事業者会員が作成した組織/プロジェクトに招待されたNHN Cloud会員または事業者会員が作成した組織に招待されたIAMメンバーは、サービス利用のために本人認証を行う必要があります。
* 在職証明書は <span style="color:red;font-weight:bold">発行日が表記されており、職印が押された書類</span>のみ可能です。<br/>
  在職証明書内の住民番号後ろ6桁は <span style="color:red;font-weight:bold">必ずマスキング(伏せ字)処理</span>してください。例) 000000-0\*\*\*\*\*\*

### 会員タイプ別に必要な書類
<table class="custom-table" style="text-align: center">
  <thead>
      <tr>
          <th>会員タイプ</th>
          <th>区分</th>
          <th>内容</th>
          <th>認証方法</th>
          <th>必要書類</th>
      </tr>
  </thead>
  <tbody>
      <tr>
          <td rowspan=3>事業者</td>
          <td rowspan=2>NHN Cloud会員</td>
          <td>事業者代表が認証する場合</td>
          <td>事業者代表の携帯電話本人認証</td>
          <td>事業者登録証、在職証明書</td>
      </tr>
      <tr>
          <td>従業員(業務担当職員)が認証する場合</td>
          <td>従業員の携帯電話本人認証</td>
          <td>事業者登録証、在職証明書</td>
      </tr>
      <tr>
          <td>IAMメンバー</td>
          <td>事業者組織に招待された従業員(業務担当職員)が認証する場合</td>
          <td>従業員の携帯電話本人認証</td>
          <td>事業者登録証、在職証明書</td>
      </tr>
  </tbody>
</table>

### 本人認証の手順
![sms_01_20240104](https://static.toastoven.net/prod_sms/SMS_01_20240104.png)
1. **発信番号事前登録**タブを選択します。
2. **携帯電話本人認証および必要書類を添付する**をクリックして手続きを開始します。
3. 個人情報収集利用同意内容を確認し、同意します。
4. 携帯電話文字本人認証または簡易本人確認認証で本人認証手続きを行います。
5. 必要な書類がある場合、書類を添付して登録します。
6. 運営者の検収および承認手続きを待ちます。
7. 本人認証手続きが完了すると、アカウントに登録されたメールに承認結果が送信されます。

### 本人認証状態の説明
+ 審査中:登録した本人認証に関する認証書類を管理者が確認している状態
+ 拒否:本人認証が却下され書類の再登録が必要な状態
+ 承認:本人認証の承認が完了した状態


## 発信番号の事前登録

### 発信番号の事前登録制施行
* 電気通信事業法により、発信番号の登録時、発信番号の名義人認証が必要です。
* 名義人認証は、発信番号の種類によって認証方法と必要な書類が決定されます。

### 発信番号に名義者に基づく認証方法
<table class="custom-table" style="text-align: center">
  <thead>
      <tr>
          <th>会員タイプ</th>
          <th>発信番号の種類</th>
          <th>認証方法</th>
          <th>必要書類</th>
      </tr>
  </thead>
  <tbody>
      <tr>
          <td rowspan=4>事業者</td>
          <td>代表者番号、当社の独自番号</td>
          <td>書類認証</td>
          <td>通信サービス利用証明書</td>
      </tr>
      <tr>
          <td>従業員番号</td>
          <td>書類認証</td>
          <td>通信サービス利用証明書、従業員在職証明書</td>
      </tr>
      <tr>
          <td>他社番号</td>
          <td>書類認証</td>
          <td>通信サービス利用証明書、利用承諾書、(他社)事業登録証、
             事業者と他社間の関係確認文書(契約書など)</td>
      </tr>
      <tr>
          <td>他人番号</td>
          <td>書類認証</td>
          <td>通信サービス利用証明証、利用承諾書</td>
      </tr>
  </tbody>
</table>

#### ※参考
* 本人認証後に発信番号の登録が可能です。
* 利用承諾書の用紙はコンソールからダウンロードできます。
* 事業者と他社間の関係確認文書は業務委託契約書、本店・支店の証明書類などがあります。
* 通信サービス <span style="color:red;font-weight:bold">利用証明書はマスキング(伏せ字)処理された部分がなく、最近3か月以内に発行された書類</span>のみ可能です。
* 在職証明書は <span style="color:red;font-weight:bold">発行日が表記されており、職印が押された書類</span>のみ可能です。<br/>
  在職証明書内の住民番号後ろ6桁は <span style="color:red;font-weight:bold">必ずマスキング(伏せ字)処理</span>してください。例) 000000-0\*\*\*\*\*\*

### 発信番号通信会社別の証明書類発行方法
* 通信サービス利用証明書は、各通信会社のWebサイトからダウンロードできます。
* 通信会社によって「加入確認書、加入証明書、利用契約登録事項証明書」など名称が異なる場合があります。

* SKT [[リンク]](https://www.tworld.co.kr/web/home)
    * 発行方法： ログイン > 私の加入情報 > 利用契約証明書の照会
    * モバイルサービスサポート: 114, 080-011-6000, 1599-0011
    * 電話サービスサポート: 080-816-2000, 1600-2000

* KT [[リンク]](https://www.kt.com/)
    * 発行方法：ログイン > 加入情報 > 照会/変更 >  加入証明書を印刷
    * モバイルサービスサポート: 114, 1588-0010
    * 電話サービスサポート: 100

* LGU+ [[リンク]](https://www.lguplus.com/)
    * 発行方法：ログイン > 商品加入案内 > 加入事実確認照会
    * モバイルサービスサポート: 114, 1544-0010
    * 電話サービスサポート: 101

* 格安SIM、その他サービスプロバイダー
    * SKブロードバンドサポート: 106
    * セジョンテレコムサポート: 1688-1000, 080-889-1000
    * KCT韓国ケーブルテレコムサポート: 070-8188-0114
    * ハローモバイルサポート: 1855-1144(LGU+), 1855-1144(KT), 1855-2114(SKT)

### 発信番号の登録形式

```
* 固定電話番号：02-YYY-YYYY (市外局番を含めて登録)
* 移動通信電話番号：010-ABYY-YYYY
* 代表電話番号：15YY、16YY、18YY(番号の前に市外局番の使用禁止)
* 共通サービス識別番号：0N0系列(番号の前に市外局番使用禁止)
* 8～11桁の発信番号を入力可能
* 存在しない市外局番にメッセージ送信不可(例：070-0YYY、070-1YYY、010-0YYY、010-1YYY)
```

#### ※参考

- 携帯電話番号のうちサービスプロバイダーのオプションサービスである'発信番号盗用遮断サービス'に加入している番号には、メッセージが送信されない。(Web/システムメッセージ送信)


### 発信番号登録の手順
![sms_02_20240104](https://static.toastoven.net/prod_sms/SMS_02_20240104.png)
1. 発信番号を登録する前に、本人認証を行っていない場合は本人認証手続きを行います。
    * 2022年3月2日以前に加入した場合、本人認証なしでコンソールの利用が可能です。
2. 本人認証で登録できる場合は**発信番号の登録および携帯電話本人認証**を、そうでない場合は**発信番号の登録および書類認証**を押して登録手続きを開始します。
3. 個人情報収集利用同意内容を確認し、同意します。
4. 登録に必要な発信番号タイプ(本人番号、代表者番号、従業員番号など)を選択します。
5. 登録する発信番号を入力します。
6. 登録する番号に合った書類を添付して登録します。
7. 運営者の検収および承認を待機します。
8. 発信番号登録認証手続きが完了すると、アカウントに登録されたメールに承認結果が送信されます。

### 発信番号登録状態の説明
+ 審査中:登録した発信番号に関する認証書類を管理者が確認している状態
+ 拒否:書類認証が却下され書類の再登録が必要な状態
+ 承認:発信番号の使用が可能な状態

正常に登録が完了した発信番号は**発信番号照会**画面で確認できます。

![sms_03_20230818](https://static.toastoven.net/prod_sms/eng/SMS_03_20230818.png)

## SMS送信

* 最大文字数は保存基準です。文字切れを防ぐために最大文字数ではなく、標準規格を基準に作成します。
* 内容標準規格
    * 国内SMS 90byte(ハングル45文字、英字90文字)、国内LMS 2,000byte(ハングル1,000文字、英字2,000文字)、国内MMS 2,000byte(画像を含むハングル1,000文字、英字2,000文字) 
    * 国際SMS UCS-2 335文字、国際SMS GSM-7bit 765文字
* MMSの場合、画像添付規格に合ったファイルでアップロードする必要があります。
    * MMS最大サイズ: 1000*1000以下ファイルのみ添付可能
    * MMSサポート規格: 1個あたり300KB以下、画像の数が3個の場合、合計800KB以下/ .jpg, .jpegファイルのみ添付可能
* 国際SMSの場合、エンコードと文字数に応じてConcatenated messageで送信されます。[[ガイド]](./international-sending-policy/#_3)
    * Concatenated messageとは、国際SMS送信で送信できる文字数の制限を克服するために、韓国のLMSのように端末に一つの長い文字でつながって見えるようにするサービスです。
    * 本文文字数、エンコード、海外通信会社のサポートの有無により、Concatenated message機能が提供されます。Concatenated messageがサポートされていない場合、複数の短文メッセージで端末に受信されることがあります。
    * Concatenated messageが作成される過程で、特定のヘッダがメッセージ接続のためにバイト数（6バイト程度）を占有し、送信できる文字数が若干減少します。課金はConcatenated messageの数に応じて行われます。

* 発信番号ブロックによりメールの送信に失敗した場合は、「番号盗用メールブロックサービス」をご確認ください。[[ガイド]](./sending-policy/#fraud-number)
* 送信結果は成功したが、メールが受信できない場合は、「通信会社の迷惑メールブロックサービス」をご確認ください。[[ガイド]](./sending-policy/#spam-number)
* 予約送信の送信日時は現在から最大60日後まで設定可能です。


### 一般送信

1.テンプレートの使用可否：ユーザーが作成したテンプレートを使用して送信できます。
* 事前に作成したテンプレートがある場合は、**テンプレートの使用可否**を**使用**に選択し、**テンプレートの選択**ウィンドウで希望のテンプレートを選択します。
    * テンプレートは**テンプレート管理**タブで登録できます。
2. 送信タイプ:送信するメッセージを設定します。
    * Type： **SMS/LMS/MMSのいずれかを選択
3. 送信内容:送信する内容を設定します。
    * Type: **一般用/広告用**から選択
    * 広告用に送信する場合、情報通信網法(第50条)が適用され、080受信拒否サービスが登録されている必要があります。 また、受信拒否処理された番号に対しては、送信が自動的に失敗します。
    * 080受信拒否番号は**080受信拒否番号設定**タブで申請可能です。
4. 発信番号:メッセージを発信する番号を選択します。
    * 登録された発信番号がない場合、**+発信番号登録**ボタンをクリックして発信番号を登録します。
5.統計イベントキー：特定のイベントが発生すると、Webhook設定で定義されたURLにPOSTリクエストを作成できます。
    * 登録された統計イベントキーがない場合、**統計イベントキー管理**ボタンをクリックしてイベントキーを登録します。
6.予約送信：設定した送信日時にメッセージが送信されます。
7. 添付ファイル: **MMS**を選択した場合にのみ表示され、**添付ファイルアップロード** ボタンをクリックして添付ファイルを追加します。
8. タイトル: LMSまたはMMSを選択した場合にのみ表示され、メッセージタイトルを入力します。
    * タイトルは最大40byteまで入力できます。
    * 文字切れを防止するために40byte(ハングル20文字、英字40文字)を基準に作成します。
9. 内容:メッセージ内容を入力します。
10. 受信者情報:メッセージを受信する番号を入力します。
    * 国内番号または国コードが含まれた番号の両方を提供します。<br/> 例：01012345678, 821012345678
11. 送信: **送信** ボタンをクリックして送信します。

### 大量送信

Excel/CSVフォーマットのテンプレートファイルを使って、複数の受信番号にSMS/MMSを送信できる機能です。

#### テンプレートファイル

テンプレートファイルは**大量送信**タブ選択後、**テンプレートダウンロード**ボタンをクリックしてダウンロードできます。

![sms_08_20230818](https://static.toastoven.net/prod_sms/eng/SMS_08_20230818.png)
![sms_09_20230818](https://static.toastoven.net/prod_sms/eng/SMS_09_20230818.png)

テンプレートファイルは、CSVファイルとExcel(xls, xlsx)ファイルを提供します。
メッセージ内容にある置換キーに応じて、CSVおよびExcelファイルに列が自動的に作成されます。

ダウンロードしたテンプレートに**受信者番号**と**置換**データを入力します。

![sms_10_20230818](https://static.toastoven.net/prod_sms/eng/SMS_10_20230818.png)

> [注意] テンプレート置換機能を使用する場合、タイトルまたは内容に'##key##'のように置換キーを##の間に入力してください。

受信番号は、'+'、'-'、スペースを含めて入力できます。

#### テンプレートファイルの有効性チェック

ファイルのアップロード時に、テンプレートファイルのデータにエラーがある場合、エラーの内容が表示されます。エラーは総エラー件数と最大10件のエラー内容を表示します。

エラーの種類

- recipient\_no列が存在しない場合：recipient\_no列は受信番号を入力する列のため、入力は必須です。
- ファイルに入力されたデータがない場合
- 受信番号が無効なフォーマットで入力された場合
- 受信番号または置換データの入力がない場合

#### 送信予約の選択(確認後に進行/即時送信)

送信情報の入力と、大量送信ファイルのアップロードを行った後に送信を進行するには、**送信予約**ボタンをクリックします。送信予約の際、受信対象番号を確認後に進行する**確認後に予約送信**と**予約送信**を選択できます。

- 確認後に予約送信：受信番号と送信内容を**大量SMS送信照会**タブで確認後に送信を進行できます。確認後に送信を行わない場合は、送信が行われませんので注意してください。
- 予約送信：受信番号と送信内容を確認せずに送信を進行します。送信結果は**大量SMS送信照会**タブで確認できます。

#### 分割送信

分割送信を使用すると、**分割回数**と**送信間隔**を設定してメッセージ分割して送信できます。

<span id='tag-send'></span>
### タグ送信

タグの条件に合ったUIDで送信できる機能です。

![sms_11_20230818](https://static.toastoven.net/prod_sms/eng/SMS_11_20230818.png)
![sms_12_20230818](https://static.toastoven.net/prod_sms/eng/SMS_12_20230818.png)
![sms_13_20230818](https://static.toastoven.net/prod_sms/eng/SMS_13_20230818.png)

タグ登録は**タグ管理**タブで、UIDと電話番号の保存は**UID管理**タブで行えます。

## 080受信拒否設定

080受信拒否サービスは、広告メッセージ送信時、受信者に受信拒否を提供するサービスです。
広告性情報を送信する時、受信者が受信拒否や受信同意の撤回を無料で行えるように<span style="color:red">無料受信拒否方法を必ず記載</span>する必要があります。

### 加入

**080受信拒否設定**タブに移動すると、加入画面を確認できます。
**080受信拒否番号の追加申請**ボタンを押すと、業者名を入力できます。

* 入力する業者名は、080受信拒否番号に電話をかける時に案内される業者名です。

![sms_14_20230818](https://static.toastoven.net/prod_sms/eng/SMS_14_20230818.png)

### 登録予約

加入申請が完了すると、登録予約状態に変更されます。
080受信拒否サービスの開通には、営業日基準で3～4日かかり、開通が完了すると使用できます。

### 登録完了

開通が完了すると、使用開始日時と状態を確認できます。
**080受信拒否サービス登録予約、使用中状態ではSMSサービスの利用を終了できません。**解約後にサービスの利用を終了できます。
解約したい時は、**解約**ボタンを押すと解約できます。

### 広告性メッセージの送信

1. 080受信拒否サービスを使用中の状態でのみ、広告性メッセージを送信できます。
2. 送信タイプを**広告用**に変更すると、受信拒否番号選択ウィンドウが表示されます。
3. **選択適用**ボタンを押すと、広告性必須文言を追加できます。
4. 広告性送信時、メッセージ本文に必ず広告性必須文言が含まれている必要があり、ルールは以下の通りです。
    - 冒頭の文言: `(広告)`
    - 最後の文言： 無料受信拒否 {080受信拒否番号}`または `無料受信拒否 {080受信拒否番号}` (この文言には空白が含まれる場合があります。)

例
```
(広告)

[無料受信拒否]080XXXXXXX
```
```
(広告)

無料拒否080XXXXXXX
```

![sms_15_20230818](https://static.toastoven.net/prod_sms/eng/SMS_15_20230818.png)
![sms_16_20230818](https://static.toastoven.net/prod_sms/eng/SMS_16_20230818.png)

### 受信拒否対象者の照会

受信拒否リクエスト日時をオプションで、受信拒否をリクエストした対象者を、下にあるパネルで照会できます。

## SMS照会

### SMSリクエスト別照会

各項目を条件に照会できます。
(リクエストIDまたは発信日時は必須値です。)

![sms_17_20230818](https://static.toastoven.net/prod_sms/eng/SMS_17_20230818.png)

* **リクエストID**または**受信番号**をクリックすると、詳細表示ポップアップが表示されます。
* 登録/発信/受信日時の検索は最大1か月以内で検索可能です。
* 全体画面に表示されたデータはExcelファイルでダウンロードできます。
* リクエスト状態で送信リクエストの状態を確認できます。
* 送信結果で送信処理の成功/失敗を確認できます。

### SMS予約送信の照会

予約送信されたリストを照会できます。

![sms_18_20230818](https://static.toastoven.net/prod_sms/eng/SMS_18_20230818.png)

* **リクエストID**または**受信番号**をクリックすると、詳細表示ポップアップが表示されます。
* 登録/発信/受信日時の検索は最大1か月以内で検索可能です。
* リクエスト状態で送信リクエストの状態を確認できます。
* 予約待機状態の場合、そのリストを選択して予約をキャンセルできます。

### 大量SMS送信の照会

送信タイプ別に大量の送信件を照会できます。

![sms_19_20230818](https://static.toastoven.net/prod_sms/eng/SMS_19_20230818.png)

* 照会:上の照会フォームで、大量SMS送信予約件を照会できます。照会件のリスト行を選択すると、下にある照会フォームで受信番号と送信情報(送信内容、送信結果)を確認できます。
* 送信/キャンセル:大量アップロード送信の予約時に「受信対象者の確認後に予約送信」を選択した場合、'送信準備完了'状態の予約件を選択し、
* **送信/キャンセル**ボタンをクリックして送信またはキャンセルができます。予約送信の場合、自動的に現在時間に送信処理されます。
* 送信失敗の確認:進行状態が'送信完了'状態の予約件で、一部の送信リクエストが失敗した件は、送信失敗件数を確認できます。**失敗件数**ボタンをクリックすると、失敗した受信番号と送信内容が表示されます。

#### 大量SMS送信の進行状態

- 待機：テンプレートファイルデータの読み込み作業を進行する前の状態です。
- 送信準備：テンプレートファイルデータをロード中の状態です。
- 送信準備完了：テンプレートファイルデータをすべてロードしてSMS送信準備が完了した状態です。予約件(リストの行)を選択すると、下にあるリストで受信番号と送信内容を確認できます。
- 送信待機：SMS送信作業を進行する前の状態です。
- 送信中：SMS送信が進行中の状態です。予約件(リストの行)を選択すると、送信進行率を確認できます。
- 送信完了：SMS送信リクエストが正常に完了した状態です。
- 送信失敗：送信進行中に送信エラーが発生した場合です。
- 送信取消：ユーザーが送信を取り消した状態です。

#### 受信者別のSMS送信の照会

大量送信件(リストの行)を選択すると、下にあるリストで受信番号別の送信内容と送信結果を照会できます。

![sms_20_20230818](https://static.toastoven.net/prod_sms/eng/SMS_20_20230818.png)

**詳細表示**ボタンをクリックすると、詳細な送信内容を確認できます。

![sms_21_20230818](https://static.toastoven.net/prod_sms/eng/SMS_21_20230818.png)

置換されたデータを確認できます。

### タグSMS送信の照会

#### 送信リクエスト別照会

タグ送信リクエスト件を照会できます。クリックすると、下にある受信者別照会で受信者別の照会ができます。

![sms_22_20230818](https://static.toastoven.net/prod_sms/eng/SMS_22_20230818.png)

#### 受信者別の送信照会

1つのリクエストで送信した受信者リストを照会できます。

![sms_23_20230818](https://static.toastoven.net/prod_sms/eng/SMS_23_20230818.png)

**詳細表示**ボタンをクリックすると、詳細な送信内容を確認できます。

![sms_24_20230818](https://static.toastoven.net/prod_sms/eng/SMS_24_20230818.png)

## テンプレートの管理

### カテゴリーの追加

**カテゴリー追加**ボタンをクリックすると、カテゴリーの追加ができます。

![sms_25,26_20230818](https://static.toastoven.net/prod_sms/eng/SMS_25,26_20230818.png)

カテゴリーを選択した状態で**カテゴリー追加**ボタンをクリックする必要があります。

### カテゴリーの修正

**カテゴリー修正**ボタンをクリックすると、カテゴリーの修正ができます。

![sms_27_1_20230818](https://static.toastoven.net/prod_sms/eng/SMS_27_1_20230818.png)

カテゴリーを選択した状態で**カテゴリー修正**ボタンをクリックする必要があります。

### テンプレートの追加

**テンプレート追加**ボタンをクリックすると、テンプレートを追加できます。

![sms_27_20230818](https://static.toastoven.net/prod_sms/eng/SMS_27_20230818.png)

1. **テンプレート追加**ボタンをクリックします。
2. ご希望の**送信タイプ、テンプレート情報を入力**します。
3. 認証番号、注文番号、クーポンコード、ポイントなどを置換して入力するには**タイトルまたは内容に'##key##'のような置換キーを##でまとめて入力**します。
4. すべての内容の入力後、必ずカテゴリーを選択した状態で**テンプレート追加**をクリックします。

### テンプレートの修正

テンプレートを選択し、修正できます。

![sms_28_20230818](https://static.toastoven.net/prod_sms/eng/SMS_28_20230818.png)

1. 修正が必要なテンプレートを選択します。
2. **送信タイプ、テンプレート情報、内容**を修正します。
3. 修正完了後、必ずカテゴリーを選択した状態で**テンプレート修正**をクリックします。

<span id='uid-manage'></span>
## UIDの管理

UIDおよび携帯電話番号の登録や削除ができます。タグとUID用語の意味は[参考](./console-guide/#tag-uid)で確認してください。

![sms_29,30_20230818](https://static.toastoven.net/prod_sms/eng/SMS_29,30_20230818.png)

**UID登録**ボタンをクリックします。CSV形式のテンプレートで大量のUIDを追加できます。

![sms_31_20230818](https://static.toastoven.net/prod_sms/eng/SMS_31_20230818.png)

uid,phoneNumber形式で入力します。<br/>
ex) sms_uuid1,01012345678

![sms_32_20230818](https://static.toastoven.net/prod_sms/eng/SMS_32_20230818.png)
テンプレート作成後、アップロード時に確認された番号の数を確認できます。

<span id='tag-manage'></span>
## タグの管理

登録されたUIDへのタグ付けや削除ができるページです。タグとUID用語の意味は[参考](./console-guide/#tag-uid)で確認してください。

![sms_32_20230818](https://static.toastoven.net/prod_sms/eng/SMS_33_20230818.png)

**タグ登録**ボタンをクリックしてタグを登録します。

![sms_34_20230818](https://static.toastoven.net/prod_sms/eng/SMS_34_20230818.png)
タグにUIDを登録します。(UIDタブで登録したUIDを登録します。)

## Webフック管理
指定したイベントが発生した場合、URLを指定してWebフックイベントを受け取れます。

![sms_35_20230818](https://static.toastoven.net/prod_sms/eng/SMS_35_20230818.png)

1. 登録するイベントタイプを選択します。
2. Webフックに送信されるデータを受信できるURLアドレスを記載します。
3. 登録するWebフック署名を入力します。(必須ではない)
4. 検証を受けた後、**追加**をクリックしてWebフックを登録します。

登録が完了したWebフックは**Webフック登録リスト**で確認可能です。

## 送信設定
### 国際SMS送信設定
* 国際SMS送信機能を利用する前に必ず[[国際SMS送信ポリシー]](./international-sending-policy)を確認してください。
* 国際SMS送信機能を使用しない場合は、未使用に設定して、国際SMS量ポンピング現象による事故を防止します。
* 送信許可国の管理
    * 最初の使用設定時に指定された主要国のみ送信できるように設定されます。**送信許可国設定 > 許可国を選択**ボタンを通じて、国別の送信可否を管理します。
* 自動ブロック月間制限件数及びしきい値通知
    * **自動ブロック月間制限件数**は、1,000件が基本で、最大10,000件まで適用可能です。
    * **自動ブロック月間制限件数**最大値である10,000件を超える調整が必要な場合は、**1万件以上希望**ボタンからお問い合わせください。
    * **自動ブロック月間制限件数**は補助機能であり、検出がリアルタイムで反映されません。補助機能の一部誤差について、NHN Cloudは責任を負いかねますので、注意してご利用ください。
    * **月間制限件数しきい値通知**を**使用**に設定すると、**自動ブロック月間制限件数**で設定した値の70%、100%に達するとプロジェクトメンバー全員に通知メールが送信されます。
* コンバージョン率に基づく送信ブロックと通知
    * コンバージョン率に基づく送信ブロックと通知**を**使用**に設定すると、国別に機能が有効になり、ブロックが発生するとプロジェクトメンバー全員に通知メールが送信されます。
    * コンバージョン率に基づく送信ブロック国は、**コンバージョン率に基づく送信ブロック国設定 > ブロック国設定 > 許可国を選択**ボタンで設定した国に対してのみ適用されます。
    * コンバージョン率によってブロックされた国の横に**Blocked**ボタンが表示されます。このボタンをクリックすると、ブロックを解除できます。
    * コンバージョン率に基づくブロックルール
        * **コンバージョン率**は、コンバージョン率に基づくブロックが発生する基準しきい値設定です。
            - 計算されたコンバージョン率がコンバージョン率しきい値設定値以下の場合、ブロックされます。
            - コンバージョン率は1%から100%まで指定できます。
        * **最小件数**は、コンバージョン率に基づくブロックが動作するためのコンバージョン率収集リクエストの送信件数のしきい値設定です。
            - 最小件数以上のコンバージョン率収集リクエストを送信することでコンバージョン率に基づくブロックが動作します。
            - 最小件数は1件から10,000件まで指定できます。
        * **時間範囲**はコンバージョン率を計算する時間範囲設定です。 
            - 送信要求時間から、その時間範囲内のコンバージョン率収集リクエスト送信件数、コンバージョン件数を基準にコンバージョン率を計算します。
            - 時間範囲は1時間から168時間(7日)まで指定できます。
            
> [注意]
世界的に国際SMSのアビューズ事例が増加しています。
月間制限件数と送信国の指定については、必ず必要な分だけ設定することを推奨します。
NHN Cloudは、アビューズにより送信された国際SMSに対して一切の責任を負いません。 

### 代替文字設定
* 送信リクエストの本文/タイトルに送信不可能な文字が含まれている場合、送信可能な文字に変換されるよう設定できます。
* 代替文字設定を「使用」に設定すると、送信不可能な文字が'？'に変換されて表示されます。

### 重複送信設定
* 重複したメッセージを送信しないように設定できます。
* 重複送信遮断設定をした場合、設定された時間(単位：分)中は、同じリクエストは送信失敗処理されます。
* 遮断できる最長時間は1時間です。
* 重複判断基準は次のとおりです。
    * メッセージタイプ(SMS/LMS/MMS/AUTH)、送信タイプ(一般/大量/タグ)、発信番号、受信番号、タイトル、本文、添付ファイル

### 広告送信制限設定
* 広告メッセージの送信時間を制限できます。
* 設定された時間内は広告送信が行われません。
    * 広告制限開始設定が可能な時間: 18:00～21:00
    * 広告制限終了設定が可能な時間: 08:00～12:00
* 未送信メッセージの設定方式によって失敗/再送信が可能です。

### バックアップ設定
* メッセージ保管期間ポリシーに従い、180日が経過した送信履歴データをバックアップできます。
* メッセージのバックアップ有無、ファイル拡張子、ファイルをアップロードするストレージ情報を入力すると、該当ストレージにバックアップ日時が含まれたファイルが作成されます。

## 統計イベントキー設定
イベントキーを登録して該当キーを送信する場合、統計イベントキーごとに統計データを収集できます。/
統計イベントキーの用語の意味は[参考](./console-guide/#tag-uid)で確認してください。

![sms_36_20230818](https://static.toastoven.net/prod_sms/eng/SMS_36_20230818.png)

1. **イベントキー登録**をクリックしてデータ収集期間を設定します。
2. 統計イベントキーの名前と詳細説明を入力します。
3. **保存**をクリックすると、イベントキーが登録されます。

データ収集期間が終了すると無効化状態になり、それ以降のデータは蓄積されません。
**データ収集期間の終了時点は、有効の場合にのみ修正が可能です。**

## 統計

### 統計照会

* 送信リクエスト期間、統計イベントキー、テンプレートなどのタイプ別に統計を照会できます。
* 送信リクエスト、成功、失敗などの送信ステータスをグラフと表で確認できます。

#### 統計分類
* メッセージ(リクエスト時間):送信リクエスト時間基準で収集された統計です。
* 次の時間基準で統計が収集されます。
    * リクエスト:送信リクエスト時間
    * 送信:送信リクエスト時間、数が増加する時点は通信事業者(ベンダー)に送信リクエストした時間です。
    * 送信失敗:送信リクエスト時間、数が増加する時点は失敗リクエストが発生した時間です。
    * 受信:送信リクエスト時間、数が増加する時点は実際の端末受信時間です。
    * 結果待機中:送信と同時に発生して数が増加、受信すると数が減少
* メッセージ(イベント時間):イベント発生時間を基準に収集された統計です。
* 次の時間基準で統計が収集されます。
    * リクエスト:送信リクエスト時間
    * 送信:通信事業者(ベンダー)に送信リクエストした時間
    * 送信失敗:失敗レスポンスが発生した時間
    * 受信: 実際の端末受信時間
* 国際送信:イベント発生時間基準で収集された統計です。
* 次の時間基準で統計が収集されます。
    * リクエスト:送信リクエスト時間
    * 送信:通信事業者(ベンダー)に送信リクエストした時間
    * 送信失敗:失敗レスポンスが発生した時間
    * 受信:送信されたメッセージの受信時間  
    * 送信件数：単件またはConcatenated message(接続)機能を介して送信されたメッセージの受信時間
    * 転換待機：転換率収集リクエスト送信件のメッセージ受信時間
    * 転換完了：転換率収集リクエスト送信件の転換が完了した時間


## 参考

<span id='tag-uid'></span>
### タグとUID

#### サービス用語
| 用語      | 説明                                  |
| ------------ | ---------------------------------------- |
| タグ(tag)      | UIDを分類するシステム。<br>UIDに複数のタグをつけてユーザーが簡単にUID情報を検索し、使用できます。 |
| UID          | ユーザーを区分するID(識別子)。<br>1つのUIDに複数の連絡先を登録し、送信に使用できます。 |
| 連絡先(contact) | 連絡をするために決めておいた宛先。<br>NotificationではPush、Email、SMSの3個のサービスで連絡先を登録できます。<br>Pushはトークン、 Emailはメールアドレス、 SMSは電話番号が連絡先になります。 |

#### タグを使用して送信
受信者情報である電話番号の代わりに、タグを選択してメッセージを送信できる機能です。

1. UIDを登録します。
    - **UID管理**タブでUIDと、1つまたは複数の電話番号を登録します。
    - 詳細は[UID管理](./console-guide/#uid-manage)を参照してください。
2. タグを登録します。
    - **タグ管理**タブで、タグを登録します。
    - 詳細は[タグ管理](./console-guide/#tag-manage)を参照してください。
3. タグにUIDを登録します。
    - **タグ管理**タブで、登録したタグにUIDを登録します。
4. タグを選択し、メッセージを送信します。
    - **一般送信**タブで、メールアドレスの代わりに**タグ送信**を選択してタグを登録します。
    - メールは、タグに登録されているUIDの電話番号に送信されます。
    - 詳細は[タグを使用したメッセージ送信](./console-guide/#tag-send)を参照してください。

#### 他のサービスのタグ機能との関係
* 同じプロジェクトでPushまたはSMSサービスを使用している場合、Emailで使用しているタグとUID情報を再登録せずに、一緒に使用できます。
* 各サービスのコンソールから、同じUIDに他の連絡先情報を追加できます。

### 統計イベントキーと統計
#### サービス用語
| 用語         | 説明                                     |
| ------------ | ---------------------------------------- |
| 統計イベントキー | 統計を特定の単位でまとめて確認したい時に使用するイベントキーです。 |
| statsId | 統計イベントキーの固有ID値です。 APIで呼び出す時、この値を主に利用します。 |

#### メッセージ送信時、特定の単位で統計を抽出したい場合
1. **統計イベントキー管理**タブで統計イベントキーを登録します。 APIを使用して送信する場合、統計ID(statsId)をこの画面で取得する必要があります。
2. Console画面またはAPIからメッセージを転送する時、統計イベントキーを一緒に送信する必要があります。

    2-1. Console画面の場合
    - **SMS送信**タブでメッセージ送信時、統計イベントキーを選択します。
    - メッセージ情報を全て入力し、**送信**ボタンをクリックします。
    - 一定時間が経過した後、**統計**タブで統計情報を確認できます。

    2-2. APIで送信する場合
    - **統計イベントキー管理**タブで取得したstatsIdをメッセージ転送パラメータへ一緒に渡します。
    - 一定時間が経過すると、**統計**タブで統計情報を確認できます。

### データ保管期間
* データ保管ポリシーに基づき、過去180日の送信履歴を保管します。
* サービスに利用された添付ファイルは7日間のみ保管され、その後は削除されて照会できません。
* ただし、テンプレートに登録された添付ファイルおよび証明書類(通信サービス利用証明書)はサービスを利用している間保管されます。
* 統計データは、過去90日の情報を保管します。
