## Notification > SMS > 서비스 정책 > 국제 SMS 발송 정책

## 국제 SMS 메시지 발송 안내
+ 국제 SMS 메시지 발송 시 아래의 주요 사항을 확인하여 주시기 바랍니다.

## 국가별 발신 번호 정책
+ 국제 SMS 메시지는 국가별 발신 번호 정책에 따라 발송되어 해당 정책을 따르지 않은 경우 스팸으로 처리될 수 있습니다.
+ 고객이 설정한 발신 번호는 수신 단말기에 노출을 보장할 수 없으며, 국제 SMS 메시지를 정상적으로 발송하기 위해 임의의 숫자나 문자, NHNcorp 등으로 변경되어 발송될 수 있습니다.

## 발송 정책
+ 중국, 베트남과 같이 국제 SMS 메시지 정책이 엄격한 국가의 경우, 메시지 발송 내용이 인증 번호(OTP)인 경우에 대해서만 정상적으로 발송될 수 있습니다.
+ 메시지를 정상적으로 발송하기 위해 인증 번호(OTP) 발송 내용을 예시와 같이 입력할 것을 권장합니다. (예시: Your verification code is 00000)
+ 마케팅 메시지 발송을 희망할 경우 사전에 [[고객 센터](https://www.nhncloud.com/kr/support/inquiry)]로 문의 바랍니다.
+ 국제 SMS 메시지를 정상적으로 발송하기 위해 국가별 정책에 따른 문구가 최대 12자까지 메시지에 추가되어 발송될 수 있으며, 해당 문구는 과금 글자 수에 포함됩니다.
+ 해외 통신 사업자의 경우 일반적으로 7일 이내의 발송 로그만 보관하므로 문의 시점에 따라 미수신 원인 확인이 어려울 수 있습니다.
+ 해외 통신 사업자를 통해 미수신 원인 등을 확인하는 데 시간이 다소 소요될 수 있으며, 정확한 원인을 확인하기 어려울 수 있습니다.
+ 국가별 전송 품질은 해당 국가의 네트워크 및 인프라 환경의 영향을 받으며, 국내 환경과 차이가 있을 수 있습니다.

## 과금 정책
+ 국제 SMS 메시지 발송 비용은 해외 통신 사업자로의 데이터 전송 성공 여부에 따라 과금됩니다.
+ 단말기 수신 결과는 해외 통신 사업자로의 데이터 전송 성공을 의미하며, 실제 단말기의 수신 결과와 상이할 수 있습니다. 실제 사용자가 메시지를 수신하지 못했더라도 과금 대상에 포함될 수 있습니다.
+ 국제 SMS는 Concatenated message(연결) 기능을 통해 긴 메시지를 발송할 수 있습니다. 긴 메시지로 연결되는 경우 글자 수 기준에 따른 발송 건수로 과금됩니다.
+ Concatenated message 적용 시 메시지를 연결하기 위한 헤더가 처리되는 과정에서 보낼 수 있는 글자 수가 일부 줄어듭니다.
+ 글자 수 및 Concatenated message 기준은 국제 SMS 표준 규격을 따릅니다.
+ Concatenated message 적용되어 발송한 메시지의 경우에도 통신사 및 단말기 정책에 따라 긴 메시지가 아닌 단문 메시지 여러 건의 형태로 단말기에 수신될 수 있습니다.
+ 메시지별 과금 개수는 콘솔 상세조회 및 상세조회 api의 messageCount 필드로 확인 가능합니다.

| 인코딩 | 1건 과금 | 2건 과금 | 3건 과금 | 4건 과금 | 5건 과금 |
| --- | --- | --- | --- | --- | --- |
| UCS-2<br>(유니코드) | 70자 | 134자<br>(=67*2) | 201자<br>(=67*3) | 268자<br>(=67*4) | 335자<br>(=67*5) |
| GSM-7bit | 160자 | 306자<br>(=153*2) | 459자<br>(=153*3) | 612자<br>(=153*4) | 765자<br>(=153*5) |

## 국제 SMS 물량 펌핑 현상

+ SMS 물량 펌핑 현상이란 회원가입 번호 인증 요청과 같은 입력 필드 등을 이용하여 OTP 메시지 발송에 대한 SMS 물량을 펌핑하는 것을 의미합니다. 일부 해외 이동통신사업자(MNO)에서 매출을 올리기 위해 인위적으로 메시지 발송을 유도하는 경우가 많습니다. 
+ SMS 물량 펌핑 발생 시 유사한 번호 범위(예: +1111111110, +1111111111, +1111111112, +1111111113 등)로 전송되는 메시지가 급증하며 인증번호(OTP) SMS를 보내는 경우 인증이 완료되지 않을 가능성이 높습니다. 
+ SMS 물량 펌핑 현상에 대비하시어 서비스를 제한적으로 운영하시기 바랍니다. SMS 물량 펌핑 발생 방지를 위해 메시지를 보낼 의도가 없는 국가 코드로의 발송을 막아두면 SMS 물량 펌핑 가능성이 낮아집니다. 또한 동일한 IP 대역이나 유사한 수신 번호에 대한 인증 요청의 최대 허용 횟수 또는 초당 발송 속도를 적절한 수준으로 제한하면 혹시 발생할지 모르는 피해의 규모를 낮출 수 있습니다.
+ SMS 물량 펌핑 현상 발생 시 해당 프로젝트에 대하여 사전 고지 없이 일부 혹은 전체 국제 SMS 물량을 발송 차단할 수 있습니다. 어뷰징 발생이나 이로 인해 차단이 발생하여 생기는 피해에 대해 NHN Cloud는 일체의 책임을 지지 않습니다.

## 전송 가능 국가
| 국가명(한글) | 국가명(영문) | 국가 코드 | 비고 |
|---|---|---|---|
| 아프가니스탄 | Afghanistan | 93 |  |
| 알바니아 | Albania | 355 |  |
| 알제리 | Algeria | 213 |  |
| 미국령사모아 | American Samoa | 1684 |  |
| 안도라 | Andorra | 376 |  |
| 앙골라 | Angola | 244 |  |
| 영국령 앵귈라 | Anguilla | 1264 |  |
| 앤티가바부다 | Antigua & Barbuda | 1268 |  |
| 아르헨티나 | Argentina | 54 | - 아르헨티나 수신 번호 입력 시 국가 코드와 현지 번호 사이의 숫자 9를 생략해야 함 |
| 아르메니아 | Armenia | 374 |  |
| 아루바 | Aruba | 297 |  |
| 호주 | Australia | 61 | - 마케팅 허용하나 반드시 내용 안에 수신 거부 방식이 명시되어야 함 |
| 오스트리아 | Austria | 43 |  |
| 아제르바이잔 | Azerbaijan | 994 |  |
| 바하마 | Bahamas | 1242 |  |
| 바레인 | Bahrain | 973 |  |
| 방글라데시 | Bangladesh | 880 |  |
| 바베이도스 | Barbados | 1246 |  |
| 벨라루스 | Belarus | 375 |  |
| 벨기에 | Belgium | 32 | - UCS-2 지원 불가할 수 있음 |
| 벨리즈 | Belize | 501 |  |
| 베냉 | Benin | 229 |  |
| 영국령 버뮤다 | Bermuda | 1441 |  |
| 부탄 | Bhutan | 975 |  |
| 볼리비아 | Bolivia | 591 |  |
| 보스니아 헤르체고비나 | Bosnia and Herzegovina | 387 |  |
| 보츠와나 | Botswana | 267 |  |
| 브라질 | Brazil | 55 |  |
| 영국령 버진아일랜드 | British Virgin Islands | 1340 |  |
| 브루나이 다루살람 | Brunei Darussalam | 673 |  |
| 불가리아 | Bulgaria | 359 |  |
| 부르키나파소 | Burkina Faso | 226 |  |
| 부룬디 | Burundi | 257 |  |
| 캄보디아 | Cambodia | 855 |  |
| 카메룬 | Cameroon | 237 |  |
| 카보베르데 | Cape Verde | 238 |  |
| 영국령 케이맨 제도 | Cayman Islands | 1345 |  |
| 중앙아프리카 공화국 | Central African Republic | 236 |  |
| 차드 | Chad | 235 |  |
| 칠레 | Chile | 56 |  |
| 중국 | China | 86 | - 한 수신자에게 3번을 초과하여 동일 메시지가 연속 발송될 경우 스팸 처리될 수 있음<br>- 하루 한 수신자에게 최대 15번까지만 발송할 수 있음<br>- URL 링크가 포함된 메시지 발송은 허용되지 않음<br>- 마케팅 내용은 반드시 사전 승인 절차 필요<br>- UCS-2 인코딩 사용 | 
| 쿡 제도 | Cocos Keeling Islands(Cook Islands) | 682 |  |
| 콜롬비아 | Colombia | 57 |  |
| 콩고 | Congo | 242 |  |
| 코스타리카 | Costa Rica | 506 |  |
| 코트디부아르 | Côte d'Ivoire | 225 |  |
| 크로아티아 | Croatia | 385 |  |
| 쿠바 | Cuba | 53 |  |
| 키프로스 | Cyprus | 357 |  |
| 체코 | Czechia | 420 |  |
| 콩고 민주 공화국 | Democratic Republic of the Congo | 243 |  |
| 덴마크 | Denmark | 45 |  |
| 지부티 | Djibouti | 253 |  |
| 도미니카 연방 | Dominica | 1767 |  |
| 도미니카 공화국 | Dominican Republic | 1809 |  |
| 에콰도르 | Ecuador | 593 |  |
| 이집트 | Egypt | 20 |  |
| 엘살바도르 | El Salvador | 503 |  |
| 적도 기니 | Equatorial Guinea | 240 |  |
| 에스토니아 | Estonia | 372 |  |
| 에티오피아 | Ethiopia | 251 |  |
| 페로 제도 | Faroe Islands | 298 |  |
| 피지 | Fiji | 679 |  |
| 핀란드 | Finland | 358 |  |
| 프랑스 | France | 33 |  |
| 프랑스령 기아나 | French Guiana | 594 |  |
| 프랑스령 폴리네시아 | French Polynesia | 689 |  |
| 가봉 | Gabon | 241 |  |
| 감비아 | Gambia | 220 |  |
| 조지아 | Georgia | 995 |  |
| 독일 | Germany | 49 |  |
| 가나 | Ghana | 233 |  |
| 지브롤터 | Gibraltar | 350 |  |
| 그리스 | Greece | 30 |  |
| 그린란드 | Greenland | 299 |  |
| 그레나다 | Grenada | 1473 |  |
| 프랑스령 과들루프 | Guadeloupe | 590 |  |
| 괌 | Guam | 1671 |  |
| 과테말라 | Guatemala | 502 |  |
| 기니 | Guinea | 224 |  |
| 기니비사우 | Guinea-Bissau | 245 |  |
| 가이아나 | Guyana | 592 |  |
| 아이티 | Haiti | 509 |  |
| 온두라스 | Honduras | 504 |  |
| 홍콩 | Hong-Kong | 852 |  |
| 헝가리 | Hungary | 36 |  |
| 아이슬란드 | Iceland | 354 |  |
| 인도 | India | 91 |  |
| 인도네시아 | Indonesia | 62 |  |
| 이란 | Iran | 98 |  |
| 이라크 | Iraq | 964 |  |
| 아일랜드 | Ireland | 353 |  |
| 이스라엘 | Israel | 972 | - 이스라엘 현지 법인이 있는 경우에만 발송 가능 |
| 이탈리아 | Italy | 39 |  |
| 자메이카 | Jamaica | 1876 |  |
| 일본 | Japan | 81 |  |
| 요르단 | Jordan | 962 |  |
| 케냐 | Kenya | 254 |  |
| 키리바시 | Kiribati | 686 |  |
| 쿠웨이트 | Kuwait | 965 | - 발신 번호 사전 등록 절차가 필요하며 등록하지 않을 경우 메시지 발송 실패됨 |
| 키르기스스탄 | Kyrgyzstan | 996 |  |
| 라오스 | Laos | 856 |  |
| 라트비아 | Latvia | 371 |  |
| 레바논 | Lebanon | 961 |  |
| 레소토 | Lesotho | 266 |  |
| 라이베리아 | Liberia | 231 |  |
| 리비아 | Libya | 218 |  |
| 리히텐슈타인 | Liechtenstein | 423 |  |
| 리투아니아 | Lithuania | 370 |  |
| 룩셈부르크 | Luxembourg | 352 |  |
| 마카오 | Macau | 853 |  |
| 마다가스카르 | Madagascar | 261 |  |
| 말라위 | Malawi | 265 |  |
| 말레이시아 | Malaysia | 60 |  |
| 몰디브 | Maldives | 960 |  |
| 말리 | Mali | 223 |  |
| 몰타 | Malta | 356 |  |
| 마셜 제도 공화국 | Marshall Islands | 692 |  |
| 프랑스령 마르티니크 | Martinique | 596 |  |
| 모리타니 | Mauritania | 222 |  |
| 모리셔스 | Mauritius | 230 |  |
| 마요트 | Mayotte | 269 |  |
| 멕시코 | Mexico | 52 | - 동일 수신자에게 짧은 시간 내에 동일 문자가 여러 차례 발송될 경우 스팸 처리될 수 있음 |
| 미크로네시아 | Micronesia | 691 |  |
| 몰도바 | Moldova | 373 |  |
| 모나코 | Monaco | 377 |  |
| 몽골 | Mongolia | 976 |  |
| 몬테네그로 | Montenegro | 382 |  |
| 영국령 몬트세랫 | Montserrat | 1664 |  |
| 모로코 | Morocco | 212 |  |
| 모잠비크 | Mozambique | 258 |  |
| 미얀마 | Myanmar | 95 |  |
| 나미비아 | Namibia | 264 |  |
| 나우루 | Nauru | 674 |  |
| 네덜란드령 안틸레스 | Nederlandse Antillen | 599 |  |
| 네팔 | Nepal | 977 |  |
| 네덜란드 | Netherlands | 31 |  |
| 뉴칼레도니아 | New Caledonia | 687 |  |
| 뉴질랜드 | New Zealand | 64 |  |
| 니카라과 | Nicaragua | 505 |  |
| 니제르 | Niger | 227 |  |
| 나이지리아 | Nigeria | 234 |  |
| 북마케도니아 | North Macedonia | 389 |  |
| 미국령 북마리아나 제도 | Northern Marianas | 1670 |  |
| 노르웨이 | Norway | 47 |  |
| 오만 | Oman | 968 |  |
| 파키스탄 | Pakistan | 92 |  |
| 팔라우 | Palau | 680 |  |
| 팔레스타인 | Palestine | 970 |  |
| 파나마 | Panama | 507 |  |
| 파푸아뉴기니 | Papua New Guinea | 675 |  |
| 파라과이 | Paraguay | 595 |  |
| 페루 | Peru | 51 | - 한자 지원 불가할 수 있음 |
| 필리핀 | Philippines | 63 | - URL이 포함된 메시지는 스팸 처리될 수 있음 |
| 폴란드 | Poland | 48 |  |
| 포르투갈 | Portugal | 351 |  |
| 푸에르토리코 | Puerto Rico | 1787 |  |
| 카타르 | Qatar | 974 |  |
| 프랑스령 레위니옹 | Réunion Island | 262 |  |
| 루마니아 | Romania | 40 |  |
| 르완다 | Rwanda | 250 |  |
| 상투메프린시페 | S. Tomé & Principe | 239 |  |
| 산마리노 | San Marino | 378 |  |
| 사우디아라비아 | Saudi Arabia | 966 |  |
| 세네갈 | Senegal | 221 |  |
| 세르비아 | Serbia | 381 |  |
| 세이셸 | Seychelles | 248 |  |
| 시에라리온 | Sierra Leone | 232 |  |
| 싱가포르 | Singapore | 65 | -  발신 번호 사정 등록 절차가 필요하며 등록하지 않을 경우 메시지 발송 실패됨(싱가포르 현지 법인이 있는 경우에만 등록 가능)<br>- 사전 등록 페이지: https://smsregistry.sg/web/login |
| 슬로바키아 | Slovakia | 421 |  |
| 슬로베니아 | Slovenia | 386 |  |
| 솔로몬 제도 | Solomon Islands | 677 |  |
| 소말리아 | Somalia | 252 |  |
| 남수단 공화국 | South Sudan | 211 |  |
| 스페인 | Spain | 34 |  |
| 스리랑카 | Sri Lanka | 94 |  |
| 세인트키츠네비스 | St. Kitts and Nevis | 1869 |  |
| 세인트루시아 | St. Lucia | 1758 |  |
| 프랑스령 생피에르 미클롱 | St. Pierre & Miquelon | 508 |  |
| 세인트 빈센트 그레나딘 | St. Vincent and the Grenadines | 1784 |  |
| 수단 | Sudan | 249 |  |
| 수리남 | Suriname | 597 |  |
| 스와질랜드 | Swaziland | 268 |  |
| 스웨덴 | Sweden | 46 |  |
| 스위스 | Switzerland | 41 |  |
| 시리아 | Syria | 963 |  |
| 대만 | Taiwan | 886 | - URL이 포함된 메시지는 스팸 처리될 수 있음<br>- GSM-7bit 인코딩 범위를 벗어난 문자열은 임의로 변경될 수 있음<br>- 마케팅 내용은 반드시 사전 승인 절차 필요 |
| 타지키스탄 | Tajikistan | 992 |  |
| 탄자니아 | Tanzania | 255 |  |
| 태국 | Thailand | 66 | - 메시지 내용의 줄바꿈(""\n"")을 허용하지 않음 |
| 동티모르 | Timor-Leste | 670 |  |
| 토고 | Togo | 228 |  |
| 통가 | Tonga | 676 |  |
| 트리니다드 토바고 | Trinidad & Tobago Rep. | 1868 |  |
| 튀니지 | Tunisia | 216 |  |
| 튀르키예 | Turkey | 90 |  |
| 투르크메니스탄 | Turkmenistan | 993 |  |
| 영국령 커크스케이커스제도 | Turks & Caicos Is. | 1649 |  |
| 우간다 | Uganda | 256 |  |
| 우크라이나 | Ukraine | 380 |  |
| 아랍에미리트연합 | United Arab Emirates | 971 | - 마케팅 내용은 반드시 사전 승인 절차 필요<br>- 내용 안에 수신 거부 방식이 명시되어야 함 |
| 영국 | United Kingdom | 44 |  |
| 미국/캐나다 | United States / Canada | 1 |  |
| 우루과이 | Uruguay | 598 |  |
| 우즈베키스탄 | Uzbekistan | 998 |  |
| 베네수엘라 | Venezuela | 58 |  |
| 베트남 | Vietnam | 84 | - 국제 SMS에 대한 베트남 당국의 강한 필터링으로 메시지 내용이 변경되어 단말기에 전달될 수 있음<br>- 인증 메시지 형식은 2개만 허용<br>: Your verification code is 000000<br>: Your OTP code is 000000<br>- 마케팅 내용은 반드시 사전 승인 절차 필요 |
| 미국령 버진아일랜드 | Virgin Islands(US) | 1284 |  |
