# チャーム組み合わせ表示 実装指示書 v2（Codex 向け・改訂16対応）

- 作成日: 2026-07-19（改訂16対応更新: 2026-07-28 — Phase 4 実測の還流〔swan 実寸確定 W90×H90・frenchie cropSource・osanpo manual-owner 決定的レシピ・toolVersion 0.7.0〕・計画書 §0-D。改訂15対応更新: 2026-07-22 — protectedFiles の実在ファイル限定化・計画書 §0-C）
- 正本計画: `plans/charm-combo-display-plan.md` **revision 16**
- 進行契約: `plans/charm-combo-codex-claude-sync-plan.md`（同期計画）**Phase 3〜7・§2-4 commit chain**
- **技術値の優先関係**: 同期計画の権限は進行機構（Phase 順序・commit chain・handoff・専用 worktree・還流票）に限定され、**技術値（素材・色・検査・数量）は計画書 rev16 が優先**する。同期計画に残る referenceIvory / DISPLAY_COLORS / 216 / protectedFiles 2点（index.html・bags.html）等の旧値は **計画書 §8 の override map** に従い DISPLAY_VARIANTS / approvedHex / 192 / protectedFiles = [index.html] へ読み替える
- revision lock: `plans/charm-combo-revision-lock.json`
- 本書は旧 `plans/impl-instructions-charm-combo-display.md`（改訂8時点・7種/168組/64配置/転記9項目）を**置換**する。旧指示書は SUPERSEDED（DO NOT EXECUTE）であり、いかなる数量・手順も旧指示書から読まない

## 0. 依頼概要

あなたは、革小物ブランド「maison &.」カラーシミュレーターの**チャーム組み合わせ表示ページ**の実装を担当します。

- **作るもの**:
  1. `tools/charm-prep.html` の改訂16対応更新（Phase 3） — INPUT_MANIFEST 8種・COLOR_REFERENCE_MANIFEST・LEATHER_VARIANTS 27・MATERIAL_PROFILES 2・schemaVersion 5・**toolVersion 0.7.0**・swan の CHARM_SPECS 登録に加え、**改訂16分 = frenchie の cropSource 定数 {x:320, y:848, w:1105, h:1312}（計画書 §0-D-2・§5-4）・osanpo の manual-alpha / manual-owner 決定的レシピ `OSANPO_COLORLESS_MANUAL`（id "osanpo-colorless-20260728-v1"・定数一式とアルゴリズムは計画書 §0-D-3 が正本 — 定数の転記は §0-D-3 の値と完全一致させる）・sizeMmMeasured 全8種 true（swan 受領日 2026-07-28・計画書 §0-D-1）・チェーン外追補 763cf1e の swan ステッチ判定 UI／selftest 復元堅牢化の正規取り込み（計画書 §0-D-4）**。**旧 phase-3 evidence（e71507d）時点のツール＋763cf1e の内容を正として拡張する**（straight RGBA・独立 F oracle・mutation・Worker 世代管理は実装済み）
  2. 派生アセット一式の再生成（Phase 4） — `source-photos/`（canonicalSource 8点・Phase 1 凍結済み）から `assets/charms/`（8種の base/masks/prep.json/manual-*）・`assets/rings/`（丸カン透過PNG 銀/金）を**全8種**生成
  3. `charms.html` の完成実装（Phase 6） — 左右チャーム（8種＋なし）× LEATHER_VARIANTS（material 絞り込み・現行 calf 24色）× 金具（丸カン銀/金）× ロゴカラー（金/銀）× ステッチ（白/黒）
- **技術仕様の正**: `plans/charm-combo-display-plan.md` revision 16（本書と矛盾したら**計画書を優先**し、矛盾に気づいたことを報告する）
- **実装前に計画書全文を精読すること**（特に §0-A・§0-B・§0-D・§4-1c・§5-2・§5-4・§5-6・§7・§9）

## 0-A. 数量契約（改訂16 — 計画書 §0-A と同一値・revision lock の counts と一致必須）

<!-- COUNTS-CONTRACT-BEGIN -->
| countKey | 値 |
|---|---:|
| planRevision | 16 |
| charms | 8 |
| materialProfiles | 2 |
| leatherVariants | 27 |
| finiteCases | 192 |
| layouts | 81 |
| charmTransferFields | 10 |
| variantTransferFields | 8 |
| materialProfileTransferFields | 6 |
<!-- COUNTS-CONTRACT-END -->

- **finiteCases = 192**（8チャーム × calf 24 variant。chevre 3 variant は appliesToCharms=[] のため 216 ではない — AE-20260718-10・計画書 §0-A）
- 転記照合: CHARMS **10項目**（sizeMm・bboxPx・fgBboxPx・pxPerMm・attachmentMode・threadColor・stitchMode・stitchPaths・baseLum・toneMode）／LEATHER_VARIANTS **8項目**（variantKey・materialKey・colorKey・canonicalLabel・aliases・approvedHex・textureProfileKey・appliesToCharms）／MATERIAL_PROFILES **6項目**（materialKey・canonicalLabel・textureProfileKey・grainPolicy・grainSourceSha256・generationMode）
- 配置検査は **81配置**（9×9）の3条件（クリップなし・重なりなし・負の余白なし・計画書 §5-5）
- 旧指示書の 7種／168組／64配置／転記9項目は**すべて失効**

## 1. 開始条件・ブランチ運用と禁止事項

- **開始条件（同期計画 §7-1 — 全て満たすまで着手しない）**:
  1. revision lock（`plans/charm-combo-revision-lock.json`）の manifestPayloadSha256・documents 3点 SHA・counts の照合が PASS
  2. selectedPhase2EvidenceCommit の親子関係（親 = selectedLockCommit ただ一つ）・許可 path・handoffPayloadSha256 が PASS（`git show` ベース）
  3. dirty worktree の保全（同期計画 §7-0 の8手順 — バックアップ・SHA 照合・復元試験・ユーザー承認）が完了
  4. **selectedPhase2EvidenceCommit そのものを開始点とする専用 worktree**（`git worktree add -b feature/charm-combo-codex-rev16 <path> <selectedPhase2EvidenceCommit>`）が clean で、元 worktree が読み取り専用として台帳化済み
- Phase 3〜7 の生成・検証・コミットは**専用 worktree だけ**で行う。元 worktree への自動コピー・merge・restore は禁止
- コミットは同期計画 §13 の順（toolCommit → phase-3 evidence → assetCommit → phase-4 evidence → shapeApprovalCommit → phase-5 evidence → charmsCommit → phase-6 evidence → colorReviewCommit → phase-7 evidence）。各 phaseOutputCommit / phaseEvidenceCommit の親子・許可 path・handoffPayloadSha256 は同期計画 §2-4 の契約どおり。メッセージは英語
- **禁止事項（厳守）**:
  - `index.html` の変更（1バイトも変えない。**protectedFiles = [`index.html`] の1点のみ**〔改訂15・計画書 §0-C-1・§8 override map〕 — `git diff --exit-code <phase3StartHead> -- index.html` と byte SHA の両方で無変更を証明。**`bags.html` は phase3StartHead の系統に存在しないため protectedFiles 対象外** — 参照は §10 の読み取り専用参照のみ。同期計画の「index.html・bags.html の2点」記述は override map で index.html 単独へ読み替える）
  - main へのマージ・リモートへの push（ローカルコミットのみ。push はユーザー指示待ち）
  - フレームワーク・ビルドツール・npm の導入（1ファイル自己完結・vanilla JS を維持）
  - Canvas `toBlob` を base/masks/manual 派生 PNG の正規エンコーダーに使う（straight RGBA エンコーダー必須 — 計画書 §5-4）
  - sourceMode と toneMode を独立に組み合わせる（1:1 対応 — 検査M-2）
  - 画像差し替え後に旧 manual レイヤを座標変換せず流用する（**全8種とも旧素材由来の manual レイヤ・旧 prep.json・旧派生アセットは流用禁止** — 計画書 §0-B-3）
  - `CHARMS` テーブル・LEATHER_VARIANTS・MATERIAL_PROFILES の単独修正（修正は必ず prep.json / freeze 転記側の経路 — 計画書 §10）
  - 会話上の「OK」だけで実ファイルを推測する／古い JPEG や未承認候補へ黙ってフォールバックする（supersededSources は検査M-1 で機械排除される）
  - 計画書・同期計画・lock manifest の編集（矛盾・誤記を見つけたら停止・報告。修正は Fable 5 側の再改訂手続き）
  - **計画外の変更が必要になったら Codex 変更還流票（同期計画 §12）を作成して停止**（勝手に代替しない）

## 2. 入力素材の所在（最初に確認すること）

生成入力は **Git 管理下に凍結済み**（Phase 1・sourceFreezeCommit `8b3bdb41bd3205287f2612ca9fb071b4d53e8376`）。Git 管理外パスを参照する必要はない。

- **canonicalSource 8点**: `source-photos/{horse,horseshoe,frenchie,dachshund,toy-poodle,osanpo,cat,swan}-colorless.png`（全種 sourceMode="colorless"）
- **canonicalReference 8点**: `reference-visuals/charms/{key}-approved.png`（形状承認見本・source と同一 byte）
- **grain 2点**: リポジトリ直下の `質感.jpg`（calf — コピー・リネームせずそのまま canonical）／`reference-visuals/colors/grains/chevre-grain.png`（chevre）
- **canonicalColorChart 5点**: `reference-visuals/colors/chart-{red,yellow,brown,blue,chevre}.jpg`
- **色の正本**: `plans/charm-approved-color-freeze.json`（27 variants・2 materials・approvedHex）／**素材の正本**: `plans/charm-approved-source-freeze.json`
- **期待 SHA-256 の正本 = source freeze JSON**（8チャーム = entries[].pipelineSource／grain の byte = color freeze の grainSource.outputSha256）。計画書 §4-1c の表は同値の**参考転記**であり、INPUT_MANIFEST・照合定数は **freeze JSON の値を直接参照して生成**する（表経由の手写し禁止 — 不一致時は freeze が勝つ）。**作業開始時に実ファイルの SHA を freeze 値と照合し、1点でも不一致なら停止して報告**する

## 3. 実装の進め方（同期計画 Phase 3〜7 を順に実施）

各 Phase の開始時検証・成果物・handoff・停止条件は同期計画の当該節（§7〜§11）を正とする。以下は停止チェックポイントの要約。

- **Phase 3（ツール同期）**: INPUT_MANIFEST（8種＋grain 2点 — **値は freeze JSON から直接生成**・§2）・CHARM_SPECS 8種（swan 登録・全種 colorless/flat・**sizeMmMeasured 全8種 true〔swan 2026-07-28〕・frenchie cropSource {x:320, y:848, w:1105, h:1312}・osanpo manualRecipe = `OSANPO_COLORLESS_MANUAL`〔id "osanpo-colorless-20260728-v1"・定数一式とアルゴリズムは計画書 §0-D-3 の正本値を完全一致で転記。manual-alpha（閉領域 flood-fill 前景化・吊り穴 α0）と manual-owner（破線 B・ロゴ G）を horseshoe と同一の補助関数群で機械生成・全 range 照合 fail-fast〕**）・COLOR_REFERENCE_MANIFEST・LEATHER_VARIANTS 27・MATERIAL_PROFILES 2・schemaVersion 5・**toolVersion "0.7.0"** へ更新し、**763cf1e の swan ステッチ判定 UI／selftest 復元堅牢化を正規に取り込む**（計画書 §0-D）。**selftest=1 全通過**（**改訂16 追加: ①CHARM_SPECS.frenchie.cropSource が契約値 {x:320, y:848, w:1105, h:1312} と完全一致 ②OSANPO_COLORLESS_MANUAL の id・全定数が計画書 §0-D-3 の契約値と完全一致 ③canonicalSource からの osanpo レシピ生成が全 range 照合〔mainWhiteRange・hangingHoleRange・stitchRange・logoRange〕PASS、の3検証項目を含む** — 計画書 §0-D-2/3）を確認し、`tools/charm-prep.html` だけを toolCommit へ固定。prep-selftest.json＋phase-3-handoff.json を phase-3 evidence commit へ。**osanpo レシピの定数は prep.json へ記録しない**（ツール定数 — 再現性は manual-*.png の SHA 経由で検査8 が機械検証。範囲・定数の変更が必要になったら還流票 → 再改訂・再ロック必須〔無断調整禁止〕。計画書 §0-D-3）
- **チェックポイント①（Phase 4 冒頭・必須停止）**: canonicalSource 8点の SHA 照合結果（§4-1c マニフェストとの完全一致）を報告してユーザーの確認を待つ
- **Phase 4（全8種再生成）**: パイプライン①〜⑩（**① frenchie は cropSource 適用**・⑦' material 別グレイン焼き込み・⑧' 二段階アスペクト補正〔**swan は confirmed=true・preDistortion>3% のため適用対象**〕・straight RGBA 書き出し）で全8種を生成。検査1〜7・M・F（独立 F oracle＋mutation 5種込み）→ 書き出し → 検査8（三者照合・最終 PNG 独立デコード）。§7 チェックリスト・81配置3条件・fixedVisualCaseCount 確定・**swan の stitchMode 確定**（前回バッチ実測は detect 成立 — B所有 1,106・差分 1,106）・**osanpo はレシピ生成 manual-alpha / manual-owner で detect 成立を確認**（B>0・差分>0・threadColor="light"。生成画素数・成分数・検査5 実測値を prep-inspection-1-8.json へ記録 — 計画書 §0-D-3）・**frenchie は crop 後の §7-4 再判定（前景長辺 ≥600px）を記録**。素材検証レポートに計画書 §7「Phase 4 の成果物」④の**改訂16 追加分（osanpo レシピ記録・frenchie crop 記録・swan 補正記録）**を含める。成果物だけを assetCommit へ固定（tools・handoff を混在させない）
- **チェックポイント②（Phase 4 完了時・必須停止）**: 素材検証レポート（8種構成 — 計画書 §7 の「Phase 4 の成果物」④）を報告し、**再撮影要否・例外承認の確認を得るまで Phase 5 の承認依頼に進まない**。「免除不可」項目の不合格は例外承認で通過できない
- **Phase 5（形状・意匠QA）**: 各チャームの4面比較 — ①pipelineSource ②approvalReference ③新アセットの **calf:ivory（`#ece7dd`）表示** ④新アセットの **calf:black（`#41403d`）固定表示**（計画書 §8 override map — chevre 適用チャーム0のため同期計画 §9-1 の「シェーブル代表色」面を置換。material 切替不変性検査は現行 N/A）。**swan ロゴの立体陰影の見た目もここで最終判断**（AE-20260718-07）。全8種の形状承認まで charms.html 完成実装へ進まない
- **Phase 6（charms.html 完成）**: 代表版を8種＋なしへ拡張。LAYOUT・BitmapLRU・renderGen・cacheKey（`charmKey|variantKey|materialKey|logo|stitch`）・flatLeatherTone・capture renderJob・ImageBitmap.close・debug harness は既存実装パターンをコピーする（同期計画 §10-2）。§9 受け入れ基準 0〜13 をセルフチェック
- **チェックポイント③（Phase 6 中間・必須停止）**: 代表チャームの一気通貫実装と `?debug=1` 検査・**192組有限値検査**の通過を報告し、**見た目のテイストについてユーザーの OK を得てから**残りへ展開（テイスト調整で LAYOUT・丸カンアセット・N を変更した場合は 81配置検査を最初から再通過してから報告）
- **Phase 7（色クライアント確認）**: 8種 × 適用可能 variant = **192組**の確認シートを生成し color-review.json とともに colorReviewCommit へ。形状は変更しない
- `?debug=1` の自動検査（恒等式・転記照合10+8+6項目・有限値192・グリッド・マスク可視化）は**リカラーの目視チューニングより先に**実装する

## 4. 計画書で特に落としやすい契約（該当節を必ず読むこと）

| 契約 | 計画書の節 |
|---|---|
| **全8種 colorless/flat**（photo 契約は適用チャーム0の予備） | §0-B・§4-1c・§5-2 |
| 色の正本は **LEATHER_VARIANTS 27件の approvedHex**（旧 COLORS/DISPLAY_COLORS/referenceIvory は全廃） | §5-6 |
| **canonicalLabel「シエルブルー」**（「シェルブルー」は alias — UI 表示・サマリーとも新表記） | §5-6・§6-3 |
| chevre 3 variant は **appliesToCharms=[]**（UI 非表示・生成 base も calf のみ）。material 2種以上のときのみ革種セレクタ表示 | §5-6・§6-1 |
| **material 別 base 派生方式**（generationMode="baked-base"・命名 `{key}.base.png`=calf／将来 `{key}.{materialKey}.base.png`） | §5-2 |
| グレイン入力は **material ごと**（calf=質感.jpg〔リポジトリ直下〕／chevre=chevre-grain.png）・grainScaleMm は全チャーム全 material 共通 | §5-2 |
| **straight RGBA PNG**（toolVersion marker・独立最終デコード・派生値/検査は最終デコード画素が正・Canvas toBlob 禁止） | §5-4 |
| **独立 F oracle**（Float64・独立 mirror・外dy内dx逐次加算・非革画素不変全列挙）＋ **mutation 5種全検出** | §5-4 |
| **horseshoe 6ストーン位相検査**（work 8近傍6成分・union 32000〜34000・center-nearest 投影 bitset 完全一致・固定面積閾値の asset 直接適用禁止） | §5-4 検査6 |
| **Worker 世代管理**（新世代開始時に旧 Promise reject＋Worker terminate・operation token 照合・timeout 後の全解放） | §5-4 |
| masks.α は**保持フラグ**（α<128=保持・保持画素の masks.RGB は読まない）・所有権恒等式3条件・共通述語 `masks.α≥128` | §5-2 |
| フェザー帯も所有を持つ・出力α = base.α（二重減衰禁止）・baseLum は下位中央値 `floor((n-1)/2)` | §5-2 |
| calibration の正本は prep.json のみ・**osanpo は manual-line 必須**（計測線は新素材上で新規指定 — 旧座標流用禁止）・**osanpo のステッチは自動検出不成立が確定 — manual-owner 決定的レシピで B 所有を機械生成**（detect 契約・検査5 の条件式は不変） | §5-2・§0-D-3 |
| 検査5 は**完全列挙型ゲート**（detect/none/fallback の条件セット不合致・未知値は即不合格）。swan の stitchMode は Phase 4 判定 | §5-2・§5-4 |
| ⑧' 二段階（⑧'-1 幾何変換〔colorless の base.RGB=nearest〕→ ⑧'-2 補正後α基準の再defringe〔④と同一関数〕）・**sizeMmMeasured は全8種 confirmed=true（改訂16）— swan は preDistortion 7.081% のため補正適用対象・補正後値で検査3 判定** | §5-2・§0-D-1 |
| **frenchie の cropSource {x:320, y:848, w:1105, h:1312}**（①で白余白を除去 → 長辺≤1200 縮小 → §7-4〔前景長辺≥600px〕を拡大なしで満たす。判定基準は不変・検査免除なし） | §5-4・§0-D-2・§7-4 |
| 配置式は軸分離（y=前景 fgBboxPx 上端・x=革本体 bboxPx）・コンテンツ縦センタリング・81配置3条件 | §5-5 |
| キャッシュキー **`charmKey|variantKey|materialKey|logo|stitch`**（丸カン金具は含めない）・LRU 8・退避時 `ImageBitmap.close()`・世代トークン | §5-7 |
| サマリー文は §6-3 を一字一句（色名=canonicalLabel・材質表記は material 2種以上のときのみ・ロゴ/ステッチ表記の条件） | §6-3 |
| 転記照合 **10＋8＋6項目**を `?debug=1` が機械照合（通常表示では prep.json を読まない） | §5-2・§5-6・§9-0 |

## 5. コード例・定義例

### 5-1. flatLeatherTone（全8種の革リカラー・計画書 §5-6 の式が正本）

計画書 §5-6 の疑似コードを**そのまま**実装する（Lab 変換 D65・ratio クランプ [0.35, 1.60]・ゼロ除算ガード 0.001・色域外は L/h 固定 C 二分探索固定20回・`Math.round` 8bit 量子化）。入力 hex は `DISPLAY_VARIANTS[variantKey].approvedHex`。**革主所有 ∧ base.α=255 の画素は 256 エントリ LUT 可**（完全一致必須）・エッジ帯は直接計算。

`photoLeatherTone` は将来素材用の予備契約（適用チャーム0）。b29274f の係数ピン留め（ratio クランプ [0.35, 1.60]・ゼロ除算ガード 0.001・ハイライト保護 lum≥0.94・上限 0.38・遷移幅 0.06）・4引数 shadeFactor=1.0 固定の契約は**計画書 §5-3 のとおり**温存し、有限値単体試験のみ維持する。

### 5-2. LEATHER_VARIANTS / MATERIAL_PROFILES の転記元と合成規則

`plans/charm-approved-color-freeze.json` から**計画書 §5-6 の転記合成規則**で機械転記する（手写し禁止 — 転記スクリプトまたはコピー後の機械照合で `?debug=1` と同じ 8＋6項目照合を通すこと）:

- **LEATHER_VARIANTS 8項目**: textureProfileKey 以外の7項目 = freeze の variants から直接転記／**textureProfileKey = variant.materialKey で freeze の materials を引いた textureProfileKey**（variants 自体はこのフィールドを持たない — join 合成）
- **MATERIAL_PROFILES 6項目**: materialKey・canonicalLabel・textureProfileKey・grainPolicy = freeze の materials から直接転記／**grainSourceSha256 = materials[].grainSource.outputSha256**／**generationMode = 計画書 §5-2 の確定値 `"baked-base"`**（freeze に存在しないフィールド — 正本は計画書）
- **MATERIAL_PROFILES の非転記メタデータ**（grain 入力の原本 SHA/ROI・canonical output の path/寸法/SHA・許可 mode pair = colorless/flat のみ）も計画書 §5-6 の定義どおり profile 定数へ持たせ、検査F-3・selftest で照合する

計画書 §5-6 の27件表は参考転記であり、**不一致時は freeze JSON が勝つ**。

### 5-3. LAYOUT オブジェクト（幾何の正本・計画書 §5-5）

39fe815 の実装値を初期値とする: `stageMm {w:220, h:160}`・`pxPerMmStage 6`・`ringCenterXMm 110`・`ringOuterMm 42`・`gapMm 8`・`ringGapMm 3`。丸カン中心 y は固定値を持たず、コンテンツ縦センタリング式（計画書 §5-5）から導出する。**81配置検査の実行前に全値を固定**し、以後の変更は検査再通過が条件。

### 5-4. リカラー合成ループと renderJob（自足定義 — 旧指示書は参照しない）

計画書 §5-2 の合成式・§5-7 の性能契約を次の骨格で実装する（実装済みパターンの参照元は 3cf9f1c の代表チャーム版 charms.html と 39fe815 — SUPERSEDED 旧指示書は参照しない）:

```js
// 描画ジョブ: 入力イベントごとに世代を1回だけ進め、全選択状態の不変スナップショット renderJob を捕捉する。
// キャッシュキー生成と合成ループは各側の bitmapJob だけを読む（アセット遅延ロード中に選択が変わると、
// グローバル state を途中で読む実装ではキーと合成内容が食い違ったビットマップがキャッシュされ得る）
const gen = ++renderGen;                          // 入力イベントごとに1回だけ進める（左右の bitmapJob で ++ を繰り返さない — §9-9）
function makeBitmapJob(side) {
  if (side.charm === "none") return null;
  const variant = LEATHER_VARIANTS_BY_KEY[side.variantKey];   // approvedHex・materialKey・textureProfileKey を捕捉（§5-6）
  return {
    gen,
    charmKey: side.charm, charm: CHARMS[side.charm],
    variantKey: side.variantKey, materialKey: variant.materialKey,
    target: hexToRgb(variant.approvedHex),
    baseLum: MATERIAL_BASE_LUM[side.charm][variant.materialKey],  // materialAssets 由来の material 別 baseLum（計画書 §5-2 — base と対で捕捉。現行は calf のみ＝CHARMS.baseLum と同値）
    logo: state.logo, stitch: state.stitch,       // state を読むのはこの捕捉時の1回だけ
  };
}
const renderJob = { gen, metal: state.metal, left: makeBitmapJob(state.left), right: makeBitmapJob(state.right) };
// キャッシュキー（計画書 §5-7 — 丸カンの金具トグルは含めない）
const cacheKey = (job) => `${job.charmKey}|${job.variantKey}|${job.materialKey}|${job.logo}|${job.stitch}`;
// base は job.materialKey に対応する material 別 base をロードする（現行は全 variant が calf のため {key}.base.png）
```

合成ループは計画書 §5-2 の式が正本: 出力α = base.α（再減衰しない）／`masks.α < 128` は保持（base.RGB をそのまま・保持画素の masks.RGB は読まない）／それ以外は `革 = flatLeatherTone(base.RGB, job.target, job.baseLum) × (R/255) ＋ ロゴ = metalTone(base.RGB, job.logo) × (G/255) ＋ ステッチ = stitchColor(base.RGB, job.stitch) × (B/255)`（R+G+B=255 のため係数和は常に1。**job.baseLum は base と同じ materialKey の materialAssets 由来** — 計画書 §5-2）。左右の非同期処理がすべて完了した時点で `renderJob.gen === renderGen` を照合し、最新なら renderJob.metal の丸カンPNGとともにステージへ合成する（旧世代なら描画せず破棄）。

## 6. 期待出力ハッシュ照合の実施手順（コミット前検査8・計画書 §5-4）

**改訂14以降は canvas 経由のデコードを使わない**。書き出し済み PNG を**独立 PNG パーサ（parsePngRgba / decodeStraightPngBlob — 39fe815 実装済み）**で straight RGBA へデコードし、`crypto.subtle.digest("SHA-256", rgba)` を prep.json の期待出力ハッシュと照合する。straight marker（toolVersion 付き）・CRC・IHDR・IDAT 順序・filter・寸法・展開量の検査を含む。三者照合（入力照合〔マニフェスト込み〕・出力照合・再生成照合）は計画書 §5-4 検査8 の定義どおり。**8種すべて合格するまで派生アセットをコミットしない**。

## 7. 有限値検査（ゲート化・計画書 §9-13）

`?debug=1` の自動検査項目に組み込む: **8種 × 当該チャームの適用可能 variant（現行 calf 24）= 192組**の全列挙で flatLeatherTone（approvedHex 入力・当該 material profile の baseLum）の出力が有限値であること。加えて分岐強制単体試験3種（ratio 上限 1.60 クランプ・ratio 下限 0.35 クランプ・**色域外圧縮の二分探索が実際に実行される**こと — chevre:green `#00894c` を含む合成入力・実行カウンタ等の決定的手段で分岐実行を確認）。**192組すべて PASS が Phase 6 チェックポイント③の報告項目**。

## 8. 検証

- ローカルHTTPサーバ（例: `python3 -m http.server 8088`）経由で確認する（`file://` 直開きは不可）
- selftest=1（Phase 3/4 の各 tool 状態で全通過）・`?debug=1`（恒等式・転記照合10+8+6・有限値192・グリッド・マスク可視化）を**全通過**させる
- 計画書 §9 の受け入れ基準 **0〜13** を上から順にセルフチェックする
- ブラウザでの目視確認・性能計測・モバイル Safari 実機確認が環境的に実施できない場合は、**未実施項目を報告に明記**してユーザーの手動確認に委ねる（できたことにしない）

## 9. 実装報告（日本語で行うこと）

各チェックポイントの報告に加え、最終報告には以下を必ず含める:

1. stitchMode の確定構成（8種それぞれ — 特に **swan の判定結果と根拠画素**）と fixedVisualCaseCount のチャーム別内訳（40 か 44 かの確定）
2. Phase ごとの phaseOutputCommit / phaseEvidenceCommit の SHA と handoffPayloadSha256
3. チューニングした係数の最終値（grain パラメータ・stitchColor 陰影係数・描画解像度 N・LAYOUT 確定値 — 初期値から変えた項目はユーザー確認を得た旨とあわせて）
4. 性能計測値（キャッシュ済み切替10回中央値・初回選択 warm cache）と計測環境
5. 計画書 §9 受け入れ基準 0〜13 のチェックリスト結果（未達成・未実施項目は理由つきで）
6. ピークメモリ概算（計画書 §5-7 の定義どおり）
7. 計画からの逸脱（原則ゼロのはず。発生したら還流票 ticketId とあわせて）

## 10. リポジトリ内の参考ファイル

- `plans/charm-combo-display-plan.md`（revision 16） — **技術仕様の正（最初に全文精読）**
- `plans/charm-combo-codex-claude-sync-plan.md` — 進行・commit chain・handoff・還流票の正本
- `plans/charm-approved-source-freeze.json` / `plans/charm-approved-color-freeze.json` — 素材・色の凍結正本
- `reports/charm-combo/rev14/phase-1-approvals.md` — 承認台帳（AE-20260718-01〜14）
- コミット 39fe815 の `tools/charm-prep.html` — straight RGBA・F oracle・mutation・Worker 世代管理の実装済み参照（`git show 39fe815:tools/charm-prep.html`）
- コミット 3cf9f1c の `charms.html` — 代表チャーム版（Phase 6 の拡張元）
- `index.html` — `metalTone()`・`extractKeyRingAsset()`（丸カン透過PNG生成に1回だけ使用）・CSS変数・スウォッチUI・`copySummaryText()`。**参照のみ、変更禁止（protectedFiles の唯一の対象・§1）**
- `bags.html`（**worktree には存在しない** — ローカルブランチ `ミニバッグ実装テスト-opus` のコミット `b29274f` を `git show b29274f:bags.html` で読む・同一リポジトリ内オブジェクト） — グループ選択UI・サマリー＋コピー・レスポンシブの流用元。**読み取り専用のコミット固定参照であり protectedFiles 対象外**（改訂15・計画書 §0-C-2。phase3StartHead の系統に bags.html は存在しないため byte 不変証明の対象にならない）

以上。丁寧に実装してください。各チェックポイントでの停止と日本語報告、Phase ごとの commit chain 契約（同期計画 §2-4）の遵守を忘れずに。
