## Notification > SMS > 서비스 정책 > 국제 SMS 발송 정책

## 국제 SMS 메시지 발송 안내
+ 국제 SMS 메시지 발송 시 아래의 주요 사항을 확인하여 주시기 바랍니다.

## 국가별 발신 번호 정책
+ 국제 SMS 메시지는 국가별 발신 번호 정책에 따라 발송되어 해당 정책을 따르지 않은 경우 스팸으로 처리될 수 있습니다.
+ 고객이 설정한 발신 번호는 수신 단말기에 노출을 보장할 수 없으며, 국제 SMS 메시지를 정상적으로 발송하기 위해 임의의 숫자나 문자, NHNcorp 등으로 변경되어 발송될 수 있습니다.

## 발송 정책
+ 국가별 상세 정책은 [[국가별 SMS 발송 상세 가이드](https://nhnnotification.imweb.me/Technology/?q=YToxOntzOjEyOiJrZXl3b3JkX3R5cGUiO3M6MzoiYWxsIjt9&bmode=view&idx=17226410&t=board)]를 참고하십시오.
+ 베트남과 같이 국제 SMS 메시지 정책이 엄격한 국가의 경우, 메시지 발송 내용이 인증 번호(OTP)인 경우에 대해서만 정상적으로 발송될 수 있습니다.
+ 메시지를 정상적으로 발송하기 위해 인증 번호(OTP) 발송 내용을 예시와 같이 입력할 것을 권장합니다. (예시: Your verification code is 00000)
+ 마케팅 메시지 발송을 희망할 경우 사전에 [[고객 센터](https://www.nhncloud.com/kr/support/inquiry)]로 문의 바랍니다.
+ 국제 SMS 메시지를 정상적으로 발송하기 위해 국가별 정책에 따른 문구가 최대 12자까지 메시지에 추가되어 발송될 수 있으며, 해당 문구는 과금 글자 수에 포함됩니다.
+ 국제 SMS는 현지 통신사로 전송되고 DLR을 반환합니다.
+ 각 DLR은 메시지 상태와 결과 코드를 제공하며, 이를 통해 특정 SMS의 전송 상태를 알 수 있으나 정확성을 절대적으로 보장할 수는 없으며 통신사 및 단말기 상황에 따라 메시지 상태 및 결과 코드가 값이 NULL 일 수 있습니다.
+ 국제 SMS의 수신 일시(단말기 도착 시간)는 해외 통신 사업자의 결과 회신 일시를 의미하며, 실제 단말기 수신 일시와는 차이가 있을 수 있습니다.
+ 해외 통신 사업자의 경우 일반적으로 7일 이내의 발송 로그만 보관하므로 문의 시점에 따라 정확한 미수신 원인 등의 확인이 어려우며 시간이 다소 소요될 수 있습니다.
+ 국가별 전송 품질은 해당 국가의 네트워크 및 인프라 환경의 영향을 받으며, 국내 환경과 차이가 있을 수 있습니다.

## 과금 정책
+ 국제 SMS 메시지 발송 비용은 해외 통신 사업자로의 데이터 전송 성공 여부에 따라 과금됩니다.
+ 국제 SMS 과금 정책은 DLR 메시지 상태 및 DLR 결과 코드와는 무관합니다.
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
+ 일부 해외 이동통신사업자(MNO)에서 매출을 올리기 위해 인위적으로 메시지 발송을 유도하는 경우가 있습니다.
+ 회원가입 인증 번호 요청 등의 페이지에서 봇 또는 어뷰저가 메시지 발송을 대량 요청합니다.
+ 대부분의 봇 또는 어뷰저는 인증 요청 후 실제 인증은 하지 않습니다. 어뷰징이 발생하면 인증번호 요청은 늘지만, 인증을 수행하고 전환되는 비율은 줄어듭니다.
+ 유의사항
    + 국제 SMS 물량 펌핑 현상 발생 시 해당 프로젝트에 대하여 사전 고지 없이 일부 혹은 전체 국제 SMS 물량을 발송 차단할 수 있습니다. 
    + 어뷰징이나 이로 인한 차단으로 피해가 발생하더라도 NHN Cloud는 일체의 책임을 지지 않으므로, 기밀정보 유출 및 어뷰징에 대해 주의하시기 바랍니다.
+ 권장 조치 사항
    + **발송 허용 국가 관리**를 통해 메시지를 보낼 의도가 없는 국가로의 발송을 제한합니다.
    + 월별로 발송 가능한 건수를 적절한 수준으로 제한합니다.
    + **전환율에 의한 차단 국가 설정**을 통해 전환율이 낮은 국가로의 발송이 차단되도록 설정합니다.
    + 동일한 IP 대역이나 유사한 수신 번호에 대한 인증 요청의 최대 허용 횟수 또는 초당 발송 속도를 적절한 수준으로 제한합니다.
    + 유사한 번호 범위(예: +1111111110, +1111111111, +1111111112, +1111111113 등)로 전송되는 메시지가 연속적으로 인입되지 않도록 합니다.

## 국제 SMS 전환율 기반 발송 차단
+ 일반적으로 수신자가 메시지 수신 후 URL 클릭이나 인증번호 입력과 같은 동작을 수행했을 때, 전환이 발생한 것으로 간주합니다.
+ 국제 SMS 전환율에 의한 차단 설정 기능을 통해 수신자의 메시지 수신 후 전환 여부를 체크하여 국제 SMS 발송의 신뢰성을 높이고 어뷰징에 대한 차단을 강화할 수 있습니다.

### 국제 SMS 전환 여부 수집 설정
+ 국제 SMS 발송 API 요청 시 전환율 수집 요청 필드로 전환율 수집 대상 여부를 설정합니다.
    + 자세한 사항은 [[API v3.0 가이드](./api-guide/#sms_1)]를 참고하세요.
+ 전환율 수집 대상으로 설정한 발송 건이 전환된 것으로 판단되면 전환 API 호출을 통해 NHN Cloud에 전환 여부를 알릴 수 있습니다.
    + 발송이 완료된 건에 대해서만 전환 API를 호출해야 하며, 발송이 완료되지 않은 건에 대해서는 전환 API 호출 시 실패 처리됩니다.
    + 자세한 사항은 [[API v3.0 가이드](./api-guide/#sms_6)]를 참고하세요.
+ 전환율 수집 설정한 발송 건 중 전환된 발송 건의 비율을 계산합니다. 전환율에 따라 특정 국가로의 메시지 발송을 차단할 수 있습니다.
    + **전환율 기반 발송 차단 및 알림**을 **사용**으로 설정한 국가로 발송한 메시지가 24시간 동안 50건 이상이고, 전환율이 50% 이하인 경우 해당 국가로의 발송이 자동 차단됩니다.

### 전환율 기반 발송 차단 및 알림 기능 사용 설정
+ 콘솔 **발송 설정 > 국제 SMS** 메뉴에서 전환율 기반 발송 차단 및 알림 설정을 **사용**으로 변경해야 합니다.
+ 기능 사용 설정 후 전환율에 따라 차단할 국가를 설정해야 합니다.
    + 전환율 기반 차단 규칙 설정에 따른 전환율 계산 및 차단이 국가별로 적용됩니다. 
+ 전환율 기반 차단 규칙 설정에 따라 차단된 국가는 콘솔에서 확인 가능하며, 차단 버튼 선택 시 해제할 수 있습니다.
    + 설정한 차단 규칙의 시간 범위 사이에 전환율 차단을 해제할 경우 재차단이 발생하지 않도록 차단을 해제한 시점부터 전환율이 계산됩니다.
+ 자세한 사항은 [[콘솔 사용 가이드](./console-guide/#sms_8)]를 참고하세요.

## 전송 가능 국가
| 국가명 | 국가 코드 |
| ------- | ----- |
| 미국 / 캐나다 | 1 |
| 러시아 / 카자흐스탄 | 7 |
| 이집트 | 20 |
| 남아프리카 공화국 | 27 |
| 그리스 | 30 |
| 네덜란드 | 31 |
| 벨기에 | 32 |
| 프랑스 | 33 |
| 스페인 | 34 |
| 헝가리 | 36 |
| 이탈리아 | 39 |
| 루마니아 | 40 |
| 스위스 | 41 |
| 오스트리아 | 43 |
| 영국 | 44 |
| 덴마크 | 45 |
| 스웨덴 | 46 |
| 노르웨이 | 47 |
| 폴란드 | 48 |
| 독일 | 49 |
| 페루 | 51 |
| 멕시코 | 52 |
| 쿠바 | 53 |
| 아르헨티나 | 54 |
| 브라질 | 55 |
| 칠레 | 56 |
| 콜롬비아 | 57 |
| 베네수엘라 | 58 |
| 말레이시아 | 60 |
| 호주 | 61 |
| 인도네시아 | 62 |
| 필리핀 | 63 |
| 뉴질랜드 | 64 |
| 싱가포르 | 65 |
| 태국 | 66 |
| 일본 | 81 |
| 베트남 | 84 |
| 중국 | 86 |
| 튀르키예 | 90 |
| 인도 | 91 |
| 파키스탄 | 92 |
| 아프가니스탄 | 93 |
| 스리랑카 | 94 |
| 미얀마 | 95 |
| 이란 | 98 |
| 남수단 공화국 | 211 |
| 모로코 | 212 |
| 알제리 | 213 |
| 튀니지 | 216 |
| 리비아 | 218 |
| 감비아 | 220 |
| 세네갈 | 221 |
| 모리타니 | 222 |
| 말리 | 223 |
| 기니 | 224 |
| 코트디부아르 | 225 |
| 부르키나파소 | 226 |
| 니제르 | 227 |
| 토고 | 228 |
| 베냉 | 229 |
| 모리셔스 | 230 |
| 라이베리아 | 231 |
| 시에라리온 | 232 |
| 가나 | 233 |
| 나이지리아 | 234 |
| 차드 | 235 |
| 중앙아프리카 공화국 | 236 |
| 카메룬 | 237 |
| 카보베르데 | 238 |
| 상투메프린시페 | 239 |
| 적도 기니 | 240 |
| 가봉 | 241 |
| 콩고 | 242 |
| 콩고 민주 공화국 | 243 |
| 앙골라 | 244 |
| 기니비사우 | 245 |
| 세이셸 | 248 |
| 수단 | 249 |
| 르완다 | 250 |
| 에티오피아 | 251 |
| 소말리아 | 252 |
| 지부티 | 253 |
| 케냐 | 254 |
| 탄자니아 | 255 |
| 우간다 | 256 |
| 부룬디 | 257 |
| 모잠비크 | 258 |
| 잠비아 | 260 |
| 마다가스카르 | 261 |
| 프랑스령 레위니옹 | 262 |
| 짐바브웨 | 263 |
| 나미비아 | 264 |
| 말라위 | 265 |
| 레소토 | 266 |
| 보츠와나 | 267 |
| 스와질랜드 | 268 |
| 마요트 / 코모로 | 269 |
| 영국령 세인트헬레나 | 290 |
| 에리트레아 | 291 |
| 아루바 | 297 |
| 페로 제도 | 298 |
| 그린란드 | 299 |
| 지브롤터 | 350 |
| 포르투갈 | 351 |
| 룩셈부르크 | 352 |
| 아일랜드 | 353 |
| 아이슬란드 | 354 |
| 알바니아 | 355 |
| 몰타 | 356 |
| 키프로스 | 357 |
| 핀란드 | 358 |
| 불가리아 | 359 |
| 리투아니아 | 370 |
| 라트비아 | 371 |
| 에스토니아 | 372 |
| 몰도바 | 373 |
| 아르메니아 | 374 |
| 벨라루스 | 375 |
| 안도라 | 376 |
| 모나코 | 377 |
| 산마리노 | 378 |
| 우크라이나 | 380 |
| 세르비아 | 381 |
| 몬테네그로 | 382 |
| 코소보 공화국 | 383 |
| 크로아티아 | 385 |
| 슬로베니아 | 386 |
| 보스니아 헤르체고비나 | 387 |
| 북마케도니아 | 389 |
| 체코 | 420 |
| 슬로바키아 | 421 |
| 리히텐슈타인 | 423 |
| 영국령 포클랜드 제도 | 500 |
| 벨리즈 | 501 |
| 과테말라 | 502 |
| 엘살바도르 | 503 |
| 온두라스 | 504 |
| 니카라과 | 505 |
| 코스타리카 | 506 |
| 파나마 | 507 |
| 프랑스령 생피에르 미클롱 | 508 |
| 아이티 | 509 |
| 프랑스령 과들루프 | 590 |
| 볼리비아 | 591 |
| 가이아나 | 592 |
| 에콰도르 | 593 |
| 프랑스령 기아나 | 594 |
| 파라과이 | 595 |
| 프랑스령 마르티니크 | 596 |
| 수리남 | 597 |
| 우루과이 | 598 |
| 네덜란드령 안틸레스 / 퀴라소 | 599 |
| 동티모르 | 670 |
| 브루나이 다루살람 | 673 |
| 나우루 | 674 |
| 파푸아뉴기니 | 675 |
| 통가 | 676 |
| 솔로몬 제도 | 677 |
| 바누아투 | 678 |
| 피지 | 679 |
| 팔라우 | 680 |
| 프랑스령 월리스 푸투나 제도 | 681 |
| 쿡 제도 | 682 |
| 사모아 | 685 |
| 키리바시 | 686 |
| 뉴칼레도니아 | 687 |
| 프랑스령 폴리네시아 | 689 |
| 미크로네시아 | 691 |
| 마셜 제도 공화국 | 692 |
| 홍콩 | 852 |
| 마카오 | 853 |
| 캄보디아 | 855 |
| 라오스 | 856 |
| 방글라데시 | 880 |
| 대만 | 886 |
| 몰디브 | 960 |
| 레바논 | 961 |
| 요르단 | 962 |
| 시리아 | 963 |
| 이라크 | 964 |
| 쿠웨이트 | 965 |
| 사우디아라비아 | 966 |
| 예멘 | 967 |
| 오만 | 968 |
| 팔레스타인 | 970 |
| 아랍에미리트연합 | 971 |
| 이스라엘 | 972 |
| 바레인 | 973 |
| 카타르 | 974 |
| 부탄 | 975 |
| 몽골 | 976 |
| 네팔 | 977 |
| 타지키스탄 | 992 |
| 투르크메니스탄 | 993 |
| 아제르바이잔 | 994 |
| 조지아 | 995 |
| 키르기스스탄 | 996 |
| 우즈베키스탄 | 998 |
| 바하마 | 1242 |
| 바베이도스 | 1246 |
| 영국령 앵귈라 | 1264 |
| 앤티가바부다 | 1268 |
| 미국령 버진아일랜드 | 1284 |
| 영국령 버진아일랜드 | 1340 |
| 영국령 케이맨 제도 | 1345 |
| 영국령 버뮤다 | 1441 |
| 그레나다 | 1473 |
| 영국령 커크스케이커스제도 | 1649 |
| 영국령 몬트세랫 | 1664 |
| 미국령 북마리아나 제도 | 1670 |
| 괌 | 1671 |
| 미국령사모아 | 1684 |
| 세인트루시아 | 1758 |
| 도미니카 연방 | 1767 |
| 세인트 빈센트 그레나딘 | 1784 |
| 푸에르토리코 | 1787, 1939 |
| 도미니카 공화국 | 1809, 1829, 1849 |
| 트리니다드 토바고 | 1868 |
| 세인트키츠네비스 | 1869 |
| 자메이카 | 1876 |
