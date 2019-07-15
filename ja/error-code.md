## Notification > SMS > エラーコード
## APIレスポンスコード
| service | isSuccess | resultCode | resultMessage |
| - | - | - | - |
| 共通 | true | 0 | 成功 |
| 共通 | false | -1000 | 無効なアプリケーションキー |
| 共通 | false | -1001 | 存在しないアプリケーションキー |
| 共通 | false | -1002 | 使用終了したアプリケーションキー |
| 共通 | false | -1003 | プロジェクトに含まれていないメンバー |
| 共通 | false | -1004 | 許可されていないIP |
| 共通 | false | -9996 | 無効なcontectType. Only application/JSON |
| 共通 | false | -9997 | 無効なJSON形式 |
| 共通 | false | -9998 | 存在しないAPI |
| 共通 | false | -9999 | システムエラー(予期せぬエラー) |
| 送信/照会 | false | -1006 | 無効な送信メッセージ(messageType)タイプ |
| 送信/照会 | false | -2000 | 無効な日付形式 |
| 送信/照会 | false | -2001 | 受信者が空の場合 |
| 送信/照会 | false | -2002 | 添付ファイル名が無効な場合 |
| 送信/照会 | false | -2003 | 添付ファイルの拡張子がjpg、jpegではない場合 |
| 送信/照会 | false | -2004 | 添付ファイルが存在しない場合 |
| 送信/照会 | false | -2005 | 添付ファイルのサイズが300KBを超える場合 |
| 送信/照会 | false | -2006 | テンプレートに設定された送信タイプと、リクエストした送信タイプが一致しない場合 |
| 送信/照会 | false | -2008 | リクエストID(requestId)が無効な場合 |
| 送信/照会 | false | -2009 | 添付ファイルのアップロード中、サーバーエラーにより正常にアップロードされなかった場合 |
| 送信/照会 | false | -2010 | 添付ファイルのアップロードタイプが無効な場合(サーバーエラー) |
| 送信/照会 | false | -2011 | 必須照会パラメータが空の場合(requestIdまたはstartRequestDate、endRequestdate)
| 送信/照会 | false | -2012 | 詳細照会パラメータが無効な場合(requestIdまたはmtPr) |
| 送信/照会 | false | -2014 | タイトルまたは本文が空の場合 |
| 送信/照会 | false | -2016 | 受信者が1,000人を超える場合 |
| 送信/照会 | false | -2017 | Excel作成が失敗した場合 |
| 送信/照会 | false | -2018 | 受信者番号が空の場合 |
| 送信/照会 | false | -2019 | 受信者番号が無効な場合 |
| 送信/照会 | false | -2021 | システムエラー(キュー保存失敗) |
| 送信/照会 | false | -2022 | リクエスト日時を現在時間より前に設定した場合 |
| 送信/照会 | false | -2023 | タイトルまたは本文に許可されていない文字(Emojiなど)が含まれている場合 |
| 送信/照会 | false | -4000 | 照会範囲が1か月を超える場合 |
| テンプレート | false | -2100 | テンプレートIDが空の場合 |
| テンプレート | false | -2101 | すでに登録されているテンプレートID |
| テンプレート | false | -2102 | テンプレート名が空の場合 |
| テンプレート | false | -2103 | 発信番号が空の場合 |
| テンプレート | false | -2104 | 送信タイプが空の場合(0： sms, 1： mms) |
| テンプレート | false | -2105 | 本文が空の場合 |
| テンプレート | false | -2106 | 使用するかどうかが無効な場合 |
| テンプレート | false | -2107 | 無効なテンプレートID(修正/削除時) |
| テンプレート | false | -2108 | カテゴリーIDが空の場合 |
| テンプレート | false | -2109 | テンプレートIDが50文字を超える場合 |
| テンプレート | false | -2110 | テンプレートが存在しない場合 |
| テンプレート | false | -2111 | 유효하지 않는 템플릿 파라미터인 경우 |
| テンプレート | false | -2112 | 최대 등록 가능한 템플릿 갯수를 초과한 경우(최대: 1000)  |
| テンプレート | false | -2113 | 이미 삭제된 템플릿 ID  |
| テンプレート | false | -2114 | 제목이 비어있는 경우  |
| テンプレート | false | -2115 | 제목이 120글자를 초과하는 경우  |
| テンプレート | false | -2116 | 발송 유형이 SMS일때, 본문 길이가 255글자를 초과하는 경우  |
| テンプレート | false | -2117 | 발송 유형이 LMS/MMS일때, 본문 길이가 4000글자를 초과하는 경우  |
| テンプレート | false | -2043 | 템플릿에 등록할 첨부파일이 이미 다른 템플릿에 등록되어 있는 경우 |
| カテゴリー | false | -2200 | 無効なカテゴリーパラメータ(登録時) |
| カテゴリー | false | -2201 | 無効なカテゴリーパラメータ(修正時) |
| カテゴリー | false | -2202 | 無効なカテゴリー(カテゴリー照会失敗) |
| カテゴリー | false | -2203 | 부모 카테고리가 존재하지 않은 경우 |
| カテゴリー | false | -2204 | 사용 여부가 잘못된 경우 |
| カテゴリー | false | -2205 | 최상위 카테고리를 삭제하려는 경우 |
| 発信番号 | false | -2312 | 発信番号が空か未登録の状態 |
| 発信番号 | false | -2313 | 遮断された発信番号 |
| 発信番号 | false | -2314 | 発信番号登録リクエストのパラメータが無効 |
| 統計 | false | -2700 | 無効な統計範囲 |
| 統計 | false | -2701 | 無効な統計検索パラメータ |
| 統計 | false | -2703 | 無効な統計詳細範囲 |
| 080受信拒否 | false | -6000 | 受信拒否機能を使用していない |
| 080受信拒否 | false | -6001 | 受信拒否された番号 |
| 080受信拒否 | false | -6003 | 本文に受信拒否案内メッセージがない |
| 080受信拒否 | false | -6004 | 受信拒否番号が使用されていない |
| タグ | false | -7000 | タグ内部エラー(API呼び出し失敗) |
| タグ | false | -7001 | 無効なパラメータ |
| タグ | false | -7002 | .csv読み込み失敗 |

## 発信照会コード
### 受信結果コード
<table class="table table-striped table-hover">
<thead>
	<tr>
    <th>コード値</th>
    <th>意味</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>MTR1</td>
    <td>成功</td>
  </tr>
  <tr>
    <td>MTR2</td>
    <td>失敗</td>
  </tr>
</tbody>
</table>

### 受信結果詳細コード
<table class="table table-striped table-hover">
<thead>
	<tr>
    <th>コード値</th>
    <th>意味</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>MTR2_1</td>
    <td>有効性チェック失敗</td>
  </tr>
  <tr>
    <td>MTR2_2</td>
    <td>サービスプロバイダーの問題</td>
  </tr>
  <tr>
    <td>MTR2_3</td>
    <td>端末の問題</td>
  </tr>
</tbody>
</table>

## EMMA v.3受信結果コード
EMMA Version： EMMA V3.3.0以上

1)移動体通信事業者：移動体通信事業者に送信後に受け取った結果コード。

2) IB G/W：Infobank G/Wがメッセージ受信後に伝達する結果コード。

3) IB EMMA：EMMAがメッセージ送信リクエストに対して処理したエラーコード。

<table class="table table-striped table-hover">
<thead>
	<tr>
		<th>区分</th>
		<th>コード値</th>
		<th>分類</th>
		<th>意味</th>
	</tr>
</thead>
<tbody>
	<tr>
		<td rowspan=29>移動体通信事業者</td>
		<td>1000</td>
		<td>success</td>
		<td>成功</td>
	</tr>
	<tr>
		<td>2000</td>
		<td>failure</td>
		<td>送信時間超過</td>
	</tr>
	<tr>
		<td>2001</td>
		<td>failure</td>
		<td>送信失敗(移動体通信事業者と基地局の間で送信失敗)</td>
	</tr>
	<tr>
		<td>2002</td>
		<td>failure</td>
<td>送信失敗(基地局と端末の間で送信失敗)</td>
	</tr>
	<tr>
		<td>2003</td>
		<td>failure</td>
		<td>端末の電源がオフ</td>
	</tr>
	<tr>
		<td>2004</td>
		<td>failure</td>
		<td>端末メッセージバッファープール</td>
	</tr>
	<tr>
		<td>2005</td>
		<td>failure</td>
		<td>電波の届かない地域</td>
	</tr>
	<tr>
		<td>2006</td>
		<td>failure</td>
		<td>メッセージ削除済</td>
	</tr>
	<tr>
		<td>2007</td>
		<td>failure</td>
		<td>一時的な端末の問題</td>
	</tr>
	<tr>
		<td>3000</td>
		<td>Invalid</td>
		<td>送信できない</td>
	</tr>
	<tr>
		<td>3001</td>
		<td>Invalid</td>
		<td>加入者がいない</td>
	</tr>
	<tr>
		<td>3002</td>
		<td>Invalid</td>
		<td>成人認証の失敗</td>
	</tr>
	<tr>
		<td>3003</td>
		<td>Invalid</td>
		<td>受信番号形式エラー</td>
	</tr>
	<tr>
		<td>3004</td>
		<td>Invalid</td>
		<td>端末サービスの一時停止</td>
	</tr>
	<tr>
		<td>3005</td>
		<td>Invalid</td>
		<td>端末呼処理状態</td>
	</tr>
	<tr>
		<td>3006</td>
		<td>Invalid</td>
		<td>着信拒否</td>
	</tr>
	<tr>
		<td>3007</td>
		<td>Invalid</td>
		<td>コールバックURLを受け取れない端末</td>
	</tr>
	<tr>
		<td>3008</td>
		<td>Invalid</td>
		<td>その他端末の問題</td>
	</tr>
	<tr>
		<td>3009</td>
		<td>Invalid</td>
		<td>メッセージ形式エラー</td>
	</tr>
	<tr>
		<td>3010</td>
		<td>Invalid</td>
		<td>MMS未サポートの端末</td>
	</tr>
	<tr>
		<td>3011</td>
		<td>Invalid</td>
		<td>サーバーエラー</td>
	</tr>
	<tr>
		<td>3012</td>
		<td>Invalid</td>
		<td>スパム</td>
	</tr>
	<tr>
		<td>3013</td>
		<td>Invalid</td>
		<td>サービス拒否</td>
	</tr>
	<tr>
		<td>3014</td>
		<td>Invalid</td>
		<td>その他</td>
	</tr>
	<tr>
		<td>3015</td>
		<td>Invalid</td>
		<td>送信パスがない</td>
	</tr>
	<tr>
		<td>3016</td>
		<td>Invalid</td>
		<td>添付ファイルのサイズ制限で失敗</td>
	</tr>
	<tr>
		<td>3017</td>
		<td>Invalid</td>
		<td>発信番号(=返信番号)改ざん防止サービスに基づく番号形式エラー</td>
	</tr>
	<tr>
		<td>3018</td>
		<td>Invalid</td>
		<td>発信番号(=返信番号)改ざん防止サービスに加入している携帯電話個人加入者番号</td>
	</tr>
	<tr>
		<td>3019</td>
		<td>Invalid</td>
		<td>発信番号(=返信番号) InfoBankに番号事前登録制を通して登録されていない番号</td>
	</tr>
	<tr>
		<td rowspan=20>IB G/W</td>
		<td>1001</td>
		<td rowspan=20></td>
		<td>Server Busy (RS内部保存Queue Full)</td>
	</tr>
	<tr>
		<td>1002</td>
		<td>受信番号形式エラー</td>
	</tr>
	<tr>
		<td>1003</td>
		<td>返信番号形式エラー</td>
	</tr>
	<tr>
		<td>1004</td>
		<td>SPAM</td>
	</tr>
	<tr>
		<td>1005</td>
		<td>使用件数の超過</td>
	</tr>
	<tr>
		<td>1006</td>
		<td>添付ファイルがない</td>
	</tr>
	<tr>
		<td>1007</td>
		<td>添付ファイルがある</td>
	</tr>
	<tr>
		<td>1008</td>
		<td>添付ファイルの保存失敗</td>
	</tr>
	<tr>
		<td>1009</td>
		<td>CLIENT_MSG_KEYがない</td>
	</tr>
	<tr>
		<td>1010</td>
		<td>CONTENTがない</td>
	</tr>
	<tr>
		<td>1011</td>
		<td>CALLBACKがない</td>
	</tr>
	<tr>
		<td>1012</td>
		<td>RECIPIENT_INFOがない</td>
	</tr>
	<tr>
		<td>1013</td>
		<td>SUBJECTがない</td>
	</tr>
	<tr>
		<td>1014</td>
		<td>添付ファイルのKEYがない</td>
	</tr>
	<tr>
		<td>1015</td>
		<td>添付ファイルの名前がない</td>
	</tr>
	<tr>
		<td>1016</td>
		<td>添付ファイルのサイズが0</td>
	</tr>
	<tr>
		<td>1017</td>
		<td>添付ファイルのContentがない</td>
	</tr>
	<tr>
		<td>1018</td>
		<td>送信権限がない</td>
	</tr>
	<tr>
		<td>1019</td>
		<td>TTL超過</td>
	</tr>
	<tr>
		<td>1020</td>
		<td>charset conversion error {@確認 - ハングルに修正してもいいですか。}</td>
	</tr>
	<tr>
		<td rowspan=24>IB EMMA</td>
		<td>E900</td>
		<td rowspan=24>Invalid-IB</td>
		<td>送信キーがない場合</td>
	</tr>
	<tr>
		<td>E901</td>
		<td>受信番号がない場合</td>
	</tr>
	<tr>
		<td>E902</td>
		<td>(ブロードキャストの場合)受信番号の順番がない場合</td>
	</tr>
	<tr>
		<td>E903</td>
		<td>タイトルがない場合</td>
	</tr>
	<tr>
		<td>E904</td>
		<td>メッセージがない場合</td>
	</tr>
	<tr>
		<td>E905</td>
		<td>返信番号がない場合</td>
	</tr>
	<tr>
		<td>E906</td>
		<td>メッセージキーがない場合</td>
	</tr>
	<tr>
		<td>E907</td>
		<td>ブロードキャスト(broadcast message)かどうかがない場合</td>
	</tr>
	<tr>
		<td>E908</td>
		<td>サービスタイプがない場合</td>
	</tr>
	<tr>
		<td>E909</td>
		<td>送信リクエスト時刻がない場合</td>
	</tr>
	<tr>
		<td>E910</td>
		<td>TTLタイムがない場合</td>
	</tr>
	<tr>
		<td>E911</td>
		<td>サービスタイプがMMS MTの場合、添付ファイルに拡張子がない場合</td>
	</tr>
	<tr>
		<td>E912</td>
		<td>サービスタイプがMMS MTの場合、 attach_fileフォルダに添付ファイルがない場合</td>
	</tr>
	<tr>
		<td>E913</td>
		<td>サービスタイプがMMS MTの場合、添付ファイルサイズが0の場合</td>
	</tr>
	<tr>
		<td>E914</td>
		<td>サービスタイプがMMS MTの場合、メッセージテーブルにはファイルグループキーがあるが、ファイルテーブルにデータがない場合</td>
	</tr>
	<tr>
		<td>E915</td>
		<td>重複メッセージ</td>
	</tr>
	<tr>
		<td>E916</td>
		<td>認証サーバー遮断番号</td>
	</tr>
	<tr>
		<td>E917</td>
		<td>顧客DB遮断番号</td>
	</tr>
	<tr>
		<td>E918</td>
		<td>USER CALLBACK FAIL</td>
	</tr>
	<tr>
		<td>E919</td>
		<td>送信制限時間の場合、メッセージの再送信処理が禁止されている場合</td>
	</tr>
	<tr>
		<td>E920</td>
		<td>サービスタイプがLMS MTの場合、メッセージテーブルにファイルグループキーがある場合</td>
	</tr>
	<tr>
		<td>E921</td>
		<td>サービスタイプがMMS MTの場合、メッセージテーブルにファイルグループキーがない場合</td>
	</tr>
	<tr>
		<td>E922</td>
		<td>ブロードキャスト(broadcast)単語制約文字使用エラー</td>
	</tr>
	<tr>
		<td>E999</td>
		<td>その他のエラー</td>
	</tr>
	<tr>
		<td rowspan=10>IB認証サーバー</td>
		<td>1000</td>
		<td rowspan=10></td>
		<td> 成功 </td>
	</tr>
	<tr>
		<td>1001</td>
		<td>IDが存在しない</td>
	</tr>
	<tr>
		<td>1002</td>
		<td>認証エラー</td>
	</tr>
	<tr>
		<td>1003</td>
		<td>サーバー内部エラー(DB接続失敗など)</td>
	</tr>
	<tr>
		<td>1004</td>
		<td>クライアントパスワードの不一致</td>
	</tr>
	<tr>
		<td>1005</td>
		<td>公開鍵がすでに登録されている</td>
	</tr>
	<tr>
		<td>1006</td>
		<td>クライアント公開鍵の重複</td>
	</tr>
	<tr>
		<td>1007</td>
		<td>IP Address認証失敗</td>
	</tr>
	<tr>
		<td>1008</td>
		<td>MACアドレス認証失敗</td>
	</tr>
	<tr>
		<td>1009</td>
		<td>サービス拒否(顧客の接続禁止)</td>
	</tr>
</tbody>
</table>
