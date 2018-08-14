## Notification > SMS > API Guide

## v2.1 API 소개

### v2.0과 달라진 사항
1) 일반 발송(SMS/LMS/MMS/AUTH) 시 SenderGroupingKey, RecipientGroupingKey 필드를 추가하여 발송할 수 있습니다.
2) 일반 발송 요청 후 응답 값에 SenderGroupingKey, RecipientGroupingKey, RecipientSeq(MtPr) 값이 추가되었습니다.
3) 일반 발송 목록 조회 시 필터 조건에 SenderGroupingKey, RecipientGroupingKey이 추가되었습니다.

### [API 도메인]

|환경|	도메인|
|---|---|
|Real|	https://api-sms.cloud.toast.com|

### [주의 사항]
* SMS은 본문 길이 90byte이하의 단문메시지이며, MMS는 본문 길이 2,000byte이하, 제목 40byte 이하로 발송해야 합니다. 해당 byte 이상 발송 시, 내용이 잘려 나갈 수 있습니다.</br>
* 본문과 제목은 euc-kr 기준으로 발송 됩니다. 따라서 euc-kr 인코딩이 지원하지 않는 이모티콘은 발송 실패 처리 됩니다.</br>

## 단문 SMS

### 단문 SMS 발송

#### 요청

[URL]

```
POST  /sms/v2.1/appKeys/{appKey}/sender/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Request body]

```
{
   "templateId":"TemplateId",
   "body":"본문",
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

|값|	타입|	필수|	설명|
|---|---|---|---|
|templateId|	String|	X|	발송 템플릿 아이디|
|body|	String|	O|	본문 내용('EUC-KR' 기준으로 90 Byte 제한)|
|sendNo|	String|	O|	발신번호|
|requestDate| String| X | 예약일시(yyyy-MM-dd HH:mm)|
|senderGroupingKey| String| X | 발신자 그룹키 |
|recipientList[].recipientNo|	String|	O|	수신번호<br/>countryCode와 조합하여 사용 가능|
|recipientList[].countryCode|	String|	X|	국가번호 [기본값: 82(한국)] <br>(국제 발송 시, euc-kr(한글,영문) 내용만 가능합니다.) |
|recipientList[].internationalRecipientNo| String| X| 국가번호가 포함된 수신번호<br/>예)821012345678<br/>recipientNo가 있을 경우 이 값은 무시된다.<br/>|
|recipientList[].templateParameter|	Object|	X|	템플릿 파라미터(템플릿 아이디 입력 시)|
|recipientList[].templateParameter.{key}|	String|	X|	치환 키(##key##)|
|recipientList[].templateParameter.{value}|	Object|	X|	치환 키에 매핑되는 Value값|
|recipientList[].recipientGroupingKey| String| X | 수신자 그룹키 |
|userId|	String|	X | 발송 구분자 ex)admin,system |

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

|값|	타입|	설명|
|---|---|---|
|header.isSuccessful|	Boolean|	성공여부|
|header.resultCode|	Integer|	실패 코드|
|header.resultMessage|	String|	실패 메시지|
|body.data.requestId|	String|	요청 아이디|
|body.data.statusCode|	String|	요청 상태 코드(1:요청중, 2:요청완료, 3:요청실패)|
|body.data.senderGroupingKey|	String|	발신자 그룹키|
|body.data.sendResultList[].recipientNo| String | 수신번호|
|body.data.sendResultList[].resultCode| Integer | 결과코드|
|body.data.sendResultList[].resultMessage| String | 결과메시지|
|body.data.sendResultList[].recipientSeq| Integer | 수신자 시퀀스(mtPr)|
|body.data.sendResultList[].recipientGroupingKey| String | 수신자 그룹키|

#### 단문 SMS 발송 예제(일반 국내 수신 번호)

| Http metho | URL |
| - | - |
| POST | https://api-sms.cloud.toast.com/sms/v2.1/appKeys/{appKey}/sender/sms|

[Request body]
```
{
   "body":"본문",
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
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-sms.cloud.toast.com/sms/v2.1/appKeys/{appKey}/sender/sms -d '{"body": "{본문 내용}","sendNo": "15446859","senderGroupingKey":"SenderGroupingKey","recipientList":[{"recipientNo": "01000000000","recipientGroupingKey":"RecipientGroupingKey"},{"recipientNo": "01000000002","recipientGroupingKey":"RecipientGroupingKey2"}]}'
```

#### 단문 SMS 발송 예제(국가 코드가 포함된 수신 번호)

| Http metho | URL |
| - | - |
| POST | https://api-sms.cloud.toast.com/sms/v2.1/appKeys/{appKey}/sender/sms|


[Request body]
```
{
   "body":"본문",
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
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-sms.cloud.toast.com/sms/v2.1/appKeys/{appKey}/sender/sms -d '{"body": "본문","sendNo": "15446859","recipientList": [{"internationalRecipientNo": "821000000000"}]}'
```

### 단문 SMS 발송리스트 조회

#### 요청

[URL]

```
GET  /sms/v2.1/appKeys/{appKey}/sender/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Query parameter] 1번 or 2번 조건 필수

|값|	타입|	필수|	설명|
|---|---|---|---|
|requestId|	String|	조건 필수 (1번) |	요청 아이디|
|startRequestDate|	String|	조건 필수 (2번) |	발송 날짜 시작 값(yyyy-MM-dd HH:mm:ss)|
|endRequestDate|	String|	조건 필수 (2번) |	발송 날짜 종료 값(yyyy-MM-dd HH:mm:ss)|
|startResultDate|	String|	옵션|	수신 날짜 시작 값(yyyy-MM-dd HH:mm:ss)|
|endResultDate|	String|	옵션|	수신 날짜 종료 값(yyyy-MM-dd HH:mm:ss)|
|sendNo|	String|	옵션|	발신번호|
|recipientNo|	String|	옵션|	수신번호|
|templateId|	String|	옵션|	템플릿번호|
|msgStatus|	String|	옵션|	메시지 상태 코드(1:요청, 2:처리중, 3:성공)|
|resultCode|	String|	옵션|	수신 결과 코드 [[조회 코드표](./error-code/#_2)]|
|subResultCode|	String|	옵션|	수신 결과 상세 코드 [[조회 코드표](./error-code/#_3)]|
|senderGroupingKey|	String|	옵션|	발송자 그룹키|
|recipientGroupingKey|	String|	옵션|	수신자 그룹키|
|subResultCode|	String|	옵션|	수신 결과 상세 코드 [[조회 코드표](./error-code/#_3)]|
|pageNum|	Integer|	옵션|	페이지 번호(Default : 1)|
|pageSize|	Integer|	옵션|	조회 건수(Default : 15)|

#### 응답

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
            "templateName":"템플릿명",
            "categoryId":0,
            "categoryName":"카테고리명",
            "body":"단문 테스트",
            "sendNo":"15446859",
            "countryCode":"82",
            "recipientNo":"01000000000",
            "msgStatus":"3",
            "msgStatusName":"성공",
            "resultCode":"1000",
            "resultCodeName":"성공",
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

|값|	타입|	설명|
|---|---|---|
|header.isSuccessful|	Boolean|	성공여부|
|header.resultCode|	Integer|	실패 코드|
|header.resultMessage|	String|	실패 메시지|
|body.pageNum|	Integer|	현재 페이지 번호|
|body.pageSize|	Integer|	조회된 데이터 건수|
|body.totalCount|	Integer|	총 데이터 건수|
|body.data[].requestId|	String|	요청 아이디|
|body.data[].requestDate|	String|	발신일시|
|body.data[].resultDate|	String|	수신일시|
|body.data[].templateId|	String|	템플릿아이디|
|body.data[].templateName|	String|	템플릿명|
|body.data[].categoryId|	String|	카테고리아이디|
|body.data[].categoryName|	String|	카테고리명|
|body.data[].body|	String|	본문내용|
|body.data[].sendNo|	String|	발신번호|
|body.data[].countryCode|	String|	국가번호|
|body.data[].recipientNo|	String|	수신번호|
|body.data[].msgStatus|	String|	메시지 상태 코드|
|body.data[].msgStatusName|	String|	메시지 상태 코드명|
|body.data[].resultCode|	String|	수신 결과 코드 [[수신 결과 코드표](./error-code/#emma-v3)]|
|body.data[].resultCodeName|	String|	수신 결과 코드명|
|body.data[].telecomCode|	Integer|	통신사코드|
|body.data[].telecomCodeName|	String|	통신사명|
|body.data[].mtPr|	Integer|	발송상세아이디(상세 조회 시 필수)|
|body.data[].sendType|	String|	발송 타입(0:SMS, 1:MMS, 2:Auth)|
|body.data[].userId|	String|	발송 요청 아이디|
|body.data[].adYn|	String|	광고 여부|
|body.data[].senderGroupingKey|	String|	발신자 그룹키|
|body.data[].recipientGroupingKey|	String|	수신자 그룹키|

### 단문 SMS 발송 단일 조회

#### 요청

[URL]

```
GET  /sms/v2.1/appKeys/{appKey}/sender/sms/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
|requestId|	String|	요청아이디|

[Query parameter]

|값|	타입|	필수|	설명|
|---|---|---|---|
|mtPr|	Integer|	필수|	발송상세아이디|

#### 응답

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
         "templateName":"템플릿명",
         "categoryId":0,
         "categoryName":"카테고리명",
         "body":"본문",
         "sendNo":"15446859",
         "countryCode":"82",
         "recipientNo":"01000000000",
         "msgStatus":"3",
         "msgStatusName":"성공",
         "resultCode":"1000",
         "resultCodeName":"성공",
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

|값|	타입|	설명|
|---|---|---|
|header.isSuccessful|	Boolean|	성공여부|
|header.resultCode|	Integer|	실패 코드|
|header.resultMessage|	String|	실패 메시지|
|body.data.requestId|	String|	요청 아이디|
|body.data.requestDate|	String|	발신일시|
|body.data.resultDate|	String|	수신일시|
|body.data.templateId|	String|	템플릿아이디|
|body.data.templateName|	String|	템플릿명|
|body.data.categoryId|	String|	카테고리아이디|
|body.data.categoryName|	String|	카테고리명|
|body.data.body|	String|	본문내용|
|body.data.sendNo|	String|	발신번호|
|body.data.countryCode|	String|	국가번호|
|body.data.recipientNo|	String|	수신번호|
|body.data.msgStatus|	String|	메시지 상태 코드|
|body.data.msgStatusName|	String|	메시지 상태 코드명|
|body.data.resultCode|	String|	수신 결과 코드 [[수신 결과 코드표](./error-code/#emma-v3)]|
|body.data.resultCodeName|	String|	수신 결과 코드명|
|body.data.telecomCode|	Integer|	통신사코드|
|body.data.telecomCodeName|	String|	통신사명|
|body.data.mtPr|	Integer|	발송상세아이디(상세 조회 시 필수)|
|body.data.sendType|	String|	발송 타입(0:SMS, 1:MMS, 2:Auth)|
|body.data.userId|	String|	발송 요청 아이디|
|body.data.adYn|	String|	광고 여부|
|body.data.senderGroupingKey|	String|	발신자 그룹키|
|body.data.recipientGroupingKey|	String|	수신자 그룹키|

## 장문 MMS

### 장문 MMS 발송(첨부파일 미포함)

#### 요청

[URL]

```
POST  /sms/v2.1/appKeys/{appKey}/sender/mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

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

|값|	타입|	필수|	설명|
|---|---|---|---|
|templateId|	String|	X|	발송 템플릿 아이디|
|title|	String|	O|	제목 <br/> ('EUC-KR' 기준으로 40Byte 제한) <br/> (영문: 1byte, 한글: 2byte)|
|body|	String|	O|	본문 <br/> ('EUC-KR' 기준으로 2000Byte 제한) <br/> (영문: 1byte, 한글: 2byte)|
|sendNo|	String|	O|	발신번호|
|requestDate| String| X | 예약일시(yyyy-MM-dd HH:mm)|
|senderGroupingKey| String| X | 발신자 그룹키 |
|recipientList[].recipientNo|	String|	O|	수신번호<br/>countryCode와 조합하여 사용 가능|
|recipientList[].countryCode|	String|	X|	국가번호 [기본값: 82(한국)] <br>(국제 발송 시, euc-kr(한글,영문) 내용만 가능합니다.) |
|recipientList[].internationalRecipientNo| String| X| 국가번호가 포함된 수신번호<br/>예)821012345678<br/>recipientNo가 있을 경우 이 값은 무시된다.<br/>|
|recipientList[].templateParameter|	Object|	X|	템플릿 파라미터(템플릿 아이디 입력 시)|
|recipientList[].templateParameter.{key}|	String|	X|	치환 키(##key##)|
|recipientList[].templateParameter.{value}|	Object|	X|	치환 키에 매핑되는 Value값|
|recipientList[].recipientGroupingKey| String| X | 수신자 그룹키 |
|userId|	String|	X | 발송 구분자 ex)admin,system |

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

|값|	타입|	설명|
|---|---|---|
|header.isSuccessful|	Boolean|	성공여부|
|header.resultCode|	Integer|	실패 코드|
|header.resultMessage|	String|	실패 메시지|
|body.data.requestId|	String|	요청 아이디|
|body.data.statusCode|	String|	요청 상태 코드(1:요청중, 2:요청완료, 3:요청실패)|
|body.data.senderGroupingKey|	String|	발신자 그룹키|
|body.data.sendResultList[].recipientNo| String | 수신번호|
|body.data.sendResultList[].resultCode| Integer | 결과코드|
|body.data.sendResultList[].resultMessage| String | 결과메시지|
|body.data.sendResultList[].recipientSeq| Integer | 수신자 시퀀스(mtPr)|
|body.data.sendResultList[].recipientGroupingKey| String | 수신자 그룹키|

#### 장문 MMS 발송 예제

| Http metho | URL |
| - | - |
| POST | https://api-sms.cloud.toast.com/sms/v2.1/appKeys/{appKey}/sender/mms|

[Request body]
```
{
   "title": "제목",
   "body":"본문",
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
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-sms.cloud.toast.com/sms/v2.1/appKeys/{appKey}/sender/mms -d '{"title": "{제목}","body": "{본문 내용}","sendNo": "{발신번호}","recipientList": [{"recipientNo": "{수신번호}","templateParameter": { }}],"userId": ""}'
```

### 장문 MMS 발송(첨부파일 포함)

#### 첨부파일 업로드

#### 요청

[URL]

```
POST  /sms/v2.1/appKeys/{appKey}/attachfile/binaryUpload
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Request body]

```
{
    "fileName": "attachment.jpg",
    "createUser": "CreateUser",
    // "fileBody": [0,10,16]
    "fileBody": "{byte[] -> Base64 인코딩한 값}"
}
```

|값|	타입|	필수|	설명|
|---|----|----|---|
|fileName|	String|	필수|	45글자 이내<br/>파일이름(확장자 jpg,jpeg(소문자)만 가능)|
|fileBody|	Byte[]|	필수| * 파일 byte[]를 Base64로 인코딩한 값.<br/>* 또는 byte 배열 값<br/>300K 이하|
|createUser|	String|	필수|	파일 업로드 유저 정보|

#### 응답

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

|값|	타입|	설명|
|---|---|---|
|header.isSuccessful|	Boolean|	성공여부|
|header.resultCode|	Integer|	실패 코드|
|header.resultMessage|	String|	실패 메시지|
|body.data.fileId|	Integer|	파일아이디|
|body.data.fileName|	String|	파일명|
|body.data.filePath|	String|	첨부파일 기본Path <br/> (https://domain/attachFile/filePath/fileName)|

#### 첨부파일 업로드 예제

| Http method | URL |
| - | - |
| POST | https://api-sms.cloud.toast.com/sms/v2.1/appKeys/{appKey}/attachfile/binaryUpload |

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

#### 첨부파일 발송 예제

| Http method | URL |
| - | - |
| POST | https://api-sms.cloud.toast.com/sms/v2.1/appKeys/{appKey}/sender/mms |

[Request body]
```
{
    "title": "제목",
    "body": "본문",
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
              "recipientNo" : {수신번호},
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

### 장문 MMS 발송리스트 조회

#### 요청

[URL]

```
GET  /sms/v2.1/appKeys/{appKey}/sender/mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Query parameter] 1번 or 2번 조건 필수

|값|	타입|	필수|	설명|
|---|---|---|---|
|requestId|	String|	조건 필수 (1번) |	요청 아이디|
|startRequestDate|	String|	조건 필수 (2번) |	발송 날짜 시작 값(yyyy-MM-dd HH:mm:ss)|
|endRequestDate|	String|	조건 필수 (2번) |	발송 날짜 종료 값(yyyy-MM-dd HH:mm:ss)|
|startResultDate|	String|	옵션|	수신 날짜 시작 값(yyyy-MM-dd HH:mm:ss)|
|endResultDate|	String|	옵션|	수신 날짜 종료 값(yyyy-MM-dd HH:mm:ss)|
|sendNo|	String|	옵션|	발신번호|
|recipientNo|	String|	옵션|	수신번호|
|templateId|	String|	옵션|	템플릿번호|
|msgStatus|	String|	옵션|	메시지 상태 코드(1:요청, 2:처리중, 3:성공)|
|resultCode|	String|	옵션|	수신 결과 코드 [[조회 코드표](./error-code/#_2)]|
|subResultCode|	String|	옵션|	수신 결과 상세 코드 [[조회 코드표](./error-code/#_3)]|
|senderGroupingKey|	String|	옵션|	발송자 그룹키|
|recipientGroupingKey|	String|	옵션|	수신자 그룹키|
|pageNum|	Integer|	옵션|	페이지 번호(Default : 1)|
|pageSize|	Integer|	옵션|	조회 건수(Default : 15)|

#### 응답

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
        "templateName":"템플릿명",
        "categoryId":0,
        "categoryName":"카테고리명",
        "title":"제목",
        "body":"본문",
        "sendNo":"15446859",
        "countryCode":"82",
        "recipientNo":"01000000000",
        "msgStatus":"3",
        "msgStatusName":"성공",
        "resultCode":"1000",
        "resultCodeName":"성공",
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

|값|	타입|	설명|
|---|---|---|
|header.isSuccessful|	Boolean|	성공여부|
|header.resultCode|	Integer|	실패 코드|
|header.resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|body.pageNum|	Integer|	현재 페이지 번호|
|body.pageSize|	Integer|	조회된 데이터 건수|
|body.totalCount|	Integer|	총 데이터 건수|
|body.data[].requestId|	String|	요청 아이디|
|body.data[].requestDate|	String|	발신일시|
|body.data[].resultDate|	String|	수신일시|
|body.data[].templateId|	String|	템플릿아이디|
|body.data[].templateName|	String|	템플릿명|
|body.data[].categoryId|	String|	카테고리아이디|
|body.data[].categoryName|	String|	카테고리명|
|body.data[].body|	String|	본문내용|
|body.data[].sendNo|	String|	발신번호|
|body.data[].countryCode|	String|	국가번호|
|body.data[].recipientNo|	String|	수신번호|
|body.data[].msgStatus|	String|	메시지 상태 코드|
|body.data[].msgStatusName|	String|	메시지 상태 코드명|
|body.data[].resultCode|	String|	수신 결과 코드 [[수신 결과 코드표](./error-code/#emma-v3)]|
|body.data[].resultCodeName|	String|	수신 결과 코드명|
|body.data[].telecomCode|	Integer|	통신사코드|
|body.data[].telecomCodeName|	String|	통신사명|
|body.data[].mtPr|	Integer|	발송상세아이디(상세 조회 시 필수)|
|body.data[].sendType|	String|	발송 타입(0:SMS, 1:MMS, 2:Auth)|
|body.data[].userId|	String|	발송 요청 아이디|
|body.data[].adYn|	String|	광고 여부|
|body.data[].attachFileList[].fileId|	Integer|	파일 아이디|
|body.data[].attachFileList[].filePath|	String|	파일 저장경로(내부용)|
|body.data[].attachFileList[].fileName|	String|	파일명|
|body.data[].senderGroupingKey|	String|	발신자 그룹키|
|body.data[].recipientGroupingKey|	String|	수신자 그룹키|


### 장문 MMS 발송 단일 조회

#### 요청

[URL]

```
GET  /sms/v2.1/appKeys/{appKey}/sender/mms/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
|requestId|	String|	요청아이디|

[Query parameter]

|값|	타입|	필수|	설명|
|---|---|---|---|
|mtPr|	Integer|	필수|	발송상세아이디|

#### 응답

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
      "templateName":"템플릿명",
      "categoryId":0,
      "categoryName":"카테고리명",
      "title":"제목",
      "body":"본문",
      "sendNo":"15446859",
      "countryCode":"82",
      "recipientNo":"01000000000",
      "msgStatus":"3",
      "msgStatusName":"성공",
      "resultCode":"1000",
      "resultCodeName":"성공",
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

|값|	타입|	설명|
|---|---|---|
|header.isSuccessful|	Boolean|	성공여부|
|header.resultCode|	Integer|	실패 코드|
|header.resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|body.pageNum|	Integer|	현재 페이지 번호|
|body.pageSize|	Integer|	조회된 데이터 건수|
|body.totalCount|	Integer|	총 데이터 건수|
|body.data[].requestId|	String|	요청 아이디|
|body.data[].requestDate|	String|	발신일시|
|body.data[].resultDate|	String|	수신일시|
|body.data[].templateId|	String|	템플릿아이디|
|body.data[].templateName|	String|	템플릿명|
|body.data[].categoryId|	String|	카테고리아이디|
|body.data[].categoryName|	String|	카테고리명|
|body.data[].body|	String|	본문내용|
|body.data[].sendNo|	String|	발신번호|
|body.data[].countryCode|	String|	국가번호|
|body.data[].recipientNo|	String|	수신번호|
|body.data[].msgStatus|	String|	메시지 상태 코드|
|body.data[].msgStatusName|	String|	메시지 상태 코드명|
|body.data[].resultCode|	String|	수신 결과 코드 [[수신 결과 코드표](./error-code/#emma-v3)]|
|body.data[].resultCodeName|	String|	수신 결과 코드명|
|body.data[].telecomCode|	Integer|	통신사코드|
|body.data[].telecomCodeName|	String|	통신사명|
|body.data[].mtPr|	Integer|	발송상세아이디(상세 조회 시 필수)|
|body.data[].sendType|	String|	발송 타입(0:SMS, 1:MMS, 2:Auth)|
|body.data[].userId|	String|	발송 요청 아이디|
|body.data[].adYn|	String|	광고 여부|
|body.data[].attachFileList[].fileId|	Integer|	파일 아이디|
|body.data[].attachFileList[].filePath|	String|	파일 저장경로(내부용)|
|body.data[].attachFileList[].fileName|	String|	파일명|
|body.data[].senderGroupingKey|	String|	발신자 그룹키|
|body.data[].recipientGroupingKey|	String|	수신자 그룹키|

## 인증용 SMS(긴급)

### 인증용 SMS 발송

#### 요청

[URL]

```
POST  /sms/v2.1/appKeys/{appKey}/sender/auth/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Request body]

```
{
   "templateId":"TemplateId",
   "body":"본문",
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

|값|	타입|	필수|	설명|
|---|---|---|---|
|templateId|	String|	X|	발송 템플릿 아이디|
|body|	String|	O|	본문 내용('EUC-KR' 기준으로 90 Byte 제한)|
|sendNo|	String|	O|	발신번호|
|requestDate| String| X | 예약일시(yyyy-MM-dd HH:mm)|
|senderGroupingKey| String| X | 발신자 그룹키 |
|recipientList[].recipientNo|	String|	O|	수신번호<br/>countryCode와 조합하여 사용 가능|
|recipientList[].countryCode|	String|	X|	국가번호 [기본값: 82(한국)] <br>(국제 발송 시, euc-kr(한글,영문) 내용만 가능합니다.) |
|recipientList[].internationalRecipientNo| String| X| 국가번호가 포함된 수신번호<br/>예)821012345678<br/>recipientNo가 있을 경우 이 값은 무시된다.<br/>|
|recipientList[].templateParameter|	Object|	X|	템플릿 파라미터(템플릿 아이디 입력 시)|
|recipientList[].templateParameter.{key}|	String|	X|	치환 키(##key##)|
|recipientList[].templateParameter.{value}|	Object|	X|	치환 키에 매핑되는 Value값|
|recipientList[].recipientGroupingKey| String| X | 수신자 그룹키 |
|userId|	String|	X | 발송 구분자 ex)admin,system |


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

|값|	타입|	설명|
|---|---|---|
|header.isSuccessful|	Boolean|	성공여부|
|header.resultCode|	Integer|	실패 코드|
|header.resultMessage|	String|	실패 메시지|
|body.data.requestId|	String|	요청 아이디|
|body.data.statusCode|	String|	요청 상태 코드(1:요청중, 2:요청완료, 3:요청실패)|
|body.data.senderGroupingKey|	String|	발신자 그룹키|
|body.data.sendResultList[].recipientNo| String | 수신번호|
|body.data.sendResultList[].resultCode| Integer | 결과코드|
|body.data.sendResultList[].resultMessage| String | 결과메시지|
|body.data.sendResultList[].recipientSeq| Integer | 수신자 시퀀스(mtPr)|
|body.data.sendResultList[].recipientGroupingKey| String | 수신자 그룹키|

#### 예제

| Http metho | URL |
| - | - |
| POST | https://api-sms.cloud.toast.com/sms/v2.1/appKeys/{appKey}/sender/auth/sms|

[Request body]
```
{
   "body":"본문",
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
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-sms.cloud.toast.com/sms/v2.1/appKeys/{appKey}/sender/auth/sms -d '{"body": "{본문 내용}","sendNo": "{발신번호}","recipientList":[{"recipientNo": "{수신번호}","templateParameter": { }}],"userId": ""}'
```

### 인증용 SMS 발송리스트 조회

#### 요청

[URL]

```
GET  /sms/v2.1/appKeys/{appKey}/sender/auth/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|----|---|
|appKey|	String|	고유의 appKey|

[Query parameter] 1번 or 2번 조건 필수

|값|	타입|	필수|	설명|
|---|---|---|---|
|requestId|	String|	조건 필수 (1번) |	요청 아이디|
|startRequestDate|	String|	조건 필수 (2번) |	발송 날짜 시작 값(yyyy-MM-dd HH:mm:ss)|
|endRequestDate|	String|	조건 필수 (2번) |	발송 날짜 종료 값(yyyy-MM-dd HH:mm:ss)|
|startResultDate|	String|	옵션|	수신 날짜 시작 값(yyyy-MM-dd HH:mm:ss)|
|endResultDate|	String|	옵션|	수신 날짜 종료 값(yyyy-MM-dd HH:mm:ss)|
|sendNo|	String|	옵션|	발신번호|
|recipientNo|	String|	옵션|	수신번호|
|templateId|	String|	옵션|	템플릿번호|
|msgStatus|	String|	옵션|	메시지 상태 코드(1:요청, 2:처리중, 3:성공)|
|resultCode|	String|	옵션|	수신 결과 코드 [[조회 코드표](./error-code/#_2)]|
|subResultCode|	String|	옵션|	수신 결과 상세 코드 [[조회 코드표](./error-code/#_3)]|
|senderGroupingKey|	String|	옵션|	발송자 그룹키|
|recipientGroupingKey|	String|	옵션|	수신자 그룹키|
|subResultCode|	String|	옵션|	수신 결과 상세 코드 [[조회 코드표](./error-code/#_3)]|
|pageNum|	Integer|	옵션|	페이지 번호(Default : 1)|
|pageSize|	Integer|	옵션|	조회 건수(Default : 15)|

#### 응답

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
            "templateName":"템플릿명",
            "categoryId":0,
            "categoryName":"카테고리명",
            "body":"단문 테스트",
            "sendNo":"15446859",
            "countryCode":"82",
            "recipientNo":"01000000000",
            "msgStatus":"3",
            "msgStatusName":"성공",
            "resultCode":"1000",
            "resultCodeName":"성공",
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

|값|	타입|	설명|
|---|---|---|
|header.isSuccessful|	Boolean|	성공여부|
|header.resultCode|	Integer|	실패 코드|
|header.resultMessage|	String|	실패 메시지|
|body.pageNum|	Integer|	현재 페이지 번호|
|body.pageSize|	Integer|	조회된 데이터 건수|
|body.totalCount|	Integer|	총 데이터 건수|
|body.data[].requestId|	String|	요청 아이디|
|body.data[].requestDate|	String|	발신일시|
|body.data[].resultDate|	String|	수신일시|
|body.data[].templateId|	String|	템플릿아이디|
|body.data[].templateName|	String|	템플릿명|
|body.data[].categoryId|	String|	카테고리아이디|
|body.data[].categoryName|	String|	카테고리명|
|body.data[].body|	String|	본문내용|
|body.data[].sendNo|	String|	발신번호|
|body.data[].countryCode|	String|	국가번호|
|body.data[].recipientNo|	String|	수신번호|
|body.data[].msgStatus|	String|	메시지 상태 코드|
|body.data[].msgStatusName|	String|	메시지 상태 코드명|
|body.data[].resultCode|	String|	수신 결과 코드 [[수신 결과 코드표](./error-code/#emma-v3)]|
|body.data[].resultCodeName|	String|	수신 결과 코드명|
|body.data[].telecomCode|	Integer|	통신사코드|
|body.data[].telecomCodeName|	String|	통신사명|
|body.data[].mtPr|	Integer|	발송상세아이디(상세 조회 시 필수)|
|body.data[].sendType|	String|	발송 타입(0:SMS, 1:MMS, 2:Auth)|
|body.data[].userId|	String|	발송 요청 아이디|
|body.data[].adYn|	String|	광고 여부|
|body.data[].senderGroupingKey|	String|	발신자 그룹키|
|body.data[].recipientGroupingKey|	String|	수신자 그룹키|

### 인증용 SMS 발송 단일 조회

#### 요청

[URL]

```
GET  /sms/v2.1/appKeys/{appKey}/sender/auth/sms/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
|requestId|	String|	요청아이디|

[Query parameter]

|값|	타입|	필수|	설명|
|---|----|---|---|
|mtPr|	Integer|	필수|	발송상세아이디|

#### 응답

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
         "templateName":"템플릿명",
         "categoryId":0,
         "categoryName":"카테고리명",
         "body":"본문",
         "sendNo":"15446859",
         "countryCode":"82",
         "recipientNo":"01000000000",
         "msgStatus":"3",
         "msgStatusName":"성공",
         "resultCode":"1000",
         "resultCodeName":"성공",
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

|값|	타입|	설명|
|---|---|---|
|header.isSuccessful|	Boolean|	성공여부|
|header.resultCode|	Integer|	실패 코드|
|header.resultMessage|	String|	실패 메시지|
|body.data.requestId|	String|	요청 아이디|
|body.data.requestDate|	String|	발신일시|
|body.data.resultDate|	String|	수신일시|
|body.data.templateId|	String|	템플릿아이디|
|body.data.templateName|	String|	템플릿명|
|body.data.categoryId|	String|	카테고리아이디|
|body.data.categoryName|	String|	카테고리명|
|body.data.body|	String|	본문내용|
|body.data.sendNo|	String|	발신번호|
|body.data.countryCode|	String|	국가번호|
|body.data.recipientNo|	String|	수신번호|
|body.data.msgStatus|	String|	메시지 상태 코드|
|body.data.msgStatusName|	String|	메시지 상태 코드명|
|body.data.resultCode|	String|	수신 결과 코드 [[수신 결과 코드표](./error-code/#emma-v3)]|
|body.data.resultCodeName|	String|	수신 결과 코드명|
|body.data.telecomCode|	Integer|	통신사코드|
|body.data.telecomCodeName|	String|	통신사명|
|body.data.mtPr|	Integer|	발송상세아이디(상세 조회 시 필수)|
|body.data.sendType|	String|	발송 타입(0:SMS, 1:MMS, 2:Auth)|
|body.data.userId|	String|	발송 요청 아이디|
|body.data.adYn|	String|	광고 여부|
|body.data.senderGroupingKey|	String|	발신자 그룹키|
|body.data.recipientGroupingKey|	String|	수신자 그룹키|

## 광고 문자
### 광고성 SMS 발송
[URL]

```
POST  /sms/v2.1/appKeys/{appKey}/sender/ad-sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Request Body]
위에 SMS 발송과 동일.
[[Request Body 참고](./api-guide/#sms_2)]

<span style="color:red">단, 본문에 아래 문구가 필수로 들어가야 합니다.</span>
080번호는 콘솔의 080수신거부 설정 탭에서 확인 가능합니다.
```
(광고)

[무료 수신거부]080XXXXXXX
```


### 광고성 MMS 발송
[URL]

```
POST  /sms/v2.1/appKeys/{appKey}/sender/ad-mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Request Body]
위에 MMS 발송과 동일.
[[Request Body 참고](./api-guide/#mms_1)]

<span style="color:red">단, 본문에 아래 문구가 필수로 들어가야 합니다.</span>
080번호는 콘솔의 080수신거부 설정 탭에서 확인 가능합니다.
```
(광고)

[무료 수신거부]080XXXXXXX
```




## 태그 발송

### 태그 SMS 발송

#### 요청

[URL]

```
POST /sms/v2.1/appKeys/{appKey}/tag-sender/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Request body]

```
{
    "body":"본문",
    "sendNo":"15446859",
    "requestDate":"2018-03-22 10:00",
    "templateId":"TemplateId",
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

|값|	타입|	필수|	설명|
|---|---|---|---|
| body | String | O | 문자 내용 |
| sendNo | String | O | 발신번호 |
| requestDate| String| X | 예약일시(yyyy-MM-dd HH:mm)|
| templateId | String | X | 템플릿 아이디 |
| tagExpression | List<String> | O | 태그 표현식<br/>ex) ["tagA","AND","tabB"] |
| userId | String | X | 요청한 유저의 아이디 |
| adYn | String | X | 광고 여부(기본N) |
| autoSendYn | String | X | 자동 발송(즉시 발송) 여부 (기본 Y) |


#### 응답
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

|값|	타입|	설명|
|---|---|---|
|header.isSuccessful|	Boolean|	성공여부|
|header.resultCode|	Integer|	실패 코드|
|header.resultMessage|	String|	실패 메시지|
|body.data.requestId|	String|	요청 아이디|

### 태그 LMS 발송

#### 요청

[URL]

```
POST  /sms/v2.1/appKeys/{appKey}/tag-sender/mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Request body]
```
{
    "body":"본문",
    "sendNo":"15446859",
    "requestDate":"2018-03-22 10:00",
    "templateId":"TemplateId",
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

|값|	타입|	필수|	설명|
|---|---|---|---|
| title | String | O | 문자 제목 |
| body | String | O | 문자 내용 |
| sendNo | String | O | 발신번호 |
| requestDate| String| X | 예약일시(yyyy-MM-dd HH:mm)|
| templateId | String | X | 템플릿 아이디 |
| tagExpression | List<String> | O | 태그 표현식<br/>ex) ["tagA","AND","tabB"] |
| attachFileIdList | List<Integer> | X | 첨부파일 아이디(fileId) |
| userId | String | X | 요청한 유저의 아이디 |
| adYn | String | X | 광고 여부(기본N) |
| autoSendYn | String | X | 자동 발송(즉시 발송) 여부 (기본 Y) |


#### 응답
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

|값|	타입|	설명|
|---|---|---|
|header.isSuccessful|	Boolean|	성공여부|
|header.resultCode|	Integer|	실패 코드|
|header.resultMessage|	String|	실패 메시지|
|body.data.requestId|	String|	요청 아이디|


### 태그 발송 리스트 조회

#### 요청

[URL]

```
GET /sms/v2.1/appKeys/{appKey}/tag-sender?sendType={sendType}&requestId={requestId}&startRequestDate={startRequestDate}&endRequestDate={endRequestDate}&statusCode={statusCode}&pageNum={pageNum}&pageSize={pageSize}
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Request body]

```
X
```

* requestId 또는 startRequestDate + endRequestDate는 필수입니다.

|값|	타입|	필수|	설명|
|---|---|---|---|
| appKey | String| O | 앱키 |
| sendType | required, String | O | 발송 타입<br>SMS : "0",<br>MMS : "1" |
| requestId | String | O | 요청 아이디 |
| startRequestDate | String | O | 발송 날짜 시작 |
| endRequestDate | String | O | 발송 날짜 종료 |
| statusCode | String | X | 발송상태코드<br>WAIT : "MAS00"<br>READY : "MAS01"<br>SENDREADY : "MAS09"<br>SENDWAIT : "MAS10"<br>SENDING : "MAS11"<br>COMPLETE : "MAS19"<br>CANCEL : "MAS91"<br>FAIL : "MAS99" |
| pageNum | optional, Integer | X | 페이지 번호 |
| pageSize | optional, Integer | X | 조회건수 |

#### 응답
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
                "templateName": "템플릿명",
                "masterStatusCode": "READY",
                "sendNo": "15446859",
                "requestDate": "2017-12-20 14:15:58",
                "tagExpression": [
                    "Kb6BjCY1"
                ],
                "title": "제목",
                "body": "본문",
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

|값|	타입|	설명|
|---|---|---|
|header.isSuccessful|	Boolean|	성공여부|
|header.resultCode|	Integer|	실패 코드|
|header.resultMessage|	String|	실패 메시지|
|body.data[].requestId | String | 요청 아이디 |
|body.data[].requestIp | String | 요청 아이피 |
|body.data[].requestDate | String | 요청 시간 |
|body.data[].tagSendStatus | String | 태그 발송 상태 |
|body.data[].tagExpression[] | List<String> | 태그 표현식 |
|body.data[].templateId | String | 템플릿 아이디 |
|body.data[].templateName | String | 템플릿명 |
|body.data[].senderName | String | 발신자명 |
|body.data[].senderMail | String | 발신자주소 |
|body.data[].title | String | 제목 |
|body.data[].body | String | 내용 |
|body.data[].adYn | String | 광고여부 |
|body.data[].autoSendYn | String | 자동 발송 여부 |
|body.data[].sendErrorCount | Integer | 에러 수신자 건수 |
|body.data[].createUser | String | 생성자 |
|body.data[].createDate | String | 생성일시 |
|body.data[].updateUser | String | 수정자 |
|body.data[].updateDate | String | 수정일시 |


### 태그 발송 수신자 리스트 조회

#### 요청

[URL]

```
GET /sms/v2.1/appKeys/{appKey}/tag-sender/{requestId}?recipientNum={recipientNum}&startRequestDate={startRequestDate}&endRequestDate={endRequestDate}&startResultDate={startResultDate}&endResultDate={endResultDate}&msgStatusName={msgStatusName}&resultCode={resultCode}&pageNum={pageNum}&pageSize={pageSize}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
| requestId | String | 요청 아이디 |
[Request body]

```
X
```

* requestId 또는 startRequestDate + endRequestDate는 필수입니다.

|값|	타입|	필수|	설명|
|---|---|---|---|
| recipientNum | String | X | 수신자 번호 |
| startRequestDate | String | O | 발송 요청 시작 날짜 |
| endRequestDate | String | O | 발송 요청 종료 날짜 |
| startResultDate | String | X | 수신 시작 날짜 |
| endResultDate | | | |
| msgStatusName | | | |
| resultCode | | | |
| pageNum | | | |
| pageSize | | | |

#### 응답
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

|값|	타입|	설명|
|---|---|---|
|header.isSuccessful|	Boolean|	성공여부|
|header.resultCode|	Integer|	실패 코드|
|header.resultMessage|	String|	실패 메시지|
|body.data.requestId | String | 요청 아이디 |
|body.data.recipientSeq | Integer | 수신자 시퀀스 |
|body.data.countryCode | String | 수신자 국가코드 |
|body.data.recipientNo | String | 수신자 번호 |
|body.data.requestDate | String | 요청 일시 |
|body.data.msgStatus | String | 메세지 상태 코드 |
|body.data.msgStatusName | String | 메세지 상태 코드명 |
|body.data.resultCode | String | 수신 결과 코드[[수신 결과 코드표](./error-code/#emma-v3)] |
|body.data.receiveDate | String | 수신 일시 |
|body.data.createDate | String | 등록 일시 |
|body.data.updateDate | String | 수정 일시 |



### 태그 발송 수신자 리스트 상세 조회

#### 요청

[URL]

```
GET /sms/v2.1/appKeys/{appKey}/tag-sender/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
| requestId | String | 요청 아이디 |
| recipientSeq | String | 시퀀스 |

[Request body]

```
X
```

#### 응답
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
            "templateName": "템플릿명",
            "sendNo": "15446859",
            "recipientNum": "01000000000",
            "countryCode": "82",
            "title": "제목",
            "body": "본문",
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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|header.isSuccessful|	Boolean|	성공여부|
|header.resultCode|	Integer|	실패 코드|
|header.resultMessage|	String|	실패 메시지|
|body.data.requestId | String | 요청 아이디 |
|body.data.recipientSeq | Integer | 수신자 시퀀스 |
|body.data.sendType | String | 발송 타입 |
|body.data.messageType | String | 메세지 타입 |
|body.data.templateId | String | 템플릿 아이디 |
|body.data.templateName | String | 템플릿명 |
|body.data.sendNo | String | 발신번호 |
|body.data.title | String | 제목 |
|body.data.body | String | 내용 |
|body.data.recipientNum | String | 수신자 번호 |
|body.data.requestDate | String | 요청일시 |
|body.data.msgStatusName | String | 메시지상태이름 |
|body.data.resultCode | String | 수신 결과 코드[[수신 결과 코드표](./error-code/#emma-v3)] |
|body.data.receiveDate | String | 수신일시 |
|body.data.attachFileList[].filePath | String | 첨부파일 - 경로 |
|body.data.attachFileList[].fileName | String | 첨부파일 - 파일명 |
|body.data.attachFileList[].fileSize | Long | 첨부파일 - 사이즈 |
|body.data.attachFileList[].fileSequence | Integer | 첨부파일 - 파일 번호 |
|body.data.attachFileList[].createDate | String | 첨부파일 - 생성일시 |
|body.data.attachFileList[].updateDate | String | 첨부파일 - 수정일시 |




## 템플릿

### 템플릿 발송(본문 수정이 필요 없는 경우)

#### 예제
![[그림 1] 템플릿 등록](http://static.toastoven.net/prod_sms/img_26.png)

|Http method| 종류 | URL|
| - | - | - |
| POST | SMS | https://api-sms.cloud.toast.com/sms/v2.1/appKeys/{appKey}/sender/sms |
| POST | MMS | https://api-sms.cloud.toast.com/sms/v2.1/appKeys/{appKey}/sender/mms |

Request URL은 템플릿 등록시 선택한 발송타입으로 선택하여 발송.

** Request parameter body 안의 값이 공란이면 해당 templateId의 body 내용으로 치환. **

[Request body] 치환하고자 하는 키와 값을 key,value로 대입.

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

### 템플릿 발송(본문 수정이 필요한 경우)

#### 템플릿 발송 예제

|Http method| 종류 | URL|
| - | - | - |
| POST | SMS | https://api-sms.cloud.toast.com/sms/v2.1/appKeys/{appKey}/sender/sms |
| POST | MMS | https://api-sms.cloud.toast.com/sms/v2.1/appKeys/{appKey}/sender/mms |


Request URL은 템플릿 등록시 선택한 발송타입으로 선택하여 발송.

** 템플릿 아이디와 Request parameter body 안의 값이 있을 경우, 발신번호와 발신 내용이 템플릿 내용으로 치환되지 않습니다. **

단, 템플릿 아이디를 입력했을 경우, 해당 템플릿으로 조회가 가능합니다.

위와 같은 경우는 템플릿 조회를 한 뒤, 템플릿의 내용을 수정하고 싶을 경우 사용할 수 있습니다.

[Request body]

```
{
    "templateId": "TemplateId",
    "body":"본문",
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


### 템플릿 리스트 조회

#### 요청

[URL]

```
GET  /sms/v2.1/appKeys/{appKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Query parameter]

|값|	타입|	필수|	설명|
|---|---|---|---|
|categoryId|	Integer|	옵션|	카테고리 아이디|
|useYn|	String|	옵션|	사용 여부(Y/N)|

#### 응답

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
            "categoryName": "카테고리명",
            "sort": 0,
            "templateName": "템플릿명",
            "templateDesc": "템플릿 설명",
            "useYn": "Y",
            "priority": "S",
            "sendNo": ""15446859String"",
            "sendType": "0",
            "sendTypeName": "SMS 발송",
            "title": "제목",
            "body": "본문",
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

|값|	타입|	설명|
|---|---|---|
|header.isSuccessful|	Boolean|	성공여부|
|header.resultCode|	Integer|	실패 코드|
|header.resultMessage|	String|	실패 메시지|
|body.pageNum|	Integer|	현재 페이지 번호|
|body.pageSize|	Integer|	조회된 데이터 건수|
|body.totalCount|	Integer|	총 데이터 건수|
|body.data[].templateId|	String|	템플릿 아이디|
|body.data[].serviceId|	Integer|	서비스 아이디(내부용, 미사용 값)|
|body.data[].categoryId|	Integer|	카테고리 아이디|
|body.data[].categoryName|	String|	카테고리명|
|body.data[].sort|	Integer|	정렬값|
|body.data[].templateName|	String|	템플릿명|
|body.data[].templateDesc|	String|	템플릿설명|
|body.data[].useYn|	String|	사용여부|
|body.data[].priority|	String|	우선순위값(미사용 값)|
|body.data[].sendNo|	String|	발신번호|
|body.data[].sendType|	String|	발송타입(0:Sms, 1:mms, 2:auth)|
|body.data[].sendTypeName|	String|	발송타입명|
|body.data[].title|	String|	제목|
|body.data[].body|	String|	본문내용|
|body.data[].attachFileYn|	String|	첨부파일여부(Y/N)|
|body.data[].delYn |	String|	삭제여부(Y/N), 현재 상태 표기용으로만 사용|
|body.data[].createDate|	String|	등록날짜|
|body.data[].createUser|	String|	등록유저|
|body.data[].updateDate|	String|	수정날짜|
|body.data[].updateUser|	String|	수정유저|
|body.data[].attachFileList[].fileId|	Integer|	첨부파일 아이디|
|body.data[].attachFileList[].serviceId|	Integer|	서비스 아이디(내부용, 미사용 값)|
|body.data[].attachFileList[].attachType|	Integer|	첨부파일 업로드 타입(0:임시,1:업로드,2:템플릿)|
|body.data[].attachFileList[].templateId|	String|	템플릿 아이디|
|body.data[].attachFileList[].filePath|	String|	첨부파일 경로|
|body.data[].attachFileList[].fileName|	String|	첨부파일명|
|body.data[].attachFileList[].fileSize|  Integer| 파일 사이즈|
|body.data[].attachFileList[].createDate|	String|	첨부파일 등록날짜|
|body.data[].attachFileList[].createUser|	String|	첨부파일 등록유저|


### 템플릿 단일 조회

#### 요청

[URL]

```
GET  /sms/v2.1/appKeys/{appKey}/templates/{templateId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
|templateId|	String|	템플릿 아이디|

#### 응답

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
            "categoryName": "카테고리명",
            "sort": 0,
            "templateName": "템플릿명",
            "templateDesc": "템플릿 설명",
            "useYn": "Y",
            "priority": "S",
            "sendNo": ""15446859String"",
            "sendType": "0",
            "sendTypeName": "SMS 발송",
            "title": "제목",
            "body": "본문",
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

|값|	타입|	설명|
|---|---|---|
|header.isSuccessful|	Boolean|	성공여부|
|header.resultCode|	Integer|	실패 코드|
|header.resultMessage|	String|	실패 메시지|
|body.pageNum|	Integer|	현재 페이지 번호|
|body.pageSize|	Integer|	조회된 데이터 건수|
|body.totalCount|	Integer|	총 데이터 건수|
|body.data.templateId|	String|	템플릿 아이디|
|body.data.serviceId|	Integer|	서비스 아이디(내부용, 미사용 값)|
|body.data.categoryId|	Integer|	카테고리 아이디|
|body.data.categoryName|	String|	카테고리명|
|body.data.sort|	Integer|	정렬값|
|body.data.templateName|	String|	템플릿명|
|body.data.templateDesc|	String|	템플릿설명|
|body.data.useYn|	String|	사용여부|
|body.data.priority|	String|	우선순위값(미사용 값)|
|body.data.sendNo|	String|	발신번호|
|body.data.sendType|	String|	발송타입(0:Sms, 1:mms, 2:auth)|
|body.data.sendTypeName|	String|	발송타입명|
|body.data.title|	String|	제목|
|body.data.body|	String|	본문내용|
|body.data.attachFileYn|	String|	첨부파일여부(Y/N)|
|body.data.delYn |	String|	삭제여부(Y/N), 현재 상태 표기용으로만 사용|
|body.data.createDate|	String|	등록날짜|
|body.data.createUser|	String|	등록유저|
|body.data.updateDate|	String|	수정날짜|
|body.data.updateUser|	String|	수정유저|
|body.data.attachFileList[].fileId|	Integer|	첨부파일 아이디|
|body.data.attachFileList[].serviceId|	Integer|	서비스 아이디(내부용, 미사용 값)|
|body.data.attachFileList[].attachType|	Integer|	첨부파일 업로드 타입(0:임시,1:업로드,2:템플릿)|
|body.data.attachFileList[].templateId|	String|	템플릿 아이디|
|body.data.attachFileList[].filePath|	String|	첨부파일 경로|
|body.data.attachFileList[].fileName|	String|	첨부파일명|
|body.data.attachFileList[].fileSize|  Integer| 파일 사이즈|
|body.data.attachFileList[].createDate|	String|	첨부파일 등록날짜|
|body.data.attachFileList[].createUser|	String|	첨부파일 등록유저|

## 080 수신거부 서비스

### 수신거부 대상자 조회

#### 요청

[URL]

```
GET  /sms/v2.1/appKeys/{appKey}/blockservice/recipients
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Query parameter]

|값|	타입|	필수|	설명|
|---|---|---|---|
|unsubscribeNo|	String|	필수 |	080수신거부번호 |
|startRequestDate|	String|	옵션 |	수신거부 요청 시작 값(yyyy-MM-dd HH:mm:ss)|
|endRequestDate|	String|	옵션 |	수신거부 요청 종료 값(yyyy-MM-dd HH:mm:ss)|
|pageNum|	Integer|	옵션|	페이지 번호(Default : 1)|
|pageSize|	Integer|	옵션|	조회 건수(Default : 15)|

#### 응답
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

### 수신거부 대상자 삭제

#### 요청

[URL]

```
DELETE  /sms/v2.1/appKeys/{appKey}/blockservice/recipients/removes?unsubscribeNo={unsubscribeNo}&updateUser={updateUser}&recipientNoList={recipientNo},{recipientNo}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Query parameter]

|값|	타입|	필수|	설명|
|---|---|---|---|
|unsubscribeNo|	String|	필수 |	080수신거부번호 |
|updateUser|	String|	필수 |	수신거부 삭제자|
|recipientNo|	String|	필수 |	삭제할 수신거부 번호|

#### 응답
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

## 발신 번호

### 발신 번호 등록 요청

#### 요청

[URL]

|Http method|	URI|
|---|---|
|POST|	/sms/v2.1/appKeys/{appKey}/reqeusts/sendNos|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

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

#### 응답
```
{
  "header" : {
    "isSuccessful" :  Boolean,
    "resultCode" :  Integer,
    "resultMessage" :  String
  }
}
```

### 발신 번호 서류 업로드

#### 요청

[URL]

|Http method|	URI|
|---|---|
|POST|	/sms/v2.1/appKeys/{appKey}/requests/attachFiles/authDocuments|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Request Body]
```
multipart/form-data ...
```

#### 응답
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

### 발신번호 인증 요청 내역 조회 API

#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET|	/sms/v2.1/appKeys/{appKey}/requests/sendNos?status={status}|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Query parameter]

|값|	타입|	설명|
|---|---|---|
|status|	String|	서류 인증 상태<br/>- SRS01	발신번호 등록 요청<br/>- SRS02	심사중<br/>- SRS03	등록 완료<br/>- SRS04	등록 불가<br/>- SRS05	핸드폰 인증 대기<br/>- SRS06	핸드폰 인증 실패<br/>- SRS07	수동 등록 완료|

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

|값|	타입|	설명|
|---|---|---|
|header.isSuccessful|	Boolean|	성공여부|
|header.resultCode|	Integer|	실패 코드|
|header.resultMessage|	String|	실패 메시지|
|body.pageNum|	Integer|	현재 페이지 번호|
|body.pageSize|	Integer|	조회된 데이터 건수|
|body.totalCount|	Integer|	총 데이터 건수|
|body.data[].authType|	String|	요청 인증 타입<br/>SMS_AUTH:SMS인증, DOCUMENT_AUTH:서류인증, REGIST_AUTH:수동인증|
|body.data[].sendNos[]|	List<String>|	등록요청 발신번호 리스트|
|body.data[].comment|	String|	커멘트 항목|
|body.data[].fileIds[]|	List<Integer>|	서류 인증 시 업로드한 파일 아이디(내부용)|
|body.data[].status|	String|	요청상태<br/>REGIST_REQUEST(SRS01), EXAMINE(SRS02), COMPLETE(SRS03), REJECT(SRS04), CERTIFYING(SRS05), CERTIFY_FAILED(SRS06), MANUAL_REGIST(SRS07)|
|body.data[].createDate|	String| 등록 일시	|
|body.data[].updateDate|	String| 수정 일시	|
|body.data[].confirmDate|	String| 승인/거절 일시	|



### 등록된 발신번호 리스트 조회 API

#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET|	/sms/v2.1/appKeys/{appKey}/sendNos?sendNo={sendNo}&useYn={useYn}&blockYn={blockYn}

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Query parameter]

|값|	타입|	설명|
|---|---|---|
| sendNo | String | 발신 번호 |
| useYn | String | 사용 여부 |
| blockYn | String | 차단 여부 |

#### 응답
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

|값|	타입|	설명|
|---|---|---|
|header.isSuccessful|	Boolean|	성공여부|
|header.resultCode|	Integer|	실패 코드|
|header.resultMessage|	String|	실패 메시지|
|body.pageNum|	Integer|	페이지 번호|
|body.pageSize|	Integer|	리스트 출력 사이즈|
|body.totalCount|	Integer|	총 데이터 수|
|body.data[].serviceId | Integer | 서비스 아이디 |
|body.data[].sendNo | String | 발신번호 |
|body.data[].useYn | String | 사용여부 |
|body.data[].blockYn | String | 차단여부 |
|body.data[].blockReason | String | 차단사유 |
|body.data[].createDate | String | 생성일시 |
|body.data[].createUser | String | 생성자 |
|body.data[].updateDate | String | 수정일시 |
|body.data[].updateUser | String | 수정자 |

## 통계 조회

### 통합 통계 조회

#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET|	/sms/v2.1/appKeys/{appKey}}/statistics/view?searchType={searchType}&from={from}&to={to}&messageTypes={messageType}&contentTypes={contentType}&templateId={templateId}|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Query parameter]

|값|	타입|	필수 |설명|
|---|---|---|---|
| searchType | String | O | 통계 구분<br/>DATE:날짜별, TIME:시간별, DAY:요일별 |
| from | String | O | 통계 조회 시작 날짜<br/>yyyy-MM-dd HH:mm |
| to | String | O | 통계 조회 종료 날짜<br/>yyyy-MM-dd HH:mm |
| messageType | X | String | 메세지 타입<br/>SMS:단문, LMS:장문, MMS:첨부파일, AUTH:인증용 |
| contentType | X | String | 컨텐츠 타입<br/>NORMAL:일반, AD:광고 |
| templateId | X | String | 템플릿 아이디 |

#### 응답
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
|값| 타입|설명|
|---|---|---|
|header.isSuccessful|	Boolean|	성공여부|
|header.resultCode|	Integer|	실패 코드|
|header.resultMessage|	String|	실패 메시지|
|body.data[].divisionName | String | 표시이름<br/>날짜,시간,요일 |
|body.data[].statisticsView | Object | |
|body.data[].requestedCount | Integer | 요청 카운트 |
|body.data[].succeedCount | Integer | 성공 카운트 |
|body.data[].failedCount | Integer | 실패 카운트 |
|body.data[].pendingCount | Integer | 발송중 카운트 |
|body.data[].succeedRate | String | 성공 비율 |
|body.data[].failedRate | String | 실패 비율 |
|body.data[].pendingRate | String | 발송중 비율 |

### 예약 발송 목록 조회

#### 요청

[URL]

```
GET /sms/v2.1/appKeys/{appKey}/reservations?sendType={sendType}&startRequestDate={startRequestDate}&endRequestDate={endRequestDate}&sendNo={sendNo}&recipientNo={recipientNo}&templateId={templateId}&requestId={requestId}&messageStatus={messageStatus}&pageNum={pageNum}&pageSize={pageSize}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Query parameter]

|값|	타입|	필수|	설명|
|---|---|---|---|
|sendType| String| 발송타입<br/>(0:SMS,1:LMS/MMS,2:AUTH) |
|requestId|	String|	조건 필수 (1번) |	요청 아이디|
|startRequestDate|	String|	조건 필수 (2번) |	발송 날짜 시작 값(yyyy-MM-dd HH:mm:ss)|
|endRequestDate|	String|	조건 필수 (2번) |	발송 날짜 종료 값(yyyy-MM-dd HH:mm:ss)|
|sendNo|	String|	옵션|	발신번호|
|recipientNo|	String|	옵션|	수신번호|
|templateId|	String|	옵션|	템플릿번호|
|messageStatus|	String|	옵션|	메세지상태<br/>(RESERVED:예약대기,SENDING:발송중,COMPLETED:발송완료,FAILED:발송실패,CANCEL:예약취소)|
|pageNum|	Integer|	옵션|	페이지 번호(Default : 1)|
|pageSize|	Integer|	옵션|	조회 건수(Default : 15)|

#### 응답

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
        "requestId":"{요청아이디}",
        "recipientSeq":1,
        "requestDate":"{예약 날짜}",
        "sendNo":"{발신번호}",
        "recipientNo":"{수신번호}",
        "countryCode":"{국가코드}",
        "sendType":"{발송타입}",
        "messageType":"{메세지타입}",
        "adYn":"{광고여부}",
        "templateId":"{템플릿 아이디}",
        "templateParameter":"{템플릿 파라미터}",
        "templateName":"{템플릿 명}",
        "title":"{제목}",
        "body":"{내용}",
        "messageStatus":"{메세지 상태}",
        "createUser":"{등록자}",
        "createDate":"{등록 일시}",
        "updateDate":"{수정 일시}"
      }
    ]
  }
}
```

|값|	타입|	설명|
|---|---|---|
|header.isSuccessful|	Boolean|	성공여부|
|header.resultCode|	Integer|	실패 코드|
|header.resultMessage|	String|	실패 메시지|
|body.pageNum|	Integer|	현재 페이지 번호|
|body.pageSize|	Integer|	조회된 데이터 건수|
|body.totalCount|	Integer|	총 데이터 건수|
|body.data[].requestId|	String|	요청 아이디|
|body.data[].recipientSeq|	Integer|	수신자 시퀀스|
|body.data[].requestDate|	String|	발신일시|
|body.data[].sendNo|	String|	발신번호|
|body.data[].recipientNo|	String|	수신번호|
|body.data[].countryCode|	String|	국가번호|
|body.data[].sendType|	String|	발송 타입(0:SMS, 1:MMS, 2:Auth)|
|body.data[].messageType|	String|	메세지타입<br/>(SMS,LMS,MMS,AUTH)|
|body.data[].adYn|	String|	광고 여부|
|body.data[].templateId|	String|	템플릿아이디|
|body.data[].templateParameter|	String(json)|	템플릿 파라미터|
|body.data[].templateName|	String|	템플릿명|
|body.data[].title|	String|	제목|
|body.data[].body|	String|	본문내용|
|body.data[].messageStatus|	String|	메세지상태<br/>(RESERVED:예약대기,SENDING:발송중,COMPLETED:발송완료,FAILED:발송실패,CANCEL:예약취소)|
|body.data[].createUser|	String|	등록자|
|body.data[].createDate|	String|	등록일시|
|body.data[].updateDate|	String|	수정일시|

### 예약 발송 상세 조회

#### 요청

[URL]

```
GET /sms/v2.1/appKeys/{appKey}/reservations/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
|requestId|	String|	요청 아이디|
|recipientSeq|	Integer|	수신자 시퀀스|

#### 응답

```
{
  "header":{
    "resultCode":0,
    "resultMessage":"success",
    "isSuccessful":true
  },
  "body":{
    "data":{
      "requestId":"{요청아이디}",
      "recipientSeq":1,
      "requestDate":"{예약 날짜}",
      "sendNo":"{발신번호}",
      "recipientNo":"{수신번호}",
      "countryCode":"{국가코드}",
      "sendType":"{발송타입}",
      "messageType":"{메세지타입}",
      "adYn":"{광고여부}",
      "templateId":"{템플릿 아이디}",
      "templateParameter":"{템플릿 파라미터}",
      "templateName":"{템플릿 명}",
      "title":"{제목}",
      "body":"{내용}",
      "messageStatus":"{메세지 상태}",
      "createUser":"{등록자}",
      "createDate":"{등록 일시}",
      "updateDate":"{수정 일시}",
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

|값|	타입|	설명|
|---|---|---|
|header.isSuccessful|	Boolean|	성공여부|
|header.resultCode|	Integer|	실패 코드|
|header.resultMessage|	String|	실패 메시지|
|body.pageNum|	Integer|	현재 페이지 번호|
|body.pageSize|	Integer|	조회된 데이터 건수|
|body.totalCount|	Integer|	총 데이터 건수|
|body.data.requestId|	String|	요청 아이디|
|body.data.recipientSeq|	Integer|	수신자 시퀀스|
|body.data.requestDate|	String|	발신일시|
|body.data.sendNo|	String|	발신번호|
|body.data.recipientNo|	String|	수신번호|
|body.data.countryCode|	String|	국가번호|
|body.data.sendType|	String|	발송 타입(0:SMS, 1:MMS, 2:Auth)|
|body.data.messageType|	String|	메세지타입<br/>(SMS,LMS,MMS,AUTH)|
|body.data.adYn|	String|	광고 여부|
|body.data.templateId|	String|	템플릿아이디|
|body.data.templateParameter|	String(json)|	템플릿 파라미터|
|body.data.templateName|	String|	템플릿명|
|body.data.title|	String|	제목|
|body.data.body|	String|	본문내용|
|body.data.messageStatus|	String|	메세지상태<br/>(RESERVED:예약대기,SENDING:발송중,COMPLETED:발송완료,FAILED:발송실패,CANCEL:예약취소)|
|body.data.createUser|	String|	등록자|
|body.data.createDate|	String|	등록일시|
|body.data.attachFileList[].fileId|	Integer|	파일아이디|
|body.data.attachFileList[].filePath|	String|	파일경로(내부용)|
|body.data.attachFileList[].fileName|	String|	파일명|


### 예약 발송 취소

#### 요청

[URL]

```
PUT /sms/v2.0/appKeys/{appKey}/reservations/cancel
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

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

|값|	타입|	필수|	설명|
|---|---|---|---|
|reservationList[].requestId| String| O | 요청 아이디|
|reservationList[].recipientSeq| Integer| O | 수신자 시퀀스|
|updateUser| String| O | 취소 요청자|

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

|값|	타입|	설명|
|---|---|---|
|header.isSuccessful|	Boolean|	성공여부|
|header.resultCode|	Integer|	실패 코드|
|header.resultMessage|	String|	실패 메시지|
|body.data.requestedCount|	Integer|	취소 요청 건수|
|body.data.canceledCount|	Integer|	취소 성공 건수|



## v2.0 API

### 단문 SMS 발송

#### 요청

[URL]

```
POST  /sms/v2.0/appKeys/{appKey}/sender/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

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

|값|	타입|	필수|	설명|
|---|---|---|---|
|templateId|	String|	X|	발송 템플릿 아이디|
|body|	String|	O|	본문 내용('EUC-KR' 기준으로 90 Byte 제한)|
|sendNo|	String|	O|	발신번호|
|requestDate| String| X | 예약일시(yyyy-MM-dd HH:mm)|
|recipientList|	List|	O|	수신자 리스트(최대 1000명)|
|- recipientNo|	String|	O|	수신번호<br/>countryCode와 조합하여 사용 가능|
|- countryCode|	String|	X|	국가번호 [기본값: 82(한국)] <br>(국제 발송 시, euc-kr(한글,영문) 내용만 가능합니다.) |
|- internationalRecipientNo| String| X| 국가번호가 포함된 수신번호<br/>예)821012345678<br/>recipientNo가 있을 경우 이 값은 무시된다.<br/>|
|- templateParameter|	Object|	X|	템플릿 파라미터(템플릿 아이디 입력 시)|
|-- #key#|	String|	X|	치환 키(##key##)|
|-- #value#|	Object|	X|	치환 키에 매핑되는 Value값|
|userId|	String|	X | 발송 구분자 ex)admin,system |

#### 응답

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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	Object|	데이터 영역|
|-- requestId|	String|	요청 아이디|
|-- statusCode|	String|	요청 상태 코드(1:요청중, 2:요청완료, 3:요청실패)|
|-- sendResultList|	List:Object|	발송 결과 리스트|
|--- recipientNo| String | 수신번호|
|--- resultCode| Integer | 결과코드|
|--- resultMessage| String | 결과메시지|

#### 단문 SMS 발송 예제(일반 국내 수신 번호)

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

[Request body] 필수 값은 { }로 표시.
```
{
    "body": "{본문 내용}",
    "sendNo": "{발신번호}",
    "recipientList": [{
        "recipientNo": "{수신번호}"
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
              "recipientNo" : {수신번호},
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
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-sms.cloud.toast.com/sms/v2.0/appKeys/{appKey}/sender/sms -d '{"body": "{본문 내용}","sendNo": "{발신번호}","recipientList":[{"recipientNo": "{수신번호}"}],"userId": "{userId}"}'
```

#### 단문 SMS 발송 예제(국가 코드가 포함된 수신 번호)

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


[Request body] 필수 값은 { }로 표시.
```
{
    "body": "{본문 내용}",
    "sendNo": "{발신번호}",
    "recipientList": [
    {
        "internationalRecipientNo": "{국가번호가 포함된 수신번호}"
    },
    {
        "recipientNo":"{수신번호}",
        "countryCode":"{국가번호}"
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
              "recipientNo" : {수신번호},
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
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-sms.cloud.toast.com/sms/v2.0/appKeys/{appKey}/sender/sms -d '{"body": "{본문 내용}","sendNo": "{발신번호}","recipientList": [{"internationalRecipientNo": "{국가번호가 포함된 수신번호}"},{"recipientNo":"{수신번호}","countryCode":"{국가번호}"}],"userId": ""}'
```

### 단문 SMS 발송리스트 조회

#### 요청

[URL]

```
GET  /sms/v2.0/appKeys/{appKey}/sender/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Query parameter] 1번 or 2번 조건 필수

|값|	타입|	필수|	설명|
|---|---|---|---|
|requestId|	String|	조건 필수 (1번) |	요청 아이디|
|startRequestDate|	String|	조건 필수 (2번) |	발송 날짜 시작 값(yyyy-MM-dd HH:mm:ss)|
|endRequestDate|	String|	조건 필수 (2번) |	발송 날짜 종료 값(yyyy-MM-dd HH:mm:ss)|
|startResultDate|	String|	옵션|	수신 날짜 시작 값(yyyy-MM-dd HH:mm:ss)|
|endResultDate|	String|	옵션|	수신 날짜 종료 값(yyyy-MM-dd HH:mm:ss)|
|sendNo|	String|	옵션|	발신번호|
|recipientNo|	String|	옵션|	수신번호|
|templateId|	String|	옵션|	템플릿번호|
|msgStatus|	String|	옵션|	메시지 상태 코드(1:요청, 2:처리중, 3:성공)|
|resultCode|	String|	옵션|	수신 결과 코드 [[조회 코드표](./error-code/#_2)]|
|subResultCode|	String|	옵션|	수신 결과 상세 코드 [[조회 코드표](./error-code/#_3)]|
|pageNum|	Integer|	옵션|	페이지 번호(Default : 1)|
|pageSize|	Integer|	옵션|	조회 건수(Default : 15)|

#### 응답

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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- pageNum|	Integer|	현재 페이지 번호|
|- pageSize|	Integer|	조회된 데이터 건수|
|- totalCount|	Integer|	총 데이터 건수|
|- data|	List|	데이터 영역|
|-- requestId|	String|	요청 아이디|
|-- requestDate|	String|	발신일시|
|-- resultDate|	String|	수신일시|
|-- templateId|	String|	템플릿아이디|
|-- templateName|	String|	템플릿명|
|-- categoryId|	String|	카테고리아이디|
|-- categoryName|	String|	카테고리명|
|-- body|	String|	본문내용|
|-- sendNo|	String|	발신번호|
|-- countryCode|	String|	국가번호|
|-- recipientNo|	String|	수신번호|
|-- msgStatus|	String|	메시지 상태 코드|
|-- msgStatusName|	String|	메시지 상태 코드명|
|-- resultCode|	String|	수신 결과 코드 [[수신 결과 코드표](./error-code/#emma-v3)]|
|-- resultCodeName|	String|	수신 결과 코드명|
|-- telecomCode|	Integer|	통신사코드|
|-- telecomCodeName|	String|	통신사명|
|-- excelSeq|	Integer|	엑셀 시퀀스 번호(엑셀로 발송한 경우 데이터 존재)|
|-- mtPr|	Integer|	발송상세아이디(상세 조회 시 필수)|
|-- sendType|	String|	발송 타입(0:SMS, 1:MMS, 2:Auth)|
|-- userId|	String|	발송 요청 아이디|

### 단문 SMS 발송 단일 조회

#### 요청

[URL]

```
GET  /sms/v2.0/appKeys/{appKey}/sender/sms/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
|requestId|	String|	요청아이디|

[Query parameter]

|값|	타입|	필수|	설명|
|---|---|---|---|
|mtPr|	Integer|	필수|	발송상세아이디|

#### 응답

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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	List|	데이터 영역|
|-- requestId|	String|	요청 아이디|
|-- requestDate|	String|	발신일시|
|-- resultDate|	String|	수신일시|
|-- templateId|	String|	템플릿아이디|
|-- templateName|	String|	템플릿명|
|-- categoryId|	String|	카테고리아이디|
|-- categoryName|	String|	카테고리명|
|-- body|	String|	본문내용|
|-- sendNo|	String|	발신번호|
|-- countryCode|	String|	국가번호|
|-- recipientNo|	String|	수신번호|
|-- msgStatus|	String|	메시지 상태 코드|
|-- msgStatusName|	String|	메시지 상태 코드명|
|-- resultCode|	String|	수신 결과 코드 [[수신 결과 코드표](./error-code/#emma-v3)]|
|-- resultCodeName|	String|	수신 결과 코드명|
|-- telecomCode|	Integer|	통신사코드|
|-- telecomCodeName|	String|	통신사명|
|-- excelSeq|	Integer|	엑셀 시퀀스 번호(엑셀로 발송한 경우 데이터 존재)|
|-- mtPr|	Integer|	발송상세아이디(상세 조회 시 필수)|
|-- sendType|	String|	발송 타입(0:SMS, 1:MMS, 2:Auth)|
|-- userId|	String|	발송 요청 아이디|

## 장문 MMS

### 장문 MMS 발송(첨부파일 미포함)

#### 요청

[URL]

```
POST  /sms/v2.0/appKeys/{appKey}/sender/mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

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

|값|	타입|	필수|	설명|
|---|---|---|---|
|templateId|	String|	옵션|	발송 템플릿 아이디|
|title|	String|	필수|	제목 <br/> ('EUC-KR' 기준으로 40Byte 제한) <br/> (영문: 1byte, 한글: 2byte)|
|body|	String|	필수|	본문 <br/> ('EUC-KR' 기준으로 2000Byte 제한) <br/> (영문: 1byte, 한글: 2byte)|
|sendNo|	String|	필수|	발신번호|
|requestDate| String| X | 예약일시(yyyy-MM-dd HH:mm)|
|attachFileIdList|	List:Integer|	옵션|	첨부파일 아이디 리스트|
|recipientList|	List|	필수|	수신자 리스트(최대 1000명)|
|- recipientNo|	String|	필수|	수신번호<br/>countryCode와 조합하여 사용 가능|
|- countryCode|	String|	X|	국가번호 [기본값: 82(한국)] <br>(국제 발송 시, euc-kr(한글,영문) 내용만 가능합니다.) |
|- internationalRecipientNo| String| X| 국가번호가 포함된 수신번호<br/>예)821012345678<br/>recipientNo가 있을 경우 이 값은 무시된다.<br/>|
|- templateParameter|	Object|	옵션|	템플릿 파라미터(템플릿 아이디 입력 시)|
|-- #key#|	String|	옵션|	치환 키(##key##)|
|-- #value#|	Object|	옵션|	치환 키에 매핑되는 Value값|
|userId|	String|	옵션|	발송 구분자 ex)admin,system|

#### 응답

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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	Object|	데이터 영역|
|-- requestId|	String|	요청 아이디|
|-- statusCode|	String|	요청 상태 코드(1:요청중, 2:요청완료, 3:요청실패)|
|-- sendResultList|	List:Object|	발송 결과 리스트|
|--- recipientNo| String | 수신번호|
|--- resultCode| Integer | 결과코드|
|--- resultMessage| String | 결과메시지|

#### 장문 MMS 발송 예제

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


[Request body] 필수 값은 { }로 표시.
```
{
    "title": "{제목}",
    "body": "{본문 내용}",
    "sendNo": "{발신번호}",
    "recipientList": [{
        "recipientNo": "{수신번호}",
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
              "recipientNo" : {수신번호},
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
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-sms.cloud.toast.com/sms/v2.0/appKeys/{appKey}/sender/mms -d '{"title": "{제목}","body": "{본문 내용}","sendNo": "{발신번호}","recipientList": [{"recipientNo": "{수신번호}","templateParameter": { }}],"userId": ""}'
```

### 장문 MMS 발송(첨부파일 포함)

#### 첨부파일 업로드

#### 요청

[URL]

```
POST  /sms/v2.0/appKeys/{appKey}/attachfile/binaryUpload
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Request body]

```
{
    "fileName": String,
    "createUser": String,
    "fileBody": Byte[]
}
```

|값|	타입|	필수|	설명|
|---|----|----|---|
|fileName|	String|	필수|	45글자 이내<br/>파일이름(확장자 jpg,jpeg(소문자)만 가능)|
|fileBody|	Byte[]|	필수|	파일의 Byte[] 값<br/>300K 이하|
|createUser|	String|	필수|	파일 업로드 유저 정보|

#### 응답

```
{
    "header": {
        "isSuccessful": Boolean,
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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	Object|	데이터 영역|
|-- fileId|	Integer|	파일아이디|
|-- fileName|	String|	파일명|
|-- filePath|	String|	첨부파일 기본Path <br/> (https://domain/attachFile/filePath/fileName)|

#### 첨부파일 업로드 예제

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
  "fileName": "{파일 이름}",
  "createUser": "{요청 아이디}",
  "fileBody": "{파일 byte 값}"
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

#### 첨부파일 발송 예제

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


[Request body] 필수 값은 { }로 표시.
```
{
    "title": "{제목}",
    "body": "{본문 내용}",
    "sendNo": "{발신번호}",
    "attachFileIdList": [
        "{파일 업로드 후 response의 fileId}"
    ],
    "recipientList": [{
        "recipientNo": "{수신번호}",
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
      "requestId": "1-201607-424631-1",
      "statusCode": "2",
      "sendResultList" : [
          {
              "recipientNo" : {수신번호},
              "resultCode" :  0,
              "resultMessage" : "SUCCESS"
          }
      ]
    }
  }
}
```

### 장문 MMS 발송리스트 조회

#### 요청

[URL]

```
GET  /sms/v2.0/appKeys/{appKey}/sender/mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Query parameter] 1번 or 2번 조건 필수

|값|	타입|	필수|	설명|
|---|---|---|---|
|requestId|	String|	조건 필수 (1번) |	요청 아이디|
|startRequestDate|	String|	조건 필수 (2번) |	발송 날짜 시작 값(yyyy-MM-dd HH:mm:ss)|
|endRequestDate|	String|	조건 필수 (2번) |	발송 날짜 종료 값(yyyy-MM-dd HH:mm:ss)|
|startResultDate|	String|	옵션|	수신 날짜 시작 값(yyyy-MM-dd HH:mm:ss)|
|endResultDate|	String|	옵션|	수신 날짜 종료 값(yyyy-MM-dd HH:mm:ss)|
|sendNo|	String|	옵션|	발신번호|
|recipientNo|	String|	옵션|	수신번호|
|templateId|	String|	옵션|	템플릿번호|
|msgStatus|	String|	옵션|	메시지 상태 코드(1:요청, 2:처리중, 3:성공)|
|resultCode|	String|	옵션|	수신 결과 코드 [[조회 코드표](./error-code/#_2)]|
|subResultCode|	String|	옵션|	수신 결과 상세 코드 [[조회 코드표](./error-code/#_3)]|
|pageNum|	Integer|	옵션|	페이지 번호(Default : 1)|
|pageSize|	Integer|	옵션|	조회 건수(Default : 15)|

#### 응답

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
                filePath: String,
                fileName: String
            }]
        }]
    }
}
```

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- pageNum|	Integer|	현재 페이지 번호|
|- pageSize|	Integer|	조회된 데이터 건수|
|- totalCount|	Integer|	총 데이터 건수|
|- data|	List|	데이터 영역|
|-- requestId|	String|	요청 아이디|
|-- requestDate|	String|	발신일시|
|-- resultDate|	String|	수신일시|
|-- templateId|	String|	템플릿아이디|
|-- templateName|	String|	템플릿명|
|-- categoryId|	String|	카테고리아이디|
|-- categoryName|	String|	카테고리명|
|-- title|	String|	제목|
|-- body|	String|	본문내용|
|-- sendNo|	String|	발신번호|
|-- countryCode|	String|	국가번호|
|-- recipientNo|	String|	수신번호|
|-- msgStatus|	String|	메시지 상태 코드|
|-- msgStatusName|	String|	메시지 상태 코드명|
|-- resultCode|	String|	수신 결과 코드 [[수신 결과 코드표](./error-code/#emma-v3)]|
|-- resultCodeName|	String|	수신 결과 코드명|
|-- telecomCode|	Integer|	통신사코드|
|-- telecomCodeName|	String|	통신사명|
|-- excelSeq|	Integer|	엑셀 시퀀스 번호(엑셀로 발송한 경우 데이터 존재)|
|-- mtPr|	Integer|	발송상세아이디(상세 조회 시 필수)|
|-- sendType|	String|	발송 타입(0:SMS, 1:MMS, 2:Auth)|
|-- userId|	String|	발송 요청 아이디|
|-- serviceType| Integer| 발송 모듈 타입(0:SMS, 2:MMS, 3:LMS) |
|-- attachFileList|	List|	첨부파일 리스트|
|--- filePath|	String|	첨부파일 기본Path <br/> (https://domain/attachFile/filePath/fileName)|
|--- filename|	String|	첨부파일명|

### 장문 MMS 발송 단일 조회

#### 요청

[URL]

```
GET  /sms/v2.0/appKeys/{appKey}/sender/mms/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
|requestId|	String|	요청아이디|

[Query parameter]

|값|	타입|	필수|	설명|
|---|---|---|---|
|mtPr|	Integer|	필수|	발송상세아이디|

#### 응답

```
{
    "header": {
        "isSuccessful": Boolean,
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
                filePath: String,
                fileName: String
            }]
        }]
    }
}
```

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	List|	데이터 영역|
|-- requestId|	String|	요청 아이디|
|-- requestDate|	String|	발신일시|
|-- resultDate|	String|	수신일시|
|-- templateId|	String|	템플릿아이디|
|-- templateName|	String|	템플릿명|
|-- categoryId|	String|	카테고리아이디|
|-- categoryName|	String|	카테고리명|
|-- title|	String|	제목|
|-- body|	String|	본문내용|
|-- sendNo|	String|	발신번호|
|-- countryCode|	String|	국가번호|
|-- recipientNo|	String|	수신번호|
|-- msgStatus|	String|	메시지 상태 코드|
|-- msgStatusName|	String|	메시지 상태 코드명|
|-- resultCode|	String|	수신 결과 코드 [[수신 결과 코드표](./error-code/#emma-v3)]|
|-- resultCodeName|	String|	수신 결과 코드명|
|-- telecomCode|	Integer|	통신사코드|
|-- telecomCodeName|	String|	통신사명|
|-- excelSeq|	Integer|	엑셀 시퀀스 번호(엑셀로 발송한 경우 데이터 존재)|
|-- mtPr|	Integer|	발송상세아이디(상세 조회 시 필수)|
|-- sendType|	String|	발송 타입(0:SMS, 1:MMS, 2:Auth)|
|-- userId|	String|	발송 요청 아이디|
|-- serviceType| Integer| 발송 모듈 타입(0:SMS, 2:MMS, 3:LMS) |
|-- attachFileList|	List|	첨부파일 리스트|
|--- filePath|	String|	첨부파일 기본Path <br/> (https://domain/attachFile/filePath/fileName)|
|--- filename|	String|	첨부파일명|

## 인증용 SMS(긴급)

### 인증용 SMS 발송

#### 요청

[URL]

```
POST  /sms/v2.0/appKeys/{appKey}/sender/auth/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

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

|값|	타입|	필수|	설명|
|---|---|---|---|
|templateId|	String|	X|	발송 템플릿 아이디|
|body|	String|	O|	본문 내용('EUC-KR' 기준 90 Byte 제한)|
|sendNo|	String|	O|	발신번호|
|requestDate| String| X | 예약일시(yyyy-MM-dd HH:mm)|
|recipientList|	List|	O|	수신자 리스트(최대 1000명)|
|- recipientNo|	String|	O|	수신번호<br/>countryCode와 조합하여 사용 가능|
|- countryCode|	String|	X|	국가번호 [기본값: 82(한국)] <br>(국제 발송 시, euc-kr(한글,영문) 내용만 가능합니다.) |
|- internationalRecipientNo| String| X| 국가번호가 포함된 수신번호<br/>예)821012345678<br/>recipientNo가 있을 경우 이 값은 무시된다.|
|- templateParameter|	Object|	X|	템플릿 파라미터(템플릿 아이디 입력 시)|
|-- #key#|	String|	X|	치환 키(##key##)|
|-- #value#|	Object|	X|	치환 키에 매핑되는 Value값|
|userId|	String|	X |	발송 구분자 ex)admin,system|


#### 응답
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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	Object|	데이터 영역|
|-- requestId|	String|	요청 아이디|
|-- statusCode|	String|	요청 상태 코드(1:요청중, 2:요청완료, 3:요청실패)|
|-- sendResultList|	List:Object|	발송 결과 리스트|
|--- recipientNo| String | 수신번호|
|--- resultCode| Integer | 결과코드|
|--- resultMessage| String | 결과메시지|

#### 예제

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


[Request body] 필수 값은 { }로 표시.
```
{
    "body": "{본문 내용}",
    "sendNo": "{발신번호}",
    "recipientList": [{
        "recipientNo": "{수신번호}",
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
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-sms.cloud.toast.com/sms/v2.0/appKeys/{appKey}/sender/auth/sms -d '{"body": "{본문 내용}","sendNo": "{발신번호}","recipientList":[{"recipientNo": "{수신번호}","templateParameter": { }}],"userId": ""}'
```

### 인증용 SMS 발송리스트 조회

#### 요청

[URL]

```
GET  /sms/v2.0/appKeys/{appKey}/sender/auth/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|----|---|
|appKey|	String|	고유의 appKey|

[Query parameter] 1번 or 2번 조건 필수

|값|	타입|	필수|	설명|
|---|---|---|---|
|requestId|	String|	조건 필수 (1번) |	요청 아이디|
|startRequestDate|	String|	조건 필수 (2번) |	발송 날짜 시작 값(yyyy-MM-dd HH:mm:ss)|
|endRequestDate|	String|	조건 필수 (2번) |	발송 날짜 종료 값(yyyy-MM-dd HH:mm:ss)|
|startResultDate|	String|	옵션|	수신 날짜 시작 값(yyyy-MM-dd HH:mm:ss)|
|endResultDate|	String|	옵션|	수신 날짜 종료 값(yyyy-MM-dd HH:mm:ss)|
|sendNo|	String|	옵션|	발신번호|
|recipientNo|	String|	옵션|	수신번호|
|templateId|	String|	옵션|	템플릿번호|
|msgStatus|	String|	옵션|	메시지 상태 코드(1:요청, 2:처리중, 3:성공)|
|resultCode|	String|	옵션|	수신 결과 코드 [[조회 코드표](./error-code/#_2)]|
|subResultCode|	String|	옵션|	수신 결과 상세 코드 [[조회 코드표](./error-code/#_3)]|
|pageNum|	Integer|	옵션|	페이지 번호(Default : 1)|
|pageSize|	Integer|	옵션|	조회 건수(Default : 15)|

#### 응답

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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- pageNum|	Integer|	현재 페이지 번호|
|- pageSize|	Integer|	조회된 데이터 건수|
|- totalCount|	Integer|	총 데이터 건수|
|- data|	List|	데이터 영역|
|-- requestId|	String|	요청 아이디|
|-- requestDate|	String|	발신일시|
|-- resultDate|	String|	수신일시|
|-- templateId|	String|	템플릿아이디|
|-- templateName|	String|	템플릿명|
|-- categoryId|	String|	카테고리아이디|
|-- categoryName|	String|	카테고리명|
|-- body|	String|	본문내용|
|-- sendNo|	String|	발신번호|
|-- countryCode|	String|	국가번호|
|-- recipientNo|	String|	수신번호|
|-- msgStatus|	String|	메시지 상태 코드|
|-- msgStatusName|	String|	메시지 상태 코드명|
|-- resultCode|	String|	수신 결과 코드 [[수신 결과 코드표](./error-code/#emma-v3)]|
|-- resultCodeName|	String|	수신 결과 코드명|
|-- telecomCode|	Integer|	통신사코드|
|-- telecomCodeName|	String|	통신사명|
|-- excelSeq|	Integer|	엑셀 시퀀스 번호(엑셀로 발송한 경우 데이터 존재)|
|-- mtPr|	Integer|	발송상세아이디(상세 조회 시 필수)|
|-- sendType|	String|	발송 타입(0:SMS, 1:MMS, 2:Auth)|
|-- userId|	String|	발송 요청 아이디|

### 인증용 SMS 발송 단일 조회

#### 요청

[URL]

```
GET  /sms/v2.0/appKeys/{appKey}/sender/auth/sms/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
|requestId|	String|	요청아이디|

[Query parameter]

|값|	타입|	필수|	설명|
|---|----|---|---|
|mtPr|	Integer|	필수|	발송상세아이디|

#### 응답

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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	List|	데이터 영역|
|-- requestId|	String|	요청 아이디|
|-- requestDate|	String|	발신일시|
|-- resultDate|	String|	수신일시|
|-- templateId|	String|	템플릿아이디|
|-- templateName|	String|	템플릿명|
|-- categoryId|	String|	카테고리아이디|
|-- categoryName|	String|	카테고리명|
|-- body|	String|	본문내용|
|-- sendNo|	String|	발신번호|
|-- countryCode|	String|	국가번호|
|-- recipientNo|	String|	수신번호|
|-- msgStatus|	String|	메시지 상태 코드|
|-- msgStatusName|	String|	메시지 상태 코드명|
|-- resultCode|	String|	수신 결과 코드 [[수신 결과 코드표](./error-code/#emma-v3)]|
|-- resultCodeName|	String|	수신 결과 코드명|
|-- telecomCode|	Integer|	통신사코드|
|-- telecomCodeName|	String|	통신사명|
|-- excelSeq|	Integer|	엑셀 시퀀스 번호(엑셀로 발송한 경우 데이터 존재)|
|-- mtPr|	Integer|	발송상세아이디(상세 조회 시 필수)|
|-- sendType|	String|	발송 타입(0:SMS, 1:MMS, 2:Auth)|
|-- userId|	String|	발송 요청 아이디|

## 광고 문자
### 광고성 SMS 발송
[URL]

```
POST  /sms/v2.0/appKeys/{appKey}/sender/ad-sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Request Body]
위에 SMS 발송과 동일.
[[Request Body 참고](./api-guide/#sms_2)]

<span style="color:red">단, 본문에 아래 문구가 필수로 들어가야 합니다.</span>
080번호는 콘솔의 080수신거부 설정 탭에서 확인 가능합니다.
```
(광고)

[무료 수신거부]080XXXXXXX
```


### 광고성 MMS 발송
[URL]

```
POST  /sms/v2.0/appKeys/{appKey}/sender/ad-mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Request Body]
위에 MMS 발송과 동일.
[[Request Body 참고](./api-guide/#mms_1)]

<span style="color:red">단, 본문에 아래 문구가 필수로 들어가야 합니다.</span>
080번호는 콘솔의 080수신거부 설정 탭에서 확인 가능합니다.
```
(광고)

[무료 수신거부]080XXXXXXX
```




## 태그 발송

### 태그 SMS 발송

#### 요청

[URL]

```
POST /sms/v2.0/appKeys/{appKey}/tag-sender/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Request body]

```
{
    "body":"SMS내용",
    "sendNo":"01012345678",
    "requestDate":"2018-03-22 10:00",
    "templateId":"TEMPLATE",
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

|값|	타입|	필수|	설명|
|---|---|---|---|
| body | String | O | 문자 내용 |
| sendNo | String | O | 발신번호 |
|requestDate| String| X | 예약일시(yyyy-MM-dd HH:mm)|
| templateId | String | X | 템플릿 아이디 |
| tagExpression | List<String> | O | 태그 표현식<br/>ex) ["tagA","AND","tabB"] |
| userId | String | X | 요청한 유저의 아이디 |
| adYn | String | X | 광고 여부(기본N) |
| autoSendYn | String | X | 자동 발송(즉시 발송) 여부 (기본 Y) |


#### 응답
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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	Object|	데이터 영역|
|-- requestId|	String|	요청 아이디|

### 태그 LMS 발송

#### 요청

[URL]

```
POST  /sms/v2.0/appKeys/{appKey}/tag-sender/mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Request body]
```
{
    "body":"SMS내용",
    "sendNo":"01012345678",
    "requestDate":"2018-03-22 10:00",
    "templateId":"TEMPLATE",
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

|값|	타입|	필수|	설명|
|---|---|---|---|
| title | String | O | 문자 제목 |
| body | String | O | 문자 내용 |
| sendNo | String | O | 발신번호 |
|requestDate| String| X | 예약일시(yyyy-MM-dd HH:mm)|
| templateId | String | X | 템플릿 아이디 |
| tagExpression | List<String> | O | 태그 표현식<br/>ex) ["tagA","AND","tabB"] |
| attachFileIdList | List<Integer> | X | 첨부파일 아이디(fileId) |
| userId | String | X | 요청한 유저의 아이디 |
| adYn | String | X | 광고 여부(기본N) |
| autoSendYn | String | X | 자동 발송(즉시 발송) 여부 (기본 Y) |


#### 응답
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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	Object|	데이터 영역|
|-- requestId|	String|	요청 아이디|


### 태그 발송 리스트 조회

#### 요청

[URL]

```
GET /sms/v2.0/appKeys/{appKey}/tag-sender?sendType={sendType}&requestId={requestId}&startRequestDate={startRequestDate}&endRequestDate={endRequestDate}&statusCode={statusCode}&pageNum={pageNum}&pageSize={pageSize}
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Request body]

```
X
```

* requestId 또는 startRequestDate + endRequestDate는 필수입니다.

|값|	타입|	필수|	설명|
|---|---|---|---|
| appKey | String| O | 앱키 |
| sendType | required, String | O | 발송 타입<br>SMS : "0",<br>MMS : "1" |
| requestId | String | O | 요청 아이디 |
| startRequestDate | String | O | 발송 날짜 시작 |
| endRequestDate | String | O | 발송 날짜 종료 |
| statusCode | String | X | 발송상태코드<br>WAIT : "MAS00"<br>READY : "MAS01"<br>SENDREADY : "MAS09"<br>SENDWAIT : "MAS10"<br>SENDING : "MAS11"<br>COMPLETE : "MAS19"<br>CANCEL : "MAS91"<br>FAIL : "MAS99" |
| pageNum | optional, Integer | X | 페이지 번호 |
| pageSize | optional, Integer | X | 조회건수 |

#### 응답
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
                "templateName": "치환테스트(SMS)",
                "masterStatusCode": "READY",
                "sendNo": "15883796",
                "requestDate": "2017-12-20 14:15:58",
                "tagExpression": [
                    "Kb6BjCY1"
                ],
                "title": "",
                "body": "SMS 발송 입니다##content##",
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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	Object|	데이터 영역|
|-- requestId | String | 요청 아이디 |
|-- requestIp | String | 요청 아이피 |
|-- requestDate | String | 요청 시간 |
|-- tagSendStatus | String | 태그 발송 상태 |
|-- tagExpression | List | 태그 표현식 |
|-- templateId | String | 템플릿 아이디 |
|-- templateName | String | 템플릿명 |
|-- senderName | String | 발신자명 |
|-- senderMail | String | 발신자주소 |
|-- title | String | 제목 |
|-- body | String | 내용 |
|-- attachYn | String | 첨부파일여부 |
|-- adYn | String | 광고여부 |
|-- createUser | String | 생성자 |
|-- createDate | String | 생성일시 |
|-- updateUser | String | 수정자 |
|-- updateDate | String | 수정일시 |


### 태그 발송 수신자 리스트 조회

#### 요청

[URL]

```
GET /sms/v2.0/appKeys/{appKey}/tag-sender/{requestId}?recipientNum={recipientNum}&startRequestDate={startRequestDate}&endRequestDate={endRequestDate}&startResultDate={startResultDate}&endResultDate={endResultDate}&msgStatusName={msgStatusName}&resultCode={resultCode}&pageNum={pageNum}&pageSize={pageSize}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
| requestId | String | 요청 아이디 |
[Request body]

```
X
```

* requestId 또는 startRequestDate + endRequestDate는 필수입니다.

|값|	타입|	필수|	설명|
|---|---|---|---|
| recipientNum | String | X | 수신자 번호 |
| startRequestDate | String | O | 발송 요청 시작 날짜 |
| endRequestDate | String | O | 발송 요청 종료 날짜 |
| startResultDate | String | X | 수신 시작 날짜 |
| endResultDate | | | |
| msgStatusName | | | |
| resultCode | | | |
| pageNum | | | |
| pageSize | | | |

#### 응답
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
                "templateName": "치환테스트(SMS)",
                "masterStatusCode": "READY",
                "sendNo": "15883796",
                "requestDate": "2017-12-20 14:15:58",
                "tagExpression": [
                    "Kb6BjCY1"
                ],
                "title": "",
                "body": "SMS 발송 입니다##content##",
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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	Object|	데이터 영역|
|-- requestId | String | 요청 아이디 |
|-- requestIp | String | 요청 아이피 |
| sendType | String | 발송 타입(0:SMS, 1:LMS/MMS)|
| templateId | String | 템플릿 아이디 |
| templateName | String | 템플릿명 |
| masterStatusCode | String | 마스터 상태코드 |
| sendNo | String | 발송번호 |
| requestDate | String | 요청일시 |
| tagExpression | List<String> | 태그 표현식 |
| title | String | 제목 |
| body | String | 본문 |
| adYn | String | 광고여부 |
| autoSendYn | String | 자동발송여부 |
| sendErrorCount | Integer | 발송 에러 수 |
| createUser | String | 생성자 |
| createDate | String | 생성일시 |
| updateUser | String | 수정자 |
| updateDate | String | 수정일시 |

### 태그 발송 수신자 리스트 상세 조회

#### 요청

[URL]

```
GET /sms/v2.0/appKeys/{appKey}/tag-sender/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
| requestId | String | 요청 아이디 |
| recipientSeq | String | 시퀀스 |

[Request body]

```
X
```

#### 응답
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
            "templateName": "치환테스트(MMS)",
            "sendNo": "15883796",
            "recipientNum": "01000000000",
            "title": "제목입니다",
            "body": "내용입니다",
            "requestDate": "2017-12-20 15:28:55.0",
            "msgStatusName": "READY",
            "msgStatus": "1",
            "resultCode": "SUCCESS",
            "receiveDate": "2017-12-25 14:06:04.0",
            "attachFileList": [
                {
                    "fileId": 58217,
                    "filePath": "10869/toast-mt-2017-07-28/1459/58217",
                    "fileName": "스크린샷 2017-07-12 오전 10.00.12.jpg",
                    "fileSize":null,
                    "createDate": "2017-07-28 14:59:31.0",
                    "updateDate": "2017-07-28 14:59:35.0"
                }
            ]
        }
}
}
```

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	Object|	데이터 영역|
|-- requestId | String | 요청 아이디 |
|-- recipientSeq | Integer | 요청 아이디 |
|-- sendType | String | 발송 타입 |
|-- messageType | String | 메세지 타입 |
|-- templateId | String | 템플릿 아이디 |
|-- templateName | String | 템플릿명 |
|-- sendNo | String | 발신번호 |
|-- title | String | 제목 |
|-- body | String | 내용 |
|-- recipientNum | String | 수신번호 |
|-- requestDate | String | 요청일시 |
|-- msgStatusName | String | 메시지상태이름 |
|-- resultCode | String | 수신코드 |
|-- receiveDate | String | 수신일시 |
|-- attachFileList | List | 첨부파일 리스트 |
|---- filePath | String | 첨부파일 - 경로 |
|---- fileName | String | 첨부파일 - 파일명 |
|---- fileSize | Long | 첨부파일 - 사이즈 |
|---- fileSequence | Integer | 첨부파일 - 파일 번호 |
|---- createDate | String | 첨부파일 - 생성일시 |
|---- updateDate | String | 첨부파일 - 수정일시 |




## 템플릿

### 템플릿 발송(본문 수정이 필요 없는 경우)

#### 예제
![[그림 1] 템플릿 등록](http://static.toastoven.net/prod_sms/img_26.png)

<table>
  <thead>
    <tr>
      <th>Http method</th>
      <th>종류</th>
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

Request URL은 템플릿 등록시 선택한 발송타입으로 선택하여 발송.

** Request parameter body 안의 값이 공란이면 해당 templateId의 body 내용으로 치환. **

[Request body] 치환하고자 하는 키와 값을 key,value로 대입.

```
{
    "templateId": "{템플릿 아이디}",
    "recipientList": [{
        "recipientNo": "{수신번호}",
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

![[그림 2] 템플릿 발송 성공](http://static.toastoven.net/prod_sms/img_27.png)

### 템플릿 발송(본문 수정이 필요한 경우)

#### 템플릿 발송 예제

<table>
  <thead>
    <tr>
      <th>Http method</th>
      <th>종류</th>
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


Request URL은 템플릿 등록시 선택한 발송타입으로 선택하여 발송.

** 템플릿 아이디와 Request parameter body 안의 값이 있을 경우, 발신번호와 발신 내용이 템플릿 내용으로 치환되지 않습니다. **

단, 템플릿 아이디를 입력했을 경우, 해당 템플릿으로 조회가 가능합니다.

위와 같은 경우는 템플릿 조회를 한 뒤, 템플릿의 내용을 수정하고 싶을 경우 사용할 수 있습니다.

[Request body]

```
{
    "templateId": "{템플릿 아이디}",
    "body": "{발신 내용}",
    "sendNo": "{발신번호}",
    "recipientList": [{
        "recipientNo": "{수신번호}",
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


### 템플릿 리스트 조회

#### 요청

[URL]

```
GET  /sms/v2.0/appKeys/{appKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Query parameter]

|값|	타입|	필수|	설명|
|---|---|---|---|
|categoryId|	Integer|	옵션|	카테고리 아이디|
|useYn|	String|	옵션|	사용 여부(Y/N)|
|pageNum|	Integer|	옵션|	페이지 번호(Default : 1)|
|pageSize|	Integer|	옵션|	조회 건수(Default : 15)|
|all|	Boolean|	옵션|	전체 조회 여부|

#### 응답

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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- pageNum|	Integer|	현재 페이지 번호|
|- pageSize|	Integer|	조회된 데이터 건수|
|- totalCount|	Integer|	총 데이터 건수|
|- data|	List|	데이터 영역|
|-- templateId|	String|	템플릿 아이디|
|-- serviceId|	Integer|	서비스 아이디(내부용, 미사용 값)|
|-- categoryId|	Integer|	카테고리 아이디|
|-- categoryName|	String|	카테고리명|
|-- sort|	Integer|	정렬값|
|-- templateName|	String|	템플릿명|
|-- templateDesc|	String|	템플릿설명|
|-- useYn|	String|	사용여부|
|-- priority|	String|	우선순위값(미사용 값)|
|-- sendNo|	String|	발신번호|
|-- sendType|	String|	발송타입(0:Sms, 1:mms, 2:auth)|
|-- sendTypeName|	String|	발송타입명|
|-- title|	String|	제목|
|-- body|	String|	본문내용|
|-- attachFileYn|	String|	첨부파일여부(Y/N)|
|-- delYn”|	String|	삭제여부(Y/N), 현재 상태 표기용으로만 사용|
|-- createDate|	String|	등록날짜|
|-- createUser|	String|	등록유저|
|-- updateDate|	String|	수정날짜|
|-- updateUser|	String|	수정유저|
|-- attachFileList|	List|	첨부파일 리스트|
|--- fileId|	Integer|	첨부파일 아이디|
|--- serviceId|	Integer|	서비스 아이디(내부용, 미사용 값)|
|--- attachType|	Integer|	첨부파일 업로드 타입(0:임시,1:업로드,2:템플릿)|
|--- templateId|	String|	템플릿 아이디|
|--- filePath|	String|	첨부파일 경로|
|--- fileName|	String|	첨부파일명|
|--- fileSize|  Integer| 파일 사이즈|
|--- createDate|	String|	첨부파일 등록날짜|
|--- createUser|	String|	첨부파일 등록유저|


### 템플릿 단일 조회

#### 요청

[URL]

```
GET  /sms/v2.0/appKeys/{appKey}/templates/{templateId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|
|templateId|	String|	템플릿 아이디|

#### 응답

```
{
    "header": {
        "isSuccessful": Boolean,
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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- data|	List|	데이터 영역|
|-- templateId|	String|	템플릿 아이디|
|-- serviceId|	Integer|	서비스 아이디(내부용, 미사용 값)|
|-- categoryId|	Integer|	카테고리 아이디|
|-- categoryName|	String|	카테고리명|
|-- sort|	Integer|	정렬값|
|-- templateName|	String|	템플릿명|
|-- templateDesc|	String|	템플릿설명|
|-- useYn|	String|	사용여부|
|-- priority|	String|	우선순위값(미사용 값)|
|-- sendNo|	String|	발신번호|
|-- sendType|	String|	발송타입(0:Sms, 1:mms, 2:auth)|
|-- sendTypeName|	String|	발송타입명|
|-- title|	String|	제목|
|-- body|	String|	본문내용|
|-- attachFileYn|	String|	첨부파일여부(Y/N)|
|-- delYn”|	String|	삭제여부(Y/N), 현재 상태 표기용으로만 사용|
|-- createDate|	String|	등록날짜|
|-- createUser|	String|	등록유저|
|-- updateDate|	String|	수정날짜|
|-- updateUser|	String|	수정유저|
|-- attachFileList|	List|	첨부파일 리스트|
|--- fileId|	Integer|	첨부파일 아이디|
|--- serviceId|	Integer|	서비스 아이디(내부용, 미사용 값)|
|--- attachType|	Integer|	첨부파일 업로드 타입(0:임시,1:업로드,2:템플릿)|
|--- templateId|	String|	템플릿 아이디|
|--- filePath|	String|	첨부파일 경로|
|--- fileName|	String|	첨부파일명|
|--- fileSize|  Integer| 파일 사이즈|
|--- createDate|	String|	첨부파일 등록날짜|
|--- createUser|	String|	첨부파일 등록유저|

## 080 수신거부 서비스

### 수신거부 대상자 조회

#### 요청

[URL]

```
GET  /sms/v2.0/appKeys/{appKey}/blockservice/recipients
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Query parameter]

|값|	타입|	필수|	설명|
|---|---|---|---|
|unsubscribeNo|	String|	필수 |	080수신거부번호 |
|startRequestDate|	String|	옵션 |	수신거부 요청 시작 값(yyyy-MM-dd HH:mm:ss)|
|endRequestDate|	String|	옵션 |	수신거부 요청 종료 값(yyyy-MM-dd HH:mm:ss)|
|pageNum|	Integer|	옵션|	페이지 번호(Default : 1)|
|pageSize|	Integer|	옵션|	조회 건수(Default : 15)|

#### 응답
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

### 수신거부 대상자 삭제

#### 요청

[URL]

```
DELETE  /sms/v2.0/appKeys/{appKey}/blockservice/recipients/removes?unsubscribeNo={unsubscribeNo}&updateUser={updateUser}&recipientNoList={recipientNo},{recipientNo}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Query parameter]

|값|	타입|	필수|	설명|
|---|---|---|---|
|unsubscribeNo|	String|	필수 |	080수신거부번호 |
|updateUser|	String|	필수 |	수신거부 삭제자|
|recipientNo|	String|	필수 |	삭제할 수신거부 번호|

#### 응답
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

## 발신 번호

### 발신 번호 등록 요청

#### 요청

[URL]

|Http method|	URI|
|---|---|
|POST|	/sms/v2.0/appKeys/{appKey}/reqeusts/sendNos|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

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

#### 응답
```
{
  "header" : {
    "isSuccessful" :  Boolean,
    "resultCode" :  Integer,
    "resultMessage" :  String
  }
}
```

### 발신 번호 서류 업로드

#### 요청

[URL]

|Http method|	URI|
|---|---|
|POST|	/sms/v2.0/appKeys/{appKey}/requests/attachFiles/authDocuments|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Request Body]
```
multipart/form-data ...
```

#### 응답
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

### 발신번호 인증 요청 내역 조회 API

#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET|	/sms/v2.0/appKeys/{appKey}/requests/sendNos?status={status}|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Query parameter]

|값|	타입|	설명|
|---|---|---|
|status|	String|	서류 인증 상태<br/>- SRS01	발신번호 등록 요청<br/>- SRS02	심사중<br/>- SRS03	등록 완료<br/>- SRS04	등록 불가<br/>- SRS05	핸드폰 인증 대기<br/>- SRS06	핸드폰 인증 실패<br/>- SRS07	수동 등록 완료|

#### 응답
```
{
  "header" : {
    "isSuccessful" :  Boolean,
    "resultCode" :  Integer,
    "resultMessage" :  String
    },
    "sendNos" : [
    {
      "sendNo" :  String,
      "fileIds" : [
      Integer
      ],
      "comment" :  String,
      "status" :  String,
      "createDate" :  String,
      "updateDate" :  String,
      "confirmDate" :  String
    }
    ]
}
```

### 등록된 발신번호 리스트 조회 API

#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET|	/sms/v2.0/appKeys/{appKey}/sendNos?sendNo={sendNo}&useYn={useYn}&blockYn={blockYn}

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Query parameter]

|값|	타입|	설명|
|---|---|---|
| sendNo | String | 발신 번호 |
| useYn | String | 사용 여부 |
| blockYn | String | 차단 여부 |

#### 응답
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

|값|	타입|	설명|
|---|---|---|
|header|	Object|	헤더 영역|
|- isSuccessful|	Boolean|	성공여부|
|- resultCode|	Integer|	실패 코드|
|- resultMessage|	String|	실패 메시지|
|body|	Object|	본문 영역|
|- pageNum|	Integer|	페이지 번호|
|- pageSize|	Integer|	리스트 출력 사이즈|
|- totalCount|	Integer|	총 데이터 수|
|-- serviceId | Integer | 서비스 아이디 |
|-- sendNo | String | 발신번호 |
|-- useYn | String | 사용여부 |
|-- blockYn | String | 차단여부 |
|-- blockReason | String | 차단사유 |
|-- createDate | String | 생성일시 |
|-- createUser | String | 생성자 |
|-- updateDate | String | 수정일시 |
|-- updateUser | String | 수정자 |

## 통계 조회

### 통합 통계 조회

#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET|	/sms/v2.0/appKeys/{appKey}}/statistics/view?searchType={searchType}&from={from}&to={to}&messageTypes={messageType}&contentTypes={contentType}&templateId={templateId}|

[Path parameter]

|값|	타입|	설명|
|---|---|---|
|appKey|	String|	고유의 appKey|

[Query parameter]

|값|	타입|	필수 |설명|
|---|---|---|---|
| searchType | String | O | 통계 구분<br/>DATE:날짜별, TIME:시간별, DAY:요일별 |
| from | String | O | 통계 조회 시작 날짜<br/>yyyy-MM-dd HH:mm |
| to | String | O | 통계 조회 종료 날짜<br/>yyyy-MM-dd HH:mm |
| messageType | X | String | 메세지 타입<br/>SMS:단문, LMS:장문, MMS:첨부파일, AUTH:인증용 |
| contentType | X | String | 컨텐츠 타입<br/>NORMAL:일반, AD:광고 |
| templateId | X | String | 템플릿 아이디 |

#### 응답
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
|값| 타입|설명|
|---|---|---|
| header | Object | |
| - isSuccessful | Boolean | 성공여부 |
| - resultCode | Integer | 결과코드 |
| - resultMessage | String | 결과메세지 |
| body.data | List | |
| - divisionName | String | 표시이름<br/>날짜,시간,요일 |
| - statisticsView | Object | |
| -- requestedCount | Integer | 요청 카운트 |
| -- succeedCount | Integer | 성공 카운트 |
| -- failedCount | Integer | 실패 카운트 |
| -- pendingCount | Integer | 발송중 카운트 |
| -- succeedRate | String | 성공 비율 |
| -- failedRate | String | 실패 비율 |
| -- pendingRate | String | 발송중 비율 |
