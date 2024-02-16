## Webフック

SMSサービス内で特定イベントが発生すると、Webフック設定に定義されているURLにPOSTリクエストを作成します。<br>
作成されたPOSTリクエストについてのAPI文書です。

### Webフック送信

[URL]

| Http method | URI                |
|-------------|--------------------|
| POST        | Webフック設定に定義した対象URL |

[Header]

| 値                         | タイプ     | 説明               |
|---------------------------|---------|------------------|
| X-Toast-Webhook-Signature | 	String | Webフック設定時に入力した署名 |

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

| 値               | タイプ       | 説明                                                                       |
|-----------------|-----------|--------------------------------------------------------------------------|
| hooksId         | String    | Webフック設定に定義されているURLにPOSTリクエストを行うたびに作成される固有のID                            |
| webhookConfigId | String    | Webフック設定ID                                                               |
| productName     | String    | Webフックイベントが発生したサービス名                                                     |
| appKey          | String    | Webフックイベントが発生したサービスアプリケーションキー                                            |
| event           | String    | Webフックイベント名<br>* UNSUBSCRIBE：広告メッセージ受信番号登録                               |
| hooks           | List<Map> | 웹훅 이벤트 발생 시 데이터<br>* 상세한 내용은 [이벤트 유형별 hooks 정의](./#event-hooks)를 참고해주세요. |

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
#### 広告メッセージ受信番号登録
| 値                       | タイプ    | 説明                                            |
|-------------------------|--------|-----------------------------------------------|
| hooks[].hookId          | String | サービスでイベント発生時に作成される固有ID                        |
| hooks[].recipientNo     | String | 受信拒否された携帯電話番号                                 |
| hooks[].unsubscribeNo   | String | 受信拒否サービスに登録された080番号                           |
| hooks[].enterpriseName  | String | 受信拒否サービスに登録された業者名                             |
| hooks[].createdDateTime | String | 受信拒否リクエスト日時<br>* yyyy-MM-dd'T'HH:mm:ss.SSSXXX |

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
