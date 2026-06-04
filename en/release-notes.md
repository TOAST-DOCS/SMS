## Notification > SMS > Release Notes

### June 23, 2026
#### Added Features
* [Console] Added daily sending limit feature by country
  * Added a feature to limit daily sending limits by country for international deliveries.
  * You can configure this in **Project Dashboard > Sending Settings > International SMS**.

### March 24, 2026
#### Added Features
* [Console] Added backup result notification for message delivery history
  * You can receive result notifications when setting up backup for message delivery history that has passed the message retention period.
  * You can configure this in **Project Dashboard > Notification Management**.

### December 31, 2025
#### Added Features
* [API] Added 080 unsubscribe number inquiry API
    * Added an API to query 080 unsubscribe numbers.
    * For more details, see [[API v3.0 Guide > 080 Unsubscribe Service](./api-guide/#080)].

### October 28, 2025
#### Feature Improvements/Changes
* [Console/API] Relaxed validation logic for 080 unsubscribe numbers when sending advertising messages
  * Changed to allow normal delivery even when '-' is inserted in the middle of 080 unsubscribe numbers when sending advertising messages.
* [ETC] Notification Management - Added monthly sending limit notification
    * You can automatically receive notifications when reaching 70%, 90%, and 100% of the configured monthly sending limit.
    * You can configure this in **Project Dashboard > Notification Management**.

#### Bug Fixes
* [API] Fixed statistics inquiry bug
  * Fixed a bug where data was retrieved twice when the data search range was set narrowly during statistics inquiry.

### June 24, 2025
#### Feature Improvements/Changes
* [Console/API] Improved international delivery statistics
    * Changed the delivery count event collection time standard in international delivery statistics to be based on delivery success time.
* [ETC] Added Cloud Monitoring metric inquiry
    * Added the ability to query metrics for delivery failure, reception success, and reception failure in the Cloud Monitoring service.

### March 4, 2025
#### Feature improvements/changes
* [Console/API] Improved international delivery reception results to reflect DLR results.
    * Reception success means the DLR status is DELIVERED.
    * Reception failure means the DLR status is EXPIRED, FAILED, REJECTED, or UNKNOWN.
    * If the international delivery result is reception failure, see the following document for reception result codes.
        * [[Reception Result Codes](./error-code/#_1)]
* [Console] International delivery statistics improvements
    * Added message type query filter to international delivery statistics classification.
    * Added reception events to international delivery statistics classification.
    * For more information about statistics events, see [[Statistics > Query Statistics](./console-guide/#_42)].
* [API] International delivery statistics API improvements
    * Added reception events to the international delivery statistics API.
    * For more information, see [[API v3.0 Guide > Search Statistics - International Delivery](./api-guide/#-_2)].

#### Feature additions
* [Console/API] Added domestic/international classification filter and country code filter when viewing delivery lists
    * Added domestic/international classification filter and country code filter to SMS query by request, mass SMS delivery query, and tagged SMS delivery query console screens.
    * Added receiverRegion and countryCode fields to the Query Parameter of all versions of Short SMS delivery list search, Long MMS delivery list search, scheduled delivery list search, SMS for verification delivery list search, mass delivery recipient list search, and tagged delivery recipient list search APIs.
    * For more information, see [[API v3.0 Guide](./api-guide)], [[API v2.4 Guide](./api-guide-v2.4)], [[API v2.3 Guide](./api-guide-v2.3)], [[API v2.2 Guide](./api-guide-v2.2)].

#### Bug fixes
* [API] Fixed international delivery statistics API bug
  * Fixed a bug where conversion rate collection request delivery success count and conversion count events were not visible in the international delivery statistics API.

### November 26, 2024
#### Feature additions
* [ETC] Added CloudTrail log when conversion rate-based blocking occurs
    * When international SMS delivery conversion rate-based blocking occurs, you can check the history in CloudTrail.

### October 7, 2024
#### Feature additions
* [API] Added message count field to message delivery result update webhook
    * Added messageCount field indicating the number of delivered messages to the message result update webhook.
    * For more information, see [[Webhook Guide > Hook definitions by event type > Message delivery result code update](./webhook/#_4)].
* [API] Added international delivery DLR update webhook
    * Added international delivery DLR update webhook.
    * For more information, see [[Webhook Guide > Hook definitions by event type > International delivery DLR update](./webhook/#dlr)].

### September 10, 2024
#### Feature additions
* [Console/API] Added monthly delivery volume limit feature by organization
    * Added monthly delivery volume limit feature by organization.
    * For more information, see [[SMS > Service Policy > Delivery Policy > 'Monthly Delivery Volume Limit'](./sending-policy/#_10)].

### August 27, 2024
#### Feature Addition
* [Console/API] Added statistical events to international delivery statistics classification
    * Conversion standby (READY) and conversion complete (CONVERTED) statistical events have been added to international delivery statistics classification.
    * For more information, see [[API Guide > Statistics Search - International Delivery](./api-guide/#-_2)].
* [Console] Added international SMS delivery conversion rate-based blocking rule configuration feature
    * A feature has been added to configure international SMS delivery conversion rate-based blocking rules.
        * You can configure conversion rate blocking threshold, minimum blocking count, and conversion rate calculation time.
    * For more information, see [[Console User Guide > International SMS Delivery Settings](./console-guide/#sms_8)].
#### Feature Improvement
* [ETC] Changed conversion rate calculation rules when releasing international SMS delivery conversion rate-based blocking
    * The conversion rate calculation rules have been changed to prevent re-blocking when sending converted messages after releasing conversion rate-based blocked countries.
    * For more information, see [[International SMS Delivery Policy > International SMS Conversion Rate-Based Delivery Blocking](./international-sending-policy/#sms_2)].

### July 1, 2024
#### Feature Improvement/Change
* [ETC] Added international delivery supported country codes and changed country names
    * Country code '1939' for Puerto Rico has been added to the list of deliverable countries.
    * The country name of Mayotte has been changed to 'Mayotte/Comoros'.
    * The country name of Netherlands Antilles has been changed to 'Netherlands Antilles/Curaçao'.

### May 28, 2024
#### Feature additions
* [Console/API] Added international SMS delivery conversion tracking feature
    * Delivery API changes
        * Added useConversion field to v3.0 Short SMS delivery, v3.0 authentication SMS delivery, and v3.0 advertising SMS delivery APIs to indicate conversion tracking requests.
        * Conversion tracking requests using this field are valid only for international delivery requests.
    * Added conversion API
        * Added an API to attempt conversion for conversion tracking requests.
    * Added conversion rate-based blocking and notification settings to Service settings > International SMS delivery settings tab
        * Added conversion rate-based delivery blocking and notification features to the international SMS delivery settings screen.
        * Added the ability to configure blocked countries for applying conversion rate-based delivery blocking.
    * For more information, see [[International SMS Delivery Policy > International SMS Conversion Rate-Based Delivery Blocking](./international-sending-policy/#sms_2)].
* [Console] Added conversion rate-based delivery blocking webhook
    * You can receive webhooks when conversion rate-based delivery blocking occurs.
    * Webhooks are triggered when delivery to a specific country is blocked due to conversion rates.
    * For more information, see [[Webhook Guide](./webhook/#hooks)].

### May 14, 2024
#### Feature improvements/changes
* [API] Added message status field to message result update webhook
    * Added messageStatus field to the message result update webhook to indicate message status.
    * For more information, see [[Webhook Guide](./webhook/#hooks)].
#### Bug fixes
* [API] Fixed message result update webhook bugs
    * Fixed a bug where the result code was missing from the message result update webhook sent when delivery failed.
    * Fixed a bug where the message single search API link included in the result update webhook was incorrectly generated when sending international authentication messages.

### April 23, 2024
#### Feature improvements/changes
* [Console] Changed maximum scheduled delivery period
    * Changed the scheduled delivery time to be configurable up to 60 days from the current time.

### March 26, 2024
#### Feature additions
* [API] Added international delivery statistics API
    * Added an international delivery statistics API.
    * For more information, see [[Statistics Search - International Delivery](./api-guide/#_105)].
#### Feature improvements/changes
* [Console] Refined message delivery types
    * Message delivery types are now categorized into SMS, LMS, and MMS when sending and registering templates.

### February 27, 2024
#### Feature additions
* [Console] Role refinement
    * Added functionality to grant separate permissions for SMS menu access and feature control based on roles.
    ([Link](https://docs.nhncloud.com/ko/nhncloud/ko/console-guide/#_24))
#### Feature improvements/changes
* [Console/API] Relaxed advertising mandatory text constraints
    * Changed to allow delivery even when other characters are included at both ends of the free unsubscribe or free rejection text that must be included in advertising messages.
#### Bug fixes
* [Console] Fixed mass delivery and tagged delivery query file download request bug
    * Fixed a bug where filtered lists from mass delivery and tagged delivery recipient-specific searches were not downloaded when requesting file downloads.

### January 23, 2024
#### Feature additions
* [Console/API] Added international SMS delivery DLR result query
    * Added the ability to query DLR results for international SMS delivery.
        * Added dlr field to v3.0 detailed search API.
        * Added DLR status, DLR network code, and DLR error code to console detailed search.
    * For more information, see [[International SMS Delivery Policy](./international-sending-policy)].
    * For DLR status and error codes, see [[DLR Result Codes](./error-code/#dlr)] in the result codes.
#### Feature improvements/changes
* [Console] Separated sender number pre-registration and identity verification tabs
    * Separated the sender number pre-registration tab and identity verification tab.

### December 15, 2023
#### Feature improvements/changes
* [Console/API] Changed individual member service usage policy
    * Terminated SMS services for individual members to provide stable service.
    * Changed to restrict identity verification for individual members, making service usage unavailable.

### November 28, 2023
#### Feature improvements/changes
* [Console] Provided international SMS delivery threshold reached notifications
    * Added functionality to send notification emails to all project members when the threshold for monthly automatic blocking count for international SMS is reached.
    * You can configure whether to use notifications in the sending settings tab.
* [Console/API] Changed body validation for advertising delivery
    * The mandatory text **free unsubscribe** required in advertising message bodies has been relaxed to **free rejection**.
* [API] Improved v3.0 tagged delivery recipient detailed search API
    * Improved v3.0 tagged delivery recipient detailed search to include identification code information.

### October 31, 2023
#### Feature Improvements/Changes
* [Console] Improved international SMS sending settings
    * The sending settings menu has been separated into General SMS and International SMS tabs.
    * Modified to allow direct setting of the 'automatic block monthly limit count'.
    * Added sending allowed country management functionality.
* [Console] Improved SMS message character count limit
    * Relaxed the maximum 255-character limit for domestic SMS sending to align with international SMS standards.
    * Maximum input character count has changed according to encoding (UCS-2: 335 characters, GSM-7bit: 765 characters).
* [Console/API] Improved mass delivery and tagged delivery
    * Applied identification code insertion according to KISA announcement revision to mass delivery and tagged delivery.

#### Bug Fixes
* [Console/API] Fixed bug where LMS/MMS delivery to KT devices was displayed as ETC
    * Fixed a bug where LMS/MMS delivery to KT devices was intermittently displayed as ETC.

### September 26, 2023
#### Feature Improvements/Changes
* [Console] Improved to show identity verification rejection reasons
    * Improved to allow checking identity verification rejection reasons in the identity verification history.
* [Console] Improved international delivery statistics
    * Improved to enable viewing request, delivery, delivery failure, reception, and delivery count events in international delivery statistics.

### August 29, 2023
#### Feature Improvements/Changes
* [API] Template deletion improvement
    * Improved to allow re-registration with deleted template IDs.
* [API] Response field createUser, updateUser improvement
    * Improved to deliver user email in the createUser and updateUser fields during queries.
* [Console/API] Mass delivery cancellation feature improvement
    * When cancellation is requested during mass delivery progress, it will be displayed as "Canceling" until all recipients are canceled, and "Cancellation Complete" after all recipients are canceled.
    * For status codes, see the API Guide [[Mass Delivery List Search](./api-guide/#_30)].
* [API] Added delivery count field to v3.0 list query API and detail query API
    * The delivery count field (messageCount) has been added to v3.0 list query and detail query APIs.
    * When long messages are split into multiple deliveries using the international delivery concat feature, you can check the number of deliveries sent based on character count criteria.
    * For character count criteria, see Service Policy [[Billing Policy](./international-sending-policy/#_3)].
* [Console] Added delivery count column to Query Delivery list
    * You can check the delivery count in the delivery lists for SMS request query, mass SMS delivery query, and tagged SMS delivery query.

### August 1, 2023
#### Added features
* [Console] Added split sending feature
    * Added a feature that distributes mass delivery requests by hour.
* [Console] Added alternative character setting feature
    * Added a feature that converts unsendable characters in the message body/subject to '?' to allow normal delivery requests.
    * This setting can be configured in the **Sending settings** tab.
* [Console/API] Provided international delivery concat
    * When sending international messages, you can send up to 765 characters for GSM-7 and 335 characters for UCS-2, and send long messages through the concat (concatenation) feature.
    * For more information, see [[Billing policy](./international-sending-policy/#_3)] in the service policy.

#### Feature improvements/changes
* [Console/API] Improved international message sending encoding
    * When sending international messages, sends using GSM-7 or UCS-2 encoding based on the character set of the message body content.
    * If the message body consists of GSM-7 basic character set and extended character set, it sends using GSM-7.
    * You can check the encoding result in **View details** in Query delivery.

### July 25, 2023
#### Feature improvements/changes
* [Console] Improved identity verification process
    * Improved so that users can change the business registration certificate registered in the organization when identity verification is rejected and re-requested.
    * Added an item to attach additional documents during identity verification.
* [Console] Improved sender number registration process
    * Added an item to attach additional documents when registering sender numbers.

### July 11, 2023
#### Bug fixes
* [API] Fixed bug that allowed sending with deleted templates
    * Improved so that sending with deleted templates is not possible.

### May 30, 2023
#### Feature improvements/changes
* [Console] Improved identity verification process
    * Improved so that identity verification is required only once within the same organization when using the SMS console.

### April 25, 2023
#### Feature additions
* [Console] Added international delivery monthly send limit feature
    * When you change international delivery to enabled, you can set monthly send limits.

#### Feature improvements/changes
* [API] Added webhook fields for message delivery result code updates
    * Added recipientGroupingKey and senderGroupingKey fields to the message delivery result code update webhook.
* [API] Added identification code field to v3.0 delivery API and detail query API
    * Added identification code field (originCode) to v3.0 delivery API and detail query API.
    * To quickly identify illegal message senders such as caller number fabrication and illegal spam, and to take measures such as service suspension, the identification code of the carrier that originally sent the message is inserted.
        * For special types of value-added telecommunications businesses, you must use the 9-digit registration number listed on the registration certificate.
        * If you don't enter a value in this field, NHN Cloud's identification code is inserted.
* [Console] Added result code value to delivery query detail page
    * Added result code value to the delivery query detail page.
* [Console] Improved file upload for MMS delivery
    * Improved to allow uploading up to 3 attachments when sending MMS.

### March 28, 2023
#### Feature additions
* [Console] Added international SMS usage setting
    * Added a setting to prevent problems such as abuse by setting to **Not in use** when you don't use international SMS delivery functionality.
* [Console] Applied CloudTrail
    * CloudTrail has been applied, allowing you to check usage history.

#### Feature improvements/changes
* [API] Fade-out of sender number-related APIs
    * Some sender number-related APIs have been faded out due to the strengthening of the sender number pre-registration system.
    * APIs subject to fade-out
        * Sender number registration request API
        * Sender number document upload API
        * Sender number verification history search API
    * After the sender number pre-registration system is strengthened, all sender number approval requests registered through APIs will be rejected.

### February 28, 2023
#### Feature additions
* [Console/API] Added validation logic for countries where delivery is possible as specified in the guide
    * Modified to process as failed for countries other than those specified in the guide as countries where delivery is possible.

### February 17, 2023
#### Feature additions
* [Console] Strengthened sender number pre-registration system
    * Added identity verification when using SMS console and owner verification when registering sender numbers.
    * Members who joined after March 2, 2022 must complete identity verification to use the SMS console.

### January 31, 2023
#### Feature improvements/changes
* [Console] Increased file size limit for recipient files when sending mass deliveries
    * Changed from the existing 10 MB to 30 MB.

### October 25, 2022
#### Bug fixes
* [API] Fixed message single search bug
    * Fixed a bug where search results were displayed without type distinction when searching for a single message.

### August 23, 2022
#### Feature improvements/changes
* [Console] Changed mass delivery and tagged delivery button names and text
    * Fixed the misleading part where the delivery process starts immediately when you click the mass delivery or tagged delivery buttons, but the button name was 'Schedule delivery'.

#### Bug fixes
* [API] Added missing field when sending webhooks
    * Added the missing recipientNo field when sending webhooks.
* [API] Fixed statistics API statisticsType bug
    * Fixed so that the statisticsType field is properly applied when calling the statistics API.

### July 26, 2022
#### Feature improvements/changes
* [API] Enhanced recipient number validation
    * Modified to fail requests when the recipient number length exceeds 15 characters, as it doesn't comply with international standards (ITU-T E.164).
* [API] Added tag validation logic
    * Modified to fail requests when requesting with non-existent tags during tagged delivery requests.

### April 26, 2022
#### Feature improvements/changes
* [Console] Improved personal information masking logic when downloading general, mass, and tagged delivery results
    * Modified to use the same logic as the existing console masking.
* [Console] Modified document extension validation logic when registering sender numbers through document verification
    * Modified to check file extensions without case sensitivity when validating uploaded files.
    * Modified so that jpeg extension files can be uploaded normally.

### March 29, 2022
#### Feature improvements/changes
* [Console] Improved/changed general, mass, and tagged delivery result download feature
    * Changed to Excel download, and when exceeding 1 million cases, generate as a .zip file.
* [API] Fixed -9999 response error when template parameters include emojis during general SMS/LMS/MMS delivery
    * Modified to return -2023 (when title or body contains disallowed characters such as emojis).

### January 25, 2022
#### Bug fixes
* [Console] Fixed bug where mass delivery remains in processing when template parameters include 4-byte emojis
    * Modified so that when template parameters include 4-byte emojis, they are filtered as invalid recipients.

### December 14, 2021
#### Feature improvements/changes
* [Console] Changed mass delivery template file (Excel, CSV) format
    * Removed the num column from template files for writing mass delivery recipient information.
        * Files created based on existing templates are still usable.

### September 14, 2021
#### Feature additions
* [API] Added mass delivery query API
    * Added APIs to query mass delivery requests and mass delivery recipients.

### July 27, 2021
#### Feature additions
* [Console] Added delivery result update webhook
    * You can receive webhooks when delivery request results are updated.
        * Webhooks can be received when delivery succeeds and is received, or when delivery fails.

#### Feature improvements/changes
* [Console/API] Enhanced duplicate checking when registering sender numbers
    * Modified so that not only already registered numbers but also numbers requested for registration cannot be registered as duplicates.

### April 27, 2021
#### Feature improvements/changes
* [Console] Added template movement feature between categories
    * Improved to allow template movement between categories.
* [Console/API] Support for MS949 delivery
    * Improved to allow delivery with MS949 encoding for domestic delivery.

### March 23, 2021
#### Feature Improvements/Changes
* [Console] Improved mass SMS delivery process
    * The mass SMS delivery process has been improved.
        * When uploading a recipient file that contains invalid recipients, downloads are provided for the following two list files:
            * Valid recipient number list download
            * Invalid recipient number list download
        * If you want to send SMS only to valid recipients, you can send by clicking the **Send SMS to valid recipients only** button.
* [Console] Statistics feature improvements/changes
    * Improved to allow querying statistics by one of the following criteria: template ID, message type, advertising status, or sender number.
* [Console/API] Template ID character restrictions
    * Improved to prevent using '/', '?', ':' characters in template IDs.

#### Bug Fixes
* [Console] Fixed sender number registration request details view display error
    * Fixed an issue where the sender number registration request details view popup was not displayed.

### January 26, 2021
#### Feature Additions
* [Console/API] Added 080 unsubscribe sharing across projects
    * A single 080 number can be shared across multiple projects.
    * Sharing is only possible from the project that applied for the 080 number.
    * Company name modification and company termination are only possible from the project that applied.
    * The unsubscribe targets for the 080 number are shared.

### December 29, 2020
#### Feature Improvements/Changes
* [API] Partial changes to response body for Short/Long/Auth SMS delivery list search and delivery single search
    * messageType/recipientSeq fields are added.
    * mtPr field is removed.
    * For more information, see the [[API Guide](./api-guide/#sms_4)].

### October 27, 2020
#### Feature Improvements/Changes
* [Console/API] Changed message body validation for advertising deliveries
    * Modified to allow omitting brackets in the **[Free Unsubscribe]** text included in the message body.

### September 22, 2020
#### Feature Additions
* [Console] Added 080 unsubscribe webhook feature
    * Added the ability to receive mobile phone numbers that have unsubscribed through 080 unsubscribe via webhook.

#### Feature Improvements/Changes
* [API] Fixed bug in API for querying result updates based on reception time
    * Fixed an issue where queries would fail when querying result updates based on reception time if the reference months for delivery time and reception time were different.
* [Console] Changed 080 unsubscribe termination period
    * Modified so that 080 unsubscribe termination is only possible 24 hours after approval.

### August 25, 2020
#### Feature Improvements/Changes
* [Console] Enhanced mass delivery Excel validation
    * If recipient numbers are not entered, validation will fail.
* [Console/API] Enhanced attachment file image format validation
    * When uploading attachment files for MMS delivery, validation will fail if the image format is not .jpg.
* [Console] Enhanced backup settings validation
    * In the backup settings of the **Sending Settings** tab, validation will fail if '/' is added before or after the file storage path.

#### Bug Fixes
* [Console] Fixed issue where uploaded files could not be downloaded in the **Sender Number Registration** tab
    * Fixed an issue where files uploaded through document verification in **Sender Number Management > Sender Number Registration** tab could not be downloaded.

### July 28, 2020
#### Bug Fixes
* [Console/API] Email format template ID creation issue
    * Fixed an issue where template IDs created in email format could not be queried, modified, or deleted.
* [Console/API] Intermittent length check error issue during international message delivery
    * Fixed an issue where international message delivery would intermittently fail due to incorrect length checking.

### June 23, 2020
#### Feature Improvements/Changes
* [Console] Changed billing information in **080 Unsubscribe Settings** tab
    * Fixed incorrectly displayed billing information in the **080 Unsubscribe Settings** tab.

#### Bug Fixes
* [Console] Issue where attachment files were not downloading from scheduled delivery list details screen
    * Fixed an issue where clicking attachment files in the scheduled delivery details view in the **Query by SMS Request** tab would result in a 404 error.

### April 28, 2020
#### Feature Improvements/Changes
* [Console] Added backup feature for delivery list data older than 180 days
    * Added a feature that creates backup files to customer OBS or AWS S3 for delivery lists (general/mass/tagged) older than 180 days.
    * Backup settings can be found in the **Sending Settings** tab.
* [API] Added request ID filter condition to statistics search API
    * Added requestId list filter condition to **Statistics Search - Event-based/Request Time-based**.

#### Bug Fixes
* [Console] Fixed pagination error in **Mass/Tagged Delivery List** tab failure list
    * Fixed an issue where pagination did not work properly when querying failure lists in the **Mass/Tagged Delivery List** tab.

### March 24, 2020
#### Feature Improvements/Changes
* [Console/API] Statistics improvements
    * When sending messages that include a **statistics event key (statsId)**, you can query by **statistics event key** in the statistics screen.
    * The existing **Statistics** page is renamed to **(Legacy) Statistics** page.
    * **Statistics Event Key Management** and **Statistics** pages are added.

### February 25, 2020
#### Feature additions
* [API] Added tag management API
    * The following APIs are added.
        * Tag query/registration/modification/deletion
        * UID list query/single query/registration/deletion
        * Contact registration/deletion
* [API] Added scheduled list query filter and scheduled list search result cancellation API
    * Added senderGroupingKey and recipientGroupingKey conditions to scheduled list query.
    * Added API that can cancel with multiple conditions in addition to single scheduled cancellation API.

#### Feature improvements/changes
* [ETC] Opt-out billing policy change
    * The opt-out billing policy changes from hourly billing to monthly billing.
* [Console/API] Increased document upload file size when registering sender number
    * Document upload file capacity is increased to 10 MB.

#### Bug fixes
* [Console] Fixed issue where uploading the same document when registering sender number doesn't work
    * Fixed an issue where uploading documents with the same file name resulted in no action.
* [Console] Fixed error when downloading file from sender list search results
    * Fixed an issue where file creation failed when there was a large amount of delivery data.

### January 21, 2020
#### Feature improvements/changes
* [Console] Changed scheduled feature for mass delivery and tagged delivery
    * When specifying a scheduled time for mass delivery or tagged delivery, the **Confirm and schedule delivery** button is removed and only the **Schedule delivery** button is displayed
* [Console/API] Strengthened validation for past time when scheduling delivery
    * Scheduled time can only be specified up to 3 hours before the current time

### December 24, 2019
#### Feature additions
* [API] Added opt-out target registration API
    * Added API to register opt-out targets to 080 unsubscribe numbers.

#### Feature improvements/changes
* [Console] Improved opt-out user query screen
    * Added search filtering functionality for 080 unsubscribe numbers.

### November 26, 2019
#### Feature additions
* [Console] Added advertising message delivery time restriction feature
    * Added functionality to restrict nighttime advertising delivery hours.

#### Feature improvements/changes
* [Console] Changed failure notification email delivery period for mass/tagged delivery
    * For mass/tagged delivery, if you select [Confirm and send] without setting a scheduled time but don't actually send, the system processes it as 'failed' 1 day after registration and sends a notification email to the user.
    * If you select [Confirm and send] after setting a scheduled time but don't actually send, the system processes it as 'failed' 1 day after the scheduled time and sends a notification email to the user.
* [Console] Improved to display byte count when entering title or body in the delivery screen
* [Console] Improved general/mass/tagged delivery query screen to query by 'delivery registrant' condition

### 2019.10.29
#### Feature additions
* [Console] Added bulk cancellation feature for scheduled delivery
    * You can cancel delivery only for messages in scheduled waiting status among scheduled delivery items.

#### Feature improvements/changes
* [Console] Warning notification for remaining resources when deactivating SMS service
    * If you're using the 080 unsubscribe service, you can deactivate the SMS service after terminating the 080 unsubscribe service.
* [Console/API] Improved advertising message delivery to allow sending only when 080 unsubscribe number status is in use
    * You can send only when the 080 unsubscribe number status is 'In use'.
* [API] Enhanced validation checks for authentication text when sending authentication messages
    * If authentication text is not included when sending authentication messages, the message cannot be sent.
    * For more information, see [[API Guide](./api-guide/#precautions-authword)].

#### Bug fixes
* [API] Fixed an issue where the comment field was not properly displayed when querying sender number authentication request history

### 2019.09.24
#### Feature improvements/changes
* [Console] Improved/changed query tab functionality in web console
    * **Query delivery list by request** and **Query scheduled delivery list** pages have been integrated.
    * When querying messages, they are queried based on registration date and time by default.
* [Console] Added **Register sender number** button in **Template management** tab
    * The **Register sender number** button, which was previously only available on the sending screen, has been added to the **Template management** tab.
* [Console] The **Cancel all scheduled** button has been removed from the **Query SMS scheduled delivery** tab.

#### Bug fixes
* [Console/API] Fixed an issue where some scheduled deliveries were not sent
    * There was an intermittent issue where some scheduled messages were not sent when scheduling for the current time.
    * Fixed so that all scheduled messages are sent even when scheduling for the current time.

### 2019.08.27
#### Feature improvements/changes
* [Console] Increased company name length limit when configuring 080 unsubscribe settings
    * Improved to allow storing company names up to 100 characters.
* [Console] Added columns displayed in Excel files when downloading files from the request list

#### Bug fixes
* [Console] Fixed an issue where delivery results appeared as 'Carrier issue' when the recipient number was subscribed to the caller number fabrication management service
* [Console] Fixed an issue where warning notifications did not appear when querying beyond the maximum query period (30 days) in SMS query by request

### July 23, 2019
#### Features Added
* [API] Added category APIs
    * Provides category registration/query/modification/deletion features through APIs.
* [API] Added template APIs
    * Provides template registration/query/modification/deletion features through APIs.

#### Features Improved/Changed
* [API] Enhanced validation for subject/body when sending messages
    * Starting from v2.2 API, subject/body length restrictions are strengthened when sending messages.
    * SMS (body: maximum 255 characters), LMS/MMS (subject: maximum 120 characters, body: maximum 4,000 characters)
* [API] Modified to prioritize request parameters when sending with templates
    * Starting from v2.2 API, when sending with templates, if request parameters include subject, body, sender number, or attachments, the data stored in templates will not be used.

#### Bug Fixes
* [API] Improved to respond with defined codes when attempting to query scheduled delivery details with invalid request IDs

### 2019.06.25
#### Feature Additions
* [Console] Added sender group key and recipient group key conditions to SMS request-specific queries
    * Added sender group key and recipient group key query conditions to SMS request-specific queries.
* [Console] Added check boxes to select whether to include title or body when scheduling file downloads after querying delivery lists
    * Added check boxes to select whether to include title or body when requesting downloads after querying general, mass, and tagged delivery lists.
* [API] Added general delivery list file creation, query, and download features
    * Added APIs to download general delivery lists as files.

#### Bug Fixes
* [Console] Fixed an issue where template parameters were displayed without replacement when querying scheduled delivery lists and detailed queries
    * Fixed an issue where template parameters were not replaced in scheduled delivery list queries and detailed query screens.
* [Console/API] Fixed statistics collection bugs
    * Fixed to collect duplicate delivery status as delivery failure instead of collecting it as waiting for result reception.

### 2019.05.28
#### Feature Improvements/Changes
* [Console/API] Improved scheduled delivery performance

#### Bug Fixes
* [API] Improved to respond with defined codes when attempting to query with invalid templates during template detail queries
* [Console] Handling of disallowed characters during template registration/modification
    * Fixed to respond with defined errors instead of system errors when registering with disallowed characters (emojis) during template registration/modification.
* [Console/API] Added validation for empty attachment files when uploading attachment files for sender number verification
    * Fixed to respond with defined errors instead of system errors when attachment file content is empty during upload.

### 2019.04.23
#### Feature Improvements/Changes
* [API] Increased LMS/MMS length limit
    * Improved to allow storing LMS/MMS subjects up to 120 characters.
    * Subject length may be transmitted differently depending on the device/carrier.
* [API] Improved to respond with error when sending international messages via LMS/MMS
    * Improved to respond with -2024 error when sending international messages via LMS/MMS.
    * LMS/MMS does not support international delivery.
* [Console] Improved length limit on template management screen
    * Improved so that replacement tags are not included in the length limit when registering/modifying templates.
* [API] Fixed API response error
    * Improved to respond with -9998 instead of server error (500) when calling non-existent APIs.

#### Bug Fixes
* [Console] Intermittent issue where opt-out list query would fail
    * Fixed an issue where opt-out list queries would intermittently fail.
* [Console] Template attachment registration error
    * Fixed an issue where attachments were not properly registered when registering templates with attachments.
* [Console] Fixed error when querying non-existent template
    * Fixed an issue where unexpected errors would occur when directly calling non-existent templates from the console.
* [API] Fixed scheduled delivery bug
    * Improved an issue where intermittent duplicate deliveries would occur when setting the scheduled time to the current time with many recipients during scheduled delivery.

### 2019.03.26
#### Feature Improvements/Changes
* [Console/API] Changed delivery history retention period
    * Queryable delivery history is now limited to 6 months from the current date.
* [Console] Improved template list query screen
    * Improved the speed of the template list query screen.
* [API] Improved sender number registration request validation
    * Added validation to fail requests with invalid attachment IDs when requesting sender number registration.

### 2019.02.26
#### Feature Additions
* [API] Added sender number to sender number authentication request history query conditions
    * Added the ability to query request history by sender number (sendNo).
    * For more details, see [[API Guide](./api-guide/#api_1)].

#### Feature Improvements/Changes
* [Console] Improved sender number input field on message delivery request screen
    * Improved to enable search functionality in the sender number input field.
* [Console/API] Improved 080 opt-out guide text validation
    * Improved to not check for spaces in the mandatory text "(Advertisement) [Free Opt-out]080xxxxxxx" that must be included in the message body when sending advertising messages.

### 2019.02.19
#### Feature Improvements/Changes
* [Console/API] Increased template ID length
    * Template ID length limit changed from 10 characters to 50 characters.

### 2018.12.27
#### Feature Additions
* [Console/API] Duplicate delivery restriction feature
    * You can set whether to use duplicate delivery restriction and the duplicate delivery blocking time.
    * The default setting is disabled, and when this feature is enabled, identical messages will not be resent for the configured time period.
    * For more details, see [[Console Guide](./console-guide/#_28)].

#### Feature Improvements/Changes
* [Console] Fixed Korean text corruption when opening downloaded CSV files in Excel
    * Added BOM type to prevent Korean text corruption when opening CSV files in Excel.
* [API] Added field to message query API based on result update criteria
    * Added carrier code to the message query API field based on result update criteria.
    * For more details, see [[API Guide](./api-guide/#_28)].

### November 27, 2018
#### Feature Additions
* [API] Added delivery result update query API
    * Added an API that can query based on the time when delivery results were updated.

#### Feature Improvements/Changes
* [Console/API] Changed delivery result update interval
    * The update interval for reception results from devices has been changed from 1 minute to 5 seconds.
* [API] Changed sender number check functionality
    * When registering a sender number, if it was previously registered with a "-" (Hyphen), it has been modified to succeed even when sending requests without the Hyphen.
* [API] Modified template delivery requests to give higher priority to request parameters
    * When making delivery requests with templates, if request parameters include subject, body, sender number, or attachments, the data stored in the template is no longer used.
* [Console/API] Modified to query up to 59 seconds when delivery list period queries are requested by minute-level dates
    * When specifying a period in delivery list queries, if querying by "year/month/day hour:minute" format, it queries up to 59 seconds.
    * Example) When querying from 2018-11-20 00:00 to 2018-11-20 01:00, data between 2018-11-20 00:00:00 and 2018-11-20 01:00:59 is queried
* [Console/API] Improved to respond with defined errors when subject/body contains unsendable characters (emojis)
    * When requesting text message delivery with unsendable characters (emojis), it has been modified to respond with -2023 response code.
* [API] Improved to allow template parameters when sending tagged messages
    * When making delivery requests with tags, it has been improved to also allow sending with template parameters.
    * For more details, see [[API Guide](./api-guide/#sms_11)].

#### Bug Fixes
* [API] Improved incorrect error code responses
    * When making text message delivery requests, if a past time is entered in the request field, it has been modified to respond with -2022 error instead of -2021 error.
    * -2021 is an error that occurs in the system when failing to store in the message queue.

### August 28, 2018
#### Feature Additions
* [API] Added scheduled delivery list query/cancel API
    * Added an API that allows you to query and cancel scheduled delivery lists.
    * For more information, see [[API Guide](./api-guide/#_73)].

#### Feature Improvements/Changes
* [Console/API] File name length restriction when uploading attachments
    * There was an issue where server errors occurred when attachment file names exceeded 45 characters.
    * Only up to 45 characters including the extension are allowed, and failure codes are returned for longer names.

* [API] Provided parameters for grouping by sender/recipient
    * Starting from v2.1 API, when sending messages, if you include senderGroupingKey and recipientGroupingKey in the parameters, you can filter and query using the query API.
    * For more information, see [[API Guide](./api-guide/#sms_1)].
    * The console screen will be applied during the next maintenance.

* [API] Applied paging parameters to sender number verification request history query API
    * Improved the sender number verification request history query API to control the number of exposed items using paging parameters.

### June 26, 2018
#### Feature Improvements/Changes
* [Console] Changed mass/tagged delivery waiting status notification email text
    * Changed the text of emails sent when you don't actually send after clicking 'Confirm and schedule delivery' for mass/tagged email delivery.

### May 29, 2018
#### Feature Improvements/Changes
* [Console] Added validation check for sender number registration through document verification
    * Modified to check number validity before displaying the document verification popup after entering a sender number.
* [Console] File size limit for mass delivery upload files (Excel, CSV)
    * Mass delivery recipient upload files are now limited to a maximum of 5MB.
* [Console] Template creation count limit
    * The maximum number of templates that can be created is limited to 100.
* [Console] Changed alert message displayed when creating with the same ID after template deletion
    * Improved the error alert message displayed when recreating with the same ID after deleting a template.
* [Console] Changed to allow cancellation when mass delivery status is "waiting"
    * Improved to allow cancellation even when the mass delivery status is "waiting".

### April 24, 2018
#### Feature additions
* [Console] Added Export feature for mass/tagged delivery history file (csv)
    * You can download mass/tagged delivery history as a CSV file.

#### Feature improvements/changes
* [Console] Multiple 080 recipient numbers can be used
    * You can use multiple 080 numbers in one project.
* [Console] 080 unsubscribe company name can be changed
    * You can modify the company name on the 080 unsubscribe service page.
* [Console] Moved tab menu from product page to console
    * The tab menu was moved to the console, allowing you to navigate pages from the left sub menu or top right.
* [Console] Removed "processing" request status from query by recipient in mass delivery request query tab
    * The unused "processing" status has been removed.
* [Console] Limited characters for category/template names and descriptions
    * Category/template names are limited to 50 characters, and descriptions are limited to 100 characters.
* [Console] Added CSV file download button to UID & Contact registration screen
    * You can download a CSV file containing sample data on the UID & Contact registration screen.

#### Bug fixes
* [Console] Improved mass delivery file upload errors
    * Fixed an issue where re-uploading with the same file name after uploading an invalid attachment file would not work.
* [Console] Added validation for mobile phone number input fields
    * Validation has been added consistently for input boxes where mobile phone numbers are entered.
    * Limited to a minimum of 8 characters and a maximum of 15 characters, with only numbers or hyphens (-) allowed.
* [API] Fixed sender number authentication request history query error
    * Fixed an issue where normal data was not returned when using status codes as a filter in sender number authentication request history.

### 2018.03.22
#### Feature Added
* [Console] CSV download feature added
    * CSV download functionality has been added to the SMS query by request and 080 opt-out settings screens.
        * In SMS query by request, you can download CSV files based on search criteria.
        * In the 080 opt-out settings screen, you can download the opt-out subscriber list as a CSV file.
* [Console/API] 080 opt-out subscriber deletion feature added
    * You can delete opt-out subscribers through the console and API.
    * For more information, see [[API Guide](./api-guide/#_55)].
* [API] Scheduled delivery added
    * Scheduled delivery is available when sending SMS/MMS/AUTH.
    * For more information, see [[API Guide](./api-guide/#sms_2)].

#### Bug Fixed
* [Console] Fixed an issue where queries didn't work when selecting "Failed" as the request status in the SMS query by request screen
    * Fixed an issue where data wasn't retrieved when selecting "Failed" as the request status.
* [Console] Fixed guide message error that was automatically entered when selecting advertising delivery
    * Fixed an error in the automatically entered body content when selecting advertising delivery.
        * "[무료 수신 거부]" -> "[무료 수신거부]"

### February 22, 2018
#### Features
* [Console/API] Added tagged delivery feature
    * Added tagged delivery functionality.
    * Added the ability to manage tags, UIDs, and recipient numbers.

#### Improvements/Changes
* [Console] Improved user messages
    * Improved some screen and popup messages.
* [Console] Added reservation cancellation feature for general/mass delivery
    * Provides reservation cancellation functionality on the SMS scheduled delivery query screen.
    * Provides reservation cancellation functionality by request unit on the mass SMS delivery query screen.
* [API] Added sender number list query API
    * Added sender number query API.
    * For more details, see [[API Guide](./api-guide/#api_2)].

#### Bug Fixes
* [Console] Fixed issue with incorrect tab display on sender number management page
    * Fixed an error where the top tabs were displayed incorrectly when entering the "Register Sender Number" and "Query Sender Number" screens from the sender number management tab.
* [Console] Fixed detail view button click error on mass delivery screen
    * Fixed an issue where a script error occurred when clicking detailed view, preventing normal display.
* [API] Fixed attachment file bug
    * Fixed an issue where delivery was processed normally even when using an invalid attachment file ID (fileId).

### December 21, 2017
#### Bug Fixes
* [Console] Fixed console delivery bug.
    * Fixed a bug where spaces in the message body were calculated as 2 bytes when sending 90-byte content, causing some content to be truncated during delivery.
    * Fixed a bug where "< characters in the message body were replaced with &lt; during delivery.

### November 23, 2017
#### Feature Additions
* [API] Added sender number registration request/inquiry API
    * Sender number registration request/inquiry, which was previously only available in the console, is now provided through API.
    * For more information, see [[API Guide](./api-guide/#_56)].

#### Feature Removal
* [API] v1.0 API deprecated

#### Feature Improvements/Changes
* [Console] Added scheduled delivery page
    * You can check the list of scheduled deliveries on the scheduled delivery page.
    * When scheduled deliveries are sent after the scheduled time, they can be viewed on the delivery list screen.
* [Console] Added statistics item
    * Deliveries pending results after delivery completion were being collected as failed deliveries.
    * A waiting item has been added and is now displayed separately from failed deliveries.
* [API] Added recipient number missing validation error message
    * When recipient number is not entered, the system no longer returns a System Error but responds with an invalid recipient number error.
* [API] Added invalid recipient number error message
    * When an invalid number is entered and the country code is null, the system no longer returns a System Error but responds with an invalid recipient number error.

#### Bug Fixes
* [Console] Fixed issue where search by requestId value didn't work for scheduled delivery
    * Fixed the issue where search by requestId value didn't work on the delivery inquiry screen after scheduled delivery.

### October 19, 2017
#### Bug Fixes
* [Console] Fixed bug where authentication SMS delivery results were not updated

### September 21, 2017
#### Feature Additions
* [Console, API] 080 unsubscribe recipient inquiry
    * Added recipient inquiry feature and API for 080 unsubscribe recipients.
    * For more information, see [[API Guide](./api-guide/#_52)].

### August 24, 2017
#### Feature Additions
* [Console] Send notification emails for sender number approval/rejection
    * Notification emails are sent to users when sender numbers are approved/rejected.
* [Console] Send emails for 080 unsubscribe approval/cancellation
    * Notification emails are sent to users when 080 unsubscribe is approved/cancelled.
* [API] Added error response for unsupported ContentType
    * When ContentType in API request header is set to an unsupported value, it now returns a clear error format instead of a system error.

#### Bug Fixes
* [Console] Fixed issue where usage status was only saved as "In Use" when registering category/template
    * Fixed the bug where usage status was saved as "In Use" even when "Not In Use" was checked during category/template registration.

### April 20, 2017
#### Feature Additions
* [API] Added 080 unsubscribe service feature
    * You can subscribe to the 080 unsubscribe service to send advertising SMS. [[API Guide](./api-guide/#sms_11)]
    * For detailed information about subscription, see [[080 Unsubscribe Service](./console-guide/#080)].

### March 23, 2017
#### Feature Improvements/Changes
* [Console] Grouped result reason select window when inquiring mass delivery.

#### Bug Fixes
* [Console] Fixed bug where detail view screen appeared blank after scheduled delivery.

### February 23, 2017
#### Bug Fixes
* [Console] Fixed bug where pagination malfunctioned due to delivery count error when inquiring MMS mass delivery.

### January 19, 2017
#### Feature Improvements/Changes
* [API] Added limit on number of recipients when sending to multiple recipient lists.
    * AS-IS: No limit on the number of recipients in multiple recipient lists
    * TO-BE: Limited to 1,000 recipients per request

#### Bug Fixes
* [Console] Fixed bug where ellipsis appeared on the right side of fields when clicking field content in delivery inquiry.
    * AS-IS: Ellipsis appeared on the right side of fields when title or content was long
    * TO-BE: Ellipsis appears normally above the corresponding field even when title or content is long

### December 22, 2016
#### Feature Improvements/Changes
* [Console] Added a feature to query failed send requests.
    * AS-IS: When SMS or MMS delivery fails, you receive a failure response but cannot query it in the console
    * TO-BE: When SMS or MMS delivery fails, the request status is changed to failed so you can query it
* [API] Changed the logic to send only valid items when sending to multiple recipient lists.
    * AS-IS: When a send request to multiple recipients fails, recipients after the failed recipient are not sent
    * TO-BE: Sends to all recipients and provides delivery results for each recipient in the response. The request result is successful even if there are delivery failures. <br/>
      For more information, see [[API Guide](./api-guide/)].
* Changed the billing method.
    * AS-IS: Billing based on the time when the message send request is made
    * TO-BE: Billing based on the time when the message delivery result response is received

#### Bug Fixes
* [API] Fixed a bug where a server error occurred when sending MMS attachments with non-existent attachment files.
    * AS-IS: Server error failure response when sending with a non-existent attachment file ID
    * TO-BE: When there are non-existent attachment files in MMS attachment delivery, the failure cause is provided as an error message
* [Console] Fixed a blank space bug in CSV template files for mass delivery.
    * AS-IS: Parsing error when there are spaces around commas between fields in the CSV template file
    * TO-BE: Normal operation when there are spaces around commas between fields in the CSV template file

### December 8, 2016
#### Feature Improvements/Changes
* [Console] Template functionality has been improved/changed.
    * AS-IS: When a template is deleted, template information is not displayed in delivery history sent with that template
    * TO-BE: Even when a template is deleted, you can view template information in delivery history sent with that template. However, deleted template IDs cannot be reused and template content displayed in sending history shows the most recently modified template information
* [Console] The result reason select box has been improved when querying sending history.
    * AS-IS: When result reason is selected as "All", the detailed result reason select box is displayed as "All"
    * TO-BE: When result reason is selected as "All", the detailed result reason select box is hidden
* [Console] Validation has been strengthened when registering sender numbers.
    * AS-IS: Only checks for duplicates
    * TO-BE: Duplicate check + sender number registration format check &nbsp;&nbsp;[[Sender Number Registration Format](./console-guide/#_16)]


#### Bug Fixes
* [Console] Fixed a bug where MMS templates could be registered without entering a title.
    * Issue: When registering an MMS template, the template was registered even without entering a title
    * Resolution: Added logic to check for blank title when registering MMS templates

### November 24, 2016
#### Feature Improvements/Changes
* [Console] Mass delivery feature improved/changed.
    * Replacement feature improved/changed: Previously, the replacement feature was applied only when a template was selected for replacement delivery. Improved so that replacement occurs when replacement keys are entered in the subject or body without selecting a template
    * Template file upload improved: In some editors like Excel, cells with editing history contain empty string data even when there is no cell data. Changed to ignore empty strings outside the input range during validation when uploading template files
    * Added caution notice: When creating CSV template files in some editors like Excel, there is an issue where characters are corrupted because Unicode information is not saved. Added caution notice for this issue when downloading templates and scheduling delivery

#### Bug Fixes
* [Console] Fixed mass delivery page errors.
    * Fixed event error: Fixed error where 'You can view after selecting a delivery request' alert was displayed when clicking the header of the request list

### 2016.10.20
#### Feature Improvements/Changes
* [Console] The mass upload delivery feature has been improved/changed.
    * Mass upload delivery CSV format support: Extended to allow delivery through CSV files as well as existing Excel files for mass upload delivery (CSV template provided)
    * Optional recipient verification process: Improved to allow optional recipient verification in the existing mass upload delivery process where recipients could be verified before sending
    * Enhanced mass upload delivery file validation: Added feature to check for errors such as incorrectly formatted recipient numbers or missing data in files during mass upload delivery
    * Added Mass SMS Delivery tab: Added Mass SMS Delivery tab to check recipient verification and delivery progress status during mass upload delivery
    * Reference: Notification > SMS > Getting Started > Mass Upload Delivery
* [Console] Changed the result reason select window in the SMS Query by Request tab to grouping.
    * Reference: Notification > SMS > Getting Started > Query SMS by Request
* [API] Changed to delivery failure when SMS delivery API request body content is blank.
    * AS-IS: When blank, Response is successful, but actually not sent because there is no content. When querying, displayed as message format error
    * TO-BE: When blank, response failure with content stating that there is no body content

#### Bug Fixes
* [Console] Fixed abnormal operation after SMS delivery in Firefox browser.
    * Issue: After delivery, issues where selection was not possible in the sender number select window or fields were not initialized
    * Solution: Fixed to initialize fields and operate normally after delivery

### 2016.09.29
#### Feature improvements/changes
* [Console] Modified to enable sending to recipient numbers that include country codes in the recipient number input field when sending from the SMS web page.
    * See: Notification > SMS > Getting Started > General SMS sending, General LMS sending, MMS sending

#### Bug fixes
* [API] Fixed a bug where the userId field in the SMS delivery query API response always returned a null value.
    * Issue: The userId field in the SMS delivery query API response always returned a null value.
    * Solution: The userId field in the SMS delivery query API response now returns normal data.
* [API] Fixed a bug where the sendType field in the authentication SMS single delivery query API response returned abnormal data.
    * Issue: The sendType in the authentication SMS single delivery query API response was 0, which is the SMS type.
    * Solution: The sendType in the authentication SMS single delivery query API response now returns normal data as 2. (0:SMS, 1:MMS, 2:Auth)

### 2016.08.18
#### Feature improvements/changes
* [API] Added a recipient number input field that includes country codes.
    * See: Notification > SMS > Developer's Guide > [Short SMS sending, Long MMS sending, Authentication SMS sending] API specifications (internationalRecipientNo field added)

#### Bug fixes
* [Console] Fixed an issue where changing the SMS template usage status didn't work properly.
    * Issue: Even when changing the usage status to enabled/disabled, the change wasn't properly reflected when calling the template again.
    * Solution: Modified so that the usage status field is properly saved and displayed.

### 2016.08.04
#### Bug fixes
* [API] Fixed a bug where MMS sending failed when the SMS sender number was registered through mobile phone verification.
    * Issue: Sender numbers saved through mobile phone verification were stored in international number format (82-1x-xxxx-xxxx), which caused MMS sending to fail.
    * Solution: Changed the storage method from international number format to South Korean mobile phone number format (01x-xxxx-xxxx).
* [API] Fixed a bug where message sending failed when countryCode was entered as blank.
    * Issue: When sending empty values (null, "") for countryCode, the response showed success but the message wasn't sent properly.
    * Solution: Modified so that when an empty value is passed for countryCode, country code 82 is set by default.