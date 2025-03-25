## Notification > SMS > 結果コード

## API結果コード

| サービス | 成否 | 結果コード | 結果コードメッセージ | APIレスポンスメッセージ | 
| - | - | - | - | - |
| 共通 | true | 0 | 成功 | SUCCESS |
| 共通 | false | 4 | パラメータ検証に失敗しました | |
| 共通 | false | -1000 | 有効ではないアプリケーションキー | Invalid appKey. |
| 共通 | false | -1001 | 存在しないアプリケーションキー | Service is not exist. |
| 共通 | false | -1002 | 使用が終了したアプリケーションキー | Service is disabled. |
| 共通 | false | -1003 | プロジェクトに含まれないメンバー | Not project member id. |
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
| 送信/照会 | false | -2022 | リクエスト日時を現在時間より前に設定した場合 | RequestDate is not before currentDate. |
| 送信/照会 | false | -2023 | タイトルまたは本文に許可されていない文字(Emojiなど)が含まれる場合 | Unacceptable characters in title and body. |
| 送信/照会 | false | -2024 | LMS/MMSで国際送信を送信する場合 | LMS/MMS Type is not sent to outside of Korea. |
| 送信/照会 | false | -2044 | 送信不可能な国にリクエストを送信した場合 | Invalid countryCode for sending. |
| 送信/照会 | false | -2045 | 国際送信をブロックした場合 | International sending blocked by service. |
| 送信/照会 | false | -2046 | ブロックした国に送信した場合 | Blocked country by service. |
| 送信/照会 | false | -2047 | ブロック制限件数を超えた場合 | Blocked by total indicator. |
| 送信/照会 | false | -2048 | 国際送信の本文が最大文字数を超えた場合 | International message body exceed maximum length. |
| 送信/照会 | false | -2050 | 国際発送のコンバージョンに失敗した場合(コンバージョン可能な状態ではありません) | Conversion status is not ready. |
| 送信/照会 | false | -2051 | コンバージョン率に基づくブロックにより送信に失敗した場合 | Conversion rate is lower than threshold. |
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
| テンプレート | false | -2109 | テンプレートIDが50文字を超える場合 | TemplateId length must be under 50. |
| テンプレート | false | -2110 | テンプレートが存在しない場合 | Template is not exist. |
| テンプレート | false | -2111 | 有効ではないテンプレートパラメータの場合 | Template add parameter is invalid. |
| テンプレート | false | -2112 | 最大登録可能なテンプレート数を超過した場合(最大: 1000)  | The maximum number of registered templates. |
| テンプレート | false | -2114 | タイトルが空白の場合 | The title can not be empty. |
| テンプレート | false | -2115 | タイトルが120文字を超える場合 | Title length must be under 120. |
| テンプレート | false | -2116 | 送信タイプがSMSの場合、本文の長さが255文字を超える場合 | SMS Body length must be under 255. |
| テンプレート | false | -2117 | 送信タイプがLMS/MMSの場合、本文の長さが4000文字を超える場合 | LMS/MMS Body length must be under 4000. |
| テンプレート | false | -2043 | テンプレートに登録する添付ファイルが既に他のテンプレートに登録されている場合 | Already used attachFileId |
| カテゴリー | false | -2200 | 有効ではないカテゴリーパラメータ(登録時) | Invalid add category parameter.(categoryName, useYn) |
| カテゴリー | false | -2201 | 有効ではないカテゴリーパラメータ(修正時) | Invalid modify category parameter.(categoryId、categoryName、useYn) |
| カテゴリー | false | -2202 | 有効ではないカテゴリー(カテゴリー照会失敗) | Invalid category. |
| カテゴリー | false | -2203 | 親カテゴリーが存在しない場合 | CategoryParentId is invalid. |
| カテゴリー | false | -2204 | 使用の有無が正しくない場合 | UseYn is invalid. |
| カテゴリー | false | -2205 | 最上位カテゴリーを削除しようとしている場合 | Cannot delete the highest category. |
| カテゴリー | false | -2206 | 存在しないカテゴリーの場合 | Category is not exist. |
| 発信番号 | false | -2312 | 発信番号が空白または未登録の状態 | Not regist sendno. |
| 発信番号 | false | -2313 | ブロックされた発信番号 | This sendno is blocked. |
| 統計 | false | -2700 | 有効ではない統計範囲 | Invalid search period. |
| 統計 | false | -2701 | 有効ではない統計検索パラメータ | Invalid statistics search parameter. | 
| 統計 | false | -2703 | 有効ではない統計詳細範囲 | Invalid duration time. |
| 統計 | false | -2704 | 有効ではない統計パラメータ | Invalid stats parameter. |
| 統計 | false | -2706 | 統計内部エラー(API呼び出し失敗) | Failed read stats. |
| 080受信拒否 | false | -6000 | 受信拒否機能を使用していない | Block service is not joined. |
| 080受信拒否 | false | -6001 | 受信拒否された番号 | Recipient Number is refused. |
| 080受信拒否 | false | -6003 | 本文に受信拒否案内メッセージがない | The body must contain block guide ment. |
| 080受信拒否 | false | -6004 | 受信拒否番号が空白または未加入の番号 | This is not a joined unsubscribeNo. |
| タグ | false | -7000 | タグ内部エラー(API呼び出し失敗) | Fail to call Tag API. |
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
| 移動体通信事業者 | 3017 | 失敗 | 発信番号改ざん防止サービスによる番号形式のエラー |
| 移動体通信事業者 | 3018 | 失敗 | 発信番号改ざん防止サービスに加入している携帯電話個人加入者番号 |
| 移動体通信事業者 | 3019 | 失敗 | KISAまたは未来部ですべての顧客会社に対してブロック処理した発信番号 |
| 国際送信 | 4001 | 失敗 | シグネチャ形式エラー |
| 国際送信 | 4002 | 失敗 | 発信番号エラー |
| 国際送信 | 4003 | 失敗 | 受信番号エラー |
| 国際送信 | 4004 | 失敗 | 一時的な端末の問題 |
| 国際送信 | 4005 | 失敗 | 加入者なし |
| 国際送信 | 4006 | 失敗 | 受信者エラーによる失敗 |
| 国際送信 | 4007 | 失敗 | サービスプロバイダーエラーまたはブロック |
| 国際送信 | 4008 | 失敗 | スパム |
| 国際送信 | 4009 | 失敗 | 一時的なネットワークエラー |
| 国際送信 | 4010 | 失敗 | 異常な送信パターンによる失敗 |
| ETC | E900 | 失敗 | その他送信エラー |
| ETC | E911 | 失敗 | 添付ファイル拡張子がない場合 |
| ETC | E913 | 失敗 | 添付ファイルサイズが0の場合 |
| ETC | E915 | 失敗 | 重複メッセージ |
| ETC | E919 | 失敗 | 送信制限時間の場合、メッセージ再送信処理が禁止されている場合 |
| ETC | E999 | 失敗 | その他のエラー |

## DLR結果コード
### DLRステータスコード
| DLRステータスコード | 意味 |
| - | - |
| DELIVERED | メッセージが端末に送信された状態 |
| ACCEPTED | メッセージが受理されたが、まだ送信されていない状態 |
| BUFFERED | メッセージが受理され、待機している状態 |
| EXPIRED | サービスプロバイダーの再送ポリシーにより、メッセージの有効期限内に送信に失敗 |
| FAILED | メッセージ送信失敗 |
| REJECTED | サービスプロバイダーがメッセージの送信を拒否した状態 |
| UNKNOWN | 不明 |

### DLRエラーコード
| DLRエラーコード | 意味 | 説明 |
| - | - | - |
| 0 | 送信済 | メッセージが正常に送信された |
| 1 | 不明 | メッセージが不明な理由で送信されなかった |
| 2 | 受信者不在 - 一時的 | 一時的な端末使用不可状態でメッセージが送信されない - 再試行してください |
| 3 | 受信者不在 - 永久的 | 該当番号は有効な状態ではなく、データベースから削除する必要がある |
| 4 | 受信者によってブロックされた番号 | 永久的なエラーで、該当番号をデータベースから削除し、サービスプロバイダーに連絡してブロックを解除する必要がある |
| 5 | 移動性エラー | 番号の移動性に関連する問題がある場合は、サービスプロバイダーに連絡して解決する必要がある |
| 6 | スパム防止フィルタブロック | メッセージがサービスプロバイダーのスパム防止フィルタによってブロックされている |
| 7 | 端末使用中 | メッセージ送信時に端末使用不可状態 - 再試行してください |
| 8 | ネットワークエラー | ネットワークエラーによりメッセージの送信に失敗 - 再試行してください |
| 9 | 間違った番号 | 受信者が特定のサービスからメッセージの受信拒否を特別に要求した場合 |
| 11 | ルーティング不可 | NHN Cloudでメッセージ送信のための適切な経路が見つからない - サポートにお問い合わせください |
| 12 | 接続できない宛先 | 該当番号に接続されるルートが見つからない - 受信者番号をご確認ください |
| 13 | 受信者の年齢制限 | 受信者の年齢制限でメッセージ受信不可 |
| 14 | サービスプロバイダーによってブロックされた番号 | 受信者の料金プランでSMSサービスを使用できるようにサービスプロバイダーに問い合わせる必要あり |
| 16 | ゲートウェイ割り当て量超過 | 期間当たりに許可された要求回数を超えたため、メッセージの送信に失敗しました。このエラーは、米国とフランスに登録されたアカウントにのみ適用されます。 |
| 20 | 不正行為防止トラフィックルール | メッセージがトラフィックポンプによって拒否された - サポートにお問い合わせください |
| 21 | 異常な連続発信を検出 | 高密度受信番号範囲のしきい値を超えています |
| 22 | 異常なトラフィックの急増を検出｜相対的な増加のしきい値を超えています |
| 39 | 宛先が米国の誤った発信者アドレス | 発信番号の問題で米国にメッセージ送信失敗 - サポートにお問い合わせください |
| 51 | ヘッダフィルタ | 発信番号の問題で米国にメッセージ送信失敗 - サポートにお問い合わせください |
| 53 | 同意フィルタ | 同意しないため、メッセージ送信失敗 |
| 54 | 規制エラー | 予想せぬ規制関連エラー - サポートにお問い合わせください |
| 99 | 一般エラー | 一般的に経路エラーを意味する - サポートにお問い合わせください |
| 1000 | その他エラー | その他のエラー |

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
