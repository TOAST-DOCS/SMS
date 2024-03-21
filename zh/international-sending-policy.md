## Notification > SMS > Service Policy > International Sending Policy

## Guide to Sending International SMS Messages
+ When sending international SMS messages, please check the following key points.
### Country-specific Sender ID Policy
+ International SMS messages are sent according to country-specific sender ID policies and may be treated as spam if you do not follow the policies.
+ The sending number set by the customer is not guaranteed to be exposed on the receiving device. In most cases, the messages are sent after changing the sending number to a random number in order to send international SMS messages normally.

### Sending Policy
+ In countries with strict international SMS message policies such as China and Vietnam, messages can be sent normally only if the content of the sending message is a verification number (OTP).
+ To send messages properly, it is recommended to enter the verification number (OTP) as follows. (Example: Your verification code is 00000)
+ If you wish to send marketing messages, please contact the [Customer Center](https://www.nhncloud.com/kr/support/inquiry) in advance.
+ To send international SMS messages normally, phrases of up to 12 characters may be added to messages according to country-specific policies, and the phrases are included in the number of charged characters.
+ International SMS is sent to international carriers and returns a DLR.
+ Each DLR provides a message status and result code, which allows you to know the delivery status of a particular SMS, but accuracy cannot be absolutely guaranteed, and the message status and result code can be NULL depending on the carrier and handset circumstances.
+ International carriers generally only keep sending logs within 7 days, so depending on the time of inquiry, it is difficult to confirm the exact cause of non-receipt and may take some time.
+ Transmission quality by country is affected by the network and infrastructure environment in that country and may differ from the domestic environment.

### Billing Policy
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


## International SMS Traffic Pumping

+ SMS traffic pumping refers to exploiting input fields, such as a request to verify a membership number, to send large volumes of SMS  for sending OTP messages. Some international mobile network operators (MNOs) artificially drive message sending to increase revenue.
+ When SMS traffic pumping occurs, there is a spike in messages sent to similar number ranges (e.g. +1111111110, +1111111111, +1111111112, +1111111113, etc.) and if you send a one-time password (OTP) SMS, there is a high chance that authentication will not complete.
+ To prevent SMS traffic pumping, if you block sending to country codes where there is no intent to send messages reduces the probability of SMS traffic pumping. Additionally, if you limit the allowed number of OTP verification requests to similar or identical numbers or limit the speed of message delivery per second, you can reduce the magnitude of the damage.
* When SMS traffic pumping occurs, NHN Cloud may block the sending of some or all volumes of international SMS without prior notice to the relevant AppKey. NHN Cloud is not responsible for any cases of abusing and any cases blocked due to abusing.


### Countries Available for Message Delivery
| Country name  | Country name | Country code | Note |
|---|---|---|---|
| Afghanistan | Afghanistan | 93 |  |
| Albania | Albania | 355 |  |
| Algeria | Algeria | 213 |  |
| American Samoa | American Samoa | 1684 |  |
| Andorra | Andorra | 376 |  |
| Angola | Angola | 244 |  |
| Anguilla | Anguilla | 1264 |  |
| Antigua & Barbuda | Antigua & Barbuda | 1268 |  |
| Argentina | Argentina | 54 | - When entering an Argentina destination number, the number 9 between the country code and the local number must be omitted |
| Armenia | Armenia | 374 |  |
| Aruba | Aruba | 297 |  |
| Australia | Australia | 61 | - Allow marketing, but make sure the content makes it clear how to unsubscribe |
| Austria | Austria | 43 |  |
| Azerbaijan | Azerbaijan | 994 |  |
| Bahamas | Bahamas | 1242 |  |
| Bahrain | Bahrain | 973 |  |
| Bangladesh | Bangladesh | 880 |  |
| Barbados | Barbados | 1246 |  |
| Belarus | Belarus | 375 |  |
| Belgium | Belgium | 32 | - UCS-2 support may not be available |
| Belize | Belize | 501 |  |
| Benin | Benin | 229 |  |
| Bermuda | Bermuda | 1441 |  |
| Bhutan | Bhutan | 975 |  |
| Bolivia | Bolivia | 591 |  |
| Bosnia and Herzegovina | Bosnia and Herzegovina | 387 |  |
| Botswana | Botswana | 267 |  |
| Brazil | Brazil | 55 |  |
| British Virgin Islands | British Virgin Islands | 1340 |  |
| Brunei Darussalam | Brunei Darussalam | 673 |  |
| Bulgaria | Bulgaria | 359 |  |
| Burkina Faso | Burkina Faso | 226 |  |
| Burundi | Burundi | 257 |  |
| Cambodia | Cambodia | 855 |  |
| Cameroon | Cameroon | 237 |  |
| Cape Verde | Cape Verde | 238 |  |
| Cayman Islands | Cayman Islands | 1345 |  |
| Central African Republic | Central African Republic | 236 |  |
| Chad | Chad | 235 |  |
| Chile | Chile | 56 |  |
| China | China | 86 | - Sending the same message more than three times in a row to a single recipient may result in spamming.<br>- You can only send a maximum of 15 times to one recipient per day<br>- Sending messages with URL links is not allowed<br>- Marketing content must be pre-approved<br>- UCS-2 encoding used  | 
| Cocos Keeling Islands (Cook Islands) | Cocos Keeling Islands (Cook Islands) | 682 |  |
| Colombia | Colombia | 57 |  |
| Congo | Congo | 242 |  |
| Costa Rica | Costa Rica | 506 |  |
| Côte d'Ivoire | Côte d'Ivoire | 225 |  |
| Croatia | Croatia | 385 |  |
| Cuba | Cuba | 53 |  |
| Cyprus | Cyprus | 357 |  |
| Czechia | Czechia | 420 |  |
| Democratic Republic of the Congo | Democratic Republic of the Congo | 243 |  |
| Denmark | Denmark | 45 |  |
| Djibouti | Djibouti | 253 |  |
| Dominica | Dominica | 1767 |  |
| Dominican Republic | Dominican Republic | 1809 |  |
| Ecuador | Ecuador | 593 |  |
| Egypt | Egypt | 20 |  |
| El Salvador | El Salvador | 503 |  |
| Equatorial Guinea | Equatorial Guinea | 240 |  |
| Eritrea | Eritrea | 291 |  |
| Estonia | Estonia | 372 |  |
| Ethiopia | Ethiopia | 251 |  |
| Falkland Islands | Falkland Islands | 500 |  |
| Faroe Islands | Faroe Islands | 298 |  |
| Fiji | Fiji | 679 |  |
| Finland | Finland | 358 |  |
| France | France | 33 |  |
| French Guiana | French Guiana | 594 |  |
| French Polynesia | French Polynesia | 689 |  |
| Gabon | Gabon | 241 |  |
| Gambia | Gambia | 220 |  |
| Georgia | Georgia | 995 |  |
| Germany | Germany | 49 |  |
| Ghana | Ghana | 233 |  |
| Gibraltar | Gibraltar | 350 |  |
| Greece | Greece | 30 |  |
| Greenland | Greenland | 299 |  |
| Grenada | Grenada | 1473 |  |
| Guadeloupe | Guadeloupe | 590 |  |
| Guam | Guam | 1671 |  |
| Guatemala | Guatemala | 502 |  |
| Guinea | Guinea | 224 |  |
| Guinea-Bissau | Guinea-Bissau | 245 |  |
| Guyana | Guyana | 592 |  |
| Haiti | Haiti | 509 |  |
| Honduras | Honduras | 504 |  |
| Hong-Kong | Hong-Kong | 852 |  |
| Hungary | Hungary | 36 |  |
| Iceland | Iceland | 354 |  |
| India | India | 91 |  |
| Indonesia | Indonesia | 62 |  |
| Iran | Iran | 98 |  |
| Iraq | Iraq | 964 |  |
| Ireland | Ireland | 353 |  |
| Israel | Israel | 972 | - Can only be sent if you have a local entity in Israel |
| Italy | Italy | 39 |  |
| Jamaica | Jamaica | 1876 |  |
| Japan | Japan | 81 |  |
| Jordan | Jordan | 962 |  |
| Kenya | Kenya | 254 |  |
| Kiribati | Kiribati | 686 |  |
| Kuwait | Kuwait | 965 | - Requires pre-registration of sending number and fails to send message if not registeredFunctionality |
| - Requires pre-registration of the sending number and fails to send the message if not registered | Kyrgyzstan | 996 |  |
| Laos | Laos | 856 |  |
| Latvia | Latvia | 371 |  |
| Lebanon | Lebanon | 961 |  |
| Lesotho | Lesotho | 266 |  |
| Liberia | Liberia | 231 |  |
| Libya | Libya | 218 |  |
| Liechtenstein | Liechtenstein | 423 |  |
| Lithuania | Lithuania | 370 |  |
| Luxembourg | Luxembourg | 352 |  |
| Macau | Macau | 853 |  |
| Madagascar | Madagascar | 261 |  |
| Malawi | Malawi | 265 |  |
| Malaysia | Malaysia | 60 |  |
| Maldives | Maldives | 960 |  |
| Mali | Mali | 223 |  |
| Malta | Malta | 356 |  |
| Marshall Islands | Marshall Islands | 692 |  |
| Martinique | Martinique | 596 |  |
| Mauritania | Mauritania | 222 |  |
| Mauritius | Mauritius | 230 |  |
| Mayotte | Mayotte | 269 |  |
| Mexico | Mexico | 52 | - Multiple texts sent to the same recipient within a short period of time may result in spamming |
| Micronesia | Micronesia | 691 |  |
| Moldova | Moldova | 373 |  |
| Monaco | Monaco | 377 |  |
| Mongolia | Mongolia | 976 |  |
| Montenegro | Montenegro | 382 |  |
| Montserrat | Montserrat | 1664 |  |
| Morocco | Morocco | 212 |  |
| Morocco | Mozambique | 258 |  |
| Myanmar | Myanmar | 95 |  |
| Namibia | Namibia | 264 |  |
| Nauru | Nauru | 674 |  |
| Nederlandse Antillen | Nederlandse Antillen | 599 |  |
| Nepal | Nepal | 977 |  |
| Netherlands | Netherlands | 31 |  |
| New Caledonia | New Caledonia | 687 |  |
| New Zealand | New Zealand | 64 |  |
| Nicaragua | Nicaragua | 505 |  |
| Niger | Niger | 227 |  |
| Nigeria | Nigeria | 234 |  |
| North Macedonia | North Macedonia | 389 |  |
| Northern Marianas | Northern Marianas | 1670 |  |
| Norway | Norway | 47 |  |
| Oman | Oman | 968 |  |
| Pakistan | Pakistan | 92 |  |
| Palau | Palau | 680 |  |
| Palestine | Palestine | 970 |  |
| Panama | Panama | 507 |  |
| Papua New Guinea | Papua New Guinea | 675 |  |
| Papua New Guinea | Paraguay | 595 |  |
| Peru | Peru | 51 | - Chinese characters support may not be available |
| Philippines | Philippines | 63 | - Messages with URLs can be spammed |
| Poland | Poland | 48 |  |
| Portugal | Portugal | 351 |  |
| Puerto Rico | Puerto Rico | 1787 |  |
| Qatar | Qatar | 974 |  |
| Réunion Island | Réunion Island | 262 |  |
| Romania | Romania | 40 |  |
| Russia / Kazakhstan | Russia / Kazakhstan | 7 |  |
| Rwanda | Rwanda | 250 |  |
| S. Tomé & Principe | S. Tomé & Principe | 239 |  |
| Saint Helena | Saint Helena | 290 |  |
| Samoa | Samoa | 685 |  |
| San Marino | San Marino | 378 |  |
| Saudi Arabia | Saudi Arabia | 966 |  |
| Senegal | Senegal | 221 |  |
| Serbia | Serbia | 381 |  |
| Seychelles | Seychelles | 248 |  |
| Sierra Leone | Sierra Leone | 232 |  |
| Singapore | Singapore | 65 | - Requires caller ID registration process in advance<br>- Pre-registration page: https://smsregistry.sg/web/login |
| Slovakia | Slovakia | 421 |  |
| Slovenia | Slovenia | 386 |  |
| Solomon Islands | Solomon Islands | 677 |  |
| Somalia | Somalia | 252 |  |
| South Africa | South Africa | 27 |  |
| South Sudan | South Sudan | 211 |  |
| Spain | Spain | 34 |  |
| Sri Lanka | Sri Lanka | 94 |  |
| St. Kitts and Nevis | St. Kitts and Nevis | 1869 |  |
| St. Lucia | St. Lucia | 1758 |  |
| St. Pierre & Miquelon | St. Pierre & Miquelon | 508 |  |
| St. Vincent and the Grenadines | St. Vincent and the Grenadines | 1784 |  |
| Sudan | Sudan | 249 |  |
| Suriname | Suriname | 597 |  |
| Swaziland | Swaziland | 268 |  |
| Sweden | Sweden | 46 |  |
| Switzerland | Switzerland | 41 |  |
| Syria | Syria | 963 |  |
| Taiwan | Taiwan | 886 | - Messages with URLs can be spammed<br>- Strings outside the GSM-7bit encoding range may be randomly altered<br>- Marketing content must be pre-approved |
| Tajikistan | Tajikistan | 992 |  |
| Tanzania | Tanzania | 255 |  |
| Thailand | Thailand | 66 | - Not allow newlines (""\n"") in message content |
| Timor-Leste | Timor-Leste | 670 |  |
| Togo | Togo | 228 |  |
| Tonga | Tonga | 676 |  |
| Trinidad & Tobago Rep. | Trinidad & Tobago Rep. | 1868 |  |
| Tunisia | Tunisia | 216 |  |
| Turkey | Turkey | 90 |  |
| Turkmenistan | Turkmenistan | 993 |  |
| Turks & Caicos Is. | Turks & Caicos Is. | 1649 |  |
| Uganda | Uganda | 256 |  |
| Ukraine | Ukraine | 380 |  |
| United Arab Emirates | United Arab Emirates | 971 | - Marketing content must be pre-approved<br>- Make sure the content makes it clear how to unsubscribe |
| United Kingdom | United Kingdom | 44 |  |
| United States / Canada | United States / Canada | 1 |  |
| Uruguay | Uruguay | 598 |  |
| Uzbekistan | Uzbekistan | 998 |  |
| Vanuatu | Vanuatu | 678 |  |
| Venezuela | Venezuela | 58 |  |
| Vietnam | Vietnam | 84 | - Vietnamese authorities' strong filtering of international SMS may alter the content of the message before reaching to the device<br>-Only two verification message types allowed<br>: Your verification code is 000000<br>: Your OTP code is 000000<br>- Marketing content must be pre-approved |
| Virgin Islands (US) | Virgin Islands (US) | 1284 |  |
| Wallis and Futuna | Wallis and Futuna | 681 |  |
| Yemen | Yemen | 967 |  |
| Zambia | Zambia | 260 |  |
| Zimbabwe | Zimbabwe | 263 |  |
