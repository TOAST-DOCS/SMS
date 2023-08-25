## Notification > SMS > Service Policy > Sending Policy

<span id="private-policy"></span>
## Guide for Notice of Personal Information Assignor

When the Customer uses NHN Cloud > SMS Service, assignment of personal information between the Customer and the Company arises, and the assignee, the Customer, is obliged to disclose the status (assignor and content of business) of his assignment of personal information to the Company, through the personal information handling policy, in accordance with the Act on Promotion of Information and Communications Network Utilization and Information Protection, and the Personal Information Protection Act.

Accordingly, the Company may provide guidelines as below for the Customer, to abide by relevant regulations in the use of NHN Cloud SMS Service and not to be adversely affected for not disclosing his assignment status:

(Example)
[Notice of Personal Information Assignor]
To use NHN Cloud SMS Service, make sure the following is displayed for 'Personal Information Handling Policy' > Assignment Status of the Customer.
Assignor: NHN Cloud
Content of Business: Send SMS in lieu of customers

<span id='fabrication-number'></span>
## Prevention of Caller Number Fabrication
+ To enable SMS, your own (or corporate-owned) caller number must be registered first.
+ If you use other's (or other company's) caller number, in accordance with <a href="https://www.msit.go.kr/bbs/view.do?sCode=user&mId=108&mPid=103&bbsSeqNo=83&nttSeqNo=1259891" target="_blank">(Notice 2015-32 by the Ministry of Science, ICT and Future Planning) Public notice on preventing damage on users due to falsified phone numbers]</a> and <a href="https://www.toast.com/terms/terms-service" target="_blank">[NHN Cloud Terms of Services]</a>, take note that following measures may be taken.


```
ㆍIf a message is sent from falsified sender number, such circuit or service of the user shall be suspended until investigation on the corresponding issue is completed.
  (Nevertheless, if such falsification turns out to be made by mistake, without malicious purposes, service may be resumed upon user's calling and evaluation.)
ㆍService shall be restricted if the user's system is designed to send messages also from other numbers, as well as registered sender numbers.
ㆍDamages shall be claimed for all other losses incurred out of sender number falsification
```

<span id="fraud-number"></span>
## Filter Messages from Spoofed Numbers
The 'Filter Messages from Spoofed Numbers' service protects user's own phone number from potentially spoofed for criminal acts or spamming.

### Please Check
+ As a free service provided by each telecommunication provider (e.g. SKT, KT, LGU+, or MVNO providers), it only requires your agreement to get registered.
+ If your text delivery is confirmed as 'Failure' on the website, even after it is sent to an appropriate number, see if your 'Filter Messages from Spoofed Numbers' service is enabled.
+ Disable 'Filter Messages from Spoofed Numbers' first, and then try sending your message.
+ It takes about 7 days to get disabled after it is applied.

### Disabling Filter Messages from Spoofed Numbers for Each Provider
+ On Website
    + For SKT: [[Go to Disable Service](http://www.tworld.co.kr/normal.do?serviceId=S_PROD2001&viewId=V_PROD2001&prod_id=NA00004406)]
    + For KT: [[Go to Disable Service](https://product.kt.com/wDic/productDetail.do?ItemCode=1047)]
    + For U+: [[Go to Disable Service](http://www.uplus.co.kr/css/pord/cosv/cosv/RetrievePsMbSDmsgInfo.hpi?catgCd=50501&prodCdKey=LRZ0002297)]
+ Via Customer Center
    + Press 114 on mobile phone + Call
    + Call 1599-0011 for SKT, 100 for KT Olleh, or 1544-0010 for LG U+

<span id="spam-number"></span>
## Filter Spams by Telecommunication Providers
+ The service is provided by each telecommunication provider to automatically block annoying spams.
+ Texts considered as spams, by each provider's criteria, are sent to Spam Inbox, instead of Message Inbox.

### Please Check
+ If your delivery result is confirmed as successful but message is still not received, see if your Filter Spams service is enabled.
+ In accordance with the comprehensive measures set by the Anti-Spam Center of Korea Internet & Security Agency, each telecommunication provider provides 'Spam-Filtering' service.
+ If your message is saved at Spam Inbox, instead of Messages Inbox, disable the spam-filtering service first.
+ Due to policy to protect personal information, no one else but you must enable or disable the service.

### Disabling Filter Spams for Each Provider
+ On Website
    + For SKT: [[Go to Disable Service](http://www.tworld.co.kr/normal.do?serviceId=S_PROD2001&viewId=V_PROD2001&prod_id=NA00002121)]
    + For KT: [[Go to Disable Service](https://product.kt.com/wDic/productDetail.do?ItemCode=479)]
    + For U+: [[Go to Disable Service](http://www.uplus.co.kr/css/pord/cosv/cosv/RetrievePsMbSDmsgInfo.hpi?catgCd=51436&prodCdKey=LRZ0000277&mid=315)]
+ Via Customer Center
    + Press 114 on mobile phone + Call
    + Call 1599-0011 for SKT, 100 for KT Olleh, or 1544-0010 for LG U+

<span id="rejection-of-receiving-080"></span>
### Unsubscribing 080 Numbers
+ With Unsubscribe 080 Numbers, recipients can reject receiving ad messages.
+ Ads must be sent along with how to unsubscribe for free, so that recipients can reject or withdraw consent to receiving ads, without a charge.
### Please Check
+ Charges for Unsubscribe 080 Numbers are not based on the delivery time, but upon the public release of each number (monthly fixed charges).
+ It takes about 3 to 4 days to get a new number for unsubscription, so one-off or repetitive service cancellation or application is not recommended.
+ Please note that a service opening cannot be cancelled while registration is reserved.
+ If you want to use the registered 080 call rejection number in other projects, you can use the **Share the blocked 080 phone numbers** feature.
+ 080 numbers that are cancelled from unsubscription or applied externally, delivery shall fail.

## Guide for Sending Speed Depending on the Size of MMS Attachments
+ When sending MMS, there may be a difference in the sending speed depending on the size of the attachments.
+ The larger the size of uploaded attachments is, the slower the sending speed and the update of reception result may become due to carrier's sending speed restrictions.
+ If you want faster delivery, we recommend that you make a request by reducing the size of attachments.

## Guide for sending contents according to character set
+ When received, messages included in EUC-KR normally appear the same as the contents sent.
+ When characters not included in EUC-KR are included in the title/body, the contents may appear as broken characters such as '?'.
     + Depending on the type of receiving device and carrier, the contents of the message may appear differently.

## Timeout Policy for Message Receiving Result
+ Depending on the device and communication status, there may be a delay in updating message receiving results.
+ In case of delay in receiving messages, delivery is attempted according to the NHN Cloud resending policy.
+ The resending policy is as follows.

| Send Type | Timeout duration | After Timeout |
|---|---|---|
| SMS | 25 hour | Do not retry. Update of receive failure result (Result code: 2000) |
| LMS | 80 hour | Do not retry. Update of receive failure result (Result code: 2000) |
| MMS | 80 hour | Do not retry. Update of receive failure result (Result code: 2000) |