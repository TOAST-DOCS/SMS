## Notification > SMS > Error Codes

## API Response Codes
| service | isSuccess | resultCode | resultMessage |
| - | - | - | - |
| Common | true | 0 | Successful |
| Common | false | -1000 | Invalid appkey |
| Common | false | -1001 | Unavailable appkey |
| Common | false | -1002 | Closed appkey |
| Common | false | -1003 | Member not included in the project |
| Common | false | -1004 | Not allowed IP |
| Common | false | -9996 | Invalid contectType. Only application/JSON |
| Common | false | -9997 | Invalid JSON format |
| Common | false | -9998 | Unavailable API |
| Common | false | -9999 | System error (unexpected error) |
| Send/Query | false | -1006 | Invalid delivery message format (messageType) |
| Send/Query | false | -2000 | Invalid date format |
| Send/Query | false | -2001 | Recipient is missing |
| Send/Query | false | -2002 | Name of attached file is invalid |
| Send/Query | false | -2003 | Extension of attached file is not jpg or jpeg |
| Send/Query | false | -2004 | Attached file is not available |
| Send/Query | false | -2005 | Attached file is sized 300KB or more |
| Send/Query | false | -2006 | Delivery type in template setting is not consistent with requested type |
| Send/Query | false | -2008 | Request ID (requestId) is invalid |
| Send/Query | false | -2009 | Attached file is not properly uploaded due to server error |
| Send/Query | false | -2010 | Upload type of attachment is invalid (server error) |
|Send/Query | false | -2011 | Required query parameters are missing (requestId or startRequestDate, endRequestdate)
| Send/Query | false | -2012 | When detailed query parameter is invalid (requestId or mtPr) |
| Send/Query | false | -2014 | Title or body is missing  |
| Send/Query | false | -2016 | The number of recipients is over 1,000 |
| Send/Query | false | -2017 | Failed to create excel |
| Send/Query | false | -2018 | Recipient number is missing |
| Send/Query | false | -2019 | Recipient number is invalid  |
| Send/Query | false | -2021 | System error (failed in saving queue) |
| Send/Query | false | -2022 | Request date and time is set earlier than the current time |
| Send/Query | false | -2023 | Title or body includes characters that are not allowed (e.g. emojis) |
| Send/Query | false | -4000 | Query range is more than a month |
| Template | false | -2100 | Template ID is missing |
| Template | false | -2101 | Already-registered Template ID |
| Template | false | -2102 | Template name is missing |
| Template | false | -2103 | Sender number is missing |
| Template | false | -2104 | Delivery type is missing (0: sms, 1: mms) |
| Template | false | -2105 | Body is missing |
| Template | false | -2106 | Incorrect if service is enabled |
| Template | false | -2107 | Invalid template ID (to modify/delete) |
| Template | false | -2108 | Category ID is missing|
| Template | false | -2109 | Template ID exceeds 50 characters |
| Template | false | -2110 | Template is unavailable |
| Template | false | -2111 | 유효하지 않는 템플릿 파라미터인 경우 |
| Template | false | -2112 | 최대 등록 가능한 템플릿 갯수를 초과한 경우(최대: 1000)  |
| Template | false | -2113 | 이미 삭제된 템플릿 ID  |
| Template | false | -2114 | 제목이 비어있는 경우  |
| Template | false | -2115 | 제목이 120글자를 초과하는 경우  |
| Template | false | -2116 | 발송 유형이 SMS일때, 본문 길이가 255글자를 초과하는 경우  |
| Template | false | -2117 | 발송 유형이 LMS/MMS일때, 본문 길이가 4000글자를 초과하는 경우  |
| Category | false | -2200 | Invalid category parameter (to register) |
| Category | false | -2201 | Invalid category parameter (to modify) |
| Category | false | -2202 | Invalid category (failed to query category |
| Category | false | -2203 | 부모 카테고리가 존재하지 않은 경우 |
| Category | false | -2204 | 사용 여부가 잘못된 경우 |
| Category | false | -2205 | 최상위 카테고리를 삭제하려는 경우 |
| Sender Number | false | -2312 | Sender number is missing or unregistered |
| Sender Number | false | -2313 | Blocked sender number |
| Sender Number | false | -2314 | Request parameter for sender number registration is invalid |
| Statistics | false | -2700 | Invalid range of statistics |
| Statistics | false | -2701 | Invalid statistics search parameter |
| Statistics | false | -2703 | Invalid detail range of statistics |
| 080 Call Rejection | false | -6000 | Call rejection is not in service |
| 080 Call Rejection | false | -6001 | Call-reject numbers |
| 080 Call Rejection | false | -6003 | Body does not include guide message on call rejection |
| 080 Call Rejection | false | -6004 | Call rejection numbers are not in service |
| Tag | false | -7000 | Internal tag error (failed to call API) |
| Tag | false | -7001 | Invalid parameter |
| Tag | false | -7002 | Failed to read .csv file  |

## Query Delivery Codes 
### Result Code of Receiving 
<table class="table table-striped table-hover">
<thead>
	<tr>
    <th>Code Value</th>
    <th>Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>MTR1</td>
    <td>Successful</td>
  </tr>
  <tr>
    <td>MTR2</td>
    <td>Failed</td>
  </tr>
</tbody>
</table>

### Detail Result Code of Receiving 
<table class="table table-striped table-hover">
<thead>
	<tr>
    <th>Code Value</th>
    <th>Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>MTR2_1</td>
    <td>Validity Check Failed</td>
  </tr>
  <tr>
    <td>MTR2_2</td>
    <td>Issue of Telecom Provider</td>
  </tr>
  <tr>
    <td>MTR2_3</td>
    <td>Issue of Device</td>
  </tr>
</tbody>
</table>

## EMMA v.3 Result Code of Receiving 
EMMA Version: EMMA V3.3.0 or higher

1) Telecom Provider: Result codes received after transferred to telecom provider.

2) IB G/W: Result codes delivered after Infobank G/W receives messages.

3) IB EMMA: Error codes processed by EMMA regarding message delivery requests. 

<table class="table table-striped table-hover">
<thead>
	<tr>
		<th>Category</th>
		<th>Code Value</th>
		<th>Classification</th>
		<th>Description</th>
	</tr>
</thead>
<tbody>
	<tr>
		<td rowspan=29>Telecom Provider</td>
		<td>1000</td>
		<td>Success</td>
		<td>Successful</td>
	</tr>
	<tr>
		<td>2000</td>
		<td>failure</td>
		<td>Delivery time exceeded</td>
	</tr>
	<tr>
		<td>2001</td>
		<td>failure</td>
		<td>Delivery failed (mobile network)</td>
	</tr>
	<tr>
		<td>2002</td>
		<td>failure</td>
		<td>Delivery failed (mobile network -> device)</td>
	</tr>
	<tr>
		<td>2003</td>
		<td>failure</td>
		<td>Device power off</td>
	</tr>
	<tr>
		<td>2004</td>
		<td>failure</td>
		<td>Device message buffer pool</td>
	</tr>
	<tr>
		<td>2005</td>
		<td>failure</td>
		<td>Grey area</td>
	</tr>
	<tr>
		<td>2006</td>
		<td>failure</td>
		<td>Message deleted</td>
	</tr>
	<tr>
		<td>2007</td>
		<td>failure</td>
		<td>Temporary device issue</td>
	</tr>
	<tr>
		<td>3000</td>
		<td>Invalid</td>
		<td>Unavailable to transfer</td>
	</tr>
	<tr>
		<td>3001</td>
		<td>Invalid</td>
		<td>No subscriber available</td>
	</tr>
	<tr>
		<td>3002</td>
		<td>Invalid</td>
		<td>Adult authentication failed</td>
	</tr>
	<tr>
		<td>3003</td>
		<td>Invalid</td>
		<td>Error of recipient number type</td>
	</tr>
	<tr>
		<td>3004</td>
		<td>Invalid</td>
		<td>Temporary service suspension on device</td>
	</tr>
	<tr>
		<td>3005</td>
		<td>Invalid</td>
		<td>Device call processing status</td>
	</tr>
	<tr>
		<td>3006</td>
		<td>Invalid</td>
		<td>Incoming call denied </td>
	</tr>
	<tr>
		<td>3007</td>
		<td>Invalid</td>
		<td>Phone unavailable to receive callback URL </td>
	</tr>
	<tr>
		<td>3008</td>
		<td>Invalid</td>
		<td>Other device issues</td>
	</tr>
	<tr>
		<td>3009</td>
		<td>Invalid</td>
		<td>Message format error </td>
	</tr>
	<tr>
		<td>3010</td>
		<td>Invalid</td>
		<td>Device not supporting MMS</td>
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
		<td>Service rejected</td>
	</tr>
	<tr>
		<td>3014</td>
		<td>Invalid</td>
		<td>Others </td>
	</tr>
	<tr>
		<td>3015</td>
		<td>Invalid</td>
		<td>No transfer route available</td>
	</tr>
	<tr>
		<td>3016</td>
		<td>Invalid</td>
		<td>Size restriction failed for attached file</td>
	</tr>
	<tr>
		<td>3017</td>
		<td>Invalid</td>
		<td>Number format error due to sender number (=reply number) falsification prevention service </td>
	</tr>
	<tr>
		<td>3018</td>
		<td>Invalid</td>
		<td>Personal mobile phone number subscribed to sender number (=reply number) falsification prevention service </td>
	</tr>
	<tr>
		<td>3019</td>
		<td>Invalid</td>
		<td>Numbers not registered at sender number(=reply number) InfoBank via pre-registration of numbers </td>
	</tr>
	<tr>
		<td rowspan=20>IB G/W</td>
		<td>1001</td>
		<td rowspan=20></td>
		<td>Server Busy (internal RS saving queue Full)</td>
	</tr>
	<tr>
		<td>1002</td>
		<td> Error in recipient number format</td>
	</tr>
	<tr>
		<td>1003</td>
		<td> Error in reply number format</td>
	</tr>
	<tr>
		<td>1004</td>
		<td>SPAM</td>
	</tr>
	<tr>
		<td>1005</td>
		<td> Available usage exceeded</td>
	</tr>
	<tr>
		<td>1006</td>
		<td>No Attached file available</td>
	</tr>
	<tr>
		<td>1007</td>
		<td> Attached file available </td>
	</tr>
	<tr>
		<td>1008</td>
		<td>Failed to save attached file</td>
	</tr>
	<tr>
		<td>1009</td>
		<td> No CLIENT_MSG_KEY available</td>
	</tr>
	<tr>
		<td>1010</td>
		<td>No content available</td>
	</tr>
	<tr>
		<td>1011</td>
		<td> No callback available</td>
	</tr>
	<tr>
		<td>1012</td>
		<td> No RECIPIENT_INFO available </td>
	</tr>
	<tr>
		<td>1013</td>
		<td> No subject available </td>
	</tr>
	<tr>
		<td>1014</td>
		<td>No attached file key available </td>
	</tr>
	<tr>
		<td>1015</td>
		<td>No attached file name available </td>
	</tr>
	<tr>
		<td>1016</td>
		<td> No attached file size available</td>
	</tr>
	<tr>
		<td>1017</td>
		<td> No attached file content available</td>
	</tr>
	<tr>
		<td>1018</td>
		<td> No transfer authority available</td>
	</tr>
	<tr>
		<td>1019</td>
		<td>TTL exceeded</td>
	</tr>
	<tr>
		<td>1020</td>
		<td>charset conversion error {@확인 - 한글로 수정해도 되나요.}</td>
	</tr>
	<tr>
		<td rowspan=24>IB EMMA</td>
		<td>E900</td>
		<td rowspan=24>Invalid-IB</td>
		<td>No transfer key available</td>
	</tr>
	<tr>
		<td>E901</td>
		<td>No recipient number available</td>
	</tr>
	<tr>
		<td>E902</td>
		<td>Order of recipient number (for broadcasts) is unavailable </td>
	</tr>
	<tr>
		<td>E903</td>
		<td>No title available</td>
	</tr>
	<tr>
		<td>E904</td>
		<td>No message available</td>
	</tr>
	<tr>
		<td>E905</td>
		<td>No reply number available</td>
	</tr>
	<tr>
		<td>E906</td>
		<td>No message key available</td>
	</tr>
	<tr>
		<td>E907</td>
		<td>Availability of broadcast message is unavailable </td>
	</tr>
	<tr>
		<td>E908</td>
		<td>No service type available</td>
	</tr>
	<tr>
		<td>E909</td>
		<td>Request hour for transfer is unavailable</td>
	</tr>
	<tr>
		<td>E910</td>
		<td>TTL time unavailable</td>
	</tr>
	<tr>
		<td>E911</td>
		<td>No attached file extension available, in the case of MMS MT</td>
	</tr>
	<tr>
		<td>E912</td>
		<td>No attached file available for attach_file folder, in the case of MMS MT</td>
	</tr>
	<tr>
		<td>E913</td>
		<td>If attached file is sized 0, in the case of MMS MT</td>
	</tr>
	<tr>
		<td>E914</td>
		<td>If data are available at message table but not at file table, in the case of MMS MT</td>
	</tr>
	<tr>
		<td>E915</td>
		<td>Duplicate message </td>
	</tr>
	<tr>
		<td>E916</td>
		<td>Prohibited number for authenticated server</td>
	</tr>
	<tr>
		<td>E917</td>
		<td>Prohibited number for customer database</td>
	</tr>
	<tr>
		<td>E918</td>
		<td>USER CALLBACK FAIL</td>
	</tr>
	<tr>
		<td>E919</td>
		<td>Resending message is prohibited during when delivery is restricted</td>
	</tr>
	<tr>
		<td>E920</td>
		<td>If file group key is available at message table, in the case of LMS MT</td>
	</tr>
	<tr>
		<td>E921</td>
		<td>If file group key is unavailable at message table, in the case of MMS MT</td>
	</tr>
	<tr>
		<td>E922</td>
		<td>Error in restricted use of characters for broadcast words </td>
	</tr>
	<tr>
		<td>E999</td>
		<td> Other errors </td>
	</tr>
	<tr>
		<td rowspan=10> IB authentication server</td>
		<td>1000</td>
		<td rowspan=10></td>
		<td> Successful </td>
	</tr>
	<tr>
		<td>1001</td>
		<td> Unavailable ID </td>
	</tr>
	<tr>
		<td>1002</td>
		<td> Authentication error </td>
	</tr>
	<tr>
		<td>1003</td>
		<td> Internal server error (e.g. failed database access)</td>
	</tr>
	<tr>
		<td>1004</td>
		<td> Invalid client password </td>
	</tr>
	<tr>
		<td>1005</td>
		<td> Public key already registered </td>
	</tr>
	<tr>
		<td>1006</td>
		<td> Client public key duplicated</td>
	</tr>
	<tr>
		<td>1007</td>
		<td>IP Address authentication failed </td>
	</tr>
	<tr>
		<td>1008</td>
		<td>MAC address authentication failed </td>
	</tr>
	<tr>
		<td>1009</td>
		<td>Service denied (customer access prohibited)</td>
	</tr>
</tbody>
</table>
