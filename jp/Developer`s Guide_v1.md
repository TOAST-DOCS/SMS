## Notification > SMS > Developer's Guide

이전 버전보기 :   <select onchange="location.href=this.value"><option value="/ko/Notification/SMS/Developer%60s%20Guide/">API v2.0</option><option selected value="/ko/Notification/SMS/Developer%60s%20Guide_v1/">API v1.0</option></select>

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

|Http method| URI|
|---|---|
|POST|	/sms/v1.0/appKeys/{appKey}/sender/sms|

[Path parameter]

|값|	타입|	설명|
|---|---|
|appKey|	String|	고유의 appKey|

[Request body]

```
{
    "templateId": String,
    "body": String,
    "sendNo": String,
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
|recipientList|	List|	O|	수신자 리스트|
|- recipientNo|	String|	O|	수신번호<br/>countryCode와 조합하여 사용 가능|
|- countryCode|	String|	X|	국가코드<br/>(국제 발송 시, euc-kr(한글,영문) 내용만 가능합니다.)|
|- internationalRecipientNo| String| X| 국가코드가 포함된 수신번호<br/>예)821012345678<br/>recipientNo가 있을 경우 이 값은 무시된다.<br/>|
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
            "statusCode":String
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

#### 단문 SMS 발송 예제(일반 국내 수신 번호)

<table>
  <thead>
    <tr><th>Http method</th><th colspan="3">URL</th></tr>
  </thead>
  <tbody>
    <tr>
      <td>POST</td>
      <td colspan="3">https://api-sms.cloud.toast.com/sms/v1.0/appKeys/{appKey}/sender/sms</td>
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
      "statusCode": "2"
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
      <td colspan="3">https://api-sms.cloud.toast.com/sms/v1.0/appKeys/{appKey}/sender/sms</td>
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
        "internationalRecipientNo": "{국가코드가 포함된 수신번호}"
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
      "statusCode": "2"
    }
  }
}
```

### 단문 SMS 발송리스트 조회

#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET|	/sms/v1.0/appKeys/{appKey}/sender/sms|

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
|resultCode|	String|	옵션|	수신 결과 코드 [[조회 코드표](./Developer`s Guide_v1/#_38)]|
|subResultCode|	String|	옵션|	수신 결과 상세 코드 [[조회 코드표](./Developer`s Guide_v1/#_39)]|
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
|-- countryCode|	String|	국가코드|
|-- recipientNo|	String|	수신번호|
|-- msgStatus|	String|	메시지 상태 코드|
|-- msgStatusName|	String|	메시지 상태 코드명|
|-- resultCode|	String|	수신 결과 코드 [[수신 결과 코드표](./Developer`s Guide_v1/#emma-v3)]|
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

|Http method|	URI|
|---|---|
|GET|	/sms/v1.0/appKeys/{appKey}/sender/sms/{requestId}|

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
|-- countryCode|	String|	국가코드|
|-- recipientNo|	String|	수신번호|
|-- msgStatus|	String|	메시지 상태 코드|
|-- msgStatusName|	String|	메시지 상태 코드명|
|-- resultCode|	String|	수신 결과 코드 [[수신 결과 코드표](./Developer`s Guide_v1/#emma-v3)]|
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

|Http method|	URI|
|---|---|
|POST|	/sms/v1.0/appKeys/{appKey}/sender/mms|

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
|attachFileIdList|	List:Integer|	옵션|	첨부파일 아이디 리스트|
|recipientList|	List|	필수|	수신자 리스트|
|- recipientNo|	String|	필수|	수신번호<br/>countryCode와 조합하여 사용 가능|
|- countryCode|	String|	옵션|	국가코드<br/>(국제 발송 시, euc-kr(한글,영문) 내용만 가능합니다.)|
|- internationalRecipientNo| String| X| 국가코드가 포함된 수신번호<br/>예)821012345678<br/>recipientNo가 있을 경우 이 값은 무시된다.<br/>|
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
            "statusCode":String
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

#### 장문 MMS 발송 예제

<table>
  <thead>
    <tr><th>Http method</th><th colspan="3">URL</th></tr>
  </thead>
  <tbody>
    <tr>
      <td>POST</td>
      <td colspan="3">https://api-sms.cloud.toast.com/sms/v1.0/appKeys/{appKey}/sender/mms</td>
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
      "statusCode": "2"
    }
  }
}
```

### 장문 MMS 발송(첨부파일 포함)

#### 첨부파일 업로드

#### 요청

[URL]

|Http method|	URI|
|---|---|
|POST|	/sms/v1.0/appKeys/{appKey}/attachfile/binaryUpload|

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
      <td colspan="3">https://api-sms.cloud.toast.com/sms/v1.0/appKeys/{appKey}/attachfile/binaryUpload</td>
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
      <td colspan="3">https://api-sms.cloud.toast.com/sms/v1.0/appKeys/{appKey}/sender/mms</td>
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
      "statusCode": "2"
    }
  }
}
```

### 장문 MMS 발송리스트 조회

#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET|	/sms/v1.0/appKeys/{appKey}/sender/mms|

[Path parameter]

|값|	타입|	설명|
|---|---|
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
|resultCode|	String|	옵션|	수신 결과 코드 [[조회 코드표](./Developer`s Guide_v1/#_38)]|
|subResultCode|	String|	옵션|	수신 결과 상세 코드 [[조회 코드표](./Developer`s Guide_v1/#_39)]|
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
|-- countryCode|	String|	국가코드|
|-- recipientNo|	String|	수신번호|
|-- msgStatus|	String|	메시지 상태 코드|
|-- msgStatusName|	String|	메시지 상태 코드명|
|-- resultCode|	String|	수신 결과 코드 [[수신 결과 코드표](./Developer`s Guide_v1/#emma-v3)]|
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

|Http method|	URI|
|---|---|
|GET|	/sms/v1.0/appKeys/{appKey}/sender/mms/{requestId}|

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
|-- countryCode|	String|	국가코드|
|-- recipientNo|	String|	수신번호|
|-- msgStatus|	String|	메시지 상태 코드|
|-- msgStatusName|	String|	메시지 상태 코드명|
|-- resultCode|	String|	수신 결과 코드 [[수신 결과 코드표](./Developer`s Guide_v1/#emma-v3)]|
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

|Http method|	URI|
|---|---|
|POST|	/sms/v1.0/appKeys/{appKey}/sender/auth/sms|

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
|recipientList|	List|	O|	수신자 리스트|
|- recipientNo|	String|	O|	수신번호<br/>countryCode와 조합하여 사용 가능|
|- countryCode|	String|	X|	국가코드<br/>(국제 발송 시, euc-kr(한글,영문) 내용만 가능합니다.)|
|- internationalRecipientNo| String| X| 국가코드가 포함된 수신번호<br/>예)821012345678<br/>recipientNo가 있을 경우 이 값은 무시된다.<br/>|
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
            "statusCode":String
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

#### 예제

<table>
  <thead>
    <tr><th>Http method</th><th colspan="3">URL</th></tr>
  </thead>
  <tbody>
    <tr>
      <td>POST</td>
      <td colspan="3">https://api-sms.cloud.toast.com/sms/v1.0/appKeys/{appKey}/sender/auth/sms</td>
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
      "statusCode": "2"
    }
  }
}
```

### 인증용 SMS 발송리스트 조회

#### 요청

[URL]

|Http method|	URI|
|---|---|
|GET|	/sms/v1.0/appKeys/{appKey}/sender/auth/sms|

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
|resultCode|	String|	옵션|	수신 결과 코드 [[조회 코드표](./Developer`s Guide_v1/#_38)]|
|subResultCode|	String|	옵션|	수신 결과 상세 코드 [[조회 코드표](./Developer`s Guide_v1/#_39)]|
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
|-- countryCode|	String|	국가코드|
|-- recipientNo|	String|	수신번호|
|-- msgStatus|	String|	메시지 상태 코드|
|-- msgStatusName|	String|	메시지 상태 코드명|
|-- resultCode|	String|	수신 결과 코드 [[수신 결과 코드표](./Developer`s Guide_v1/#emma-v3)]|
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

|Http method|	URI|
|---|---|
|GET|	/sms/v1.0/appKeys/{appKey}/sender/auth/sms/{requestId}|

[Path parameter]

|값|	타입|	설명|
|---|---|---|---|
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
|-- countryCode|	String|	국가코드|
|-- recipientNo|	String|	수신번호|
|-- msgStatus|	String|	메시지 상태 코드|
|-- msgStatusName|	String|	메시지 상태 코드명|
|-- resultCode|	String|	수신 결과 코드 [[수신 결과 코드표](./Developer`s Guide_v1/#emma-v3)]|
|-- resultCodeName|	String|	수신 결과 코드명|
|-- telecomCode|	Integer|	통신사코드|
|-- telecomCodeName|	String|	통신사명|
|-- excelSeq|	Integer|	엑셀 시퀀스 번호(엑셀로 발송한 경우 데이터 존재)|
|-- mtPr|	Integer|	발송상세아이디(상세 조회 시 필수)|
|-- sendType|	String|	발송 타입(0:SMS, 1:MMS, 2:Auth)|
|-- userId|	String|	발송 요청 아이디|

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
      <td colspan="3">https://api-sms.cloud.toast.com/sms/v1.0/appKeys/{appKey}/sender/sms</td>
    </tr>
    <tr>
      <td>POST</td>
      <td>MMS</td>
      <td colspan="3">https://api-sms.cloud.toast.com/sms/v1.0/appKeys/{appKey}/sender/mms</td>
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
      <td colspan="3">https://api-sms.cloud.toast.com/sms/v1.0/appKeys/{appKey}/sender/sms</td>
    </tr>
    <tr>
      <td>POST</td>
      <td>MMS</td>
      <td colspan="3">https://api-sms.cloud.toast.com/sms/v1.0/appKeys/{appKey}/sender/mms</td>
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

|Http method|	URI|
|---|---|
|GET|	/sms/v1.0/appKeys/{appKey}/templates|

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

|Http method|	URI|
|---|---|
|GET|	/sms/v1.0/appKeys/{appKey}/templates/{templateId}|

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

## 발신 조회 코드
### 수신 결과 코드
<table class="table table-striped table-hover">
<thead>
	<tr>
    <th>코드값</th>
    <th>의미</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>MTR1</td>
    <td>성공</td>
  </tr>
  <tr>
    <td>MTR2</td>
    <td>실패</td>
  </tr>
</tbody>
</table>

### 수신 결과 상세 코드
<table class="table table-striped table-hover">
<thead>
	<tr>
    <th>코드값</th>
    <th>의미</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>MTR2_1</td>
    <td>유효성 검사 실패</td>
  </tr>
  <tr>
    <td>MTR2_2</td>
    <td>통신사 문제</td>
  </tr>
  <tr>
    <td>MTR2_3</td>
    <td>단말기 문제</td>
  </tr>
</tbody>
</table>

## EMMA v.3 수신결과코드

- EMMA Version : EMMA V3.3.0 이상
- 1) 이통사 : 이통사 전송 후 받은 결과코드이다.
- 2) IB G/W : Infobank G/W가 메시지 수신후 주는 결과코드이다.
- 3) IB EMMA : EMMA가 메시지 전송 요청에 대해 처리한 에러코드이다.

<table class="table table-striped table-hover">
<thead>
	<tr>
		<th>구분</th>
		<th>코드값</th>
		<th>분류</th>
		<th>의미</th>
	</tr>
</thead>
<tbody>
	<tr>
		<td rowspan=29>이통사</td>
		<td>1000</td>
		<td>success</td>
		<td>성공</td>
	</tr>
	<tr>
		<td>2000</td>
		<td>failure</td>
		<td>전송 시간 초과</td>
	</tr>
	<tr>
		<td>2001</td>
		<td>failure</td>
		<td>전송 실패 (무선망단)</td>
	</tr>
	<tr>
		<td>2002</td>
		<td>failure</td>
		<td>전송 실패 (무선망 -> 단말기단)</td>
	</tr>
	<tr>
		<td>2003</td>
		<td>failure</td>
		<td>단말기 전원 꺼짐</td>
	</tr>
	<tr>
		<td>2004</td>
		<td>failure</td>
		<td>단말기 메시지 버퍼 풀</td>
	</tr>
	<tr>
		<td>2005</td>
		<td>failure</td>
		<td>음영지역</td>
	</tr>
	<tr>
		<td>2006</td>
		<td>failure</td>
		<td>메시지 삭제됨</td>
	</tr>
	<tr>
		<td>2007</td>
		<td>failure</td>
		<td>일시적인 단말 문제</td>
	</tr>
	<tr>
		<td>3000</td>
		<td>Invalid</td>
		<td>전송할 수 없음</td>
	</tr>
	<tr>
		<td>3001</td>
		<td>Invalid</td>
		<td>가입자 없음</td>
	</tr>
	<tr>
		<td>3002</td>
		<td>Invalid</td>
		<td>성인 인증 실패</td>
	</tr>
	<tr>
		<td>3003</td>
		<td>Invalid</td>
		<td>수신번호 형식 오류</td>
	</tr>
	<tr>
		<td>3004</td>
		<td>Invalid</td>
		<td>단말기 서비스 일시 정지</td>
	</tr>
	<tr>
		<td>3005</td>
		<td>Invalid</td>
		<td>단말기 호 처리 상태</td>
	</tr>
	<tr>
		<td>3006</td>
		<td>Invalid</td>
		<td>착신 거절</td>
	</tr>
	<tr>
		<td>3007</td>
		<td>Invalid</td>
		<td>Callback URL을 받을 수 없는 폰</td>
	</tr>
	<tr>
		<td>3008</td>
		<td>Invalid</td>
		<td>기타 단말기 문제</td>
	</tr>
	<tr>
		<td>3009</td>
		<td>Invalid</td>
		<td>메시지 형식 오류</td>
	</tr>
	<tr>
		<td>3010</td>
		<td>Invalid</td>
		<td>MMS 미지원 단말</td>
	</tr>
	<tr>
		<td>3011</td>
		<td>Invalid</td>
		<td>서버 오류</td>
	</tr>
	<tr>
		<td>3012</td>
		<td>Invalid</td>
		<td>스팸</td>
	</tr>
	<tr>
		<td>3013</td>
		<td>Invalid</td>
		<td>서비스 거부</td>
	</tr>
	<tr>
		<td>3014</td>
		<td>Invalid</td>
		<td>기타</td>
	</tr>
	<tr>
		<td>3015</td>
		<td>Invalid</td>
		<td>전송 경로 없음</td>
	</tr>
	<tr>
		<td>3016</td>
		<td>Invalid</td>
		<td>첨부파일 사이즈 제한 실패</td>
	</tr>
	<tr>
		<td>3017</td>
		<td>Invalid</td>
		<td>발신번호(=회신번호) 변작 방지 서비스에 의거한 번호 형식 오류</td>
	</tr>
	<tr>
		<td>3018</td>
		<td>Invalid</td>
		<td>발신번호(=회신번호) 변작 방지 서비스에 가입된 휴대폰 개인가입자 번호</td>
	</tr>
	<tr>
		<td>3019</td>
		<td>Invalid</td>
		<td>발신번호(=회신번호) 인포뱅크에 번호 사전등록제를 통해 등록되지 않은 번호</td>
	</tr>
	<tr>
		<td rowspan=20>IB G/W</td>
		<td>1001</td>
		<td rowspan=20></td>
		<td>Server Busy (RS 내부 저장 Queue Full)</td>
	</tr>
	<tr>
		<td>1002</td>
		<td>수신번호 형식 오류</td>
	</tr>
	<tr>
		<td>1003</td>
		<td>회신번호 형식 오류</td>
	</tr>
	<tr>
		<td>1004</td>
		<td>SPAM</td>
	</tr>
	<tr>
		<td>1005</td>
		<td>사용 건수 초과</td>
	</tr>
	<tr>
		<td>1006</td>
		<td>첨부 파일 없음</td>
	</tr>
	<tr>
		<td>1007</td>
		<td>첨부 파일 있음</td>
	</tr>
	<tr>
		<td>1008</td>
		<td>첨부 파일 저장 실패</td>
	</tr>
	<tr>
		<td>1009</td>
		<td>CLIENT_MSG_KEY 없음</td>
	</tr>
	<tr>
		<td>1010</td>
		<td>CONTENT 없음</td>
	</tr>
	<tr>
		<td>1011</td>
		<td>CALLBACK 없음</td>
	</tr>
	<tr>
		<td>1012</td>
		<td>RECIPIENT_INFO 없음</td>
	</tr>
	<tr>
		<td>1013</td>
		<td>SUBJECT 없음</td>
	</tr>
	<tr>
		<td>1014</td>
		<td>첨부 파일 KEY 없음</td>
	</tr>
	<tr>
		<td>1015</td>
		<td>첨부 파일 NAME 없음</td>
	</tr>
	<tr>
		<td>1016</td>
		<td>첨부 파일 크기 없음</td>
	</tr>
	<tr>
		<td>1017</td>
		<td>첨부 파일 Content 없음</td>
	</tr>
	<tr>
		<td>1018</td>
		<td>전송 권한 없음</td>
	</tr>
	<tr>
		<td>1019</td>
		<td>TTL 초과</td>
	</tr>
	<tr>
		<td>1020</td>
		<td>charset conversion error</td>
	</tr>
	<tr>
		<td rowspan=24>IB EMMA</td>
		<td>E900</td>
		<td rowspan=24>Invalid-IB</td>
		<td>전송키가 없는 경우</td>
	</tr>
	<tr>
		<td>E901</td>
		<td>수신번호가 없는 경우</td>
	</tr>
	<tr>
		<td>E902</td>
		<td>(동보인 경우) 수신번호순번이 없는 경우</td>
	</tr>
	<tr>
		<td>E903</td>
		<td>제목 없는 경우</td>
	</tr>
	<tr>
		<td>E904</td>
		<td>메시지가 없는 경우</td>
	</tr>
	<tr>
		<td>E905</td>
		<td>회신번호가 없는 경우</td>
	</tr>
	<tr>
		<td>E906</td>
		<td>메시지키가 없는 경우</td>
	</tr>
	<tr>
		<td>E907</td>
		<td>동보 여부가 없는 경우</td>
	</tr>
	<tr>
		<td>E908</td>
		<td>서비스 타입이 없는 경우</td>
	</tr>
	<tr>
		<td>E909</td>
		<td>전송요청시각이 없는 경우</td>
	</tr>
	<tr>
		<td>E910</td>
		<td>TTL 타임이 없는 경우</td>
	</tr>
	<tr>
		<td>E911</td>
		<td>서비스 타입이 MMS MT인 경우, 첨부파일 확장자가 없는 경우</td>
	</tr>
	<tr>
		<td>E912</td>
		<td>서비스 타입이 MMS MT인 경우, attach_file 폴더에 첨부파일이 없는 경우</td>
	</tr>
	<tr>
		<td>E913</td>
		<td>서비스 타입이 MMS MT인 경우, 첨부파일 사이즈가 0인 경우</td>
	</tr>
	<tr>
		<td>E914</td>
		<td>서비스 타입이 MMS MT인 경우, 메시지 테이블에는 파일그룹키가 있는데 파일 테이 블에 데이터가 없는 경우</td>
	</tr>
	<tr>
		<td>E915</td>
		<td>중복메시지</td>
	</tr>
	<tr>
		<td>E916</td>
		<td>인증서버 차단번호</td>
	</tr>
	<tr>
		<td>E917</td>
		<td>고객DB 차단번호</td>
	</tr>
	<tr>
		<td>E918</td>
		<td>USER CALLBACK FAIL</td>
	</tr>
	<tr>
		<td>E919</td>
		<td>발송 제한 시간인 경우, 메시지 재발송 처리가 금지 된 경우</td>
	</tr>
	<tr>
		<td>E920</td>
		<td>서비스 타입이 LMS MT인 경우, 메시지 테이블에 파일그룹키가 있는 경우</td>
	</tr>
	<tr>
		<td>E921</td>
		<td>서비스 타입이 MMS MT인 경우, 메시지 테이블에 파일그룹키가 없는 경우</td>
	</tr>
	<tr>
		<td>E922</td>
		<td>동보단어 제약문자 사용 오류</td>
	</tr>
	<tr>
		<td>E999</td>
		<td>기타오류</td>
	</tr>
	<tr>
		<td rowspan=10>IB 인증서버</td>
		<td>1000</td>
		<td rowspan=10></td>
		<td> 성공 </td>
	</tr>
	<tr>
		<td>1001</td>
		<td>ID 존재하지 않음</td>
	</tr>
	<tr>
		<td>1002</td>
		<td>인증 오류</td>
	</tr>
	<tr>
		<td>1003</td>
		<td>서버 내부 오류 (DB 접속 실패 등)</td>
	</tr>
	<tr>
		<td>1004</td>
		<td>클라이언트 패스워드 틀림</td>
	</tr>
	<tr>
		<td>1005</td>
		<td>공개키가 이미 등록 되어 있음</td>
	</tr>
	<tr>
		<td>1006</td>
		<td>클라이언트 공개키 중복</td>
	</tr>
	<tr>
		<td>1007</td>
		<td>IP Address 인증 실패</td>
	</tr>
	<tr>
		<td>1008</td>
		<td>MAC Address 인증 실패</td>
	</tr>
	<tr>
		<td>1009</td>
		<td>서비스 거부 됨 (고객 접속 금지)</td>
	</tr>
</tbody>
</table>
