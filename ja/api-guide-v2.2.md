## Notification > SMS > API v2.2ガイド

## v2.2 API紹介

### v2.1からの変更事項

1. テンプレートで送信をリクエストする時、リクエストパラメータの優先順位が高くなるように修正しました。
    - 例)テンプレートで送信をリクエストする時、リクエストパラメータにタイトル、本文、発信番号、添付ファイルが含まれている場合、テンプレートに保存されたデータを使用しないように修正
2. メッセージ送信時、タイトル/本文の有効性チェックを強化
    * メッセージ送信時のタイトル/本文の文字数制限が下記のように変更されます。
    * SMS(本文：最大255文字)、LMS/MMS(タイトル：最大120文字、本文：最大4000文字)

### [APIドメイン]

| 環境   | 	ドメイン                            |
|------|----------------------------------|
| Real | 	https://api-sms.cloud.toast.com |

<span id="precautions"></span>

### [注意事項]

* サポートする文字数は下記の通りです。
* 最大サポート文字数は保存基準で、文字切れ防止のため、標準規格で作成してください。
* エンコードはEUC-KR基準で送信され、サポートしていない顔文字は送信に失敗します。

| 分類      | 最大サポート  | 標準規格                          |
|---------|---------|-------------------------------|
| SMS本文   | 255文字   | 90バイト(全角45文字、半角90文字)          |
| MMSタイトル | 120文字   | 40バイト(全角20文字、半角40文字)          |
| MMS本文   | 4,000文字 | 2,000バイト(全角1,000文字、半角2,000文字) |

## 短文SMS

### 短文SMSの送信

#### リクエスト

[URL]

```
POST  /sms/v2.2/appKeys/{appKey}/sender/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Request body]

```json
{
  "templateId": "TemplateId",
  "body": "本文",
  "sendNo": "15446859",
  "requestDate": "2018-08-10 10:00",
  "senderGroupingKey": "SenderGroupingKey",
  "recipientList": [
    {
      "recipientNo": "01000000000",
      "countryCode": "82",
      "internationalRecipientNo": "821000000000",
      "templateParameter": {
        "key": "value"
      },
      "recipientGroupingKey": "recipientGroupingKey"
    }
  ],
  "userId": "UserId"
}
```

| 値                                         | 	タイプ    | 最大                                                             | 	必須 | 	説明                                                                |
|-------------------------------------------|---------|----------------------------------------------------------------|-----|--------------------------------------------------------------------|
| templateId                                | 	String | 50                                                             | 	X  | 	送信テンプレートID                                                        |
| body                                      | 	String | 標準：90バイト、最大：255文字(EUC-KR基準) [[注意事項](./api-guide/#precautions)] | 	O  | 	本文内容                                                              |
| sendNo                                    | 	String | 13                                                             | 	O  | 	発信番号                                                              |
| requestDate                               | String  | -                                                                  | X   | 予約日時(yyyy-MM-dd HH:mm)<br/>現在から最大60日後まで設定可                                                   |
| senderGroupingKey                         | String  | 100                                                            | X   | 発信者グループキー                                                          |
| recipientList[].recipientNo               | String  | 20                                                             | 	O  | 	受信番号<br/>countryCodeと組み合わせて使用可能<br/>最大1,000人                      |
| recipientList[].countryCode               | 	String | 8                                                              | 	X  | 	国番号[デフォルト値：82(韓国)]                                                |
| recipientList[].internationalRecipientNo  | String  | 20                                                             | X   | 国番号を含む受信番号<br/>例)821012345678<br/>recipientNoがある場合、この値は無視される。<br/> |
| recipientList[].templateParameter         | 	Object | -                                                              | 	X  | 	テンプレートパラメータ(テンプレートID入力時)                                          |
| recipientList[].templateParameter.{key}   | String  | -                                                              | 	X  | 	置換キー(##key##)                                                     |
| recipientList[].templateParameter.{value} | 	Object | -                                                              | 	X  | 	置換キーにマッピングされるValue値                                               |
| recipientList[].recipientGroupingKey      | String  | 100                                                            | X   | 受信者グループキー                                                          |
| userId                                    | 	String | 	100                                                           | X   | 送信セパレータex)admin,system                                             |

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "data": {
      "requestId": "20180810100630ReZQ6KZzAH0",
      "statusCode": "2",
      "senderGroupingKey": "SenderGroupingKey",
      "sendResultList": [
        {
          "recipientNo": "01000000000",
          "resultCode": 0,
          "resultMessage": "SUCCESS",
          "recipientSeq": 1,
          "recipientGroupingKey": "RecipientGroupingKey"
        }
      ]
    }
  }
}
```

| 値                                               | 	タイプ     | 	説明                                          |
|-------------------------------------------------|----------|----------------------------------------------|
| header.isSuccessful                             | 	Boolean | 	成否                                          |
| header.resultCode                               | 	Integer | 	失敗コード                                       |
| header.resultMessage                            | 	String  | 	失敗メッセージ                                     |
| body.data.requestId                             | 	String  | 	リクエストID                                     |
| body.data.statusCode                            | 	String  | 	リクエストステータスコード(1：リクエスト中、2：リクエスト完了、3：リクエスト失敗) |
| body.data.senderGroupingKey                     | 	String  | 	発信者グループキー                                   |
| body.data.sendResultList[].recipientNo          | String   | 受信番号                                         |
| body.data.sendResultList[].resultCode           | Integer  | 結果コード                                        |
| body.data.sendResultList[].resultMessage        | String   | 結果メッセージ                                      |
| body.data.sendResultList[].recipientSeq         | Integer  | 受信者シーケンス(mtPr)                               |
| body.data.sendResultList[].recipientGroupingKey | String   | 受信者グループキー                                    |

#### 短文SMSの送信例(一般国内受信番号)

| Http metho | URL                                                                  |
|------------|----------------------------------------------------------------------|
| POST       | https://api-sms.cloud.toast.com/sms/v2.2/appKeys/{appKey}/sender/sms |

[Request body]

```json
{
  "body": "本文",
  "sendNo": "15446859",
  "senderGroupingKey": "SenderGroupingKey",
  "recipientList": [
    {
      "recipientNo": "01000000000",
      "recipientGroupingKey": "recipientGroupingKey"
    },
    {
      "recipientNo": "01000000001",
      "recipientGroupingKey": "RecipientGroupingKey2"
    }
  ]
}
```

[Response]

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "data": {
      "requestId": "20180810100630ReZQ6KZzAH0",
      "statusCode": "2",
      "senderGroupingKey": "SenderGroupingKey",
      "sendResultList": [
        {
          "recipientNo": "01000000000",
          "resultCode": 0,
          "resultMessage": "SUCCESS",
          "recipientSeq": 1,
          "recipientGroupingKey": "RecipientGroupingKey"
        },
        {
          "recipientNo": "01000000001",
          "resultCode": 0,
          "resultMessage": "SUCCESS",
          "recipientSeq": 2,
          "recipientGroupingKey": "RecipientGroupingKey2"
        }
      ]
    }
  }
}
```

[curl]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-sms.cloud.toast.com/sms/v2.2/appKeys/{appKey}/sender/sms -d '{"body": "{本文内容}","sendNo": "15446859","senderGroupingKey":"SenderGroupingKey","recipientList":[{"recipientNo": "01000000000","recipientGroupingKey":"RecipientGroupingKey"},{"recipientNo": "01000000002","recipientGroupingKey":"RecipientGroupingKey2"}]}'
```

#### 短文SMS送信例(国コードが含まれている受信番号)

| Http metho | URL                                                                  |
|------------|----------------------------------------------------------------------|
| POST       | https://api-sms.cloud.toast.com/sms/v2.2/appKeys/{appKey}/sender/sms |

[Request body]

```json
{
  "body": "本文",
  "sendNo": "15446859",
  "senderGroupingKey": "SenderGroupingKey",
  "recipientList": [
    {
      "internationalRecipientNo": "821000000000",
      "recipientGroupingKey": "recipientGroupingKey"
    }
  ],
  "userId": ""
}
```

[Response]

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "data": {
      "requestId": "20180810100630ReZQ6KZzAH0",
      "statusCode": "2",
      "senderGroupingKey": "SenderGroupingKey",
      "sendResultList": [
        {
          "recipientNo": "01000000000",
          "resultCode": 0,
          "resultMessage": "SUCCESS",
          "recipientSeq": 1,
          "recipientGroupingKey": "RecipientGroupingKey"
        }
      ]
    }
  }
}
```

[curl]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-sms.cloud.toast.com/sms/v2.2/appKeys/{appKey}/sender/sms -d '{"body": "本文","sendNo": "15446859","recipientList": [{"internationalRecipientNo": "821000000000"}]}'
```

### 短文SMS送信リストの照会

#### リクエスト

[URL]

```
GET  /sms/v2.2/appKeys/{appKey}/sender/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Query parameter]

* requestIdまたはstartRequestDate + endRequestDateまたはstartCreateDate + endCreateDateは必須です。
* 登録日時と発信日時を同時に照会する場合、発信日時は無視されます。

| 値                    | 	タイプ     | 	最大  | 必須     | 	説明                                                      |
|----------------------|----------|------|--------|---------------------------------------------------------|
| requestId            | 	String  | 25   | 	必須    | 	リクエストID                                                 |
| startRequestDate     | 	String  | -    | 	必須    | 	送信日の開始値(yyyy-MM-dd HH:mm:ss)                            |
| endRequestDate       | 	String  | -    | 	必須    | 	送信日の終了値(yyyy-MM-dd HH:mm:ss)                            |
| startCreateDate      | 	String  | -    | 	必須    | 	送信日の開始値(yyyy-MM-dd HH:mm:ss)                            |
| endCreateDate        | 	String  | -    | 	必須    | 	送信日の終了値(yyyy-MM-dd HH:mm:ss)                            |
| startResultDate      | 	String  | -    | 	オプション | 	受信日の開始値(yyyy-MM-dd HH:mm:ss)                            |
| endResultDate        | 	String  | -    | 	オプション | 	受信日の終了値(yyyy-MM-dd HH:mm:ss)                            |
| sendNo               | 	String  | 13   | 	オプション | 	発信番号                                                    |
| recipientNo          | 	String  | 20   | 	オプション | 	受信番号                                                    |
| templateId           | 	String  | 50   | 	オプション | 	テンプレート番号                                                |
| msgStatus            | 	String  | 1    | 	オプション | 	メッセージステータスコード(0：失敗、 1：リクエスト、 2：処理中、 3：成功、 4：予約キャンセル、 5：重複失敗、 6:失敗(広告制限)、 再送信待機(広告制限)) |
| resultCode           | 	String  | 10     | 	オプション | 	受信結果コード[[照会コード表](./error-code/#_2)]                |
| subResultCode        | 	String  | 10   | 	オプション | 	受信結果詳細コード[[照会コード表](./error-code/#_3)]                   |
| senderGroupingKey    | 	String  | 100  | 	オプション | 	送信者グループキー                                               |
| recipientGroupingKey | 	String  | 100  | 	オプション | 	受信者グループキー                                               |
| receiverRegion       | 	String  | -    | 	オプション | 	国内/国際 (DOMESTIC: 国内, INTERNATIONAL: 国際) |
| countryCode          | 	String  | -    | 	オプション | 	国コード [[送信可能国](./international-sending-policy/#_5)] |
| pageNum              | 	Integer | -    | 	オプション | 	ページ番号(デフォルト値：1)                                         |
| pageSize             | 	Integer | 1000 | 	オプション | 	照会数(デフォルト値：15)                                          |

#### レスポンス

```json
{
  "header": {
    "resultCode": 0,
    "resultMessage": "SUCCESS",
    "isSuccessful": true
  },
  "body": {
    "pageNum": 1,
    "pageSize": 15,
    "totalCount": 1,
    "data": [
      {
        "requestId": "20180810100630ReZQ6KZzAH0",
        "requestDate": "2018-08-10 10:06:30.0",
        "resultDate": "2018-08-10 10:06:42.0",
        "templateId": "TemplateId",
        "templateName": "テンプレート名",
        "categoryId": 0,
        "categoryName": "カテゴリー名",
        "body": "短文テスト",
        "sendNo": "15446859",
        "countryCode": "82",
        "recipientNo": "01000000000",
        "msgStatus": "3",
        "msgStatusName": "成功",
        "resultCode": "1000",
        "resultCodeName": "成功",
        "telecomCode": 10001,
        "telecomCodeName": "SKT",
        "mtPr": "1",
        "sendType": "0",
        "userId": "tester",
        "adYn": "N",
        "resultMessage": "",
        "senderGroupingKey": "SenderGroupingKey",
        "recipientGroupingKey": "RecipientGroupingKey"
      }
    ]
  }
}
```

| 値                                | 	タイプ     | 	説明                                         |
|----------------------------------|----------|---------------------------------------------|
| header.isSuccessful              | 	Boolean | 	成否                                         |
| header.resultCode                | 	Integer | 	失敗コード                                      |
| header.resultMessage             | 	String  | 	失敗メッセージ                                    |
| body.pageNum                     | 	Integer | 	現在のページ番号                                   |
| body.pageSize                    | 	Integer | 	照会されたデータ数                                  |
| body.totalCount                  | 	Integer | 	総データ数                                      |
| body.data[].requestId            | 	String  | 	リクエストID                                    |
| body.data[].requestDate          | 	String  | 	発信日時                                       |
| body.data[].resultDate           | 	String  | 	受信日時                                       |
| body.data[].templateId           | 	String  | 	テンプレートID                                   |
| body.data[].templateName         | 	String  | 	テンプレート名                                    |
| body.data[].categoryId           | 	Integer | 	カテゴリーID                                    |
| body.data[].categoryName         | 	String  | 	カテゴリー名                                     |
| body.data[].body                 | 	String  | 	本文内容                                       |
| body.data[].sendNo               | 	String  | 	発信番号                                       |
| body.data[].countryCode          | 	String  | 	国番号                                        |
| body.data[].recipientNo          | 	String  | 	受信番号                                       |
| body.data[].msgStatus            | 	String  | 	メッセージステータスコード                              |
| body.data[].msgStatusName        | 	String  | 	メッセージステータスコード名                             |
| body.data[].resultCode           | 	String  | 	受信結果コード[[受信結果コード表](./error-code/#emma-v3)] |
| body.data[].resultCodeName       | 	String  | 	受信結果コード名                                   |
| body.data[].telecomCode          | 	Integer | 	サービスプロバイダーコード                              |
| body.data[].telecomCodeName      | 	String  | 	サービスプロバイダー名                                |
| body.data[].mtPr                 | 	Integer | 	送信詳細ID(詳細照会時は必須)                           |
| body.data[].sendType             | 	String  | 	送信タイプ(0：Sms、1：Mms、2：Auth)                  |
| body.data[].userId               | 	String  | 	送信リクエストID                                  |
| body.data[].adYn                 | 	String  | 	広告かどうか                                     |
| body.data[].senderGroupingKey    | 	String  | 	発信者グループキー                                  |
| body.data[].recipientGroupingKey | 	String  | 	受信者グループキー                                  |

### 短文SMS送信の単一照会

#### リクエスト

[URL]

```
GET  /sms/v2.2/appKeys/{appKey}/sender/sms/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値         | 	タイプ    | 	説明            |
|-----------|---------|----------------|
| appKey    | 	String | 	固有のアプリケーションキー |
| requestId | 	String | 	リクエストID       |

[Query parameter]

| 値    | 	タイプ     | 	必須 | 	説明     |
|------|----------|-----|---------|
| mtPr | 	Integer | 	必須 | 	送信詳細ID |

#### レスポンス

```json
{
  "header": {
    "resultCode": 0,
    "resultMessage": "SUCCESS",
    "isSuccessful": true
  },
  "body": {
    "data": {
      "requestId": "20180810100630ReZQ6KZzAH0",
      "requestDate": "2018-08-10 10:06:30.0",
      "resultDate": "2018-08-10 10:06:42.0",
      "templateId": "TemplateId",
      "templateName": "テンプレート名",
      "categoryId": 0,
      "categoryName": "カテゴリー名",
      "body": "本文",
      "sendNo": "15446859",
      "countryCode": "82",
      "recipientNo": "01000000000",
      "msgStatus": "3",
      "msgStatusName": "成功",
      "resultCode": "1000",
      "resultCodeName": "成功",
      "telecomCode": 10001,
      "telecomCodeName": "SKT",
      "mtPr": "1",
      "sendType": "0",
      "userId": "tester",
      "adYn": "N",
      "resultMessage": "",
      "senderGroupingKey": "SenderGroupingKey",
      "recipientGroupingKey": "recipientGroupingKey"
    }
  }
}
```

| 値                              | 	タイプ     | 	説明                                         |
|--------------------------------|----------|---------------------------------------------|
| header.isSuccessful            | 	Boolean | 	成否                                         |
| header.resultCode              | 	Integer | 	失敗コード                                      |
| header.resultMessage           | 	String  | 	失敗メッセージ                                    |
| body.data.requestId            | 	String  | 	リクエストID                                    |
| body.data.requestDate          | 	String  | 	発信日時                                       |
| body.data.resultDate           | 	String  | 	受信日時                                       |
| body.data.templateId           | 	String  | 	テンプレートID                                   |
| body.data.templateName         | 	String  | 	テンプレート名                                    |
| body.data.categoryId           | 	Integer | 	カテゴリーID                                    |
| body.data.categoryName         | 	String  | 	カテゴリー名                                     |
| body.data.body                 | 	String  | 	本文内容                                       |
| body.data.sendNo               | 	String  | 	発信番号                                       |
| body.data.countryCode          | 	String  | 	国番号                                        |
| body.data.recipientNo          | 	String  | 	受信番号                                       |
| body.data.msgStatus            | 	String  | 	メッセージステータスコード                              |
| body.data.msgStatusName        | 	String  | 	メッセージステータスコード名                             |
| body.data.resultCode           | 	String  | 	受信結果コード[[受信結果コード表](./error-code/#emma-v3)] |
| body.data.resultCodeName       | 	String  | 	受信結果コード名                                   |
| body.data.telecomCode          | 	Integer | 	サービスプロバイダーコード                              |
| body.data.telecomCodeName      | 	String  | 	サービスプロバイダー名                                |
| body.data.mtPr                 | 	Integer | 	送信詳細ID(詳細照会時は必須)                           |
| body.data.sendType             | 	String  | 	送信タイプ(0：Sms、1：Mms、2：Auth)                  |
| body.data.userId               | 	String  | 	送信リクエストID                                  |
| body.data.adYn                 | 	String  | 	広告かどうか                                     |
| body.data.senderGroupingKey    | 	String  | 	発信者グループキー                                  |
| body.data.recipientGroupingKey | 	String  | 	受信者グループキー                                  |

## 長文MMS

### 長文MMS送信(添付ファイルは含まない)

※ MMSは韓国外への送信はできません。

#### リクエスト

[URL]

```
POST  /sms/v2.2/appKeys/{appKey}/sender/mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Request body]

```json
{
  "templateId": "TemplateId",
  "title": "Title",
  "body": "Body",
  "sendNo": "15446859",
  "requestDate": "2018-08-10 10:00",
  "senderGroupingKey": "SenderGroupingKey",
  "recipientList": [
    {
      "recipientNo": "01000000000",
      "countryCode": "82",
      "internationalRecipientNo": "821000000000",
      "templateParameter": {
        "key": "value"
      },
      "recipientGroupingKey": "recipientGroupingKey"
    }
  ],
  "userId": "UserId"
}
```

| 値                                         | 	タイプ    | 最大                | 	必須 | 	説明                                                                |
|-------------------------------------------|---------|-------------------|-----|--------------------------------------------------------------------|
| templateId                                | 	String | 50                | 	X  | 	送信テンプレートID                                                        |
| title                                     | 	String | 40バイト(EUC-KR基準)   | 	O  | 	タイトル                                                              |
| body                                      | 	String | 2000バイト(EUC-KR基準) | 	O  | 	本文                                                                |
| sendNo                                    | 	String | 13                | 	O  | 	発信番号                                                              |
| requestDate                               | String  | -     | X   | 予約日時(yyyy-MM-dd HH:mm)<br/>現在から最大60日後まで設定可                                                  |
| senderGroupingKey                         | String  | 100               | X   | 発信者グループキー                                                          |
| recipientList[].recipientNo               | String  | 20                | 	O  | 	受信番号<br/>countryCodeと組み合わせて使用可能                                   |
| recipientList[].countryCode               | String  | 8                 | 	X  | 	国番号[デフォルト値：82(韓国)]                                                |
| recipientList[].internationalRecipientNo  | String  | 20                | X   | 国番号を含む受信番号<br/>例)821012345678<br/>recipientNoがある場合、この値は無視される。<br/> |
| recipientList[].templateParameter         | 	Object | -                 | 	X  | 	テンプレートパラメータ(テンプレートID入力時)                                          |
| recipientList[].templateParameter.{key}   | 	String | -                 | 	X  | 	置換キー(##key##)                                                     |
| recipientList[].templateParameter.{value} | 	Object | -                 | 	X  | 	置換キーにマッピングされるValue値                                               |
| recipientList[].recipientGroupingKey      | String  | 1000              | X   | 受信者グループキー                                                          |
| userId                                    | 	String | 100               | 	X  | 送信セパレータex)admin,system                                             |

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "data": {
      "requestId": "20180810100630ReZQ6KZzAH0",
      "statusCode": "2",
      "senderGroupingKey": "SenderGroupingKey",
      "sendResultList": [
        {
          "recipientNo": "01000000000",
          "resultCode": 0,
          "resultMessage": "SUCCESS",
          "recipientSeq": 1,
          "recipientGroupingKey": "RecipientGroupingKey"
        }
      ]
    }
  }
}
```

| 値                                               | 	タイプ     | 	説明                                          |
|-------------------------------------------------|----------|----------------------------------------------|
| header.isSuccessful                             | 	Boolean | 	成否                                          |
| header.resultCode                               | 	Integer | 	失敗コード                                       |
| header.resultMessage                            | 	String  | 	失敗メッセージ                                     |
| body.data.requestId                             | 	String  | 	リクエストID                                     |
| body.data.statusCode                            | 	String  | 	リクエストステータスコード(1：リクエスト中、2：リクエスト完了、3：リクエスト失敗) |
| body.data.senderGroupingKey                     | 	String  | 	発信者グループキー                                   |
| body.data.sendResultList[].recipientNo          | String   | 受信番号                                         |
| body.data.sendResultList[].resultCode           | Integer  | 結果コード                                        |
| body.data.sendResultList[].resultMessage        | String   | 結果メッセージ                                      |
| body.data.sendResultList[].recipientSeq         | Integer  | 受信者シーケンス(mtPr)                               |
| body.data.sendResultList[].recipientGroupingKey | String   | 受信者グループキー                                    |

#### 長文MMSの送信例

| Http metho | URL                                                                  |
|------------|----------------------------------------------------------------------|
| POST       | https://api-sms.cloud.toast.com/sms/v2.2/appKeys/{appKey}/sender/mms |

[Request body]

```json
{
  "title": "タイトル",
  "body": "本文",
  "sendNo": "15446859",
  "senderGroupingKey": "SenderGroupingKey",
  "recipientList": [
    {
      "recipientNo": "01000000000",
      "recipientGroupingKey": "recipientGroupingKey"
    },
    {
      "recipientNo": "01000000001",
      "recipientGroupingKey": "RecipientGroupingKey2"
    }
  ]
}
```

[Response]

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "data": {
      "requestId": "20180810100630ReZQ6KZzAH0",
      "statusCode": "2",
      "senderGroupingKey": "SenderGroupingKey",
      "sendResultList": [
        {
          "recipientNo": "01000000000",
          "resultCode": 0,
          "resultMessage": "SUCCESS",
          "recipientSeq": 1,
          "recipientGroupingKey": "RecipientGroupingKey"
        },
        {
          "recipientNo": "01000000001",
          "resultCode": 0,
          "resultMessage": "SUCCESS",
          "recipientSeq": 2,
          "recipientGroupingKey": "RecipientGroupingKey2"
        }
      ]
    }
  }
}
```

[curl]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-sms.cloud.toast.com/sms/v2.2/appKeys/{appKey}/sender/mms -d '{"title": "{タイトル}","body": "{本文内容}","sendNo": "{発信番号}","recipientList": [{"recipientNo": "{受信番号}","templateParameter": { }}],"userId": ""}'
```

### 長文MMSの送信(添付ファイル含む)

#### 添付ファイルの送信例

| Http method | URL                                                                  |
|-------------|----------------------------------------------------------------------|
| POST        | https://api-sms.cloud.toast.com/sms/v2.2/appKeys/{appKey}/sender/mms |

[Request body]

```json
{
  "title": "タイトル",
  "body": "本文",
  "sendNo": "15446859",
  "senderGroupingKey": "SenderGrouping",
  "attachFileIdList": [
0
  ],
  "recipientList": [
    {
      "recipientNo": "01010000000",
      "recipientGroupingKey": "RecipientGroupingKey"
    }
  ]
}
```

[Response]

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "data": {
      "requestId": "20180810100630ReZQ6KZzAH0",
      "statusCode": "2",
      "senderGroupingKey": "SenderGrouping",
      "sendResultList": [
        {
          "recipientNo": "01012341234",
          "resultCode": 0,
          "resultMessage": "SUCCESS",
          "recipientSeq": 1,
          "recipientGroupingKey": "RecipientGroupingKey"
        }
      ]
    }
  }
}
```

##### 説明

- 添付ファイル(フィールド名：attachFileIdList)を含む長文MMSを送信するためには、事前に添付ファイルのアップロードを行う必要があります。<br>
- [[添付ファイルアップロード](./api-guide/#binaryUpload)]</a> ガイドを参照してください。
- 付イメージ制限事項
    - サポートコーデック：.jpg、.jpeg
    - 添付イメージ数：3個以下
    - 添付イメージサイズ： 1個当り300KB以下。ただし、添付したイメージの数が3個の場合は合計800KB以下。
    - 添付イメージの解像度： 1000 x 1000以下

### 長文MMS送信リストの照会

#### リクエスト

[URL]

```
GET  /sms/v2.2/appKeys/{appKey}/sender/mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Query parameter]

* requestIdまたはstartRequestDate + endRequestDateまたはstartCreateDate + endCreateDateは必須です。
* 登録日時と発信日時を同時に照会する場合、発信日時は無視されます。

| 値                    | 	タイプ     | 最大   | 	必須    | 	説明                                                      |
|----------------------|----------|------|--------|---------------------------------------------------------|
| requestId            | 	String  | 25   | 	必須    | 	リクエストID                                                 |
| startRequestDate     | 	String  | -    | 	必須    | 	送信日の開始値(yyyy-MM-dd HH:mm:ss)                            |
| endRequestDate       | 	String  | -    | 	必須    | 	送信日の終了値(yyyy-MM-dd HH:mm:ss)                            |
| startCreateDate      | 	String  | -    | 	必須    | 	送信日の開始値(yyyy-MM-dd HH:mm:ss)                            |
| endCreateDate        | 	String  | -    | 	必須    | 	送信日の終了値(yyyy-MM-dd HH:mm:ss)                            |
| startResultDate      | 	String  | -    | 	オプション | 	受信日の開始値(yyyy-MM-dd HH:mm:ss)                            |
| endResultDate        | 	String  | -    | 	オプション | 	受信日の終了値(yyyy-MM-dd HH:mm:ss)                            |
| sendNo               | 	String  | 13   | 	オプション | 	発信番号                                                    |
| recipientNo          | 	String  | 20   | 	オプション | 	受信番号                                                    |
| templateId           | 	String  | 50   | 	オプション | 	テンプレート番号                                                |
| msgStatus            | 	String  | 1    | 	オプション | 	メッセージステータスコード(0：失敗、 1：リクエスト、 2：処理中、 3：成功、 4：予約キャンセル、 5：重複失敗、 6:失敗(広告制限)、 再送信待機(広告制限)) |
| resultCode           | 	String  | 10   | 	オプション | 	受信結果コード[[照会コード表](./error-code/#_2)]                     |
| subResultCode        | 	String  | 10   | 	オプション | 	受信結果詳細コード[[照会コード表](./error-code/#_3)]                   |
| senderGroupingKey    | 	String  | 100  | 	オプション | 	送信者グループキー                                               |
| recipientGroupingKey | 	String  | 100  | 	オプション | 	受信者グループキー                                               |
| receiverRegion       | 	String  | -    | 	オプション | 	国内/国際 (DOMESTIC: 国内, INTERNATIONAL: 国際) |
| countryCode          | 	String  | -    | 	オプション | 	国コード [[送信可能国](./international-sending-policy/#_5)] |
| pageNum              | 	Integer | -    | 	オプション | 	ページ番号(デフォルト値：1)                                         |
| pageSize             | 	Integer | 1000 | 	オプション | 	照会数(デフォルト値：15)                                          |

#### レスポンス

```json
{
  "header": {
    "resultCode": 0,
    "resultMessage": "SUCCESS",
    "isSuccessful": true
  },
  "body": {
    "pageNum": 1,
    "pageSize": 15,
    "totalCount": 1,
    "data": [
      {
        "requestId": "20230918160734BWt01iOWZm2",
        "requestDate": "2023-09-18 16:07:34.0",
        "resultDate": null,
        "templateId": null,
        "templateName": null,
        "categoryId": null,
        "categoryName": null,
        "title": "Title",
        "body": "本文",
        "sendNo": "15771234",
        "countryCode": "82",
        "recipientNo": "01012341234",
        "msgStatus": "3",
        "msgStatusName": "成功",
        "resultCode": null,
        "resultCodeName": null,
        "telecomCode": null,
        "telecomCodeName": null,
        "mtPr": "1",
        "sendType": "1",
        "userId": null,
        "adYn": "N",
        "attachFileList": [
          {
            "fileId": 535191,
            "serviceId": null,
            "attachType": null,
            "templateId": null,
            "filePath": "/temporary/71191/toast-mt-2023-09-18/1607/535191/",
            "fileName": "attachment.jpg",
            "saveFileName": "20230918KKkqQC0.jpg",
            "fileSize": null,
            "createDate": null,
            "createUser": null,
            "updateDate": null,
            "updateUser": null,
            "uploadType": "TEMPORARY",
            "existFileName": "20230918KKkqQC0.jpg"
          }
        ],
        "resultMessage": "SUCCESS",
        "senderGroupingKey": "SenderGroupingKey",
        "recipientGroupingKey": "recipientGroupingKey"
      }
    ]
  }
}
```

| 値                                          | 	タイプ     | 	説明                                         |
|--------------------------------------------|----------|---------------------------------------------|
| header.isSuccessful                        | 	Boolean | 	成否                                         |
| header.resultCode                          | 	Integer | 	失敗コード                                      |
| header.resultMessage                       | 	String  | 	失敗メッセージ                                    |
| body                                       | 	Object  | 	本文領域                                       |
| body.pageNum                               | 	Integer | 	現在のページ番号                                   |
| body.pageSize                              | 	Integer | 	照会されたデータ数                                  |
| body.totalCount                            | 	Integer | 	総データ数                                      |
| body.data[].requestId                      | 	String  | 	リクエストID                                    |
| body.data[].requestDate                    | 	String  | 	発信日時                                       |
| body.data[].resultDate                     | 	String  | 	受信日時                                       |
| body.data[].templateId                     | 	String  | 	テンプレートID                                   |
| body.data[].templateName                   | 	String  | 	テンプレート名                                    |
| body.data[].categoryId                     | 	Integer | 	カテゴリーID                                    |
| body.data[].categoryName                   | 	String  | 	カテゴリー名                                     |
| body.data[].body                           | 	String  | 	本文内容                                       |
| body.data[].sendNo                         | 	String  | 	発信番号                                       |
| body.data[].countryCode                    | 	String  | 	国番号                                        |
| body.data[].recipientNo                    | 	String  | 	受信番号                                       |
| body.data[].msgStatus                      | 	String  | 	メッセージステータスコード                              |
| body.data[].msgStatusName                  | 	String  | 	メッセージステータスコード名                             |
| body.data[].resultCode                     | 	String  | 	受信結果コード[[受信結果コード表](./error-code/#emma-v3)] |
| body.data[].resultCodeName                 | 	String  | 	受信結果コード名                                   |
| body.data[].telecomCode                    | 	Integer | 	サービスプロバイダーコード                              |
| body.data[].telecomCodeName                | 	String  | 	サービスプロバイダー名                                |
| body.data[].mtPr                           | 	Integer | 	送信詳細ID(詳細照会時は必須)                           |
| body.data[].sendType                       | 	String  | 	送信タイプ(0：Sms、1：Mms、2：Auth)                  |
| body.data[].userId                         | 	String  | 	送信リクエストID                                  |
| body.data[].adYn                           | 	String  | 	広告かどうか                                     |
| body.data[].attachFileList[].fileId        | 	Integer | 	ファイルID                                     |
| body.data[].attachFileList[].serviceId     | 	Integer | 	サービスID                                     |
| body.data[].attachFileList[].attachType    | 	Integer | 	添付ファイルタイプ                                  |
| body.data[].attachFileList[].templateId    | 	String  | 	テンプレートID                                   |
| body.data[].attachFileList[].filePath      | 	String  | 	ファイル保存パス(内部用)                              |
| body.data[].attachFileList[].fileName      | 	String  | 	ファイル名                                      |
| body.data[].attachFileList[].saveFileName  | 	String  | 	保存された添付ファイルの名前                             |
| body.data[].attachFileList[].fileSize      | 	Long    | 	添付ファイルの大きさ                                 |
| body.data[].attachFileList[].createDate    | 	String  | 	作成日時                                       |
| body.data[].attachFileList[].createUser    | 	String  | 	作成者                                        |
| body.data[].attachFileList[].updateDate    | 	String  | 	修正日時                                       |
| body.data[].attachFileList[].updateUser    | 	String  | 	修正者                                        |
| body.data[].attachFileList[].uploadType    | 	String  | 	アップロードタイプ                                  |
| body.data[].attachFileList[].existFileName | 	String  | 	保存された添付ファイルの名前                             |
| body.data[].senderGroupingKey              | 	String  | 	発信者グループキー                                  |
| body.data[].recipientGroupingKey           | 	String  | 	受信者グループキー                                  |

### 長文MMS送信の単一照会

#### リクエスト

[URL]

```
GET  /sms/v2.2/appKeys/{appKey}/sender/mms/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値         | 	タイプ    | 	説明            |
|-----------|---------|----------------|
| appKey    | 	String | 	固有のアプリケーションキー |
| requestId | 	String | 	リクエストID       |

[Query parameter]

| 値    | 	タイプ     | 	必須 | 	説明     |
|------|----------|-----|---------|
| mtPr | 	Integer | 	必須 | 	送信詳細ID |

#### レスポンス

```json
{
  "header": {
    "resultCode": 0,
    "resultMessage": "SUCCESS",
    "isSuccessful": true
  },
  "body": {
    "data": {
      "requestId": "20230918160734BWt01iOWZm2",
      "requestDate": "2023-09-18 16:07:34.0",
      "resultDate": "2023-09-18 16:07:36.0",
      "templateId": null,
      "templateName": null,
      "categoryId": null,
      "categoryName": null,
      "title": "Title",
      "body": "本文",
      "sendNo": "15771234",
      "countryCode": "82",
      "recipientNo": "01012341234",
      "msgStatus": "3",
      "msgStatusName": "成功",
      "resultCode": "1000",
      "resultCodeName": "成功",
      "telecomCode": 10003,
      "telecomCodeName": "LGU",
      "mtPr": "1",
      "sendType": "1",
      "userId": null,
      "adYn": "N",
      "attachFileList": [
        {
          "fileId": 535191,
          "serviceId": null,
          "attachType": null,
          "templateId": null,
          "filePath": "/temporary/71191/toast-mt-2023-09-18/1607/535191/",
          "fileName": "attachment.jpg",
          "saveFileName": "20230918KKkqQC0.jpg",
          "fileSize": null,
          "createDate": null,
          "createUser": null,
          "updateDate": null,
          "updateUser": null,
          "uploadType": "TEMPORARY",
          "existFileName": "20230918KKkqQC0.jpg"
        }
      ],
      "resultMessage": "SUCCESS",
      "senderGroupingKey": "SenderGroupingKey",
      "recipientGroupingKey": "recipientGroupingKey"
    }
  }
}
```

| 値                                          | 	タイプ     | 	説明                                         |
|--------------------------------------------|----------|---------------------------------------------|
| header.isSuccessful                        | 	Boolean | 	成否                                         |
| header.resultCode                          | 	Integer | 	失敗コード                                      |
| header.resultMessage                       | 	String  | 	失敗メッセージ                                    |
| body                                       | 	Object  | 	本文領域                                       |
| body.pageNum                               | 	Integer | 	現在のページ番号                                   |
| body.pageSize                              | 	Integer | 	照会されたデータ数                                  |
| body.totalCount                            | 	Integer | 	総データ数                                      |
| body.data[].requestId                      | 	String  | 	リクエストID                                    |
| body.data[].requestDate                    | 	String  | 	発信日時                                       |
| body.data[].resultDate                     | 	String  | 	受信日時                                       |
| body.data[].templateId                     | 	String  | 	テンプレートID                                   |
| body.data[].templateName                   | 	String  | 	テンプレート名                                    |
| body.data[].categoryId                     | 	Integer | 	カテゴリーID                                    |
| body.data[].categoryName                   | 	String  | 	カテゴリー名                                     |
| body.data[].body                           | 	String  | 	本文内容                                       |
| body.data[].sendNo                         | 	String  | 	発信番号                                       |
| body.data[].countryCode                    | 	String  | 	国番号                                        |
| body.data[].recipientNo                    | 	String  | 	受信番号                                       |
| body.data[].msgStatus                      | 	String  | 	メッセージステータスコード                              |
| body.data[].msgStatusName                  | 	String  | 	メッセージステータスコード名                             |
| body.data[].resultCode                     | 	String  | 	受信結果コード[[受信結果コード表](./error-code/#emma-v3)] |
| body.data[].resultCodeName                 | 	String  | 	受信結果コード名                                   |
| body.data[].telecomCode                    | 	Integer | 	サービスプロバイダーコード                              |
| body.data[].telecomCodeName                | 	String  | 	サービスプロバイダー名                                |
| body.data[].mtPr                           | 	Integer | 	送信詳細ID(詳細照会時は必須)                           |
| body.data[].sendType                       | 	String  | 	送信タイプ(0：Sms、1：Mms、2：Auth)                  |
| body.data[].userId                         | 	String  | 	送信リクエストID                                  |
| body.data[].adYn                           | 	String  | 	広告かどうか                                     |
| body.data[].attachFileList[].fileId        | 	Integer | 	ファイルID                                     |
| body.data[].attachFileList[].serviceId     | 	Integer | 	サービスID                                     |
| body.data[].attachFileList[].attachType    | 	Integer | 	添付ファイルタイプ                                  |
| body.data[].attachFileList[].templateId    | 	String  | 	テンプレートID                                   |
| body.data[].attachFileList[].filePath      | 	String  | 	ファイル保存パス(内部用)                              |
| body.data[].attachFileList[].fileName      | 	String  | 	ファイル名                                      |
| body.data[].attachFileList[].saveFileName  | 	String  | 	保存された添付ファイルの名前                             |
| body.data[].attachFileList[].fileSize      | 	Long    | 	添付ファイルの大きさ                                 |
| body.data[].attachFileList[].createDate    | 	String  | 	作成日時                                       |
| body.data[].attachFileList[].createUser    | 	String  | 	作成者                                        |
| body.data[].attachFileList[].updateDate    | 	String  | 	修正日時                                       |
| body.data[].attachFileList[].updateUser    | 	String  | 	修正者                                        |
| body.data[].attachFileList[].uploadType    | 	String  | 	アップロードタイプ                                  |
| body.data[].attachFileList[].existFileName | 	String  | 	保存された添付ファイルの名前                             |
| body.data[].senderGroupingKey              | 	String  | 	発信者グループキー                                  |
| body.data[].recipientGroupingKey           | 	String  | 	受信者グループキー                                  |

## 認証用SMS(緊急)

### 認証用SMSの送信

#### リクエスト

[URL]

```
POST  /sms/v2.2/appKeys/{appKey}/sender/auth/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Request body]

```json
{
  "templateId": "TemplateId",
  "body": "本文",
  "sendNo": "15446859",
  "requestDate": "2018-08-10 10:00",
  "senderGroupingKey": "SenderGroupingKey",
  "recipientList": [
    {
      "recipientNo": "01000000000",
      "countryCode": "82",
      "internationalRecipientNo": "821000000000",
      "templateParameter": {
        "key": "value"
      },
      "recipientGroupingKey": "recipientGroupingKey"
    }
  ],
  "userId": "UserId"
}
```

| 値                                         | 	タイプ    | 最大                                                             | 	必須 | 	説明                                                            |
|-------------------------------------------|---------|----------------------------------------------------------------|-----|----------------------------------------------------------------|
| templateId                                | 	String | 50                                                             | 	X  | 	送信テンプレートID                                                    |
| body                                      | 	String | 標準：90バイト、最大：255文字(EUC-KR基準) [[注意事項](./api-guide/#precautions)] | 	O  | 	本文内容                                                          |
| sendNo                                    | 	String | 13                                                             | 	O  | 	発信番号                                                          |
| requestDate                               | String  | -                                                                  | X   | 予約日時(yyyy-MM-dd HH:mm)<br/>現在から最大60日後まで設定可                                               |
| senderGroupingKey                         | String  | 100                                                            | X   | 発信者グループキー                                                      |
| recipientList[].recipientNo               | 	String | 20                                                             | 	O  | 	受信番号<br/>countryCodeと組み合わせて使用可能                               |
| recipientList[].countryCode               | 	String | 8                                                              | 	X  | 	国番号[デフォルト値：82(韓国)]                                            |
| recipientList[].internationalRecipientNo  | String  | 20                                                             | X   | 国番号を含む受信番号<br/>例)821012345678<br/>recipientNoがある場合、この値は無視<br/> |
| recipientList[].templateParameter         | 	Object | -                                                              | 	X  | 	テンプレートパラメータ(テンプレートID入力時)                                      |
| recipientList[].templateParameter.{key}   | String  | -                                                              | 	X  | 	置換キー(##key##)                                                 |
| recipientList[].templateParameter.{value} | Object  | -                                                              | 	X  | 	置換キーにマッピングされるValue値                                           |
| recipientList[].recipientGroupingKey      | String  | 100                                                            | X   | 受信者グループキー                                                      |
| userId                                    | 	String | 100                                                            | 	X  | 送信セパレータex)admin,system                                         |

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "data": {
      "requestId": "20180810100630ReZQ6KZzAH0",
      "statusCode": "2",
      "senderGroupingKey": "SenderGroupingKey",
      "sendResultList": [
        {
          "recipientNo": "01000000000",
          "resultCode": 0,
          "resultMessage": "SUCCESS",
          "recipientSeq": 1,
          "recipientGroupingKey": "RecipientGroupingKey"
        }
      ]
    }
  }
}
```

| 値                                               | 	タイプ     | 	説明                                          |
|-------------------------------------------------|----------|----------------------------------------------|
| header.isSuccessful                             | 	Boolean | 	成否                                          |
| header.resultCode                               | 	Integer | 	失敗コード                                       |
| header.resultMessage                            | 	String  | 	失敗メッセージ                                     |
| body.data.requestId                             | 	String  | 	リクエストID                                     |
| body.data.statusCode                            | 	String  | 	リクエストステータスコード(1：リクエスト中、2：リクエスト完了、3：リクエスト失敗) |
| body.data.senderGroupingKey                     | 	String  | 	発信者グループキー                                   |
| body.data.sendResultList[].recipientNo          | String   | 受信番号                                         |
| body.data.sendResultList[].resultCode           | Integer  | 結果コード                                        |
| body.data.sendResultList[].resultMessage        | String   | 結果メッセージ                                      |
| body.data.sendResultList[].recipientSeq         | Integer  | 受信者シーケンス(mtPr)                               |
| body.data.sendResultList[].recipientGroupingKey | String   | 受信者グループキー                                    |

#### 例

| Http metho | URL                                                                       |
|------------|---------------------------------------------------------------------------|
| POST       | https://api-sms.cloud.toast.com/sms/v2.2/appKeys/{appKey}/sender/auth/sms |

[Request body]

```json
{
  "body": "本文",
  "sendNo": "15446859",
  "senderGroupingKey": "SenderGroupingKey",
  "recipientList": [
    {
      "recipientNo": "01000000000",
      "recipientGroupingKey": "recipientGroupingKey"
    },
    {
      "recipientNo": "01000000001",
      "recipientGroupingKey": "RecipientGroupingKey2"
    }
  ]
}
```

[Response]

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "data": {
      "requestId": "20180810100630ReZQ6KZzAH0",
      "statusCode": "2",
      "senderGroupingKey": "SenderGroupingKey",
      "sendResultList": [
        {
          "recipientNo": "01000000000",
          "resultCode": 0,
          "resultMessage": "SUCCESS",
          "recipientSeq": 1,
          "recipientGroupingKey": "RecipientGroupingKey"
        },
        {
          "recipientNo": "01000000001",
          "resultCode": 0,
          "resultMessage": "SUCCESS",
          "recipientSeq": 2,
          "recipientGroupingKey": "RecipientGroupingKey2"
        }
      ]
    }
  }
}
```

[curl]

```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-sms.cloud.toast.com/sms/v2.2/appKeys/{appKey}/sender/auth/sms -d '{"body": "{本文内容}","sendNo": "{発信番号}","recipientList":[{"recipientNo": "{受信番号}","templateParameter": { }}],"userId": ""}'
```

### 認証用SMS送信リストの照会

#### リクエスト

[URL]

```
GET  /sms/v2.2/appKeys/{appKey}/sender/auth/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Query parameter]

* requestIdまたはstartRequestDate + endRequestDateまたはstartCreateDate + endCreateDateは必須です。
* 登録日時と発信日時を同時に照会する場合、発信日時は無視されます。

| 値                    | 	タイプ     | 	最大  | 必須     | 	説明                                                   |
|----------------------|----------|------|--------|------------------------------------------------------|
| requestId            | 	String  | 25   | 	必須    | 	リクエストID                                              |
| startRequestDate     | 	String  | -    | 	必須    | 	送信日の開始値(yyyy-MM-dd HH:mm:ss)                         |
| endRequestDate       | 	String  | -    | 	必須    | 	送信日の終了値(yyyy-MM-dd HH:mm:ss)                         |
| startCreateDate      | 	String  | -    | 	必須    | 	送信日の開始値(yyyy-MM-dd HH:mm:ss)                         |
| endCreateDate        | 	String  | -    | 	必須    | 	送信日の終了値(yyyy-MM-dd HH:mm:ss)                         |
| startResultDate      | 	String  | -    | 	オプション | 	受信日の開始値(yyyy-MM-dd HH:mm:ss)                         |
| endResultDate        | 	String  | -    | 	オプション | 	受信日の終了値(yyyy-MM-dd HH:mm:ss)                         |
| sendNo               | 	String  | 13   | 	オプション | 	発信番号                                                 |
| recipientNo          | 	String  | 20   | 	オプション | 	受信番号                                                 |
| templateId           | 	String  | 50   | 	オプション | 	テンプレート番号                                             |
| msgStatus            | 	String  | 1    | 	オプション | 	メッセージステータスコード(0：失敗、 1：リクエスト、 2：処理中、 3：成功、 4：予約キャンセル、 5：重複失敗、 6:失敗(広告制限)、 再送信待機(広告制限)) |
| resultCode           | 	String  | 10   | 	オプション | 	受信結果コード[[照会コード表](./error-code/#_2)]                  |
| subResultCode        | 	String  | 10   | 	オプション | 	受信結果詳細コード[[照会コード表](./error-code/#_3)]                |
| senderGroupingKey    | 	String  | 100  | 	オプション | 	送信者グループキー                                            |
| recipientGroupingKey | 	String  | 100  | 	オプション | 	受信者グループキー                                            |
| receiverRegion       | 	String  | -    | 	オプション | 	国内/国際 (DOMESTIC: 国内, INTERNATIONAL: 国際) |
| countryCode          | 	String  | -    | 	オプション | 	国コード [[送信可能国](./international-sending-policy/#_5)] |
| pageNum              | 	Integer | -    | 	オプション | 	ページ番号(デフォルト値：1)                                      |
| pageSize             | 	Integer | 1000 | 	オプション | 	照会数(デフォルト値：15)                                       |

#### レスポンス

```json
{
  "header": {
    "resultCode": 0,
    "resultMessage": "SUCCESS",
    "isSuccessful": true
  },
  "body": {
    "pageNum": 1,
    "pageSize": 15,
    "totalCount": 1,
    "data": [
      {
        "requestId": "20180810100630ReZQ6KZzAH0",
        "requestDate": "2018-08-10 10:06:30.0",
        "resultDate": "2018-08-10 10:06:42.0",
        "templateId": "TemplateId",
        "templateName": "テンプレート名",
        "categoryId": 0,
        "categoryName": "カテゴリー名",
        "body": "短文テスト",
        "sendNo": "15446859",
        "countryCode": "82",
        "recipientNo": "01000000000",
        "msgStatus": "3",
        "msgStatusName": "成功",
        "resultCode": "1000",
        "resultCodeName": "成功",
        "telecomCode": 10001,
        "telecomCodeName": "SKT",
        "mtPr": "1",
        "sendType": "0",
        "userId": "tester",
        "adYn": "N",
        "resultMessage": "",
        "senderGroupingKey": "SenderGroupingKey",
        "recipientGroupingKey": "RecipientGroupingKey"
      }
    ]
  }
}
```

| 値                                | 	タイプ     | 	説明                                         |
|----------------------------------|----------|---------------------------------------------|
| header.isSuccessful              | 	Boolean | 	成否                                         |
| header.resultCode                | 	Integer | 	失敗コード                                      |
| header.resultMessage             | 	String  | 	失敗メッセージ                                    |
| body.pageNum                     | 	Integer | 	現在のページ番号                                   |
| body.pageSize                    | 	Integer | 	照会されたデータ数                                  |
| body.totalCount                  | 	Integer | 	総データ数                                      |
| body.data[].requestId            | 	String  | 	リクエストID                                    |
| body.data[].requestDate          | 	String  | 	発信日時                                       |
| body.data[].resultDate           | 	String  | 	受信日時                                       |
| body.data[].templateId           | 	String  | 	テンプレートID                                   |
| body.data[].templateName         | 	String  | 	テンプレート名                                    |
| body.data[].categoryId           | 	Integer | 	カテゴリーID                                    |
| body.data[].categoryName         | 	String  | 	カテゴリー名                                     |
| body.data[].body                 | 	String  | 	本文内容                                       |
| body.data[].sendNo               | 	String  | 	発信番号                                       |
| body.data[].countryCode          | 	String  | 	国番号                                        |
| body.data[].recipientNo          | 	String  | 	受信番号                                       |
| body.data[].msgStatus            | 	String  | 	メッセージステータスコード                              |
| body.data[].msgStatusName        | 	String  | 	メッセージステータスコード名                             |
| body.data[].resultCode           | 	String  | 	受信結果コード[[受信結果コード表](./error-code/#emma-v3)] |
| body.data[].resultCodeName       | 	String  | 	受信結果コード名                                   |
| body.data[].telecomCode          | 	Integer | 	サービスプロバイダーコード                              |
| body.data[].telecomCodeName      | 	String  | 	サービスプロバイダー名                                |
| body.data[].mtPr                 | 	Integer | 	送信詳細ID(詳細照会時は必須)                           |
| body.data[].sendType             | 	String  | 	送信タイプ(0：Sms、1：Mms、2：Auth)                  |
| body.data[].userId               | 	String  | 	送信リクエストID                                  |
| body.data[].adYn                 | 	String  | 	広告かどうか                                     |
| body.data[].senderGroupingKey    | 	String  | 	発信者グループキー                                  |
| body.data[].recipientGroupingKey | 	String  | 	受信者グループキー                                  |

### 認証用SMS送信の単一照会

#### リクエスト

[URL]

```
GET  /sms/v2.2/appKeys/{appKey}/sender/auth/sms/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値         | 	タイプ    | 	説明            |
|-----------|---------|----------------|
| appKey    | 	String | 	固有のアプリケーションキー |
| requestId | 	String | 	リクエストID       |

[Query parameter]

| 値    | 	タイプ     | 	必須 | 	説明     |
|------|----------|-----|---------|
| mtPr | 	Integer | 	必須 | 	送信詳細ID |

#### レスポンス

```json
{
  "header": {
    "resultCode": 0,
    "resultMessage": "SUCCESS",
    "isSuccessful": true
  },
  "body": {
    "data": {
      "requestId": "20180810100630ReZQ6KZzAH0",
      "requestDate": "2018-08-10 10:06:30.0",
      "resultDate": "2018-08-10 10:06:42.0",
      "templateId": "TemplateId",
      "templateName": "テンプレート名",
      "categoryId": 0,
      "categoryName": "カテゴリー名",
      "body": "本文",
      "sendNo": "15446859",
      "countryCode": "82",
      "recipientNo": "01000000000",
      "msgStatus": "3",
      "msgStatusName": "成功",
      "resultCode": "1000",
      "resultCodeName": "成功",
      "telecomCode": 10001,
      "telecomCodeName": "SKT",
      "mtPr": "1",
      "sendType": "0",
      "userId": "tester",
      "adYn": "N",
      "resultMessage": "",
      "senderGroupingKey": "SenderGroupingKey",
      "recipientGroupingKey": "recipientGroupingKey"
    }
  }
}
```

| 値                              | 	タイプ     | 	説明                                         |
|--------------------------------|----------|---------------------------------------------|
| header.isSuccessful            | 	Boolean | 	成否                                         |
| header.resultCode              | 	Integer | 	失敗コード                                      |
| header.resultMessage           | 	String  | 	失敗メッセージ                                    |
| body.data.requestId            | 	String  | 	リクエストID                                    |
| body.data.requestDate          | 	String  | 	発信日時                                       |
| body.data.resultDate           | 	String  | 	受信日時                                       |
| body.data.templateId           | 	String  | 	テンプレートID                                   |
| body.data.templateName         | 	String  | 	テンプレート名                                    |
| body.data.categoryId           | 	Integer | 	カテゴリーID                                    |
| body.data.categoryName         | 	String  | 	カテゴリー名                                     |
| body.data.body                 | 	String  | 	本文内容                                       |
| body.data.sendNo               | 	String  | 	発信番号                                       |
| body.data.countryCode          | 	String  | 	国番号                                        |
| body.data.recipientNo          | 	String  | 	受信番号                                       |
| body.data.msgStatus            | 	String  | 	メッセージステータスコード                              |
| body.data.msgStatusName        | 	String  | 	メッセージステータスコード名                             |
| body.data.resultCode           | 	String  | 	受信結果コード[[受信結果コード表](./error-code/#emma-v3)] |
| body.data.resultCodeName       | 	String  | 	受信結果コード名                                   |
| body.data.telecomCode          | 	Integer | 	サービスプロバイダーコード                              |
| body.data.telecomCodeName      | 	String  | 	サービスプロバイダー名                                |
| body.data.mtPr                 | 	Integer | 	送信詳細ID(詳細照会時は必須)                           |
| body.data.sendType             | 	String  | 	送信タイプ(0：Sms、1：Mms、2：Auth)                  |
| body.data.userId               | 	String  | 	送信リクエストID                                  |
| body.data.adYn                 | 	String  | 	広告かどうか                                     |
| body.data.senderGroupingKey    | 	String  | 	発信者グループキー                                  |
| body.data.recipientGroupingKey | 	String  | 	受信者グループキー                                  |

## 広告文字

### 広告性SMS送信

[URL]

```
POST  /sms/v2.2/appKeys/{appKey}/sender/ad-sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Request body]
上のSMS送信と同じ。
[[Request Body参考](./api-guide/#sms_2)]

<span style="color:red">ただし、本文に広告性必須文言が含まれている必要があります</span>

080番号はコンソールの**080受信拒否設定**タブで確認できます。

広告性必須文言のルールは次のとおりです。
- 冒頭の文言: `(広告)`
- 最後の文言： 無料受信拒否 {080受信拒否番号}`または `無料受信拒否 {080受信拒否番号}` (この文言には空白が含まれる場合があります。)

例
```
(広告)
[無料受信拒否]080XXXXXXX
```
```
(広告)

[無料拒否]080XXXXXXX
```

### 広告性MMS送信

※ MMSは韓国外への送信はできません。

[URL]

```
POST  /sms/v2.2/appKeys/{appKey}/sender/ad-mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Request body]
上のMMS送信と同じ。
[[Request Body参考](./api-guide/#mms_1)]

<span style="color:red">ただし、本文に広告性必須文言が含まれている必要があります</span>

080番号はコンソールの**080受信拒否設定**タブで確認できます。

広告性必須文言のルールは次のとおりです。
- 冒頭の文言: `(広告)`
- 最後の文言： 無料受信拒否 {080受信拒否番号}`または `無料受信拒否 {080受信拒否番号}` (この文言には空白が含まれる場合があります。)

例
```
(広告)
[無料受信拒否]080XXXXXXX
```
```
(広告)

[無料拒否]080XXXXXXX
```

## 結果アップデート基準メッセージ照会

* 該当APIは、メッセージ送信結果アップデート時間基準で照会されます。
* 端末送信結果をサービスの外で使用する場合、このAPIを使用してください。

### メッセージ照会

#### リクエスト

[URL]

```
GET /sms/v2.2/appKeys/{appKey}/message-results?startUpdateDate={startUpdateDate}&endUpdateDate={endUpdateDate}&messageType={messageType}&pageNum={pageNum}&pageSize={pageSize}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Query parameter]

* 検索開始時間と検索終了時間の範囲は1日に制限されます。

| 値               | 	タイプ    | 	必須    | 	説明                                       |
|-----------------|---------|--------|-------------------------------------------|
| startUpdateDate | 	String | 	必須    | 	結果アップデート照会の開始時間 <br/>yyyy-MM-dd HH:mm:ss |
| endUpdateDate   | 	String | 	必須    | 	結果アップデート照会の終了時間 <br/>yyyy-MM-dd HH:mm:ss |
| messageType     | 	String | 	オプション | 	メッセージタイプ(SMS/LMS/MMS/AUTH)               |
| pageNum         | Integer | オプション  | ページ番号(デフォルト値：1)                           |
| pageSize        | Integer | オプション  | 照会数(デフォルト値：15)                            |

#### レスポンス

```json
{
  "header": {
    "resultCode": 0,
    "resultMessage": "success",
    "isSuccessful": true
  },
  "body": {
    "pageNum": 1,
    "pageSize": 15,
    "totalCount": 35,
    "data": [
      {
        "messageType": "SMS",
        "requestId": "20230914101505oAJTbxHkIB0",
        "recipientSeq": 1,
        "resultCode": "1000",
        "resultCodeName": "成功",
        "requestDate": "2023-09-14 10:15:05.0",
        "resultDate": "2023-09-14 10:15:08.0",
        "updateDate": "2023-09-14 10:15:12.0",
        "telecomCode": "10002",
        "telecomCodeName": "KT",
        "senderGroupingKey": "SenderGroupingKey",
        "recipientGroupingKey": "recipientGroupingKey"
      }
    ]
  }
}
```

| 値                                                 | 	タイプ     | 	説明                               |
|---------------------------------------------------|----------|-----------------------------------|
| header.isSuccessful                               | 	Boolean | 	成否                               |
| header.resultCode                                 | 	Integer | 	失敗コード                            |
| header.resultMessage                              | 	String  | 	失敗メッセージ                          |
| body.data.resultUpdateList[].messageType          | String   | メッセージタイプ(SMS/LMS/MMS/AUTH)        |
| body.data.resultUpdateList[].requestId            | String   | リクエストID                           |
| body.data.resultUpdateList[].recipientSeq         | Integer  | 受信者シーケンス                          |
| body.data.resultUpdateList[].resultCode           | String   | 結果コード                             |
| body.data.resultUpdateList[].resultCodeName       | String   | 結果コード名                            |
| body.data.resultUpdateList[].requestDate          | String   | リクエスト日時(yyyy-MM-dd HH:mm:ss.S)    |
| body.data.resultUpdateList[].resultDate           | String   | 受信日時(yyyy-MM-dd HH:mm:ss.S)       |
| body.data.resultUpdateList[].updateDate           | String   | 結果アップデート日時(yyyy-MM-dd HH:mm:ss.S) |
| body.data.resultUpdateList[].telecomCode          | String   | サービスプロバイダーコード                     |
| body.data.resultUpdateList[].telecomCodeName      | String   | サービスプロバイダーコード名                    |
| body.data.resultUpdateList[].senderGroupingKey    | String   | 発信者グループキー                         |
| body.data.resultUpdateList[].recipientGroupingKey | String   | 受信者グループキー                         |

## タグ送信

### タグSMS送信

#### リクエスト

[URL]

```
POST /sms/v2.2/appKeys/{appKey}/tag-sender/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Request body]

```json
{
  "body": "本文",
  "sendNo": "15446859",
  "requestDate": "2018-03-22 10:00",
  "templateId": "TemplateId",
  "templateParameter": {
    "key": "value"
  },
  "tagExpression": [
    "tag1",
    "AND",
    "tag2"
  ],
  "userId": "user_id",
  "adYn": "N",
  "autoSendYn": "N"
}
```

| 値                 | 	タイプ                | 	最大                                                            | 必須 | 	説明                                 |
|-------------------|---------------------|----------------------------------------------------------------|----|-------------------------------------|
| body              | 	String             | 標準：90バイト、最大：255文字(EUC-KR基準) [[注意事項](./api-guide/#precautions)] | 	O | 	本文内容                               |
| sendNo            | String              | 13                                                             | O  | 発信番号                                |
| requestDate       | String              | -                                                                  | X  | 予約日時(yyyy-MM-dd HH:mm)<br/>現在から最大60日後まで設定可            |
| templateId        | String              | 50                                                             | X  | テンプレートID                            |
| templateParameter | Map<String, String> | -                                                              | X  | テンプレートパラメータ                         |
| tagExpression     | List<String>        | -                                                              | O  | タグ表現式<br/>ex) ["tagA","AND","tabB"] |
| userId            | String              | 100                                                            | X  | リクエストしたユーザーID                       |
| adYn              | String              | 1                                                              | X  | 広告かどうか(デフォルト値：N)                    |
| autoSendYn        | String              | 1                                                              | X  | 自動送信(即時送信)するかどうか(デフォルト値：Y)          |

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "data": {
      "requestId": "201808130914102Y94ABMNF30"
    }
  }
}
```

| 値                    | 	タイプ     | 	説明      |
|----------------------|----------|----------|
| header.isSuccessful  | 	Boolean | 	成否      |
| header.resultCode    | 	Integer | 	失敗コード   |
| header.resultMessage | 	String  | 	失敗メッセージ |
| body.data.requestId  | 	String  | 	リクエストID |

### タグLMS送信

※ MMSは韓国外への送信はできません。

#### リクエスト

[URL]

```
POST  /sms/v2.2/appKeys/{appKey}/tag-sender/mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Request body]

```json
{
  "body": "本文",
  "sendNo": "15446859",
  "requestDate": "2018-03-22 10:00",
  "templateId": "TemplateId",
  "templateParameter": {
    "key": "value"
  },
  "attachFileIdList": [
    1,
    2,
3
  ],
  "tagExpression": [
    "tag1",
    "AND",
    "tag2"
  ],
  "userId": "user_id",
  "adYn": "N",
  "autoSendYn": "N"
}
```

| 値                 | 	タイプ                | 	最大               | 必須 | 	説明                                 |
|-------------------|---------------------|-------------------|----|-------------------------------------|
| title             | String              | 40バイト(EUC-KR基準)   | O  | タイトル                                |
| body              | String              | 2000バイト(EUC-KR基準) | O  | 内容                                  |
| sendNo            | String              | 13                | O  | 発信番号                                |
| requestDate       | String              | -      | X  | 予約日時(yyyy-MM-dd HH:mm)<br/>現在から最大60日後まで設定可            |
| templateId        | String              | 50                | X  | テンプレートID                            |
| templateParameter | Map<String, String> | -                 | X  | テンプレートパラメータ                         |
| tagExpression     | List<String>        | -                 | O  | タグ表現式<br/>ex) ["tagA","AND","tabB"] |
| attachFileIdList  | List<Integer>       | -                 | X  | 添付ファイルID(fileId)                    |
| userId            | String              | 100               | X  | リクエストしたユーザーID                       |
| adYn              | String              | 1                 | X  | 広告かどうか(デフォルト値：N)                    |
| autoSendYn        | String              | 1                 | X  | 自動送信(即時送信)するかどうか(基本Y)               |

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "data": {
      "requestId": "201808130914102Y94ABMNF30"
    }
  }
}
```

| 値                    | 	タイプ     | 	説明      |
|----------------------|----------|----------|
| header.isSuccessful  | 	Boolean | 	成否      |
| header.resultCode    | 	Integer | 	失敗コード   |
| header.resultMessage | 	String  | 	失敗メッセージ |
| body.data.requestId  | 	String  | 	リクエストID |

### タグ送信リストの照会

#### リクエスト

[URL]

```
GET /sms/v2.2/appKeys/{appKey}/tag-sender
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Query parameter]

* requestIdまたはstartRequestDate + endRequestDateまたはstartCreateDate + endCreateDateは必須です。

| 値                | 	タイプ              | 最大   | 	必須 | 	説明                                                                                                                                                           |
|------------------|-------------------|------|-----|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| appKey           | String            | -    | O   | アプリケーションキー                                                                                                                                                    |
| sendType         | required, String  | 1    | O   | 送信タイプ<br>SMS："0",<br>MMS："1"                                                                                                                                  |
| requestId        | String            | -    | O   | リクエストID                                                                                                                                                       |
| startRequestDate | String            | -    | O   | 送信日の開始                                                                                                                                                        |
| endRequestDate   | String            | -    | O   | 送信日の終了                                                                                                                                                        |
| startCreateDate  | 	String           | -    | 	O  | 	送信日の開始値                                                                                                                                                      |
| endCreateDate    | 	String           | -    | 	O  | 	送信日の終了値                                                                                                                                                      |
| statusCode       | String            | 10   | X   | 送信ステータスコード<br>WAIT："MAS00"<br>READY："MAS01"<br>SENDREADY："MAS09"<br>SENDWAIT："MAS10"<br>SENDING："MAS11"<br>COMPLETE："MAS19"<br>CANCEL："MAS91"<br>FAIL："MAS99" |
| pageNum          | optional, Integer | -    | X   | ページ番号                                                                                                                                                         |
| pageSize         | optional, Integer | 1000 | X   | 照会数                                                                                                                                                           |

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "success"
  },
  "body": {
    "pageNum": 0,
    "pageSize": 0,
    "totalCount": 0,
    "data": [
      {
        "requestId": "20171220141558eonMsyDI6P0",
        "requestIp": "127.0.0.1",
        "sendType": "0",
        "templateId": "TemplateId",
        "templateName": "テンプレート名",
        "masterStatusCode": "READY",
        "sendNo": "15446859",
        "requestDate": "2017-12-20 14:15:58",
        "tagExpression": [
          "Kb6BjCY1"
        ],
        "title": "タイトル",
        "body": "本文",
        "adYn": "N",
        "autoSendYn": "Y",
        "sendErrorCount": 0,
        "createUser": "CreateUser",
        "createDate": "2017-12-20 14:15:58.0",
        "updateUser": "UpdateUser",
        "updateDate": "2017-12-20 14:15:58.0"
      }
    ]
  }
}
```

| 値                           | 	タイプ         | 	説明        |
|-----------------------------|--------------|------------|
| header.isSuccessful         | 	Boolean     | 	成否        |
| header.resultCode           | 	Integer     | 	失敗コード     |
| header.resultMessage        | 	String      | 	失敗メッセージ   |
| body.data[].requestId       | String       | リクエストID    |
| body.data[].requestIp       | String       | リクエストIP    |
| body.data[].requestDate     | String       | リクエスト時間    |
| body.data[].tagSendStatus   | String       | タグ送信ステータス  |
| body.data[].tagExpression[] | List<String> | タグ表現式      |
| body.data[].templateId      | String       | テンプレートID   |
| body.data[].templateName    | String       | テンプレート名    |
| body.data[].senderName      | String       | 発信者名       |
| body.data[].senderMail      | String       | 発信者アドレス    |
| body.data[].title           | String       | タイトル       |
| body.data[].body            | String       | 内容         |
| body.data[].adYn            | String       | 広告かどうか     |
| body.data[].autoSendYn      | String       | 自動送信するかどうか |
| body.data[].sendErrorCount  | Integer      | エラー受信者件数   |
| body.data[].createUser      | String       | 作成者        |
| body.data[].createDate      | String       | 作成日時       |
| body.data[].updateUser      | String       | 修正したユーザー   |
| body.data[].updateDate      | String       | 修正日        |

### タグ送信受信者リストの照会

#### リクエスト

[URL]

```
GET /sms/v2.2/appKeys/{appKey}/tag-sender/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値         | 	タイプ    | 	説明            |
|-----------|---------|----------------|
| appKey    | 	String | 	固有のアプリケーションキー |
| requestId | String  | リクエストID        |

[Query parameter]

* requestIdまたはstartRequestDate + endRequestDateは必須です。

| 値                | 	タイプ    | 最大   | 	必須 | 	説明                                                                                                 |
|------------------|---------|------|-----|-----------------------------------------------------------------------------------------------------|
| recipientNum     | String  | 20   | X   | 受信者番号                                                                                               |
| startRequestDate | String  | -    | O   | 送信リクエストの開始日                                                                                         |
| endRequestDate   | String  | -    | O   | 送信リクエストの終了日                                                                                         |
| startResultDate  | String  | -    | X   | 受信開始日                                                                                               |
| endResultDate    | String  | -    | X   | 受信終了日                                                                                               |
| msgStatusName    | String  | 10   | X   | メッセージステータスコード<br/> - READY：準備<br/> - SENDING：送信リクエスト中<br/> - COMPLETED：送信リクエスト完了<br/> - FAILED：送信失敗 |
| resultCode       | String  | 10   | X   | 受信結果コード                                                                                             |
| receiverRegion   | String  | -    | X   | 国内/国際 (DOMESTIC: 国内, INTERNATIONAL: 国際) |
| countryCode      | String  | -    | X   | 国コード [[送信可能国](./international-sending-policy/#_5)] |
| pageNum          | Integer | -    | X   | ページ番号                                                                                               |
| pageSize         | Integer | 1000 | X   | 照会数                                                                                                 |

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "success"
  },
  "body": {
    "pageNum": 0,
    "pageSize": 0,
    "totalCount": 0,
    "data": [
      {
        "requestId": "20180813022044Jps2xJ1qsv0",
        "recipientSeq": 99,
        "countryCode": "82",
        "recipientNo": "01000000000",
        "requestDate": "2018-08-13 02:20:44.0",
        "msgStatus": "3",
        "msgStatusName": "COMPLETED",
        "resultCode": "3015",
        "receiveDate": "2018-08-13 02:20:53.0",
        "createDate": "2018-08-13 02:20:46.0",
        "updateDate": "2018-08-13 02:27:00.0"
      }
    ]
  }
}
```

| 値                       | 	タイプ     | 	説明                                        |
|-------------------------|----------|--------------------------------------------|
| header.isSuccessful     | 	Boolean | 	成否                                        |
| header.resultCode       | 	Integer | 	失敗コード                                     |
| header.resultMessage    | 	String  | 	失敗メッセージ                                   |
| body.data.requestId     | String   | リクエストID                                    |
| body.data.recipientSeq  | Integer  | 受信者シーケンス                                   |
| body.data.countryCode   | String   | 受信者国コード                                    |
| body.data.recipientNo   | String   | 受信者番号                                      |
| body.data.requestDate   | String   | リクエスト日時                                    |
| body.data.msgStatus     | String   | メッセージステータスコード                              |
| body.data.msgStatusName | String   | メッセージステータスコード名                             |
| body.data.resultCode    | String   | 受信結果コード[[受信結果コード表](./error-code/#emma-v3)] |
| body.data.receiveDate   | String   | 受信日時                                       |
| body.data.createDate    | String   | 登録日時                                       |
| body.data.updateDate    | String   | 修正日                                        |

### タグ送信受信者リストの詳細照会

#### リクエスト

[URL]

```
GET /sms/v2.2/appKeys/{appKey}/tag-sender/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値            | 	タイプ    | 	説明            |
|--------------|---------|----------------|
| appKey       | 	String | 	固有のアプリケーションキー |
| requestId    | String  | リクエストID        |
| recipientSeq | String  | シーケンス          |

[Request body]

```
X
```

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "data": {
      "requestId": "20180813022044Jps2xJ1qsv0",
      "recipientSeq": 14,
      "sendType": "0",
      "messageType": "SMS",
      "templateId": "TemplateId",
      "templateName": "テンプレート名",
      "sendNo": "15446859",
      "recipientNum": "01000000000",
      "countryCode": "82",
      "title": "タイトル",
      "body": "本文",
      "requestDate": "2018-08-13 02:20:44.0",
      "msgStatusName": "COMPLETED",
      "msgStatus": "3",
      "resultCode": "3015",
      "receiveDate": "2018-08-13 02:20:48.0",
      "attachFileList": []
    }
  }
}
```

| 値                                       | 	タイプ     | 	説明                                        |
|-----------------------------------------|----------|--------------------------------------------|
| header                                  | 	Object  | 	ヘッダ領域                                     |
| header.isSuccessful                     | 	Boolean | 	成否                                        |
| header.resultCode                       | 	Integer | 	失敗コード                                     |
| header.resultMessage                    | 	String  | 	失敗メッセージ                                   |
| body.data.requestId                     | String   | リクエストID                                    |
| body.data.recipientSeq                  | Integer  | 受信者シーケンス                                   |
| body.data.sendType                      | String   | 送信タイプ                                      |
| body.data.messageType                   | String   | メッセージタイプ                                   |
| body.data.templateId                    | String   | テンプレートID                                   |
| body.data.templateName                  | String   | テンプレート名                                    |
| body.data.sendNo                        | String   | 発信番号                                       |
| body.data.title                         | String   | タイトル                                       |
| body.data.body                          | String   | 内容                                         |
| body.data.recipientNum                  | String   | 受信者番号                                      |
| body.data.requestDate                   | String   | リクエスト日時                                    |
| body.data.msgStatusName                 | String   | メッセージのステータス名                               |
| body.data.resultCode                    | String   | 受信結果コード[[受信結果コード表](./error-code/#emma-v3)] |
| body.data.receiveDate                   | String   | 受信日時                                       |
| body.data.attachFileList[].filePath     | String   | 添付ファイル - パス                                |
| body.data.attachFileList[].fileName     | String   | 添付ファイル - ファイル名                             |
| body.data.attachFileList[].fileSize     | Long     | 添付ファイル - サイズ                               |
| body.data.attachFileList[].fileSequence | Integer  | 添付ファイル - ファイル番号                            |
| body.data.attachFileList[].createDate   | String   | 添付ファイル - 作成日時                              |
| body.data.attachFileList[].updateDate   | String   | 添付ファイル - 修正日                               |

<span id="binaryUpload"></span>

## 添付ファイル

### 添付ファイルのアップロード

#### リクエスト

[URL]

```
POST  /sms/v2.2/appKeys/{appKey}/attachfile/binaryUpload
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Request body]

```json
{
  "fileName": "attachment.jpg",
  "createUser": "CreateUser",
  "fileBody": "{byte[] -> Base64エンコードした値}"
}
```

| 値          | タイプ    | 最大    | 必須 | 説明                                          |
|------------|--------|-------|----|---------------------------------------------|
| fileName   | String | 45    | 必須 | ファイル名(拡張子はjpg、jpegのみ可能)                     |
| fileBody   | Byte[] | 300KB | 必須 | ファイルbyte[]をBase64でエンコードした値。<br/>* またはバイト配列値 |
| createUser | String | 100   | 必須 | ファイルアップロードユーザー情報                            |

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "data": {
      "fileId": 0,
      "fileName": "attachment.jpg",
      "filePath": "0/toast-mt-2018-08-10/1609/178576"
    }
  }
}
```

| 値                    | 	タイプ     | 	説明                                                              |
|----------------------|----------|------------------------------------------------------------------|
| header.isSuccessful  | 	Boolean | 	成否                                                              |
| header.resultCode    | 	Integer | 	失敗コード                                                           |
| header.resultMessage | 	String  | 	失敗メッセージ                                                         |
| body.data.fileId     | 	Integer | 	ファイルID                                                          |
| body.data.fileName   | 	String  | 	ファイル名                                                           |
| body.data.filePath   | 	String  | 	添付ファイルの基本パス <br/> (https://domain/attachFile/filePath/fileName) |

#### 添付ファイルのアップロード例

| Http method | URL                                                                               |
|-------------|-----------------------------------------------------------------------------------|
| POST        | https://api-sms.cloud.toast.com/sms/v2.2/appKeys/{appKey}/attachfile/binaryUpload |

[Request body]

```json
{
  "fileName": "attachement.jpg",
  "createUser": "CreateUser",
  "fileBody": "/9j/4AAQSkZJRgABAQEAYABgAAD/4RDSRXhpZgAATU0AKgAAAAgABAE7AAIAAAAESkdHAIdpAAQAAAABAAAISpydAAEAAAAIAAAQwuocAAcAAAgMAAAAPgAAAAAc6gAAAAgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAFkAMAAgAAABQAABCYkAQAAgAAABQAABCskpEAAgAAAAM1MgAAkpIAAgAAAAM1MgAA6hwABwAACAwAAAiMAAAAABzqAAAACAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAMjAxODowMjowOCAxNjoyOTo0MQAyMDE4OjAyOjA4IDE2OjI5OjQxAAAASgBHAEcAAAD/4QsWaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wLwA8P3hwYWNrZXQgYmVnaW49J++7vycgaWQ9J1c1TTBNcENlaGlIenJlU3pOVGN6a2M5ZCc/Pg0KPHg6eG1wbWV0YSB4bWxuczp4PSJhZG9iZTpuczptZXRhLyI+PHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj48cmRmOkRlc2NyaXB0aW9uIHJkZjphYm91dD0idXVpZDpmYWY1YmRkNS1iYTNkLTExZGEtYWQzMS1kMzNkNzUxODJmMWIiIHhtbG5zOmRjPSJodHRwOi8vcHVybC5vcmcvZGMvZWxlbWVudHMvMS4xLyIvPjxyZGY6RGVzY3JpcHRpb24gcmRmOmFib3V0PSJ1dWlkOmZhZjViZGQ1LWJhM2QtMTFkYS1hZDMxLWQzM2Q3NTE4MmYxYiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIj48eG1wOkNyZWF0ZURhdGU+MjAxOC0wMi0wOFQxNjoyOTo0MS41MTc8L3htcDpDcmVhdGVEYXRlPjwvcmRmOkRlc2NyaXB0aW9uPjxyZGY6RGVzY3JpcHRpb24gcmRmOmFib3V0PSJ1dWlkOmZhZjViZGQ1LWJhM2QtMTFkYS1hZDMxLWQzM2Q3NTE4MmYxYiIgeG1sbnM6ZGM9Imh0dHA6Ly9wdXJsLm9yZy9kYy9lbGVtZW50cy8xLjEvIj48ZGM6Y3JlYXRvcj48cmRmOlNlcSB4bWxuczpyZGY9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkvMDIvMjItcmRmLXN5bnRheC1ucyMiPjxyZGY6bGk+SkdHPC9yZGY6bGk+PC9yZGY6U2VxPg0KCQkJPC9kYzpjcmVhdG9yPjwvcmRmOkRlc2NyaXB0aW9uPjwvcmRmOlJERj48L3g6eG1wbWV0YT4NCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIAogICAgICAgICAgICAgICAgICAgICAgICAgICAgPD94cGFja2V0IGVuZD0ndyc/Pv/bAEMABwUFBgUEBwYFBggHBwgKEQsKCQkKFQ8QDBEYFRoZGBUYFxseJyEbHSUdFxgiLiIlKCkrLCsaIC8zLyoyJyorKv/bAEMBBwgICgkKFAsLFCocGBwqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKv/AABEIABoAHQMBIgACEQEDEQH/xAAfAAABBQEBAQEBAQAAAAAAAAAAAQIDBAUGBwgJCgv/xAC1EAACAQMDAgQDBQUEBAAAAX0BAgMABBEFEiExQQYTUWEHInEUMoGRoQgjQrHBFVLR8CQzYnKCCQoWFxgZGiUmJygpKjQ1Njc4OTpDREVGR0hJSlNUVVZXWFlaY2RlZmdoaWpzdHV2d3h5eoOEhYaHiImKkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4eLj5OXm5+jp6vHy8/T19vf4+fr/xAAfAQADAQEBAQEBAQEBAAAAAAAAAQIDBAUGBwgJCgv/xAC1EQACAQIEBAMEBwUEBAABAncAAQIDEQQFITEGEkFRB2FxEyIygQgUQpGhscEJIzNS8BVictEKFiQ04SXxFxgZGiYnKCkqNTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNkZWZnaGlqc3R1dnd4eXqCg4SFhoeIiYqSk5SVlpeYmZqio6Slpqeoqaqys7S1tre4ubrCw8TFxsfIycrS09TV1tfY2dri4+Tl5ufo6ery8/T19vf4+fr/2gAMAwEAAhEDEQA/APD6K6e88IxWemTamdQaSx+zW8trJ5GDPJLkGMjd8u0pKCcn7nTmq/jDTbHSvH2qadao1vY2940aqmZGRAe24/MQPU8+tVs7ev4f8OT0uYFFdvrmhWmo/Ee30wXVpp9rcWlvIs0FmIAQbZXX90ZCPMbgY34LHqM1zes6bbaXq81mHv18rAK3tiLeZWxyGj3tt/M5HpTWrsGyv/Wpb1bXIp/DekaLYzXEsFj5kztcRhP3shGVUBmyq44ORksxwM1JrfiyLXtSa/u/DmkpdSXHnzyRPdDz/VWBmIAPfaFPoRXO0UgOk1Pxemq6vb3914d0jMMAgeFftBjmRYxGgbMpYFVUYKlTnqTWfruuz69dQSTW8FrFa26W1vb24bZFGucKC7Mx5JOWYnn0wKy6KAP/2Q=="
}
```

[Response]

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "data": {
      "fileId": 0,
      "fileName": "attachment.jpg",
      "filePath": "0/toast-mt-2018-08-10/1609/178576"
    }
  }
}
```

## カテゴリー

### カテゴリーの登録

#### リクエスト

[URL]

```
POST  /sms/v2.2/appKeys/{appKey}/categories
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Request body]

```json
{
  "categoryParentId": 0,
  "categoryName": "",
  "categoryDesc": "",
  "useYn": "",
  "createUser": ""
}
```

| 値                | 	タイプ     | 	最大文字数 | 必須     | 	説明                        |
|------------------|----------|--------|--------|----------------------------|
| categoryParentId | 	Integer | 	-     | オプション  | 親カテゴリーID [デフォルト値：最上位カテゴリー] |
| categoryName     | String   | 50     | 必須     | カテゴリーID                    |
| categoryDesc     | 	String  | 100    | 	オプション | 	カテゴリー名                    |
| useYn            | 	String  | 1      | 	必須    | 使用有無(Y/N)                  |
| createUser       | 	String  | 100    | オプション  | 登録したユーザー                   |

##### 説明

- categoryParentId値が空の場合、最上位カテゴリーのすぐ下に登録されます。

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "data": {
      "categoryId": 0,
      "categoryParentId": 0,
      "depth": 0,
      "sort": 0,
      "categoryName": "",
      "categoryDesc": "",
      "useYn": "",
      "createUser": ""
    }
  }
}
```

| 値                                   | 	タイプ     | 	説明         |
|-------------------------------------|----------|-------------|
| header.isSuccessful                 | 	Boolean | 	成否         |
| header.resultCode                   | 	Integer | 	失敗コード      |
| header.resultMessage                | 	String  | 	失敗メッセージ    |
| body.data[].categoryId              | 	Integer | 	カテゴリーID    |
| body.data[].categoryParentId        | 	Integer | 	親カテゴリーID   |
| body.data[].depth                   | 	Integer | 	カテゴリーの深さ   |
| body.data[].sort                    | 	Integer | 	カテゴリーソート順序 |
| body.data[].categoryName            | 	String  | 	カテゴリー名     |
| body.data[].categorycategoryDescame | 	String  | 	カテゴリーの説明   |
| body.data[].useYn                   | 	String  | 	使用有無       |
| body.data[].createUser              | 	String  | 	登録したユーザー   |

### カテゴリーリストの照会

#### リクエスト

[URL]

```
GET  /sms/v2.2/appKeys/{appKey}/categories
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Query parameter]

| 値        | 	タイプ     | 	最大文字数 | 必須     | 	説明              |
|----------|----------|--------|--------|------------------|
| pageNum  | 	Integer | -      | 	オプション | 	ページ番号(デフォルト値：1) |
| pageSize | 	Integer | 1000   | 	オプション | 	照会数(デフォルト値：15)  |

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "pageNum": 1,
    "pageSize": 15,
    "totalCount": 1,
    "data": [
      {
        "categoryId": 137612,
        "categoryParentId": 0,
        "depth": 0,
        "sort": 0,
        "categoryName": "カテゴリー",
        "categoryDesc": "最上位カテゴリー",
        "useYn": "Y",
        "createDate": "2018-04-17 15:39:56.0",
        "createUser": "bb076dc0-ef5e-11e7-9ede-005056ac7022",
        "updateDate": "2018-04-17 15:39:56.0",
        "updateUser": "bb076dc0-ef5e-11e7-9ede-005056ac7022"
      }
    ]
  }
}
```

| 値                                   | 	タイプ     | 	説明         |
|-------------------------------------|----------|-------------|
| header.isSuccessful                 | 	Boolean | 	成否         |
| header.resultCode                   | 	Integer | 	失敗コード      |
| header.resultMessage                | 	String  | 	失敗メッセージ    |
| body.pageNum                        | 	Integer | 	現在のページ番号   |
| body.pageSize                       | 	Integer | 	照会されたデータ数  |
| body.totalCount                     | 	Integer | 	データ総数      |
| body.data[].categoryId              | 	Integer | 	カテゴリーID    |
| body.data[].categoryParentId        | 	Integer | 	親カテゴリーID   |
| body.data[].depth                   | 	Integer | 	カテゴリーの深さ   |
| body.data[].sort                    | 	Integer | 	カテゴリーソート順序 |
| body.data[].categoryName            | 	String  | 	カテゴリー名     |
| body.data[].categorycategoryDescame | 	String  | 	カテゴリーの説明   |
| body.data[].useYn                   | 	String  | 	使用有無       |
| body.data[].createDate              | 	String  | 	登録日        |
| body.data[].createUser              | 	String  | 	登録したユーザー   |
| body.data[].updateDate              | 	String  | 	修正日        |
| body.data[].updateUser              | 	String  | 	修正したユーザー   |

### カテゴリーの単件照会

#### リクエスト

[URL]

```
GET  /sms/v2.2/appKeys/{appKey}/categories/{categoryId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値          | 	タイプ     | 	説明            |
|------------|----------|----------------|
| appKey     | 	String  | 	固有のアプリケーションキー |
| categoryId | 	Integer | 	カテゴリーID       |

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "data": [
      {
        "categoryId": 137612,
        "categoryParentId": 0,
        "depth": 0,
        "sort": 0,
        "categoryName": "カテゴリー",
        "categoryDesc": "最上位カテゴリー",
        "useYn": "Y",
        "createDate": "2018-04-17 15:39:56.0",
        "createUser": "bb076dc0-ef5e-11e7-9ede-005056ac7022",
        "updateDate": "2018-04-17 15:39:56.0",
        "updateUser": "bb076dc0-ef5e-11e7-9ede-005056ac7022"
      }
    ]
  }
}
```

| 値                                   | 	タイプ     | 	説明         |
|-------------------------------------|----------|-------------|
| header.isSuccessful                 | 	Boolean | 	成否         |
| header.resultCode                   | 	Integer | 	失敗コード      |
| header.resultMessage                | 	String  | 	失敗メッセージ    |
| body.data[].categoryId              | 	Integer | 	カテゴリーID    |
| body.data[].categoryParentId        | 	Integer | 	親カテゴリーID   |
| body.data[].depth                   | 	Integer | 	カテゴリーの深さ   |
| body.data[].sort                    | 	Integer | 	カテゴリーソート順序 |
| body.data[].categoryName            | 	String  | 	カテゴリー名     |
| body.data[].categorycategoryDescame | 	String  | 	カテゴリーの説明   |
| body.data[].useYn                   | 	String  | 	使用有無       |
| body.data[].createDate              | 	String  | 	登録日        |
| body.data[].createUser              | 	String  | 	登録したユーザー   |
| body.data[].updateDate              | 	String  | 	修正日        |
| body.data[].updateUser              | 	String  | 	修正したユーザー   |

### カテゴリーの修正

#### リクエスト

[URL]

```
PUT  /sms/v2.2/appKeys/{appKey}/categories/{categoryId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値          | 	タイプ     | 	説明            |
|------------|----------|----------------|
| appKey     | 	String  | 	固有のアプリケーションキー |
| categoryId | 	Integer | 	カテゴリーID       |

[Request body]

```json
{
  "categoryName": "",
  "categoryDesc": "",
  "useYn": "",
  "updateUser": ""
}
```

| 値            | 	タイプ    | 	最大文字数 | 必須     | 	説明       |
|--------------|---------|--------|--------|-----------|
| categoryName | String  | 50     | 必須     | カテゴリーID   |
| categoryDesc | 	String | 100    | 	オプション | 	カテゴリー名   |
| useYn        | 	String | 1      | 	必須    | 使用有無(Y/N) |
| updateUser   | 	String | 100    | 	オプション | 修正したユーザー  |

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": "",
    "resultMessage": ""
  }
}
```

### カテゴリーの削除

#### リクエスト

[URL]

```
DELETE  /sms/v2.2/appKeys/{appKey}/categories/{categoryId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値          | 	タイプ     | 	説明            |
|------------|----------|----------------|
| appKey     | 	String  | 	固有のアプリケーションキー |
| categoryId | 	Integer | 	カテゴリーID       |

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": "",
    "resultMessage": ""
  }
}
```

## テンプレート

### テンプレートの登録

#### リクエスト

[URL]

```
POST  /sms/v2.2/appKeys/{appKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Request body]

```json
{
  "categoryId": 0,
  "templateId": "",
  "templateName": "",
  "templateDesc": "",
  "sendNo": "",
  "sendType": "",
  "title": "",
  "body": "",
  "useYn": "",
  "attachFileIdList": [
    0,
1
  ]
}
```

| 値                | 	タイプ          | 	最大文字数 | 必須     | 	説明                             |
|------------------|---------------|--------|--------|---------------------------------|
| categoryId       | 	Integer      | 	-     | 必須     | 	カテゴリーID                        |
| templateId       | String        | 50     | 必須     | テンプレートID                        |
| templateName     | 	String       | 50     | 	必須    | 	テンプレート名                        |
| templateDesc     | 	String       | 100    | 	オプション | 	テンプレートの説明                      |
| sendNo           | String        | 13     | 必須     | 発信番号                            |
| sendType         | String        | 1      | 必須     | 送信タイプ(0：Sms、1：Lms/Mms)          |
| title            | String        | 120    | オプション  | メッセージのタイトル(送信タイプがLms/MmSの場合は必須) |
| body             | String        | 4000   | 必須     | メッセージの内容                        |
| useYn            | 	String       | 1      | 	必須    | 	使用有無(Y/N)                      |
| attachFileIdList | List<Integer> | -      | オプション  | 添付ファイルID(fileId)                |

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": "",
    "resultMessage": ""
  }
}
```

#### テンプレートの登録例

| Http method | URL                                                                 |
|-------------|---------------------------------------------------------------------|
| POST        | https://api-sms.cloud.toast.com/sms/v2.2/appKeys/{appKey}/templates |

[Request body]

```json
{
  "categoryId": 199376,
  "templateId": "TemplateId",
  "templateName": "テンプレート送信例",
  "templateDesc": "テンプレート送信例",
  "sendNo": "01012341234",
  "sendType": "1",
  "title": "example",
  "body": "一般送信テスト用です。\r\n##key1##さん。\r\n##key2## です。",
  "useYn": "Y",
  "attachFileIdList": [
    123123,
456456
  ]
}
```

[Response]

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": "",
    "resultMessage": ""
  }
}
```

##### 説明

- 添付ファイル(フィールド名：attachFileIdList)を含むテンプレートの登録するためには、事前に添付ファイルのアップロードを行う必要があります。<br>
- [[添付ファイルアップロード](./api-guide/#binaryUpload)]</a> ガイドを参照してください。
- 付イメージ制限事項
    - サポートコーデック：.jpg
    - 添付イメージ数：2個以下
    - 添付イメージサイズ：300KB以下
    - 添付イメージ解像度：1000 x 1000以下

### テンプレート送信(本文修正が必要ない場合)

| Http method | 種類  | URL                                                                  |
|-------------|-----|----------------------------------------------------------------------|
| POST        | SMS | https://api-sms.cloud.toast.com/sms/v2.2/appKeys/{appKey}/sender/sms |
| POST        | MMS | https://api-sms.cloud.toast.com/sms/v2.2/appKeys/{appKey}/sender/mms |

Request URLは、テンプレート登録時に選択した送信タイプに選択して送信します。

**Request parameter body内の値が空欄の場合、該当templateIdのbody内容に置換します。**

[Request body]置換するキーと値をkey、valueに代入。

```json
{
  "templateId": "TemplateId",
  "senderGroupingKey": "SenderGroupingKey",
  "recipientList": [
    {
      "recipientNo": "01000000000",
      "templateParameter": {
        "key1": "Toast Cloud",
        "key2": "SMS"
      },
      "recipientGroupingKey": "RecipientGroupingKey"
    }
  ]
}
```

[Response]

```json
{
  "header": {
    "resultCode": 0,
    "resultMessage": "SUCCESS",
    "isSuccessful": true
  },
  "body": {
    "data": {
      "requestId": "20180813095534I4VcVuPBpd0",
      "statusCode": "2",
      "senderGroupingKey": "SenderGroupingKey",
      "sendResultList": [
        {
          "recipientNo": "01000000000",
          "resultCode": 0,
          "resultMessage": "SUCCESS",
          "recipientSeq": 1,
          "recipientGroupingKey": "RecipientGroupingKey"
        }
      ]
    }
  }
}
```

![[図1]テンプレート送信に成功](http://static.toastoven.net/prod_sms/img_27.png)

### テンプレート送信(本文修正が必要な場合)

#### テンプレート送信例

| Http method | 種類  | URL                                                                  |
|-------------|-----|----------------------------------------------------------------------|
| POST        | SMS | https://api-sms.cloud.toast.com/sms/v2.2/appKeys/{appKey}/sender/sms |
| POST        | MMS | https://api-sms.cloud.toast.com/sms/v2.2/appKeys/{appKey}/sender/mms |

Request URLは、テンプレート登録時に選択した送信タイプに選択して送信します。

**テンプレートIDとRequest parameter body内の値がある場合、発信番号と発信内容がテンプレート内容に置換されません。**

ただし、テンプレートIDを入力した場合、該当テンプレートで照会できます。

上記のような場合は、テンプレートを照会した後、テンプレートの内容を修正したい時に使用できます。

[Request body]

```json
{
  "templateId": "TemplateId",
  "body": "本文",
  "sendNo": "15446859",
  "senderGroupingKey": "SenderGroupingKey",
  "recipientList": [
    {
      "recipientNo": "01000000000",
      "templateParameter": {
        "key1": "Toast Cloud",
        "key2": "SMS"
      },
      "recipientGroupingKey": "RecipientGroupingKey"
    }
  ]
}
```

[Response]

```json
{
  "header": {
    "resultCode": 0,
    "resultMessage": "SUCCESS",
    "isSuccessful": true
  },
  "body": {
    "data": {
      "requestId": "20180813095534I4VcVuPBpd0",
      "statusCode": "2",
      "senderGroupingKey": "SenderGroupingKey",
      "sendResultList": [
        {
          "recipientNo": "01000000000",
          "resultCode": 0,
          "resultMessage": "SUCCESS",
          "recipientSeq": 1,
          "recipientGroupingKey": "RecipientGroupingKey"
        }
      ]
    }
  }
}
```

### テンプレートリストの照会

#### リクエスト

[URL]

```
GET  /sms/v2.2/appKeys/{appKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Query parameter]

| 値          | 	タイプ     | 	必須    | 	説明              |
|------------|----------|--------|------------------|
| categoryId | 	Integer | 	オプション | 	カテゴリーID         |
| useYn      | 	String  | 	オプション | 	使用(Y/N)         |
| pageNum    | 	Integer | オプション  | 	ページ番号(デフォルト値：1) |
| pageSize   | 	Integer | オプション  | 	検索数(デフォルト値：15)  |

#### レスポンス

```json
{
  "header": {
    "resultCode": 0,
    "resultMessage": "success",
    "isSuccessful": true
  },
  "body": {
    "pageNum": 1,
    "pageSize": 1000,
    "totalCount": 1,
    "data": [
      {
        "templateId": "0cc60fce-4251-44a0-bbdf-06b863ac2212",
        "serviceId": 71191,
        "categoryId": 415975,
        "categoryName": "categoryName",
        "sort": 0,
        "templateName": "templateName",
        "templateDesc": "templateDescription",
        "useYn": "Y",
        "priority": "S",
        "sendNo": "15771234",
        "sendType": "0",
        "sendTypeName": "SMS送信",
        "title": "title",
        "body": "body",
        "attachFileYn": "Y",
        "delYn": "N",
        "createDate": "2023-09-18 14:03:13.0",
        "createUser": null,
        "updateDate": "2023-09-18 14:03:13.0",
        "updateUser": null,
        "attachFileList": [
          {
            "fileId": 535162,
            "serviceId": 71191,
            "attachType": 2,
            "templateId": "0cc60fce-4251-44a0-bbdf-06b863ac2212",
            "filePath": "/permanent/71191/toast-mt-2023-09-18/1403/535162",
            "fileName": "attachment.jpg",
            "saveFileName": "20230918bc7eyh0.jpg",
            "fileSize": null,
            "createDate": "2023-09-18 14:03:11.0",
            "createUser": "CreateUser",
            "updateDate": null,
            "updateUser": null,
            "uploadType": "TEMPORARY",
            "existFileName": "20230918bc7eyh0.jpg"
          }
        ]
      }
    ]
  }
}
```

| 値                                          | 	タイプ     | 	説明                                |
|--------------------------------------------|----------|------------------------------------|
| header.isSuccessful                        | 	Boolean | 	成否                                |
| header.resultCode                          | 	Integer | 	失敗コード                             |
| header.resultMessage                       | 	String  | 	失敗メッセージ                           |
| body.pageNum                               | 	Integer | 	現在のページ番号                          |
| body.pageSize                              | 	Integer | 	照会されたデータ数                         |
| body.totalCount                            | 	Integer | 	総データ数                             |
| body.data[].templateId                     | 	String  | 	テンプレートID                          |
| body.data[].serviceId                      | 	Integer | 	サービスID(内部用、未使用値)                  |
| body.data[].categoryId                     | 	Integer | 	カテゴリーID                           |
| body.data[].categoryName                   | 	String  | 	カテゴリー名                            |
| body.data[].sort                           | 	Integer | 	ソート値                              |
| body.data[].templateName                   | 	String  | 	テンプレート名                           |
| body.data[].templateDesc                   | 	String  | 	テンプレートの説明                         |
| body.data[].useYn                          | 	String  | 	使用するかどうか                          |
| body.data[].priority                       | 	String  | 	優先順位値(未使用値)                       |
| body.data[].sendNo                         | 	String  | 	発信番号                              |
| body.data[].sendType                       | 	String  | 	送信タイプ(0：Sms、1：Mms、2：Auth)         |
| body.data[].sendTypeName                   | 	String  | 	送信タイプ名                            |
| body.data[].title                          | 	String  | 	タイトル                              |
| body.data[].body                           | 	String  | 	本文内容                              |
| body.data[].attachFileYn                   | 	String  | 	添付ファイルの有無(Y/N)                    |
| body.data[].delYn                          | 	String  | 	削除されているかどうか(Y/N)、現在ステータスの表記用にのみ使用 |
| body.data[].createDate                     | 	String  | 	登録日                               |
| body.data[].createUser                     | 	String  | 	登録したユーザー                          |
| body.data[].updateDate                     | 	String  | 	修正日                               |
| body.data[].updateUser                     | 	String  | 	修正したユーザー                          |
| body.data[].attachFileList[].fileId        | 	Integer | 	ファイルID                            |
| body.data[].attachFileList[].serviceId     | 	Integer | 	サービスID                            |
| body.data[].attachFileList[].attachType    | 	Integer | 	添付ファイルタイプ                         |
| body.data[].attachFileList[].templateId    | 	String  | 	テンプレートID                          |
| body.data[].attachFileList[].filePath      | 	String  | 	ファイル保存パス(内部用)                     |
| body.data[].attachFileList[].fileName      | 	String  | 	ファイル名                             |
| body.data[].attachFileList[].saveFileName  | 	String  | 	保存された添付ファイルの名前                    |
| body.data[].attachFileList[].fileSize      | 	Long    | 	添付ファイルの大きさ                        |
| body.data[].attachFileList[].createDate    | 	String  | 	作成日時                              |
| body.data[].attachFileList[].createUser    | 	String  | 	作成者                               |
| body.data[].attachFileList[].updateDate    | 	String  | 	修正日時                              |
| body.data[].attachFileList[].updateUser    | 	String  | 	修正者                               |
| body.data[].attachFileList[].uploadType    | 	String  | 	アップロードタイプ                         |
| body.data[].attachFileList[].existFileName | 	String  | 	保存された添付ファイルの名前                    |

### テンプレートの単一照会

#### リクエスト

[URL]

```
GET  /sms/v2.2/appKeys/{appKey}/templates/{templateId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値          | 	タイプ    | 	説明            |
|------------|---------|----------------|
| appKey     | 	String | 	固有のアプリケーションキー |
| templateId | 	String | 	テンプレートID      |

#### レスポンス

```json
{
  "header": {
    "resultCode": 0,
    "resultMessage": "success",
    "isSuccessful": true
  },
  "body": {
    "data": {
      "templateId": "accc7d00-8d50-4656-9a21-5dc82e8368e6",
      "serviceId": 71191,
      "categoryId": 415981,
      "categoryName": "categoryName",
      "sort": 0,
      "templateName": "templateName",
      "templateDesc": "templateDescription",
      "useYn": "Y",
      "priority": "S",
      "sendNo": "15771234",
      "sendType": "0",
      "sendTypeName": "SMS送信",
      "title": "title",
      "body": "body",
      "attachFileYn": "Y",
      "delYn": "N",
      "createDate": "2023-09-18 15:50:13.0",
      "createUser": null,
      "updateDate": "2023-09-18 15:50:13.0",
      "updateUser": null,
      "attachFileList": [
        {
          "fileId": 535190,
          "serviceId": 71191,
          "attachType": 2,
          "templateId": "accc7d00-8d50-4656-9a21-5dc82e8368e6",
          "filePath": "/permanent/71191/toast-mt-2023-09-18/1550/535190",
          "fileName": "attachment.jpg",
          "saveFileName": "20230918BppZWI0.jpg",
          "fileSize": null,
          "createDate": "2023-09-18 15:50:11.0",
          "createUser": "CreateUser",
          "updateDate": null,
          "updateUser": null,
          "uploadType": "TEMPORARY",
          "existFileName": "20230918BppZWI0.jpg"
        }
      ]
    }
  }
}
```

| 値                                     | 	タイプ     | 	説明                                       |
|---------------------------------------|----------|-------------------------------------------|
| header.isSuccessful                   | 	Boolean | 	成否                                       |
| header.resultCode                     | 	Integer | 	失敗コード                                    |
| header.resultMessage                  | 	String  | 	失敗メッセージ                                  |
| body.pageNum                          | 	Integer | 	現在のページ番号                                 |
| body.pageSize                         | 	Integer | 	照会されたデータ数                                |
| body.totalCount                       | 	Integer | 	総データ数                                    |
| body.data.templateId                  | 	String  | 	テンプレートID                                 |
| body.data.serviceId                   | 	Integer | 	サービスID(内部用、未使用値)                         |
| body.data.categoryId                  | 	Integer | 	カテゴリーID                                  |
| body.data.categoryName                | 	String  | 	カテゴリー名                                   |
| body.data.sort                        | 	Integer | 	ソート値                                     |
| body.data.templateName                | 	String  | 	テンプレート名                                  |
| body.data.templateDesc                | 	String  | 	テンプレートの説明                                |
| body.data.useYn                       | 	String  | 	使用するかどうか                                 |
| body.data.priority                    | 	String  | 	優先順位値(未使用値)                              |
| body.data.sendNo                      | 	String  | 	発信番号                                     |
| body.data.sendType                    | 	String  | 	送信タイプ(0：Sms、1：Mms、2：Auth)                |
| body.data.sendTypeName                | 	String  | 	送信タイプ名                                   |
| body.data.title                       | 	String  | 	タイトル                                     |
| body.data.body                        | 	String  | 	本文内容                                     |
| body.data.attachFileYn                | 	String  | 	添付ファイル有無(Y/N)                            |
| body.data.delYn                       | 	String  | 	削除されているかどうか(Y/N)、現在ステータスの表記用にのみ使用        |
| body.data.createDate                  | 	String  | 	登録日                                      |
| body.data.createUser                  | 	String  | 	登録したユーザー                                 |
| body.data.updateDate                  | 	String  | 	修正日                                      |
| body.data.updateUser                  | 	String  | 	修正したユーザー                                 |
| body.data.attachFileList[].fileId     | 	Integer | 	添付ファイルID                                 |
| body.data.attachFileList[].serviceId  | 	Integer | 	サービスID(内部用、未使用値)                         |
| body.data.attachFileList[].attachType | 	Integer | 	添付ファイルのアップロードタイプ(0：臨時、1：アップロード、2：テンプレート) |
| body.data.attachFileList[].templateId | 	String  | 	テンプレートID                                 |
| body.data.attachFileList[].filePath   | 	String  | 	添付ファイルのパス                                |
| body.data.attachFileList[].fileName   | 	String  | 	添付ファイル名                                  |
| body.data.attachFileList[].fileSize   | Integer  | ファイルサイズ                                   |
| body.data.attachFileList[].createDate | 	String  | 	添付ファイルの登録日                               |
| body.data.attachFileList[].createUser | 	String  | 	添付ファイル登録ユーザー                             |

### テンプレートの修正

#### リクエスト

[URL]

```
PUT  /sms/v2.2/appKeys/{appKey}/templates/{templateId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Request body]

```json
{
  "templateName": "",
  "templateDesc": "",
  "sendNo": "",
  "sendType": "",
  "title": "",
  "body": "",
  "useYn": "",
  "attachFileIdList": [
    0,
1
  ]
}
```

| 値                | 	タイプ          | 	最大文字数 | 必須     | 	説明                             |
|------------------|---------------|--------|--------|---------------------------------|
| templateName     | 	String       | 50     | 	必須    | 	テンプレート名                        |
| templateDesc     | 	String       | 100    | 	オプション | 	テンプレートの説明                      |
| sendNo           | String        | 13     | 必須     | 発信番号                            |
| sendType         | String        | 1      | 必須     | 送信タイプ(0：Sms、1：Lms/Mms)          |
| title            | String        | 120    | オプション  | メッセージのタイトル(送信タイプがLms/MmSの場合は必須) |
| body             | String        | 4000   | 必須     | メッセージの内容                        |
| useYn            | 	String       | 1      | 	必須    | 	使用有無(Y/N)                      |
| attachFileIdList | List<Integer> | -      | オプション  | 添付ファイルID(fileId)                |

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": "",
    "resultMessage": ""
  }
}
```

### テンプレートの削除

#### リクエスト

[URL]

```
DELETE  /sms/v2.2/appKeys/{appKey}/templates/{templateId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値          | 	タイプ    | 	説明            |
|------------|---------|----------------|
| appKey     | 	String | 	固有のアプリケーションキー |
| templateId | 	String | 	テンプレートID      |

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": "",
    "resultMessage": ""
  }
}
```

## 080受信拒否サービス

### ﻿受信拒否対象者を登録

#### リクエスト

[URL]

```
POST /sms/v2.2/appKeys/{appKey}/blockservice/recipients
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Request body]

```json
{
  "unsubscribeNo": "0800000000",
  "recipientNoList": [
    "0100000000",
    "0100000001"
  ]
}
```

| 値               | 	タイプ         | 	最大文字数 | 必須 | 	説明           |
|-----------------|--------------|--------|----|---------------|
| unsubscribeNo   | String       | 25     | O  | 080受信拒否番号     |
| recipientNoList | List<String> | 10     | O  | 追加する受信拒否対象者番号 |

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "Success"
  }
}
```

### 受信拒否対象者の照会

#### リクエスト

[URL]

```
GET  /sms/v2.2/appKeys/{appKey}/blockservice/recipients
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Query parameter]

| 値                | 	タイプ     | 	最大  | 必須     | 	説明                                 |
|------------------|----------|------|--------|-------------------------------------|
| unsubscribeNo    | 	String  | 25   | 	必須    | 	080受信拒否番号                          |
| recipientNo      | String   | 25   | オプション  | 受信拒否番号                              |
| startRequestDate | 	String  | -    | 	オプション | 	受信拒否リクエストの開始値(yyyy-MM-dd HH:mm:ss) |
| endRequestDate   | 	String  | -    | 	オプション | 	受信拒否リクエストの終了値(yyyy-MM-dd HH:mm:ss) |
| pageNum          | 	Integer | -    | 	オプション | 	ページ番号(デフォルト値：1)                    |
| pageSize         | 	Integer | 1000 | 	オプション | 	照会数(デフォルト値：15)                     |

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "pageNum": 1,
    "pageSize": 15,
    "totalCount": 1,
    "data": [
      {
        "unsubscribeNo": "0808888888",
        "recipientNo": "01000000000",
        "requestDate": "2018-05-14 17:07:29.0"
      }
    ]
  }
}
```

### 受信拒否対象者の削除

#### リクエスト

[URL]

```
DELETE  /sms/v2.2/appKeys/{appKey}/blockservice/recipients/removes?unsubscribeNo={unsubscribeNo}&updateUser={updateUser}&recipientNoList={recipientNo},{recipientNo}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Query parameter]

| 値             | 	タイプ    | 	最大  | 必須  | 	説明         |
|---------------|---------|------|-----|-------------|
| unsubscribeNo | 	String | 20   | 	必須 | 	080受信拒否番号  |
| updateUser    | 	String | 	100 | 必須  | 	受信拒否削除者    |
| recipientNo   | 	String | 	20  | 必須  | 	削除する受信拒否番号 |

#### レスポンス

```json
{
  "header": {
    "resultCode": 0,
    "resultMessage": "SUCCESS",
    "isSuccessful": true
  },
  "body": null
}
```

## 発信番号

### 登録された発信番号リストの照会API

#### リクエスト

[URL]

| Http method | 	URI                                                                                                                      |
|-------------|---------------------------------------------------------------------------------------------------------------------------|
| GET         | 	/sms/v2.2/appKeys/{appKey}/sendNos?sendNo={sendNo}&useYn={useYn}&blockYn={blockYn}&pageNum={pageNum}&pageSize={pageSize} |

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Query parameter]

| 値        | 	タイプ     | 	説明             |
|----------|----------|-----------------|
| sendNo   | String   | 発信番号            |
| useYn    | String   | 使用するかどうか        |
| blockYn  | String   | 遮断するかどうか        |
| pageNum  | 	Integer | ページ番号(デフォルト値：1) |
| pageSize | 	Integer | 照会数(デフォルト値：15)  |

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "pageNum": 1,
    "pageSize": 15,
    "totalCount": 2,
    "data": [
      {
        "appKey": null,
        "serviceId": 71191,
        "sendNo": "01012341234",
        "useYn": "Y",
        "blockYn": "N",
        "blockReason": null,
        "createDate": "2023-07-18 12:05:35",
        "createUser": "test@nhn.com",
        "updateDate": "2023-07-31 13:45:48",
        "updateUser": "test@nhn.com"
      },
      {
        "appKey": null,
        "serviceId": 71191,
        "sendNo": "12341234",
        "useYn": "Y",
        "blockYn": "N",
        "blockReason": null,
        "createDate": "2023-09-14 10:30:30",
        "createUser": "test@nhn.com",
        "updateDate": "2023-09-14 10:30:30",
        "updateUser": null
      }
    ]
  }
}
```

| 値                       | 	タイプ     | 	説明        |
|-------------------------|----------|------------|
| header.isSuccessful     | 	Boolean | 	成否        |
| header.resultCode       | 	Integer | 	失敗コード     |
| header.resultMessage    | 	String  | 	失敗メッセージ   |
| body.pageNum            | 	Integer | 	ページ番号     |
| body.pageSize           | 	Integer | 	照会されたデータ数 |
| body.totalCount         | 	Integer | 	総データ数     |
| body.data[].serviceId   | Integer  | サービスID     |
| body.data[].sendNo      | String   | 発信番号       |
| body.data[].useYn       | String   | 使用するかどうか   |
| body.data[].blockYn     | String   | 遮断するかどうか   |
| body.data[].blockReason | String   | 遮断理由       |
| body.data[].createDate  | String   | 作成日時       |
| body.data[].createUser  | String   | 作成者        |
| body.data[].updateDate  | String   | 修正日        |
| body.data[].updateUser  | String   | 修正したユーザー   |

## 統計照会

### 統合統計照会

#### リクエスト

[URL]

| Http method | 	URI                                                                                                                                                                  |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| GET         | 	/sms/v2.2/appKeys/{appKey}/statistics/view?searchType={searchType}&from={from}&to={to}&messageTypes={messageType}&contentTypes={contentType}&templateId={templateId} |

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Query parameter]

| 値           | 	タイプ   | 	最大 | 必須 | 説明                                             |
|-------------|--------|-----|----|------------------------------------------------|
| searchType  | String | 10  | O  | 統計区分<br/>DATE：日付別、TIME：時間別、DAY：曜日別             |
| from        | String | -   | O  | 統計照会開始日時<br/>yyyy-MM-dd HH:mm                  |
| to          | String | -   | O  | 統計照会の終了日時<br/>yyyy-MM-dd HH:mm                 |
| messageType | String | 10  | X  | メッセージタイプ<br/>SMS：短文、LMS：長文、MMS：添付ファイル、AUTH：認証用 |
| contentType | String | 10  | X  | コンテンツタイプ<br/>NORMAL：一般、AD：広告                   |
| templateId  | String | 50  | X  | テンプレートID                                       |

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "data": [
      {
        "divisionName": "2018-06-01",
        "statisticsView": {
          "requestedCount": 10,
          "succeedCount": 10,
          "failedCount": 0,
          "pendingCount": 0,
          "succeedRate": "100.00",
          "failedRate": "0.00",
          "pendingRate": "0.00"
        }
      }
    ]
  }
}
```

| 値                          | タイプ      | 説明               |
|----------------------------|----------|------------------|
| header.isSuccessful        | 	Boolean | 	成否              |
| header.resultCode          | 	Integer | 	失敗コード           |
| header.resultMessage       | 	String  | 	失敗メッセージ         |
| body.data[].divisionName   | String   | 表示名<br/>日付、時間、曜日 |
| body.data[].statisticsView | Object   |                  |
| body.data[].requestedCount | Integer  | リクエスト数           |
| body.data[].succeedCount   | Integer  | 成功数              |
| body.data[].failedCount    | Integer  | 失敗数              |
| body.data[].pendingCount   | Integer  | 送信中の数            |
| body.data[].succeedRate    | String   | 成功比率             |
| body.data[].failedRate     | String   | 失敗比率             |
| body.data[].pendingRate    | String   | 送信中の比率           |

## 予約送信

### 予約送信リストの照会

#### リクエスト

[URL]

```
GET /sms/v2.2/appKeys/{appKey}/reservations
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Query parameter]

| 値                | 	タイプ     | 	最大  | 必須     | 	説明                                                                                                    |
|------------------|----------|--------|-----|------------------------------------------------------------------------------------------------------------------------------------------------------|
| sendType         | String   | 1    | オプション  | 送信タイプ<br/>(0：SMS、1：LMS/MMS、2：AUTH)                                                                     |
| requestId        | 	String  | 25   | 	オプション | 	リクエストID                                                                                               |
| startRequestDate | 	String  | -    | 	オプション | 	送信日の開始値(yyyy-MM-dd HH:mm:ss)                                                                          |
| endRequestDate   | 	String  | -    | 	オプション | 	送信日の終了値(yyyy-MM-dd HH:mm:ss)                                                                          |
| startCreateDate  | 	String  | -    | 	オプション | 	送信日の開始値(yyyy-MM-dd HH:mm:ss)                                                                          |
| endCreateDate    | 	String  | -    | 	オプション | 	送信日の終了値(yyyy-MM-dd HH:mm:ss)                                                                          |
| sendNo           | 	String  | 13   | オプション  | 	発信番号                                                                                                  |
| recipientNo      | 	String  | 20   | 	オプション | 	受信番号                                                                                                  |
| templateId       | 	String  | 50   | 	オプション | 	テンプレート番号                                                                                              |
| messageStatus    | 	String  | 10   | 	オプション | 	メッセージのステータス<br/>(RESERVED：予約待機、SENDING：送信中、COMPLETED：送信完了、FAILED：送信失敗、CANCEL：キャンセル、DUPLICATED：重複送信、FAILED_AD:失敗(広告制限)、再送信待機(広告制限)) |
| receiverRegion   | 	String  | -    | 	オプション | 	国内/国際 (DOMESTIC: 国内, INTERNATIONAL: 国際) |
| countryCode      | 	String  | -    | 	オプション | 	国コード [[送信可能国](./international-sending-policy/#_5)] |
| pageNum          | 	Integer | -    | 	オプション | 	ページ番号(デフォルト値：1)                                                                                       |
| pageSize         | 	Integer | 1000 | 	オプション | 	照会数(デフォルト値：15)                                                                                        |

#### レスポンス

```json
{
  "header": {
    "resultCode": 0,
    "resultMessage": "SUCCESS",
    "isSuccessful": true
  },
  "body": {
    "pageNum": 1,
    "pageSize": 15,
    "totalCount": 15,
    "data": [
      {
        "requestId": "{リクエストID}",
        "recipientSeq": 1,
        "requestDate": "{予約日}",
        "sendNo": "{発信番号}",
        "recipientNo": "{受信番号}",
        "countryCode": "{国コード}",
        "sendType": "{送信タイプ}",
        "messageType": "{メッセージタイプ}",
        "adYn": "{広告かどうか}",
        "templateId": "{テンプレートID}",
        "templateParameter": "{テンプレートパラメータ}",
        "templateName": "{テンプレート名}",
        "title": "{タイトル}",
        "body": "{内容}",
        "messageStatus": "{メッセージのステータス}",
        "createUser": "{登録したユーザー}",
        "createDate": "{登録日時}",
        "updateDate": "{修正日}"
      }
    ]
  }
}
```

| 値                             | 	タイプ          | 	説明                                                                                                    |
|-------------------------------|---------------|------------------------------------------------------------------------------------------------------------------------------------------------------|
| header.isSuccessful           | 	Boolean      | 	成否                                                                                                    |
| header.resultCode             | 	Integer      | 	失敗コード                                                                                                 |
| header.resultMessage          | 	String       | 	失敗メッセージ                                                                                               |
| body.pageNum                  | 	Integer      | 	現在のページ番号                                                                                              |
| body.pageSize                 | 	Integer      | 	照会されたデータ数                                                                                             |
| body.totalCount               | 	Integer      | 	総データ数                                                                                                 |
| body.data[].requestId         | 	String       | 	リクエストID                                                                                               |
| body.data[].recipientSeq      | 	Integer      | 	受信者シーケンス                                                                                              |
| body.data[].requestDate       | 	String       | 	発信日時                                                                                                  |
| body.data[].sendNo            | 	String       | 	発信番号                                                                                                  |
| body.data[].recipientNo       | 	String       | 	受信番号                                                                                                  |
| body.data[].countryCode       | 	String       | 	国番号                                                                                                   |
| body.data[].sendType          | 	String       | 	送信タイプ(0：Sms、1：Mms、2：Auth)                                                                             |
| body.data[].messageType       | 	String       | 	メッセージタイプ<br/>(SMS,LMS,MMS,AUTH)                                                                       |
| body.data[].adYn              | 	String       | 	広告かどうか                                                                                                |
| body.data[].templateId        | 	String       | 	テンプレートID                                                                                              |
| body.data[].templateParameter | 	String(json) | 	テンプレートパラメータ                                                                                           |
| body.data[].templateName      | 	String       | 	テンプレート名                                                                                               |
| body.data[].title             | 	String       | 	タイトル                                                                                                  |
| body.data[].body              | 	String       | 	本文内容                                                                                                  |
| body.data[].messageStatus     | 	String       | 	メッセージのステータス<br/>(RESERVED：予約待機、SENDING：送信中、COMPLETED：送信完了、FAILED：送信失敗、CANCEL：キャンセル、DUPLICATED：重複送信、FAILED_AD:失敗(広告制限)、再送信待機(広告制限)) |
| body.data[].createUser        | 	String       | 	登録したユーザー                                                                                              |
| body.data[].createDate        | 	String       | 	登録日                                                                                                   |
| body.data[].updateDate        | 	String       | 	修正日                                                                                                   |

### 予約送信の詳細照会

#### リクエスト

[URL]

```
GET /sms/v2.2/appKeys/{appKey}/reservations/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値            | 	タイプ     | 	説明            |
|--------------|----------|----------------|
| appKey       | 	String  | 	固有のアプリケーションキー |
| requestId    | 	String  | 	リクエストID       |
| recipientSeq | 	Integer | 	受信者シーケンス      |

#### レスポンス

```json
{
  "header": {
    "resultCode": 0,
    "resultMessage": "SUCCESS",
    "isSuccessful": true
  },
  "body": {
    "data": {
      "requestId": "{リクエストID}",
      "recipientSeq": 1,
      "requestDate": "{予約日}",
      "sendNo": "{発信番号}",
      "recipientNo": "{受信番号}",
      "countryCode": "{国コード}",
      "sendType": "{送信タイプ}",
      "messageType": "{メッセージタイプ}",
      "adYn": "{広告かどうか}",
      "templateId": "{テンプレートID}",
      "templateParameter": "{テンプレートパラメータ}",
      "templateName": "{テンプレート名}",
      "title": "{タイトル}",
      "body": "{内容}",
      "messageStatus": "{メッセージのステータス}",
      "createUser": "{登録したユーザー}",
      "createDate": "{登録日時}",
      "updateDate": "{修正日}",
      "attachFileList": [
        {
          "fileId": 0,
          "filePath": "26606/toast-mt-2018-02-07/1555/105887/",
          "fileName": "file_attach_test.jpg"
        }
      ]
    }
  }
}
```

| 値                                   | 	タイプ          | 	説明                                                                                                    |
|-------------------------------------|---------------|--------------------------------------------------------------------------------------------------------|
| header.isSuccessful                 | 	Boolean      | 	成否                                                                                                    |
| header.resultCode                   | 	Integer      | 	失敗コード                                                                                                 |
| header.resultMessage                | 	String       | 	失敗メッセージ                                                                                               |
| body.pageNum                        | 	Integer      | 	現在のページ番号                                                                                              |
| body.pageSize                       | 	Integer      | 	照会されたデータ数                                                                                             |
| body.totalCount                     | 	Integer      | 	総データ数                                                                                                 |
| body.data.requestId                 | 	String       | 	リクエストID                                                                                               |
| body.data.recipientSeq              | 	Integer      | 	受信者シーケンス                                                                                              |
| body.data.requestDate               | 	String       | 	発信日時                                                                                                  |
| body.data.sendNo                    | 	String       | 	発信番号                                                                                                  |
| body.data.recipientNo               | 	String       | 	受信番号                                                                                                  |
| body.data.countryCode               | 	String       | 	国番号                                                                                                   |
| body.data.sendType                  | 	String       | 	送信タイプ(0：Sms、1：Mms、2：Auth)                                                                             |
| body.data.messageType               | 	String       | 	メッセージタイプ<br/>(SMS、LMS、MMS、AUTH)                                                                       |
| body.data.adYn                      | 	String       | 	広告かどうか                                                                                                |
| body.data.templateId                | 	String       | 	テンプレートID                                                                                              |
| body.data.templateParameter         | 	String(json) | 	テンプレートパラメータ                                                                                           |
| body.data.templateName              | 	String       | 	テンプレート名                                                                                               |
| body.data.title                     | 	String       | 	タイトル                                                                                                  |
| body.data.body                      | 	String       | 	本文内容                                                                                                  |
| body.data.messageStatus             | 	String       | 	メッセージのステータス<br/>(RESERVED：予約待機、SENDING：送信中、COMPLETED：送信完了、FAILED：送信失敗、CANCEL：キャンセル、DUPLICATED：重複送信、FAILED_AD:失敗(広告制限)、再送信待機(広告制限)) |
| body.data.createUser                | 	String       | 	登録したユーザー                                                                                              |
| body.data.createDate                | 	String       | 	登録日                                                                                                   |
| body.data.attachFileList[].fileId   | 	Integer      | 	ファイルID                                                                                                |
| body.data.attachFileList[].filePath | 	String       | 	ファイルパス(内部用)                                                                                           |
| body.data.attachFileList[].fileName | 	String       | 	ファイル名                                                                                                 |

### 予約送信の取消

#### リクエスト

[URL]

```
PUT /sms/v2.2/appKeys/{appKey}/reservations/cancel
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Request body]

```json
{
  "reservationList": [
    {
      "requestId": "{requestId}",
      "recipientSeq": 1
    }
  ],
  "updateUser": "{updateUser}"
}
```

| 値                              | 	タイプ    | 	最大 | 必須 | 	説明         |
|--------------------------------|---------|-----|----|-------------|
| reservationList[].requestId    | String  | 25  | O  | リクエストID     |
| reservationList[].recipientSeq | Integer | -   | O  | 受信者シーケンス    |
| updateUser                     | String  | 100 | O  | キャンセルリクエスト者 |

[Response body]

```json
{
  "header": {
    "resultCode": 0,
    "resultMessage": "SUCCESS",
    "isSuccessful": true
  },
  "body": {
    "data": {
      "requestedCount": 1,
      "canceledCount": 1
    }
  }
}
```

| 値                        | 	タイプ     | 	説明           |
|--------------------------|----------|---------------|
| header.isSuccessful      | 	Boolean | 	成否           |
| header.resultCode        | 	Integer | 	失敗コード        |
| header.resultMessage     | 	String  | 	失敗メッセージ      |
| body.data.requestedCount | 	Integer | 	キャンセルリクエスト件数 |
| body.data.canceledCount  | 	Integer | 	キャンセル成功件数    |

## 送信結果ファイルのダウンロード

### 照会ファイル作成リクエスト

#### リクエスト

[URL]

```
POST /sms/v2.2/appKeys/{appKey}/sender/download-reservations
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Request body]

* requestIdまたはstartRequestDate + endRequestDateまたはstartCreateDate + endCreateDateは必須です。
* 登録日時と発信日時を同時に照会する場合、発信日時は無視されます。

```json
{
  "sendType": "1",
  "requestId": "20190601100630ReZQ6KZzAH0",
  "startRequestDate": "2019-06-01 00:00:00",
  "endRequestDate": "2019-06-08 00:00:00",
  "startCreateDate": "2019-06-01 00:00:00",
  "endCreateDate": "2019-06-08 00:00:00",
  "startResultDate": "2019-06-01 00:00:00",
  "endResultDate": "2019-06-08 00:00:00",
  "sendNo": "15446859",
  "recipientNo": "01000000000",
  "templateId": "TemplateId",
  "msgStatus": "3",
  "resultCode": "MTR2",
  "subResultCode": "MTR2_3",
  "senderGroupingKey": "{送信者グループキー}",
  "recipientGroupingKey": "{受信者グループキー}",
  "isIncludeTitleAndBody": true
}
```

| 値                     | 	タイプ    | 	最大長さ | 必須        | 	説明                                                     |
|-----------------------|---------|-------|-----------|---------------------------------------------------------|
| sendType              | String  | 1     | 必須        | 送信タイプ(0：Sms、1：Mms、2：Auth)                               |
| requestId             | 	String | 25    | 	必須条件(1番) | 	リクエストID                                                |
| startRequestDate      | 	String | -     | 	必須条件(2番) | 	送信日開始値(yyyy-MM-dd HH:mm:ss)                            |
| endRequestDate        | 	String | -     | 	必須条件(2番) | 	送信日終了値(yyyy-MM-dd HH:mm:ss)                            |
| startResultDate       | 	String | -     | 	オプション    | 	受信日開始値(yyyy-MM-dd HH:mm:ss)                            |
| endResultDate         | 	String | -     | 	オプション    | 	受信日終了値(yyyy-MM-dd HH:mm:ss)                            |
| sendNo                | 	String | 13    | 	オプション    | 	発信番号                                                   |
| recipientNo           | 	String | 20    | 	オプション    | 	受信番号                                                   |
| templateId            | 	String | 50    | 	オプション    | 	テンプレート番号                                               |
| msgStatus             | 	String | 1     | 	オプション    | メッセージステータスコード(0：失敗、 1：リクエスト、 2：処理中、 3：成功、 4：予約キャンセル、 5：重複失敗、 6:失敗(広告制限)、 再送信待機(広告制限)) |
| resultCode            | 	String | 10    | 	オプション    | 	受信結果コード[[照会コード表](./error-code/#_2)]                    |
| subResultCode         | 	String | 10    | 	オプション    | 	受信結果詳細コード[[照会コード表](./error-code/#_3)]                  |
| senderGroupingKey     | 	String | 100   | 	オプション    | 	送信者グループキー                                              |
| recipientGroupingKey  | 	String | 100   | 	オプション    | 	受信者グループキー                                              |
| isIncludeTitleAndBody | Boolean | -     | オプション     | タイトル、本文を含めるかどうか                                         |

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "success"
  },
  "body": {
    "data": {
      "downloadId": "20190610100630ReZQ6KZzAH0",
      "downloadType": "NORMAL",
      "fileType": "CSV",
      "downloadStatusCode": "COMPLETED",
      "expiredDate": "2019-07-09 10:06:00.0"
    }
  }
}
```

| 値                            | 	タイプ     | 	説明                                                                                                         |
|------------------------------|----------|-------------------------------------------------------------------------------------------------------------|
| header.isSuccessful          | 	Boolean | 	成否                                                                                                         |
| header.resultCode            | 	Integer | 	失敗コード                                                                                                      |
| header.resultMessage         | 	String  | 	失敗メッセージ                                                                                                    |
| body.data.downloadId         | 	String  | 	ダウンロードID                                                                                                   |
| body.data.downloadType       | 	String  | 	ダウンロードタイプ<br/>- BLOCK：受信拒否<br/>- NORMAL：一般送信<br/>- MASS：大量送信<br/>- TAG：タグ送信                                |
| body.data.fileType           | 	String  | 	ファイルタイプ(現在csvのみサポート)                                                                                       |
| body.data.downloadStatusCode | 	String  | 	ファイル作成状態<br/>- READY：作成準備<br/>- MAKING：作成中<br/>- COMPLETED：作成完了<br/>- FAILED：作成失敗<br/>- EXPIRED：ダウンロード期間終了 |
| body.data.expiredDate        | 	String  | 	ダウンロード期間終了日時                                                                                               |

### 送信結果ファイル作成リクエストの履歴照会

#### リクエスト

[URL]

```
GET /sms/v2.2/appKeys/{appKey}/download-reservations?downloadId={downloadId}&downloadStatusCode={downloadStatusCode}&pageNum={pageNum}&pageSize={pageSize}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Query parameter]

| 値                  | 	タイプ     | 最大長さ  | 	必須   | 	説明                  |
|--------------------|----------|-------|-------|----------------------|
| downloadId         | 	String  | 	25   | オプション | ダウンロードID             |
| downloadStatusCode | 	String  | 10    | オプション | 	ダウンロードファイルのステータスコード |
| pageNum            | 	Integer | 	-    | オプション | ページ番号(デフォルト値：1)      |
| pageSize           | 	Integer | 	1000 | オプション | 照会数(デフォルト値：15)       |

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "totalCount": 0,
    "data": [
      {
        "downloadId": "",
        "downloadType": "",
        "fileType": "",
        "parameter": "",
        "size": 0,
        "downloadStatusCode": "",
        "resultMessage": "",
        "expiredDate": "",
        "createUser": "",
        "createDate": "",
        "updateDate": ""
      }
    ]
  }
}
```

| 値                              | 	タイプ     | 	説明                                                                                                        |
|--------------------------------|----------|------------------------------------------------------------------------------------------------------------|
| header.isSuccessful            | 	Boolean | 	成否                                                                                                        |
| header.resultCode              | 	Integer | 	失敗コード                                                                                                     |
| header.resultMessage           | 	String  | 	失敗メッセージ                                                                                                   |
| body.totalCount                | Integer  | 総件数                                                                                                        |
| body.data[].downloadId         | String   | ダウンロードID                                                                                                   |
| body.data[].downloadType       | String   | ダウンロードタイプ<br/>- BLOCK：受信拒否<br/>- NORMAL：一般送信<br/>- MASS：大量送信<br/>- TAG：タグ送信                                |
| body.data[].fileType           | String   | ファイルタイプ                                                                                                    |
| body.data[].parameter          | String   | リクエストパラメータ                                                                                                 |
| body.data[].size               | Integer  | 照会データサイズ                                                                                                   |
| body.data[].downloadStatusCode | String   | ファイル作成状態<br/>- READY：作成準備<br/>- MAKING：作成中<br/>- COMPLETED：作成完了<br/>- FAILED：作成失敗<br/>- EXPIRED：ダウンロード期間終了 |
| body.data[].resultMessage      | String   | 結果メッセージ(失敗時のレスポンス)                                                                                         |
| body.data[].expiredDate        | String   | ファイル有効期限                                                                                                   |
| body.data[].createUser         | String   | ファイル作成リクエスト者                                                                                               |
| body.data[].createDate         | String   | ファイル作成リクエスト日時                                                                                              |
| body.data[].updateDate         | String   | ファイル作成完了、失敗日時                                                                                              |

### 送信結果ファイルのダウンロードリクエスト

#### リクエスト

[URL]

```
GET /sms/v2.2/appKeys/{appKey}/download-reservations/{downloadId}/download
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値          | 	タイプ    | 	説明            |
|------------|---------|----------------|
| appKey     | 	String | 	固有のアプリケーションキー |
| downloadId | String  | ダウンロードID       |

#### レスポンス

```
file byte
```

## タグ管理

### タグ照会

#### リクエスト

[URL]

```
GET /sms/v2.2/appKeys/{appKey}/tags?pageNum={pageNum}&pageSize={pageSize}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Query parameter]

| 値        | 	タイプ     | 最大長さ  | 	必須   | 	説明   |
|----------|----------|-------|-------|-------|
| pageNum  | 	Integer | 	-    | オプション | オプション | ページ番号(デフォルト値：1)|
| pageSize | 	Integer | 	1000 | オプション | オプション | 照会数(デフォルト値：15)|

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "pageNum": 1,
    "pageSize": 1,
    "totalCount": 1,
    "data": [
      {
        "tagId": "ABCD1234",
        "tagName": "TAG",
        "createdDate": "2019-01-01 00:00:00",
        "updatedDate": "2019-01-01 00:00:00"
      }
    ]
  }
}
```

| 値                       | 	タイプ     | 	説明      |
|-------------------------|----------|----------|
| header.isSuccessful     | 	Boolean | 	成否      |
| header.resultCode       | 	Integer | 	失敗コード   |
| header.resultMessage    | 	String  | 	失敗メッセージ |
| body.pageNum            | 	Integer | 	ページ番号   |
| body.pageSize           | 	Integer | 	照会数     |
| body.totalCount         | 	Integer | 	総件数     |
| body.data[].tagId       | String   | タグID     |
| body.data[].tagName     | String   | タグ名      |
| body.data[].createdDate | String   | 作成日時     |
| body.data[].tagId       | String   | 修正日時     |

### タグ登録

[URL]

```
POST /sms/v2.2/appKeys/{appKey}/tags
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Request body]

```json
{
  "tagName": "TAG"
}
```

| 値       | 	タイプ   | 最大長さ | 	必須 | 	説明 |
|---------|--------|------|-----|-----|
| tagName | String | 30   | 必須  | タグ名 |

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "data": {
      "tagId": "ABCD1234"
    }
  }
}
```

| 値                    | 	タイプ     | 	説明      |
|----------------------|----------|----------|
| header.isSuccessful  | 	Boolean | 	成否      |
| header.resultCode    | 	Integer | 	失敗コード   |
| header.resultMessage | 	String  | 	失敗メッセージ |
| body.data.tagId      | String   | タグID     |

### タグ修正

[URL]

```
PUT /sms/v2.2/appKeys/{appKey}/tags/{tagId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |
| tagId  | 	String | 	タグID          |

[Request body]

```json
{
  "tagName": "TAG"
}
```

| 値       | 	タイプ   | 最大長さ | 	必須 | 	説明 |
|---------|--------|------|-----|-----|
| tagName | String | 30   | 必須  | タグ名 |

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": null
}
```

| 値                    | 	タイプ     | 	説明      |
|----------------------|----------|----------|
| header.isSuccessful  | 	Boolean | 	成否      |
| header.resultCode    | 	Integer | 	失敗コード   |
| header.resultMessage | 	String  | 	失敗メッセージ |

### タグ削除

[URL]

```
DELETE /sms/v2.2/appKeys/{appKey}/tags/{tagId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |
| tagId  | 	String | 	タグID          |

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": null
}
```

| 値                    | 	タイプ     | 	説明      |
|----------------------|----------|----------|
| header.isSuccessful  | 	Boolean | 	成否      |
| header.resultCode    | 	Integer | 	失敗コード   |
| header.resultMessage | 	String  | 	失敗メッセージ |

## UIDの管理

### UIDの照会

#### リクエスト

[URL]

```
GET /sms/v2.2/appKeys/{appKey}/uids?wheres={wheres}&offsetUid={offsetUid}&offset={offset}&limit={limit}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Query parameter]

| 値         | 	タイプ          | 最大長さ | 	必須   | 	説明                                                                              |
|-----------|---------------|------|-------|----------------------------------------------------------------------------------|
| wheres    | 	List<String> | 	-   | オプション | 照会条件。<br/>英数字、括弧で構成された文字列。<br/>括弧は1個、AND、ORは3個まで使用できる。<br/>(例) tagId1,AND,tagId2 |
| offsetUid | 	String       | 	-   | オプション | offset UID                                                                       |
| offset    | Integer       | -    | オプション | offset(Default : 0)                                                              |
| limit     | Integer       | 1000 | オプション | 照会件数(Default：15)                                                                 |

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "data": {
      "uids": [
        {
          "uid": "UID",
          "tags": [
            {
              "tagId": "ABCD1234",
              "tagName": "TAG",
              "createdDate": "2019-01-01 00:00:00",
              "updatedDate": "2019-01-01 00:00:00"
            }
          ],
          "contacts": [
            {
              "contactType": "PHONE_NUMBER",
              "contact": "test@nhn.com",
              "createdDate": "2019-01-01 00:00:00"
            }
          ]
        }
      ],
      "last": false,
      "totalCount": 5
    }
  }
}
```

| 値                                       | 	タイプ     | 	説明         |
|-----------------------------------------|----------|-------------|
| header.isSuccessful                     | 	Boolean | 	成否         |
| header.resultCode                       | 	Integer | 	失敗コード      |
| header.resultMessage                    | 	String  | 	失敗メッセージ    |
| body.data.uids[].uid                    | String   | UID         |
| body.data.uids[].tags[].tagId           | String   | タグID        |
| body.data.uids[].tags[].tagName         | String   | タグ名         |
| body.data.uids[].tags[].createdDate     | String   | タグ 作成日時     |
| body.data.uids[].tags[].updatedDate     | String   | タグ 修正日時     |
| body.data.uids[].contacts[].contactType | String   | 連絡先タイプ      |
| body.data.uids[].contacts[].contact     | String   | 連絡先(携帯電話番号) |
| body.data.uids[].contacts[].createdDate | String   | 連絡先作成日時     |
| body.data.uids[].last                   | Boolean  | 最後のリストかどうか  |

### UID単件照会

#### リクエスト

[URL]

```
GET /sms/v2.2/appKeys/{appKey}/uids/{uid}
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |
| uid    | 	String | 	UID           |

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "data": {
      "uid": "UID",
      "tags": [
        {
          "tagId": "ABCD1234",
          "tagName": "TAG",
          "createdDate": "2019-01-01 00:00:00",
          "updatedDate": "2019-01-01 00:00:00"
        }
      ],
      "contacts": [
        {
          "contactType": "PHONE_NUMBER",
          "contact": "0100000000",
          "createdDate": "2019-01-01 00:00:00"
        }
      ]
    }
  }
}
```

| 値                                | 	タイプ     | 	説明         |
|----------------------------------|----------|-------------|
| header.isSuccessful              | 	Boolean | 	成否         |
| header.resultCode                | 	Integer | 	失敗コード      |
| header.resultMessage             | 	String  | 	失敗メッセージ    |
| body.data.uid                    | String   | UID         |
| body.data.tags[].tagId           | String   | タグID        |
| body.data.tags[].tagName         | String   | タグ名         |
| body.data.tags[].createdDate     | String   | タグ 作成日時     |
| body.data.tags[].updatedDate     | String   | タグ 修正日時     |
| body.data.contacts[].contactType | String   | 連絡先タイプ      |
| body.data.contacts[].contact     | String   | 連絡先(携帯電話番号) |
| body.data.contacts[].createdDate | String   | 連絡先 作成日時    |

### UIDの登録

[URL]

```
POST /sms/v2.2/appKeys/{appKey}/uids
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Request body]

```json
{
  "uids": [
    {
      "uid": "UID",
      "tagIds": [
        "ABCD1234"
      ],
      "contacts": [
        {
          "contactType": "PHONE_NUMBER",
          "contact": "0100000000"
        }
      ]
    }
  ]
}
```

| 値                      | 	タイプ   | 最大長さ | 	必須 | 	説明                  |
|------------------------|--------|------|-----|----------------------|
| uid                    | String | -    | 必須  | UID                  |
| tagIds[]               | String | -    | 必須  | タグIDリスト              |
| contacts[].contactType | String | -    | 必須  | 連絡先タイプ(PHONE_NUMBER) |
| contacts[].contact     | String | -    | 必須  | 連絡先 (携帯電話番号)         |

[注意]

* tagIdsが与えられている場合、contactsは必須値ではない。
* contactsが与えられている場合、tagIdsは必須値ではない。
* 本サービスの場合、contactTypeは必ず"PHONE_NUMBER"値でリクエストする必要がある。

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": null
}
```

| 値                    | 	タイプ     | 	説明      |
|----------------------|----------|----------|
| header.isSuccessful  | 	Boolean | 	成否      |
| header.resultCode    | 	Integer | 	失敗コード   |
| header.resultMessage | 	String  | 	失敗メッセージ |

### UIDの削除

[URL]

```
DELETE /sms/v2.2/appKeys/{appKey}/uids/{uid}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |
| uid    | 	String | 	UID           |

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": null
}
```

| 値                    | 	タイプ     | 	説明      |
|----------------------|----------|----------|
| header.isSuccessful  | 	Boolean | 	成否      |
| header.resultCode    | 	Integer | 	失敗コード   |
| header.resultMessage | 	String  | 	失敗メッセージ |

### 携帯電話番号の登録

[URL]

```
POST /sms/v2.2/appKeys/{appKey}/uids/{uid}/phone-numbers
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |
| uid    | String  | UID            |

[Request body]

```json
{
  "phoneNumber": "0100000000"
}
```

| 値           | 	タイプ   | 最大長さ | 	必須 | 	説明    |
|-------------|--------|------|-----|--------|
| phoneNumber | String | -    | 必須  | 携帯電話番号 |

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": null
}
```

| 値                    | 	タイプ     | 	説明      |
|----------------------|----------|----------|
| header.isSuccessful  | 	Boolean | 	成否      |
| header.resultCode    | 	Integer | 	失敗コード   |
| header.resultMessage | 	String  | 	失敗メッセージ |

### 携帯電話番号の削除

[URL]

```
DELETE /sms/v2.2/appKeys/{appKey}/uids/{uid}/phone-numbers/{phoneNumber}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値           | 	タイプ    | 	説明            |
|-------------|---------|----------------|
| appKey      | 	String | 	固有のアプリケーションキー |
| uid         | String  | UID            |
| phoneNumber | String  | 携帯電話番号         |

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": null
}
```

| 値                    | 	タイプ     | 	説明      |
|----------------------|----------|----------|
| header.isSuccessful  | 	Boolean | 	成否      |
| header.resultCode    | 	Integer | 	失敗コード   |
| header.resultMessage | 	String  | 	失敗メッセージ |
