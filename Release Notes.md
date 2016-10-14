## Notification > SMS > Release Notes

### 2016.10.20
#### 기능 개선/변경
* [Console] 대량 업로드 발송 기능 개선
    * 대량 업로드 발송 CSV 포맷 지원 : 기존 대량 업로드 발송시 엑셀 파일 뿐만 아니라 CSV파일을 통해서도 발송 할 수 있도록 확장 (CSV 템플릿 제공)
    * 선택적 대상자 확인 프로세스: 기존 대량 업로드 발송시 대상자 확인 후 발송 할 수 있었던 프로세스에서 선택적으로 대상자를 확인 할 수 있도록 개선
    * 대량 업로드 발송 파일 validation 강화 : 대량 업로드 발송시 파일에 잘못된 포맷의 수신자 번호 입력 또는 누락된 데이터가 있는지 오류를 확인 해주는 기능이 추가
    * 대량SMS발송 탭 추가 : 대량 업로드 발송시 대상자 확인과 발송 진행 상태를 확인 할 수 있는 대량SMS발송 탭이 추가
    * 참고 : Notification > SMS > Getting Started > 대량 업로드 발송
* [Console] SMS 요청별 조회 탭에서 결과 사유 셀렉트 창 그룹화
    * 참고 : Notification > SMS > Getting Started > SMS 요청별 조회
* [API] SMS 발송 API request body 본문 내용 공란일 경우 발송 실패로 변경
    * AS-IS : 공란일 경우 Response는 성공이지만, 실제로 내용이 없기 때문에 미발송. 조회 시, 메시지 형식 오류로 표시됨
    * TO-BE : 공란일 경우 본문 내용이 없다는 내용과 함께 응답 실패

#### 버그 수정
* [Console] 파이어폭스 브라우저에서 SMS 발송 후 비정상 작동 수정
    * 현상 : 발송 후, 발신번호 셀렉트 창에서 선택이 되지 않거나, 필드 초기화 되지 않는 이슈
    * 해결 : 발송 후, 필드 초기화 및 정상적으로 동작하도록 수정

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
