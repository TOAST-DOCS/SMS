## Notification > SMS > 결과 코드

## API 결과 코드

| 서비스 | 성공 여부 | 결과 코드 | 결과 코드 메시지 | API 응답 메시지 | 
| - | - | - | - | - |
| 공통 | true | 0 | 성공 | SUCCESS |
| 공통 | false | 4 | 파라미터 유효성 검증 실패 | | 
| 공통 | false | -1000 | 유효하지 않은 앱키 | Invalid appKey. |
| 공통 | false | -1001 | 존재하지 않는 앱키 | Service is not exist. |
| 공통 | false | -1002 | 사용 종료된 앱키 | Service is disabled. |
| 공통 | false | -1003 | 프로젝트에 포함되지 않는 멤버 | Not project memeber id. |
| 공통 | false | -1004 | 허용되지 않는 IP | Not allow ip. |
| 공통 | false | -1007 | 유효하지 않은 멤버 | MemberType is invalid. |
| 공통 | false | -1008 | 차단된 프로젝트 | Service is blocked. |
| 공통 | false | -9995 | 유효하지 않는 API 버전 | Invalid api version. |
| 공통 | false | -9996 | 유효하지 않는 contentType. Only application/JSON | Only application/json Content-type is supported. |
| 공통 | false | -9997 | 유효하지 않는 JSON 형식 | Invalid API parameters. |
| 공통 | false | -9998 | 존재하지 않는 API | Not exist API. |
| 공통 | false | -9999 | 시스템 오류(예기치 못한 오류) | System error. Please inquire at support@toast.com. |
| 발송/조회 | false | -1005 | 유효하지 않는 검색 조건 | Service parameter is invalid. | 
| 발송/조회 | false | -1006 | 유효하지 않는 발송 메시지(messageType) 유형 | MessageType is invalid. |
| 발송/조회 | false | -2000 | 유효하지 않는 날짜 형식 | Date format error. |
| 발송/조회 | false | -2001 | 수신자가 비어 있는 경우 | RecipientList can not be null. |
| 발송/조회 | false | -2002 | 첨부 파일 이름이 잘못된 경우 | Invalid attach file name. |
| 발송/조회 | false | -2003 | 첨부 파일 확장자가 jpg, jpeg가 아닌 경우 | Attach file required jpg or jpeg. |
| 발송/조회 | false | -2004 | 첨부 파일이 만료되거나 존재하지 않는 경우 | File is expired or does not exist. | 
| 발송/조회 | false | -2005 | 첨부 파일 사이즈가 300KB가 넘는 경우 | The file size must be greater than 0 and less than 300KB. |
| 발송/조회 | false | -2006 | 템플릿에 설정된 발송 유형과 요청온 발송 유형이 맞지 않는 경우 | Invalid template type. |
| 발송/조회 | false | -2008 | 요청 ID(requestId)가 잘못된 경우 | Invalid requestId. |
| 발송/조회 | false | -2009 | 첨부 파일 업로드 도중 서버 오류로 인해 정상적으로 업로드되지 않은 경우 | Upload attach file error. | 
| 발송/조회 | false | -2010 | 첨부 파일 업로드 유형이 잘못된 경우(서버 오류) | Upload attach file type can not be empty. |
| 발송/조회 | false | -2011 | 필수 조회 파라미터가 비어 있는 경우(requestId 또는 startRequestDate, endRequestdate) | RequestId or start/endRequestDate or start/endCreateDate is required. | 
| 발송/조회 | false | -2012 | 상세 조회 파라미터가 잘못된 경우(requestId 또는 mtPr) | Search parameter is invalid.(requestId and mtPr). |
| 발송/조회 | false | -2014 | 제목 또는 본문이 비어 있는 경우 | The recipient can not be empty. |
| 발송/조회 | false | -2015 | 제목 또는 본문이 초과된 경우 | Title or Body exceed maximum byte. |
| 발송/조회 | false | -2016 | 수신자가 1,000명이 넘어간 경우 | The max recipient size is 1000. |
| 발송/조회 | false | -2017 | Excel 생성이 실패한 경우 | Making Excel file is failed. |
| 발송/조회 | false | -2018 | 수신자 번호가 비어 있는 경우 | RecipientNo can not be empty. |
| 발송/조회 | false | -2019 | 수신자 번호가 유효하지 않는 경우 | RecipientNo is invalid. |
| 발송/조회 | false | -2021 | 시스템 오류(큐 저장 실패) | System error. Failed insert queue. |
| 발송/조회 | false | -2022 | 요청 일시를 현재 시간보다 이전으로 설정한 경우 | RequestDate is not before currentDate. |
| 발송/조회 | false | -2023 | 제목 또는 본문에 허용되지 않는 문자(Emoji 등)가 포함된 경우 | Unacceptable characters in title and body. |
| 발송/조회 | false | -2024 | LMS/MMS로 국제 발송을 전송하는 경우 | LMS/MMS Type is not sent to outside of Korea. |
| 발송/조회 | false | -2044 | 발송 불가능한 국가로 요청을 보낸 경우 | Invalid countryCode for sending. |
| 발송/조회 | false | -2045 | 국제 발송을 차단한 경우 | International sending blocked by service. |
| 발송/조회 | false | -2046 | 차단한 국가에 발송한 경우 | Blocked country by service. |
| 발송/조회 | false | -2047 | 차단 제한 건수가 넘은 경우 | Blocked by total indicator. |
| 발송/조회 | false | -2048 | 국제 발송 본문이 초과된 경우 | International message body exceed maximum length. |
| 발송/조회 | false | -4000 | 조회 범위가 한달이 넘어간 경우 | Search is possible within one month. |
| 발송/조회 | false | -8000 | 인증 발송에 인증 문구가 포함되지 않은 경우 | The body must contain auth guide ment. |
| 템플릿 | false | -2100 | 템플릿 ID가 비어 있는 경우 | The templateId can not be empty. |
| 템플릿 | false | -2101 | 이미 등록된 템플릿 ID | Already used templateId. |
| 템플릿 | false | -2102 | 템플릿 이름이 비어 있는 경우 | The template name can not be empty. |
| 템플릿 | false | -2103 | 발신 번호가 비어 있는 경우 | The sendNo can not be empty. |
| 템플릿 | false | -2104 | 발송 유형이 비어 있는 경우(0: sms, 1: mms) | The sendType can not be empty.(0-sms, 1-mms) |
| 템플릿 | false | -2105 | 본문이 비어 있는 경우 | The body can not be empty. |
| 템플릿 | false | -2106 | 사용 여부가 잘못된 경우 | UseYn is invalid. |
| 템플릿 | false | -2107 | 유효하지 않는 템플릿 ID(수정/삭제 시) | Invalid template. |
| 템플릿 | false | -2108 | 카테고리 ID가 비어 있는 경우 | The categoryId can not be empty. |
| 템플릿 | false | -2109 | 템플릿 ID가 50 글자를 초과하는 경우 | TemplateId length must be under 50. |
| 템플릿 | false | -2110 | 템플릿이 존재하지 않는 경우 | Template is not exist. |
| 템플릿 | false | -2111 | 유효하지 않는 템플릿 파라미터인 경우 | Template add parameter is invalid. |
| 템플릿 | false | -2112 | 최대 등록 가능한 템플릿 개수를 초과한 경우(최대: 1000)  | The maximum number of registered templates. |
| 템플릿 | false | -2114 | 제목이 비어 있는 경우  | The title can not be empty. |
| 템플릿 | false | -2115 | 제목이 120 글자를 초과하는 경우  | Title length must be under 120. |
| 템플릿 | false | -2116 | 발송 유형이 SMS일 때 본문 길이가 255 글자를 초과하는 경우  | SMS Body length must be under 255. |
| 템플릿 | false | -2117 | 발송 유형이 LMS/MMS일 때 본문 길이가 4000 글자를 초과하는 경우  | LMS/MMS Body length must be under 4000. |
| 템플릿 | false | -2043 | 템플릿에 등록할 첨부 파일이 이미 다른 템플릿에 등록되어 있는 경우 | Already used attachFileId |
| 카테고리 | false | -2200 | 유효하지 않는 카테고리 파라미터(등록 시) | Invalid add category parameter.(categoryName, useYn) |
| 카테고리 | false | -2201 | 유효하지 않는 카테고리 파라미터(수정 시) | Invalid modify category parameter.(categoryId, categoryName, useYn) |
| 카테고리 | false | -2202 | 유효하지 않는 카테고리(카테고리 조회 실패) | Invalid category. |
| 카테고리 | false | -2203 | 부모 카테고리가 존재하지 않은 경우 | CategoryParentId is invalid. |
| 카테고리 | false | -2204 | 사용 여부가 잘못된 경우 | UseYn is invalid. |
| 카테고리 | false | -2205 | 최상위 카테고리를 삭제하려는 경우 | Cannot delete the highest category. |
| 카테고리 | false | -2206 | 존재하지 않은 카테고리인 경우 | Category is not exist. |
| 발신 번호 | false | -2312 | 발신 번호가 비어 있거나 미등록 상태 | Not regist sendno. |
| 발신 번호 | false | -2313 | 차단된 발신 번호 | This sendno is blocked. |
| 통계 | false | -2700 | 유효하지 않는 통계 범위 | Invalid search period. |
| 통계 | false | -2701 | 유효하지 않는 통계 검색 파라미터 | Invalid statistics search parameter. | 
| 통계 | false | -2703 | 유효하지 않는 통계 세부 범위 | Invalid duration time. |
| 통계 | false | -2704 | 유효하지 않는 통계 파라미터 | Invalid stats parameter. |
| 080 수신거부 | false | -6000 | 수신 거부 기능을 사용하고 있지 않음 | Block service is not joined. |
| 080 수신거부 | false | -6001 | 수신 거부된 번호 | Recipient Number is refused. |
| 080 수신거부 | false | -6003 | 본문에 수신 거부 안내 메시지가 없음 | The body must contain block guide ment. |
| 080 수신거부 | false | -6004 | 수신 거부 번호가 사용되고 있지 않음 | This is not a joined unsubscribeNo. |
| 태그 | false | -7000 | 태그 내부 오류(APi 호출 실패) | Fail to call Tag API. |
| 태그 | false | -7001 | 유효하지 않는 파라미터 | Invalid parameter. |
| 태그 | false | -7002 | .csv 읽기 실패 | Invalid csv read. |

## 수신 결과 코드

| 구분 | 결과 코드 | 분류 | 의미 |
| - | - | - | - |
| 이통사 | 1000 | 성공 | 성공 |
| 이통사 | 1001 | 실패 | Server Busy |
| 이통사 | 1002 | 실패 | 수신 번호 형식 오류 |
| 이통사 | 1003 | 실패 | 발신 번호 형식 오류 |
| 이통사 | 1019 | 실패 | TTL 초과 |
| 이통사 | 2000 | 실패 | 전송 시간 초과 |
| 이통사 | 2001 | 실패 | 전송 실패(무선망단) |
| 이통사 | 2002 | 실패 | 전송 실패(무선망 > 단말기단) |
| 이통사 | 2003 | 실패 | 단말기 전원 꺼짐 |
| 이통사 | 2004 | 실패 | 통신사와 단말 사이에 메시지 버퍼가 가득 차 전달 불가 상태 |
| 이통사 | 2005 | 실패 | 음영 지역 |
| 이통사 | 2006 | 실패 | 메시지 삭제됨 |
| 이통사 | 2007 | 실패 | 일시적인 단말 문제 |
| 이통사 | 3000 | 실패 | 전송할 수 없음 |
| 이통사 | 3001 | 실패 | 가입자 없음 |
| 이통사 | 3002 | 실패 | 성인 인증 실패 |
| 이통사 | 3003 | 실패 | 수신 번호 형식 오류 또는 결번(없는 번호) |
| 이통사 | 3004 | 실패 | 단말기 서비스 일시 정지 |
| 이통사 | 3005 | 실패 | 단말기 호 처리(Call processing) 상태, 단말에 도달하지 못함  |
| 이통사 | 3006 | 실패 | 착신 거절 |
| 이통사 | 3007 | 실패 | 콜백 URL을 받을 수 없는 단말기 |
| 이통사 | 3008 | 실패 | 기타 단말기 문제 |
| 이통사 | 3009 | 실패 | 메시지 형식 오류 |
| 이통사 | 3010 | 실패 | MMS 미지원 단말 |
| 이통사 | 3011 | 실패 | 서버 오류 |
| 이통사 | 3012 | 실패 | 스팸 |
| 이통사 | 3013 | 실패 | 서비스 거부 |
| 이통사 | 3014 | 실패 | 기타 |
| 이통사 | 3015 | 실패 | 전송 경로 없음 |
| 이통사 | 3016 | 실패 | 첨부 파일 크기 제한 실패 |
| 이통사 | 3017 | 실패 | 발신 번호 변작 방지 서비스에 의거한 번호 형식 오류 |
| 이통사 | 3018 | 실패 | 발신 번호 변작 방지 서비스에 가입된 휴대폰 개인 가입자 번호 |
| 이통사 | 3019 | 실패 | 발신 번호 KISA or 미래부에서 모든 고객사에 대하여 차단 처리된 번호 |
| ETC | E911 | 실패 | 첨부파일 확장자가 없는 경우 |
| ETC | E913 | 실패 | 첨부파일 크기가 0인 경우 |
| ETC | E915 | 실패 | 중복 메시지 |
| ETC | E919 | 실패 | 발송 제한 시간인 경우, 메시지 재발송 처리가 금지된 경우 |
| ETC | E999 | 실패 | 기타 오류 |

## 결과 조회 코드
### 수신 결과 조회 코드

| 코드 값 | 의미 | 
| - | - |
| MTR1 | 성공 | 
| MTR2 | 실패 | 

### 수신 결과 조회 상세 코드

| 코드 값 | 의미 | 
| - | - |
| MTR2_1 | 유효성 검사 실패 | 
| MTR2_2 | 통신사 문제 | 
| MTR2_3 | 단말기 문제 | 
