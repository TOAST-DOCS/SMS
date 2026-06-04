## Notification > SMS > Result Code

## API Result Code

| Category | Success | Result Code | Result Code Message | API Response Message | 
| - | - |-------| - | - |
| Common | true | 0     | Success | SUCCESS |
| Common | false | 4     | Parameter validation failed | | 
| Common | false | -1000 | Invalid appKey | Invalid appKey. |
| Common | false | -1001 | Non-existent appKey | Service is not exist. |
| Common | false | -1002 | Terminated appKey | Service is disabled. |
| Common | false | -1003 | Member not included in project | Not project memeber id. |
| Common | false | -1004 | IP not allowed | Not allow ip. |
| Common | false | -1007 | Invalid member | MemberType is invalid. |
| Common | false | -1008 | Blocked project | Service is blocked. |
| Common | false | -9995 | Invalid API version | Invalid api version. |
| Common | false | -9996 | Invalid contentType. Only application/JSON | Only application/json Content-type is supported. |
| Common | false | -9997 | Invalid JSON format | Invalid API parameters. |
| Common | false | -9998 | Non-existent API | Not exist API. |
| Common | false | -9999 | System error (unexpected error) | System error. Please inquire at support@toast.com. |
| Send/Query | false | -1005 | Invalid search condition | Service parameter is invalid. | 
| Send/Query | false | -1006 | Invalid message type (messageType) | MessageType is invalid. |
| Send/Query | false | -2000 | Invalid date format | Date format error. |
| Send/Query | false | -2001 | Empty recipients | RecipientList can not be null. |
| Send/Query | false | -2002 | Invalid attachment file name | Invalid attach file name. |
| Send/Query | false | -2003 | Attachment file extension is not jpg or jpeg | Attach file required jpg or jpeg. |
| Send/Query | false | -2004 | Attachment file is expired or does not exist | File is expired or does not exist. | 
| Send/Query | false | -2005 | Attachment file size exceeds 300KB | The file size must be greater than 0 and less than 300KB. |
| Send/Query | false | -2006 | Send type set in template does not match requested send type | Invalid template type. |
| Send/Query | false | -2008 | Invalid request ID (requestId) | Invalid requestId. |
| Send/Query | false | -2009 | Attachment file not uploaded properly due to server error during upload | Upload attach file error. | 
| Send/Query | false | -2010 | Invalid attachment file upload type (server error) | Upload attach file type can not be empty. |
| Send/Query | false | -2011 | Required query parameter is empty (requestId or startRequestDate, endRequestdate) | RequestId or start/endRequestDate or start/endCreateDate is required. | 
| Send/Query | false | -2012 | Invalid detailed query parameter (requestId or mtPr) | Search parameter is invalid.(requestId and mtPr). |
| Send/Query | false | -2014 | Title or body is empty | The recipient can not be empty. |
| Send/Query | false | -2015 | Title or body exceeded | Title or Body exceed maximum byte. |
| Send/Query | false | -2016 | More than 1,000 recipients | The max recipient size is 1000. |
| Send/Query | false | -2017 | Excel creation failed | Making Excel file is failed. |
| Send/Query | false | -2018 | Recipient number is empty | RecipientNo can not be empty. |
| Send/Query | false | -2019 | Recipient number is invalid | RecipientNo is invalid. |
| Send/Query | false | -2021 | System error (queue storage failure) | System error. Failed insert queue. |
| Send/Query | false | -2022 | Request date set to before current time | RequestDate is not before currentDate. |
| Send/Query | false | -2023 | Title or body contains unacceptable characters (emojis, etc.) | Unacceptable characters in title and body. |
| Send/Query | false | -2024 | LMS/MMS international delivery | LMS/MMS Type is not sent to outside of Korea. |
| Send/Query | false | -2044 | Request to country where delivery is not possible | Invalid countryCode for sending. |
| Send/Query | false | -2045 | International delivery blocked | International sending blocked by service. |
| Send/Query | false | -2046 | Delivery to blocked country | Blocked country by service. |
| Send/Query | false | -2047 | Blocked limit count exceeded | Blocked by total indicator. |
| Send/Query | false | -2048 | International delivery body exceeded | International message body exceed maximum length. |
| Send/Query | false | -2050 | International delivery conversion failed (not in convertible state) | Conversion status is not ready. |
| Send/Query | false | -2051 | Delivery failed due to conversion rate-based blocking | Conversion rate is lower than threshold. |
| Send/Query | false | -2052 | Delivery failed due to exceeding monthly delivery limit per organization | Blocked by organization message sending count exceed. |
| Send/Query | false | -2053 | International delivery failed due to daily sending limit per country | Blocked by daily country send limit. |
| Send/Query | false | -4000 | Query range exceeds one month | Search is possible within one month. |
| Send/Query | false | -8000 | Authentication delivery does not contain authentication text | The body must contain auth guide ment. |
| Template | false | -2100 | Template ID is empty | The templateId can not be empty. |
| Template | false | -2101 | Already registered template ID | Already used templateId. |
| Template | false | -2102 | Template name is empty | The template name can not be empty. |
| Template | false | -2103 | Sender number is empty | The sendNo can not be empty. |
| Template | false | -2104 | Send type is empty (0: sms, 1: mms) | The sendType can not be empty.(0-sms, 1-mms) |
| Template | false | -2105 | Body is empty | The body can not be empty. |
| Template | false | -2106 | Invalid use status | UseYn is invalid. |
| Template | false | -2107 | Invalid template ID (when modifying/deleting) | Invalid template. |
| Template | false | -2108 | Category ID is empty | The categoryId can not be empty. |
| Template | false | -2109 | Template ID exceeds 50 characters | TemplateId length must be under 50. |
| Template | false | -2110 | Template does not exist | Template is not exist. |
| Template | false | -2111 | Invalid template parameter | Template add parameter is invalid. |
| Template | false | -2112 | Maximum registrable template count exceeded (max: 1000) | The maximum number of registered templates. |
| Template | false | -2114 | Title is empty | The title can not be empty. |
| Template | false | -2115 | Title exceeds 120 characters | Title length must be under 120. |
| Template | false | -2116 | Body length exceeds 255 characters when send type is SMS | SMS Body length must be under 255. |
| Template | false | -2117 | Body length exceeds 4000 characters when send type is LMS/MMS | LMS/MMS Body length must be under 4000. |
| Template | false | -2043 | Attachment file to be registered in template is already registered in another template | Already used attachFileId |
| Category | false | -2200 | Invalid category parameter (when registering) | Invalid add category parameter.(categoryName, useYn) |
| Category | false | -2201 | Invalid category parameter (when modifying) | Invalid modify category parameter.(categoryId, categoryName, useYn) |
| Category | false | -2202 | Invalid category (category query failure) | Invalid category. |
| Category | false | -2203 | Parent category does not exist | CategoryParentId is invalid. |
| Category | false | -2204 | Invalid use status | UseYn is invalid. |
| Category | false | -2205 | Attempting to delete top-level category | Cannot delete the highest category. |
| Category | false | -2206 | Non-existent category | Category is not exist. |
| Sender Number | false | -2312 | Sender number is empty or unregistered | Not regist sendno. |
| Sender Number | false | -2313 | Blocked sender number | This sendno is blocked. |
| Statistics | false | -2700 | Invalid statistics range | Invalid search period. |
| Statistics | false | -2701 | Invalid statistics search parameter | Invalid statistics search parameter. | 
| Statistics | false | -2703 | Invalid statistics detail range | Invalid duration time. |
| Statistics | false | -2704 | Invalid statistics parameter | Invalid stats parameter. |
| Statistics | false | -2706 | Statistics internal error (API call failure) | Failed read stats. |
| 080 Unsubscribe | false | -6000 | Not using unsubscribe service | Block service is not joined. |
| 080 Unsubscribe | false | -6001 | Unsubscribed number | Recipient Number is refused. |
| 080 Unsubscribe | false | -6003 | No unsubscribe guide message in body | The body must contain block guide ment. |
| 080 Unsubscribe | false | -6004 | Unsubscribe number is empty or not subscribed | This is not a joined unsubscribeNo. |
| Tag | false | -7000 | Tag internal error (API call failure) | Fail to call Tag API. |
| Tag | false | -7001 | Invalid parameter | Invalid parameter. |
| Tag | false | -7002 | .csv read failure | Invalid csv read. |

## Reception Result Code

| Classification | Result Code | Category | Meaning |
| - | - | - | - |
| Carrier | 1000 | Success | Success |
| Carrier | 1001 | Failure | Server Busy |
| Carrier | 1002 | Failure | Recipient number format error |
| Carrier | 1003 | Failure | Sender number format error |
| Carrier | 1019 | Failure | TTL exceeded |
| Carrier | 2000 | Failure | Transmission timeout |
| Carrier | 2001 | Failure | Transmission failure (wireless network) |
| Carrier | 2002 | Failure | Transmission failure (wireless network > handset) |
| Carrier | 2003 | Failure | Handset power off |
| Carrier | 2004 | Failure | Message buffer full between carrier and handset, delivery impossible |
| Carrier | 2005 | Failure | Shadow area |
| Carrier | 2006 | Failure | Message deleted |
| Carrier | 2007 | Failure | Temporary handset problem |
| Carrier | 3000 | Failure | Cannot transmit |
| Carrier | 3001 | Failure | No subscriber |
| Carrier | 3002 | Failure | Adult verification failed |
| Carrier | 3003 | Failure | Recipient number format error or non-existent number |
| Carrier | 3004 | Failure | Handset service temporarily suspended |
| Carrier | 3005 | Failure | Handset call processing state, unable to reach handset |
| Carrier | 3006 | Failure | Call rejected |
| Carrier | 3007 | Failure | Handset unable to receive callback URL |
| Carrier | 3008 | Failure | Other handset problems |
| Carrier | 3009 | Failure | Message format error |
| Carrier | 3010 | Failure | MMS unsupported handset |
| Carrier | 3011 | Failure | Server error |
| Carrier | 3012 | Failure | Spam |
| Carrier | 3013 | Failure | Service denied |
| Carrier | 3014 | Failure | Other |
| Carrier | 3015 | Failure | No transmission path |
| Carrier | 3016 | Failure | Attachment file size limit failure |
| Carrier | 3017 | Failure | Number format error due to caller number anti-spoofing service |
| Carrier | 3018 | Failure | Mobile phone individual subscriber number subscribed to caller number anti-spoofing service |
| Carrier | 3019 | Failure | Sender number blocked by KISA or Ministry of Future for all customers |
| International | 4001 | Failure | Signature format error |
| International | 4002 | Failure | Sender number error |
| International | 4003 | Failure | Recipient number error |
| International | 4004 | Failure | Temporary handset problem |
| International | 4005 | Failure | No subscriber |
| International | 4006 | Failure | Failure due to recipient error |
| International | 4007 | Failure | Carrier error or blocking |
| International | 4008 | Failure | Spam |
| International | 4009 | Failure | Temporary network error |
| International | 4010 | Failure | Failure due to abnormal transmission pattern |
| ETC | E900 | Failure | Other delivery error |
| ETC | E911 | Failure | Attachment file has no extension |
| ETC | E913 | Failure | Attachment file size is 0 |
| ETC | E915 | Failure | Duplicate message |
| ETC | E919 | Failure | During delivery restriction time, message resend processing prohibited |
| ETC | E999 | Failure | Other error |

## DLR Result Code
### DLR Status Code
| DLR Status Code | Meaning |
| - | - |
| DELIVERED | Message has been transmitted to handset |
| ACCEPTED | Message has been accepted but not yet transmitted |
| BUFFERED | Message has been accepted and is queued |
| EXPIRED | Message transmission failed within expiry deadline due to carrier's retransmission policy |
| FAILED | Message transmission failed |
| REJECTED | Carrier rejected message transmission |
| UNKNOWN | Unknown |

### DLR Error Code
| DLR Error Code | Meaning | Description |
| - | - | - |
| 0 | Delivered | Message successfully transmitted |
| 1 | Unknown | Message not transmitted for unknown reason |
| 2 | Absent subscriber - temporary | Message not transmitted due to temporary handset unavailability - please retry |
| 3 | Absent subscriber - permanent | This number is no longer active and should be removed from database |
| 4 | Number blocked by recipient | Permanent error requiring removal from database and carrier contact to lift block |
| 5 | Portability error | Number portability issue requiring carrier contact for resolution |
| 6 | Anti-spam filter block | Message blocked by carrier's anti-spam filter |
| 7 | Handset busy | Handset unavailable during message transmission - please retry |
| 8 | Network error | Message transmission failed due to network error - please retry |
| 9 | Invalid number | Recipient specifically requested not to receive messages from certain services |
| 11 | Unroutable | NHN Cloud unable to find appropriate route for message transmission - contact customer support |
| 12 | Unreachable destination | Cannot find route to this number - check recipient number |
| 13 | Recipient age restriction | Cannot receive message due to recipient age restriction |
| 14 | Number blocked by carrier | Contact carrier to enable SMS service on recipient's plan |
| 16 | Gateway quota exceeded | Message transmission failed due to exceeding allowed requests per period. This error applies only to accounts registered in the US and France |
| 20 | Anti-fraud traffic rule | Message rejected due to traffic pumping - contact customer support |
| 21 | Abnormal consecutive sending detected | High-density recipient number range threshold exceeded |
| 22 | Abnormal traffic surge detected | Relative increase threshold exceeded |
| 39 | Invalid sender address for US destination | Message transmission to US failed due to sender number issue - contact customer support |
| 51 | Header filter | Message transmission to US failed due to sender number issue - contact customer support |
| 53 | Consent filter | Message transmission failed due to lack of consent |
| 54 | Regulatory error | Unexpected regulatory error - contact customer support |
| 99 | General error | Generally indicates routing error - contact customer support |
| 1000 | Other error | Other error |

## Result Query Code
### Reception Result Query Code

| Code Value | Meaning | 
| - | - |
| MTR1 | Success | 
| MTR2 | Failure | 

### Reception Result Query Detail Code

| Code Value | Meaning | 
| - | - |
| MTR2_1 | Validation failure | 
| MTR2_2 | Carrier problem | 
| MTR2_3 | Handset problem |