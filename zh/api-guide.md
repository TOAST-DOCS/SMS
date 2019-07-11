## Notification > SMS > API v2.2 Guide

## v2.2 API Overview  

### Changes from v 2.1
1) 템플릿으로 발송 요청 시 요청 파라미터의 우선순위가 높아지도록 수정되었습니다.
   - 예시) 템플릿으로 발송 요청 시 요청 파라미터에 제목, 본문, 발신번호, 첨부파일이 포함된 경우 템플릿에 저장된 데이터를 사용되지 않도록 수정
2) 문자 발송시 제목/본문에 대한 유효성 검사 강화
    * 아래와 같이 문자 발송시 제목/본문 길이 제한이 강화됩니다.
    * SMS(본문: 최대 255자), LMS/MMS(제목: 최대 120자, 본문: 최대 4000자)

### [API Domain]

|Environment| Domain |
|---|---|
|Real|	https://api-sms.cloud.toast.com|

<span id="precautions"></span>
### [Caution]
* SMS is a short message with 90 or less bytes for the text body , while MMS cannot have more than 2,000 bytes for body, and 40 bytes or less for title. If more bytes are sent than specified, messages may be cut. </br>
   * 예시) SMS 발송일 경우, 요청 파라미터 내 본문 길이를 255자 이하로 입력하여 요청할 수 있지만 단말기 수신 시 90 바이트 이하로 잘린 내용으로 수신됩니다.</br>
* Body and title are sent in the euc-kr standard. Therefore, emoticons that are not supported by the euc-kr encoding shall fail in delivery. </br>

## Short SMS

### Send Short SMS

#### Request

[URL]

```
POST  /sms/v2.2/appKeys/{appKey}/sender/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Request body]

```
{
   "templateId":"TemplateId",
   "body":"Body",
   "sendNo":"15446859",
   "requestDate":"2018-08-10 10:00",
   "senderGroupingKey":"SenderGroupingKey",
   "recipientList":[
      {
         "recipientNo":"01000000000",
         "countryCode":"82",
         "internationalRecipientNo":"821000000000",
         "templateParameter":{
            "key":"value"
         },
         "recipientGroupingKey":"recipientGroupingKey"
      }
   ],
   "userId":"UserId"
}
```

|Value| Type | Max Length | Required | Description |
|---|---|---|---|---|
|templateId|	String | 50 |	X| Delivery template ID |
|body|	String| 255 [[Precautions](./api-guide/#precautions)] |	O|	Body |
|sendNo|	String| 13 |	O| Sender number |
|requestDate| String| - | X | Request date and time (yyyy-MM-dd HH:mm) |
|senderGroupingKey| String| 100 | X | Sender's group key |
|recipientList[].recipientNo| String| 20 |	O| Recipient number <br/>Available in combination with country code <br/>Up to 1,000 |
|recipientList[].countryCode|	String| 8 |	X|	Country code [Default: 82 (Korea)] |
|recipientList[].internationalRecipientNo| String|  20 | X| Recipient number including country code<br/>e.g.) 821012345678<br/>To be ignored if recipientNo is available.<br/> |
|recipientList[].templateParameter|	Object|  - |	X| Template parameter (with the input of template ID) |
|recipientList[].templateParameter.{key}| String| - |	X| Replacement key (##key##) |
|recipientList[].templateParameter.{value}|	Object| - |	X| Value which is mapped for replacement key |
|recipientList[].recipientGroupingKey| String| 100 | X | Recipient group key |
|userId|	String|	100 | X | Delivery delimiter e.g) admin,system |

#### Response

```
{
   "header":{
      "isSuccessful":true,
      "resultCode":0,
      "resultMessage":"SUCCESS"
   },
   "body":{
      "data":{
         "requestId":"20180810100630ReZQ6KZzAH0",
         "statusCode":"2",
         "senderGroupingKey":"SenderGroupingKey",
         "sendResultList":[
            {
               "recipientNo":"01000000000",
               "resultCode":0,
               "resultMessage":"SUCCESS",
               "recipientSeq":1,
               "recipientGroupingKey":"RecipientGroupingKey"
            }
         ]
      }
   }
}
```

|Value| Type | Description |
|---|---|---|
|header.isSuccessful|	Boolean| Successful or not |
|header.resultCode|	Integer| Failure code |
|header.resultMessage|	String| Failure message |
|body.data.requestId|	String| Request ID |
|body.data.statusCode|	String| Request status code (1:Requesting, 2:Request completed, 3:Request failed) |
|body.data.senderGroupingKey|	String| Sender group key |
|body.data.sendResultList[].recipientNo| String | Recipient number |
|body.data.sendResultList[].resultCode| Integer | Result code |
|body.data.sendResultList[].resultMessage| String | Result message |
|body.data.sendResultList[].recipientSeq| Integer | Recipient sequence (mtPr) |
|body.data.sendResultList[].recipientGroupingKey| String | Recipient group key |

#### Example of Sending Short SMS (general domestic recipient numbers)

| Http metho | URL |
| - | - |
| POST | https://api-sms.cloud.toast.com/sms/v2.2/appKeys/{appKey}/sender/sms|

[Request body]
```
{
   "body":"Body",
   "sendNo":"15446859",
   "senderGroupingKey":"SenderGroupingKey",
   "recipientList":[
      {
         "recipientNo":"01000000000",
         "recipientGroupingKey":"RecipientGroupingKey"
      },
      {
         "recipientNo":"01000000001",
         "recipientGroupingKey":"RecipientGroupingKey2"
      }
   ]
}
```

[Response]
```
{
   "header":{
      "isSuccessful":true,
      "resultCode":0,
      "resultMessage":"SUCCESS"
   },
   "body":{
      "data":{
         "requestId":"20180810100630ReZQ6KZzAH0",
         "statusCode":"2",
         "senderGroupingKey":"SenderGroupingKey",
         "sendResultList":[
            {
               "recipientNo":"01000000000",
               "resultCode":0,
               "resultMessage":"SUCCESS",
               "recipientSeq":1,
               "recipientGroupingKey":"RecipientGroupingKey"
            },
            {
               "recipientNo":"01000000001",
               "resultCode":0,
               "resultMessage":"SUCCESS",
               "recipientSeq":2,
               "recipientGroupingKey":"RecipientGroupingKey2"
            }
         ]
      }
   }
}
```

[curl]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-sms.cloud.toast.com/sms/v2.2/appKeys/{appKey}/sender/sms -d '{"body": "{body message}","sendNo": "15446859","senderGroupingKey":"SenderGroupingKey","recipientList":[{"recipientNo": "01000000000","recipientGroupingKey":"RecipientGroupingKey"},{"recipientNo": "01000000002","recipientGroupingKey":"RecipientGroupingKey2"}]}'
```

#### Example of Sending Short SMS (with country code included to recipient numbers)

| Http metho | URL |
| - | - |
| POST | https://api-sms.cloud.toast.com/sms/v2.2/appKeys/{appKey}/sender/sms|


[Request body]
```
{
   "body":"Body",
   "sendNo":"15446859",
   "senderGroupingKey":"SenderGroupingKey",
   "recipientList":[
      {
         "internationalRecipientNo":"821000000000",
         "recipientGroupingKey":"RecipientGroupingKey"
      }
   ],
   "userId":""
}
```

[Response]
```
{
   "header":{
      "isSuccessful":true,
      "resultCode":0,
      "resultMessage":"SUCCESS"
   },
   "body":{
      "data":{
         "requestId":"20180810100630ReZQ6KZzAH0",
         "statusCode":"2",
         "senderGroupingKey":"SenderGroupingKey",
         "sendResultList":[
            {
               "recipientNo":"01000000000",
               "resultCode":0,
               "resultMessage":"SUCCESS",
               "recipientSeq":1,
               "recipientGroupingKey":"RecipientGroupingKey"
            }
         ]
      }
   }
}
```

[curl]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-sms.cloud.toast.com/sms/v2.2/appKeys/{appKey}/sender/sms -d '{"body": "body","sendNo": "15446859","recipientList": [{"internationalRecipientNo": "821000000000"}]}'
```

### List Delivery of Short SMS 

#### Request

[URL]

```
GET  /sms/v2.2/appKeys/{appKey}/sender/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Query parameter] No.1 or 2 is conditionally required 

|Value| Type |	Max Length | Required | Description |
|---|---|---|---|---|
|requestId|	String| 25 |	Conditionally required (no.1) | Request ID |
|startRequestDate|	String| - |	Conditionally required (no. 2) | Start date of delivery (yyyy-MM-dd HH:mm:ss) |
|endRequestDate|	String| - |	Conditionally required (no. 2) | End date of delivery (yyyy-MM-dd HH:mm:ss) |
|startResultDate|	String| - | Optional | Start date of receiving (yyyy-MM-dd HH:mm:ss) |
|endResultDate|	String| - | Optional | End date of receiving (yyyy-MM-dd HH:mm:ss) |
|sendNo|	String| 13 | Optional | Sender number |
|recipientNo|	String| 20 | Optional | Recipient number |
|templateId|	String| 50 | Optional | Template number |
|msgStatus|	String| 1 | Optional | Message status code (1:requesting, 2: processing, 3: successful) |
|resultCode|	String| 10 | Optional | Result code of receiving [[Table on Query Codes](./error-code/#_2)] |
|subResultCode|	String| 10 | Optional | Detail result code of receiving [[Table on Query Codes](./error-code/#_3)] |
|senderGroupingKey|	String| 100 | Optional | Sender's group key |
|recipientGroupingKey|	String| 100 | Optional | Recipient's group key |
|pageNum|	Integer| - | Optional | Page number (default : 1) |
|pageSize|	Integer| 1000 | Optional | Number of queries (default: 15) |

#### Response

```
{
   "header":{
      "resultCode":0,
      "resultMessage":"SUCCESS",
      "isSuccessful":true
   },
   "body":{
      "pageNum":1,
      "pageSize":15,
      "totalCount":1,
      "data":[
         {
            "requestId":"20180810100630ReZQ6KZzAH0",
            "requestDate":"2018-08-10 10:06:30.0",
            "resultDate":"2018-08-10 10:06:42.0",
            "templateId":"TemplateId",
            "templateName":"template name",
            "categoryId":0,
            "categoryName":"category name",
            "body":"single-text test",
            "sendNo":"15446859",
            "countryCode":"82",
            "recipientNo":"01000000000",
            "msgStatus":"3",
            "msgStatusName":"successful",
            "resultCode":"1000",
            "resultCodeName":"successful",
            "telecomCode":10001,
            "telecomCodeName":"SKT",
            "mtPr":"1",
            "sendType":"0",
            "userId":"tester",
            "adYn":"N",
            "resultMessage": "",
            "senderGroupingKey":"SenderGroupingKey",
            "recipientGroupingKey":"RecipientGroupingKey"
         }
      ]
   }
}
```

|Value| Type | Description |
|---|---|---|
|header.isSuccessful|	Boolean| Successful or not |
|header.resultCode|	Integer| Failure code |
|header.resultMessage|	String| Failure message |
|body.pageNum|	Integer| Current page number |
|body.pageSize|	Integer| Number of queried data |
|body.totalCount|	Integer| Number of total data |
|body.data[].requestId|	String| Request ID |
|body.data[].requestDate|	String| Date and time of sending |
|body.data[].resultDate|	String| Date and time of receiving |
|body.data[].templateId|	String| Template ID |
|body.data[].templateName|	String| Template name |
|body.data[].categoryId|	String| Category ID |
|body.data[].categoryName|	String| Category name |
|body.data[].body|	String| Body |
|body.data[].sendNo|	String| Sender number |
|body.data[].countryCode|	String| Country code |
|body.data[].recipientNo|	String| Recipient number |
|body.data[].msgStatus|	String| Message status code |
|body.data[].msgStatusName|	String| Name of message status code |
|body.data[].resultCode|	String| Result code of receiving [[Table on Result Code of Receiving](./error-code/#emma-v3)] |
|body.data[].resultCodeName|	String| Result code name of receiving |
|body.data[].telecomCode|	Integer| Code of telecom provider |
|body.data[].telecomCodeName|	String| Name of telecom provider |
|body.data[].mtPr|	Integer| Detail delivery ID (required to query details) |
|body.data[].sendType|	String| Delivery type (0:Sms, 1:Lms/Mms, 2:Auth) |
|body.data[].userId|	String| Delivery request ID |
|body.data[].adYn|	String| Ad or not |
|body.data[].senderGroupingKey|	String| Sender's group key |
|body.data[].recipientGroupingKey|	String| Recipient's group key |

### Query Delivery of Short SMS 

#### Request

[URL]

```
GET  /sms/v2.2/appKeys/{appKey}/sender/sms/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |
|requestId|	String| Request ID |

[Query parameter]

|Value| Type | Required | Description |
|---|---|---|---|
|mtPr|	Integer| Required | Detail delivery ID |

#### Response

```
{
   "header":{
      "resultCode":0,
      "resultMessage":"SUCCESS",
      "isSuccessful":true
   },
   "body":{
      "data":{
         "requestId":"20180810100630ReZQ6KZzAH0",
         "requestDate":"2018-08-10 10:06:30.0",
         "resultDate":"2018-08-10 10:06:42.0",
         "templateId":"TemplateId",
         "templateName":"template name",
         "categoryId":0,
         "categoryName":"Category name",
         "body":"body",
         "sendNo":"15446859",
         "countryCode":"82",
         "recipientNo":"01000000000",
         "msgStatus":"3",
         "msgStatusName":"successful",
         "resultCode":"1000",
         "resultCodeName":"successful",
         "telecomCode":10001,
         "telecomCodeName":"SKT",
         "mtPr":"1",
         "sendType":"0",
         "userId":"tester",
         "adYn":"N",
         "resultMessage": "",
         "senderGroupingKey":"SenderGroupingKey",
         "recipientGroupingKey":"RecipientGroupingKey"
      }
   }
}
```

|Value| Type | Description |
|---|---|---|
|header.isSuccessful|	Boolean| Successful or not |
|header.resultCode|	Integer| Failure code |
|header.resultMessage|	String| Failure message |
|body.data.requestId|	String| Request ID |
|body.data.requestDate|	String| Date and time of sending |
|body.data.resultDate|	String| Date and time of receiving |
|body.data.templateId|	String| Template ID |
|body.data.templateName|	String| Template name |
|body.data.categoryId|	String| Category ID |
|body.data.categoryName|	String| Category name |
|body.data.body|	String| Body |
|body.data.sendNo|	String| Sender name |
|body.data.countryCode|	String| Country code |
|body.data.recipientNo|	String| Recipient number |
|body.data.msgStatus|	String| Message status code |
|body.data.msgStatusName|	String| Name of message status code |
|body.data.resultCode|	String| Result code of receiving [[Table on Result Code of Receiving](./error-code/#emma-v3)] |
|body.data.resultCodeName|	String| Result code name of receiving |
|body.data.telecomCode|	Integer| Telecom provider code |
|body.data.telecomCodeName|	String| Telecom provider name |
|body.data.mtPr|	Integer| Detail delivery ID (required to query details) |
|body.data.sendType|	String| Delivery type (0:Sms, 1:Lms/Mms, 2:Auth) |
|body.data.userId|	String| Delivery request ID |
|body.data.adYn|	String| Ad or not |
|body.data.senderGroupingKey|	String| Sender's group key |
|body.data.recipientGroupingKey|	String| Recipient's group key |

## Long MMS

### Send Long MMS (attached file excluded)

#### Request

[URL]

```
POST  /sms/v2.2/appKeys/{appKey}/sender/mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Request body]

```
{
   "templateId":"TemplateId",
   "title":"Title",
   "body":"Body",
   "sendNo":"15446859",
   "requestDate":"2018-08-10 10:00",
   "senderGroupingKey":"SenderGroupingKey",
   "recipientList":[
      {
         "recipientNo":"01000000000",
         "countryCode":"82",
         "internationalRecipientNo":"821000000000",
         "templateParameter":{
            "key":"value"
         },
         "recipientGroupingKey":"recipientGroupingKey"
      }
   ],
   "userId":"UserId"
}
```

|Value| Type | Maximum Length | Required | Description |
|---|---|---|---|---|
|templateId|	String| 50 |	X| Delivery template ID |
|title|	String| 40 bytes (in EUC-KR) |	O| Title |
|body|	String| 2000 bytes (in EUC-KR) |	O|	Body |
|sendNo|	String| 13 |	O| Sender number |
|requestDate| String| - | X | Request date and time(yyyy-MM-dd HH:mm) |
|senderGroupingKey| String| 100 | X | Sender's group key |
|recipientList[].recipientNo| String| 20 |	O| Recipient number<br/>Available in combination of countryCode |
|recipientList[].countryCode| String| 8 |	X|	Country code [Default: 82 (Korea)] |
|recipientList[].internationalRecipientNo| String| 20 | X| Recipient number including country code<br/>e.g.) 821012345678<br/>To be ignored if recipientNo is available.<br/> |
|recipientList[].templateParameter|	Object| - |	X| Template parameter (with the input of template ID) |
|recipientList[].templateParameter.{key}|	String| - |	X| Replacement key (##key##) |
|recipientList[].templateParameter.{value}|	Object| - |	X| Value which is mapped for replacement key |
|recipientList[].recipientGroupingKey| String| 1000 | X | Recipient group key |
|userId|	String| 100 |	X | Delivery delimiter  e.g.) admin,system |

#### Response

```
{
   "header":{
      "isSuccessful":true,
      "resultCode":0,
      "resultMessage":"SUCCESS"
   },
   "body":{
      "data":{
         "requestId":"20180810100630ReZQ6KZzAH0",
         "statusCode":"2",
         "senderGroupingKey":"SenderGroupingKey",
         "sendResultList":[
            {
               "recipientNo":"01000000000",
               "resultCode":0,
               "resultMessage":"SUCCESS",
               "recipientSeq":1,
               "recipientGroupingKey":"RecipientGroupingKey"
            }
         ]
      }
   }
}
```

|Value| Type | Description |
|---|---|---|
|header.isSuccessful|	Boolean| Successful or not |
|header.resultCode|	Integer| Failure code |
|header.resultMessage|	String| Failure message |
|body.data.requestId|	String| Request ID |
|body.data.statusCode|	String| Request status code (1: requesting, 2:request completed, 3: request failed) |
|body.data.senderGroupingKey|	String| Sender's group key |
|body.data.sendResultList[].recipientNo| String | Recipient number |
|body.data.sendResultList[].resultCode| Integer | Result code |
|body.data.sendResultList[].resultMessage| String | Result message |
|body.data.sendResultList[].recipientSeq| Integer | Recipient sequence (mtPr) |
|body.data.sendResultList[].recipientGroupingKey| String | Recipient's group key |

#### Example of Sending Long MMS 

| Http metho | URL |
| - | - |
| POST | https://api-sms.cloud.toast.com/sms/v2.2/appKeys/{appKey}/sender/mms|

[Request body]
```
{
   "title": "Title",
   "body":"Body",
   "sendNo":"15446859",
   "senderGroupingKey":"SenderGroupingKey",
   "recipientList":[
      {
         "recipientNo":"01000000000",
         "recipientGroupingKey":"RecipientGroupingKey"
      },
      {
         "recipientNo":"01000000001",
         "recipientGroupingKey":"RecipientGroupingKey2"
      }
   ]
}
```

[Response]
```
{
   "header":{
      "isSuccessful":true,
      "resultCode":0,
      "resultMessage":"SUCCESS"
   },
   "body":{
      "data":{
         "requestId":"20180810100630ReZQ6KZzAH0",
         "statusCode":"2",
         "senderGroupingKey":"SenderGroupingKey",
         "sendResultList":[
            {
               "recipientNo":"01000000000",
               "resultCode":0,
               "resultMessage":"SUCCESS",
               "recipientSeq":1,
               "recipientGroupingKey":"RecipientGroupingKey"
            },
            {
               "recipientNo":"01000000001",
               "resultCode":0,
               "resultMessage":"SUCCESS",
               "recipientSeq":2,
               "recipientGroupingKey":"RecipientGroupingKey2"
            }
         ]
      }
   }
}
```

[curl]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-sms.cloud.toast.com/sms/v2.2/appKeys/{appKey}/sender/mms -d '{"title": "{title}","body": "{body message}","sendNo": "{sender number}","recipientList": [{"recipientNo": "{recipient number}","templateParameter": { }}],"userId": ""}'
```

### Send MMS (attached file included)

#### Upload Attached Files

#### Request

[URL]

```
POST  /sms/v2.2/appKeys/{appKey}/attachfile/binaryUpload
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Request body]

```
{
    "fileName": "attachment.jpg",
    "createUser": "CreateUser",
    // "fileBody": [0,10,16]
    "fileBody": "{byte[] -> encoded value in Base64}"
}
```

|Value| Type | Max Length | Required | Description |
|---|----|---|----|---|
|fileName|	String|	45 | Required | File name (extensions available only in jpg or jpeg (small-case letters)) |
|fileBody|	Byte[]| 300K | Required | File byte[] value encoded in Base64.<br/>* or byte arrangement value |
|createUser|	String|	100 | Required | File uploading user information |

#### Response

```
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

|Value| Type | Description |
|---|---|---|
|header.isSuccessful|	Boolean| Successful or not |
|header.resultCode|	Integer| Failure code |
|header.resultMessage|	String| Failure message |
|body.data.fileId|	Integer| File ID |
|body.data.fileName|	String| File name |
|body.data.filePath|	String| Default path of attached file <br/> (https://domain/attachFile/filePath/fileName) |

#### Example of Uploading Attached Files

| Http method | URL |
| - | - |
| POST | https://api-sms.cloud.toast.com/sms/v2.2/appKeys/{appKey}/attachfile/binaryUpload |

[Request body]
```
{
  "fileName": "attachement.jpg",
  "createUser": "CreateUser",
  "fileBody": "/9j/4AAQSkZJRgABAQEAYABgAAD/4RDSRXhpZgAATU0AKgAAAAgABAE7AAIAAAAESkdHAIdpAAQAAAABAAAISpydAAEAAAAIAAAQwuocAAcAAAgMAAAAPgAAAAAc6gAAAAgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAFkAMAAgAAABQAABCYkAQAAgAAABQAABCskpEAAgAAAAM1MgAAkpIAAgAAAAM1MgAA6hwABwAACAwAAAiMAAAAABzqAAAACAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAMjAxODowMjowOCAxNjoyOTo0MQAyMDE4OjAyOjA4IDE2OjI5OjQxAAAASgBHAEcAAAD/4QsWaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wLwA8P3hwYWNrZXQgYmVnaW49J++7vycgaWQ9J1c1TTBNcENlaGlIenJlU3pOVGN6a2M5ZCc/Pg0KPHg6eG1wbWV0YSB4bWxuczp4PSJhZG9iZTpuczptZXRhLyI+PHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj48cmRmOkRlc2NyaXB0aW9uIHJkZjphYm91dD0idXVpZDpmYWY1YmRkNS1iYTNkLTExZGEtYWQzMS1kMzNkNzUxODJmMWIiIHhtbG5zOmRjPSJodHRwOi8vcHVybC5vcmcvZGMvZWxlbWVudHMvMS4xLyIvPjxyZGY6RGVzY3JpcHRpb24gcmRmOmFib3V0PSJ1dWlkOmZhZjViZGQ1LWJhM2QtMTFkYS1hZDMxLWQzM2Q3NTE4MmYxYiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIj48eG1wOkNyZWF0ZURhdGU+MjAxOC0wMi0wOFQxNjoyOTo0MS41MTc8L3htcDpDcmVhdGVEYXRlPjwvcmRmOkRlc2NyaXB0aW9uPjxyZGY6RGVzY3JpcHRpb24gcmRmOmFib3V0PSJ1dWlkOmZhZjViZGQ1LWJhM2QtMTFkYS1hZDMxLWQzM2Q3NTE4MmYxYiIgeG1sbnM6ZGM9Imh0dHA6Ly9wdXJsLm9yZy9kYy9lbGVtZW50cy8xLjEvIj48ZGM6Y3JlYXRvcj48cmRmOlNlcSB4bWxuczpyZGY9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkvMDIvMjItcmRmLXN5bnRheC1ucyMiPjxyZGY6bGk+SkdHPC9yZGY6bGk+PC9yZGY6U2VxPg0KCQkJPC9kYzpjcmVhdG9yPjwvcmRmOkRlc2NyaXB0aW9uPjwvcmRmOlJERj48L3g6eG1wbWV0YT4NCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIAogICAgICAgICAgICAgICAgICAgICAgICAgICAgPD94cGFja2V0IGVuZD0ndyc/Pv/bAEMABwUFBgUEBwYFBggHBwgKEQsKCQkKFQ8QDBEYFRoZGBUYFxseJyEbHSUdFxgiLiIlKCkrLCsaIC8zLyoyJyorKv/bAEMBBwgICgkKFAsLFCocGBwqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKioqKv/AABEIABoAHQMBIgACEQEDEQH/xAAfAAABBQEBAQEBAQAAAAAAAAAAAQIDBAUGBwgJCgv/xAC1EAACAQMDAgQDBQUEBAAAAX0BAgMABBEFEiExQQYTUWEHInEUMoGRoQgjQrHBFVLR8CQzYnKCCQoWFxgZGiUmJygpKjQ1Njc4OTpDREVGR0hJSlNUVVZXWFlaY2RlZmdoaWpzdHV2d3h5eoOEhYaHiImKkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4eLj5OXm5+jp6vHy8/T19vf4+fr/xAAfAQADAQEBAQEBAQEBAAAAAAAAAQIDBAUGBwgJCgv/xAC1EQACAQIEBAMEBwUEBAABAncAAQIDEQQFITEGEkFRB2FxEyIygQgUQpGhscEJIzNS8BVictEKFiQ04SXxFxgZGiYnKCkqNTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNkZWZnaGlqc3R1dnd4eXqCg4SFhoeIiYqSk5SVlpeYmZqio6Slpqeoqaqys7S1tre4ubrCw8TFxsfIycrS09TV1tfY2dri4+Tl5ufo6ery8/T19vf4+fr/2gAMAwEAAhEDEQA/APD6K6e88IxWemTamdQaSx+zW8trJ5GDPJLkGMjd8u0pKCcn7nTmq/jDTbHSvH2qadao1vY2940aqmZGRAe24/MQPU8+tVs7ev4f8OT0uYFFdvrmhWmo/Ee30wXVpp9rcWlvIs0FmIAQbZXX90ZCPMbgY34LHqM1zes6bbaXq81mHv18rAK3tiLeZWxyGj3tt/M5HpTWrsGyv/Wpb1bXIp/DekaLYzXEsFj5kztcRhP3shGVUBmyq44ORksxwM1JrfiyLXtSa/u/DmkpdSXHnzyRPdDz/VWBmIAPfaFPoRXO0UgOk1Pxemq6vb3914d0jMMAgeFftBjmRYxGgbMpYFVUYKlTnqTWfruuz69dQSTW8FrFa26W1vb24bZFGucKC7Mx5JOWYnn0wKy6KAP/2Q=="
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
            "fileId": 0,
            "fileName": "attachment.jpg",
            "filePath": "0/toast-mt-2018-08-10/1609/178576"
        }
    }
}
```

#### Example of Sending Attached Files

| Http method | URL |
| - | - |
| POST | https://api-sms.cloud.toast.com/sms/v2.2/appKeys/{appKey}/sender/mms |

[Request body]
```
{
    "title": "Title",
    "body": "Body",
    "sendNo": "15446859",
    "senderGroupingKey": "SenderGrouping",
    "attachFileIdList": [0],
    "recipientList": [{
        "recipientNo": "01010000000",
        "recipientGroupingKey":"RecipientGroupingKey"
    }]
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
      "requestId": "1-201607-424631-1",
      "statusCode": "2",
      "senderGroupingKey": "SenderGrouping",
      "sendResultList" : [
          {
              "recipientNo" : {recipient number},
              "resultCode" :  0,
              "resultMessage" : "SUCCESS"
              "recipientSeq": 1,
              "recipientGroupingKey":"RecipientGroupingKey"
          }
      ]
    }
  }
}
```

### List Delivery of Long MMS Request

[URL]

```
GET  /sms/v2.2/appKeys/{appKey}/sender/mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Query parameter] No. 1 or 2 conditionally required

|Value| Type | Max Length | Required | Description |
|---|---|---|---|---|
|requestId|	String| 25 |	Conditionally required (no.1) | Request ID |
|startRequestDate|	String| - |	Conditionally required (no. 2) | Start date of sending (yyyy-MM-dd HH:mm:ss) |
|endRequestDate|	String| - |	Conditionally required (no. 2) | End date of sending (yyyy-MM-dd HH:mm:ss) |
|startResultDate|	String| - | Optional | Start date of receiving (yyyy-MM-dd HH:mm:ss) |
|endResultDate|	String| - | Optional | End date of receiving (yyyy-MM-dd HH:mm:ss) |
|sendNo|	String| 13 | Optional | Sender number |
|recipientNo|	String| 20 | Optional | Recipient numbe |
|templateId|	String| 50 | Optional | Template number |
|msgStatus|	String| 1 | Optional | Message status code (1:requesting, 2:processing, 3:successful) |
|resultCode|	String| 10 | Optional | Result code of receiving [[Table on Query Codes](./error-code/#_2)] |
|subResultCode|	String| 10 | Optional | Detail result code of receiving [[Table on Query Codes](./error-code/#_3)] |
|senderGroupingKey|	String| 100 | Optional | Sender's group key |
|recipientGroupingKey|	String| 100 | Optional | Recipient's group key |
|pageNum|	Integer| - | Optional | Page number (default : 1) |
|pageSize|	Integer| 1000 | Optional | Number of queries (default: 15) |

#### Response

```
{
  "header":{
    "resultCode":0,
    "resultMessage":"SUCCESS",
    "isSuccessful":true
  },
  "body":{
    "pageNum":1,
    "pageSize":15,
    "totalCount":1,
    "data":[
      {
        "requestId":"20180810100630ReZQ6KZzAH0",
        "requestDate":"2018-08-10 10:06:30.0",
        "resultDate":"2018-08-10 10:06:42.0",
        "templateId":"TemplateId",
        "templateName":"template name",
        "categoryId":0,
        "categoryName":"category name",
        "title":"title",
        "body":"body",
        "sendNo":"15446859",
        "countryCode":"82",
        "recipientNo":"01000000000",
        "msgStatus":"3",
        "msgStatusName":"successful",
        "resultCode":"1000",
        "resultCodeName":"successful",
        "telecomCode":10001,
        "telecomCodeName":"SKT",
        "mtPr":"1",
        "sendType":"0",
        "userId":"tester",
        "adYn":"N",
        "attachFileList":[
          {
            "fileId":0,
            "filePath":"26606/toast-mt-2018-02-08/1639/107303/",
            "fileName":"small_img.jpg"
          }
        ],
        "resultMessage":"",
        "senderGroupingKey":"SenderGroupingKey",
        "recipientGroupingKey":"RecipientGroupingKey"
      }
    ]
  }
}
```

|Value| Type | Description |
|---|---|---|
|header.isSuccessful|	Boolean| Successful or not |
|header.resultCode|	Integer| Failure code |
|header.resultMessage|	String| Failure message |
|body|	Object| Body area |
|body.pageNum|	Integer| Current page number |
|body.pageSize|	Integer| Queried data count |
|body.totalCount|	Integer| Total data count |
|body.data[].requestId|	String| Request ID |
|body.data[].requestDate|	String| Date and time of request |
|body.data[].resultDate|	String| Date and time of receiving |
|body.data[].templateId|	String| Template ID |
|body.data[].templateName|	String| Template name |
|body.data[].categoryId|	String| Category ID |
|body.data[].categoryName|	String| Category name |
|body.data[].body|	String| Body message |
|body.data[].sendNo|	String| Sender number |
|body.data[].countryCode|	String| Country code |
|body.data[].recipientNo|	String| Recipient number |
|body.data[].msgStatus|	String| Message status code |
|body.data[].msgStatusName|	String| Name of message status code |
|body.data[].resultCode|	String| Result code of receiving [[Table on Result Code of Receiving](./error-code/#emma-v3)] |
|body.data[].resultCodeName|	String| Result code name of receiving |
|body.data[].telecomCode|	Integer| Code of telecom provider |
|body.data[].telecomCodeName|	String| Name of telecom provider |
|body.data[].mtPr|	Integer| Detail delivery ID (required to query details) |
|body.data[].sendType|	String| Delivery type (0:Sms, 1:Lms/Mms, 2:Auth) |
|body.data[].userId|	String| Delivery request ID |
|body.data[].adYn|	String| Ad or not |
|body.data[].attachFileList[].fileId|	Integer| File ID |
|body.data[].attachFileList[].filePath|	String| Path of file saving (for internal purpose) |
|body.data[].attachFileList[].fileName|	String| File name |
|body.data[].senderGroupingKey|	String| Sender's group key |
|body.data[].recipientGroupingKey|	String| Recipient's group key |


### Query Single Delivery of Long MMS 

#### Request

[URL]

```
GET  /sms/v2.2/appKeys/{appKey}/sender/mms/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Origional appkey |
|requestId|	String| Request ID |

[Query parameter]

|Value| Type | Required | Description |
|---|---|---|---|
|mtPr|	Integer| Required | Detail delivery ID |

#### Response

```
{
  "header":{
    "resultCode":0,
    "resultMessage":"SUCCESS",
    "isSuccessful":true
  },
  "body":{
    "data":{
      "requestId":"20180810100630ReZQ6KZzAH0",
      "requestDate":"2018-08-10 10:06:30.0",
      "resultDate":"2018-08-10 10:06:42.0",
      "templateId":"TemplateId",
      "templateName":"template name",
      "categoryId":0,
      "categoryName":"category name",
      "title":"title",
      "body":"body",
      "sendNo":"15446859",
      "countryCode":"82",
      "recipientNo":"01000000000",
      "msgStatus":"3",
      "msgStatusName":"succssful",
      "resultCode":"1000",
      "resultCodeName":"successful",
      "telecomCode":10001,
      "telecomCodeName":"SKT",
      "mtPr":"1",
      "sendType":"0",
      "userId":"tester",
      "adYn":"N",
      "attachFileList":[
        {
          "fileId":0,
          "filePath":"26606/toast-mt-2018-02-08/1639/107303/",
          "fileName":"small_img.jpg"
        }
      ],
      "resultMessage":"",
      "senderGroupingKey":"SenderGroupingKey",
      "recipientGroupingKey":"RecipientGroupingKey"
    }
  }
}
```

|Value| Type | Description |
|---|---|---|
|header.isSuccessful|	Boolean| Successful or not |
|header.resultCode|	Integer| Failure code |
|header.resultMessage|	String| Failure message |
|body|	Object| Body area |
|body.pageNum|	Integer| Current page number |
|body.pageSize|	Integer| Queried data count |
|body.totalCount|	Integer| Total data count |
|body.data[].requestId|	String| Request ID |
|body.data[].requestDate|	String| Date and time of sending |
|body.data[].resultDate|	String| Date and time of receiving |
|body.data[].templateId|	String| Template ID |
|body.data[].templateName|	String| Template name |
|body.data[].categoryId|	String| Category ID |
|body.data[].categoryName|	String| Category name |
|body.data[].body|	String| Body message |
|body.data[].sendNo|	String| Sender number |
|body.data[].countryCode|	String| Country code |
|body.data[].recipientNo|	String| Recipient number |
|body.data[].msgStatus|	String| Message status code |
|body.data[].msgStatusName|	String| Name of message status code |
|body.data[].resultCode|	String| Result code of receiving [[Table on Result Code of Receiving](./error-code/#emma-v3)] |
|body.data[].resultCodeName|	String| Result code name of receiving |
|body.data[].telecomCode|	Integer| Code of telecom provider |
|body.data[].telecomCodeName|	String| Name of telecome provider |
|body.data[].mtPr|	Integer| Detail delivery ID (required to query details) |
|body.data[].sendType|	String| Delivery type (0:Sms, 1:Lms/Mms, 2:Auth) |
|body.data[].userId|	String| Delivery request ID |
|body.data[].adYn|	String| Ad or not |
|body.data[].attachFileList[].fileId|	Integer| File ID |
|body.data[].attachFileList[].filePath|	String| Path of file saving (for internal purpose) |
|body.data[].attachFileList[].fileName|	String| File name |
|body.data[].senderGroupingKey|	String| Sender's group key |
|body.data[].recipientGroupingKey|	String| Recipient's group key |

## SMS for Authentication (emergency)

### Send SMS for Authentication

#### Request

[URL]

```
POST  /sms/v2.2/appKeys/{appKey}/sender/auth/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Request body]

```
{
   "templateId":"TemplateId",
   "body":"body",
   "sendNo":"15446859",
   "requestDate":"2018-08-10 10:00",
   "senderGroupingKey":"SenderGroupingKey",
   "recipientList":[
      {
         "recipientNo":"01000000000",
         "countryCode":"82",
         "internationalRecipientNo":"821000000000",
         "templateParameter":{
            "key":"value"
         },
         "recipientGroupingKey":"recipientGroupingKey"
      }
   ],
   "userId":"UserId"
}
```

|Value| Type | Max Length | Required | Description |
|---|---|---|---|---|
|templateId|	String| 50 |	X| Delivery template ID |
|body|	String| 255 [[Precautions](./api-guide/#precautions)] |	O|	Body |
|sendNo|	String| 13 |	O| Sender number |
|requestDate| String| - | X | Date and time of schedule (yyyy-MM-dd HH:mm) |
|senderGroupingKey| String| 100 | X | Sender's group key |
|recipientList[].recipientNo|	String| 20 |	O| Recipient number<br/>Available in combination of the country code |
|recipientList[].countryCode|	String| 8 |	X|	Country code [default: 82 (Korea)] |
|recipientList[].internationalRecipientNo| String| 20 | X| Recipient number including country code<br/>e.g.) 821012345678<br/>To be ignored if recipientNo is available<br/> |
|recipientList[].templateParameter|	Object| - |	X| Template parameter (with the input of template ID) |
|recipientList[].templateParameter.{key}| String| - |	X| Replacement key (##key##) |
|recipientList[].templateParameter.{value}| Object| - |	X| Value which is mapped for replacement key |
|recipientList[].recipientGroupingKey| String| 100 | X | Recipient's group key |
|userId|	String| 100 |	X | Delivery delimiter e.g.) admin,system |


#### Response
```
{
   "header":{
      "isSuccessful":true,
      "resultCode":0,
      "resultMessage":"SUCCESS"
   },
   "body":{
      "data":{
         "requestId":"20180810100630ReZQ6KZzAH0",
         "statusCode":"2",
         "senderGroupingKey":"SenderGroupingKey",
         "sendResultList":[
            {
               "recipientNo":"01000000000",
               "resultCode":0,
               "resultMessage":"SUCCESS",
               "recipientSeq":1,
               "recipientGroupingKey":"RecipientGroupingKey"
            }
         ]
      }
   }
}
```

|Value| Type | Description |
|---|---|---|
|header.isSuccessful|	Boolean| Successful or not |
|header.resultCode|	Integer| Failure code |
|header.resultMessage|	String| Failure message |
|body.data.requestId|	String| Request ID |
|body.data.statusCode|	String| Request status code (1: Requesting, 2: Request completed, 3: Request failed) |
|body.data.senderGroupingKey|	String| Sender's group key |
|body.data.sendResultList[].recipientNo| String | Recipient number |
|body.data.sendResultList[].resultCode| Integer | Result code |
|body.data.sendResultList[].resultMessage| String | Result message |
|body.data.sendResultList[].recipientSeq| Integer | Recipient sequence (mtPr) |
|body.data.sendResultList[].recipientGroupingKey| String | Recipient's group key |

#### Example

| Http metho | URL |
| - | - |
| POST | https://api-sms.cloud.toast.com/sms/v2.2/appKeys/{appKey}/sender/auth/sms|

[Request body]
```
{
   "body":"body",
   "sendNo":"15446859",
   "senderGroupingKey":"SenderGroupingKey",
   "recipientList":[
      {
         "recipientNo":"01000000000",
         "recipientGroupingKey":"RecipientGroupingKey"
      },
      {
         "recipientNo":"01000000001",
         "recipientGroupingKey":"RecipientGroupingKey2"
      }
   ]
}
```

[Response]
```
{
   "header":{
      "isSuccessful":true,
      "resultCode":0,
      "resultMessage":"SUCCESS"
   },
   "body":{
      "data":{
         "requestId":"20180810100630ReZQ6KZzAH0",
         "statusCode":"2",
         "senderGroupingKey":"SenderGroupingKey",
         "sendResultList":[
            {
               "recipientNo":"01000000000",
               "resultCode":0,
               "resultMessage":"SUCCESS",
               "recipientSeq":1,
               "recipientGroupingKey":"RecipientGroupingKey"
            },
            {
               "recipientNo":"01000000001",
               "resultCode":0,
               "resultMessage":"SUCCESS",
               "recipientSeq":2,
               "recipientGroupingKey":"RecipientGroupingKey2"
            }
         ]
      }
   }
}
```

[curl]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-sms.cloud.toast.com/sms/v2.2/appKeys/{appKey}/sender/auth/sms -d '{"body": "{Body}","sendNo": "{sender number}","recipientList":[{"recipientNo": "{recipient number}","templateParameter": { }}],"userId": ""}'
```

### List SMS Delivery for Authentication

#### Request

[URL]

```
GET  /sms/v2.2/appKeys/{appKey}/sender/auth/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|----|---|
|appKey|	String| Original appkey |

[Query parameter] No. 1 or 2 is conditionally required 

|Value| Type |	Max Length | Required | Description |
|---|---|---|---|---|
|requestId|	String| 25 |	Conditionally required (no.1) | Request ID |
|startRequestDate|	String| - |	Conditionally required (no.2) | Start date of sending (yyyy-MM-dd HH:mm:ss) |
|endRequestDate|	String| - |	Conditionally required (no.2) | End date of sending (yyyy-MM-dd HH:mm:ss) |
|startResultDate|	String| - | Optional | Start date of receiving (yyyy-MM-dd HH:mm:ss) |
|endResultDate|	String| - | Optional | End date of receiving (yyyy-MM-dd HH:mm:ss) |
|sendNo|	String| 13 | Optional | Sender number |
|recipientNo|	String| 20 | Optional | Recipient number |
|templateId|	String| 50 | Optional | Template number |
|msgStatus|	String| 1 | Optional | Message status code (1:Requesting 2: Processing, 3:Successful) |
|resultCode|	String| 10 | Optional | Result code of receiving [[Table on Query Codes](./error-code/#_2)] |
|subResultCode|	String| 10 | Optional | Detail result code of receiving [[Table on Query Codes](./error-code/#_3)] |
|senderGroupingKey|	String| 100 | Optional | Sender's group key |
|recipientGroupingKey|	String| 100 | Optional | Recipient's group key |
|pageNum|	Integer| - | Optional | Page number (Default : 1) |
|pageSize|	Integer| 1000 | Optional | Number of queries (Default: 15) |

#### Response

```
{
   "header":{
      "resultCode":0,
      "resultMessage":"SUCCESS",
      "isSuccessful":true
   },
   "body":{
      "pageNum":1,
      "pageSize":15,
      "totalCount":1,
      "data":[
         {
            "requestId":"20180810100630ReZQ6KZzAH0",
            "requestDate":"2018-08-10 10:06:30.0",
            "resultDate":"2018-08-10 10:06:42.0",
            "templateId":"TemplateId",
            "templateName":"template name",
            "categoryId":0,
            "categoryName":"category name",
            "body":"single text test",
            "sendNo":"15446859",
            "countryCode":"82",
            "recipientNo":"01000000000",
            "msgStatus":"3",
            "msgStatusName":"successful",
            "resultCode":"1000",
            "resultCodeName":"successful",
            "telecomCode":10001,
            "telecomCodeName":"SKT",
            "mtPr":"1",
            "sendType":"0",
            "userId":"tester",
            "adYn":"N",
            "resultMessage": "",
            "senderGroupingKey":"SenderGroupingKey",
            "recipientGroupingKey":"RecipientGroupingKey"
         }
      ]
   }
}
```

|Value| Type | Description |
|---|---|---|
|header.isSuccessful|	Boolean| Successful or not |
|header.resultCode|	Integer| Failure code |
|header.resultMessage|	String| Failure message |
|body.pageNum|	Integer| Current page number |
|body.pageSize|	Integer| Queried data count |
|body.totalCount|	Integer| Total data count |
|body.data[].requestId|	String| Request ID |
|body.data[].requestDate|	String| Date and time of sending |
|body.data[].resultDate|	String| Date and time of receiving |
|body.data[].templateId|	String| Template ID |
|body.data[].templateName|	String| Template name |
|body.data[].categoryId|	String| Category ID |
|body.data[].categoryName|	String| Category name |
|body.data[].body|	String| Body message |
|body.data[].sendNo|	String| Sender number |
|body.data[].countryCode|	String| Country code |
|body.data[].recipientNo|	String| Recipient number |
|body.data[].msgStatus|	String| Message status code |
|body.data[].msgStatusName|	String| Name of message status code |
|body.data[].resultCode|	String| Result code of receiving [[Table on Result Code of Receiving](./error-code/#emma-v3)] |
|body.data[].resultCodeName|	String| Result code name of receiving |
|body.data[].telecomCode|	Integer| Code of telecom provider |
|body.data[].telecomCodeName|	String| Name of telecom provider |
|body.data[].mtPr|	Integer| Detail delivery ID (required to query details) |
|body.data[].sendType|	String| Delivery type (0:Sms, 1:Lms/Mms, 2:Auth) |
|body.data[].userId|	String| Request ID for sending |
|body.data[].adYn|	String| Ad or not |
|body.data[].senderGroupingKey|	String| Sender's group key |
|body.data[].recipientGroupingKey|	String| Recipient's group key |

### Query Single SMS Delivery for Authentication

#### Request	

[URL]

```
GET  /sms/v2.2/appKeys/{appKey}/sender/auth/sms/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |
|requestId|	String| Request ID |

[Query parameter]

|Value| Type | Required | Description |
|---|----|---|---|
|mtPr|	Integer| Required | Detail delivery ID |

#### Response

```
{
   "header":{
      "resultCode":0,
      "resultMessage":"SUCCESS",
      "isSuccessful":true
   },
   "body":{
      "data":{
         "requestId":"20180810100630ReZQ6KZzAH0",
         "requestDate":"2018-08-10 10:06:30.0",
         "resultDate":"2018-08-10 10:06:42.0",
         "templateId":"TemplateId",
         "templateName":"template name",
         "categoryId":0,
         "categoryName":"category name",
         "body":"body",
         "sendNo":"15446859",
         "countryCode":"82",
         "recipientNo":"01000000000",
         "msgStatus":"3",
         "msgStatusName":"successful",
         "resultCode":"1000",
         "resultCodeName":"successful",
         "telecomCode":10001,
         "telecomCodeName":"SKT",
         "mtPr":"1",
         "sendType":"0",
         "userId":"tester",
         "adYn":"N",
         "resultMessage": "",
         "senderGroupingKey":"SenderGroupingKey",
         "recipientGroupingKey":"RecipientGroupingKey"
      }
   }
}
```

|Value| Type | Description |
|---|---|---|
|header.isSuccessful|	Boolean| Successful or not |
|header.resultCode|	Integer| Failure code |
|header.resultMessage|	String| Failure message |
|body.data.requestId|	String| Requst ID |
|body.data.requestDate|	String| Date and time of sending |
|body.data.resultDate|	String| Date and time of receiving |
|body.data.templateId|	String| Template ID |
|body.data.templateName|	String| Template name |
|body.data.categoryId|	String| Category ID |
|body.data.categoryName|	String| Category name |
|body.data.body|	String| Body message |
|body.data.sendNo|	String| Sender number |
|body.data.countryCode|	String| Country code |
|body.data.recipientNo|	String| Recipient number |
|body.data.msgStatus|	String| Message status code |
|body.data.msgStatusName|	String| Name of message status code |
|body.data.resultCode|	String| Result code of receiving [[Table on result code of receiving](./error-code/#emma-v3)] |
|body.data.resultCodeName|	String| Result code name of receiving |
|body.data.telecomCode|	Integer| Code of telecom provider |
|body.data.telecomCodeName|	String| Name of telecom provider |
|body.data.mtPr|	Integer| Detail delivery ID (required to query details) |
|body.data.sendType|	String| Delivery type (0:Sms, 1:Lms/Mms, 2:Auth) |
|body.data.userId|	String| Request ID for sending |
|body.data.adYn|	String| Ad or not |
|body.data.senderGroupingKey|	String| Sender's group key |
|body.data.recipientGroupingKey|	String| Recipient's group key |

## Ad Messages
### Send SMS for Advertisement
[URL]

```
POST  /sms/v2.2/appKeys/{appKey}/sender/ad-sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Request Body]
Same as Send SMS in the above. 
[[See Request Body](./api-guide/#sms_2)]

<span style="color:red"> However, following messages must be included in the body. </span>
080 numbers are available in **Setting for Rejection of Receiving 080 Numbers** on console. 

```
(Ad)

[Reject receiving ads charge-free]080XXXXXXX
```


### Send MMS for Advertisement 
[URL]

```
POST  /sms/v2.2/appKeys/{appKey}/sender/ad-mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Request Body]
Same as Send MMS in the above. 
[[See Request Body](./api-guide/#mms_1)]

<span style="color:red"> However, following messages must be included in the body. </span>
080 numbers are available in **Setting for Rejection of Receiving 080 Numbers** on console. 

```
(Ad)

[Reject receiving ads charge-free]080XXXXXXX
```



## Query Messages by Result Updates
* The API is queried as of the update time of message delivery result. 
* Please apply this API to import delivery results on device from service. 

### Query Messages 

#### Request

[URL]

```
GET /sms/v2.2/appKeys/{appKey}/message-results?startUpdateDate={startUpdateDate}&endUpdateDate={endUpdateDate}&messageType={messageType}&pageNum={pageNum}&pageSize={pageSize}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Query parameter]

|Value| Type | Required | Description |
|---|---|---|---|
|startUpdateDate|	String| Required | Start time of query result updates <br/>yyyy-MM-dd HH:mm:ss |
|endUpdateDate|	String| Required | End time of query result updates <br/>yyyy-MM-dd HH:mm:ss |
|messageType|	String| Optional | Message type (SMS/LMS/MMS/AUTH) |
| pageNum | Integer | Optional | Page number (default:1) |
| pageSize | Integer | Optional |Number of queries (default:15) |

#### Response
```
{
  "header":{
    "isSuccessful":true,
    "resultCode":0,
    "resultMessage":"Success."
  },
  "body":{
    "pageNum":1,
    "pageSize":15,
    "totalCount":1,
    "data":[
        {
          "messageType":"SMS",
          "requestId":"",
          "recipientSeq":0,
          "resultCode":"1000",
          "resultCodeName":"successful",
          "requestDate":"2018-10-04 16:16:00.0",
          "resultDate":"2018-10-04 16:17:10.0",
          "updateDate":"2018-10-04 16:17:15.0",
          "telecomCode": "10003",
          "telecomCodeName": "LGU",
          "senderGroupingKey":"senderGroupingKey",
          "recipientGroupingKey":"recipientGroupingKey"
        }
    ]
  }
}
```

|Value| Type | Description |
|---|---|---|
|header.isSuccessful|	Boolean| Successful or not |
|header.resultCode|	Integer| Failure code |
|header.resultMessage|	String| Failure message |
|body.data.resultUpdateList[].messageType|String| Message type (SMS/LMS/MMS/AUTH) |
|body.data.resultUpdateList[].requestId | String | Request ID |
|body.data.resultUpdateList[].recipientSeq | Integer | Recipient sequence |
|body.data.resultUpdateList[].resultCode | String | Result code |
|body.data.resultUpdateList[].resultCodeName | String | Name of result code |
|body.data.resultUpdateList[].requestDate | String | Date and time of request (yyyy-MM-dd HH:mm:ss.S) |
|body.data.resultUpdateList[].resultDate | String | Date and time of receiving (yyyy-MM-dd HH:mm:ss.S) |
|body.data.resultUpdateList[].updateDate | String | Date and time of result updates (yyyy-MM-dd HH:mm:ss.S) |
|body.data.resultUpdateList[].telecomCode | String | Code of telecom provider |
|body.data.resultUpdateList[].telecomCodeName | String | Name of telecom provider |
|body.data.resultUpdateList[].senderGroupingKey | String | Sender's group key |
|body.data.resultUpdateList[].recipientGroupingKey | String | Recipient's group key |


## Tag Delivery

### Send Tagged SMS 

#### Request

[URL]

```
POST /sms/v2.2/appKeys/{appKey}/tag-sender/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Request body]

```
{
    "body":"body",
    "sendNo":"15446859",
    "requestDate":"2018-03-22 10:00",
    "templateId":"TemplateId",
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

|Value| Type |	Max Length | Required | Description |
|---|---|---|---|---|
|body|	String| 255 [[Precautions](./api-guide/#precautions)] |	O|	Body |
| sendNo | String | 13 | O | Sender number |
| requestDate| String| - | X | Date and time of schedule (yyyy-MM-dd HH:mm) |
| templateId | String | 50 | X | Template ID |
| templateParameter | Map<String, String> | - | X | Template parameter |
| tagExpression | List<String> | - | O | Tag expression <br/>ex) ["tagA","AND","tabB"] |
| userId | String | 100 | X | Requester ID |
| adYn | String | 1 | X | Ad or not (default: N) |
| autoSendYn | String | 1 | X | Auto delivery or not (immediate delivery) (default: Y) |


#### Response
```
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

|Value| Type | Description |
|---|---|---|
|header.isSuccessful|	Boolean| Successful or not |
|header.resultCode|	Integer| Failure code |
|header.resultMessage|	String| Failure message |
|body.data.requestId|	String| Request ID |

### Send Tagged LMS

#### Request 

[URL]

```
POST  /sms/v2.2/appKeys/{appKey}/tag-sender/mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Request body]
```
{
    "body":"body",
    "sendNo":"15446859",
    "requestDate":"2018-03-22 10:00",
    "templateId":"TemplateId",
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

|Value| Type |	Max Length | Required | Description |
|---|---|---|---|---|
| title | String | 40 bytes (in EUC-KR) | O | Title of text |
| body | String | 2000 bytes (in EUC-KR) | O | Body message |
| sendNo | String | 13 | O | Sender number |
| requestDate| String| - | X | Date and time of schedule (yyyy-MM-dd HH:mm) |
| templateId | String | 50 | X | Template ID |
| templateParameter | Map<String, String> | - | X | Template parameter |
| tagExpression | List<String> | - | O | Tax expression<br/>ex) ["tagA","AND","tabB"] |
| attachFileIdList | List<Integer> | - | X | Attached file ID (fileId) |
| userId | String | 100 | X | Requester ID |
| adYn | String | 1 | X | Ad or not (default: N) |
| autoSendYn | String | 1 | X | Auto delivery or not (immediate delivery) (default: Y) |


#### Response
```
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

|Value| Type | Description |
|---|---|---|
|header.isSuccessful|	Boolean| Successful or not |
|header.resultCode|	Integer| Failure code |
|header.resultMessage|	String| Failure message |
|body.data.requestId|	String| Request ID |


### List Tag Delivery  

#### Request 

[URL]

```
GET /sms/v2.2/appKeys/{appKey}/tag-sender?sendType={sendType}&requestId={requestId}&startRequestDate={startRequestDate}&endRequestDate={endRequestDate}&statusCode={statusCode}&pageNum={pageNum}&pageSize={pageSize}
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Request body]

```
X
```

* requestId or startRequestDate + endRequestDate is required.

	Value|	Type| Max Length | Required|	Description|
|---|---|---|---|---|
| appKey | String| - | O | appkey |
| sendType | required, String | 1 | O | Delivery Type<br>SMS : "0",<br>MMS : "1" |
| requestId | String | - | O | Request ID |
| startRequestDate | String | - | O | Start date of delivery |
| endRequestDate | String | - | O | End date of delivery |
| statusCode | String | 10 | X | Delivery status code<br>WAIT : "MAS00"<br>READY : "MAS01"<br>SENDREADY : "MAS09"<br>SENDWAIT : "MAS10"<br>SENDING : "MAS11"<br>COMPLETE : "MAS19"<br>CANCEL : "MAS91"<br>FAIL : "MAS99" |
| pageNum | optional, Integer | - | X | Page number |
| pageSize | optional, Integer | 1000 | X | Number of queries |

#### Response
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
                "templateId": "TemplateId",
                "templateName": "template name",
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

|Value| Type | Description |
|---|---|---|
|header.isSuccessful|	Boolean| Successful or not |
|header.resultCode|	Integer| Failure code |
|header.resultMessage|	String| Failure message |
|body.data[].requestId | String | Request ID |
|body.data[].requestIp | String | Request IP |
|body.data[].requestDate | String | Request time |
|body.data[].tagSendStatus | String | Tag delivery status |
|body.data[].tagExpression[] | List<String> | Tag expression |
|body.data[].templateId | String | Template ID |
|body.data[].templateName | String | Template name |
|body.data[].senderName | String | Sender's name |
|body.data[].senderMail | String | Sender's address |
|body.data[].title | String | Title |
|body.data[].body | String | Body |
|body.data[].adYn | String | Ad or not |
|body.data[].autoSendYn | String | Auto delivery or not |
|body.data[].sendErrorCount | Integer | Error counts in recipients |
|body.data[].createUser | String | Creator |
|body.data[].createDate | String | Date and time of creation |
|body.data[].updateUser | String | Modifier |
|body.data[].updateDate | String | Date and time of modification |


### List Recipients of Tag Delivery 

#### Request

[URL]

```
GET /sms/v2.2/appKeys/{appKey}/tag-sender/{requestId}?recipientNum={recipientNum}&startRequestDate={startRequestDate}&endRequestDate={endRequestDate}&startResultDate={startResultDate}&endResultDate={endResultDate}&msgStatusName={msgStatusName}&resultCode={resultCode}&pageNum={pageNum}&pageSize={pageSize}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |
| requestId | String | Request ID |

[Request body]

```
X
```

* requestId or startRequestDate + endRequestDate is required.

	Value| Type| Max Length |Required|Description|
|---|---|---|---|---|
| recipientNum | String | 20 | X | Recipient number |
| startRequestDate | String | - | O | Start date of delivery request |
| endRequestDate | String | - | O | End date of delivery request |
| startResultDate | String | - | X | Start date of receiving |
| endResultDate | String | - | X | End date of receiving |
| msgStatusName | String | 10 |  X | Message status code<br/> - READY: Ready<br/> - SENDING: Requesting for delivery <br/> - COMPLETED : Request for delivery completed<br/> - FAILED : Delivery failed |
| resultCode | String | 10 | X | Result code of receiving |
| pageNum | Integer | - | X | Page number |
| pageSize | Integer | 1000 | X | Number of queries |

#### Response
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
        "data" :[
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

|Value| Type | Description |
|---|---|---|
|header.isSuccessful|	Boolean| Successful or not |
|header.resultCode|	Integer| Failure code |
|header.resultMessage|	String| Failure message |
|body.data.requestId | String | Request ID |
|body.data.recipientSeq | Integer | Recipient sequence |
|body.data.countryCode | String | Recipient's country code |
|body.data.recipientNo | String | Recipient number |
|body.data.requestDate | String | Date and time of request |
|body.data.msgStatus | String | Message status code |
|body.data.msgStatusName | String | Name of message status code |
|body.data.resultCode | String | Result code of receiving [[Table on result code of receiving](./error-code/#emma-v3)] |
|body.data.receiveDate | String | Date and time of receiving |
|body.data.createDate | String | Date and time of registration |
|body.data.updateDate | String | Date of modification |

### List Recipient Details of Tagged Delivery  

#### Request

[URL]

```
GET /sms/v2.2/appKeys/{appKey}/tag-sender/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |
| requestId | String | Request ID |
| recipientSeq | String | Sequence |

[Request body]

```
X
```

#### Response 
```
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
            "templateName": "template name",
            "sendNo": "15446859",
            "recipientNum": "01000000000",
            "countryCode": "82",
            "title": "title",
            "body": "body",
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

|Value| Type | Description |
|---|---|---|
|header|	Object| Header area |
|header.isSuccessful|	Boolean| Successful or not |
|header.resultCode|	Integer| Failure code |
|header.resultMessage|	String| Failure message |
|body.data.requestId | String | Request ID |
|body.data.recipientSeq | Integer | Recipient sequence |
|body.data.sendType | String | Delivery type |
|body.data.messageType | String | Message type |
|body.data.templateId | String | Template ID |
|body.data.templateName | String | Template name |
|body.data.sendNo | String | Sender number |
|body.data.title | String | Title |
|body.data.body | String | Body |
|body.data.recipientNum | String | Recipient number |
|body.data.requestDate | String | Date and time of request |
|body.data.msgStatusName | String | Message status name |
|body.data.resultCode | String | Result code of receiving [[Table on result code of receiving](./error-code/#emma-v3)] |
|body.data.receiveDate | String | Date and time of receiving |
|body.data.attachFileList[].filePath | String | Attached file- path |
|body.data.attachFileList[].fileName | String | Attached file - file name |
|body.data.attachFileList[].fileSize | Long | Attached file - size |
|body.data.attachFileList[].fileSequence | Integer | Attached file - file sequence |
|body.data.attachFileList[].createDate | String | Attached file - date of creation |
|body.data.attachFileList[].updateDate | String | Attached file - date of modification |


## 카테고리

### 카테고리 등록

#### 요청

[URL]

```
POST  /sms/v2.2/appKeys/{appKey}/categories
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 앱키|

[Request body]

```
{
   "categoryParentId" : 0,
   "categoryName" : "",
   "categoryDesc" : "",
   "useYn" : "",
   "createUser" : ""
}
```

|값|	타입|	최대 길이 | 필수|	설명|
|---|---|---|---|---|
| categoryParentId |	Integer|	- | 옵션 | 부모 카테고리 ID [기본값: 최상위 카테고리]  |
| categoryName | String | 50 | 필수 | 카테고리 ID |
| categoryDesc |	String| 100 |	옵션 |	카테고리명|
| useYn |	String| 1 |	필수| 사용 여부(Y/N)|
| createUser |	String| 100 | 옵션| 등록한 사용자|

##### Description
- categoryParentId 값이 비어있는 경우, 최상위 카테고리 바로 아래에 등록됩니다.


#### 응답

```
{  
   "header":{  
      "isSuccessful":true,
      "resultCode":0,
      "resultMessage":"SUCCESS"
   },
   "body":{  
      "data":{  
         "categoryId":0,
         "categoryParentId":0,
         "depth":0,
         "sort":0,
         "categoryName":"",
         "categoryDesc":"",
         "useYn":"",
         "createUser":""
      }
   }
}
```

|값|	타입|	설명|
|---|---|---|
|header.isSuccessful|	Boolean|	성공 여부|
|header.resultCode|	Integer|	실패 코드|
|header.resultMessage|	String|	실패 메시지|
|body.data[].categoryId|	Integer|	카테고리 ID|
|body.data[].categoryParentId|	Integer|	부모 카테고리 ID|
|body.data[].depth|	Integer|	카테고리 깊이|
|body.data[].sort|	Integer|	카테고리 정렬 순서|
|body.data[].categoryName|	String|	카테고리명|
|body.data[].categorycategoryDescame|	String|	카테고리 설명|
|body.data[].useYn|	String|	사용 여부|
|body.data[].createUser|	String|	등록한 사용자|

### 카테고리 리스트 조회

#### 요청

[URL]

```
GET  /sms/v2.2/appKeys/{appKey}/categories
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 앱키|

[Query parameter]

|값|	타입|	최대 길이 | 필수|	설명|
|---|---|---|---|---|
|pageNum|	Integer| - |	옵션|	페이지 번호(기본값 : 1)|
|pageSize|	Integer| 1000 |	옵션|	조회 수(기본값 : 15)|

#### 응답

```
{  
   "header":{  
      "isSuccessful":true,
      "resultCode":0,
      "resultMessage":"SUCCESS"
   },
   "body":{  
      "pageNum":1,
      "pageSize":15,
      "totalCount":1,
      "data":[  
         {  
            "categoryId":137612,
            "categoryParentId":0,
            "depth":0,
            "sort" :0,
            "categoryName":"카테고리",
            "categoryDesc":"최상위 카테고리",
            "useYn":"Y",
            "createDate":"2018-04-17 15:39:56.0",
            "createUser":"bb076dc0-ef5e-11e7-9ede-005056ac7022",
            "updateDate":"2018-04-17 15:39:56.0",
            "updateUser":"bb076dc0-ef5e-11e7-9ede-005056ac7022"
         }
      ]
   }
}
```

|값|	타입|	설명|
|---|---|---|
|header.isSuccessful|	Boolean|	성공 여부|
|header.resultCode|	Integer|	실패 코드|
|header.resultMessage|	String|	실패 메시지|
|body.pageNum|	Integer|	현재 페이지 번호|
|body.pageSize|	Integer|	조회된 데이터 수|
|body.totalCount|	Integer|	총 데이터 수|
|body.data[].categoryId|	Integer|	카테고리 ID|
|body.data[].categoryParentId|	Integer|	부모 카테고리 ID|
|body.data[].depth|	Integer|	카테고리 깊이|
|body.data[].sort|	Integer|	카테고리 정렬 순서|
|body.data[].categoryName|	String|	카테고리명|
|body.data[].categorycategoryDescame|	String|	카테고리 설명|
|body.data[].useYn|	String|	사용 여부|
|body.data[].createDate|	String|	등록 날짜|
|body.data[].createUser|	String|	등록한 사용자|
|body.data[].updateDate|	String|	수정 날짜|
|body.data[].updateUser|	String|	수정한 사용자|

### 카테고리 단건 조회

#### 요청

[URL]

```
GET  /sms/v2.2/appKeys/{appKey}/categories/{categoryId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 앱키|
|categoryId|	String|	카테고리 ID|

#### 응답

```
{  
   "header":{  
      "isSuccessful":true,
      "resultCode":0,
      "resultMessage":"SUCCESS"
   },
   "body":{  
      "data":[  
         {  
            "categoryId":137612,
            "categoryParentId":0,
            "depth":0,
            "sort":0,
            "categoryName":"카테고리",
            "categoryDesc":"최상위 카테고리",
            "useYn":"Y",
            "createDate":"2018-04-17 15:39:56.0",
            "createUser":"bb076dc0-ef5e-11e7-9ede-005056ac7022",
            "updateDate":"2018-04-17 15:39:56.0",
            "updateUser":"bb076dc0-ef5e-11e7-9ede-005056ac7022"
         }
      ]
   }
}
```

|값|	타입|	설명|
|---|---|---|
|header.isSuccessful|	Boolean|	성공 여부|
|header.resultCode|	Integer|	실패 코드|
|header.resultMessage|	String|	실패 메시지|
|body.data[].categoryId|	Integer|	카테고리 ID|
|body.data[].categoryParentId|	Integer|	부모 카테고리 ID|
|body.data[].depth|	Integer|	카테고리 깊이|
|body.data[].sort|	Integer|	카테고리 정렬 순서|
|body.data[].categoryName|	String|	카테고리명|
|body.data[].categorycategoryDescame|	String|	카테고리 설명|
|body.data[].useYn|	String|	사용 여부|
|body.data[].createDate|	String|	등록 날짜|
|body.data[].createUser|	String|	등록한 사용자|
|body.data[].updateDate|	String|	수정 날짜|
|body.data[].updateUser|	String|	수정한 사용자|


### 카테고리 수정

#### 요청

[URL]

```
PUT  /sms/v2.2/appKeys/{appKey}/categories/{categoryId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 앱키|
|categoryId|	String|	카테고리 ID|

[Request body]

```
{
   "categoryName" : "",
   "categoryDesc" : "",
   "useYn" : "",
   "updateUser" : ""
}
```

|값|	타입|	최대 길이 | 필수|	설명|
|---|---|---|---|---|
| categoryName | String | 50 | 필수 | 카테고리 ID |
| categoryDesc |	String| 100 |	옵션 |	카테고리명|
| useYn |	String| 1 |	필수| 사용 여부(Y/N)|
| updateUser |	String| 100 |	옵션| 수정한 사용자|


#### 응답

```
{
   "header" : {
      "isSuccessful" : true,
      "resultCode" : "",
      "resultMessage" : ""
   }
}
```

### 카테고리 삭제

#### 요청

[URL]

```
DELETE  /sms/v2.2/appKeys/{appKey}/categories/{categoryId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 앱키|
|categoryId|	String|	카테고리 ID|

#### 응답

```
{
   "header" : {
      "isSuccessful" : true,
      "resultCode" : "",
      "resultMessage" : ""
   }
}
```


## Templates


### 템플릿 등록

#### 요청

[URL]

```
POST  /sms/v2.2/appKeys/{appKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 앱키|

[Request body]

```
{
   "categoryId" : 0,
   "templateId" : "",
   "templateName" : "",
   "templateDesc" : "",
   "sendNo" : "",
   "sendType" : "",
   "title" : "",
   "body" : "",
   "useYn" : "",
   "attachFileIdList" : [
      0,
      1
   ]
}
```

|값|	타입|	최대 길이 | 필수|	설명|
|---|---|---|---|---|
| categoryId |	Integer|	- | 필수 |	카테고리 ID |
| templateId | String | 50 | 필수 | 템플릿 ID |
| templateName |	String| 50 |	필수 |	템플릿명|
| templateDesc |	String| 100 |	옵션 |	템플릿 설명|
| sendNo | String | 13 | 필수 | 발신 번호 |
| sendType | String| 1| 필수| 발송 유형(0:Sms, 1:Lms/Mms) |
| title | String | 120 | 옵션 | 문자 제목(발송 유형이 Lms/MmS인 경우 필수) |
| body | String | 4000 | 필수 | 문자 내용 |
| useYn |	String| 1 |	필수|	사용 여부(Y/N)|
| attachFileIdList | List<Integer> | - | X | 첨부 파일 ID(fileId) |


#### 응답

```
{
   "header" : {
      "isSuccessful" : true,
      "resultCode" : "",
      "resultMessage" : ""
   }
}
```


### Send Templates (requiring no body updates)

#### Example 
![[그림 1] 템플릿 등록](http://static.toastoven.net/prod_sms/img_26.png)

|Http method| Type | URL|
| - | - | - |
| POST | SMS | https://api-sms.cloud.toast.com/sms/v2.2/appKeys/{appKey}/sender/sms |
| POST | MMS | https://api-sms.cloud.toast.com/sms/v2.2/appKeys/{appKey}/sender/mms |

For Request URL, choose a delivery type selected to register templates.  

**If request parameter body is empty, replace it with the body of the corresponding templateId.**

[Request body] Replace with key and value for those for replacement. 

```
{
    "templateId": "TemplateId",
    "senderGroupingKey": "SenderGroupingKey",
    "recipientList": [{
        "recipientNo": "01000000000",
        "templateParameter": {
          "key1" : "Toast Cloud",
          "key2" : "SMS"
        },
        "recipientGroupingKey": "RecipientGroupingKey"
    }]
}
```

[Response]
```
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

![[그림 2] 템플릿 발송 성공](http://static.toastoven.net/prod_sms/img_27.png)

### Send Templates (requiring body updates)

#### Example of Sending Tempaltes

|Http method| Type | URL|
| - | - | - |
| POST | SMS | https://api-sms.cloud.toast.com/sms/v2.2/appKeys/{appKey}/sender/sms |
| POST | MMS | https://api-sms.cloud.toast.com/sms/v2.2/appKeys/{appKey}/sender/mms |

For Request URL, choose a delivery type selected to register templates.

**If template ID and request parameter body include values, sender number and body message are not replaced with template. **

Nevertheless, with the input of template ID, it is available to query with the template. 

Such case is applicable when template needs to be modified after queried. 

[Request body]

```
{
    "templateId": "TemplateId",
    "body":"body",
    "sendNo":"15446859",
    "senderGroupingKey": "SenderGroupingKey",
    "recipientList": [{
        "recipientNo": "01000000000",
        "templateParameter": {
          "key1" : "Toast Cloud",
          "key2" : "SMS"
        },
        "recipientGroupingKey": "RecipientGroupingKey"
    }]
}
```

[Response]
```
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


### List Templates 

#### Request

[URL]

```
GET  /sms/v2.2/appKeys/{appKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Query parameter]

|Value| Type | Required | Description |
|---|---|---|---|
|categoryId|	Integer| Optional | Category ID |
|useYn|	String| Optional | Use or Not (Y/N) |

#### Response

```
{
    "header": {
        "isSuccessful": Boolean,
        "resultCode": Integer,
        "resultMessage": String
    },
    "body": {
        "pageNum": Integer,
        "pageSize": Integer,
        "totalCount": Integer,
        "data": [{
            "templateId": "TemplateId",
            "serviceId": 0,
            "categoryId": 0,
            "categoryName": "category name",
            "sort": 0,
            "templateName": "template name",
            "templateDesc": "template description",
            "useYn": "Y",
            "priority": "S",
            "sendNo": ""15446859String"",
            "sendType": "0",
            "sendTypeName": "send SMS",
            "title": "title",
            "body": "body",
            "attachFileYn": "N",
            "delYn": "N",
            "createDate": "2018-01-28 17:50:55.0,
            "createUser": "CreateUser",
            "updateDate": "2018-01-28 17:50:55.0",
            "updateUser": "UpdateUser",
            "attachFileList": [{
                "fileId": 0,
                "serviceId": 0,
                "attachType": 0,
                "templateId": "TemplateId",
                "filePath": "26606/toast-mt-2018-01-29/1427/105316",
                "fileName": "attachment.jpg",
                "fileSize": 0,
                "createDate": "2018-01-28 17:50:55.0",
                "createUser": "CreateUser"
            }]
        }]
    }
}
```

|Value| Type | Description |
|---|---|---|
|header.isSuccessful|	Boolean| Successful or not |
|header.resultCode|	Integer| Failure code |
|header.resultMessage|	String| Faliure message |
|body.pageNum|	Integer| Current page number |
|body.pageSize|	Integer| Queried data count |
|body.totalCount|	Integer| Total data count |
|body.data[].templateId|	String| Template ID |
|body.data[].serviceId|	Integer| Service ID (unused, for internal purpose) |
|body.data[].categoryId|	Integer| Category ID |
|body.data[].categoryName|	String| Category name |
|body.data[].sort|	Integer| Sorted value |
|body.data[].templateName|	String| Template name |
|body.data[].templateDesc|	String| Template description |
|body.data[].useYn|	String| Use or not |
|body.data[].priority|	String| Priority value (unused) |
|body.data[].sendNo|	String| Sender number |
|body.data[].sendType|	String| Delivery type (0:Sms, 1:Lms/Mms, 2:Auth) |
|body.data[].sendTypeName|	String| Name of delivery type |
|body.data[].title|	String| Title |
|body.data[].body|	String| Body message |
|body.data[].attachFileYn|	String| Attached or not (Y/N) |
|body.data[].delYn |	String| Deleted or not (Y/N): only to show current status |
|body.data[].createDate|	String| Date of registration |
|body.data[].createUser|	String| Registered user |
|body.data[].updateDate|	String| Date of modification |
|body.data[].updateUser|	String| Modifier |
|body.data[].attachFileList[].fileId|	Integer| Attached file ID |
|body.data[].attachFileList[].serviceId|	Integer| Service ID (unused, for internal purpose) |
|body.data[].attachFileList[].attachType|	Integer| Upload type of attachment (0:Temporary,1:Upload,2:Template) |
|body.data[].attachFileList[].templateId|	String| Template ID |
|body.data[].attachFileList[].filePath|	String| Path of attached file |
|body.data[].attachFileList[].fileName|	String| Name of attached file |
|body.data[].attachFileList[].fileSize|  Integer| File size |
|body.data[].attachFileList[].createDate|	String| Date of registration for attachment |
|body.data[].attachFileList[].createUser|	String| Registered user of attachment |


### Query Single Template

#### Request

[URL]

```
GET  /sms/v2.2/appKeys/{appKey}/templates/{templateId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |
|templateId|	String| Template ID |

#### Response

```
{
    "header": {
        "isSuccessful": Boolean,
        "resultCode": Integer,
        "resultMessage": String
    },
    "body": {
        "pageNum": Integer,
        "pageSize": Integer,
        "totalCount": Integer,
        "data": {
            "templateId": "TemplateId",
            "serviceId": 0,
            "categoryId": 0,
            "categoryName": "category name",
            "sort": 0,
            "templateName": "template name",
            "templateDesc": "template description",
            "useYn": "Y",
            "priority": "S",
            "sendNo": ""15446859String"",
            "sendType": "0",
            "sendTypeName": "send SMS",
            "title": "title",
            "body": "body",
            "attachFileYn": "N",
            "delYn": "N",
            "createDate": "2018-01-28 17:50:55.0,
            "createUser": "CreateUser",
            "updateDate": "2018-01-28 17:50:55.0",
            "updateUser": "UpdateUser",
            "attachFileList": [{
                "fileId": 0,
                "serviceId": 0,
                "attachType": 0,
                "templateId": "TemplateId",
                "filePath": "26606/toast-mt-2018-01-29/1427/105316",
                "fileName": "attachment.jpg",
                "fileSize": 0,
                "createDate": "2018-01-28 17:50:55.0",
                "createUser": "CreateUser"
            }]
        }
    }
}
```

|Value| Type | Description |
|---|---|---|
|header.isSuccessful|	Boolean| Successful or not |
|header.resultCode|	Integer| Failure code |
|header.resultMessage|	String| Failure message |
|body.pageNum|	Integer| Current page number |
|body.pageSize|	Integer| Queried data count |
|body.totalCount|	Integer| Total data count |
|body.data.templateId|	String| Template ID |
|body.data.serviceId|	Integer| Service ID (unused, for internal purpose) |
|body.data.categoryId|	Integer| Category ID |
|body.data.categoryName|	String| Category name |
|body.data.sort|	Integer| Sorted value |
|body.data.templateName|	String| Template name |
|body.data.templateDesc|	String| Template description |
|body.data.useYn|	String| Use or not |
|body.data.priority|	String| Priority value (unused) |
|body.data.sendNo|	String| Sender number |
|body.data.sendType|	String| Delivery type (0:Sms, 1:Lms/Mms, 2:Auth) |
|body.data.sendTypeName|	String| Name of delivery type |
|body.data.title|	String| Title |
|body.data.body|	String| Body message |
|body.data.attachFileYn|	String| Attached or not (Y/N) |
|body.data.delYn |	String| Deleted or not (Y/N): only to show current status |
|body.data.createDate|	String| Date of registration |
|body.data.createUser|	String| Registered user |
|body.data.updateDate|	String| Date of modification |
|body.data.updateUser|	String| Modifier |
|body.data.attachFileList[].fileId|	Integer| Attached file ID |
|body.data.attachFileList[].serviceId|	Integer| Service ID (unused, for internal purpose) |
|body.data.attachFileList[].attachType|	Integer| Upload type of attachment (0:Temporary,1:Upload,2:Template) |
|body.data.attachFileList[].templateId|	String| Template ID |
|body.data.attachFileList[].filePath|	String| Path of attachment |
|body.data.attachFileList[].fileName|	String| Name of attachment |
|body.data.attachFileList[].fileSize|  Integer| File size |
|body.data.attachFileList[].createDate|	String| Date of registration of attachment |
|body.data.attachFileList[].createUser|	String| Registered user of attachment |


### 템플릿 수정

#### 요청

[URL]

```
PUT  /sms/v2.2/appKeys/{appKey}/templates/{templateId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 앱키|

[Request body]

```
{
   "templateName" : "",
   "templateDesc" : "",
   "sendNo" : "",
   "sendType" : "",
   "title" : "",
   "body" : "",
   "useYn" : "",
   "attachFileIdList" : [
      0,
      1
   ]
}
```

|값|	타입|	최대 길이 | 필수|	설명|
|---|---|---|---|---|
| templateName |	String| 50 |	필수 |	템플릿명|
| templateDesc |	String| 100 |	옵션 |	템플릿 설명|
| sendNo | String | 13 | 필수 | 발신 번호 |
| sendType | String| 1| 필수| 발송 유형(0:Sms, 1:Lms/Mms) |
| title | String | 120 | 옵션 | 문자 제목(발송 유형이 Lms/MmS인 경우 필수) |
| body | String | 4000 | 필수 | 문자 내용 |
| useYn |	String| 1 |	필수|	사용 여부(Y/N)|
| attachFileIdList | List<Integer> | - | X | 첨부 파일 ID(fileId) |


#### 응답

```
{
   "header" : {
      "isSuccessful" : true,
      "resultCode" : "",
      "resultMessage" : ""
   }
}
```

### 템플릿 삭제

#### 요청

[URL]

```
DELETE  /sms/v2.2/appKeys/{appKey}/templates/{templateId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 앱키|
|templateId|	String|	템플릿 ID|

#### 응답

```
{
   "header" : {
      "isSuccessful" : true,
      "resultCode" : "",
      "resultMessage" : ""
   }
}
```


## Rejection of Receiving 080 Numbers

### Query Target of Rejection 

#### Request

[URL]

```
GET  /sms/v2.2/appKeys/{appKey}/blockservice/recipients
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Query parameter]

|Value| Type |	Max Length | Required | Description |
|---|---|---|---|---|
|unsubscribeNo|	String| 25 |	Required |	080 numbers to reject receiving |
|startRequestDate|	String| - |	Optional | Start value of request for reject receiving (yyyy-MM-dd HH:mm:ss) |
|endRequestDate|	String| - |	Optional | End value of request for reject receiving (yyyy-MM-dd HH:mm:ss) |
|pageNum|	Integer| - | Optional | Page number (default: 1) |
|pageSize|	Integer| 1000 | Optional | Number of queries (default: 15) |

#### Response
```
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

### Delete Target of Rejection

#### Request

[URL]

```
DELETE  /sms/v2.2/appKeys/{appKey}/blockservice/recipients/removes?unsubscribeNo={unsubscribeNo}&updateUser={updateUser}&recipientNoList={recipientNo},{recipientNo}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Query parameter]

|Value| Type |	Max Length | Required | Description |
|---|---|---|---|---|
|unsubscribeNo|	String| 20 |	Required |	080 numbers to reject receiving |
|updateUser|	String|	100 | Required | User who delete rejection of receiving |
|recipientNo|	String|	20 | Required | Rejected numbers to be deleted |

#### Response
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

## Sender Numbers

### Request for Sender Number Registration 

#### Request

[URL]

```
POST /sms/v2.2/appKeys/{appKey}/reqeusts/sendNos|
Content-Type: application/json;charset=UTF-8
```


[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Request Body]
```
{
  "sendNos" : [
    String,
    String
  ],
  "fileIds" : [
    Integer
  ],
  "comment" :  String
}

```

#### Response
```
{
  "header" : {
    "isSuccessful" :  Boolean,
    "resultCode" :  Integer,
    "resultMessage" :  String
  }
}
```

### Upload Sender Number Documents 

#### Request

[URL]
```
POST /sms/v2.2/appKeys/{appKey}/requests/attachFiles/authDocuments
Content-Type : multipart/form-data;
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Request Body]

|Value|Type|Description|
|---|---|---|
| attachFile | MultiPartFile | File data receivable as MultiPartFile |


#### Response
```
{
  "header" : {
    "isSuccessful" :  Boolean,
    "resultCode" :  Integer,
    "resultMessage" :  String
    },
    "file" : {
      "fileId" :  Integer
    }
  }
```

### Query History of Request for Sender Number Authentication API

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/sms/v2.2/appKeys/{appKey}/requests/sendNos?sendNo={sendNo}&status={status}&pageNum={pageNum}&pageSize={pageSize}|

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Query parameter]

|Value| Type | Description |
|---|---|---|
| sendNo|String | Sender numbers requested for registration |
|status|	String| Document authentication status<br/>- SRS01	Requested for sender number registration <br/>- SRS02	Evaluating<br/>- SRS03	Registration completed<br/>- SRS04	Registration unavailable<br/>- SRS05	Ready for mobile phone authentication <br/>- SRS06	Mobile phone authentication failed<br/>- SRS07	Manual registration completed |
|pageNum|	Integer| Page number (default: 1) |
|pageSize|	Integer| Number of queries (default: 15) |

#### Response
```
{
  "header":{
    "isSuccessful":true,
    "resultCode":0,
    "resultMessage":"SUCCESS"
  },
  "body":{
    "pageNum":1,
    "pageSize":15,
    "totalCount":1,
    "data":[
      {
        "authType":"{SMS_AUTH|DOCUMENT_AUTH|MANUAL_REGIST_AUTH}",
        "sendNos":[
          "01000000000",
          "01011111111"
        ],
        "comment":"{comment}",
        "fileIds":null,
        "status":"{REGIST_REQUEST|EXAMINE|COMPLETE|REJECT|CERTIFYING|CERTIFY_FAILED|MANUAL_REGIST}",
        "createDate":"2018-08-14 16:34:54",
        "updateDate":"2018-08-14 16:34:54",
        "confirmDate":"2018-08-14 16:34:54"
      }
    ]
  }
}
```

|Value| Type | Description |
|---|---|---|
|header.isSuccessful|	Boolean| Successful or not |
|header.resultCode|	Integer| Failure code |
|header.resultMessage|	String| Failure message |
|body.pageNum|	Integer| Current page number |
|body.pageSize|	Integer| Queried data count |
|body.totalCount|	Integer| Total data count |
|body.data[].authType|	String| Requested authentication type<br/>- SMS_AUTH: SMS authentication<br/>- DOCUMENT_AUTH: Document authentication<br/>- REGIST_AUTH: Manual authentication |
|body.data[].sendNos[]|	List<String>| Sender number list requested for registration |
|body.data[].comment|	String| Comment items |
|body.data[].fileIds[]|	List<Integer>| Uploaded filed ID for document authentication (for internal purpose) |
|body.data[].status|	String| Request Status<br/>- REGIST_REQUEST(SRS01)<br/>- EXAMINE(SRS02)<br/>- COMPLETE(SRS03)<br/>- REJECT(SRS04)<br/>- CERTIFYING(SRS05)<br/>- CERTIFY_FAILED(SRS06)<br/>- MANUAL_REGIST(SRS07) |
|body.data[].createDate|	String| Date and time of registration	|
|body.data[].updateDate|	String| Date of modification	|
|body.data[].confirmDate|	String| Date and time of approval/refusal	|



### List Registered Sender Numbers API

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/sms/v2.2/appKeys/{appKey}/sendNos?sendNo={sendNo}&useYn={useYn}&blockYn={blockYn}&pageNum={pageNum}&pageSize={pageSize}

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Query parameter]

|Value| Type | Description |
|---|---|---|
| sendNo | String | Sender number |
| useYn | String | Use or not |
| blockYn | String | Block or not |
|pageNum|	Integer| Page number (default: 1) |
|pageSize|	Integer| Number of queries (default: 15) |

#### Response
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

|Value| Type | Description |
|---|---|---|
|header.isSuccessful|	Boolean| Successful or not |
|header.resultCode|	Integer| Failure code |
|header.resultMessage|	String| Failure message |
|body.pageNum|	Integer| Page number |
|body.pageSize|	Integer| Queried data count |
|body.totalCount|	Integer| Total data count |
|body.data[].serviceId | Integer | Service ID |
|body.data[].sendNo | String | Sender number |
|body.data[].useYn | String | Use or not |
|body.data[].blockYn | String | Block or not |
|body.data[].blockReason | String | Cause of blockage |
|body.data[].createDate | String | Date and time of creation |
|body.data[].createUser | String | Creator |
|body.data[].updateDate | String | Date of modification |
|body.data[].updateUser | String | Modified user |

## Query Statistics

### Query Integrated Statistics 

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/sms/v2.2/appKeys/{appKey}}/statistics/view?searchType={searchType}&from={from}&to={to}&messageTypes={messageType}&contentTypes={contentType}&templateId={templateId}|

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Query parameter]

|Value| Type |	Max Length | Required |Description|
|---|---|---|---|---|
| searchType | String | 10 | O | Type of statistics <br/>DATE:By date, TIME:By time, DAY:By day |
| from | String |  - |O | Start date of query statistics<br/>yyyy-MM-dd HH:mm |
| to | String | - | O | End date of query statistics<br/>yyyy-MM-dd HH:mm |
| messageType | String | 10 |  X | Message type<br/>SMS: Short messages, LMS: Long messages, MMS: Attachment, AUTH: Authentication |
| contentType | String | 10 |  X | Content type <br/>NORMAL: General, AD: Advertisement |
| templateId | String | 50 |  X | Template ID |

#### Response
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
|Value| Type |Description|
|---|---|---|
|header.isSuccessful|	Boolean| Successful or not |
|header.resultCode|	Integer| Failure code |
|header.resultMessage|	String| Failure message |
|body.data[].divisionName | String | Display name<br/>Date, time, Day |
|body.data[].statisticsView | Object | |
|body.data[].requestedCount | Integer | Number of requests |
|body.data[].succeedCount | Integer | Success count |
|body.data[].failedCount | Integer | Failure count |
|body.data[].pendingCount | Integer | Delivery count |
|body.data[].succeedRate | String | Success rate |
|body.data[].failedRate | String | Failure rate |
|body.data[].pendingRate | String | Delivery rate |

## Scheduled Delivery

### List Scheduled Delivery 

#### Request

[URL]

```
GET /sms/v2.2/appKeys/{appKey}/reservations?sendType={sendType}&startRequestDate={startRequestDate}&endRequestDate={endRequestDate}&sendNo={sendNo}&recipientNo={recipientNo}&templateId={templateId}&requestId={requestId}&messageStatus={messageStatus}&pageNum={pageNum}&pageSize={pageSize}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Query parameter]

|Value| Type |	Max Length | Required | Description |
|---|---|---|---|---|
|sendType| String| 1 | Optional | Delivery type<br/>(0:SMS, 1:LMS/MMS, 2:AUTH) |
|requestId|	String| 25 |	Optional | Request ID |
|startRequestDate|	String| - |	Optional | Start date of sending (yyyy-MM-dd HH:mm:ss) |
|endRequestDate|	String| - |	Optional | End date of sending (yyyy-MM-dd HH:mm:ss) |
|sendNo|	String| 13 | Optional | Sender number |
|recipientNo|	String| 20 | Optional | Recipient number |
|templateId|	String| 50 | Optional | Template number |
|messageStatus|	String| 10 | Optional | Message status<br/>(RESERVED: Ready for schedule, SENDING: Sending, COMPLETED: Sending completed, FAILED: Sending failed, CANCEL: Schedule canceled, and DUPLICATED: Duplicate delivery) |
|pageNum|	Integer| - | Optional | Page number (default: 1) |
|pageSize|	Integer| 1000 | Optional | Number of queries (default: 15) |

#### Response

```
{
  "header":{
    "resultCode":0,
    "resultMessage":"success",
    "isSuccessful":true
  },
  "body":{
    "pageNum":1,
    "pageSize":15,
    "totalCount":15,
    "data":[
      {
        "requestId":"{request ID}",
        "recipientSeq":1,
        "requestDate":"{scheduled date}",
        "sendNo":"{sender number}",
        "recipientNo":"{recipient number}",
        "countryCode":"{country code}",
        "sendType":"{delivery type}",
        "messageType":"{message type}",
        "adYn":"{Ad or not}",
        "templateId":"{template ID}",
        "templateParameter":"{template parameter}",
        "templateName":"{template name}",
        "title":"{title}",
        "body":"{body}",
        "messageStatus":"{message status}",
        "createUser":"{registered user}",
        "createDate":"{date and time of registration}",
        "updateDate":"{modified date}"
      }
    ]
  }
}
```

|Value| Type | Description |
|---|---|---|
|header.isSuccessful|	Boolean| Successful or not |
|header.resultCode|	Integer| Failure code |
|header.resultMessage|	String| Failure message |
|body.pageNum|	Integer| Current page number |
|body.pageSize|	Integer| Queried data count |
|body.totalCount|	Integer| Total data count |
|body.data[].requestId|	String| Request ID |
|body.data[].recipientSeq|	Integer| Recipient sequence |
|body.data[].requestDate|	String| Date and time of sending |
|body.data[].sendNo|	String| Sender number |
|body.data[].recipientNo|	String| Recipient number |
|body.data[].countryCode|	String| Country code |
|body.data[].sendType|	String| Delivery type (0:Sms, 1:Lms/Mms, 2:Auth) |
|body.data[].messageType|	String| Message type<br/>(SMS,LMS,MMS,AUTH) |
|body.data[].adYn|	String| Ad or not |
|body.data[].templateId|	String| Template ID |
|body.data[].templateParameter|	String(json)| Template parameter |
|body.data[].templateName|	String| Template name |
|body.data[].title|	String| Title |
|body.data[].body|	String| Body message |
|body.data[].messageStatus|	String| Message status<br/>(RESERVED: Ready for schedule, SENDING: Sending, COMPLETED:Delivery completed, FAILED: Delivery failed, CANCEL: Schedule canceled, DUPLICATED: Duplicate delivery) |
|body.data[].createUser|	String| Registered user |
|body.data[].createDate|	String| Date of registration |
|body.data[].updateDate|	String| Date of modification |

### Query Detail Scheduled Delivery 

#### Request

[URL]

```
GET /sms/v2.2/appKeys/{appKey}/reservations/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |
|requestId|	String| Request ID |
|recipientSeq|	Integer| Recipient sequence |

#### Response

```
{
  "header":{
    "resultCode":0,
    "resultMessage":"success",
    "isSuccessful":true
  },
  "body":{
    "data":{
      "requestId":"{request ID}",
      "recipientSeq":1,
      "requestDate":"{scheduled date}",
      "sendNo":"{sender number}",
      "recipientNo":"{recipient number}",
      "countryCode":"{country code}",
      "sendType":"{delivery type}",
      "messageType":"{message type}",
      "adYn":"{ad or not}",
      "templateId":"{template ID}",
      "templateParameter":"{template parameter}",
      "templateName":"{template name}",
      "title":"{title}",
      "body":"{body}",
      "messageStatus":"{message status}",
      "createUser":"{registered user}",
      "createDate":"{date and time of registration}",
      "updateDate":"{modified date}",
      "attachFileList":[
        {
          "fileId":0,
          "filePath":"26606/toast-mt-2018-02-07/1555/105887/",
          "fileName":"file_attach_test.jpg"
        }
      ]
    }
  }
}
```

|Value| Type | Description |
|---|---|---|
|header.isSuccessful|	Boolean| Successful or not |
|header.resultCode|	Integer| Failure code |
|header.resultMessage|	String| Failure message |
|body.pageNum|	Integer| Current page number |
|body.pageSize|	Integer| Queried data count |
|body.totalCount|	Integer| Total data count |
|body.data.requestId|	String| Request ID |
|body.data.recipientSeq|	Integer| Recipient sequence |
|body.data.requestDate|	String| Date and time of sending |
|body.data.sendNo|	String| Sender number |
|body.data.recipientNo|	String| Recipient number |
|body.data.countryCode|	String| Country code |
|body.data.sendType|	String| Delivery type (0:Sms, 1:Lms/Mms, 2:Auth) |
|body.data.messageType|	String| Message type <br/>(SMS,LMS,MMS,AUTH) |
|body.data.adYn|	String| Ad or not |
|body.data.templateId|	String| Template ID |
|body.data.templateParameter|	String(json)| Template parameter |
|body.data.templateName|	String| Template name |
|body.data.title|	String| Title |
|body.data.body|	String| Body message |
|body.data.messageStatus|	String| Message status <br/>(RESERVED: Ready for schedule, SENDING: Sending, COMPLETED:Sending completed, FAILED:Sending failed, CANCEL:Schedule canceled, and DUPLICATED: Duplicate delivery) |
|body.data.createUser|	String| Registered user |
|body.data.createDate|	String| Date of registration |
|body.data.attachFileList[].fileId|	Integer| File ID |
|body.data.attachFileList[].filePath|	String| File path (for internal purpose) |
|body.data.attachFileList[].fileName|	String| File name |


### Cancel Scheduled Delivery 

#### Request

[URL]

```
PUT /sms/v2.2/appKeys/{appKey}/reservations/cancel
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Request body]

```
{
  "reservationList":[
    {
      "requestId":"{requestId}",
      "recipientSeq":1
    }
  ],
  "updateUser":"{updateUser}"
}
```

|Value| Type |	Max Length | Required | Description |
|---|---|---|---|---|
|reservationList[].requestId| String| 25 | O | Request ID |
|reservationList[].recipientSeq| Integer| - | O | Recipient sequence |
|updateUser| String| 100 | O | Requesting user for cancellation |

[Response body]
```
{
  "header":{
    "resultCode":0,
    "resultMessage":"success",
    "isSuccessful":true
  },
  "body":{
    "data":{
      "requestedCount":1,
      "canceledCount":1
    }
  }
}
```

|Value| Type | Description |
|---|---|---|
|header.isSuccessful|	Boolean| Successful or not |
|header.resultCode|	Integer| Failure code |
|header.resultMessage|	String| Failure message |
|body.data.requestedCount|	Integer| Number of failed requests |
|body.data.canceledCount|	Integer| Number of successful cancellation |

## Download Delivery Result Files 

### Request for Creating Query Files 

#### Request 

[URL]

```
POST /sms/v2.2/appKeys/{appKey}/sender/download-reservations
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Request body]

```
{
  "sendType":"1",
  "requestId":"20190601100630ReZQ6KZzAH0",
  "startRequestDate":"2019-06-01 00:00:00",
  "endRequestDate":"2019-06-08 00:00:00",
  "startResultDate":"2019-06-01 00:00:00",
  "endResultDate":"2019-06-08 00:00:00",
  "sendNo":"15446859",
  "recipientNo":"01000000000",
  "templateId":"TemplateId",
  "msgStatus":"3",
  "resultCode":"MTR2",
  "subResultCode":"MTR2_3",
  "senderGroupingKey":"{sender's group key}",
  "recipientGroupingKey":"{recipient's group key}",
  "isIncludeTitleAndBody":true
}
```

|Value| Type |	Max Length | Required | Description |
|---|---|---|---|---|
|sendType| String| 1| O| Delivery type (0:Sms, 1:Lms/Mms, 2:Auth) |
|requestId|	String| 25 |	Conditionally required (no.1) | Request ID |
|startRequestDate|	String| - |	Conditionally required (no.2) | Start date of delivery (yyyy-MM-dd HH:mm:ss) |
|endRequestDate|	String| - |	Conditionally required (no.2) | End date of delivery (yyyy-MM-dd HH:mm:ss) |
|startResultDate|	String| - | Optional | Start date of receiving (yyyy-MM-dd HH:mm:ss) |
|endResultDate|	String| - | Optional | End date of receiving (yyyy-MM-dd HH:mm:ss) |
|sendNo|	String| 13 | Optional | Sender number |
|recipientNo|	String| 20 | Optional | Receiving number |
|templateId|	String| 50 | Optional | Template number |
|msgStatus|	String| 1 | Optional | Message status code (1: requesting 2: processing, 3:successful) |
|resultCode|	String| 10 | Optional | Result code of receiving [[Table on Query Codes](./error-code/#_2)] |
|subResultCode|	String| 10 | Optional | Detail code of receiving [[Table on Query Codes](./error-code/#_3)] |
|senderGroupingKey|	String| 100 | Optional | Sender's group key |
|recipientGroupingKey|	String| 100 | Optional | Recipient's group key |
|isIncludeTitleAndBody | Boolean | - | Optional | Title and body included or not |

#### Response

```
{
  "header":{
    "isSuccessful":true,
    "resultCode":0,
    "resultMessage":"SUCCESS"
  },
  "body":{
    "data":{
      "donwloadId":"20190610100630ReZQ6KZzAH0",
      "downloadType":"NORMAL",
      "fileType":"CSV",
      "downloadStatusCode":"COMPLETED",
      "expiredDate":"2019-07-09 10:06:00.0"
    }
  }
}
```

|Value| Type | Description |
|---|---|---|
|header.isSuccessful|	Boolean| Successful or not |
|header.resultCode|	Integer| Failure code |
|header.resultMessage|	String| Failure message |
|body.data.donwloadId|	String| Download ID |
|body.data.downloadType|	String| Download type<br/>- BLOCK: Block receiving<br/>- NORMAL: General delivery<br/>- MASS: Mass delivery<br/>- TAG: Tag delivery |
|body.data.fileType|	String| File type (currently supports csv only) |
|body.data.downloadStatusCode|	String| Status of File Creation<br/>- READY: Preparing to create<br/>- MAKING: Creating<br/>- COMPLETED: Creation completed<br/>- FAILED: Creation failed<br/>- EXPIRED: Download period expired |
|body.data.expiredDate|	String|	Date and time of expiration for download period|\


### Query Request History for Delivery Result of File Creation

#### Request

[URL]

```
GET /sms/v2.2/appKeys/{appKey}/download-reservations?downloadId={downloadId}&downloadStatusCode={downloadStatusCode}&pageNum={pageNum}&pageSize={pageSize}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Query parameter]

|Value| Type | Max Length | Required | Description |
|---|---|---|---|---|
|downloadId|	String|	25 | Optional | Download ID |
|downloadStatusCode|	String| 10 | Optional | Status code of download file |
|pageNum|	Integer|	- | Optional | Page number (default: 1) |
|pageSize|	Integer|	1000 | Optional | Number of queries (default: 15) |

#### Response

```json
{
  "header":{
    "isSuccessful":true,
    "resultCode":0,
    "resultMessage":"SUCCESS"
  },
  "body":{
    "totalCount":0,
    "data":[
      {
        "downloadId":"",
        "downloadType":"",
        "fileType":"",
        "parameter":"",
        "size":0,
        "downloadStatusCode":"",
        "resultMessage":"",
        "expiredDate":"",
        "createUser":"",
        "createDate":"",
        "updateDate":""
      }
    ]
  }
}
```

|Value| Type | Description |
|---|---|---|
|header.isSuccessful|	Boolean| Successful or not |
|header.resultCode|	Integer| Failure code |
|header.resultMessage|	String| Failure message |
|body.totalCount| Integer | Total count |
|body.data[].downloadId| String | Download ID |
|body.data[].downloadType| String | Download type<br/>- BLOCK: Block receiving<br/>- NORMAL: General delivery<br/>- MASS: Mass delivery<br/>- TAG: Tag delivery |
|body.data[].fileType| String | File type |
|body.data[].parameter| String | Request parameter |
|body.data[].size| Integer | Size of query data |
|body.data[].downloadStatusCode| String | Status of file creation<br/>- READY: Preparing to create<br/>- MAKING: Creating<br/>- COMPLETED: Creation completed<br/>- FAILED: Creation failed<br/>- EXPIRED: Download period expired |
|body.data[].resultMessage| String | Result message (respond when it fails) |
|body.data[].expiredDate| String | Date and time of file expiration |
|body.data[].createUser| String | Requester for file creation |
|body.data[].createDate| String | Date and time of request for file creation |
|body.data[].updateDate| String | Date and time of completion or failure of file creation |


### Request for Downloading Delivery Result Files 

#### Request 

[URL]

```
GET /sms/v2.2/appKeys/{appKey}/download-reservations/{downloadId}/download
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |
|downloadId| String | Download ID |

#### Response

```
file byte
```
