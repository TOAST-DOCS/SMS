## 웹훅

SMS 서비스 내 특정 이벤트가 발생하면 웹훅 설정에 정의된 URL로 POST 요청을 생성합니다.<br>
생성된 POST 요청에 대한 API 문서입니다.

### 웹훅 발송

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

| 값               | 타입        | 설명                                                                  |
|-----------------|-----------|---------------------------------------------------------------------|
| hooksId         | String    | 웹훅 설정에 정의된 URL로 POST 요청을 할 때마다 고유하게 생성되는 ID                         |
| webhookConfigId | String    | 웹훅 설정 ID                                                            |
| productName     | String    | 웹훅 이벤트가 발생한 서비스명                                                    |
| appKey          | String    | 웹훅 이벤트가 발생한 서비스 앱키                                                  |
| event           | String    | 웹훅 이벤트명<br>* UNSUBSCRIBE: 광고 문자 수신 번호 등록                            |
| hooks           | List<Map> | 웹훅 이벤트 발생 시 데이터<br>* 상세한 내용은 [이벤트 유형별 hooks 정의](./#hooks)를 참고해 주세요. |

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

### 이벤트 유형별 hooks 정의
웹훅 설정에 정의된 URL로 POST 요청을 생성할 때 이벤트 타입별 훅(hook) 데이터입니다.
#### 광고 문자 수신 번호 등록
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
