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
