## Notification > SMS > 結果コード

## API結果コード

| カテゴリー | 成功可否 | 結果コード | 結果コードメッセージ | API応答メッセージ | 
| - | - |-------| - | - |
| 共通 | true | 0     | 成功 | SUCCESS |
| 共通 | false | 4     | パラメータ有効性検証失敗 | | 
| 共通 | false | -1000 | 有効ではないappKey | Invalid appKey. |
| 共通 | false | -1001 | 存在しないappKey | Service is not exist. |
| 共通 | false | -1002 | 使用終了されたappKey | Service is disabled. |
| 共通 | false | -1003 | プロジェクトに含まれていないメンバー | Not project memeber id. |
| 共通 | false | -1004 | 許可されていないIP | Not allow ip. |
| 共通 | false | -1007 | 有効ではないメンバー | MemberType is invalid. |
| 共通 | false | -1008 | ブロックされたプロジェクト | Service is blocked. |
| 共通 | false | -9995 | 有効ではないAPIバージョン | Invalid api version. |
| 共通 | false | -9996 | 有効ではないcontentType. Only application/JSON | Only application/json Content-type is supported. |
| 共通 | false | -9997 | 有効ではないJSON形式 | Invalid API parameters. |
| 共通 | false | -9998 | 存在しないAPI | Not exist API. |
| 共通 | false | -9999 | システムエラー（予期せぬエラー） | System error. Please inquire at support@toast.com. |
| 送信/照会 | false | -1005 | 有効ではない検索条件 | Service parameter is invalid. | 
| 送信/照会 | false | -1006 | 有効ではない送信メッセージ（messageType）タイプ | MessageType is invalid. |
| 送信/照会 | false | -2000 | 有効ではない日付形式 | Date format error. |
| 送信/照会 | false | -2001 | 受信者が空の場合 | RecipientList can not be null. |
| 送信/照会 | false | -2002 | 添付ファイル名が間違っている場合 | Invalid attach file name. |
| 送信/照会 | false | -2003 | 添付ファイル拡張子がjpg、jpeg以外の場合 | Attach file required jpg or jpeg. |
| 送信/照会 | false | -2004 | 添付ファイルが期限切れまたは存在しない場合 | File is expired or does not exist. | 
| 送信/照会 | false | -2005 | 添付ファイルサイズが300KBを超える場合 | The file size must be greater than 0 and less than 300KB. |
| 送信/照会 | false | -2006 | テンプレートに設定された送信タイプとリクエストされた送信タイプが一致しない場合 | Invalid template type. |
| 送信/照会 | false | -2008 | リクエストID（requestId）が間違っている場合 | Invalid requestId. |
| 送信/照会 | false | -2009 | 添付ファイルアップロード中にサーバーエラーにより正常にアップロードされていない場合 | Upload attach file error. | 
| 送信/照会 | false | -2010 | 添付ファイルアップロードタイプが間違っている場合（サーバーエラー） | Upload attach file type can not be empty. |
| 送信/照会 | false | -2011 | 必須照会パラメータが空の場合（requestIdまたはstartRequestDate、endRequestdate） | RequestId or start/endRequestDate or start/endCreateDate is required. | 
| 送信/照会 | false | -2012 | 詳細照会パラメータが間違っている場合（requestIdまたはmtPr） | Search parameter is invalid.(requestId and mtPr). |
| 送信/照会 | false | -2014 | 件名または本文が空の場合 | The recipient can not be empty. |
| 送信/照会 | false | -2015 | 件名または本文が超過した場合 | Title or Body exceed maximum byte. |
| 送信/照会 | false | -2016 | 受信者が1,000名を超えた場合 | The max recipient size is 1000. |
| 送信/照会 | false | -2017 | Excel生成が失敗した場合 | Making Excel file is failed. |
| 送信/照会 | false | -2018 | 受信番号が空の場合 | RecipientNo can not be empty. |
| 送信/照会 | false | -2019 | 受信番号が有効ではない場合 | RecipientNo is invalid. |
| 送信/照会 | false | -2021 | システムエラー（キュー保存失敗） | System error. Failed insert queue. |
| 送信/照会 | false | -2022 | リクエスト日時を現在時間より前に設定した場合 | RequestDate is not before currentDate. |
| 送信/照会 | false | -2023 | 件名または本文に許可されていない文字（絵文字など）が含まれている場合 | Unacceptable characters in title and body. |
| 送信/照会 | false | -2024 | LMS/MMSで国際送信を送信する場合 | LMS/MMS Type is not sent to outside of Korea. |
| 送信/照会 | false | -2044 | 送信不可能な国にリクエストを送った場合 | Invalid countryCode for sending. |
| 送信/照会 | false | -2045 | 国際送信をブロックした場合 | International sending blocked by service. |
| 送信/照会 | false | -2046 | ブロックした国に送信した場合 | Blocked country by service. |
| 送信/照会 | false | -2047 | ブロック制限件数を超えた場合 | Blocked by total indicator. |
| 送信/照会 | false | -2048 | 国際送信本文が超過した場合 | International message body exceed maximum length. |
| 送信/照会 | false | -2050 | 国際送信転換に失敗した場合（転換可能な状態ではない） | Conversion status is not ready. |
| 送信/照会 | false | -2051 | 転換率ベースのブロックにより送信に失敗した場合 | Conversion rate is lower than threshold. |
| 送信/照会 | false | -2052 | 組織当たり月別送信量超過により送信に失敗した場合 | Blocked by organization message sending count exceed. |
| 送信/照会 | false | -2053 | 国別1日送信限度制限により国際送信に失敗した場合 | Blocked by daily country send limit. |
| 送信/照会 | false | -4000 | 照会範囲が1ヶ月を超えた場合 | Search is possible within one month. |
| 送信/照会 | false | -8000 | 認証送信に認証文言が含まれていない場合 | The body must contain auth guide ment. |
| テンプレート | false | -2100 | テンプレートIDが空の場合 | The templateId can not be empty. |
| テンプレート | false | -2101 | すでに登録されたテンプレートID | Already used templateId. |
| テンプレート | false | -2102 | テンプレート名が空の場合 | The template name can not be empty. |
| テンプレート | false | -2103 | 発信番号が空の場合 | The sendNo can not be empty. |
| テンプレート | false | -2104 | 送信タイプが空の場合（0: sms、1: mms） | The sendType can not be empty.(0-sms, 1-mms) |
| テンプレート | false | -2105 | 本文が空の場合 | The body can not be empty. |
| テンプレート | false | -2106 | 使用可否が間違っている場合 | UseYn is invalid. |
| テンプレート | false | -2107 | 有効ではないテンプレートID（修正/削除時） | Invalid template. |
| テンプレート | false | -2108 | カテゴリーIDが空の場合 | The categoryId can not be empty. |
| テンプレート | false | -2109 | テンプレートIDが50文字を超える場合 | TemplateId length must be under 50. |
| テンプレート | false | -2110 | テンプレートが存在しない場合 | Template is not exist. |
| テンプレート | false | -2111 | 有効ではないテンプレートパラメータの場合 | Template add parameter is invalid. |
| テンプレート | false | -2112 | 最大登録可能テンプレート数を超過した場合（最大：1000）  | The maximum number of registered templates. |
| テンプレート | false | -2114 | 件名が空の場合  | The title can not be empty. |
| テンプレート | false | -2115 | 件名が120文字を超える場合  | Title length must be under 120. |
| テンプレート | false | -2116 | 送信タイプがSMSの時に本文長が255文字を超える場合  | SMS Body length must be under 255. |
| テンプレート | false | -2117 | 送信タイプがLMS/MMSの時に本文長が4000文字を超える場合  | LMS/MMS Body length must be under 4000. |
| テンプレート | false | -2043 | テンプレートに登録する添付ファイルがすでに他のテンプレートに登録されている場合 | Already used attachFileId |
| カテゴリー | false | -2200 | 有効ではないカテゴリーパラメータ（登録時） | Invalid add category parameter.(categoryName, useYn) |
| カテゴリー | false | -2201 | 有効ではないカテゴリーパラメータ（修正時） | Invalid modify category parameter.(categoryId, categoryName, useYn) |
| カテゴリー | false | -2202 | 有効ではないカテゴリー（カテゴリー照会失敗） | Invalid category. |
| カテゴリー | false | -2203 | 親カテゴリーが存在しない場合 | CategoryParentId is invalid. |
| カテゴリー | false | -2204 | 使用可否が間違っている場合 | UseYn is invalid. |
| カテゴリー | false | -2205 | 最上位カテゴリーを削除しようとする場合 | Cannot delete the highest category. |
| カテゴリー | false | -2206 | 存在しないカテゴリーの場合 | Category is not exist. |
| 発信番号 | false | -2312 | 発信番号が空または未登録状態 | Not regist sendno. |
| 発信番号 | false | -2313 | ブロックされた発信番号 | This sendno is blocked. |
| 統計 | false | -2700 | 有効ではない統計範囲 | Invalid search period. |
| 統計 | false | -2701 | 有効ではない統計検索パラメータ | Invalid statistics search parameter. | 
| 統計 | false | -2703 | 有効ではない統計詳細範囲 | Invalid duration time. |
| 統計 | false | -2704 | 有効ではない統計パラメータ | Invalid stats parameter. |
| 統計 | false | -2706 | 統計内部エラー（API呼び出し失敗） | Failed read stats. |
| 080受信拒否 | false | -6000 | 受信拒否機能を使用していない | Block service is not joined. |
| 080受信拒否 | false | -6001 | 受信拒否された番号 | Recipient Number is refused. |
| 080受信拒否 | false | -6003 | 本文に受信拒否案内メッセージがない | The body must contain block guide ment. |
| 080受信拒否 | false | -6004 | 受信拒否番号が空または未加入番号 | This is not a joined unsubscribeNo. |
| タグ | false | -7000 | タグ内部エラー（API呼び出し失敗） | Fail to call Tag API. |
| タグ | false | -7001 | 有効ではないパラメータ | Invalid parameter. |
| タグ | false | -7002 | .csv読み込み失敗 | Invalid csv read. |

## 受信結果コード

| 区分 | 結果コード | 分類 | 意味 |
| - | - | - | - |
| 通信事業者 | 1000 | 成功 | 成功 |
| 通信事業者 | 1001 | 失敗 | Server Busy |
| 通信事業者 | 1002 | 失敗 | 受信番号形式エラー |
| 通信事業者 | 1003 | 失敗 | 発信番号形式エラー |
| 通信事業者 | 1019 | 失敗 | TTL超過 |
| 通信事業者 | 2000 | 失敗 | 送信時間超過 |
| 通信事業者 | 2001 | 失敗 | 送信失敗（無線網段） |
| 通信事業者 | 2002 | 失敗 | 送信失敗（無線網 > 端末機段） |
| 通信事業者 | 2003 | 失敗 | 端末機電源オフ |
| 通信事業者 | 2004 | 失敗 | 通信事業者と端末間でメッセージバッファが満杯となり配信不可状態 |
| 通信事業者 | 2005 | 失敗 | 圏外地域 |
| 通信事業者 | 2006 | 失敗 | メッセージ削除 |
| 通信事業者 | 2007 | 失敗 | 一時的な端末問題 |
| 通信事業者 | 3000 | 失敗 | 送信不可 |
| 通信事業者 | 3001 | 失敗 | 加入者なし |
| 通信事業者 | 3002 | 失敗 | 成人認証失敗 |
| 通信事業者 | 3003 | 失敗 | 受信番号形式エラーまたは欠番（存在しない番号） |
| 通信事業者 | 3004 | 失敗 | 端末機サービス一時停止 |
| 通信事業者 | 3005 | 失敗 | 端末機呼処理（Call processing）状態、端末に到達できない  |
| 通信事業者 | 3006 | 失敗 | 着信拒否 |
| 通信事業者 | 3007 | 失敗 | コールバックURLを受信できない端末機 |
| 通信事業者 | 3008 | 失敗 | その他端末機問題 |
| 通信事業者 | 3009 | 失敗 | メッセージ形式エラー |
| 通信事業者 | 3010 | 失敗 | MMS非対応端末 |
| 通信事業者 | 3011 | 失敗 | サーバーエラー |
| 通信事業者 | 3012 | 失敗 | スパム |
| 通信事業者 | 3013 | 失敗 | サービス拒否 |
| 通信事業者 | 3014 | 失敗 | その他 |
| 通信事業者 | 3015 | 失敗 | 送信経路なし |
| 通信事業者 | 3016 | 失敗 | 添付ファイルサイズ制限失敗 |
| 通信事業者 | 3017 | 失敗 | 発信番号偽装防止サービスによる番号形式エラー |
| 通信事業者 | 3018 | 失敗 | 発信番号偽装防止サービスに加入した携帯電話個人加入者番号 |
| 通信事業者 | 3019 | 失敗 | KISAまたは未来部ですべての顧客会社に対してブロック処理した発信番号 |
| 国際送信 | 4001 | 失敗 | シグネチャ形式エラー |
| 国際送信 | 4002 | 失敗 | 発信番号エラー |
| 国際送信 | 4003 | 失敗 | 受信番号エラー |
| 国際送信 | 4004 | 失敗 | 一時的な端末問題 |
| 国際送信 | 4005 | 失敗 | 加入者なし |
| 国際送信 | 4006 | 失敗 | 受信者エラーによる失敗 |
| 国際送信 | 4007 | 失敗 | 通信事業者エラーまたはブロック |
| 国際送信 | 4008 | 失敗 | スパム |
| 国際送信 | 4009 | 失敗 | 一時的ネットワークエラー |
| 国際送信 | 4010 | 失敗 | 異常な送信パターンによる失敗 |
| ETC | E900 | 失敗 | その他送信エラー |
| ETC | E911 | 失敗 | 添付ファイル拡張子がない場合 |
| ETC | E913 | 失敗 | 添付ファイルサイズが0の場合 |
| ETC | E915 | 失敗 | 重複メッセージ |
| ETC | E919 | 失敗 | 送信制限時間の場合、メッセージ再送信処理が禁止された場合 |
| ETC | E999 | 失敗 | その他エラー |

## DLR結果コード
### DLR状態コード
| DLR状態コード | 意味 |
| - | - |
| DELIVERED | メッセージが端末機に送信された状態 |
| ACCEPTED | メッセージが受諾されたがまだ送信されていない状態 |
| BUFFERED | メッセージが受諾され待機中の状態 |
| EXPIRED | 通信事業者の再送信ポリシーによりメッセージ有効期限内で送信失敗 |
| FAILED | メッセージ送信失敗 |
| REJECTED | 通信事業者がメッセージ送信を拒否した状態 |
| UNKNOWN | 不明 |

### DLRエラーコード
| DLRエラーコード | 意味 | 説明 |
| - | - | - |
| 0 | 送信完了 | メッセージが正常に送信完了 |
| 1 | 不明 | メッセージが不明な理由で送信されず |
| 2 | 受信者不在 - 一時的 | 一時的な端末機使用不可状態でメッセージが送信されず - 再試行してください |
| 3 | 受信者不在 - 永久的 | 該当番号はもはや有効状態ではなく、データベースから削除する必要があります |
| 4 | 受信者によりブロックされた番号 | 永続的なエラーで該当番号を必ずデータベースから削除し、通信事業者に問い合わせ後にブロックを解除する必要があります |
| 5 | モビリティエラー | 番号ポータビリティと関連する問題がある場合、通信事業者に問い合わせて解決する必要があります |
| 6 | スパム防止フィルターブロック | メッセージが通信事業者のスパム防止フィルターによりブロック |
| 7 | 端末機使用中 | メッセージ送信時に端末機使用不可状態 - 再試行してください |
| 8 | ネットワークエラー | ネットワークエラーによりメッセージ送信失敗 - 再試行してください |
| 9 | 間違った番号 | 受信者が特定サービスからのメッセージ受信拒否を特別に要求した場合 |
| 11 | ルーティング不可 | NHN Cloudでメッセージ送信のための適切な経路を見つけられず - カスタマーセンターにお問い合わせください |
| 12 | 接続できない宛先 | 該当番号に接続される経路を見つけられず - 受信者番号を確認してください |
| 13 | 受信者年齢制限 | 受信者年齢制限によりメッセージ受信不可 |
| 14 | 通信事業者によりブロックされた番号 | 受信者の料金プランでSMSサービスが利用できるよう通信事業者に問い合わせる必要があります |
| 16 | ゲートウェイ割当量超過 | 期間当たり許可されたリクエスト回数超過によりメッセージ送信失敗。このエラーは米国とフランスに登録されたアカウントのみ該当します |
| 20 | 不正行為防止トラフィックルール | メッセージがトラフィックポンピングにより拒否 - カスタマーセンターにお問い合わせください |
| 21 | 異常連続発信検知 | 高密度受信番号範囲しきい値を超過しました |
| 22 | 異常トラフィック急増検知 | 相対増加しきい値を超過しました |
| 39 | 宛先が米国での間違った発信者アドレス | 発信番号問題により米国へのメッセージ送信失敗 - カスタマーセンターにお問い合わせください |
| 51 | ヘッダーフィルター | 発信番号問題により米国へのメッセージ送信失敗 - カスタマーセンターにお問い合わせください |
| 53 | 同意フィルター | 同意していないためメッセージ送信失敗 |
| 54 | 規制エラー | 予想外の規制関連エラー - カスタマーセンターにお問い合わせください |
| 99 | 一般エラー | 一般的に経路エラーを意味 - カスタマーセンターにお問い合わせください |
| 1000 | その他エラー | その他エラー |

## 結果照会コード
### 受信結果照会コード

| コード値 | 意味 | 
| - | - |
| MTR1 | 成功 | 
| MTR2 | 失敗 | 

### 受信結果照会詳細コード

| コード値 | 意味 | 
| - | - |
| MTR2_1 | 有効性検査失敗 | 
| MTR2_2 | 通信事業者問題 | 
| MTR2_3 | 端末機問題 |