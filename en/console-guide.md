<style>
    .custom-table thead {
        background-color: #FAFAFA;
    }
    
    .custom-table tbody tr {
        background-color: white;
    }
    
    .custom-table td {
        vertical-align: middle;
    }
</style>

## Notification > SMS > Console Guide

> To enable SMS Service, you may first register sender numbers on [Console > SMS > Sender Number Management] (Telecommunications Business Law).

## Identity Verification
* In order to comply with the amendment of the Telecommunications Business Act, an enhanced sender number pre-registration system has been applied to the SMS service.
    * Only for members who joined after March 2, 2022
* To use the SMS service, you must go through identity verification, which basically requires mobile phone identity verification and additional documentation depending on your membership type.
    * If you don't verify your identity, all features other than the **Sender Number Pre-Registration** tab will be disabled.
* The name and phone number you enter when you sign up must match the information you enter when you verify your identity to be approved.
* **For individual members, only the owner of the organization can authenticate his or her identity.** NHN Cloud members invited to an organization/project created by an individual member or IAM members invited to an organization created by an individual member cannot authenticate their identity.
* Proof of employment can only be <span style="color:red;font-weight:bold">documents with the date of issuance and a stamp.<span style="color:red;font-weight:bold"><br/>
Make sure you <span style="color:red;font-weight:bold">mask (hide) the last 6 digits of your resident registration number<span style="color:red;font-weight:bold"> in your employment certificate. Example) 000000-0\*\*\*\*\** 

### Required Documentation by Member Type
<table class="custom-table" style="text-align: center">
  <thead>
      <tr>
          <th>Member Type</th>
          <th>Category</th>
          <th>Content</th>
          <th>Verification Method</th>
          <th>Required Document</th>
      </tr>
  </thead>
  <tbody>
      <tr>
          <td rowspan=3>Business</td>
          <td rowspan=2>NHN Cloud member</td>
          <td>When by a business representative</td>
          <td>Business representative’s own mobile phone identity verification</td>
          <td>Business registration certificate, proof of employment</td>
      </tr>
      <tr>
          <td>When verified by an executive or employee (employee in charge of business)</td>
          <td>Mobile verification by employee</td>
          <td>Business registration certificate, proof of employment</td>
      </tr>
      <tr>
          <td>IAM member</td>
          <td>When authenticated by an executive or employee (employee in charge) invited to the business organization</td>
          <td>Personal verification by employee’s own mobile</td>
          <td>Business registration certificate, proof of employment</td>
      </tr>
      <tr>
          <td>Personal</td>
          <td>NHN Cloud member</td>
          <td>When individual members authenticates themselves</td>
          <td>Mobile verification</td>
          <td>-</td>
      </tr>
  </tbody>
</table>

### Identity Verification Process
![sms_01_20230818](https://static.toastoven.net/prod_sms/eng/SMS_01_20230818.png)
1. Select the **Sender Number Pre-registration**tab.
2. Click **Verify phone identity and attach required documents** to start the process.
3. Confirm and agree to the Consent to collection and usage of personal information.
4. Proceed to verify your identity with mobile verification or a quick identity verification.
5. Attach and register any required documents if necessary.
6. Wait for the operator review and approval process. 
7. Once the identity verification process is complete, the approval result will be sent to the email registered to your account.

### Description of identity verification status
+ Reviewing: The administrator is reviewing the authentication documents for registered identity verification.
+ Rejected: A state in which identity verification has been rejected and documents must be re-registered.
+ Approved: Identity verification approval completed


## Pre-register Sender Numbers

### Enforce pre-registration of sender numbers
* In accordance with the Telecommunications Business Act, the registration of a sender number requires the verification of the owner of the sender number.
* The owner verification method and required documents are determined according to the member and sender number types.

### Owner verification method by calling number
<table class="custom-table" style="text-align: center">
  <thead>
      <tr>
          <th>Member Type</th>
          <th>Sender Number Type</th>
          <th>Verification Method</th>
          <th>Required Document</th>
      </tr>
  </thead>
  <tbody>
      <tr>
          <td rowspan=4>Business</td>
          <td>Representative number, company number</td>
          <td>Document verification</td>
          <td>Communication service use certificate</td>
      </tr>
      <tr>
          <td>Employee number</td>
          <td>Document verification</td>
          <td>Communication service use certificate, proof of employment</td>
      </tr>
      <tr>
          <td>Third-party number</td>
          <td>Document verification</td>
          <td>Telecommunication service usage certificate, usage agreement, (third-party) business license, documents confirming the relationship between the business and the third party (contract, etc.)</td>
      </tr>
      <tr>
          <td>Other’s number</td>
          <td>Document verification</td>
          <td>Telecommunication service usage certificate, usage agreement</td>
      </tr>
      <tr>
          <td rowspan=3>Personal</td>
          <td>Personal number (mobile)</td>
          <td>Document verification</td>
          <td>Communication service use certificate</td>
      </tr>
      <tr>
          <td>Personal number (general)</td>
          <td>Document verification</td>
          <td>Communication service use certificate</td>
      </tr>
      <tr>
          <td>Company/Third-party number</td>
          <td>Cannot register</td>
          <td>-</td>
      </tr>
  </tbody>
</table>

#### ※ For reference
* Usage agreement form can be downloaded from the console.
* Documents confirming the relationship between the business and the third party can be consignment agreements, proof of headquarters and branch offices, etc.
* Personal member cannot register company and third-party numbers.
* After verifying your identity, you can register your sender number.
* Numbers for which mobile phone identity verification is not possible are verified through the communication service use certificate.
* There are no masked (hidden) parts of the communication service use certificate, and only documents issued within the last 3 months are accepted.
* Proof of employment can only be <span style="color:red;font-weight:bold">documents with the date of issuance and a stamp.<span style="color:red;font-weight:bold"><br/>
Make sure you <span style="color:red;font-weight:bold">mask (hide) the last 6 digits of your resident registration number<span style="color:red;font-weight:bold"> in your employment certificate. Example) 000000-0\*\*\*\*\**

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
![sms_02_20230818](https://static.toastoven.net/prod_sms/eng/SMS_02_20230818.png)
1. If you did not verify your identity before registering your sender number, proceed with identity verification.
    * If you are a subscriber before March 2, 2022, you can use the console without identity verification.
2. If you can register with identity authentication, click **Register Sender Number and Verify Mobile Phone**, otherwise select **Register Sender Number and Verify Documents** to start the registration process.
3. Confirm and agree to the Consent to collection and usage of personal information.
4. Select the type of sender number required for registration (personal number, representative number, employee number, etc.).
5. Enter the sender number to register.
6. Attach and register documents suitable for the number to be registered.
7. Wait for the operator review and approval process. 
8. Once the sender number verification process is complete, the approval result will be sent to the email registered to your account.

### Description for Sender Number Registration Status
+ Reviewing: The administrator is reviewing the authentication documents for the registered sender number.
+ Rejected: Document authentication is rejected and document re-registration is required.
+ Approved: Sender number is available

Sender numbers that are properly registered can be found on the **Retrieve Outgoing Numbers** page.

![sms_03_20230818](https://static.toastoven.net/prod_sms/eng/SMS_03_20230818.png)

## Sending SMS
* The maximum character count is based on storage. To avoid character truncation, write to a standard size, not a maximum character count.
* Content standard: Domestic SMS 90 bytes (45 Korean characters, 90 English characters) / Domestic MMS 2,000 bytes (1,000 Korean characters, 2,000 English characters) / International SMS UCS-2 335 characters / International SMS GSM-7bit 765 characters   
* For international SMS, it is concatenated based on encoding and character count.
    * Concatenated message is a service that makes a message appear to be connected as one long text on the device, like an LMS in Korea, to overcome the limitation of the number of characters that can be sent in international SMS sending.
    * The Concatenated message feature is available depending on the number of characters in the body, encoding, and support from international mobile carriers. If concatenated message is not supported, the device may receive multiple short messages.
    * When a concatenated message is created, certain headers take up a number of bytes (around 6) to concatenate the message, slightly reducing the number of characters you can send. Billing is based on the number of concatenated messages.

* If you fail to send a text due to blocking the sender number, please check the 'Stolen Number Text Blocking Service'. [Go to the user guide](./sending-policy/#fraud-number)
* If the sending is successful but you do not receive the text, please check ‘Mobile Carrier Spam Blocking Service’. [Shortcut to guide](./sending-policy/#spam-number)


### Send General SMS

Following is the guide to send Short Message Service (SMS).

![sms_04_20230818](https://static.toastoven.net/prod_sms/eng/SMS_04_20230818.png)

1. Select SMS for **Text Type**.
2. Select **Sender Number** (numbers can be added on **Pre-register Sender Numbers** > **Register Sender Number and Verify Owner**.)
3. To choose time of delivery, specify it **Scheduled Delivery**.
4. Fill in the **Body** of message.
5. **Recipient Information** provides all numbers including domestic numbers or country code.
	- e.g.) 01012345678, 821012345678
6. Click the **Send** button.

### Send General LMS

Following is the guide to send general Long Message Service (LMS).

![sms_05_20230818](https://static.toastoven.net/prod_sms/eng/SMS_05_20230818.png)

1. Select MMS for **Text Type**.
2. Select **Sender Number** (numbers can be added on **Sender Number Management**.)
3. To choose time of delivery, specify it **Scheduled Delivery**.
4. Enter the title. (To prevent character truncation, write based on 40 bytes (20 Korean characters, 40 English characters).)
5. Fill in the **Body** of message.
6. **Recipient Information** provides all numbers including domestic numbers or country code.
	- e.g.) 01012345678, 821012345678
7. Click the **Send** button.

### Send MMS (Send Attachment)

Multimedia Messaging Service (MMS) can be sent as below:

![sms_06_20230818](https://static.toastoven.net/prod_sms/eng/SMS_06_20230818.png)

1. Select MMS for **Text Type**.
2. Select **Sender Number** (numbers can be added on **Pre-register Sender Numbers** > **Register Sender Number and Verify Owner**.)
3. To choose time of delivery, specify it **Scheduled Delivery**.
4. Click **Upload Attachment** and upload attached files.
5. Enter the title. (To prevent character truncation, write based on 40 bytes (20 Korean characters, 40 English characters).)
6. Fill in the **Body** of message.
7. **Recipient Information** provides all numbers including domestic numbers or country code.
	- e.g.) 01012345678, 821012345678
8. Click the **Send** button.

### Send Templates

You can send messages on user-created templates.

![sms_07_20230818](https://static.toastoven.net/prod_sms/eng/SMS_07_20230818.png)

1. Enable **Use Templates** and select a template from **Select Templates**. (Template can be registered in the Template Management tab.) 
2. Select **Sender Number** (numbers can be added on **Pre-register Sender Numbers** > **Register Sender Number and Verify Owner**.)
3. To choose time of delivery, specify it **Scheduled Delivery**.
4. Fill in the **Body** of message.
5. **Recipient Information** provides all numbers including domestic numbers or country code.
	- e.g.) 01012345678, 821012345678
6. Click the **Send** button.

### Mass Delivery

You can send SMS/MMS to many numbers via template files in Excel/CSV.

#### Template Files

To download template files, select **Mass Delivery** and click **Download Templates**.

![sms_08_20230818](https://static.toastoven.net/prod_sms/eng/SMS_08_20230818.png)
![sms_09_20230818](https://static.toastoven.net/prod_sms/eng/SMS_09_20230818.png)

Template files are available either in CSV or Excel (xlx, xlsx).
Rows are automatically created in files according to the replacement key in message.

Fill out **recipient numbers** and **replacement data** in the downloaded template.

![sms_10_20230818](https://static.toastoven.net/prod_sms/eng/SMS_10_20230818.png)

> [Caution] To enable template replacement, bind the replacement key with ##, like '##key##'.

Recipient numbers can include '+', '-', or space characters.

#### Validity Check for Template Files

When a template file includes data error while uploaded, such errors, including the total number and no more than 10 errors, are displayed.

Type of Errors

- When the recipient_no row does not exist: The recipient_no row, which is for recipient numbers, is a required row.
- When there is no data input in file
- When recipient numbers are entered in a wrong format
- When recipient numbers or replacement data are missing

#### Select Scheduled Delivery (Deliver after Check/Immediate Delivery)

To send after delivery information and mass delivery files are uploaded, click **Schedule Delivery**. To schedule delivery, you may select either **Deliver after Check** or **Scheduled Delivery**.

- Scheduled Delivery after Check: Check recipient numbers and message body to send, from the **Query Mass SMS Delivery** tab and then send. Otherwise, you cannot send messages.
- Scheduled Delivery: Send immediately, without checking recipient numbers and text messages. Find delivery results on the **Query Mass SMS Delivery** tab.

#### Split Send

Split send allows you to split messages before sending by setting **Number of Splits** and **Send Interval**.

<span id='tag-send'></span>
### Send Tags

Send with UID according to tag conditions.

![sms_11_20230818](https://static.toastoven.net/prod_sms/eng/SMS_11_20230818.png)
![sms_12_20230818](https://static.toastoven.net/prod_sms/eng/SMS_12_20230818.png)
![sms_13_20230818](https://static.toastoven.net/prod_sms/eng/SMS_13_20230818.png)

Tags can be registered on **Tag Management**, while UID and phone numbers can be saved on the **UID Management** tab.

## Setting for Rejection of Receiving 080 Numbers

The rejection of receiving 080 numbers service allows recipients to reject receiving of ad messages.
Advertisement messages<span style="color:red"> must include how to reject receiving charge-free </span> for recipients to reject or withdraw consent of receiving.

### Subscription

Go to **Setting for rejection of receiving 080 numbers** to find the subscription page.
Click **Add 080 Numbers to Reject Receiving** to enter business names.

* Business names refer to such businesses which are guided when you make calls via 080 numbers rejected of receiving.

![sms_14_20230818](https://static.toastoven.net/prod_sms/eng/SMS_14_20230818.png)

### Registration Scheduled

When subscription is fully applied, the status is changed to Registration Scheduled. It takes 3 to 4 business days to open the rejection of receiving 080-number service, and the service is enabled after opening.

### Registration Completed

When the service is completely open, you can find the start date and status of service.**While the rejection of 080-number is scheduled for registration or in service, SMS Service cannot be closed.** Service can be closed only after it is canceled.To cancel the service, press **Cancel Service**.

### Send Ad Messages

1. Ad messages can be sent only when the rejection 080-number service is enable.
2. When the delivery type is changed into **For Advertisement**, you can find an option to select numbers to reject receiving.
3. Ad messages caClick **Apply Option**, and the message body is changed to the phrase as below.n be sent only when the rejection 080-number service is enable.
4. To send ad messages, following phrase must be included; otherwise, sending fails.

```
(Ad)

[Reject receiving charge-free]080XXXXXXX
```

![sms_15_20230818](https://static.toastoven.net/prod_sms/eng/SMS_15_20230818.png)
![sms_16_20230818](https://static.toastoven.net/prod_sms/eng/SMS_16_20230818.png)

### Query Target of Rejection

Rejection targets, requested with request date and time as optional, can be queried from the panel at the bottom.

## Query of SMS

### Query by SMS Request

Each item can be queried by conditions.
(request id or date and time of delivery are required).

![sms_17_20230818](https://static.toastoven.net/prod_sms/eng/SMS_17_20230818.png)

* Click **Request ID** or **Recipient Numbers** to show pop-up window for details.
* Registration/sending/receiving date search is possible within a maximum of one month.
* You can download the displayed data in full screen as an Excel file.
* You can check the status of your send request through the request status.
* You can check the success/failure of sending processing through the sending results.

### Query Scheduled SMS Delivery

You can query the list of scheduled delivery.

![sms_18_20230818](https://static.toastoven.net/prod_sms/eng/SMS_18_20230818.png)

* Click **Request ID** or **Recipient Numbers** to show pop-up window for details.
* Registration/sending/receiving date search is possible within a maximum of one month.
* You can check the status of your send request through the request status.
* If your scheduled sending is waiting, you can cancel it by selecting it from the list.

### Query Mass SMS Delivery

You can search for mass delivery by sending type.

![sms_19_20230818](https://static.toastoven.net/prod_sms/eng/SMS_19_20230818.png)

* Query: You can search for bulk SMS sending reservations in the inquiry form at the top. When you select a row in the inquiry list, you can check the receiving number and sending information (sending details, sending results) in the inquiry form at the bottom.
* Send/Cancel: When making a reservation for bulk upload sending, if you select the scheduled send after confirming the recipient, select the reservation with the status of ‘Ready to Send’ and cancel.
* You can send or cancel by clicking the **Send/Cancel** button. In case of scheduled sending is automatically processed at the current time.
* Check Failed Delivery: If delivery request fails while the progress status is 'Delivery Completed', the number of failure can be found. Click **Failure Cases** to check failed recipient numbers and messages.

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

Select mass delivery (column on the list) to check delivery information of each recipient number and the result.

![sms_20_20230818](https://static.toastoven.net/prod_sms/eng/SMS_20_20230818.png)

To find more details of delivery, click **View Details**.

![sms_21_20230818](https://static.toastoven.net/prod_sms/eng/SMS_21_20230818.png)

You can find successfully replaced data.

### Query Tagged SMS Delivery

#### Query by Delivery Request

You can query requests for tag delivery. Click to query each recipient as below.

![sms_22_20230818](https://static.toastoven.net/prod_sms/eng/SMS_22_20230818.png)

#### Query Sending by Recipient

You can query the list of recipients sent from one request.

![sms_23_20230818](https://static.toastoven.net/prod_sms/eng/SMS_23_20230818.png)

To find more details of delivery, click **View Details**.

![sms_24_20230818](https://static.toastoven.net/prod_sms/eng/SMS_24_20230818.png)

## Template Management

### Add Categories

Click **Add Categories** to add categories.

![sms_25,26_20230818](https://static.toastoven.net/prod_sms/eng/SMS_25,26_20230818.png)

Make sure to click **Add Categories** while a category is selected.

### Modify Categories

Click **Modify Categories** to modify categories.

![sms_27_1_20230818](https://static.toastoven.net/prod_sms/eng/SMS_27_1_20230818.png)

Make sure to click **Modify Categories** while a category is selected.

### Add Templates

Click **Add Templates** to add templates.

![sms_27_20230818](https://static.toastoven.net/prod_sms/eng/SMS_27_20230818.png)

1. Click the **Add Template** button.
2. Enter a **sending type and template information**.
3. To replace authentication numbers, order numbers, coupon codes, points, etc., **enter replacement keys enclosed in ##, such as '##key##', in the title or content**.
4. After entering all the information, make sure to select a category, click **Add Template**.

### Modify Templates

Select a template to modify.

![sms_28_20230818](https://static.toastoven.net/prod_sms/eng/SMS_28_20230818.png)

1. Select the template that needs to be modified.
2. Modify the sending type, template information, and content.
3. Make sure to click **Modify Templates** while a category is selected after modification is completed.

<span id='uid-manage'></span>
## UID Management

You can register and delete UID and mobile phone number. Please refer to [the reference](./console-guide/#tag-uid)

![sms_29,30_20230818](https://static.toastoven.net/prod_sms/eng/SMS_29,30_20230818.png)

Click **Register UID**.
Mass UIDs can be added in the CSV template.

![sms_31_20230818](https://static.toastoven.net/prod_sms/eng/SMS_31_20230818.png)

Enter in uid,phoneNumber format.<br/>
ex) sms_uuid1,01012345678

![sms_32_20230818](https://static.toastoven.net/prod_sms/eng/SMS_32_20230818.png)
Find the number counts while uploading a template which is created.

<span id='tag-manage'></span>
## Tag Management

This is a page where you can tag or delete registered UIDs. Please refer to the reference for the meaning of tags and UID terms.

![sms_32_20230818](https://static.toastoven.net/prod_sms/eng/SMS_33_20230818.png)

Click **Register Tag** to register the tag.

![sms_34_20230818](https://static.toastoven.net/prod_sms/eng/SMS_34_20230818.png)
Register UID in tag. (Register the UID registered in the UID tab.) 

## Webhook Management
You can receive a webhook event by specifying a URL when a specified event occurs.

![sms_35_20230818](https://static.toastoven.net/prod_sms/eng/SMS_35_20230818.png)

1. Select the event type to register.
2. Enter the URL address where data to be sent via webhook can be received.
3. Enter the webhook signature to register (not required).
4. After verification, click **Add** to register the webhook.

Registered webhooks can be checked in the **webhook registration list**.

## Sending Settings
### International SMS Sending Settings
* Before using the international SMS sending feature, see [International SMS Sending Policy](./international-sending-policy).
* If you do not want to use the international SMS sending feature, you can prevent accidents due to abusing by setting it to unused.
* Only the specified major countries are enabled for sending during the initial setup. You can manage whether to ship to each country through the [Select countries to allow] button.
* The sending limit is 1,000 per month by default, and can be adjusted up to 10,000. If you need to adjust the limit over 10,000, please contact us via the [Request to exceed 10,000] button.
* The 'International Auto Blocking Monthly Limit' is an subsidiary feature and the time of blocking may not be accurate. NHN Cloud is not responsible for any errors in the subsidiary features.

> [Caution]
Cases of international SMS abuse are increasing globally.
It is recommended to set the monthly limit and the country of origin only as much as necessary.
NHN Cloud is not responsible for any international SMS sent due to abuse.

### Alternative Characters Settings
* If the body/subject of the delivery request contains unsendable text, you can set it to be converted to sendable text.
* When the alternative characters setting is enabled, unsendable characters are converted to '?' and displayed.

### Set Duplicate Delivery
* By setting, duplicate messages may not be sent.
* When the duplicate delivery setting is blocked, delivery is processed as failure for same requests during specified period (unit:minute).
* The maximum available block time is 1 hour.
* A message is determined as duplicate by the following criteria:
    * Message type (SMS/LMS/MMS/AUTH), delivery type (general/mass/tag), sender number, recipient number, title, body, and attached file

### Limit Advertising Messages
* You can limit the sending time of advertising messages.
* Advertising messages will not be sent during the set time.
  * Ad limit start time can be set: 18:00~21:00
  * Ad limit end time can be set: 08:00~12:00
* Failure/re-delivery is possible depending on how the undelivered message is set up.

### Backup Settings
* Depending on the message retention period policy, you can back up sending history data that is older than 180 days. 
* If you enter information about whether to back up messages, the file extension, and the storage to upload the file to, a file containing the backup date will be created in that storage.


## Statistical Event Key Settings
When registering an event key and sending with that key, you can collect statistical data by statistical event key./
Please refer to the [reference](./console-guide/#tag-uid) for the meaning of statistical event key terms.

![sms_36_20230818](https://static.toastoven.net/prod_sms/eng/SMS_36_20230818.png)

1. Click **Register Event Key** to set the data collection period.
2. Enter a name and detailed description for the statistical event key.
3. Click **Save** and the event key will be registered.

When the data collection period ends, it becomes inactive and no longer collects data.<br/>
**The end point of the data collection period can be modified if activated.**

## Statistics

### Query Statistics

![sms_37_20230818](https://static.toastoven.net/prod_sms/eng/SMS_37_20230818.png)

* You can view statistics by delivery request duration, template, delivery type, and delivery content.
* You can view delivery requests, successes, and failures in graphs and tables.


## [Note]

<span id='tag-uid'></span>
### Tags and UID

#### Glossary
| Term           | Description                                       |
| ------------ | ---------------------------------------- |
| Tag      | A system for classifying UIDs. <br>By attaching multiple tags to UID, users can easily search and use UID information. |
| UID          | An ID (identifier) that identifies the user. <br>Multiple contacts can be registered in one UID and used for sending. |
| Contact | A place designated for contact. <br>In Notification, you can register contact information from three services: Push, Email, and SMS. <br>Push refers to a token, Email refers to an email address, and SMS refers to a phone number. |

#### Send using tags
This feature allows you to send a text message by selecting a tag instead of the phone number that is the recipient's information.

1. Register UID.
    - Register UID and one or multiple phone numbers in the **UID management** tab.
    - For more information, please refer to [UID Management](./console-guide/#uid-manage).
2. Register a tag.
    - Register tags in the **Manage Tags** tab.
    - For more information, please refer to [Manage Tags](./console-guide/#tag-manage).
3. Register UID in tag.
    - Register the UID to the tag registered in the **Manage Tags** tab.
4. Select a tag and send a text message.
    - In the **General Send** tab, select **Send Tag** instead of email address to register the tag.
    - The email is sent to the phone number of the UID registered in the tag.
    - For more information, please refer to [Sending messages using tags](./console-guide/#tag-send).

#### Relationship with tag features in other services
* If you are using Push or SMS services in the same project, you can use the tag and UID information used in Email together without re-registering.
* You can add additional contact information to the same UID through each service's console.

### Statistics Event Keys and Statistics
#### Glossary
| Term           | Description                                       |
| ------------ | ---------------------------------------- |
| Statistical Event Key | This is an event key used when you want to view statistics grouped into specific units. |
| statsId | Unique ID of the statistical event key. This value is mainly used when calling the API. |

#### If you want to extract statistics in specific units when sending a message
1. Register statistical event keys in the **statistical event key management** tab. If sending using the API, you must obtain the statistics ID (statsId) from this screen.
2. When sending a message from the console or to the API, you must also send the statistics event key.

    2-1. When sending from console 
    * When sending a text message in the **Deliver SMS** tab, select the statistical event key.
    * After entering all message information, click the **Send** button.
    * You can check statistical information after a certain period of time in the **Statistics** tab.

    2-2. When sending via API
    * Enter the statsId obtained from the **statistics event key management** tab into the message transmission parameters.
    * You can check statistical information after a certain period of time in the **Statistics** tab.

### Data retention period
* Retains the sending history for the last 180 days in accordance with the data retention policy.
* The attachments used in services are retained for 7 days. After 7 days, they are deleted and cannot be retrieved.
* However, the attachments and evidential documents (communications service certificate) registered to a template are retained as long as the service is provided.
* Statistical data stores information from the last 90 days.
