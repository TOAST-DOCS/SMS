<!-- pre-align:aligned sig=2ab3edfe65d0 -->

## Notification > SMS > API v3.0 Guide

<a id="v30-api-overview"></a>

## v3.0 API紹介

<a id="changes-from-v24"></a>

### v2.4と異なる事項

1. シークレットキーを導入しました。

* v3.0 API呼び出し時にはヘッダにシークレットキーを追加すると成功します。

2. 大量送信リクエストを照会することができるAPIが追加されました。

* 大量送信リスト検索API、大量送信受信者リスト検索API、大量送信受信者リスト詳細検索APIが追加されました。

<a id="api-domain"></a>

### [APIドメイン]

| 環境   | 	ドメイン                            |
|------|----------------------------------|
| Real | 	https://sms.api.nhncloudservice.com |

<span id="precautions"></span>

<a id="caution"></a>

### [注意事項]

* サポートする文字の長さは次のとおりです。
* 最大サポート文字数は保存基準であり、文字切れを防ぐために標準規格で作成してください。
* エンコードはEUC-KRで送信され、サポートしない絵文字は送信に失敗します。

| 分類      | 最大サポート  | 標準規格                            |
|---------|---------|---------------------------------|
| SMS本文   | 255文字   | 90バイト(ハングル45文字、英字90文字)          |
| MMSタイトル | 120文字   | 40バイト(ハングル20文字、英字40文字)          |
| MMS本文   | 4,000文字 | 2,000バイト(ハングル1,000文字、英字2,000文字) |

<a id="short-sms"></a>

## SMS

<a id="send-short-sms"></a>

### SMS送信

<a id="request"></a>

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
  "originCode": "123456789",
  "useConversion": true
}
```

| 値                                         | 	タイプ    | 最大長さ                                                           | 	必須 | 	説明                                                                  |
|-------------------------------------------|---------|----------------------------------------------------------------|-----|----------------------------------------------------------------------|
| templateId                                | 	String | 50                                                             | 	X  | 	送信テンプレートID                                                          |
| body                                      | 	String | 標準：90バイト、最大：255文字(EUC-KR基準) [[注意事項](./api-guide/#precautions)] | 	O  | 	本文内容                                                                |
| sendNo                                    | 	String | 13                                                             | 	O  | 	発信番号                                                                |
| requestDate                               | String  | -                                                                 | X   | 予約日時(yyyy-MM-dd HH:mm)<br/>現在から最大60日後まで設定可                                                                                      |
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
| useConversion                             | Boolean | -                                                                 | X   | コンバージョン率収集リクエスト(デフォルト値: false)<br/>予約日時が設定された場合は使用不可                                                                                           |

<a id="curl"></a>

#### cURL

```
curl -X POST \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/sender/sms' \
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

<a id="response"></a>

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

| 値                                               | タイプ    | Not Null | 説明                                            |
|-------------------------------------------------|---------|----------|------------------------------------------------|
| header                                          | Object  | O        | ヘッダ領域                                        |
| header.isSuccessful                             | Boolean | O        | 成否                                            |
| header.resultCode                               | Integer | O        | 失敗コード                                         |
| header.resultMessage                            | String  | O        | 失敗メッセージ                                       |
| body.data.requestId                             | String  | O        | リクエストID                                       |
| body.data.statusCode                            | String  | O        | リクエストステータスコード(1：リクエスト中、 2：リクエスト完了、 3：リクエスト失敗) |
| body.data.senderGroupingKey                     | String  | X        | 発信者グループキー                                     |
| body.data.sendResultList[].recipientNo          | String  | O        | 受信番号                                           |
| body.data.sendResultList[].resultCode           | Integer | X        | 結果コード                                          |
| body.data.sendResultList[].resultMessage        | String  | X        | 結果メッセージ                                        |
| body.data.sendResultList[].recipientSeq         | Integer | O        | 受信者シーケンス(mtPr)                                 |
| body.data.sendResultList[].recipientGroupingKey | String  | X        | 受信者グループキー                                      |

<a id="example-of-sending-short-sms-general-domestic-recipient-numbers"></a>

#### SMS送信例(一般国内受信番号)

| Http method | URL                                                                  |
|-------------|----------------------------------------------------------------------|
| POST        | https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/{appKey}/sender/sms |

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

<a id="example-of-sending-short-sms-with-country-code-included-to-recipient-numbers"></a>

#### SMS送信例(国コードが含まれた受信番号)

| Http method | URL                                                                  |
|-------------|----------------------------------------------------------------------|
| POST        | https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/{appKey}/sender/sms |

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

<a id="list-delivery-of-short-sms"></a>

### SMS送信リスト検索

<a id="request-2"></a>

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
| msgStatus            | 	String  | 1     | 	オプション | メッセージステータスコード(0：失敗、 1：リクエスト、 2：処理中、 3：成功、 4：予約キャンセル、 5：重複失敗、 6:失敗(広告制限)、 再送信待機(広告制限)) |
| resultCode           | 	String  | 10    | 	オプション | 	受信結果コード[[検索コード表](./error-code/#_2)]                         |
| subResultCode        | 	String  | 10    | 	オプション | 	受信結果詳細コード[[検索コード表](./error-code/#_3)]                       |
| senderGroupingKey    | 	String  | 100   | 	オプション | 	送信者グループキー                                                   |
| recipientGroupingKey | 	String  | 100   | 	オプション | 	受信者グループキー                                                   |
| receiverRegion       | 	String  | -     | 	オプション | 	国内/国際 (DOMESTIC: 国内, INTERNATIONAL: 国際) |
| countryCode          | 	String  | -     | 	オプション | 	国コード [[送信可能国](./international-sending-policy/#_5)] |
| pageNum              | 	Integer | -     | 	オプション | 	ページ番号(デフォルト値：1)                                             |
| pageSize             | 	Integer | 1000  | 	オプション | 	検索数(デフォルト値：15)                                              |

<a id="curl-2"></a>

#### cURL

```
curl -X GET \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/sender/sms?startRequestDate='"${START_DATE}"'&endRequestDate='"${END_DATE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-2"></a>

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

| 値                                | タイプ    | Not Null | 説明                                         |
|----------------------------------|---------|----------|---------------------------------------------|
| header                           | Object  | O        | ヘッダ領域                                     |
| header.isSuccessful              | Boolean | O        | 成否                                         |
| header.resultCode                | Integer | O        | 失敗コード                                      |
| header.resultMessage             | String  | O        | 失敗メッセージ                                    |
| body                             | Object  | X        | 本文領域                                        |
| body.pageNum                     | Integer | O        | 現在のページ番号                                   |
| body.pageSize                    | Integer | O        | 検索されたデータ数                                  |
| body.totalCount                  | Integer | O        | データの総数                                     |
| body.data[].requestId            | String  | O        | リクエストID                                    |
| body.data[].requestDate          | String  | O        | 発信日時                                       |
| body.data[].resultDate           | String  | X        | 受信日時                                       |
| body.data[].templateId           | String  | X        | テンプレートID                                   |
| body.data[].templateName         | String  | X        | テンプレート名                                    |
| body.data[].categoryId           | Integer | X        | カテゴリーID                                    |
| body.data[].categoryName         | String  | X        | カテゴリー名                                     |
| body.data[].body                 | String  | O        | 本文内容                                       |
| body.data[].sendNo               | String  | O        | 発信番号                                       |
| body.data[].countryCode          | String  | O        | 国番号                                        |
| body.data[].recipientNo          | String  | O        | 受信番号                                       |
| body.data[].msgStatus            | String  | O        | メッセージステータスコード                              |
| body.data[].msgStatusName        | String  | O        | メッセージステータスコード名                             |
| body.data[].resultCode           | String  | X        | 受信結果コード[[受信結果コード表](./error-code/#emma-v3)] |
| body.data[].resultCodeName       | String  | X        | 受信結果コード名                                   |
| body.data[].telecomCode          | Integer | X        | サービスプロバイダーコード                              |
| body.data[].telecomCodeName      | String  | X        | サービスプロバイダー名                                |
| body.data[].recipientSeq         | Integer | O        | 送信詳細ID(詳細検索時は必須)(旧mtPr)                    |
| body.data[].sendType             | String  | O        | 送信タイプ(0:Sms, 1:Lms/Mms, 2:Auth)            |
| body.data[].messageType          | String  | O        | メッセージタイプ(SMS/LMS/MMS/AUTH)                 |
| body.data[].messageCount         | Integer | O        | 送信されたメッセージ件数                                |
| body.data[].userId               | String  | X        | 送信リクエストID                                  |
| body.data[].adYn                 | String  | O        | 広告かどうか                                     |
| body.data[].resultMessage        | String  | X        | 結果メッセージ                                    |
| body.data[].senderGroupingKey    | String  | X        | 発信者グループキー                                  |
| body.data[].recipientGroupingKey | String  | X        | 受信者グループキー                                  |

<a id="query-delivery-of-short-sms"></a>

### SMS送信単一検索

<a id="request-3"></a>

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

<a id="curl-3"></a>

#### cURL

```
curl -X GET \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/sender/sms/'"${REQUEST_ID}"'?recipientSeq='"${RECIPIENT_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-3"></a>

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
      "recipientGroupingKey": "RecipientGroupingKey",
      "dlr": {
        "dlrStatus": "DELIVERED",
        "networkCode": "12345",
        "errorCode": "0"
      }
    }
  }
}
```

| 値                              | タイプ    | Not Null | 説明                                                           |
|--------------------------------|---------|----------|----------------------------------------------------------------|
| header                         | Object  | O        | ヘッダ領域                                                       |
| header.isSuccessful            | Boolean | O        | 成否                                                           |
| header.resultCode              | Integer | O        | 失敗コード                                                      |
| header.resultMessage           | String  | O        | 失敗メッセージ                                                  |
| body                           | Object  | X        | 本文領域                                                       |
| body.data.requestId            | String  | O        | リクエストID                                                   |
| body.data.requestDate          | String  | O        | 発信日時                                                       |
| body.data.resultDate           | String  | X        | 受信日時                                                       |
| body.data.templateId           | String  | X        | テンプレートID                                                 |
| body.data.templateName         | String  | X        | テンプレート名                                                 |
| body.data.categoryId           | Integer | X        | カテゴリーID                                                   |
| body.data.categoryName         | String  | X        | カテゴリー名                                                   |
| body.data.body                 | String  | O        | 本文内容                                                       |
| body.data.sendNo               | String  | O        | 発信番号                                                       |
| body.data.countryCode          | String  | O        | 国番号                                                         |
| body.data.recipientNo          | String  | O        | 受信番号                                                       |
| body.data.msgStatus            | String  | O        | メッセージステータスコード                                     |
| body.data.msgStatusName        | String  | O        | メッセージステータスコード名                                   |
| body.data.resultCode           | String  | X        | 受信結果コード[[受信結果コード表](./error-code/#emma-v3)]       |
| body.data.resultCodeName       | String  | X        | 受信結果コード名                                               |
| body.data.telecomCode          | Integer | X        | サービスプロバイダーコード                                     |
| body.data.telecomCodeName      | String  | X        | サービスプロバイダー名                                         |
| body.data.recipientSeq         | Integer | O        | 送信詳細ID(詳細検索時は必須)(旧mtPr)                           |
| body.data.sendType             | String  | O        | 送信タイプ(0:Sms, 1:Lms/Mms, 2:Auth)                          |
| body.data.messageType          | String  | O        | メッセージタイプ(SMS/LMS/MMS/AUTH)                             |
| body.data.messageCount         | Integer | O        | 送信されたメッセージの件数                                     |
| body.data.userId               | String  | X        | 送信リクエストID                                               |
| body.data.adYn                 | String  | O        | 広告かどうか                                                   |
| body.data.originCode           | String  | X        | 識別コード(特殊なタイプの付加通信事業者登録証に記載されている記号、文字、空白を除外した登録番号9桁の数字) |
| body.data.resultMessage        | String  | X        | 結果メッセージ                                                 |
| body.data.senderGroupingKey    | String  | X        | 発信者グループキー                                             |
| body.data.recipientGroupingKey | String  | X        | 受信者グループキー                                             |
| body.data.dlr.dlrStatus        | String  | X        | DLRステータスコード                                            |
| body.data.dlr.networkCode      | String  | X        | DLRネットワークコード                                          |
| body.data.dlr.errorCode        | String  | X        | DLRエラーコード                                                |

<a id="convert-internation-delivery-of-short-sms"></a>

### 短文SMS国際送信コンバージョン

* コンバージョンAPIは、短文SMS国際送信時にコンバージョン率収集をリクエストした送信件に対して、正常にコンバージョンされたことをレスポンスするAPIです。
* このAPIを使用すると、正常に送信されたメッセージのコンバージョン率を管理できます。
* 送信時にuseConversionフィールドでコンバージョン率の収集をリクエストしていない場合、または送信が完了していない場合、このAPIは失敗としてレスポンスします。

<a id="request-4"></a>

#### リクエスト

[URL]

```
POST  /sms/v3.0/appKeys/{appKey}/sender/sms/do-convert
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値    | タイプ   | 説明   |
|--------|--------|--------|
| appKey | String | 固有のアプリキー |

[Header]

```json
{
  "X-Secret-Key": "{secret-key}"
}
```

| 値          | タイプ   | 説明      |
|--------------|--------|-----------|
| X-Secret-Key | String | 固有のシークレットキー |

[Request body]

```json
{
  "requestId": "requestId",
  "recipientSeq": 1
}
```

| 値          | タイプ    | 最大長さ | 必須 | 説明    |
|--------------|---------|-------|----|---------|
| requestId    | String  | 25    | O  | リクエストID   |
| recipientSeq | Integer | -     | O  | 受信者シーケンス |

<a id="curl-4"></a>

#### cURL

```
curl -X GET \
'https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/sender/sms/do-convert \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}' \
-d '{
    "requestId": "requestId",
    "recipientSeq": 1
}'
```

<a id="response-4"></a>

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  }
}
```

| 値                  | タイプ    | Not Null | 説明         |
|---------------------|---------|----------|--------------|
| header              | Object  | O        | ヘッダ領域     |
| header.isSuccessful | Boolean | O        | 成否         |
| header.resultCode   | Integer | O        | 失敗コード    |
| header.resultMessage| String  | O        | 失敗メッセージ |

<a id="long-mms"></a>

## 長文MMS

<a id="send-long-mms-attachments-not-included"></a>

### 長文MMS送信(添付ファイル含まず)

※ LMS/MMSは海外送信ができません。しかし、国際SMS限定でSMSのConcatenated Message(接続)機能を使って長いメッセージを送信できます。 [[国際SMS送信ポリシー](./international-sending-policy/#_3)]

<a id="request-5"></a>

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
| requestDate                               | String  | -     | X   | 予約日時(yyyy-MM-dd HH:mm)<br/>現在から最大60日後まで設定可                                                                                                                 |
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

<a id="curl-5"></a>

#### cURL

```
curl -X POST \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/sender/mms' \
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

<a id="response-5"></a>

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

| 値                                               | タイプ    | Not Null | 説明                                             |
|-------------------------------------------------|---------|----------|--------------------------------------------------|
| header                                          | Object  | O        | ヘッダ領域                                         |
| header.isSuccessful                             | Boolean | O        | 成否                                             |
| header.resultCode                               | Integer | O        | 失敗コード                                          |
| header.resultMessage                            | String  | O        | 失敗メッセージ                                        |
| body                                            | Object  | X        | 本文領域                                           |
| body.data.requestId                             | String  | O        | リクエストID                                        |
| body.data.statusCode                            | String  | O        | リクエストステータスコード(1：リクエスト中、 2：リクエスト完了、 3：リクエスト失敗) |
| body.data.senderGroupingKey                     | String  | X        | 発信者グループキー                                      |
| body.data.sendResultList[].recipientNo          | String  | O        | 受信番号                                            |
| body.data.sendResultList[].resultCode           | Integer | X        | 結果コード                                           |
| body.data.sendResultList[].resultMessage        | String  | X        | 結果メッセージ                                         |
| body.data.sendResultList[].recipientSeq         | Integer | O        | 受信者シーケンス(mtPr)                                  |
| body.data.sendResultList[].recipientGroupingKey | String  | X        | 受信者グループキー                                       |

<a id="example-of-sending-long-mms"></a>

#### 長文MMS送信例

| Http method | URL                                                                  |
|-------------|----------------------------------------------------------------------|
| POST        | https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/{appKey}/sender/mms |

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

<a id="send-mms-attached-file-included"></a>

### 長文MMS送信(添付ファイルを含む)

<a id="example-of-sending-attached-files"></a>

#### 添付ファイル送信例

| Http method | URL                                                                  |
|-------------|----------------------------------------------------------------------|
| POST        | https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/{appKey}/sender/mms |

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

<a id="list-delivery-of-long-mms-request"></a>

### 長文MMS送信リスト検索

<a id="request-6"></a>

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
| msgStatus            | 	String  | 1    | 	オプション | メッセージステータスコード(0：失敗、 1：リクエスト、 2：処理中、 3：成功、 4：予約キャンセル、 5：重複失敗、 6:失敗(広告制限)、 再送信待機(広告制限)) |
| resultCode           | 	String  | 10   | 	オプション | 	受信結果コード[[検索コード表](./error-code/#_2)]                         |
| subResultCode        | 	String  | 10   | 	オプション | 	受信結果詳細コード[[検索コード表](./error-code/#_3)]                       |
| senderGroupingKey    | 	String  | 100  | 	オプション | 	送信者グループキー                                                   |
| recipientGroupingKey | 	String  | 100  | 	オプション | 	受信者グループキー                                                   |
| receiverRegion       | 	String  | -     | 	オプション | 	国内/国際 (DOMESTIC: 国内, INTERNATIONAL: 国際) |
| countryCode          | 	String  | -     | 	オプション | 	国コード [[送信可能国](./international-sending-policy/#_5)] |
| pageNum              | 	Integer | -    | 	オプション | 	ページ番号(デフォルト値：1)                                             |
| pageSize             | 	Integer | 1000 | 	オプション | 	検索数(デフォルト値：15)                                              |

<a id="curl-6"></a>

#### cURL

```
curl -X GET \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/sender/mms?startRequestDate='"${START_DATE}"'&endRequestDate='"${END_DATE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-6"></a>

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

| 値                                         | タイプ    | Not Null | 説明                                         |
|-------------------------------------------|---------|----------|---------------------------------------------|
| header                                    | Object  | O        | ヘッダ領域                                     |
| header.isSuccessful                       | Boolean | O        | 成否                                         |
| header.resultCode                         | Integer | O        | 失敗コード                                      |
| header.resultMessage                      | String  | O        | 失敗メッセージ                                    |
| body                                      | Object  | X        | 本文領域                                        |
| body.pageNum                              | Integer | O        | 現在のページ番号                                   |
| body.pageSize                             | Integer | O        | 検索されたデータ数                                  |
| body.totalCount                           | Integer | O        | データの総数                                     |
| body.data[].requestId                     | String  | O        | リクエストID                                    |
| body.data[].requestDate                   | String  | O        | 発信日時                                       |
| body.data[].resultDate                    | String  | X        | 受信日時                                       |
| body.data[].templateId                    | String  | X        | テンプレートID                                   |
| body.data[].templateName                  | String  | X        | テンプレート名                                    |
| body.data[].categoryId                    | Integer | X        | カテゴリーID                                    |
| body.data[].categoryName                  | String  | X        | カテゴリー名                                     |
| body.data[].body                          | String  | O        | 本文内容                                       |
| body.data[].sendNo                        | String  | O        | 発信番号                                       |
| body.data[].countryCode                   | String  | O        | 国番号                                        |
| body.data[].recipientNo                   | String  | O        | 受信番号                                       |
| body.data[].msgStatus                     | String  | O        | メッセージステータスコード                              |
| body.data[].msgStatusName                 | String  | O        | メッセージステータスコード名                             |
| body.data[].resultCode                    | String  | X        | 受信結果コード[[受信結果コード表](./error-code/#emma-v3)] |
| body.data[].resultCodeName                | String  | X        | 受信結果コード名                                   |
| body.data[].telecomCode                   | Integer | X        | サービスプロバイダーコード                              |
| body.data[].telecomCodeName               | String  | X        | サービスプロバイダー名                                |
| body.data[].recipientSeq                  | Integer | O        | 送信詳細ID(詳細検索時は必須)(旧mtPr)                    |
| body.data[].sendType                      | String  | O        | 送信タイプ(0:Sms, 1:Lms/Mms, 2:Auth)            |
| body.data[].messageType                   | String  | O        | メッセージタイプ(SMS/LMS/MMS/AUTH)                 |
| body.data[].messageCount                  | Integer | O        | 送信されたメッセージ件数                               |
| body.data[].userId                        | String  | X        | 送信リクエストID                                  |
| body.data[].adYn                          | String  | O        | 広告かどうか                                     |
| body.data[].attachFileList[].fileId       | Integer | O        | ファイルID                                     |
| body.data[].attachFileList[].filePath     | String  | X        | ファイル保存パス(内部用)                              |
| body.data[].attachFileList[].filename     | String  | X        | ファイル名                                      |
| body.data[].attachFileList[].saveFileName | String  | X        | 保存された添付ファイル名                               |
| body.data[].attachFileList[].uploadType   | String  | X        | アップロードタイプ                                  |
| body.data[].senderGroupingKey             | String  | X        | 発信者グループキー                                  |
| body.data[].recipientGroupingKey          | String  | X        | 受信者グループキー                                  |
| body.data[].resultMessage                 | String  | X        | 結果メッセージ                                    |

<a id="query-single-delivery-of-long-mms"></a>

### 長文MMS送信単一検索

<a id="request-7"></a>

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

<a id="curl-7"></a>

#### cURL

```
curl -X GET \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/sender/mms/'"${REQUEST_ID}"'?recipientSeq='"${RECIPIENT_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-7"></a>

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

| 値                                       | タイプ     | Not Null | 説明                                                           |
|-----------------------------------------|----------|----------|----------------------------------------------------------------|
| header                                  | Object   | O        | ヘッダ領域                                                       |
| header.isSuccessful                     | Boolean  | O        | 成否                                                           |
| header.resultCode                       | Integer  | O        | 失敗コード                                                        |
| header.resultMessage                    | String   | O        | 失敗メッセージ                                                      |
| body                                    | Object   | X        | 本文領域                                                          |
| body.pageNum                            | Integer  | O        | 現在のページ番号                                                     |
| body.pageSize                           | Integer  | O        | 検索されたデータ数                                                    |
| body.totalCount                         | Integer  | O        | データの総数                                                       |
| body.data.requestId                     | String   | O        | リクエストID                                                      |
| body.data.requestDate                   | String   | O        | 発信日時                                                         |
| body.data.resultDate                    | String   | X        | 受信日時                                                         |
| body.data.templateId                    | String   | X        | テンプレートID                                                     |
| body.data.templateName                  | String   | X        | テンプレート名                                                      |
| body.data.categoryId                    | Integer  | X        | カテゴリーID                                                      |
| body.data.categoryName                  | String   | X        | カテゴリー名                                                       |
| body.data.body                          | String   | O        | 本文内容                                                         |
| body.data.sendNo                        | String   | O        | 発信番号                                                         |
| body.data.countryCode                   | String   | O        | 国家番号                                                         |
| body.data.recipientNo                   | String   | O        | 受信番号                                                         |
| body.data.msgStatus                     | String   | O        | メッセージステータスコード                                                |
| body.data.msgStatusName                 | String   | O        | メッセージステータスコード名                                               |
| body.data.resultCode                    | String   | X        | 受信結果コード[[受信結果コード表](./error-code/#emma-v3)]                   |
| body.data.resultCodeName                | String   | X        | 受信結果コード名                                                     |
| body.data.telecomCode                   | Integer  | X        | サービスプロバイダーコード                                                |
| body.data.telecomCodeName               | String   | X        | サービスプロバイダー名                                                  |
| body.data.recipientSeq                  | Integer  | O        | 送信詳細ID(詳細検索時に必須)                                             |
| body.data.sendType                      | String   | O        | 送信タイプ(0:Sms, 1:Lms/Mms, 2:Auth)                              |
| body.data.messageType                   | String   | O        | メッセージタイプ(SMS/LMS/MMS/AUTH)                                   |
| body.data.userId                        | String   | X        | 送信リクエストID                                                    |
| body.data.adYn                          | String   | O        | 広告かどうか                                                       |
| body.data.originCode                    | String   | X        | 識別コード(特殊なタイプの付加通信事業者登録証に記載されている記号、文字、空白を除外した登録番号9桁の数字) |
| body.data.attachFileList[].fileId       | Integer  | O        | ファイルID                                                       |
| body.data.attachFileList[].filePath     | String   | X        | ファイル保存パス(内部用)                                                |
| body.data.attachFileList[].filename     | String   | X        | ファイル名                                                        |
| body.data.attachFileList[].saveFileName | String   | X        | 保存された添付ファイル名                                                 |
| body.data.attachFileList[].uploadType   | String   | X        | アップロードタイプ                                                    |
| body.data.resultMessage                 | String   | X        | 結果メッセージ                                                      |
| body.data.senderGroupingKey             | String   | X        | 発信者グループキー                                                    |
| body.data.recipientGroupingKey          | String   | X        | 受信者グループキー                                                    |

<a id="sms-for-authentication-emergency"></a>

## 認証用SMS(緊急)

<a id="send-sms-for-authentication"></a>

### 認証用SMS送信

<span id="precautions-authword"></span>

1. 認証用SMS送信時に本文内に含まれている必要がある認証文言案内

| 区分         | 認証文言                                       |
|------------|--------------------------------------------| 
| 認証用SMS(緊急) | auth、password、verif、にんしょう、認証、パスワード、認証 |

- 例1-1)認証用SMS(緊急) API送信リクエスト時に全文(テンプレート日本語識別子含む)に認証文言が含まれていない場合は送信に失敗します。
- 例1-2)認証文言が英字の場合、大文字/小文字を区別せずに有効性チェックが行われます。

<a id="request-8"></a>

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
  "originCode": "123456789",
  "useConversion": true
}
```

| 値                                       | 	タイプ   | 最大長さ                                                           | 	必須 | 	説明                                                                                                                                    |
|-------------------------------------------|---------|-------------------------------------------------------------------|-----|------------------------------------------------------------------------------------------------------------------------------------------|
| templateId                                | 	String | 50                                                                | 	X  | 	送信テンプレートID                                                                                                                               |
| body                                      | 	String | 標準: 90バイト、最大: 255文字(EUC-KR基準) [[注意事項](./api-guide/#precautions)] | 	O  | 	本文内容[[注意事項](./api-guide/#precautions-authword)]                                                                                       |
| sendNo                                    | 	String | 13                                                                | 	O  | 	発信番号                                                                                                                                 |
| requestDate                               | String  | -                                                                 | X   | 予約日時(yyyy-MM-dd HH:mm)<br/>現在から最大60日後まで設定可能                                                                                    |
| senderGroupingKey                         | String  | 100                                                               | X   | 発信者グループキー                                                                                                                               |
| recipientList[].recipientNo               | 	String | 20                                                                | 	O  | 	受信番号<br/>countryCodeと組み合わせて使用可能                                                                                                     |
| recipientList[].countryCode               | 	String | 8                                                                 | 	X  | 	国家番号[デフォルト値: 82(韓国)]                                                                                                                     |
| recipientList[].internationalRecipientNo  | String  | 20                                                                | X   | 国番号を含む受信番号<br/>例)821012345678<br/>recipientNoがある場合、この値は無視<br/>                                                                  |
| recipientList[].templateParameter         | 	Object | -                                                                 | 	X  | 	テンプレートパラメータ(テンプレートID入力時)                                                                                                                   |
| recipientList[].templateParameter.{key}   | String  | -                                                                 | 	X  | 	置換キー(##key##)                                                                                                                           |
| recipientList[].templateParameter.{value} | Object  | -                                                                 | 	X  | 	置換キーにマッピングされるValue値                                                                                                                     |
| recipientList[].recipientGroupingKey      | String  | 100                                                               | X   | 受信者グループキー                                                                                                                               |
| userId                                    | 	String | 100                                                               | 	X  | 送信セパレータex)admin,system                                                                                                                   |
| statsId                                   | String  | 10                                                                | X   | 統計ID(発信検索条件には含まれません)                                                                                                              |
| originCode                                | String  | 9                                                              | X   | 識別コード(特殊なタイプの付加通信事業者登録証に記載されている記号、文字、空白を除外した登録番号9桁の数字)<br/>特殊なタイプの付加通信事業者ではない場合は使用しません。基本的にNHN Cloudの識別コードが挿入されます。<br/> |
| useConversion                             | Boolean | -                                                                 | X   | コンバージョン率収集リクエスト(デフォルト値: false)<br/>予約日時が設定されている場合は使用不可                                                                                        |

<a id="curl-8"></a>

#### cURL

```
curl -X POST \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/sender/auth/sms' \
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

<a id="response-8"></a>

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

| 値                                               | タイプ    | Not Null | 説明                                                     |
|-------------------------------------------------|---------|----------|----------------------------------------------------------|
| header                                          | Object  | O        | ヘッダ領域                                               |
| header.isSuccessful                             | Boolean | O        | 成否                                                     |
| header.resultCode                               | Integer | O        | 失敗コード                                               |
| header.resultMessage                            | String  | O        | 失敗メッセージ                                           |
| body                                            | Object  | X        | 本文領域                                                |
| body.data.requestId                             | String  | O        | リクエストID                                             |
| body.data.statusCode                            | String  | O        | リクエストステータスコード(1：リクエスト中、 2：リクエスト完了、 3：リクエスト失敗) |
| body.data.senderGroupingKey                     | String  | X        | 発信者グループキー                                       |
| body.data.sendResultList[].recipientNo          | String  | O        | 受信番号                                                 |
| body.data.sendResultList[].resultCode           | Integer | X        | 結果コード                                               |
| body.data.sendResultList[].resultMessage        | String  | X        | 結果メッセージ                                           |
| body.data.sendResultList[].recipientSeq         | Integer | O        | 受信者シーケンス(mtPr)                                   |
| body.data.sendResultList[].recipientGroupingKey | String  | X        | 受信者グループキー                                       |

<a id="example"></a>

#### 例

| Http method | URL                                                                       |
|-------------|---------------------------------------------------------------------------|
| POST        | https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/{appKey}/sender/auth/sms |

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

<a id="list-sms-delivery-for-authentication"></a>

### 認証用SMS送信リスト検索

<a id="request-9"></a>

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
| msgStatus            | 	String  | 1     | 	オプション | メッセージステータスコード(0：失敗、 1：リクエスト、 2：処理中、 3：成功、 4：予約キャンセル、 5：重複失敗、 6:失敗(広告制限)、 再送信待機(広告制限)) |
| resultCode           | 	String  | 10    | 	オプション | 	受信結果コード[[検索コード表](./error-code/#_2)]                         |
| subResultCode        | 	String  | 10    | 	オプション | 	受信結果詳細コード[[検索コード表](./error-code/#_3)]                       |
| senderGroupingKey    | 	String  | 100   | 	オプション | 	送信者グループキー                                                   |
| recipientGroupingKey | 	String  | 100   | 	オプション | 	受信者グループキー                                                   |
| receiverRegion       | 	String  | -     | 	オプション | 	国内/国際 (DOMESTIC: 国内, INTERNATIONAL: 国際) |
| countryCode          | 	String  | -     | 	オプション | 	国コード [[送信可能国](./international-sending-policy/#_5)] |
| pageNum              | 	Integer | -     | 	オプション | 	ページ番号(デフォルト値：1)                                             |
| pageSize             | 	Integer | 1000  | 	オプション | 	検索数(デフォルト値：15)                                              |

<a id="curl-9"></a>

#### cURL

```
curl -X GET \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/sender/auth/sms?startRequestDate='"${START_DATE}"'&endRequestDate='"${END_DATE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-9"></a>

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

| 値                                | タイプ    | Not Null | 説明                                         |
|----------------------------------|---------|----------|---------------------------------------------|
| header                           | Object  | O        | ヘッダ領域                                     |
| header.isSuccessful              | Boolean | O        | 成否                                         |
| header.resultCode                | Integer | O        | 失敗コード                                      |
| header.resultMessage             | String  | O        | 失敗メッセージ                                    |
| body                             | Object  | X        | 本文領域                                        |
| body.pageNum                     | Integer | O        | 現在のページ番号                                   |
| body.pageSize                    | Integer | O        | 検索されたデータ数                                  |
| body.totalCount                  | Integer | O        | データの総数                                     |
| body.data[].requestId            | String  | O        | リクエストID                                    |
| body.data[].requestDate          | String  | O        | 発信日時                                       |
| body.data[].resultDate           | String  | X        | 受信日時                                       |
| body.data[].templateId           | String  | X        | テンプレートID                                   |
| body.data[].templateName         | String  | X        | テンプレート名                                    |
| body.data[].categoryId           | Integer | X        | カテゴリーID                                    |
| body.data[].categoryName         | String  | X        | カテゴリー名                                     |
| body.data[].body                 | String  | O        | 本文内容                                       |
| body.data[].sendNo               | String  | O        | 発信番号                                       |
| body.data[].countryCode          | String  | O        | 国番号                                        |
| body.data[].recipientNo          | String  | O        | 受信番号                                       |
| body.data[].msgStatus            | String  | O        | メッセージステータスコード                              |
| body.data[].msgStatusName        | String  | O        | メッセージステータスコード名                             |
| body.data[].resultCode           | String  | X        | 受信結果コード[[受信結果コード表](./error-code/#emma-v3)] |
| body.data[].resultCodeName       | String  | X        | 受信結果コード名                                   |
| body.data[].telecomCode          | Integer | X        | サービスプロバイダーコード                              |
| body.data[].telecomCodeName      | String  | X        | サービスプロバイダー名                                |
| body.data[].recipientSeq         | Integer | O        | 送信詳細ID(詳細検索時は必須)(旧mtPr)                    |
| body.data[].sendType             | String  | O        | 送信タイプ(0:Sms, 1:Lms/Mms, 2:Auth)            |
| body.data[].messageType          | String  | O        | メッセージタイプ(SMS/LMS/MMS/AUTH)                 |
| body.data[].messageCount         | Integer | O        | 送信されたメッセージ件数                               |
| body.data[].userId               | String  | X        | 送信リクエストID                                  |
| body.data[].adYn                 | String  | O        | 広告かどうか                                     |
| body.data[].senderGroupingKey    | String  | X        | 発信者グループキー                                  |
| body.data[].recipientGroupingKey | String  | X        | 受信者グループキー                                  |

<a id="query-single-sms-delivery-for-authentication"></a>

### 認証用SMS送信単一検索

<a id="request-10"></a>

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

<a id="curl-10"></a>

#### cURL

```
curl -X GET \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/sender/auth/sms/'"${REQUEST_ID}"'?recipientSeq='"${RECIPIENT_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-10"></a>

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
      "recipientGroupingKey": "RecipientGroupingKey",
      "dlr": {
        "dlrStatus": "DELIVERED",
        "networkCode": "12345",
        "errorCode": "0"
      }
    }
  }
}
```

| 値                              | タイプ    | Not Null | 説明                                                                              |
|--------------------------------|---------|----------|-----------------------------------------------------------------------------------|
| header                         | Object  | O        | ヘッダ領域                                                                         |
| header.isSuccessful            | Boolean | O        | 成否                                                                              |
| header.resultCode              | Integer | O        | 失敗コード                                                                         |
| header.resultMessage           | String  | O        | 失敗メッセージ                                                                      |
| body                           | Object  | X        | 本文領域                                                                           |
| body.data.requestId            | String  | O        | リクエストID                                                                       |
| body.data.requestDate          | String  | O        | 発信日時                                                                          |
| body.data.resultDate           | String  | X        | 受信日時                                                                          |
| body.data.templateId           | String  | X        | テンプレートID                                                                    |
| body.data.templateName         | String  | X        | テンプレート名                                                                     |
| body.data.categoryId           | Integer | X        | カテゴリーID                                                                      |
| body.data.categoryName         | String  | X        | カテゴリー名                                                                      |
| body.data.body                 | String  | O        | 本文内容                                                                          |
| body.data.sendNo               | String  | O        | 発信番号                                                                          |
| body.data.countryCode          | String  | O        | 国番号                                                                           |
| body.data.recipientNo          | String  | O        | 受信番号                                                                          |
| body.data.msgStatus            | String  | O        | メッセージステータスコード                                                           |
| body.data.msgStatusName        | String  | O        | メッセージステータスコード名                                                        |
| body.data.resultCode           | String  | X        | 受信結果コード[[受信結果コード表](./error-code/#emma-v3)]                           |
| body.data.resultCodeName       | String  | X        | 受信結果コード名                                                                   |
| body.data.telecomCode          | Integer | X        | サービスプロバイダーコード                                                          |
| body.data.telecomCodeName      | String  | X        | サービスプロバイダー名                                                             |
| body.data.recipientSeq         | Integer | O        | 送信詳細ID(詳細検索時は必須)(旧mtPr)                                                 |
| body.data.sendType             | String  | O        | 送信タイプ(0:Sms, 1:Lms/Mms, 2:Auth)                                               |
| body.data.messageType          | String  | O        | メッセージタイプ(SMS/LMS/MMS/AUTH)                                                 |
| body.data.messageCount         | Integer | O        | 送信されたメッセージの件数                                                          |
| body.data.userId               | String  | X        | 送信リクエストID                                                                   |
| body.data.adYn                 | String  | O        | 広告かどうか                                                                      |
| body.data.originCode           | String  | X        | 識別コード(特殊なタイプの付加通信事業者登録証に記載されている記号、文字、空白を除外した登録番号9桁の数字) |
| body.data.resultMessage        | String  | X        | 結果メッセージ                                                                     |
| body.data.senderGroupingKey    | String  | X        | 発信者グループキー                                                                |
| body.data.recipientGroupingKey | String  | X        | 受信者グループキー                                                                |
| body.data.dlr.dlrStatus        | String  | X        | DLRステータスコード                                                               |
| body.data.dlr.networkCode      | String  | X        | DLRネットワークコード                                                             |
| body.data.dlr.errorCode        | String  | X        | DLRエラーコード                                                                   |

<a id="convert-authentication-sms-internaional-delivery"></a>

### 認証SMS国際送信コンバージョン

* コンバージョンAPIは認証SMS国際送信時にコンバージョン率収集をリクエストした送信件に対して、正常にコンバージョンされたことをレスポンスするAPIです。
* このAPIを使用すると、正常に送信されたメッセージのコンバージョン率を管理できます。
* 送信時にuseConversionフィールドでコンバージョン率収集をリクエストしていない場合、または送信が完了していない場合、このAPIは失敗としてレスポンスします。

<a id="request-11"></a>

#### リクエスト

[URL]

```
POST  /sms/v3.0/appKeys/{appKey}/sender/auth/sms/do-convert
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値    | タイプ   | 説明   |
|--------|--------|--------|
| appKey | String | 固有のアプリキー |

[Header]

```json
{
  "X-Secret-Key": "{secret-key}"
}
```

| 値          | タイプ   | 説明      |
|--------------|--------|-----------|
| X-Secret-Key | String | 固有のシークレットキー |

[Request body]

```json
{
  "requestId": "requestId",
  "recipientSeq": 1
}
```

| 値          | タイプ    | 最大長さ | 必須 | 説明    |
|--------------|---------|-------|----|---------|
| requestId    | String  | 25    | O  | リクエストID   |
| recipientSeq | Integer | -     | O  | 受信者シーケンス |

<a id="curl-11"></a>

#### cURL

```
curl -X GET \
'https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/sender/auth/sms/do-convert \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}' \
-d '{
    "requestId": "requestId",
    "recipientSeq": 1
}'
```

<a id="response-11"></a>

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  }
}
```

| 値                   | タイプ    | Not Null | 説明        |
|---------------------|---------|----------|-------------|
| header              | Object  | O        | ヘッダ領域    |
| header.isSuccessful | Boolean | O        | 成否        |
| header.resultCode   | Integer | O        | 失敗コード    |
| header.resultMessage| String  | O        | 失敗メッセージ |

<a id="advertising-message"></a>

## 広告文字

<a id="send-advertising-sms"></a>

### 広告性SMS送信

<a id="request-12"></a>

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
- 最後の文言： 無料受信拒否 {080受信拒否番号}`または `無料受信拒否 {080受信拒否番号}`
  -この文言には空白が含まれる場合があります。
 - 080受信拒否番号の間に「-」を含めることができます。
  
例
```
(広告)

[無料受信拒否]080XXXXXXX
```
```
(広告)
無料拒否080XXXXXXX
```
```
(広告)
無料拒否 080-XXX-XXXX
```

<a id="curl-12"></a>

#### cURL

```
curl -X POST \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/sender/ad-sms' \
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

<a id="send-mms-for-advertisement"></a>

### 広告性MMS送信

※ LMS/MMSは海外送信ができません。しかし、国際SMS限定でSMSのConcatenated Message(接続)機能を使って長いメッセージを送信できます。 [[国際SMS送信ポリシー](./international-sending-policy/#_3)]

<a id="request-13"></a>

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
```
(広告)
無料拒否 080-XXX-XXXX
```

<a id="curl-13"></a>

#### cURL

```
curl -X POST \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/sender/ad-mms' \
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

<a id="convert-advertising-sms-internaional-delivery"></a>

### 広告SMS国際送信コンバージョン

* コンバージョンAPIは広告SMS国際送信時にコンバージョン率収集をリクエストした送信件に対して、正常にコンバージョンされたことをレスポンスするAPIです。
* このAPIを使用すると、正常に送信されたメッセージのコンバージョン率を管理できます。

<a id="request-14"></a>

#### リクエスト

[URL]

```
POST  /sms/v3.0/appKeys/{appKey}/sender/sms/do-convert
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値    | タイプ   | 説明   |
|--------|--------|--------|
| appKey | String | 固有のアプリキー |

[Header]

```json
{
  "X-Secret-Key": "{secret-key}"
}
```

| 値          | タイプ   | 説明      |
|--------------|--------|-----------|
| X-Secret-Key | String | 固有のシークレットキー |

[Request body]

```json
{
  "requestId": "requestId",
  "recipientSeq": 1
}
```

| 値          | タイプ    | 最大長さ | 必須 | 説明    |
|--------------|---------|-------|----|---------|
| requestId    | String  | 25    | O  | リクエストID   |
| recipientSeq | Integer | -     | O  | 受信者シーケンス |

<a id="curl-14"></a>

#### cURL

```
curl -X GET \
'https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/sender/sms/do-convert \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}' \
-d '{
    "requestId": {requestId},
    "recipientSeq": 1
}'
```

<a id="reponse"></a>

#### レスポンス

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  }
}
```

| 値                   | タイプ    | Not Null | 説明       |
|----------------------|---------|----------|------------|
| header               | Object  | O        | ヘッダ領域    |
| header.isSuccessful  | Boolean | O        | 成否       |
| header.resultCode    | Integer | O        | 失敗コード |
| header.resultMessage | String  | O        | 失敗メッセージ |

<a id="search-messages-based-on-result-update"></a>

## 結果アップデート基準メッセージ検索

* このAPIはメッセージ送信結果アップデート時間を基準に検索されます。
* 端末送信結果をサービス外で使用する場合はこのAPIを使用してください。

<a id="search-messages"></a>

### メッセージ検索

<a id="request-15"></a>

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

<a id="curl-15"></a>

#### cURL

```
curl -X GET \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/message-results?startRequestDate='"${START_DATE}"'&endRequestDate='"${END_DATE}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-12"></a>

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

| 値                                                 | タイプ    | Not Null | 説明                             |
|----------------------------------------------------|---------|----------|----------------------------------|
| header                                             | Object  | O        | ヘッダ領域                          |
| header.isSuccessful                                | Boolean | O        | 成否                              |
| header.resultCode                                  | Integer | O        | 失敗コード                          |
| header.resultMessage                               | String  | O        | 失敗メッセージ                       |
| body                                               | Object  | X        | 本文領域                           |
| body.data.resultUpdateList[].messageType           | String  | O        | メッセージタイプ(SMS/LMS/MMS/AUTH)    |
| body.data.resultUpdateList[].requestId             | String  | O        | リクエストID                        |
| body.data.resultUpdateList[].recipientSeq          | Integer | O        | 受信者シーケンス                      |
| body.data.resultUpdateList[].resultCode            | String  | X        | 結果コード                          |
| body.data.resultUpdateList[].resultCodeName        | String  | X        | 結果コード名                         |
| body.data.resultUpdateList[].requestDate           | String  | O        | リクエスト日時(yyyy-MM-dd HH:mm:ss.S) |
| body.data.resultUpdateList[].resultDate            | String  | X        | 受信日時(yyyy-MM-dd HH:mm:ss.S)     |
| body.data.resultUpdateList[].updateDate            | String  | X        | 結果アップデート日時(yyyy-MM-dd HH:mm:ss.S) |
| body.data.resultUpdateList[].telecomCode           | String  | X        | サービスプロバイダーコード            |
| body.data.resultUpdateList[].telecomCodeName       | String  | X        | サービスプロバイダーコード名          |
| body.data.resultUpdateList[].senderGroupingKey     | String  | X        | 発信者グループキー                    |
| body.data.resultUpdateList[].recipientGroupingKey  | String  | X        | 受信者グループキー                    |

<a id="mass-delivery"></a>

## 大量送信

<a id="list-mass-delivery"></a>

### 大量送信リスト検索

<a id="request-16"></a>

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

<a id="curl-16"></a>

#### cURL

```
curl -X GET \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/mass-sender?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-13"></a>

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

| 値                            | タイプ     | Not Null  | 説明               |
|------------------------------|----------|------------|-------------------|
| header                       | Object   | O          | ヘッダ領域           |
| header.isSuccessful          | Boolean  | O          | 成否               |
| header.resultCode            | Integer  | O          | 失敗コード           |
| header.resultMessage         | String   | O          | 失敗メッセージ        |
| body                         | Object   | X          | 本文領域            |
| body.data[].requestId        | String   | O          | リクエストID         |
| body.data[].requestDate      | String   | O          | リクエスト時間        |
| body.data[].masterStatusCode | String   | O          | 大量送信ステータスコード |
| body.data[].masterStatus     | String   | X          | 大量送信ステータス     |
| body.data[].templateId       | String   | X          | テンプレートID       |
| body.data[].sendNo           | String   | O          | 発信者番号          |
| body.data[].title            | String   | X          | タイトル           |
| body.data[].body             | String   | O          | 内容              |
| body.data[].adYn             | String   | X          | 広告かどうか         |
| body.data[].autoSendYn       | String   | O          | 自動送信を行うかどうか  |
| body.data[].sendErrorCount   | Integer  | X          | エラー受信者件数      |
| body.data[].createUser       | String   | X          | 作成者            |
| body.data[].createDate       | String   | O          | 作成日時           |

<a id="list-recipients-of-mass-delivery"></a>

### 大量送信受信者リスト検索

<a id="request-17"></a>

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
| receiverRegion   | String  | -    | X   | 国内/国際 (DOMESTIC: 国内, INTERNATIONAL: 国際) |
| countryCode      | String  | -    | X   | 国コード [[送信可能国](./international-sending-policy/#_5)] |
| pageNum          | Integer | -    | X   | ページ番号                                                                                                 |
| pageSize         | Integer | 1000 | X   | 検索数                                                                                                   |

<a id="curl-17"></a>

#### cURL

```
curl -X GET \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/mass-sender/requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-14"></a>

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

| 値                         | タイプ    | Not Null  | 説明                                                |
|----------------------------|---------|-----------|-----------------------------------------------------|
| header                     | Object  | O         | ヘッダ領域                                            |
| header.isSuccessful        | Boolean | O         | 成否                                                |
| header.resultCode          | Integer | O         | 失敗コード                                           |
| header.resultMessage       | String  | O         | 失敗メッセージ                                       |
| body                       | Object  | X         | 本文領域                                           |
| body.data[].requestId      | String  | O         | リクエストID                                         |
| body.data[].recipientSeq   | Integer | O         | 受信者シーケンス                                     |
| body.data[].countryCode    | String  | O         | 受信者国家コード                                     |
| body.data[].recipientNo    | String  | O         | 受信者番号                                           |
| body.data[].requestDate    | String  | O         | リクエスト日時                                       |
| body.data[].msgStatus      | String  | O         | メッセージステータスコード                            |
| body.data[].msgStatusName  | String  | O         | メッセージステータスコード名                          |
| body.data[].messageCount   | Integer | O         | 送信されたメッセージの件数                            |
| body.data[].resultCode     | String  | X         | 受信結果コード[[受信結果コード表](./error-code/#emma-v3)] |
| body.data[].receiveDate    | String  | X         | 受信日時                                             |

<a id="list-recipient-details-of-mass-delivery"></a>

### 大量送信受信者リスト詳細検索

<a id="request-18"></a>

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

<a id="curl-18"></a>

#### cURL

```
curl -X GET \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/mass-sender/receive/'"${REQUEST_ID}"'/'"${RECIPIENT_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-15"></a>

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
      "attachFileList": [],
      "dlr": {
        "dlrStatus": "DELIVERED",
        "networkCode": "12345",
        "errorCode": "0"
      }
    }
  }
}
```

| 値                                       | タイプ    | Not Null  | 説明                                              |
|-----------------------------------------|---------|------------|---------------------------------------------------|
| header                                  | Object  | O          | ヘッダ領域                                         |
| header.isSuccessful                     | Boolean | O          | 成否                                              |
| header.resultCode                       | Integer | O          | 失敗コード                                        |
| header.resultMessage                    | String  | O          | 失敗メッセージ                                    |
| body                                    | Object  | X          | 本文領域                                        |
| body.data.requestId                     | String  | O          | リクエストID                                      |
| body.data.recipientSeq                  | Integer | O          | 受信者シーケンス                                  |
| body.data.sendType                      | String  | O          | 送信タイプ                                        |
| body.data.messageType                   | String  | O          | メッセージタイプ                                  |
| body.data.templateId                    | String  | X          | テンプレートID                                    |
| body.data.templateName                  | String  | X          | テンプレート名                                    |
| body.data.sendNo                        | String  | O          | 発信番号                                          |
| body.data.title                         | String  | X          | タイトル                                          |
| body.data.body                          | String  | O          | 内容                                              |
| body.data.recipientNo                   | String  | O          | 受信者番号                                        |
| body.data.countryCode                   | String  | O          | 受信者国コード                                    |
| body.data.requestDate                   | String  | O          | リクエスト日時                                    |
| body.data.msgStatus                     | String  | O          | メッセージ状態                                    |
| body.data.msgStatusName                 | String  | O          | メッセージ状態名                                  |
| body.data.messageCount                  | Integer | O          | 送信されたメッセージの件数                        |
| body.data.resultCode                    | String  | X          | 受信結果コード[[受信結果コード表](./error-code/#emma-v3)] |
| body.data.receiveDate                   | String  | X          | 受信日時                                          |
| body.data.createDate                    | String  | X          | 登録日時                                          |
| body.data.attachFileList[].filePath     | String  | X          | 添付ファイル - パス                               |
| body.data.attachFileList[].fileName     | String  | X          | 添付ファイル - ファイル名                         |
| body.data.attachFileList[].fileSize     | Long    | X          | 添付ファイル - サイズ                             |
| body.data.attachFileList[].fileSequence | Integer | X          | 添付ファイル - ファイル番号                       |
| body.data.attachFileList[].createDate   | String  | X          | 添付ファイル - 作成日時                           |
| body.data.attachFileList[].updateDate   | String  | X          | 添付ファイル - 修正日                             |
| body.data.dlr.dlrStatus                 | String  | X          | DLRステータスコード                               |
| body.data.dlr.networkCode               | String  | X          | DLRネットワークコード                             |
| body.data.dlr.errorCode                 | String  | X          | DLRエラーコード                                   |

<a id="tag-delivery"></a>

## タグ送信

<a id="send-tagged-sms"></a>

### タグSMS送信

<a id="request-19"></a>

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
| requestDate       | String              | -                                                                 | X  | 予約日時(yyyy-MM-dd HH:mm)<br/>現在から最大60日後まで設定可            |
| templateId        | String              | 50                                                             | X  | テンプレートID                            |
| templateParameter | Map<String, String> | -                                                              | X  | テンプレートパラメータ                         |
| tagExpression     | List<String>        | -                                                              | O  | タグ表現式<br/>ex) ["tagA","AND","tabB"] |
| userId            | String              | 100                                                            | X  | リクエストしたユーザーID                       |
| adYn              | String              | 1                                                              | X  | 広告かどうか(デフォルト値：N)                    |
| autoSendYn        | String              | 1                                                              | X  | 自動送信(即時送信)を行うかどうか(デフォルト値：Y)         |
| statsId           | String              | 10                                                             | X  | 統計ID(発信検索条件には含まれません)                |
| originCode        | String              | 10                                                             | X  | Identification code (9-digit registration number, excluding symbols, letters, and spaces, as listed on certificates for special value-added telecommunications business operators)<br/>Do not use unless you are special value-added telecommunications business operator. NHN Cloud's identification code is added by default.<br/> |

<a id="curl-19"></a>

#### cURL

```
curl -X POST \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/tag-sender/sms' \
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

<a id="response-16"></a>

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

| 値                    | タイプ    | Not Null  | 説明        |
|----------------------|---------|------------|-------------|
| header               | Object  | O          | ヘッダ領域    |
| header.isSuccessful  | Boolean | O          | 成否        |
| header.resultCode    | Integer | O          | 失敗コード  |
| header.resultMessage | String  | O          | 失敗メッセージ |
| body                 | Object  | X          | 本文領域    |
| body.data.requestId  | String  | O          | リクエストID |

<a id="send-tagged-lms"></a>

### タグLMS送信

※ LMS/MMSは海外送信ができません。しかし、国際SMS限定でSMSのConcatenated Message(接続)機能を使って長いメッセージを送信できます。 [[国際SMS送信ポリシー](./international-sending-policy/#_3)]

<a id="request-20"></a>

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
| requestDate       | String              | -      | X  | 予約日時(yyyy-MM-dd HH:mm)<br/>現在から最大60日後まで設定可            |
| templateId        | String              | 50    | X  | テンプレートID                            |
| templateParameter | Map<String, String> | -     | X  | テンプレートパラメータ                         |
| tagExpression     | List<String>        | -     | O  | タグ表現式<br/>ex) ["tagA","AND","tabB"] |
| attachFileIdList  | List<Integer>       | -     | X  | 添付ファイルID(fileId)                    |
| userId            | String              | 100   | X  | リクエストしたユーザーID                       |
| adYn              | String              | 1     | X  | 広告かどうか(デフォルト値：N)                    |
| autoSendYn        | String              | 1     | X  | 自動送信(即時送信)を行うかどうか(基本Y)              |
| statsId           | String              | 10    | X  | 統計ID(発信検索条件には含まれません)                |
| originCode        | String              | 10    | X  | 識別コード(特殊なタイプの付加通信事業者登録証に記載されている記号、文字、空白を除外した登録番号9桁の数字)               |

<a id="curl-20"></a>

#### cURL

```
curl -X POST \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/tag-sender/mms' \
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

<a id="response-17"></a>

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

| 値                    | タイプ    | Not Null | 説明         |
|----------------------|---------|----------|--------------|
| header               | Object  | O        | ヘッダ領域    |
| header.isSuccessful  | Boolean | O        | 成否         |
| header.resultCode    | Integer | O        | 失敗コード    |
| header.resultMessage | String  | O        | 失敗メッセージ |
| body                 | Object  | X        | 本文領域     |
| body.data.requestId  | String  | O        | リクエストID  |

<a id="list-tag-delivery"></a>

### タグ送信リスト検索

<a id="request-21"></a>

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

<a id="curl-21"></a>

#### cURL

```
curl -X GET \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/tag-sender?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-18"></a>

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

| 値                           | タイプ         | Not Null | 説明                |
|-----------------------------|--------------|----------|---------------------|
| header                      | Object       | O        | ヘッダ領域            |
| header.isSuccessful         | Boolean      | O        | 成否                |
| header.resultCode           | Integer      | O        | 失敗コード          |
| header.resultMessage        | String       | O        | 失敗メッセージ      |
| body                        | Object       | X        | 本文領域           |
| body.data[].requestId       | String       | O        | リクエストID        |
| body.data[].requestIp       | String       | X        | リクエストIP        |
| body.data[].requestDate     | String       | O        | リクエスト時間      |
| body.data[].tagSendStatus   | String       | X        | タグ送信ステータス  |
| body.data[].tagExpression[] | List<String> | O        | タグ表現式          |
| body.data[].templateId      | String       | X        | テンプレートID      |
| body.data[].templateName    | String       | X        | テンプレート名      |
| body.data[].senderName      | String       | X        | 発信者名            |
| body.data[].senderMail      | String       | X        | 発信者アドレス      |
| body.data[].title           | String       | X        | タイトル            |
| body.data[].body            | String       | O        | 内容                |
| body.data[].adYn            | String       | X        | 広告かどうか        |
| body.data[].autoSendYn      | String       | O        | 自動送信を行うかどうか |
| body.data[].sendErrorCount  | Integer      | O        | エラー受信者件数    |
| body.data[].createUser      | String       | X        | 作成者              |
| body.data[].createDate      | String       | O        | 作成日時            |
| body.data[].updateUser      | String       | X        | 修正したユーザー    |
| body.data[].updateDate      | String       | X        | 修正日              |

<a id="list-recipients-of-tag-delivery"></a>

### タグ送信受信者リスト検索

<a id="request-22"></a>

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
| receiverRegion   | String  | -    | X   | 国内/国際 (DOMESTIC: 国内, INTERNATIONAL: 国際) |
| countryCode      | String  | -    | X   | 国コード [[送信可能国](./international-sending-policy/#_5)] |
| pageNum          | Integer | -    | X   | ページ番号                                                                                                 |
| pageSize         | Integer | 1000 | X   | 検索数                                                                                                   |

<a id="curl-22"></a>

#### cURL

```
curl -X GET \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/tag-sender/'"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-19"></a>

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

| 値                         | タイプ    | Not Null | 説明                                           |
|----------------------------|---------|----------|-----------------------------------------------|
| header                     | Object  | O        | ヘッダ領域                                     |
| header.isSuccessful        | Boolean | O        | 成否                                          |
| header.resultCode          | Integer | O        | 失敗コード                                     |
| header.resultMessage       | String  | O        | 失敗メッセージ                                 |
| body                       | Object  | X        | 本文領域                                      |
| body.data[].requestId      | String  | O        | リクエストID                                  |
| body.data[].recipientSeq   | Integer | O        | 受信者シーケンス                               |
| body.data[].countryCode    | String  | O        | 受信者国家コード                               |
| body.data[].recipientNo    | String  | O        | 受信者番号                                     |
| body.data[].requestDate    | String  | O        | リクエスト日時                                 |
| body.data[].msgStatus      | String  | O        | メッセージステータスコード                      |
| body.data[].msgStatusName  | String  | O        | メッセージステータスコード名                    |
| body.data[].messageCount   | Integer | O        | 送信されたメッセージ件数                        |
| body.data[].resultCode     | String  | X        | 受信結果コード[[受信結果コード表](./error-code/#emma-v3)] |
| body.data[].receiveDate    | String  | X        | 受信日時                                       |
| body.data[].createDate     | String  | X        | 登録日時                                       |
| body.data[].updateDate     | String  | X        | 修正日付                                       |

<a id="list-recipient-details-of-tagged-delivery"></a>

### タグ送信受信者詳細検索

<a id="request-23"></a>

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

<a id="curl-23"></a>

#### cURL

```
curl -X GET \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/tag-sender/'"${REQUEST_ID}"'/'"${RECIPIENT_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-20"></a>

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
      "originCode": "123456789",
      "dlr": {
        "dlrStatus": "DELIVERED",
        "networkCode": "12345",
        "errorCode": "0"
      }
    }
  }
}
```

| 値                                       | タイプ    | Not Null | 説明                                                                   |
|-----------------------------------------|---------|----------|------------------------------------------------------------------------|
| header                                  | Object  | O        | ヘッダ領域                                                                |
| header.isSuccessful                     | Boolean | O        | 成否                                                                   |
| header.resultCode                       | Integer | O        | 失敗コード                                                              |
| header.resultMessage                    | String  | O        | 失敗メッセージ                                                          |
| body                                    | Object  | X        | 本文領域                                                              |
| body.data.requestId                     | String  | O        | リクエストID                                                           |
| body.data.recipientSeq                  | Integer | O        | 受信者シーケンス                                                        |
| body.data.sendType                      | String  | O        | 送信タイプ                                                             |
| body.data.messageType                   | String  | O        | メッセージタイプ                                                        |
| body.data.templateId                    | String  | X        | テンプレートID                                                         |
| body.data.templateName                  | String  | X        | テンプレート名                                                         |
| body.data.sendNo                        | String  | O        | 発信番号                                                              |
| body.data.title                         | String  | X        | タイトル                                                              |
| body.data.body                          | String  | O        | 内容                                                                  |
| body.data.recipientNum                  | String  | O        | 受信者番号                                                             |
| body.data.requestDate                   | String  | O        | リクエスト日時                                                         |
| body.data.msgStatusName                 | String  | O        | メッセージ状態名                                                        |
| body.data.messageCount                  | Integer | O        | 送信されたメッセージの件数                                               |
| body.data.resultCode                    | String  | X        | 受信結果コード[[受信結果コード表](./error-code/#emma-v3)]                 |
| body.data.receiveDate                   | String  | X        | 受信日時                                                              |
| body.data.attachFileList[].filePath     | String  | X        | 添付ファイル - パス                                                     |
| body.data.attachFileList[].fileName     | String  | X        | 添付ファイル - ファイル名                                                |
| body.data.attachFileList[].fileSize     | Long    | X        | 添付ファイル - サイズ                                                    |
| body.data.attachFileList[].fileSequence | Integer | X        | 添付ファイル - ファイル番号                                               |
| body.data.attachFileList[].createDate   | String  | X        | 添付ファイル - 作成日時                                                  |
| body.data.attachFileList[].updateDate   | String  | X        | 添付ファイル - 修正日                                                    |
| body.data.originCode                    | String  | X        | 識別コード（特殊なタイプの付加通信事業者登録証に記載されている記号、文字、空白を除く登録番号9桁の数字) |
| body.data.dlr.dlrStatus                 | String  | X        | DLRステータスコード                                                      |
| body.data.dlr.networkCode               | String  | X        | DLRネットワークコード                                                   |
| body.data.dlr.errorCode                 | String  | X        | DLRエラーコード                                                        |
<span id="binaryUpload"></span>

<a id="attached-files"></a>

## 添付ファイル

<a id="upload-attached-files"></a>

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

| 値          | タイプ    | 最大長さ | 必須 | 説明                                          |
|------------|--------|------|----|---------------------------------------------|
| fileName   | String | 45   | 必須 | ファイル名(拡張子はjpg, jpegのみ可能)                    |
| fileBody   | Byte[] | 300K | 必須 | ファイルbyte[]をBase64でエンコードした値。<br/>* またはバイト配列値 |
| createUser | String | 100  | 必須 | ファイルアップロードユーザー情報                            |

<a id="curl-24"></a>

#### cURL

```
curl -X POST \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/attachfile/binaryUpload' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}'  \
-d '{
    "fileName": "attachement.jpg",
    "createUser": "API Guide",
    "fileBody": "1234567890"
}'
```

<a id="response-21"></a>

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

| 値                    | タイプ    | Not Null | 説明                                                             |
|----------------------|---------|----------|------------------------------------------------------------------|
| header               | Object  | O        | ヘッダ領域                                                         |
| header.isSuccessful  | Boolean | O        | 成否                                                             |
| header.resultCode    | Integer | O        | 失敗コード                                                         |
| header.resultMessage | String  | O        | 失敗メッセージ                                                      |
| body                 | Object  | X        | 本文領域                                                          |
| body.data.fileId     | Integer | O        | ファイルID                                                        |
| body.data.fileName   | String  | X        | ファイル名                                                         |
| body.data.filePath   | String  | X        | 添付ファイル基本パス <br/>(https://domain/attachFile/filePath/fileName) |

<a id="example-of-uploading-attached-files"></a>

#### 添付ファイルアップロード例

| Http method | URL                                                                               |
|-------------|-----------------------------------------------------------------------------------|
| POST        | https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/{appKey}/attachfile/binaryUpload |

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

<a id="category"></a>

## カテゴリー

<a id="register"></a>

### カテゴリー登録

<a id="request-24"></a>

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

<a id="curl-25"></a>

#### cURL

```
curl -X POST \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/categories' \
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

<a id="response-22"></a>

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

| 値                                   | タイプ     | Not Null | 説明             |
|-------------------------------------|----------|----------|------------------|
| header                              | Object   | O        | ヘッダ領域         |
| header.isSuccessful                 | Boolean  | O        | 成否             |
| header.resultCode                   | Integer  | O        | 失敗コード       |
| header.resultMessage                | String   | O        | 失敗メッセージ   |
| body                                | Object   | X        | 本文領域       |
| body.data[].categoryId              | Integer  | O        | カテゴリーID     |
| body.data[].categoryParentId        | Integer  | X        | 親カテゴリーID   |
| body.data[].depth                   | Integer  | X        | カテゴリーの深さ |
| body.data[].sort                    | Integer  | X        | カテゴリーソート順序 |
| body.data[].categoryName            | String   | X        | カテゴリー名     |
| body.data[].categorycategoryDescame | String   | X        | カテゴリー説明   |
| body.data[].useYn                   | String   | O        | 使用するかどうか |
| body.data[].createUser              | String   | X        | 登録したユーザー |

<a id="list-category"></a>

### カテゴリーリスト検索

<a id="request-25"></a>

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

<a id="curl-26"></a>

#### cURL

```
curl -X GET \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/categories' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-23"></a>

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

| 値                                   | タイプ    | Not Null | 説明           |
|-------------------------------------|---------|----------|---------------|
| header                              | Object  | O        | ヘッダ領域       |
| header.isSuccessful                 | Boolean | O        | 成否           |
| header.resultCode                   | Integer | O        | 失敗コード       |
| header.resultMessage                | String  | O        | 失敗メッセージ     |
| body                                | Object  | X        | 本文領域        |
| body.pageNum                        | Integer | O        | 現在のページ番号   |
| body.pageSize                       | Integer | O        | 検索されたデータ数  |
| body.totalCount                     | Integer | O        | データの総数      |
| body.data[].categoryId              | Integer | O        | カテゴリーID     |
| body.data[].categoryParentId        | Integer | X        | 親カテゴリーID    |
| body.data[].depth                   | Integer | X        | カテゴリーの深さ   |
| body.data[].sort                    | Integer | X        | カテゴリーソート順序 |
| body.data[].categoryName            | String  | X        | カテゴリー名      |
| body.data[].categorycategoryDescame | String  | X        | カテゴリー説明     |
| body.data[].useYn                   | String  | O        | 使用するかどうか   |
| body.data[].createDate              | String  | X        | 登録日         |
| body.data[].createUser              | String  | X        | 登録したユーザー   |
| body.data[].updateDate              | String  | X        | 修正日         |
| body.data[].updateUser              | String  | X        | 修正したユーザー   |

<a id="get-category"></a>

### カテゴリー単件検索

<a id="request-26"></a>

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

<a id="curl-27"></a>

#### cURL

```
curl -X GET \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/categories/'"${CATEGORY_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-24"></a>

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

| 値                                   | タイプ    | Not Null | 説明             |
|-------------------------------------|---------|----------|------------------|
| header                              | Object  | O        | ヘッダ領域          |
| header.isSuccessful                 | Boolean | O        | 成否              |
| header.resultCode                   | Integer | O        | 失敗コード          |
| header.resultMessage                | String  | O        | 失敗メッセージ       |
| body                                | Object  | X        | 本文領域           |
| body.data[].categoryId              | Integer | O        | カテゴリーID        |
| body.data[].categoryParentId        | Integer | X        | 親カテゴリーID      |
| body.data[].depth                   | Integer | X        | カテゴリーの深さ     |
| body.data[].sort                    | Integer | X        | カテゴリーソート順序  |
| body.data[].categoryName            | String  | X        | カテゴリー名         |
| body.data[].categorycategoryDescame | String  | X        | カテゴリー説明       |
| body.data[].useYn                   | String  | O        | 使用するかどうか     |
| body.data[].createDate              | String  | X        | 登録日             |
| body.data[].createUser              | String  | X        | 登録したユーザー     |
| body.data[].updateDate              | String  | X        | 修正日             |
| body.data[].updateUser              | String  | X        | 修正したユーザー     |

<a id="modify-category"></a>

### カテゴリー修正

<a id="request-27"></a>

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

<a id="curl-28"></a>

#### cURL

```
curl -X PUT \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/categories/'"${C_ID}" \
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

<a id="response-25"></a>

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

| 値                    | タイプ     | Not Null | 説明         |
|----------------------|----------|----------|--------------|
| header               | Object   | O        | ヘッダ領域     |
| header.isSuccessful  | Boolean  | O        | 成否         |
| header.resultCode    | Integer  | O        | 失敗コード   |
| header.resultMessage | String   | O        | 失敗メッセージ |

<a id="delete-category"></a>

### カテゴリー削除

<a id="request-28"></a>

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

<a id="curl-29"></a>

#### cURL

```
curl -X DELETE \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/categories/'"${CATEGORY_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-26"></a>

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

| 値                    | タイプ     | Not Null | 説明         |
|----------------------|----------|----------|--------------|
| header               | Object   | O        | ヘッダ領域     |
| header.isSuccessful  | Boolean  | O        | 成否         |
| header.resultCode    | Integer  | O        | 失敗コード   |
| header.resultMessage | String   | O        | 失敗メッセージ |

<a id="templates"></a>

## テンプレート

<a id="register-2"></a>

### テンプレート登録

<a id="request-29"></a>

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

<a id="curl-30"></a>

#### cURL

```
curl -X POST \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/templates' \
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

<a id="response-27"></a>

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

| 値                    | タイプ     | Not Null | 説明         |
|----------------------|----------|----------|--------------|
| header               | Object   | O        | ヘッダ領域     |
| header.isSuccessful  | Boolean  | O        | 成否         |
| header.resultCode    | Integer  | O        | 失敗コード   |
| header.resultMessage | String   | O        | 失敗メッセージ |

<a id="example-of-registration"></a>

#### テンプレート登録例

| Http method | URL                                                                 |
|-------------|---------------------------------------------------------------------|
| POST        | https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/{appKey}/templates |

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

<a id="send-templates-requiring-no-body-updates"></a>

### テンプレート送信(本文の修正が必要ない場合)

| Http method | 種類  | URL                                                                  |
|-------------|-----|----------------------------------------------------------------------|
| POST        | SMS | https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/{appKey}/sender/sms |
| POST        | MMS | https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/{appKey}/sender/mms |

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

<a id="send-templates-requiring-body-updates"></a>

### テンプレート送信(本文の修正が必要な場合)

<a id="example-of-sending-templates"></a>

#### テンプレート送信例

| Http method | 種類  | URL                                                                  |
|-------------|-----|----------------------------------------------------------------------|
| POST        | SMS | https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/{appKey}/sender/sms |
| POST        | MMS | https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/{appKey}/sender/mms |

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

<a id="list-templates"></a>

### テンプレートリスト検索

<a id="request-30"></a>

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

| 値        | 	タイプ   | 	必須  | 	説明          |
|------------|----------|-----|----------------|
| categoryId | 	Integer | 	オプション | 	カテゴリーID       |
| useYn      | 	String  | 	オプション | 	使用するかどうか(Y/N)    |
| pageNum    | 	Integer | オプション | 	ページ番号(デフォルト値：1) |
| pageSize   | 	Integer | オプション | 	検索数(デフォルト値：15) |

<a id="curl-31"></a>

#### cURL

```
curl -X GET \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/templates' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-28"></a>

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
        "sendTypeName": "SMS送信",
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

| 値                                         | タイプ    | Not Null | 説明                               |
|--------------------------------------------|---------|----------|-----------------------------------|
| header                                     | Object  | O        | ヘッダ領域                           |
| header.isSuccessful                        | Boolean | O        | 成否                               |
| header.resultCode                          | Integer | O        | 失敗コード                           |
| header.resultMessage                       | String  | O        | 失敗メッセージ                        |
| body                                       | Object  | X        | 本文領域                            |
| body.pageNum                               | Integer | O        | 現在のページ番号                       |
| body.pageSize                              | Integer | O        | 検索されたデータ数                     |
| body.totalCount                            | Integer | O        | データの総数                         |
| body.data[].templateId                     | String  | O        | テンプレートID                        |
| body.data[].serviceId                      | Integer | O        | サービスID(内部用、未使用値)             |
| body.data[].categoryId                     | Integer | X        | カテゴリーID                         |
| body.data[].categoryName                   | String  | X        | カテゴリー名                          |
| body.data[].sort                           | Integer | X        | ソート値                            |
| body.data[].templateName                   | String  | X        | テンプレート名                        |
| body.data[].templateDesc                   | String  | X        | テンプレート説明                       |
| body.data[].useYn                          | String  | X        | 使用するかどうか                       |
| body.data[].priority                       | String  | X        | 優先順位値(未使用値)                   |
| body.data[].sendNo                         | String  | X        | 発信番号                            |
| body.data[].sendType                       | String  | X        | 送信タイプ(0:Sms, 1:Lms/Mms, 2:Auth) |
| body.data[].sendTypeName                   | String  | X        | 送信タイプ名                          |
| body.data[].title                          | String  | X        | タイトル                            |
| body.data[].body                           | String  | X        | 本文内容                            |
| body.data[].attachFileYn                   | String  | X        | 添付ファイルかどうか(Y/N)               |
| body.data[].delYn                          | String  | X        | 削除Y/N、現在状態表記用でのみ使用          |
| body.data[].createDate                     | String  | X        | 登録日                             |
| body.data[].createUser                     | String  | X        | 登録したユーザー                      |
| body.data[].updateDate                     | String  | X        | 修正日                             |
| body.data[].updateUser                     | String  | X        | 修正したユーザー                      |
| body.data[].attachFileList[].fileId        | Integer | O        | ファイルID                          |
| body.data[].attachFileList[].filePath      | String  | X        | ファイル保存パス(内部用)                |
| body.data[].attachFileList[].filename      | String  | X        | ファイル名                           |
| body.data[].attachFileList[].saveFileName  | String  | X        | 保存された添付ファイル名                 |
| body.data[].attachFileList[].uploadType    | String  | X        | アップロードタイプ                     |

<a id="query-single-template"></a>

### テンプレート単一検索

<a id="request-31"></a>

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

<a id="curl-32"></a>

#### cURL

```
curl -X GET \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/templates/'"${TEMPLATE_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-29"></a>

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
      "sendTypeName": "SMS送信",
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

| 値                                         | タイプ     | Not Null | 説明                             |
|-------------------------------------------|----------|----------|----------------------------------|
| header                                    | Object   | O        | ヘッダ領域                          |
| header.isSuccessful                       | Boolean  | O        | 成否                              |
| header.resultCode                         | Integer  | O        | 失敗コード                          |
| header.resultMessage                      | String   | O        | 失敗メッセージ                       |
| body                                       | Object  | X        | 本文領域                           |
| body.pageNum                              | Integer  | O        | 現在のページ番号                     |
| body.pageSize                             | Integer  | O        | 検索されたデータ数                    |
| body.totalCount                           | Integer  | O        | データの総数                        |
| body.data.templateId                      | String   | O        | テンプレートID                       |
| body.data.serviceId                       | Integer  | O        | サービスID(内部用、未使用値)             |
| body.data.categoryId                      | Integer  | X        | カテゴリーID                         |
| body.data.categoryName                    | String   | X        | カテゴリー名                          |
| body.data.sort                            | Integer  | X        | ソート値                             |
| body.data.templateName                    | String   | X        | テンプレート名                         |
| body.data.templateDesc                    | String   | X        | テンプレート説明                       |
| body.data.useYn                           | String   | X        | 使用するかどうか                       |
| body.data.priority                        | String   | X        | 優先順位値(未使用値)                   |
| body.data.sendNo                          | String   | X        | 発信番号                            |
| body.data.sendType                        | String   | X        | 送信タイプ(0:Sms, 1:Lms/Mms, 2:Auth) |
| body.data.sendTypeName                    | String   | X        | 送信タイプ名                          |
| body.data.title                           | String   | X        | タイトル                             |
| body.data.body                            | String   | X        | 本文内容                             |
| body.data.attachFileYn                    | String   | X        | 添付ファイルかどうか(Y/N)                |
| body.data.delYn                           | String   | X        | 削除Y/N、現在態表記用でのみ使用            |
| body.data.createDate                      | String   | X        | 登録日                              |
| body.data.createUser                      | String   | X        | 登録したユーザー                       |
| body.data.updateDate                      | String   | X        | 修正日                              |
| body.data.updateUser                      | String   | X        | 修正したユーザー                       |
| body.data[].attachFileList[].fileId       | Integer  | O        | ファイルID                           |
| body.data[].attachFileList[].filePath     | String   | X        | ファイル保存パス(内部用)                |
| body.data[].attachFileList[].filename     | String   | X        | ファイル名                           |
| body.data[].attachFileList[].saveFileName | String   | X        | 保存された添付ファイル名                 |
| body.data[].attachFileList[].uploadType   | String   | X        | アップロードタイプ                     |

<a id="modify-template"></a>

### テンプレート修正

<a id="request-32"></a>

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

<a id="curl-33"></a>

#### cURL

```
curl -X GET \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/templates/'"${TEMPLATE_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-30"></a>

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

| 値                    | タイプ     | Not Null | 説明         |
|----------------------|----------|----------|--------------|
| header               | Object   | O        | ヘッダ領域     |
| header.isSuccessful  | Boolean  | O        | 成否         |
| header.resultCode    | Integer  | O        | 失敗コード   |
| header.resultMessage | String   | O        | 失敗メッセージ |

<a id="delete-template"></a>

### テンプレート削除

<a id="request-33"></a>

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

<a id="curl-34"></a>

#### cURL

```
curl -X DELETE \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/templates/'"${TEMPLATE_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-31"></a>

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

| 値                    | タイプ     | Not Null | 説明         |
|----------------------|----------|----------|--------------|
| header               | Object   | O        | ヘッダ領域     |
| header.isSuccessful  | Boolean  | O        | 成否         |
| header.resultCode    | Integer  | O        | 失敗コード   |
| header.resultMessage | String   | O        | 失敗メッセージ |

<a id="toll-free-opt-out-service"></a>

## 080受信拒否サービス

<a id="retrieve-opt-out-list"></a>

### 受信拒否番号リスト検索

<a id="request-34"></a>

#### リクエスト

[URL]

```
GET /sms/v3.0/appKeys/{appKey}/blockservice/unsubscribe-nos
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値     | 	タイプ    | 	説明    |
|--------|---------|---------|
| appKey | 	String | 	固有のアプリキー |

[Header]

```json
{
  "X-Secret-Key": "{secret-key}"
}
```

| 値           | 	タイプ    | 	説明       |
|--------------|---------|------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

[Query parameter]

| 値                | 	タイプ      | 	最大長 | 必須  | 	説明                                |
|------------------|----------|--------|-----|------------------------------------|
| pageNum          | 	Integer | -      | 	オプション | 	ページ番号(デフォルト値：1)                    |
| pageSize         | 	Integer | 1000   | 	オプション | 	検索数(デフォルト値：15)                     |

<a id="curl-35"></a>

#### cURL

```
curl -X GET \
'https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/blockservice/unsubscribe-nos' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}' 
```

<a id="response-32"></a>

#### レスポンス

```
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
        "unsubscribeNo": "0808888888",
        "enterpriseName": "業者名",
        "requestDateTime": "2025-01-01 00:00:00",
        "startDateTime": "2025-01-01 00:00:00",
        "status": "RESERVE_USE/USED/TERMINATE/EXTERNAL_REGIST/EXTERNAL_TERMINATE",
        "shareType": "MASTER/SHARE"
      }
    ]
  }
}
```

| 値                                        | タイプ    | Not Null | 説明                               |
|-------------------------------------------|---------|----------|-------------------------------------|
| header                                    | Object  | O        | ヘッダ領域                            |
| header.isSuccessful                       | Boolean | O        | 成否                            |
| header.resultCode                         | Integer | O        | 失敗コード                            |
| header.resultMessage                      | String  | O        | 失敗メッセージ                          |
| body                                      | Object  | X        | 本文領域                           |
| body.pageNum                           | Integer | O        | 現在のページ番号                        |
| body.pageSize                          | Integer | O        | 検索されたデータ数                        |
| body.totalCount                        | Integer | O        | 総データ数                             |
| body.data.unsubscribeNo                   | String  | O        | 080受信拒否番号                    |
| body.data.enterpriseName                  | String  | O        | 業者名                               |
| body.data.requestDateTime                 | String  | O        | 080受信拒否番号申請日時             |
| body.data.startDateTime                   | String  | X        | 080受信拒否番号使用開始 日時         |
| body.data.status                       | String  | O        | 状態(RESERVE_USE：リクエスト / USED：使用中 / TERMINATE：削除 / EXTERNAL_REGIST：外部080番号)                        |
| body.data.shareType                    | String  | O        | 共有タイプ(MASTER：所有 / SHARE：共有)|

<a id="single-search-for-opt-out-number"></a>

### 受信拒否番号の単一検索

<a id="request-35"></a>

#### リクエスト

[URL]

```
GET /sms/v3.0/appKeys/{appKey}/blockservice/unsubscribe-nos/{unsubscribeNo}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 値     | 	タイプ    | 	説明    |
|--------|---------|---------|
| appKey | 	String | 	固有のアプリキー |
| unsubscribeNo | String | 080受信拒否番号(ハイフン除く) |

[Header]

```json
{
  "X-Secret-Key": "{secret-key}"
}
```

| 値           | 	タイプ    | 	説明       |
|--------------|---------|------------|
| X-Secret-Key | 	String | 	固有のシークレットキー |

<a id="curl-36"></a>

#### cURL

```
curl -X GET \
'https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/blockservice/unsubscribe-nos/{unsubscribeNo}' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key:{secretkey}' 
```
<a id="response-33"></a>

#### レスポンス

```
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
        "unsubscribeNo": "0808888888",
        "enterpriseName": "業者名",
        "requestDateTime": "2025-01-01 00:00:00",
        "startDateTime": "2025-01-01 00:00:00",
        "status": "RESERVE_USE/USED/TERMINATE/EXTERNAL_REGIST/EXTERNAL_TERMINATE",
        "shareType": "MASTER/SHARE"
      }
    ]
  }
}
```

| 値                                        | タイプ    | Not Null | 説明                               |
|-------------------------------------------|---------|----------|-------------------------------------|
| header                                    | Object  | O        | ヘッダ領域                            |
| header.isSuccessful                       | Boolean | O        | 成否                            |
| header.resultCode                         | Integer | O        | 失敗コード                            |
| header.resultMessage                      | String  | O        | 失敗メッセージ                          |
| body                                      | Object  | X        | 本文領域                           |
| body.data.unsubscribeNo                   | String  | O        | 080受信拒否番号                    |
| body.data.enterpriseName                  | String  | O        | 業者名                               |
| body.data.requestDateTime                 | String  | O        | 080受信拒否番号申請日時             |
| body.data.startDateTime                   | String  | X        | 080受信拒否番号使用開始 日時         |
| body.data.status                       | String  | O        | 状態(RESERVE_USE：リクエスト / USED：使用中 / TERMINATE：削除 / EXTERNAL_REGIST：外部080番号)                        |
| body.data.shareType                    | String  | O        | 共有タイプ(MASTER：所有 / SHARE：共有)|

<a id="register-unsubscribers"></a>

### 受信拒否対象者の登録

<a id="request-36"></a>

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

<a id="curl-37"></a>

#### cURL

```
curl -X POST \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/blockservice/recipients' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}'  \
-d '{
    "unsubscribeNo": "0800000000",
    "recipientNoList": ["0100000000", "0100000001"]
}'
```

<a id="response-34"></a>

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

| 値                    | タイプ     | Not Null | 説明         |
|----------------------|----------|----------|--------------|
| header               | Object   | O        | ヘッダ領域     |
| header.isSuccessful  | Boolean  | O        | 成否         |
| header.resultCode    | Integer  | O        | 失敗コード   |
| header.resultMessage | String   | O        | 失敗メッセージ |

<a id="query-target-of-rejection"></a>

### 受信拒否対象者の検索

<a id="request-37"></a>

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
| unsubscribeNo    | 	String  | 25    | 	オプション    | 	080受信拒否番号                         |
| recipientNo      | String   | 25    | オプション  | 受信拒否対象者番号                          |
| startRequestDate | 	String  | -     | 	オプション | 	受信拒否リクエスト開始値(yyyy-MM-dd HH:mm:ss) |
| endRequestDate   | 	String  | -     | 	オプション | 	受信拒否リクエスト終了値(yyyy-MM-dd HH:mm:ss) |
| pageNum          | 	Integer | -     | 	オプション | 	ページ番号(デフォルト値：1)                   |
| pageSize         | 	Integer | 1000  | 	オプション | 	検索数(デフォルト値：15)                    |

<a id="curl-38"></a>

#### cURL

```
curl -X GET \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/blockservice/recipients' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-35"></a>

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

<a id="delete-target-of-rejection"></a>

### 受信拒否対象者の削除

<a id="request-38"></a>

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

<a id="curl-39"></a>

#### cURL

```
curl -X DELETE \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/blockservice/recipients/removes?unsubscribeNo='"${UNSUB_NO}"'&updateUser='"${UPDATE_USER}"'&recipientNoList='"${RECIPIENT_NO}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-36"></a>

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

| 値                    | タイプ     | Not Null | 説明         |
|----------------------|----------|----------|--------------|
| header               | Object   | O        | ヘッダ領域     |
| header.isSuccessful  | Boolean  | O        | 成否         |
| header.resultCode    | Integer  | O        | 失敗コード   |
| header.resultMessage | String   | O        | 失敗メッセージ |

<a id="sender-numbers"></a>

## 発信番号

<a id="list-registered-sender-numbers-api"></a>

### 登録された発信番号リスト検索API

<a id="request-39"></a>

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

<a id="curl-40"></a>

#### cURL

```
curl -X GET \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/sendNos' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-37"></a>

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

| 値                       | タイプ     | Not Null | 説明            |
|-------------------------|----------|----------|----------------|
| header                  | Object   | O        | ヘッダ領域        |
| header.isSuccessful     | Boolean  | O        | 成否            |
| header.resultCode       | Integer  | O        | 失敗コード        |
| header.resultMessage    | String   | O        | 失敗メッセージ      |
| body                    | Object   | X        | 本文領域          |
| body.pageNum            | Integer  | O        | ページ番号        |
| body.pageSize           | Integer  | O        | 検索されたデータ数  |
| body.totalCount         | Integer  | O        | データの総数       |
| body.data[].serviceId   | Integer  | O        | サービスID        |
| body.data[].sendNo      | String   | O        | 発信番号         |
| body.data[].useYn       | String   | X        | 使用するかどうか    |
| body.data[].blockYn     | String   | X        | ブロックするかどうか |
| body.data[].blockReason | String   | X        | ブロック理由       |
| body.data[].createDate  | String   | X        | 作成日時         |
| body.data[].createUser  | String   | X        | 作成者          |
| body.data[].updateDate  | String   | X        | 修正日          |
| body.data[].updateUser  | String   | X        | 修正したユーザー   |

<a id="query-statistics"></a>

## 統計

<a id="search-statistics---based-on-events"></a>

### 統計検索 - イベント基盤

* イベント発生時間を基準に収集された統計です。
* 次の時間を基準に統計が収集されます。
    * リクエスト数(requested)：送信リクエスト時間
    * 送信数(sent)：通信事業者(ベンダー)に送信リクエストした時間
    * 成功数(received)：実際の端末受信時間
    * 失敗数(sentFailed)：失敗レスポンスが発生した時間

<a id="request-40"></a>

#### リクエスト

[URL]

| Http method | URI                              |
|-------------|----------------------------------|
| GET         | /sms/v3.0/appKeys/{appKey}/stats |

[Path parameter]

| 値     | タイプ    | 説明    |
|--------|--------|--------|
| appKey | String | 固有のアプリケーションキー |

[Header]

```json
{
  "X-Secret-Key": "{secret-key}"
}
```

| 値           | タイプ    | 説明       |
|--------------|--------|-----------|
| X-Secret-Key | String | 固有のシークレットキー |

[Query parameter]

| 値             | タイプ          | 最大長さ | 必須 | 説明                                                                |
|----------------|--------------|-------|----|--------------------------------------------------------------------|
| statisticsType | String       | -     | 必須 | 統計区分<br/>NORMAL:基本、 MINUTELY:分別, HOURLY:時間別、 DAILY:日別、 BY_DAY:曜日別 |
| from           | String       | -     | 必須 | 統計検索開始日<br/>yyyy-MM-dd HH:mm:ss                                | 
| to             | String       | -     | 必須 | 統計検索終了日<br/>yyyy-MM-dd HH:mm:ss                                |
| statsIds       | List<String> | -     | オプション | 統計IDリスト                                                          |
| messageType    | String       | -     | オプション | メッセージタイプ<br/>SMS, LMS, MMS, AUTH                                     |
| isAd           | Boolean      | -     | オプション | 広告かどうか<br/>true/false                                               |
| templateIds    | List<String> | -     | オプション | テンプレートIDリスト                                                         |
| requestIds     | List<String> | 5     | オプション | リクエストIDリスト                                                          |

<a id="curl-41"></a>

#### cURL

```
curl -X GET \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/stats?statisticsType='"${STATISTICS_TYPE}"'&from='"${FROM}"'&to='"${TO}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-38"></a>

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
          "requested": 10,
          "sent": 10,
          "sentFailed" : 0,
          "received": 0
        }
      }
    ]
  }
}
```

| 値                   | タイプ    | Not Null | 説明                    |
|----------------------|---------|----------|------------------------|
| header               | Object  | O        | ヘッダ領域                |
| header.isSuccessful  | Boolean | O        | 成否                    |
| header.resultCode    | Integer | O        | 失敗コード                |
| header.resultMessage | String  | O        | 失敗メッセージ             |
| body                 | Object  | X        | 本文領域                 |
| body.data            | List    | O        | 統計イベントオブジェクトリスト |

<a id="statistical-event-objects"></a>

#### 統計イベントオブジェクト
| 値                | タイプ    | Not Null | 説明                              |
|-------------------|---------|----------|-----------------------------------|
| eventDateTime     | String  | O        | 表示名<br/>分別、時間別、曜日別、月別 |
| events            | Object  | O        | 統計値オブジェクト                |
| events.requested  | Integer | O        | リクエスト数                      |
| events.sent       | Integer | O        | 送信数                            |
| events.sentFailed | Integer | O        | 失敗数                            |
| events.received   | Integer | O        | 成功数                            |

<a id="statistics-search---based-on-request-time"></a>

### 統計検索 - リクエスト時間ベース

* 送信リクエスト時間基準で収集された統計です。
* 次の時間を基準に統計が収集されます。
    * リクエスト数(requested)：送信リクエスト時間
    * 送信数(sent)：送信リクエスト時間で、数が増加する時点は通信事業者(ベンダー)に送信リクエストした時間
    * 成功数(received)：送信リクエスト時間で、数が増加する時点は、実際の端末受信時間
    * 失敗数(sentFailed)：送信リクエスト時間で、数が増加する時点は失敗レスポンスが発生した時間

<a id="request-41"></a>

#### リクエスト

[URL]

| Http method | URI                                     |
|-------------|-----------------------------------------|
| GET         | /sms/v3.0/appKeys/{appKey}/stats/legacy |

[Path parameter]

| 値     | タイプ    | 説明    |
|--------|--------|--------|
| appKey | String | 固有のアプリケーションキー |

[Header]

```json
{
  "X-Secret-Key": "{secret-key}"
}
```

| 値           | タイプ    | 説明       |
|--------------|--------|-----------|
| X-Secret-Key | String | 固有のシークレットキー |

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

<a id="response-39"></a>

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
          "requested": 10,
          "sent": 10,
          "sentFailed": 0,
          "received": 0,
          "pending": 0
        }
      }
    ]
  }
}
```

| 値                   | タイプ    | Not Null | 説明                      |
|----------------------|---------|----------|--------------------------|
| header               | Object  | O        | ヘッダ領域                |
| header.isSuccessful  | Boolean | O        | 成否                    |
| header.resultCode    | Integer | O        | 失敗コード               |
| header.resultMessage | String  | O        | 失敗メッセージ            |
| body                 | Object  | X        | 本文領域                 |
| body.data            | List    | O        | 統計イベントオブジェクトリスト |

<a id="statistical-event-objects-2"></a>

#### 統計イベントオブジェクト
| 値                | タイプ    | Not Null | 説明                          |
|-------------------|---------|----------|-------------------------------|
| eventDateTime     | String  | O        | 表示名<br/>分別、時間別、曜日別、月別 |
| events            | Object  | O        | 統計値オブジェクト            |
| events.requested  | Integer | O        | リクエスト数                  |
| events.sent       | Integer | O        | 送信数                        |
| events.sentFailed | Integer | O        | 失敗数                        |
| events.received   | Integer | O        | 成功数                        |
| events.pending    | Integer | O        | 送信中の数                    |

<a id="statistic-search---international-send"></a>

### 統計検索 - 国際送信

* イベント発生時間基準で収集された統計です。
* 次の時間基準で統計が収集されます。
    * リクエスト数(requested):送信リクエスト時間
    * 送信数(sent):通信事業者(ベンダー)に送信リクエストした時間
    * 送信失敗数(sentFailed):失敗レスポンスが発生した時間
    * 受信数(received):送信されたメッセージの受信時間
    * 送信数(concat):単件またはConcatenated message(接続)機能により送信されたメッセージの受信時間
    * コンバージョン待機数(ready):コンバージョン率収集リクエスト送信件のメッセージ受信時間
    * コンバージョン完了数(converted):コンバージョン率収集リクエスト送信件がコンバージョン完了した時間

<a id="request-42"></a>

#### リクエスト

[URL]

| Http method | URI                                            |
|-------------|------------------------------------------------|
| GET         | /sms/v3.0/appKeys/{appKey}/stats/international |

[Path parameter]

| 値     | タイプ    | 説明    |
|--------|--------|--------|
| appKey | String | 固有のアプリケーションキー |

[Header]

```json
{
  "X-Secret-Key": "{secret-key}"
}
```

| 値           | タイプ    | 説明       |
|--------------|--------|-----------|
| X-Secret-Key | String | 固有のシークレットキー |

[Query parameter]

| 値            | 	タイプ        | 	最大長さ | 必須 | 説明                                                                                                    |
|----------------|--------------|--------|----|---------------------------------------------------------------------------------------------------------|
| statisticsType | String       | -      | 必須 | 統計区分<br/>NORMAL:基本、MINUTELY:分別、HOURLY:時間別、DAILY:日別、BY_DAY:曜日別                                    |
| from           | String       | -      | 必須 | 統計検索開始日<br/>yyyy-MM-dd HH:mm:ss                                                                     | 
| to             | String       | -      | 必須 | 統計検索終了日<br/>yyyy-MM-dd HH:mm:ss                                                                     |
| statsIds       | List<String> | -      | オプション | 統計IDリスト                                                                                              |
| messageType    | String       | -      | オプション | メッセージタイプ<br/>SMS, LMS, MMS, AUTH                                                                          |
| countryCode    | String       | -      | オプション | 国コード                                                                                                 |
| requestIds     | List<String> | 5      | オプション | リクエストIDリスト                                                                                              |
| statsCriteria  | List<String> | -      | オプション | 統計基準<br/>- EVENT:イベント(デフォルト値)<br/>- MESSAGE_TYPE,EVENT:メッセージタイプ、イベント<br/>- COUNTRY_CODE,EVENT:国コード、イベント |

<a id="response-statistics-criteria-default-value"></a>

#### レスポンス(統計基準:デフォルト値)

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
          "REQUESTED": 10,
          "SENT": 10,
          "SENT_FAILED": 0,
          "RECEIVED": 10,
          "CONCAT": 20,
          "READY": 5,
          "CONVERTED": 3
        }
      }
    ]
  }
}
```

| 値                    | タイプ    | Not Null | 説明                  |
|----------------------|---------|----------|-----------------------|
| header               | Object  | O        | ヘッダ領域               |
| header.isSuccessful  | Boolean | O        | 成否                   |
| header.resultCode    | Integer | O        | 失敗コード               |
| header.resultMessage | String  | O        | 失敗メッセージ            |
| body                 | Object  | X        | 本文領域                |
| body.data            | List    | O        | 統計イベントオブジェクトリスト |

<a id="statistical-event-objects-statistics-criteria-default-value"></a>

#### 統計イベントオブジェクト(統計基準：デフォルト値)
| 値                | タイプ    | Not Null | 説明                                                           |
|-------------------|---------|----------|----------------------------------------------------------------|
| eventDateTime      | String  | O        | 表示名<br/>分別、時間別、曜日別、月別                            |
| events             | Object  | O        | statsCriteriaをEVENTにのみ設定した場合、{statsCriteriaValue}は省略されます |
| events.REQUESTED   | Integer | O        | リクエスト数                                                   |
| events.SENT        | Integer | O        | 送信数                                                         |
| events.SENT_FAILED | Integer | O        | 失敗数                                                         |
| events.RECEIVED    | Integer | O        | 受信数                                                         |
| events.CONCAT      | Integer | O        | 実受信成功数                                                   |
| events.READY       | Integer | O        | コンバージョン率収集リクエストの送信成功数                     |
| events.CONVERTED   | Integer | O        | コンバージョン数                                               |

<a id="response-statistics-criteria-added"></a>

#### レスポンス(統計基準追加)

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
          "{statsCriteriaValue}": {
            "REQUESTED": 10,
            "SENT": 10,
            "SENT_FAILED": 0,
            "RECEIVED": 10,
            "CONCAT": 10,
            "READY": 5,
            "CONVERTED": 3
          },
          "{statsCriteriaValue}": {
            "REQUESTED": 10,
            "SENT": 10,
            "SENT_FAILED": 0,
            "RECEIVED": 10,
            "CONCAT": 20,
            "READY": 5,
            "CONVERTED": 3
          }
        }
      }
    ]
  }
}
```

| 値                    | タイプ    | Not Null | 説明                   |
|----------------------|---------|----------|------------------------|
| header               | Object  | O        | ヘッダ領域                |
| header.isSuccessful  | Boolean | O        | 成否                    |
| header.resultCode    | Integer | O        | 失敗コード                |
| header.resultMessage | String  | O        | 失敗メッセージ             |
| body                 | Object  | X        | 本文領域                 |
| body.data            | List    | O        | 統計イベントオブジェクトリスト |

<a id="statistical-event-objects-statistics-criteria-added"></a>

#### 統計イベントオブジェクト(統計基準追加)
| 値                                     | タイプ    | Not Null | 説明                                                |
|----------------------------------------|---------|----------|-----------------------------------------------------|
| eventDateTime                           | String  | O        | 表示名前<br/>分別、時間別、曜日別、月別              |
| events.{statsCriteriaValue}             | Object  | O        | statsCriteriaに該当する値<br/>国コード値が入ることがある |
| events.{statsCriteriaValue}.REQUESTED   | Integer | O        | リクエスト数                                        |
| events.{statsCriteriaValue}.SENT        | Integer | O        | 送信数                                              |
| events.{statsCriteriaValue}.SENT_FAILED | Integer | O        | 失敗数                                              |
| events.{statsCriteriaValue}.RECEIVED    | Integer | O        | 受信数                                              |
| events.{statsCriteriaValue}.CONCAT      | Integer | O        | 実受信成功数                                        |
| events.{statsCriteriaValue}.READY       | Integer | O        | コンバージョン率収集リクエストの送信成功数           |
| events.{statsCriteriaValue}.CONVERTED   | Integer | O        | コンバージョン数                                    |

<a id="oldquery-integrated-statistics"></a>

### (旧)統合統計検索

<a id="request-43"></a>

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

<a id="response-40"></a>

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

| 値                          | タイプ    | Not Null | 説明               |
|----------------------------|---------|----------|--------------------|
| header                     | Object  | O        | ヘッダ領域            |
| header.isSuccessful        | Boolean | O        | 成否                |
| header.resultCode          | Integer | O        | 失敗コード            |
| header.resultMessage       | String  | O        | 失敗メッセージ         |
| body                       | Object  | X        | 本文領域              |
| body.data[].divisionName   | String  | X        | 表示名<br/>日、時間、曜日 |
| body.data[].statisticsView | Object  | X        |                     |
| body.data[].requestedCount | Integer | X        | リクエスト数           |
| body.data[].succeedCount   | Integer | X        | 成功数               |
| body.data[].failedCount    | Integer | X        | 失敗数               |
| body.data[].pendingCount   | Integer | X        | 送信中の数            |
| body.data[].succeedRate    | String  | X        | 成功比率              |
| body.data[].failedRate     | String  | X        | 失敗比率              |
| body.data[].pendingRate    | String  | X        | 送信中の比率           |

<a id="scheduled-delivery"></a>

## 予約送信

<a id="list-scheduled-delivery"></a>

### 予約送信リスト検索

<a id="request-44"></a>

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
|------------------|----------|--------|-----|------------------------------------------------------------------------------------------------------------------------------------------------------|
| sendType         | String   | 1     | オプション  | 送信タイプ<br/>(0：SMS、1：LMS/MMS、2：AUTH)                                                                    |
| requestId        | 	String  | 25    | 	オプション | 	リクエストID                                                                                              |
| startRequestDate | 	String  | -     | 	オプション | 	送信日開始値(yyyy-MM-dd HH:mm:ss)                                                                          |
| endRequestDate   | 	String  | -     | 	オプション | 	送信日終了値(yyyy-MM-dd HH:mm:ss)                                                                          |
| startCreateDate  | 	String  | -     | 	オプション | 	登録日開始値(yyyy-MM-dd HH:mm:ss)                                                                          |
| endCreateDate    | 	String  | -     | 	オプション | 	登録日終了値(yyyy-MM-dd HH:mm:ss)                                                                          |
| sendNo           | 	String  | 13    | オプション  | 	発信番号                                                                                                 |
| recipientNo      | 	String  | 20    | 	オプション | 	受信番号                                                                                                 |
| templateId       | 	String  | 50    | 	オプション | 	テンプレート番号                                                                                             |
| messageStatus    | 	String  | 10    | 	オプション | 	メッセージのステータス<br/>(RESERVED：予約待機、SENDING：送信中、COMPLETED：送信完了、FAILED：送信失敗、CANCEL：キャンセル、DUPLICATED：重複送信、FAILED_AD:失敗(広告制限)、再送信待機(広告制限)) |
| receiverRegion   | 	String  | -     | 	オプション | 	国内/国際 (DOMESTIC: 国内, INTERNATIONAL: 国際) |
| countryCode      | 	String  | -     | 	オプション | 	国コード [[送信可能国](./international-sending-policy/#_5)] |
| pageNum          | 	Integer | -     | 	オプション | 	ページ番号(デフォルト値：1)                                                                                      |
| pageSize         | 	Integer | 1000  | 	オプション | 	検索数(デフォルト値：15)                                                                                       |

<a id="curl-42"></a>

#### cURL

```
curl -X GET \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/reservations' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-41"></a>

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

| 値                             | タイプ         | Not Null | 説明                                                                                                |
|-------------------------------|---------------|----------|----------------------------------------------------------------------------------------------------|
| header                        | Object        | O        | ヘッダ領域                                                                                            |
| header.isSuccessful           | Boolean       | O        | 成否                                                                                                |
| header.resultCode             | Integer       | O        | 失敗コード                                                                                            |
| header.resultMessage          | String        | O        | 失敗メッセージ                                                                                         |
| body                          | Object        | X        | 本文領域                                                                                             |
| body.pageNum                  | Integer       | O        | 現在のページ番号                                                                                       |
| body.pageSize                 | Integer       | O        | 検索されたデータ数                                                                                     |
| body.totalCount               | Integer       | O        | データの総数                                                                                         |
| body.data[].requestId         | String        | O        | リクエストID                                                                                         |
| body.data[].recipientSeq      | Integer       | O        | 受信者シーケンス                                                                                       |
| body.data[].requestDate       | String        | O        | 発信日時                                                                                            |
| body.data[].sendNo            | String        | O        | 発信番号                                                                                            |
| body.data[].recipientNo       | String        | O        | 受信番号                                                                                            |
| body.data[].countryCode       | String        | O        | 国番号                                                                                             |
| body.data[].sendType          | String        | O        | 送信タイプ(0:Sms, 1:Lms/Mms, 2:Auth)                                                                 |
| body.data[].messageType       | String        | O        | メッセージタイプ<br/>(SMS,LMS,MMS,AUTH)                                                                |
| body.data[].adYn              | String        | O        | 広告かどうか                                                                                          |
| body.data[].templateId        | String        | X        | テンプレートID                                                                                        |
| body.data[].templateParameter | String(json)  | X        | テンプレートパラメータ                                                                                   |
| body.data[].templateName      | String        | X        | テンプレート名                                                                                         |
| body.data[].title             | String        | X        | タイトル                                                                                             |
| body.data[].body              | String        | O        | 本文内容                                                                                             |
| body.data[].messageStatus     | String        | O        | メッセージのステータス<br/>(RESERVED：予約待機、SENDING：送信中、COMPLETED：送信完了、FAILED：送信失敗、CANCEL：キャンセル、DUPLICATED：重複送信、FAILED_AD:失敗(広告制限)、再送信待機(広告制限)) |
| body.data[].createUser        | String        | X        | 登録したユーザー                                                                                       |
| body.data[].createDate        | String        | O        | 登録日                                                                                               |
| body.data[].updateDate        | String        | X        | 修正日                                                                                               |

<a id="query-detail-scheduled-delivery"></a>

### 予約送信詳細検索

<a id="request-45"></a>

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

<a id="curl-43"></a>

#### cURL

```
curl -X GET \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/reservations/'"${R_ID}"'/'"${R_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-42"></a>

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

| 値                                         | タイプ        | Not Null | 説明                                                                                                  |
|-------------------------------------------|-------------|----------|-------------------------------------------------------------------------------------------------------|
| header                                    | Object      | O        | ヘッダ領域                                                                                               |
| header.isSuccessful                       | Boolean     | O        | 成否                                                                                                   |
| header.resultCode                         | Integer     | O        | 失敗コード                                                                                               |
| header.resultMessage                      | String      | O        | 失敗メッセージ                                                                                            |
| body                                      | Object      | X        | 本文領域                                                                                                |
| body.pageNum                              | Integer     | O        | 現在のページ番号                                                                                          |
| body.pageSize                             | Integer     | O        | 検索されたデータ数                                                                                        |
| body.totalCount                           | Integer     | O        | データの総数                                                                                             |
| body.data.requestId                       | String      | O        | リクエストID                                                                                             |
| body.data.recipientSeq                    | Integer     | O        | 受信者シーケンス                                                                                          |
| body.data.requestDate                     | String      | O        | 発信日時                                                                                                |
| body.data.sendNo                          | String      | O        | 発信番号                                                                                                |
| body.data.recipientNo                     | String      | O        | 受信番号                                                                                                |
| body.data.countryCode                     | String      | O        | 国番号                                                                                                 |
| body.data.sendType                        | String      | O        | 送信タイプ(0:Sms, 1:Lms/Mms, 2:Auth)                                                                     |
| body.data.messageType                     | String      | O        | メッセージタイプ<br/>(SMS、LMS、MMS、AUTH)                                                                    |
| body.data.adYn                            | String      | O        | 広告かどうか                                                                                              |
| body.data.templateId                      | String      | X        | テンプレートID                                                                                            |
| body.data.templateParameter               | String(json) | X        | テンプレートパラメータ                                                                                      |
| body.data.templateName                    | String      | X        | テンプレート名                                                                                             |
| body.data.title                           | String      | X        | タイトル                                                                                                |
| body.data.body                            | String      | O        | 本文内容                                                                                                |
| body.data.messageStatus                   | String      | O        | メッセージのステータス<br/>(RESERVED：予約待機、SENDING：送信中、COMPLETED：送信完了、FAILED：送信失敗、CANCEL：キャンセル、DUPLICATED：重複送信、FAILED_AD:失敗(広告制限)、再送信待機(広告制限)) |
| body.data.createUser                      | String      | X        | 登録したユーザー                                                                                           |
| body.data.createDate                      | String      | X        | 登録日                                                                                                  |
| body.data[].attachFileList[].fileId       | Integer     | O        | ファイルID                                                                                               |
| body.data[].attachFileList[].filePath     | String      | X        | ファイル保存パス(内部用)                                                                                     |
| body.data[].attachFileList[].filename     | String      | X        | ファイル名                                                                                                |
| body.data[].attachFileList[].saveFileName | String      | X        | 保存された添付ファイル名                                                                                      |
| body.data[].attachFileList[].uploadType   | String      | X        | アップロードタイプ                                                                                           |

<a id="cancel-scheduled-delivery"></a>

### 予約送信キャンセル

<a id="request-46"></a>

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

<a id="curl-44"></a>

#### cURL

```
curl -X PUT \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/reservations/cancel' \
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

<a id="response-43"></a>

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

| 値                        | タイプ    | Not Null | 説明               |
|--------------------------|---------|----------|--------------------|
| header                   | Object  | O        | ヘッダ領域            |
| header.isSuccessful      | Boolean | O        | 成否                |
| header.resultCode        | Integer | O        | 失敗コード            |
| header.resultMessage     | String  | O        | 失敗メッセージ         |
| body                     | Object  | X        | 本文領域             |
| body.data.requestedCount | Integer | O        | キャンセルリクエスト件数 |
| body.data.canceledCount  | Integer | O        | キャンセル成功件数      |

<a id="cancel-scheduled-delivery---multiple-filter"></a>

### 予約送信キャンセル - 多重フィルタ

<a id="request-47"></a>

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
    "senderGroupingKey": "SenderGroupingKey",
    "receiverRegion": "DOMESTIC",
    "countryCode": "82"
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
| searchParameter.receiverRegion       | String | -     | オプション | 国内/国際(DOMESTIC：国内、INTERNATIONAL：国際) |
| searchParameter.countryCode          | String | -     | オプション | 国コード [[送信可能国](./international-sending-policy/#_5)] |
| updateUser                           | String | 100   | 必須    | 予約キャンセルリクエスト者                  |

<a id="curl-45"></a>

#### cURL

```
curl -X PUT \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/reservations/search-cancels' \
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
        "senderGroupingKey": "SenderGroupingKey",
        "receiverRegion": "DOMESTIC",
        "countryCode": "82"
    },
    "updateUser": "API Guide"
}'
```

<a id="response-44"></a>

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

| 値                                 | タイプ    | Not Null | 説明                                                                                                        |
|------------------------------------|--------|----------|-------------------------------------------------------------------------------------------------------------|
| header                            | Object  | O        | ヘッダ領域                                                                                                     |
| header.isSuccessful               | Boolean | O        | 成否                                                                                                         |
| header.resultCode                 | Integer | O        | 失敗コード                                                                                                     |
| header.resultMessage              | String  | O        | 失敗メッセージ                                                                                                  |
| body                              | Object  | X        | 本文領域                                                                                                      |
| body.data.reservationCancelId     | Integer | O        | 予約キャンセルID                                                                                                |
| body.data.requestedDateTime       | String  | O        | 予約キャンセル時間(yyyy-MM-dd HH:mm:ss)                                                                          |
| body.data.reservationCancelStatus | String  | O        | 予約キャンセル状態<br/>- READY ：予約準備<br/>- PROCESSING ：予約キャンセル中<br/>- COMPLETED ：予約キャンセル完了<br/>- FAILED ：予約キャンセル失敗 |

<a id="list-request-of-scheduled-delivery-cancellation---multiple-filter"></a>

### 予約送信キャンセルリクエストリスト検索 - 多重フィルタ

<a id="request-48"></a>

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
|------------------------|----------|-------|--------|--------------------------------------|
| startRequestedDateTime | String   | -     | オプション  | 予約キャンセルリクエスト開始時間(yyyy-MM-dd HH:mm:ss)  |
| endRequestedDateTime   | 	String  | -     | 	オプション | 	予約キャンセルリクエスト終了時間(yyyy-MM-dd HH:mm:ss) |
| reservationCancelId    | 	String  | 25    | 	オプション | 予約キャンセルID                              |
| pageNum                | 	Integer | -     | 	オプション | 	ページ番号(デフォルト値：1)                       |
| pageSize               | 	Integer | 1000  | 	オプション | 	検索数(デフォルト値：15)                        |

<a id="curl-46"></a>

#### cURL

```
curl -X GET \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/reservations/search-cancels' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-45"></a>

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

| 値                                   | タイプ                | Not Null | 説明                                                                                                        |
|-------------------------------------|---------------------|----------|-------------------------------------------------------------------------------------------------------------|
| header                              | Object              | O        | ヘッダ領域                                                                                                     |
| header.isSuccessful                 | Boolean             | O        | 成否                                                                                                         |
| header.resultCode                   | Integer             | O        | 失敗コード                                                                                                     |
| header.resultMessage                | String              | O        | 失敗メッセージ                                                                                                  |
| body                                | Object              | X        | 本文領域                                                                                                      |
| body.data[].reservationCancelId     | String              | O        | 予約キャンセルID                                                                                                |
| body.data[].searchParameter         | Map<String, Object> | O        | 予約キャンセルリクエストパラメータ                                                                                    |
| body.data[].requestedDateTime       | String              | O        | 予約キャンセルリクエスト時間                                                                                        |
| body.data[].completedDateTime       | String              | X        | 予約キャンセル完了時間                                                                                            |
| body.data[].reservationCancelStatus | String              | X        | 予約キャンセル状態<br/>- READY ：予約準備<br/>- PROCESSING ：予約キャンセル中<br/>- COMPLETED ：予約キャンセル完了<br/>- FAILED ：予約キャンセル失敗 |
| body.data[].totalCount              | Integer             | X        | 予約キャンセル対象件数                                                                                            |
| body.data[].successCount            | Integer             | X        | 予約キャンセル成功件数                                                                                            |
| body.data[].createUser              | String              | X        | 予約キャンセルリクエスト者                                                                                         |
| body.data[].createdDateTime         | String              | X        | 予約キャンセルリクエスト作成時間                                                                                     |
| body.data[].updatedDateTime         | String              | X        | 予約キャンセル修正時間                                                                                            |

<a id="download-delivery-result-files"></a>

## 送信結果ファイルダウンロード

<a id="request-for-creating-query-files"></a>

### 検索ファイル作成リクエスト

<a id="request-49"></a>

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
| msgStatus             | 	String | 1     | 	オプション | メッセージステータスコード(0：失敗、 1：リクエスト、 2：処理中、 3：成功、 4：予約キャンセル、 5：重複失敗、 6:失敗(広告制限)、 再送信待機(広告制限)) |
| resultCode            | 	String | 10    | 	オプション | 	受信結果コード[[検索コード表](./error-code/#_2)]                         |
| subResultCode         | 	String | 10    | 	オプション | 	受信結果詳細コード[[検索コード表](./error-code/#_3)]                       |
| senderGroupingKey     | 	String | 100   | 	オプション | 	送信者グループキー                                                   |
| recipientGroupingKey  | 	String | 100   | 	オプション | 	受信者グループキー                                                   |
| isIncludeTitleAndBody | Boolean | -     | オプション  | タイトル、本文を含むかどうか                                               |

<a id="curl-47"></a>

#### cURL

```
curl -X POST \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/sender/download-reservations' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}'  \
-d '{
    "sendType": "1",
    "startRequestDate": "2020-08-01T00:00:00",
    "endRequestDate": "2020-08-08T00:00:00"
}'
```

<a id="response-46"></a>

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

| 値                            | タイプ    | Not Null | 説明                                                                                                        |
|------------------------------|---------|----------|-------------------------------------------------------------------------------------------------------------|
| header                       | Object  | O        | ヘッダ領域                                                                                                     |
| header.isSuccessful          | Boolean | O        | 成否                                                                                                         |
| header.resultCode            | Integer | O        | 失敗コード                                                                                                     |
| header.resultMessage         | String  | O        | 失敗メッセージ                                                                                                  |
| body                         | Object  | X        | 本文領域                                                                                                      |
| body.data.downloadId         | String  | O        | ダウンロードID                                                                                                 |
| body.data.downloadType       | String  | O        | ダウンロードタイプ<br/>- BLOCK：受信拒否<br/>- NORMAL：一般送信<br/>- MASS：大量送信<br/>- TAG：タグ送信                    |
| body.data.fileType           | String  | X        | ファイルタイプ(現在csvのみサポート)                                                                                 |
| body.data.downloadStatusCode | String  | O        | ファイル作成状態<br/>- READY：作成準備<br/>- MAKING：作成中<br/>- COMPLETED：作成完了<br/>- FAILED：作成失敗<br/>- EXPIRED：ダウンロード期間終了 |
| body.data.expiredDate        | String  | X        | ダウンロード期間終了日時                                                                                          |

<a id="query-request-history-for-delivery-result-of-file-creation"></a>

### 送信結果ファイル作成リクエスト内訳検索

<a id="request-50"></a>

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

<a id="curl-48"></a>

#### cURL

```
curl -X GET \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/download-reservations' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-47"></a>

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

| 値                              | タイプ    | Not Null | 説明                                                                                                        |
|--------------------------------|---------|----------|-------------------------------------------------------------------------------------------------------------|
| header                         | Object  | O        | ヘッダ領域                                                                                                     |
| header.isSuccessful            | Boolean | O        | 成否                                                                                                         |
| header.resultCode              | Integer | O        | 失敗コード                                                                                                     |
| header.resultMessage           | String  | O        | 失敗メッセージ                                                                                                  |
| body                           | Object  | X        | 本文領域                                                                                                      |
| body.totalCount                | Integer | O        | 全体件数                                                                                                     |
| body.data[].downloadId         | String  | O        | ダウンロードID                                                                                                 |
| body.data[].downloadType       | String  | O        | ダウンロードタイプ<br/>- BLOCK：受信拒否<br/>- NORMAL：一般送信<br/>- MASS：大量送信<br/>- TAG：タグ送信                    |
| body.data[].fileType           | String  | X        | ファイルタイプ                                                                                                  |
| body.data[].parameter          | String  | X        | リクエストパラメータ                                                                                             |
| body.data[].size               | Integer | X        | 検索データサイズ                                                                                                |
| body.data[].downloadStatusCode | String  | O        | ファイル作成状態<br/>- READY：作成準備<br/>- MAKING：作成中<br/>- COMPLETED：作成完了<br/>- FAILED：作成失敗<br/>- EXPIRED：ダウンロード期間終了 |
| body.data[].resultMessage      | String  | X        | 結果メッセージ(失敗時レスポンス)                                                                                    |
| body.data[].expiredDate        | String  | X        | ファイル終了日時                                                                                                |
| body.data[].createUser         | String  | O        | ファイル作成リクエスト者                                                                                          |
| body.data[].createDate         | String  | O        | ファイル作成リクエスト日時                                                                                         |
| body.data[].updateDate         | String  | X        | ファイル作成完了、失敗日時                                                                                         |

<a id="request-for-downloading-delivery-result-files"></a>

### 送信結果ファイルダウンロードリクエスト

<a id="request-51"></a>

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

<a id="curl-49"></a>

#### cURL

```
curl -X GET \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/download-reservations/'"${DOWNLOAD_RESERVATION_ID}"'/download' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-48"></a>

#### レスポンス

```
file byte
```

<a id="tag-management"></a>

## タグ管理

<a id="query-tags"></a>

### タグ検索

<a id="request-52"></a>

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

<a id="curl-50"></a>

#### cURL

```
curl -X GET \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/tags' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-49"></a>

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

| 値                       | タイプ    | Not Null | 説明           |
|-------------------------|---------|----------|----------------|
| header                  | Object  | O        | ヘッダ領域        |
| header.isSuccessful     | Boolean | O        | 成否            |
| header.resultCode       | Integer | O        | 失敗コード       |
| header.resultMessage    | String  | O        | 失敗メッセージ    |
| body                    | Object  | X        | 本文領域         |
| body.pageNum            | Integer | O        | 現在のページ番号   |
| body.pageSize           | Integer | O        | 検索されたデータ数 |
| body.totalCount         | Integer | O        | データの総数     |
| body.data[].tagId       | String  | O        | タグID         |
| body.data[].tagName     | String  | X        | タグ名          |
| body.data[].createdDate | String  | O        | 作成日時        |
| body.data[].updatedDate | String  | O        | 修正日時        |

<a id="register-tags"></a>

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

<a id="curl-51"></a>

#### cURL

```
curl -X POST \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/tags' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}'  \
-d '{
    "tagName": "API-Guide"
}'
```

<a id="response-50"></a>

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

| 値                    | タイプ    | Not Null | 説明       |
|----------------------|---------|----------|------------|
| header               | Object  | O        | ヘッダ領域    |
| header.isSuccessful  | Boolean | O        | 成否         |
| header.resultCode    | Integer | O        | 失敗コード    |
| header.resultMessage | String  | O        | 失敗メッセージ |
| body                 | Object  | X        | 本文領域      |
| body.data.tagId      | String  | O        | タグID       |

<a id="modify-tags"></a>

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

<a id="curl-52"></a>

#### cURL

```
curl -X PUT \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/tags/'"${TAG_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}'  \
-d '{
    "tagName": "API-Guide2"
}'
```

<a id="response-51"></a>

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

| 値                    | タイプ     | Not Null | 説明         |
|----------------------|----------|----------|--------------|
| header               | Object   | O        | ヘッダ領域     |
| header.isSuccessful  | Boolean  | O        | 成否         |
| header.resultCode    | Integer  | O        | 失敗コード   |
| header.resultMessage | String   | O        | 失敗メッセージ |

<a id="delete-tags"></a>

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

<a id="curl-53"></a>

#### cURL

```
curl -X DELETE \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/tags/'"${TAG_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-52"></a>

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

| 値                    | タイプ     | Not Null | 説明         |
|----------------------|----------|----------|--------------|
| header               | Object   | O        | ヘッダ領域     |
| header.isSuccessful  | Boolean  | O        | 成否         |
| header.resultCode    | Integer  | O        | 失敗コード   |
| header.resultMessage | String   | O        | 失敗メッセージ |

<a id="uid-management"></a>

## UID管理

<a id="query-uids"></a>

### UID検索

<a id="request-53"></a>

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
|-----------|---------------|------|-------|----------------------------------------------------------------------------------|
| wheres    | 	List<String> | 	-   | オプション | 検索条件。<br/>英字、数字、括弧で校正された文字列配列。<br/>括弧は1個、 AND/ORは3個まで使用可能<br/>(例) tagId1、AND、tagId2 |
| offsetUid | 	String       | 	-   | オプション | 検索を始めるuid                                                                                |
| offset    | Integer       | -     | オプション | offset(デフォルト値：0)                                                                             |
| limit     | Integer       | 1000  | オプション | 検索件数(デフォルト値：15)                                                                             |

<a id="curl-54"></a>

#### cURL

```
curl -X GET \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/uids' \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-53"></a>

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

| 値                                       | タイプ     | Not Null | 説明           |
|-----------------------------------------|----------|----------|----------------|
| header                                  | Object   | O        | ヘッダ領域       |
| header.isSuccessful                     | Boolean  | O        | 成否           |
| header.resultCode                       | Integer  | O        | 失敗コード       |
| header.resultMessage                    | String   | O        | 失敗メッセージ     |
| body                                    | Object   | X        | 本文領域         |
| body.data.uids[].uid                    | String   | O        | UID            |
| body.data.uids[].tags[].tagId           | String   | O        | タグID          |
| body.data.uids[].tags[].tagName         | String   | X        | タグ名          |
| body.data.uids[].tags[].createdDate     | String   | O        | タグ作成日時      |
| body.data.uids[].tags[].updatedDate     | String   | O        | タグ修正日時      |
| body.data.uids[].contacts[].contactType | String   | O        | 連絡先タイプ      |
| body.data.uids[].contacts[].contact     | String   | O        | 連絡先(携帯電話番号) |
| body.data.uids[].contacts[].createdDate | String   | O        | 連絡先作成日時     |
| body.data.uids[].last                   | Boolean  | X        | 最後のリストかどうか  |

<a id="get-uids"></a>

### UID単件検索

<a id="request-54"></a>

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

<a id="curl-55"></a>

#### cURL

```
curl -X GET \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/uids/'"${USER_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-54"></a>

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

| 値                                | タイプ    | Not Null | 説明             |
|----------------------------------|---------|----------|------------------|
| header                           | Object  | O        | ヘッダ領域         |
| header.isSuccessful              | Boolean | O        | 成否             |
| header.resultCode                | Integer | O        | 失敗コード         |
| header.resultMessage             | String  | O        | 失敗メッセージ      |
| body                             | Object  | X        | 本文領域          |
| body.data.uid                    | String  | O        | UID             |
| body.data.tags[].tagId           | String  | O        | タグID           |
| body.data.tags[].tagName         | String  | X        | タグ名           |
| body.data.tags[].createdDate     | String  | O        | タグ作成日時       |
| body.data.tags[].updatedDate     | String  | O        | タグ修正日時       |
| body.data.contacts[].contactType | String  | O        | 連絡先タイプ       |
| body.data.contacts[].contact     | String  | O        | 連絡先(携帯電話番号) |
| body.data.contacts[].createdDate | String  | O        | 連絡先作成日時      |

<a id="register-uids"></a>

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

<a id="curl-56"></a>

#### cURL

```
curl -X POST \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/uids/' \
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

<a id="response-55"></a>

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

| 値                    | タイプ     | Not Null | 説明         |
|----------------------|----------|----------|--------------|
| header               | Object   | O        | ヘッダ領域     |
| header.isSuccessful  | Boolean  | O        | 成否         |
| header.resultCode    | Integer  | O        | 失敗コード   |
| header.resultMessage | String   | O        | 失敗メッセージ |

<a id="delete-uids"></a>

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

<a id="curl-57"></a>

#### cURL

```
curl -X DELETE \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/uids/'"${USER_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-56"></a>

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

| 値                    | タイプ     | Not Null | 説明         |
|----------------------|----------|----------|--------------|
| header               | Object   | O        | ヘッダ領域     |
| header.isSuccessful  | Boolean  | O        | 成否         |
| header.resultCode    | Integer  | O        | 失敗コード   |
| header.resultMessage | String   | O        | 失敗メッセージ |

<a id="register-phone-number"></a>

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

<a id="curl-58"></a>

#### cURL

```
curl -X POST \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/uids/'"${USER_ID}/phone-numbers" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}'  \
-d '{
    "phoneNumber": "0100000000"
}'
```

<a id="response-57"></a>

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

| 値                    | タイプ     | Not Null | 説明         |
|----------------------|----------|----------|--------------|
| header               | Object   | O        | ヘッダ領域     |
| header.isSuccessful  | Boolean  | O        | 成否         |
| header.resultCode    | Integer  | O        | 失敗コード   |
| header.resultMessage | String   | O        | 失敗メッセージ |

<a id="delete-phone-number"></a>

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

<a id="curl-59"></a>

#### cURL

```
curl -X DELETE \
https://sms.api.nhncloudservice.com/sms/v3.0/appKeys/'"${APP_KEY}"'/uids/'"${USER_ID}"'/phone-numbers/'"${P_NO}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-H 'X-Secret-Key: {secretkey}' 
```

<a id="response-58"></a>

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

| 値                    | タイプ     | Not Null | 説明         |
|----------------------|----------|----------|--------------|
| header               | Object   | O        | ヘッダ領域     |
| header.isSuccessful  | Boolean  | O        | 成否         |
| header.resultCode    | Integer  | O        | 失敗コード   |
| header.resultMessage | String   | O        | 失敗メッセージ |
