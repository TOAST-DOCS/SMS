## Notification > SMS > API v2.0ガイド
## v2.0 APIの紹介

### [APIドメイン]

|環境|	ドメイン|
|---|---|
|Real|	https://api-sms.cloud.toast.com|

### [注意事項]
* サポートする文字数は下記の通りです。
* 最大サポート文字数は保存基準で、文字切れ防止のため、標準規格で作成してください。
* エンコードはEUC-KR基準で送信され、サポートしていない顔文字は送信に失敗します。

| 分類 | 最大サポート | 標準規格 |
| --- | --- | --- |
| SMS本文 | 255文字 | 90バイト(全角45文字、半角90文字) |
| MMSタイトル | 120文字 | 40バイト(全角20文字、半角40文字) |
| MMS本文 | 4,000文字 | 2,000バイト(全角1,000文字、半角2,000文字) |


## 短文SMS

### 短文SMSの送信

#### リクエスト

[URL]

```
POST  /sms/v2.0/appKeys/{appKey}/sender/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Request body]

```
{
    "templateId": String,
    "body": String,
    "sendNo": String,
    "requestDate": String,
    "recipientList": [{
        "recipientNo": String,
        "countryCode": String,
        "internationalRecipientNo": String,
        "templateParameter": {
            String: String
        }
    }],
    "userId": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|templateId|	String|	X|	送信テンプレートID|
|body|	String|	O|	本文内容(EUC-KR基準で標準：90バイト、最大：255文字)|
|sendNo|	String|	O|	発信番号|
|requestDate| String| X | 予約日時(yyyy-MM-dd HH:mm)|
|recipientList|	List|	O|	受信者リスト(最大1000人)|
|- recipientNo|	String|	O|	受信番号<br/>countryCodeと組み合わせて使用可能|
|- countryCode|	String|	X|	国番号[デフォルト値：82(韓国)] <br>(海外に送信時、euc-kr(ハングル、アルファベット)内容のみ可能です。) |
|- internationalRecipientNo| String| X| 国番号を含む受信番号<br/>例)821012345678<br/>recipientNoがある場合、この値は無視されます。<br/>|
|- templateParameter|	Object|	X|	テンプレートパラメータ(テンプレートID入力時)|
|-- #key#|	String|	X|	置換キー(##key##)|
|-- #value#|	Object|	X|	置換キーにマッピングされるValue値|
|userId|	String|	X | 送信セパレータex)admin,system |

#### レスポンス

```
{
    "header": {
        "isSuccessful":boolean,
        "resultCode":Integer,
        "resultMessage":String
    },
    "body": {
        "data": {
            "requestId":String,
            "statusCode":String,
            "sendResultList" : [
                {
                    "recipientNo" : String,
                    "resultCode" :  Integer,
                    "resultMessage" : String
                }
            ]
        }
    }
}
```

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	Object|	データ領域|
|-- requestId|	String|	リクエストID|
|-- statusCode|	String|	リクエストステータスコード(1：リクエスト中、2：リクエスト完了、3：リクエスト失敗)|
|-- sendResultList|	List:Object|	送信結果リスト|
|--- recipientNo| String | 受信番号|
|--- resultCode| Integer | 結果コード|
|--- resultMessage| String | 結果メッセージ|

#### 短文SMSの送信例(一般国内受信番号)

<table>
  <thead>
    <tr><th>Http method</th><th colspan="3">URL</th></tr>
  </thead>
  <tbody>
    <tr>
      <td>POST</td>
      <td colspan="3">https://api-sms.cloud.toast.com/sms/v2.0/appKeys/{appKey}/sender/sms</td>
    </tr>
  </tbody>
</table>

[Request body]必須値は{ }で表示。
```
{
    "body": "{本文内容}",
    "sendNo": "{発信番号}",
    "recipientList": [{
        "recipientNo": "{受信番号}"
    }],
    "userId": ""
}
```

[Response]
```
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "data": {
      "requestId": "0-201607-424541-1",
      "statusCode": "2",
      "sendResultList" : [
          {
              "recipientNo" : {受信番号},
              "resultCode" :  0,
              "resultMessage" : "SUCCESS"
          }
      ]
    }
  }
}
```

[curl]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-sms.cloud.toast.com/sms/v2.0/appKeys/{appKey}/sender/sms -d '{"body": "{本文内容}","sendNo": "{発信番号}","recipientList":[{"recipientNo": "{受信番号}"}],"userId": "{userId}"}'
```

#### 短文SMS送信例(国コードが含まれている受信番号)

<table>
  <thead>
    <tr><th>Http method</th><th colspan="3">URL</th></tr>
  </thead>
  <tbody>
    <tr>
      <td>POST</td>
      <td colspan="3">https://api-sms.cloud.toast.com/sms/v2.0/appKeys/{appKey}/sender/sms</td>
    </tr>
  </tbody>
</table>


[Request body]必須値は{ }で表示。
```
{
    "body": "{本文内容}",
    "sendNo": "{発信番号}",
    "recipientList": [
    {
        "internationalRecipientNo": "{国番号を含む受信番号}"
    },
    {
        "recipientNo":"{受信番号}",
        "countryCode":"{国番号}"
      }
    ],
    "userId": ""
}
```

[Response]
```
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "data": {
      "requestId": "0-201607-424541-1",
      "statusCode": "2",
      "sendResultList" : [
          {
              "recipientNo" : {受信番号},
              "resultCode" :  0,
              "resultMessage" : "SUCCESS"
          }
      ]
    }
  }
}
```

[curl]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-sms.cloud.toast.com/sms/v2.0/appKeys/{appKey}/sender/sms -d '{"body": "{本文内容}","sendNo": "{発信番号}","recipientList": [{"internationalRecipientNo": "{国番号を含む受信番号}"},{"recipientNo":"{受信番号}","countryCode":"{国番号}"}],"userId": ""}'
```

### 短文SMS送信リストの照会

#### リクエスト

[URL]

```
GET  /sms/v2.0/appKeys/{appKey}/sender/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Query parameter] 1番or 2番の条件必須

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|requestId|	String|	条件必須(1番) |	リクエストID|
|startRequestDate|	String|	条件必須(2番) |	送信日時の開始値(yyyy-MM-dd HH:mm:ss)|
|endRequestDate|	String|	条件必須(2番) |	送信日時の終了値(yyyy-MM-dd HH:mm:ss)|
|startResultDate|	String|	オプション|	受信日時の開始値(yyyy-MM-dd HH:mm:ss)|
|endResultDate|	String|	オプション|	受信日時の終了値(yyyy-MM-dd HH:mm:ss)|
|sendNo|	String|	オプション|	発信番号|
|recipientNo|	String|	オプション|	受信番号|
|templateId|	String|	オプション|	テンプレート番号|
|msgStatus|	String|	オプション|	メッセージステータスコード(0：失敗、1：リクエスト、2：処理中、3：成功、4：送信取消、5:重複送信)|
|resultCode|	String|	オプション|	受信結果コード[[照会コード表](./error-code/#_2)]|
|subResultCode|	String|	オプション|	受信結果詳細コード[[照会コード表](./error-code/#_3)]|
|pageNum|	Integer|	オプション|	ページ番号(Default：1)|
|pageSize|	Integer|	オプション|	照会件数(Default：15)|

#### レスポンス

```
{
    "header": {
        "isSuccessful": boolean,
        "resultCode": Integer,
        "resultMessage": String
    },
    "body": {
        "pageNum": Integer,
        "pageSize": Integer,
        "totalCount": Integer,
        "data": [
            {
                "requestId": String,
                "requestDate": String,
                "resultDate": String,
                "templateId": String,
                "templateName": String,
                "categoryId": String,
                "categoryName": String,
                "body": String,
                "sendNo": String,
                "countryCode": String,
                "recipientNo": String,
                "msgStatus": String,
                "msgStatusName": String,
                "resultCode": String,
                "resultCodeName": String,
                "telecomCode": Integer,
                "telecomCodeName": String,
                "excelSeq": Integer,
                "mtPr": Integer,
                "sendType": String,
                "userId": String
            }
        ]
    }
}
```

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- pageNum|	Integer|	現在のページ番号|
|- pageSize|	Integer|	照会されたデータ件数|
|- totalCount|	Integer|	総データ件数|
|- data|	List|	データ領域|
|-- requestId|	String|	リクエストID|
|-- requestDate|	String|	発信日時|
|-- resultDate|	String|	受信日時|
|-- templateId|	String|	テンプレートID|
|-- templateName|	String|	テンプレート名|
|-- categoryId|	String|	カテゴリーID|
|-- categoryName|	String|	カテゴリー名|
|-- body|	String|	本文内容|
|-- sendNo|	String|	発信番号|
|-- countryCode|	String|	国番号|
|-- recipientNo|	String|	受信番号|
|-- msgStatus|	String|	メッセージステータスコード|
|-- msgStatusName|	String|	メッセージステータスコード名|
|-- resultCode|	String|	受信結果コード[[受信結果コード表](./error-code/#emma-v3)]|
|-- resultCodeName|	String|	受信結果コード名|
|-- telecomCode|	Integer|	サービスプロバイダーコード|
|-- telecomCodeName|	String|	サービスプロバイダー名|
|-- excelSeq|	Integer|	Excelシーケンス番号(Excelで送信した場合、データ存在)|
|-- mtPr|	Integer|	送信詳細ID(詳細照会時は必須)|
|-- sendType|	String|	送信タイプ(0：SMS、1：MMS、2：Auth)|
|-- userId|	String|	送信リクエストID|

### 短文SMS送信の単一照会

#### リクエスト

[URL]

```
GET  /sms/v2.0/appKeys/{appKey}/sender/sms/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
|requestId|	String|	リクエストID|

[Query parameter]

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|mtPr|	Integer|	必須|	送信詳細ID|

#### レスポンス

```
{
    "header": {
        "isSuccessful": boolean,
        "resultCode": Integer,
        "resultMessage": String
    },
    "body": {
        "data": [
            {
                "requestId": String,
                "requestDate": String,
                "resultDate": String,
                "templateId": String,
                "templateName": String,
                "categoryId": String,
                "categoryName": String,
                "body": String,
                "sendNo": String,
                "countryCode": String,
                "recipientNo": String,
                "msgStatus": String,
                "msgStatusName": String,
                "resultCode": String,
                "resultCodeName": String,
                "telecomCode": Integer,
                "telecomCodeName": String,
                "excelSeq": Integer,
                "mtPr": Integer,
                "sendType": String,
                "userId": String
            }
        ]
    }
}
```

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	List|	データ領域|
|-- requestId|	String|	リクエストID|
|-- requestDate|	String|	発信日時|
|-- resultDate|	String|	受信日時|
|-- templateId|	String|	テンプレートID|
|-- templateName|	String|	テンプレート名|
|-- categoryId|	String|	カテゴリーID|
|-- categoryName|	String|	カテゴリー名|
|-- body|	String|	本文内容|
|-- sendNo|	String|	発信番号|
|-- countryCode|	String|	国番号|
|-- recipientNo|	String|	受信番号|
|-- msgStatus|	String|	メッセージステータスコード|
|-- msgStatusName|	String|	メッセージステータスコード名|
|-- resultCode|	String|	受信結果コード[[受信結果コード表](./error-code/#emma-v3)]|
|-- resultCodeName|	String|	受信結果コード名|
|-- telecomCode|	Integer|	サービスプロバイダーコード|
|-- telecomCodeName|	String|	サービスプロバイダー名|
|-- excelSeq|	Integer|	Excelシーケンス番号(Excelで送信した場合、データ存在)|
|-- mtPr|	Integer|	送信詳細ID(詳細照会時は必須)|
|-- sendType|	String|	送信タイプ(0：SMS、1：MMS、2：Auth)|
|-- userId|	String|	送信リクエストID|

## 長文MMS

### 長文MMSの送信(添付ファイルなし)
※ MMSは韓国外への送信はできません。

#### リクエスト

[URL]

```
POST  /sms/v2.0/appKeys/{appKey}/sender/mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Request body]

```
{
  "templateId":String,
  "title":String,
  "body":String,
  "sendNo":String,
  "requestDate":String,
  "attachFileIdList":[Integer],
  "recipientList":[{
         "recipientNo":String,
         "countryCode":String,
         "internationalRecipientNo":String,
         "templateParameter":{String:String}
  }],
  "userId":String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|templateId|	String|	オプション|	送信テンプレートID|
|title|	String|	必須|	タイトル <br/> ('EUC-KR'基準で40Byte制限) <br/> (アルファベット：1byte、ハングル：2byte)|
|body|	String|	必須|	本文 <br/> ('EUC-KR'基準で2000Byte制限) <br/> (アルファベット：1byte、ハングル：2byte)|
|sendNo|	String|	必須|	発信番号|
|requestDate| String| X | 予約日時(yyyy-MM-dd HH:mm)|
|attachFileIdList|	List:Integer|	オプション|	添付ファイルIDリスト|
|recipientList|	List|	必須|	受信者リスト(最大1000人)|
|- recipientNo|	String|	必須|	受信番号<br/>countryCodeと組み合わせて使用可能|
|- countryCode|	String|	X|	国番号[デフォルト値：82(韓国)] <br>(海外に送信時、euc-kr(ハングル、アルファベット)内容のみ可能です。) |
|- internationalRecipientNo| String| X| 国番号を含む受信番号<br/>例)821012345678<br/>recipientNoがある場合、この値は無視されます。<br/>|
|- templateParameter|	Object|	オプション|	テンプレートパラメータ(テンプレートID入力時)|
|-- #key#|	String|	オプション|	置換キー(##key##)|
|-- #value#|	Object|	オプション|	置換キーにマッピングされるValue値|
|userId|	String|	オプション|	送信セパレータex)admin,system|

#### レスポンス

```
{
    "header": {
        "isSuccessful":boolean,
        "resultCode":Integer,
        "resultMessage":String
    },
    "body": {
        "data": {
            "requestId":String,
            "statusCode":String,
            "sendResultList" : [
                {
                    "recipientNo" : String,
                    "resultCode" :  Integer,
                    "resultMessage" : String
                }
            ]
        }
    }
}
```

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	Object|	データ領域|
|-- requestId|	String|	リクエストID|
|-- statusCode|	String|	リクエストステータスコード(1：リクエスト中、2：リクエスト完了、3：リクエスト失敗)|
|-- sendResultList|	List:Object|	送信結果リスト|
|--- recipientNo| String | 受信番号|
|--- resultCode| Integer | 結果コード|
|--- resultMessage| String | 結果メッセージ|

#### 長文MMSの送信例

<table>
  <thead>
    <tr><th>Http method</th><th colspan="3">URL</th></tr>
  </thead>
  <tbody>
    <tr>
      <td>POST</td>
      <td colspan="3">https://api-sms.cloud.toast.com/sms/v2.0/appKeys/{appKey}/sender/mms</td>
    </tr>
  </tbody>
</table>


[Request body]必須値は{ }で表示。
```
{
    "title": "{タイトル}",
    "body": "{本文内容}",
    "sendNo": "{発信番号}",
    "recipientList": [{
        "recipientNo": "{受信番号}",
        "templateParameter": { }
    }],
    "userId": ""
}
```

[Response]
```
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "data": {
      "requestId": "0-201607-424541-1",
      "statusCode": "2",
      "sendResultList" : [
          {
              "recipientNo" : {受信番号},
              "resultCode" :  0,
              "resultMessage" : "SUCCESS"
          }
      ]
    }
  }
}
```

[curl]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-sms.cloud.toast.com/sms/v2.0/appKeys/{appKey}/sender/mms -d '{"title": "{タイトル}","body": "{本文内容}","sendNo": "{発信番号}","recipientList": [{"recipientNo": "{受信番号}","templateParameter": { }}],"userId": ""}'
```

### 長文MMSの送信(添付ファイル含む)

#### 添付ファイルのアップロード

#### リクエスト

[URL]

```
POST  /sms/v2.0/appKeys/{appKey}/attachfile/binaryUpload
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Request body]

```
{
    "fileName": String,
    "createUser": String,
    "fileBody": Byte[]
}
```

|値|	タイプ|	必須|	説明|
|---|----|----|---|
|fileName|	String|	必須|	45文字以内<br/>ファイル名(拡張子はjpg、jpeg(小文字)のみ可能)|
|fileBody|	Byte[]|	必須|	ファイルのByte[]値<br/>300K以下|
|createUser|	String|	必須|	ファイルアップロードユーザー情報|

#### レスポンス

```
{
    "header": {
        "isSuccessful": boolean,
        "resultCode": Integer,
        "resultMessage": String
    },
    "body": {
        "data": {
            "fileId": Integer,
            "fileName": String,
            "filePath": String
        }
    }
}
```

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	Object|	データ領域|
|-- fileId|	Integer|	ファイルID|
|-- fileName|	String|	ファイル名|
|-- filePath|	String|	添付ファイル基本Path <br/> (https://domain/attachFile/filePath/fileName)|

#### 添付ファイルのアップロード例

<table>
  <thead>
    <tr><th>Http method</th><th colspan="3">URL</th></tr>
  </thead>
  <tbody>
    <tr>
      <td>POST</td>
      <td colspan="3">https://api-sms.cloud.toast.com/sms/v2.0/appKeys/{appKey}/attachfile/binaryUpload</td>
    </tr>
  </tbody>
</table>


[Request body]
```
{
  "fileName": "{ファイル名}",
  "createUser": "{リクエストID}",
  "fileBody": "{ファイルbyte値}"
}
```

[Response]
```
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "data": {
      "fileId": 252,
      "fileName": "sample_image.jpg",
      "filePath": "19/toast-mt-2016-07-05/1652/252"
    }
  }
}
```

#### 添付ファイルの送信例

<table>
  <thead>
    <tr><th>Http method</th><th colspan="3">URL</th></tr>
  </thead>
  <tbody>
    <tr>
      <td>POST</td>
      <td colspan="3">https://api-sms.cloud.toast.com/sms/v2.0/appKeys/{appKey}/sender/mms</td>
    </tr>
  </tbody>
</table>


[Request body]必須値は{ }で表示。
```
{
    "title": "{タイトル}",
    "body": "{本文内容}",
    "sendNo": "{発信番号}",
    "attachFileIdList": [
        "{ファイルアップロード後のresponseのfileId}"
    ],
    "recipientList": [{
        "recipientNo": "{受信番号}",
        "templateParameter": { }
    }],
    "userId": ""
}
```

[Response]
```
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
      "sendResultList" : [
          {
              "recipientNo" : {受信番号},
              "resultCode" :  0,
              "resultMessage" : "SUCCESS"
          }
      ]
    }
  }
}
```

### 長文MMS送信リストの照会

#### リクエスト

[URL]

```
GET  /sms/v2.0/appKeys/{appKey}/sender/mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Query parameter] 1番or 2番の条件必須

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|requestId|	String|	条件必須(1番) |	リクエストID|
|startRequestDate|	String|	条件必須(2番) |	送信日時の開始値(yyyy-MM-dd HH:mm:ss)|
|endRequestDate|	String|	条件必須(2番) |	送信日時の終了値(yyyy-MM-dd HH:mm:ss)|
|startResultDate|	String|	オプション|	受信日時の開始値(yyyy-MM-dd HH:mm:ss)|
|endResultDate|	String|	オプション|	受信日時の終了値(yyyy-MM-dd HH:mm:ss)|
|sendNo|	String|	オプション|	発信番号|
|recipientNo|	String|	オプション|	受信番号|
|templateId|	String|	オプション|	テンプレート番号|
|msgStatus|	String|	オプション|	メッセージステータスコード(0：失敗、1：リクエスト、2：処理中、3：成功、4：送信取消、5:重複送信)|
|resultCode|	String|	オプション|	受信結果コード[[照会コード表](./error-code/#_2)]|
|subResultCode|	String|	オプション|	受信結果詳細コード[[照会コード表](./error-code/#_3)]|
|pageNum|	Integer|	オプション|	ページ番号(Default：1)|
|pageSize|	Integer|	オプション|	照会件数(Default：15)|

#### レスポンス

```
{
    "header": {
        "isSuccessful": boolean,
        "resultCode": Integer,
        "resultMessage": String
    },
    "body": {
        "pageNum": Integer,
        "pageSize": Integer,
        "totalCount": Integer,
        "data": [{
            "requestId": String,
            "requestDate": String,
            "resultDate": String,
            "templateId": String,
            "templateName": String,
            "categoryId": Integer,
            "categoryName": String,
            "title": String,
            "body": String,
            "sendNo": String,
            "countryCode": String,
            "recipientNo": String,
            "msgStatus": String,
            "msgStatusName": String,
            "resultCode": String,
            "resultCodeName": String,
            "telecomCode": Integer,
            "telecomCodeName": String,
            "excelSeq": Integer,
            "mtPr": Integer,
            "sendType": String,
            "userId": String,
            "serviceType": String,
            "attachFileList": [{
                fileId: Integer,
                serviceId: Integer,
                attachType: String,
                templateId: Integer,
                filePath: String,
                fileName: String,
                saveFileName: String,
                fileSize: Long,
                createDate: String,
                createUser: String,
                updateDate: String,
                updateUser: String,
                uploadType: String,
                existFileName: String
            }]
        }]
    }
}
```

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- pageNum|	Integer|	現在のページ番号|
|- pageSize|	Integer|	照会されたデータ件数|
|- totalCount|	Integer|	総データ件数|
|- data|	List|	データ領域|
|-- requestId|	String|	リクエストID|
|-- requestDate|	String|	発信日時|
|-- resultDate|	String|	受信日時|
|-- templateId|	String|	テンプレートID|
|-- templateName|	String|	テンプレート名|
|-- categoryId|	String|	カテゴリーID|
|-- categoryName|	String|	カテゴリー名|
|-- title|	String|	タイトル|
|-- body|	String|	本文内容|
|-- sendNo|	String|	発信番号|
|-- countryCode|	String|	国番号|
|-- recipientNo|	String|	受信番号|
|-- msgStatus|	String|	メッセージステータスコード|
|-- msgStatusName|	String|	メッセージステータスコード名|
|-- resultCode|	String|	受信結果コード[[受信結果コード表](./error-code/#emma-v3)]|
|-- resultCodeName|	String|	受信結果コード名|
|-- telecomCode|	Integer|	サービスプロバイダーコード|
|-- telecomCodeName|	String|	サービスプロバイダー名|
|-- excelSeq|	Integer|	Excelシーケンス番号(Excelで送信した場合、データ存在)|
|-- mtPr|	Integer|	送信詳細ID(詳細照会時は必須)|
|-- sendType|	String|	送信タイプ(0：SMS、1：MMS、2：Auth)|
|-- userId|	String|	送信リクエストID|
|-- serviceType| Integer| 送信モジュールタイプ(0：SMS、2：MMS、3：LMS) |
|-- attachFileList|	List|	添付ファイルリスト|
|--- fileId|	Integer|	ファイルID|
|--- serviceId|	Integer|	サービスID|
|--- attachType|	Integer|	添付ファイルタイプ|
|--- templateId|	String|	テンプレートID|
|--- filePath|	String|	添付ファイルの基本Path <br/> (https://domain/attachFile/filePath/fileName)|
|--- filename|	String|	添付ファイル名|
|--- saveFileName|	String|	保存された添付ファイルの名前|
|--- fileSize|	Long|	添付ファイルの大きさ|
|--- createDate|	String|	作成日時|
|--- createUser|	String|	作成者|
|--- updateDate|	String|	修正日時|
|--- updateUser|	String|	修正者|
|--- uploadType|	String|	アップロードタイプ|
|--- existFileName|	String|	保存された添付ファイルの名前|

### 長文MMS送信の単一照会

#### リクエスト

[URL]

```
GET  /sms/v2.0/appKeys/{appKey}/sender/mms/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
|requestId|	String|	リクエストID|

[Query parameter]

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|mtPr|	Integer|	必須|	送信詳細ID|

#### レスポンス

```
{
    "header": {
        "isSuccessful": boolean,
        "resultCode": Integer,
        "resultMessage": String
    },
    "body": {
        "data": [{
            "requestId": String,
            "requestDate": String,
            "resultDate": String,
            "templateId": String,
            "templateName": String,
            "categoryId": Integer,
            "categoryName": String,
            "title": String,
            "body": String,
            "sendNo": String,
            "countryCode": String,
            "recipientNo": String,
            "msgStatus": String,
            "msgStatusName": String,
            "resultCode": String,
            "resultCodeName": String,
            "telecomCode": Integer,
            "telecomCodeName": String,
            "excelSeq": Integer,
            "mtPr": Integer,
            "sendType": String,
            "userId": String,
            "serviceType": String,
            "attachFileList": [{
                fileId: Integer,
                serviceId: Integer,
                attachType: String,
                templateId: Integer,
                filePath: String,
                fileName: String,
                saveFileName: String,
                fileSize: Long,
                createDate: String,
                createUser: String,
                updateDate: String,
                updateUser: String,
                uploadType: String,
                existFileName: String
            }]
        }]
    }
}
```

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	List|	データ領域|
|-- requestId|	String|	リクエストID|
|-- requestDate|	String|	発信日時|
|-- resultDate|	String|	受信日時|
|-- templateId|	String|	テンプレートID|
|-- templateName|	String|	テンプレート名|
|-- categoryId|	String|	カテゴリーID|
|-- categoryName|	String|	カテゴリー名|
|-- title|	String|	タイトル|
|-- body|	String|	本文内容|
|-- sendNo|	String|	発信番号|
|-- countryCode|	String|	国番号|
|-- recipientNo|	String|	受信番号|
|-- msgStatus|	String|	メッセージステータスコード|
|-- msgStatusName|	String|	メッセージステータスコード名|
|-- resultCode|	String|	受信結果コード[[受信結果コード表](./error-code/#emma-v3)]|
|-- resultCodeName|	String|	受信結果コード名|
|-- telecomCode|	Integer|	サービスプロバイダーコード|
|-- telecomCodeName|	String|	サービスプロバイダー名|
|-- excelSeq|	Integer|	Excelシーケンス番号(Excelで送信した場合、データ存在)|
|-- mtPr|	Integer|	送信詳細ID(詳細照会時は必須)|
|-- sendType|	String|	送信タイプ(0：SMS、1：MMS、2：Auth)|
|-- userId|	String|	送信リクエストID|
|-- serviceType| Integer| 送信モジュールタイプ(0：SMS、2：MMS、3：LMS) |
|-- attachFileList|	List|	添付ファイルリスト|
|--- fileId|	Integer|	ファイルID|
|--- serviceId|	Integer|	サービスID|
|--- attachType|	Integer|	添付ファイルタイプ|
|--- templateId|	String|	テンプレートID|
|--- filePath|	String|	添付ファイルの基本Path <br/> (https://domain/attachFile/filePath/fileName)|
|--- filename|	String|	添付ファイル名|
|--- saveFileName|	String|	保存された添付ファイルの名前|
|--- fileSize|	Long|	添付ファイルの大きさ|
|--- createDate|	String|	作成日時|
|--- createUser|	String|	作成者|
|--- updateDate|	String|	修正日時|
|--- updateUser|	String|	修正者|
|--- uploadType|	String|	アップロードタイプ|
|--- existFileName|	String|	保存された添付ファイルの名前|

## 認証用SMS(緊急)

### 認証用SMSの送信

#### リクエスト

[URL]

```
POST  /sms/v2.0/appKeys/{appKey}/sender/auth/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Request body]

```
{
    "templateId": String,
    "body": String,
    "sendNo": String,
    "requestDate":String,
    "recipientList": [{
        "recipientNo": String,
        "countryCode": String,
        "internationalRecipientNo": String,
        "templateParameter": {
            String: String
        }
    }],
    "userId": String
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|templateId|	String|	X|	送信テンプレートID|
|body|	String|	O|	本文内容(EUC-KR基準で標準：90バイト、最大：255文字)|
|sendNo|	String|	O|	発信番号|
|requestDate| String| X | 予約日時(yyyy-MM-dd HH:mm)|
|recipientList|	List|	O|	受信者リスト(最大1000人)|
|- recipientNo|	String|	O|	受信番号<br/>countryCodeと組み合わせて使用可能|
|- countryCode|	String|	X|	国番号[デフォルト値：82(韓国)] <br>(海外に送信時、euc-kr(ハングル、アルファベット)内容のみ可能です。) |
|- internationalRecipientNo| String| X| 国番号を含む受信番号<br/>例)821012345678<br/>recipientNoがある場合、この値は無視されます。|
|- templateParameter|	Object|	X|	テンプレートパラメータ(テンプレートID入力時)|
|-- #key#|	String|	X|	置換キー(##key##)|
|-- #value#|	Object|	X|	置換キーにマッピングされるValue値|
|userId|	String|	X |	送信セパレータex)admin,system|


#### レスポンス
```
{
    "header": {
        "isSuccessful":boolean,
        "resultCode":Integer,
        "resultMessage":String
    },
    "body": {
        "data": {
            "requestId":String,
            "statusCode":String,
            "sendResultList" : [
                {
                    "recipientNo" : String,
                    "resultCode" :  Integer,
                    "resultMessage" : String
                }
            ]
        }
    }
}
```

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	Object|	データ領域|
|-- requestId|	String|	リクエストID|
|-- statusCode|	String|	リクエストステータスコード(1：リクエスト中、2：リクエスト完了、3：リクエスト失敗)|
|-- sendResultList|	List:Object|	送信結果リスト|
|--- recipientNo| String | 受信番号|
|--- resultCode| Integer | 結果コード|
|--- resultMessage| String | 結果メッセージ|

#### 例

<table>
  <thead>
    <tr><th>Http method</th><th colspan="3">URL</th></tr>
  </thead>
  <tbody>
    <tr>
      <td>POST</td>
      <td colspan="3">https://api-sms.cloud.toast.com/sms/v2.0/appKeys/{appKey}/sender/auth/sms</td>
    </tr>
  </tbody>
</table>


[Request body]必須値は{ }で表示。
```
{
    "body": "{本文内容}",
    "sendNo": "{発信番号}",
    "recipientList": [{
        "recipientNo": "{受信番号}",
        "templateParameter": { }
    }],
    "userId": ""
}
```

[Response]
```
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "data": {
      "requestId": "2-201607-424651-1",
      "statusCode": "2",
      "sendResultList" : [
          {
              "recipientNo" : String,
              "resultCode" :  Integer,
              "resultMessage" : String
          }
      ]
    }
  }
}
```

[curl]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-sms.cloud.toast.com/sms/v2.0/appKeys/{appKey}/sender/auth/sms -d '{"body": "{本文内容}","sendNo": "{発信番号}","recipientList":[{"recipientNo": "{受信番号}","templateParameter": { }}],"userId": ""}'
```

### 認証用SMS送信リストの照会

#### リクエスト

[URL]

```
GET  /sms/v2.0/appKeys/{appKey}/sender/auth/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|----|---|
|appKey|	String|	固有のappKey|

[Query parameter] 1番or 2番の条件必須

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|requestId|	String|	条件必須(1番) |	リクエストID|
|startRequestDate|	String|	条件必須(2番) |	送信日時の開始値(yyyy-MM-dd HH:mm:ss)|
|endRequestDate|	String|	条件必須(2番) |	送信日時の終了値(yyyy-MM-dd HH:mm:ss)|
|startResultDate|	String|	オプション|	受信日時の開始値(yyyy-MM-dd HH:mm:ss)|
|endResultDate|	String|	オプション|	受信日時の終了値(yyyy-MM-dd HH:mm:ss)|
|sendNo|	String|	オプション|	発信番号|
|recipientNo|	String|	オプション|	受信番号|
|templateId|	String|	オプション|	テンプレート番号|
|msgStatus|	String|	オプション|	メッセージステータスコード(0：失敗、1：リクエスト、2：処理中、3：成功、4：送信取消、5:重複送信)|
|resultCode|	String|	オプション|	受信結果コード[[照会コード表](./error-code/#_2)]|
|subResultCode|	String|	オプション|	受信結果詳細コード[[照会コード表](./error-code/#_3)]|
|pageNum|	Integer|	オプション|	ページ番号(Default：1)|
|pageSize|	Integer|	オプション|	照会件数(Default：15)|

#### レスポンス

```
{
    "header": {
        "isSuccessful": boolean,
        "resultCode": Integer,
        "resultMessage": String
    },
    "body": {
        "pageNum": Integer,
        "pageSize": Integer,
        "totalCount": Integer,
        "data": [
            {
                "requestId": String,
                "requestDate": String,
                "resultDate": String,
                "templateId": String,
                "templateName": String,
                "categoryId": String,
                "categoryName": String,
                "body": String,
                "sendNo": String,
                "countryCode": String,
                "recipientNo": String,
                "msgStatus": String,
                "msgStatusName": String,
                "resultCode": String,
                "resultCodeName": String,
                "telecomCode": Integer,
                "telecomCodeName": String,
                "excelSeq": Integer,
                "mtPr": Integer,
                "sendType": String,
                "userId": String
            }
        ]
    }
}
```

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- pageNum|	Integer|	現在のページ番号|
|- pageSize|	Integer|	照会されたデータ件数|
|- totalCount|	Integer|	総データ件数|
|- data|	List|	データ領域|
|-- requestId|	String|	リクエストID|
|-- requestDate|	String|	発信日時|
|-- resultDate|	String|	受信日時|
|-- templateId|	String|	テンプレートID|
|-- templateName|	String|	テンプレート名|
|-- categoryId|	String|	カテゴリーID|
|-- categoryName|	String|	カテゴリー名|
|-- body|	String|	本文内容|
|-- sendNo|	String|	発信番号|
|-- countryCode|	String|	国番号|
|-- recipientNo|	String|	受信番号|
|-- msgStatus|	String|	メッセージステータスコード|
|-- msgStatusName|	String|	メッセージステータスコード名|
|-- resultCode|	String|	受信結果コード[[受信結果コード表](./error-code/#emma-v3)]|
|-- resultCodeName|	String|	受信結果コード名|
|-- telecomCode|	Integer|	サービスプロバイダーコード|
|-- telecomCodeName|	String|	サービスプロバイダー名|
|-- excelSeq|	Integer|	Excelシーケンス番号(Excelで送信した場合、データ存在)|
|-- mtPr|	Integer|	送信詳細ID(詳細照会時は必須)|
|-- sendType|	String|	送信タイプ(0：SMS、1：MMS、2：Auth)|
|-- userId|	String|	送信リクエストID|

### 認証用SMS送信の単一照会

#### リクエスト

[URL]

```
GET  /sms/v2.0/appKeys/{appKey}/sender/auth/sms/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
|requestId|	String|	リクエストID|

[Query parameter]

|値|	タイプ|	必須|	説明|
|---|----|---|---|
|mtPr|	Integer|	必須|	送信詳細ID|

#### レスポンス

```
{
    "header": {
        "isSuccessful": boolean,
        "resultCode": Integer,
        "resultMessage": String
    },
    "body": {
        "data": [
            {
                "requestId": String,
                "requestDate": String,
                "resultDate": String,
                "templateId": String,
                "templateName": String,
                "categoryId": String,
                "categoryName": String,
                "body": String,
                "sendNo": String,
                "countryCode": String,
                "recipientNo": String,
                "msgStatus": String,
                "msgStatusName": String,
                "resultCode": String,
                "resultCodeName": String,
                "telecomCode": Integer,
                "telecomCodeName": String,
                "excelSeq": Integer,
                "mtPr": Integer,
                "sendType": String,
                "userId": String
            }
        ]
    }
}
```

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	List|	データ領域|
|-- requestId|	String|	リクエストID|
|-- requestDate|	String|	発信日時|
|-- resultDate|	String|	受信日時|
|-- templateId|	String|	テンプレートID|
|-- templateName|	String|	テンプレート名|
|-- categoryId|	String|	カテゴリーID|
|-- categoryName|	String|	カテゴリー名|
|-- body|	String|	本文内容|
|-- sendNo|	String|	発信番号|
|-- countryCode|	String|	国番号|
|-- recipientNo|	String|	受信番号|
|-- msgStatus|	String|	メッセージステータスコード|
|-- msgStatusName|	String|	メッセージステータスコード名|
|-- resultCode|	String|	受信結果コード[[受信結果コード表](./error-code/#emma-v3)]|
|-- resultCodeName|	String|	受信結果コード名|
|-- telecomCode|	Integer|	サービスプロバイダーコード|
|-- telecomCodeName|	String|	サービスプロバイダー名|
|-- excelSeq|	Integer|	Excelシーケンス番号(Excelで送信した場合、データ存在)|
|-- mtPr|	Integer|	送信詳細ID(詳細照会時は必須)|
|-- sendType|	String|	送信タイプ(0：SMS、1：MMS、2：Auth)|
|-- userId|	String|	送信リクエストID|

## 広告文字
### 広告性SMS送信
[URL]

```
POST  /sms/v2.0/appKeys/{appKey}/sender/ad-sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Request body]
上のSMS送信と同じ。
[[Request Body参考](./api-guide/#sms_2)]

<span style="color:red">ただし、本文に下記の文言を入れる必要があります。</span>
080番号はコンソールの080受信拒否設定タブで確認可能です。
```
(広告)

[無料受信拒否]080XXXXXXX
```


### 広告性MMS送信
※ MMSは韓国外への送信はできません。

[URL]

```
POST  /sms/v2.0/appKeys/{appKey}/sender/ad-mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Request body]
上のMMS送信と同じ。
[[Request Body参考](./api-guide/#mms_1)]

<span style="color:red">ただし、本文に下記の文言を入れる必要があります。</span>
080番号はコンソールの080受信拒否設定タブで確認可能です。
```
(広告)

[無料受信拒否]080XXXXXXX
```




## タグ送信

### タグSMS送信

#### リクエスト

[URL]

```
POST /sms/v2.0/appKeys/{appKey}/tag-sender/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Request body]

```
{
    "body":"SMS内容",
    "sendNo":"01012345678",
    "requestDate":"2018-03-22 10:00",
    "templateId":"TEMPLATE",
    "templateParameter" : {
        "key" : "value"
    },
    "tagExpression":[
        "tag1",
        "AND",
        "tag2"
     ],
    "userId":"user_id",
    "adYn":"N",
    "autoSendYn":"N"
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
| body | String | O | 本文内容(EUC-KR基準で標準：90バイト、最大：255文字) |
| sendNo | String | O | 発信番号 |
|requestDate| String| X | 予約日時(yyyy-MM-dd HH:mm)|
| templateId | String | X | テンプレートID |
| templateParameter | Map<String, String> | X | テンプレートパラメータ |
| tagExpression | List<String> | O | タグ表現式<br/>ex) ["tagA","AND","tabB"] |
| userId | String | X | リクエストしたユーザーのID |
| adYn | String | X | 広告かどうか(基本N) |
| autoSendYn | String | X | 自動送信(即時送信)するかどうか(基本Y) |


#### レスポンス
```
{
    "header" : {
        "isSuccessful" :  true,
        "resultCode" :  0,
        "resultMessage" :  "."
    },
    "body" : {
        "data" : {
            "requestId" :  ""
        }
    }
}
```

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	Object|	データ領域|
|-- requestId|	String|	リクエストID|

### タグLMS送信
※ MMSは韓国外への送信はできません。

#### リクエスト

[URL]

```
POST  /sms/v2.0/appKeys/{appKey}/tag-sender/mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Request body]
```
{
    "body":"SMS内容",
    "sendNo":"01012345678",
    "requestDate":"2018-03-22 10:00",
    "templateId":"TEMPLATE",
    "templateParameter" : {
        "key" : "value"
    },
    "attachFileIdList" : [
     1,
     2,
3
    ],
    "tagExpression":[
        "tag1",
        "AND",
        "tag2"
     ],
    "userId":"user_id",
    "adYn":"N",
    "autoSendYn":"N"
}
```

|値|	タイプ|	必須|	説明|
|---|---|---|---|
| title | String | O | タイトル |
| body | String | O | 内容 |
| sendNo | String | O | 発信番号 |
|requestDate| String| X | 予約日時(yyyy-MM-dd HH:mm)|
| templateId | String | X | テンプレートID |
| templateParameter | Map<String, String> | X | テンプレートパラメータ |
| tagExpression | List<String> | O | タグ表現式<br/>ex) ["tagA","AND","tabB"] |
| attachFileIdList | List<Integer> | X | 添付ファイルID(fileId) |
| userId | String | X | リクエストしたユーザーのID |
| adYn | String | X | 広告かどうか(基本N) |
| autoSendYn | String | X | 自動送信(即時送信)するかどうか(基本Y) |


#### レスポンス
```
{
    "header" : {
        "isSuccessful" :  true,
        "resultCode" :  0,
        "resultMessage" :  "."
    },
    "body" : {
        "data" : {
            "requestId" :  ""
        }
    }
}
```

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	Object|	データ領域|
|-- requestId|	String|	リクエストID|


### タグ送信リストの照会

#### リクエスト

[URL]

```
GET /sms/v2.0/appKeys/{appKey}/tag-sender?sendType={sendType}&requestId={requestId}&startRequestDate={startRequestDate}&endRequestDate={endRequestDate}&statusCode={statusCode}&pageNum={pageNum}&pageSize={pageSize}
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Request body]

```
X
```

* requestIdまたはstartRequestDate + endRequestDateは必須です。

|値|	タイプ|	必須|	説明|
|---|---|---|---|
| appKey | String| O | アプリケーションキー |
| sendType | required, String | O | 送信タイプ<br>SMS："0"、<br>MMS："1" |
| requestId | String | O | リクエストID |
| startRequestDate | String | O | 送信日開始 |
| endRequestDate | String | O | 送信日終了 |
| statusCode | String | X | 送信ステータスコード<br>WAIT："MAS00"<br>READY："MAS01"<br>SENDREADY："MAS09"<br>SENDWAIT："MAS10"<br>SENDING："MAS11"<br>COMPLETE："MAS19"<br>CANCEL："MAS91"<br>FAIL："MAS99" |
| pageNum | optional, Integer | X | ページ番号 |
| pageSize | optional, Integer | X | 照会件数 |

#### レスポンス
```
{
    "header" : {
    "isSuccessful" :  true,
    "resultCode" :  0,
    "resultMessage" :  "."
    },
    "body":{
        "pageNum":0,
        "pageSize":0,
        "totalCount":0,
        "data" :[{
                "requestId": "20171220141558eonMsyDI6P0",
                "requestIp": "127.0.0.1",
                "sendType": "0",
                "templateId": "TPL04",
                "templateName": "置換テスト(SMS)",
                "masterStatusCode": "READY",
                "sendNo": "15883796",
                "requestDate": "2017-12-20 14:15:58",
                "tagExpression": [
                    "Kb6BjCY1"
                ],
                "title": "",
                "body": "SMS送信です##content##",
                "adYn": "N",
                "autoSendYn": "Y",
                "sendErrorCount": 0,
                "createUser": "zisu.kim",
                "createDate": "2017-12-20 14:15:58.0",
                "updateUser": "zisu.kim",
                "updateDate": "2017-12-20 14:15:58.0"
            }
        ]
    }
}
```

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	Object|	データ領域|
|-- requestId | String | リクエストID |
|-- requestIp | String | リクエストIP |
|-- requestDate | String | リクエスト時間 |
|-- tagSendStatus | String | タグ送信ステータス |
|-- tagExpression | List | タグ表現式 |
|-- templateId | String | テンプレートID |
|-- templateName | String | テンプレート名 |
|-- senderName | String | 発信者名 |
|-- senderMail | String | 発信者アドレス |
|-- title | String | タイトル |
|-- body | String | 内容 |
|-- attachYn | String | 添付ファイル有無 |
|-- adYn | String | 広告かどうか |
|-- createUser | String | 作成者 |
|-- createDate | String | 作成日時 |
|-- updateUser | String | 修正者 |
|-- updateDate | String | 修正日時 |


### タグ送信受信者リストの照会

#### リクエスト

[URL]

```
GET /sms/v2.0/appKeys/{appKey}/tag-sender/{requestId}?recipientNum={recipientNum}&startRequestDate={startRequestDate}&endRequestDate={endRequestDate}&startResultDate={startResultDate}&endResultDate={endResultDate}&msgStatusName={msgStatusName}&resultCode={resultCode}&pageNum={pageNum}&pageSize={pageSize}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
| requestId | String | リクエストID |
[Request body]

```
X
```

* requestIdまたはstartRequestDate + endRequestDateは必須です。

|値|	タイプ|	必須|	説明|
|---|---|---|---|
| recipientNum | String | X | 受信者番号 |
| startRequestDate | String | O | 送信リクエスト開始日 |
| endRequestDate | String | O | 送信リクエスト終了日 |
| startResultDate | String | X | 受信開始日 |
| endResultDate | | | |
| msgStatusName | | | |
| resultCode | | | |
| pageNum | | | |
| pageSize | | | |

#### レスポンス
```
{
    "header" : {
    "isSuccessful" :  true,
    "resultCode" :  0,
    "resultMessage" :  "."
    },
    "body":{
        "pageNum":0,
        "pageSize":0,
        "totalCount":0,
        "data" :[{
                "requestId": "20171220141558eonMsyDI6P0",
                "requestIp": "127.0.0.1",
                "sendType": "0",
                "templateId": "TPL04",
                "templateName": "置換テスト(SMS)",
                "masterStatusCode": "READY",
                "sendNo": "15883796",
                "requestDate": "2017-12-20 14:15:58",
                "tagExpression": [
                    "Kb6BjCY1"
                ],
                "title": "",
                "body": "SMS送信です##content##",
                "adYn": "N",
                "autoSendYn": "Y",
                "sendErrorCount": 0,
                "createUser": "zisu.kim",
                "createDate": "2017-12-20 14:15:58.0",
                "updateUser": "zisu.kim",
                "updateDate": "2017-12-20 14:15:58.0"
            }
        ]
    }
}
```

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	Object|	データ領域|
|-- requestId | String | リクエストID |
|-- requestIp | String | リクエストIP |
| sendType | String | 送信タイプ(0：SMS、1：LMS/MMS)|
| templateId | String | テンプレートID |
| templateName | String | テンプレート名 |
| masterStatusCode | String | マスターステータスコード |
| sendNo | String | 送信番号 |
| requestDate | String | リクエスト日時 |
| tagExpression | List<String> | タグ表現式 |
| title | String | タイトル |
| body | String | 本文 |
| adYn | String | 広告かどうか |
| autoSendYn | String | 自動送信するかどうか |
| sendErrorCount | Integer | 送信エラー数 |
| createUser | String | 作成者 |
| createDate | String | 作成日時 |
| updateUser | String | 修正者 |
| updateDate | String | 修正日時 |

### タグ送信受信者リストの詳細照会

#### リクエスト

[URL]

```
GET /sms/v2.0/appKeys/{appKey}/tag-sender/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
| requestId | String | リクエストID |
| recipientSeq | String | シーケンス |

[Request body]

```
X
```

#### レスポンス
```
{
    "header" : {
    "isSuccessful" :  true,
    "resultCode" :  0,
    "resultMessage" :  "."
},
"body":{
"data" :{
            "requestId": "20171220152855qS0QZ2UAP92",
            "recipientSeq": 0,
            "sendType": "0",
            "messageType": "SMS",
            "templateId": "TPL06",
            "templateName": "置換テスト(MMS)",
            "sendNo": "15883796",
            "recipientNum": "01000000000",
            "title": "タイトルです",
            "body": "内容です",
            "requestDate": "2017-12-20 15:28:55.0",
            "msgStatusName": "READY",
            "msgStatus": "1",
            "resultCode": "SUCCESS",
            "receiveDate": "2017-12-25 14:06:04.0",
            "attachFileList": [
                {
                    "fileId": 58217,
                    "filePath": "10869/toast-mt-2017-07-28/1459/58217",
                    "fileName": "スクリーンショット2017-07-12午前10.00.12.jpg",
                    "fileSize":null,
                    "createDate": "2017-07-28 14:59:31.0",
                    "updateDate": "2017-07-28 14:59:35.0"
                }
            ]
        }
}
}
```

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	Object|	データ領域|
|-- requestId | String | リクエストID |
|-- recipientSeq | Integer | リクエストID |
|-- sendType | String | 送信タイプ |
|-- messageType | String | メッセージタイプ |
|-- templateId | String | テンプレートID |
|-- templateName | String | テンプレート名 |
|-- sendNo | String | 発信番号 |
|-- title | String | タイトル |
|-- body | String | 内容 |
|-- recipientNum | String | 受信番号 |
|-- requestDate | String | リクエスト日時 |
|-- msgStatusName | String | メッセージステータス名 |
|-- resultCode | String | 受信コード |
|-- receiveDate | String | 受信日時 |
|-- attachFileList | List | 添付ファイルリスト |
|---- filePath | String | 添付ファイル - パス |
|---- fileName | String | 添付ファイル - ファイル名 |
|---- fileSize | Long | 添付ファイル - サイズ |
|---- fileSequence | Integer | 添付ファイル - ファイル番号 |
|---- createDate | String | 添付ファイル - 作成日時 |
|---- updateDate | String | 添付ファイル - 修正日時 |




## テンプレート

### テンプレート送信(本文修正が必要ない場合)

#### 例
![[図1]テンプレートの登録](http://static.toastoven.net/prod_sms/img_26.png)

<table>
  <thead>
    <tr>
      <th>Http method</th>
      <th>種類</th>
      <th colspan="3">URL</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>POST</td>
      <td>SMS</td>
      <td colspan="3">https://api-sms.cloud.toast.com/sms/v2.0/appKeys/{appKey}/sender/sms</td>
    </tr>
    <tr>
      <td>POST</td>
      <td>MMS</td>
      <td colspan="3">https://api-sms.cloud.toast.com/sms/v2.0/appKeys/{appKey}/sender/mms</td>
    </tr>
  </tbody>
</table>

Request URLは、テンプレート登録時に選択した送信タイプに選択して送信。

**Request parameter body内の値が空欄の時は、該当templateIdのbody内容に置換。**

[Request body]置換するキーと値をkey、valueに代入。

```
{
    "templateId": "{テンプレートID}",
    "recipientList": [{
        "recipientNo": "{受信番号}",
        "templateParameter": {
          "key1" : "Toast Cloud",
          "key2" : "SMS"
        }
    }],
    "userId": ""
}
```

[Response]
```
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "data": {
      "requestId": "2-201607-424651-1",
      "statusCode": "2"
    }
  }
}
```

![[図2]テンプレート送信に成功](http://static.toastoven.net/prod_sms/img_27.png)

### テンプレート送信(本文修正が必要な場合)

#### テンプレート送信例

<table>
  <thead>
    <tr>
      <th>Http method</th>
      <th>種類</th>
      <th colspan="3">URL</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>POST</td>
      <td>SMS</td>
      <td colspan="3">https://api-sms.cloud.toast.com/sms/v2.0/appKeys/{appKey}/sender/sms</td>
    </tr>
    <tr>
      <td>POST</td>
      <td>MMS</td>
      <td colspan="3">https://api-sms.cloud.toast.com/sms/v2.0/appKeys/{appKey}/sender/mms</td>
    </tr>
  </tbody>
</table>


Request URLは、テンプレート登録時に選択した送信タイプに選択して送信。

**テンプレートIDとRequest parameter body内の値がある場合、発信番号と発信内容がテンプレート内容に置換されません。**

ただし、テンプレートIDを入力した場合、該当テンプレートで照会が可能です。

上記のような場合は、テンプレート照会を行った後、テンプレートの内容を修正したい場合に使用できます。

[Request body]

```
{
    "templateId": "{テンプレートID}",
    "body": "{発信内容}",
    "sendNo": "{発信番号}",
    "recipientList": [{
        "recipientNo": "{受信番号}",
        "templateParameter": { }
    }],
    "userId": ""
}
```

[Response]
```
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "SUCCESS"
  },
  "body": {
    "data": {
      "requestId": "2-201607-424651-1",
      "statusCode": "2"
    }
  }
}
```


### テンプレートリストの照会

#### リクエスト

[URL]

```
GET  /sms/v2.0/appKeys/{appKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Query parameter]

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|categoryId|	Integer|	オプション|	カテゴリーID|
|useYn|	String|	オプション|	使用(Y/N)|
|pageNum|	Integer|	オプション|	ページ番号(Default：1)|
|pageSize|	Integer|	オプション|	照会件数(Default：15)|
|all|	Boolean|	オプション|	全体照会するかどうか|

#### レスポンス

```
{
    "header": {
        "isSuccessful": boolean,
        "resultCode": Integer,
        "resultMessage": String
    },
    "body": {
        "pageNum": Integer,
        "pageSize": Integer,
        "totalCount": Integer,
        "data": [{
            "templateId": String,
            "serviceId": Integer,
            "categoryId": Integer,
            "categoryName": String,
            "sort": Integer,
            "templateName": String,
            "templateDesc": String,
            "useYn": String,
            "priority": String,
            "sendNo": String,
            "sendType": String,
            "sendTypeName": String,
            "title": String,
            "body": String,
            "attachFileYn": String,
            "delYn": String,
            "createDate": String,
            "createUser": String,
            "updateDate": String,
            "updateUser": String,
            "attachFileList": [{
                "fileId": Integer,
                "serviceId": Integer,
                "attachType": Integer,
                "templateId": String,
                "filePath": String,
                "fileName": String,
                "fileSize": Integer,
                "createDate": String,
                "createUser": String
            }]
        }]
    }
}
```

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- pageNum|	Integer|	現在のページ番号|
|- pageSize|	Integer|	照会されたデータ件数|
|- totalCount|	Integer|	総データ件数|
|- data|	List|	データ領域|
|-- templateId|	String|	テンプレートID|
|-- serviceId|	Integer|	サービスID(内部用、未使用値)|
|-- categoryId|	Integer|	カテゴリーID|
|-- categoryName|	String|	カテゴリー名|
|-- sort|	Integer|	ソート値|
|-- templateName|	String|	テンプレート名|
|-- templateDesc|	String|	テンプレートの説明|
|-- useYn|	String|	使用するかどうか|
|-- priority|	String|	優先順位値(未使用値)|
|-- sendNo|	String|	発信番号|
|-- sendType|	String|	送信タイプ(0：Sms、1：mms、2：auth)|
|-- sendTypeName|	String|	送信タイプ名|
|-- title|	String|	タイトル|
|-- body|	String|	本文内容|
|-- attachFileYn|	String|	添付ファイル有無(Y/N)|
|-- delYn”|	String|	削除するかどうか(Y/N)、現在ステータス表記用にのみ使用|
|-- createDate|	String|	登録日|
|-- createUser|	String|	登録ユーザー|
|-- updateDate|	String|	修正日|
|-- updateUser|	String|	修正ユーザー|
|-- attachFileList|	List|	添付ファイルリスト|
|--- fileId|	Integer|	添付ファイルID|
|--- serviceId|	Integer|	サービスID(内部用、未使用値)|
|--- attachType|	Integer|	添付ファイルアップロードタイプ(0：臨時、1：アップロード、2：テンプレート)|
|--- templateId|	String|	テンプレートID|
|--- filePath|	String|	添付ファイルのパス|
|--- filename|	String|	添付ファイル名|
|--- fileSize|  Integer| ファイルサイズ|
|--- createDate|	String|	添付ファイルの登録日|
|--- createUser|	String|	添付ファイルの登録ユーザー|


### テンプレートの単一照会

#### リクエスト

[URL]

```
GET  /sms/v2.0/appKeys/{appKey}/templates/{templateId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|
|templateId|	String|	テンプレートID|

#### レスポンス

```
{
    "header": {
        "isSuccessful": boolean,
        "resultCode": Integer,
        "resultMessage": String
    },
    "body": {
        "data": [{
            "templateId": String,
            "serviceId": Integer,
            "categoryId": Integer,
            "categoryName": String,
            "sort": Integer,
            "templateName": String,
            "templateDesc": String,
            "useYn": String,
            "priority": String,
            "sendNo": String,
            "sendType": String,
            "sendTypeName": String,
            "title": String,
            "body": String,
            "attachFileYn": String,
            "delYn": String,
            "createDate": String,
            "createUser": String,
            "updateDate": String,
            "updateUser": String,
            "attachFileList": [{
                "fileId": Integer,
                "serviceId": Integer,
                "attachType": Integer,
                "templateId": String,
                "filePath": String,
                "fileName": String,
                "fileSize": Integer,
                "createDate": String,
                "createUser": String
            }]
        }]
    }
}
```

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- data|	List|	データ領域|
|-- templateId|	String|	テンプレートID|
|-- serviceId|	Integer|	サービスID(内部用、未使用値)|
|-- categoryId|	Integer|	カテゴリーID|
|-- categoryName|	String|	カテゴリー名|
|-- sort|	Integer|	ソート値|
|-- templateName|	String|	テンプレート名|
|-- templateDesc|	String|	テンプレートの説明|
|-- useYn|	String|	使用するかどうか|
|-- priority|	String|	優先順位値(未使用値)|
|-- sendNo|	String|	発信番号|
|-- sendType|	String|	送信タイプ(0：Sms、1：mms、2：auth)|
|-- sendTypeName|	String|	送信タイプ名|
|-- title|	String|	タイトル|
|-- body|	String|	本文内容|
|-- attachFileYn|	String|	添付ファイル有無(Y/N)|
|-- delYn”|	String|	削除するかどうか(Y/N)、現在ステータス表記用にのみ使用|
|-- createDate|	String|	登録日|
|-- createUser|	String|	登録ユーザー|
|-- updateDate|	String|	修正日|
|-- updateUser|	String|	修正ユーザー|
|-- attachFileList|	List|	添付ファイルリスト|
|--- fileId|	Integer|	添付ファイルID|
|--- serviceId|	Integer|	サービスID(内部用、未使用値)|
|--- attachType|	Integer|	添付ファイルアップロードタイプ(0：臨時、1：アップロード、2：テンプレート)|
|--- templateId|	String|	テンプレートID|
|--- filePath|	String|	添付ファイルのパス|
|--- filename|	String|	添付ファイル名|
|--- fileSize|  Integer| ファイルサイズ|
|--- createDate|	String|	添付ファイルの登録日|
|--- createUser|	String|	添付ファイルの登録ユーザー|

## 080受信拒否サービス

### ﻿受信拒否対象者を登録

#### リクエスト

[URL]

```
POST /sms/v2.0/appKeys/{appKey}/blockservice/recipients
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のアプリケーションキー|

[Request body]

```
{
    "unsubscribeNo":"0800000000",
    "recipientNoList":["0100000000", "0100000001"]
}
```

|値|	タイプ|	最大文字数 | 必須|	説明|
|---|---|---|---|---|
|unsubscribeNo |    String | 25 | O | 080受信拒否番号|
| recipientNoList | List<String> | 10 | O | 追加する受信拒否対象者番号 |

#### レスポンス

```
{
   "header":{
       "isSuccessful":true,
       "resultCode":0,
       "resultMessage":"Success"
   }
}
```

### 受信拒否対象者の照会

#### リクエスト

[URL]

```
GET  /sms/v2.0/appKeys/{appKey}/blockservice/recipients
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Query parameter]

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|unsubscribeNo|	String|	必須 |	080受信拒否番号 |
|recipientNo| String | オプション | 受信拒否番号 |
|startRequestDate|	String|	オプション |	受信拒否リクエストの開始値(yyyy-MM-dd HH:mm:ss)|
|endRequestDate|	String|	オプション |	受信拒否リクエストの終了値(yyyy-MM-dd HH:mm:ss)|
|pageNum|	Integer|	オプション|	ページ番号(Default：1)|
|pageSize|	Integer|	オプション|	照会件数(Default：15)|

#### レスポンス
```
{
    "header": {
        "isSuccessful": boolean,
        "resultCode": Integer,
        "resultMessage": String
    },
    "body": {
        "pageNum": Integer,
        "pageSize": Integer,
        "totalCount": Integer,
        "data": [
            {
                "unsubscribeNo": String,
                "recipientNo": String,
                "requestDate": String
            }
        ]
    }
}
```

### 受信拒否対象者の削除

#### リクエスト

[URL]

```
DELETE  /sms/v2.0/appKeys/{appKey}/blockservice/recipients/removes?unsubscribeNo={unsubscribeNo}&updateUser={updateUser}&recipientNoList={recipientNo},{recipientNo}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Query parameter]

|値|	タイプ|	必須|	説明|
|---|---|---|---|
|unsubscribeNo|	String|	必須 |	080受信拒否番号 |
|updateUser|	String|	必須 |	受信拒否削除者|
|recipientNo|	String|	必須 |	削除する受信拒否番号|

#### レスポンス
```
{
    "header": {
        "isSuccessful": boolean,
        "resultCode": Integer,
        "resultMessage": String
    },
    "body": null
}
```

## 発信番号

### 登録された発信番号リストの照会API

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET|	/sms/v2.0/appKeys/{appKey}/sendNos?sendNo={sendNo}&useYn={useYn}&blockYn={blockYn}

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Query parameter]

|値|	タイプ|	説明|
|---|---|---|
| sendNo | String | 発信番号 |
| useYn | String | 使用するかどうか |
| blockYn | String | 遮断するかどうか |

#### レスポンス
```
{
    "header" : {
        "isSuccessful" :  true,
        "resultCode" :  0,
        "resultMessage" :  ""
    },
    "body" : {
        "pageNum" :  0,
        "pageSize" :  0,
        "totalCount" :  0,
        "data" : [
        {
            "serviceId" :  0,
            "sendNo" :  "",
            "useYn" :  "",
            "blockYn" :  "",
            "blockReason" :  "",
            "createDate" :  "",
            "createUser" :  "",
            "updateDate" :  "",
            "updateUser" :  ""
        }
        ]
    }
}
```

|値|	タイプ|	説明|
|---|---|---|
|header|	Object|	ヘッダ領域|
|- isSuccessful|	Boolean|	成否|
|- resultCode|	Integer|	失敗コード|
|- resultMessage|	String|	失敗メッセージ|
|body|	Object|	本文領域|
|- pageNum|	Integer|	ページ番号|
|- pageSize|	Integer|	リスト出力サイズ|
|- totalCount|	Integer|	総データ数|
|-- serviceId | Integer | サービスID |
|-- sendNo | String | 発信番号 |
|-- useYn | String | 使用するかどうか |
|-- blockYn | String | 遮断するかどうか |
|-- blockReason | String | 遮断理由 |
|-- createDate | String | 作成日時 |
|-- createUser | String | 作成者 |
|-- updateDate | String | 修正日時 |
|-- updateUser | String | 修正者 |

## 統計照会

### 統合統計照会

#### リクエスト

[URL]

|Http method|	URI|
|---|---|
|GET|	/sms/v2.0/appKeys/{appKey}}/statistics/view?searchType={searchType}&from={from}&to={to}&messageTypes={messageType}&contentTypes={contentType}&templateId={templateId}|

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のappKey|

[Query parameter]

|値|	タイプ|	必須 |説明|
|---|---|---|---|
| searchType | String | O | 統計区分<br/>DATE：日付別、TIME：時間別、DAY：曜日別 |
| from | String | O | 統計照会の開始日<br/>yyyy-MM-dd HH:mm |
| to | String | O | 統計照会の終了日<br/>yyyy-MM-dd HH:mm |
| messageType | X | String | メッセージタイプ<br/>SMS：短文、LMS：長文、MMS：添付ファイル、AUTH：認証用 |
| contentType | X | String | コンテンツタイプ<br/>NORMAL：一般、AD：広告 |
| templateId | X | String | テンプレートID |

#### レスポンス
```
{
    "header" : {
        "isSuccessful" : true,
        "resultCode" : 0,
        "resultMessage" : "SUCCESS""
    },
    "body" : {
        "data" :
        [
          {
            "divisionName" : "2018-06-01",
            "statisticsView" :
            {
              "requestedCount" : 10,
              "succeedCount" : 10,
              "failedCount" : 0,
              "pendingCount" : 0,
              "succeedRate" : "100.00",
              "failedRate" : "0.00",
              "pendingRate" : "0.00"
            }
          }
        ]
    }
}
```
|値| タイプ|説明|
|---|---|---|
| header | Object | |
| - isSuccessful | Boolean | 成否 |
| - resultCode | Integer | 結果コード |
| - resultMessage | String | 結果メッセージ |
| body.data | List | |
| - divisionName | String | 表示名<br/>日付、時間、曜日 |
| - statisticsView | Object | |
| -- requestedCount | Integer | リクエスト数 |
| -- succeedCount | Integer | 成功数 |
| -- failedCount | Integer | 失敗数 |
| -- pendingCount | Integer | 送信中の数 |
| -- succeedRate | String | 成功の比率 |
| -- failedRate | String | 失敗の比率 |
| -- pendingRate | String | 送信中の比率 |

## タグ管理

### タグ照会

#### リクエスト

[URL]

```
GET /sms/v2.0/appKeys/{appKey}/tags?pageNum={pageNum}&pageSize={pageSize}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のアプリケーションキー|

[Query parameter]

|値|	タイプ| 最大長さ |	必須|	説明|
|---|---|---|---|---|
|pageNum|	Integer|	- | オプション | オプション | ページ番号(デフォルト値：1)|
|pageSize|	Integer|	1000 | オプション | オプション | 照会数(デフォルト値：15)|

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

|値|	タイプ|	説明|
|---|---|---|
|header.isSuccessful|	Boolean|	成否|
|header.resultCode|	Integer|	失敗コード|
|header.resultMessage|	String|	失敗メッセージ|
|body.pageNum|	Integer|	ページ番号|
|body.pageSize|	Integer|	照会数|
|body.totalCount|	Integer|	総件数|
|body.data[].tagId| String | タグID |
|body.data[].tagName| String | タグ名 |
|body.data[].createdDate| String | 作成日時 |
|body.data[].tagId| String | 修正日時 |

### タグ登録

[URL]

```
POST /sms/v2.0/appKeys/{appKey}/tags
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のアプリケーションキー|

[Request body]

```json
{
  "tagName": "TAG"
}
```

|値|	タイプ| 最大長さ |	必須|	説明|
|---|---|---|---|---|
| tagName | String | 30 | 必須 | タグ名 |

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

|値|	タイプ|	説明|
|---|---|---|
|header.isSuccessful|	Boolean|	成否|
|header.resultCode|	Integer|	失敗コード|
|header.resultMessage|	String|	失敗メッセージ|
|body.data.tagId| String | タグID |

### タグ 수정

[URL]

```
PUT /sms/v2.0/appKeys/{appKey}/tags/{tagId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のアプリケーションキー|
|tagId|	String|	タグID|

[Request body]

```json
{
  "tagName": "TAG"
}
```

|値|	タイプ| 最大長さ |	必須|	説明|
|---|---|---|---|---|
| tagName | String | 30 | 必須 | タグ名 |

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

|値|	タイプ|	説明|
|---|---|---|
|header.isSuccessful|	Boolean|	成否|
|header.resultCode|	Integer|	失敗コード|
|header.resultMessage|	String|	失敗メッセージ|

### タグ 삭제

[URL]

```
DELETE /sms/v2.0/appKeys/{appKey}/tags/{tagId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のアプリケーションキー|
|tagId|	String|	タグID|

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

|値|	タイプ|	説明|
|---|---|---|
|header.isSuccessful|	Boolean|	成否|
|header.resultCode|	Integer|	失敗コード|
|header.resultMessage|	String|	失敗メッセージ|


## UIDの管理

### UIDの照会

#### リクエスト

[URL]

```
GET /sms/v2.0/appKeys/{appKey}/uids?wheres={wheres}&offsetUid={offsetUid}&offset={offset}&limit={limit}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のアプリケーションキー|

[Query parameter]

|値|	タイプ| 最大長さ |	必須|	説明|
|---|---|---|---|---|
|wheres|	List<String>|	- | オプション | 照会条件。<br/>英数字、括弧で構成された文字列。<br/>括弧は1個、AND、ORは3個まで使用できる。<br/>(例) tagId1,AND,tagId2|
|offsetUid|	String|	- | オプション | offset UID|
|offset | Integer | - | オプション | offset(Default : 0)|
|limit | Integer | 1000 | オプション | 照会件数(Default：15)|

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
            "isLast": false,
            "totalCount": 5
        }
    }
}
```

|値|	タイプ|	説明|
|---|---|---|
|header.isSuccessful|	Boolean|	成否|
|header.resultCode|	Integer|	失敗コード|
|header.resultMessage|	String|	失敗メッセージ|
|body.data.uids[].uid| String | UID |
|body.data.uids[].tags[].tagId| String | タグID |
|body.data.uids[].tags[].tagName| String | タグ名 |
|body.data.uids[].tags[].createdDate| String | タグ 作成日時 |
|body.data.uids[].tags[].updatedDate| String | タグ 修正日時 |
|body.data.uids[].contacts[].contactType| String | 連絡先タイプ |
|body.data.uids[].contacts[].contact| String | 連絡先(携帯電話番号) |
|body.data.uids[].contacts[].createdDate| String | 連絡先作成日時 |
|body.data.uids[].isLast| Boolean| 最後のリストかどうか |
|body.data.uids[].totalCount| Integer| 総データ件数 |

### UID単件照会

#### リクエスト

[URL]

```
GET /sms/v2.0/appKeys/{appKey}/uids/{uid}
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のアプリケーションキー|
|uid|	String|	UID|

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

|値|	タイプ|	説明|
|---|---|---|
|header.isSuccessful|	Boolean|	成否|
|header.resultCode|	Integer|	失敗コード|
|header.resultMessage|	String|	失敗メッセージ|
|body.data.uid| String | UID |
|body.data.tags[].tagId| String | タグID |
|body.data.tags[].tagName| String | タグ名 |
|body.data.tags[].createdDate| String | タグ 作成日時 |
|body.data.tags[].updatedDate| String | タグ 修正日時 |
|body.data.contacts[].contactType| String | 連絡先タイプ |
|body.data.contacts[].contact| String | 連絡先(携帯電話番号) |
|body.data.contacts[].createdDate| String | 連絡先 作成日時 |

### UIDの登録

[URL]

```
POST /sms/v2.0/appKeys/{appKey}/uids
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のアプリケーションキー|

[Request body]

```json
{
  "uids": [
  {
      "uid": "UID",
      "tagIds": ["ABCD1234"],
      "contacts": [
        {
          "contactType": "PHONE_NUMBER",
          "contact": "0100000000"
        }
      ]
  }]
}
```

|値|	タイプ| 最大長さ |	必須|	説明|
|---|---|---|---|---|
| uid | String | - | 必須 | UID |
| tagIds[] | String | - | 必須 | タグIDリスト |
| contacts[].contactType | String | - | 必須 | 連絡先タイプ(PHONE_NUMBER) |
| contacts[].contact | String | - | 必須 | 連絡先 (携帯電話番号) |

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

|値|	タイプ|	説明|
|---|---|---|
|header.isSuccessful|	Boolean|	成否|
|header.resultCode|	Integer|	失敗コード|
|header.resultMessage|	String|	失敗メッセージ|


### UIDの削除

[URL]

```
DELETE /sms/v2.0/appKeys/{appKey}/uids/{uid}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のアプリケーションキー|
|uid|	String|	UID|

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

|値|	タイプ|	説明|
|---|---|---|
|header.isSuccessful|	Boolean|	成否|
|header.resultCode|	Integer|	失敗コード|
|header.resultMessage|	String|	失敗メッセージ|

### 携帯電話番号 등록

[URL]

```
POST /sms/v2.0/appKeys/{appKey}/uids/{uid}/phone-numbers
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のアプリケーションキー|
|uid | String | UID |

[Request body]

```json
{
  "phoneNumber": "0100000000"
}
```

|値|	タイプ| 最大長さ |	必須|	説明|
|---|---|---|---|---|
| phoneNumber| String | - | 必須 | 携帯電話番号 |


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

|値|	タイプ|	説明|
|---|---|---|
|header.isSuccessful|	Boolean|	成否|
|header.resultCode|	Integer|	失敗コード|
|header.resultMessage|	String|	失敗メッセージ|

### 携帯電話番号 삭제

[URL]

```
DELETE /sms/v2.0/appKeys/{appKey}/uids/{uid}/phone-numbers/{phoneNumber}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|値|	タイプ|	説明|
|---|---|---|
|appKey|	String|	固有のアプリケーションキー|
|uid | String | UID |
|phoneNumber | String | 携帯電話番号 |

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

|値|	タイプ|	説明|
|---|---|---|
|header.isSuccessful|	Boolean|	成否|
|header.resultCode|	Integer|	失敗コード|
|header.resultMessage|	String|	失敗メッセージ|