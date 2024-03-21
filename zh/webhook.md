## Webhook

When a specific event occurs in the SMS service, a Post request is created to the URL defined in the webhook settings.<br>
This is a document for the API for the created POST request.

### Webhook Delivery

[URL]

| Http method | URI                                        |
|-------------|--------------------------------------------|
| POST        | Target URL defined in the webhook settings |

[Header]

| Value                     | 	Type   | Description                            |
|---------------------------|---------|----------------------------------------|
| X-Toast-Webhook-Signature | 	String | Signature entered when setting webhook |

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

| Value           | Type      | Description                                                                                                    |
|-----------------|-----------|----------------------------------------------------------------------------------------------------------------|
| hooksId         | String    | Unique ID created whenever a POST request is made to the URL defined in the webhook settings                   |
| webhookConfigId | String    | Webhook configuration ID                                                                                       |
| productName     | String    | Service name to which webhook events occur                                                                     |
| appKey          | String    | Service appkey to which webhook events occur                                                                   |
| event           | String    | Webhook event name<br>* UNSUBSCRIBE: Registration of recipient number for ad messages                          |
| hooks           | List<Map> | Data when Webhook event occurs<br>* For more details, see [Hooks Definitions by Event Type](./webhook/#hooks). |

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
                "hookId": "202007271010101010sadasdavas",
                "recipientNo": "01012341234",
                "unsubscribeNo": "08012341234",
                "enterpriseName": "NHN Cloud",
                "createdDateTime": "2020-09-09T11:25:10.000+09:00"
            }
        ]
    }
'
```

### Hooks Definitions by Event Type
Hook data per event type when generating a POST request to the URL defined in the webhook settings.
#### Registration of recipient number for ad messages
| Value                   | Type   | Description                                                               |
|-------------------------|--------|---------------------------------------------------------------------------|
| hooks[].hookId          | String | Unique ID created when an event occurs in the service                     |
| hooks[].recipientNo     | String | Unsubscribed mobile phone number                                          |
| hooks[].unsubscribeNo   | String | 080 numbers registered in unsubscription service                          |
| hooks[].enterpriseName  | String | Enterprise names registered in unsubscription service                     |
| hooks[].createdDateTime | String | Date and time of unsubscription request<br>* yyyy-MM-dd'T'HH:mm:ss.SSSXXX |

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
