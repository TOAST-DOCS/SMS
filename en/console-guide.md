## Notification > SMS > Console Guide

> To enable SMS Service, you may first register sender numbers on [Console > SMS > Sender Number Management] (Article 84 of Telecommunications Business Law).

## Sending SMS  

### Send General SMS

Following is the guide to send Short Message Service (SMS). 

![sms_01_201909](https://static.toastoven.net/prod_sms/sms_01_201909.png)

1. Select SMS for **Text Type**. 
2. Select **Sender Number** (numbers can be added on **Sender Number Management**.)
3. To choose time of delivery, specify it **Scheduled Delivery**.
4. Fill in the **Body** of message.
5. **Recipient Information** provides all numbers including domestic numbers or country code.
	- e.g.) 01012345678, 821012345678
6. Click **Send**.

### Send General LMS

Following is the guide to send general Long Message Service (LMS).

![sms_02_201909](https://static.toastoven.net/prod_sms/sms_02_201909.png)

1. Select MMS for **Text Type**. 
2. Select **Sender Number** (numbers can be added on **Sender Number Management**.)
3. To choose time of delivery, specify it in **Scheduled Delivery**.
4. Fill in the **Body** of message.
5. **Recipient Information** provides all numbers including domestic numbers or country code. 
	- e.g.) 01012345678, 821012345678
6. Click **Send**.

### Send MMS (Send Attachment)

Multimedia Messaging Service (MMS) can be sent as below:

![sms_03_201909](https://static.toastoven.net/prod_sms/sms_03_201909.png)

1. Select MMS for **Text Type**.
2. Select **Sender Number** (numbers can be added on **Sender Number Management**.)
3. To choose time of delivery, specify it in **Scheduled Delivery**.
4. Click **Upload Attachment** and upload attached files. 
5. Fill in the **Body** of message.
6. **Recipient Information** provides all numbers including domestic numbers or country code.
	- e.g.) 01012345678, 821012345678
7. Click **Send**.

### Send Templates 

You can send messages on user-created templates.

![sms_04_201909](https://static.toastoven.net/prod_sms/sms_04_201909.png)

1. Enable **Use Templates** and select a template from **Select Templates**.
2. Select **Sender Numbers** (numbers can be added from **Sender Number Management**.)
3. To choose time of delivery, specify in **Scheduled Delivery**.
4. Fill in the **Body** of message. 
5. **Recipient Information** provides all numbers including domestic numbers or country code. 
  - e.g.) 01012345678, 821012345678
6. Click **Send**.

### Send Mass Messages

You can send SMS/MMS to many numbers via template files in Excel/CSV.  

#### Template Files 

To download template files, select **Mass Delivery** and click **Download Templates**. 

![sms_05_201909](https://static.toastoven.net/prod_sms/sms_05_201909.png)

Template files are available either in CSV or Excel (xlx, xlsx). 
Rows are automatically created in files according to the replacement key in message. 

Fill out recipient numbers and replacement data in the downloaded template. 

![sms_06_201812](https://static.toastoven.net/prod_sms/sms_06_201812.png)

> [Caution] To enable template replacement, bind the replacement key with ##, like '##key##'.  

Recipient numbers can include '+', '-', or space characters.  

#### Validity Check for Template Files 

When a template file includes data error while uploaded, such errors, including the total number and no more than 10 errors, are displayed.  

Type of Errors 

- When the recipient\_no row does not exist: The recipient\_no row, which is for recipient numbers, is a required row. 
- When there is no data input in file 
- When recipient numbers are entered in a wrong format  
- When recipient numbers or replacement data are missing 

#### Select Scheduled Delivery (Deliver after Check/Immediate Delivery)

To send after delivery information and mass delivery files are uploaded, click **Schedule Delivery**. To schedule delivery, you may select either **Deliver after Check** or **Scheduled Delivery**. 

- Scheduled Delivery after Check: Check recipient numbers and message body to send, from the **Query Mass SMS Delivery** tab and then send. Otherwise, you cannot send messages. 
- Scheduled Delivery: Send immediately, without checking recipient numbers and text messages. Find delivery results on the **Query Mass SMS Delivery** tab.   

### Split Send

Split send allows you to split messages before sending by setting **Number of Splits** and **Send Interval**.

### Send Tags

Send with UID according to tag conditions. 

![sms_07_1_201909](https://static.toastoven.net/prod_sms/sms_07_1_201909.png)

Tags can be registered on **Tag Management**, while UID and phone numbers can be saved on the **UID Management** tab.  

## Query of SMS 

### Query by SMS Request 

Each item can be queried by conditions. 
(request id or date and time of delivery are required). 

![sms_08_201812](https://static.toastoven.net/prod_sms/sms_08_201812.png)

Click **Request ID** or **Recipient Numbers** to show pop-up window for details. 

### Query Scheduled SMS Delivery 

You can query the list of scheduled delivery. 

![sms_09_201812](https://static.toastoven.net/prod_sms/sms_09_201812.png)

Waiting for Schedule/Delivery Completed/Delivery Failed can be queried, and completed delivery can be found on **Query per SMS Request**. 

### Query Mass SMS Delivery 

- Query: You can query mass SMS delivery schedule in the above format. Select a column on the list of query, and check recipient numbers and delivery information (e.g. body and result of delivery). 
- Send/Cancel: To schedule delivery of mass upload, check recipients and select scheduled delivery; then, select a schedule which is 'ready for delivery', and click **Send/Cancel** to deliver or cancel delivery.  For scheduled delivery, messages are automatically sent as of the current time. 
- Check Failed Delivery: If delivery request fails while the progress status is 'Delivery Completed', the number of failure can be found. Click **Failure Cases** to check failed recipient numbers and messages. 

![sms_10_201812](https://static.toastoven.net/prod_sms/sms_10_201812.png)

#### Delivery Status of Mass SMS  

- Waiting: Template file data are yet to be read.
- Preparing: Loading template file data.
- Ready: All template file data are loaded and SMS delivery is ready. Select a schedule (column on the list) and you can find recipient numbers and delivery information. 
- Waiting for Delivery: SMS delivery is yet to be processed. 
- Delivering: SMS delivery is currently underway. Select a schedule (column on the list) to check the delivery progress rate. 
- Delivery Completed: Request for SMS delivery has been properly completed.
- Delivery Failed: Error occurred during delivery.
- Delivery Canceled: User has canceled delivery. 

#### Query SMS Delivery per Recipient 

Select a schedule of mass SMS delivery (column on the list) to check delivery information of each recipient number and the result. 

![sms_11_201812](https://static.toastoven.net/prod_sms/sms_11_201812.png)

To find more details of delivery, click **View Details**. 

![sms_12_201812](https://static.toastoven.net/prod_sms/sms_12_201812.png)

You can find successfully replaced data. 

### Query Tagged SMS Delivery 

#### Query by Delivery Request 

You can query requests for tag delivery. Click to query each recipient as below.  

![sms_13_201812](https://static.toastoven.net/prod_sms/sms_13_201812.png)

#### Query by Recipient 

You can query the list of recipients sent from one request. 

![sms_14_201812](https://static.toastoven.net/prod_sms/sms_14_201812.png)

Click **View Details** to see delivery details. 

![sms_15_201812](https://static.toastoven.net/prod_sms/sms_15_201812.png)

## Template Management 

### Add Categories 

Click **Add Categories** to add categories. 

![sms_16_201812](https://static.toastoven.net/prod_sms/sms_16_201812.png)

![sms_17_201812](https://static.toastoven.net/prod_sms/sms_17_201812.png)

Make sure to click **Add Categories** while a category is selected. 

### Modify Categories 

Click **Modify Categories** to modify categories.  

![sms_18_201812](https://static.toastoven.net/prod_sms/sms_18_201812.png)

Make sure to click **Modify Categories** while a category is selected. 

### Add Templates

Click **Add Templates** to add templates. 

![sms_19_201909](https://static.toastoven.net/prod_sms/sms_19_201909.png)

Make sure to click **Add Templates** while a category is selected. 

### Modify Templates 

Select a template to modify. 

![sms_20_201909](https://static.toastoven.net/prod_sms/sms_20_201909.png)

## Tag Management

Tags can be added or deleted for a registered UID. 

![sms_21_201812](https://static.toastoven.net/prod_sms/sms_21_201812.png)

## UID Management 

UIDs and mobile phone numbers can be registered or deleted. 

![sms_22_201812](https://static.toastoven.net/prod_sms/sms_22_201812.png)

Click **Register UIDs**. 

![sms_23_201812](https://static.toastoven.net/prod_sms/sms_23_201812.png)

Mass UIDs can be added in the CSV template. 

Find the number counts while uploading a template which is created. 

![sms_24_201812](https://static.toastoven.net/prod_sms/sms_24_201812.png)

## Identity Verification
* In order to comply with the amendment of Article 84(2) of the Telecommunications Business Act, an enhanced sender number pre-registration system has been applied to the SMS service.
    * Only for members who joined after March 2, 2022
* To use the SMS service, you must go through identity verification, which basically requires mobile phone identity verification and additional documentation depending on your membership type.
    * If you don't verify your identity, all features other than the **Sender Number Pre-Registration** tab will be disabled.
* The name and phone number you enter when you sign up must match the information you enter when you verify your identity to be approved.

### Required Documentation by Member Type

| Member Type | Verification Method | Required Documents |
| --- | --- | --- |
| Representative | Mobile verification | Business registration certificate, proof of employment |
| Employee | Mobile verification | Business registration certificate, proof of employment |
| IAM member | Mobile verification | Business registration certificate, proof of employment |
| Personal | Mobile verification | - |


### Identity Verification Process
1. Select the **Sender Number Pre-registration**tab.
2.  Click **Verify phone identity and attach required documents** to start the process.
3. Confirm and agree to the Consent to collection and usage of personal information.
4. Proceed to verify your identity with mobile verification or a quick identity verification.
5. Attach and register any required documents if necessary.
6. Wait for the operator review and approval process. 
7. Once the identity verification process is complete, the approval result will be sent to the email registered to your account.

## Pre-register Sender Numbers

#### Enforce pre-registration of sender numbers
* In accordance with Article 84 of the Telecommunications Business Act, the registration of a sender number requires the verification of the owner of the sender number.
* The owner verification method and required documents are determined according to the member and sender number types.

### Owner verification method by calling number

* Representative (CEO, employee, IAM)

| Sender Number Type | Verification Method | Required Documents |
| --- | --- | --- |
| Representative number, company number | Document verification | Communication service use certificate | 
| Employee number | Document verification | Communication service use certificate, proof of employment |
| Third-party number | Document verification | Telecommunication service usage certificate, usage agreement, (third-party) business license, documents confirming the relationship between the business and the third party (contract, etc.) |
| Other’s number | Document verification | Telecommunication service usage certificate, usage agreement |


* Personal

| Sender Number Type | Verification Method | Required Documents |
| --- | --- | --- |
| Personal number (mobile) | Mobile verification or document verification | Communication service use certificate | 
| Personal number (general) | Document verification | Communication service use certificate |
| Company/Third-party number | Cannot register | - |

#### ※ For reference

* Usage agreement form can be downloaded from the console.
* Documents confirming the relationship between the business and the third party can be consignment agreements, proof of headquarters and branch offices, etc.
* Personal member cannot register company and third-party numbers.
* After verifying your identity, you can register your sender number.

### Registration Format for Sender Numbers

```
* Fixed phone number: 02-YYY-YYYY (including area code)
* Mobile phone number: 010-ABYY-YYYY
* Official phone number: 15YY, 16YY, 18YY (area code prohibited ahead of the number)
* Common service identification number: 0N0 (area code prohibited ahead of the number)
* Sender numbers are available between 8 and 11 characters 
* Unavailable to send messaged from invalid number bandwidth (e.g.:070-0YYY, 070-1YYY, 010-0YYY, 010-1YYY)
```

#### ※ For reference

- Numbers that are subscribed to 'Blocking Sender Number Abuses' as part of additional telecom services, do not receive messages (web/system text delivery).  


### Register Sender Numbers
1. If you did not verify your identity before registering your sender number, proceed with identity verification.
    * Even for accounts registered before March 2, 2022, you must go through identity verification.
2. If you can register with identity authentication, click **Register Sender Number and Verify Mobile Phone**, otherwise select **Register Sender Number and Verify Documents** to start the registration process.
3. Confirm and agree to the Consent to collection and usage of personal information.
4. Select the type of sender number required for registration (personal number, representative number, employee number, etc.).
5. Enter the sender number to register.
6. Attach and register documents suitable for the number to be registered.
7. Wait for the operator review and approval process. 
8. Once the identity verification process is complete, the approval result will be sent to the email registered to your account.

### Select Authentication Type

1. Authentication on Mobile Phone 
  ![sms_26_201812](https://static.toastoven.net/prod_sms/sms_26_201812.png)
  ![sms_27_201812](https://static.toastoven.net/prod_sms/sms_27_201812.png)
  -  After authentication is completed, check sender number registration from **Status of Sender Number Screening**.   
  - Enter personal information and execute authentication. 
  - To send messages on your own mobile phone, **Authenticate on Mobile Phone** to immediately register sender numbers.  

2. Authenticate by Documents
  ![sms_28_201812](https://static.toastoven.net/prod_sms/sms_28_201812.png)
  ![sms_29_201812](https://static.toastoven.net/prod_sms/sms_29_201812.png)
  - After authentication is completed, check sender number registration in **Sender Number Evaluation Status**. 
  - Upload required documents for authentication. 
  - To make calls to phone numbers authenticated on documents by telecom service providers, register numbers by **Document Authentication**. 
  - Enter sender number and press Enter to register more than one number. 


Sender numbers that are properly registered can be found on the **Query Sender Numbers** page. 

![sms_31_201812](https://static.toastoven.net/prod_sms/sms_31_201812.png)

## Setting for Rejection of Receiving 080 Numbers 

The rejection of receiving 080 numbers service allows recipients to reject receiving of ad messages. 
Advertisement messages<span style="color:red"> must include how to reject receiving charge-free </span> for recipients to reject or withdraw consent of receiving.   

### Subscription

Go to **Setting for rejection of receiving 080 numbers** to find the subscription page. 
Click **Add 080 Numbers to Reject Receiving** to enter business names. 

* Business names refer to such businesses which are guided when you make calls via 080 numbers rejected of receiving.   

![sms_33_201812](https://static.toastoven.net/prod_sms/sms_33_201812.png)

### Registration Scheduled

When subscription is fully applied, the status is changed to Registration Scheduled.  It takes 3 to 4 business days to open the rejection of receiving 080-number service, and the service is enabled after opening. 

### Registration Completed

When the service is completely open, you can find the start date and status of service.  
**While the rejection of 080-number is scheduled for registration or in service, SMS Service cannot be closed.** Service can be closed only after it is canceled.  
To cancel the service, press **Cancel Service**. 

### Send Ad Messages 

1. Ad messages can be sent only when the rejection 080-number service is enable. 
2. When the delivery type is changed into **For Advertisement**, you can find an option to select numbers to reject receiving. 
3. Click **Apply Option**, and the message body is changed to the phrase as below. 
4. To send ad messages, following phrase must be included; otherwise, sending fails. 

```
(Ad)

[Reject receiving charge-free]080XXXXXXX
```

![sms_34_1_201909](https://static.toastoven.net/prod_sms/sms_34_1_201909.png)

### Query Target of Rejection 

Rejection targets, requested with request date and time as optional, can be queried from the panel at the bottom.    

## Setting for Duplicates 

### Set Duplicate Delivery 

* By setting, duplicate messages may not be sent. 
* When the duplicate delivery setting is blocked, delivery is processed as failure for same requests during specified period (unit:minute). 
* The maximum available block time is 1 hour. 
* A message is determined as duplicate by the following criteria: 
    * Message type (SMS/LMS/MMS/AUTH), delivery type (general/mass/tag), sender number, recipient number, title, body, and attached file

![sms_35_201812](https://static.toastoven.net/prod_sms/sms_35_201812.png)

## Statistics 

### Query Statistics 

![sms_36_201812](https://static.toastoven.net/prod_sms/sms_36_201812.png)

* Statistics are available by delivery request period, template, delivery type, or content of delivery. 
* Request of delivery, success, or failure can be found on graph or table. 

## Guide for Notice of Personal Information Assignor 

When the Customer uses TOAST > SMS Service, assignment of personal information between the Customer and the Company arises, and the assignee, the Customer, is obliged to disclose the status (assignor and content of business) of his assignment of personal information to the Company, through the personal information handling policy, in accordance with the Act on Promotion of Information and Communications Network Utilization and Information Protection, and the Personal Information Protection Act. 

Accordingly, the Company may provide guidelines as below for the Customer, to abide by relevant regulations in the use of TOAST SMS Service and not to be adversely affected for not disclosing his assignment status:

(Example)

[Notice of Personal Information Assignor] 
To use TOAST SMS Service, make sure the following is displayed for 'Personal Information Handling Policy' > Assignment Status of the Customer.

Assignor: NHN 
Content of Business: Send SMS in lieu of customers