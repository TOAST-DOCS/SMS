## Notification > SMS > API v2.4 Guide

## v2.4 API 소개

### v2.3과 달라진 사항

1. 각 메시지(단문, 장문, 인증) 발송 목록 검색 및 발송 단일 검색 응답 필드가 추가되었습니다.
    - 추가된 필드: messageType, recipientSeq
2. 발송 단일 검색 조건에 사용되는 [mtPr]이 [recipientSeq]로 변경되었습니다.

### [API 도메인]

| 환경   | 	도메인                             |
|------|----------------------------------|
| Real | 	https://api-sms.cloud.toast.com |

<span id="precautions"></span>

### [주의 사항]

* 지원하는 문자 길이는 아래와 같습니다.
* 최대 지원 글자 수는 저장 기준이며 문자 잘림을 방지하기 위해서 표준 규격으로 작성해주세요.
* 인코딩은 EUC-KR 기준으로 발송되며 지원하지 않는 이모티콘은 발송에 실패합니다.

| 분류     | 최대 지원  | 표준 규격                          |
|--------|--------|--------------------------------|
| SMS 본문 | 255자   | 90바이트(한글 45자, 영문 90자)          |
| MMS 제목 | 120자   | 40바이트(한글 20자, 영문 40자)          |
| MMS 본문 | 4,000자 | 2,000바이트(한글 1,000자, 영문 2,000자) |

## 단문 SMS

### 단문 SMS 발송

#### 요청

[URL]

```
POST  /sms/v2.4/appKeys/{appKey}/sender/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

[Request body]

```json
{
  "templateId": "TemplateId",
  "body": "본문",
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
  "statsId": "statsId"
}
```

| 값                                         | 	타입     | 최대 길이                                                             | 	필수 | 	설명                                                                        |
|-------------------------------------------|---------|-------------------------------------------------------------------|-----|----------------------------------------------------------------------------|
| templateId                                | 	String | 50                                                                | 	X  | 	발송 템플릿 ID                                                                 |
| body                                      | 	String | 표준: 90바이트, 최대: 255자(EUC-KR 기준) [[주의사항](./api-guide/#precautions)] | 	O  | 	본문 내용                                                                     |
| sendNo                                    | 	String | 13                                                                | 	O  | 	발신 번호                                                                     |
| requestDate                               | String  | -                                                                 | X   | 예약 일시(yyyy-MM-dd HH:mm)                                                    |
| senderGroupingKey                         | String  | 100                                                               | X   | 발신자 그룹키                                                                    |
| recipientList[].recipientNo               | String  | 20                                                                | 	O  | 	수신 번호<br/>countryCode와 조합하여 사용 가능<br/>최대 1,000명                           |
| recipientList[].countryCode               | 	String | 8                                                                 | 	X  | 	국가 번호 [기본값: 82(한국)]                                                       |
| recipientList[].internationalRecipientNo  | String  | 20                                                                | X   | 국가 번호가 포함된 수신 번호<br/>예)821012345678<br/>recipientNo가 있을 경우 이 값은 무시된다.<br/> |
| recipientList[].templateParameter         | 	Object | -                                                                 | 	X  | 	템플릿 파라미터(템플릿 ID 입력 시)                                                     |
| recipientList[].templateParameter.{key}   | String  | -                                                                 | 	X  | 	치환 키(##key##)                                                             |
| recipientList[].templateParameter.{value} | 	Object | -                                                                 | 	X  | 	치환 키에 매핑되는 Value값                                                         |
| recipientList[].recipientGroupingKey      | String  | 100                                                               | X   | 수신자 그룹키                                                                    |
| userId                                    | 	String | 	100                                                              | X   | 발송 구분자 ex)admin,system                                                     |
| statsId                                   | String  | 10                                                                | X   | 통계 ID(발신 검색 조건에는 포함되지 않습니다)                                                |

#### cURL

```
curl -X POST \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/sender/sms' \
-H 'Content-Type: application/json;charset=UTF-8' \
-d '{
    "body": "본문",
    "sendNo": "15446859",
    "recipientList": [{
            "internationalRecipientNo": "821000000000"
        }
    ]
}'
```

#### 응답

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

| 값                                               | 	타입      | 	설명                                 |
|-------------------------------------------------|----------|-------------------------------------|
| header.isSuccessful                             | 	Boolean | 	성공 여부                              |
| header.resultCode                               | 	Integer | 	실패 코드                              |
| header.resultMessage                            | 	String  | 	실패 메시지                             |
| body.data.requestId                             | 	String  | 	요청 ID                              |
| body.data.statusCode                            | 	String  | 	요청 상태 코드(1:요청 중, 2:요청 완료, 3:요청 실패) |
| body.data.senderGroupingKey                     | 	String  | 	발신자 그룹키                            |
| body.data.sendResultList[].recipientNo          | String   | 수신 번호                               |
| body.data.sendResultList[].resultCode           | Integer  | 결과 코드                               |
| body.data.sendResultList[].resultMessage        | String   | 결과 메시지                              |
| body.data.sendResultList[].recipientSeq         | Integer  | 수신자 시퀀스(mtPr)                       |
| body.data.sendResultList[].recipientGroupingKey | String   | 수신자 그룹키                             |

#### 단문 SMS 발송 예제(일반 국내 수신 번호)

| Http metho | URL                                                                  |
|------------|----------------------------------------------------------------------|
| POST       | https://api-sms.cloud.toast.com/sms/v2.4/appKeys/{appKey}/sender/sms |

[Request body]

```json
{
  "body": "본문",
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

#### 단문 SMS 발송 예제(국가 코드가 포함된 수신 번호)

| Http metho | URL                                                                  |
|------------|----------------------------------------------------------------------|
| POST       | https://api-sms.cloud.toast.com/sms/v2.4/appKeys/{appKey}/sender/sms |

[Request body]

```json
{
  "body": "본문",
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

### 단문 SMS 발송목록 검색

#### 요청

[URL]

```
GET  /sms/v2.4/appKeys/{appKey}/sender/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

[Query parameter]

* requestId 또는 startRequestDate + endRequestDate 또는 startCreateDate + endCreateDate는 필수입니다.
* 등록 날짜/발송 날짜를 동시에 검색하는 경우, 발송 날짜는 무시됩니다.

| 값                    | 	타입      | 	최대 길이 | 필수  | 	설명                                                    |
|----------------------|----------|--------|-----|--------------------------------------------------------|
| requestId            | 	String  | 25     | 	필수 | 	요청 ID                                                 |
| startRequestDate     | 	String  | -      | 	필수 | 	발송 날짜 시작값(yyyy-MM-dd HH:mm:ss)                        |
| endRequestDate       | 	String  | -      | 	필수 | 	발송 날짜 종료값(yyyy-MM-dd HH:mm:ss)                        |
| startCreateDate      | 	String  | -      | 	필수 | 	등록 날짜 시작값(yyyy-MM-dd HH:mm:ss)                        |
| endCreateDate        | 	String  | -      | 	필수 | 	등록 날짜 종료값(yyyy-MM-dd HH:mm:ss)                        |
| startResultDate      | 	String  | -      | 	옵션 | 	수신 날짜 시작값(yyyy-MM-dd HH:mm:ss)                        |
| endResultDate        | 	String  | -      | 	옵션 | 	수신 날짜 종료값(yyyy-MM-dd HH:mm:ss)                        |
| sendNo               | 	String  | 13     | 	옵션 | 	발신 번호                                                 |
| recipientNo          | 	String  | 20     | 	옵션 | 	수신 번호                                                 |
| templateId           | 	String  | 50     | 	옵션 | 	템플릿 번호                                                |
| msgStatus            | 	String  | 1      | 	옵션 | 메시지 상태 코드(0: 실패, 1: 요청, 2: 처리 중, 3:성공, 4:예약취소, 5:중복실패) |
| resultCode           | 	String  | 10     | 	옵션 | 	수신 결과 코드 [[검색 코드표](./error-code/#_2)]                 |
| subResultCode        | 	String  | 10     | 	옵션 | 	수신 결과 상세 코드 [[검색 코드표](./error-code/#_3)]              |
| senderGroupingKey    | 	String  | 100    | 	옵션 | 	발송자 그룹키                                               |
| recipientGroupingKey | 	String  | 100    | 	옵션 | 	수신자 그룹키                                               |
| pageNum              | 	Integer | -      | 	옵션 | 	페이지 번호(기본값 : 1)                                       |
| pageSize             | 	Integer | 1000   | 	옵션 | 	검색 수(기본값 : 15)                                        |

#### cURL

```
curl -X GET \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/sender/sms?startRequestDate='"${START_DATE}"'&endRequestDate='"${END_DATE}" \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

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
        "templateName": "템플릿명",
        "categoryId": 0,
        "categoryName": "카테고리명",
        "body": "단문 테스트",
        "sendNo": "15446859",
        "countryCode": "82",
        "recipientNo": "01000000000",
        "msgStatus": "3",
        "msgStatusName": "성공",
        "resultCode": "1000",
        "resultCodeName": "성공",
        "telecomCode": 10001,
        "telecomCodeName": "SKT",
        "recipientSeq": 1,
        "sendType": "0",
        "messageType": "SMS",
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

| 값                                | 	타입      | 	설명                                            |
|----------------------------------|----------|------------------------------------------------|
| header.isSuccessful              | 	Boolean | 	성공 여부                                         |
| header.resultCode                | 	Integer | 	실패 코드                                         |
| header.resultMessage             | 	String  | 	실패 메시지                                        |
| body.pageNum                     | 	Integer | 	현재 페이지 번호                                     |
| body.pageSize                    | 	Integer | 	검색된 데이터 수                                     |
| body.totalCount                  | 	Integer | 	총 데이터 수                                       |
| body.data[].requestId            | 	String  | 	요청 ID                                         |
| body.data[].requestDate          | 	String  | 	발신 일시                                         |
| body.data[].resultDate           | 	String  | 	수신 일시                                         |
| body.data[].templateId           | 	String  | 	템플릿 ID                                        |
| body.data[].templateName         | 	String  | 	템플릿명                                          |
| body.data[].categoryId           | 	Integer | 	카테고리 ID                                       |
| body.data[].categoryName         | 	String  | 	카테고리명                                         |
| body.data[].body                 | 	String  | 	본문 내용                                         |
| body.data[].sendNo               | 	String  | 	발신 번호                                         |
| body.data[].countryCode          | 	String  | 	국가 번호                                         |
| body.data[].recipientNo          | 	String  | 	수신 번호                                         |
| body.data[].msgStatus            | 	String  | 	메시지 상태 코드                                     |
| body.data[].msgStatusName        | 	String  | 	메시지 상태 코드명                                    |
| body.data[].resultCode           | 	String  | 	수신 결과 코드 [[수신 결과 코드표](./error-code/#emma-v3)] |
| body.data[].resultCodeName       | 	String  | 	수신 결과 코드명                                     |
| body.data[].telecomCode          | 	Integer | 	통신사 코드                                        |
| body.data[].telecomCodeName      | 	String  | 	통신사명                                          |
| body.data[].recipientSeq         | 	Integer | 	발송 상세 ID(상세 검색 시 필수)(구 mtPr)                  |
| body.data[].sendType             | 	String  | 	발송 유형(0:Sms, 1:Lms/Mms, 2:Auth)               |
| body.data[].messageType          | 	String  | 	메시지 타입(SMS/LMS/MMS/AUTH)                      |
| body.data[].userId               | 	String  | 	발송 요청 ID                                      |
| body.data[].adYn                 | 	String  | 	광고 여부                                         |
| body.data[].senderGroupingKey    | 	String  | 	발신자 그룹키                                       |
| body.data[].recipientGroupingKey | 	String  | 	수신자 그룹키                                       |

### 단문 SMS 발송 단일 검색

#### 요청

[URL]

```
GET  /sms/v2.4/appKeys/{appKey}/sender/sms/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값         | 	타입     | 	설명     |
|-----------|---------|---------|
| appKey    | 	String | 	고유의 앱키 |
| requestId | 	String | 	요청 ID  |

[Query parameter]

| 값            | 	타입      | 	필수 | 	설명       |
|--------------|----------|-----|-----------|
| recipientSeq | 	Integer | 	필수 | 	발송 상세 ID |

#### cURL

```
curl -X GET \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/sender/sms/'"${REQUEST_ID}"'?recipientSeq='"${RECIPIENT_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

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
      "templateName": "템플릿명",
      "categoryId": 0,
      "categoryName": "카테고리명",
      "body": "본문",
      "sendNo": "15446859",
      "countryCode": "82",
      "recipientNo": "01000000000",
      "msgStatus": "3",
      "msgStatusName": "성공",
      "resultCode": "1000",
      "resultCodeName": "성공",
      "telecomCode": 10001,
      "telecomCodeName": "SKT",
      "recipientSeq": 1,
      "sendType": "0",
      "messageType": "SMS",
      "userId": "tester",
      "adYn": "N",
      "resultMessage": "",
      "senderGroupingKey": "SenderGroupingKey",
      "recipientGroupingKey": "RecipientGroupingKey"
    }
  }
}
```

| 값                              | 	타입      | 	설명                                            |
|--------------------------------|----------|------------------------------------------------|
| header.isSuccessful            | 	Boolean | 	성공 여부                                         |
| header.resultCode              | 	Integer | 	실패 코드                                         |
| header.resultMessage           | 	String  | 	실패 메시지                                        |
| body.data.requestId            | 	String  | 	요청 ID                                         |
| body.data.requestDate          | 	String  | 	발신 일시                                         |
| body.data.resultDate           | 	String  | 	수신 일시                                         |
| body.data.templateId           | 	String  | 	템플릿 ID                                        |
| body.data.templateName         | 	String  | 	템플릿명                                          |
| body.data.categoryId           | 	Integer | 	카테고리 ID                                       |
| body.data.categoryName         | 	String  | 	카테고리명                                         |
| body.data.body                 | 	String  | 	본문 내용                                         |
| body.data.sendNo               | 	String  | 	발신 번호                                         |
| body.data.countryCode          | 	String  | 	국가 번호                                         |
| body.data.recipientNo          | 	String  | 	수신 번호                                         |
| body.data.msgStatus            | 	String  | 	메시지 상태 코드                                     |
| body.data.msgStatusName        | 	String  | 	메시지 상태 코드명                                    |
| body.data.resultCode           | 	String  | 	수신 결과 코드 [[수신 결과 코드표](./error-code/#emma-v3)] |
| body.data.resultCodeName       | 	String  | 	수신 결과 코드명                                     |
| body.data.telecomCode          | 	Integer | 	통신사 코드                                        |
| body.data.telecomCodeName      | 	String  | 	통신사명                                          |
| body.data[].recipientSeq       | 	Integer | 	발송 상세 ID(상세 검색 시 필수)(구 mtPr)                  |
| body.data.sendType             | 	String  | 	발송 유형(0:Sms, 1:Lms/Mms, 2:Auth)               |
| body.data.messageType          | 	String  | 	메시지 타입(SMS/LMS/MMS/AUTH)                      |
| body.data.userId               | 	String  | 	발송 요청 ID                                      |
| body.data.adYn                 | 	String  | 	광고 여부                                         |
| body.data.senderGroupingKey    | 	String  | 	발신자 그룹키                                       |
| body.data.recipientGroupingKey | 	String  | 	수신자 그룹키                                       |

## 장문 MMS

### 장문 MMS 발송(첨부 파일 미포함)

※ LMS/MMS는 해외 발송이 불가능합니다.

#### 요청

[URL]

```
POST  /sms/v2.4/appKeys/{appKey}/sender/mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

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
  "statsId": "statsId"
}
```

| 값                                         | 	타입     | 최대 길이 | 	필수 | 	설명                                                                        |
|-------------------------------------------|---------|-------|-----|----------------------------------------------------------------------------|
| templateId                                | 	String | 50    | 	X  | 	발송 템플릿 ID                                                                 |
| title                                     | 	String | 120   | 	O  | 	제목                                                                        |
| body                                      | 	String | 4000  | 	O  | 	본문                                                                        |
| sendNo                                    | 	String | 13    | 	O  | 	발신 번호                                                                     |
| requestDate                               | String  | -     | X   | 예약 일시(yyyy-MM-dd HH:mm)                                                    |
| senderGroupingKey                         | String  | 100   | X   | 발신자 그룹키                                                                    |
| recipientList[].recipientNo               | String  | 20    | 	O  | 	수신 번호<br/>countryCode와 조합하여 사용 가능                                         |
| recipientList[].countryCode               | String  | 8     | 	X  | 	국가 번호 [기본값: 82(한국)]<br/>MMS는 해외 발송 불가                                     |
| recipientList[].internationalRecipientNo  | String  | 20    | X   | 국가 번호가 포함된 수신 번호<br/>예)821012345678<br/>recipientNo가 있을 경우 이 값은 무시된다.<br/> |
| recipientList[].templateParameter         | 	Object | -     | 	X  | 	템플릿 파라미터(템플릿 ID 입력 시)                                                     |
| recipientList[].templateParameter.{key}   | 	String | -     | 	X  | 	치환 키(##key##)                                                             |
| recipientList[].templateParameter.{value} | 	Object | -     | 	X  | 	치환 키에 매핑되는 Value값                                                         |
| recipientList[].recipientGroupingKey      | String  | 1000  | X   | 수신자 그룹키                                                                    |
| userId                                    | 	String | 100   | 	X  | 발송 구분자 ex)admin,system                                                     |
| statsId                                   | String  | 10    | X   | 통계 ID(발신 검색 조건에는 포함되지 않습니다)                                                |

#### cURL

```
curl -X POST \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/sender/mms' \
-H 'Content-Type: application/json;charset=UTF-8' \
-d '{
    "title": "{제목}",
    "body": "{본문 내용}",
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

#### 응답

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

| 값                                               | 	타입      | 	설명                                 |
|-------------------------------------------------|----------|-------------------------------------|
| header.isSuccessful                             | 	Boolean | 	성공 여부                              |
| header.resultCode                               | 	Integer | 	실패 코드                              |
| header.resultMessage                            | 	String  | 	실패 메시지                             |
| body.data.requestId                             | 	String  | 	요청 ID                              |
| body.data.statusCode                            | 	String  | 	요청 상태 코드(1:요청 중, 2:요청 완료, 3:요청 실패) |
| body.data.senderGroupingKey                     | 	String  | 	발신자 그룹키                            |
| body.data.sendResultList[].recipientNo          | String   | 수신 번호                               |
| body.data.sendResultList[].resultCode           | Integer  | 결과 코드                               |
| body.data.sendResultList[].resultMessage        | String   | 결과 메시지                              |
| body.data.sendResultList[].recipientSeq         | Integer  | 수신자 시퀀스(mtPr)                       |
| body.data.sendResultList[].recipientGroupingKey | String   | 수신자 그룹키                             |

#### 장문 MMS 발송 예제

| Http metho | URL                                                                  |
|------------|----------------------------------------------------------------------|
| POST       | https://api-sms.cloud.toast.com/sms/v2.4/appKeys/{appKey}/sender/mms |

[Request body]

```json
{
  "title": "제목",
  "body": "본문",
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

### 장문 MMS 발송(첨부 파일 포함)

#### 첨부 파일 발송 예제

| Http method | URL                                                                  |
|-------------|----------------------------------------------------------------------|
| POST        | https://api-sms.cloud.toast.com/sms/v2.4/appKeys/{appKey}/sender/mms |

[Request body]

```json
{
  "title": "제목",
  "body": "본문",
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

- 첨부 파일(필드명: attachFileIdList)을 포함한 장문 MMS 발송을 위해서는 사전에 첨부 파일 업로드가 진행되어야 합니다.<br>
- [[첨부 파일 업로드](./api-guide/#binaryUpload)]</a> 가이드를 참고하시기 바랍니다.
- 첨부 이미지 제한 사항
    - 지원 코덱: .jpg, .jpeg
    - 첨부 이미지 개수: 3개 이하
    - 첨부 이미지 사이즈: 1개당 300KB 이하. 단, 첨부한 이미지의 개수가 3개일 경우 합산 800KB 이하.
    - 첨부 이미지 해상도: 1000*1000 이하

### 장문 MMS 발송 목록 검색

#### 요청

[URL]

```
GET  /sms/v2.4/appKeys/{appKey}/sender/mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

[Query parameter]

* requestId 또는 startRequestDate + endRequestDate 또는 startCreateDate + endCreateDate는 필수입니다.
* 등록 날짜/발송 날짜를 동시에 검색하는 경우, 발송 날짜는 무시됩니다.

| 값                    | 	타입      | 최대 길이 | 	필수 | 	설명                                                    |
|----------------------|----------|-------|-----|--------------------------------------------------------|
| requestId            | 	String  | 25    | 	필수 | 	요청 ID                                                 |
| startRequestDate     | 	String  | -     | 	필수 | 	발송 날짜 시작값(yyyy-MM-dd HH:mm:ss)                        |
| endRequestDate       | 	String  | -     | 	필수 | 	발송 날짜 종료값(yyyy-MM-dd HH:mm:ss)                        |
| startCreateDate      | 	String  | -     | 	필수 | 	등록 날짜 시작값(yyyy-MM-dd HH:mm:ss)                        |
| endCreateDate        | 	String  | -     | 	필수 | 	등록 날짜 종료값(yyyy-MM-dd HH:mm:ss)                        |
| startResultDate      | 	String  | -     | 	옵션 | 	수신 날짜 시작값(yyyy-MM-dd HH:mm:ss)                        |
| endResultDate        | 	String  | -     | 	옵션 | 	수신 날짜 종료값(yyyy-MM-dd HH:mm:ss)                        |
| sendNo               | 	String  | 13    | 	옵션 | 	발신 번호                                                 |
| recipientNo          | 	String  | 20    | 	옵션 | 	수신 번호                                                 |
| templateId           | 	String  | 50    | 	옵션 | 	템플릿 번호                                                |
| msgStatus            | 	String  | 1     | 	옵션 | 메시지 상태 코드(0: 실패, 1: 요청, 2: 처리 중, 3:성공, 4:예약취소, 5:중복실패) |
| resultCode           | 	String  | 10    | 	옵션 | 	수신 결과 코드 [[검색 코드표](./error-code/#_2)]                 |
| subResultCode        | 	String  | 10    | 	옵션 | 	수신 결과 상세 코드 [[검색 코드표](./error-code/#_3)]              |
| senderGroupingKey    | 	String  | 100   | 	옵션 | 	발송자 그룹키                                               |
| recipientGroupingKey | 	String  | 100   | 	옵션 | 	수신자 그룹키                                               |
| pageNum              | 	Integer | -     | 	옵션 | 	페이지 번호(기본값 : 1)                                       |
| pageSize             | 	Integer | 1000  | 	옵션 | 	검색 수(기본값 : 15)                                        |

#### cURL

```
curl -X GET \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/sender/mms?startRequestDate='"${START_DATE}"'&endRequestDate='"${END_DATE}" \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

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
        "templateName": "템플릿명",
        "categoryId": 0,
        "categoryName": "카테고리명",
        "title": "제목",
        "body": "본문",
        "sendNo": "15446859",
        "countryCode": "82",
        "recipientNo": "01000000000",
        "msgStatus": "3",
        "msgStatusName": "성공",
        "resultCode": "1000",
        "resultCodeName": "성공",
        "telecomCode": 10001,
        "telecomCodeName": "SKT",
        "recipientSeq": 1,
        "sendType": "0",
        "messageType": "LMS",
        "userId": "tester",
        "adYn": "N",
        "attachFileList": [
          {
            fileId: Integer,
            filePath: String,
            fileName: String,
            saveFileName: String,
            uploadType: String
          }
        ],
        "resultMessage": "",
        "senderGroupingKey": "SenderGroupingKey",
        "recipientGroupingKey": "RecipientGroupingKey"
      }
    ]
  }
}
```

| 값                                         | 	타입      | 	설명                                            |
|-------------------------------------------|----------|------------------------------------------------|
| header.isSuccessful                       | 	Boolean | 	성공 여부                                         |
| header.resultCode                         | 	Integer | 	실패 코드                                         |
| header.resultMessage                      | 	String  | 	실패 메시지                                        |
| body                                      | 	Object  | 	본문 영역                                         |
| body.pageNum                              | 	Integer | 	현재 페이지 번호                                     |
| body.pageSize                             | 	Integer | 	검색된 데이터 수                                     |
| body.totalCount                           | 	Integer | 	총 데이터 수                                       |
| body.data[].requestId                     | 	String  | 	요청 ID                                         |
| body.data[].requestDate                   | 	String  | 	발신 일시                                         |
| body.data[].resultDate                    | 	String  | 	수신 일시                                         |
| body.data[].templateId                    | 	String  | 	템플릿 ID                                        |
| body.data[].templateName                  | 	String  | 	템플릿명                                          |
| body.data[].categoryId                    | 	Integer | 	카테고리 ID                                       |
| body.data[].categoryName                  | 	String  | 	카테고리명                                         |
| body.data[].body                          | 	String  | 	본문 내용                                         |
| body.data[].sendNo                        | 	String  | 	발신 번호                                         |
| body.data[].countryCode                   | 	String  | 	국가 번호                                         |
| body.data[].recipientNo                   | 	String  | 	수신 번호                                         |
| body.data[].msgStatus                     | 	String  | 	메시지 상태 코드                                     |
| body.data[].msgStatusName                 | 	String  | 	메시지 상태 코드명                                    |
| body.data[].resultCode                    | 	String  | 	수신 결과 코드 [[수신 결과 코드표](./error-code/#emma-v3)] |
| body.data[].resultCodeName                | 	String  | 	수신 결과 코드명                                     |
| body.data[].telecomCode                   | 	Integer | 	통신사 코드                                        |
| body.data[].telecomCodeName               | 	String  | 	통신사명                                          |
| body.data[].recipientSeq                  | 	Integer | 	발송 상세 ID(상세 검색 시 필수)(구 mtPr)                  |
| body.data[].sendType                      | 	String  | 	발송 유형(0:Sms, 1:Lms/Mms, 2:Auth)               |
| body.data[].messageType                   | 	String  | 	메시지 타입(SMS/LMS/MMS/AUTH)                      |
| body.data[].userId                        | 	String  | 	발송 요청 ID                                      |
| body.data[].adYn                          | 	String  | 	광고 여부                                         |
| body.data[].attachFileList[].fileId       | 	Integer | 	파일 ID                                         |
| body.data[].attachFileList[].filePath     | 	String  | 	파일 저장경로(내부용)                                  |
| body.data[].attachFileList[].filename     | 	String  | 	파일명                                           |
| body.data[].attachFileList[].saveFileName | 	String  | 	저장된 첨부파일명                                     |
| body.data[].attachFileList[].uploadType   | 	String  | 	업로드 타입                                        |
| body.data[].senderGroupingKey             | 	String  | 	발신자 그룹키                                       |
| body.data[].recipientGroupingKey          | 	String  | 	수신자 그룹키                                       |

### 장문 MMS 발송 단일 검색

#### 요청

[URL]

```
GET  /sms/v2.4/appKeys/{appKey}/sender/mms/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값         | 	타입     | 	설명     |
|-----------|---------|---------|
| appKey    | 	String | 	고유의 앱키 |
| requestId | 	String | 	요청 ID  |

[Query parameter]

| 값            | 	타입      | 	필수 | 	설명       |
|--------------|----------|-----|-----------|
| recipientSeq | 	Integer | 	필수 | 	발송 상세 ID |

#### cURL

```
curl -X GET \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/sender/mms/'"${REQUEST_ID}"'?recipientSeq='"${RECIPIENT_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

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

| 값                                         | 	타입      | 	설명                                            |
|-------------------------------------------|----------|------------------------------------------------|
| header.isSuccessful                       | 	Boolean | 	성공 여부                                         |
| header.resultCode                         | 	Integer | 	실패 코드                                         |
| header.resultMessage                      | 	String  | 	실패 메시지                                        |
| body                                      | 	Object  | 	본문 영역                                         |
| body.pageNum                              | 	Integer | 	현재 페이지 번호                                     |
| body.pageSize                             | 	Integer | 	검색된 데이터 수                                     |
| body.totalCount                           | 	Integer | 	총 데이터 수                                       |
| body.data[].requestId                     | 	String  | 	요청 ID                                         |
| body.data[].requestDate                   | 	String  | 	발신 일시                                         |
| body.data[].resultDate                    | 	String  | 	수신 일시                                         |
| body.data[].templateId                    | 	String  | 	템플릿 ID                                        |
| body.data[].templateName                  | 	String  | 	템플릿명                                          |
| body.data[].categoryId                    | 	Integer | 	카테고리 ID                                       |
| body.data[].categoryName                  | 	String  | 	카테고리명                                         |
| body.data[].body                          | 	String  | 	본문 내용                                         |
| body.data[].sendNo                        | 	String  | 	발신 번호                                         |
| body.data[].countryCode                   | 	String  | 	국가 번호                                         |
| body.data[].recipientNo                   | 	String  | 	수신 번호                                         |
| body.data[].msgStatus                     | 	String  | 	메시지 상태 코드                                     |
| body.data[].msgStatusName                 | 	String  | 	메시지 상태 코드명                                    |
| body.data[].resultCode                    | 	String  | 	수신 결과 코드 [[수신 결과 코드표](./error-code/#emma-v3)] |
| body.data[].resultCodeName                | 	String  | 	수신 결과 코드명                                     |
| body.data[].telecomCode                   | 	Integer | 	통신사 코드                                        |
| body.data[].telecomCodeName               | 	String  | 	통신사명                                          |
| body.data[].recipientSeq                  | 	Integer | 	발송 상세 ID(상세 검색 시 필수)                          |
| body.data[].sendType                      | 	String  | 	발송 유형(0:Sms, 1:Lms/Mms, 2:Auth)               |
| body.data[].messageType                   | 	String  | 	메시지 타입(SMS/LMS/MMS/AUTH)                      |
| body.data[].userId                        | 	String  | 	발송 요청 ID                                      |
| body.data[].adYn                          | 	String  | 	광고 여부                                         |
| body.data[].attachFileList[].fileId       | 	Integer | 	파일 ID                                         |
| body.data[].attachFileList[].filePath     | 	String  | 	파일 저장경로(내부용)                                  |
| body.data[].attachFileList[].filename     | 	String  | 	파일명                                           |
| body.data[].attachFileList[].saveFileName | 	String  | 	저장된 첨부파일명                                     |
| body.data[].attachFileList[].uploadType   | 	String  | 	업로드 타입                                        |
| body.data[].senderGroupingKey             | 	String  | 	발신자 그룹키                                       |
| body.data[].recipientGroupingKey          | 	String  | 	수신자 그룹키                                       |

## 인증용 SMS(긴급)

### 인증용 SMS 발송

<span id="precautions-authword"></span>

1. 인증용 SMS 발송 시 본문 내 포함되어야 할 인증 문구 안내

| 구분          | 인증 문구                                      |
|-------------|--------------------------------------------| 
| 인증용 SMS(긴급) | auth, password, verif, にんしょう, 認証, 비밀번호, 인증 |

- 예시 1-1) 인증용 SMS(긴급) API 발송 요청 시 전문(템플릿 치환자 포함)에 인증 문구가 포함되어 있지 않은 경우 발송에 실패합니다.
- 예시 1-2) 인증 문구가 영문인 경우 대소문자 구분 없이 유효성 검사가 진행됩니다.

#### 요청

[URL]

```
POST  /sms/v2.4/appKeys/{appKey}/sender/auth/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

[Request body]

```json
{
  "templateId": "TemplateId",
  "body": "본문",
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
  "statsId": "statsId"
}
```

| 값                                         | 	타입     | 최대 길이                                                             | 	필수 | 	설명                                                                     |
|-------------------------------------------|---------|-------------------------------------------------------------------|-----|-------------------------------------------------------------------------|
| templateId                                | 	String | 50                                                                | 	X  | 	발송 템플릿 ID                                                              |
| body                                      | 	String | 표준: 90바이트, 최대: 255자(EUC-KR 기준) [[주의사항](./api-guide/#precautions)] | 	O  | 	본문 내용 [[주의사항](./api-guide/#precautions-authword)]                      |
| sendNo                                    | 	String | 13                                                                | 	O  | 	발신 번호                                                                  |
| requestDate                               | String  | -                                                                 | X   | 예약 일시(yyyy-MM-dd HH:mm)                                                 |
| senderGroupingKey                         | String  | 100                                                               | X   | 발신자 그룹키                                                                 |
| recipientList[].recipientNo               | 	String | 20                                                                | 	O  | 	수신 번호<br/>countryCode와 조합하여 사용 가능                                      |
| recipientList[].countryCode               | 	String | 8                                                                 | 	X  | 	국가 번호 [기본값: 82(한국)]                                                    |
| recipientList[].internationalRecipientNo  | String  | 20                                                                | X   | 국가 번호가 포함된 수신 번호<br/>예)821012345678<br/>recipientNo가 있을 경우 이 값은 무시<br/> |
| recipientList[].templateParameter         | 	Object | -                                                                 | 	X  | 	템플릿 파라미터(템플릿 ID 입력 시)                                                  |
| recipientList[].templateParameter.{key}   | String  | -                                                                 | 	X  | 	치환 키(##key##)                                                          |
| recipientList[].templateParameter.{value} | Object  | -                                                                 | 	X  | 	치환 키에 매핑되는 Value값                                                      |
| recipientList[].recipientGroupingKey      | String  | 100                                                               | X   | 수신자 그룹키                                                                 |
| userId                                    | 	String | 100                                                               | 	X  | 발송 구분자 ex)admin,system                                                  |
| statsId                                   | String  | 10                                                                | X   | 통계 ID(발신 검색 조건에는 포함되지 않습니다)                                             |

#### cURL

```
curl -X POST \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/sender/auth/sms' \
-H 'Content-Type: application/json;charset=UTF-8' \
-d '{
    "body": "인증 테스트",
    "sendNo": "15446859",
    "recipientList": [{
            "recipientNo": "01000000000",
            "templateParameter": {}
        }
    ],
    "userId": ""
}'
```

#### 응답

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

| 값                                               | 	타입      | 	설명                                 |
|-------------------------------------------------|----------|-------------------------------------|
| header.isSuccessful                             | 	Boolean | 	성공 여부                              |
| header.resultCode                               | 	Integer | 	실패 코드                              |
| header.resultMessage                            | 	String  | 	실패 메시지                             |
| body.data.requestId                             | 	String  | 	요청 ID                              |
| body.data.statusCode                            | 	String  | 	요청 상태 코드(1:요청 중, 2:요청 완료, 3:요청 실패) |
| body.data.senderGroupingKey                     | 	String  | 	발신자 그룹키                            |
| body.data.sendResultList[].recipientNo          | String   | 수신 번호                               |
| body.data.sendResultList[].resultCode           | Integer  | 결과 코드                               |
| body.data.sendResultList[].resultMessage        | String   | 결과 메시지                              |
| body.data.sendResultList[].recipientSeq         | Integer  | 수신자 시퀀스(mtPr)                       |
| body.data.sendResultList[].recipientGroupingKey | String   | 수신자 그룹키                             |

#### 예제

| Http metho | URL                                                                       |
|------------|---------------------------------------------------------------------------|
| POST       | https://api-sms.cloud.toast.com/sms/v2.4/appKeys/{appKey}/sender/auth/sms |

[Request body]

```json
{
  "body": "본문",
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

### 인증용 SMS 발송목록 검색

#### 요청

[URL]

```
GET  /sms/v2.4/appKeys/{appKey}/sender/auth/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

[Query parameter]

* requestId 또는 startRequestDate + endRequestDate 또는 startCreateDate + endCreateDate는 필수입니다.
* 등록 날짜/발송 날짜를 동시에 검색하는 경우, 발송 날짜는 무시됩니다.

| 값                    | 	타입      | 	최대 길이 | 필수  | 	설명                                                    |
|----------------------|----------|--------|-----|--------------------------------------------------------|
| requestId            | 	String  | 25     | 	필수 | 	요청 ID                                                 |
| startRequestDate     | 	String  | -      | 	필수 | 	발송 날짜 시작값(yyyy-MM-dd HH:mm:ss)                        |
| endRequestDate       | 	String  | -      | 	필수 | 	발송 날짜 종료값(yyyy-MM-dd HH:mm:ss)                        |
| startCreateDate      | 	String  | -      | 	필수 | 	등록 날짜 시작값(yyyy-MM-dd HH:mm:ss)                        |
| endCreateDate        | 	String  | -      | 	필수 | 	등록 날짜 종료값(yyyy-MM-dd HH:mm:ss)                        |
| startResultDate      | 	String  | -      | 	옵션 | 	수신 날짜 시작값(yyyy-MM-dd HH:mm:ss)                        |
| endResultDate        | 	String  | -      | 	옵션 | 	수신 날짜 종료값(yyyy-MM-dd HH:mm:ss)                        |
| sendNo               | 	String  | 13     | 	옵션 | 	발신 번호                                                 |
| recipientNo          | 	String  | 20     | 	옵션 | 	수신 번호                                                 |
| templateId           | 	String  | 50     | 	옵션 | 	템플릿 번호                                                |
| msgStatus            | 	String  | 1      | 	옵션 | 메시지 상태 코드(0: 실패, 1: 요청, 2: 처리 중, 3:성공, 4:예약취소, 5:중복실패) |
| resultCode           | 	String  | 10     | 	옵션 | 	수신 결과 코드 [[검색 코드표](./error-code/#_2)]                 |
| subResultCode        | 	String  | 10     | 	옵션 | 	수신 결과 상세 코드 [[검색 코드표](./error-code/#_3)]              |
| senderGroupingKey    | 	String  | 100    | 	옵션 | 	발송자 그룹키                                               |
| recipientGroupingKey | 	String  | 100    | 	옵션 | 	수신자 그룹키                                               |
| pageNum              | 	Integer | -      | 	옵션 | 	페이지 번호(기본값 : 1)                                       |
| pageSize             | 	Integer | 1000   | 	옵션 | 	검색 수(기본값 : 15)                                        |

#### cURL

```
curl -X GET \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/sender/auth/sms?startRequestDate='"${START_DATE}"'&endRequestDate='"${END_DATE}" \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

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
        "templateName": "템플릿명",
        "categoryId": 0,
        "categoryName": "카테고리명",
        "body": "단문 테스트",
        "sendNo": "15446859",
        "countryCode": "82",
        "recipientNo": "01000000000",
        "msgStatus": "3",
        "msgStatusName": "성공",
        "resultCode": "1000",
        "resultCodeName": "성공",
        "telecomCode": 10001,
        "telecomCodeName": "SKT",
        "recipientSeq": 1,
        "sendType": "0",
        "messageType": "AUTH",
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

| 값                                | 	타입      | 	설명                                            |
|----------------------------------|----------|------------------------------------------------|
| header.isSuccessful              | 	Boolean | 	성공 여부                                         |
| header.resultCode                | 	Integer | 	실패 코드                                         |
| header.resultMessage             | 	String  | 	실패 메시지                                        |
| body.pageNum                     | 	Integer | 	현재 페이지 번호                                     |
| body.pageSize                    | 	Integer | 	검색된 데이터 수                                     |
| body.totalCount                  | 	Integer | 	총 데이터 수                                       |
| body.data[].requestId            | 	String  | 	요청 ID                                         |
| body.data[].requestDate          | 	String  | 	발신 일시                                         |
| body.data[].resultDate           | 	String  | 	수신 일시                                         |
| body.data[].templateId           | 	String  | 	템플릿 ID                                        |
| body.data[].templateName         | 	String  | 	템플릿명                                          |
| body.data[].categoryId           | 	Integer | 	카테고리 ID                                       |
| body.data[].categoryName         | 	String  | 	카테고리명                                         |
| body.data[].body                 | 	String  | 	본문 내용                                         |
| body.data[].sendNo               | 	String  | 	발신 번호                                         |
| body.data[].countryCode          | 	String  | 	국가 번호                                         |
| body.data[].recipientNo          | 	String  | 	수신 번호                                         |
| body.data[].msgStatus            | 	String  | 	메시지 상태 코드                                     |
| body.data[].msgStatusName        | 	String  | 	메시지 상태 코드명                                    |
| body.data[].resultCode           | 	String  | 	수신 결과 코드 [[수신 결과 코드표](./error-code/#emma-v3)] |
| body.data[].resultCodeName       | 	String  | 	수신 결과 코드명                                     |
| body.data[].telecomCode          | 	Integer | 	통신사 코드                                        |
| body.data[].telecomCodeName      | 	String  | 	통신사명                                          |
| body.data[].recipientSeq         | 	Integer | 	발송 상세 ID(상세 검색 시 필수)(구 mtPr)                  |
| body.data[].sendType             | 	String  | 	발송 유형(0:Sms, 1:Lms/Mms, 2:Auth)               |
| body.data[].messageType          | 	String  | 	메시지 타입(SMS/LMS/MMS/AUTH)                      |
| body.data[].userId               | 	String  | 	발송 요청 ID                                      |
| body.data[].adYn                 | 	String  | 	광고 여부                                         |
| body.data[].senderGroupingKey    | 	String  | 	발신자 그룹키                                       |
| body.data[].recipientGroupingKey | 	String  | 	수신자 그룹키                                       |

### 인증용 SMS 발송 단일 검색

#### 요청

[URL]

```
GET  /sms/v2.4/appKeys/{appKey}/sender/auth/sms/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값         | 	타입     | 	설명     |
|-----------|---------|---------|
| appKey    | 	String | 	고유의 앱키 |
| requestId | 	String | 	요청 ID  |

[Query parameter]

| 값            | 	타입      | 	필수 | 	설명       |
|--------------|----------|-----|-----------|
| recipientSeq | 	Integer | 	필수 | 	발송 상세 ID |

#### cURL

```
curl -X GET \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/sender/auth/sms/'"${REQUEST_ID}"'?recipientSeq='"${RECIPIENT_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

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
      "templateName": "템플릿명",
      "categoryId": 0,
      "categoryName": "카테고리명",
      "body": "본문",
      "sendNo": "15446859",
      "countryCode": "82",
      "recipientNo": "01000000000",
      "msgStatus": "3",
      "msgStatusName": "성공",
      "resultCode": "1000",
      "resultCodeName": "성공",
      "telecomCode": 10001,
      "telecomCodeName": "SKT",
      "recipientSeq": 1,
      "sendType": "0",
      "messageType": "AUTH",
      "userId": "tester",
      "adYn": "N",
      "resultMessage": "",
      "senderGroupingKey": "SenderGroupingKey",
      "recipientGroupingKey": "RecipientGroupingKey"
    }
  }
}
```

| 값                              | 	타입      | 	설명                                            |
|--------------------------------|----------|------------------------------------------------|
| header.isSuccessful            | 	Boolean | 	성공 여부                                         |
| header.resultCode              | 	Integer | 	실패 코드                                         |
| header.resultMessage           | 	String  | 	실패 메시지                                        |
| body.data.requestId            | 	String  | 	요청 ID                                         |
| body.data.requestDate          | 	String  | 	발신 일시                                         |
| body.data.resultDate           | 	String  | 	수신 일시                                         |
| body.data.templateId           | 	String  | 	템플릿 ID                                        |
| body.data.templateName         | 	String  | 	템플릿명                                          |
| body.data.categoryId           | 	Integer | 	카테고리 ID                                       |
| body.data.categoryName         | 	String  | 	카테고리명                                         |
| body.data.body                 | 	String  | 	본문 내용                                         |
| body.data.sendNo               | 	String  | 	발신 번호                                         |
| body.data.countryCode          | 	String  | 	국가 번호                                         |
| body.data.recipientNo          | 	String  | 	수신 번호                                         |
| body.data.msgStatus            | 	String  | 	메시지 상태 코드                                     |
| body.data.msgStatusName        | 	String  | 	메시지 상태 코드명                                    |
| body.data.resultCode           | 	String  | 	수신 결과 코드 [[수신 결과 코드표](./error-code/#emma-v3)] |
| body.data.resultCodeName       | 	String  | 	수신 결과 코드명                                     |
| body.data.telecomCode          | 	Integer | 	통신사 코드                                        |
| body.data.telecomCodeName      | 	String  | 	통신사명                                          |
| body.data[].recipientSeq       | 	Integer | 	발송 상세 ID(상세 검색 시 필수)(구 mtPr)                  |
| body.data.sendType             | 	String  | 	발송 유형(0:Sms, 1:Lms/Mms, 2:Auth)               |
| body.data.messageType          | 	String  | 	메시지 타입(SMS/LMS/MMS/AUTH)                      |
| body.data.userId               | 	String  | 	발송 요청 ID                                      |
| body.data.adYn                 | 	String  | 	광고 여부                                         |
| body.data.senderGroupingKey    | 	String  | 	발신자 그룹키                                       |
| body.data.recipientGroupingKey | 	String  | 	수신자 그룹키                                       |

## 광고 문자

### 광고성 SMS 발송

#### 요청

[URL]

```
POST  /sms/v2.4/appKeys/{appKey}/sender/ad-sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

[Request Body]
위에 SMS 발송과 동일.
[[Request Body 참고](./api-guide/#sms_2)]

<span style="color:red">단, 본문에 광고성 필수 문구가 포함되어야 합니다.</span>

080 번호는 콘솔의 **080 수신 거부 설정** 탭에서 확인할 수 있습니다.

광고성 필수 문구의 규칙은 다음과 같습니다.
- 시작 문구: `(광고)`
- 마지막 문구: `무료수신거부 {080수신거부번호}` 또는 `무료거부 {080수신거부번호}` (해당 문구에는 공백이 포함될 수 있습니다.)

예시
```
(광고)

[무료 수신거부]080XXXXXXX
```
```
(광고)

무료거부 080XXXXXXX
```

#### cURL

```
curl -X POST \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/sender/ad-sms' \
-H 'Content-Type: application/json;charset=UTF-8' \
-d '{
    "body": "(광고) 테스트\n [무료 수신 거부]0808880327",
    "sendNo": "15446859",
    "recipientList": [{
            "recipientNo": "01000000000",
            "templateParameter": {}
        }
    ],
    "userId": ""
}'
```

### 광고성 MMS 발송

※ LMS/MMS는 해외 발송이 불가능합니다.

#### 요청

[URL]

```
POST  /sms/v2.4/appKeys/{appKey}/sender/ad-mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

[Request Body]
위에 MMS 발송과 동일.
[[Request Body 참고](./api-guide/#mms_1)]

<span style="color:red">단, 본문에 광고성 필수 문구가 포함되어야 합니다.</span>

080 번호는 콘솔의 **080 수신 거부 설정** 탭에서 확인할 수 있습니다.

광고성 필수 문구의 규칙은 다음과 같습니다.
- 시작 문구: `(광고)`
- 마지막 문구: `무료수신거부 {080수신거부번호}` 또는 `무료거부 {080수신거부번호}` (해당 문구에는 공백이 포함될 수 있습니다.)

예시
```
(광고)

[무료 수신거부]080XXXXXXX
```
```
(광고)

무료거부 080XXXXXXX
```

#### cURL

```
curl -X POST \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/sender/ad-mms' \
-H 'Content-Type: application/json;charset=UTF-8'
-d '{
    "title": "{제목}",
    "body": "(광고) 테스트\n [무료 수신 거부]0808880327",
    "sendNo": "15446859",
    "recipientList": [{
            "recipientNo": "01000000000",
            "templateParameter": {}
        }
    ],
    "userId": ""
}'
```

## 결과 업데이트 기준 메시지 검색

* 해당 API는 메시지 발송 결과 업데이트 시간 기준으로 검색됩니다.
* 단말기 발송 결과를 서비스에서 가져가 사용하시는 경우 이 API를 사용해주세요.

### 메시지 검색

#### 요청

[URL]

```
GET /sms/v2.4/appKeys/{appKey}/message-results?startUpdateDate={startUpdateDate}&endUpdateDate={endUpdateDate}&messageType={messageType}&pageNum={pageNum}&pageSize={pageSize}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

[Query parameter]

* 검색 시작 시간과 검색 종료 시간의 범위는 하루로 제한됩니다.

| 값               | 	타입     | 	필수 | 	설명                                        |
|-----------------|---------|-----|--------------------------------------------|
| startUpdateDate | 	String | 	필수 | 	결과 업데이트 검색 시작 시간 <br/>yyyy-MM-dd HH:mm:ss |
| endUpdateDate   | 	String | 	필수 | 	결과 업데이트 검색 종료 시간 <br/>yyyy-MM-dd HH:mm:ss |
| messageType     | 	String | 	옵션 | 	메시지 타입(SMS/LMS/MMS/AUTH)                  |
| pageNum         | Integer | 옵션  | 페이지 번호(기본값:1)                              |
| pageSize        | Integer | 옵션  | 검색 수(기본값:15)                               |

#### cURL

```
curl -X GET \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/message-results?startRequestDate='"${START_DATE}"'&endRequestDate='"${END_DATE}" \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

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

| 값                                                 | 	타입      | 	설명                               |
|---------------------------------------------------|----------|-----------------------------------|
| header.isSuccessful                               | 	Boolean | 	성공 여부                            |
| header.resultCode                                 | 	Integer | 	실패 코드                            |
| header.resultMessage                              | 	String  | 	실패 메시지                           |
| body.data.resultUpdateList[].messageType          | String   | 메시지 타입(SMS/LMS/MMS/AUTH)          |
| body.data.resultUpdateList[].requestId            | String   | 요청 ID                             |
| body.data.resultUpdateList[].recipientSeq         | Integer  | 수신자 시퀀스                           |
| body.data.resultUpdateList[].resultCode           | String   | 결과 코드                             |
| body.data.resultUpdateList[].resultCodeName       | String   | 결과 코드명                            |
| body.data.resultUpdateList[].requestDate          | String   | 요청 일시(yyyy-MM-dd HH:mm:ss.S)      |
| body.data.resultUpdateList[].resultDate           | String   | 수신 일시(yyyy-MM-dd HH:mm:ss.S)      |
| body.data.resultUpdateList[].updateDate           | String   | 결과 업데이트 일시(yyyy-MM-dd HH:mm:ss.S) |
| body.data.resultUpdateList[].telecomCode          | String   | 통신사 코드                            |
| body.data.resultUpdateList[].telecomCodeName      | String   | 통신사 코드명                           |
| body.data.resultUpdateList[].senderGroupingKey    | String   | 발신자 그룹 키                          |
| body.data.resultUpdateList[].recipientGroupingKey | String   | 수신자 그룹 키                          |

## 태그 발송

### 태그 SMS 발송

#### 요청

[URL]

```
POST /sms/v2.4/appKeys/{appKey}/tag-sender/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

[Request body]

```json
{
  "body": "본문",
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
  "statsId": "statsId"
}
```

| 값                 | 	타입                 | 	최대 길이                                                            | 필수 | 	설명                                  |
|-------------------|---------------------|-------------------------------------------------------------------|----|--------------------------------------|
| body              | 	String             | 표준: 90바이트, 최대: 255자(EUC-KR 기준) [[주의사항](./api-guide/#precautions)] | 	O | 	본문 내용                               |
| sendNo            | String              | 13                                                                | O  | 발신 번호                                |
| requestDate       | String              | -                                                                 | X  | 예약 일시(yyyy-MM-dd HH:mm)              |
| templateId        | String              | 50                                                                | X  | 템플릿 ID                               |
| templateParameter | Map<String, String> | -                                                                 | X  | 템플릿 파라미터                             |
| tagExpression     | List<String>        | -                                                                 | O  | 태그 표현식<br/>ex) ["tagA","AND","tabB"] |
| userId            | String              | 100                                                               | X  | 요청한 유저 ID                            |
| adYn              | String              | 1                                                                 | X  | 광고 여부(기본값: N)                        |
| autoSendYn        | String              | 1                                                                 | X  | 자동 발송(즉시 발송) 여부(기본값: Y)              |
| statsId           | String              | 10                                                                | X  | 통계 ID(발신 검색 조건에는 포함되지 않습니다)          |

#### cURL

```
curl -X POST \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/tag-sender/sms' \
-H 'Content-Type: application/json;charset=UTF-8' \
-d '{
    "body": "본문",
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

#### 응답

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

| 값                    | 	타입      | 	설명     |
|----------------------|----------|---------|
| header.isSuccessful  | 	Boolean | 	성공 여부  |
| header.resultCode    | 	Integer | 	실패 코드  |
| header.resultMessage | 	String  | 	실패 메시지 |
| body.data.requestId  | 	String  | 	요청 ID  |

### 태그 LMS 발송

※ LMS/MMS는 해외 발송이 불가능합니다.

#### 요청

[URL]

```
POST  /sms/v2.4/appKeys/{appKey}/tag-sender/mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

[Request body]

```json
{
  "body": "본문",
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
  "statsId": "statsId"
}
```

| 값                 | 	타입                 | 	최대 길이 | 필수 | 	설명                                  |
|-------------------|---------------------|--------|----|--------------------------------------|
| title             | String              | 120    | O  | 문자 제목                                |
| body              | String              | 4000   | O  | 문자 내용                                |
| sendNo            | String              | 13     | O  | 발신 번호                                |
| requestDate       | String              | -      | X  | 예약 일시(yyyy-MM-dd HH:mm)              |
| templateId        | String              | 50     | X  | 템플릿 ID                               |
| templateParameter | Map<String, String> | -      | X  | 템플릿 파라미터                             |
| tagExpression     | List<String>        | -      | O  | 태그 표현식<br/>ex) ["tagA","AND","tabB"] |
| attachFileIdList  | List<Integer>       | -      | X  | 첨부 파일 ID(fileId)                     |
| userId            | String              | 100    | X  | 요청한 유저 ID                            |
| adYn              | String              | 1      | X  | 광고 여부(기본값: N)                        |
| autoSendYn        | String              | 1      | X  | 자동 발송(즉시 발송) 여부(기본 Y)                |
| statsId           | String              | 10     | X  | 통계 ID(발신 검색 조건에는 포함되지 않습니다)          |

#### cURL

```
curl -X POST \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/tag-sender/mms' \
-H 'Content-Type: application/json;charset=UTF-8' \
-d '{
    "title": "제목",
    "body": "본문",
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

#### 응답

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

| 값                    | 	타입      | 	설명     |
|----------------------|----------|---------|
| header.isSuccessful  | 	Boolean | 	성공 여부  |
| header.resultCode    | 	Integer | 	실패 코드  |
| header.resultMessage | 	String  | 	실패 메시지 |
| body.data.requestId  | 	String  | 	요청 ID  |

### 태그 발송 목록 검색

#### 요청

[URL]

```
GET /sms/v2.4/appKeys/{appKey}/tag-sender
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

[Query parameter]

* requestId 또는 startRequestDate + endRequestDate 또는 startCreateDate + endCreateDate는 필수입니다.

| 값                | 	타입               | 최대 길이 | 	필수 | 	설명                                                                                                                                                                                                |
|------------------|-------------------|-------|-----|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| appKey           | String            | -     | O   | 앱키                                                                                                                                                                                                 |
| sendType         | required, String  | 1     | O   | 발송 유형<br>SMS : "0",<br>MMS : "1"                                                                                                                                                                   |
| requestId        | String            | -     | O   | 요청 ID                                                                                                                                                                                              |
| startRequestDate | String            | -     | O   | 발송 날짜 시작                                                                                                                                                                                           |
| endRequestDate   | String            | -     | O   | 발송 날짜 종료                                                                                                                                                                                           |
| startCreateDate  | 	String           | -     | 	O  | 	등록 날짜 시작                                                                                                                                                                                          |
| endCreateDate    | 	String           | -     | 	O  | 	등록 날짜 종료                                                                                                                                                                                          |
| statusCode       | String            | 10    | X   | 발송 상태 코드<br>WAIT : "MAS00"<br>READY : "MAS01"<br>SENDREADY : "MAS09"<br>SENDWAIT : "MAS10"<br>SENDING : "MAS11"<br>COMPLETE : "MAS19"<br>CANCELING : "MAS90"<br>CANCEL : "MAS91"<br>FAIL : "MAS99" |
| pageNum          | optional, Integer | -     | X   | 페이지 번호                                                                                                                                                                                             |
| pageSize         | optional, Integer | 1000  | X   | 검색 수                                                                                                                                                                                               |

#### cURL

```
curl -X GET \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/tag-sender?requestId='"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

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

| 값                           | 	타입          | 	설명       |
|-----------------------------|--------------|-----------|
| header.isSuccessful         | 	Boolean     | 	성공 여부    |
| header.resultCode           | 	Integer     | 	실패 코드    |
| header.resultMessage        | 	String      | 	실패 메시지   |
| body.data[].requestId       | String       | 요청 ID     |
| body.data[].requestIp       | String       | 요청 IP     |
| body.data[].requestDate     | String       | 요청 시간     |
| body.data[].tagSendStatus   | String       | 태그 발송 상태  |
| body.data[].tagExpression[] | List<String> | 태그 표현식    |
| body.data[].templateId      | String       | 템플릿 ID    |
| body.data[].templateName    | String       | 템플릿명      |
| body.data[].senderName      | String       | 발신자명      |
| body.data[].senderMail      | String       | 발신자 주소    |
| body.data[].title           | String       | 제목        |
| body.data[].body            | String       | 내용        |
| body.data[].adYn            | String       | 광고 여부     |
| body.data[].autoSendYn      | String       | 자동 발송 여부  |
| body.data[].sendErrorCount  | Integer      | 에러 수신자 건수 |
| body.data[].createUser      | String       | 생성자       |
| body.data[].createDate      | String       | 생성 일시     |
| body.data[].updateUser      | String       | 수정한 사용자   |
| body.data[].updateDate      | String       | 수정 날짜     |

### 태그 발송 수신자 목록 검색

#### 요청

[URL]

```
GET /sms/v2.4/appKeys/{appKey}/tag-sender/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값         | 	타입     | 	설명     |
|-----------|---------|---------|
| appKey    | 	String | 	고유의 앱키 |
| requestId | String  | 요청 ID   |

[Query parameter]

* requestId 또는 startRequestDate + endRequestDate는 필수입니다.

| 값                | 	타입     | 최대 길이 | 	필수 | 	설명                                                                                               |
|------------------|---------|-------|-----|---------------------------------------------------------------------------------------------------|
| recipientNum     | String  | 20    | X   | 수신자 번호                                                                                            |
| startRequestDate | String  | -     | O   | 발송 요청 시작 날짜                                                                                       |
| endRequestDate   | String  | -     | O   | 발송 요청 종료 날짜                                                                                       |
| startResultDate  | String  | -     | X   | 수신 시작 날짜                                                                                          |
| endResultDate    | String  | -     | X   | 수신 종료 날짜                                                                                          |
| msgStatusName    | String  | 10    | X   | 메시지 상태 코드<br/> - READY:준비<br/> - SENDING:발송 요청 중<br/> - COMPLETED : 발송요청 완료<br/> - FAILED : 발송 실패 |
| resultCode       | String  | 10    | X   | 수신 결과 코드                                                                                          |
| pageNum          | Integer | -     | X   | 페이지 번호                                                                                            |
| pageSize         | Integer | 1000  | X   | 검색 수                                                                                              |

#### cURL

```
curl -X GET \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/tag-sender/'"${REQUEST_ID}" \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

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

| 값                       | 	타입      | 	설명                                          |
|-------------------------|----------|----------------------------------------------|
| header.isSuccessful     | 	Boolean | 	성공 여부                                       |
| header.resultCode       | 	Integer | 	실패 코드                                       |
| header.resultMessage    | 	String  | 	실패 메시지                                      |
| body.data.requestId     | String   | 요청 ID                                        |
| body.data.recipientSeq  | Integer  | 수신자 시퀀스                                      |
| body.data.countryCode   | String   | 수신자 국가코드                                     |
| body.data.recipientNo   | String   | 수신자 번호                                       |
| body.data.requestDate   | String   | 요청 일시                                        |
| body.data.msgStatus     | String   | 메시지 상태 코드                                    |
| body.data.msgStatusName | String   | 메시지 상태 코드명                                   |
| body.data.resultCode    | String   | 수신 결과 코드[[수신 결과 코드표](./error-code/#emma-v3)] |
| body.data.receiveDate   | String   | 수신 일시                                        |
| body.data.createDate    | String   | 등록 일시                                        |
| body.data.updateDate    | String   | 수정 날짜                                        |

### 태그 발송 수신자 상세 검색

#### 요청

[URL]

```
GET /sms/v2.4/appKeys/{appKey}/tag-sender/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값            | 	타입     | 	설명     |
|--------------|---------|---------|
| appKey       | 	String | 	고유의 앱키 |
| requestId    | String  | 요청 ID   |
| recipientSeq | String  | 시퀀스     |

[Request body]

```
X
```

#### cURL

```
curl -X GET \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/tag-sender/'"${REQUEST_ID}"'/'"${RECIPIENT_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

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

| 값                                       | 	타입      | 	설명                                          |
|-----------------------------------------|----------|----------------------------------------------|
| header                                  | 	Object  | 	헤더 영역                                       |
| header.isSuccessful                     | 	Boolean | 	성공 여부                                       |
| header.resultCode                       | 	Integer | 	실패 코드                                       |
| header.resultMessage                    | 	String  | 	실패 메시지                                      |
| body.data.requestId                     | String   | 요청 ID                                        |
| body.data.recipientSeq                  | Integer  | 수신자 시퀀스                                      |
| body.data.sendType                      | String   | 발송 유형                                        |
| body.data.messageType                   | String   | 메시지 타입                                       |
| body.data.templateId                    | String   | 템플릿 ID                                       |
| body.data.templateName                  | String   | 템플릿명                                         |
| body.data.sendNo                        | String   | 발신 번호                                        |
| body.data.title                         | String   | 제목                                           |
| body.data.body                          | String   | 내용                                           |
| body.data.recipientNum                  | String   | 수신자 번호                                       |
| body.data.requestDate                   | String   | 요청 일시                                        |
| body.data.msgStatusName                 | String   | 메시지 상태 이름                                    |
| body.data.resultCode                    | String   | 수신 결과 코드[[수신 결과 코드표](./error-code/#emma-v3)] |
| body.data.receiveDate                   | String   | 수신 일시                                        |
| body.data.attachFileList[].filePath     | String   | 첨부 파일 - 경로                                   |
| body.data.attachFileList[].fileName     | String   | 첨부 파일 - 파일명                                  |
| body.data.attachFileList[].fileSize     | Long     | 첨부 파일 - 사이즈                                  |
| body.data.attachFileList[].fileSequence | Integer  | 첨부 파일 - 파일 번호                                |
| body.data.attachFileList[].createDate   | String   | 첨부 파일 - 생성 일시                                |
| body.data.attachFileList[].updateDate   | String   | 첨부 파일 - 수정 날짜                                |

<span id="binaryUpload"></span>

## 첨부 파일

### 첨부 파일 업로드

[URL]

```
POST  /sms/v2.4/appKeys/{appKey}/attachfile/binaryUpload
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

[Request body]

```json
{
  "fileName": "attachment.jpg",
  "createUser": "CreateUser",
  // "fileBody": [0,10,16]
  "fileBody": "{byte[] -> Base64 인코딩한 값}"
}
```

| 값          | 	타입     | 최대 길이 | 	필수 | 	설명                                          |
|------------|---------|-------|-----|----------------------------------------------|
| fileName   | 	String | 	45   | 필수  | 	파일 이름(확장자는 jpg, jpeg(소문자)만 가능)              |
| fileBody   | 	Byte[] | 300K  | 	필수 | 파일 byte[]를 Base64로 인코딩한 값.<br/>* 또는 바이트 배열 값 |
| createUser | 	String | 	100  | 필수  | 	파일 업로드 사용자 정보                               |

#### cURL

```
curl -X POST \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/attachfile/binaryUpload' \
-H 'Content-Type: application/json;charset=UTF-8' \
-d '{
    "fileName": "attachement.jpg",
    "createUser": "API Guide",
    "fileBody": "1234567890"
}'
```

#### 응답

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

| 값                    | 	타입      | 	설명                                                             |
|----------------------|----------|-----------------------------------------------------------------|
| header.isSuccessful  | 	Boolean | 	성공 여부                                                          |
| header.resultCode    | 	Integer | 	실패 코드                                                          |
| header.resultMessage | 	String  | 	실패 메시지                                                         |
| body.data.fileId     | 	Integer | 	파일 ID                                                          |
| body.data.fileName   | 	String  | 	파일명                                                            |
| body.data.filePath   | 	String  | 	첨부 파일 기본 경로 <br/>(https://domain/attachFile/filePath/fileName) |

#### 첨부 파일 업로드 예제

| Http method | URL                                                                               |
|-------------|-----------------------------------------------------------------------------------|
| POST        | https://api-sms.cloud.toast.com/sms/v2.4/appKeys/{appKey}/attachfile/binaryUpload |

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

## 카테고리

### 카테고리 등록

#### 요청

[URL]

```
POST  /sms/v2.4/appKeys/{appKey}/categories
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

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

| 값                | 	타입      | 	최대 길이 | 필수  | 	설명                        |
|------------------|----------|--------|-----|----------------------------|
| categoryParentId | 	Integer | 	-     | 옵션  | 부모 카테고리 ID [기본값: 최상위 카테고리] |
| categoryName     | String   | 50     | 필수  | 카테고리 ID                    |
| categoryDesc     | 	String  | 100    | 	옵션 | 	카테고리명                     |
| useYn            | 	String  | 1      | 	필수 | 사용 여부(Y/N)                 |
| createUser       | 	String  | 100    | 옵션  | 등록한 사용자                    |

##### Description

- categoryParentId 값이 비어있는 경우, 최상위 카테고리 바로 아래에 등록됩니다.

#### cURL

```
curl -X POST \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/categories' \
-H 'Content-Type: application/json;charset=UTF-8' \
-d '{
    "categoryParentId": 0,
    "categoryName": "API Guide",
    "categoryDesc": "API 가이드 테스트",
    "useYn": "Y",
    "createUser": "API Guide"
}'
```

#### 응답

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

| 값                                   | 	타입      | 	설명         |
|-------------------------------------|----------|-------------|
| header.isSuccessful                 | 	Boolean | 	성공 여부      |
| header.resultCode                   | 	Integer | 	실패 코드      |
| header.resultMessage                | 	String  | 	실패 메시지     |
| body.data[].categoryId              | 	Integer | 	카테고리 ID    |
| body.data[].categoryParentId        | 	Integer | 	부모 카테고리 ID |
| body.data[].depth                   | 	Integer | 	카테고리 깊이    |
| body.data[].sort                    | 	Integer | 	카테고리 정렬 순서 |
| body.data[].categoryName            | 	String  | 	카테고리명      |
| body.data[].categorycategoryDescame | 	String  | 	카테고리 설명    |
| body.data[].useYn                   | 	String  | 	사용 여부      |
| body.data[].createUser              | 	String  | 	등록한 사용자    |

### 카테고리 목록 검색

#### 요청

[URL]

```
GET  /sms/v2.4/appKeys/{appKey}/categories
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

[Query parameter]

| 값        | 	타입      | 	최대 길이 | 필수  | 	설명              |
|----------|----------|--------|-----|------------------|
| pageNum  | 	Integer | -      | 	옵션 | 	페이지 번호(기본값 : 1) |
| pageSize | 	Integer | 1000   | 	옵션 | 	검색 수(기본값 : 15)  |

#### cURL

```
curl -X GET \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/categories' \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

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
        "categoryName": "카테고리",
        "categoryDesc": "최상위 카테고리",
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

| 값                                   | 	타입      | 	설명         |
|-------------------------------------|----------|-------------|
| header.isSuccessful                 | 	Boolean | 	성공 여부      |
| header.resultCode                   | 	Integer | 	실패 코드      |
| header.resultMessage                | 	String  | 	실패 메시지     |
| body.pageNum                        | 	Integer | 	현재 페이지 번호  |
| body.pageSize                       | 	Integer | 	검색된 데이터 수  |
| body.totalCount                     | 	Integer | 	총 데이터 수    |
| body.data[].categoryId              | 	Integer | 	카테고리 ID    |
| body.data[].categoryParentId        | 	Integer | 	부모 카테고리 ID |
| body.data[].depth                   | 	Integer | 	카테고리 깊이    |
| body.data[].sort                    | 	Integer | 	카테고리 정렬 순서 |
| body.data[].categoryName            | 	String  | 	카테고리명      |
| body.data[].categorycategoryDescame | 	String  | 	카테고리 설명    |
| body.data[].useYn                   | 	String  | 	사용 여부      |
| body.data[].createDate              | 	String  | 	등록 날짜      |
| body.data[].createUser              | 	String  | 	등록한 사용자    |
| body.data[].updateDate              | 	String  | 	수정 날짜      |
| body.data[].updateUser              | 	String  | 	수정한 사용자    |

### 카테고리 단건 검색

#### 요청

[URL]

```
GET  /sms/v2.4/appKeys/{appKey}/categories/{categoryId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값          | 	타입      | 	설명      |
|------------|----------|----------|
| appKey     | 	String  | 	고유의 앱키  |
| categoryId | 	Integer | 	카테고리 ID |

#### cURL

```
curl -X GET \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/categories/'"${CATEGORY_ID}" \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

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
        "categoryName": "카테고리",
        "categoryDesc": "최상위 카테고리",
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

| 값                                   | 	타입      | 	설명         |
|-------------------------------------|----------|-------------|
| header.isSuccessful                 | 	Boolean | 	성공 여부      |
| header.resultCode                   | 	Integer | 	실패 코드      |
| header.resultMessage                | 	String  | 	실패 메시지     |
| body.data[].categoryId              | 	Integer | 	카테고리 ID    |
| body.data[].categoryParentId        | 	Integer | 	부모 카테고리 ID |
| body.data[].depth                   | 	Integer | 	카테고리 깊이    |
| body.data[].sort                    | 	Integer | 	카테고리 정렬 순서 |
| body.data[].categoryName            | 	String  | 	카테고리명      |
| body.data[].categorycategoryDescame | 	String  | 	카테고리 설명    |
| body.data[].useYn                   | 	String  | 	사용 여부      |
| body.data[].createDate              | 	String  | 	등록 날짜      |
| body.data[].createUser              | 	String  | 	등록한 사용자    |
| body.data[].updateDate              | 	String  | 	수정 날짜      |
| body.data[].updateUser              | 	String  | 	수정한 사용자    |

### 카테고리 수정

#### 요청

[URL]

```
PUT  /sms/v2.4/appKeys/{appKey}/categories/{categoryId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값          | 	타입      | 	설명      |
|------------|----------|----------|
| appKey     | 	String  | 	고유의 앱키  |
| categoryId | 	Integer | 	카테고리 ID |

[Request body]

```json
{
  "categoryName": "",
  "categoryDesc": "",
  "useYn": "",
  "updateUser": ""
}
```

| 값            | 	타입     | 	최대 길이 | 필수  | 	설명        |
|--------------|---------|--------|-----|------------|
| categoryName | String  | 50     | 필수  | 카테고리 ID    |
| categoryDesc | 	String | 100    | 	옵션 | 	카테고리명     |
| useYn        | 	String | 1      | 	필수 | 사용 여부(Y/N) |
| updateUser   | 	String | 100    | 	옵션 | 수정한 사용자    |

#### cURL

```
curl -X PUT \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/categories/'"${C_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-d '{
    "categoryParentId": 788,
    "categoryName": "secondMMS",
    "categoryDesc": "second category MMS",
    "useYn": "Y",
    "createUser": "467d9790-ea74-11e5-9ad3-005056ac76e8"
}'
```

#### 응답

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": "",
    "resultMessage": ""
  }
}
```

### 카테고리 삭제

#### 요청

[URL]

```
DELETE  /sms/v2.4/appKeys/{appKey}/categories/{categoryId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값          | 	타입      | 	설명      |
|------------|----------|----------|
| appKey     | 	String  | 	고유의 앱키  |
| categoryId | 	Integer | 	카테고리 ID |

#### cURL

```
curl -X DELETE \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/categories/'"${CATEGORY_ID}" \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": "",
    "resultMessage": ""
  }
}
```

## 템플릿

### 템플릿 등록

#### 요청

[URL]

```
POST  /sms/v2.4/appKeys/{appKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

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

| 값                | 	타입           | 	최대 길이 | 필수  | 	설명                          |
|------------------|---------------|--------|-----|------------------------------|
| categoryId       | 	Integer      | 	-     | 필수  | 	카테고리 ID                     |
| templateId       | String        | 50     | 필수  | 템플릿 ID                       |
| templateName     | 	String       | 50     | 	필수 | 	템플릿명                        |
| templateDesc     | 	String       | 100    | 	옵션 | 	템플릿 설명                      |
| sendNo           | String        | 13     | 필수  | 발신 번호                        |
| sendType         | String        | 1      | 필수  | 발송 유형(0:Sms, 1:Lms/Mms)      |
| title            | String        | 120    | 옵션  | 문자 제목(발송 유형이 Lms/MmS인 경우 필수) |
| body             | String        | 4000   | 필수  | 문자 내용                        |
| useYn            | 	String       | 1      | 	필수 | 	사용 여부(Y/N)                  |
| attachFileIdList | List<Integer> | -      | 옵션  | 첨부 파일 ID(fileId)             |

#### cURL

```
curl -X POST \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/templates' \
-H 'Content-Type: application/json;charset=UTF-8' \
-d '{
    "categoryId": 199376,
    "templateId": "TemplateId",
    "templateName": "템플릿 발송 예시",
    "templateDesc": "템플릿 발송 예시",
    "sendNo": "01012341234",
    "sendType": "1",
    "title": "example",
    "body": "일반 발송 테스트용 입니다.\r\n##key1## 님 안녕하세요.\r\n##key2## 입니다.",
    "useYn": "Y"
}'
```

#### 응답

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": "",
    "resultMessage": ""
  }
}
```

#### 템플릿 등록 예시

| Http method | URL                                                                 |
|-------------|---------------------------------------------------------------------|
| POST        | https://api-sms.cloud.toast.com/sms/v2.4/appKeys/{appKey}/templates |

[Request body]

```json
{
  "categoryId": 199376,
  "templateId": "TemplateId",
  "templateName": "템플릿 발송 예시",
  "templateDesc": "템플릿 발송 예시",
  "sendNo": "01012341234",
  "sendType": "1",
  "title": "example",
  "body": "일반 발송 테스트용 입니다.\r\n##key1## 님 안녕하세요.\r\n##key2## 입니다.",
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

- 첨부 파일(필드명: attachFileIdList)을 포함한 템플릿 등록은 사전에 첨부 파일 업로드가 진행되어야 합니다.<br>
- [[첨부 파일 업로드](./api-guide/#binaryUpload)]</a> 가이드를 참고하시기 바랍니다.
- 첨부 이미지 제한 사항
    - 지원 코덱 : jpg
    - 첨부 이미지 개수 : 3개 이하
    - 첨부 이미지 사이즈 : 300K 이하
    - 첨부 이미지 해상도 : 1000 x 1000 이하

### 템플릿 발송(본문 수정이 필요 없는 경우)

| Http method | 종류  | URL                                                                  |
|-------------|-----|----------------------------------------------------------------------|
| POST        | SMS | https://api-sms.cloud.toast.com/sms/v2.4/appKeys/{appKey}/sender/sms |
| POST        | MMS | https://api-sms.cloud.toast.com/sms/v2.4/appKeys/{appKey}/sender/mms |

Request URL은 템플릿 등록시 선택한 발송 유형으로 선택하여 발송합니다.

**Request parameter body 안의 값이 공란이면 해당 templateId의 body 내용으로 치환합니다.**

[Request body] 치환하고자 하는 키와 값을 key,value로 대입.

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

![[그림 1] 템플릿 발송 성공](http://static.toastoven.net/prod_sms/img_27.png)

### 템플릿 발송(본문 수정이 필요한 경우)

#### 템플릿 발송 예제

| Http method | 종류  | URL                                                                  |
|-------------|-----|----------------------------------------------------------------------|
| POST        | SMS | https://api-sms.cloud.toast.com/sms/v2.4/appKeys/{appKey}/sender/sms |
| POST        | MMS | https://api-sms.cloud.toast.com/sms/v2.4/appKeys/{appKey}/sender/mms |

Request URL은 템플릿 등록 시 선택한 발송 유형으로 선택하여 발송합니다.

**템플릿 ID와 Request parameter body 안의 값이 있을 경우, 발신 번호와 발신 내용이 템플릿 내용으로 치환되지 않습니다.**

단, 템플릿 ID를 입력했을 경우, 해당 템플릿으로 검색할 수 있습니다.

위와 같은 경우는 템플릿을 검색한 뒤, 템플릿의 내용을 수정하고 싶을 때 사용할 수 있습니다.

[Request body]

```json
{
  "templateId": "TemplateId",
  "body": "본문",
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

### 템플릿 목록 검색

#### 요청

[URL]

```
GET  /sms/v2.4/appKeys/{appKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

[Query parameter]

| 값          | 	타입      | 	필수 | 	설명              |
|------------|----------|-----|------------------|
| categoryId | 	Integer | 	옵션 | 	카테고리 ID         |
| useYn      | 	String  | 	옵션 | 	사용 여부(Y/N)      |
| pageNum    | 	Integer | 옵션  | 	페이지 번호(기본값 : 1) |
| pageSize   | 	Integer | 옵션  | 	검색 수(기본값 : 15)  |

#### cURL

```
curl -X GET \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/templates' \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

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

| 값                                         | 	타입      | 	설명                              |
|-------------------------------------------|----------|----------------------------------|
| header.isSuccessful                       | 	Boolean | 	성공 여부                           |
| header.resultCode                         | 	Integer | 	실패 코드                           |
| header.resultMessage                      | 	String  | 	실패 메시지                          |
| body.pageNum                              | 	Integer | 	현재 페이지 번호                       |
| body.pageSize                             | 	Integer | 	검색된 데이터 수                       |
| body.totalCount                           | 	Integer | 	총 데이터 수                         |
| body.data[].templateId                    | 	String  | 	템플릿 ID                          |
| body.data[].serviceId                     | 	Integer | 	서비스 ID(내부용, 미사용값)               |
| body.data[].categoryId                    | 	Integer | 	카테고리 ID                         |
| body.data[].categoryName                  | 	String  | 	카테고리명                           |
| body.data[].sort                          | 	Integer | 	정렬값                             |
| body.data[].templateName                  | 	String  | 	템플릿명                            |
| body.data[].templateDesc                  | 	String  | 	템플릿 설명                          |
| body.data[].useYn                         | 	String  | 	사용 여부                           |
| body.data[].priority                      | 	String  | 	우선순위값(미사용값)                     |
| body.data[].sendNo                        | 	String  | 	발신 번호                           |
| body.data[].sendType                      | 	String  | 	발송 유형(0:Sms, 1:Lms/Mms, 2:Auth) |
| body.data[].sendTypeName                  | 	String  | 	발송 유형명                          |
| body.data[].title                         | 	String  | 	제목                              |
| body.data[].body                          | 	String  | 	본문 내용                           |
| body.data[].attachFileYn                  | 	String  | 	첨부 파일 여부(Y/N)                   |
| body.data[].delYn                         | 	String  | 	삭제여부(Y/N), 현재 상태 표기용으로만 사용      |
| body.data[].createDate                    | 	String  | 	등록 날짜                           |
| body.data[].createUser                    | 	String  | 	등록한 사용자                         |
| body.data[].updateDate                    | 	String  | 	수정 날짜                           |
| body.data[].updateUser                    | 	String  | 	수정한 사용자                         |
| body.data[].attachFileList[].fileId       | 	Integer | 	파일 ID                           |
| body.data[].attachFileList[].filePath     | 	String  | 	파일 저장경로(내부용)                    |
| body.data[].attachFileList[].filename     | 	String  | 	파일명                             |
| body.data[].attachFileList[].saveFileName | 	String  | 	저장된 첨부파일명                       |
| body.data[].attachFileList[].uploadType   | 	String  | 	업로드 타입                          |

### 템플릿 단일 검색

#### 요청

[URL]

```
GET  /sms/v2.4/appKeys/{appKey}/templates/{templateId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값          | 	타입     | 	설명     |
|------------|---------|---------|
| appKey     | 	String | 	고유의 앱키 |
| templateId | 	String | 	템플릿 ID |

#### cURL

```
curl -X GET \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/templates/'"${TEMPLATE_ID}" \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

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

| 값                                         | 	타입      | 	설명                              |
|-------------------------------------------|----------|----------------------------------|
| header.isSuccessful                       | 	Boolean | 	성공 여부                           |
| header.resultCode                         | 	Integer | 	실패 코드                           |
| header.resultMessage                      | 	String  | 	실패 메시지                          |
| body.pageNum                              | 	Integer | 	현재 페이지 번호                       |
| body.pageSize                             | 	Integer | 	검색된 데이터 수                       |
| body.totalCount                           | 	Integer | 	총 데이터 수                         |
| body.data.templateId                      | 	String  | 	템플릿 ID                          |
| body.data.serviceId                       | 	Integer | 	서비스 ID(내부용, 미사용값)               |
| body.data.categoryId                      | 	Integer | 	카테고리 ID                         |
| body.data.categoryName                    | 	String  | 	카테고리명                           |
| body.data.sort                            | 	Integer | 	정렬값                             |
| body.data.templateName                    | 	String  | 	템플릿명                            |
| body.data.templateDesc                    | 	String  | 	템플릿 설명                          |
| body.data.useYn                           | 	String  | 	사용 여부                           |
| body.data.priority                        | 	String  | 	우선순위값(미사용값)                     |
| body.data.sendNo                          | 	String  | 	발신 번호                           |
| body.data.sendType                        | 	String  | 	발송 유형(0:Sms, 1:Lms/Mms, 2:Auth) |
| body.data.sendTypeName                    | 	String  | 	발송 유형명                          |
| body.data.title                           | 	String  | 	제목                              |
| body.data.body                            | 	String  | 	본문 내용                           |
| body.data.attachFileYn                    | 	String  | 	첨부 파일 여부(Y/N)                   |
| body.data.delYn                           | 	String  | 	삭제여부(Y/N), 현재 상태 표기용으로만 사용      |
| body.data.createDate                      | 	String  | 	등록 날짜                           |
| body.data.createUser                      | 	String  | 	등록한 사용자                         |
| body.data.updateDate                      | 	String  | 	수정 날짜                           |
| body.data.updateUser                      | 	String  | 	수정한 사용자                         |
| body.data[].attachFileList[].fileId       | 	Integer | 	파일 ID                           |
| body.data[].attachFileList[].filePath     | 	String  | 	파일 저장경로(내부용)                    |
| body.data[].attachFileList[].filename     | 	String  | 	파일명                             |
| body.data[].attachFileList[].saveFileName | 	String  | 	저장된 첨부파일명                       |
| body.data[].attachFileList[].uploadType   | 	String  | 	업로드 타입                          |

### 템플릿 수정

#### 요청

[URL]

```
PUT  /sms/v2.4/appKeys/{appKey}/templates/{templateId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

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

| 값                | 	타입           | 	최대 길이 | 필수  | 	설명                          |
|------------------|---------------|--------|-----|------------------------------|
| templateName     | 	String       | 50     | 	필수 | 	템플릿명                        |
| templateDesc     | 	String       | 100    | 	옵션 | 	템플릿 설명                      |
| sendNo           | String        | 13     | 필수  | 발신 번호                        |
| sendType         | String        | 1      | 필수  | 발송 유형(0:Sms, 1:Lms/Mms)      |
| title            | String        | 120    | 옵션  | 문자 제목(발송 유형이 Lms/MmS인 경우 필수) |
| body             | String        | 4000   | 필수  | 문자 내용                        |
| useYn            | 	String       | 1      | 	필수 | 	사용 여부(Y/N)                  |
| attachFileIdList | List<Integer> | -      | 옵션  | 첨부 파일 ID(fileId)             |

#### cURL

```
curl -X GET \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/templates/'"${TEMPLATE_ID}" \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": "",
    "resultMessage": ""
  }
}
```

### 템플릿 삭제

#### 요청

[URL]

```
DELETE  /sms/v2.4/appKeys/{appKey}/templates/{templateId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값          | 	타입     | 	설명     |
|------------|---------|---------|
| appKey     | 	String | 	고유의 앱키 |
| templateId | 	String | 	템플릿 ID |

#### cURL

```
curl -X DELETE \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/templates/'"${TEMPLATE_ID}" \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": "",
    "resultMessage": ""
  }
}
```

## 080 수신 거부 서비스

### 수신 거부 대상자 등록

#### 요청

[URL]

```
POST /sms/v2.4/appKeys/{appKey}/blockservice/recipients
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

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

| 값               | 	타입          | 	최대 길이 | 필수 | 	설명          |
|-----------------|--------------|--------|----|--------------|
| unsubscribeNo   | String       | 25     | O  | 080 수신거부번호   |
| recipientNoList | List<String> | 10     | O  | 수신 거부 대상자 번호 |

#### cURL

```
curl -X POST \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/blockservice/recipients' \
-H 'Content-Type: application/json;charset=UTF-8' \
-d '{
    "unsubscribeNo": "0800000000",
    "recipientNoList": ["0100000000", "0100000001"]
}'
```

#### 응답

```json
{
  "header": {
    "isSuccessful": true,
    "resultCode": 0,
    "resultMessage": "Success"
  }
}
```

### 수신 거부 대상자 검색

#### 요청

[URL]

```
GET  /sms/v2.4/appKeys/{appKey}/blockservice/recipients
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

[Query parameter]

| 값                | 	타입      | 	최대 길이 | 필수  | 	설명                                |
|------------------|----------|--------|-----|------------------------------------|
| unsubscribeNo    | 	String  | 25     | 	필수 | 	080 수신 거부 번호                      |
| recipientNo      | String   | 25     | 옵션  | 수신 거부 대상자 번호                       |
| startRequestDate | 	String  | -      | 	옵션 | 	수신 거부 요청 시작값(yyyy-MM-dd HH:mm:ss) |
| endRequestDate   | 	String  | -      | 	옵션 | 	수신 거부 요청 종료값(yyyy-MM-dd HH:mm:ss) |
| pageNum          | 	Integer | -      | 	옵션 | 	페이지 번호(기본값 : 1)                   |
| pageSize         | 	Integer | 1000   | 	옵션 | 	검색 수(기본값 : 15)                    |

#### cURL

```
curl -X GET \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/blockservice/recipients' \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

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

### 수신 거부 대상자 삭제

#### 요청

[URL]

```
DELETE  /sms/v2.4/appKeys/{appKey}/blockservice/recipients/removes?unsubscribeNo={unsubscribeNo}&updateUser={updateUser}&recipientNoList={recipientNo},{recipientNo}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

[Query parameter]

| 값             | 	타입     | 	최대 길이 | 필수  | 	설명           |
|---------------|---------|--------|-----|---------------|
| unsubscribeNo | 	String | 20     | 	필수 | 	080 수신 거부 번호 |
| updateUser    | 	String | 	100   | 필수  | 	수신 거부 삭제자    |
| recipientNo   | 	String | 	20    | 필수  | 	삭제할 수신 거부 번호 |

#### cURL

```
curl -X DELETE \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/blockservice/recipients/removes?unsubscribeNo='"${UNSUB_NO}"'&updateUser='"${UPDATE_USER}"'&recipientNoList='"${RECIPIENT_NO}" \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

```json
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

### 등록된 발신 번호 목록 검색 API

#### 요청

[URL]

| Http method | 	URI                                                                                                                      |
|-------------|---------------------------------------------------------------------------------------------------------------------------|
| GET         | 	/sms/v2.4/appKeys/{appKey}/sendNos?sendNo={sendNo}&useYn={useYn}&blockYn={blockYn}&pageNum={pageNum}&pageSize={pageSize} 

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

[Query parameter]

| 값        | 	타입      | 	설명             |
|----------|----------|-----------------|
| sendNo   | String   | 발신 번호           |
| useYn    | String   | 사용 여부           |
| blockYn  | String   | 차단 여부           |
| pageNum  | 	Integer | 페이지 번호(기본값 : 1) |
| pageSize | 	Integer | 검색 수(기본값 : 15)  |

#### cURL

```
curl -X GET \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/sendNos' \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

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

| 값                       | 	타입      | 	설명        |
|-------------------------|----------|------------|
| header.isSuccessful     | 	Boolean | 	성공 여부     |
| header.resultCode       | 	Integer | 	실패 코드     |
| header.resultMessage    | 	String  | 	실패 메시지    |
| body.pageNum            | 	Integer | 	페이지 번호    |
| body.pageSize           | 	Integer | 	검색된 데이터 수 |
| body.totalCount         | 	Integer | 	총 데이터 수   |
| body.data[].serviceId   | Integer  | 서비스 ID     |
| body.data[].sendNo      | String   | 발신 번호      |
| body.data[].useYn       | String   | 사용 여부      |
| body.data[].blockYn     | String   | 차단여부       |
| body.data[].blockReason | String   | 차단 사유      |
| body.data[].createDate  | String   | 생성 일시      |
| body.data[].createUser  | String   | 생성자        |
| body.data[].updateDate  | String   | 수정 날짜      |
| body.data[].updateUser  | String   | 수정한 사용자    |

## 통계

### 통계 검색 - 이벤트 기반

* 이벤트 발생 시간 기준으로 수집된 통계입니다.
* 다음 시간 기준으로 통계가 수집됩니다.
    * 요청 개수(requested): 발송 요청 시간
    * 발송 개수(sent): 통신 사업자(벤더)로 발송 요청한 시간
    * 성공 개수(received): 실제 단말기 수신 시간
    * 실패 개수(sentFailed): 실패 응답이 발생한 시간

#### 요청

[URL]

| Http method | 	URI                              |
|-------------|-----------------------------------|
| GET         | 	/sms/v2.4/appKeys/{appKey}/stats |

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

[Query parameter]

| 값              | 	타입          | 	최대 길이 | 필수                                                                                                                                                             | 설명                                                                 |
|----------------|--------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------|
| statisticsType | String       | -      | 필수                                                                                                                                                             | 통계 구분<br/>NORMAL:기본, MINUTELY:분별, HOURLY:시간별, DAILY:일별, BY_DAY:요일별 |
| from           | String       | -      | 필수                                                                                                                                                             | 통계 검색 시작 날짜<br/>yyyy-MM-dd HH:mm:ss                                | 
| to             | String       | -      | 필수                                                                                                                                                             | 통계 검색 종료 날짜<br/>yyyy-MM-dd HH:mm:ss                                |
| statsIds       | List<String> | -      | 옵션                                                                                                                                                             | 통계 ID 목록                                                           |
| messageType    | String       | -      | 옵션                                                                                                                                                             | 메시지 타입<br/>SMS, LMS, MMS, AUTH                                     |
| isAd           | Boolean      | -      | 옵션                                                                                                                                                             | 광고 여부<br/>true/false                                               |
| templateIds    | List<String> | -      | 옵션                                                                                                                                                             | 템플릿 ID 목록                                                          |
| requestIds     | List<String> | 5      | 옵션                                                                                                                                                             | 요청 ID 목록                                                           |
| statsCriteria  | List<String> | 옵션     | 통계 기준<br/>- EVENT: 이벤트(기본 값)<br/>- TEMPLATE_ID,EVENT: 템플릿, 이벤트<br/>- EXTRA_1,EVENT: 메시지 타입, 이벤트<br/>- EXTRA_2,EVENT: 광고여부, 이벤트<br/>- EXTRA_3,EVENT: 발신 번호, 이벤트 |

#### cURL

```
curl -X GET \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/stats?statisticsType='"${STATISTICS_TYPE}"'&from='"${FROM}"'&to='"${TO}" \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

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

| 값                                                  | 	타입      | 	설명                                                                                                                 |
|----------------------------------------------------|----------|---------------------------------------------------------------------------------------------------------------------|
| header.isSuccessful                                | 	Boolean | 	성공 여부                                                                                                              |
| header.resultCode                                  | 	Integer | 	실패 코드                                                                                                              |
| header.resultMessage                               | 	String  | 	실패 메시지                                                                                                             |
| body.data.eventDateTime                            | 	String  | 	표시 이름<br/>분별, 시간별, 요일별, 월별                                                                                         |
| body.data.events[].{statsCriteriaValue}            | List     | statsCriteria에 해당 하는 값<br/>메시지 타입/광고 유형/발신 번호 값이 올 수 있음<br/>statsCriteria를 EVENT로만 설정한 경우 {statsCriteriaValue}는 생략됨 |
| body.data.events[].{statsCriteriaValue}.requested  | 	Integer | 	요청 개수                                                                                                              |
| body.data.events[].{statsCriteriaValue}.sent       | 	Integer | 	발송 개수                                                                                                              |
| body.data.events[].{statsCriteriaValue}.sentFailed | 	Integer | 	실패 개수                                                                                                              |
| body.data.events[].{statsCriteriaValue}.received   | 	Integer | 	성공 개수                                                                                                              |

### 통계 검색 - 요청 시간 기반

* 발송 요청 시간 기준으로 수집된 통계입니다.
* 다음 시간 기준으로 통계가 수집됩니다.
    * 요청 개수(requested): 발송 요청 시간
    * 발송 개수(sent): 발송 요청 시간으로, 개수가 증가하는 시점은 통신 사업자(밴더)로 발송 요청한 시간
    * 성공 개수(received): 발송 요청 시간으로, 개수가 증가하는 시점은 실제 단말기 수신 시간
    * 실패 개수(sentFailed): 발송 요청 시간으로, 개수가 증가하는 시점은 실패 응답이 발생한 시간

#### 요청

[URL]

| Http method | 	URI                                     |
|-------------|------------------------------------------|
| GET         | 	/sms/v2.4/appKeys/{appKey}/stats/legacy |

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

[Query parameter]

| 값              | 	타입          | 	최대 길이 | 필수                                                                                                                                                             | 설명                                                                 |
|----------------|--------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------|
| statisticsType | String       | -      | 필수                                                                                                                                                             | 통계 구분<br/>NORMAL:기본, MINUTELY:분별, HOURLY:시간별, DAILY:일별, BY_DAY:요일별 |
| from           | String       | -      | 필수                                                                                                                                                             | 통계 검색 시작 날짜<br/>yyyy-MM-dd HH:mm:ss                                | 
| to             | String       | -      | 필수                                                                                                                                                             | 통계 검색 종료 날짜<br/>yyyy-MM-dd HH:mm:ss                                |
| statsIds       | List<String> | -      | 옵션                                                                                                                                                             | 통계 ID 목록                                                           |
| messageType    | String       | -      | 옵션                                                                                                                                                             | 메시지 타입<br/>SMS, LMS, MMS, AUTH                                     |
| isAd           | Boolean      | -      | 옵션                                                                                                                                                             | 광고 여부<br/>true/false                                               |
| templateIds    | List<String> | -      | 옵션                                                                                                                                                             | 템플릿 ID 목록                                                          |
| requestIds     | List<String> | 5      | 옵션                                                                                                                                                             | 요청 ID 목록                                                           |
| statsCriteria  | List<String> | 옵션     | 통계 기준<br/>- EVENT: 이벤트(기본 값)<br/>- TEMPLATE_ID,EVENT: 템플릿, 이벤트<br/>- EXTRA_1,EVENT: 메시지 타입, 이벤트<br/>- EXTRA_2,EVENT: 광고여부, 이벤트<br/>- EXTRA_3,EVENT: 발신 번호, 이벤트 |

#### 응답

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

| 값                                                  | 	타입      | 	설명                                                                                                                 |
|----------------------------------------------------|----------|---------------------------------------------------------------------------------------------------------------------|
| header.isSuccessful                                | 	Boolean | 	성공 여부                                                                                                              |
| header.resultCode                                  | 	Integer | 	실패 코드                                                                                                              |
| header.resultMessage                               | 	String  | 	실패 메시지                                                                                                             |
| body.data.eventDateTime                            | 	String  | 	표시 이름<br/>분별, 시간별, 요일별, 월별                                                                                         |
| body.data.events[].{statsCriteriaValue}            | List     | statsCriteria에 해당 하는 값<br/>메시지 타입/광고 유형/발신 번호 값이 올 수 있음<br/>statsCriteria를 EVENT로만 설정한 경우 {statsCriteriaValue}는 생략됨 |
| body.data.events[].{statsCriteriaValue}.requested  | 	Integer | 	요청 개수                                                                                                              |
| body.data.events[].{statsCriteriaValue}.sent       | 	Integer | 	발송 개수                                                                                                              |
| body.data.events[].{statsCriteriaValue}.sentFailed | 	Integer | 	실패 개수                                                                                                              |
| body.data.events[].{statsCriteriaValue}.received   | 	Integer | 	성공 개수                                                                                                              |
| body.data.events[].{statsCriteriaValue}.pending    | 	Integer | 	발송 중 개수                                                                                                            |

### (구)통합 통계 검색

#### 요청

[URL]

| Http method | 	URI                                                                                                                                                                  |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| GET         | 	/sms/v2.4/appKeys/{appKey}/statistics/view?searchType={searchType}&from={from}&to={to}&messageTypes={messageType}&contentTypes={contentType}&templateId={templateId} |

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

[Query parameter]

| 값           | 	타입    | 	최대 길이 | 필수 | 설명                                             |
|-------------|--------|--------|----|------------------------------------------------|
| searchType  | String | 10     | O  | 통계 구분<br/>DATE:날짜별, TIME:시간별, DAY:요일별          |
| from        | String | -      | O  | 통계 검색 시작 날짜<br/>yyyy-MM-dd HH:mm               |
| to          | String | -      | O  | 통계 검색 종료 날짜<br/>yyyy-MM-dd HH:mm               |
| messageType | String | 10     | X  | 메시지 타입<br/>SMS:단문, LMS:장문, MMS:첨부 파일, AUTH:인증용 |
| contentType | String | 10     | X  | 콘텐츠 타입<br/>NORMAL: 일반, AD: 광고                  |
| templateId  | String | 50     | X  | 템플릿 ID                                         |

#### 응답

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

| 값                          | 타입       | 설명                  |
|----------------------------|----------|---------------------|
| header.isSuccessful        | 	Boolean | 	성공 여부              |
| header.resultCode          | 	Integer | 	실패 코드              |
| header.resultMessage       | 	String  | 	실패 메시지             |
| body.data[].divisionName   | String   | 표시이름<br/>날짜, 시간, 요일 |
| body.data[].statisticsView | Object   |                     |
| body.data[].requestedCount | Integer  | 요청 개수               |
| body.data[].succeedCount   | Integer  | 성공 개수               |
| body.data[].failedCount    | Integer  | 실패 개수               |
| body.data[].pendingCount   | Integer  | 발송 중 개수             |
| body.data[].succeedRate    | String   | 성공 비율               |
| body.data[].failedRate     | String   | 실패 비율               |
| body.data[].pendingRate    | String   | 발송 중 비율             |

## 예약 발송

### 예약 발송 목록 검색

#### 요청

[URL]

```
GET /sms/v2.4/appKeys/{appKey}/reservations
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

[Query parameter]

| 값                | 	타입      | 	최대 길이 | 필수  | 	설명                                                                                                     |
|------------------|----------|--------|-----|---------------------------------------------------------------------------------------------------------|
| sendType         | String   | 1      | 옵션  | 발송 유형<br/>(0:SMS, 1:LMS/MMS, 2:AUTH)                                                                    |
| requestId        | 	String  | 25     | 	옵션 | 	요청 ID                                                                                                  |
| startRequestDate | 	String  | -      | 	옵션 | 	발송 날짜 시작값(yyyy-MM-dd HH:mm:ss)                                                                         |
| endRequestDate   | 	String  | -      | 	옵션 | 	발송 날짜 종료값(yyyy-MM-dd HH:mm:ss)                                                                         |
| startCreateDate  | 	String  | -      | 	옵션 | 	등록 날짜 시작값(yyyy-MM-dd HH:mm:ss)                                                                         |
| endCreateDate    | 	String  | -      | 	옵션 | 	등록 날짜 종료값(yyyy-MM-dd HH:mm:ss)                                                                         |
| sendNo           | 	String  | 13     | 옵션  | 	발신 번호                                                                                                  |
| recipientNo      | 	String  | 20     | 	옵션 | 	수신 번호                                                                                                  |
| templateId       | 	String  | 50     | 	옵션 | 	템플릿 번호                                                                                                 |
| messageStatus    | 	String  | 10     | 	옵션 | 	메시지 상태<br/>(RESERVED:예약 대기, SENDING:발송 중, COMPLETED:발송 완료, FAILED:발송 실패,CANCEL:취소,DUPLICATED:중복 발송) |
| pageNum          | 	Integer | -      | 	옵션 | 	페이지 번호(기본값 : 1)                                                                                        |
| pageSize         | 	Integer | 1000   | 	옵션 | 	검색 수(기본값 : 15)                                                                                         |

#### cURL

```
curl -X GET \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/reservations' \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

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
        "requestId": "{요청 ID}",
        "recipientSeq": 1,
        "requestDate": "{예약 날짜}",
        "sendNo": "{발신 번호}",
        "recipientNo": "{수신 번호}",
        "countryCode": "{국가코드}",
        "sendType": "{발송 유형}",
        "messageType": "{메시지타입}",
        "adYn": "{광고 여부}",
        "templateId": "{템플릿 ID}",
        "templateParameter": "{템플릿 파라미터}",
        "templateName": "{템플릿 명}",
        "title": "{제목}",
        "body": "{내용}",
        "messageStatus": "{메시지 상태}",
        "createUser": "{등록한 사용자}",
        "createDate": "{등록 일시}",
        "updateDate": "{수정 날짜}"
      }
    ]
  }
}
```

| 값                             | 	타입           | 	설명                                                                                                  |
|-------------------------------|---------------|------------------------------------------------------------------------------------------------------|
| header.isSuccessful           | 	Boolean      | 	성공 여부                                                                                               |
| header.resultCode             | 	Integer      | 	실패 코드                                                                                               |
| header.resultMessage          | 	String       | 	실패 메시지                                                                                              |
| body.pageNum                  | 	Integer      | 	현재 페이지 번호                                                                                           |
| body.pageSize                 | 	Integer      | 	검색된 데이터 수                                                                                           |
| body.totalCount               | 	Integer      | 	총 데이터 수                                                                                             |
| body.data[].requestId         | 	String       | 	요청 ID                                                                                               |
| body.data[].recipientSeq      | 	Integer      | 	수신자 시퀀스                                                                                             |
| body.data[].requestDate       | 	String       | 	발신 일시                                                                                               |
| body.data[].sendNo            | 	String       | 	발신 번호                                                                                               |
| body.data[].recipientNo       | 	String       | 	수신 번호                                                                                               |
| body.data[].countryCode       | 	String       | 	국가 번호                                                                                               |
| body.data[].sendType          | 	String       | 	발송 유형(0:Sms, 1:Lms/Mms, 2:Auth)                                                                     |
| body.data[].messageType       | 	String       | 	메시지타입<br/>(SMS,LMS,MMS,AUTH)                                                                        |
| body.data[].adYn              | 	String       | 	광고 여부                                                                                               |
| body.data[].templateId        | 	String       | 	템플릿 ID                                                                                              |
| body.data[].templateParameter | 	String(json) | 	템플릿 파라미터                                                                                            |
| body.data[].templateName      | 	String       | 	템플릿명                                                                                                |
| body.data[].title             | 	String       | 	제목                                                                                                  |
| body.data[].body              | 	String       | 	본문 내용                                                                                               |
| body.data[].messageStatus     | 	String       | 	메시지 상태<br/>(RESERVED:예약 대기,SENDING:발송 중,COMPLETED:발송 완료,FAILED:발송 실패,CANCEL:취소,DUPLICATED:중복 발송) |
| body.data[].createUser        | 	String       | 	등록한 사용자                                                                                             |
| body.data[].createDate        | 	String       | 	등록 날짜                                                                                               |
| body.data[].updateDate        | 	String       | 	수정 날짜                                                                                               |

### 예약 발송 상세 검색

#### 요청

[URL]

```
GET /sms/v2.4/appKeys/{appKey}/reservations/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값            | 	타입      | 	설명      |
|--------------|----------|----------|
| appKey       | 	String  | 	고유의 앱키  |
| requestId    | 	String  | 	요청 ID   |
| recipientSeq | 	Integer | 	수신자 시퀀스 |

#### cURL

```
curl -X GET \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/reservations/'"${R_ID}"'/'"${R_SEQ}" \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

```json
{
  "header": {
    "resultCode": 0,
    "resultMessage": "success",
    "isSuccessful": true
  },
  "body": {
    "data": {
      "requestId": "{요청 ID}",
      "recipientSeq": 1,
      "requestDate": "{예약 날짜}",
      "sendNo": "{발신 번호}",
      "recipientNo": "{수신 번호}",
      "countryCode": "{국가코드}",
      "sendType": "{발송 유형}",
      "messageType": "{메시지타입}",
      "adYn": "{광고 여부}",
      "templateId": "{템플릿 ID}",
      "templateParameter": "{템플릿 파라미터}",
      "templateName": "{템플릿 명}",
      "title": "{제목}",
      "body": "{내용}",
      "messageStatus": "{메시지 상태}",
      "createUser": "{등록한 사용자}",
      "createDate": "{등록 일시}",
      "updateDate": "{수정 날짜}",
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

| 값                                   | 	타입           | 	설명                                                                                                  |
|-------------------------------------|---------------|------------------------------------------------------------------------------------------------------|
| header.isSuccessful                 | 	Boolean      | 	성공 여부                                                                                               |
| header.resultCode                   | 	Integer      | 	실패 코드                                                                                               |
| header.resultMessage                | 	String       | 	실패 메시지                                                                                              |
| body.pageNum                        | 	Integer      | 	현재 페이지 번호                                                                                           |
| body.pageSize                       | 	Integer      | 	검색된 데이터 수                                                                                           |
| body.totalCount                     | 	Integer      | 	총 데이터 수                                                                                             |
| body.data.requestId                 | 	String       | 	요청 ID                                                                                               |
| body.data.recipientSeq              | 	Integer      | 	수신자 시퀀스                                                                                             |
| body.data.requestDate               | 	String       | 	발신 일시                                                                                               |
| body.data.sendNo                    | 	String       | 	발신 번호                                                                                               |
| body.data.recipientNo               | 	String       | 	수신 번호                                                                                               |
| body.data.countryCode               | 	String       | 	국가 번호                                                                                               |
| body.data.sendType                  | 	String       | 	발송 유형(0:Sms, 1:Lms/Mms, 2:Auth)                                                                     |
| body.data.messageType               | 	String       | 	메시지타입<br/>(SMS,LMS,MMS,AUTH)                                                                        |
| body.data.adYn                      | 	String       | 	광고 여부                                                                                               |
| body.data.templateId                | 	String       | 	템플릿 ID                                                                                              |
| body.data.templateParameter         | 	String(json) | 	템플릿 파라미터                                                                                            |
| body.data.templateName              | 	String       | 	템플릿명                                                                                                |
| body.data.title                     | 	String       | 	제목                                                                                                  |
| body.data.body                      | 	String       | 	본문 내용                                                                                               |
| body.data.messageStatus             | 	String       | 	메시지 상태<br/>(RESERVED:예약 대기,SENDING:발송 중,COMPLETED:발송 완료,FAILED:발송 실패,CANCEL:취소,DUPLICATED:중복 발송) |
| body.data.createUser                | 	String       | 	등록한 사용자                                                                                             |
| body.data.createDate                | 	String       | 	등록 날짜                                                                                               |
| body.data.attachFileList[].fileId   | 	Integer      | 	파일 ID                                                                                               |
| body.data.attachFileList[].filePath | 	String       | 	파일경로(내부용)                                                                                           |
| body.data.attachFileList[].fileName | 	String       | 	파일명                                                                                                 |

### 예약 발송 취소

#### 요청

[URL]

```
PUT /sms/v2.4/appKeys/{appKey}/reservations/cancel
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

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

| 값                              | 	타입     | 	최대 길이 | 필수 | 	설명     |
|--------------------------------|---------|--------|----|---------|
| reservationList[].requestId    | String  | 25     | O  | 요청 ID   |
| reservationList[].recipientSeq | Integer | -      | O  | 수신자 시퀀스 |
| updateUser                     | String  | 100    | O  | 취소 요청자  |

#### cURL

```
curl -X PUT \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/reservations/cancel' \
-H 'Content-Type: application/json;charset=UTF-8' \
-d '{
    "reservationList": [{
            "requestId": "1",
            "recipientSeq": 1
        }
    ],
    "updateUser": "API Guide"
}'
```

#### 응답

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

| 값                        | 	타입      | 	설명       |
|--------------------------|----------|-----------|
| header.isSuccessful      | 	Boolean | 	성공 여부    |
| header.resultCode        | 	Integer | 	실패 코드    |
| header.resultMessage     | 	String  | 	실패 메시지   |
| body.data.requestedCount | 	Integer | 	취소 요청 건수 |
| body.data.canceledCount  | 	Integer | 	취소 성공 건수 |

### 예약 발송 취소 - 다중 필터

#### 요청

* 예약 취소 요청은 상태가 '예약 중(RESERVED)'인 경우에만 가능합니다.
* 이미 발송된 메시지는 취소할 수 없습니다.

[URL]

```
PUT /sms/v2.4/appKeys/{appKey}/reservations/search-cancels
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

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

* startRequestDate + endRequestDate 또는 startCreateDate + endCreateDate는 필수입니다.
* 등록 날짜 / 예약 날짜를 동시에 검색하는 경우, 예약 날짜는 무시됩니다.

| 값                                    | 	타입    | 	최대 길이 | 필수 | 	설명                             |
|--------------------------------------|--------|--------|----|---------------------------------|
| searchParameter.sendType             | String | 1      | 필수 | 발송 유형(0:Sms, 1:Lms/Mms, 2:Auth) |
| searchParameter.startRequestDate     | String | -      | 필수 | 예약 날짜 시작                        |
| searchParameter.endRequestDate       | String | -      | 필수 | 예약 날짜 종료                        |
| searchParameter.startCreateDate      | String | -      | 필수 | 등록 날짜 시작                        |
| searchParameter.endCreateDate        | String | -      | 필수 | 등록 날짜 종료                        |
| searchParameter.sendNo               | String | 20     | 옵션 | 발신 번호                           |
| searchParameter.recipientNo          | String | 20     | 옵션 | 수신 번호                           |
| searchParameter.templateId           | String | 50     | 옵션 | 템플릿 ID                          |
| searchParameter.requestId            | String | 25     | 옵션 | 요청 아이디                          |
| searchParameter.createUser           | String | 100    | 옵션 | 예약 발송 생성자                       |
| searchParameter.senderGroupingKey    | String | 100    | 옵션 | 발신자 그룹키                         |
| searchParameter.recipientGroupingKey | String | 100    | 옵션 | 수신자 그룹키                         |
| updateUser                           | String | 100    | 필수 | 예약 취소 요청자                       |

#### cURL

```
curl -X PUT \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/reservations/search-cancels' \
-H 'Content-Type: application/json;charset=UTF-8' \
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

#### 응답

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

| 값                                 | 	타입      | 	설명                                                                                                         |
|-----------------------------------|----------|-------------------------------------------------------------------------------------------------------------|
| header.isSuccessful               | 	Boolean | 	성공 여부                                                                                                      |
| header.resultCode                 | 	Integer | 	실패 코드                                                                                                      |
| header.resultMessage              | 	String  | 	실패 메시지                                                                                                     |
| body.data.reservationCancelId     | 	Integer | 	예약 취소 ID                                                                                                   |
| body.data.requestedDateTime       | 	String  | 	예약 취소 시간(yyyy-MM-dd HH:mm:ss)                                                                              |
| body.data.reservationCancelStatus | 	String  | 	예약 취소 상태<br/>- READY : 예약 준비<br/>- PROCESSING : 예약 취소 중<br/>- COMPLETED : 예약 취소 완료<br/>- FAILED : 예약 취소 실패 |

### 예약 발송 취소 요청 목록 검색 - 다중 필터

#### 요청

[URL]

```
GET /sms/v2.4/appKeys/{appKey}/reservations/search-cancels?startRequestedDateTime={startRequestedDateTime}&endRequestedDateTime={endRequestedDateTime}&reservationCancelId={reservationCancelId}&pageNum={pageNum}&pageSize={pageSize}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

[Query parameter]

| 값                      | 	타입      | 	최대 길이 | 필수  | 	설명                                  |
|------------------------|----------|--------|-----|--------------------------------------|
| startRequestedDateTime | String   | -      | 옵션  | 예약 취소 요청 시작 시간(yyyy-MM-dd HH:mm:ss)  |
| endRequestedDateTime   | 	String  | -      | 	옵션 | 	예약 취소 요청 종료 시간(yyyy-MM-dd HH:mm:ss) |
| reservationCancelId    | 	String  | 25     | 	옵션 | 예약 취소 ID                             |
| pageNum                | 	Integer | -      | 	옵션 | 	페이지 번호(기본값 : 1)                     |
| pageSize               | 	Integer | 1000   | 	옵션 | 	검색 수(기본값 : 15)                      |

#### cURL

```
curl -X GET \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/reservations/search-cancels' \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

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

| 값                                   | 	타입                  | 	설명                                                                                                         |
|-------------------------------------|----------------------|-------------------------------------------------------------------------------------------------------------|
| header.isSuccessful                 | 	Boolean             | 	성공 여부                                                                                                      |
| header.resultCode                   | 	Integer             | 	실패 코드                                                                                                      |
| header.resultMessage                | 	String              | 	실패 메시지                                                                                                     |
| body.data[].reservationCancelId     | 	String              | 	 예약 취소 ID                                                                                                  |
| body.data[].searchParameter         | 	Map<String, Object> | 예약 취소 요청 파라미터                                                                                               |
| body.data[].requestedDateTime       | 	String              | 	예약 취소 요청 시간                                                                                                |
| body.data[].completedDateTime       | 	String              | 	예약 취소 완료 시간                                                                                                |
| body.data[].reservationCancelStatus | 	String              | 	예약 취소 상태<br/>- READY : 예약 준비<br/>- PROCESSING : 예약 취소 중<br/>- COMPLETED : 예약 취소 완료<br/>- FAILED : 예약 취소 실패 |
| body.data[].totalCount              | 	Integer             | 예약 취소 대상 건수                                                                                                 |
| body.data[].successCount            | 	Integer             | 예약 취소 성공 건수                                                                                                 |
| body.data[].createUser              | 	String              | 예약 취소 요청자	                                                                                                  |
| body.data[].createdDateTime         | 	String              | 	예약 취소 요청 생성 시간                                                                                             |
| body.data[].updatedDateTime         | 	String              | 	예약 취소 수정 시간                                                                                                |

## 발송 결과 파일 다운로드

### 검색 파일 생성 요청

#### 요청

[URL]

```
POST /sms/v2.4/appKeys/{appKey}/sender/download-reservations
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

[Request body]

* requestId 또는 startRequestDate + endRequestDate 또는 startCreateDate + endCreateDate는 필수입니다.
* 등록 날짜/발송 날짜를 동시에 검색하는 경우, 발송 날짜는 무시됩니다.

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
  "senderGroupingKey": "{발송자 그룹 키}",
  "recipientGroupingKey": "{수신자 그룹 키}",
  "isIncludeTitleAndBody": true
}
```

| 값                     | 	타입     | 	최대 길이 | 필수  | 	설명                                                    |
|-----------------------|---------|--------|-----|--------------------------------------------------------|
| sendType              | String  | 1      | 필수  | 발송 유형(0:Sms, 1:Lms/Mms, 2:Auth)                        |
| requestId             | 	String | 25     | 	필수 | 	요청 ID                                                 |
| startRequestDate      | 	String | -      | 	필수 | 	발송 날짜 시작값(yyyy-MM-dd HH:mm:ss)                        |
| endRequestDate        | 	String | -      | 	필수 | 	발송 날짜 종룟값(yyyy-MM-dd HH:mm:ss)                        |
| startCreateDate       | 	String | -      | 	필수 | 	등록 날짜 시작값(yyyy-MM-dd HH:mm:ss)                        |
| endCreateDate         | 	String | -      | 	필수 | 	등록 날짜 종룟값(yyyy-MM-dd HH:mm:ss)                        |
| startResultDate       | 	String | -      | 	옵션 | 	수신 날짜 시작값(yyyy-MM-dd HH:mm:ss)                        |
| endResultDate         | 	String | -      | 	옵션 | 	수신 날짜 종룟값(yyyy-MM-dd HH:mm:ss)                        |
| sendNo                | 	String | 13     | 	옵션 | 	발신 번호                                                 |
| recipientNo           | 	String | 20     | 	옵션 | 	수신 번호                                                 |
| templateId            | 	String | 50     | 	옵션 | 	템플릿 번호                                                |
| msgStatus             | 	String | 1      | 	옵션 | 메시지 상태 코드(0: 실패, 1: 요청, 2: 처리 중, 3:성공, 4:예약취소, 5:중복실패) |
| resultCode            | 	String | 10     | 	옵션 | 	수신 결과 코드 [[검색 코드표](./error-code/#_2)]                 |
| subResultCode         | 	String | 10     | 	옵션 | 	수신 결과 상세 코드 [[검색 코드표](./error-code/#_3)]              |
| senderGroupingKey     | 	String | 100    | 	옵션 | 	발송자 그룹 키                                              |
| recipientGroupingKey  | 	String | 100    | 	옵션 | 	수신자 그룹 키                                              |
| isIncludeTitleAndBody | Boolean | -      | 옵션  | 제목, 본문 포함 여부                                           |

#### cURL

```
curl -X POST \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/sender/download-reservations' \
-H 'Content-Type: application/json;charset=UTF-8' \
-d '{
    "sendType": "1",
    "startRequestDate": "2020-08-01T00:00:00",
    "endRequestDate": "2020-08-08T00:00:00"
}'
```

#### 응답

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

| 값                            | 	타입      | 	설명                                                                                                                  |
|------------------------------|----------|----------------------------------------------------------------------------------------------------------------------|
| header.isSuccessful          | 	Boolean | 	성공 여부                                                                                                               |
| header.resultCode            | 	Integer | 	실패 코드                                                                                                               |
| header.resultMessage         | 	String  | 	실패 메시지                                                                                                              |
| body.data.downloadId         | 	String  | 	다운로드 ID                                                                                                             |
| body.data.downloadType       | 	String  | 	다운로드 유형<br/>- BLOCK: 수신거부<br/>- NORMAL: 일반 발송<br/>- MASS: 대량 발송<br/>- TAG: 태그 발송                                    |
| body.data.fileType           | 	String  | 	파일 타입(현재 csv만 지원)                                                                                                   |
| body.data.downloadStatusCode | 	String  | 	파일 생성 상태<br/>- READY: 생성 준비<br/>- MAKING: 생성 중<br/>- COMPLETED: 생성 완료<br/>- FAILED: 생성 실패<br/>- EXPIRED: 다운로드 기간 만료 |
| body.data.expiredDate        | 	String  | 	다운로드 기간 만료 일시                                                                                                       |

### 발송 결과 파일 생성 요청 내역 검색

#### 요청

[URL]

```
GET /sms/v2.4/appKeys/{appKey}/download-reservations
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

[Query parameter]

| 값                  | 	타입      | 최대 길이 | 	필수 | 	설명            |
|--------------------|----------|-------|-----|----------------|
| downloadId         | 	String  | 	25   | 옵션  | 다운로드 아이디       |
| downloadStatusCode | 	String  | 10    | 옵션  | 	다운로드 파일 상태 코드 |
| pageNum            | 	Integer | 	-    | 옵션  | 페이지 번호(기본값: 1) |
| pageSize           | 	Integer | 	1000 | 옵션  | 검색 수(기본값: 15)  |

#### cURL

```
curl -X GET \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/download-reservations' \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

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

| 값                              | 	타입      | 	설명                                                                                                                 |
|--------------------------------|----------|---------------------------------------------------------------------------------------------------------------------|
| header.isSuccessful            | 	Boolean | 	성공 여부                                                                                                              |
| header.resultCode              | 	Integer | 	실패 코드                                                                                                              |
| header.resultMessage           | 	String  | 	실패 메시지                                                                                                             |
| body.totalCount                | Integer  | 전체 건수                                                                                                               |
| body.data[].downloadId         | String   | 다운로드 ID                                                                                                             |
| body.data[].downloadType       | String   | 다운로드 유형<br/>- BLOCK: 수신 거부<br/>- NORMAL: 일반 발송<br/>- MASS: 대량 발송<br/>- TAG: 태그 발송                                   |
| body.data[].fileType           | String   | 파일 타입                                                                                                               |
| body.data[].parameter          | String   | 요청 파라미터                                                                                                             |
| body.data[].size               | Integer  | 검색 데이터 크기                                                                                                           |
| body.data[].downloadStatusCode | String   | 파일 생성 상태<br/>- READY: 생성 준비<br/>- MAKING: 생성 중<br/>- COMPLETED: 생성 완료<br/>- FAILED: 생성 실패<br/>- EXPIRED: 다운로드 기간 만료 |
| body.data[].resultMessage      | String   | 결과 메시지(실패 시 응답)                                                                                                     |
| body.data[].expiredDate        | String   | 파일 만료 일시                                                                                                            |
| body.data[].createUser         | String   | 파일 생성 요청자                                                                                                           |
| body.data[].createDate         | String   | 파일 생성 요청 일시                                                                                                         |
| body.data[].updateDate         | String   | 파일 생성 완료, 실패 일시                                                                                                     |

### 발송 결과 파일 다운로드 요청

#### 요청

[URL]

```
GET /sms/v2.4/appKeys/{appKey}/download-reservations/{downloadId}/download
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값          | 	타입     | 	설명     |
|------------|---------|---------|
| appKey     | 	String | 	고유의 앱키 |
| downloadId | String  | 다운로드 ID |

#### cURL

```
curl -X GET \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/download-reservations/'"${DOWNLOAD_RESERVATION_ID}"'/download' \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

```
file byte
```

## 태그 관리

### 태그 검색

#### 요청

[URL]

```
GET /sms/v2.4/appKeys/{appKey}/tags?pageNum={pageNum}&pageSize={pageSize}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

[Query parameter]

| 값        | 	타입      | 최대 길이 | 	필수 | 	설명            |
|----------|----------|-------|-----|----------------|
| pageNum  | 	Integer | 	-    | 옵션  | 페이지 번호(기본값: 1) |
| pageSize | 	Integer | 	1000 | 옵션  | 검색 수(기본값: 15)  |

#### cURL

```
curl -X GET \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/tags' \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

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

| 값                       | 	타입      | 	설명        |
|-------------------------|----------|------------|
| header.isSuccessful     | 	Boolean | 	성공 여부     |
| header.resultCode       | 	Integer | 	실패 코드     |
| header.resultMessage    | 	String  | 	실패 메시지    |
| body.pageNum            | 	Integer | 	현재 페이지 번호 |
| body.pageSize           | 	Integer | 	검색된 데이터 수 |
| body.totalCount         | 	Integer | 	총 데이터 수   |
| body.data[].tagId       | String   | 태그 ID      |
| body.data[].tagName     | String   | 태그 이름      |
| body.data[].createdDate | String   | 생성 일시      |
| body.data[].tagId       | String   | 수정 일시      |

### 태그 등록

[URL]

```
POST /sms/v2.4/appKeys/{appKey}/tags
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

[Request body]

```json
{
  "tagName": "TAG"
}
```

| 값       | 	타입    | 최대 길이 | 	필수 | 	설명   |
|---------|--------|-------|-----|-------|
| tagName | String | 30    | 필수  | 태그 이름 |

#### cURL

```
curl -X POST \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/tags' \
-H 'Content-Type: application/json;charset=UTF-8' \
-d '{
    "tagName": "API-Guide"
}'
```

#### 응답

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

| 값                    | 	타입      | 	설명     |
|----------------------|----------|---------|
| header.isSuccessful  | 	Boolean | 	성공 여부  |
| header.resultCode    | 	Integer | 	실패 코드  |
| header.resultMessage | 	String  | 	실패 메시지 |
| body.data.tagId      | String   | 태그 ID   |

### 태그 수정

[URL]

```
PUT /sms/v2.4/appKeys/{appKey}/tags/{tagId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |
| tagId  | 	String | 	태그 ID  |

[Request body]

```json
{
  "tagName": "TAG"
}
```

| 값       | 	타입    | 최대 길이 | 	필수 | 	설명   |
|---------|--------|-------|-----|-------|
| tagName | String | 30    | 필수  | 태그 이름 |

#### cURL

```
curl -X PUT \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/tags/'"${TAG_ID}" \
-H 'Content-Type: application/json;charset=UTF-8' \
-d '{
    "tagName": "API-Guide2"
}'
```

#### 응답

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

| 값                    | 	타입      | 	설명     |
|----------------------|----------|---------|
| header.isSuccessful  | 	Boolean | 	성공 여부  |
| header.resultCode    | 	Integer | 	실패 코드  |
| header.resultMessage | 	String  | 	실패 메시지 |

### 태그 삭제

[URL]

```
DELETE /sms/v2.4/appKeys/{appKey}/tags/{tagId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |
| tagId  | 	String | 	태그 ID  |

#### cURL

```
curl -X DELETE \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/tags/'"${TAG_ID}" \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

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

| 값                    | 	타입      | 	설명     |
|----------------------|----------|---------|
| header.isSuccessful  | 	Boolean | 	성공 여부  |
| header.resultCode    | 	Integer | 	실패 코드  |
| header.resultMessage | 	String  | 	실패 메시지 |

## UID 관리

### UID 검색

#### 요청

[URL]

```
GET /sms/v2.4/appKeys/{appKey}/uids?wheres={wheres}&offsetUid={offsetUid}&offset={offset}&limit={limit}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

[Query parameter]

| 값         | 	타입           | 최대 길이 | 	필수 | 	설명                                                                                            |
|-----------|---------------|-------|-----|------------------------------------------------------------------------------------------------|
| wheres    | 	List<String> | 	-    | 옵션  | 검색 조건.<br/>알파뱃, 숫자, 괄호로 이루어진 문자열 배열.<br/>괄호는 1개, AND/OR은 3개까지 사용 가능<br/>(예시) tagId1,AND,tagId2 |
| offsetUid | 	String       | 	-    | 옵션  | 검색을 시작할 uid                                                                                    |
| offset    | Integer       | -     | 옵션  | offset 0(기본값)                                                                                  |
| limit     | Integer       | 1000  | 옵션  | 검색 건수 15(기본값)                                                                                  |

#### cURL

```
curl -X GET \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/uids' \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

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

| 값                                       | 	타입      | 	설명         |
|-----------------------------------------|----------|-------------|
| header.isSuccessful                     | 	Boolean | 	성공 여부      |
| header.resultCode                       | 	Integer | 	실패 코드      |
| header.resultMessage                    | 	String  | 	실패 메시지     |
| body.data.uids[].uid                    | String   | UID         |
| body.data.uids[].tags[].tagId           | String   | 태그 ID       |
| body.data.uids[].tags[].tagName         | String   | 태그 이름       |
| body.data.uids[].tags[].createdDate     | String   | 태그 생성 일시    |
| body.data.uids[].tags[].updatedDate     | String   | 태그 수정 일시    |
| body.data.uids[].contacts[].contactType | String   | 연락처 타입      |
| body.data.uids[].contacts[].contact     | String   | 연락처(휴대폰 번호) |
| body.data.uids[].contacts[].createdDate | String   | 연락처 생성 일시   |
| body.data.uids[].last                   | Boolean  | 마지막 목록 여부   |

### UID 단건 검색

#### 요청

[URL]

```
GET /sms/v2.4/appKeys/{appKey}/uids/{uid}
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |
| uid    | 	String | 	UID    |

#### cURL

```
curl -X GET \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/uids/'"${USER_ID}" \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

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

| 값                                | 	타입      | 	설명         |
|----------------------------------|----------|-------------|
| header.isSuccessful              | 	Boolean | 	성공 여부      |
| header.resultCode                | 	Integer | 	실패 코드      |
| header.resultMessage             | 	String  | 	실패 메시지     |
| body.data.uid                    | String   | UID         |
| body.data.tags[].tagId           | String   | 태그 ID       |
| body.data.tags[].tagName         | String   | 태그 이름       |
| body.data.tags[].createdDate     | String   | 태그 생성 일시    |
| body.data.tags[].updatedDate     | String   | 태그 수정 일시    |
| body.data.contacts[].contactType | String   | 연락처 타입      |
| body.data.contacts[].contact     | String   | 연락처(휴대폰 번호) |
| body.data.contacts[].createdDate | String   | 연락처 생성 일시   |

### UID 등록

[URL]

```
POST /sms/v2.4/appKeys/{appKey}/uids
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |

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

| 값                      | 	타입    | 최대 길이 | 	필수 | 	설명                  |
|------------------------|--------|-------|-----|----------------------|
| uid                    | String | -     | 필수  | UID                  |
| tagIds[]               | String | -     | 필수  | 태그 ID 목록             |
| contacts[].contactType | String | -     | 필수  | 연락처 타입(PHONE_NUMBER) |
| contacts[].contact     | String | -     | 필수  | 연락처(휴대폰 번호)          |

[주의]

* tagIds가 주어지는 경우 contacts는 필수 값이 아닙니다.
* contacts가 주어지는 경우 tagIds는 필수 값이 아닙니다.
* 본 상품의 경우, contactType은 반드시 "PHONE_NUMBER" 값으로 요청해야 합니다.

#### cURL

```
curl -X POST \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/uids/' \
-H 'Content-Type: application/json;charset=UTF-8' \
-d '{
    "uids": [{
            "uid": "USER ID",
            "contacts": [{
                    "contactType": "PHONE_NUMBER",
                    "contact": "0100000000"
                }
            ]
        }
    ]
}'
```

#### 응답

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

| 값                    | 	타입      | 	설명     |
|----------------------|----------|---------|
| header.isSuccessful  | 	Boolean | 	성공 여부  |
| header.resultCode    | 	Integer | 	실패 코드  |
| header.resultMessage | 	String  | 	실패 메시지 |

### UID 삭제

[URL]

```
DELETE /sms/v2.4/appKeys/{appKey}/uids/{uid}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |
| uid    | 	String | 	UID    |

#### cURL

```
curl -X DELETE \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/uids/'"${USER_ID}" \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

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

| 값                    | 	타입      | 	설명     |
|----------------------|----------|---------|
| header.isSuccessful  | 	Boolean | 	성공 여부  |
| header.resultCode    | 	Integer | 	실패 코드  |
| header.resultMessage | 	String  | 	실패 메시지 |

### 휴대폰 번호 등록

[URL]

```
POST /sms/v2.4/appKeys/{appKey}/uids/{uid}/phone-numbers
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값      | 	타입     | 	설명     |
|--------|---------|---------|
| appKey | 	String | 	고유의 앱키 |
| uid    | String  | UID     |

[Request body]

```json
{
  "phoneNumber": "0100000000"
}
```

| 값           | 	타입    | 최대 길이 | 	필수 | 	설명    |
|-------------|--------|-------|-----|--------|
| phoneNumber | String | -     | 필수  | 휴대폰 번호 |

#### cURL

```
curl -X POST \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/uids/'"${USER_ID}/phone-numbers" \
-H 'Content-Type: application/json;charset=UTF-8' \
-d '{
    "phoneNumber": "0100000000"
}'
```

#### 응답

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

| 값                    | 	타입      | 	설명     |
|----------------------|----------|---------|
| header.isSuccessful  | 	Boolean | 	성공 여부  |
| header.resultCode    | 	Integer | 	실패 코드  |
| header.resultMessage | 	String  | 	실패 메시지 |

### 휴대폰 번호 삭제

[URL]

```
DELETE /sms/v2.4/appKeys/{appKey}/uids/{uid}/phone-numbers/{phoneNumber}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

| 값           | 	타입     | 	설명     |
|-------------|---------|---------|
| appKey      | 	String | 	고유의 앱키 |
| uid         | String  | UID     |
| phoneNumber | String  | 휴대폰 번호  |

#### cURL

```
curl -X DELETE \
'https://api-sms.cloud.toast.com/sms/v2.4/appKeys/'"${APP_KEY}"'/uids/'"${USER_ID}"'/phone-numbers/'"${P_NO}" \
-H 'Content-Type: application/json;charset=UTF-8'
```

#### 응답

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

| 값                    | 	타입      | 	설명     |
|----------------------|----------|---------|
| header.isSuccessful  | 	Boolean | 	성공 여부  |
| header.resultCode    | 	Integer | 	실패 코드  |
| header.resultMessage | 	String  | 	실패 메시지 |

## 웹훅

SMS 서비스 내 특정 이벤트가 발생하면 웹훅 설정에 정의된 URL로 POST 요청을 생성합니다.<br>
생성된 POST 요청에 대한 API 문서입니다.

### 웹훅 발송

[URL]

| Http method | 	URI              |
|-------------|-------------------|
| POST        | 웹훅 설정에 정의한 대상 URL |

[Header]

| 값                         | 	타입     | 	설명             |
|---------------------------|---------|-----------------|
| X-Toast-Webhook-Signature | 	String | 	웹훅 설정 시 입력한 서명 |

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

| 값                       | 	타입     | 	설명                                           |
|-------------------------|---------|-----------------------------------------------|
| hooksId                 | 	String | 	웹훅 설정에 정의된 URL로 POST 요청을 할 때마다 고유하게 생성되는 ID  |
| webhookConfigId         | 	String | 웹훅 설정 ID                                      |
| productName             | 	String | 	웹훅 이벤트가 발생한 서비스명                             |
| appKey                  | 	String | 웹훅 이벤트가 발생한 서비스 앱키                            |
| event                   | 	String | 	웹훅 이벤트명<br>* UNSUBSCRIBE: 광고 문자 수신 번호 등록     |
| hooks[].hookId          | 	String | 서비스에서 이벤트 발생 시 생성되는 고유 ID                     |
| hooks[].recipientNo     | 	String | 	수신 거부된 휴대폰 번호                                |
| hooks[].unsubscribeNo   | 	String | 	수신 거부 서비스에 등록된 080 번호                        |
| hooks[].enterpriseName  | 	String | 	수신 거부 서비스에 등록된 업체명                           |
| hooks[].createdDateTime | 	String | 수신 거부 요청 일시<br>* yyyy-MM-dd'T'HH:mm:ss.SSSXXX |

#### cURL

```
curl -X POST \
    '{TargetUrl}' \
    -H 'Content-Type: application/json;charset=UTF-8' \
    -H 'X-Toast-Webhook-Signature: application/json;charset=UTF-8' \
    -d '{
        "hooksId":"202007271010101010sadasdavas",
        "webhookConfigId":"String",
        "productName":"Sms",
        "appKey":"akb3dukdmdjsdSvgk",
        "event":"UNSUBSCRIBE",
        "hooks":[
            {
                "hookId":"202007271010101010sadasdavas",
                "recipientNo":"01012341234",
                "unsubscribeNo":"08012341234",
                "enterpriseName":"NHN Cloud",
                "createdDateTime":"2020-09-09T11:25:10.000+09:00"
            }
        ]
    }
'
```
