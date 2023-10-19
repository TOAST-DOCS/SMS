## Notification > SMS > Release Notes

### 2023. 10. 31.
#### 기능 개선/변경
* [Console] 발신번호 등록 페이지 문구 추가
    * 발신번호 등록 시 필요한 서류인 재직증명서와 통신서비스 이용증명서에 대한 설명이 추가되었습니다.
* [Console] 국제 SMS 발송 설정 개선
    * 발송 설정 메뉴가 일반 SMS, 국제 SMS 탭으로 구분되었습니다.
    * "자동 차단 월 제한 건수"를 직접 설정할 수 있게 되었습니다.
    * 발송 허용 국가 관리 기능이 추가되었습니다.
* [Console] SMS 발송 내용 글자 수 제한 개선
    * 국내 SMS 발송 기준인 최대 255자 제한을 완화하여 국제 SMS 기준에도 부합하도록 변경되었습니다.
    * 인코딩에 따라 최대 입력 가능한 글자 수가 변경됩니다. (UCS-2: 335자, GSM-7bit: 765자)

### September 26, 2023
#### Feature Updates
* [Console] Improved international sending statistics
    * Improved to view request, sending, sending failed, receiving, and message count event in international sending statistics.
* [API] Improved international sending process
    * Improved to send international messages via a sender number without pre-registering the number.

### August 29, 2023
#### Feature Updates
* [API] Improved template deletion
    * Improved to allow reregistration with deleted template ID.
* [API] Improved the createUser, updateUser response fields
    * Improved to send user email addresses to the createUser and updateUser fields when querying.
* [Console/API] Improved the mass delivery cancellation feature
    * During mass delivery, cancellation request is viewed as "Canceling" until all recipients have been canceled, and "Canceled" after all recipients have been canceled.
    * For more information on status codes, see [[List Mass Delivery](./api-guide/#_30)].
* [API] Added the delivered message count field to the v3.0 list query and detailed query APIs
    * Added the delivered message count (messageCount) to the v3.0 list query and detailed query APIs.
    * If a long message is divided into several messages and sent using the international concat function, you can check the number of messages sent based on the number of characters.
    * For information on the character count, refer to [](./international-sending-policy/#_3)Pricing Policy[](./international-sending-policy/#_3).
* [Console] Added the delivered message count column to the retrieved list
    * You can check the number of messages sent in the delivery lists of Query by SMS Request, Query Mass SMS Delivery, Query Tagged SMS Delivery.

### August 1, 2023
#### Added Features
* [Console] Added split sending feature
    * Added a feature to send messages by time when requesting mass delivery.
* [Console] Added alternative character setting feature
    * Added a feature to convert non-sendable characters in the body/subject of a sending request to '?' so that it can be requested as a normal sending.
    * The setting is available on the **Delivery Setting** tab.
* [Console/API] Provide international sending concat
    * When sending international text messages, you can send up to 765 characters in GSM-7 and 335 characters in UCS-2, and send them as long messages through the concat function.
    * For more details, please refer to the [[Billing Policy]](./international-sending-policy/#_3) of the service policy.

#### Feature Updates
* [Console/API] Improved international text message sending encoding
    * When sending international text messages, they are sent in GSM-7 or UCS-2 encoding depending on the character set of the body text.
    * If the character body consists of GSM-7 basic and extended character sets, it is sent as GSM-7.
    * Encoding results can be checked in **Retrieve Details** under Retrieve Delivery.

### July 25, 2023
#### Feature Updates
* [Console] Improved the identity verification process
    * Improved so that, when identity verification is rejected or requested again, users can change business registration certificates registered in the organization.
    * Added the attachment field when verifying identities.
* [Console] Improved sender number registration process
    * Added the attachment field when registering sender numbers.

### July 11, 2023
#### Bug Fixes
* [API] Fixed a bug where message could be sent with a deleted template
    * Improved so that message could not be sent with a deleted template.

### May 30, 2023
#### Feature Updates
* [Console] Improved the identity verification process
    * Improved so that, when using the SMS console, you only need to authenticate once within the same organization.

### April 25, 2023
#### Added Features
* Added a feature to set the monthly delivery limit on international SMS
    * If you enable international SMS delivery, you can set the monthly limit.

#### Feature Updates
* [API] Added code update webhook field as a result of sending message
    * Added the recipientGroupingKey, senderGroupingKey fields to code update webhook as a result of sending message.
* [API] Added the identification code field to the v3.0 delivery API and detailed query API
    * Added the identification code field (originCode) to the v3.0 delivery API and detailed query API.
    * Add the identification code of the mobile carrier that first sent message to quickly identify illegal text message senders, such as misrepresenting the sender number, illegal spam, etc. for shutdown action.
        * For special value-added telecommunication business operators, must use the 9 digit registration number listed on certificates.
        * If null in the field, NHN Cloud’s identification code is added.
* [Console] Added a result code value to the detailed query delivery page
    * Added result code values to the detailed query delivery page.
* [Console] Improved attachment upload when sending MMS
    * Improved to upload up to three attachments when sending an MMS.

### March 28, 2023
#### Added Features
* [Console] Added a setting to use international SMS
    * Added a setting that allows you to set the international SMS delivery feature to 'Not Used' when not in use to prevent issues such as abusing.
* [Console] Aplied CloudTrail
    * Applied CloudTrail to allow you to check usage history.

#### Feature Updates
* [API] Faded out APIs related to sender numbers
    * Faded out some APIs related to sender numbers due to enhanced sender number pre-registration system.
    * Faded-out APIs
        * API to request sender number registration
        * API to upload sender number document
        * API to retrieve sender number authentication history
    * After the enhancement of the sender number pre-registration system, all requests for approval of sender numbers registered through APIs will be rejected.

### February 28, 2023
#### Added Features
* [Console/API] Added the validation logic for avaliable countries stated in the guide
    * Modified to fail for countries other than those specified in the guide.

### February 17, 2023
#### Added Features
* [Console] Enhanced pre-registration of calling numbers
    * Added verification processes when using the SMS console and registering calling numbers.
    * Modified the console so that members who signed up after March 2, 2022 must verify their identity before using the console.

### January 31, 2023
#### Feature Updates
* [Console] Increased the recipient file size limit to upload for mass delivery
    * Changed the limit from 10MB to 30MB.

### October 25, 2022
#### Bug Fixes
* [API] Fixed a bug that occurred to a single search of messages
    * Fixed the bug where search results are exposed without type classification in a single search of messages.

### August 23, 2022
#### Feature Updates
* [Console] Changed the button name and message for mass and tag delivery
    * Changed the button name of Schedule Delivery to reduce confusion because pressing the mass delivery or tag delivery button immediately executes delivery.

#### Bug Fixes
* [API] Added a field that was omitted for webhook delivery
    * Added the recipientNo field that was omitted for webhook delivery.
* [API] Fixed a bug that occurred to the statisticsType field in Statistics API
    * Modified so that the statisticsType field works normally when calling Statistics API.

### July 26, 2022
#### Feature Updates
* [API] Enhanced validation of recipient numbers
    * Modified so that, if the length of the recipient number exceeds 15 characters, the number is determined as not conforming to international telecommunication standards (E.164 of ITU-T) and the request is processed as a failure.
* [API] Added the validation logic for tags
    * Modified so that, if a request is made to send a tag that does not exist, the request is processed as a failure.

### April 26, 2022
#### Feature Updates
* [Console] Improved the personal information masking logic used when downloading results of general, mass, or tag delivery
    * Modified the logic so that it is the same as the masking logic used in the console.
* [Console] Modified the validation logic for the extension of the uploaded document when registering a sender number with the document authentication method
    * Modified the logic so that extension checking is not case-sensitive when validating the extension of the uploaded file.
    * Made adjustments so that files with jpeg extension can be uploaded normally.

### March 29, 2022
#### Feature Updates
* [Console] Improved and changed the feature to download general, mass, and tag sending results
    * For Excel download, changed to create a .zip file for more than 1 million results.
* [API] Fixed an error where -9999 was returned when an emoji was included in the template parameter when sending regular SMS/LMS/MMS
    * Modified so that -2023 (Title or body includes characters that are not allowed (e.g. emojis)) is returned.

### January 25, 2022
#### Bug Fixes
* [Console] Fixed a bug where, when a 4-byte emoji is included in the template parameter for mass delivery, it is left as in-progress status
    * Modified so that, if the template parameter includes a 4-byte emoji, it is filtered as an invalid recipient.

### December 14, 2021
#### Feature Updates
* [Console] Changed format of template file (Excel, CSV) for mass delivery
    * The num column has been removed from the template file for creating mass delivery recipient information.
        * Files created based on the existing template can also be used.

### September 14, 2021
#### Added Features
* [API] Added a mass delivery query API
    * Added an API to query mass delivery requests and mass delivery recipients.

### July 27, 2021
#### Added Features
* [Console] Added update webhook as a result of sending
    * Users can receive webhook when updating the result on the request to send.
        * Users can receive webhook when succeeded or failed to send.

#### Feature Updates
* [Console/API] Enhanced duplicate check when registering calling number
    * The number that has already been registered as well as the number requested for registration has been modified so that it cannot be duplicated.

### April 27, 2021
#### Feature Updates
* [Console] Added the feature of moving a template between categories
    * Updated to allow template transfer between categories.
* [Console/API] Supports dispatch of MS949
    * Updated system to allow dispatch in MS949 encoding for domestic sending.

### March 23, 2021
#### Feature Updates
* [Console] Improved the process of sending a large volume of SMS
    * Improved the process of sending a large volume of SMS.
        * The following two lists are available for download if invalid recipients are included in the uploaded recipient file.
            * Download the list of valid recipient numbers
            * Download the list of invalid recipient numbers
        * If you want to send SMS to valid recipients only, you can do so by clicking the **Send Text Messages to Valid Recipients Only**  button.
* [Console] Statistics feature improved/changed
    * Improved to enable viewing the stats by any one of the following: template ID, message type, presence of ad, and calling number.
* [Console/API] Disallowed use of some special characters for template ID
    * Improved to disallow the use of '/', '?', ':' for template ID.

#### Bug Fixes
* [Console] Fixed the bug of not exposing the calling number registration request details
    * Fixed the bug where the calling number registration request details popup would not be exposed properly.

### January 26, 2021
#### Added Features
* [Console/API] Added a feature of sharing blocked 080 phone numbers across projects
    * A single 080 phone number can be shared across multiple projects.
    * Only the project that requested an 080 phone number can share phone numbers.
    * Only the requested project can edit the business name and cancel the business.
    * The blocked 080 phone numbers are shared.

### December 29, 2020
#### Feature Updates
* [API] Applied partial changes to response body of SMS sent list search and send single search for short text/long text/authentication
    * Added the messageType/recipientSeq field.
    * Deleted the mtPr field.
    * For more information, see [[API Guide](./api-guide/#sms_4)].

### October 27, 2020
#### Feature Updates
* [Console/API] Changed body validation when sending advertisements
    * Modified so that brackets can be omitted from the text **[Unsubscribe for free]** included in the body.

### September 22, 2020
#### Added Features
* [Console] Added a 080 opt-out webhook feature
    * Added a feature to receive a mobile phone number that has been opt out by 080 opt-out with a webhook.

#### Feature Updates
* [API] Fixed a bug of API to query result updates based on received time
    * Fixed an issue where, when querying result updates based on the received time, the query failed if the base month of the sending date and the receiving date were different.
* [Console] Changed the cancellation deadline for 080 opt-out
    * Modified so that 080 opt-out cancellation is possible 24 hours after approval.

### August 25, 2020
#### Feature Updates
* [Console] Enhanced validation of Excel for mass delivery
    * If the recipient's number is not entered, validation will fail.
* [Console/API] Enhanced validation of attachment image format
    * When uploading attachments for MMS delivery, validation will fail if the image format is not .jpg.
* [Console] Enhanced validation of backup settings
    * If '/' is appended before or after the file save path in the backup setting of the **Delivery Setting** tab, the validation will fail.

#### Bug Fixes
* [Console] Fixed an issue where a file uploaded from the **Register Sender Number** tab could not be downloaded
    * Fixed an issue where, after uploading a file with document authentication in the **Manage Sender Number > Register Sender Number** tab, the file could not be downloaded.

### July 28, 2020
#### Bug Fixes
* [Console/API] Fixed an issue related to creating email-type template ID
    * Fixed an issue where querying, modifying, or deleting does not work if a template ID is created in the email type.
* [Console/API] Fixed infrequent errors in the length check for sending international text messages
    * Fixed infrequent errors of length check resulting in failed delivery of international text messages

### June 23, 2020
#### Feature Updates
* [Console] Changed charging information on the **Setting for Unsubscribe 080 Numbers**
    * Modified invalid charging information on the **Setting for Unsubscribe 080 Numbers** tab.

#### Bug Fixes
* [Console] Unable to download attached files from the detail page of scheduled delivery list
    * Fixed the 404 error occurred at the click of an attached file within View Details of Scheduled Delivery on the **Query SMS Request** tab.

### April 28, 2020
#### Feature Updates
* [Console] Backup is available for delivery list data older than 180 days
    * Newly added a feature of creating backup files in OBS or AWS S3 for delivery data (general/mass/tag) that are older than 180 days.
    * Backup setting is available on the **Delivery Setting** tab.
* [API] Added Request ID as a filter condition for Search Statistics API
    * Added a filter condition of listing request IDs for **Search Statistics- Based on Event/Request Time**.

#### Bug Fixes
* [Console] Fixed error of pagination on the list of failed data from the **Mass/Tag Delivery List** tab.
    * Fixed inoperability of pagination while listing failure from the **Mass/Tag Delivery List** tab.

### March 24, 2020
#### Feature Updates
* [Console/API] Updated Statistics
    * When **Statistics Event Key (statsId)** is included for a delivery, you can query by **Statistics Event Key** on the statistics page.
    * The **Statistics** page of the past shall be replaced by **(Old) Statistics**.
    * Added the **Statistics Event Key Management** and the **Statistics** page.

### February 25, 2020
#### Added Features
* [API] Added API for Tag Management
    * Following APIs have been added:
        * Query/Register/Modify/Delete Tags
        * List/Get/Register/Delete UIDs
        * Register/Delete Contact
* [API] Added Filters for List Scheduled and API for Cancel Search Result of List Scheduled
    * Added senderGroupingKey, recipientGroupingKey conditions to List Scheduled
    * Added API to cancel in many conditions, other than Cancel Single Schedule API

#### Feature Updates
* [ETC] Changed Charging Policy for Unsubscription
    * The hourly charging policy for unsubscription has been changed into monthly-based policy.
* [Console/API] Increased Size of Document Upload Files along with Sender Number Registration
    * Document upload file volume has been increased to 10MB.

#### Bug Fixes
* [Console] Fixed Unavailability of Same Document Uploads for Sender Number Registration
    * Failure in uploading documents in same file names has been fixed.
* [Console] Fixed Error in Downloading Search Result from List of Sent Messages
    * Fixed failure in file creation when there is a lot of data to send

### January 21, 2020
#### Feature Updates
* [Console] Change of Scheduling for Mass/Tag Message Delivery
    * When mass or tag delivery time is scheduled, the **Check and Schedule Delivery** button is deleted, to show the **Schedule Delivery** button only.
* [Console/API] For scheduled delivery, tighter validation is applied on past time.
    * Modified to specify scheduling up to three hours before the current time.

### December 24, 2019
#### Added Features
* [API] Added API for Registering Unsubscribed Users
    * Added API to register users who unsubscribe 080 numbers

#### Feature Updates
* [Console] Updated Query Page for Unsuscribed Users
    * Added the filter-search feature for unsubscribed 080 numbers

### November 26, 2019
#### Added Features
* [Console] Added the Feature of Ad Delivery Restriction Time
    * Night-time ad delivery can be restricted.

#### Feature Updates
* [Console] Changed Mailing Period for Guide on Failed Mass/Tag Message Delivery
    * In case mass/tag messages are not actually delivered even after [Confirm and Send] is selected without time scheduled, they are 'Processed as Failure' one day after registered, and guiding mail is sent to users.
    * If  a message is not actually delivered after [Confirm and Send] is selected with time scheduled, it is 'Processed as Failure' one day after scheduled time and guiding mail is sent to user.
* [Console] Updated to show byte counts for title or body on the deliver page
* [Console] Updated to query by the 'Sender Registrant' on the General/Mass/Tag delivery page

### October 29, 2019
#### Added Features
* [Console] Canceling scheduled delivery in batch
    * Delivery can be canceled only for the messages waiting to be scheduled

#### Feature Updates
* [Console] Warning message for remaining resources with SMS service disabled
    * While the 080 number rejection service is enabled, it must be disabled first to disable the SMS service.
* [Console/API] Sending ad messages only when the 080 number rejection service is enabled
    * Delivery is available only when the 080 number rejection service is 'Enabled'.
* [API] Tighter validation for the delivery of certification messages
    * Message delivery is unavailable when authentication message is not included
    * For more details, see [[API User Guide](./api-guide/#precautions-authword)].

#### Bug Fixes
* [API] Fixed the issue in which the comment field is not properly shown when querying history of sender number authentication requests

### September 24, 2019
#### Feature Updates
* [Console] Improved/updated Query Tab of Web Console
    * Combined the **List Delivery per Request** page and the **List Scheduled Delivery** page.
    * Messages are queried by date of registration.
* [Console] Added **Register Sender Number** within **Template Management**
    * Added the **Register Sender Number** button for delivery page, also onto the **Template Management** tab.
* [Console] Deleted the **Cancel All Schedules** button from the **Query Scheduled SMS Delivery** tab.

#### Bug Fixes
* [Console/API] Fixed Partial Failure in Scheduled Delivery
    * Infrequently, messages scheduled as of current time were partially not delivered.
    * Fixed the issue so as messages, which are scheduled even as of current time, can all be delivered.


### August 27, 2019
#### Feature Updates
* [Console] Allowed more length for a business name,  for the setting of Rejection of Receiving 080 Numbers
    * Updated to save up to 100 characters for a business name
* [Console] Added a column on an excel sheet when downloading files on the list of each request

#### Bug Fixes
* [Console] Fixed an issue in which delivery result is not available due to 'telecom provider's issue', when recipient's number has been managed by the prevention service of sender number falsification
* [Console] Fixed an issue in which the warning window does not show, if query of each SMS request exceeds the maximum period (30 days)

### July 23, 2019
#### Added Features
* [API] Category API Added
    * Provide APIs for Register/Query/Edit/Delete Category.
* [API] Template API Added
    * Provide APIs for Register/Query/Edit/Delete Template.

#### Feature Updates
* [API] Validation Tightened for Title/Body in Text Delivery
    * More restrictions in the length of title/body, from v2.2 API.
    * SMS (Body: Up to 255 characters), LMS/MMS (Title: Up to 120, Body: Up to 4000 characters)
* [API] Template Delivery Higher on Priority of Request Parameter
    * Modified, as of v2.2 API, that data saved on a template cannot be used when request parameter includes title, body, sender number, or attached files, if delivery is requested via template.

#### Bug Fixes
* [API] Updated for the query of scheduled delivery details, to respond with defined codes when the query is attempted with invalid request ID.


### June 25, 2019
#### Added Features
* [Console] Added sender's group key and recipient's group key in the query of request by SMS
    * Sender's group key and recipient's group key have been added as part of query conditions for each SMS request.
* [Console] Added check boxes to include or exclude title/body in scheduling a file downloading after query of delivery list
    * Check boxes have been added to include title/body or not, in the request of downloading after query of general/mass/tag delivery list.
* [API] Added Create/Query/Download delivery list in files for General messages
    * APIs have been added to download general delivery list in files.

#### Bug Fixes
* [Console] Fixed template parameter which is not replaced and therefore not visible, in the query of scheduled delivery list and details
    * The issue of template parameter which was not properly replaced in the query of scheduled delivery list and details has been fixed.
* [Console/API] Fixed bugs in collecting statistics
    * It has been modified to collect duplicate delivery as failed delivery, not as ready for receiving


### May 28, 2019
#### Feature Updates
* [Console/API] Improved performance of scheduled delivery

#### Bug Fixes
* [API] Fixed to respond with defined codes for the query of template details, when it is tried with invalid template
* [Console] Processing disallowed characters for template registration/modification
    * When a template is registered/modified, if disallowed characters (emojis) are tried, defined errors, not system errors, are sent as response.
* [Console/API] Added validation for empty attached files when uploading attached files for sender number authentication
    * If attached file is missing for upload, defined error is sent as response, instead of a system error.

### April 23, 2019
#### Feature Updates
* [API] Increased LMS/MMS length limit
    * Improved so that LMS/MMS titles can be saved up to 120 characters.
    * Depending on the device/carrier, the length of the sent title may be differently.
* [API] Improved to respond with an error when performing international delivery by LMS/MMS
    * Improved to respond with -2024 error when performing international delivery by LMS/MMS
    * LMS/MMS does not support international delivery.
* [Console] Improved the length restriction issue of the template management screen
    * Improved so that, when registering/modifying a template, the replacement element is not included in the length limit calculation.
* [API] Fixed an API response error
    * Improved to respond with -9998 instead of server error (500) when calling an API that does not exist.

#### Bug Fixes
* [Console] Fixed an issue where the query failed intermittently when querying an opt-out list
    * Resolved the issue where the query failed intermittently when querying an opt-out list.
* [Console] Fixed an error for registering template attachments
    * Fixed an issue where, when registering a template with attachments, the attachments were not registered properly.
* [Console] Fixed an error for querying a template that does not exist
    * Fixed an issue where, when calling an unavailable template directly from the console, an unexpected error occurred.
* [API] Fixed a but in scheduled delivery
    * Improved an issue where, when the scheduled time is set to the current time and there are many recipients for a scheduled delivery, a duplicate delivery occurred intermittently.

### March 26, 2019
#### Feature Updates
* [Console/API] Retention period changed for delivery history
    * Delivery history can be queried down to 6 months before.
* [Console] Query page improved for template list
    * The query page for template list has improved in speed.
* [API] Improved validation for the request of sender number registration
    * Added validation to send failure if request for sender number registration is made under invalid attached file ID


### February 26, 2019
#### Added Features
* [API] Sender numbers added as part of query conditions requesting for history of sender number authentication
    * Added to query request history with sender numbers (sendNo).
    * See [[API Guide](./api-guide/#api_1)] for more details.

#### Feature Updates
* [Console] Input windows improved for sender number on the page requesting for message delivery
    * Search is available on the input window for sender numbers
* [Console/API] Improved validation for guidance message for rejection of receiving 080 numbers
    * Improved to not check space for "(Ad) [Reject Receiving Charge-free] 080xxxxxxx", which is required in the body of ad messages

### February 19, 2019
#### Feature Updates
* [Console/API] Longer template ID
    * Allowed length of template ID has changed to 50 characters, from 10

### December 27, 2018
#### Added Features
* [Console/API] Restriction of Duplicate Delivery
    * You may set whether to enable duplicate delivery, as well as block time
    * The service is disabled by default, and if it is enabled, a same message is not re-sent during configured time period.
    * See [[Console Guide](./console-guide/#_28)] for more details.

#### Feature Updates
* [Console] Korean goes unbroken if downloaded CSV file opens up with excel
    * Added BOM type, so as Korean opens unbroken even on CSV file from excel
* [API] Query Message API field added, as of result updates
    * Telecom providers' codes are added to Query Message API field as of result updates.
    * See [[API Guide](./api-guide/#_28)] for more details.

### November 27, 2018
#### Added Features
* [API] Query Delivery Result Updates API Added
    * Added API to query delivery results as of updated time

#### Feature Updates
* [Console/API] Update cycle changed for delivery results
    * Cycle of updates of receiving result on device changes from 1 minute to 5 seconds.
* [API] Change of Sender Number Checks
    * If sender number is registered with "-" (hyphen), delivery request is available even without hyphen.
* [API] Template Delivery Higher on Priority of Request Parameter
    * In case a parameter, requested with templates, includes title, body, sender number, and attached file, data saved in the templates shall not be applied.
* [Console/API] Query of delivery list by the minute is available up to the next 59 seconds.
    * If period is specified in the query of delivery list, such as :"year/month/day hour:minute", you can query up to the next 59 seconds.
    * e.g.) To query 2018-11-20 00:00 ~ 2018-11-20 01:00, query data between 2018-11-20 00:00:00 ~ 2018-11-20 01:00:59
* [Console/API] If title/body includes unavailable characters (emojis) to send, respond with defined errors.
    * If request includes unavailable characters (emojis) to send text messages, 2023 Response Code is sent as response.
* [API] Template parameters are available for tag delivery.
    * You can send with template parameters to send request by tags.
    * See [[API Guide](./api-guide/#sms_11)] for more details.

#### Bug Fixes
* [API] Fixed invalid error code response
    * For text delivery request, if a request field includes past time, it is responded with -2022 error, not -2021.
    * Error code -2021 occurs in a system when it fails to save in a message queue.

### August 28, 2018
#### Added Features
* [API] Query/Cancel Scheduled Delivery API Added
    * Added API to list and cancel scheduled delivery.
    * See [[API Guide](./api-guide/#_73)] for more details.

#### Feature Updates
* [Console/API] Length restriction in file name of attachment when uploaded
    * Server error occurred when the length of an attached file name exceeded 45 characters.
    * Allows up to 45 characters only, including extension, and after 45, it shall be responded with failure code.

* [API] Parameters to group by sender/recipient
    * As of v2.1 API, if parameters include senderGroupingKey or recipientGroupingKey for message delivery, query is available after filtering in Query API.
    * See [[API Guide](./api-guide/#sms_1)] for more details.
    * Changes on the console page will be applied during next maintenance.

* [API] Paging parameter applied for the Query Request for Sender Number Authentication API
    * The number of exposed items can be controlled by paging parameter in the Query Request for Sender Number Authentication API.

### June 26, 2018
#### Feature Updates
* [Console] Change guiding email text regarding ready for mass/tag delivery
    * Changed email text, sent when message is not actually sent after 'Scheduled Delivery after Check' is clicked.

### May 29, 2018
#### Feature Updates
* [Console] Added validation for sender number registration via document authentication
    * Modified to check number validity after sender number is entered and before pop-up is exposed on document authentication.
* [Console] File volume restricted for the upload of mass delivery (excel or csv)
    * The largest recipient uploading file for mass delivery is restricted to 5M.
* [Console] Restriction of the number of template creation
    * The largest available number of template creation is restricted to 100.
* [Console] Change in alert message exposed when same ID of deleted template is created
    * Improved the alert message on errors exposed when same ID of a deleted template is recreated.
* [Console] Allowed to cancel mass delivery when it is "ready"
    * It is possible to cancel mass delivery even when it is "ready".

### April 24, 2018
#### Added Features
* [Console] Added exporting files (csv) for the history of mass/tag delivery
    * Mass/tag delivery history can be downloaded in csv files.

#### Feature Updates
* [Console] Available to use many 080 numbers
    * Many 080 numbers are available in a single project.
* [Console] Name change of business rejected for receiving 080 numbers
    * Name change of business is available on the page of rejection of receiving 080 numbers.
* [Console] Tab menu on service page moved to console
    * Tab menu can be located on console, to move to pages, from the menu on the left or on the top right.
* [Console] "Processing" status deleted from query by recipient on the query of mass delivery request
    * The "Processing" status which is not used, has been deleted.
* [Console] Characters restricted in category/template name and description
    * Category/template name can have no more than 50 characters, or 100 for description.
* [Console] Download CSV Files added on the UID & Contact registration page
    * CSV files, including sample data, can be downloaded on the UID & Contact Registration page.

#### Bug Fixes
* [Console] Fixed error of uploading mass delivery files
    * Fixed the issue of failed operations of an attached file in the same name when it is re-uploaded after uploaded with invalid attached file
* [Console] Added validation for the input window of mobile phone numbers
    * Same validation is added to the input box for mobile phone numbers.
    * Characters are restricted between 8 and 15, allowing numbers or hyphen (-) only.
* [API] Modified error in query history of sender number authentication
    * Fixed the issue of not returning normal data when status code was inserted as filter in the history of requesting sender number authentication.


### March 22, 2018
#### Added Features
* [Console] Downloading CSV added
    * Added the feature of downloading CSV on the page of query by SMS request, and of setting for rejection of receiving 080 numbers.
        * CSV files can be downloaded by search conditions, for the query by SMS request.
        * On the page of rejection of receiving 080 numbers, the list of rejection targets can be downloaded in csv files.
* [Console/API] Deletion of rejection target of receiving 080 numbers
    * Target of rejection can be deleted on console or API.
    * See [[API Guide](./api-guide/#_55)] for more details.
* [API] Scheduled delivery added
    * Scheduled delivery is available for SMS/MMS/AUTH.
    * See [[API Guide](./api-guide/#sms_2)] for more details.

#### Bug Fixes
* [Console] Fixed the bug in which query was unavailable after request status was selected as "failure" on the query page by SMS request
    * Fixed the issue of unavailability of data query when the request status was selected as "failure".
* [Console] Fixed the error of Guide content, which was automatically entered with the selection of ad delivery
    * Modified as the automatically entered body message includes error
        * "[Reject Receiving Charge-free]" -> "[RejectReceiving Charge-free]"

### February 22, 2018
#### Added Features
* [Console/API] Tag delivery added
    * The function of tag delivery has been added.
    * Managing tags, UIDs, and recipient numbers are available now.

#### Feature Updates
* [Console] User message improvement
    * Messages on pages and pop-ups have been partially improved.
* [Console] Cancellation of general/mass delivery schedules
    * Schedule can be canceled on the page of query scheduled delivery of SMS.
    * Cancellation is available by the request on the page of query mass SMS delivery
* [API] List Sender Number API added
    * Added Query Sender Number API.
    * See [[API Guide](./api-guide/#api_2)] for more details.

#### Bug Fixes
* [Console] Incorrect exposure of tabs on the sender number management page
    * Fixed the error by which wrong tabs show on top, when the sender number management tab is displayed on the "sender number registration" or "sender number query" page.
* [Console] Fixed the error in the click of Show Details on the mass delivery page
    * Fixed the issue by which Show Details are clicked to result in script error and not properly exposed.
* [API] Fixed the bug of attached files
    * Fixed the issue by which delivery was normally processed even with invalid attached file ID.


### December 21, 2017
#### Bug Fixes
* [Console] Fixed delivery bugs on console.
    * Fixed the bug by which 90-byte messages are partially sent, with each space is deemed as 2 bytes.
    * Fixed the bug by which "< character strings in the body message are replaced by &It ;.

### November 23, 2017
#### Added Features
* [API] Request/Query Sender Number Registration API added
    * Request/Query Sender Number Registration which was available only on console is now provided in APIs.
    * See [[API Guide](./api-guide/#_56)] for more details.

#### Deleted
* [API] v1.0 API deprecated

#### Feature Updates
* [Console] Added Schedule Delivery page
    * Check scheduled delivery list on the page.
    * If delivery is made after scheduled time, query on the page of delivery list.
* [Console] Added item for statistics
    * Cases waiting for result after completely delivered were collected as failure.
    * Waiting Item has been added to be separately managed from failed cases.
* [API] Added valid error message with no input of recipient number
    * When recipient number is missing, it is not returned as system error but responded as invalid recipient number.
* [API] Added error message for invalid recipient numbers
    * When the country code is null with the input of invalid number, it is not returned as system error but as invalid recipient number.

#### Bug Fixes
* [Console] Query unavailable with requestId for scheduled delivery
    * Modified the issue in which query was unavailable with requestId, after scheduled delivery, on the page of query delivery.

### October 19, 2017
#### Bug Fixes
* [Console] Fixed the bug in which delivery result of SMS for authentication is not updated

### September 21, 2017
#### Added Features
* [Console, API] Query target of rejection receiving 080 numbers
    * Added querying recipients rejecting 080 numbers and API
    * See [[API Guide](./api-guide/#_52)] for more details.

### August 24, 2017
#### Added Features
* [Console] Send email guidance on approval/denial of sender numbers
    * Guidance is sent to users by email regarding sender number approval or denial.
* [Console] Send emails on approval/denial of rejection of receiving 080 numbers
    * Guidance is sent to users by email regarding approval/denial of rejection receiving 080 numbers.
* [API] Error response added to unsupported ContentType
    * Modified to return as clear error, not a system error, if API requesting header is set with unsupported ContentType.

#### Bug Fixes
* [Console] Fixed the bug by which category/template registration is saved only as "enabled"
    * The bug by which category/template registration is saved only as "Enabled" even though the status was actually checked as "Disabled".

### April 20, 2017
#### Added Features
* [API]  Rejection of Receiving 080 Numbers added
    * You may join the rejection service of receiving 080 numbers to send ad messages. [[API Guide](./api-guide/#sms_11)]
    * See [[Rejection of Receiving 080 Numbers](./console-guide/#080)] for more details on subscription.

### March 23, 2017
#### Feature Updates
* [Console] In the query of mass delivery, select window for cause of result has been grouped.

#### Bug Fixes
* [Console] Fixed the bug by which show details of scheduled delivery are returned as empty.


### February 23, 2017
#### Bug Fixes
* [Console] Fixed the bug of abnormal operations of pagination due to error of delivery count in the query of mass MMS delivery.

### January 19, 2017
#### Feature Updates
* [API] Restriction of individuals added to send to a number of recipients.
    * AS-IS: No restriction was available for a number of recipients.
    * TO-BE: Each request can have no more than 1000 recipients.

#### Bug Fixes
* [Console] Fixed the bug, in query delivery, by which field is clicked to show ellipsis on the right of the field.
    * AS-IS: Long title and body results in ellipsis created on the right of the field.
    * TO-BE: Regardless of the length, ellipsis is normally created in the above of each field.

### December 22, 2016
#### Feature Updates
* [Console] Added the function by which query is available for failed delivery cases.
    * AS-IS: For failed SMS or MMS delivery, response is available but cannot be queried on console.
    * TO-BE: If SMS or MMS delivery fails, the request status can be changed to failure so as to allow query.
* [API] Changed the logic to allow only normal cases from validation to be sent to a number of recipients.
    * AS-IS: If request of sending for a number of recipients fails, message is not sent to other recipients after failed recipient.
    * TO-BE: Message is sent to all recipients, and delivery result by recipient is provided at the response. Even with failed delivery, result of request becomes successful. <br/>
      See [[API Guide](./api-guide/)] for more details.
* Method of calculating charges has changed.
    * AS-IS: Charged on the basis of request time of text delivery
    * TO-BE: Charged by the response time for delivery result



#### Bug Fixes
* [API] Fixed the bug in which server error occurred for the sending of MMS attachment when the file was unavailable attached file
    * AS-IS: Responded with server error when sent by unavailable attached file ID
    * TO-BE: Provide error messages for causes of failure, when there is unavailable attached file for MMS attachment delivery   M
* [Console] Fixed the bug of space in CSV template files for mass delivery.
    * AS-IS: Error in parsing when there is space between fields in CSV template files
    * TO-BE: Normal operations for spaces by , between fields in CSV template files

### December 8, 2016
#### Feature Updates
* [Console] Improved/changed template features.
    * AS-IS: When a template was deleted, information was not shown on the delivery history of the template
    * TO-BE: Even when a template is deleted, template information is available on the delivery history of such template. Nevertheless, template, once deleted, cannot be used again, and queried template on the deliver history is to be replaced by recently-modified template.
* [Console] In the query of delivery, the window for selection of cause of result has been improved.
    * AS-IS: When all is selected for the cause of result, the selection window for the cause of result shows all.
    * TO-BE: When all is selected for the cause of result, the selection windows for the cause of result is not available.
* [Console] For sender number registration, validation has been enforced.
    * AS-IS: Check duplicate checks only
    * TO-BE: Duplicate checks + Check sender number registration format    [[Format of Sender Number Registration](./console-guide/#_16)]


#### Bug Fixes
* [Console] Fixed the bug by which MMS template was registered even without title.
    * Issue: Regarding MMS template registration, template can be registered without a title.
    * Solution: To register MMS templates, a check is added to enter title.

### November 24, 2016
#### Feature Updates
* [Console] Features of mass delivery improved/changed.
    * Replacement Improved/Changed: Replacement delivery was available by selecting templates only, but now it is available by entering replacement key on the title or body without selecting a template.
    * Uploading Template Files: In some editors, like excel, even when there is no available data for cell which has editing history, empty character string data are included in saving. Now, it has been changed that empty character strings which are not within the range of input are ignored from validation to upload template files.
    * Caution Messages Added: In some editors, like excel, CSV template files are created and uni codes are not saved, which results in broken characters. Cautions on such issue are to be provided for template downloads and schedule delivery.

#### Bug Fixes
* [Console] Modified error on the mass delivery page.
    * Modified event errors: Modified the error in which an alert shows like 'Select a delivery request to query', at the click of the header of the request list.

### October 20, 2016
#### Feature Updates
* [Console] Mass upload delivery has improved/changed.
    * CSV Format Supported for Mass Upload Delivery: Mass upload delivery becomes available not only on excel but CSV file (with CSV template provided)
    * Check Process of Selective Targets: Allows to selectively check target of delivery of mass upload
    * Enforced File Validation for Mass Upload Delivery: Check if there is any invalid format of recipient number or missing data in the file
    * Tab Added for Mass SMS Delivery: Mass SMS Delivery tab has been added to check target and progress status of mass upload delivery
    * For reference: Notification > SMS > Getting Started > Mass Upload Delivery
* [Console] The Query by SMS Request tab has changed to Group Window Selecting for Reasons.
    * For reference: Notification > SMS > Getting Started > Query by SMS Request
* [API] If the request body of Send SMS API is empty, delivery is deemed as failure.
    * AS-IS: If the body is empty, response may be successful but since content is missing, it is deemed as undelivered. When queried, it shows error in message type.
    * TO-BE: If the body is empty, response fails along with the message that body message is empty.

#### Bug Fixes
* [Console] Modified abnormal operations after SMS delivery, on Firefox .
    * Issue: After delivery, sender numbers could not be selected or field was not returned to default.
    * Solution: Modified to return field to default or normally operate

### September 29, 2016
#### Feature Updates
* [Console] Modified to allow sending SMS from the web page, to recipient numbers which include country code.
    * Reference: Notification > SMS > Getting Started > Send General SMS, General LMS, and MMS

#### Bug Fixes
* [API] Fixed the bug in which userId is always responded with null, among responses for Query SMS Delivery API.
    * Issue: userId was responded always with null, among responses for Query SMS Delivery API.
    * Solution: userId is responded with normal data, among responses for Query SMS Delivery API.
* [API] Fixed the bug in which sendType is responded with abnormal data, among responses for Query Single SMS Delivery for Authentication.
    * Issue: SMS type for sendType is 0, among responses for Query Single SMS Delivery for Authentication API.
    * Solution: Send 2, which is normal data, for sendType, among responses for Query Single SMS Delivery for Authentication. (0:SMS, 1:MMS, 2:Auth)

### August 18, 2016
#### Feature Updates
* [API] Field added for recipient numbers, including country code
    * Reference: Notification > SMS > Developer's Guide > [Send Single SMS, Send Long MMS, Send SMS for Authentication] API Specifications (internationalRecipientNo field added)

#### Bug Fixes
* [Console] Allowed to normally change to enable or disable SMS template.
    * Issue: Even after service is changed to Enable/Disable, it was not normally applied when template was called.
    * Solution: Modified to normally save and expose the enable/disable field

### August 4, 2016
#### Bug Fixes
* [API] Modified to allow MMS sending if SMS sender number is registered by mobile phone authentication.
    * Issue: Send numbers saved by mobile phone authentication were saved in the format of international numbers, which prohibited MMS delivery.
    * Solution: Modified to change the saving format to the Korean mobile phone numbers (01x-xxxx-xxxx).
* [API] Modified to allow sending messages even if the countryCode is missing.
    * Issue: When the countryCode is sent as empty value (null, ""), response is successfully exposed but not normally sent
    * Solution: Modified to set 82 as the default country code, if it comes with empty value
