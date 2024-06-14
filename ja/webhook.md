## Webフック

SMSサービス内で特定イベントが発生すると、Webフック設定に定義されているURLにPOSTリクエストを作成します。<br>
作成されたPOSTリクエストについてのAPI文書です。

## Webフック送信

[URL]

| Http method | URI                |
|-------------|--------------------|
| POST        | Webフック設定に定義した対象URL |

[Header]

| 値                         | タイプ    | 説明               |
|---------------------------|--------|------------------|
| X-Toast-Webhook-Signature | String | Webフック設定時に入力した署名 |

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

| 値             | タイプ      | 説明                                                                                                                           |
|-----------------|-----------|--------------------------------------------------------------------------------------------------------------------------------|
| hooksId         | String    | Webフック設定に定義されているURLにPOSTリクエストを行うたびに作成される固有のID                                                                                    |
| webhookConfigId | String    | Webフック設定ID                                                                                                                       |
| productName     | String    | Webフックイベントが発生したサービス名                                                                                                              |
| appKey          | String    | Webフックイベントが発生したサービスアプリキー                                                                                                           |
| event           | String    | Webフックイベント名<br>* UNSUBSCRIBE:広告文字受信番号登録<br>* MESSAGE_RESULT_UPDATE:メッセージ送信結果コードアップデート<br>* CONVERSION_BLOCK:コンバージョン率によるブロック国発生 |
| hooks           | List<Map> | Webフックイベント発生時のデータ<br>* 詳細は[イベントタイプ別hooks定義](./webhook/#hooks)を参照してください。                                                    |

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

## イベントタイプ別hooks定義
Webフック設定で定義されたURLでPOSTリクエストを作成する時、イベントタイプ別のフック(hook)データです。
### 広告メッセージ受信番号登録
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

### メッセージ送信結果コードアップデート
| 値                      | タイプ    | 説明                                           |
|-------------------------|--------|-----------------------------------------------|
| hooks[].hookId          | String | サービスでイベント発生時に作成される固有ID                     |
| hooks[].senderType      | String | 送信タイプ                                |
| hooks[].requestId       | String | リクエストID                         |
| hooks[].recipientSeq    | String | 送信詳細ID(詳細検索時に必須)  |
| hooks[].requestDate     | String | 送信日時<br>* yyyy-MM-dd'T'HH:mm:ss |
| hooks[].receiveDate     | String | 受信日時<br>* yyyy-MM-dd'T'HH:mm:ss |
| hooks[].sendNo          | String | 発信番号 |
| hooks[].recipientNo     | String | 受信番号 |
| hooks[].messageStatus   | String | メッセージ状態 <br>(RESERVED:予約待機、 SENDING:送信中、 COMPLETED:送信完了、 FAILED:送信失敗、 CANCEL:キャンセル、 DUPLICATED:重複送信、 FAILED_AD:失敗(広告制限), RESEND_AD:再送信待機(広告制限)) |
| hooks[].recipientGroupingKey | String | 受信者グループキー |
| hooks[].senderGroupingKey | String | 送信者グループキー |
| hooks[].resultCode      | String | 結果コード |
| hooks[]._links.self.href | String | メッセージ単一検索APIリンク | 

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

### コンバージョン率によるブロック国発生
| 値                     | タイプ   | 説明                                          |
|-------------------------|--------|-----------------------------------------------|
| hooks[].hookId          | String | サービスでイベント発生時に作成される固有ID                     |
| hooks[].countryCode     | String | 国コード                                       |
| hooks[].blockedDateTime | String | 遮断国家発生日時<br>* yyyy-MM-dd'T'HH:mm:ss.SSSXXX |

```json
"hooks": [
  {
    "hookId": "20240429205809GcSUXthVA00",
    "countryCode": "NORMAL_SMS",
    "createdDateTime": "2024-05-28T09:00:00.000+09:00"
  }
]
```
