## Notification > SMS > API v2.0 Guide 

## v2.0 API Overview

### [API Domain]

|Environment| Domain |
|---|---|
|Real|	https://api-sms.cloud.toast.com|

### [Caution]
* Character lengths are supported as follows.
* The maximum supported character counts are based on those saved; please write in standard specifications to prevent any text cutoff.
* Delivery is made upon EUC-KR encoding and it fails for unsupported emoticons.

| Category | Maximum Support | Standard Specifications |
| --- | --- | --- |
| SMS Body | 255 characters | 90 bytes (45 characters for Korean, or 90 for English) |
| MMS Title | 120 characters | 40 bytes (20 characters for Korean, or 40 for English) |
| MMS Body | 4,000 characters | 2,000 bytes (1,000 characters for Korean, or 2,000 for English) |


## SMS for Short-Messages

### Send Short SMS 

#### Request 

[URL]

```
POST  /sms/v2.0/appKeys/{appKey}/sender/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

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

|Value| Type | Required | Description |
|---|---|---|---|
|templateId|	String|	X| Delivery template ID |
|body|	String|	O| Text Body (as of EUC-KR, standard: 90 bytes, Max: 255 characters) |
|sendNo|	String|	O| Sender number |
|requestDate| String| X | Request date and time (yyyy-MM-dd HH:mm) |
|recipientList|	List|	O| List of recipients (up to 1000) |
|- recipientNo|	String|	O| Recipient number<br/>Available in combination with country code |
|- countryCode|	String|	X|	Country code [Default: 82 (Korea)] <br>(for international deliveries, only euc-kr (Korean, English) is available.) |
|- internationalRecipientNo| String| X| Recipient number including country code <br/>e.g.) 821012345678<br/>To be ignored when recipientNo is available. <br/> |
|- templateParameter|	Object|	X| Template parameter (with the input of template ID) |
|-- #key#|	String|	X| Replacement key (##key##) |
|-- #value#|	Object|	X| Value which is mapped for replacement key |
|userId|	String|	X | Delivery delimiter e.g.) admin,system |

#### Response

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

|Value| Type | Description |
|---|---|---|
|header|	Object| Header area |
|- isSuccessful|	Boolean| Successful or not |
|- resultCode|	Integer| Failure code |
|- resultMessage|	String| Failure message |
|body|	Object| Body area |
|- data|	Object| Data area |
|-- requestId|	String| Request ID |
|-- statusCode|	String| Request status code (1: requesting, 2: completely requested, 3: request failed) |
|-- sendResultList|	List:Object| List of delivery results |
|--- recipientNo| String | Recipient number |
|--- resultCode| Integer | Result code |
|--- resultMessage| String | Result message |

#### Example of Sending Short SMS (general domestic recipient numbers)

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
[Request body]  { } refers to a required value. 

```
{
    "body": "{body}",
    "sendNo": "{sender number}",
    "recipientList": [{
        "recipientNo": "{recipient number}"
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
              "recipientNo" : {recipient number},
              "resultCode" :  0,
              "resultMessage" : "SUCCESS"
          }
      ]
    }
  }
}
```

[curl]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-sms.cloud.toast.com/sms/v2.0/appKeys/{appKey}/sender/sms -d '{"body": "{body}","sendNo": "{sender number}","recipientList":[{"recipientNo": "{recipient number}"}],"userId": "{userId}"}'
```

#### Example of Sending Short SMS (with country code included to recipient number)

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


[Request body] { } refers to a required value. 
```
{
    "body": "{body}",
    "sendNo": "{sender number}",
    "recipientList": [
    {
        "internationalRecipientNo": "{sender number including national code}"
    },
    {
        "recipientNo":"{recipient number}",
        "countryCode":"{country code}"
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
              "recipientNo" : {recipient number},
              "resultCode" :  0,
              "resultMessage" : "SUCCESS"
          }
      ]
    }
  }
}
```

[curl]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-sms.cloud.toast.com/sms/v2.0/appKeys/{appKey}/sender/sms -d '{"body": "{body}","sendNo": "{sender number}","recipientList": [{"internationalRecipientNo": "{recipient number including country code}"},{"recipientNo":"{recipient number}","countryCode":"{country code}"}],"userId": ""}'
```

### List Delivery of Short SMS 

#### Request 

[URL]

```
GET  /sms/v2.0/appKeys/{appKey}/sender/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Query parameter] Number 1 or 2 is conditionally required  

|Value| Type | Required | Description |
|---|---|---|---|
|requestId|	String|	Conditionally required (no.1) | Request ID |
|startRequestDate|	String|	Conditionally required (no.2) | Start date of delivery (yyyy-MM-dd HH:mm:ss) |
|endRequestDate|	String|	Conditionally required (no.2) | End date of delivery (yyyy-MM-dd HH:mm:ss) |
|startResultDate|	String| Optional | Start date of receiving (yyyy-MM-dd HH:mm:ss) |
|endResultDate|	String| Optional | End date of receiving (yyyy-MM-dd HH:mm:ss) |
|sendNo|	String| Optional | Sender number |
|recipientNo|	String| Optional | Recipient number |
|templateId|	String| Optional | Template number |
|msgStatus|	String| Optional | Message status code (0:Failed, 1: Requesting, 2: Processing, 3:Successful, 4:Delivery Cancelled, 5:Duplicate Delivery) |
|resultCode|	String| Optional | Result code of receiving [[Table on Query Codes](./error-code/#_2)] |
|subResultCode|	String| Optional | Detail result code of receiving [[Table on Query Codes](./error-code/#_3)] |
|pageNum|	Integer| Optional | Page number (Default : 1) |
|pageSize|	Integer| Optional | Number of queries (Default : 15) |

#### Response

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

|Value| Type | Description |
|---|---|---|
|header|	Object| Header area |
|- isSuccessful|	Boolean| Successful or not |
|- resultCode|	Integer| Failure code |
|- resultMessage|	String| Failure message |
|body|	Object| Body area |
|- pageNum|	Integer| Current page number |
|- pageSize|	Integer| Number of queried data |
|- totalCount|	Integer| Total number of data |
|- data|	List| Data area |
|-- requestId|	String| Request ID |
|-- requestDate|	String| Date and time of sending |
|-- resultDate|	String| Date and time of receiving |
|-- templateId|	String| Template ID |
|-- templateName|	String| Template name |
|-- categoryId|	String| Category ID |
|-- categoryName|	String| Category name |
|-- body|	String| Body |
|-- sendNo|	String| Sender number |
|-- countryCode|	String| Country code |
|-- recipientNo|	String| Recipient number |
|-- msgStatus|	String| Message status name |
|-- msgStatusName|	String| Name of message status code |
|-- resultCode|	String| Result code of receiving [[Table on Result Code of Receiving](./error-code/#emma-v3)] |
|-- resultCodeName|	String| Result code name of receiving |
|-- telecomCode|	Integer| Telecom provider code |
|-- telecomCodeName|	String| Name of telecom provider |
|-- excelSeq|	Integer| Excel sequence number (data exists when it is delivered in excel) |
|-- mtPr|	Integer| Detail delivery ID (required to query details) |
|-- sendType|	String| Delivery type (0:SMS, 1:MMS, 2:Auth) |
|-- userId|	String| Delivery request ID |

### Query Delivery of Short SMS  

#### Request

[URL]

```
GET  /sms/v2.0/appKeys/{appKey}/sender/sms/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |
|requestId|	String| Request ID |

[Query parameter]

|Value| Type | Required | Description |
|---|---|---|---|
|mtPr|	Integer| Required | Detail delivery ID |

#### Response

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

|Value| Type | Description |
|---|---|---|
|header|	Object| Header area |
|- isSuccessful|	Boolean| Successful or not |
|- resultCode|	Integer| Failure code |
|- resultMessage|	String| Failure message |
|body|	Object| Body area |
|- data|	List| Data area |
|-- requestId|	String| Request ID |
|-- requestDate|	String| Date and time of sending |
|-- resultDate|	String| Date and time of receiving |
|-- templateId|	String| Template ID |
|-- templateName|	String| Template Name |
|-- categoryId|	String| Category ID |
|-- categoryName|	String| Category Name |
|-- body|	String| Body |
|-- sendNo|	String| Sender number |
|-- countryCode|	String| Country code |
|-- recipientNo|	String| Recipient number |
|-- msgStatus|	String| Message status name |
|-- msgStatusName|	String| Name of message status code |
|-- resultCode|	String| Result code of receiving [[Table on Result Code of Receiving](./error-code/#emma-v3)] |
|-- resultCodeName|	String| Result code name of receiving |
|-- telecomCode|	Integer| Telecom provider code |
|-- telecomCodeName|	String| Telecom provider name |
|-- excelSeq|	Integer| Excel sequence number (data exists when it is delivered in excel) |
|-- mtPr|	Integer| Delivery detail ID (required to query details) |
|-- sendType|	String| Delivery type (0:SMS, 1:MMS, 2:Auth) |
|-- userId|	String| Delivery request ID |

## Long MMS  

### Send Long MMS (attached file excluded)

#### Request

[URL]

```
POST  /sms/v2.0/appKeys/{appKey}/sender/mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

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

|Value| Type | Required | Description |
|---|---|---|---|
|templateId|	String| Optional | Delivery template ID |
|title|	String| Required | Title <br/> (less than 40 bytes in 'EUC-KR') <br/> (English: 1 byte, Korean: 2 bytes) |
|body|	String| Required | Body <br/> (less than 2000 bytes in 'EUC-KR') <br/> (English: 1 byte, Korean: 2 bytes) |
|sendNo|	String| Required | Sender number |
|requestDate| String| X | Request date and time (yyyy-MM-dd HH:mm) |
|attachFileIdList|	List:Integer| Optional | List of attached file IDs |
|recipientList|	List| Required | List of recipients (up to 1000) |
|- recipientNo|	String| Required | Recipient number<br/>Available in combination of country code |
|- countryCode|	String|	X|	Country code [default: 82 (Korea)] <br>(for international deliveries, only euc-kr (Korean, English)  is available.) |
|- internationalRecipientNo| String| X| Recipient number including country code<br/>e.g.) 821012345678<br/>To be ignored when recipientNo is available. <br/> |
|- templateParameter|	Object| Optional | Template parameter (with the input of template ID) |
|-- #key#|	String| Optional | Replacement key (##key##) |
|-- #value#|	Object| Optional | Value which is mapped to replacement key |
|userId|	String| Optional | Delivery delimiter e.g.) admin,system |

#### Response

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

|Value| Type | Description |
|---|---|---|
|header|	Object| Header area |
|- isSuccessful|	Boolean| Successful or not |
|- resultCode|	Integer| Failure code |
|- resultMessage|	String| Failure message |
|body|	Object| Body area |
|- data|	Object| Data area |
|-- requestId|	String| Request ID |
|-- statusCode|	String| Request status code (1:requesting, 2: completely requested, 3: request failed) |
|-- sendResultList|	List:Object| List of delivery results |
|--- recipientNo| String | Recipient number |
|--- resultCode| Integer | Result code |
|--- resultMessage| String | Result message |

#### Example of Sending Long MMS

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


[Request body] { } refers to a required value. 
```
{
    "title": "{title}",
    "body": "{body}",
    "sendNo": "{sender number}",
    "recipientList": [{
        "recipientNo": "{recipient number}",
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
              "recipientNo" : {recipient number},
              "resultCode" :  0,
              "resultMessage" : "SUCCESS"
          }
      ]
    }
  }
}
```

[curl]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-sms.cloud.toast.com/sms/v2.0/appKeys/{appKey}/sender/mms -d '{"title": "{title}","body": "{body}","sendNo": "{sender number}","recipientList": [{"recipientNo": "{recipient number}","templateParameter": { }}],"userId": ""}'
```

### Send MMS (attached file included)

#### Upload Attached Files

#### Request

[URL]

```
POST  /sms/v2.0/appKeys/{appKey}/attachfile/binaryUpload
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Request body]

```
{
    "fileName": String,
    "createUser": String,
    "fileBody": Byte[]
}
```

|Value| Type | Required | Description |
|---|----|----|---|
|fileName|	String| Required | Less than 45 characters<br/>File name (extensions available only in jpg or jpeg (small-case letters) |
|fileBody|	Byte[]| Required | File Byte[] value <br/>Less than 300K |
|createUser|	String| Required | File uploading user information |

#### Response

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

|Value| Type | Description |
|---|---|---|
|header|	Object| Header area |
|- isSuccessful|	Boolean| Successful or not |
|- resultCode|	Integer| Failure code |
|- resultMessage|	String| Failure message |
|body|	Object| Body area |
|- data|	Object| Data area |
|-- fileId|	Integer| File ID |
|-- fileName|	String| File name |
|-- filePath|	String| Default path of attached file <br/> (https://domain/attachFile/filePath/fileName) |

#### Example of Uploading Attached Files

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
  "fileName": "{file name}",
  "createUser": "{request ID}",
  "fileBody": "{file byte value}"
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

#### Example of Sending Attached Files

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


[Request body] { } refers to a required value. 
```
{
    "title": "{Title}",
    "body": "{Body Message}",
    "sendNo": "{Sender Number}",
    "attachFileIdList": [
        "{fielID of response after file uploaded}"
    ],
    "recipientList": [{
        "recipientNo": "{recipient number}",
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
      "requestId": "20180810100630ReZQ6KZzAH0",
      "statusCode": "2",
      "sendResultList" : [
          {
              "recipientNo" : {recipient number},
              "resultCode" :  0,
              "resultMessage" : "SUCCESS"
          }
      ]
    }
  }
}
```

### List Delivery of Long MMS Request

#### Request

[URL]

```
GET  /sms/v2.0/appKeys/{appKey}/sender/mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Query parameter] No. 1 or 2 conditionally required

|Value| Type | Required | Description |
|---|---|---|---|
|requestId|	String|	Conditionally required (no.1) | Request ID |
|startRequestDate|	String|	Conditionally required (no.2) | Start date of sending (yyyy-MM-dd HH:mm:ss) |
|endRequestDate|	String|	Conditionally required (no.2) | End date of sending (yyyy-MM-dd HH:mm:ss) |
|startResultDate|	String| Optional | Start date of receiving (yyyy-MM-dd HH:mm:ss) |
|endResultDate|	String| Optional | End date of receiving (yyyy-MM-dd HH:mm:ss) |
|sendNo|	String| Optional | Sender number |
|recipientNo|	String| Optional | Recipient number |
|templateId|	String| Optional | Template number |
|msgStatus|	String| Optional | Message status code (0:Failed, 1: Requesting, 2: Processing, 3:Successful, 4:Delivery Cancelled, 5:Duplicate Delivery) |
|resultCode|	String| Optional | Result code of receiving [[Table on Query Codes](./error-code/#_2)] |
|subResultCode|	String| Optional | Detail result code of receiving [[Table on Query Codes](./error-code/#_3)] |
|pageNum|	Integer| Optional | Page number (default : 1) |
|pageSize|	Integer| Optional | Number of queries  (default : 15) |

#### Response 

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
                fileId: Integer,
                serviceId: Integer,
                attachType: String,
                templateId: Integer,
                filePath: String,
                fileName: String,
                saveFileName: String,
                fileSize: Long,
                createDate: String,
                createUser: String,
                updateDate: String,
                updateUser: String,
                uploadType: String,
                existFileName: String
            }]
        }]
    }
}
```

|Value| Type | Description |
|---|---|---|
|header|	Object| Header area |
|- isSuccessful|	Boolean| Successful or not |
|- resultCode|	Integer| Failure code |
|- resultMessage|	String| Failure message |
|body|	Object| Body area |
|- pageNum|	Integer| Current page number |
|- pageSize|	Integer| Number of queried data |
|- totalCount|	Integer| Total number of data |
|- data|	List| Data area |
|-- requestId|	String| Request ID |
|-- requestDate|	String| Date and time of sending |
|-- resultDate|	String| Date and time of receiving |
||  | Template ID |
|-- templateName|	String| Template name |
|-- categoryId|	String| Category ID |
|-- categoryName|	String| Category name |
|-- title|	String| Title |
|-- body|	String| Body |
|-- sendNo|	String| Sender number |
|-- countryCode|	String| Country code |
|-- recipientNo|	String| Recipient number |
|-- msgStatus|	String| Message status code |
|-- msgStatusName|	String| Name of message status code |
|-- resultCode|	String| Result code of receiving [[Table on Receiving Result](./error-code/#emma-v3)] |
|-- resultCodeName|	String| Code name of receiving result |
|-- telecomCode|	Integer| Code of telecom provider |
|-- telecomCodeName|	String| Name of telecome provider |
|-- excelSeq|	Integer| Excel sequence number (data exists if sent in excel) |
|-- mtPr|	Integer| Detail delivery ID (required to query details) |
|-- sendType|	String| Delivery type (0:SMS, 1:MMS, 2:Auth) |
|-- userId|	String| Delivery request ID |
|-- serviceType| Integer| Delivery module type (0:SMS, 2:MMS, 3:LMS) |
|-- attachFileList|	List| List of attached files |
|--- fileId|	Integer|	File ID|
|--- serviceId|	Integer|	Service ID|
|--- attachType|	Integer|	Attachment Type|
|--- templateId|	String|	Template ID|
|--- filePath|	String| Default path of attached files <br/> (https://domain/attachFile/filePath/fileName) |
|--- filename|	String| Name of attached file |
|--- saveFileName|	String|	Name of saved file|
|--- fileSize|	Long|	Size of file|
|--- createDate|	String|	Date and time of creation |
|--- createUser|	String|	Creator|
|--- updateDate|	String|	Date and time of creation|
|--- updateUser|	String|	Modifier|
|--- uploadType|	String|	Upload Type|
|--- existFileName|	String|	Name of saved file|

### Query Single Delivery of Long MMS  

#### Request

[URL]

```
GET  /sms/v2.0/appKeys/{appKey}/sender/mms/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |
|requestId|	String| Request ID |

[Query parameter]

|Value| Type | Required | Description |
|---|---|---|---|
|mtPr|	Integer| Required | Detail delivery ID |

#### Response

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
                fileId: Integer,
                serviceId: Integer,
                attachType: String,
                templateId: Integer,
                filePath: String,
                fileName: String,
                saveFileName: String,
                fileSize: Long,
                createDate: String,
                createUser: String,
                updateDate: String,
                updateUser: String,
                uploadType: String,
                existFileName: String
            }]
        }]
    }
}
```

|Value| Type | Description |
|---|---|---|
|header|	Object| Header area |
|- isSuccessful|	Boolean| Successful or not |
|- resultCode|	Integer| Failure code |
|- resultMessage|	String| Failure message |
|body|	Object| Body area |
|- data|	List| Data area |
|-- requestId|	String| Request ID |
|-- requestDate|	String| Date and time of sending |
|-- resultDate|	String| Date and time of receiving |
|-- templateId|	String| Template ID |
|-- templateName|	String| Template name |
|-- categoryId|	String| Category ID |
|-- categoryName|	String| Category name |
|-- title|	String| Title |
|-- body|	String| Body |
|-- sendNo|	String| Sender number |
|-- countryCode|	String| Country code |
|-- recipientNo|	String| Recipient number |
|-- msgStatus|	String| Message status code |
|-- msgStatusName|	String| Name of message status code |
|-- resultCode|	String| Result code of receiving [[Table on Result Code of Receiving](./error-code/#emma-v3)] |
|-- resultCodeName|	String| Result code name of receiving |
|-- telecomCode|	Integer| Code of telecom provider |
|-- telecomCodeName|	String| Name of telecom provider |
|-- excelSeq|	Integer| Excel sequence number (data exists if sent in excel) |
|-- mtPr|	Integer| Detail delivery ID (required to query details) |
|-- sendType|	String| Delivery type (0:SMS, 1:MMS, 2:Auth) |
|-- userId|	String| Delivery request ID |
|-- serviceType| Integer| Delivery module type (0:SMS, 2:MMS, 3:LMS) |
|-- attachFileList|	List| List of attached files |
|--- fileId|	Integer|	File ID|
|--- serviceId|	Integer|	Service ID|
|--- attachType|	Integer|	Type of attachment|
|--- templateId|	String|	Template ID|
|--- filePath|	String| Default path of attached files <br/> (https://domain/attachFile/filePath/fileName) |
|--- filename|	String| Name of attached file |
|--- saveFileName|	String|	Name of saved file|
|--- fileSize|	Long|	Size of file|
|--- createDate|	String|	Date and time of creation |
|--- createUser|	String|	Creator|
|--- updateDate|	String|	Date and time of creation|
|--- updateUser|	String|	Modifier|
|--- uploadType|	String|	Type of uploaded|
|--- existFileName|	String|	Name of saved file|

## SMS for Authentication (emergency)

### Send SMS for Authenticataion

#### Request 

[URL]

```
POST  /sms/v2.0/appKeys/{appKey}/sender/auth/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

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

|Value| Type | Required | Description |
|---|---|---|---|
|templateId|	String|	X| Delivery template ID |
|body|	String|	O| Text Body (as of EUC-KR, standard: 90 bytes, Max: 255 characters) |
|sendNo|	String|	O| Sender number |
|requestDate| String| X | Request date and time (yyyy-MM-dd HH:mm) |
|recipientList|	List|	O| List of recipients (up to 1000) |
|- recipientNo|	String|	O| Recipient number<br/>Available in combination of the country code |
|- countryCode|	String|	X|	Country code [default: 82 (Korea)] <br>(for international deliveries, only euc-kr (Korean, English) is available.) |
|- internationalRecipientNo| String| X| Recipient number including country code<br/>e.g.) 821012345678<br/>To be ignored if recipientNo is available. |
|- templateParameter|	Object|	X| Template parameter (with the input of template ID) |
|-- #key#|	String|	X| Replacement key (##key##) |
|-- #value#|	Object|	X| Value which is mapped for replacement key |
|userId|	String|	X | Delivery delimiter e.g.) admin,system |


#### Response
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

|Value| Type | Description |
|---|---|---|
|header|	Object| Header area |
|- isSuccessful|	Boolean| Successful or not |
|- resultCode|	Integer| Failure code |
|- resultMessage|	String| Failure message |
|body|	Object| Body area |
|- data|	Object| Data area |
|-- requestId|	String| Request ID |
|-- statusCode|	String| Request status code (1: requesting, 2: completely requested, 3: request failed) |
|-- sendResultList|	List:Object| List of delivery results |
|--- recipientNo| String | Recipient number |
|--- resultCode| Integer | Result code |
|--- resultMessage| String | Result messsage |

#### Example 

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


[Request body] Mark required values with { }.
```
{
    "body": "{body message}",
    "sendNo": "{sender number}",
    "recipientList": [{
        "recipientNo": "{recipient number}",
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

[curl]
```
curl -X POST -H "Content-Type: application/json;charset=UTF-8" https://api-sms.cloud.toast.com/sms/v2.0/appKeys/{appKey}/sender/auth/sms -d '{"body": "{body message}","sendNo": "{sender number}","recipientList":[{"recipientNo": "{recipient number}","templateParameter": { }}],"userId": ""}'
```

### List SMS Delivery for Authentication

#### Request 

[URL]

```
GET  /sms/v2.0/appKeys/{appKey}/sender/auth/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|----|---|
|appKey|	String| Original appkey |

[Query parameter] No. 1 or 2 is conditionally required 

|Value| Type | Required | Description |
|---|---|---|---|
|requestId|	String|	Conditionally required (no. 1) | Request ID |
|startRequestDate|	String|	Conditionally required (no. 2) | Start date of sending (yyyy-MM-dd HH:mm:ss) |
|endRequestDate|	String|	Conditionally required (no. 2) | End date of sending (yyyy-MM-dd HH:mm:ss) |
|startResultDate|	String| Optional | Start date of receiving (yyyy-MM-dd HH:mm:ss) |
|endResultDate|	String| Optional | End date of receiving (yyyy-MM-dd HH:mm:ss) |
|sendNo|	String| Optional | Sender number |
|recipientNo|	String| Optional | Recipient number |
|templateId|	String| Optional | Template number |
|msgStatus|	String| Optional | Message status code (0:Failed, 1: Requesting, 2: Processing, 3:Successful, 4:Delivery Cancelled, 5:Duplicate Delivery) |
|resultCode|	String| Optional | Result code of receiving [[Table on Query Codes](./error-code/#_2)] |
|subResultCode|	String| Optional | Detail result code of receiving [[Table on Query Codes](./error-code/#_3)] |
|pageNum|	Integer| Optional | Page number (default : 1) |
|pageSize|	Integer| Optional | Number of queries (default : 15) |

#### Response

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

|Value| Type | Description |
|---|---|---|
|header|	Object| Header area |
|- isSuccessful|	Boolean| Successful or not |
|- resultCode|	Integer| Failure code |
|- resultMessage|	String| Failure message |
|body|	Object| Body area |
|- pageNum|	Integer| Current page number |
|- pageSize|	Integer| Number of queried data |
|- totalCount|	Integer| Total number of data |
|- data|	List| Data area |
|-- requestId|	String| Request ID |
|-- requestDate|	String| Date and time of sending |
|-- resultDate|	String| Date and time of receiving |
|-- templateId|	String| Template ID |
|-- templateName|	String| Template name |
|-- categoryId|	String| Category ID |
|-- categoryName|	String| Category name |
|-- body|	String| Body |
|-- sendNo|	String| Sender number |
|-- countryCode|	String| Country code |
|-- recipientNo|	String| Recipient number |
|-- msgStatus|	String| Message status code |
|-- msgStatusName|	String| Name of message status code |
|-- resultCode|	String| Result code of receiving [[Table on result code of receiving](./error-code/#emma-v3)] |
|-- resultCodeName|	String| Name of result code of  receiving |
|-- telecomCode|	Integer| Code of telecom provider |
|-- telecomCodeName|	String| Name of telecom provider |
|-- excelSeq|	Integer| Excel sequence number (data exists if sent in excel) |
|-- mtPr|	Integer| ID of delivery details (required to query details) |
|-- sendType|	String| Delivery type (0:SMS, 1:MMS, 2:Auth) |
|-- userId|	String| Delivery request ID |

### Query Single SMS Delivery for Authentication

#### Request 

[URL]

```
GET  /sms/v2.0/appKeys/{appKey}/sender/auth/sms/{requestId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |
|requestId|	String| Request ID |

[Query parameter]

|Value| Type | Required | Description |
|---|----|---|---|
|mtPr|	Integer| Required | Detail delivery ID |

#### Response 

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

|Value| Type | Description |
|---|---|---|
|header|	Object| Header area |
|- isSuccessful|	Boolean| Successful or not |
|- resultCode|	Integer| Failure code |
|- resultMessage|	String| Failure message |
|body|	Object| Body area |
|- data|	List| Data area |
|-- requestId|	String| Request ID |
|-- requestDate|	String| Date and time of sending |
|-- resultDate|	String| Date and time of receiving |
|-- templateId|	String| Template ID |
|-- templateName|	String| Template name |
|-- categoryId|	String| Category ID |
|-- categoryName|	String| Category name |
|-- body|	String| Body |
|-- sendNo|	String| Sender number |
|-- countryCode|	String| Country code |
|-- recipientNo|	String| Recipient number |
|-- msgStatus|	String| Message status code |
|-- msgStatusName|	String| Name of message status code |
|-- resultCode|	String| Result code of receiving [[Table on Result Code of Receiving](./error-code/#emma-v3)] |
|-- resultCodeName|	String| Result code name of receiving |
|-- telecomCode|	Integer| Code of telecom provider |
|-- telecomCodeName|	String| Name of telecom provider |
|-- excelSeq|	Integer| Excel sequence number (data exists if sent in excel) |
|-- mtPr|	Integer| ID of delivery details (required to query details) |
|-- sendType|	String| Delivery type (0:SMS, 1:MMS, 2:Auth) |
|-- userId|	String| ID of delivery request |

## Ad Messages 
### Send SMS for Advertisement
[URL]

```
POST  /sms/v2.0/appKeys/{appKey}/sender/ad-sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Request Body]
Same as Send SMS in the above. 
[[See Request Body](./api-guide/#sms_2)]

<span style="color:red"> However, following messages must be included in the body. </span>
080 numbers are available in **080 Call Rejection Settings** on console. 

```
(Ad)

[Reject receiving ads charge-free]080XXXXXXX
```


### Send MMS for Advertisement
[URL]

```
POST  /sms/v2.0/appKeys/{appKey}/sender/ad-mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Request Body]
Same as Send MMS in the above. 
[[See Request Body](./api-guide/#mms_1)]

<span style="color:red"> However, following messages must be included in the body. </span>
080 numbers are available in **Setting for Rejection of Receiving 080 Numbers ** on console. 

```
(Ad)

[Reject receiving ads charge-free]080XXXXXXX
```




## Tag Delivery

### Send Tagged SMS

#### Request 

[URL]

```
POST /sms/v2.0/appKeys/{appKey}/tag-sender/sms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Request body]

```
{
    "body":"SMS body",
    "sendNo":"01012345678",
    "requestDate":"2018-03-22 10:00",
    "templateId":"TEMPLATE",
    "templateParameter" : {
        "key" : "value"
    },
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

|Value| Type | Required | Description |
|---|---|---|---|
| body | String | O | Text Body (as of EUC-KR, standard: 90 bytes, Max: 255 characters) |
| sendNo | String | O | Sender number |
|requestDate| String| X | Date and time of request (yyyy-MM-dd HH:mm) |
| templateId | String | X | Template ID |
| templateParameter | Map<String, String> | X | Template parameter |
| tagExpression | List<String> | O | Tag expression<br/>e.g.) ["tagA","AND","tabB"] |
| userId | String | X | Requester ID |
| adYn | String | X | Ad or not (default is N) |
| autoSendYn | String | X | Auto delivery (immediate delivery) or not (default is Y) |


#### Response
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

|Value| Type | Description |
|---|---|---|
|header|	Object| Header area |
|- isSuccessful|	Boolean| Successful or not |
|- resultCode|	Integer| Failure code |
|- resultMessage|	String| Failure message |
|body|	Object| Body area |
|- data|	Object| Data area |
|-- requestId|	String| Request ID |

### Send Tagged LMS

#### Request 

[URL]

```
POST  /sms/v2.0/appKeys/{appKey}/tag-sender/mms
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Request body]
```
{
    "body":"SMS body",
    "sendNo":"01012345678",
    "requestDate":"2018-03-22 10:00",
    "templateId":"TEMPLATE",
    "templateParameter" : {
        "key" : "value"
    },
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

|Value| Type | Required | Description |
|---|---|---|---|
| title | String | O | Title |
| body | String | O | Body |
| sendNo | String | O | Sender number |
|requestDate| String| X | Request time and date  (yyyy-MM-dd HH:mm) |
| templateId | String | X | Template ID |
| templateParameter | Map<String, String> | X | Template parameter |
| tagExpression | List<String> | O | Tag expression<br/>e.g.) ["tagA","AND","tabB"] |
| attachFileIdList | List<Integer> | X | ID of attached file (fileId) |
| userId | String | X | Requester ID |
| adYn | String | X | Ad or not (default is N) |
| autoSendYn | String | X | Auto delivery (immediate delivery) or not (default is Y) |


#### Response
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

|Value| Type | Description |
|---|---|---|
|header|	Object| Header area |
|- isSuccessful|	Boolean| Successful or not |
|- resultCode|	Integer| Failure code |
|- resultMessage|	String| Failure message |
|body|	Object| Body area |
|- data|	Object| Data area |
|-- requestId|	String| Request ID |


### List Tag Delivery 

#### Request 

[URL]

```
GET /sms/v2.0/appKeys/{appKey}/tag-sender?sendType={sendType}&requestId={requestId}&startRequestDate={startRequestDate}&endRequestDate={endRequestDate}&statusCode={statusCode}&pageNum={pageNum}&pageSize={pageSize}
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Request body]

```
X
```

* requestId or startRequestDate + endRequestDate is required.

	Value|	Type|	Required|	Description|
|---|---|---|---|
| appKey | String| O | appkey |
| sendType | required, String | O |Delivery type<br>SMS : "0",<br>MMS : "1" |
| requestId | String | O |Request ID |
| startRequestDate | String | O | Start date of delivery |
| endRequestDate | String | O | End date of delivery |
| statusCode | String | X | Delivery status code<br>WAIT : "MAS00"<br>READY : "MAS01"<br>SENDREADY : "MAS09"<br>SENDWAIT : "MAS10"<br>SENDING : "MAS11"<br>COMPLETE : "MAS19"<br>CANCEL : "MAS91"<br>FAIL : "MAS99" |
| pageNum | optional, Integer | X | Page number |
| pageSize | optional, Integer | X | Number of queries |

#### Response
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
                "templateName": "replacement test(SMS)",
                "masterStatusCode": "READY",
                "sendNo": "15883796",
                "requestDate": "2017-12-20 14:15:58",
                "tagExpression": [
                    "Kb6BjCY1"
                ],
                "title": "",
                "body": "We send SMS##content##",
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

|Value| Type | Description |
|---|---|---|
|header|	Object| Header area |
|- isSuccessful|	Boolean| Successful or not |
|- resultCode|	Integer| Failure code |
|- resultMessage|	String| Failure message |
|body|	Object| Body area |
|- data|	Object| Data area |
|-- requestId | String | Request ID |
|-- requestIp | String | Request IP |
|-- requestDate | String | Request time |
|-- tagSendStatus | String | Tag delivery status |
|-- tagExpression | List | Tag expression |
|-- templateId | String | Template ID |
|-- templateName | String | Template name |
|-- senderName | String | Sender's name |
|-- senderMail | String | Sender's address |
|-- title | String | Title |
|-- body | String | Body |
|-- attachYn | String | Attached file or not |
|-- adYn | String | Ad or not |
|-- createUser | String | Creator |
|-- createDate | String | Date and time of creation |
|-- updateUser | String | Modifier |
|-- updateDate | String | Date and time of modification |


### List Recipients of Tag Delivery 

#### Request

[URL]

```
GET /sms/v2.0/appKeys/{appKey}/tag-sender/{requestId}?recipientNum={recipientNum}&startRequestDate={startRequestDate}&endRequestDate={endRequestDate}&startResultDate={startResultDate}&endResultDate={endResultDate}&msgStatusName={msgStatusName}&resultCode={resultCode}&pageNum={pageNum}&pageSize={pageSize}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |
| requestId | String | Request ID |
[Request body]

```
X
```

* requestId or startRequestDate + endRequestDate is required.

	Value|	Type|	Required|	Description|
|---|---|---|---|
| recipientNum | String | X | Recipient number |
| startRequestDate | String | O | Start date of delivery request |
| endRequestDate | String | O | End date of delivery request |
| startResultDate | String | X | Start date of receiving |
| endResultDate | | | |
| msgStatusName | | | |
| resultCode | | | |
| pageNum | | | |
| pageSize | | | |

#### Response
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
                "templateName": "replacement test(SMS)",
                "masterStatusCode": "READY",
                "sendNo": "15883796",
                "requestDate": "2017-12-20 14:15:58",
                "tagExpression": [
                    "Kb6BjCY1"
                ],
                "title": "",
                "body": "We send SMS##content##",
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

|Value| Type | Description |
|---|---|---|
|header|	Object| Header area |
|- isSuccessful|	Boolean| Successful or not |
|- resultCode|	Integer| Failure code |
|- resultMessage|	String| Failure message |
|body|	Object| Body area |
|- data|	Object| Data area |
|-- requestId | String | Request ID |
|-- requestIp | String | Request IP |
| sendType | String | Delivery type (0:SMS, 1:LMS/MMS) |
| templateId | String | Template ID |
| templateName | String | Template name |
| masterStatusCode | String | Master status code |
| sendNo | String | Delivery number |
| requestDate | String | Date and time of request |
| tagExpression | List<String> | Tag expression |
| title | String | Title |
| body | String | Body |
| adYn | String | Ad or not |
| autoSendYn | String | Auto delivery or not |
| sendErrorCount | Integer | Number of delivery errors |
| createUser | String | Creator |
| createDate | String | Date and time of creation |
| updateUser | String | Modifier |
| updateDate | String | Date and time of modification |

### List Recipient Details of Tag Delivery

#### Request

[URL]

```
GET /sms/v2.0/appKeys/{appKey}/tag-sender/{requestId}/{recipientSeq}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |
| requestId | String | Request ID |
| recipientSeq | String | Sequence |

[Request body]

```
X
```

#### Response
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
            "templateName": "replacement test(MMS)",
            "sendNo": "15883796",
            "recipientNum": "01000000000",
            "title": "Title",
            "body": "Body",
            "requestDate": "2017-12-20 15:28:55.0",
            "msgStatusName": "READY",
            "msgStatus": "1",
            "resultCode": "SUCCESS",
            "receiveDate": "2017-12-25 14:06:04.0",
            "attachFileList": [
                {
                    "fileId": 58217,
                    "filePath": "10869/toast-mt-2017-07-28/1459/58217",
                    "fileName": "screen shot 2017-07-12 AM 10.00.12.jpg",
                    "fileSize":null,
                    "createDate": "2017-07-28 14:59:31.0",
                    "updateDate": "2017-07-28 14:59:35.0"
                }
            ]
        }
}
}
```

|Value| Type | Description |
|---|---|---|
|header|	Object| Header area |
|- isSuccessful|	Boolean| Successful or not |
|- resultCode|	Integer| Failure code |
|- resultMessage|	String| Failure message |
|body|	Object| Body area |
|- data|	Object| Data area |
|-- requestId | String | Request ID |
|-- recipientSeq | Integer | Request ID |
|-- sendType | String | Delivery type |
|-- messageType | String | Message type |
|-- templateId | String | Template ID |
|-- templateName | String | Template name |
|-- sendNo | String | Delivery number |
|-- title | String | Title |
|-- body | String | Body |
|-- recipientNum | String | Recipient number |
|-- requestDate | String | Date and time of request |
|-- msgStatusName | String | Message status name |
|-- resultCode | String | Receiving code |
|-- receiveDate | String | Date and time of receiving |
|-- attachFileList | List | List of attached files |
|---- filePath | String | Attached file- Path |
|---- fileName | String | Attached file - Name |
|---- fileSize | Long | Attached file - Size |
|---- fileSequence | Integer | Attached file - File number |
|---- createDate | String | Attached file - Date and time of creation |
|---- updateDate | String | Attached file - Date and time of modification |




## Templates

### Send Templates (requiring no body updates)

#### Example 
![[그림 1] 템플릿 등록](http://static.toastoven.net/prod_sms/img_26.png)

<table>
  <thead>
    <tr>
      <th>Http method</th>
      <th>Type</th>
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

For Request URL, choose a delivery type selected to register templates.  

**If request parameter body is empty, replace it with the body of the corresponding templateId.**

[Request body] Replace with key and value for those for replacement. 

```
{
    "templateId": "{template ID}",
    "recipientList": [{
        "recipientNo": "{recipient number}",
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

### Send Templates (requiring body updates)

#### Example of Sending Templates  

<table>
  <thead>
    <tr>
      <th>Http method</th>
      <th>Type</th>
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


For Request URL, choose a delivery type selected to register templates.

**If template ID and request parameter body include values, sender number and body message are not replaced with template. **

Nevertheless, with the input of template ID, it is available to query with the template. 

Such case is applicable when template needs to be modified after queried. 

[Request body]

```
{
    "templateId": "{template ID}",
    "body": "{body message}",
    "sendNo": "{sender number}",
    "recipientList": [{
        "recipientNo": "{recipient number}",
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


### List Templates 

#### Request

[URL]

```
GET  /sms/v2.0/appKeys/{appKey}/templates
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Query parameter]

|Value| Type | Required | Description |
|---|---|---|---|
|categoryId|	Integer| Optional | Category ID |
|useYn|	String| Optional | Use or not (Y/N) |
|pageNum|	Integer| Optional | Page number (Default : 1) |
|pageSize|	Integer| Optional | Number of queries (Default : 15) |
|all|	Boolean| Optional | Query all or not |

#### Response

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

|Value| Type | Description |
|---|---|---|
|header|	Object| Header area |
|- isSuccessful|	Boolean| Successful or not |
|- resultCode|	Integer| Failure code |
|- resultMessage|	String| Failure message |
|body|	Object| Body area |
|- pageNum|	Integer| Current page number |
|- pageSize|	Integer| Number of queried data |
|- totalCount|	Integer| Total number of data |
|- data|	List| Data area |
|-- templateId|	String| Template ID |
|-- serviceId|	Integer| Service ID (unused, for internal purpose) |
|-- categoryId|	Integer| Category ID |
|-- categoryName|	String| Category name |
|-- sort|	Integer| Sorted value |
|-- templateName|	String| Template name |
|-- templateDesc|	String| Template description |
|-- useYn|	String| Use or not |
|-- priority|	String| Priority value (unused) |
|-- sendNo|	String| Sender number |
|-- sendType|	String| Delivery type (0:Sms, 1:mms, 2:auth) |
|-- sendTypeName|	String| Name of delivery type |
|-- title|	String| Title |
|-- body|	String| Body message |
|-- attachFileYn|	String| Attached or not (Y/N) |
|-- delYn”|	String| Deleted or not (Y/N): only to show current status |
|-- createDate|	String| Date of registration |
|-- createUser|	String| Registered user |
|-- updateDate|	String| Date of modification |
|-- updateUser|	String| Modifier |
|-- attachFileList|	List| List of attached files |
|--- fileId|	Integer| Attached file ID |
|--- serviceId|	Integer| Service ID (unused, for internal purpose) |
|--- attachType|	Integer| Upload type of attachment (0:Temporary,1:Upload,2:Template) |
|--- templateId|	String| Template ID |
|--- filePath|	String| Path of attached file |
|--- fileName|	String| Name of attached file |
|--- fileSize|  Integer| File size |
|--- createDate|	String| Date of registration of attahment |
|--- createUser|	String| Registered user of attachment |


### Query Single Template

#### Request 

[URL]

```
GET  /sms/v2.0/appKeys/{appKey}/templates/{templateId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |
|templateId|	String| Template ID |

#### Response

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

|Value| Type | Description |
|---|---|---|
|header|	Object| Header area |
|- isSuccessful|	Boolean| Successful or not |
|- resultCode|	Integer| Failure code |
|- resultMessage|	String| Failure message |
|body|	Object| Body area |
|- data|	List| Data area |
|-- templateId|	String| Template ID |
|-- serviceId|	Integer| Service ID  (unused, for internal purpose) |
|-- categoryId|	Integer| Category ID |
|-- categoryName|	String| Category name |
|-- sort|	Integer| Sorted value |
|-- templateName|	String| Template name |
|-- templateDesc|	String| Template description |
|-- useYn|	String| Use or not |
|-- priority|	String| Priority value (unused) |
|-- sendNo|	String| Sender number |
|-- sendType|	String| Delivery type (0:Sms, 1:mms, 2:auth) |
|-- sendTypeName|	String| Name of delivery type |
|-- title|	String| Title |
|-- body|	String| Body |
|-- attachFileYn|	String| Attached or not (Y/N) |
|-- delYn”|	String| Delete or not (Y/N), to display current status only |
|-- createDate|	String| Date of registration |
|-- createUser|	String| Registered user |
|-- updateDate|	String| Date of modification |
|-- updateUser|	String| Modified user |
|-- attachFileList|	List| List of attached files |
|--- fileId|	Integer| ID of attached file |
|--- serviceId|	Integer| Service ID  (unused, for internal purpose) |
|--- attachType|	Integer| Upload type for attached file (0: temporary,1: upload, 2: template) |
|--- templateId|	String| Template ID |
|--- filePath|	String| Path of attached file |
|--- fileName|	String| Name of attached file |
|--- fileSize|  Integer| File size |
|--- createDate|	String| Date of registration for attached files |
|--- createUser|	String| Registered user of attached files |

## Rejection of Receiving 080 Numbers
### Register Unsubsribers

#### Request

[URL]

```
POST /sms/v2.0/appKeys/{appKey}/blockservice/recipients
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String|	Original appkey|

[Request body]

```
{
    "unsubscribeNo":"0800000000",
    "recipientNoList":["0100000000", "0100000001"]
}
```

|Value| Type |	Max Length | Required | Description |
|---|---|---|---|---|
|unsubscribeNo |    String | 25 | O | 080 numbers to reject receiving|
| recipientNoList | List<String> | 10 | O | Contact number of unsubscribers to be added |

#### Response

```
{
   "header":{
       "isSuccessful":true,
       "resultCode":0,
       "resultMessage":"Success"
   }
}
```

### Query Target of Rejection 

#### Request 

[URL]

```
GET  /sms/v2.0/appKeys/{appKey}/blockservice/recipients
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Query parameter]

|Value| Type | Required | Description |
|---|---|---|---|
|unsubscribeNo|	String|	Required |	080 numbers to reject receiving |
|startRequestDate|	String|	Optional | Start value of request for reject receiving (yyyy-MM-dd HH:mm:ss) |
|endRequestDate| String |	Optional | End value of request for reject receiving (yyyy-MM-dd HH:mm:ss) |
|pageNum|	Integer| Optional | Page number (Default: 1) |
|pageSize|	Integer| Optional | Number of queries (Default : 15) |

#### Response
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

### Delete Target of Rejection

#### Request

[URL]

```
DELETE  /sms/v2.0/appKeys/{appKey}/blockservice/recipients/removes?unsubscribeNo={unsubscribeNo}&updateUser={updateUser}&recipientNoList={recipientNo},{recipientNo}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Query parameter]

|Value| Type | Required | Description |
|---|---|---|---|
|unsubscribeNo|	String|	Required |	080 numbers to reject receiving |
|updateUser|	String|	Required | User who delete rejection of receiving |
|recipientNo|	String|	Required | Rejected numbers to be deleted |

#### Response
```
{
    "header": {
        "isSuccessful": boolean,
        "resultCode": Integer,
        "resultMessage": String
    },
    "body": null
}
```

## Sender Number 

### Request for Sender Number Registration 

#### Request

[URL]

```
POST /sms/v2.0/appKeys/{appKey}/requests/sendNos
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

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

|Value| Type |	Max Length | Required | Description |
|---|---|---|---|---|
| sendNos[] |	List | - | Required |	Sender Numbers|
| fileIds[] |	List| - | Optional | File ID for updated document|
| comment | String | 4000 | Optional | Messages for the administrator who approved sender number  |

#### Response
```
{
  "header" : {
    "isSuccessful" :  Boolean,
    "resultCode" :  Integer,
    "resultMessage" :  String
  }
}
```

### Upload Sender Number Documents 

#### Request 

[URL]

```
POST /sms/v2.0/appKeys/{appKey}/requests/attachFiles/authDocuments
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Request Body]

|Value|Type|Description|
|---|---|---|
| attachFile | MultiPartFile | File data receivable as MultiPartFile |
#### Response
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

### Query History of Request for Sender Number Authentication API

#### Request 

[URL]

|Http method|	URI|
|---|---|
|GET|	/sms/v2.0/appKeys/{appKey}/requests/sendNos?status={status}|

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Query parameter]

|Value| Type | Description |
|---|---|---|
|status|	String| Document authentication status<br/>- SRS01	Requested for sender number registration 
- SRS02	Evaluating
- SRS03	Registration completed
- SRS04	Registration unavailable
- SRS05	Ready for mobile phone authentication 
- SRS06	Mobile phone authentication failed
- SRS07	Manual registration completed |

#### Response 
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

### List Registered Sender Numbers API

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/sms/v2.0/appKeys/{appKey}/sendNos?sendNo={sendNo}&useYn={useYn}&blockYn={blockYn}

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Query parameter]

|Value| Type | Description |
|---|---|---|
| sendNo | String | Sender number |
| useYn | String | Use or not |
| blockYn | String | Block or not |

#### Response
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

|Value| Type | Description |
|---|---|---|
|header|	Object| Header area |
|- isSuccessful|	Boolean| Successful or not |
|- resultCode|	Integer| Failure code |
|- resultMessage|	String| Failure message |
|body|	Object| Body area |
|- pageNum|	Integer| Page number |
|- pageSize|	Integer| List output size |
|- totalCount|	Integer| Total data count |
|-- serviceId | Integer | Service ID |
|-- sendNo | String | Sender number |
|-- useYn | String | Use or not |
|-- blockYn | String | Block or not |
|-- blockReason | String | Cause of blockage |
|-- createDate | String | Date and time of creation |
|-- createUser | String | Creator |
|-- updateDate | String | Date and time of modification |
|-- updateUser | String | Modifier |

## Query Statistics 

### Query Integrated Statistics 

#### Request

[URL]

|Http method|	URI|
|---|---|
|GET|	/sms/v2.0/appKeys/{appKey}}/statistics/view?searchType={searchType}&from={from}&to={to}&messageTypes={messageType}&contentTypes={contentType}&templateId={templateId}|

[Path parameter]

|Value| Type | Description |
|---|---|---|
|appKey|	String| Original appkey |

[Query parameter]

|Value| Type |	Required |Description|
|---|---|---|---|
| searchType | String | O | Type of statistics <br/>DATE:By date, TIME:By time, DAY:By day |
| from | String | O | Start date of query statistics<br/>yyyy-MM-dd HH:mm |
| to | String | O | End date of query statistics<br/>yyyy-MM-dd HH:mm |
| messageType | X | String | Message type<br/>SMS: Short messages, LMS: Long messages, MMS: Attachment, AUTH: Authentication |
| contentType | X | String | Content type <br/>NORMAL: General, AD: Advertisement |
| templateId | X | String | Template ID |

#### Response
```
{
    "header" : {
        "isSuccessful" : true,
        "resultCode" : 0,
        "resultMessage" : "SUCCESS""
    },
    "body" : {
        "data" :
        [
          {
            "divisionName" : "2018-06-01",
            "statisticsView" :
            {
              "requestedCount" : 10,
              "succeedCount" : 10,
              "failedCount" : 0,
              "pendingCount" : 0,
              "succeedRate" : "100.00",
              "failedRate" : "0.00",
              "pendingRate" : "0.00"
            }
          }
        ]
    }
}
```
|Value| Type |Description|
|---|---|---|
| header | Object | |
| - isSuccessful | Boolean | Successful or not |
| - resultCode | Integer | Result code |
| - resultMessage | String | Result message |
| body.data | List | |
| - divisionName | String | Display name<br/>Date, time, Day |
| - statisticsView | Object | |
| -- requestedCount | Integer | Number of requests |
| -- succeedCount | Integer | Number of successes |
| -- failedCount | Integer | Number of failures |
| -- pendingCount | Integer | Number of pending cases |
| -- succeedRate | String | Rate of success |
| -- failedRate | String | Rate of failure |
| -- pendingRate | String | Rate of delivery |

## Tag Management

### Query Tags

#### Request

[URL]

```
GET /sms/v2.0/appKeys/{appKey}/tags?pageNum={pageNum}&pageSize={pageSize}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value|	Type|	Description|
|---|---|---|
|appKey|	String|	Original appKey|

[Query parameter]

|Value| Type | Max Length | Required | Description |
|---|---|---|---|---|
|pageNum|	Integer|	- | Optional | Page number (Default : 1)|
|pageSize|	Integer|	1000 | Optional | Number of queries (Default : 15)|

#### Response

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

|Value|	Type|	Description|
|---|---|---|
|header.isSuccessful|	Boolean|	Successful or not|
|header.resultCode|	Integer|	Failure code|
|header.resultMessage|	String|	Failure message|
|body.pageNum|	Integer|	Page number|
|body.pageSize|	Integer|	Number of queries|
|body.totalCount|	Integer|	Total data count|
|body.data[].tagId| String | Tag ID |
|body.data[].tagName| String | Tag name |
|body.data[].createdDate| String | Date and time of creation |
|body.data[].tagId| String | Date and time of modification |

### Register Tags

[URL]

```
POST /sms/v2.0/appKeys/{appKey}/tags
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value|	Type|	Description|
|---|---|---|
|appKey|	String|	Original appKey|

[Request body]

```json
{
  "tagName": "TAG"
}
```

|Value| Type | Max Length | Required | Description |
|---|---|---|---|---|
| tagName | String | 30 | Required | Tag name |

#### Response

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

|Value|	Type|	Description|
|---|---|---|
|header.isSuccessful|	Boolean|	Successful or not|
|header.resultCode|	Integer|	Failure code|
|header.resultMessage|	String|	Failure message|
|body.data.tagId| String | Tag ID |

### Modify Tags

[URL]

```
PUT /sms/v2.0/appKeys/{appKey}/tags/{tagId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value|	Type|	Description|
|---|---|---|
|appKey|	String|	Original appKey|
|tagId|	String|	Tag ID|

[Request body]

```json
{
  "tagName": "TAG"
}
```

|Value| Type | Max Length | Required | Description |
|---|---|---|---|---|
| tagName | String | 30 | Required | Tag name |

#### Response

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

|Value|	Type|	Description|
|---|---|---|
|header.isSuccessful|	Boolean|	Successful or not|
|header.resultCode|	Integer|	Failure code|
|header.resultMessage|	String|	Failure message|

### Delete Tags

[URL]

```
DELETE /sms/v2.0/appKeys/{appKey}/tags/{tagId}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value|	Type|	Description|
|---|---|---|
|appKey|	String|	Original appKey|
|tagId|	String|	Tag ID|

#### Response

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

|Value|	Type|	Description|
|---|---|---|
|header.isSuccessful|	Boolean|	Successful or not|
|header.resultCode|	Integer|	Failure code|
|header.resultMessage|	String|	Failure message|


## UID Management

### Query UIDs

#### Request

[URL]

```
GET /sms/v2.0/appKeys/{appKey}/uids?wheres={wheres}&offsetUid={offsetUid}&offset={offset}&limit={limit}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value|	Type|	Description|
|---|---|---|
|appKey|	String|	Original appKey|

[Query parameter]

|Value| Type | Max Length | Required | Description |
|---|---|---|---|---|
|wheres|	List<String>|	- | Optional | Query conditions.<br/>Character strings comprised of alphabets, numbers, and parentheses.<br/>Allows one parenthesis, and up to three AND or ORs.<br/>e.g.) tagId1,AND,tagId2|
|offsetUid|	String|	- | Optional | offset UID|
|offset | Integer | - | Optional | offset (default: 0)|
|limit | Integer | 1000 | Optional | Number of queries (default: 15)|

#### Response

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
            "isLast": false,
            "totalCount": 5
        }
    }
}
```

|Value|	Type|	Description|
|---|---|---|
|header.isSuccessful|	Boolean|	Successful or not|
|header.resultCode|	Integer|	Failure code|
|header.resultMessage|	String|	Failure message|
|body.data.uids[].uid| String | UID |
|body.data.uids[].tags[].tagId| String | Tag ID |
|body.data.uids[].tags[].tagName| String | Tag name |
|body.data.uids[].tags[].createdDate| String | Date and time of tag creation |
|body.data.uids[].tags[].updatedDate| String | Date and time of tag modification |
|body.data.uids[].contacts[].contactType| String | Contact type(PHONE_NUMBER) |
|body.data.uids[].contacts[].contact| String | Contact (phone number) |
|body.data.uids[].contacts[].createdDate| String | Date and time of contact creation |
|body.data.uids[].isLast| Boolean| Last on list or not |
|body.data.uids[].totalCount| Integer| Total number of data |

### Get UIDs

#### Request

[URL]

```
GET /sms/v2.0/appKeys/{appKey}/uids/{uid}
```

[Path parameter]

|Value|	Type|	Description|
|---|---|---|
|appKey|	String|	Original appKey|
|uid|	String|	UID|

#### Response

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

|Value|	Type|	Description|
|---|---|---|
|header.isSuccessful|	Boolean|	Successful or not|
|header.resultCode|	Integer|	Failure code|
|header.resultMessage|	String|	Failure message|
|body.data.uid| String | UID |
|body.data.tags[].tagId| String | Tag ID |
|body.data.tags[].tagName| String | Tag name |
|body.data.tags[].createdDate| String | Date and time of tag creation |
|body.data.tags[].updatedDate| String | Date and time of tag modification |
|body.data.contacts[].contactType| String | Contact type |
|body.data.contacts[].contact| String | Contact(phone number) |
|body.data.contacts[].createdDate| String | Date and time of contact creation |

### Register UIDs

[URL]

```
POST /sms/v2.0/appKeys/{appKey}/uids
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value|	Type|	Description|
|---|---|---|
|appKey|	String|	Original appKey|

[Request body]

```json
{
  "uids": [
  {
      "uid": "UID",
      "tagIds": ["ABCD1234"],
      "contacts": [
        {
          "contactType": "PHONE_NUMBER",
          "contact": "0100000000"
        }
      ]
  }]
}
```

|Value| Type | Max Length | Required | Description |
|---|---|---|---|---|
| uid | String | - | Required | UID |
| tagIds[] | String | - | Required | List of tag IDs |
| contacts[].contactType | String | - | Required | Contact type(PHONE_NUMBER) |
| contacts[].contact | String | - | Required | Contact (phone number) |

[주의]
* When tagIds is provided, contacts is not required.
* When contacts is provided, tagIds is not required.
* For this product, contactType must be requested in the "PHONE_NUMBER" value.

#### Response

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

|Value|	Type|	Description|
|---|---|---|
|header.isSuccessful|	Boolean|	Successful or not|
|header.resultCode|	Integer|	Failure code|
|header.resultMessage|	String|	Failure message|


### Delete UIDs

[URL]

```
DELETE /sms/v2.0/appKeys/{appKey}/uids/{uid}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value|	Type|	Description|
|---|---|---|
|appKey|	String|	Original appKey|
|uid|	String|	UID|

#### Response

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

|Value|	Type|	Description|
|---|---|---|
|header.isSuccessful|	Boolean|	Successful or not|
|header.resultCode|	Integer|	Failure code|
|header.resultMessage|	String|	Failure message|

### Register Phone Number

[URL]

```
POST /sms/v2.0/appKeys/{appKey}/uids/{uid}/phone-numbers
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value|	Type|	Description|
|---|---|---|
|appKey|	String|	Original appKey|
|uid | String | UID |

[Request body]

```json
{
  "phoneNumber": "0100000000"
}
```

|Value| Type | Max Length | Required | Description |
|---|---|---|---|---|
| phoneNumber| String | - | Required | Phone number |


#### Response

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

|Value|	Type|	Description|
|---|---|---|
|header.isSuccessful|	Boolean|	Successful or not|
|header.resultCode|	Integer|	Failure code|
|header.resultMessage|	String|	Failure message|

### Delete phone number

[URL]

```
DELETE /sms/v2.0/appKeys/{appKey}/uids/{uid}/phone-numbers/{phoneNumber}
Content-Type: application/json;charset=UTF-8
```

[Path parameter]

|Value|	Type|	Description|
|---|---|---|
|appKey|	String|	Original appKey|
|uid | String | UID |
|phoneNumber | String | Phone number |

#### Response

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

|Value|	Type|	Description|
|---|---|---|
|header.isSuccessful|	Boolean|	Successful or not|
|header.resultCode|	Integer|	Failure code|
|header.resultMessage|	String|	Failure message|