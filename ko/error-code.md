## Notification > SMS > 오류 코드

## API 응답 코드
| service | isSuccess | resultCode | resultMessage |
| - | - | - | - |
| 공통 | true | 0 | 성공 |
| 공통 | false | 4 | 파라미터 검증 실패<br/>- 단문 발송 시 본문의 길이가 4000글자를 넘어가는 경우<br/>- 장문/첨부 발송 시 제목의 길이가 120글자를 넘어가는 경우<br/>- 통계 조회 시 searchType, from, to 항목이 비어있는 경우 |
| 공통 | false | -1000 | 유효하지 않은 앱키 |
| 공통 | false | -1001 | 존재하지 않는 앱키 |
| 공통 | false | -1002 | 사용 종료된 앱키 |
| 공통 | false | -1003 | 프로젝트에 포함되지 않는 멤버 |
| 공통 | false | -1004 | 허용되지 않는 IP |
| 공통 | false | -9996 | 유효하지 않는 contectType. Only application/JSON |
| 공통 | false | -9997 | 유효하지 않는 JSON 형식 |
| 공통 | false | -9998 | 존재하지 않는 API |
| 공통 | false | -9999 | 시스템 오류(예기치 못한 오류) |
| 발송/조회 | false | -1006 | 유효하지 않는 발송 메시지(messageType) 유형 |
| 발송/조회 | false | -2000 | 유효하지 않는 날짜 형식 |
| 발송/조회 | false | -2001 | 수신자가 비어 있는 경우 |
| 발송/조회 | false | -2002 | 첨부 파일 이름이 잘못된 경우 |
| 발송/조회 | false | -2003 | 첨부 파일 확장자가 jpg, jpeg가 아닌 경우 |
| 발송/조회 | false | -2004 | 첨부 파일이 존재하지 않는 경우 |
| 발송/조회 | false | -2005 | 첨부 파일 사이즈가 300KB이 넘는 경우 |
| 발송/조회 | false | -2006 | 템플릿에 설정된 발송 유형과 요청온 발송 유형이 맞지 않는 경우 |
| 발송/조회 | false | -2008 | 요청 ID(requestId)가 잘못된 경우 |
| 발송/조회 | false | -2009 | 첨부 파일 업로드 도중 서버오류로 인해 정상적으로 업로드되지 않은 경우 |
| 발송/조회 | false | -2010 | 첨부 파일 업로드 유형이 잘못된 경우(서버 오류) |
| 발송/조회 | false | -2011 | 필수 조회 파라미터가 비어 있는 경우(requestId 또는 startRequestDate, endRequestdate)
| 발송/조회 | false | -2012 | 상세 조회 파라미터가 잘못된 경우(requestId 또는 mtPr) |
| 발송/조회 | false | -2014 | 제목 또는 본문이 비어 있는 경우 |
| 발송/조회 | false | -2016 | 수신자가 1,000명이 넘어간 경우 |
| 발송/조회 | false | -2017 | Excel 생성이 실패한 경우 |
| 발송/조회 | false | -2018 | 수신자 번호가 비어 있는 경우 |
| 발송/조회 | false | -2019 | 수신자 번호가 유효하지 않는 경우 |
| 발송/조회 | false | -2021 | 시스템 오류(큐 저장 실패) |
| 발송/조회 | false | -2022 | 요청 일시를 현재 시간보다 이전으로 설정한 경우 |
| 발송/조회 | false | -2023 | 제목 또는 본문에 허용되지 않는 문자(Emoji 등)가 포함된 경우 |
| 발송/조회 | false | -2024 | LMS/MMS로 국제 발송을 전송하는 경우 |
| 발송/조회 | false | -4000 | 조회 범위가 한달이 넘어간 경우 |
| 템플릿 | false | -2100 | 템플릿 ID가 비어 있는 경우 |
| 템플릿 | false | -2101 | 이미 등록된 템플릿 ID |
| 템플릿 | false | -2102 | 템플릿 이름이 비어 있는 경우 |
| 템플릿 | false | -2103 | 발신 번호가 비어 있는 경우 |
| 템플릿 | false | -2104 | 발송 유형이 비어 있는 경우(0: sms, 1: mms) |
| 템플릿 | false | -2105 | 본문이 비어 있는 경우 |
| 템플릿 | false | -2106 | 사용 여부가 잘못된 경우 |
| 템플릿 | false | -2107 | 유효하지 않는 템플릿 ID(수정/삭제 시) |
| 템플릿 | false | -2108 | 카테고리 ID가 비어 있는 경우 |
| 템플릿 | false | -2109 | 템플릿 ID가 50글자를 초과하는 경우 |
| 템플릿 | false | -2110 | 템플릿이 존재하지 않는 경우 |
| 템플릿 | false | -2111 | 유효하지 않는 템플릿 파라미터인 경우 |
| 템플릿 | false | -2112 | 최대 등록 가능한 템플릿 갯수를 초과한 경우(최대: 1000)  |
| 템플릿 | false | -2113 | 이미 삭제된 템플릿 ID  |
| 템플릿 | false | -2114 | 제목이 비어있는 경우  |
| 템플릿 | false | -2115 | 제목이 120글자를 초과하는 경우  |
| 템플릿 | false | -2116 | 발송 유형이 SMS일때, 본문 길이가 255글자를 초과하는 경우  |
| 템플릿 | false | -2117 | 발송 유형이 LMS/MMS일때, 본문 길이가 4000글자를 초과하는 경우  |
| 카테고리 | false | -2200 | 유효하지 않는 카테고리 파라미터(등록 시) |
| 카테고리 | false | -2201 | 유효하지 않는 카테고리 파라미터(수정 시) |
| 카테고리 | false | -2202 | 유효하지 않는 카테고리(카테고리 조회 실패) |
| 카테고리 | false | -2203 | 부모 카테고리가 존재하지 않은 경우 |
| 카테고리 | false | -2204 | 사용 여부가 잘못된 경우 |
| 카테고리 | false | -2205 | 최상위 카테고리를 삭제하려는 경우 |
| 발신 번호 등록 요청 | false | -2301 | 이미 등록된 발신번호 |
| 발신 번호 등록 요청 | false | -2302 | 등록 요청한 발신번호 리스트 파라미터가 비어있는 상태  |
| 발신 번호 등록 요청 | false | -2304 | 유효하지 않는 등록 파라미터(첨부파일) |
| 발신 번호 | false | -2312 | 발신 번호가 비어있거나 미등록 상태 |
| 발신 번호 | false | -2313 | 차단된 발신 번호 |
| 발신 번호 | false | -2314 | 발신 번호 등록 요청 파라미터가 유효하지 않음 |
| 통계 | false | -2700 | 유효하지 않는 통계 범위 |
| 통계 | false | -2701 | 유효하지 않는 통계 검색 파라미터 |
| 통계 | false | -2703 | 유효하지 않는 통계 세부 범위 |
| 080 수신거부 | false | -6000 | 수신 거부 기능을 사용하고 있지 않음 |
| 080 수신거부 | false | -6001 | 수신 거부된 번호 |
| 080 수신거부 | false | -6003 | 본문에 수신 거부 안내 메시지가 없음 |
| 080 수신거부 | false | -6004 | 수신 거부 번호가 사용되고 있지 않음 |
| 태그 | false | -7000 | 태그 내부 오류(APi 호출 실패) |
| 태그 | false | -7001 | 유효하지 않는 파라미터 |
| 태그 | false | -7002 | .csv 읽기 실패 |

## 발신 조회 코드
### 수신 결과 코드
<table class="table table-striped table-hover">
<thead>
	<tr>
    <th>코드값</th>
    <th>의미</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>MTR1</td>
    <td>성공</td>
  </tr>
  <tr>
    <td>MTR2</td>
    <td>실패</td>
  </tr>
</tbody>
</table>

### 수신 결과 상세 코드
<table class="table table-striped table-hover">
<thead>
	<tr>
    <th>코드값</th>
    <th>의미</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>MTR2_1</td>
    <td>유효성 검사 실패</td>
  </tr>
  <tr>
    <td>MTR2_2</td>
    <td>통신사 문제</td>
  </tr>
  <tr>
    <td>MTR2_3</td>
    <td>단말기 문제</td>
  </tr>
</tbody>
</table>

## EMMA v.3 수신 결과 코드
EMMA Version: EMMA V3.3.0 이상

1) 이통사: 이통사 전송 후 받은 결과 코드이다.

2) IB G/W: Infobank G/W가 메시지 수신 후 전달하는 결과 코드이다.

3) IB EMMA: EMMA가 메시지 전송 요청에 대해 처리한 오류 코드이다.

<table class="table table-striped table-hover">
<thead>
	<tr>
		<th>구분</th>
		<th>코드값</th>
		<th>분류</th>
		<th>의미</th>
	</tr>
</thead>
<tbody>
	<tr>
		<td rowspan=29>이통사</td>
		<td>1000</td>
		<td>success</td>
		<td>성공</td>
	</tr>
	<tr>
		<td>2000</td>
		<td>failure</td>
		<td>전송 시간 초과</td>
	</tr>
	<tr>
		<td>2001</td>
		<td>failure</td>
		<td>전송 실패 (무선망단)</td>
	</tr>
	<tr>
		<td>2002</td>
		<td>failure</td>
		<td>전송 실패 (무선망 -> 단말기단)</td>
	</tr>
	<tr>
		<td>2003</td>
		<td>failure</td>
		<td>단말기 전원 꺼짐</td>
	</tr>
	<tr>
		<td>2004</td>
		<td>failure</td>
		<td>단말기 메시지 버퍼 풀</td>
	</tr>
	<tr>
		<td>2005</td>
		<td>failure</td>
		<td>음영 지역</td>
	</tr>
	<tr>
		<td>2006</td>
		<td>failure</td>
		<td>메시지 삭제됨</td>
	</tr>
	<tr>
		<td>2007</td>
		<td>failure</td>
		<td>일시적인 단말 문제</td>
	</tr>
	<tr>
		<td>3000</td>
		<td>Invalid</td>
		<td>전송할 수 없음</td>
	</tr>
	<tr>
		<td>3001</td>
		<td>Invalid</td>
		<td>가입자 없음</td>
	</tr>
	<tr>
		<td>3002</td>
		<td>Invalid</td>
		<td>성인 인증 실패</td>
	</tr>
	<tr>
		<td>3003</td>
		<td>Invalid</td>
		<td>수신 번호 형식 오류</td>
	</tr>
	<tr>
		<td>3004</td>
		<td>Invalid</td>
		<td>단말기 서비스 일시 정지</td>
	</tr>
	<tr>
		<td>3005</td>
		<td>Invalid</td>
		<td>단말기 호 처리 상태</td>
	</tr>
	<tr>
		<td>3006</td>
		<td>Invalid</td>
		<td>착신 거절</td>
	</tr>
	<tr>
		<td>3007</td>
		<td>Invalid</td>
		<td>콜백 URL을 받을 수 없는 폰</td>
	</tr>
	<tr>
		<td>3008</td>
		<td>Invalid</td>
		<td>기타 단말기 문제</td>
	</tr>
	<tr>
		<td>3009</td>
		<td>Invalid</td>
		<td>메시지 형식 오류</td>
	</tr>
	<tr>
		<td>3010</td>
		<td>Invalid</td>
		<td>MMS 미지원 단말</td>
	</tr>
	<tr>
		<td>3011</td>
		<td>Invalid</td>
		<td>서버 오류</td>
	</tr>
	<tr>
		<td>3012</td>
		<td>Invalid</td>
		<td>스팸</td>
	</tr>
	<tr>
		<td>3013</td>
		<td>Invalid</td>
		<td>서비스 거부</td>
	</tr>
	<tr>
		<td>3014</td>
		<td>Invalid</td>
		<td>기타</td>
	</tr>
	<tr>
		<td>3015</td>
		<td>Invalid</td>
		<td>전송 경로 없음</td>
	</tr>
	<tr>
		<td>3016</td>
		<td>Invalid</td>
		<td>첨부 파일 크기 제한 실패</td>
	</tr>
	<tr>
		<td>3017</td>
		<td>Invalid</td>
		<td>발신 번호(=회신 번호) 변작 방지 서비스에 의거한 번호 형식 오류</td>
	</tr>
	<tr>
		<td>3018</td>
		<td>Invalid</td>
		<td>발신 번호(=회신 번호) 변작 방지 서비스에 가입된 휴대폰 개인 가입자 번호</td>
	</tr>
	<tr>
		<td>3019</td>
		<td>Invalid</td>
		<td>발신 번호(=회신 번호) 인포뱅크에 번호 사전 등록제를 통해 등록되지 않은 번호</td>
	</tr>
	<tr>
		<td rowspan=20>IB G/W</td>
		<td>1001</td>
		<td rowspan=20></td>
		<td>Server Busy (RS 내부 저장 Queue Full)</td>
	</tr>
	<tr>
		<td>1002</td>
		<td>수신 번호 형식 오류</td>
	</tr>
	<tr>
		<td>1003</td>
		<td>회신 번호 형식 오류</td>
	</tr>
	<tr>
		<td>1004</td>
		<td>SPAM</td>
	</tr>
	<tr>
		<td>1005</td>
		<td>사용 건수 초과</td>
	</tr>
	<tr>
		<td>1006</td>
		<td>첨부 파일 없음</td>
	</tr>
	<tr>
		<td>1007</td>
		<td>첨부 파일 있음</td>
	</tr>
	<tr>
		<td>1008</td>
		<td>첨부 파일 저장 실패</td>
	</tr>
	<tr>
		<td>1009</td>
		<td>CLIENT_MSG_KEY 없음</td>
	</tr>
	<tr>
		<td>1010</td>
		<td>CONTENT 없음</td>
	</tr>
	<tr>
		<td>1011</td>
		<td>CALLBACK 없음</td>
	</tr>
	<tr>
		<td>1012</td>
		<td>RECIPIENT_INFO 없음</td>
	</tr>
	<tr>
		<td>1013</td>
		<td>SUBJECT 없음</td>
	</tr>
	<tr>
		<td>1014</td>
		<td>첨부 파일 KEY 없음</td>
	</tr>
	<tr>
		<td>1015</td>
		<td>첨부 파일 NAME 없음</td>
	</tr>
	<tr>
		<td>1016</td>
		<td>첨부 파일 크기 없음</td>
	</tr>
	<tr>
		<td>1017</td>
		<td>첨부 파일 Content 없음</td>
	</tr>
	<tr>
		<td>1018</td>
		<td>전송 권한 없음</td>
	</tr>
	<tr>
		<td>1019</td>
		<td>TTL 초과</td>
	</tr>
	<tr>
		<td>1020</td>
		<td>charset conversion error {@확인 - 한글로 수정해도 되나요.}</td>
	</tr>
	<tr>
		<td rowspan=24>IB EMMA</td>
		<td>E900</td>
		<td rowspan=24>Invalid-IB</td>
		<td>전송 키가 없는 경우</td>
	</tr>
	<tr>
		<td>E901</td>
		<td>수신 번호가 없는 경우</td>
	</tr>
	<tr>
		<td>E902</td>
		<td>(동보인 경우) 수신 번호 순번이 없는 경우</td>
	</tr>
	<tr>
		<td>E903</td>
		<td>제목 없는 경우</td>
	</tr>
	<tr>
		<td>E904</td>
		<td>메시지가 없는 경우</td>
	</tr>
	<tr>
		<td>E905</td>
		<td>회신 번호가 없는 경우</td>
	</tr>
	<tr>
		<td>E906</td>
		<td>메시지 키가 없는 경우</td>
	</tr>
	<tr>
		<td>E907</td>
		<td>동보(broadcast message) 여부가 없는 경우</td>
	</tr>
	<tr>
		<td>E908</td>
		<td>서비스 유형이 없는 경우</td>
	</tr>
	<tr>
		<td>E909</td>
		<td>전송 요청 시각이 없는 경우</td>
	</tr>
	<tr>
		<td>E910</td>
		<td>TTL 타임이 없는 경우</td>
	</tr>
	<tr>
		<td>E911</td>
		<td>서비스 유형이 MMS MT인 경우, 첨부 파일 확장자가 없는 경우</td>
	</tr>
	<tr>
		<td>E912</td>
		<td>서비스 유형이 MMS MT인 경우, attach_file 폴더에 첨부 파일이 없는 경우</td>
	</tr>
	<tr>
		<td>E913</td>
		<td>서비스 유형이 MMS MT인 경우, 첨부 파일 크기가 0인 경우</td>
	</tr>
	<tr>
		<td>E914</td>
		<td>서비스 유형이 MMS MT인 경우, 메시지 테이블에는 파일 그룹 키가 있는데 파일 테이블에 데이터가 없는 경우</td>
	</tr>
	<tr>
		<td>E915</td>
		<td>중복메시지</td>
	</tr>
	<tr>
		<td>E916</td>
		<td>인증 서버 차단번호</td>
	</tr>
	<tr>
		<td>E917</td>
		<td>고객 DB 차단번호</td>
	</tr>
	<tr>
		<td>E918</td>
		<td>USER CALLBACK FAIL</td>
	</tr>
	<tr>
		<td>E919</td>
		<td>발송 제한 시간인 경우, 메시지 재발송 처리가 금지된 경우</td>
	</tr>
	<tr>
		<td>E920</td>
		<td>서비스 유형이 LMS MT인 경우, 메시지 테이블에 파일 그룹 키가 있는 경우</td>
	</tr>
	<tr>
		<td>E921</td>
		<td>서비스 유형이 MMS MT인 경우, 메시지 테이블에 파일 그룹 키가 없는 경우</td>
	</tr>
	<tr>
		<td>E922</td>
		<td>동보(broadcast) 단어 제약 문자 사용 오류</td>
	</tr>
	<tr>
		<td>E999</td>
		<td>기타오류</td>
	</tr>
	<tr>
		<td rowspan=10>IB 인증 서버</td>
		<td>1000</td>
		<td rowspan=10></td>
		<td> 성공 </td>
	</tr>
	<tr>
		<td>1001</td>
		<td>ID 존재하지 않음</td>
	</tr>
	<tr>
		<td>1002</td>
		<td>인증 오류</td>
	</tr>
	<tr>
		<td>1003</td>
		<td>서버 내부 오류 (DB 접속 실패 등)</td>
	</tr>
	<tr>
		<td>1004</td>
		<td>클라이언트 비밀번호 틀림</td>
	</tr>
	<tr>
		<td>1005</td>
		<td>공개 키가 이미 등록 되어 있음</td>
	</tr>
	<tr>
		<td>1006</td>
		<td>클라이언트 공개 키 중복</td>
	</tr>
	<tr>
		<td>1007</td>
		<td>IP Address 인증 실패</td>
	</tr>
	<tr>
		<td>1008</td>
		<td>MAC 주소 인증 실패</td>
	</tr>
	<tr>
		<td>1009</td>
		<td>서비스 거부됨 (고객 접속 금지)</td>
	</tr>
</tbody>
</table>
