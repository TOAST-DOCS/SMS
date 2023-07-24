## Notification > SMS > 結果コード

## API結果コード

| サービス | 成否 | 結果コード | 結果コードメッセージ | APIレスポンスメッセージ | 
| - | - | - | - | - |
| 共通 | true | 0 | 成功 | SUCCESS |
| 共通 | false | 4 | パラメータ検証に失敗しました | |
| 共通 | false | -1000 | 有効ではないアプリケーションキー | Invalid appKey. |
| 共通 | false | -1001 | 存在しないアプリケーションキー | Service is not exist. |
| 共通 | false | -1002 | 使用が終了したアプリケーションキー | Service is disabled. |
| 共通 | false | -1003 | プロジェクトに含まれないメンバー | Not project memeber id. |
| 共通 | false | -1004 | 許可されていないIP | Not allow ip. |
| 共通 | false | -1007 | 有効ではないメンバー | MemberType is invalid. |
| 共通 | false | -1008 | ブロックされたプロジェクト | Service is blocked. |
| 共通 | false | -9995 | 有効ではないAPIバージョン | Invalid api version. |
| 共通 | false | -9996 | 有効ではないcontentType. Only application/JSON | Only application/json Content-type is supported. |
| 共通 | false | -9997 | 有効ではないJSON形式 | Invalid API parameters. |
| 共通 | false | -9998 | 存在しないAPI | Not exist API. |
| 共通 | false | -9999 | システムエラー(予期しないエラー) | System error. Please inquire at support@toast.com. |
| 送信/照会 | false | -1005 | 有効ではない検索条件 | Service parameter is invalid. | 
| 送信/照会 | false | -1006 | 有効ではない送信メッセージ(messageType)タイプ | MessageType is invalid. |
| 送信/照会 | false | -2000 | 有効ではない日付形式 | Date format error. |
| 送信/照会 | false | -2001 | 受信者が空白の場合 | RecipientList can not be null. |
| 送信/照会 | false | -2002 | 添付ファイル名が無効な場合 | Invalid attach file name. |
| 送信/照会 | false | -2003 | 添付ファイルの拡張子がjpg、jpegではない場合 | Attach file required jpg or jpeg. |
| 送信/照会 | false | -2004 | 添付ファイルが期限切れまたは存在しない場合 | File is expired or does not exist. | 
| 送信/照会 | false | -2005 | 添付ファイルのサイズが300KBを超える場合 | The file size must be greater than 0 and less than 300KB. |
| 送信/照会 | false | -2006 | テンプレートに設定された送信タイプとリクエストされた送信タイプが異なる場合 | Invalid template type. |
| 送信/照会 | false | -2008 | リクエストID(requestId)が間違っている場合 | Invalid requestId. |
| 送信/照会 | false | -2009 | 添付ファイルのアップロード中にサーバーエラーで正常にアップロードされなかった場合 | Upload attach file error. | 
| 送信/照会 | false | -2010 | 添付ファイルのアップロードタイプが間違っている場合(サーバーエラー) | Upload attach file type can not be empty. |
| 送信/照会 | false | -2011 | 必須照会パラメータが空白の場合(requestIdまたはstartRequestDate, endRequestdate) | RequestId or start/endRequestDate or start/endCreateDate is required. | 
| 送信/照会 | false | -2012 | 詳細照会パラメータが無効な場合(requestIdまたはmtPr) | Search parameter is invalid.(requestId and mtPr). |
| 送信/照会 | false | -2014 | タイトルまたは本文が空白の場合 | The recipient can not be empty. |
| 送信/照会 | false | -2015 | タイトルまたは本文が超過した場合 | Title or Body exceed maximum byte. |
| 送信/照会 | false | -2016 | 受信者が1,000人を超える場合 | The max recipient size is 1000. |
| 送信/照会 | false | -2017 | Excelの作成が失敗した場合 | Making Excel file is failed. |
| 送信/照会 | false | -2018 | 受信者番号が空白の場合 | RecipientNo can not be empty. |
| 送信/照会 | false | -2019 | 受信者番号が有効ではない場合 | RecipientNo is invalid. |
| 送信/照会 | false | -2021 | システムエラー(キュー保存失敗) | System error. Failed insert queue. |
| 送信/照会 | false | -2022 | リクエスト日時を現在時間より前に設定した場合 | requestDate is not before currentDate |
| 送信/照会 | false | -2023 | タイトルまたは本文に許可されていない文字(Emojiなど)が含まれる場合 | Unacceptable characters in title and body|
| 送信/照会 | false | -2024 | LMS/MMSで国際送信を送信する場合 | LMS/MMS Type is not sent to outside of Korea. |
| 送信/照会 | false | -4000 | 照会範囲が1か月を超える場合 | Search is possible within one month. |
| 送信/照会 | false | -8000 | 認証送信に認証文言が含まれていない場合 | The body must contain auth guide ment. |
| テンプレート | false | -2100 | テンプレートIDが空白の場合 | The templateId can not be empty. |
| テンプレート | false | -2101 | すでに登録されているテンプレートID | Already used templateId. |
| テンプレート | false | -2102 | テンプレート名が空白の場合 | The template name can not be empty. |
| テンプレート | false | -2103 | 発信番号が空白の場合 | The sendNo can not be empty. |
| テンプレート | false | -2104 | 送信タイプが空白の場合(0: sms, 1: mms) | The sendType can not be empty.(0-sms, 1-mms) |
| テンプレート | false | -2105 | 本文が空白の場合 | The body can not be empty. |
| テンプレート | false | -2106 | 使用有無が間違っている場合 | UseYn is invalid. |
| テンプレート | false | -2107 | 有効ではないテンプレートID(修正/削除時) | Invalid template. |
| テンプレート | false | -2108 | カテゴリーIDが空白の場合 | The categoryId can not be empty. |
| テンプレート | false | -2109 | テンプレートIDが50文字を超える場合 | templateId length must be under 50. |
| テンプレート | false | -2110 | テンプレートが存在しない場合 | template is not exist. |
| テンプレート | false | -2111 | 有効ではないテンプレートパラメータの場合 | template add parameter is invalid. |
| テンプレート | false | -2112 | 最大登録可能なテンプレート数を超過した場合(最大: 1000)  | The maximum number of registered templates. |
| テンプレート | false | -2114 | タイトルが空白の場合 | The title can not be empty. |
| テンプレート | false | -2115 | タイトルが120文字を超える場合 | Title length must be under 120. |
| テンプレート | false | -2116 | 送信タイプがSMSの場合、本文の長さが255文字を超える場合 | SMS Body length must be under 255. |
| テンプレート | false | -2117 | 送信タイプがLMS/MMSの場合、本文の長さが4000文字を超える場合 | LMS/MMS Body length must be under 4000. |
| テンプレート | false | -2043 | テンプレートに登録する添付ファイルが既に他のテンプレートに登録されている場合 | Already used attachFileId |
| カテゴリー | false | -2200 | 有効ではないカテゴリーパラメータ(登録時) | Invalid add category parameter.(categoryName, useYn) |
| カテゴリー | false | -2201 | 有効ではないカテゴリーパラメータ(修正時) | Invalid modify category parameter.(categoryId、categoryName、useYn) |
| カテゴリー | false | -2202 | 有効ではないカテゴリー(カテゴリー照会失敗) | Invalid category. |
| カテゴリー | false | -2203 | 親カテゴリーが存在しない場合 | categoryParentId is invalid. |
| カテゴリー | false | -2204 | 使用の有無が正しくない場合 | UseYn is invalid. |
| カテゴリー | false | -2205 | 最上位カテゴリーを削除しようとしている場合 | Cannot delete the highest category. |
| カテゴリー | false | -2206 | 存在しないカテゴリーの場合 | category is not exist. |
| 発信番号 | false | -2312 | 発信番号が空白または未登録の状態 | Not regist sendno. |
| 発信番号 | false | -2313 | ブロックされた発信番号 | This sendno is blocked. |
| 統計 | false | -2700 | 有効ではない統計範囲 | Invalid search period. |
| 統計 | false | -2701 | 有効ではない統計検索パラメータ | Invalid statistics search parameter. | 
| 統計 | false | -2703 | 有効ではない統計詳細範囲 | Invalid duration time. |
| 統計 | false | -2704 | 有効ではない統計パラメータ | Invalid stats parameter. |
| 080受信拒否 | false | -6000 | 受信拒否機能を使用していない | Block service is not joined. |
| 080受信拒否 | false | -6001 | 受信拒否された番号 | Recipient Number is refused. |
| 080受信拒否 | false | -6003 | 本文に受信拒否案内メッセージがない | The body must contain block guide ment. |
| 080受信拒否 | false | -6004 | 受信拒否番号が使用されていない | This is not a joined unsubscribeNo. |
| タグ | false | -7000 | タグ内部エラー(APi呼び出し失敗) | Fail to call Tag API. |
| タグ | false | -7001 | 有効ではないパラメータ | Invalid parameter. |
| タグ | false | -7002 | .csv読み込み失敗 | Invalid csv read. |

## 受信結果コード

| 区分 | 結果コード | 分類 | 意味 |
| - | - | - | - |
| 移動体通信事業者 | 1000 | 成功 | 成功 |
| 移動体通信事業者 | 1001 | 失敗 | Server Busy |
| 移動体通信事業者 | 1002 | 失敗 | 受信番号形式エラー |
| 移動体通信事業者 | 1003 | 失敗 | 返信番号形式エラー |
| 移動体通信事業者 | 1019 | 失敗 | TTL超過 |
| 移動体通信事業者 | 2000 | 失敗 | 送信タイムアウト |
| 移動体通信事業者 | 2001 | 失敗 | 送信失敗(移動体通信事業者と基地局の間で送信失敗) |
| 移動体通信事業者 | 2002 | 失敗 | 送信失敗(基地局と端末の間で送信失敗) |
| 移動体通信事業者 | 2003 | 失敗 | 端末の電源が切れている |
| 移動体通信事業者 | 2004 | 失敗 | サービスプロバイダーと端末の間のメッセージバッファがいっぱいで転送不可状態 |
| 移動体通信事業者 | 2005 | 失敗 | 電波が届かない地域 |
| 移動体通信事業者 | 2006 | 失敗 | メッセージ削除済み |
| 移動体通信事業者 | 2007 | 失敗 | 一時な端末問の題 |
| 移動体通信事業者 | 3000 | 失敗 | 送信不可 |
| 移動体通信事業者 | 3001 | 失敗 | 加入者なし |
| 移動体通信事業者 | 3002 | 失敗 | 成人認証失敗 |
| 移動体通信事業者 | 3003 | 失敗 | 受信番号形式エラーまたは欠番(ない番号) |
| 移動体通信事業者 | 3004 | 失敗 | 端末サービスの一時停止 |
| 移動体通信事業者 | 3005 | 失敗 | 端末呼処理(Call processing)状態、端末に届かない |
| 移動体通信事業者 | 3006 | 失敗 | 着信拒否 |
| 移動体通信事業者 | 3007 | 失敗 | コールバックURLを受け取れない端末 |
| 移動体通信事業者 | 3008 | 失敗 | その他端末の問題 |
| 移動体通信事業者 | 3009 | 失敗 | メッセージ形式エラー |
| 移動体通信事業者 | 3010 | 失敗 | MMS未サポート端末 |
| 移動体通信事業者 | 3011 | 失敗 | サーバーエラー |
| 移動体通信事業者 | 3012 | 失敗 | スパム |
| 移動体通信事業者 | 3013 | 失敗 | サービス拒否 |
| 移動体通信事業者 | 3014 | 失敗 | その他 |
| 移動体通信事業者 | 3015 | 失敗 | 送信経路なし |
| 移動体通信事業者 | 3016 | 失敗 | 添付ファイルサイズ制限失敗 |
| 移動体通信事業者 | 3017 | 失敗 | 発信番号(=返信番号)改ざん防止サービスによる番号形式エラー |
| 移動体通信事業者 | 3018 | 失敗 | 発信番号(=返信番号)改ざん防止サービスに加入している携帯電話個人加入者番号 |
| 移動体通信事業者 | 3019 | 失敗 | 発信番号(=返信番号) InfoBankに番号事前登録制を通じて登録されていない番号 |
| ETC | E911 | 失敗 | 添付ファイル拡張子がない場合 |
| ETC | E913 | 失敗 | 添付ファイルサイズが0の場合 |
| ETC | E915 | 失敗 | 重複メッセージ |
| ETC | E919 | 失敗 | 送信制限時間の場合、メッセージ再送信処理が禁止されている場合 |
| ETC | E999 | 失敗 | その他のエラー |

## 発信照会コード
### 受信結果コード

| コード値 | 意味 | 
| - | - |
| MTR1 | 成功 | 
| MTR2 | 失敗 | 

### 受信結果詳細コード

| コード値 | 意味 | 
| - | - |
| MTR2_1 | 有効性チェック失敗 | 
| MTR2_2 | サービスプロバイダーの問題 | 
| MTR2_3 | 端末の問題 | 