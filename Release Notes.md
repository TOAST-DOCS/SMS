## Notification > SMS > Release Notes

### 2016.09.29
#### 기능 개선/변경
* [Console] SMS 웹 페이지에서 발송 시, 수신자 번호 입력란에 국가 코드가 포함된 수신자 번호로 발송 가능하도록 수정.
    * 참고 : Notification > SMS > Getting Started > 일반 SMS 발송, 일반 LMS 발송, MMS 발송

#### 버그 수정
* [API] SMS 발송 조회 API response 값 중 userId 항목에 항상 null값이 응답하는 현상.
    * 현상 : SMS 발송 조회 API response 값 중 userId 항목에 항상 null값이 응답하는 현상.
    * 해결 : SMS 발송 조회 API response 값 중 userId 항목이 정상적인 데이터로 응답.
* [API] 인증용 SMS 단일 발송 조회 API response 값 중 sendType 항목에 비정상 데이터로 응답하는 현상.
    * 현상 : 인증용 SMS 단일 발송 조회 API response 값 중 sendType이 SMS 타입인 0인 문제.
    * 해결 : 인증용 SMS 단일 발송 조회 API response 값 중 sendType이 정상적인 데이터 2로 응답. (0:SMS, 1:MMS, 2:Auth)

### 2016.08.18
#### 기능 개선/변경
* [API] 국가 코드가 포함된 수신자 번호 입력 필드 추가
    * 참고 : Notification > SMS > Developer's Guide > [단문 SMS 발송, 장문 MMS 발송, 인증용 SMS 발송] API 명세서 (internationalRecipientNo 필드 추가)

#### 버그 수정
* [Console] SMS 템플릿 사용여부 변경이 정상적으로 안되는 문제 수정
    * 현상 : 사용여부를 사용/미사용 변경하더라도 다시 템플릿 호출 시 정상적으로 반영되지 않는 현상
    * 해결 : 사용여부 필드가 정상적으로 저장 및 노출되도록 수정

### 2016.08.04
#### 버그 수정
* [API] SMS 발신번호가 핸드폰 인증으로 등록된 경우 MMS 발송이 안되는 현상
    * 현상 : 핸드폰 인증으로 저장되는 발신번호가 국제번호 형태(82-1x-xxxx-xxxx)로 저장이 되며, 이로인해 MMS 발송이 안되는 문제가 있음
    * 해결 : 국제번호로 저장되던 방식을 대한민국 핸드폰 번호 형태(01x-xxxx-xxxx)로 변경하여 저장되도록 수정
* [API] countryCode 공백으로 입력 시 문자 발송이 안되는 버그 수정
    * 현상 : countryCode에 빈 값(null, "") 전송 시 응답은 성공으로 노출되나 정상적으로 발송이 안되는 현상
    * 해결 : countryCode에 빈 값이 넘어올 경우 국가 번호 82가 기본적으로 설정되도록 수정
