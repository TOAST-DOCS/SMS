## Notification > SMS > サービスポリシー > 国際SMS送信ポリシー

## 国際SMSメッセージ送信案内
+ 国際SMSメッセージを送信する時は、以下の重要事項を確認してください。
### 国別発信番号ポリシー
'+ 国際SMSメッセージは、国別発信番号ポリシーに従って送信され、このポリシーに従わなかった場合、スパムとして処理されることがあります。
'+ 顧客が設定した発信番号は受信端末に表示されることを保障しません。国際SMSメッセージを正常に送信するために、任意の数字や文字、 NHNcorpなどに変更されて送信されることがあります。

### 送信ポリシー
+ 国別詳細ポリシーは[[国別SMS送信詳細ガイド](https://nhnnotification.imweb.me/Technology/?q=YToxOntzOjEyOiJrZXl3b3JkX3R5cGUiO3M6MzoiYWxsIjt9&bmode=view&idx=17226410&t=board)]を参照してください。
+ 中国、ベトナムのように国際 SMSメッセージポリシーが厳格な国の場合、メッセージ送信内容が認証番号(OTP)の場合に限り正常に送信されることがあります。
+ メッセージを正常に送信するために、認証番号(OTP)送信内容を例のように入力することを推奨します。 (例: Your verification code is 00000)
+ マーケティングメッセージの送信を希望する場合、事前に[[サポート](https://www.nhncloud.com/kr/support/inquiry)]にお問い合わせください。
+ 国際SMSメッセージを正常に送信するために、国別ポリシーに従って文言が最大12文字までメッセージに追加されて送信されることがあり、該当文言は課金文字数に含まれます。
'+ 国際SMSは現地サービスプロバイダーに送信され、DLRを返します。
'+ 各DLRはメッセージのステータスと結果コードを提供し、これにより、特定のSMSの送信状態を知ることができますが、正確性を絶対的に保証することはできず、通信会社や端末の状況により、メッセージのステータスと結果コードの値がNULLになることがあります。
+ 海外通信事業者の場合、一般的に7日以内の送信ログのみを保管するため、お問い合わせのタイミングによっては、正確な未受信原因などの確認が難しく、時間が多少かかる場合があります。
+ 各国の送信品質は、当該国のネットワーク及びインフラ環境の影響を受け、国内環境と異なる場合があります。

### 課金ポリシー
+ 国際SMSメッセージ送信費用は海外通信事業者へのデータ転送成否に基づいて課金されます。
+ 国際SMS課金ポリシーは、DLRメッセージの状態やDLR結果コードとは関係ありません。
+ 端末受信結果は海外通信事業者へのデータ転送成功を意味し、実際の端末の受信結果とは異なる場合があります。実際のユーザーがメッセージを受信していなくても課金対象に含まれることがあります。
+ 国際SMSはConcatenated message(接続)機能により、長いメッセージを送信できます。長いメッセージに接続される場合、文字数基準による送信件数で課金されます。
'+ Concatenated message適用時、メッセージを連結するためのヘッダーが処理される過程で送信できる文字数が一部減少します。
'+ 文字数およびConcatenated message基準は国際SMS標準規格に従います。
'+ Concatenated messageが適用されて送信したメッセージの場合でも、通信会社および端末のポリシーにより、長いメッセージではなく、短文メッセージ複数件の形で端末に受信することがあります。
+ メッセージごとの課金数はコンソール詳細照会および詳細照会apiのmessageCountフィールドで確認できます。

| エンコード | 1件課金 | 2件課金 | 3件課金 | 4件課金 | 5件課金 |
| --- | --- | --- | --- | --- | --- |
| UCS-2<br>(Unicode) | 70文字 | 134文字<br>(=67*2) | 201文字<br>(=67*3) | 268文字<br>(=67*4) | 335文字<br>(=67*5) |
| GSM-7bit | 160文字 | 306文字<br>(=153*2) | 459文字<br>(=153*3) | 612文字<br>(=153*4) | 765文字<br>(=153*5) |

## 国際SMSトラフィックポンピング現象

+ SMSトラフィックポンピング現象とは、会員登録番号の認証リクエストのような入力フィールドを利用してOTPメッセージ送信に対するSMSトラフィックをポンピングすることを意味します。一部の海外携帯電話会社(MNO)で売上を増やすために人為的にメッセージの送信を誘導するケースが増加しています。
+ SMSトラフィックポンピング発生時、類似する番号範囲(例：+1111111110、+1111111111、+1111111112、+1111111113など)へ送信されるメッセージが急増しており、認証番号(OTP) SMSを送信する場合、認証が完了していない可能性が高いです。 
+ SMSトラフィックポンピングの発生を防止するために、メッセージを送る可能性がない国コードへの送信を遮断しておくとSMSトラフィックポンピングが発生する可能性が低下します。また、似た番号や同一の番号へのOTP認証リクエストの許容回数とX秒あたりのメッセージ送信速度を適切な水準に制限すれば被害を抑えることができます。


### 送信可能国
| 国名(日本語) | 国名(英語) | 国コード |
|---|---|---|
| アフガニスタン | Afghanistan | 93 |
| アルバニア | Albania | 355 |
| アルジェリア | Algeria | 213 |
| アメリカ領サモア | American Samoa | 1684 |
| アンドラ | Andorra | 376 |
| アンゴラ | Angola | 244 |
| イギリス領アンギラ | Anguilla | 1264 |
| アンティグア・バーブーダ | Antigua & Barbuda | 1268 |
| アルゼンチン | Argentina | 54 |
| アルメニア | Armenia | 374 |
| アルバ | Aruba | 297 |
| オーストラリア | Australia | 61 |
| オーストリア | Austria | 43 |
| アゼルバイジャン | Azerbaijan | 994 |
| バハマ | Bahamas | 1242 |
| バーレーン | Bahrain | 973 |
| バングラデシュ | Bangladesh | 880 |
| バルバドス | Barbados | 1246 |
| ベラルーシ | Belarus | 375 |
| ベルギー | Belgium | 32 |
| ベリーズ | Belize | 501 |
| ベナン | Benin | 229 |
| イギリス領バミューダ | Bermuda | 1441 |
| ブータン | Bhutan | 975 |
| ボリビア | Bolivia | 591 |
| ボスニア・ヘルツェゴビナ | Bosnia and Herzegovina | 387 |
| ボツワナ | Botswana | 267 |
| ブラジル | Brazil | 55 |
| イギリス領ヴァージン諸島 | British Virgin Islands | 1340 |
| ブルネイ | Brunei Darussalam | 673 |
| ブルガリア | Bulgaria | 359 |
| ブルキナファソ | Burkina Faso | 226 |
| ブルンジ | Burundi | 257 |
| カンボジア | Cambodia | 855 |
| カメルーン | Cameroon | 237 |
| カーボベルデ | Cape Verde | 238 |
| イギリス領ケイマン諸島 | Cayman Islands | 1345 |
| 中央アフリカ共和国 | Central African Republic | 236 |
| チャド | Chad | 235 |
| チリ | Chile | 56 |
| 中国 | China | 86 |
| クック諸島 | Cocos Keeling Islands(Cook Islands) | 682 |
| コロンビア | Colombia | 57 |
| コンゴ共和国 | Congo | 242 |
| コスタリカ | Costa Rica | 506 |
| コートジボワール | Côte d'Ivoire | 225 |
| クロアチア | Croatia | 385 |
| キューバ | Cuba | 53 |
| キプロス | Cyprus | 357 |
| チェコ | Czechia | 420 |
| コンゴ民主共和国 | Democratic Republic of the Congo | 243 |
| デンマーク | Denmark | 45 |
| ジブチ | Djibouti | 253 |
| ドミニカ共和国 | Dominica | 1767 |
| ドミニカ共和国1 | Dominican Republic1 | 1809 |
| エクアドル | Ecuador | 593 |
| エジプト | Egypt | 20 |
| エルサルバドル | El Salvador | 503 |
| 赤道ギニア | Equatorial Guinea | 240 |
| エリトリア | Eritrea | 291 |
| エストニア | Estonia | 372 |
| エチオピア | Ethiopia | 251 |
| イギリス領フォークランド諸島 | Falkland Islands | 500 |
| フェロー諸島 | Faroe Islands | 298 |
| フィジー | Fiji | 679 |
| フィンランド | Finland | 358 |
| フランス | France | 33 |
| フランス領ギアナ | French Guiana | 594 |
| フランス領ポリネシア | French Polynesia | 689 |
| ガボン | Gabon | 241 |
| ガンビア | Gambia | 220 |
| ジョージア | Georgia | 995 |
| ドイツ | Germany | 49 |
| ガーナ | Ghana | 233 |
| ジブラルタル | Gibraltar | 350 |
| ギリシャ | Greece | 30 |
| グリーンランド | Greenland | 299 |
| グレナダ | Grenada | 1473 |
| フランス領グアドループ | Guadeloupe | 590 |
| グアム | Guam | 1671 |
| グアテマラ | Guatemala | 502 |
| ギニア | Guinea | 224 |
| ギニアビサウ | Guinea-Bissau | 245 |
| ガイアナ | Guyana | 592 |
| ハイチ | Haiti | 509 |
| ホンジュラス | Honduras | 504 |
| 香港 | Hong-Kong | 852 |
| ハンガリー | Hungary | 36 |
| アイスランド | Iceland | 354 |
| インド | India | 91 |
| インドネシア | Indonesia | 62 |
| イラン | Iran | 98 |
| イラク | Iraq | 964 |
| アイルランド | Ireland | 353 |
| イスラエル | Israel | 972 |
| イタリア | Italy | 39 |
| ジャマイカ | Jamaica | 1876 |
| 日本 | Japan | 81 |
| ヨルダン | Jordan | 962 |
| ケニア | Kenya | 254 |
| キリバス | Kiribati | 686 |
| クウェート | Kuwait | 965 |
| キルギス | Kyrgyzstan | 996 |
| ラオス | Laos | 856 |
| ラトビア | Latvia | 371 |
| レバノン | Lebanon | 961 |
| レソト | Lesotho | 266 |
| リベリア | Liberia | 231 |
| リビア | Libya | 218 |
| リヒテンシュタイン | Liechtenstein | 423 |
| リトアニア | Lithuania | 370 |
| ルクセンブルク | Luxembourg | 352 |
| マカオ | Macau | 853 |
| マダガスカル | Madagascar | 261 |
| マラウイ | Malawi | 265 |
| マレーシア | Malaysia | 60 |
| モルディブ | Maldives | 960 |
| マリ | Mali | 223 |
| マルタ | Malta | 356 |
| マーシャル諸島 | Marshall Islands | 692 |
| フランス領マルティニーク | Martinique | 596 |
| モーリタニア | Mauritania | 222 |
| モーリシャス | Mauritius | 230 |
| マヨット | Mayotte | 269 |
| メキシコ | Mexico | 52 |
| ミクロネシア | Micronesia | 691 |
| モルドバ | Moldova | 373 |
| モナコ | Monaco | 377 |
| モンゴル | Mongolia | 976 |
| モンテネグロ | Montenegro | 382 |
| イギリス領モントセラト | Montserrat | 1664 |
| モロッコ | Morocco | 212 |
| モザンビーク | Mozambique | 258 |
| ミャンマー | Myanmar | 95 |
| ナミビア | Namibia | 264 |
| ナウル | Nauru | 674 |
| オランダ領アンティル | Nederlandse Antillen | 599 |
| ネパール | Nepal | 977 |
| オランダ | Netherlands | 31 |
| ニューカレドニア | New Caledonia | 687 |
| ニュージーランド | New Zealand | 64 |
| ニカラグア | Nicaragua | 505 |
| ニジェール | Niger | 227 |
| ナイジェリア | Nigeria | 234 |
| 北マケドニア | North Macedonia | 389 |
| アメリカ領北マリアナ諸島 | Northern Marianas | 1670 |
| ノルウェー | Norway | 47 |
| オマーン | Oman | 968 |
| パキスタン | Pakistan | 92 |
| パラオ | Palau | 680 |
| パレスチナ | Palestine | 970 |
| パナマ | Panama | 507 |
| パプアニューギニア | Papua New Guinea | 675 |
| パラグアイ | Paraguay | 595 |
| ペルー | Peru | 51 |
| フィリピン | Philippines | 63 |
| ポーランド | Poland | 48 |
| ポルトガル | Portugal | 351 |
| プエルトリコ | Puerto Rico | 1787 |
| カタール | Qatar | 974 |
| フランス領レユニオン | Réunion Island | 262 |
| ルーマニア | Romania | 40 |
| ロシア/カザフスタン | Russia / Kazakhstan | 7 |
| ルワンダ | Rwanda | 250 |
| サントメ・プリンシペ | S. Tomé & Principe | 239 |
| イギリス領セントヘレナ | Saint Helena | 290 |
| サモア | Samoa | 685 |
| サンマリノ | San Marino | 378 |
| サウジアラビア | Saudi Arabia | 966 |
| セネガル | Senegal | 221 |
| セルビア | Serbia | 381 |
| セーシェル | Seychelles | 248 |
| シエラレオネ | Sierra Leone | 232 |
| シンガポール | Singapore | 65 |
| スロバキア | Slovakia | 421 |
| スロベニア | Slovenia | 386 |
| ソロモン諸島 | Solomon Islands | 677 |
| ソマリア | Somalia | 252 |
| 南アフリカ共和国 | South Africa | 27 |
| 南スーダン | South Sudan | 211 |
| スペイン | Spain | 34 |
| スリランカ | Sri Lanka | 94 |
| セントクリストファー・ネイビス | St. Kitts and Nevis | 1869 |
| セントルシア | St. Lucia | 1758 |
| フランス領セントピエールミクロン島 | St. Pierre & Miquelon | 508 |
| セントビンセントおよびグレナディーン諸島 | St. Vincent and the Grenadines | 1784 |
| スーダン | Sudan | 249 |
| スリナム | Suriname | 597 |
| エスワティニ | Swaziland | 268 |
| スウェーデン | Sweden | 46 |
| スイス | Switzerland | 41 |
| シリア | Syria | 963 |
| 台湾 | Taiwan | 886 |
| タジキスタン | Tajikistan | 992 |
| タンザニア | Tanzania | 255 |
| タイ | Thailand | 66 |
| 東ティモール | Timor-Leste | 670 |
| トーゴ | Togo | 228 |
| トンガ | Tonga | 676 |
| トリニダード・トバゴ共和国（Trinidad & Tobago Rep. | Trinidad & Tobago Rep. | 1868 |
| チュニジア | Tunisia | 216 |
| トルコ | Turkey | 90 |
| トルクメニスタン | Turkmenistan | 993 |
| イギリス領カークスカイコス諸島 | Turks & Caicos Is. | 1649 |
| ウガンダ | Uganda | 256 |
| ウクライナ | Ukraine | 380 |
| アラブ首長国連邦 | United Arab Emirates | 971 |
| イギリス | United Kingdom | 44 |
| アメリカ/カナダ | United States / Canada | 1 |
| ウルグアイ | Uruguay | 598 |
| ウズベキスタン | Uzbekistan | 998 |
| バヌアツ | Vanuatu | 678 |
| ベネズエラ | Venezuela | 58 |
| ベトナム | Vietnam | 84 |
| アメリカ領バージン諸島 | Virgin Islands(US) | 1284 |
| フランス領ウォリス・フツナ諸島 | Wallis and Futuna | 681 |
| イエメン | Yemen | 967 |
| ザンビア | Zambia | 260 |
| ジンバブエ | Zimbabwe | 263 |
