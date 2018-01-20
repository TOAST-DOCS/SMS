## Notification > SMS > 오류 코드

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

## EMMA v.3 수신결과코드
- EMMA Version : EMMA V3.3.0 이상
- 1) 이통사 : 이통사 전송 후 받은 결과코드이다.
- 2) IB G/W : Infobank G/W가 메시지 수신후 주는 결과코드이다.
- 3) IB EMMA : EMMA가 메시지 전송 요청에 대해 처리한 에러코드이다.

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
		<td>음영지역</td>
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
		<td>수신번호 형식 오류</td>
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
		<td>Callback URL을 받을 수 없는 폰</td>
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
		<td>첨부파일 사이즈 제한 실패</td>
	</tr>
	<tr>
		<td>3017</td>
		<td>Invalid</td>
		<td>발신번호(=회신번호) 변작 방지 서비스에 의거한 번호 형식 오류</td>
	</tr>
	<tr>
		<td>3018</td>
		<td>Invalid</td>
		<td>발신번호(=회신번호) 변작 방지 서비스에 가입된 휴대폰 개인가입자 번호</td>
	</tr>
	<tr>
		<td>3019</td>
		<td>Invalid</td>
		<td>발신번호(=회신번호) 인포뱅크에 번호 사전등록제를 통해 등록되지 않은 번호</td>
	</tr>
	<tr>
		<td rowspan=20>IB G/W</td>
		<td>1001</td>
		<td rowspan=20></td>
		<td>Server Busy (RS 내부 저장 Queue Full)</td>
	</tr>
	<tr>
		<td>1002</td>
		<td>수신번호 형식 오류</td>
	</tr>
	<tr>
		<td>1003</td>
		<td>회신번호 형식 오류</td>
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
		<td>charset conversion error</td>
	</tr>
	<tr>
		<td rowspan=24>IB EMMA</td>
		<td>E900</td>
		<td rowspan=24>Invalid-IB</td>
		<td>전송키가 없는 경우</td>
	</tr>
	<tr>
		<td>E901</td>
		<td>수신번호가 없는 경우</td>
	</tr>
	<tr>
		<td>E902</td>
		<td>(동보인 경우) 수신번호순번이 없는 경우</td>
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
		<td>회신번호가 없는 경우</td>
	</tr>
	<tr>
		<td>E906</td>
		<td>메시지키가 없는 경우</td>
	</tr>
	<tr>
		<td>E907</td>
		<td>동보 여부가 없는 경우</td>
	</tr>
	<tr>
		<td>E908</td>
		<td>서비스 타입이 없는 경우</td>
	</tr>
	<tr>
		<td>E909</td>
		<td>전송요청시각이 없는 경우</td>
	</tr>
	<tr>
		<td>E910</td>
		<td>TTL 타임이 없는 경우</td>
	</tr>
	<tr>
		<td>E911</td>
		<td>서비스 타입이 MMS MT인 경우, 첨부파일 확장자가 없는 경우</td>
	</tr>
	<tr>
		<td>E912</td>
		<td>서비스 타입이 MMS MT인 경우, attach_file 폴더에 첨부파일이 없는 경우</td>
	</tr>
	<tr>
		<td>E913</td>
		<td>서비스 타입이 MMS MT인 경우, 첨부파일 사이즈가 0인 경우</td>
	</tr>
	<tr>
		<td>E914</td>
		<td>서비스 타입이 MMS MT인 경우, 메시지 테이블에는 파일그룹키가 있는데 파일 테이 블에 데이터가 없는 경우</td>
	</tr>
	<tr>
		<td>E915</td>
		<td>중복메시지</td>
	</tr>
	<tr>
		<td>E916</td>
		<td>인증서버 차단번호</td>
	</tr>
	<tr>
		<td>E917</td>
		<td>고객DB 차단번호</td>
	</tr>
	<tr>
		<td>E918</td>
		<td>USER CALLBACK FAIL</td>
	</tr>
	<tr>
		<td>E919</td>
		<td>발송 제한 시간인 경우, 메시지 재발송 처리가 금지 된 경우</td>
	</tr>
	<tr>
		<td>E920</td>
		<td>서비스 타입이 LMS MT인 경우, 메시지 테이블에 파일그룹키가 있는 경우</td>
	</tr>
	<tr>
		<td>E921</td>
		<td>서비스 타입이 MMS MT인 경우, 메시지 테이블에 파일그룹키가 없는 경우</td>
	</tr>
	<tr>
		<td>E922</td>
		<td>동보단어 제약문자 사용 오류</td>
	</tr>
	<tr>
		<td>E999</td>
		<td>기타오류</td>
	</tr>
	<tr>
		<td rowspan=10>IB 인증서버</td>
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
		<td>클라이언트 패스워드 틀림</td>
	</tr>
	<tr>
		<td>1005</td>
		<td>공개키가 이미 등록 되어 있음</td>
	</tr>
	<tr>
		<td>1006</td>
		<td>클라이언트 공개키 중복</td>
	</tr>
	<tr>
		<td>1007</td>
		<td>IP Address 인증 실패</td>
	</tr>
	<tr>
		<td>1008</td>
		<td>MAC Address 인증 실패</td>
	</tr>
	<tr>
		<td>1009</td>
		<td>서비스 거부 됨 (고객 접속 금지)</td>
	</tr>
</tbody>
</table>