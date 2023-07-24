## Notification > SMS > Result Code

## API Result Code

| Service | Successful or not | Result code | Result code message | API response message |
| - | - | - | - | - |
| Common | true | 0 | Successful | SUCCESS |
| Common | false | 4 | Parameter validation failed | |
| Common | false | -1000 | Invalid appkey | Invalid appKey. |
| Common | false | -1001 | Not exist appkey | Service does not exist. |
| Common | false | -1002 | Terminated appkey | Service is disabled. |
| Common | false | -1003 | Member not inlcuded in the project | Not project memeber id. |
| Common | false | -1004 | Not allowed IP | Not allow ip. |
| Common | false | -1007 | Invalid member | MemberType is invalid. |
| Common | false | -1008 | Blocked project | Service is blocked. |
| Common | false | -9995 | Invalid API version | Invalid api version. |
| Common | false | -9996 | Invalid content type. Only application/JSON | Only application/json Content-type is supported. |
| Common | false | -9997 | Invalid JSON type | Invalid API parameters. |
| Common | false | -9998 | Not exist API | Not exist API. |
| Common | false | -9999 | System error (unexpected error) | System error. Please inquire at support@toast.com. |
| Send/Query | false | -1005 | Invalid search condition | Service parameter is invalid. |
| Send/Query | false | -1006 | Invalid delivery message format (messageType) | MessageType is invalid. |
| Send/Query | false | -2000 | Invalid date format | Date format error. |
| Send/Query | false | -2001 | Recipient is missing | RecipientList can not be null. |
| Send/Query | false | -2002 |Name of attached file is invalid | Invalid attach file name. |
| Send/Query | false | -2003 | Extension of attached file is not jpg or jpeg | Attach file required jpg or jpeg. |
| Send/Query | false | -2004 | The attachment has expired or does not exist | File is expired or does not exist. |
| Send/Query | false | -2005 | Attached file is sized 300KB or more  | The file size must be greater than 0 and less than 300KB. |
| Send/Query | false | -2006 | Delivery type in template setting is not consistent with requested type | Invalid template type. |
| Send/Query | false | -2008 |  Request ID (requestId) is invalid | Invalid requestId. |
| Send/Query | false | -2009 | Attached file is not properly uploaded due to server error | Upload attach file error. |
| Send/Query | false | -2010 | Upload type of attachment is invalid (server error) | Upload attach file type can not be empty. |
| Send/Query | false | -2011 | Required query parameters are missing (requestId or startRequestDate, endRequestdate) | RequestId or start/endRequestDate or start/endCreateDate is required. |
| Send/Query | false | -2012 | When detailed query parameter is invalid (requestId or mtPr) | Search parameter is invalid.(requestId and mtPr). |
| Send/Query | false | -2014 |Title or body is missing | The recipient can not be empty. |
| Send/Query | false | -2015 | Title or body exceed maximum byte | Title or Body exceed maximum byte. |
| Send/Query | false | -2016 | The number of recipients is over 1,000 | The max recipient size is 1000. |
| Send/Query | false | -2017 | Failed to create excel | Making Excel file is failed. |
| Send/Query | false | -2018 | Recipient number is missing | RecipientNo can not be empty. |
| Send/Query | false | -2019 | Recipient number is invalid | RecipientNo is invalid. |
| Send/Query | false | -2021 | System error (failed in saving queue) | System error. Failed insert queue. |
| Send/Query | false | -2022 | Request date and time is set earlier than the current time | requestDate is not before currentDate |
| Send/Query | false | -2023 | Title or body includes characters that are not allowed (e.g. emojis) | Unacceptable characters in title and body|
| Send/Query | false | -2024 |  International delivery is sent with LMS/MMS | LMS/MMS Type is not sent to outside of Korea. |
| Send/Query | false | -4000 | Query range is more than a month | Search is possible within one month. |
| Send/Query | false | -8000 | If authenticaiton doesn't include a authentication statement | The body must contain auth guide ment. |
| Template | false | -2100 | Template ID is missing | The templateId can not be empty. |
| Template | false | -2101 | Already registered Template ID | Already used templateId. |
| Template | false | -2102 | Template name is missing | The template name can not be empty. |
| Template | false | -2103 | Sender number is missing | The sendNo can not be empty. |
| Template | false | -2104 | Delivery type is missing (0: sms, 1: mms) | The sendType can not be empty.(0-sms, 1-mms) |
| Template | false | -2105 | Body is missing | The body can not be empty. |
| Template | false | -2106 | Use or not is invalid | UseYn is invalid. |
| Template | false | -2107 | Invalid Template ID(When modifing/deleting) | Invalid template. |
| Template | false | -2108 | Category ID is missing | The categoryId can not be empty. |
| Template | false | -2109 | Template ID exceeds 50 characters | templateId length must be under 50. |
| Template | false | -2110 | Not exist Template | template is not exist. |
| Template | false | -2111 | Invalid Template parameter | template add parameter is invalid. |
| Template | false | -2112 | Exceeded the maximum available number of templates for registration (Max: 1000)  | The maximum number of registered templates. |
| Template | false | -2114 | Title is empty  | The title can not be empty. |
| Template | false | -2115 | Title exceeds 120 characters  | Title length must be under 120. |
| Template | false | -2116 | Body length exceeds 255 characters, for SMS delivery   | SMS Body length must be under 255. |
| Template | false | -2117 | Body length exceeds 4000 characters, for LMS/MMS delivery  | LMS/MMS Body length must be under 4000. |
| Template | false | -2043 | Attached file for template registration is already registered at another template | Already used attachFileId |
| Template | false | -2044 | Request is sent to unavailable country | Invalid countryCode for sending. |
| Template | false | -2045 | International sending is blocked | International sending blocked by service. |
| Template | false | -2046 | Sent to blocked country  | Blocked country by service. |
| Template | false | -2047 | Exceeded the block limit | Blocked by total indicator. |
| Category | false | -2200 | Invalid category parameter (to register) | Invalid add category parameter.(categoryName, useYn) |
| Category | false | -2201 | Invalid category parameter (to modify) | Invalid modify category parameter.(categoryId, categoryName, useYn) |
| Category | false | -2202 | Invalid category (failed to query category) | Invalid category. |
| Category | false | -2203 | Parent category does not exist | categoryParentId is invalid. |
| Category | false | -2204 | Use or not is invalid | UseYn is invalid. |
| Category | false | -2205 | Deleting the highest category | Cannot delete the highest category. |
| Category | false | -2206 | Category does not exist | category is not exist. |
| Sender number | false | -2312 | Sender number is missing or unregistered | Not regist sendno. |
| Sender number | false | -2313 | Blocked Sender number | This sendno is blocked. |
| Statistics | false | -2700 | Invalid statistics range | Invalid search period. |
| Statistics | false | -2701 | Invalid statistics search parameter | Invalid statistics search parameter. |
| Statistics | false | -2703 | Invalid detail range of statistics | Invalid duration time. |
| Statistics | false | -2704 | Invalid statistics parameter | Invalid stats parameter. |
| 080 Call Regection | false | -6000 | Call rejection is not used | Block service is not joined. |
| 080 Call Regection | false | -6001 | Refused recipient number | Recipient Number is refused. |
| 080 Call Regection | false | -6003 | Body does not include guide message on call rejection | The body must contain block guide ment. |
| 080 Call Regection | false | -6004 | Call rejection numbers are not in service | This is not a joined unsubscribeNo. |
| Tag | false | -7000 | Internal tag error (failed to call API) | Fail to call Tag API. |
| Tag | false | -7001 | Invalid paramter | Invalid parameter. |
| Tag | false | -7002 | Failed to read .csv file | Invalid csv read. |

## Result Code of Receiving

| Category | Result code | Classification | Description |
| - | - | - | - |
| Telecom Provider | 1000 | Successful | Success |
| Telecom Provider | 1001 | Failure | Server Busy |
| Telecom Provider | 1002 | Failure | Receiving number format error |
| Telecom Provider | 1003 | Failure | Reply-to number format error |
| Telecom Provider | 1019 | Failure | Exceeded TTL |
| Telecom Provider | 2000 | Failure | Delivery time exceeded |
| Telecom Provider | 2001 | Failure | Delivery failed (mobile network) |
| Telecom Provider | 2002 | Failure | Delivery failed (mobile network -> device) |
| Telecom Provider | 2003 | Failure | Device power off |
| Telecom Provider | 2004 | Failure | Device message buffer full |
| Telecom Provider | 2005 | Failure | Grey area |
| Telecom Provider | 2006 | Failure | Message deleted |
| Telecom Provider | 2007 | Failure | Temporary device issue |
| Telecom Provider | 3000 | Failure | Unavailable to transfer |
| Telecom Provider | 3001 | Failure | No subscriber available |
| Telecom Provider | 3002 | Failure | Adult authentication failed |
| Telecom Provider | 3003 | Failure | Recipient number format error or missing (unavailable number) |
| Telecom Provider | 3004 | Failure | Temporary service suspension on device |
| Telecom Provider | 3005 | Failure | Device call processing status, Not reach to the device  |
| Telecom Provider | 3006 | Failure | Incoming call denied |
| Telecom Provider | 3007 | Failure | Device unavailable to receive callback URL |
| Telecom Provider | 3008 | Failure | Other device issues |
| Telecom Provider | 3009 | Failure | Message format error |
| Telecom Provider | 3010 | Failure | Device not supporting MMS|
| Telecom Provider | 3011 | Failure | Server error |
| Telecom Provider | 3012 | Failure | Spam |
| Telecom Provider | 3013 | Failure | Service rejected |
| Telecom Provider | 3014 | Failure | Others |
| Telecom Provider | 3015 | Failure | No transfer route available |
| Telecom Provider | 3016 | Failure | Size restriction failed for attached file |
| Telecom Provider | 3017 | Failure | Number format error due to sender number (=reply number) falsification prevention service |
| Telecom Provider | 3018 | Failure | Personal mobile phone number subscribed to sender number (=reply number) falsification prevention service  |
| Telecom Provider | 3019 | Failure | Numbers not registered at sender number(=reply number) InfoBank via pre-registration of numbers |
| ETC | E911 | Failure | No attached file extension available |
| ETC | E913 | Failure | Attached file size is 0 |
| ETC | E915 | Failure | Duplicate messages |
| ETC | E919 | Failure | Resending message is prohibited during when delivery is restricted |
| ETC | E999 | Failure | Other errors |

## Query Delivery Codes
### Result Code of Receiving

| Code Value | Description | 
| - | - |
| MTR1 | Successful | 
| MTR2 | Failed | 

### Detail Result Code of Receiving 

| Code Value | Description | 
| - | - |
| MTR2_1 | Validity Check Failed | 
| MTR2_2 | Issue of Telecom Provider | 
| MTR2_3 | Issue of Device | 