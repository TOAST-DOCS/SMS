## Notification > SMS > Overview 

The text message delivery system enables to send SMS, LMS, or MMS, schedule delivery, manage templates, and query history of deliveries. 
RESTful API is provided for easy integration. 

## Specifications 

- Send SMS, LMS, and MMS. 
- Support Mass Delivery 
  - Enter the list of recipients in excel and send SMS in mass. 
- Schedule Delivery 
  -	Text messages at a time of choice. 
- Provide Replacement Tags 
  -	Send personalized SMS for each recipient by using replacement tags. 
- Support Templates
  - Register frequently-used SMS as templates. 

## Main Features 

RESTful API is provided to send and query text messages from client's applications.   
UIs are supported to send SMS, query delivery history, and manage templates. 


## Reference 

<span id='tag-uid'></span>
### Tags and UIDs

#### Glossary
| Term    | Description                                                  |
| ------- | ------------------------------------------------------------ |
| Tag     | A system that classifies UIDs. <br>Many tags can be attached to an UID so as to help users to easily search and use UID information. |
| UID     | ID (identifier) that classifies users. <br>One UID can have multiple contacts to be applied for delivery. |
| Contact | A specified location to contact. <br>Notification provides three products to register contact: Push, Email, and SMS. <br>Push regards to tokens; Email to mail addresses; and, SMS to phone numbers. |

#### Use Tags to Send SMS 
You can text by selecting tags, instead of phone numbers, as recipient information. 

1. Register UID.

* Go to **Manage UIDs** and register UID and one or many phone numbers. 
* For more details, see [Manage UIDs](./console-guide/#uid).

2. Register tags.

* Go to **Manage Tags** to register tags. 
* For more details, see [Manage Tags](./console-guide/#_15).

3. Register UID to a tag. 

* Go to **Manage Tags** to register UID to a registered tag. 

4. Select tags to send text messages. 

* Go to **Send General SMS** and select **Send Tags** instead of mail addresses, and register tags.
* Mails are to be sent to phone numbers of UID which is registered to a tag. 
* For more details, see [Send SMS using Tags](./console-guide/#_8).

#### Tags of Other Services 
* You can share your tag and UI information of Email, with Push or SMS in a same project, with no need of re-registration. 
* Other contacts can be added to a same UID of each product console. 

### Statistics Event Key and Statistics 
#### Terminology 
| Term           | Description                                       |
| ------------ | ---------------------------------------- |
| Statistics Event Key | Event key enabled to collect statistics by specific unit. |
| statsId | Original ID Of a statistics event key, mostly each value called via API.  |

#### Extracting Statistics by Specific Unit for Message Delivery 
1. Go to the **Statistics Event Key Management** tab and register statistics event key. To send via API, statistics ID (statsId) must be acquired from this page. 
2. To send messages via console or API, statistics event key must be sent as well. 
2-1. Sending from Console  
    * To send text messages from the **SMS Delivery** tab, select statistics event key.  
    * Enter all message information and click **Send**.  
    * After some time, check statistics data on the **Statistics** tab. 
2-2. Sending via API
    * Put statsId gained from the **Statistics Event Key Management** tab into message delivery parameter. 
    * After some time, check statistics data on the **Statistics** tab. 

### 데이터 보관 기간
* 데이터 보관 정책에 따라, 최근 180일의 발송 이력을 보관합니다.
* 서비스에 이용된 첨부 파일은 7일간만 보관되며 그 이후에는 삭제되어 조회할 수 없습니다.
* 다만, 템플릿에 등록된 첨부 파일 및 증빙 서류(통신 서비스 이용 증명원)는 서비스를 이용하는 동안 보관됩니다.