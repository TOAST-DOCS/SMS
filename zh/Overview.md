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

### 통계 이벤트 키와 통계
#### 서비스 용어
| 용어           | 설명                                       |
| ------------ | ---------------------------------------- |
| 통계 이벤트 키 | 통계를 특정 단위로 묶어서 보고 싶을 때 사용하는 이벤트 키입니다. |
| statsId | 통계 이벤트 키의 고유 ID 입니다. API로 호출 시 해당 값을 주로 이용합니다. |

#### 메시지 발송 시 특정 단위로 통계를 추출하고 싶은 경우
1. **통계 이벤트 키 관리** 탭에서 통계 이벤트 키를 등록합니다. API를 사용하여 발송하는 경우 통계 아이디(statsId)를 이 화면에서 획득해야 합니다.
2. 콘솔에서 또는 API로 메시지를 전송할 때 통계 이벤트 키를 함께 보내야 합니다.
2-1. 콘솔에서 발송하는 경우 
    * **SMS 발송** 탭에서 문자 발송 시, 통계 이벤트 키를 선택합니다.
    * 메시지 정보를 모두 입력한 후 **보내기** 버튼을 클릭합니다.
    * **통계** 탭에서 일정 시간이 지난 후 통계 정보를 확인할 수 있습니다.
2-2. API로 발송하는 경우
    * **통계 이벤트 키 관리** 탭에서 얻은 statsId를 메시지 전송 파라미터에 같이 넣습니다.
    * **통계** 탭에서 일정 시간이 지난 후 통계 정보를 확인할 수 있습니다.

### 데이터 보관 기간
* 데이터 보관 정책에 따라, 최근 180일의 발송 이력을 보관합니다.
* 서비스에 이용된 첨부 파일은 7일간만 보관되며 그 이후에는 삭제되어 조회할 수 없습니다.
* 다만, 템플릿에 등록된 첨부 파일 및 증빙 서류(통신 서비스 이용 증명원)는 서비스를 이용하는 동안 보관됩니다.