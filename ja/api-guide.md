## Notification > SMS > API v3.0 Guide

## v3.0 API紹介

### v2.4と異なる事項

1. シークレットキーを導入しました。

* v3.0 API呼び出し時にはヘッダにシークレットキーを追加すると成功します。

2. 大量送信リクエストを照会することができるAPIが追加されました。

* 大量送信リスト検索API、大量送信受信者リスト検索API、大量送信受信者リスト詳細検索APIが追加されました。

### [APIドメイン]

| 環境   | 	ドメイン                            |
|------|----------------------------------|
| Real | 	https://api-sms.cloud.toast.com |

<span id="precautions"></span>

### [注意事項]

* サポートする文字の長さは次のとおりです。
* 最大サポート文字数は保存基準であり、文字切れを防ぐために標準規格で作成してください。
* エンコードはEUC-KRで送信され、サポートしない絵文字は送信に失敗します。

| 分類      | 最大サポート  | 標準規格                            |
|---------|---------|---------------------------------|
| SMS本文   | 255文字   | 90バイト(ハングル45文字、英字90文字)          |
| MMSタイトル | 120文字   | 40バイト(ハングル20文字、英字40文字)          |
| MMS本文   | 4,000文字 | 2,000バイト(ハングル1,000文字、英字2,000文字) |

## SMS

### SMS送信

#### リクエスト

[URL]

```
POST  /sms/v3.0/appKeys/{appKey}/sender/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
  "X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

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
  "userId": "UserId",
  "statsId": "statsId",
  "originCode": "123456789"
}
```

| 値                                         | 	タイプ    | 最大長さ                                                           | 	必須 | 	説明                                                                  |
|-------------------------------------------|---------|----------------------------------------------------------------|-----|----------------------------------------------------------------------|
| templateId                                | 	String | 50                                                             | 	X  | 	送信テンプレートID                                                          |
| body                                      | 	String | 標準：90バイト、最大：255文字(EUC-KR基準) [[注意事項](./api-guide/#precautions)] | 	O  | 	本文内容                                                                |
| sendNo                                    | 	String | 13                                                             | 	O  | 	発信番号                                                                |
| requestDate                               | String  | -                                                              | X   | 予約日時(yyyy-MM-dd HH:mm)                                               |
| senderGroupingKey                         | String  | 100                                                            | X   | 発信者グループキー                                                            |
| recipientList[].recipientNo               | String  | 20                                                             | 	O  | 	受信番号<br/>countryCodeと組み合わせて使用可能<br/>最大1,000人                        |
| recipientList[].countryCode               | 	String | 8                                                              | 	X  | 	国番号[デフォルト値：82(韓国)]                                                  |
| recipientList[].internationalRecipientNo  | String  | 20                                                             | X   | 国番号が含まれた受信番号<br/>例)821012345678<br/>recipientNoがある場合、この値は無視される。<br/> |
| recipientList[].templateParameter         | 	Object | -                                                              | 	X  | 	テンプレートパラメータ(テンプレートID入力時)                                            |
| recipientList[].templateParameter.{key}   | String  | -                                                              | 	X  | 	置換キー(##key##)                                                       |
| recipientList[].templateParameter.{value} | 	Object | -                                                              | 	X  | 	置換キーにマッピングされるValue値                                                 |
| recipientList[].recipientGroupingKey      | String  | 100                                                            | X   | 受信者グループキー                                                            |
| userId                                    | 	String | 	100                                                           | X   | 送信セパレータex)admin,system                                               |
| statsId                                   | String  | 10                                                             | X   | 統計ID(発信検索条件には含まれません)                                                 |
| originCode                                | String  | 9                                                              | X   | 識別コード(特殊なタイプの付加通信事業者登録証に記載されている記号、文字、空白を除外した登録番号9桁の数字)               |

#### cURL

```
curl -X POST \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/sender/sms' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}'  \
-d '{
    "body"："本文",
    "sendNo": "15446859",
    "recipientList": [
        {
            "internationalRecipientNo": "821000000000"
        }
    ]
}'
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

| 値                                               | 	タイプ     | 	説明                                            |
|-------------------------------------------------|----------|------------------------------------------------|
| header.isSuccessful                             | 	Boolean | 	成否                                            |
| header.resultCode                               | 	Integer | 	失敗コード                                         |
| header.resultMessage                            | 	String  | 	失敗メッセージ                                       |
| body.data.requestId                             | 	String  | 	リクエストID                                       |
| body.data.statusCode                            | 	String  | 	リクエストステータスコード(1：リクエスト中、 2：リクエスト完了、 3：リクエスト失敗) |
| body.data.senderGroupingKey                     | 	String  | 	発信者グループキー                                     |
| body.data.sendResultList[].recipientNo          | String   | 受信番号                                           |
| body.data.sendResultList[].resultCode           | Integer  | 結果コード                                          |
| body.data.sendResultList[].resultMessage        | String   | 結果メッセージ                                        |
| body.data.sendResultList[].recipientSeq         | Integer  | 受信者シーケンス(mtPr)                                 |
| body.data.sendResultList[].recipientGroupingKey | String   | 受信者グループキー                                      |

#### SMS送信例(一般国内受信番号)

| Http method | URL                                                                  |
|-------------|----------------------------------------------------------------------|
| POST        | https://api-sms.cloud.toast.com/sms/v3.0/appKeys/{appKey}/sender/sms |

[Request body]

```json
{
  "body": "本文",
  "sendNo": "15446859",
  "senderGroupingKey": "SenderGroupingKey",
  "recipientList": [
    {
      "recipientNo": "01000000000",
      "recipientGroupingKey": "RecipientGroupingKey"
    },
    {
      "recipientNo": "01000000001",
      "recipientGroupingKey": "RecipientGroupingKey2"
    }
  ],
  "statsId": "statsId"
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

#### SMS送信例(国コードが含まれた受信番号)

| Http method | URL                                                                  |
|-------------|----------------------------------------------------------------------|
| POST        | https://api-sms.cloud.toast.com/sms/v3.0/appKeys/{appKey}/sender/sms |

[Request body]

```json
{
  "body": "本文",
  "sendNo": "15446859",
  "senderGroupingKey": "SenderGroupingKey",
  "recipientList": [
    {
      "internationalRecipientNo": "821000000000",
      "recipientGroupingKey": "RecipientGroupingKey"
    }
  ],
  "userId": "",
  "statsId": "statsId"
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

### SMS送信リスト検索

#### リクエスト

[URL]

```
GET  /sms/v3.0/appKeys/{appKey}/sender/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
  "X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Query parameter]

* requestIdまたはstartRequestDate + endRequestDateまたはstartCreateDate + endCreateDateは必須です。
* 登録日/送信日を同時に検索する場合、送信日は無視されます。

| 値                    | 	タイプ     | 	最大長さ | 必須     | 	説明                                                          |
|----------------------|----------|-------|--------|--------------------------------------------------------------|
| requestId            | 	String  | 25    | 	必須    | 	リクエストID                                                     |
| startRequestDate     | 	String  | -     | 	必須    | 	送信日開始値(yyyy-MM-dd HH:mm:ss)                                 |
| endRequestDate       | 	String  | -     | 	必須    | 	送信日終了値(yyyy-MM-dd HH:mm:ss)                                 |
| startCreateDate      | 	String  | -     | 	必須    | 	登録日開始値(yyyy-MM-dd HH:mm:ss)                                 |
| endCreateDate        | 	String  | -     | 	必須    | 	登録日終了値(yyyy-MM-dd HH:mm:ss)                                 |
| startResultDate      | 	String  | -     | 	オプション | 	受信日開始値(yyyy-MM-dd HH:mm:ss)                                 |
| endResultDate        | 	String  | -     | 	オプション | 	受信日終了値(yyyy-MM-dd HH:mm:ss)                                 |
| sendNo               | 	String  | 13    | 	オプション | 	発信番号                                                        |
| recipientNo          | 	String  | 20    | 	オプション | 	受信番号                                                        |
| templateId           | 	String  | 50    | 	オプション | 	テンプレート番号                                                    |
| msgStatus            | 	String  | 1     | 	オプション | メッセージステータスコード(0：失敗、 1：リクエスト、 2：処理中、 3：成功、 4：予約キャンセル、 5：重複失敗) |
| resultCode           | 	String  | 10    | 	オプション | 	受信結果コード[[検索コード表](./error-code/#_2)]                         |
| subResultCode        | 	String  | 10    | 	オプション | 	受信結果詳細コード[[検索コード表](./error-code/#_3)]                       |
| senderGroupingKey    | 	String  | 100   | 	オプション | 	送信者グループキー                                                   |
| recipientGroupingKey | 	String  | 100   | 	オプション | 	受信者グループキー                                                   |
| pageNum              | 	Integer | -     | 	オプション | 	ページ番号(デフォルト値：1)                                             |
| pageSize             | 	Integer | 1000  | 	オプション | 	検索数(デフォルト値：15)                                              |

#### cURL

```
curl -X GET \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/sender/sms?startRequestDate='"${START_DATE}"'&endRequestDate='"${END_DATE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

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
        "recipientSeq": 1,
        "sendType": "0",
        "messageType": "SMS",
        "messageCount": 1,
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
| body.pageSize                    | 	Integer | 	検索されたデータ数                                  |
| body.totalCount                  | 	Integer | 	データの総数                                     |
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
| body.data[].recipientSeq         | 	Integer | 	送信詳細ID(詳細検索時は必須)(旧mtPr)                    |
| body.data[].sendType             | 	String  | 	送信タイプ(0:Sms, 1:Lms/Mms, 2:Auth)            |
| body.data[].messageType          | 	String  | 	メッセージタイプ(SMS/LMS/MMS/AUTH)                 |
| body.data[].messageCount         | Integer  | 送信されたメッセージ件数                                |
| body.data[].userId               | 	String  | 	送信リクエストID                                  |
| body.data[].adYn                 | 	String  | 	広告かどうか                                     |
| body.data[].resultMessage        | 	String  | 	結果メッセージ                                    |
| body.data[].senderGroupingKey    | 	String  | 	発信者グループキー                                  |
| body.data[].recipientGroupingKey | 	String  | 	受信者グループキー                                  |

### SMS送信単一検索

#### リクエスト

[URL]

```
GET  /sms/v3.0/appKeys/{appKey}/sender/sms/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値         | 	タイプ    | 	説明            |
|-----------|---------|----------------|
| appKey    | 	String | 	固有のアプリケーションキー |
| requestId | 	String | 	リクエストID       |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Query parameter]

| 値            | 	タイプ     | 	必須 | 	説明     |
|--------------|----------|-----|---------|
| recipientSeq | 	Integer | 	必須 | 	送信詳細ID |

#### cURL

```
curl -X GET \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/sender/sms/'"${REQUEST_ID}"'?recipientSeq='"${RECIPIENT_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

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
      "recipientSeq": 1,
      "sendType": "0",
      "messageType": "SMS",
      "messageCount": 1,
      "userId": "tester",
      "adYn": "N",
      "originCode": "123456789",
      "resultMessage": "",
      "senderGroupingKey": "SenderGroupingKey",
      "recipientGroupingKey": "RecipientGroupingKey"
    }
  }
}
```

| 値                              | 	タイプ     | 	説明                                                    |
|--------------------------------|----------|--------------------------------------------------------|
| header.isSuccessful            | 	Boolean | 	成否                                                    |
| header.resultCode              | 	Integer | 	失敗コード                                                 |
| header.resultMessage           | 	String  | 	失敗メッセージ                                               |
| body.data.requestId            | 	String  | 	リクエストID                                               |
| body.data.requestDate          | 	String  | 	発信日時                                                  |
| body.data.resultDate           | 	String  | 	受信日時                                                  |
| body.data.templateId           | 	String  | 	テンプレートID                                              |
| body.data.templateName         | 	String  | 	テンプレート名                                               |
| body.data.categoryId           | 	Integer | 	カテゴリーID                                               |
| body.data.categoryName         | 	String  | 	カテゴリー名                                                |
| body.data.body                 | 	String  | 	本文内容                                                  |
| body.data.sendNo               | 	String  | 	発信番号                                                  |
| body.data.countryCode          | 	String  | 	国番号                                                   |
| body.data.recipientNo          | 	String  | 	受信番号                                                  |
| body.data.msgStatus            | 	String  | 	メッセージステータスコード                                         |
| body.data.msgStatusName        | 	String  | 	メッセージステータスコード名                                        |
| body.data.resultCode           | 	String  | 	受信結果コード[[受信結果コード表](./error-code/#emma-v3)]            |
| body.data.resultCodeName       | 	String  | 	受信結果コード名                                              |
| body.data.telecomCode          | 	Integer | 	サービスプロバイダーコード                                         |
| body.data.telecomCodeName      | 	String  | 	サービスプロバイダー名                                           |
| body.data.recipientSeq         | 	Integer | 	送信詳細ID(詳細検索時は必須)(旧mtPr)                               |
| body.data.sendType             | 	String  | 	送信タイプ(0:Sms, 1:Lms/Mms, 2:Auth)                       |
| body.data.messageType          | 	String  | 	メッセージタイプ(SMS/LMS/MMS/AUTH)                            |
| body.data.messageCount         | 	Integer | 	送信されたメッセージの件数                                         |
| body.data.userId               | 	String  | 	送信リクエストID                                             |
| body.data.adYn                 | 	String  | 	広告かどうか                                                |
| body.data.originCode           | String   | 識別コード(特殊なタイプの付加通信事業者登録証に記載されている記号、文字、空白を除外した登録番号9桁の数字) |
| body.data.resultMessage        | 	String  | 	結果メッセージ                                               |
| body.data.senderGroupingKey    | 	String  | 	発信者グループキー                                             |
| body.data.recipientGroupingKey | 	String  | 	受信者グループキー                                             |

## 長文MMS

### 長文MMS送信(添付ファイル含まず)

※ LMS/MMSは海外送信ができません。

#### リクエスト

[URL]

```
POST  /sms/v3.0/appKeys/{appKey}/sender/mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

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
  "userId": "UserId",
  "statsId": "statsId",
  "originCode": "123456789"
}
```

| 値                                         | 	タイプ    | 最大長さ | 	必須 | 	説明                                                                                                                     |
|-------------------------------------------|---------|------|-----|-------------------------------------------------------------------------------------------------------------------------|
| templateId                                | 	String | 50   | 	X  | 	送信テンプレートID                                                                                                             |
| title                                     | 	String | 120  | 	O  | 	タイトル                                                                                                                   |
| body                                      | 	String | 4000 | 	O  | 	本文                                                                                                                     |
| sendNo                                    | 	String | 13   | 	O  | 	発信番号                                                                                                                   |
| requestDate                               | String  | -    | X   | 予約日時(yyyy-MM-dd HH:mm)                                                                                                  |
| senderGroupingKey                         | String  | 100  | X   | 発信者グループキー                                                                                                               |
| recipientList[].recipientNo               | String  | 20   | 	O  | 	受信番号<br/>countryCodeと組み合わせて使用可能                                                                                        |
| recipientList[].countryCode               | String  | 8    | 	X  | 	国番号[デフォルト値：82(韓国)]<br/>MMSは海外送信不可                                                                                      |
| recipientList[].internationalRecipientNo  | String  | 20   | X   | 国番号が含まれる受信番号<br/>例)821012345678<br/>recipientNoがある場合、この値は無視される。<br/>                                                    |
| recipientList[].templateParameter         | 	Object | -    | 	X  | 	テンプレートパラメータ(テンプレートID入力時)                                                                                               |
| recipientList[].templateParameter.{key}   | 	String | -    | 	X  | 	置換キー(##key##)                                                                                                          |
| recipientList[].templateParameter.{value} | 	Object | -    | 	X  | 	置換キーにマッピングされるValue値                                                                                                    |
| recipientList[].recipientGroupingKey      | String  | 1000 | X   | 受信者グループキー                                                                                                               |
| userId                                    | 	String | 100  | 	X  | 送信セパレータex)admin,system                                                                                                  |
| statsId                                   | String  | 10   | X   | 統計ID(発信検索条件には含まれません)                                                                                                    |
| originCode                                | String  | 9    | X   | 識別コード(特殊なタイプの付加通信事業者登録証に記載されている記号、文字、空白を除外した登録番号9桁の数字)<br/>特殊なタイプの付加通信事業者ではない場合は使用しません。基本的にNHN Cloudの識別コードが挿入されます。<br/> |

#### cURL

```
curl -X POST \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/sender/mms' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}'  \
-d '{
    "title"："{タイトル}",
    "body"："{本文内容}",
    "sendNo": "15446859",
    "attachFileIdList": [0],
    "recipientList": [{
            "recipientNo": "01000000000",
            "templateParameter": {}
        }
    ],
    "userId": ""
}'
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

| 値                                               | 	タイプ     | 	説明                                            |
|-------------------------------------------------|----------|------------------------------------------------|
| header.isSuccessful                             | 	Boolean | 	成否                                            |
| header.resultCode                               | 	Integer | 	失敗コード                                         |
| header.resultMessage                            | 	String  | 	失敗メッセージ                                       |
| body.data.requestId                             | 	String  | 	リクエストID                                       |
| body.data.statusCode                            | 	String  | 	リクエストステータスコード(1：リクエスト中、 2：リクエスト完了、 3：リクエスト失敗) |
| body.data.senderGroupingKey                     | 	String  | 	発信者グループキー                                     |
| body.data.sendResultList[].recipientNo          | String   | 受信番号                                           |
| body.data.sendResultList[].resultCode           | Integer  | 結果コード                                          |
| body.data.sendResultList[].resultMessage        | String   | 結果メッセージ                                        |
| body.data.sendResultList[].recipientSeq         | Integer  | 受信者シーケンス(mtPr)                                 |
| body.data.sendResultList[].recipientGroupingKey | String   | 受信者グループキー                                      |

#### 長文MMS送信例

| Http method | URL                                                                  |
|-------------|----------------------------------------------------------------------|
| POST        | https://api-sms.cloud.toast.com/sms/v3.0/appKeys/{appKey}/sender/mms |

[Request body]

```json
{
  "title":  "title",
  "body": "本文",
  "sendNo": "15446859",
  "senderGroupingKey": "SenderGroupingKey",
  "recipientList": [
    {
      "recipientNo": "01000000000",
      "recipientGroupingKey": "RecipientGroupingKey"
    },
    {
      "recipientNo": "01000000001",
      "recipientGroupingKey": "RecipientGroupingKey2"
    }
  ],
  "statsId": "statsId"
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

### 長文MMS送信(添付ファイルを含む)

#### 添付ファイル送信例

| Http method | URL                                                                  |
|-------------|----------------------------------------------------------------------|
| POST        | https://api-sms.cloud.toast.com/sms/v3.0/appKeys/{appKey}/sender/mms |

[Request body]

```json
{
  "title": "title",
  "body": "body",
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
  ],
  "statsId": "statsId"
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

##### Description

- 添付ファイル(フィールド名：attachFileIdList)を含む長文MMSを送信するには、事前に添付ファイルのアップロードを行う必要があります。<br>
- [[添付ファイルアップロード](./api-guide/#binaryUpload)]</a> ガイドを参照してください。
- 添付画像制限事項
    - サポートコーデック：.jpg、.jpeg
    - 添付イメージ数：3個以下
    - 添付イメージサイズ： 1個当り300KB以下。ただし、添付したイメージの数が3個の場合は合計800KB以下。
    - 添付イメージの解像度： 1000 x 1000以下

### 長文MMS送信リスト検索

#### リクエスト

[URL]

```
GET  /sms/v3.0/appKeys/{appKey}/sender/mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Query parameter]

* requestIdまたはstartRequestDate + endRequestDateまたはstartCreateDate + endCreateDateは必須です。
* 登録日/送信日を同時に検索する場合、送信日は無視されます。

| 値                    | 	タイプ     | 最大長さ | 	必須    | 	説明                                                          |
|----------------------|----------|------|--------|--------------------------------------------------------------|
| requestId            | 	String  | 25   | 	必須    | 	リクエストID                                                     |
| startRequestDate     | 	String  | -    | 	必須    | 	送信日開始値(yyyy-MM-dd HH:mm:ss)                                 |
| endRequestDate       | 	String  | -    | 	必須    | 	送信日終了値(yyyy-MM-dd HH:mm:ss)                                 |
| startCreateDate      | 	String  | -    | 	必須    | 	登録日開始値(yyyy-MM-dd HH:mm:ss)                                 |
| endCreateDate        | 	String  | -    | 	必須    | 	登録日終了値(yyyy-MM-dd HH:mm:ss)                                 |
| startResultDate      | 	String  | -    | 	オプション | 	受信日開始値(yyyy-MM-dd HH:mm:ss)                                 |
| endResultDate        | 	String  | -    | 	オプション | 	受信日終了値(yyyy-MM-dd HH:mm:ss)                                 |
| sendNo               | 	String  | 13   | 	オプション | 	発信番号                                                        |
| recipientNo          | 	String  | 20   | 	オプション | 	受信番号                                                        |
| templateId           | 	String  | 50   | 	オプション | 	テンプレート番号                                                    |
| msgStatus            | 	String  | 1    | 	オプション | メッセージステータスコード(0：失敗、 1：リクエスト、 2：処理中、 3：成功、 4：予約キャンセル、 5：重複失敗) |
| resultCode           | 	String  | 10   | 	オプション | 	受信結果コード[[検索コード表](./error-code/#_2)]                         |
| subResultCode        | 	String  | 10   | 	オプション | 	受信結果詳細コード[[検索コード表](./error-code/#_3)]                       |
| senderGroupingKey    | 	String  | 100  | 	オプション | 	送信者グループキー                                                   |
| recipientGroupingKey | 	String  | 100  | 	オプション | 	受信者グループキー                                                   |
| pageNum              | 	Integer | -    | 	オプション | 	ページ番号(デフォルト値：1)                                             |
| pageSize             | 	Integer | 1000 | 	オプション | 	検索数(デフォルト値：15)                                              |

#### cURL

```
curl -X GET \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/sender/mms?startRequestDate='"${START_DATE}"'&endRequestDate='"${END_DATE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

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
        "resultDate": "2023-09-18 16:07:36.0",
        "templateId": null,
        "templateName": null,
        "categoryId": null,
        "categoryName": null,
        "title": "Title",
        "body": "본문",
        "sendNo": "15771234",
        "countryCode": "82",
        "recipientNo": "01012341234",
        "msgStatus": "3",
        "msgStatusName": "성공",
        "resultCode": "1000",
        "resultCodeName": "성공",
        "telecomCode": 10003,
        "telecomCodeName": "LGU",
        "mtPr": "1",
        "sendType": "1",
        "messageType": "LMS",
        "messageCount": 1,
        "userId": null,
        "adYn": "N",
        "attachFileList": [
          {
            "fileId": 535191,
            "filePath": "/temporary/71191/toast-mt-2023-09-18/1607/535191/",
            "fileName": "attachment.jpg",
            "saveFileName": "20230918KKkqQC0.jpg",
            "uploadType": "TEMPORARY"
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

| 値                                         | 	タイプ     | 	説明                                         |
|-------------------------------------------|----------|---------------------------------------------|
| header.isSuccessful                       | 	Boolean | 	成否                                         |
| header.resultCode                         | 	Integer | 	失敗コード                                      |
| header.resultMessage                      | 	String  | 	失敗メッセージ                                    |
| body                                      | 	Object  | 	本文領域                                       |
| body.pageNum                              | 	Integer | 	現在のページ番号                                   |
| body.pageSize                             | 	Integer | 	検索されたデータ数                                  |
| body.totalCount                           | 	Integer | 	データの総数                                     |
| body.data[].requestId                     | 	String  | 	リクエストID                                    |
| body.data[].requestDate                   | 	String  | 	発信日時                                       |
| body.data[].resultDate                    | 	String  | 	受信日時                                       |
| body.data[].templateId                    | 	String  | 	テンプレートID                                   |
| body.data[].templateName                  | 	String  | 	テンプレート名                                    |
| body.data[].categoryId                    | 	Integer | 	カテゴリーID                                    |
| body.data[].categoryName                  | 	String  | 	カテゴリー名                                     |
| body.data[].body                          | 	String  | 	本文内容                                       |
| body.data[].sendNo                        | 	String  | 	発信番号                                       |
| body.data[].countryCode                   | 	String  | 	国番号                                        |
| body.data[].recipientNo                   | 	String  | 	受信番号                                       |
| body.data[].msgStatus                     | 	String  | 	メッセージステータスコード                              |
| body.data[].msgStatusName                 | 	String  | 	メッセージステータスコード名                             |
| body.data[].resultCode                    | 	String  | 	受信結果コード[[受信結果コード表](./error-code/#emma-v3)] |
| body.data[].resultCodeName                | 	String  | 	受信結果コード名                                   |
| body.data[].telecomCode                   | 	Integer | 	サービスプロバイダーコード                              |
| body.data[].telecomCodeName               | 	String  | 	サービスプロバイダー名                                |
| body.data[].recipientSeq                  | 	Integer | 	送信詳細ID(詳細検索時は必須)(旧mtPr)                    |
| body.data[].sendType                      | 	String  | 	送信タイプ(0:Sms, 1:Lms/Mms, 2:Auth)            |
| body.data[].messageType                   | 	String  | 	メッセージタイプ(SMS/LMS/MMS/AUTH)                 |
| body.data[].messageCount                  | 	Integer | 	送信されたメッセージ件数                               |
| body.data[].userId                        | 	String  | 	送信リクエストID                                  |
| body.data[].adYn                          | 	String  | 	広告かどうか                                     |
| body.data[].attachFileList[].fileId       | 	Integer | 	ファイルID                                     |
| body.data[].attachFileList[].filePath     | 	String  | 	ファイル保存パス(内部用)                              |
| body.data[].attachFileList[].filename     | 	String  | 	ファイル名                                      |
| body.data[].attachFileList[].saveFileName | 	String  | 	保存された添付ファイル名                               |
| body.data[].attachFileList[].uploadType   | 	String  | 	アップロードタイプ                                  |
| body.data[].senderGroupingKey             | 	String  | 	発信者グループキー                                  |
| body.data[].recipientGroupingKey          | 	String  | 	受信者グループキー                                  |

### 長文MMS送信単一検索

#### リクエスト

[URL]

```
GET  /sms/v3.0/appKeys/{appKey}/sender/mms/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値         | 	タイプ    | 	説明            |
|-----------|---------|----------------|
| appKey    | 	String | 	固有のアプリケーションキー |
| requestId | 	String | 	リクエストID       |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Query parameter]

| 値            | 	タイプ     | 	必須 | 	説明     |
|--------------|----------|-----|---------|
| recipientSeq | 	Integer | 	必須 | 	送信詳細ID |

#### cURL

```
curl -X GET \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/sender/mms/'"${REQUEST_ID}"'?recipientSeq='"${RECIPIENT_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

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
      "body": "본문",
      "sendNo": "15771234",
      "countryCode": "82",
      "recipientNo": "01012341234",
      "msgStatus": "3",
      "msgStatusName": "성공",
      "resultCode": "1000",
      "resultCodeName": "성공",
      "telecomCode": 10003,
      "telecomCodeName": "LGU",
      "mtPr": "1",
      "sendType": "1",
      "userId": null,
      "adYn": "N",
      "originCode": "123456789",
      "attachFileList": [
        {
          "fileId": 535191,
          "filePath": "/temporary/71191/toast-mt-2023-09-18/1607/535191/",
          "fileName": "attachment.jpg",
          "saveFileName": "20230918KKkqQC0.jpg",
          "uploadType": "TEMPORARY"
        }
      ],
      "resultMessage": "SUCCESS",
      "senderGroupingKey": "SenderGroupingKey",
      "recipientGroupingKey": "recipientGroupingKey"
    }
  }
}
```

| 値                                       | 	タイプ     | 	説明                                                    |
|-----------------------------------------|----------|--------------------------------------------------------|
| header.isSuccessful                     | 	Boolean | 	成否                                                    |
| header.resultCode                       | 	Integer | 	失敗コード                                                 |
| header.resultMessage                    | 	String  | 	失敗メッセージ                                               |
| body                                    | 	Object  | 	本文領域                                                  |
| body.pageNum                            | 	Integer | 	現在のページ番号                                              |
| body.pageSize                           | 	Integer | 	検索されたデータ数                                             |
| body.totalCount                         | 	Integer | 	データの総数                                                |
| body.data.requestId                     | 	String  | 	リクエストID                                               |
| body.data.requestDate                   | 	String  | 	発信日時                                                  |
| body.data.resultDate                    | 	String  | 	受信日時                                                  |
| body.data.templateId                    | 	String  | 	テンプレートID                                              |
| body.data.templateName                  | 	String  | 	テンプレート名                                               |
| body.data.categoryId                    | 	Integer | 	カテゴリーID                                               |
| body.data.categoryName                  | 	String  | 	カテゴリー名                                                |
| body.data.body                          | 	String  | 	本文内容                                                  |
| body.data.sendNo                        | 	String  | 	発信番号                                                  |
| body.data.countryCode                   | 	String  | 	国家番号                                                  |
| body.data.recipientNo                   | 	String  | 	受信番号                                                  |
| body.data.msgStatus                     | 	String  | 	メッセージステータスコード                                         |
| body.data.msgStatusName                 | 	String  | 	メッセージステータスコード名                                        |
| body.data.resultCode                    | 	String  | 	受信結果コード[[受信結果コード表](./error-code/#emma-v3)]            |
| body.data.resultCodeName                | 	String  | 	受信結果コード名                                              |
| body.data.telecomCode                   | 	Integer | 	サービスプロバイダーコード                                         |
| body.data.telecomCodeName               | 	String  | 	サービスプロバイダー名                                           |
| body.data.recipientSeq                  | 	Integer | 	送信詳細ID(詳細検索時に必須)                                      |
| body.data.sendType                      | 	String  | 	送信タイプ(0:Sms, 1:Lms/Mms, 2:Auth)                       |
| body.data.messageType                   | 	String  | 	メッセージタイプ(SMS/LMS/MMS/AUTH)                            |
| body.data.userId                        | 	String  | 	送信リクエストID                                             |
| body.data.adYn                          | 	String  | 	広告かどうか                                                |
| body.data.originCode                    | String   | 識別コード(特殊なタイプの付加通信事業者登録証に記載されている記号、文字、空白を除外した登録番号9桁の数字) |
| body.data.attachFileList[].fileId       | 	Integer | 	ファイルID                                                |
| body.data.attachFileList[].filePath     | 	String  | 	ファイル保存パス(内部用)                                         |
| body.data.attachFileList[].filename     | 	String  | 	ファイル名                                                 |
| body.data.attachFileList[].saveFileName | 	String  | 	保存された添付ファイル名                                          |
| body.data.attachFileList[].uploadType   | 	String  | 	アップロードタイプ                                             |
| body.data.resultMessage                 | 	String  | 結果メッセージ                                                |
| body.data.senderGroupingKey             | 	String  | 	発信者グループキー                                             |
| body.data.recipientGroupingKey          | 	String  | 	受信者グループキー                                             |

## 認証用SMS(緊急)

### 認証用SMS送信

<span id="precautions-authword"></span>

1. 認証用SMS送信時に本文内に含まれている必要がある認証文言案内

| 区分         | 認証文言                                       |
|------------|--------------------------------------------| 
| 認証用SMS(緊急) | auth、 password、 verif、 にんしょう、 認証、 비밀번호、 인증 |

- 例1-1)認証用SMS(緊急) API送信リクエスト時に全文(テンプレート日本語識別子含む)に認証文言が含まれていない場合は送信に失敗します。
- 例1-2)認証文言が英字の場合、大文字/小文字を区別せずに有効性チェックが行われます。

#### リクエスト

[URL]

```
POST  /sms/v3.0/appKeys/{appKey}/sender/auth/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

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
  "userId": "UserId",
  "statsId": "statsId",
  "originCode": "123456789"
}
```

| 値                                         | 	タイプ    | 最大長さ                                                           | 	必須 | 	説明                                                                                                                     |
|-------------------------------------------|---------|----------------------------------------------------------------|-----|-------------------------------------------------------------------------------------------------------------------------|
| templateId                                | 	String | 50                                                             | 	X  | 	送信テンプレートID                                                                                                             |
| body                                      | 	String | 標準：90バイト、最大：255文字(EUC-KR基準) [[注意事項](./api-guide/#precautions)] | 	O  | 	本文内容[[注意事項](./api-guide/#precautions-authword)]                                                                        |
| sendNo                                    | 	String | 13                                                             | 	O  | 	発信番号                                                                                                                   |
| requestDate                               | String  | -                                                              | X   | 予約日時(yyyy-MM-dd HH:mm)                                                                                                  |
| senderGroupingKey                         | String  | 100                                                            | X   | 発信者グループキー                                                                                                               |
| recipientList[].recipientNo               | 	String | 20                                                             | 	O  | 	受信番号<br/>countryCodeと組み合わせて使用可能                                                                                        |
| recipientList[].countryCode               | 	String | 8                                                              | 	X  | 	国番号[デフォルト値：82(韓国)]                                                                                                     |
| recipientList[].internationalRecipientNo  | String  | 20                                                             | X   | 国番号が含まれる受信番号<br/>例)821012345678<br/>recipientNoがある場合、この値は無視<br/>                                                        |
| recipientList[].templateParameter         | 	Object | -                                                              | 	X  | 	テンプレートパラメータ(テンプレートID入力時)                                                                                               |
| recipientList[].templateParameter.{key}   | String  | -                                                              | 	X  | 	置換キー(##key##)                                                                                                          |
| recipientList[].templateParameter.{value} | Object  | -                                                              | 	X  | 	置換キーにマッピングされるValue値                                                                                                    |
| recipientList[].recipientGroupingKey      | String  | 100                                                            | X   | 受信者グループキー                                                                                                               |
| userId                                    | 	String | 100                                                            | 	X  | 送信セパレータex)admin,system                                                                                                  |
| statsId                                   | String  | 10                                                             | X   | 統計ID(発信検索条件には含まれません)                                                                                                    |
| originCode                                | String  | 9                                                              | X   | 識別コード(特殊なタイプの付加通信事業者登録証に記載されている記号、文字、空白を除外した登録番号9桁の数字)<br/>特殊なタイプの付加通信事業者ではない場合は使用しません。基本的にNHN Cloudの識別コードが挿入されます。<br/> |

#### cURL

```
curl -X POST \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/sender/auth/sms' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}'  \
-d '{
    "body"："認証テスト",
    "sendNo": "15446859",
    "recipientList": [
        {
            "recipientNo": "01000000000",
            "templateParameter": {}
        }
    ],
    "userId": ""
}'
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

| 値                                               | 	タイプ     | 	説明                                            |
|-------------------------------------------------|----------|------------------------------------------------|
| header.isSuccessful                             | 	Boolean | 	成否                                            |
| header.resultCode                               | 	Integer | 	失敗コード                                         |
| header.resultMessage                            | 	String  | 	失敗メッセージ                                       |
| body.data.requestId                             | 	String  | 	リクエストID                                       |
| body.data.statusCode                            | 	String  | 	リクエストステータスコード(1：リクエスト中、 2：リクエスト完了、 3：リクエスト失敗) |
| body.data.senderGroupingKey                     | 	String  | 	発信者グループキー                                     |
| body.data.sendResultList[].recipientNo          | String   | 受信番号                                           |
| body.data.sendResultList[].resultCode           | Integer  | 結果コード                                          |
| body.data.sendResultList[].resultMessage        | String   | 結果メッセージ                                        |
| body.data.sendResultList[].recipientSeq         | Integer  | 受信者シーケンス(mtPr)                                 |
| body.data.sendResultList[].recipientGroupingKey | String   | 受信者グループキー                                      |

#### 例

| Http method | URL                                                                       |
|-------------|---------------------------------------------------------------------------|
| POST        | https://api-sms.cloud.toast.com/sms/v3.0/appKeys/{appKey}/sender/auth/sms |

[Request body]

```json
{
  "body": "本文",
  "sendNo": "15446859",
  "senderGroupingKey": "SenderGroupingKey",
  "recipientList": [
    {
      "recipientNo": "01000000000",
      "recipientGroupingKey": "RecipientGroupingKey"
    },
    {
      "recipientNo": "01000000001",
      "recipientGroupingKey": "RecipientGroupingKey2"
    }
  ],
  "statsId": "statsId"
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

### 認証用SMS送信リスト検索

#### リクエスト

[URL]

```
GET  /sms/v3.0/appKeys/{appKey}/sender/auth/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Query parameter]

* requestIdまたはstartRequestDate + endRequestDateまたはstartCreateDate + endCreateDateは必須です。
* 登録日/送信日を同時に検索する場合、送信日は無視されます。

| 値                    | 	タイプ     | 	最大長さ | 必須     | 	説明                                                          |
|----------------------|----------|-------|--------|--------------------------------------------------------------|
| requestId            | 	String  | 25    | 	必須    | 	リクエストID                                                     |
| startRequestDate     | 	String  | -     | 	必須    | 	送信日開始値(yyyy-MM-dd HH:mm:ss)                                 |
| endRequestDate       | 	String  | -     | 	必須    | 	送信日終了値(yyyy-MM-dd HH:mm:ss)                                 |
| startCreateDate      | 	String  | -     | 	必須    | 	登録日開始値(yyyy-MM-dd HH:mm:ss)                                 |
| endCreateDate        | 	String  | -     | 	必須    | 	登録日終了値(yyyy-MM-dd HH:mm:ss)                                 |
| startResultDate      | 	String  | -     | 	オプション | 	受信日開始値(yyyy-MM-dd HH:mm:ss)                                 |
| endResultDate        | 	String  | -     | 	オプション | 	受信日終了値(yyyy-MM-dd HH:mm:ss)                                 |
| sendNo               | 	String  | 13    | 	オプション | 	発信番号                                                        |
| recipientNo          | 	String  | 20    | 	オプション | 	受信番号                                                        |
| templateId           | 	String  | 50    | 	オプション | 	テンプレート番号                                                    |
| msgStatus            | 	String  | 1     | 	オプション | メッセージステータスコード(0：失敗、 1：リクエスト、 2：処理中、 3：成功、 4：予約キャンセル、 5：重複失敗) |
| resultCode           | 	String  | 10    | 	オプション | 	受信結果コード[[検索コード表](./error-code/#_2)]                         |
| subResultCode        | 	String  | 10    | 	オプション | 	受信結果詳細コード[[検索コード表](./error-code/#_3)]                       |
| senderGroupingKey    | 	String  | 100   | 	オプション | 	送信者グループキー                                                   |
| recipientGroupingKey | 	String  | 100   | 	オプション | 	受信者グループキー                                                   |
| pageNum              | 	Integer | -     | 	オプション | 	ページ番号(デフォルト値：1)                                             |
| pageSize             | 	Integer | 1000  | 	オプション | 	検索数(デフォルト値：15)                                              |

#### cURL

```
curl -X GET \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/sender/auth/sms?startRequestDate='"${START_DATE}"'&endRequestDate='"${END_DATE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

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
        "recipientSeq": 1,
        "sendType": "0",
        "messageType": "AUTH",
        "messageCount": 1,
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
| body.pageSize                    | 	Integer | 	検索されたデータ数                                  |
| body.totalCount                  | 	Integer | 	データの総数                                     |
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
| body.data[].recipientSeq         | 	Integer | 	送信詳細ID(詳細検索時は必須)(旧mtPr)                    |
| body.data[].sendType             | 	String  | 	送信タイプ(0:Sms, 1:Lms/Mms, 2:Auth)            |
| body.data[].messageType          | 	String  | 	メッセージタイプ(SMS/LMS/MMS/AUTH)                 |
| body.data[].messageCount         | 	Integer | 	送信されたメッセージ件数                               |
| body.data[].userId               | 	String  | 	送信リクエストID                                  |
| body.data[].adYn                 | 	String  | 	広告かどうか                                     |
| body.data[].senderGroupingKey    | 	String  | 	発信者グループキー                                  |
| body.data[].recipientGroupingKey | 	String  | 	受信者グループキー                                  |

### 認証用SMS送信単一検索

#### リクエスト

[URL]

```
GET  /sms/v3.0/appKeys/{appKey}/sender/auth/sms/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値         | 	タイプ    | 	説明            |
|-----------|---------|----------------|
| appKey    | 	String | 	固有のアプリケーションキー |
| requestId | 	String | 	リクエストID       |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Query parameter]

| 値            | 	タイプ     | 	必須 | 	説明     |
|--------------|----------|-----|---------|
| recipientSeq | 	Integer | 	必須 | 	送信詳細ID |

#### cURL

```
curl -X GET \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/sender/auth/sms/'"${REQUEST_ID}"'?recipientSeq='"${RECIPIENT_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

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
      "recipientSeq": 1,
      "sendType": "0",
      "messageType": "AUTH",
      "messageCount": 1,
      "userId": "tester",
      "adYn": "N",
      "originCode": "123456789",
      "resultMessage": "",
      "senderGroupingKey": "SenderGroupingKey",
      "recipientGroupingKey": "RecipientGroupingKey"
    }
  }
}
```

| 値                              | 	タイプ     | 	説明                                                    |
|--------------------------------|----------|--------------------------------------------------------|
| header.isSuccessful            | 	Boolean | 	成否                                                    |
| header.resultCode              | 	Integer | 	失敗コード                                                 |
| header.resultMessage           | 	String  | 	失敗メッセージ                                               |
| body.data.requestId            | 	String  | 	リクエストID                                               |
| body.data.requestDate          | 	String  | 	発信日時                                                  |
| body.data.resultDate           | 	String  | 	受信日時                                                  |
| body.data.templateId           | 	String  | 	テンプレートID                                              |
| body.data.templateName         | 	String  | 	テンプレート名                                               |
| body.data.categoryId           | 	Integer | 	カテゴリーID                                               |
| body.data.categoryName         | 	String  | 	カテゴリー名                                                |
| body.data.body                 | 	String  | 	本文内容                                                  |
| body.data.sendNo               | 	String  | 	発信番号                                                  |
| body.data.countryCode          | 	String  | 	国番号                                                   |
| body.data.recipientNo          | 	String  | 	受信番号                                                  |
| body.data.msgStatus            | 	String  | 	メッセージステータスコード                                         |
| body.data.msgStatusName        | 	String  | 	メッセージステータスコード名                                        |
| body.data.resultCode           | 	String  | 	受信結果コード[[受信結果コード表](./error-code/#emma-v3)]            |
| body.data.resultCodeName       | 	String  | 	受信結果コード名                                              |
| body.data.telecomCode          | 	Integer | 	サービスプロバイダーコード                                         |
| body.data.telecomCodeName      | 	String  | 	サービスプロバイダー名                                           |
| body.data.recipientSeq         | 	Integer | 	送信詳細ID(詳細検索時は必須)(旧mtPr)                               |
| body.data.sendType             | 	String  | 	送信タイプ(0:Sms, 1:Lms/Mms, 2:Auth)                       |
| body.data.messageType          | 	String  | 	メッセージタイプ(SMS/LMS/MMS/AUTH)                            |
| body.data.messageCount         | Integer  | 送信されたメッセージの件数                                          |
| body.data.userId               | 	String  | 	送信リクエストID                                             |
| body.data.adYn                 | 	String  | 	広告かどうか                                                |
| body.data.originCode           | String   | 識別コード(特殊なタイプの付加通信事業者登録証に記載されている記号、文字、空白を除外した登録番号9桁の数字) |
| body.data.senderGroupingKey    | 	String  | 	発信者グループキー                                             |
| body.data.recipientGroupingKey | 	String  | 	受信者グループキー                                             |

## 広告文字

### 広告性SMS送信

#### リクエスト

[URL]

```
POST  /sms/v3.0/appKeys/{appKey}/sender/ad-sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Request Body]
上のSMS送信と同じ
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
無料拒否080XXXXXXX
```

#### cURL

```
curl -X POST \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/sender/ad-sms' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}'  \
-d '{
    "body"："(広告)テスト\n [無料受信拒否]0808880327",
    "sendNo": "15446859",
    "recipientList": [{
            "recipientNo": "01000000000",
            "templateParameter": {}
        }
    ],
    "userId": ""
}'
```

### 広告性MMS送信

※ LMS/MMSは海外送信ができません。

#### リクエスト

[URL]

```
POST  /sms/v3.0/appKeys/{appKey}/sender/ad-mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Request Body]
上のMMS送信と同じ
[[Request Body参考](./api-guide/#mms_1)]

<span style="color:red">ただし、本文に以下の文言が必ず入る必要があります。</span>

080番号はコンソールの**080受信拒否設定**タブで確認できます。

```
(広告)

[無料受信拒否]080XXXXXXX
```

#### cURL

```
curl -X POST \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/sender/ad-mms' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
-d '{
    "title"："{タイトル}",
    "body"："(広告)テスト\n [無料受信拒否]0808880327",
    "sendNo": "15446859",
    "recipientList": [{
            "recipientNo": "01000000000",
            "templateParameter": {}
        }
    ],
    "userId": ""
}'
```

## 結果アップデート基準メッセージ検索

* このAPIはメッセージ送信結果アップデート時間を基準に検索されます。
* 端末送信結果をサービス外で使用する場合はこのAPIを使用してください。

### メッセージ検索

#### リクエスト

[URL]

```
GET /sms/v3.0/appKeys/{appKey}/message-results?startUpdateDate={startUpdateDate}&endUpdateDate={endUpdateDate}&messageType={messageType}&pageNum={pageNum}&pageSize={pageSize}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

* 検索開始時間と検索終了時間の範囲は1日に制限されます。

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Query parameter]

| 値               | 	タイプ    | 	必須    | 	説明                                      |
|-----------------|---------|--------|------------------------------------------|
| startUpdateDate | 	String | 	必須    | 	結果アップデート検索開始時間 <br/>yyyy-MM-dd HH:mm:ss |
| endUpdateDate   | 	String | 	必須    | 	結果アップデート検索終了時間 <br/>yyyy-MM-dd HH:mm:ss |
| messageType     | 	String | 	オプション | 	メッセージタイプ(SMS/LMS/MMS/AUTH)              |
| pageNum         | Integer | オプション  | ページ番号(デフォルト値：1)                          |
| pageSize        | Integer | オプション  | 検索数(デフォルト値：15)                           |

#### cURL

```
curl -X GET \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/message-results?startRequestDate='"${START_DATE}"'&endRequestDate='"${END_DATE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

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
        "resultCodeName": "성공",
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

## 大量送信

### 大量送信リスト検索

#### リクエスト

[URL]

```
GET /sms/v3.0/appKeys/{appKey}/mass-sender
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Query parameter]

* requestIdまたはstartRequestDate + endRequestDateまたはstartCreateDate + endCreateDateは必須です。

| 値                | 	タイプ              | 最大長さ | 	必須 | 	説明                                                                                                                                                                   |
|------------------|-------------------|------|-----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| sendType         | required, String  | 1    | O   | 送信タイプ<br>SMS ："0",<br>MMS ："1"                                                                                                                                        |
| requestId        | String            | -    | O   | リクエストID                                                                                                                                                               |
| startRequestDate | String            | -    | O   | 送信日開始                                                                                                                                                                 |
| endRequestDate   | String            | -    | O   | 送信日終了                                                                                                                                                                 |
| startCreateDate  | 	String           | -    | 	O  | 	登録日開始                                                                                                                                                                |
| endCreateDate    | 	String           | -    | 	O  | 	登録日終了                                                                                                                                                                |
| statusCode       | String            | 10   | X   | 送信ステータスコード<br>WAIT ："MAS00"<br>READY ："MAS01"<br>SENDREADY ："MAS09"<br>SENDWAIT ："MAS10"<br>SENDING ："MAS11"<br>COMPLETE ："MAS19"<br>CANCEL ："MAS91"<br>FAIL ："MAS99" |
| pageNum          | optional, Integer | -    | X   | ページ番号                                                                                                                                                                 |
| pageSize         | optional, Integer | 1000 | X   | 検索数                                                                                                                                                                   |

#### cURL

```
curl -X GET \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/mass-sender?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
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
    "pageNum": 1,
    "pageSize": 15,
    "totalCount": 1,
    "data": [
      {
        "requestId": "20230918100859e1SoH6zC4o0",
        "requestDate": "2023-09-18 10:08:59",
        "masterStatusCode": "MAS19",
        "masterStatus": "COMPLETE",
        "templateId": "",
        "sendNo": "12341234",
        "title": null,
        "body": "body",
        "adYn": "N",
        "autoSendYn": "Y",
        "sendErrorCount": 0,
        "createUser": "25d09a62-0bf7-4d6f-b823-80e49536cc08",
        "createDate": "2023-09-18 10:08:59.0"
      }
    ]
  }
}
```

| 値                            | 	タイプ     | 	説明          |
|------------------------------|----------|--------------|
| header.isSuccessful          | 	Boolean | 	成否          |
| header.resultCode            | 	Integer | 	失敗コード       |
| header.resultMessage         | 	String  | 	失敗メッセージ     |
| body.data[].requestId        | String   | リクエストID      |
| body.data[].requestDate      | String   | リクエスト時間      |
| body.data[].masterStatusCode | String   | 大量送信ステータスコード |
| body.data[].masterStatus     | String   | 大量送信ステータス    |
| body.data[].templateId       | String   | テンプレートID     |
| body.data[].sendNo           | String   | 発信者番号        |
| body.data[].title            | String   | タイトル         |
| body.data[].body             | String   | 内容           |
| body.data[].adYn             | String   | 広告かどうか       |
| body.data[].autoSendYn       | String   | 自動送信を行うかどうか  |
| body.data[].sendErrorCount   | Integer  | エラー受信者件数     |
| body.data[].createUser       | String   | 作成者          |
| body.data[].createDate       | String   | 作成日時         |

### 大量送信受信者リスト検索

#### リクエスト

[URL]

```
GET /sms/v3.0/appKeys/{appKey}/mass-sender/receive/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値         | 	タイプ    | 	説明            |
|-----------|---------|----------------|
| appKey    | 	String | 	固有のアプリケーションキー |
| requestId | String  | リクエストID        |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Query parameter]

* requestIdまたはstartRequestDate + endRequestDateは必須です。

| 値                | 	タイプ    | 最大長さ | 	必須 | 	説明                                                                                                   |
|------------------|---------|------|-----|-------------------------------------------------------------------------------------------------------|
| recipientNo      | String  | 20   | X   | 受信者番号                                                                                                 |
| startRequestDate | String  | -    | O   | 送信リクエスト開始日                                                                                            |
| endRequestDate   | String  | -    | O   | 送信リクエスト終了日                                                                                            |
| startResultDate  | String  | -    | X   | 受信開始日                                                                                                 |
| endResultDate    | String  | -    | X   | 受信終了日                                                                                                 |
| msgStatusName    | String  | 10   | X   | メッセージステータスコード<br/> - READY：準備<br/> - SENDING：送信リクエスト中<br/> - COMPLETED ：送信リクエスト完了<br/> - FAILED ：送信失敗 |
| resultCode       | String  | 10   | X   | 受信結果コード                                                                                               |
| pageNum          | Integer | -    | X   | ページ番号                                                                                                 |
| pageSize         | Integer | 1000 | X   | 検索数                                                                                                   |

#### cURL

```
curl -X GET \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/mass-sender/requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
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
    "pageNum": 1,
    "pageSize": 15,
    "totalCount": 1,
    "data": [
      {
        "requestId": "20210901033436ZLdZtl8GWZ0",
        "recipientSeq": 1,
        "countryCode": "82",
        "recipientNo": "01020060836",
        "requestDate": "2021-09-01 03:34:36.0",
        "msgStatus": "3",
        "msgStatusName": "COMPLETED",
        "messageCount": 1,
        "resultCode": null,
        "receiveDate": null
      }
    ]
  }
}
```

| 値                         | 	タイプ     | 	説明                                        |
|---------------------------|----------|--------------------------------------------|
| header.isSuccessful       | 	Boolean | 	成否                                        |
| header.resultCode         | 	Integer | 	失敗コード                                     |
| header.resultMessage      | 	String  | 	失敗メッセージ                                   |
| body.data[].requestId     | String   | リクエストID                                    |
| body.data[].recipientSeq  | Integer  | 受信者シーケンス                                   |
| body.data[].countryCode   | String   | 受信者国家コード                                   |
| body.data[].recipientNo   | String   | 受信者番号                                      |
| body.data[].requestDate   | String   | リクエスト日時                                    |
| body.data[].msgStatus     | String   | メッセージステータスコード                              |
| body.data[].msgStatusName | String   | メッセージステータスコード名                             |
| body.data[].messageCount  | Integer  | 送信されたメッセージの件数                              |
| body.data[].resultCode    | String   | 受信結果コード[[受信結果コード表](./error-code/#emma-v3)] |
| body.data[].receiveDate   | String   | 受信日時                                       |

### 大量送信受信者リスト詳細検索

#### リクエスト

[URL]

```
GET /sms/v3.0/appKeys/{appKey}/mass-sender/receive/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値            | 	タイプ    | 	説明            |
|--------------|---------|----------------|
| appKey       | 	String | 	固有のアプリケーションキー |
| requestId    | String  | リクエストID        |
| recipientSeq | String  | シーケンス          |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

#### cURL

```
curl -X GET \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/mass-sender/'"${REQUEST_ID}"'/'"${RECIPIENT_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
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
      "requestId": "20210901033436ZLdZtl8GWZ0",
      "recipientSeq": 1,
      "sendType": "0",
      "messageType": "SMS",
      "templateId": "",
      "templateName": null,
      "sendNo": "01012345000",
      "title": null,
      "body": "test",
      "recipientNo": "01020060836",
      "countryCode": "82",
      "requestDate": "2021-09-01 03:34:36.0",
      "msgStatus": "3",
      "msgStatusName": "COMPLETED",
      "messageCount": 0,
      "resultCode": null,
      "receiveDate": null,
      "createDate": null,
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
| body.data.recipientNo                   | String   | 受信者番号                                      |
| body.data.countryCode                   | String   | 受信者国コード                                    |
| body.data.requestDate                   | String   | リクエスト日時                                    |
| body.data.msgStatus                     | String   | メッセージ状態                                    |
| body.data.msgStatusName                 | String   | メッセージ状態名                                   |
| body.data.messageCount                  | Integer  | 送信されたメッセージの件数                              |
| body.data.resultCode                    | String   | 受信結果コード[[受信結果コード表](./error-code/#emma-v3)] |
| body.data.receiveDate                   | String   | 受信日時                                       |
| body.data.createDate                    | String   | 登録日時                                       |
| body.data.attachFileList[].filePath     | String   | 添付ファイル - パス                                |
| body.data.attachFileList[].fileName     | String   | 添付ファイル - ファイル名                             |
| body.data.attachFileList[].fileSize     | Long     | 添付ファイル - サイズ                               |
| body.data.attachFileList[].fileSequence | Integer  | 添付ファイル - ファイル番号                            |
| body.data.attachFileList[].createDate   | String   | 添付ファイル - 作成日時                              |
| body.data.attachFileList[].updateDate   | String   | 添付ファイル - 修正日                               |

## タグ送信

### タグSMS送信

#### リクエスト

[URL]

```
POST /sms/v3.0/appKeys/{appKey}/tag-sender/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

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
  "autoSendYn": "N",
  "statsId": "statsId",
  "originCode": "123456789"
}
```

| 値                 | 	タイプ                | 	最大長さ                                                          | 必須 | 	説明                                 |
|-------------------|---------------------|----------------------------------------------------------------|----|-------------------------------------|
| body              | 	String             | 標準：90バイト、最大：255文字(EUC-KR基準) [[注意事項](./api-guide/#precautions)] | 	O | 	本文内容                               |
| sendNo            | String              | 13                                                             | O  | 発信番号                                |
| requestDate       | String              | -                                                              | X  | 予約日時(yyyy-MM-dd HH:mm)              |
| templateId        | String              | 50                                                             | X  | テンプレートID                            |
| templateParameter | Map<String, String> | -                                                              | X  | テンプレートパラメータ                         |
| tagExpression     | List<String>        | -                                                              | O  | タグ表現式<br/>ex) ["tagA","AND","tabB"] |
| userId            | String              | 100                                                            | X  | リクエストしたユーザーID                       |
| adYn              | String              | 1                                                              | X  | 広告かどうか(デフォルト値：N)                    |
| autoSendYn        | String              | 1                                                              | X  | 自動送信(即時送信)を行うかどうか(デフォルト値：Y)         |
| statsId           | String              | 10                                                             | X  | 統計ID(発信検索条件には含まれません)                |
| originCode        | String              | 10                                                             | X  | Identification code (9-digit registration number, excluding symbols, letters, and spaces, as listed on certificates for special value-added telecommunications business operators)<br/>Do not use unless you are special value-added telecommunications business operator. NHN Cloud's identification code is added by default.<br/> |

#### cURL

```
curl -X POST \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/tag-sender/sms' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}'  \
-d '{
    "body"："本文",
    "sendNo": "15446859",
    "templateParameter": {
        "key": "value"
    },
    "tagExpression": ["TsNuyRPF"],
    "userId": "user_id",
    "adYn": "N",
    "autoSendYn": "N",
    "statsId": "statsId"
}'
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

※ LMS/MMSは海外送信ができません。

#### リクエスト

[URL]

```
POST  /sms/v3.0/appKeys/{appKey}/tag-sender/mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

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
  "autoSendYn": "N",
  "statsId": "statsId",
  "originCode": "123456789"
}
```

| 値                 | 	タイプ                | 	最大長さ | 必須 | 	説明                                 |
|-------------------|---------------------|-------|----|-------------------------------------|
| title             | String              | 120   | O  | メッセージタイトル                           |
| body              | String              | 4000  | O  | メッセージ内容                             |
| sendNo            | String              | 13    | O  | 発信番号                                |
| requestDate       | String              | -     | X  | 予約日時(yyyy-MM-dd HH:mm)              |
| templateId        | String              | 50    | X  | テンプレートID                            |
| templateParameter | Map<String, String> | -     | X  | テンプレートパラメータ                         |
| tagExpression     | List<String>        | -     | O  | タグ表現式<br/>ex) ["tagA","AND","tabB"] |
| attachFileIdList  | List<Integer>       | -     | X  | 添付ファイルID(fileId)                    |
| userId            | String              | 100   | X  | リクエストしたユーザーID                       |
| adYn              | String              | 1     | X  | 広告かどうか(デフォルト値：N)                    |
| autoSendYn        | String              | 1     | X  | 自動送信(即時送信)を行うかどうか(基本Y)              |
| statsId           | String              | 10    | X  | 統計ID(発信検索条件には含まれません)                |
| originCode        | String              | 10    | X  | 識別コード(特殊なタイプの付加通信事業者登録証に記載されている記号、文字、空白を除外した登録番号9桁の数字)               |

#### cURL

```
curl -X POST \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/tag-sender/mms' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}'  \
-d '{
    "title"："タイトル",
    "body"："本文",
    "sendNo": "15446859",
    "templateParameter": {
        "key": "value"
    },
    "tagExpression": ["TsNuyRPF"],
    "userId": "user_id",
    "adYn": "N",
    "autoSendYn": "Y"
}'
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

### タグ送信リスト検索

#### リクエスト

[URL]

```
GET /sms/v3.0/appKeys/{appKey}/tag-sender
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Query parameter]

* requestIdまたはstartRequestDate + endRequestDateまたはstartCreateDate + endCreateDateは必須です。

| 値                | 	タイプ              | 最大長さ | 	必須 | 	説明                                                                                                                                                                   |
|------------------|-------------------|------|-----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| appKey           | String            | -    | O   | アプリケーションキー                                                                                                                                                            |
| sendType         | required, String  | 1    | O   | 送信タイプ<br>SMS ："0",<br>MMS ："1"                                                                                                                                        |
| requestId        | String            | -    | O   | リクエストID                                                                                                                                                               |
| startRequestDate | String            | -    | O   | 送信日開始                                                                                                                                                                 |
| endRequestDate   | String            | -    | O   | 送信日終了                                                                                                                                                                 |
| startCreateDate  | 	String           | -    | 	O  | 	登録日開始                                                                                                                                                                |
| endCreateDate    | 	String           | -    | 	O  | 	登録日終了                                                                                                                                                                |
| statusCode       | String            | 10   | X   | 送信ステータスコード<br>WAIT ："MAS00"<br>READY ："MAS01"<br>SENDREADY ："MAS09"<br>SENDWAIT ："MAS10"<br>SENDING ："MAS11"<br>COMPLETE ："MAS19"<br>CANCEL ："MAS91"<br>FAIL ："MAS99" |
| pageNum          | optional, Integer | -    | X   | ページ番号                                                                                                                                                                 |
| pageSize         | optional, Integer | 1000 | X   | 検索数                                                                                                                                                                   |

#### cURL

```
curl -X GET \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/tag-sender?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "."
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
        "templateName": "templateName",
        "masterStatusCode": "READY",
        "sendNo": "15446859",
        "requestDate": "2017-12-20 14:15:58",
        "tagExpression": [
          "Kb6BjCY1"
        ],
        "title": "title",
        "body": "body",
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

| 値                           | 	タイプ         | 	説明         |
|-----------------------------|--------------|-------------|
| header.isSuccessful         | 	Boolean     | 	成否         |
| header.resultCode           | 	Integer     | 	失敗コード      |
| header.resultMessage        | 	String      | 	失敗メッセージ    |
| body.data[].requestId       | String       | リクエストID     |
| body.data[].requestIp       | String       | リクエストIP     |
| body.data[].requestDate     | String       | リクエスト時間     |
| body.data[].tagSendStatus   | String       | タグ送信ステータス   |
| body.data[].tagExpression[] | List<String> | タグ表現式       |
| body.data[].templateId      | String       | テンプレートID    |
| body.data[].templateName    | String       | テンプレート名     |
| body.data[].senderName      | String       | 発信者名        |
| body.data[].senderMail      | String       | 発信者アドレス     |
| body.data[].title           | String       | タイトル        |
| body.data[].body            | String       | 内容          |
| body.data[].adYn            | String       | 広告かどうか      |
| body.data[].autoSendYn      | String       | 自動送信を行うかどうか |
| body.data[].sendErrorCount  | Integer      | エラー受信者件数    |
| body.data[].createUser      | String       | 作成者         |
| body.data[].createDate      | String       | 作成日時        |
| body.data[].updateUser      | String       | 修正したユーザー    |
| body.data[].updateDate      | String       | 修正日         |

### タグ送信受信者リスト検索

#### リクエスト

[URL]

```
GET /sms/v3.0/appKeys/{appKey}/tag-sender/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値         | 	タイプ    | 	説明            |
|-----------|---------|----------------|
| appKey    | 	String | 	固有のアプリケーションキー |
| requestId | String  | リクエストID        |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Query parameter]

* requestIdまたはstartRequestDate + endRequestDateは必須です。

| 値                | 	タイプ    | 最大長さ | 	必須 | 	説明                                                                                                   |
|------------------|---------|------|-----|-------------------------------------------------------------------------------------------------------|
| recipientNum     | String  | 20   | X   | 受信者番号                                                                                                 |
| startRequestDate | String  | -    | O   | 送信リクエスト開始日                                                                                            |
| endRequestDate   | String  | -    | O   | 送信リクエスト終了日                                                                                            |
| startResultDate  | String  | -    | X   | 受信開始日                                                                                                 |
| endResultDate    | String  | -    | X   | 受信終了日                                                                                                 |
| msgStatusName    | String  | 10   | X   | メッセージステータスコード<br/> - READY：準備<br/> - SENDING：送信リクエスト中<br/> - COMPLETED ：送信リクエスト完了<br/> - FAILED ：送信失敗 |
| resultCode       | String  | 10   | X   | 受信結果コード                                                                                               |
| pageNum          | Integer | -    | X   | ページ番号                                                                                                 |
| pageSize         | Integer | 1000 | X   | 検索数                                                                                                   |

#### cURL

```
curl -X GET \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/tag-sender/'"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "."
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
        "messageCount": 1,
        "resultCode": "3015",
        "receiveDate": "2018-08-13 02:20:53.0",
        "createDate": "2018-08-13 02:20:46.0",
        "updateDate": "2018-08-13 02:27:00.0"
      }
    ]
  }
}
```

| 値                         | 	タイプ     | 	説明                                        |
|---------------------------|----------|--------------------------------------------|
| header.isSuccessful       | 	Boolean | 	成否                                        |
| header.resultCode         | 	Integer | 	失敗コード                                     |
| header.resultMessage      | 	String  | 	失敗メッセージ                                   |
| body.data[].requestId     | String   | リクエストID                                    |
| body.data[].recipientSeq  | Integer  | 受信者シーケンス                                   |
| body.data[].countryCode   | String   | 受信者国家コード                                   |
| body.data[].recipientNo   | String   | 受信者番号                                      |
| body.data[].requestDate   | String   | リクエスト日時                                    |
| body.data[].msgStatus     | String   | メッセージステータスコード                              |
| body.data[].msgStatusName | String   | メッセージステータスコード名                             |
| body.data[].messageCount  | Integer  | 送信されたメッセージ件数                               |
| body.data[].resultCode    | String   | 受信結果コード[[受信結果コード表](./error-code/#emma-v3)] |
| body.data[].receiveDate   | String   | 受信日時                                       |
| body.data[].createDate    | String   | 登録日時                                       |
| body.data[].updateDate    | String   | 修正日付                                       |

### タグ送信受信者詳細検索

#### リクエスト

[URL]

```
GET /sms/v3.0/appKeys/{appKey}/tag-sender/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値            | 	タイプ    | 	説明            |
|--------------|---------|----------------|
| appKey       | 	String | 	固有のアプリケーションキー |
| requestId    | String  | リクエストID        |
| recipientSeq | String  | シーケンス          |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Request body]

```
X
```

#### cURL

```
curl -X GET \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/tag-sender/'"${REQUEST_ID}"'/'"${RECIPIENT_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
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
      "templateName": "templateName",
      "sendNo": "15446859",
      "recipientNum": "01000000000",
      "countryCode": "82",
      "title": "title",
      "body": "body",
      "requestDate": "2018-08-13 02:20:44.0",
      "msgStatusName": "COMPLETED",
      "msgStatus": "3",
      "messageCount": 0,
      "resultCode": "3015",
      "receiveDate": "2018-08-13 02:20:48.0",
      "attachFileList": [],
      "originCode": "123456789"
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
| body.data.msgStatusName                 | String   | メッセージ状態名                                   |
| body.data.messageCount                  | Integer  | 送信されたメッセージの件数                              |
| body.data.resultCode                    | String   | 受信結果コード[[受信結果コード表](./error-code/#emma-v3)] |
| body.data.receiveDate                   | String   | 受信日時                                       |
| body.data.attachFileList[].filePath     | String   | 添付ファイル - パス                                |
| body.data.attachFileList[].fileName     | String   | 添付ファイル - ファイル名                             |
| body.data.attachFileList[].fileSize     | Long     | 添付ファイル - サイズ                               |
| body.data.attachFileList[].fileSequence | Integer  | 添付ファイル - ファイル番号                            |
| body.data.attachFileList[].createDate   | String   | 添付ファイル - 作成日時                              |
| body.data.attachFileList[].updateDate   | String   | 添付ファイル - 修正日                               |
| body.data.originCode                    | String   | 識別コード（特殊なタイプの付加通信事業者登録証に記載されている記号、文字、空白を除く登録番号9桁の数字) |
<span id="binaryUpload"></span>

## 添付ファイル

### 添付ファイルアップロード

[URL]

```
POST  /sms/v3.0/appKeys/{appKey}/attachfile/binaryUpload
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Request body]

```json
{
  "fileName": "attachment.jpg",
  "createUser": "CreateUser",
  "fileBody": "{byte[] -> Base64エンコードした値}"
}
```

| 値          | 	タイプ    | 最大長さ | 	必須 | 	説明                                         |
|------------|---------|------|-----|---------------------------------------------|
| fileName   | 	String | 	45  | 必須  | 	ファイル名(拡張子はjpg, jpeg(小文字)のみ可能)              |
| fileBody   | 	Byte[] | 300K | 	必須 | ファイルbyte[]をBase64でエンコードした値。<br/>* またはバイト配列値 |
| createUser | 	String | 	100 | 必須  | 	ファイルアップロードユーザー情報                           |

#### cURL

```
curl -X POST \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/attachfile/binaryUpload' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}'  \
-d '{
    "fileName": "attachement.jpg",
    "createUser": "API Guide",
    "fileBody": "1234567890"
}'
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
      "fileId": 0,
      "fileName": "attachment.jpg",
      "filePath": "0/toast-mt-2018-08-10/1609/178576"
    }
  }
}
```

| 値                    | 	タイプ     | 	説明                                                            |
|----------------------|----------|----------------------------------------------------------------|
| header.isSuccessful  | 	Boolean | 	成否                                                            |
| header.resultCode    | 	Integer | 	失敗コード                                                         |
| header.resultMessage | 	String  | 	失敗メッセージ                                                       |
| body.data.fileId     | 	Integer | 	ファイルID                                                        |
| body.data.fileName   | 	String  | 	ファイル名                                                         |
| body.data.filePath   | 	String  | 	添付ファイル基本パス <br/>(https://domain/attachFile/filePath/fileName) |

#### 添付ファイルアップロード例

| Http method | URL                                                                               |
|-------------|-----------------------------------------------------------------------------------|
| POST        | https://api-sms.cloud.toast.com/sms/v3.0/appKeys/{appKey}/attachfile/binaryUpload |

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

### カテゴリー登録

#### リクエスト

[URL]

```
POST  /sms/v3.0/appKeys/{appKey}/categories
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

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

| 値                | 	タイプ     | 	最大長さ | 必須     | 	説明                        |
|------------------|----------|-------|--------|----------------------------|
| categoryParentId | 	Integer | 	-    | オプション  | 親カテゴリーID [デフォルト値：最上位カテゴリー] |
| categoryName     | String   | 50    | 必須     | カテゴリーID                    |
| categoryDesc     | 	String  | 100   | 	オプション | 	カテゴリー名                    |
| useYn            | 	String  | 1     | 	必須    | 使用するかどうか(Y/N)              |
| createUser       | 	String  | 100   | オプション  | 登録したユーザー                   |

##### Description

- categoryParentId値が空の場合、最上位カテゴリー直下に登録されます。

#### cURL

```
curl -X POST \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/categories' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}'  \
-d '{
    "categoryParentId": 0,
    "categoryName": "API Guide",
    "categoryDesc"："APIガイドテスト",
    "useYn": "Y",
    "createUser": "API Guide"
}'
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
| body.data[].categorycategoryDescame | 	String  | 	カテゴリー説明    |
| body.data[].useYn                   | 	String  | 	使用するかどうか   |
| body.data[].createUser              | 	String  | 	登録したユーザー   |

### カテゴリーリスト検索

#### リクエスト

[URL]

```
GET  /sms/v3.0/appKeys/{appKey}/categories
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Query parameter]

| 値        | 	タイプ     | 	最大長さ | 必須     | 	説明              |
|----------|----------|-------|--------|------------------|
| pageNum  | 	Integer | -     | 	オプション | 	ページ番号(デフォルト値：1) |
| pageSize | 	Integer | 1000  | 	オプション | 	検索数(デフォルト値：15)  |

#### cURL

```
curl -X GET \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/categories' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
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
    "pageNum": 1,
    "pageSize": 15,
    "totalCount": 1,
    "data": [
      {
        "categoryId": 137612,
        "categoryParentId": 0,
        "depth": 0,
        "sort": 0,
        "categoryName":  "categoryName",
        "categoryDesc":  "categoryDescription",
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
| body.pageSize                       | 	Integer | 	検索されたデータ数  |
| body.totalCount                     | 	Integer | 	データの総数     |
| body.data[].categoryId              | 	Integer | 	カテゴリーID    |
| body.data[].categoryParentId        | 	Integer | 	親カテゴリーID   |
| body.data[].depth                   | 	Integer | 	カテゴリーの深さ   |
| body.data[].sort                    | 	Integer | 	カテゴリーソート順序 |
| body.data[].categoryName            | 	String  | 	カテゴリー名     |
| body.data[].categorycategoryDescame | 	String  | 	カテゴリー説明    |
| body.data[].useYn                   | 	String  | 	使用するかどうか   |
| body.data[].createDate              | 	String  | 	登録日        |
| body.data[].createUser              | 	String  | 	登録したユーザー   |
| body.data[].updateDate              | 	String  | 	修正日        |
| body.data[].updateUser              | 	String  | 	修正したユーザー   |

### カテゴリー単件検索

#### リクエスト

[URL]

```
GET  /sms/v3.0/appKeys/{appKey}/categories/{categoryId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値          | 	タイプ     | 	説明            |
|------------|----------|----------------|
| appKey     | 	String  | 	固有のアプリケーションキー |
| categoryId | 	Integer | 	カテゴリーID       |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

#### cURL

```
curl -X GET \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/categories/'"${CATEGORY_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
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
    "data": [
      {
        "categoryId": 137612,
        "categoryParentId": 0,
        "depth": 0,
        "sort": 0,
        "categoryName": "categoryName",
        "categoryDesc": "categoryDescription",
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
| body.data[].categorycategoryDescame | 	String  | 	カテゴリー説明    |
| body.data[].useYn                   | 	String  | 	使用するかどうか   |
| body.data[].createDate              | 	String  | 	登録日        |
| body.data[].createUser              | 	String  | 	登録したユーザー   |
| body.data[].updateDate              | 	String  | 	修正日        |
| body.data[].updateUser              | 	String  | 	修正したユーザー   |

### カテゴリー修正

#### リクエスト

[URL]

```
PUT  /sms/v3.0/appKeys/{appKey}/categories/{categoryId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値          | 	タイプ     | 	説明            |
|------------|----------|----------------|
| appKey     | 	String  | 	固有のアプリケーションキー |
| categoryId | 	Integer | 	カテゴリーID       |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Request body]

```json
{
  "categoryName": "",
  "categoryDesc": "",
  "useYn": "",
  "updateUser": ""
}
```

| 値            | 	タイプ    | 	最大長さ | 必須     | 	説明           |
|--------------|---------|-------|--------|---------------|
| categoryName | String  | 50    | 必須     | カテゴリーID       |
| categoryDesc | 	String | 100   | 	オプション | 	カテゴリー名       |
| useYn        | 	String | 1     | 	必須    | 使用するかどうか(Y/N) |
| updateUser   | 	String | 100   | 	オプション | 修正したユーザー      |

#### cURL

```
curl -X PUT \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/categories/'"${C_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}'  \
-d '{
    "categoryParentId": 788,
    "categoryName": "secondMMS",
    "categoryDesc": "second category MMS",
    "useYn": "Y",
    "createUser": "467d9790-ea74-11e5-9ad3-005056ac76e8"
}'
```

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

### カテゴリー削除

#### リクエスト

[URL]

```
DELETE  /sms/v3.0/appKeys/{appKey}/categories/{categoryId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値          | 	タイプ     | 	説明            |
|------------|----------|----------------|
| appKey     | 	String  | 	固有のアプリケーションキー |
| categoryId | 	Integer | 	カテゴリーID       |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

#### cURL

```
curl -X DELETE \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/categories/'"${CATEGORY_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

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

### テンプレート登録

#### リクエスト

[URL]

```
POST  /sms/v3.0/appKeys/{appKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

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

| 値                | 	タイプ          | 	最大長さ | 必須     | 	説明                            |
|------------------|---------------|-------|--------|--------------------------------|
| categoryId       | 	Integer      | 	-    | 必須     | 	カテゴリーID                       |
| templateId       | String        | 50    | 必須     | テンプレートID                       |
| templateName     | 	String       | 50    | 	必須    | 	テンプレート名                       |
| templateDesc     | 	String       | 100   | 	オプション | 	テンプレート説明                      |
| sendNo           | String        | 13    | 必須     | 発信番号                           |
| sendType         | String        | 1     | 必須     | 送信タイプ(0：Sms、1：Lms/Mms)         |
| title            | String        | 120   | オプション  | メッセージタイトル(送信タイプがLMS/MMSの場合は必須) |
| body             | String        | 4000  | 必須     | メッセージ内容                        |
| useYn            | 	String       | 1     | 	必須    | 	使用するかどうか(Y/N)                 |
| attachFileIdList | List<Integer> | -     | X      | 添付ファイルID(fileId)               |

#### cURL

```
curl -X POST \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/templates' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}'  \
-d '{
    "categoryId": 199376,
    "templateId": "TemplateId",
    "templateName"："テンプレート送信例",
    "templateDesc"："テンプレート送信例",
    "sendNo": "01012341234",
    "sendType": "1",
    "title": "example",
    "body"："一般送信テスト用です。\r\n##key1## さん、こんにちは。\r\n##key2## です。",
    "useYn": "Y"
}'
```

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

#### テンプレート登録例

| Http method | URL                                                                 |
|-------------|---------------------------------------------------------------------|
| POST        | https://api-sms.cloud.toast.com/sms/v3.0/appKeys/{appKey}/templates |

[Request body]

```json
{
  "categoryId": 199376,
  "templateId": "TemplateId",
  "templateName": "templateName",
  "templateDesc": "templateDescription",
  "sendNo": "01012341234",
  "sendType": "1",
  "title": "example",
  "body": "一般送信テスト用です。\r\n##key1## さん、こんにちは。\r\n##key2## です。",
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

##### Description

- 添付ファイル(フィールド名：attachFileIdList)を含むテンプレート登録は、事前に添付ファイルのアップロードを行う必要があります。<br>
- [[添付ファイルアップロード](./api-guide/#binaryUpload)]</a> ガイドを参照してください。
- 添付画像制限事項
    - サポートコーデック：jpg
    - 添付画像数：3個以下
    - 添付画像サイズ：300K以下
    - 添付画像解像度：1000 x 1000以下

### テンプレート送信(本文の修正が必要ない場合)

| Http method | 種類  | URL                                                                  |
|-------------|-----|----------------------------------------------------------------------|
| POST        | SMS | https://api-sms.cloud.toast.com/sms/v3.0/appKeys/{appKey}/sender/sms |
| POST        | MMS | https://api-sms.cloud.toast.com/sms/v3.0/appKeys/{appKey}/sender/mms |

Request URLはテンプレート登録時に選択した送信タイプを選択して送信します。

**Request parameter body内の値が空欄の場合、そのtemplateIdのbody内容に置換します。**

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

![[図1]テンプレート送信成功](http://static.toastoven.net/prod_sms/img_27.png)

### テンプレート送信(本文の修正が必要な場合)

#### テンプレート送信例

| Http method | 種類  | URL                                                                  |
|-------------|-----|----------------------------------------------------------------------|
| POST        | SMS | https://api-sms.cloud.toast.com/sms/v3.0/appKeys/{appKey}/sender/sms |
| POST        | MMS | https://api-sms.cloud.toast.com/sms/v3.0/appKeys/{appKey}/sender/mms |

Request URLはテンプレート登録時に選択した送信タイプを選択して送信します。

**テンプレートIDとRequest parameter body内の値がある場合、発信番号と発信内容がテンプレートの内容に置換されません。**

ただし、テンプレートIDを入力した場合、そのテンプレートで検索できます。

上記のような場合はテンプレートを検索した後、テンプレートの内容を修正したいときに使用できます。

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

### テンプレートリスト検索

#### リクエスト

[URL]

```
GET  /sms/v3.0/appKeys/{appKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Query parameter]

| 値          | 	タイプ     | 	必須    | 	説明              |
|------------|----------|--------|------------------|
| categoryId | 	Integer | 	オプション | 	カテゴリーID         |
| useYn      | 	String  | 	オプション | 	使用するかどうか(Y/N)   |
| pageNum    | 	Integer | オプション  | 	ページ番号(デフォルト値：1) |
| pageSize   | 	Integer | オプション  | 	検索数(デフォルト値：15)  |

#### cURL

```
curl -X GET \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/templates' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

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
    "totalCount": 1,
    "data": [
      {
        "templateId": "ee13efe5-58c2-4e61-a59f-1fa47cc21cd0",
        "serviceId": 71191,
        "categoryId": 415978,
        "categoryName": "categoryName",
        "sort": 0,
        "templateName": "templateName",
        "templateDesc": "templateDescription",
        "useYn": "Y",
        "priority": "S",
        "sendNo": "12341234",
        "sendType": "0",
        "sendTypeName": "SMS 발송",
        "title": "title",
        "body": "body",
        "attachFileYn": "Y",
        "delYn": "N",
        "createDate": "2023-09-18 14:30:03.0",
        "createUser": null,
        "updateDate": "2023-09-18 14:30:03.0",
        "updateUser": null,
        "attachFileList": [
          {
            "fileId": 535186,
            "filePath": "/permanent/71191/toast-mt-2023-09-18/1430/535186",
            "fileName": "attachment.jpg",
            "saveFileName": "20230918eA8JmR0.jpg",
            "uploadType": "TEMPORARY"
          }
        ]
      }
    ]
  }
}
```

| 値                                         | 	タイプ     | 	説明                              |
|-------------------------------------------|----------|----------------------------------|
| header.isSuccessful                       | 	Boolean | 	成否                              |
| header.resultCode                         | 	Integer | 	失敗コード                           |
| header.resultMessage                      | 	String  | 	失敗メッセージ                         |
| body.pageNum                              | 	Integer | 	現在のページ番号                        |
| body.pageSize                             | 	Integer | 	検索されたデータ数                       |
| body.totalCount                           | 	Integer | 	データの総数                          |
| body.data[].templateId                    | 	String  | 	テンプレートID                        |
| body.data[].serviceId                     | 	Integer | 	サービスID(内部用、未使用値)                |
| body.data[].categoryId                    | 	Integer | 	カテゴリーID                         |
| body.data[].categoryName                  | 	String  | 	カテゴリー名                          |
| body.data[].sort                          | 	Integer | 	ソート値                            |
| body.data[].templateName                  | 	String  | 	テンプレート名                         |
| body.data[].templateDesc                  | 	String  | 	テンプレート説明                        |
| body.data[].useYn                         | 	String  | 	使用するかどうか                        |
| body.data[].priority                      | 	String  | 	優先順位値(未使用値)                     |
| body.data[].sendNo                        | 	String  | 	発信番号                            |
| body.data[].sendType                      | 	String  | 	送信タイプ(0:Sms, 1:Lms/Mms, 2:Auth) |
| body.data[].sendTypeName                  | 	String  | 	送信タイプ名                          |
| body.data[].title                         | 	String  | 	タイトル                            |
| body.data[].body                          | 	String  | 	本文内容                            |
| body.data[].attachFileYn                  | 	String  | 	添付ファイルかどうか(Y/N)                 |
| body.data[].delYn                         | 	String  | 	削除Y/N、現在状態表記用でのみ使用              |
| body.data[].createDate                    | 	String  | 	登録日                             |
| body.data[].createUser                    | 	String  | 	登録したユーザー                        |
| body.data[].updateDate                    | 	String  | 	修正日                             |
| body.data[].updateUser                    | 	String  | 	修正したユーザー                        |
| body.data[].attachFileList[].fileId       | 	Integer | 	ファイルID                          |
| body.data[].attachFileList[].filePath     | 	String  | 	ファイル保存パス(内部用)                   |
| body.data[].attachFileList[].filename     | 	String  | 	ファイル名                           |
| body.data[].attachFileList[].saveFileName | 	String  | 	保存された添付ファイル名                    |
| body.data[].attachFileList[].uploadType   | 	String  | 	アップロードタイプ                       |

### テンプレート単一検索

#### リクエスト

[URL]

```
GET  /sms/v3.0/appKeys/{appKey}/templates/{templateId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値          | 	タイプ    | 	説明            |
|------------|---------|----------------|
| appKey     | 	String | 	固有のアプリケーションキー |
| templateId | 	String | 	テンプレートID      |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

#### cURL

```
curl -X GET \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/templates/'"${TEMPLATE_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

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
      "templateId": "ee13efe5-58c2-4e61-a59f-1fa47cc21cd0",
      "serviceId": 71191,
      "categoryId": 415978,
      "categoryName": "categoryName",
      "sort": 0,
      "templateName": "templateName",
      "templateDesc": "templateDescription",
      "useYn": "Y",
      "priority": "S",
      "sendNo": "12341234",
      "sendType": "0",
      "sendTypeName": "SMS 발송",
      "title": "title",
      "body": "body",
      "attachFileYn": "Y",
      "delYn": "N",
      "createDate": "2023-09-18 14:30:03.0",
      "createUser": null,
      "updateDate": "2023-09-18 14:30:03.0",
      "updateUser": null,
      "attachFileList": [
        {
          "fileId": 535186,
          "filePath": "/permanent/71191/toast-mt-2023-09-18/1430/535186",
          "fileName": "attachment.jpg",
          "saveFileName": "20230918eA8JmR0.jpg",
          "uploadType": "TEMPORARY"
        }
      ]
    }
  }
}
```

| 値                                         | 	タイプ     | 	説明                              |
|-------------------------------------------|----------|----------------------------------|
| header.isSuccessful                       | 	Boolean | 	成否                              |
| header.resultCode                         | 	Integer | 	失敗コード                           |
| header.resultMessage                      | 	String  | 	失敗メッセージ                         |
| body.pageNum                              | 	Integer | 	現在のページ番号                        |
| body.pageSize                             | 	Integer | 	検索されたデータ数                       |
| body.totalCount                           | 	Integer | 	データの総数                          |
| body.data.templateId                      | 	String  | 	テンプレートID                        |
| body.data.serviceId                       | 	Integer | 	サービスID(内部用、未使用値)                |
| body.data.categoryId                      | 	Integer | 	カテゴリーID                         |
| body.data.categoryName                    | 	String  | 	カテゴリー名                          |
| body.data.sort                            | 	Integer | 	ソート値                            |
| body.data.templateName                    | 	String  | 	テンプレート名                         |
| body.data.templateDesc                    | 	String  | 	テンプレート説明                        |
| body.data.useYn                           | 	String  | 	使用するかどうか                        |
| body.data.priority                        | 	String  | 	優先順位値(未使用値)                     |
| body.data.sendNo                          | 	String  | 	発信番号                            |
| body.data.sendType                        | 	String  | 	送信タイプ(0:Sms, 1:Lms/Mms, 2:Auth) |
| body.data.sendTypeName                    | 	String  | 	送信タイプ名                          |
| body.data.title                           | 	String  | 	タイトル                            |
| body.data.body                            | 	String  | 	本文内容                            |
| body.data.attachFileYn                    | 	String  | 	添付ファイルかどうか(Y/N)                 |
| body.data.delYn                           | 	String  | 	削除Y/N、現在態表記用でのみ使用               |
| body.data.createDate                      | 	String  | 	登録日                             |
| body.data.createUser                      | 	String  | 	登録したユーザー                        |
| body.data.updateDate                      | 	String  | 	修正日                             |
| body.data.updateUser                      | 	String  | 	修正したユーザー                        |
| body.data[].attachFileList[].fileId       | 	Integer | 	ファイルID                          |
| body.data[].attachFileList[].filePath     | 	String  | 	ファイル保存パス(内部用)                   |
| body.data[].attachFileList[].filename     | 	String  | 	ファイル名                           |
| body.data[].attachFileList[].saveFileName | 	String  | 	保存された添付ファイル名                    |
| body.data[].attachFileList[].uploadType   | 	String  | 	アップロードタイプ                       |

### テンプレート修正

#### リクエスト

[URL]

```
PUT  /sms/v3.0/appKeys/{appKey}/templates/{templateId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

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

| 値                | 	タイプ          | 	最大長さ | 必須     | 	説明                            |
|------------------|---------------|-------|--------|--------------------------------|
| templateName     | 	String       | 50    | 	必須    | 	テンプレート名                       |
| templateDesc     | 	String       | 100   | 	オプション | 	テンプレート説明                      |
| sendNo           | String        | 13    | 必須     | 発信番号                           |
| sendType         | String        | 1     | 必須     | 送信タイプ(0：Sms、1：Lms/Mms)         |
| title            | String        | 120   | オプション  | メッセージタイトル(送信タイプがLMS/MMSの場合は必須) |
| body             | String        | 4000  | 必須     | メッセージ内容                        |
| useYn            | 	String       | 1     | 	必須    | 	使用するかどうか(Y/N)                 |
| attachFileIdList | List<Integer> | -     | オプション  | 添付ファイルID(fileId)               |

#### cURL

```
curl -X GET \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/templates/'"${TEMPLATE_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

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

### テンプレート削除

#### リクエスト

[URL]

```
DELETE  /sms/v3.0/appKeys/{appKey}/templates/{templateId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値          | 	タイプ    | 	説明            |
|------------|---------|----------------|
| appKey     | 	String | 	固有のアプリケーションキー |
| templateId | 	String | 	テンプレートID      |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

#### cURL

```
curl -X DELETE \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/templates/'"${TEMPLATE_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

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

### 受信拒否対象者の登録

#### リクエスト

[URL]

```
POST /sms/v3.0/appKeys/{appKey}/blockservice/recipients
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

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

| 値               | 	タイプ         | 	最大長さ | 必須 | 	説明       |
|-----------------|--------------|-------|----|-----------|
| unsubscribeNo   | String       | 25    | O  | 080受信拒否番号 |
| recipientNoList | List<String> | 10    | O  | 受信拒否対象者番号 |

#### cURL

```
curl -X POST \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/blockservice/recipients' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}'  \
-d '{
    "unsubscribeNo": "0800000000",
    "recipientNoList": ["0100000000", "0100000001"]
}'
```

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

### 受信拒否対象者の検索

#### リクエスト

[URL]

```
GET  /sms/v3.0/appKeys/{appKey}/blockservice/recipients
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Query parameter]

| 値                | 	タイプ     | 	最大長さ | 必須     | 	説明                                |
|------------------|----------|-------|--------|------------------------------------|
| unsubscribeNo    | 	String  | 25    | 	必須    | 	080受信拒否番号                         |
| recipientNo      | String   | 25    | オプション  | 受信拒否対象者番号                          |
| startRequestDate | 	String  | -     | 	オプション | 	受信拒否リクエスト開始値(yyyy-MM-dd HH:mm:ss) |
| endRequestDate   | 	String  | -     | 	オプション | 	受信拒否リクエスト終了値(yyyy-MM-dd HH:mm:ss) |
| pageNum          | 	Integer | -     | 	オプション | 	ページ番号(デフォルト値：1)                   |
| pageSize         | 	Integer | 1000  | 	オプション | 	検索数(デフォルト値：15)                    |

#### cURL

```
curl -X GET \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/blockservice/recipients' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
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
DELETE  /sms/v3.0/appKeys/{appKey}/blockservice/recipients/removes?unsubscribeNo={unsubscribeNo}&updateUser={updateUser}&recipientNoList={recipientNo},{recipientNo}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Query parameter]

| 値             | 	タイプ    | 	最大長さ | 必須  | 	説明         |
|---------------|---------|-------|-----|-------------|
| unsubscribeNo | 	String | 20    | 	必須 | 	080受信拒否番号  |
| updateUser    | 	String | 	100  | 必須  | 	受信拒否削除者    |
| recipientNo   | 	String | 	20   | 必須  | 	削除する受信拒否番号 |

#### cURL

```
curl -X DELETE \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/blockservice/recipients/removes?unsubscribeNo='"${UNSUB_NO}"'&updateUser='"${UPDATE_USER}"'&recipientNoList='"${RECIPIENT_NO}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

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

### 登録された発信番号リスト検索API

#### リクエスト

[URL]

| Http method | 	URI                                                                                                                      |
|-------------|---------------------------------------------------------------------------------------------------------------------------|
| GET         | 	/sms/v3.0/appKeys/{appKey}/sendNos?sendNo={sendNo}&useYn={useYn}&blockYn={blockYn}&pageNum={pageNum}&pageSize={pageSize} |

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Query parameter]

| 値        | 	タイプ     | 	説明             |
|----------|----------|-----------------|
| sendNo   | String   | 発信番号            |
| useYn    | String   | 使用するかどうか        |
| blockYn  | String   | ブロックするかどうか      |
| pageNum  | 	Integer | ページ番号(デフォルト値：1) |
| pageSize | 	Integer | 検索数(デフォルト値：15)  |

#### cURL

```
curl -X GET \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/sendNos' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
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
| body.pageSize           | 	Integer | 	検索されたデータ数 |
| body.totalCount         | 	Integer | 	データの総数    |
| body.data[].serviceId   | Integer  | サービスID     |
| body.data[].sendNo      | String   | 発信番号       |
| body.data[].useYn       | String   | 使用するかどうか   |
| body.data[].blockYn     | String   | ブロックするかどうか |
| body.data[].blockReason | String   | ブロック理由     |
| body.data[].createDate  | String   | 作成日時       |
| body.data[].createUser  | String   | 作成者        |
| body.data[].updateDate  | String   | 修正日        |
| body.data[].updateUser  | String   | 修正したユーザー   |

## 統計

### 統計検索 - イベント基盤

* イベント発生時間を基準に収集された統計です。
* 次の時間を基準に統計が収集されます。
    * リクエスト数(requested)：送信リクエスト時間
    * 送信数(sent)：通信事業者(ベンダー)に送信リクエストした時間
    * 成功数(received)：実際の端末受信時間
    * 失敗数(sentFailed)：失敗レスポンスが発生した時間

#### リクエスト

[URL]

| Http method | 	URI                              |
|-------------|-----------------------------------|
| GET         | 	/sms/v3.0/appKeys/{appKey}/stats |

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Query parameter]

| 値              | 	タイプ         | 	最大長さ | 必須                                                                                                                                                             | 説明                                                             |
|----------------|--------------|-------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------|
| statisticsType | String       | -     | 必須                                                                                                                                                             | 統計区分<br/>NORMAL：基本、 MINUTELY：分別、HOURLY：時間別、DAILY：日別、BY_DAY：曜日別 |
| from           | String       | -     | 必須                                                                                                                                                             | 統計検索開始日<br/>yyyy-MM-dd HH:mm:ss                                | 
| to             | String       | -     | 必須                                                                                                                                                             | 統計検索終了日<br/>yyyy-MM-dd HH:mm:ss                                |
| statsIds       | List<String> | -     | オプション                                                                                                                                                          | 統計IDリスト                                                        |
| messageType    | String       | -     | オプション                                                                                                                                                          | メッセージタイプ<br/>SMS、LMS、MMS、AUTH                                  |
| isAd           | Boolean      | -     | オプション                                                                                                                                                          | 広告かどうか<br/>true/false                                          |
| templateIds    | List<String> | -     | オプション                                                                                                                                                          | テンプレートIDリスト                                                    |
| requestIds     | List<String> | 5     | オプション                                                                                                                                                          | リクエストIDリスト                                                     |
| statsCriteria  | List<String> | オプション | 統計基準<br/>- EVENT：イベント(基本値)<br/>- TEMPLATE_ID、EVENT：テンプレート、イベント<br/>- EXTRA_1、EVENT：メッセージタイプ、イベント<br/>- EXTRA_2、EVENT：広告かどうか、イベント<br/>- EXTRA_3、EVENT：発信番号、イベント |

#### cURL

```
curl -X GET \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/stats?statisticsType='"${STATISTICS_TYPE}"'&from='"${FROM}"'&to='"${TO}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
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
    "data": [
      {
        "eventDateTime": "",
        "events": {
          "{statsCriteriaValue}.requested": 10,
          "{statsCriteriaValue}.sent": 10,
          "{statsCriteriaValue}.sentFailed": 0,
          "{statsCriteriaValue}.received": 0
        }
      }
    ]
  }
}
```

| 値                                                  | 	タイプ     | 	説明                                                                                                             |
|----------------------------------------------------|----------|-----------------------------------------------------------------------------------------------------------------|
| header.isSuccessful                                | 	Boolean | 	成否                                                                                                             |
| header.resultCode                                  | 	Integer | 	失敗コード                                                                                                          |
| header.resultMessage                               | 	String  | 	失敗メッセージ                                                                                                        |
| body.data.eventDateTime                            | 	String  | 	表示名<br/>分別、時間別、曜日別、月別                                                                                          |
| body.data.events[].{statsCriteriaValue}            | List     | statsCriteriaに該当する値<br/>メッセージタイプ/広告タイプ/発信番号値が来る場合がある<br/>statsCriteriaをEVENTにのみ設定した場合{statsCriteriaValue}は省略される |
| body.data.events[].{statsCriteriaValue}.requested  | 	Integer | 	リクエスト数                                                                                                         |
| body.data.events[].{statsCriteriaValue}.sent       | 	Integer | 	送信数                                                                                                            |
| body.data.events[].{statsCriteriaValue}.sentFailed | 	Integer | 	失敗数                                                                                                            |
| body.data.events[].{statsCriteriaValue}.received   | 	Integer | 	成功数                                                                                                            |

### 統計検索 - リクエスト時間ベース

* 送信リクエスト時間基準で収集された統計です。
* 次の時間を基準に統計が収集されます。
    * リクエスト数(requested)：送信リクエスト時間
    * 送信数(sent)：送信リクエスト時間で、数が増加する時点は通信事業者(ベンダー)に送信リクエストした時間
    * 成功数(received)：送信リクエスト時間で、数が増加する時点は、実際の端末受信時間
    * 失敗数(sentFailed)：送信リクエスト時間で、数が増加する時点は失敗レスポンスが発生した時間

#### リクエスト

[URL]

| Http method | 	URI                                     |
|-------------|------------------------------------------|
| GET         | 	/sms/v3.0/appKeys/{appKey}/stats/legacy |

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Query parameter]

| 値              | 	タイプ         | 	最大長さ | 必須                                                                                                                                                             | 説明                                                             |
|----------------|--------------|-------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------|
| statisticsType | String       | -     | 必須                                                                                                                                                             | 統計区分<br/>NORMAL：基本、 MINUTELY：分別、HOURLY：時間別、DAILY：日別、BY_DAY：曜日別 |
| from           | String       | -     | 必須                                                                                                                                                             | 統計検索開始日<br/>yyyy-MM-dd HH:mm:ss                                | 
| to             | String       | -     | 必須                                                                                                                                                             | 統計検索終了日<br/>yyyy-MM-dd HH:mm:ss                                |
| statsIds       | List<String> | -     | オプション                                                                                                                                                          | 統計IDリスト                                                        |
| messageType    | String       | -     | オプション                                                                                                                                                          | メッセージタイプ<br/>SMS、LMS、MMS、AUTH                                  |
| isAd           | Boolean      | -     | オプション                                                                                                                                                          | 広告かどうか<br/>true/false                                          |
| templateIds    | List<String> | -     | オプション                                                                                                                                                          | テンプレートIDリスト                                                    |
| requestIds     | List<String> | 5     | オプション                                                                                                                                                          | リクエストIDリスト                                                     |
| statsCriteria  | List<String> | オプション | 統計基準<br/>- EVENT：イベント(基本値)<br/>- TEMPLATE_ID、EVENT：テンプレート、イベント<br/>- EXTRA_1、EVENT：メッセージタイプ、イベント<br/>- EXTRA_2、EVENT：広告かどうか、イベント<br/>- EXTRA_3、EVENT：発信番号、イベント |

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
        "eventDateTime": "",
        "events": {
          "{statsCriteriaValue}.requested": 10,
          "{statsCriteriaValue}.sent": 10,
          "{statsCriteriaValue}.sentFailed": 0,
          "{statsCriteriaValue}.received": 0,
          "{statsCriteriaValue}.pending": 0
        }
      }
    ]
  }
}
```

| 値                                                  | 	タイプ     | 	説明                                                                                                             |
|----------------------------------------------------|----------|-----------------------------------------------------------------------------------------------------------------|
| header.isSuccessful                                | 	Boolean | 	成否                                                                                                             |
| header.resultCode                                  | 	Integer | 	失敗コード                                                                                                          |
| header.resultMessage                               | 	String  | 	失敗メッセージ                                                                                                        |
| body.data.eventDateTime                            | 	String  | 	表示名<br/>分別、時間別、曜日別、月別                                                                                          |
| body.data.events[].{statsCriteriaValue}            | List     | statsCriteriaに該当する値<br/>メッセージタイプ/広告タイプ/発信番号値が来る場合がある<br/>statsCriteriaをEVENTにのみ設定した場合{statsCriteriaValue}は省略される |
| body.data.events[].{statsCriteriaValue}.requested  | 	Integer | 	リクエスト数                                                                                                         |
| body.data.events[].{statsCriteriaValue}.sent       | 	Integer | 	送信数                                                                                                            |
| body.data.events[].{statsCriteriaValue}.sentFailed | 	Integer | 	失敗数                                                                                                            |
| body.data.events[].{statsCriteriaValue}.received   | 	Integer | 	成功数                                                                                                            |
| body.data.events[].{statsCriteriaValue}.pending    | 	Integer | 	送信中数                                                                                                           |

### (旧)統合統計検索

#### リクエスト

[URL]

| Http method | 	URI                                                                                                                                                                  |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| GET         | 	/sms/v3.0/appKeys/{appKey}/statistics/view?searchType={searchType}&from={from}&to={to}&messageTypes={messageType}&contentTypes={contentType}&templateId={templateId} |

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Query parameter]

| 値           | 	タイプ   | 	最大長さ | 必須 | 説明                                              |
|-------------|--------|-------|----|-------------------------------------------------|
| searchType  | String | 10    | O  | 統計区分<br/>DATE：日別、TIME：時間別、 DAY：曜日別              |
| from        | String | -     | O  | 統計検索開始日<br/>yyyy-MM-dd HH:mm                    |
| to          | String | -     | O  | 統計検索終了日<br/>yyyy-MM-dd HH:mm                    |
| messageType | String | 10    | X  | メッセージタイプ<br/>SMS：短文、LMS：長文、MMS：添付ファイル、 AUTH：認証用 |
| contentType | String | 10    | X  | コンテンツタイプ<br/>NORMAL：一般、 AD：広告                   |
| templateId  | String | 50    | X  | テンプレートID                                        |

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

| 値                          | タイプ      | 説明              |
|----------------------------|----------|-----------------|
| header.isSuccessful        | 	Boolean | 	成否             |
| header.resultCode          | 	Integer | 	失敗コード          |
| header.resultMessage       | 	String  | 	失敗メッセージ        |
| body.data[].divisionName   | String   | 表示名<br/>日、時間、曜日 |
| body.data[].statisticsView | Object   |                 |
| body.data[].requestedCount | Integer  | リクエスト数          |
| body.data[].succeedCount   | Integer  | 成功数             |
| body.data[].failedCount    | Integer  | 失敗数             |
| body.data[].pendingCount   | Integer  | 送信中の数           |
| body.data[].succeedRate    | String   | 成功比率            |
| body.data[].failedRate     | String   | 失敗比率            |
| body.data[].pendingRate    | String   | 送信中の比率          |

## 予約送信

### 予約送信リスト検索

#### リクエスト

[URL]

```
GET /sms/v3.0/appKeys/{appKey}/reservations
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Query parameter]

| 値                | 	タイプ     | 	最大長さ | 必須     | 	説明                                                                                                   |
|------------------|----------|-------|--------|-------------------------------------------------------------------------------------------------------|
| sendType         | String   | 1     | オプション  | 送信タイプ<br/>(0：SMS、1：LMS/MMS、2：AUTH)                                                                    |
| requestId        | 	String  | 25    | 	オプション | 	リクエストID                                                                                              |
| startRequestDate | 	String  | -     | 	オプション | 	送信日開始値(yyyy-MM-dd HH:mm:ss)                                                                          |
| endRequestDate   | 	String  | -     | 	オプション | 	送信日終了値(yyyy-MM-dd HH:mm:ss)                                                                          |
| startCreateDate  | 	String  | -     | 	オプション | 	登録日開始値(yyyy-MM-dd HH:mm:ss)                                                                          |
| endCreateDate    | 	String  | -     | 	オプション | 	登録日終了値(yyyy-MM-dd HH:mm:ss)                                                                          |
| sendNo           | 	String  | 13    | オプション  | 	発信番号                                                                                                 |
| recipientNo      | 	String  | 20    | 	オプション | 	受信番号                                                                                                 |
| templateId       | 	String  | 50    | 	オプション | 	テンプレート番号                                                                                             |
| messageStatus    | 	String  | 10    | 	オプション | 	メッセージ状態<br/>(RESERVED：予約待機、 SENDING：送信中、 COMPLETED：送信完了、 FAILED：送信失敗、CANCEL：キャンセル、DUPLICATED：重複送信) |
| pageNum          | 	Integer | -     | 	オプション | 	ページ番号(デフォルト値：1)                                                                                      |
| pageSize         | 	Integer | 1000  | 	オプション | 	検索数(デフォルト値：15)                                                                                       |

#### cURL

```
curl -X GET \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/reservations' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

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
        "messageStatus": "{メッセージ状態}",
        "createUser": "{登録したユーザー}",
        "createDate": "{登録日時}",
        "updateDate": "{修正日}"
      }
    ]
  }
}
```

| 値                             | 	タイプ          | 	説明                                                                                                |
|-------------------------------|---------------|----------------------------------------------------------------------------------------------------|
| header.isSuccessful           | 	Boolean      | 	成否                                                                                                |
| header.resultCode             | 	Integer      | 	失敗コード                                                                                             |
| header.resultMessage          | 	String       | 	失敗メッセージ                                                                                           |
| body.pageNum                  | 	Integer      | 	現在のページ番号                                                                                          |
| body.pageSize                 | 	Integer      | 	検索されたデータ数                                                                                         |
| body.totalCount               | 	Integer      | 	データの総数                                                                                            |
| body.data[].requestId         | 	String       | 	リクエストID                                                                                           |
| body.data[].recipientSeq      | 	Integer      | 	受信者シーケンス                                                                                          |
| body.data[].requestDate       | 	String       | 	発信日時                                                                                              |
| body.data[].sendNo            | 	String       | 	発信番号                                                                                              |
| body.data[].recipientNo       | 	String       | 	受信番号                                                                                              |
| body.data[].countryCode       | 	String       | 	国番号                                                                                               |
| body.data[].sendType          | 	String       | 	送信タイプ(0:Sms, 1:Lms/Mms, 2:Auth)                                                                   |
| body.data[].messageType       | 	String       | 	メッセージタイプ<br/>(SMS,LMS,MMS,AUTH)                                                                   |
| body.data[].adYn              | 	String       | 	広告かどうか                                                                                            |
| body.data[].templateId        | 	String       | 	テンプレートID                                                                                          |
| body.data[].templateParameter | 	String(json) | 	テンプレートパラメータ                                                                                       |
| body.data[].templateName      | 	String       | 	テンプレート名                                                                                           |
| body.data[].title             | 	String       | 	タイトル                                                                                              |
| body.data[].body              | 	String       | 	本文内容                                                                                              |
| body.data[].messageStatus     | 	String       | 	メッセージ状態<br/>(RESERVED：予約待機、SENDING：送信中、COMPLETED：送信完了、FAILED：送信失敗、CANCEL：キャンセル、DUPLICATED：重複送信) |
| body.data[].createUser        | 	String       | 	登録したユーザー                                                                                          |
| body.data[].createDate        | 	String       | 	登録日                                                                                               |
| body.data[].updateDate        | 	String       | 	修正日                                                                                               |

### 予約送信詳細検索

#### リクエスト

[URL]

```
GET /sms/v3.0/appKeys/{appKey}/reservations/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値            | 	タイプ     | 	説明            |
|--------------|----------|----------------|
| appKey       | 	String  | 	固有のアプリケーションキー |
| requestId    | 	String  | 	リクエストID       |
| recipientSeq | 	Integer | 	受信者シーケンス      |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

#### cURL

```
curl -X GET \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/reservations/'"${R_ID}"'/'"${R_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

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
      "messageStatus": "{メッセージ状態}",
      "createUser": "{登録したユーザー}",
      "createDate": "{登録日時}",
      "updateDate": "{修正日}",
      "attachFileList": [
        {
          "fileId": 535186,
          "filePath": "/permanent/71191/toast-mt-2023-09-18/1430/535186",
          "fileName": "attachment.jpg",
          "saveFileName": "20230918eA8JmR0.jpg",
          "uploadType": "TEMPORARY"
        }
      ]
    }
  }
}
```

| 値                                         | 	タイプ          | 	説明                                                                                                |
|-------------------------------------------|---------------|----------------------------------------------------------------------------------------------------|
| header.isSuccessful                       | 	Boolean      | 	成否                                                                                                |
| header.resultCode                         | 	Integer      | 	失敗コード                                                                                             |
| header.resultMessage                      | 	String       | 	失敗メッセージ                                                                                           |
| body.pageNum                              | 	Integer      | 	現在のページ番号                                                                                          |
| body.pageSize                             | 	Integer      | 	検索されたデータ数                                                                                         |
| body.totalCount                           | 	Integer      | 	データの総数                                                                                            |
| body.data.requestId                       | 	String       | 	リクエストID                                                                                           |
| body.data.recipientSeq                    | 	Integer      | 	受信者シーケンス                                                                                          |
| body.data.requestDate                     | 	String       | 	発信日時                                                                                              |
| body.data.sendNo                          | 	String       | 	発信番号                                                                                              |
| body.data.recipientNo                     | 	String       | 	受信番号                                                                                              |
| body.data.countryCode                     | 	String       | 	国番号                                                                                               |
| body.data.sendType                        | 	String       | 	送信タイプ(0:Sms, 1:Lms/Mms, 2:Auth)                                                                   |
| body.data.messageType                     | 	String       | 	メッセージタイプ<br/>(SMS、LMS、MMS、AUTH)                                                                   |
| body.data.adYn                            | 	String       | 	広告かどうか                                                                                            |
| body.data.templateId                      | 	String       | 	テンプレートID                                                                                          |
| body.data.templateParameter               | 	String(json) | 	テンプレートパラメータ                                                                                       |
| body.data.templateName                    | 	String       | 	テンプレート名                                                                                           |
| body.data.title                           | 	String       | 	タイトル                                                                                              |
| body.data.body                            | 	String       | 	本文内容                                                                                              |
| body.data.messageStatus                   | 	String       | 	メッセージ状態<br/>(RESERVED：予約待機、SENDING：送信中、COMPLETED：送信完了、FAILED：送信失敗、CANCEL：キャンセル、DUPLICATED：重複送信) |
| body.data.createUser                      | 	String       | 	登録したユーザー                                                                                          |
| body.data.createDate                      | 	String       | 	登録日                                                                                               |
| body.data[].attachFileList[].fileId       | 	Integer      | 	ファイルID                                                                                            |
| body.data[].attachFileList[].filePath     | 	String       | 	ファイル保存パス(内部用)                                                                                     |
| body.data[].attachFileList[].filename     | 	String       | 	ファイル名                                                                                             |
| body.data[].attachFileList[].saveFileName | 	String       | 	保存された添付ファイル名                                                                                      |
| body.data[].attachFileList[].uploadType   | 	String       | 	アップロードタイプ                                                                                         |

### 予約送信キャンセル

#### リクエスト

[URL]

```
PUT /sms/v3.0/appKeys/{appKey}/reservations/cancel
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

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

| 値                              | 	タイプ    | 	最大長さ | 必須 | 	説明         |
|--------------------------------|---------|-------|----|-------------|
| reservationList[].requestId    | String  | 25    | O  | リクエストID     |
| reservationList[].recipientSeq | Integer | -     | O  | 受信者シーケンス    |
| updateUser                     | String  | 100   | O  | キャンセルリクエスト者 |

#### cURL

```
curl -X PUT \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/reservations/cancel' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}'  \
-d '{
    "reservationList": [{
            "requestId": "1",
            "recipientSeq": 1
        }
    ],
    "updateUser": "API Guide"
}'
```

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

### 予約送信キャンセル - 多重フィルタ

#### リクエスト

* 予約キャンセルリクエストは状態が「予約中(RESERVED)」の場合にのみ可能です。
* 送信済みのメッセージはキャンセルできません。

[URL]

```
PUT /sms/v3.0/appKeys/{appKey}/reservations/search-cancels
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Request body]

```json
{
  "searchParameter": {
    "sendType": "0",
    "startRequestDate": "2020-02-01 00:00",
    "endRequestDate": "2020-02-01 10:00",
    "startCreateDate": "2020-02-01 00:00",
    "endCreateDate": "2020-02-01 10:00",
    "sendNo": "15880000",
    "recipientNo": "0100000000",
    "templateId": "TemplateId",
    "requestId": "20200201010630ReZQ6KZzAH0",
    "createUser": "CreateUser",
    "senderGroupingKey": "SenderGroupingKey"
  },
  "updateUser": "UpdateUser"
}
```

* startRequestDate + endRequestDateまたはstartCreateDate + endCreateDateは必須です。
* 登録日 / 予約日を同時に検索する場合、予約日は無視されます。

| 値                                    | 	タイプ   | 	最大長さ | 必須    | 	説明                            |
|--------------------------------------|--------|-------|-------|--------------------------------|
| searchParameter.sendType             | String | 1     | 必須    | 送信タイプ(0：Sms、1：Lms/Mms, 2：Auth) |
| searchParameter.startRequestDate     | String | -     | 必須    | 予約日開始                          |
| searchParameter.endRequestDate       | String | -     | 必須    | 予約日終了                          |
| searchParameter.startCreateDate      | String | -     | 必須    | 登録日開始                          |
| searchParameter.endCreateDate        | String | -     | 必須    | 登録日終了                          |
| searchParameter.sendNo               | String | 20    | オプション | 発信番号                           |
| searchParameter.recipientNo          | String | 20    | オプション | 受信番号                           |
| searchParameter.templateId           | String | 50    | オプション | テンプレートID                       |
| searchParameter.requestId            | String | 25    | オプション | リクエストID                        |
| searchParameter.createUser           | String | 100   | オプション | 予約送信作成者                        |
| searchParameter.senderGroupingKey    | String | 100   | オプション | 発信者グループキー                      |
| searchParameter.recipientGroupingKey | String | 100   | オプション | 受信者グループキー                      |
| updateUser                           | String | 100   | 必須    | 予約キャンセルリクエスト者                  |

#### cURL

```
curl -X PUT \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/reservations/search-cancels' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}'  \
-d '{
    "searchParameter": {
        "sendType": "0",
        "startRequestDate": "2020-02-01 00:00",
        "endRequestDate": "2020-02-01 10:00",
        "startCreateDate": "2020-02-01 00:00",
        "endCreateDate": "2020-02-01 10:00",
        "sendNo": "15880000",
        "recipientNo": "0100000000",
        "templateId": "TemplateId",
        "requestId": "20200201010630ReZQ6KZzAH0",
        "createUser": "CreateUser",
        "senderGroupingKey": "SenderGroupingKey"
    },
    "updateUser": "API Guide"
}'
```

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
      "reservationCancelId": "20200210113330OepQ1sAzSDa",
      "requestedDateTime": "2020-02-10 11:33:30",
      "reservationCancelStatus": "READY"
    }
  }
}
```

| 値                                 | 	タイプ     | 	説明                                                                                                        |
|-----------------------------------|----------|------------------------------------------------------------------------------------------------------------|
| header.isSuccessful               | 	Boolean | 	成否                                                                                                        |
| header.resultCode                 | 	Integer | 	失敗コード                                                                                                     |
| header.resultMessage              | 	String  | 	失敗メッセージ                                                                                                   |
| body.data.reservationCancelId     | 	Integer | 	予約キャンセルID                                                                                                 |
| body.data.requestedDateTime       | 	String  | 	予約キャンセル時間(yyyy-MM-dd HH:mm:ss)                                                                            |
| body.data.reservationCancelStatus | 	String  | 	予約キャンセル状態<br/>- READY ：予約準備<br/>- PROCESSING ：予約キャンセル中<br/>- COMPLETED ：予約キャンセル完了<br/>- FAILED ：予約キャンセル失敗 |

### 予約送信キャンセルリクエストリスト検索 - 多重フィルタ

#### リクエスト

[URL]

```
GET /sms/v3.0/appKeys/{appKey}/reservations/search-cancels?startRequestedDateTime={startRequestedDateTime}&endRequestedDateTime={endRequestedDateTime}&reservationCancelId={reservationCancelId}&pageNum={pageNum}&pageSize={pageSize}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Query parameter]

| 値                      | 	タイプ     | 	最大長さ | 必須     | 	説明                                    |
|------------------------|----------|-------|--------|----------------------------------------|
| startRequestedDateTime | String   | -     | オプション  | 予約キャンセルリクエスト開始時間(yyyy-MM-dd HH:mm:ss)  |
| endRequestedDateTime   | 	String  | -     | 	オプション | 	予約キャンセルリクエスト終了時間(yyyy-MM-dd HH:mm:ss) |
| reservationCancelId    | 	String  | 25    | 	オプション | 予約キャンセルID                              |
| pageNum                | 	Integer | -     | 	オプション | 	ページ番号(デフォルト値：1)                       |
| pageSize               | 	Integer | 1000  | 	オプション | 	検索数(デフォルト値：15)                        |

#### cURL

```
curl -X GET \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/reservations/search-cancels' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

#### レスポンス

```json
{
  "header": {
    "resultCode": 0,
    "resultMessage": "success",
    "isSuccessful": true
  },
  "body": {
    "data": [
      {
        "reservationCancelId": "",
        "searchParameter": {
        },
        "requestedDateTime": "",
        "completedDateTime": "",
        "reservationCancelStatus": "",
        "totalCount": 0,
        "successCount": 0,
        "createUser": "",
        "createdDateTime": "",
        "updatedDateTime": ""
      }
    ]
  }
}
```

| 値                                   | 	タイプ                 | 	説明                                                                                                        |
|-------------------------------------|----------------------|------------------------------------------------------------------------------------------------------------|
| header.isSuccessful                 | 	Boolean             | 	成否                                                                                                        |
| header.resultCode                   | 	Integer             | 	失敗コード                                                                                                     |
| header.resultMessage                | 	String              | 	失敗メッセージ                                                                                                   |
| body.data[].reservationCancelId     | 	String              | 	予約キャンセルID                                                                                                 |
| body.data[].searchParameter         | 	Map<String, Object> | 予約キャンセルリクエストパラメータ                                                                                          |
| body.data[].requestedDateTime       | 	String              | 	予約キャンセルリクエスト時間                                                                                            |
| body.data[].completedDateTime       | 	String              | 	予約キャンセル完了時間                                                                                               |
| body.data[].reservationCancelStatus | 	String              | 	予約キャンセル状態<br/>- READY ：予約準備<br/>- PROCESSING ：予約キャンセル中<br/>- COMPLETED ：予約キャンセル完了<br/>- FAILED ：予約キャンセル失敗 |
| body.data[].totalCount              | 	Integer             | 予約キャンセル対象件数                                                                                                |
| body.data[].successCount            | 	Integer             | 予約キャンセル成功件数                                                                                                |
| body.data[].createUser              | 	String              | 予約キャンセルリクエスト者	                                                                                             |
| body.data[].createdDateTime         | 	String              | 	予約キャンセルリクエスト作成時間                                                                                          |
| body.data[].updatedDateTime         | 	String              | 	予約キャンセル修正時間                                                                                               |

## 送信結果ファイルダウンロード

### 検索ファイル作成リクエスト

#### リクエスト

[URL]

```
POST /sms/v3.0/appKeys/{appKey}/sender/download-reservations
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Request body]

* requestIdまたはstartRequestDate + endRequestDateまたはstartCreateDate + endCreateDateは必須です。
* 登録日/送信日を同時に検索する場合、送信日は無視されます。

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
  "senderGroupingKey": "senderGroupingKey",
  "recipientGroupingKey": "recipientGroupingKey",
  "isIncludeTitleAndBody": true
}
```

| 値                     | 	タイプ    | 	最大長さ | 必須     | 	説明                                                          |
|-----------------------|---------|-------|--------|--------------------------------------------------------------|
| sendType              | String  | 1     | 必須     | 送信タイプ(0：Sms、1：LMS/MMS、2：Auth)                                |
| requestId             | 	String | 25    | 	必須    | 	リクエストID                                                     |
| startRequestDate      | 	String | -     | 	必須    | 	送信日開始値(yyyy-MM-dd HH:mm:ss)                                 |
| endRequestDate        | 	String | -     | 	必須    | 	送信日終了値(yyyy-MM-dd HH:mm:ss)                                 |
| startCreateDate       | 	String | -     | 	必須    | 	登録日開始値(yyyy-MM-dd HH:mm:ss)                                 |
| endCreateDate         | 	String | -     | 	必須    | 	登録日終了値(yyyy-MM-dd HH:mm:ss)                                 |
| startResultDate       | 	String | -     | 	オプション | 	受信日開始値(yyyy-MM-dd HH:mm:ss)                                 |
| endResultDate         | 	String | -     | 	オプション | 	受信日終了値(yyyy-MM-dd HH:mm:ss)                                 |
| sendNo                | 	String | 13    | 	オプション | 	発信番号                                                        |
| recipientNo           | 	String | 20    | 	オプション | 	受信番号                                                        |
| templateId            | 	String | 50    | 	オプション | 	テンプレート番号                                                    |
| msgStatus             | 	String | 1     | 	オプション | メッセージステータスコード(0：失敗、 1：リクエスト、 2：処理中、 3：成功、 4：予約キャンセル、 5：重複失敗) |
| resultCode            | 	String | 10    | 	オプション | 	受信結果コード[[検索コード表](./error-code/#_2)]                         |
| subResultCode         | 	String | 10    | 	オプション | 	受信結果詳細コード[[検索コード表](./error-code/#_3)]                       |
| senderGroupingKey     | 	String | 100   | 	オプション | 	送信者グループキー                                                   |
| recipientGroupingKey  | 	String | 100   | 	オプション | 	受信者グループキー                                                   |
| isIncludeTitleAndBody | Boolean | -     | オプション  | タイトル、本文を含むかどうか                                               |

#### cURL

```
curl -X POST \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/sender/download-reservations' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}'  \
-d '{
    "sendType": "1",
    "startRequestDate": "2020-08-01T00:00:00",
    "endRequestDate": "2020-08-08T00:00:00"
}'
```

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

### 送信結果ファイル作成リクエスト内訳検索

#### リクエスト

[URL]

```
GET /sms/v3.0/appKeys/{appKey}/download-reservations
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Query parameter]

| 値                  | 	タイプ     | 最大長さ  | 	必須   | 	説明                 |
|--------------------|----------|-------|-------|---------------------|
| downloadId         | 	String  | 	25   | オプション | ダウンロードID            |
| downloadStatusCode | 	String  | 10    | オプション | 	ダウンロードファイルステータスコード |
| pageNum            | 	Integer | 	-    | オプション | ページ番号(デフォルト値：1)     |
| pageSize           | 	Integer | 	1000 | オプション | 検索数(デフォルト値：15)      |

#### cURL

```
curl -X GET \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/download-reservations' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

#### レスポンス

```json
{
  "header": {
    "resultCode": 0,
    "resultMessage": "success",
    "isSuccessful": true
  },
  "body": {
    "totalCount": 11,
    "data": [
      {
        "downloadId": "20230918142321iut0Dvz7bb0",
        "downloadType": "NORMAL",
        "fileType": "CSV",
        "parameter": "{\"pageNum\":1,\"pageSize\":15,\"offset\":0,\"rowCount\":15,\"serviceId\":71191,\"requestId\":\"202309181423208yLgttheqX0\",\"sendNo\":\"12341234\",\"recipientNo\":\"01012341234\",\"startRequestDate\":\"2023-09-18 00:00:20\",\"endRequestDate\":\"2023-09-18 23:59:20\",\"startResultDate\":\"2023-09-18 00:00:20\",\"endResultDate\":\"2023-09-18 23:59:20\",\"startCreateDate\":\"2023-09-18 00:00:20\",\"endCreateDate\":\"2023-09-18 14:23:21\",\"msgStatus\":\"3\",\"msgStatusName\":\"COMPLETED\",\"resultCode\":\"MTR2\",\"subResultCode\":\"MTR2_3\",\"resultCodeList\":[\"2004\",\"2003\",\"2006\",\"2005\",\"2000\",\"2002\",\"2001\"],\"sendType\":\"1\",\"senderGroupingKey\":\"senderGroupingKey\",\"recipientGroupingKey\":\"recipientGroupingKey\",\"countryCodeSet\":[],\"isIncludeTitleAndBody\":true,\"searchedByCreateDate\":true,\"targetMonths\":[\"sep\"],\"lastResultCode\":\"MTR2_3\"}",
        "size": 0,
        "downloadStatusCode": "COMPLETED",
        "resultMessage": null,
        "expiredDate": "2023-09-25 14:23:22.0",
        "createUser": "test@nhn.com",
        "createDate": "2023-09-18 14:23:21.0",
        "updateDate": "2023-09-18 14:23:22.0"
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
| body.totalCount                | Integer  | 全体件数                                                                                                       |
| body.data[].downloadId         | String   | ダウンロードID                                                                                                   |
| body.data[].downloadType       | String   | ダウンロードタイプ<br/>- BLOCK：受信拒否<br/>- NORMAL：一般送信<br/>- MASS：大量送信<br/>- TAG：タグ送信                                |
| body.data[].fileType           | String   | ファイルタイプ                                                                                                    |
| body.data[].parameter          | String   | リクエストパラメータ                                                                                                 |
| body.data[].size               | Integer  | 検索データサイズ                                                                                                   |
| body.data[].downloadStatusCode | String   | ファイル作成状態<br/>- READY：作成準備<br/>- MAKING：作成中<br/>- COMPLETED：作成完了<br/>- FAILED：作成失敗<br/>- EXPIRED：ダウンロード期間終了 |
| body.data[].resultMessage      | String   | 結果メッセージ(失敗時レスポンス)                                                                                          |
| body.data[].expiredDate        | String   | ファイル終了日時                                                                                                   |
| body.data[].createUser         | String   | ファイル作成リクエスト者                                                                                               |
| body.data[].createDate         | String   | ファイル作成リクエスト日時                                                                                              |
| body.data[].updateDate         | String   | ファイル作成完了、失敗日時                                                                                              |

### 送信結果ファイルダウンロードリクエスト

#### リクエスト

[URL]

```
GET /sms/v3.0/appKeys/{appKey}/download-reservations/{downloadId}/download
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値          | 	タイプ    | 	説明            |
|------------|---------|----------------|
| appKey     | 	String | 	固有のアプリケーションキー |
| downloadId | String  | ダウンロードID       |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

#### cURL

```
curl -X GET \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/download-reservations/'"${DOWNLOAD_RESERVATION_ID}"'/download' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

#### レスポンス

```
file byte
```

## タグ管理

### タグ検索

#### リクエスト

[URL]

```
GET /sms/v3.0/appKeys/{appKey}/tags?pageNum={pageNum}&pageSize={pageSize}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Query parameter]

| 値        | 	タイプ     | 最大長さ  | 	必須   | 	説明             |
|----------|----------|-------|-------|-----------------|
| pageNum  | 	Integer | 	-    | オプション | ページ番号(デフォルト値：1) |
| pageSize | 	Integer | 	1000 | オプション | 検索数(デフォルト値：15)  |

#### cURL

```
curl -X GET \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/tags' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
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

| 値                       | 	タイプ     | 	説明        |
|-------------------------|----------|------------|
| header.isSuccessful     | 	Boolean | 	成否        |
| header.resultCode       | 	Integer | 	失敗コード     |
| header.resultMessage    | 	String  | 	失敗メッセージ   |
| body.pageNum            | 	Integer | 	現在のページ番号  |
| body.pageSize           | 	Integer | 	検索されたデータ数 |
| body.totalCount         | 	Integer | 	データの総数    |
| body.data[].tagId       | String   | タグID       |
| body.data[].tagName     | String   | タグ名        |
| body.data[].createdDate | String   | 作成日時       |
| body.data[].tagId       | String   | 修正日時       |

### タグ登録

[URL]

```
POST /sms/v3.0/appKeys/{appKey}/tags
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Request body]

```json
{
  "tagName": "TAG"
}
```

| 値       | 	タイプ   | 最大長さ | 	必須 | 	説明 |
|---------|--------|------|-----|-----|
| tagName | String | 30   | 必須  | タグ名 |

#### cURL

```
curl -X POST \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/tags' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}'  \
-d '{
    "tagName": "API-Guide"
}'
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
PUT /sms/v3.0/appKeys/{appKey}/tags/{tagId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |
| tagId  | 	String | 	タグID          |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Request body]

```json
{
  "tagName": "TAG"
}
```

| 値       | 	タイプ   | 最大長さ | 	必須 | 	説明 |
|---------|--------|------|-----|-----|
| tagName | String | 30   | 必須  | タグ名 |

#### cURL

```
curl -X PUT \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/tags/'"${TAG_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}'  \
-d '{
    "tagName": "API-Guide2"
}'
```

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
DELETE /sms/v3.0/appKeys/{appKey}/tags/{tagId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |
| tagId  | 	String | 	タグID          |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

#### cURL

```
curl -X DELETE \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/tags/'"${TAG_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

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

## UID管理

### UID検索

#### リクエスト

[URL]

```
GET /sms/v3.0/appKeys/{appKey}/uids?wheres={wheres}&offsetUid={offsetUid}&offset={offset}&limit={limit}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Query parameter]

| 値         | 	タイプ          | 最大長さ | 	必須   | 	説明                                                                                 |
|-----------|---------------|------|-------|-------------------------------------------------------------------------------------|
| wheres    | 	List<String> | 	-   | オプション | 検索条件。<br/>英字、数字、括弧で校正された文字列配列。<br/>括弧は1個、 AND/ORは3個まで使用可能<br/>(例) tagId1、AND、tagId2 |
| offsetUid | 	String       | 	-   | オプション | 検索を始めるuid                                                                           |
| offset    | Integer       | -    | オプション | offset 0(デフォルト値)                                                                    |
| limit     | Integer       | 1000 | オプション | 検索件数15(デフォルト値)                                                                      |

#### cURL

```
curl -X GET \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/uids' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
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
| body.data.uids[].tags[].createdDate     | String   | タグ作成日時      |
| body.data.uids[].tags[].updatedDate     | String   | タグ修正日時      |
| body.data.uids[].contacts[].contactType | String   | 連絡先タイプ      |
| body.data.uids[].contacts[].contact     | String   | 連絡先(携帯電話番号) |
| body.data.uids[].contacts[].createdDate | String   | 連絡先作成日時     |
| body.data.uids[].last                   | Boolean  | 最後のリストかどうか  |

### UID単件検索

#### リクエスト

[URL]

```
GET /sms/v3.0/appKeys/{appKey}/uids/{uid}
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |
| uid    | 	String | 	UID           |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

#### cURL

```
curl -X GET \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/uids/'"${USER_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
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
| body.data.tags[].createdDate     | String   | タグ作成日時      |
| body.data.tags[].updatedDate     | String   | タグ修正日時      |
| body.data.contacts[].contactType | String   | 連絡先タイプ      |
| body.data.contacts[].contact     | String   | 連絡先(携帯電話番号) |
| body.data.contacts[].createdDate | String   | 連絡先作成日時     |

### UID登録

[URL]

```
POST /sms/v3.0/appKeys/{appKey}/uids
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

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
| contacts[].contact     | String | -    | 必須  | 連絡先(携帯電話番号)          |

[注意]

* tagIdsが与えられた場合、contactsは必須値ではありません。
* contactsが与えられた場合、tagIdsは必須値ではありません。
* この商品の場合、 contactTypeは必ず"PHONE_NUMBER"値でリクエストする必要があります。

#### cURL

```
curl -X POST \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/uids/' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}'  \
-d '{
    "uids": [
        {
            "uid": "USER ID",
            "contacts": [
                {
                    "contactType": "PHONE_NUMBER",
                    "contact": "0100000000"
                }
            ]
        }
    ]
}'
```

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

### UID削除

[URL]

```
DELETE /sms/v3.0/appKeys/{appKey}/uids/{uid}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |
| uid    | 	String | 	UID           |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

#### cURL

```
curl -X DELETE \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/uids/'"${USER_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

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
POST /sms/v3.0/appKeys/{appKey}/uids/{uid}/phone-numbers
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値      | 	タイプ    | 	説明            |
|--------|---------|----------------|
| appKey | 	String | 	固有のアプリケーションキー |
| uid    | String  | UID            |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Request body]

```json
{
  "phoneNumber": "0100000000"
}
```

| 値           | 	タイプ   | 最大長さ | 	必須 | 	説明    |
|-------------|--------|------|-----|--------|
| phoneNumber | String | -    | 必須  | 携帯電話番号 |

#### cURL

```
curl -X POST \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/uids/'"${USER_ID}/phone-numbers" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}'  \
-d '{
    "phoneNumber": "0100000000"
}'
```

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

### 携帯電話番号削除

[URL]

```
DELETE /sms/v3.0/appKeys/{appKey}/uids/{uid}/phone-numbers/{phoneNumber}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値           | 	タイプ    | 	説明            |
|-------------|---------|----------------|
| appKey      | 	String | 	固有のアプリケーションキー |
| uid         | String  | UID            |
| phoneNumber | String  | 携帯電話番号         |

[Header]

```json
{
"X-Secret-Key": "{secret-key}"
}
```

| 値            | 	タイプ    | 	説明          |
|--------------|---------|--------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

#### cURL

```
curl -X DELETE \
https://api-sms.cloud.toast.com/sms/v3.0/appKeys/'"${APP_KEY}"'/uids/'"${USER_ID}"'/phone-numbers/'"${P_NO}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

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

## Webフック

SMSサービス内で特定イベントが発生すると、Webフック設定に定義されているURLにPOSTリクエストを作成します。<br>
作成されたPOSTリクエストについてのAPI文書です。

### Webフック送信

[URL]

| Http method | 	URI               |
|-------------|--------------------|
| POST        | Webフック設定に定義した対象URL |

[Header]

| 値                         | 	タイプ    | 	説明               |
|---------------------------|---------|-------------------|
| X-Toast-Webhook-Signature | 	String | 	Webフック設定時に入力した署名 |

[Request body]

```json
{
  "hooksId": "202007271010101010sadasdavas",
  "webhookConfigId": "String",
  "productName": "SMS",
  "appKey": "akb3dukdmdjsdSvgk",
  "event": "UNSUBSCRIBE",
  "hooks": [
    {
      "hookId": "202007271010101010sadasdavas",
      "recipientNo": "01012341234",
      "unsubscribeNo": "08012341234",
      "enterpriseName": "NHN Cloud",
      "createdDateTime": "2020-09-09T11:25:10.000+09:00"
    }
  ]
}
```

| 値                       | 	タイプ    | 	説明                                            |
|-------------------------|---------|------------------------------------------------|
| hooksId                 | 	String | 	Webフック設定に定義されているURLにPOSTリクエストを行うたびに作成される固有のID |
| webhookConfigId         | 	String | Webフック設定ID                                     |
| productName             | 	String | 	Webフックイベントが発生したサービス名                          |
| appKey                  | 	String | Webフックイベントが発生したサービスアプリケーションキー                  |
| event                   | 	String | 	Webフックイベント名<br>* UNSUBSCRIBE：広告メッセージ受信番号登録    |
| hooks[].hookId          | 	String | サービスでイベント発生時に作成される固有ID                         |
| hooks[].recipientNo     | 	String | 	受信拒否された携帯電話番号                                 |
| hooks[].unsubscribeNo   | 	String | 	受信拒否サービスに登録された080番号                           |
| hooks[].enterpriseName  | 	String | 	受信拒否サービスに登録された業者名                             |
| hooks[].createdDateTime | 	String | 受信拒否リクエスト日時<br>* yyyy-MM-dd'T'HH:mm:ss.SSSXXX  |

#### cURL

```
curl -X POST \
    '{TargetUrl}' \
    -H 'Content-Type: application/json;charset=UTF-8' \
    -H 'X-Toast-Webhook-Signature: application/json;charset=UTF-8' \
    -d '{
        "hooksId": "202007271010101010sadasdavas",
        "webhookConfigId": "String",
        "productName": "Sms",
        "appKey": "akb3dukdmdjsdSvgk",
        "event": "UNSUBSCRIBE",
        "hooks": [
            {
                "hookId": "202007271010101010sadasdavas",
                "recipientNo": "01012341234",
                "unsubscribeNo": "08012341234",
                "enterpriseName": "NHN Cloud",
                "createdDateTime": "2020-09-09T11:25:10.000+09:00"
            }
        ]
    }

```
