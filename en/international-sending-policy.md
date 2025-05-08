## Notification > SMS > Service Policy > International Sending Policy

## Guide to Sending International SMS Messages
+ When sending international SMS messages, please check the following key points.

### Country-specific Sender ID Policy
+ International SMS messages are sent according to country-specific sender ID policies and may be treated as spam if you do not follow the policies.
+ The sending number set by the customer is not guaranteed to be exposed on the receiving device. In most cases, the messages are sent after changing the sending number to a random number in order to send international SMS messages normally.

## Sending Policy
* For detailed country-specific policies, see the [Detailed guide to SMS sending by country](https://nhnnotification.imweb.me/Technology/?q=YToxOntzOjEyOiJrZXl3b3JkX3R5cGUiO3M6MzoiYWxsIjt9&bmode=view&idx=17226410&t=board).
+ In countries with strict international SMS message policies such as Vietnam, messages can be sent normally only if the content of the sending message is a verification number (OTP).
+ To send messages properly, it is recommended to enter the verification number (OTP) as follows. (Example: Your verification code is 00000)
+ If you wish to send marketing messages, please contact the [Customer Center](https://www.nhncloud.com/kr/support/inquiry) in advance.
+ To send international SMS messages normally, phrases of up to 12 characters may be added to messages according to country-specific policies, and the phrases are included in the number of charged characters.
+ International SMS is sent to international carriers and returns a DLR.
+ Each DLR provides a message status and result code, which allows you to know the delivery status of a particular SMS, but accuracy cannot be absolutely guaranteed, and the message status and result code can be NULL depending on the carrier and handset circumstances.
+ International carriers generally only keep sending logs within 7 days, so depending on the time of inquiry, it is difficult to confirm the exact cause of non-receipt and may take some time.
+ Transmission quality by country is affected by the network and infrastructure environment in that country and may differ from the domestic environment.

## Billing Policy
+ International SMS messages are charged based on successful data transmission from overseas carriers.
+ International SMS billing policies are independent of DLR message status and DLR result codes.
+ The device reception result means the success of data transmission to the overseas communication service provider, and may differ from the actual device reception result. Even if the actual user did not receive the message, it may still count towards billing.
+ International SMS can send long messages through the Concatenated message feature. In the case of a long message, you will be charged for the number of messages sent based on character count.
+ With Concatenated message applied, the number of characters that can be sent is reduced while processing headers.
+ The number of characters and concatenated message standards follow the international SMS standards.
+ Even if the message is sent with the concatenated message applied, it may be received by the device in the form of several short messages rather than a long message, depending on the mobile carrier and device policy.
+ The number of charges per message can be checked with the messageCount field of the detailed inquiry from the console and the detailed inquiry api.

| encoding | 1 charge | 2 charges | 3 charges | 4 charges | 5 charges |
| --- | --- | --- | --- | --- | --- |
| UCS-2<br>(Unicode) | 70 characters | 134 characters<br>(=67*2) | 201 characters<br>(=67*3) | 268 characters<br>(=67*4) | 335 characters<br>(=67*5) |
| GSM-7bit | 160 characters | 306 characters<br>(=153*2) | 459 characters<br>(=153*3) | 612 characters<br>(=153*4) | 765 characters<br>(=153*5) |

## International SMS Traffic pumping
+ Some international mobile network operators (MNOs) may artificially trigger the sending of messages to increase revenues.
+ A bot or abuser makes a bulk request to send a message on a page, such as a request for a signup verification number.
+ Most bots or abusers don't actually authenticate after requesting authentication. When abusing occurs, requests for authentication number increases, but the percentage that authenticate and convert decreases.
+ Precautions
    + In the event of international SMS volume pumping, we may block some or all international SMS volumes without prior notice to the project. 
    + NHN Cloud is not responsible for any damage caused by abusing or blocking, so please be careful about leaking confidential information and abusing.
+ Recommended actions
    + Restrict sending to countries you don't intend to send messages to via the **manage countries to allow**.
    + Limit the number of sendings you can send per month to an appropriate level.
    + The **Set blocking countries by conversion rate** setting allows you to block sending to countries with low conversion rates.
    + Limit the maximum number of authentication requests allowed for the same IP band or similar incoming numbers, or the rate at which they are sent per second, as appropriate.
    + Prevent messages that are being sent to similar number ranges (e.g., +1111111110, +1111111111, +1111111112, +1111111113, etc.) from being ingested consecutively.

## Sending Blocking based on international SMS conversion rate
+ In general, you consider a conversion to have occurred when a recipient takes an action after receiving your message, such as clicking a URL or entering a verification number.
+ The Block by international SMS conversion rate feature allows you to check whether recipients convert after receiving a message, increasing the reliability of your international SMS sending and strengthening your protection against abusive behavior.

### Collection of international SMS conversions
+ Set whether or not to collect conversion rates in the request to collect conversion rates field when making the international SMS sending API request.
    + For more information, see the [API v3.0 guide](./api-guide/#sms_1).
+ When you determine that a shipment that you set up to collect conversion rates has converted, you can notify NHN Cloud of the conversion through a conversion API call.
    + You should only call the conversion API for messages that have been sent, otherwise the call to the conversion API will fail.
    + For more information, see [API v3.0 Guide](./api-guide/#sms_6).
+ Calculates the percentage of sent messaged that are converted from the messages set for collection of conversion rates, you can block messages from being sent to certain countries.
    + If you have more than 50 messages sent to a country with **Conversion Rate Based Sending Blocking and Notifications** enabled in a 24-hour period, and your conversion rate is 50% or less, sending to that country is automatically blocked.

### Set blocking countries by conversion rate
+ In **Delivery Settings > International SMS** from the console, you must change the Block by conversion rate setting to Enable.
+ After enabling the feature, you must set which countries to block based on conversion rate.
    + Conversion rate is calculated and blocking applies for each country based on the conversion rate-based blocking rule settings. 
+ Countries that are blocked based on conversion rate-based blocking rule settings are visible in the console and can be unblocked by selecting Block.
     + If you unblock a conversion rate between the time range of the blocking rule you set, the conversion rate is calculated from the time you unblocked it to avoid reblocking.
+ For more information, see the [console user guide](./console-guide/#sms_8).

## Available countries
| Country name | Country code |
| ------- | ----- |
| United States / Canada | 1 |
| Russia / Kazakhstan | 7 |
| Egypt | 20 |
| South Africa | 27 |
| Greece | 30 |
| Netherlands | 31 |
| Belgium | 32 |
| France | 33 |
| Spain | 34 |
| Hungary | 36 |
| Italy | 39 |
| Romania | 40 |
| Switzerland | 41 |
| Austria | 43 |
| United Kingdom | 44 |
| Denmark | 45 |
| Sweden | 46 |
| Norway | 47 |
| Poland | 48 |
| Germany | 49 |
| Peru | 51 |
| Mexico | 52 |
| Cuba | 53 |
| Argentina | 54 |
| Brazil | 55 |
| Chile | 56 |
| Colombia | 57 |
| Venezuela | 58 |
| Malaysia | 60 |
| Australia | 61 |
| Indonesia | 62 |
| Philippines | 63 |
| New Zealand | 64 |
| Singapore | 65 |
| Thailand | 66 |
| Japan | 81 |
| Vietnam | 84 |
| China | 86 |
| Turkey | 90 |
| India | 91 |
| Pakistan | 92 |
| Afghanistan | 93 |
| Sri Lanka | 94 |
| Myanmar | 95 |
| Iran | 98 |
| South Sudan | 211 |
| Morocco | 212 |
| Algeria | 213 |
| Tunisia | 216 |
| Libya | 218 |
| Gambia | 220 |
| Senegal | 221 |
| Mauritania | 222 |
| Mali | 223 |
| Guinea | 224 |
| Côte d'Ivoire | 225 |
| Burkina Faso | 226 |
| Niger | 227 |
| Togo | 228 |
| Benin | 229 |
| Mauritius | 230 |
| Liberia | 231 |
| Sierra Leone | 232 |
| Ghana | 233 |
| Nigeria | 234 |
| Chad | 235 |
| Central African Republic | 236 |
| Cameroon | 237 |
| Cape Verde | 238 |
| S. Tomé & Principe | 239 |
| Equatorial Guinea | 240 |
| Gabon | 241 |
| Congo | 242 |
| Democratic Republic of the Congo | 243 |
| Angola | 244 |
| Guinea-Bissau | 245 |
| Seychelles | 248 |
| Sudan | 249 |
| Rwanda | 250 |
| Ethiopia | 251 |
| Somalia | 252 |
| Djibouti | 253 |
| Kenya | 254 |
| Tanzania | 255 |
| Uganda | 256 |
| Burundi | 257 |
| Mozambique | 258 |
| Zambia | 260 |
| Madagascar | 261 |
| Réunion Island | 262 |
| Zimbabwe | 263 |
| Namibia | 264 |
| Malawi | 265 |
| Lesotho | 266 |
| Botswana | 267 |
| Swaziland | 268 |
| Mayotte / Comoros | 269 |
| Saint Helena | 290 |
| Eritrea | 291 |
| Aruba | 297 |
| Faroe Islands | 298 |
| Greenland | 299 |
| Gibraltar | 350 |
| Portugal | 351 |
| Luxembourg | 352 |
| Ireland | 353 |
| Iceland | 354 |
| Albania | 355 |
| Malta | 356 |
| Cyprus | 357 |
| Finland | 358 |
| Bulgaria | 359 |
| Lithuania | 370 |
| Latvia | 371 |
| Estonia | 372 |
| Moldova | 373 |
| Armenia | 374 |
| Belarus | 375 |
| Andorra | 376 |
| Monaco | 377 |
| San Marino | 378 |
| Ukraine | 380 |
| Serbia | 381 |
| Montenegro | 382 |
| Republic of Kosovo | 383 |
| Croatia | 385 |
| Slovenia | 386 |
| Bosnia and Herzegovina | 387 |
| North Macedonia | 389 |
| Czechia | 420 |
| Slovakia | 421 |
| Liechtenstein | 423 |
| Falkland Islands | 500 |
| Belize | 501 |
| Guatemala | 502 |
| El Salvador | 503 |
| Honduras | 504 |
| Nicaragua | 505 |
| Costa Rica | 506 |
| Panama | 507 |
| St. Pierre & Miquelon | 508 |
| Haiti | 509 |
| Guadeloupe | 590 |
| Bolivia | 591 |
| Guyana | 592 |
| Ecuador | 593 |
| French Guiana | 594 |
| Paraguay | 595 |
| Martinique | 596 |
| Suriname | 597 |
| Uruguay | 598 |
| Nederlandse Antillen / Curaçao | 599 |
| Timor-Leste | 670 |
| Brunei Darussalam | 673 |
| Nauru | 674 |
| Papua New Guinea | 675 |
| Tonga | 676 |
| Solomon Islands | 677 |
| Vanuatu | 678 |
| Fiji | 679 |
| Palau | 680 |
| Wallis and Futuna | 681 |
| Cocos Keeling Islands (Cook Islands) | 682 |
| Samoa | 685 |
| Kiribati | 686 |
| New Caledonia | 687 |
| French Polynesia | 689 |
| Micronesia | 691 |
| Marshall Islands | 692 |
| Hong-Kong | 852 |
| Macau | 853 |
| Cambodia | 855 |
| Laos | 856 |
| Bangladesh | 880 |
| Taiwan | 886 |
| Maldives | 960 |
| Lebanon | 961 |
| Jordan | 962 |
| Syria | 963 |
| Iraq | 964 |
| Kuwait | 965 |
| Saudi Arabia | 966 |
| Yemen | 967 |
| Oman | 968 |
| Palestine | 970 |
| United Arab Emirates | 971 |
| Israel | 972 |
| Bahrain | 973 |
| Qatar | 974 |
| Bhutan | 975 |
| Mongolia | 976 |
| Nepal | 977 |
| Tajikistan | 992 |
| Turkmenistan | 993 |
| Azerbaijan | 994 |
| Georgia | 995 |
| Kyrgyzstan | 996 |
| Uzbekistan | 998 |
| Bahamas | 1242 |
| Barbados | 1246 |
| Anguilla | 1264 |
| Antigua & Barbuda | 1268 |
| Virgin Islands (US) | 1284 |
| British Virgin Islands | 1340 |
| Cayman Islands | 1345 |
| Bermuda | 1441 |
| Grenada | 1473 |
| Turks & Caicos Is. | 1649 |
| Montserrat | 1664 |
| Northern Marianas | 1670 |
| Guam | 1671 |
| American Samoa | 1684 |
| St. Lucia | 1758 |
| Dominica | 1767 |
| St. Vincent and the Grenadines | 1784 |
| Puerto Rico | 1787, 1939 |
| Dominican Republic | 1809, 1829, 1849 |
| Trinidad & Tobago Rep. | 1868 |
| St. Kitts and Nevis | 1869 |
| Jamaica | 1876 |
