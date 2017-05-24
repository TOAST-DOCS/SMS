Notification &gt; SMS &gt; Developer's Guide
--------------------------------------------

Send SMS
--------

### \[API Domain\]

| Environment | Domain                          |
|-------------|---------------------------------|
| Real        | https://api-sms.cloud.toast.com |

Short SMS
---------

### Send Short SMS

#### Request

\[URL\]

| Http method | URI                                   |
|-------------|---------------------------------------|
| POST        | /sms/v1.0/appKeys/{appKey}/sender/sms |

\[Path parameter\]

| Value  | Type   |
|--------|--------|
| appKey | String |

\[Request body\]

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

| Value                      | Type   | Required | Description                                                                                                                                        |
|----------------------------|--------|----------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| templateId                 | String | X        | Send template ID                                                                                                                                   |
| body                       | String | O        | Body content (Limited to 90 Byte based on 'EUC-KR')                                                                                                |
| sendNo                     | String | O        | Sender number                                                                                                                                      |
| recipientList              | List   | O        | Recipient list                                                                                                                                     |
| - recipientNo              | String | O        | Recipient number can be used combining countryCode                                                                                                 |
| - countryCode              | String | X        | Country code (Currently supports national send only)                                                                                               |
| - internationalRecipientNo | String | X        | Recipient number including the country code. ex) In case of 821012345678recipientNo, this value is ignored.(Currently supports national send only) |
| - templateParameter        | Object | X        | Template parameter (In case of entering template ID)                                                                                               |
| -- \#key\#                 | String | X        | Switching key (\#\#key\#\#)                                                                                                                        |
| -- \#value\#               | Object | X        | Mapping value for switching key                                                                                                                    |
| userId                     | String | X        | Send separator ex)admin,system                                                                                                                     |

#### Response

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

| Value           | Type    | Description                                                              |
|-----------------|---------|--------------------------------------------------------------------------|
| header          | Object  | Header area                                                              |
| - isSUccessful  | Boolean | Success or not                                                           |
| - resultCode    | Integer | Failure code                                                             |
| - resultMessage | String  | Failure message                                                          |
| body            | Object  | Body area                                                                |
| - data          | Object  | Data area                                                                |
| -- requestId    | String  | Request ID                                                               |
| -- statusCOde   | String  | Request status code(1:Requesting, 2:Request completed, 3:Request failed) |

#### Example of sending short SMS

\[URL\]

POST
https://api-sms.cloud.toast.com/sms/v1.0/appKeys/{appKey}/sender/sms

\[Request body\] indicate required values as { }.

    {
        "templateId": "",
        "body": "{Body content}",
        "sendNo": "{Send number}",
        "recipientList": [{
            "recipientNo": "{Recipient number}"
        }],
        "userId": "{Requester ID}"
    }

\[Response\]

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

#### Example of sending short SMS with international number

\[URL\]

POST
https://api-sms.cloud.toast.com/sms/v1.0/appKeys/{appKey}/sender/sms

\[Request body\] Indicate required values as { }.

    {
        "templateId": "",
        "body": "{Body content}",
        "sendNo": "{Send number}",
        "recipientList": [
        {
            "internationalRecipientNo": "{Recipient number with country code}"
        },
        {
            "recipientNo":"{Recipient number}",
            "countryCode":"{Country code}"
          }
        ],
        "userId": "{Requester ID}"
    }

\[Response\]

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

### Look Up Short SMS Send List

#### Request

\[URL\]

| Http method | URI                                   |
|-------------|---------------------------------------|
| GET         | /sms/v1.0/appKeys/{appKey}/sender/sms |

\[Path parameter\]

| Value  | Type   | Description   |
|--------|--------|---------------|
| appKey | String | Unique appKey |

\[Query parameter\]

| Value            | Type    | Required | Description                                               |
|------------------|---------|----------|-----------------------------------------------------------|
| requestId        | String  | Required | Request ID                                                |
| startRequestDate | String  | Required | Start send date value(yyyy-MM-dd HH:mm:ss)                |
| endRequestDate   | String  | Required | End send date value(yyyy-MM-dd HH:mm:ss)                  |
| startResultDate  | String  | Optional | Start receive date value(yyyy-MM-dd HH:mm:ss)             |
| endResultDate    | String  | Optional | End receive date value(yyyy-MM-dd HH:mm:ss)               |
| sendNo           | String  | Optional | Sender number                                             |
| recipientNum     | String  | Optional | Recipient number                                          |
| templateId       | String  | Optional | Template number                                           |
| msgStatus        | String  | Optional | Message status code(1:request, 2:processing, 3:succeeded) |
| resultCode       | String  | Optional | Recipient result code [[refer to result code table](./Developer`s Guide_en/#resultcode)]|
| subResultCode    | String  | Optional | Recipient sub result code [[refer to result code table](./Developer`s Guide_en/#subresultcode)]|
| pageNum          | Integer | Optional | Page number(Default : 1)                                  |
| pageSize         | Integer | Optional | Number of lookup(Default : 15)                            |

#### Response

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

| Value              | Type    | Description                                         |
|--------------------|---------|-----------------------------------------------------|
| header             | Object  | Header area                                         |
| - isSUccessful     | Boolean | Success or failure                                  |
| - resultCode       | Integer | Failure code                                        |
| - resultMessage    | String  | Failure message                                     |
| body               | Object  | Body area                                           |
| - pageNum          | Integer | Current page number                                 |
| -pageSize          | Integer | Number of retrieved data                            |
| - totalCount       | Integer | Total number of data                                |
| - data             | List    | Data area                                           |
| -- requestId       | String  | Request ID                                          |
| -- requestDate     | String  | Date sent                                           |
| -- resultDate      | String  | Date received                                       |
| -- templateId      | String  | Template ID                                         |
| -- templateName    | String  | Template name                                       |
| -- categoryId      | String  | Category Id                                         |
| -- categoryName    | String  | Category name                                       |
| -- body            | String  | Body content                                        |
| -- sendNo          | String  | Sender number                                       |
| -- countryCode     | String  | Country code                                        |
| -- recipientNo     | String  | Recipient number                                    |
| -- msgStatus       | String  | Message status code                                 |
| -- msgStatusName   | String  | Message status code name                            |
| -- resultCode      | String  | Received result code [[refer to Received result code](./Developer`s Guide_en/#emma-v3-send-result-code)] |
| -- resultCodeName  | String  | Received result code name                           |
| -- telecomCode     | Integer | Telecommunications company code                     |
| -- telecomCodeName | String  | Telecommunications company name                     |
| -- excelSeq        | Integer | Excel sequence number(Data exists if sent in excel) |
| -- mtPr            | Integer | Send detail ID(Required for detailed lookup)        |
| -- sendType        | String  | Send type(0:SMS, 1:MMS, 2:Auth)                     |
| -- userId          | String  | Send request ID                                     |

### Look Up Short SMS Send Only

#### Request

\[URL\]

| Http method | URI                                               |
|-------------|---------------------------------------------------|
| GET         | /sms/v1.0/appKeys/{appKey}/sender/sms/{requestId} |

\[Path parameter\]

| Value     | Type   | Description   |
|-----------|--------|---------------|
| appKey    | String | Unique appKey |
| requestId | String | Request ID    |

\[Query parameter\]

| Value | Type    | Required | Description    |
|-------|---------|----------|----------------|
| mtPr  | Integer | Required | Send detail ID |

#### Response

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

| Value              | Type    | Description                                         |
|--------------------|---------|-----------------------------------------------------|
| header             | Object  | Header area                                         |
| - isSUccessful     | Boolean | Success or failure                                  |
| - resultCode       | Integer | Failure code                                        |
| - resultMessage    | String  | Failure message                                     |
| body               | Object  | Body area                                           |
| - pageNum          | Integer | Current page number                                 |
| -pageSize          | Integer | Number of retrieved data                            |
| - totalCount       | Integer | Total number of data                                |
| - data             | List    | Data area                                           |
| -- requestId       | String  | Request ID                                          |
| -- requestDate     | String  | Date sent                                           |
| -- resultDate      | String  | Date received                                       |
| -- templateId      | String  | Template ID                                         |
| -- templateName    | String  | Template name                                       |
| -- categoryId      | String  | Category Id                                         |
| -- categoryName    | String  | Category name                                       |
| -- body            | String  | Body content                                        |
| -- sendNo          | String  | Sender number                                       |
| -- countryCode     | String  | Country code                                        |
| -- recipientNo     | String  | Recipient number                                    |
| -- msgStatus       | String  | Message status code                                 |
| -- msgStatusName   | String  | Message status code name                            |
| -- resultCode      | String  | Received result code [[refer to Received result code](./Developer`s Guide_en/#emma-v3-send-result-code)] |
| -- resultCodeName  | String  | Received result code name                           |
| -- telecomCode     | Integer | Telecommunications company code                     |
| -- telecomCodeName | String  | Telecommunications company name                     |
| -- excelSeq        | Integer | Excel sequence number(Data exists if sent in excel) |
| -- mtPr            | Integer | Send detail ID(required for detailed lookup)        |
| -- sendType        | String  | Send type(0:SMS, 1:MMS, 2:Auth)                     |
| -- userId          | String  | Send request ID                                     |

Long MMS
--------

### Sending Long MMS (No Attachment)

#### Request

\[URL\]

| Http method | URI                                   |
|-------------|---------------------------------------|
| POST        | /sms/v1.0/appKeys/{appKey}/sender/mms |

\[Path parameter\]

| Value  | Type   | Description   |
|--------|--------|---------------|
| appKey | String | Unique appKey |

\[Request body\]

    {
      "templateId":String,
      "title":String,
      "body":String,
      "sendNo":String,
      "attachFileIdList":[Integer]
      "recipientList":[{
             "recipientNo":String,
             "countryCode":String,
             "internationalRecipientNo":String,
             "templateParameter":{String:String}
      }],
      "userId":String
    }

| Value                      | Type         | Required | Description                                                                                                                         |
|----------------------------|--------------|----------|-------------------------------------------------------------------------------------------------------------------------------------|
| templateId                 | String       | Optional | Send template ID                                                                                                                    |
| title                      | String       | Optional | Title (Limited to 48Byte based on 'EUC-KR') (Within 40 letters)                                                                     |
| body                       | String       | Required | Body (Limited to 4000Byte based on 'EUC-KR') (Within 4000 letters)                                                                  |
| sendNo                     | String       | Required | Sender number                                                                                                                       |
| attachFileIdList           | List:Integer | Optional | Attachment ID list                                                                                                                  |
| recipientList              | List         | Required | Recipient list                                                                                                                      |
| - recipientNo              | String       | Required | Recipient number can be used combining countryCode                                                                                  |
| - countryCode              | String       | Optional | Country code(Does not support international send)                                                                                   |
| - internationalRecipientNo | String       | X        | Recipient number including the country code ex)821012345678recipientNo, this value is ignored.(Does not support international send) |
| - templateParameter        | Object       | Optional | Template parameter(When entering template ID)                                                                                       |
| -- \#key\#                 | String       | Optional | Switching key(\#\#key\#\#)                                                                                                          |
| -- \#value\#               | Object       | Optional | Mapping value for switching key                                                                                                     |
| userId                     | String       | Optional | Send separator ex)admin,system                                                                                                      |

#### Response

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

| Value           | Type    | Description                                                           |
|-----------------|---------|-----------------------------------------------------------------------|
| header          | Object  | Header area                                                           |
| - isSUccessful  | Boolean | Success or failure                                                    |
| - resultCode    | Integer | Failure code                                                          |
| - resultMessage | String  | Failure message                                                       |
| body            | Object  | Body area                                                             |
| - data          | Object  | Data area                                                             |
| -- requestId    | String  | Request ID                                                            |
| -- statusCOde   | String  | Request status code(1:request, 2:request completed, 3:request failed) |

#### Example of sending long MMS

\[URL\]

POST
https://api-sms.cloud.toast.com/sms/v1.0/appKeys/{appKey}/sender/mms

\[Request body\] Indicate required values as { }.

    {
        "templateId": "",
        "title": "{Title}",
        "body": "{Body content}",
        "sendNo": "{Send number}",
        "recipientList": [{
            "recipientNo": "{Recipient number}",
            "templateParameter": { }
        }],
        "userId": "{Requester ID}"
    }

\[Response\]

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

### Sending Long MMS (With Attachment)

#### Upload attachment

#### Request

\[URL\]

| Http method | URI                                                |
|-------------|----------------------------------------------------|
| POST        | /sms/v1.0/appKeys/{appKey}/attachfile/binaryUpload |

\[Path parameter\]

| Value  | Type   | Description   |
|--------|--------|---------------|
| appKey | String | Unique appKey |

\[Request body\]

    {
        "fileName": String,
        "createUser": String,
        "fileBody": Byte[]
    }

| Value      | Type     | Required | Description                  |
|------------|----------|----------|------------------------------|
| fileName   | String   | Required | File name(jpg and jpeg only) |
| fileBody   | Byte\[\] | Required | File Byte\[\] value          |
| createUser | String   | Required | File upload user information |

#### Response

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

| Value           | Type    | Description                                                         |
|-----------------|---------|---------------------------------------------------------------------|
| header          | Object  | Header area                                                         |
| - isSUccessful  | Boolean | Success or failure                                                  |
| - resultCode    | Integer | Failure code                                                        |
| - resultMessage | String  | Failure message                                                     |
| body            | Object  | Body area                                                           |
| - data          | Object  | Data area                                                           |
| -- fileId       | Integer | File ID                                                             |
| -- fileName     | String  | File name                                                           |
| -- filePath     | String  | Attachment basic Path (https://domain/attachFile/filePath/fileName) |

#### Example of attachment upload

\[URL\]

POST
https://api-sms.cloud.toast.com/sms/v1.0/appKeys/{appKey}/attachfile/binaryUpload

\[Request body\]

    {
      "fileName": "{File name}",
      "createUser": "{Request ID}",
      "fileBody": "{File byte value}"
    }

\[Response\]

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

#### Example of sending attachment

\[URL\]

POST
https://api-sms.cloud.toast.com/sms/v1.0/appKeys/{appKey}/sender/mms

\[Request body\] Indicate required values as { }.

    {
        "templateId": "",
        "title": "{Title}",
        "body": "{Body content}",
        "sendNo": "{Send number}",
        "attachFileIdList": [
            "{fileID of response after uploading files}"
        ],
        "recipientList": [{
            "recipientNo": "{Recipient number}",
            "templateParameter": { }
        }],
        "userId": "{Requester ID}"
    }

\[Response\]

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

### Look Up Long MMS Send List

#### Request

\[URL\]

| Http method | URI                                   |
|-------------|---------------------------------------|
| GET         | /sms/v1.0/appKeys/{appKey}/sender/mms |

\[Path parameter\]

| Value  | Type   |
|--------|--------|
| appKey | String |

\[Query parameter\]

| Value            | Type    | Required | Description                                               |
|------------------|---------|----------|-----------------------------------------------------------|
| requestId        | String  | Required | Request ID                                                |
| startRequestDate | String  | Required | Start send date value(yyyy-MM-dd HH:mm:ss)                |
| endRequestDate   | String  | Required | End send date value(yyyy-MM-dd HH:mm:ss)                  |
| startResultDate  | String  | Optional | Start receive date value(yyyy-MM-dd HH:mm:ss)             |
| endResultDate    | String  | Optional | End receive date value(yyyy-MM-dd HH:mm:ss)               |
| sendNo           | String  | Optional | Sender number                                             |
| recipientNum     | String  | Optional | Recipient number                                          |
| templateId       | String  | Optional | Template number                                           |
| msgStatus        | String  | Optional | Message status code(1:request, 2:processing, 3:succeeded) |
| resultCode       | String  | Optional | Recipient result code [[refer to result code table](./Developer`s Guide_en/#resultcode)]|
| subResultCode    | String  | Optional | Recipient sub result code [[refer to result code table](./Developer`s Guide_en/#subresultcode)]|
| pageNum          | Integer | Optional | Page number(Default : 1)                                  |
| pageSize         | Integer | Optional | Number of lookup(Default : 15)                            |

#### Response

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

| Value              | Type    | Description                                                         |
|--------------------|---------|---------------------------------------------------------------------|
| header             | Object  | Header area                                                         |
| - isSUccessful     | Boolean | Success or failure                                                  |
| - resultCode       | Integer | Failure code                                                        |
| - resultMessage    | String  | Failure message                                                     |
| body               | Object  | Body area                                                           |
| - pageNum          | Integer | Current page number                                                 |
| -pageSize          | Integer | Number of retrieved data                                            |
| - totalCount       | Integer | Total number of data                                                |
| - data             | List    | Data area                                                           |
| -- requestId       | String  | Request ID                                                          |
| -- requestDate     | String  | Date sent                                                           |
| -- resultDate      | String  | Date received                                                       |
| -- templateId      | String  | Template ID                                                         |
| -- templateName    | String  | Template name                                                       |
| -- categoryId      | String  | Category ID                                                         |
| -- categoryName    | String  | Category name                                                       |
| -- title           | String  | Title                                                               |
| -- body            | String  | Body content                                                        |
| -- sendNo          | String  | Sender number                                                       |
| -- countryCode     | String  | Country code                                                        |
| -- recipientNo     | String  | Recipient number                                                    |
| -- msgStatus       | String  | Message status code                                                 |
| -- msgStatusName   | String  | Message status code name                                            |
| -- resultCode      | String  | Received result code [[refer to Received result code](./Developer`s Guide_en/#emma-v3-send-result-code)] |
| -- resultCodeName  | String  | Received result code name                                           |
| -- telecomCode     | Integer | Telecommunications company code                                     |
| -- telecomCodeName | String  | Telecommunications company name                                     |
| -- excelSeq        | Integer | Excel sequence number(data exists if sent in excel)                 |
| -- mtPr            | Integer | Send detail ID(required for detailed lookup)                        |
| -- sendType        | String  | Send type(0:SMS, 1:MMS, 2:Auth)                                     |
| -- userId          | String  | Send request ID                                                     |
| -- attachFileList  | List    | Attachment list                                                     |
| --- filePath       | String  | Attachment basic Path (https://domain/attachFile/filePath/fileName) |
| --- filename       | String  | Attachment name                                                     |

### Look up Long MMS Send Only

#### Request

\[URL\]

| Http method | URI                                               |
|-------------|---------------------------------------------------|
| GET         | /sms/v1.0/appKeys/{appKey}/sender/mms/{requestId} |

\[Path parameter\]

| Value     | Type   | Description   |
|-----------|--------|---------------|
| appKey    | String | Unique appKey |
| requestId | String | Request ID    |

\[Query parameter\]

| Value | Type    | Required | Description    |
|-------|---------|----------|----------------|
| mtPr  | Integer | Required | Send detail ID |

#### Response

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

| Value              | Type    | Description                                                         |
|--------------------|---------|---------------------------------------------------------------------|
| header             | Object  | Header area                                                         |
| - isSUccessful     | Boolean | Success or failure                                                  |
| - resultCode       | Integer | Failure code                                                        |
| - resultMessage    | String  | Failure message                                                     |
| body               | Object  | Body area                                                           |
| - pageNum          | Integer | Current page number                                                 |
| -pageSize          | Integer | Number of retrieved data                                            |
| - totalCount       | Integer | Total number of data                                                |
| - data             | List    | Data area                                                           |
| -- requestId       | String  | Request ID                                                          |
| -- requestDate     | String  | Date sent                                                           |
| -- resultDate      | String  | Date received                                                       |
| -- templateId      | String  | Template ID                                                         |
| -- templateName    | String  | Template name                                                       |
| -- categoryId      | String  | Category ID                                                         |
| -- categoryName    | String  | Category name                                                       |
| -- title           | String  | Title                                                               |
| -- body            | String  | Body content                                                        |
| -- sendNo          | String  | Sender number                                                       |
| -- countryCode     | String  | Country code                                                        |
| -- recipientNo     | String  | Recipient number                                                    |
| -- msgStatus       | String  | Message status code                                                 |
| -- msgStatusName   | String  | Message status code name                                            |
| -- resultCode      | String  | Received result code [[refer to Received result code](./Developer`s Guide_en/#emma-v3-send-result-code)] |
| -- resultCodeName  | String  | Received result code name                                           |
| -- telecomCode     | Integer | Telecommunications company code                                     |
| -- telecomCodeName | String  | Telecommunications company name                                     |
| -- excelSeq        | Integer | Excel sequence number(data exists if sent in excel)                 |
| -- mtPr            | Integer | Send detail ID(required for detailed lookup)                        |
| -- sendType        | String  | Send type(0:SMS, 1:MMS, 2:Auth)                                     |
| -- userId          | String  | Send request ID                                                     |
| -- attachFileList  | List    | Attachment list                                                     |
| --- filePath       | String  | Attachment basic Path (https://domain/attachFile/filePath/fileName) |
| --- filename       | String  | Attachment name                                                     |

SMS for Authority
-----------------

### Send SMS for Authority

#### Request

\[URL\]

| Http method | URI                                        |
|-------------|--------------------------------------------|
| POST        | /sms/v1.0/appKeys/{appKey}/sender/auth/sms |

\[Path parameter\]

| Value  | Type   | Description   |
|--------|--------|---------------|
| appKey | String | Unique appKey |

\[Request body\]

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

| Value                      | Type   | Required | Description                                                                                                                           |
|----------------------------|--------|----------|---------------------------------------------------------------------------------------------------------------------------------------|
| templateId                 | String | X        | Send template ID                                                                                                                      |
| body                       | String | O        | Body content(Limited to 90 Byte based on 'EUC-KR')                                                                                    |
| sendNo                     | String | O        | Sender number                                                                                                                         |
| recipientList              | List   | O        | Recipient list                                                                                                                        |
| - recipientNo              | String | O        | Recipient number can be used combining countryCode                                                                                    |
| - countryCode              | String | X        | Country code(Currently supports national send only)                                                                                   |
| - internationalRecipientNo | String | X        | Recipient number including the country code ex)821012345678recipientNo, this value is ignored.(Currently supports national send only) |
| - templateParameter        | Object | X        | Template parameter(When entering template ID)                                                                                         |
| -- \#key\#                 | String | X        | Switching key(\#\#key\#\#)                                                                                                            |
| -- \#value\#               | Object | X        | Mapping value for switching key                                                                                                       |
| userId                     | String | X        | Send separator ex)admin,system                                                                                                        |

\*\* ※ It will not be sent if countryCode is blank value so please remove from body or apply 82 which is Korea country code. \*\*

#### Response

\`\`

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

| Value           | Type    | Description                                                              |
|-----------------|---------|--------------------------------------------------------------------------|
| header          | Object  | Header area                                                              |
| - isSUccessful  | Boolean | Success or failure                                                       |
| - resultCode    | Integer | Failure code                                                             |
| - resultMessage | String  | Failure message                                                          |
| body            | Object  | Body area                                                                |
| - data          | Object  | Data area                                                                |
| -- requestId    | String  | Request ID                                                               |
| -- statusCOde   | String  | Request status code(1:requesting, 2:request completed, 3:request failed) |

#### Example

\[URL\]

POST
https://api-sms.cloud.toast.com/sms/v1.0/appKeys/{appKey}/sender/auth/sms

\[Request body\] Indicate required values as { }.

    {
        "templateId": "",
        "body": "{Body content}",
        "sendNo": "{Send number}",
        "recipientList": [{
            "recipientNo": "{Recipient number}",
            "templateParameter": { }
        }],
        "userId": "{Requester ID}"
    }

\[Response\]

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

### Look Up SMS For Authority Send List

#### Request

\[URL\]

| Http method | URI                                        |
|-------------|--------------------------------------------|
| GET         | /sms/v1.0/appKeys/{appKey}/sender/auth/sms |

\[Path parameter\]

| Value  | Type   | Description   |
|--------|--------|---------------|
| appKey | String | Unique appKey |

\[Query parameter\]

| Value            | Type    | Required | Description                                              |
|------------------|---------|----------|----------------------------------------------------------|
| requestId        | String  | Required | Request ID                                               |
| startRequestDate | String  | Required | Start send date value(yyyy-MM-dd HH:mm:ss)               |
| endRequestDate   | String  | Required | End send date value(yyyy-MM-dd HH:mm:ss)                 |
| startResultDate  | String  | Optional | Start receive date value(yyyy-MM-dd HH:mm:ss)            |
| endResultDate    | String  | Optional | Send receive date value(yyyy-MM-dd HH:mm:ss)             |
| sendNo           | String  | Optional | Sender number                                            |
| recipientNum     | String  | Optional | Recipient number                                         |
| templateId       | String  | Optional | Template number                                          |
| msgStatus        | String  | Optional | Message status code(1:Request, 2:Processing, 3:Suceeded) |
| resultCode       | String  | Optional | Recipient result code [[refer to result code table](./Developer`s Guide_en/#resultcode)]|
| subResultCode    | String  | Optional | Recipient sub result code [[refer to result code table](./Developer`s Guide_en/#subresultcode)]|
| pageNum          | Integer | Optional | Page number(Default : 1)                                 |
| pageSize         | Integer | Optional | Number of lookup(Default : 15)                           |

#### Response

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

| Value              | Type    | Description                                         |
|--------------------|---------|-----------------------------------------------------|
| header             | Object  | Header area                                         |
| - isSUccessful     | Boolean | Success or failure                                  |
| - resultCode       | Integer | Failure code                                        |
| - resultMessage    | String  | Failure message                                     |
| body               | Object  | Body area                                           |
| - pageNum          | Integer | Current page number                                 |
| -pageSize          | Integer | Number of retrieved data                            |
| - totalCount       | Integer | Total number of data                                |
| - data             | List    | Data area                                           |
| -- requestId       | String  | Request ID                                          |
| -- requestDate     | String  | Date sent                                           |
| -- resultDate      | String  | Date received                                       |
| -- templateId      | String  | Template ID                                         |
| -- templateName    | String  | Template name                                       |
| -- categoryId      | String  | Category ID                                         |
| -- categoryName    | String  | Category name                                       |
| -- body            | String  | Body content                                        |
| -- sendNo          | String  | Send number                                         |
| -- countryCode     | String  | Country code                                        |
| -- recipientNo     | String  | Recipient number                                    |
| -- msgStatus       | String  | Message status code                                 |
| -- msgStatusName   | String  | Message status code name                            |
| -- resultCode      | String  | Received result code [[refer to Received result code](./Developer`s Guide_en/#emma-v3-send-result-code)] |
| -- resultCodeName  | String  | Receive result code name                            |
| -- telecomCode     | Integer | Telecommunications company code                     |
| -- telecomCodeName | String  | Telecommunications company name                     |
| -- excelSeq        | Integer | Excel sequence number(data exists if sent in excel) |
| -- mtPr            | Integer | Send detail ID(required for detailed lookup)        |
| -- sendType        | String  | Send type(0:SMS, 1:MMS, 2:Auth)                     |
| -- userId          | String  | Send request ID                                     |

### Look up SMS for Authority Send Only

#### Request

\[URL\]

| Http method | URI                                                    |
|-------------|--------------------------------------------------------|
| GET         | /sms/v1.0/appKeys/{appKey}/sender/auth/sms/{requestId} |

\[Path parameter\]

| Value     | Type   | Description   |     |
|-----------|--------|---------------|-----|
| appKey    | String | Unique appKey |     |
| requestId | String | Request ID    |     |

\[Query parameter\]

| Value | Type    | Required | Description    |
|-------|---------|----------|----------------|
| mtPr  | Integer | Required | Send detail ID |

#### Response

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

| Value              | Type    | Description                                         |
|--------------------|---------|-----------------------------------------------------|
| header             | Object  | Header area                                         |
| - isSUccessful     | Boolean | Success or failure                                  |
| - resultCode       | Integer | Failure code                                        |
| - resultMessage    | String  | Failure message                                     |
| body               | Object  | Body area                                           |
| - pageNum          | Integer | Current page number                                 |
| -pageSize          | Integer | Number of retrieved data                            |
| - totalCount       | Integer | Total number of data                                |
| - data             | List    | Data area                                           |
| -- requestId       | String  | Request ID                                          |
| -- requestDate     | String  | Date sent                                           |
| -- resultDate      | String  | Date received                                       |
| -- templateId      | String  | Template ID                                         |
| -- templateName    | String  | Template name                                       |
| -- categoryId      | String  | Category ID                                         |
| -- categoryName    | String  | Category name                                       |
| -- body            | String  | Body content                                        |
| -- sendNo          | String  | Sender number                                       |
| -- countryCode     | String  | Country code                                        |
| -- recipientNo     | String  | Recipient number                                    |
| -- msgStatus       | String  | Message status code                                 |
| -- msgStatusName   | String  | Message status code name                            |
| -- resultCode      | String  | Received result code [[refer to Received result code](./Developer`s Guide_en/#emma-v3-send-result-code)] |
| -- resultCodeName  | String  | Receive result code name                            |
| -- telecomCode     | Integer | Telecommunications company code                     |
| -- telecomCodeName | String  | Telecommunications company name                     |
| -- excelSeq        | Integer | Excel sequence number(data exists if sent in excel) |
| -- mtPr            | Integer | Send detail ID(Required for detailed lookup)        |
| -- sendType        | String  | Send type(0:SMS, 1:MMS, 2:Auth)                     |
| -- userId          | String  | Send request ID                                     |

Template
--------

### Send Template

#### Example

\[URL\]

POST
https://api-sms.cloud.toast.com/sms/v1.0/appKeys/{appKey}/sender/sms

It can apply to all SMS,MMS and for authority.

\[Request body\] Substitute keys and values to switch for key,value.

    {
        "templateId": "{Template ID}",
        "body": "{Body content}",
        "sendNo": "{Send number}",
        "recipientList": [{
            "recipientNo": "{Recipient number}",
            "templateParameter": {
              "key1" : "value1",
              "key2" : "value2"
            }
        }],
        "userId": "{Requester ID}"
    }

\[Response\]

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

### Look up Template List

#### Request

\[URL\]

| Http method | URI                                  |
|-------------|--------------------------------------|
| GET         | /sms/v1.0/appKeys/{appKey}/templates |

\[Path parameter\]

| Value  | Type   | Description   |
|--------|--------|---------------|
| appKey | String | Unique appKey |

\[Query parameter\]

| Value      | Type    | Required | Description                    |
|------------|---------|----------|--------------------------------|
| categoryId | Integer | Optional | Category ID                    |
| useYn      | String  | Optional | To use or not(Y/N)             |
| pageNum    | Integer | Optional | Page number(Default : 1)       |
| pageSize   | Integer | Optional | Number of lookup(Default : 15) |
| all        | Boolean | Optional | To retrieve all or not         |

#### Response

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
                    "attachType": Integer,
                    "templateId": String,
                    "filePath": String,
                    "fileName": String,
                    "createDate": String,
                    "createUser": String
                }]
            }]
        }
    }

| Value             | Type    | Description                                               |
|-------------------|---------|-----------------------------------------------------------|
| header            | Object  | Header area                                               |
| - isSUccessful    | Boolean | Success or failure                                        |
| - resultCode      | Integer | Failure code                                              |
| - resultMessage   | String  | Failure message                                           |
| body              | Object  | Body area                                                 |
| - pageNum         | Integer | Current page number                                       |
| - pageSize        | Integer | Number of retrieved data                                  |
| - totalCount      | Integer | Total number of data                                      |
| - data            | List    | Data area                                                 |
| -- templateId     | String  | Template ID                                               |
| -- serviceId      | Integer | Service ID(For internal use, unused value)                |
| -- categoryId     | Integer | Category ID                                               |
| -- categoryName   | String  | Category name                                             |
| -- sort           | Integer | Sort value                                                |
| -- templateName   | String  | Template name                                             |
| -- templateDesc   | String  | Template description                                      |
| -- useYn          | String  | To use or not                                             |
| -- priority       | String  | Priority value(Unused value)                              |
| -- sendNo         | String  | Sender number                                             |
| -- sendType       | String  | Send type(0:Sms, 1:mms, 2:auth)                           |
| -- sendTypeName   | String  | Send type name                                            |
| -- title          | String  | Title                                                     |
| -- body           | String  | Body content                                              |
| -- attachFileYn   | String  | To attach a file or not(Y/N)                              |
| -- delYn”         | String  | To delete or not(Y/N), For current status indication only |
| -- attachFileList | List    | Attachment list                                           |
| --- fileId        | Integer | Attachment ID                                             |
| --- attachType    | Integer | Attachment upload type(0:Temporary,1:upload,2:template)   |
| --- templateId    | String  | Template ID                                               |
| --- filePath      | String  | Attachment path                                           |
| --- fileName      | String  | Attachment name                                           |
| --- createDate    | String  | Attachment registered date                                |
| --- createUser    | String  | Attachment registered user                                |
| -- createDate     | String  | Registered date                                           |
| -- createUser     | String  | Registered user                                           |
| -- updateDate     | String  | Updated date                                              |
| -- updateUser     | String  | Updated user                                              |

### Look up Template Only

#### Request

\[URL\]

| Http method | URI                                               |
|-------------|---------------------------------------------------|
| GET         | /sms/v1.0/appKeys/{appKey}/templates/{templateId} |

\[Path parameter\]

| Value      | Type   | Description   |
|------------|--------|---------------|
| appKey     | String | Unique appKey |
| templateId | String | Template ID   |

#### Response

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
                    "attachType": Integer,
                    "templateId": String,
                    "filePath": String,
                    "fileName": String,
                    "createDate": String,
                    "createUser": String
                }]
            }]
        }
    }

| Value             | Type    | Description                                               |
|-------------------|---------|-----------------------------------------------------------|
| header            | Object  | Header area                                               |
| - isSUccessful    | Boolean | Success or failure                                        |
| - resultCode      | Integer | Failure code                                              |
| - resultMessage   | String  | Failure message                                           |
| body              | Object  | Body area                                                 |
| - pageNum         | Integer | Current page number                                       |
| - pageSize        | Integer | Number of retrieved data                                  |
| - totalCount      | Integer | Total number of data                                      |
| - data            | List    | Data area                                                 |
| -- templateId     | String  | Template ID                                               |
| -- serviceId      | Integer | Service ID(For internal use, unused value)                |
| -- categoryId     | Integer | Category ID                                               |
| -- categoryName   | String  | Category name                                             |
| -- sort           | Integer | Sort value                                                |
| -- templateName   | String  | Template name                                             |
| -- templateDesc   | String  | Template description                                      |
| -- useYn          | String  | To use or not                                             |
| -- priority       | String  | Priority value(Unused value)                              |
| -- sendNo         | String  | Send number                                               |
| -- sendType       | String  | Send type(0:Sms, 1:mms, 2:auth)                           |
| -- sendTypeName   | String  | Send type name                                            |
| -- title          | String  | Title                                                     |
| -- body           | String  | Body content                                              |
| -- attachFileYn   | String  | To attach a file or not(Y/N)                              |
| -- delYn”         | String  | To delete or not(Y/N), For current status indication only |
| -- attachFileList | List    | Attachment list                                           |
| --- fileId        | Integer | Attachment ID                                             |
| --- attachType    | Integer | Attachment upload type(0:Temporary,1:upload,2:template)   |
| --- templateId    | String  | Template ID                                               |
| --- filePath      | String  | Attachment path                                           |
| --- fileName      | String  | Attachment name                                           |
| --- createDate    | String  | Attachment registered date                                |
| --- createUser    | String  | Attachment registered user                                |
| -- createDate     | String  | Registered date                                           |
| -- createUser     | String  | Registered user                                           |
| -- updateDate     | String  | Updated date                                              |
| -- updateUser     | String  | Updated user                                              |

## Look Up Send List ResultCode
### resultCode
<table class="table table-striped table-hover">
<thead>
	<tr>
    <th>Code value</th>
    <th>Meaning</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>MTR1</td>
    <td>Succeeded</td>
  </tr>
  <tr>
    <td>MTR2</td>
    <td>Failure</td>
  </tr>
</tbody>
</table>

### subResultCode
<table class="table table-striped table-hover">
<thead>
	<tr>
    <th>Code value</th>
    <th>Meaning</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>MTR2_1</td>
    <td>Fail to valid check</td>
  </tr>
  <tr>
    <td>MTR2_2</td>
    <td>Telecommunications company problem</td>
  </tr>
  <tr>
    <td>MTR2_3</td>
    <td>Mobile device problem</td>
  </tr>
</tbody>
</table>

EMMA v.3 Send Result Code
-------------------------

-   EMMA Version : EMMA V3.3.0 or higher

    1.  Telecommunications company: Result code after sending telecommunications company.

    <!-- -->

    1.  IB G/W : Result code after Infobank G/W receives a message.

    <!-- -->

    1.  IB EMMA : error code which EMMA processed for message send request.

<!-- -->

    <tr>
        <th>Division</th>
        <th>Code value</th>
        <th>Classification</th>
        <th>Meaning</th>
    </tr>

    <tr>
        <td rowspan=29>Telecommunications company</td>
        <td>1000</td>
        <td>success</td>
        <td>Succeeded</td>
    </tr>
    <tr>
        <td>2000</td>
        <td>failure</td>
        <td>Exceeded transmitting time</td>
    </tr>
    <tr>
        <td>2001</td>
        <td>failure</td>
        <td>Failed to send (Wireless network)</td>
    </tr>
    <tr>
        <td>2002</td>
        <td>failure</td>
        <td>Failed to send (Wireless network -> Device)</td>
    </tr>
    <tr>
        <td>2003</td>
        <td>failure</td>
        <td>Device is off</td>
    </tr>
    <tr>
        <td>2004</td>
        <td>failure</td>
        <td>Device message buffer full</td>
    </tr>
    <tr>
        <td>2005</td>
        <td>failure</td>
        <td>Shading area</td>
    </tr>
    <tr>
        <td>2006</td>
        <td>failure</td>
        <td>Message is deleted</td>
    </tr>
    <tr>
        <td>2007</td>
        <td>failure</td>
        <td>Temporary device problem</td>
    </tr>
    <tr>
        <td>3000</td>
        <td>Invalid</td>
        <td>Unable to send</td>
    </tr>
    <tr>
        <td>3001</td>
        <td>Invalid</td>
        <td>No member</td>
    </tr>
    <tr>
        <td>3002</td>
        <td>Invalid</td>
        <td>Adult verification failed</td>
    </tr>
    <tr>
        <td>3003</td>
        <td>Invalid</td>
        <td>Recipient number format error</td>
    </tr>
    <tr>
        <td>3004</td>
        <td>Invalid</td>
        <td>Device service temporarily stopped</td>
    </tr>
    <tr>
        <td>3005</td>
        <td>Invalid</td>
        <td>Device call process status</td>
    </tr>
    <tr>
        <td>3006</td>
        <td>Invalid</td>
        <td>Incoming rejected</td>
    </tr>
    <tr>
        <td>3007</td>
        <td>Invalid</td>
        <td>Phone cannot receive Callback URL</td>
    </tr>
    <tr>
        <td>3008</td>
        <td>Invalid</td>
        <td>Other device problem</td>
    </tr>
    <tr>
        <td>3009</td>
        <td>Invalid</td>
        <td>Message format error</td>
    </tr>
    <tr>
        <td>3010</td>
        <td>Invalid</td>
        <td>Device does not support MMS</td>
    </tr>
    <tr>
        <td>3011</td>
        <td>Invalid</td>
        <td>Server error</td>
    </tr>
    <tr>
        <td>3012</td>
        <td>Invalid</td>
        <td>Spam</td>
    </tr>
    <tr>
        <td>3013</td>
        <td>Invalid</td>
        <td>Service refused</td>
    </tr>
    <tr>
        <td>3014</td>
        <td>Invalid</td>
        <td>Others</td>
    </tr>
    <tr>
        <td>3015</td>
        <td>Invalid</td>
        <td>No transfer path</td>
    </tr>
    <tr>
        <td>3016</td>
        <td>Invalid</td>
        <td>Failed to restrict attachment size</td>
    </tr>
    <tr>
        <td>3017</td>
        <td>Invalid</td>
        <td>Caller number(=Callback number) number format error in accordance with alteration prevention service </td>
    </tr>
    <tr>
        <td>3018</td>
        <td>Invalid</td>
        <td>Caller number(=Callback number) Phone number of personal mobile user who joined alteration prevention service </td>
    </tr>
    <tr>
        <td>3019</td>
        <td>Invalid</td>
        <td>Caller number(=Callback number) Number that is not registered in infobank through number advance registration system </td>
    </tr>
    <tr>
        <td rowspan=20>IB G/W</td>
        <td>1001</td>
        <td rowspan=20></td>
        <td>Server Busy (RS Internal save Queue Full)</td>
    </tr>
    <tr>
        <td>1002</td>
        <td>Recipient number format error</td>
    </tr>
    <tr>
        <td>1003</td>
        <td>Callback number format error</td>
    </tr>
    <tr>
        <td>1004</td>
        <td>SPAM</td>
    </tr>
    <tr>
        <td>1005</td>
        <td>Exceeded number of use</td>
    </tr>
    <tr>
        <td>1006</td>
        <td>No attachment</td>
    </tr>
    <tr>
        <td>1007</td>
        <td>There is attachment</td>
    </tr>
    <tr>
        <td>1008</td>
        <td>Failed to save attachment</td>
    </tr>
    <tr>
        <td>1009</td>
        <td>No CLIENT_MSG_KEY</td>
    </tr>
    <tr>
        <td>1010</td>
        <td>No CONTENT</td>
    </tr>
    <tr>
        <td>1011</td>
        <td>No CALLBACK</td>
    </tr>
    <tr>
        <td>1012</td>
        <td>No RECIPIENT_INFO</td>
    </tr>
    <tr>
        <td>1013</td>
        <td>No SUBJECT</td>
    </tr>
    <tr>
        <td>1014</td>
        <td>No attachment KEY</td>
    </tr>
    <tr>
        <td>1015</td>
        <td>No attachment NAME</td>
    </tr>
    <tr>
        <td>1016</td>
        <td>No attachment size</td>
    </tr>
    <tr>
        <td>1017</td>
        <td>No attachment Content</td>
    </tr>
    <tr>
        <td>1018</td>
        <td>No authority to send</td>
    </tr>
    <tr>
        <td>1019</td>
        <td>Exceeded TTL</td>
    </tr>
    <tr>
        <td>1020</td>
        <td>charset conversion error</td>
    </tr>
    <tr>
        <td rowspan=24>IB EMMA</td>
        <td>E900</td>
        <td rowspan=24>Invalid-IB</td>
        <td>In case of no send key</td>
    </tr>
    <tr>
        <td>E901</td>
        <td>In case of no recipient number</td>
    </tr>
    <tr>
        <td>E902</td>
        <td>(In case of address) In case of no recipient number order</td>
    </tr>
    <tr>
        <td>E903</td>
        <td>In case of no title</td>
    </tr>
    <tr>
        <td>E904</td>
        <td>In case of no message</td>
    </tr>
    <tr>
        <td>E905</td>
        <td>In case of no callback number</td>
    </tr>
    <tr>
        <td>E906</td>
        <td>In case of no message key</td>
    </tr>
    <tr>
        <td>E907</td>
        <td>In case of no address</td>
    </tr>
    <tr>
        <td>E908</td>
        <td>In case of no service type</td>
    </tr>
    <tr>
        <td>E909</td>
        <td>In case of no send request time </td>
    </tr>
    <tr>
        <td>E910</td>
        <td>In case of no TTL time</td>
    </tr>
    <tr>
        <td>E911</td>
        <td>In case service type is MMS MT, in case there is no attachment extension</td>
    </tr>
    <tr>
        <td>E912</td>
        <td>In case service type is MMS MT, in case there is no attachment in attach_file folder</td>
    </tr>
    <tr>
        <td>E913</td>
        <td>In case service type is MMS MT, in case attachment size is 0</td>
    </tr>
    <tr>
        <td>E914</td>
        <td>In case service type is MMS MT, In case there is file group key in message table but no data in file table </td>
    </tr>
    <tr>
        <td>E915</td>
        <td>Duplicate message</td>
    </tr>
    <tr>
        <td>E916</td>
        <td>Verification server blocked number</td>
    </tr>
    <tr>
        <td>E917</td>
        <td>Client DB blocked number</td>
    </tr>
    <tr>
        <td>E918</td>
        <td>USER CALLBACK FAIL</td>
    </tr>
    <tr>
        <td>E919</td>
        <td>In case of send restricted time, In case resend of message is restricted </td>
    </tr>
    <tr>
        <td>E920</td>
        <td>In case service type is LMS MT, In case there is file group key in message table </td>
    </tr>
    <tr>
        <td>E921</td>
        <td>In case service type is MMS MT, In case of no file group key in message table </td>
    </tr>
    <tr>
        <td>E922</td>
        <td>Use of restricted letter in address word error</td>
    </tr>
    <tr>
        <td>E999</td>
        <td>Other error</td>
    </tr>
    <tr>
        <td rowspan=10>IB Verification server</td>
        <td>1000</td>
        <td rowspan=10></td>
        <td> Success </td>
    </tr>
    <tr>
        <td>1001</td>
        <td>ID does not exist</td>
    </tr>
    <tr>
        <td>1002</td>
        <td>Verification error</td>
    </tr>
    <tr>
        <td>1003</td>
        <td>Server internal error (DB connection failure, etc)</td>
    </tr>
    <tr>
        <td>1004</td>
        <td>Wrong client password</td>
    </tr>
    <tr>
        <td>1005</td>
        <td>Public key is already registered</td>
    </tr>
    <tr>
        <td>1006</td>
        <td>Duplicate client public key</td>
    </tr>
    <tr>
        <td>1007</td>
        <td>IP Address verification failed</td>
    </tr>
    <tr>
        <td>1008</td>
        <td>MAC Address verification failed</td>
    </tr>
    <tr>
        <td>1009</td>
        <td>Service refused (Customer access prohibited)</td>
    </tr>
