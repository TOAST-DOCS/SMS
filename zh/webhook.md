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
                ...
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

### Code Update for Message Sending Result
| Value                       | Type     | Descriptions                                            |
|-------------------------|--------|-----------------------------------------------|
| hooks[].hookId          | String | A unique ID generated when an event occurs in the service                     |
| hooks[].senderType      | String | Send type                                 |
| hooks[].requestId       | String | Request ID                         |
| hooks[].recipientSeq    | Integer | Sending Detail ID (required for detailed search)  |
| hooks[].requestDate     | String | Date and time of sending<br>\* yyyy-MM-dd'T'HH:mm:ss |
| hooks[].receiveDate     | String | Date and time of receiving<br>\* yyyy-MM-dd'T'HH:mm:ss |
| hooks[].sendNo          | String | Sender number |
| hooks[].recipientNo     | String | Recipient number |
| hooks[].messageStatus   | String | Message status <br>(COMPLETED: Completed, FAILED: Failed, CANCEL: Canceled, DUPLICATED: Duplicated, FAILED_AD: Failed (ad restrictions)) |
| hooks[].recipientGroupingKey | String | Recipient group key |
| hooks[].senderGroupingKey | String | Sender group key |
| hooks[].resultCode      | String | Result code |
| hooks[]._links.self.href | String | Message Single Search API link | 

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

### Blocked Countries by Conversion Rate
| Value                       | Type     | Description                                            |
|-------------------------|--------|-----------------------------------------------|
| hooks[].hookId          | String | Original ID created when an event occurs to the service                     |
| hooks[].countryCode     | String | Country code                                         |
| hooks[].blockedDateTime | String | When the country is blocked<br>* yyyy-MM-dd'T'HH:mm:ss.SSSXXX |

```json
"hooks": [
{
"hookId": "20240429205809GcSUXthVA00",
"countryCode": "1",
"blockedDateTime": "2024-05-28T09:00:00.000+09:00"
}
]
```
