## 웹훅

SMS 서비스 내 특정 이벤트가 발생하면 웹훅 설정에 정의된 URL로 POST 요청을 생성합니다.<br>
생성된 POST 요청에 대한 API 문서입니다.

## 웹훅 발송

[URL]

| Http method | URI               |
|-------------|-------------------|
| POST        | 웹훅 설정에 정의한 대상 URL |

[Header]

| 값                         | 타입      | 설명             |
|---------------------------|---------|----------------|
| X-Toast-Webhook-Signature | 	String | 웹훅 설정 시 입력한 서명 |

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
      ...
    }
  ]
}
```

| 값               | 타입        | 설명                                                                                                                             |
|-----------------|-----------|--------------------------------------------------------------------------------------------------------------------------------|
| hooksId         | String    | 웹훅 설정에 정의된 URL로 POST 요청을 할 때마다 고유하게 생성되는 ID                                                                                    |
| webhookConfigId | String    | 웹훅 설정 ID                                                                                                                       |
| productName     | String    | 웹훅 이벤트가 발생한 서비스명                                                                                                               |
| appKey          | String    | 웹훅 이벤트가 발생한 서비스 앱키                                                                                                             |
| event           | String    | 웹훅 이벤트명<br>* UNSUBSCRIBE: 광고 문자 수신 번호 등록<br>* MESSAGE_RESULT_UPDATE: 메시지 발송 결과 코드 업데이트<br>* CONVERSION_BLOCK: 전환율에 의한 차단 국가 발생 |
| hooks           | List<Map> | 웹훅 이벤트 발생 시 데이터<br>* 상세한 내용은 [이벤트 유형별 hooks 정의](./webhook/#hooks)를 참고하세요.                                                      |

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
                ...
            }
        ]
    }
'
```

## 이벤트 유형별 hooks 정의
웹훅 설정에 정의된 URL로 POST 요청을 생성할 때 이벤트 타입별 훅(hook) 데이터입니다.
### 광고 문자 수신 번호 등록
| 값                       | 타입     | 설명                                            |
|-------------------------|--------|-----------------------------------------------|
| hooks[].hookId          | String | 서비스에서 이벤트 발생 시 생성되는 고유 ID                     |
| hooks[].recipientNo     | String | 수신 거부된 휴대폰 번호                                 |
| hooks[].unsubscribeNo   | String | 수신 거부 서비스에 등록된 080 번호                         |
| hooks[].enterpriseName  | String | 수신 거부 서비스에 등록된 업체명                            |
| hooks[].createdDateTime | String | 수신 거부 요청 일시<br>* yyyy-MM-dd'T'HH:mm:ss.SSSXXX |

```json
"hooks": [
  {
    "hookId": "202007271010101010sadasdavas",
    "recipientNo": "01012341234",
    "unsubscribeNo": "08012341234",
    "enterpriseName": "NHN Cloud",
    "createdDateTime": "2020-09-09T11:25:10.000+09:00"    
  }
]
```

### 메시지 발송 결과 코드 업데이트
| 값                       | 타입     | 설명                                            |
|-------------------------|--------|-----------------------------------------------|
| hooks[].hookId          | String | 서비스에서 이벤트 발생 시 생성되는 고유 ID                     |
| hooks[].senderType      | String | 발송 타입                                 |
| hooks[].requestId       | String | 요청 ID                         |
| hooks[].recipientSeq    | Integer | 발송 상세 ID(상세 검색 시 필수)  |
| hooks[].requestDate     | String | 발신 일시<br>* yyyy-MM-dd'T'HH:mm:ss |
| hooks[].receiveDate     | String | 수신 일시<br>* yyyy-MM-dd'T'HH:mm:ss |
| hooks[].sendNo          | String | 발신 번호 |
| hooks[].recipientNo     | String | 수신 번호 |
| hooks[].messageStatus   | String | 메시지 상태 <br>(RESERVED: 예약 대기, SENDING: 발송 중, COMPLETED: 발송 완료, FAILED: 발송 실패, CANCEL: 취소, DUPLICATED: 중복 발송, FAILED_AD: 실패(광고 제한), RESEND_AD: 재발송 대기(광고제한)) |
| hooks[].recipientGroupingKey | String | 수신자 그룹 키 |
| hooks[].senderGroupingKey | String | 발신자 그룹 키 |
| hooks[].resultCode      | String | 결과 코드 |
| hooks[]._links.self.href | String | 메시지 단일 검색 API 링크 | 

```json
"hooks": [
  {
    "hookId": "20240429205809GcSUXthVA00",
    "senderType": "NORMAL_SMS",
    "requestId": "20240429205802y0Tl7Gbz0e0",
    "recipientSeq": 1,
    "requestDate": "2024-04-29T20:58:02",
    "receiveDate": "2024-04-29T20:58:04",
    "sendNo": "15446859",
    "recipientNo": "01012341234",
    "messageStatus": "COMPLETED",
    "recipientGropuingKey": "RecipientGroupingKey",
    "senderGroupingKey": "SenderGroupingKey",
    "resultCode": "1000",
    "_link": {
      "self": {
        "href": "https://api-sms.cloud.toast.com/sms/v2.4/appKeys/{appKey}/sender/sms/20240429205802y0Tl7Gbz0e0?recipientSeq=1"
      }
    },
  }
]
```

### 전환율에 의한 차단 국가 발생
| 값                       | 타입     | 설명                                            |
|-------------------------|--------|-----------------------------------------------|
| hooks[].hookId          | String | 서비스에서 이벤트 발생 시 생성되는 고유 ID                     |
| hooks[].countryCode     | String | 국가 코드                                         |
| hooks[].blockedDateTime | String | 차단 국가 발생 일시<br>* yyyy-MM-dd'T'HH:mm:ss.SSSXXX |

```json
"hooks": [
  {
    "hookId": "20240429205809GcSUXthVA00",
    "countryCode": "NORMAL_SMS",
    "createdDateTime": "2024-05-28T09:00:00.000+09:00"
  }
]
```
