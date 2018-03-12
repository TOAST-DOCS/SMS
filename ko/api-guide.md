## Notification > SMS > API 가이드

## SMS 발송

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
|resultCode|	String|	옵션|	수신 결과 코드 [[조회 코드표](./Developer`s Guide/#_39)]|
|subResultCode|	String|	옵션|	수신 결과 상세 코드 [[조회 코드표](./Developer`s Guide/#_40)]|
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
|-- resultCode|	String|	수신 결과 코드 [[수신 결과 코드표](./Developer`s Guide/#emma-v3)]|
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
|-- resultCode|	String|	수신 결과 코드 [[수신 결과 코드표](./Developer`s Guide/#emma-v3)]|
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
|fileName|	String|	필수|	파일이름(확장자 jpg,jpeg만 가능)|
|fileBody|	Byte[]|	필수|	파일의 Byte[] 값|
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
|resultCode|	String|	옵션|	수신 결과 코드 [[조회 코드표](./Developer`s Guide/#_39)]|
|subResultCode|	String|	옵션|	수신 결과 상세 코드 [[조회 코드표](./Developer`s Guide/#_40)]|
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
|-- resultCode|	String|	수신 결과 코드 [[수신 결과 코드표](./Developer`s Guide/#emma-v3)]|
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
|-- resultCode|	String|	수신 결과 코드 [[수신 결과 코드표](./Developer`s Guide/#emma-v3)]|
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
|resultCode|	String|	옵션|	수신 결과 코드 [[조회 코드표](./Developer`s Guide/#_39)]|
|subResultCode|	String|	옵션|	수신 결과 상세 코드 [[조회 코드표](./Developer`s Guide/#_40)]|
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
|-- resultCode|	String|	수신 결과 코드 [[수신 결과 코드표](./Developer`s Guide/#emma-v3)]|
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
|-- resultCode|	String|	수신 결과 코드 [[수신 결과 코드표](./Developer`s Guide/#emma-v3)]|
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
[[Request Body 참고](./Developer`s Guide/#sms_2)]

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
[[Request Body 참고](./Developer`s Guide/#mms_1)]

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
    "sendNo":"ABCDEFG",
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
    "sendNo":"ABCDEFG",
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
