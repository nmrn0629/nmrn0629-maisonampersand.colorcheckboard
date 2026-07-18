# チャーム画像差し替え・Codex実装差分 Claude Code同期計画

- 作成日: 2026-07-18
- 対象ブランチ: feature/charm-combo-codex
- 現在の実装基準: 39fe815
- 次の計画作成担当: Claude Code / Fable 5
- 実装担当: Codex
- 状態: Fable 5へ渡す移行計画。既存の plans/charm-combo-display-plan.md を置き換える正本ではない

## 0. 目的と権限

本計画は、Claude Codeが作成した設計と、その後Codex側で行われた画像差し替え・準備ツール・検査実装の差分を、次の正本計画へ漏れなく還流するためのものとする。

役割は次のとおり固定する。

| 役割 | 所有する判断 |
|---|---|
| ユーザー | 採用画像、形状・縁・ロゴ・色の最終承認 |
| Claude Code / Fable 5 | 正本計画、素材台帳、契約、受け入れ基準、例外の更新 |
| Codex | 正本計画に従う実装、検査、実装差分報告 |

Codexは plans/charm-combo-display-plan.md を独自編集しない。実装中に素材・数式・検査・受け入れ条件へ影響する変更が生じた場合は、後述の「Codex変更還流票」を提出して停止する。Fable 5が正本計画へ反映し、Codexレビュー ok:true を取得した後に実装を再開する。

## 1. 現在の状態

### 1-1. Git上の実装

| コミット | 内容 | 計画へ還流する必要 |
|---|---|---|
| cd09451 | photo 7種の派生アセット、丸カン、初版準備ツール | あり |
| 3cf9f1c | 代表チャーム版 charms.html | あり。完成版ではなくフェーズ1試作として記録 |
| cf1653e | 既存計画の改訂13 | 既存正本 |
| ed51bb5 | horseshoe無着色素材、grain、改訂13準備ツール | あり |
| 39fe815 | straight RGBA、独立F oracle、Worker世代管理、自己試験 | あり |

現在のworktreeにはprep.json 7種、horseshoeの派生PNG、manual-alpha、manual-ownerの未コミット変更がある。これらは次の正本計画が確定するまで正式成果物としてコミットしない。.DS_Storeとネストした同名リポジトリは常に除外する。

### 1-2. 現行計画と実装の不一致

現行計画とtools/charm-prep.htmlは、次の入力を正本としている。

| key | 現行入力 | sourceMode / toneMode | 状態 |
|---|---|---|---|
| horse | source-photos/horse.jpg | photo / photo | 差し替え未反映 |
| horseshoe | source-photos/horseshoe-colorless.png | colorless / flat | 差し替え反映済み |
| frenchie | source-photos/frenchie.jpg | photo / photo | 差し替え未反映 |
| dachshund | source-photos/dachshund.jpg | photo / photo | 差し替え未反映 |
| toypoodle | source-photos/toy-poodle.jpg | photo / photo | 差し替え未反映 |
| osanpo | source-photos/osanpo.jpg | photo / photo | 差し替え未反映 |
| cat | source-photos/cat.jpg | photo / photo | 差し替え未反映 |
| swan | source-photos/swan-colorless.png | colorless / flat予定 | SHA未確定、CHARM_SPECS未登録 |

改訂13はphoto 6種を再生成不要としている。一方、39fe815は最終PNGをstraight RGBAとして独立デコードし、最終画素から検査1〜7と検査8を再判定する。旧Canvas出力のphoto 6種はstraight markerを持たず、最終デコード後の検査4も不合格であるため、この二つは両立しない。

次の正本改訂では「photo 6種は再生成不要」を撤回する。採用画像を凍結した後、全8種を新しい生成規則で再生成する。

## 2. Phase 0: Documentation Discovery

このフェーズは本計画作成時点で実施済みである。Fable 5とCodexは実装前に、次の参照箇所を再読する。

### 2-1. 参照資料

| 資料 | 主な参照箇所 |
|---|---|
| plans/charm-combo-display-plan.md | §4-1、§4-1c、§5-2、§5-4、§5-5、§5-6、§7、§8、§9、§12 |
| plans/impl-instructions-charm-combo-display.md | 禁止事項、チェックポイント、検証・報告 |
| tools/charm-prep.html | INPUT_MANIFEST、CHARM_SPECS、PNG処理、F oracle、検査1〜8 |
| charms.html | LAYOUT、BitmapLRU、renderGen、cacheKey、代表チャーム実装 |
| commit 39fe815 | Codexレビューで追加された安全契約 |
| Git管理外カラーチャート5枚 | 既存24色の実物見本＋シェーブル新色3色。Phase 1でSHA凍結 |

現行の実装指示書は改訂8時点の7種・168組・64配置・転記9項目を含むため、そのまま再利用しない。

次の正本は改訂14とし、実装指示書の固定パスを plans/impl-instructions-charm-combo-display-v2.md とする。Fable 5は改訂14計画とv2指示書を同時に作成する。

旧 plans/impl-instructions-charm-combo-display.md の冒頭には次を追加する。

- status: SUPERSEDED — DO NOT EXECUTE
- supersededAt: 2026-07-18
- successorPlan: plans/charm-combo-display-plan.md revision 14
- successorInstructions: plans/impl-instructions-charm-combo-display-v2.md
- lockManifest: plans/charm-combo-revision-lock.json

自己参照するGitコミット番号を同じコミット内へ書くことはできないため、公開は四段階にする。

1. 改訂14計画、v2指示書、旧指示書の失効ヘッダーをdocument commitへ固定する
2. document commitを対象にしたCodexレビュー結果をreview evidence commitへ固定する
3. document／review evidence、3文書SHA-256、数量、改訂番号をplans/charm-combo-revision-lock.jsonへ記録するlock commitを作成する
4. revision-lock-verificationとphase-2-handoffをphase-2 evidence commitへ固定する

Phase 3はロックコミット後だけ開始できる。数量契約は改訂14計画とv2指示書の2正本だけから抽出して照合する。旧指示書は失効メタデータだけを検査し、旧本文に残る改訂8時点の数量は実行契約として解釈しない。2正本とlock manifestの相互参照、8種・2 material profile・27 leatherVariant・216有限値・81配置・CHARMS転記10項目・variant転記8項目・material転記6項目が一致しなければ停止する。

### 2-2. 許可された既存API・実装パターン

新しいAPIを発明せず、次の既存実装を正としてコピー・拡張する。

| 責務 | 既存実装 |
|---|---|
| 入力正本 | tools/charm-prep.html の INPUT_MANIFEST |
| チャーム準備定義 | tools/charm-prep.html の CHARM_SPECS |
| straight PNG生成 | imageDataToBlob、その内部のfilterStraightRgba、pngChunk |
| PNG独立検査 | parsePngRgba、decodeStraightPngBlob |
| operation管理 | beginOperation、assertOperationToken、endOperation |
| production grain | exactMirrorBoxRatioMap |
| 独立F oracle | getFlatOracleResource、auditFlatBakeOracle |
| horseshoe保持検査 | validateHorseshoeManualKeep、validateFinalHorseshoeKeep |
| 内部自己試験 | runInternalSelfTests、selftest=1 |
| 実行時キャッシュ | charms.html の BitmapLRU |
| 実行時世代管理 | charms.html の renderGen |
| 配置の正本 | charms.html と準備ツールの LAYOUT |

### 2-3. 禁止する実装

- index.html、bags.htmlを変更しない
- framework、npm、build toolを導入しない
- Canvas toBlobをbase/masks/manual派生PNGの正規エンコーダーに使わない
- sourceModeとtoneModeを独立に組み合わせない
- 画像差し替え後に旧manualレイヤを座標変換せず流用しない
- CHARMSだけを直接修正しない
- 会話上の「OK」だけで実ファイルを推測しない
- 生成プレビューを、入力素材と同じディレクトリ・同じ役割で管理しない
- 古いJPEGや未承認候補へ黙ってフォールバックしない
- 色調整未承認を形状承認と混同しない

### 2-4. 全Phase共通の成果物・handoff固定契約

別セッションへ渡す成果物は、作業ツリーのpathだけで参照しない。各Phaseは必ず次の二段階で固定する。

1. `phaseOutputCommit`: そのPhaseの正規成果物だけを固定する
2. `phaseEvidenceCommit`: `phaseOutputCommit`を唯一の親とし、許可された検査レポートと`phase-N-handoff.json`だけを固定する

履歴の連続性も成果物契約の一部とする。Phase 2は`documentCommit`を外部選択済み`selectedPhase1EvidenceCommit`の唯一の子として開始し、§6-5のdocument→review evidence→lock→phase-2 evidenceを一本道で作る。Phase 3〜7の`phaseOutputCommit`は、直前Phaseで外部選択された`selectedPhase(N-1)EvidenceCommit`を唯一の親として作る。merge commit、孤立履歴、別branch上の同一byte成果物は不合格とする。

全handoffの共通必須フィールドは`schemaVersion`、`phase`、`planRevision`、`selectedLockCommit`、`phaseInputCommits`、`phaseOutputCommit`、全成果物のpath/byteLength/SHA-256、未解決事項、次Phase開始可否、`handoffPayloadSha256`とする。Phase 1だけはlock作成前なのでselectedLockCommitをnullとし、planRevisionは作成予定の14を記録する。Phase 2以降はnullを禁止する。`handoffPayloadSha256`は当該フィールドを除外し、object keyをUnicodeコードポイント昇順、array順序保持、余分な空白なし、UTF-8、末尾改行なしで直列化したbyte列のSHA-256とする。

次Phaseの開始時は次を全て検証する。

- 選択されたphaseEvidenceCommitがphaseOutputCommitを唯一の親に持つ
- phaseEvidenceCommitの変更pathが当該Phaseで許可されたhandoff・検査証跡だけである
- `git show <phaseEvidenceCommit>:<handoffPath>`から読んだhandoffのpayload SHAが一致する
- `git show <phaseOutputCommit>:<artifactPath>`のbyteLength/SHAがhandoffと一致する
- handoffのselectedLockCommitが現在選択されたlockと一致する
- phaseInputCommitsが直前Phaseで選択されたoutput/evidence commitと一致する
- `git rev-list --parents -n 1`で、当該Phaseの最初のcommitと直前のselected evidenceとの直接親関係、およびphaseOutput→phaseEvidenceの直接親関係がいずれも一親で一致する
- `git merge-base --is-ancestor`でsourceFreezeCommit、selectedLockCommit、直前のselected evidenceが当該phaseOutput／phaseEvidenceの祖先である。Phase 1ではselectedLockCommit検査を行わない

同じphaseOutputCommitを親とするvalidなphaseEvidenceCommitが複数ある場合は自動選択せず停止し、Fable 5またはユーザーが選んだcommitを次Phaseのhandoffへ`selectedPhaseNEvidenceCommit`として記録する。phaseEvidenceCommit自身をhandoffへ自己参照させない。

| Phase | phaseOutputCommit | Phase最初のcommitの唯一の親 | phaseEvidenceCommitで許可するpath |
|---|---|---|---|
| 1 | sourceFreezeCommit | 開始基準commit。phaseInputCommitsへ記録 | phase-1-handoff.json |
| 2 | selectedLockCommit | documentCommitの親=`selectedPhase1EvidenceCommit`。以降は§6-5 | revision-lock-verification.json、phase-2-handoff.json、再改訂時のみsupersession.json |
| 3 | toolCommit | selectedPhase2EvidenceCommit | prep-selftest.json、phase-3-handoff.json |
| 4 | assetCommit | selectedPhase3EvidenceCommit | prep-selftest.json、prep-inspection-1-8.json、phase-4-handoff.json |
| 5 | shapeApprovalCommit | selectedPhase4EvidenceCommit | phase-5-handoff.json |
| 6 | charmsCommit | selectedPhase5EvidenceCommit | runtime-debug.json、layout-81.json、phase-6-handoff.json |
| 7 | colorReviewCommit | selectedPhase6EvidenceCommit | phase-7-handoff.json |

Phase 2のdocument／review／lockの特殊な親子鎖は§6-5を正とし、その後のphase-2 evidenceだけ本共通契約へ従う。

Phase 7だけは後続Phaseがないため、選択されたphase-7 evidenceを自己参照なしで確定する終端記録を§11-4の`finalAcceptanceCommit`へ作る。これは新しいphaseOutput／phaseEvidence対ではなく、外部選択されたphase-7 evidenceを唯一の親とする最終選択記録である。

## 3. 採用素材候補台帳

以下はGit管理外のcharm一覧に存在する候補である。フチ画像はPNGだがalphaを持たない。背景透過画像はalphaを持つ。どちらを正規入力にするかはPhase 1で1点に凍結する。

| key | フチ画像候補 | 寸法 | SHA-256 | 背景透過候補 | 寸法 | SHA-256 |
|---|---|---:|---|---|---:|---|
| horse | horseフチ画像.png | 2158×2233 | ec6ef0cc6a2da23c172b0f3d04a06c4c2e91958babe980ee1908683d7955e6f0 | horse背景透過.png | 1233×1276 | ce7883da7bd6f212bbf62f7428627edf3aea52f9caee08cb2618365fe04f2e03 |
| horseshoe | horseshoeフチ画像.png | 2020×2385 | 111196581550f80d8d1edb6a4f929af9e7bdb3e239c1680bbc6459c53bf9341c | horseshoe背景透過.png | 2020×2385 | c062e95d747a6ee8c0521f67be208b629dede5b6aa60f985ef9443aa4525271b |
| frenchie | frenchieフチ画像.png | 1808×2664 | 05c08873322c11f6ed48ef2b6e272f2582bd6f597fd2ced874ef15d84b9d6edf | frenchie背景透過.png | 1808×2664 | 168c760a708066298f53075f278240ddfa9f1eb822624c2da02af86c3ee2c5bf |
| dachshund | dachshundフチ画像.png | 2195×2195 | e0e228f792b69eae074695ef020c9af1c2ad34306cf05fb735e7b131d17487da | dachshund背景透過.png | 2195×2195 | 55556311336047a828cc1327962c72ac6b4fd11d584a2f2fcacccd44958a7da9 |
| toypoodle | toy poodleフチ画像.png | 2176×2176 | b4251b764c0ca3f35af6d6f0904b52c909811bc44637dc64b12d8ccdf64bfcba | toy poodle背景透過.png | 2176×2176 | 35f772667b36b6f712f5e122c7322a89678f92763edaa44ff4a4548be67c700d |
| osanpo | osanpoフチ画像.png | 2304×2304 | f9153732573aac77090914c5abf08fb98c5456e8a345cd098e74d23a48cef28e | osanpo背景透過.png | 2304×2304 | 5e790856fe9f8792e53ae14862c2a53c51ce0dd9a9b0e82bcba311ba72d18910 |
| cat | catフチ画像.png | 2195×2195 | 563b5c975eaebbafa3fbb25a1720d0c5021022dca23cb2f069d46553be10bce7 | cat背景透過.png | 2195×2195 | c5e74c805fbf8c9036dbf13f3f3fad1e0f40523df0f817324606f62fd11c085f |
| swan | swanフチ画像.png | 1662×2498 | 847e75bbd688269dfbdd9d72560f0253021b65717ffc750718d6fff5f5d28de7 | swan背景透過.png | 1662×2498 | 27515ba2263fb75dd965b127f680f4f9e867c149824d2728b86bd7c88343012b |

horseshoeフチ画像.pngと現在のsource-photos/horseshoe-colorless.pngは同じSHAである。

### 3-1. カラーチャート候補台帳（2026-07-18受領）

Git管理外の`nmrn0629-maisonampersand.colorcheckboard/カラーチャート/`に次の5枚がある。ファイル名はmacOS上でUnicode分解形を含み得るため、取り込み時は表示名だけでなくSHA-256で同定する。

| ファイル | 寸法 | byteLength | SHA-256 | 読み取れる色名 |
|---|---:|---:|---|---|
| レッド系.jpg | 1080×1072 | 104084 | d11a010b7a33db961882ff387dfb3b8a57c288920cfb5cb53da2e3c89007a696 | ピンク、レッド、ローズピンク、パープル、ペールピンク、コーラルピンク |
| イエロー系.jpg | 1080×1080 | 103192 | 4138b08db778624bfaf3bfb71f55115c64ab8016d78974cfacfc8c102e621165 | ゴールド、オレンジ、ライムイエロー、ペールイエロー、ブリック、ピスタチオ |
| ブラウン系.jpg | 1079×1079 | 96180 | f391a93bf5ecca9410b46a1edc41cec71c80a58ccfa24cc03fe81fe65d7e1348 | ブラック、グレージュ、ブラウン、チャコールグレー、アイボリー、エトープ |
| ブルー系.jpg | 1076×1076 | 110808 | 0ae678f0cad086ce4215f5e2e78d541c060f0be9806b75cb003808483a1cae12 | ライトブルー、シエルブルー、ミストブルー、アイスグレー、ロイヤルブルー、ネイビー |
| ニューカラー（シェーブル）.jpg | 1080×1081 | 696422 | 36195408593c446ad99b5c0f0bdc43282ed07db634703dd3cdcf6fd8c185c930 | ペールグレージュ、グリーン、ラベンダー |

既存24色にシェーブル（ヤギ革）3色が追加されたため、候補は計27の`leatherVariant`となる。単純な27色配列にはせず、各variantを`materialKey + colorKey`で識別する。シェーブル3色のmaterialKeyは`chevre`とする。既存24色の革種名、シェーブル専用の革質感を表示へ反映するか、左右それぞれで革種を選べるかは正本化前のユーザー確認事項であり、推測で確定しない。

画像表記は「シエルブルー」、現行COLORSは「シェルブルー」で不一致がある。別色として増やさず、ユーザーが確定したcanonicalLabelと旧aliasを色凍結台帳に記録するまで実装を進めない。

JPEGは照明・陰影・圧縮・文字を含むため、画面表示用hexを単純な全画像平均や一点サンプルで決めない。各パネルの採用ROI、統計方式、提案hex、現行hexとの差分比較シートを作り、ユーザー承認後にのみ色正本へ固定する。

## 4. 会話上の承認状態

この表は見た目の承認を記録するものであり、正規入力ファイルの確定を意味しない。

| key | 最新の承認内容 | 未確定事項 |
|---|---|---|
| horse | horseフチ画像の縦横比と形状を維持。黒い外縁は革色へなじませる。形状は一旦承認 | 最終入力と最終承認プレビューの1対1対応。色は後日 |
| horseshoe | 外形、吊り穴、アンパサンド内部、アイボリー見本を承認。ロゴはチャーム角度に合わせる | ペール／ライム系は未承認。最終入力・承認見本の凍結 |
| frenchie | 透過素材を用いた形状見本を承認 | 最終入力の選択。色は後日 |
| dachshund | dachshundフチ画像準拠の最初の生成版を採用 | 採用プレビューの正本コピーとSHA |
| toypoodle | 透過素材を用いた形状見本を承認 | 最終入力の選択。色は後日 |
| osanpo | 透過素材を用いた形状見本を承認 | 最終入力の選択。色は後日 |
| cat | catフチ画像の線に忠実な見本を承認 | 採用プレビューの正本コピーとSHA。色は後日 |
| swan | 形状と革質感を承認。ロゴ外周の黒線を除去し、箔押しを平面化。元画像のロゴ位置・大きさ・傾きを維持 | 最終承認プレビューの正本コピーとSHA、くちばし・ストーン・ロゴ所有分類 |

全チャームの色は、既存24色＋シェーブル3色の27 leatherVariantを最終出力してクライアント確認を受ける後工程へ送る。形状・比率・意匠承認と色承認を別ゲートにする。

## 5. Phase 1: 採用素材と承認見本の凍結

### 5-0. 開始時参照と固定入力

新しいセッションは次を読む。

- 本同期計画
- plans/charm-combo-display-plan.md revision 13
- Git管理外候補ルートの絶対パス
  - /Users/ah266/nmrn0629-maisonampersand.colorcheckboard/nmrn0629-maisonampersand.colorcheckboard/charm一覧/
- Git管理外カラーチャートルートの絶対パス
  - /Users/ah266/nmrn0629-maisonampersand.colorcheckboard/nmrn0629-maisonampersand.colorcheckboard/カラーチャート/
- 採用素材候補台帳
- カラーチャート候補台帳
- 会話上の承認状態

開始時に両候補ルートを再走査する。チャーム候補はファイル名、寸法、alpha有無、byteLength、SHA-256をreports/charm-combo/rev14/source-candidates.jsonへ、色見本はファイル名、寸法、byteLength、SHA-256、読み取ったパネル名をreports/charm-combo/rev14/color-candidates.jsonへ記録する。各JSONのSHAを候補台帳SHAとして固定する。

### 5-1. 実施内容

8種それぞれについて次の2ファイルを1点ずつ選ぶ。

1. pipelineSource: 派生アセット生成へ入力する画像
2. approvalReference: 形状・縁・ロゴ・保持装飾を照合する承認済み見本

同一ファイルでもよいが、役割は台帳上で分離する。生成比較シートをpipelineSourceにしない。

Fable 5は正本計画へ次の列を持つ採用素材台帳を追加する。

| field | 内容 |
|---|---|
| key | charm key |
| pipelineSourceOriginal | charm一覧内の元ファイル |
| canonicalSource | Git管理下のsource-photos名 |
| sourceWidth / sourceHeight | デコード寸法 |
| sourceSha256 | ファイルSHA |
| hasAlpha | alpha有無 |
| sourceMode / toneMode | photo/photo または colorless/flat |
| approvalReferenceOriginal | 承認見本の元パス |
| canonicalReference | Git管理下のreference名 |
| referenceSha256 | 承認見本SHA |
| approvalScope | shape / edge / logo / decoration / color |
| supersededSources | 生成入力禁止の旧ファイル |
| approvalEvidenceId | ユーザー再確認で発行する安定ID |
| approvalEvidenceText | 承認範囲の短い引用ではなく要約 |
| approvalStatus | selected / pending / rejected |

色については別の`leatherVariant`台帳を作る。既存の`COLORS`を直接書き換えず、次を確定する。

1. 5枚のchartをGit管理下`reference-visuals/colors/`へSHA同一でコピーする
2. 27 variantを`variantKey = materialKey + ":" + colorKey`で一意化する
3. 既存24色のmaterialKey/表示名をユーザー確認する
4. シェーブル3色はmaterialKey=`chevre`、表示名=`シェーブル`、色名はペールグレージュ／グリーン／ラベンダーとする
5. 「シエルブルー」／「シェルブルー」のcanonicalLabelとaliasをユーザー確認する
6. パネルごとのexact ROI、色抽出法、提案hex、採用hex、承認Evidence IDを固定する
7. materialごとのtextureProfileKey、grain source/ROI/SHA、既存grain再利用可否を固定する
8. 27 variantが全8チャームへ適用可能かを確定する。photo/photo素材でシェーブル質感を再現できない場合は、全素材のcolorless/flat化または適用対象制限を正本へ記録するまで停止する。適用対象を制限する場合はfiniteCasesとクライアント確認件数を`Σ variants.appliesToCharms.length`で再計算し、216を置換してCodexレビューをやり直す

### 5-2. 正規配置

推奨するGit管理名は次のとおり。

- source-photos/horse-colorless.png
- source-photos/horseshoe-colorless.png
- source-photos/frenchie-colorless.png
- source-photos/dachshund-colorless.png
- source-photos/toy-poodle-colorless.png
- source-photos/osanpo-colorless.png
- source-photos/cat-colorless.png
- source-photos/swan-colorless.png
- reference-visuals/charms/horse-approved.png
- reference-visuals/charms/horseshoe-approved.png
- reference-visuals/charms/frenchie-approved.png
- reference-visuals/charms/dachshund-approved.png
- reference-visuals/charms/toy-poodle-approved.png
- reference-visuals/charms/osanpo-approved.png
- reference-visuals/charms/cat-approved.png
- reference-visuals/charms/swan-approved.png

上記名称は全8種をcolorless/flatへ移行する場合の案である。画像の色・明暗を写真として利用する素材は、Fable 5がphoto/photoを選び、colorlessという名前を付けない。

### 5-3. 検証

- canonicalSourceと元候補のSHAが一致する
- canonicalReferenceと承認済み見本のSHAが一致する
- 8種すべてにsourceとreferenceが1点だけある
- sourceModeとtoneModeが許可された組だけである
- 旧入力がsupersededSourcesへ明記される
- ユーザーが採用素材台帳を承認する
- selected項目すべてにapprovalEvidenceIdとreference SHAがある
- source freeze JSONをcanonical JSONとして再出力してSHAが一致する
- color freeze JSONが5 chart・27 variant・chevre 3色・216有限値を一意に定義する
- materialKey、approvedHex、textureProfileKey、appliesToCharmsが全variantで確定する

Phase 1の機械PASS式は次の論理積だけとする。

`entries.length === 8` ∧ keyが8種のenumと一対一 ∧ 全entryのapprovalStatusが`selected` ∧ 全approvalEvidenceIdが非空 ∧ pipelineSource/approvalReferenceの全必須フィールドが妥当 ∧ sourceMode/toneModeが許可pair ∧ pendingCount=0 ∧ rejectedCount=0 ∧ `git show <sourceFreezeCommit>:<canonicalPath>`で得た生byte列のSHA-256が台帳と一致。

`canStartPhase2`はこの論理積から自動算出し、手入力を禁止する。一項目でも偽ならfalseとする。

### 5-4. 停止条件

次の場合はPhase 2へ進まない。

- 会話上の承認と実ファイルが一意に対応しない
- 最新指示と古い指示の優先順が決められない
- pipelineSourceとapprovalReferenceが混同されている
- swanのくちばし、ストーン、ロゴ所有が決まらない
- 色承認待ちを形状不承認として扱っている
- approvalStatusがselected以外、pendingCountまたはrejectedCountが1以上
- 既存24色のmaterial表示名、シェーブル質感方針、3新色のhexまたは適用チャームが未確定

### 5-5. 成果物と引渡し

- Git管理下へコピーしたcanonicalSource 8点
- Git管理下へコピーしたcanonicalReference 8点
- plans/charm-approved-source-freeze.json
- reports/charm-combo/rev14/source-candidates.json
- Git管理下へコピーしたcanonicalColorChart 5点
- color freezeがdistinctと定めたcanonicalMaterialGrain
- ROI方式variantのcanonicalSamplingMask
- plans/charm-approved-color-freeze.json
- reports/charm-combo/rev14/color-candidates.json
- reports/charm-combo/rev14/phase-1-handoff.json
- source freeze commit
- phase-1 evidence commit

charm-approved-source-freeze.jsonはschemaVersion 1とし、必須フィールドは`schemaVersion`（integer 1）、`generatedAt`（ISO 8601 string）、`candidateInventory`、`entries`、`pendingCount`、`rejectedCount`、`canStartPhase2`である。unknown fieldは禁止する。

`candidateInventory`は`path`、`byteLength`、`sha256`を持つ。entriesは固定`keyOrder=["horse","horseshoe","frenchie","dachshund","toypoodle","osanpo","cat","swan"]`の順にちょうど8件並べ、各keyを重複なく1回ずつ持つ。この列挙は辞書順ではなく正規配列順である。各entryの必須フィールドは`key`、`pipelineSource`、`approvalReference`、`sourceMode`、`toneMode`、`approvalScope`、`approvalEvidenceId`、`approvalEvidenceText`、`approvalStatus`、`supersededSources`である。freeze JSON内のapprovalStatusは`selected`だけを許可し、pending/rejectedはsource-candidates.jsonだけに記録する。

pipelineSourceとapprovalReferenceは共通して次の必須フィールドを持つ。

| field | 型・制約 |
|---|---|
| originalPath | string。実在する通常ファイルの正規化済み絶対pathでsymlinkではない |
| canonicalPath | POSIX相対path。絶対path、空要素、`.`、`..`、symlink禁止 |
| width / height | 1以上のinteger |
| byteLength | 1以上のinteger |
| sha256 | lowercase hex 64文字 |
| hasAlpha | boolean |

pipelineSource.originalPathは固定候補ルート配下だけを許可する。approvalReference.originalPathは、固定候補ルート、`/Users/ah266/Downloads/`、`/Users/ah266/.codex/generated_images/`のいずれかの配下に限定する。prefix文字列比較ではなくrealpath包含判定を行い、許可ルート外ならユーザー確認と計画改訂まで停止する。

pipelineSource.canonicalPathは`source-photos/`配下、approvalReference.canonicalPathは`reference-visuals/charms/`配下に限定する。sourceModeは`photo|colorless`、toneModeは`photo|flat`で、許可pairは`photo/photo`と`colorless/flat`だけとする。approvalEvidenceIdは空白以外を含むstring、approvalScopeとsupersededSourcesは重複のないarrayとする。

freeze JSONのcanonical化は`entries`を上記keyOrder、object keyをUnicodeコードポイント昇順、arrayは明記された順序を保持、余分な空白なし、UTF-8、末尾改行なしとする。pendingCount/rejectedCountはfreezeの8 entriesから再計算し、いずれも0で全entry selectedの場合だけcanStartPhase2=trueとする。source-candidates.json内の不採用候補はこの件数へ算入しない。

charm-approved-color-freeze.jsonはschemaVersion 1とし、`generatedAt`、`chartInventory`（5件）、`materials`（2件）、`variants`（27件）、`aliases`、`pendingCount`、`canStartPhase2`を必須とする。各chartはoriginalPath、canonicalPath、width、height、byteLength、sha256、`decoder={name,version,options,decodedRgbaSha256}`、`panels`を持つ。optionsは実際に使用したimageOrientation、colorSpaceConversion、premultiplyAlphaと、デコーダー固有の全指定値をunknownなしのcanonical objectで記録する。各materialはmaterialKey、canonicalLabel、animalType、textureProfileKey、grainPolicy=`distinct|approved-reuse`、grainSource、approvalEvidenceIdを持つ。

各chartのpanelsは`panelIndex`、`canonicalLabel`、`panelRect`、`approvalEvidenceId`を持つ。panelIndexはchart内で0から始まる連続integerかつ重複禁止、panelRectはx/yが0以上・w/hが1以上でchart内に完全に収める。5 chartのpanel件数は6+6+6+6+3=27で、全canonicalLabelを27 variantへ一対一対応させる。パネル境界とlabelの対応は比較シートでユーザー承認し、approvalEvidenceIdなしではfreezeしない。

grainSourceは次を必須とする。

- inputPath、inputWidth/inputHeight、inputByteLength、inputSha256
- roi={x,y,w,h}。x/yは0以上、w/hは1以上のintegerで画像内に完全に収める。approved-reuseでも採用領域をfull-frame ROIとして明記する
- decoder={name,version,options,decodedRgbaSha256}。optionsはchart decoderと同じ規則
- grainSamplingMethod=`jpeg-roi-luma-straight-v1|approved-existing-asset-v1`
- extractionParams、outputPath、outputWidth/outputHeight、outputByteLength、outputSha256
- comparisonEvidence={path,byteLength,sha256,approvalEvidenceId}

`jpeg-roi-luma-straight-v1`は、固定decoder/optionsで入力全体を8-bit RGBAへデコードしてdecodedRgbaSha256を照合し、整数ROIを無補間で切り出し、各画素を`Math.round(0.2126*R + 0.7152*G + 0.0722*B)`の同一値RGB・A=255へ変換し、§6-2のstraight RGBA encoderで無拡縮PNGへ書き出す。extractionParamsには係数、rounding=`Math.round`、resample=`none`、alpha=`255`、encoder marker/toolVersionを記録する。`approved-existing-asset-v1`は既存canonical grainをbyte同一で使い、入力/output SHA一致と明示承認を必須とする。

comparisonEvidenceは同一チャーム・同一approvedHexで既存materialと対象materialの質感だけを比較するシートとし、ユーザーがgrainの差または再利用を承認したEvidence IDを持つ。input JPEGだけ、ROIだけ、または比較未承認のoutputを正本grainにしない。

各variantはvariantKey、materialKey、colorKey、canonicalLabel、aliases、chartSha256、panelIndex、panelRect、samplingRect、samplingMethod、samplingEvidence、proposedHex、approvedHex、approvalEvidenceId、appliesToCharms（8keyの部分集合）、approvalStatusを持つ。rectのx/yは0以上、w/hは1以上のintegerで画像内に完全に収め、`samplingRect.x/y >= panelRect.x/y`かつ`samplingRect.x+w <= panelRect.x+w`かつ`samplingRect.y+h <= panelRect.y+h`、すなわちsamplingRectをpanelRectへ完全包含させる。variantのchartSha256、panelIndex、canonicalLabel、panelRectは対応するchart.panelsの1 entryと完全一致させる。hexはlowercase `#[0-9a-f]{6}`、approvalStatusはfreeze内ではselectedだけを許可する。variantKey、materialKey+colorKey、canonicalLabelはそれぞれ重複禁止とする。

samplingMethodは`approved-explicit-hex`または`roi-lab-lower-median-v1`だけを許可する。samplingEvidenceは`decoderRgbaSha256`、`exclusionMask`、`sampledPixelSetSha256`、`acceptedPixelCount`、`recomputedProposedHex`を必須とする。前者はユーザーが指定・承認したhexとEvidence IDを記録し、decoderRgbaSha256をchartのdecoderと一致させ、後ろ4フィールドを順にnull、null、0、nullとする。

`roi-lab-lower-median-v1`はsamplingRectから文字、区切り線、強い反射・影を除外したROIをユーザー承認する。exclusionMaskは`canonicalPath`、`byteLength`、`sha256`を持つ。canonicalPathは`reference-visuals/colors/sampling-masks/`配下のPOSIX相対pathに限定し、絶対path、`.`、`..`、symlinkを禁止する。mask本体はsamplingRect内のrow-major 1 byte/画素（0=採用、1=除外）で、byteLength=`w*h`、全byteが0または1でなければならない。mask本体をsourceFreezeCommitへ固定する。

独立再計算では最初にchartSha256、panelIndex、canonicalLabel、panelRectがchart.panelsと完全一致し、samplingRectがpanelRectへ完全包含されることを検証する。その後、固定chart decoderで得たstraight RGBAからmask byte=0の画素を同じrow-major順で連結してsampledPixelSetを再構築し、そのSHAをsampledPixelSetSha256へ記録する。acceptedPixelCountはmask内の0 byte数と一致し、1以上を必須とする。chartのdecodedRgbaSha256を照合し、再構築した採用画素を既存flatLeatherToneと同じD65 Lab変換へ通し、L/a/b各成分の下位中央値`floor((n-1)/2)`を採り、同じsRGB色域圧縮・丸め規則でrecomputedProposedHexへ戻す。recomputedProposedHexとproposedHexの完全一致を必須とする。proposedHexは自動採用せず、approvedHexは比較シート承認後にだけ設定する。

本計画の216件契約を採用する場合の色freeze機械PASS式は、chartInventory=5件かつ全file SHA・decoder/version/options・decoded RGBA SHA一致 ∧ chart panels=6+6+6+6+3件でindex連続・label/rect/Evidence確定 ∧ materials=2件 ∧ variants=27件 ∧ 各variantのchartSha256/panelIndex/label/panelRectがchart.panelsと一対一一致 ∧ samplingRect⊆panelRect ∧ 既存materialの24件＋chevreの3件 ∧ 全status selected ∧ 全approvedHex/Evidence ID非空 ∧ label alias衝突なし ∧ 全rectが範囲内かつw/h>0 ∧ ROI方式はmask artifactのpath/byteLength/SHA一致・長さ=`w*h`・全byte∈{0,1}・maskから再構築した採用RGBA SHA一致・acceptedPixelCount>0・下位中央値からのrecomputedProposedHex=proposedHex ∧ explicit方式はconditional null/0規則一致 ∧ 全materialのgrainPolicy、decoder、decoded RGBA SHA、samplingMethod、extractionParams、output SHA、comparisonEvidence確定 ∧ 全variantのappliesToCharmsが8key全件と一致 ∧ pendingCount=0、とする。`canStartPhase2`はこの論理積から自動導出する。適用制限を採用した改訂では、件数式とPASS式を同時に更新する。

色freezeも§2-4と同じcanonical JSON規則を使い、variantsはvariantKey昇順、materialsはmaterialKey昇順、chartInventoryはcanonicalPath昇順に固定する。

二段階の固定順は次のとおり。

1. canonicalSource 8点、canonicalReference 8点、canonicalColorChart 5点、2つのfreeze JSON、2つのcandidate JSON、全canonicalSamplingMask、およびcolor freezeがdistinctと定めたcanonicalMaterialGrainだけをsourceFreezeCommitへ固定する
2. `git show <sourceFreezeCommit>:<path>`で25点のcore成果物＋全canonicalSamplingMask＋全canonicalMaterialGrainのbyteLength/SHAを再照合し、chart.panels対応とsamplingRect⊆panelRectを検証してから、各maskから採用RGBA、acceptedPixelCount、sampledPixelSetSha256、recomputedProposedHexを独立再計算する
3. phase-1-handoffを作成し、§2-4のpayload SHAを記録する
4. phase-1-handoffだけを、sourceFreezeCommitを唯一の親とするphase-1 evidence commitへ固定する

phase-1-handoffにはphaseOutputCommit=`sourceFreezeCommit`、source/color freeze JSON SHA、source/color候補台帳SHA、8点のsource/reference SHA、5点のchart file/decoder/options/decoded RGBA SHA、chart.panels台帳SHAと27件の一対一対応・samplingRect包含検査、27 variantのsamplingMethod／samplingEvidence、全canonicalSamplingMaskのpath/byteLength/SHAと再計算結果、materialProfileCount=2、全grain input/decoded/output/comparison SHA、charmCount=8、leatherVariantCount=27、finiteCases=216、双方のpendingCount=0、canStartPhase2=trueを記録する。最終canStartPhase2はsource freezeとcolor freezeの両方がtrueの場合だけtrueとする。Phase 2は選択されたphase-1 evidence commit、handoff payload SHA、sourceFreezeCommit、全canonical file SHAと双方のPASS式を`git show`で照合してから開始する。

### 5-6. アンチパターン

- Git管理外の絶対パスだけをhandoffへ残さない
- approvalEvidenceIdのないselectedを作らない
- pending候補を正本計画へ確定値として転記しない
- referenceだけ承認してpipelineSourceを推測しない
- source freeze commit後に同じファイル名の内容を差し替えない

## 6. Phase 2: Fable 5による正本計画改訂

### 6-0. 開始時参照と固定入力

新しいセッションは次を読む。

- 本同期計画
- plans/charm-approved-source-freeze.json
- plans/charm-approved-color-freeze.json
- reports/charm-combo/rev14/phase-1-handoff.json
- selected phase-1 evidence commit
- sourceFreezeCommit
- plans/charm-combo-display-plan.md revision 13
- plans/impl-instructions-charm-combo-display.md
- commit 39fe815のtools/charm-prep.html

source/color freeze JSON、handoff、Git上のcanonical filesを独立にSHA照合する。selected phase-1 evidence commitがsourceFreezeCommitを唯一の親に持たない、許可path以外を変更する、handoff payload SHA、chart SHA、27 variantまたは成果物SHAが一致しない場合は停止する。

### 6-1. 実施内容

Fable 5はplans/charm-combo-display-plan.mdを次の改訂へ更新する。

1. §4-1、§4-1b、§4-1cを採用素材台帳へ合わせる
2. INPUT_MANIFESTの全8種ファイル、SHA、modeを確定する
3. swanを「未受領」から「候補受領済み／正本確定済み」の正しい状態へ更新する
4. photo 6種の再生成不要条項を撤回する
5. 全8種の再生成と旧成果物混在禁止を契約化する
6. schemaVersionとtoolVersionを増分する
7. straight RGBA PNGと独立最終デコードを§5-4へ追加する
8. 独立F oracleとmutation試験を§5-4へ追加する
9. horseshoeの6ストーン位相検査を検査6へ追加する
10. production／oracle Workerの世代、中断、timeout、cleanupを契約化する
11. Codex実装還流台帳と恒常的な同期手順を追加する
12. 古い実装指示書を更新または失効させる
13. 改訂12/13の二段階アスペクト補正と再defringeを新版へ移植する
14. 5枚のcolor chartとcolor freezeを色正本として追加する
15. `COLORS`をmaterial-awareな27 leatherVariantへ移行し、既存24＋chevre 3の有効組だけを定義する
16. 有限値・色回帰・クライアント確認を216件へ更新する
17. シェーブル専用textureProfile、asset/runtime適用方式、UI、cache keyを契約化する

### 6-2. Codex実装から還流する技術契約

#### straight RGBA PNG

- base、masks、manual派生PNGはstraight RGBAエンコーダーで生成する
- 8-bit RGBA、non-interlaceとする
- CRC、IHDR、IDAT順序、filter、寸法、展開量を検査する
- toolVersion付きmarkerを記録する
- 最終PNGを独立デコードして生成ImageDataと完全一致させる
- 派生値、検査1〜7、M/F、期待ハッシュ、byteLengthは最終デコード画素を正とする

#### 独立F oracle

- raw grain bytesのSHAを再照合する
- production helperを再利用せず独立デコードする
- Float64を使う
- mirrorを独立実装する
- 外側dy、内側dxの順で逐次加算する
- shade、u、v、scaleから全革画素の期待grayを算出する
- 非革画素が不変であることも全列挙する

#### mutation試験

- Float32化
- mirror誤り
- dx外／dy内へのloop-order変更
- grain map全1
- 非革画素変更

各変異はproduction相当のafterBakeを生成し、独立oracleへ通して不一致を検出する。正常出力は不一致0とする。

#### horseshoe保持領域

- work manual-keepは8近傍でちょうど6成分
- work座標のunion areaは32000から34000
- aspect補正後はcenter-nearest投影した期待bitsetと最終masksを完全一致させる
- missing 0、extra 0
- 期待bitsetと実bitsetの双方が6成分
- asset座標へwork座標の固定面積閾値を直接適用しない

#### 二段階アスペクト補正と再defringe

- ④の初回defringeと⑧'-2の再defringeは同一関数・同一最近傍規則を使う
- ⑧'-1で幾何補正を行う
- photoのbase.RGBはbilinear、colorlessのbase.RGBとmasksは計画のnearest規則を使う
- 補正後のbase.alphaを正として⑧'-2再defringeを行う
- alpha=255画素は⑧'-1のRGBが最終
- alpha<255画素は、alpha=0を含め、最近傍alpha=255画素のRGBで再defringeした値が最終
- 最近傍は二乗Euclidean距離最小、同距離は線形index最小
- alpha=255画素が0件なら生成エラー
- ⑧'-2完了後にbbox、fgBbox、baseLum、calibration、keep、stitch等の派生値を再計算
- straight PNG生成後に独立デコードし、その最終画素から派生値と検査1〜7・M/Fをもう一度計算
- 検査8の再生成側も⑧'-1、⑧'-2、straight encode、独立decodeの同じ順を通る

#### material-aware leatherVariantとシェーブル

- 旧`COLORS`の24色だけを前提にせず、色選択の正本を27件の`LEATHER_VARIANTS`へ移行する
- variantKeyはmaterialKeyとcolorKeyの組で一意にし、無効なmaterial×colorの直積を生成しない
- 左右stateはそれぞれvariantKeyを持ち、UIはmaterialを選ぶとそのmaterialに属する色だけを表示する
- シェーブル3色は`chevre:pale-greige`、`chevre:green`、`chevre:lavender`相当の安定keyを持つ。実際のkey綴りと日本語labelはcolor freezeを正とする
- `MATERIAL_PROFILES`はmaterialKey、textureProfileKey、grain source SHA/ROI、生成方式、適用可能なsourceMode/toneModeを持つ
- シェーブルのgrainPolicyがdistinctなら、現行の単一`leather-grain.jpg`だけを全materialへ流用しない。material別base派生またはアセット解像度でのmaterial別grain合成のどちらかを正本で一意に選び、F oracle、検査8、memory/cache見積りを連動更新する
- photo/photoチャームへ異なる革種のシボを正確に適用できない場合、全チャームcolorless/flat化またはそのvariantの適用対象制限を選ぶ。元写真のシボをシェーブルと表示する代替は禁止する
- `DISPLAY_COLORS`は既存materialの24色とreferenceIvory互換を維持しつつ、最終的にはapprovedHexを持つ`DISPLAY_VARIANTS`へ移行する。シェーブル3色へfrenchie由来referenceIvoryを誤適用しない
- LEATHER_VARIANTSのdebug転記照合8項目はvariantKey、materialKey、colorKey、canonicalLabel、aliases、approvedHex、textureProfileKey、appliesToCharmsとする
- MATERIAL_PROFILESのdebug転記照合6項目はmaterialKey、canonicalLabel、textureProfileKey、grainPolicy、grainSourceSha256、generationModeとする
- 完成bitmapのcache keyは少なくとも`charmKey|variantKey|materialKey|logo|stitch`を含む。丸カン金具は引き続き含めない
- 216有限値は8チャーム×27有効variantだけを全列挙する。material profile分岐、色域外圧縮、texture適用も各variantの正しいprofileで検査する
- 色名、hex、grain、material UIのいずれかを変更したら、color freeze、MATERIAL_PROFILES、DISPLAY_VARIANTS、debug、216有限値、27 variant回帰、クライアント確認を連動更新する

#### ロゴ／ステッチ固定目視回帰セット

216有限値・27 variant回帰・81配置とは別に、ブラックとアイボリーに対応する既存materialの2 variantを使う固定目視セットを維持する。各チャームのケース数は次式で機械算出する。

`2 baselineVariants × (hasLogo ? 2 logoColors : 1) × (stitchMode === "none" ? 1 : 2 stitchColors)`

既知7種はロゴ有・stitchなし5種×4件＋ロゴ有・stitch有2種×8件=36件。swanは、ロゴ有・stitchなしなら合計40、ロゴ有・stitch有なら44、ロゴなし・stitchなしなら38、ロゴなし・stitch有なら40となる。prep確定後に`fixedVisualCaseCount`とチャーム別内訳を検査レポート・v2指示書へ記録する。ロゴ有無またはstitchModeが未確定ならPhase 4を完了しない。

#### 非同期・Worker

- production grainとoracleは別世代状態を持つ
- 新世代開始時に旧Promiseをrejectし旧Workerをterminateする
- generationとoperation tokenを照合する
- main-thread fallbackも定期的に中断を確認する
- timeout後はtimer、Worker、Blob URL、state参照を解放する

### 6-3. 連動更新

| 変更 | 更新対象 |
|---|---|
| 採用画像 | §4-1、§4-1b、§4-1c、INPUT_MANIFEST、CHARM_SPECS |
| sourceMode | grain、aspect補間、M/F、toneMode、216有限値 |
| 寸法・比率 | crop、workSize、calibration、bbox、fgBbox、81配置 |
| 所有領域 | manual-alpha、owner、keep、inpaint、恒等式、目視セット |
| frenchie入力 | referenceIvory、全flat DISPLAY_COLORS、F-3/F-4/8 |
| color chart | color freeze、27 leatherVariant、approvedHex、label/alias、216有限値、27色回帰、variant転記8項目 |
| シェーブル | materialKey、textureProfile、grain source、asset/runtime分岐、cache、UI、summary、material転記6項目 |
| swan | SHA、所有分類、ロゴ、stitchMode、実寸、UI、ロゴ／ステッチ固定目視回帰セットの算出値 |
| PNG規則 | schema/tool、全成果物、期待hash、byteLength、検査1〜8 |
| 派生値 | prep正本、CHARMS転記10項目、debug照合 |

### 6-4. 検証

- 正本計画だけで8種の入力と承認見本が一意に決まる
- 「未受領」「再生成不要」など旧状態が残っていない
- 数量が8種、27 leatherVariant、216有限値、81配置、CHARMS転記10項目、variant転記8項目、material転記6項目で統一される
- 実装指示書と正本計画が同じ改訂内容を指す
- Codexレビューを反復しok:trueを取得する

### 6-5. revision lock manifest

固定パスはplans/charm-combo-revision-lock.jsonとする。schemaVersion 1の必須フィールドは次のとおり。

| field | 内容 |
|---|---|
| schemaVersion | 1 |
| planRevision | 14 |
| documentCommit | 改訂14・v2・失効ヘッダーを作成した直前コミット |
| sourceFreezeCommit | Phase 1の固定コミット |
| documents | pathとsha256の配列 |
| counts | charms=8、materialProfiles=2、leatherVariants=27、finiteCases=216、layouts=81、charmTransferFields=10、variantTransferFields=8、materialProfileTransferFields=6 |
| codexReview | path、reviewEvidenceCommit、targetDocumentCommit、status、resultSha256、iterations、phases |
| supersedesLockCommit | 初回はnull。再改訂時は直前に選択されたlock commit |
| manifestPayloadSha256 | 本フィールドを除外したcanonical JSONのSHA-256 |
| generatedAt | ISO日時 |

documentsはリポジトリ相対パスを正規化し、..、絶対パス、symlinkを禁止する。対象は次の3文書。

- plans/charm-combo-display-plan.md
- plans/impl-instructions-charm-combo-display-v2.md
- plans/impl-instructions-charm-combo-display.md

manifestのcanonical JSONは、`manifestPayloadSha256`を除外し、全object keyをUnicodeコードポイント昇順、array順序は保持、余分な空白なし、UTF-8、末尾改行なしで直列化する。SHA-256はこのbyte列から算出する。

Phase 2のGit親子鎖は次の順に固定する。

1. documentCommit: 外部選択済みselectedPhase1EvidenceCommitを唯一の親とし、3文書だけを固定する
2. reviewEvidenceCommit: documentCommitを唯一の親とし、固定パス`reports/charm-combo/rev14/plan-review-{arch,diff,cross-check}.json`と`plan-review.json`だけを追加する
3. selectedLockCommit: reviewEvidenceCommitを唯一の親とし、`plans/charm-combo-revision-lock.json`だけを追加・更新する
4. phase-2 evidence commit: selectedLockCommitを唯一の親とし、revision-lock-verification.json、phase-2-handoff.json、再改訂時はsupersession.jsonだけを追加する

各plan-review-{phase}.jsonは対象documentCommit、対象3文書SHA、phase、ok、summary、issuesを必須とする。plan-review.jsonはschemaVersion、targetDocumentCommit、3文書のpath/SHA、各レビュー結果JSONのpath/SHA/ok/反復数、overallOkを必須とする。必須phaseは`arch`、`diff`、`cross-check`で、overallOkは3phaseすべてok:trueの場合だけtrueとする。lock commit番号はmanifest自身へ埋め込まない。

検証アルゴリズムは次のとおり。

1. git show documentCommit:pathで各文書のbyte列を取得する
2. byte列のSHA-256を再計算する
3. documents配列のpath、sha256と完全一致させる
4. manifestPayloadSha256をcanonical JSON規則で再計算して一致させる
5. reviewEvidenceCommitの親がdocumentCommitただ一つ、変更対象が固定された4レビューJSONだけであることを確認する
6. `git show <reviewEvidenceCommit>:<reviewPath>`で4JSONを読み、各結果のtargetDocumentCommit、3文書SHA、phase、ok=true、結果SHA、および統合結果のoverallOk=trueを照合する
7. selectedLockCommitの親がreviewEvidenceCommitただ一つであり、当該commitの変更対象がlock manifestだけであることを確認する
8. 改訂14計画とv2指示書の2文書から、改訂14、8種、2 material profile、27 leatherVariant、216有限値、81配置、CHARMS 10項目、variant 8項目、material 6項目を独立抽出する
9. 2文書の抽出値をcountsと完全一致させる
10. 旧指示書は先頭の失効メタデータだけを解析し、status、supersededAt、successorPlan、successorInstructions、lockManifestを完全一致させる。旧本文の数量は抽出対象外とする
11. sourceFreezeCommitとselected phase-1 evidence commitがphase-1-handoffと一致する
12. manifestのcodexReview.path、reviewEvidenceCommit、targetDocumentCommit、resultSha256、status ok:trueがplan-review.jsonと一致する
13. reviewEvidenceCommitを親とするvalidなlock commit候補が複数ある場合は自動選択せず停止し、Fable 5またはユーザーが選んだcommitをphase-2-handoffのselectedLockCommitへ記録する
14. reports/charm-combo/rev14/revision-lock-verification.jsonへ、選択lock commit、manifest payload、親子鎖、正本2文書の数量照合、旧指示書の失効照合を別フィールドで保存する
15. `git rev-list --parents -n 1`でdocumentCommitの唯一の親がselectedPhase1EvidenceCommitであることを確認し、`git merge-base --is-ancestor`でsourceFreezeCommit→phase-1 evidence→document→review evidence→selectedLockCommitが一本道の祖先鎖であることを確認する

一つでも不一致ならPhase 3を開始しない。

### 6-6. 成果物と引渡し

- plans/charm-combo-display-plan.md revision 14
- plans/impl-instructions-charm-combo-display-v2.md
- 失効ヘッダー付き旧実装指示書
- plans/charm-combo-revision-lock.json
- reports/charm-combo/rev14/plan-review.json
- reports/charm-combo/rev14/plan-review-arch.json
- reports/charm-combo/rev14/plan-review-diff.json
- reports/charm-combo/rev14/plan-review-cross-check.json
- reports/charm-combo/rev14/revision-lock-verification.json
- reports/charm-combo/rev14/phase-2-handoff.json
- document commit、review evidence commit、lock commit、phase-2 evidence commit

phase-2-handoffにはselectedPhase1EvidenceCommit、sourceFreezeCommit、documentCommit、reviewEvidenceCommit、selectedLockCommit、supersedesLockCommit、3文書SHA、lock manifest SHA、manifestPayloadSha256、Codexレビュー結果path/SHA、counts、Phase 3開始可否を記録する。phase-2-handoff自身は§2-4に従いselectedLockCommit直後のphase-2 evidence commitへ固定する。以降の全handoffは同じselectedLockCommitとselectedPhase2EvidenceCommitを必須入力として伝播する。

### 6-7. 停止条件

- Phase 1のSHAまたはcommit不一致
- 改訂14計画とv2指示書の改訂、数量、相互参照が不一致
- 旧指示書の失効メタデータが欠落またはsuccessor/lock参照と不一致
- 旧指示書にDO NOT EXECUTEがない
- Codexレビューがok:false
- revision lock検証がFAIL

### 6-8. アンチパターン

- 旧指示書を曖昧な「参考」に留めない
- 自己参照するlock commitをmanifestへ書かない
- source freeze後に別画像へ黙って差し替えない
- 改訂12/13の再defringeをstraight PNGで代替したことにしない
- 計画だけ14、指示書だけ旧数量という混在を残さない

## 7. Phase 3: Codexへの計画同期

### 7-0. dirty worktree保全ゲート

現在のfeature/charm-combo-codexには、ユーザー所有の未コミットassetと未追跡manualレイヤがある。merge、checkout、再生成、削除より先に次を行う。

1. git status --shortを記録する
2. 対象ファイルの絶対パス、種別、byteLength、SHA-256を記録する
3. tracked変更はgit diff --binaryで保存する
4. `git rev-parse --git-path codex-checkpoints`でGit管理外の保全ルートを解決し、その配下の`charm-rev14-premerge-UTC日時/`へmodified/untracked対象をコピーする
5. コピー先のSHAを再計算し元ファイルと完全一致させる
6. 空の一時ディレクトリへ復元し、SHA一致を再確認する
7. 保全ルートが`git status --short --untracked-files=all`に出ず、`git add --dry-run --all`の対象にもならないことを確認する
8. バックアップ台帳をユーザーへ報告し、既存assetを再生成で置換してよいか確認する

対象には少なくとも次を含める。

- assets/charms/cat.prep.json
- assets/charms/dachshund.prep.json
- assets/charms/frenchie.prep.json
- assets/charms/horse.prep.json
- assets/charms/horseshoe.base.png
- assets/charms/horseshoe.manual-keep.png
- assets/charms/horseshoe.masks.png
- assets/charms/horseshoe.prep.json
- assets/charms/osanpo.prep.json
- assets/charms/toypoodle.prep.json
- assets/charms/horseshoe.manual-alpha.png
- assets/charms/horseshoe.manual-owner.png
- nmrn0629-maisonampersand.colorcheckboard/カラーチャート/レッド系.jpg
- nmrn0629-maisonampersand.colorcheckboard/カラーチャート/イエロー系.jpg
- nmrn0629-maisonampersand.colorcheckboard/カラーチャート/ブラウン系.jpg
- nmrn0629-maisonampersand.colorcheckboard/カラーチャート/ブルー系.jpg
- nmrn0629-maisonampersand.colorcheckboard/カラーチャート/ニューカラー（シェーブル）.jpg

バックアップがない状態でgit checkout、git restore、git reset、clean、上書き生成を行わない。ユーザー承認後も元のdirty worktreeではmerge、生成、検証、コミットを行わない。Phase 3以降は次の専用worktree契約を必須とする。

1. 元worktreeのbranch tip、status、バックアップ台帳SHAを記録し、以後は読み取り専用とする
2. selectedPhase2EvidenceCommitそのものを開始点として`feature/charm-combo-codex-revNN`を作り、`git worktree add -b <branch> <path> <selectedPhase2EvidenceCommit>`相当でリポジトリ外の専用ディレクトリへcheckoutする
3. 専用worktreeのHEADがselectedPhase2EvidenceCommitと完全一致しcleanであることを確認する。起点ブランチのmerge、merge commit、cherry-pickは禁止し、branch名が既存またはHEAD不一致なら停止する
4. Phase 3〜7の生成、検証、コミットは専用worktreeだけで行う
5. 元worktreeの未コミットassetを取り込む場合は、バックアップ台帳のpath/SHAごとにユーザー承認されたものだけを専用worktreeへコピーし、コピー後SHAを照合する。承認のないファイルは取り込まない
6. canonicalSource、source freeze、旧未コミットassetが同じ出力先を要求する、またはSHAが競合する場合は停止する
7. 完了後も元worktreeへファイルを自動コピー・merge・restoreしない。専用branchのcommitをユーザーへ報告し、統合指示を待つ

Phase 3で無変更を保証する固定配列`protectedFiles`は次の2点とする。

- index.html
- bags.html

比較基準`phase3StartHead`はselectedPhase2EvidenceCommitそのものとし、`git diff --exit-code <phase3StartHead> -- index.html bags.html`とbyte SHA-256の両方で無変更を証明する。計画・指示書・lockは`protectedFiles`ではなく`lockedDocuments`として、revision lockの`documentCommit`に対して照合する。ユーザー所有の未コミットassetは`preservedWorktreeFiles`としてバックアップ台帳のSHAと照合し、3分類を混同しない。

### 7-1. 開始条件

- Fable 5の計画改訂がローカル起点ブランチへコミット済み
- 計画のCodexレビューがok:true
- 採用素材台帳がユーザー承認済み
- canonicalSourceとcanonicalReferenceがGit管理下に存在
- revision lock manifestの照合がPASS
- selected phase-2 evidence commitの親子関係、許可path、payload SHAがPASS
- dirty worktreeのバックアップ、復元試験、ユーザー承認が完了
- Phase 3専用worktreeがcleanで、元worktreeが読み取り専用として台帳化済み

### 7-2. 実施内容

1. selectedPhase2EvidenceCommitを外部選択し、その親がphase-2-handoff記載のselectedLockCommitであること、許可path、payload SHAを`git show`で照合する
2. selectedPhase2EvidenceCommitから直接、専用branch `feature/charm-combo-codex-revNN`とclean worktreeを作り、`git rev-parse HEAD`一致、親がselectedLockCommitただ一つ、fast-forward以外の統合なしを確認する。選択SHAはphase-2-handoff自身ではなくphase-3-handoffへ記録する
3. 計画コミットと採用素材台帳を読み込む
4. 既存未コミットassetは元worktreeに残し、ユーザーがSHA単位で採用承認したものだけを専用worktreeへ明示コピーする
5. INPUT_MANIFESTとCHARM_SPECSを準備ツール入力から更新する
6. COLOR_REFERENCE_MANIFEST、LEATHER_VARIANTS、MATERIAL_PROFILESをcolor freezeから生成可能な定義へ追加する
7. schemaVersionとtoolVersionを計画値へ更新する
8. swanをCHARM_SPECSとバッチへ正式登録する
9. 画像座標が変わる全チャームのmanualレイヤを作り直す
10. CHARMSはprep確定後まで変更しない

### 7-3. 検証

- INPUT_MANIFESTと採用素材台帳のファイル、SHA、modeが完全一致
- CHARM_SPECSが8種
- LEATHER_VARIANTSが27件、MATERIAL_PROFILESと許可組がcolor freezeに一致
- 古いJPEGや候補PNGへフォールバックしない
- selftest=1が全通過
- `protectedFiles`がphase3StartHeadとbyte単位で一致する
- `lockedDocuments`がrevision lockのdocumentCommit・SHAと一致する
- `preservedWorktreeFiles`がバックアップ台帳・復元試験のSHAと一致する
- 元worktreeのstatusと全preservedWorktreeFiles SHAがPhase 3開始前から変化していない
- phase-3-handoff.jsonへ専用worktree絶対パス、専用branch、元branch tip、phase3StartHead=`selectedPhase2EvidenceCommit`、selectedLockCommit、入力source commit、tool commit、直接親／祖先検査、3分類の照合結果、検査結果を記録

### 7-4. アンチパターン

- 画像を差し替えてcrop座標だけ旧値のままにしない
- manualレイヤを画像サイズだけ伸縮して流用しない
- reference画像をpipelineSourceとして誤使用しない
- swanをSHA nullのままバッチへ含めない
- dirty worktreeで直接merge・再生成しない
- `git rev-parse --git-path codex-checkpoints`配下の保全物を作業ツリーへコピーし戻さない
- 専用worktreeの成果を元worktreeへ自動コピー・自動mergeしない

### 7-5. 成果物と引渡し

- 新版INPUT_MANIFESTとCHARM_SPECS
- 新版schema/tool定義
- COLOR_REFERENCE_MANIFEST、LEATHER_VARIANTS、MATERIAL_PROFILES
- swanを含む8種バッチ
- reports/charm-combo/rev14/phase-3-handoff.json
- tool commit
- phase-3 evidence commit

固定順は次のとおり。

1. selectedPhase2EvidenceCommitを唯一の親とし、`tools/charm-prep.html`だけをtoolCommitへ固定する。別ファイルへ実装を分離する必要が生じた場合は還流票で計画改訂する
2. `git show <toolCommit>:tools/charm-prep.html`のSHAと、INPUT_MANIFEST、CHARM_SPECS、COLOR_REFERENCE_MANIFEST、LEATHER_VARIANTS、MATERIAL_PROFILES、schema/toolVersion抽出値をphase-3検査結果と照合する
3. 最終toolCommit上のコードでselftestを再実行する
4. prep-selftest.jsonとphase-3-handoffを、toolCommitを唯一の親とするphase-3 evidence commitへ固定する

handoff JSONにはselectedLockCommit、selectedPhase2EvidenceCommit、sourceFreezeCommit、source/color freeze SHA、toolCommit、tool file SHA、inputManifestSha、colorReferenceManifestSha、leatherVariantCount=27、materialProfile定義SHA、selftest結果、未解決事項、次フェーズ開始可否、handoffPayloadSha256を記録する。

## 8. Phase 4: 全8種の派生アセット再生成

### 8-0. 開始時参照と固定入力

新しいセッションは作業前に次を読む。

- plans/charm-combo-display-plan.md revision 14
- plans/impl-instructions-charm-combo-display-v2.md
- plans/charm-combo-revision-lock.json
- reports/charm-combo/rev14/phase-3-handoff.json
- selected phase-3 evidence commit
- tools/charm-prep.html
- 採用素材台帳
- color freezeとcanonicalColorChart 5点

Phase 4開始時にselectedPhase3EvidenceCommitを外部選択する。その親がphase-3-handoff記載のtoolCommitであること、許可path、payload、tool SHAを§2-4どおり検証する。selectedPhase3EvidenceCommitはphase-3-handoff自身には記録せず、phase-4-handoffへ記録する。他の固定入力はselectedLockCommit、sourceFreezeCommit、toolCommitで識別し、不一致なら停止する。

### 8-1. 実施内容

採用済み正規入力から全8種を再生成する。

- base.png
- masks.png
- prep.json
- manual-alpha.png
- manual-owner.png
- manual-keep.png
- 必要な場合manual-inpaint.png

Phase 2で選択されたmaterial適用方式も同時に実施する。

- grainPolicy=`distinct`のmaterialは承認済みgrain source/ROI/SHAだけを使う
- material別base派生方式なら、v2指示書で固定された命名規則に従い、各flatチャーム×適用textureProfileのbaseを生成する。masks共有は寸法・alpha・所有権が完全一致する場合だけ許可する
- runtime material合成方式なら、neutral baseと全material profileをstraight RGBA/独立oracleで検査し、ステージ座標でのpixel処理を行わない
- prep.jsonへmaterialAssets、textureProfileKey、grain source SHA、生成方式、期待hash/byteLengthを記録する
- photo/photoチャームに適用不能なvariantを黙って表示しない。color freezeのappliesToCharmsと一致させる

旧photo成果物、旧horseshoe成果物、座標が異なるmanualレイヤは流用しない。

### 8-2. 検査順

1. 入力SHAと寸法
2. 背景・alpha
3. 所有権
4. ④の初回defringe
5. ロゴ
6. ステッチ
7. 保持装飾
8. ⑧'-1アスペクト幾何補正
9. ⑧'-2補正後alpha基準の再defringe
10. ⑧'-2後の派生値再算出
11. straight PNG生成
12. 最終PNG独立デコード
13. 最終デコード画素から派生値再算出
14. 最終デコード画素で検査1〜7・M
15. colorlessはF
16. 検査8の独立再生成で①〜15を再実施

### 8-3. frenchie連動

frenchieを差し替えた場合、referenceIvoryを再算出する。その値から全flatチャームのDISPLAY_COLORSを再導出し、F-3、F-4、検査8、charms.html転記を再実施する。

### 8-4. 検証

- 全8種が新版schema/tool
- 全PNGにstraight marker
- 最終デコード画素で検査4が0.5%未満
- 所有権恒等式
- 8種すべて検査1〜8・M、flatはF
- 全適用material profileでgrain SHA、生成分岐、独立oracle、検査8がPASS
- 27 variantのappliesToCharmsと生成可能asset/profileが一致
- prepの期待hashとbyteLengthが最終ファイルと一致
- 全8種再生成後の最終tool状態でselftest=1を再実行
- selftestのtoolVersion、ブラウザ名・版、実行時刻、各試験結果を保存
- アスペクト補正対象は⑧'-1後だけでなく⑧'-2後の最終検査4を通過
- 検査8の再生成物も⑧'-2後の最終画素と完全一致

### 8-5. 成果物

- assets/charms配下の8種派生アセット一式
- Phase 2で確定したmaterial別派生baseまたはmaterial profile asset一式
- reports/charm-combo/rev14/prep-selftest.json
- reports/charm-combo/rev14/prep-inspection-1-8.json
- reports/charm-combo/rev14/phase-4-handoff.json

検査がすべてPASSした後、次の二段階で固定する。

1. selectedPhase3EvidenceCommitを唯一の親としてasset commitを作成する。コミット対象は8種それぞれの`*.base.png`、`*.masks.png`、`*.prep.json`と、採用台帳・新版prepが要求する各`*.manual-*.png`だけに限定する。`tools/charm-prep.html`、source freeze、QA画像、handoffは混在させない
2. `git show <assetCommit>:<path>`から全コミット済み成果物を読み、byteLengthとSHA-256を再計算してprep、検査レポート、作業ツリーの三者と一致させる
3. phase-4-handoffを作成し、assetCommitを唯一の親とするphase-4 evidence commitでhandoff、selftest、検査1〜8レポートだけを固定する

phase-4-handoffの必須フィールドは、selectedLockCommit、selectedPhase3EvidenceCommit、入力3コミット、source/color freeze SHA、phaseOutputCommit=`assetCommit`、全成果物SHA、material asset/profile SHA、leatherVariantCount=27、schema/tool、検査1〜8・M/F、material oracle、selftest、未解決事項、Phase 5開始可否、`handoffPayloadSha256`とする。payload規則とevidence選択は§2-4に従う。

Phase 5開始時は、(a) `assetCommit`の`git show`内容、(b) phase-4-handoffの成果物SHA、(c) 作業ツリーの実ファイルを三者照合し、一つでも不一致なら開始しない。handoff自身も`handoffPayloadSha256`を再計算して照合する。

### 8-6. 停止条件

- 入力コミットまたはSHAがhandoffと不一致
- straight markerがない
- 検査1〜8・M/FまたはselftestにFAIL
- manualレイヤの由来が新入力と一致しない
- swanの所有分類が未確定
- シェーブルgrainPolicy、生成方式、appliesToCharmsが未確定またはcolor freezeと不一致
- asset commitの対象外ファイル混入、またはcommit・handoff・作業ツリーの三者不一致
- handoffPayloadSha256不一致

### 8-7. アンチパターン

- 旧成果物の期待hashだけを書き換えない
- 検査前ImageDataを最終成果として扱わない
- ブラウザ自己試験を未実施のままPASSと記録しない
- 検査FAILをlegacy例外で免除しない

## 9. Phase 5: 形状・意匠QA

色のクライアント確認と分離し、まず形状・意匠だけを承認する。

### 9-0. 開始時参照と固定入力

新しいセッションは次を読む。

- revision 14計画とv2指示書
- revision lock manifest
- phase-4-handoff.json
- selected phase-4 evidence commit
- 採用素材台帳
- canonicalReference 8点

Phase 5開始時にselectedPhase4EvidenceCommitを外部選択する。その親がphase-4-handoff記載のassetCommitであること、許可path、payload SHA、commit／handoff／作業ツリーの三者を照合する。selectedPhase4EvidenceCommitはphase-4-handoff自身には記録せず、phase-5-handoffへ記録する。

### 9-1. 比較方法

各チャームについて次の4面比較を作る。

1. pipelineSource
2. approvalReference
3. 新派生アセットのアイボリー表示
4. 新派生アセットのシェーブル代表色表示

必要に応じて輪郭差分overlayを追加する。縦横比を変えて比較しない。

### 9-2. 共通合格条件

- 外形が承認見本と一致
- 縦横比、傾き、向きが一致
- 革質感が端まで連続
- 不要な黒い外縁がない
- 吊り穴の縁が革色と整合
- ロゴの位置、大きさ、角度が一致
- 箔押しに不要な立体感・黒いhaloがない
- 保持装飾は色変更で変化しない
- material切替で外形・alpha・ロゴ・保持装飾が変わらず、革所有画素の質感だけが承認profileに従う

### 9-3. 個別確認

- horse: 比率を変更しない。形状承認済み。色は後工程
- horseshoe: 外形、穴、アンパサンド内部、ロゴ傾き、アイボリーを基準
- dachshund: 最新指示で採用された最初の生成版をreferenceへ固定
- cat: 地面側の前足を含めcatフチ画像の線に忠実
- swan: ロゴの黒haloなし、平面箔、元の位置・大きさ・傾き
- frenchie、toypoodle、osanpo: 承認済み見本との形状一致

### 9-4. ゲート

全8種の形状・意匠が承認されるまでcharms.html完成実装へ進まない。27 leatherVariantの色承認待ちは、このゲートを妨げない。

### 9-5. 成果物

- reference-visualsとの4面比較8点
- 必要な輪郭差分overlay
- reports/charm-combo/rev14/shape-approval.json
- reports/charm-combo/rev14/phase-5-handoff.json
- shape approval commit
- phase-5 evidence commit

shape-approvalにはkey、source SHA、reference SHA、asset SHA、承認範囲、承認日時、色承認の保留状態を記録する。

selectedPhase4EvidenceCommitを唯一の親とし、全8種の比較画像、overlay、shape-approval.jsonだけをshapeApprovalCommitへ固定する。`git show`で全path/SHAを再照合後、phase-5-handoffだけをshapeApprovalCommitを唯一の親とするphase-5 evidence commitへ固定する。phase-5-handoffにはselectedLockCommit、selectedPhase4EvidenceCommit、assetCommit、shapeApprovalCommit、承認資料SHA、handoffPayloadSha256、Phase 6開始可否を記録する。

### 9-6. 停止条件

- 形状・比率・ロゴ・保持装飾のいずれかが不一致
- ユーザーの最新承認とreference SHAが対応しない
- 色の不承認だけを理由に形状を再生成しようとしている

### 9-7. アンチパターン

- 比較時に縦横比を変更しない
- referenceを再生成して差分を隠さない
- 色修正と同時に形状を変更しない
- 会話上の古いOKを最新承認として使わない

## 10. Phase 6: charms.html完成実装

### 10-0. 開始時参照と固定入力

新しいセッションは次を読む。

- revision 14計画とv2指示書
- revision lock manifest
- phase-4-handoff.json
- phase-5-handoff.json
- shape-approval.json
- selected phase-5 evidence commit
- plans/charm-approved-color-freeze.json
- 現在のcharms.htmlとcommit 3cf9f1c

Phase 6開始時にselectedPhase5EvidenceCommitを外部選択し、その親がphase-5-handoffから得たshapeApprovalCommitであること、許可path、payload SHA、承認資料SHAを照合する。選択SHAはphase-5-handoff自身ではなくphase-6-handoffへ記録する。他の固定入力はassetCommit、shapeApprovalCommit、selectedLockCommitである。

### 10-1. 実施内容

代表版charms.htmlを8種＋なしへ拡張する。

- 左右各9択
- 左右各27 leatherVariant（materialKey＋colorKey。materialで絞り込み）
- 丸カン銀・金
- ロゴ金・銀
- ステッチ白・黒
- toneMode別リカラー
- 左右独立のmaterial/variant stateとmaterial絞り込みUI
- DISPLAY_VARIANTSとMATERIAL_PROFILES
- material方式に応じたbase/profile選択
- コンテンツ縦センタリング
- 8エントリLRU
- renderGen
- debug照合10項目
- variant転記8項目＋material転記6項目

### 10-2. コピーする既存パターン

- LAYOUT
- BitmapLRU
- renderGen
- cacheKey
- photoLeatherTone
- flatLeatherTone
- capture renderJob
- ImageBitmap.close
- debug harness

### 10-3. 検証

- 216有限値
- 81配置のクリップ、重なり、負余白
- CHARMS転記10項目
- 既存24色のreferenceIvory／旧DISPLAY_COLORS互換値
- DISPLAY_VARIANTS 8項目とMATERIAL_PROFILES 6項目の転記照合
- variantKey/materialKeyを含むcache key、material切替時の古い非同期結果破棄
- 素早い連続切替
- LRU退避と再生成
- なし側UI
- ロゴ・ステッチの有効条件

### 10-4. 成果物

- 完成版charms.html
- reports/charm-combo/rev14/runtime-debug.json
- reports/charm-combo/rev14/layout-81.json
- reports/charm-combo/rev14/phase-6-handoff.json

selectedPhase5EvidenceCommitを唯一の親とし、`charms.html`だけをcharmsCommitへ固定する。`index.html`と`bags.html`が比較基準commitからbyte不変であることを再確認する。runtime-debug.json、layout-81.json、phase-6-handoffだけをcharmsCommitを唯一の親とするphase-6 evidence commitへ固定する。

phase-6-handoffにはselectedLockCommit、selectedPhase5EvidenceCommit、source/color freeze SHA、assetCommit、shapeApprovalCommit、phaseOutputCommit=`charmsCommit`、27 leatherVariant、216有限値、81配置、CHARMS 10項目、variant 8項目、material 6項目照合、material-aware cache、LRU、世代トークン、各レポートSHA、handoffPayloadSha256、Phase 7開始可否を記録する。

### 10-5. 停止条件

- prepとCHARMSの10項目が不一致
- color freezeとvariant 8項目／material 6項目が不一致
- 216有限値または81配置にFAIL
- shape approval後に形状が変化
- index.htmlまたはbags.htmlに差分

### 10-6. アンチパターン

- 代表チャームのハードコードを残さない
- cache keyへ丸カン金具を含めない
- stateを非同期合成途中で再読込しない
- CHARMSだけを手修正しない
- 形状調整をruntime CSS transformで隠さない

## 11. Phase 7: 色のクライアント確認

### 11-0. 開始時参照と固定入力

新しいセッションは次を読む。

- revision 14計画とv2指示書
- phase-5 shape approval
- phase-6-handoff.json
- selected phase-6 evidence commit
- referenceIvoryとDISPLAY_COLORSの正本値
- color freeze、DISPLAY_VARIANTS、MATERIAL_PROFILESの正本値

Phase 7開始時にselectedPhase6EvidenceCommitを外部選択し、その親がphase-6-handoff記載のcharmsCommitであること、許可path、payload SHA、成果物SHAを照合する。選択SHAはphase-6-handoff自身ではなくphase-7-handoffへ記録する。他の固定入力はshape承認済みassetCommit、shapeApprovalCommit、charmsCommit、selectedLockCommitである。

全8種について27 leatherVariantを出力する。

このフェーズでは形状を変更しない。色修正が必要な場合は、DISPLAY_COLORS、referenceIvory、flatLeatherToneまたはphotoLeatherToneのどの層を変えるかをFable 5が計画へ記録する。

個別の色合わせをCHARMSや出力PNGへ直接焼き込まない。色変更後は216有限値、27 variant回帰、material profile検査、検査F、固定検証セットを再実施する。

### 11-1. 成果物

- 8種×27 variantのクライアント確認シート
- reports/charm-combo/rev14/color-review.json
- reports/charm-combo/rev14/phase-7-handoff.json

color-reviewは8種×27 leatherVariantの一意な216 entryを持ち、各entryへcharm key、variantKey、materialKey、colorKey、canonicalLabel、approvedHex、表示画像path/SHA、status=`approved|changes_requested|pending`、コメント、approvalEvidenceIdを記録する。集計値としてapprovedCount、changesRequestedCount、pendingCount、submissionReady、productAcceptedを持つ。

`submissionReady`は216組が重複なく存在し、全表示画像SHAと提出用シートSHAが固定され、クライアントへの提出証跡IDがある場合にtrueとする。`productAccepted`はsubmissionReady=true ∧ approvedCount=216 ∧ changesRequestedCount=0 ∧ pendingCount=0 ∧ 全approvalEvidenceId非空の場合だけtrueとし、手入力を禁止する。

selectedPhase6EvidenceCommitを唯一の親とし、8種×27 variantの確認シートとcolor-review.jsonだけをcolorReviewCommitへ固定する。全path/SHAを`git show`で照合後、phase-7-handoffだけをcolorReviewCommitを唯一の親とするphase-7 evidence commitへ固定する。phase-7-handoffにはselectedLockCommit、selectedPhase6EvidenceCommit、assetCommit、shapeApprovalCommit、charmsCommit、colorReviewCommit、確認シートSHA、leatherVariantCount=27、approvedCount、changesRequestedCount、pendingCount、submissionReady、productAccepted、handoffPayloadSha256を記録する。

初回提出時にpendingが残る場合、submissionReady=true・productAccepted=falseのPhase 7 evidenceは「提出完了」証跡として保存できるが、最終完了には使わない。出力画像を変えず承認状態だけ更新する場合は、新しいcolorReviewCommitとphase-7 evidenceを同じselectedLockCommit上で再発行する。色定数、変換式、referenceIvory、DISPLAY_COLORS、アセットまたは表示画像を変える修正依頼は§12の還流票・再改訂・restartPhase判定を通し、新selectedLockCommitに基づく新Phase 7 evidenceがproductAccepted=trueになるまで完了としない。

### 11-2. 停止条件

- shape承認済みassetまたはcharms commitが変わった
- 色変更が形状・alpha・所有権へ影響する
- referenceIvory変更後に全flat再導出をしていない
- 216組が一意でない、表示画像SHAまたは提出証跡がない
- 最終完了を判定する時点でchangesRequestedCountまたはpendingCountが1以上

### 11-3. アンチパターン

- 一つのチャームだけDISPLAY_COLORSを単独上書きしない
- 色修正を出力PNGへ直接塗らない
- クライアント未確認を承認済みと記録しない
- 色調整のために承認済み形状を再生成しない

### 11-4. 最終選択記録

Fable 5またはユーザーは、有効なphase-7 evidence commitを外部選択する。選択後、次を全て検証する。

- selectedPhase7EvidenceCommitがphase-7-handoff記載のcolorReviewCommitを唯一の親に持つ
- selectedPhase7EvidenceCommitの変更pathが`reports/charm-combo/rev14/phase-7-handoff.json`だけである
- handoffPayloadSha256、selectedLockCommit、selectedPhase6EvidenceCommit、colorReviewCommit、確認シートSHAが一致する
- submissionReady=trueである。製品最終承認を記録する場合は加えてproductAccepted=true、approvedCount=216、changesRequestedCount=0、pendingCount=0である

検証結果を`reports/charm-combo/rev14/final-acceptance.json`へ固定する。必須フィールドはschemaVersion、planRevision、selectedLockCommit、selectedPhase7EvidenceCommit、colorReviewCommit、phase7HandoffPath／byteLength／SHA-256、colorReviewPath／byteLength／SHA-256、confirmationSheetPath／byteLength／SHA-256、submissionReady、productAccepted、verifiedAt、verifier、acceptancePayloadSha256とする。`acceptancePayloadSha256`の直列化規則は§2-4のhandoffPayloadSha256と同じとする。

`finalAcceptanceCommit`はselectedPhase7EvidenceCommitを唯一の親とし、変更pathを`final-acceptance.json`だけに限定する。commit SHA自身はJSONへ書かず、自己参照させない。同じselectedPhase7EvidenceCommitを親とするvalidなfinalAcceptanceCommitが複数ある場合は自動選択せず停止する。最終報告は選択されたselectedPhase7EvidenceCommitとfinalAcceptanceCommitの両SHAを記載する。

## 12. Codex変更還流票

Codexが計画外の調整を必要とした場合、固定パス`reports/charm-combo/revNN/change-tickets/<ticketId>.json`へschemaVersion 1のJSONを作成してClaude Codeへ返す。ticketIdは`UTC日時-対象key-連番`形式で、同一revision内の重複を禁止する。

| field | 必須内容 |
|---|---|
| schemaVersion / ticketId / ticketPath | 1、安定ID、上記固定相対path |
| selectedLockCommit | 発生時に選択されていたlock commit |
| originPhase / requestedRestartPhase | Phase 1〜7のenum。後者は§12-2に基づく要求巻き戻し先 |
| implementationCommit | Codex側コミット |
| affectedCharms | 対象keyの重複なしarray |
| changedArtifacts | path、before/after byteLength・SHA。未作成側はnull |
| inputBefore / inputAfter | 入力画像とSHA |
| contractChanged | sourceMode、座標、所有権、数式、検査、文言 |
| reason | 変更が必要になった実測・不具合 |
| approvalEvidenceId | 関連するユーザー承認ID。未承認ならnullと理由 |
| visualApproval | ユーザー承認内容とreference SHA |
| derivedImpact | prep、manual、CHARMS、DISPLAY_COLORS、配置 |
| verification | 検査1〜8・M/F・selftest・目視結果 |
| planSections | 更新が必要な節 |
| stopStatus | 計画改訂まで停止したか |
| ticketPayloadSha256 | 本フィールドを除外したcanonical JSONのSHA-256 |

ticketのpathはPOSIX相対path、絶対path・`.`・`..`・symlink禁止とする。canonical JSON規則は§2-4と同一である。changedArtifactsとverificationが参照する証跡ファイルは`reports/charm-combo/revNN/change-tickets/<ticketId>/`配下に置き、各path/SHAをticketへ記録する。

停止時はimplementationCommitを親とし、ticket JSONとその証跡ファイルだけを変更するchangeTicketCommitを作成する。`git show <changeTicketCommit>:<ticketPath>`からpayload SHA、selectedLockCommit、originPhase、implementationCommit、artifact SHAを再検証できなければFable 5は受領しない。計画外の実装を未コミットのままticketへ混ぜず、元worktreeへ自動反映しない。

Fable 5は還流票を受けたら、少なくとも次を実施する。

1. 採用素材台帳を更新
2. 影響する技術契約を更新
3. フェーズゲートと受け入れ基準を更新
4. 数量・文言・転記項目を連動更新
5. Codexレビュー ok:trueを取得
6. 計画コミットをCodexへ通知

### 12-1. 再改訂・再ロック手順

正本、入力、派生規則、受け入れ基準、実装契約のいずれかへ影響する還流票を採用した場合、既存revisionへ追記して再利用しない。次の手順を必須とする。

1. §12-2でrestartPhaseを先に確定し、planRevisionを必ず1増分して報告先を`reports/charm-combo/revNN/`へ切り替える。旧lock commitを参照するhandoffは全てstaleとし、以後の入力へ使わない
2. 還流票がある場合は検証済みchangeTicketCommit、還流票がない場合は直前に外部選択したphaseEvidenceCommitを`restartAnchorCommit`とする。親が一つで旧selectedLockCommitを祖先に持つことを検証し、supersession.jsonへ種別とSHAを記録する
3. restartPhase=Phase 1の場合は、restartAnchorCommitを唯一の親として新canonicalSource/reference/chart/mask/grain、freeze/candidate JSONだけを新sourceFreezeCommitへ固定する。§5の全PASS式を再実行し、新phase-1-handoffだけを新sourceFreezeCommitの唯一の子である新phase-1 evidence commitへ固定する。Fable 5またはユーザーがそのevidenceを外部選択し、親・許可path・payload・全SHAを検証するまで文書改訂へ進まない。新documentCommitの唯一の親はこのselected new phase-1 evidenceとする
4. restartPhaseがPhase 2以降の場合は、既存sourceFreezeCommitの全SHA・PASS式とrestartAnchorCommitからの祖先関係を再検証する。新documentCommitの唯一の親はrestartAnchorCommitとする
5. 正本計画、v2指示書、旧指示書の失効メタデータを新revisionへ更新し、手順3または4で定めた唯一の親の直後に3文書だけを新document commitへ固定する。merge commitは禁止する
6. 新document commitそのものをCodexレビューし、ok:trueまで反復する。修正が必要なら同じ一本道上に新document commitを作り直してレビュー対象を更新する
7. 最終review結果をdocument commitの唯一の子であるreview evidence commitへ固定し、新lock manifestへ直前のselectedLockCommitを`supersedesLockCommit`、手順3または4で選んだsourceFreezeCommitを`sourceFreezeCommit`として記録する。review evidence commitを唯一の親とする単一lock commitを作成する
8. validなlock候補が複数なら停止し、Fable 5またはユーザーの明示選択を新phase-2-handoffへ記録する
9. `reports/charm-combo/revNN/supersession.json`へ旧selectedLockCommit、新selectedLockCommit、restartAnchorCommit、任意のchangeTicketCommit、還流票path/payload SHA、旧・新sourceFreezeCommit、変更分類、restartPhase、失効handoff一覧、失効成果物commit、carry-forward候補を記録する
10. 新phase-2-handoffとrevision-lock-verificationを再生成し、selectedLockCommitを唯一の親とするphase-2 evidence commitへ固定する。新lock、handoff、phase-2 evidenceが手順3または4で選んだsourceFreezeCommitを参照し、そのcommitが全ての祖先であることを検証する
11. restartPhaseより前の成果物だけは、新契約でSHA・数量・受け入れ条件を再照合し、PASSしたものをcarry-forwardできる。この場合、旧output commitへ枝分かれせず、新phase-2 evidence commitを起点に、各carry-forward output commitを直前の外部選択済みevidence commitの唯一の子として順に作る（内容変更がなければempty commit可）。各再発行handoff evidence commitは対応するcarry-forward output commitを唯一の親とする。直接親と`git merge-base --is-ancestor`を各段で検証し、黙って旧handoffを流用しない。restartPhase=Phase 1ではcarry-forwardを禁止する
12. restartPhase以降の成果物、承認、commit、レポートは無効として再実行する
13. 各フェーズ開始時にselectedLockCommit、選択された新旧sourceFreezeCommit、toolCommit、assetCommit、charmsCommitの適用対象と一本道の祖先関係を再照合する

旧handoffファイルと旧lock commitは監査証跡として履歴に残し、内容を書き換えない。失効はsupersession.jsonと、新しいhandoffのselectedLockCommit不一致判定で機械的に表現する。

### 12-2. 変更分類と巻き戻し表

| 変更内容 | restartPhase | 無効化する主な成果物 |
|---|---:|---|
| canonicalSource / canonicalReference / color chart / color freeze / SHA / 実寸 / sourceMode / toneMode / material・色の採用 | Phase 1 | source/color freeze、Phase 1〜7の全handoffと全派生成果物 |
| 計画権限、数量、実装指示、受け入れ基準だけの変更 | Phase 2 | 旧lockとPhase 2以降のhandoff。後続成果物は再照合後のみcarry-forward可 |
| INPUT_MANIFEST、CHARM_SPECS、COLOR_REFERENCE_MANIFEST、LEATHER_VARIANTS、MATERIAL_PROFILES、schema/tool、PNG、F oracle、Worker、検査実装 | Phase 3 | tool commit、Phase 3〜7のhandoff、以降の派生物 |
| manualレイヤ、base/masks/prep、material asset/grain、defringe、aspect、referenceIvory、DISPLAY_VARIANTS導出 | Phase 4 | asset commit、shape承認、charms commit、色レビュー |
| asset不変で承認状態・比較基準・形状QAコメントだけ変更 | Phase 5 | shape approval以降 |
| LAYOUT、丸カン、N、配置、runtime recolor、cache、世代token、UI、文言 | Phase 6 | charms commit、Phase 6〜7のhandoff、色レビュー |
| client色承認・色コメントだけ変更し、生成契約とreferenceIvoryは不変 | Phase 7 | color reviewとPhase 7 handoff |

複数分類にまたがる場合は最も早いPhaseへ巻き戻す。分類不能または入力画像への影響が否定できない場合はPhase 1へ戻す。色変更がreferenceIvory、DISPLAY_COLORS導出、base/masks/prepへ影響する場合はPhase 7扱いにせずPhase 4へ戻す。

### 12-3. 再開ゲート

- 新revisionのCodexレビューがok:true
- 新lock manifestのpayload SHA、親commit、documents、countsがPASS
- supersession.jsonの旧・新selectedLockCommitがGit履歴と一致
- restartPhaseより前のcarry-forward handoffが新selectedLockCommitで再発行済み
- restartPhase以降の旧commitや旧handoffを入力として参照していない
- 専用worktreeが新selectedLockCommitを含みcleanである

一つでも満たさない場合、Codexは実装を再開しない。

## 13. コミット単位

推奨するローカルコミット順は次のとおり。

1. `sourceFreezeCommit`: Freeze approved charm sources and references
2. `phase-1 evidence`: Record source freeze handoff
3. `documentCommit`: Revise charm source and verification plan
4. `reviewEvidenceCommit`: Record reviewed plan evidence
5. `selectedLockCommit`: Lock revised charm contracts
6. `phase-2 evidence`: Record revision lock handoff
7. `toolCommit`: Sync preparation manifest for approved sources
8. `phase-3 evidence`: Verify synchronized preparation tool
9. `assetCommit`: Regenerate straight RGBA charm assets
10. `phase-4 evidence`: Verify eight-charm preparation assets
11. `shapeApprovalCommit`: Record approved charm shapes
12. `phase-5 evidence`: Record shape approval handoff
13. `charmsCommit`: Expand charm combination preview
14. `phase-6 evidence`: Verify complete charm preview
15. `colorReviewCommit`: Record client color review
16. `phase-7 evidence`: Record final color handoff
17. `finalAcceptanceCommit`: Select verified final color evidence

push、main mergeはユーザー指示まで行わない。

## 14. 最終受け入れ条件

「27 variant提出完了」はPhase 7のsubmissionReady=trueで報告できる。「製品最終承認」はproductAccepted=trueの場合だけ成立し、両者を同じ完了状態として扱わない。

- 8種のpipelineSourceとapprovalReferenceがSHA付きで一意
- 5枚のcanonicalColorChart、27 leatherVariant、material profile、approvedHexがSHA/Evidence ID付きで一意
- 正本計画とINPUT_MANIFESTが一致
- sourceModeとtoneModeが全種確定
- swanのSHA、所有権、ロゴ、stitchMode、実寸が確定
- 全8種が新版schema/tool
- straight PNG最終画素で検査1〜8・M/Fを通過
- frenchie差し替え後のreferenceIvoryと全flat色を再導出
- 216有限値
- 81配置3条件
- CHARMS転記10項目がprepとdebug一致
- LEATHER_VARIANTS転記8項目とMATERIAL_PROFILES転記6項目がcolor freezeとdebug一致
- 全8種の形状・意匠をユーザー承認
- 8種×27 variantの216組をクライアント確認へ提出しsubmissionReady=true
- 製品最終承認時はapprovedCount=216、changesRequestedCount=0、pendingCount=0、productAccepted=true
- 色修正で契約または出力が変わった場合、新lockのrestartPhaseから再実行した最新Phase 7 evidenceがproductAccepted=true
- index.html、bags.htmlは無変更
- 計画からの逸脱は還流票と計画改訂で解消済み
- Phase 1〜7の全output/evidence commit対が§2-4の親子・許可path・payload SHA検証を通過
- Phase 1 handoffはselectedLockCommit=nullかつsource freeze証跡が一致し、Phase 2〜7の全handoffはselectedLockCommitが最新の選択lockと一致して旧handoffが入力へ混入していない
- 最終報告時はselectedPhase7EvidenceCommitを唯一の親とするfinalAcceptanceCommitが存在し、変更path、payload SHA、submissionReady／productAcceptedの報告区分が§11-4と一致する

## 15. Fable 5への実行指示

Fable 5は本計画を資料として、plans/charm-combo-display-plan.mdの次改訂と、新しい実装指示書を作成する。

ただし、Phase 1の採用素材台帳でpipelineSourceとapprovalReferenceが一意になっていないチャームは、推測で確定しない。候補、寸法、SHA、会話上の承認範囲をユーザーへ提示し、回答後に正本へ記録する。

正本計画を更新した後は、Codexレビューを最大5反復でok:trueまで収束させる。計画コミット番号と、Codex側でmerge後に最初に実施すべきフェーズを明記して引き渡す。
