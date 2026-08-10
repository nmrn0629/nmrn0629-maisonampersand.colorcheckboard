# チャーム組み合わせ表示 実装指示書 v2（Codex 向け・改訂18対応）

- 作成日: 2026-07-19（**改訂18対応更新: 2026-08-10 — Phase 4 検査8 の③再生成照合 FAIL（全8種・7種 = inspections17 fail-fast: logoRange・swan = manualRecipe 未定義参照の TypeError〔Cannot read properties of undefined (reading 'backgroundDistance')〕）の還流。検証 spec の合成を計画書 §0-F-1 の契約（CHARM_SPECS ツール定数層の合成・一致 assert 4点〔重複一致①〜③＋logoRoi↔logoRoiWork 恒等一致④〕）へ是正・selftest ⑬ 追加〔§0-F-2〕・toolVersion 0.9.0〔schemaVersion 6 不変・§0-F-3〕。生成規則・数量・色は不変**。**改訂17対応更新: 2026-07-30 — 還流票 923e2094 の受領と派生アセットの構造的欠陥の是正〔6種の革面欠落を `COLORLESS_CLOSED_WHITE` で解消・グレイン境界欠陥を `grainSourceInset: 2` で解消・輪郭線の `shadeMin: 0.78`・osanpo レシピ v2・horseshoe の `outlineCaptureRadius: 12`・swan くちばしのガイドライン除去と keep 化・全8種のロゴ契約確定・schemaVersion 6・toolVersion 0.8.0〕・計画書 §0-E**。改訂16対応更新: 2026-07-28 — Phase 4 実測の還流〔swan 実寸確定 W90×H90・frenchie cropSource・osanpo manual-owner 決定的レシピ・toolVersion 0.7.0〕・計画書 §0-D。改訂15対応更新: 2026-07-22 — protectedFiles の実在ファイル限定化・計画書 §0-C）
- 正本計画: `plans/charm-combo-display-plan.md` **revision 18**
- 進行契約: `plans/charm-combo-codex-claude-sync-plan.md`（同期計画）**Phase 3〜7・§2-4 commit chain**
- **技術値の優先関係**: 同期計画の権限は進行機構（Phase 順序・commit chain・handoff・専用 worktree・還流票）に限定され、**技術値（素材・色・検査・数量）は計画書 rev18 が優先**する。同期計画に残る referenceIvory / DISPLAY_COLORS / 216 / protectedFiles 2点（index.html・bags.html）等の旧値は **計画書 §8 の override map** に従い DISPLAY_VARIANTS / approvedHex / 192 / protectedFiles = [index.html] へ読み替える
- revision lock: `plans/charm-combo-revision-lock.json`
- 本書は旧 `plans/impl-instructions-charm-combo-display.md`（改訂8時点・7種/168組/64配置/転記9項目）を**置換**する。旧指示書は SUPERSEDED（DO NOT EXECUTE）であり、いかなる数量・手順も旧指示書から読まない

## 0. 依頼概要

あなたは、革小物ブランド「maison &.」カラーシミュレーターの**チャーム組み合わせ表示ページ**の実装を担当します。

- **作るもの**:
  1. `tools/charm-prep.html` の改訂18対応更新（Phase 3） — INPUT_MANIFEST 8種・COLOR_REFERENCE_MANIFEST・LEATHER_VARIANTS 27・MATERIAL_PROFILES 2・**schemaVersion 6**・**toolVersion 0.9.0〔改訂18 — 計画書 §0-F-3〕**・**改訂18分 = 検査8 再生成照合の検証 spec 合成の是正〔計画書 §0-F-1 — ツール定数層の合成＋一致 assert 4点〔重複一致①〜③＋logoRoi↔logoRoiWork 恒等一致④〕〕＋selftest ⑬〔同 §0-F-2〕**・**改訂17分 = ①`COLORLESS_CLOSED_WHITE` レシピの新設と6種（horse / frenchie / dachshund / toypoodle / cat / swan）への適用〔共通定数＋チャーム別定数は計画書 §0-E-1 が正本 — 完全一致で転記〕 ②グレイン構成へ `grainSourceInset: 2`〔§0-E-2〕 ③⑦' グレイン焼き込みへ `shadeMin: 0.78`〔正本式 = `gray = round(clamp(grainBase × shadeEff × grain, 0, 255))`・`shadeEff` は**厳密 one-hot 革所有画素（`masks.α ≥ 128 ∧ masks.R = 255 ∧ masks.G = 0 ∧ masks.B = 0`）でのみ `max(shade, shadeMin)`、それ以外の革主所有画素では `shade`**・§0-E-3・§5-2〕 ④osanpo レシピを v2 へ〔id `osanpo-colorless-20260730-v2`・ステッチ除外域を `dilate(logoMask, logoProtectDilatePasses = 2) ∪ dilate(hangingHoleRoi 矩形, exclusionDilatePasses = 2)` へ〔logo 保護と吊り穴除外の両方を含む — logoRoi 矩形の除外は廃止〕・`dilatePasses` 3→0・`stitchRange` 更新・§0-E-4〕 ⑤horseshoe レシピへ `outlineCaptureRadius: 12` / `outlineLumMax: 0.75`〔§0-E-5〕 ⑥swan へ `BEAK_OUTLINE_REMOVAL` と `BEAK_EXPECTED_DIFF`〔§0-E-6〕 ⑦cat / swan の `logoRoi`・全8種の `logoRange` と採用 threshold・`detectLogo` の keep 除外〔§0-E-8〕 ⑧prep.json を **schemaVersion 6** へ〔grain へ `grainSourceInset`・`effectiveWidth`・`effectiveHeight`・`effectiveRoiSha256`・`shadeMin` を追加・§0-E-9〕**。加えて swan の CHARM_SPECS 登録と、**改訂16分 = frenchie の cropSource 定数 {x:320, y:848, w:1105, h:1312}（計画書 §0-D-2・§5-4）・osanpo の manual-alpha / manual-owner 決定的レシピ `OSANPO_COLORLESS_MANUAL`（**改訂17 で id は "osanpo-colorless-20260730-v2" へ失効更新済み — 旧 id "osanpo-colorless-20260728-v1" をツールへ転記してはならない**。alpha 生成の定数とアルゴリズムは計画書 §0-D-3 が正本で完全一致転記、owner 生成は §0-E-4 の改訂を適用）・sizeMmMeasured 全8種 true（swan 受領日 2026-07-28・計画書 §0-D-1）・チェーン外追補 763cf1e の swan ステッチ判定 UI／selftest 復元堅牢化の正規取り込み（計画書 §0-D-4）**。**実装の正は本書＋計画書 rev18（§0-F）であり、Phase 3 の起点は新 revision lock が選択した selectedPhase2EvidenceCommit（§1）である。失効した rev17 toolCommit `0c5dbb5`（上記の改訂16分＋改訂17分を実装済み — straight RGBA・独立 F oracle・mutation・Worker 世代管理を含む）とその rev17 phase-3 evidence `cc5ce2b`（= 改訂18 の restartAnchorCommit・計画書 §0-F-4）・rev15 phase-3 evidence `e71507d`・チェーン外追補 `763cf1e` は読み取り専用の監査参照として内容を取り込み直してよいが、これらのコミットを直接の実装基盤・起点にしない**
  2. 派生アセット一式の再生成（Phase 4） — `source-photos/`（canonicalSource 8点・Phase 1 凍結済み）から `assets/charms/`（8種の base/masks/prep.json/manual-*）・`assets/rings/`（丸カン透過PNG 銀/金）を**全8種**生成
  3. `charms.html` の完成実装（Phase 6） — 左右チャーム（8種＋なし）× LEATHER_VARIANTS（material 絞り込み・現行 calf 24色）× 金具（丸カン銀/金）× ロゴカラー（金/銀）× ステッチ（白/黒）
- **技術仕様の正**: `plans/charm-combo-display-plan.md` revision 18（本書と矛盾したら**計画書を優先**し、矛盾に気づいたことを報告する）
- **実装前に計画書全文を精読すること**（特に §0-A・§0-B・§0-D・§4-1c・§5-2・§5-4・§5-6・§7・§9）

## 0-A. 数量契約（改訂18 — 計画書 §0-A と同一値・revision lock の counts と一致必須）

<!-- COUNTS-CONTRACT-BEGIN -->
| countKey | 値 |
|---|---:|
| planRevision | 18 |
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
  4. **selectedPhase2EvidenceCommit そのものを開始点とする専用 worktree**（`git worktree add -b feature/charm-combo-codex-rev18 <path> <selectedPhase2EvidenceCommit>`）が clean で、元 worktree が読み取り専用として台帳化済み
- Phase 3〜7 の生成・検証・コミットは**専用 worktree だけ**で行う。元 worktree への自動コピー・merge・restore は禁止
- **報告先は `reports/charm-combo/rev18/`**（計画書 §0-F-4・同期計画 §12-1 手順1 — **今回実施する Phase 3〜7 の新規成果物**〔evidence・handoff・検査レポート〕はこの配下へ固定する。rev14〜17 配下は過去改訂の証跡であり**新規追記・変更を禁止（読み取り専用の参照は可）** — とくに rev14 配下の Phase 1 成果物〔`phase-1-approvals.md`・`phase-1-handoff.json` 等の ancestry〕は現行契約が参照する歴史的親成果物であり、変更・追記せず §10 の参照リストどおり読み取りのみ行う。rev17 配下への複製・移動もしない）
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

- **Phase 3（ツール同期）**: INPUT_MANIFEST（8種＋grain 2点 — **値は freeze JSON から直接生成**・§2）・CHARM_SPECS 8種（swan 登録・全種 colorless/flat・**sizeMmMeasured 全8種 true〔swan 2026-07-28〕・frenchie cropSource {x:320, y:848, w:1105, h:1312}・osanpo manualRecipe = `OSANPO_COLORLESS_MANUAL`〔**改訂17 では id "osanpo-colorless-20260730-v2" が唯一の有効値。旧 id "osanpo-colorless-20260728-v1" は §0-E-4 で失効しておりツールへ転記してはならない（selftest ⑦ が id 一致で機械検出する）**。alpha 生成の定数・アルゴリズムは計画書 §0-D-3 の正本値を完全一致で転記し、owner 生成は §0-E-4 の2点の改訂〔ステッチ除外域 = dilate(logoMask, 2) ∪ dilate(hangingHoleRoi 矩形, 2)・stitch.dilatePasses = 0・更新後 stitchRange〕を適用する。manual-alpha（閉領域 flood-fill 前景化・吊り穴 α0）と manual-owner（破線 B・ロゴ G）を horseshoe と同一の補助関数群で機械生成・全 range 照合 fail-fast〕**）・COLOR_REFERENCE_MANIFEST・LEATHER_VARIANTS 27・MATERIAL_PROFILES 2・**schemaVersion 6**・**toolVersion "0.9.0"〔改訂18 — 計画書 §0-F-3。selftest の固定期待値（改訂17実装の旧固定値 0.8.0）も 0.9.0 へ連動更新し、prep-selftest.json と phase-3-handoff.json へ 0.9.0 の PASS 結果を記録する〕**・**6種の `manualRecipe` = `COLORLESS_CLOSED_WHITE`〔計画書 §0-E-1 の共通定数とチャーム別定数を完全一致で転記〕・osanpo の `manualRecipe` を v2 へ〔§0-E-4〕・horseshoe レシピへ `outlineCaptureRadius: 12`/`outlineLumMax: 0.75` 追加〔§0-E-5〕・swan へ `BEAK_OUTLINE_REMOVAL`／`BEAK_EXPECTED_DIFF`〔§0-E-6〕・cat/swan の `logoRoi` と全8種の `logoRange`〔§0-E-8〕・グレイン構成へ `grainSourceInset: 2`〔§0-E-2〕・⑦' へ `shadeMin: 0.78`〔§0-E-3〕** へ更新し、**763cf1e の swan ステッチ判定 UI／selftest 復元堅牢化を正規に取り込む**（計画書 §0-D）。**改訂18分 = 検査8 ③再生成照合の検証 spec 合成を計画書 §0-F-1 の契約へ是正する（検証 spec 合成関数へ CHARM_SPECS のツール定数層〔label・source・sourceMode・toneMode・sizeMm・sizeMmMeasured・attachmentMode・threadColor／stitchMode〔swan を除く7種〕・logoThreshold・logoRoi・logoRange・manualRecipe・cropSource・beakRemoval・stoneRoi・stoneLumMax〕を合成し、一致 assert 4点〔①sizeMm・sizeMmMeasured・attachmentMode ②threadColor・stitchMode〔swan 除く〕③crop の一意化〔frenchie = cropSource と完全一致・他7種 = 全面クロップ {x:0, y:0, w:原寸, h:原寸} と完全一致〕④logoRoi↔pipeline.logoRoiWork の恒等一致〔logoRoiSource を持たない現行全8種〕〕を新設する。prep.json スキーマ・生成規則・straight PNG 契約は変更しない — 計画書 §0-F-1／§0-F-3）**。**selftest=1 全通過**（**改訂16 追加: ①CHARM_SPECS.frenchie.cropSource が契約値 {x:320, y:848, w:1105, h:1312} と完全一致 ②osanpo レシピの id・全定数が計画書の契約値と完全一致 ③canonicalSource からの osanpo レシピ生成が全 range 照合 PASS、の3項目**。**改訂17 追加: ④**CHARM_SPECS.horse の `stitchMode` = "none"・`threadColor` = null・`stitchPathsWork` = [] が契約値と完全一致**〔§0-E-7 — 実測で破線ステッチ不在を確定済み。`detect` や `fallback` を設定してはならない〕、および `COLORLESS_CLOSED_WHITE` のレシピ id "colorless-closedwhite-20260730-v1"・共通定数〔backgroundDistance 14・autoAlphaLow 12・autoAlphaHigh 32・designPaddingPx 12・sealPasses 2・mainComponentAreaFraction 0.05〕と6種のチャーム別定数が計画書 §0-E-1 の契約値と完全一致 ⑤6種の canonicalSource からのレシピ生成が全 range 照合〔mainWhiteRange の area／bbox／**componentMin・componentMax**〔5種 1/1・swan 3/3〕・hangingHoleRange・minorClosedWhiteMaxArea〕PASS、かつ吊り穴が `hangingHoleRoi` 内包 ＋ `hangingHoleSeed` 最小距離規則で選択されていること ⑥`grainSourceInset` = 2・`shadeMin` = 0.78 が契約値と一致し、`effectiveWidth`／`effectiveHeight` が calf 1007×672・chevre 820×379 と再計算一致すること。加えて **canonicalSource から生成した全8種について、革面の grain 比 局所偏差（§0-E-2 の定義。rowDev と colDev の両方を算出）が `max(rowDev, colDev) ≤ 0.15` を満たすこと**（`grainSourceInset` を 0 にすると該当7種が +0.2177〜+0.2279 で閾値を超えることも併せて回帰検査し、閾値が実際に機能することを確認する） ⑦osanpo レシピ v2 の id・全定数・更新後 `stitchRange` が §0-E-4 の契約値と完全一致し、`dilatePasses = 0` で **final ＝ rawStitch の集合恒等性（final の B 画素集合が rawStitch 集合と画素単位で一致）**が成立し、**raw / final の実測値がいずれも更新後 `stitchRange` 内**であること（**9,659px / 28成分 は基準実装値〔PIL 縮小・§0-E-4 の測定系表〕の参照値であり、ツール selftest の合格条件ではない** — ツール実測値は縮小実装差により別値となり、その差は `stitchRange` が吸収する） ⑧horseshoe の **取り込み半径 `outlineCaptureRadius` = 12**（シルエットは外周方向へ最大 12px 拡張される）・`outlineLumMax` = 0.75・**評価専用の `outlineProbeRadius` = 8**（取り込み半径とは別物であり混同してはならない）が契約値と一致し、**canonicalSource から生成した全8種の外周取り残し率が ≤ 10.0%**（§0-E-5 の定義 — 評価 α ＝ alphaAfterCapture〔horseshoe は取り込み手順①〜⑤適用後・他7種は manual-alpha 完了後〕・outside / outerBg は外側背景限定・boundary は dilate4(outerBg, iterations = 1)。作業座標の canonicalSource 上で評価すること — base.png では α=0 の RGB が未定義）であること。**加えて horseshoe は固有閾値 ≤ 1.0% で判定し**（基準実装値 0.00%・§0-E-5 の実効性検証）、**`outlineCaptureRadius` を 8 に下げた対照生成では取り残し率 > 1.0% となること**（基準実装値 4.46% — 取り込み機構の実効性回帰・§0-E-5） ⑨swan `BEAK_OUTLINE_REMOVAL` の全定数が §0-E-6 の契約値と一致し、くちばしとストーンが manual-keep へ書き込まれて owner 生成の対象外になっていること（**swan の keep は この2集合のみ**・実測 3,629 + 1,515 = 5,144px が `keepPxCountWork` の期待値）。加えて `BEAK_EXPECTED_DIFF` の**5合格条件**（①工程②の有無で生成した α どうしの XOR がマスクと完全一致 ②α_with が α_without の部分集合 ③画素数が `beakExpectedDiffRange` 1,069〜1,307 内 **④⑧'-1 直後〔⑧'-2 適用前〕のアセット座標で `(⑧'-1(α_without)>0) XOR (⑧'-1(α_with)>0)` が `BEAK_EXPECTED_DIFF_ASSET` と画素単位で完全一致 ⑤同座標で `⑧'-1(α_with)>0` が `⑧'-1(α_without)>0` の部分集合**〔④⑤ は ⑧'-1 が最近傍写像で再標本化と XOR が可換であることによる厳密な恒等式・§0-E-6〕）を判定し、**作業座標版と `BEAK_EXPECTED_DIFF_ASSET`（⑧'-1 と同一の nearest 変換を適用したアセット座標版）の両方**について画素数・SHA-256・width・height（§0-E-6 の直列化規則）を出力すること ⑩**全8種の `logoPxCountWork` と成分数（作業座標・⑤ の検出／生成直後の G 所有画素）が §0-E-8 の `logoRange` 内**であること（**照合は作業座標のみ — アセット座標の `derived.logoPxCount` は記録のみ・§0-E-8**。horseshoe **3,378〜5,066** / 2〜4 成分・osanpo 1,000〜1,500 / 1〜2 成分は `manualRecipe` の G 所有画素に対する照合）。加えて **(a)** `detectLogo` を用いる6種の採用 threshold が契約値〔horse/frenchie/dachshund/toypoodle/cat = 18・swan = 10〕と一致し、**horseshoe / osanpo は `logoThreshold` が `null`**（両者は `manualRecipe` が G を直接生成するため threshold は非該当） **(b)** cat の `logoRoi` = [.4875, .7092, .5900, .7958]・swan の `logoRoi` = [.4630, .7412, .5590, .8083] が契約値と完全一致し、**swan の ROI 画素化〔§0-E-1 の floor/ceil 規則〕が作業座標 797×1200 で {x0: 369, y0: 889, x1: 446, y1: 970}〔半開〕を返すこと（§0-E-8 の宣言境界の機械照合）** **(c)** swan の `stoneRoi` = [.5245, .7600, .6155, .8158]・`stoneLumMax` = 0.965 が契約値と一致し、**ROI 画素化が {x0: 418, y0: 912, x1: 491, y1: 979}〔半開〕を返し**、ストーン選択（stoneRoi 内の輝度 < stoneLumMax の8近傍連結成分のうち**最大成分**、同面積なら最小 flat index。候補0件は生成エラー）が実測 1,515px を返すこと **(d)** `detectLogo` が keep 画素を基準色サンプリング・候補判定の双方から除外していること（除外の有無で swan の検出が 1,221px/6成分 → 898px/5成分 と変わることを回帰検査する — 数値は修正後 logoRoi での実測・§0-E-8） ⑪**混合所有画素数（`masks.α≥128 ∧ masks.R≥128 ∧ (masks.G>0 ∨ masks.B>0)`）= 0**〔§0-E-3〕、および**⑤-0 keepMask がパイプラインの ⑤（自動所有権候補）より前に確定し、⑤ の全候補生成（革・ロゴ・ステッチ）から keep 画素が除外されていること／⑥' 適用後の keep 集合が ⑤-0 の keepMask と画素単位で完全一致すること**〔§5-4 の ⑤-0・§0-E-6/§0-E-8〕 ⑫prep.json の `schemaVersion` = **6**〔§0-E-9〕、の9項目を追加。**改訂18 追加: ⑬検査8 再生成spec合成の網羅照合〔**正本 = 計画書 §0-F-2（本転記と §0-F-2 に差異がある場合は §0-F-2 に従う）** — ツールは**チャーム単位の帰属表（field attribution map — 全8種 × CHARM_SPECS[key] の own フィールド名 → §0-F-1 の実測入力層／ツール定数層のどちらか一方）を単一の定数**として持ち（swan の threadColor・stitchMode だけが実測入力層・他7種の同名フィールドはツール定数層というチャーム別例外を表単位で一意に表現・1フィールドの二重帰属は表の整合エラーとして FAIL・**帰属表は CHARM_SPECS から動的に導出せず独立した明示的な凍結定数〔全8種 × フィールド名の静的列挙〕として定義する〔動的導出は (i) を恒真化するため禁止〕**）、(i) 分類の完全性と排他性〔per-charm〕 = 全8種で CHARM_SPECS[key] の全 own フィールド名が帰属表の当該チャーム行に包含され、かつ各フィールドの層帰属が一意であること〔**未分類フィールド・二重帰属の存在は FAIL** — 将来 CHARM_SPECS へフィールドを追加した際の合成漏れを機械検出する〕 (ii) ツール定数層の一致〔per-charm・本番関数直呼び〕 = 生成直後の prep 相当レコード（**書き出しと同一の生成関数の出力**）から**本番の合成関数（verificationSpecFromPrep 相当）そのもの**を呼んで合成した検証 spec について、**チャームごとに** CHARM_SPECS[key] の own フィールドのうちツール定数層に分類される全フィールドが CHARM_SPECS[key] と jsonEqual であること（**ネスト構造は jsonEqual が丸ごと照合**・分類列挙に載っていても当該チャームに無いフィールドは対象外・swan の threadColor・stitchMode は実測入力層帰属のため対象外・**照合の写し実装を別に作らない** — この per-charm 照合により既分類フィールド名を別チャームへ追加した場合も当該チャームの照合が合成漏れを検出する） (ii)-b 実測入力層の構成一致〔per-charm〕 = 同じ合成 spec について、帰属表で実測入力層に分類される spec フィールド（calibration・stitchPathsWork・grain・swan の stitchMode／threadColor）が prep 相当レコードの対応記録値と jsonEqual であること〔**実測入力層フィールドを prep から構成しない実装・ツール定数で上書きする実装を検出する**。帰属表の各エントリは {field〔CHARM_SPECS[key] の own フィールド名〕, 層, specPath〔合成 spec 上の対応パス〕, prepPath〔prep 相当レコード上の対応パス・純ツール定数は null〕} を持つ固定 projection とし（**specPath はトップレベルの同名フィールド**。prepPath の全列挙: sizeMm → inputs.sizeMm・sizeMmMeasured → inputs.sizeMmMeasured・attachmentMode → inputs.attachmentMode・threadColor → inputs.threadColor・stitchMode → inputs.stitchMode・logoThreshold → pipeline.logoThreshold・calibration → inputs.calibration・stitchPathsWork → inputs.stitchPathsWork・grain → inputs.grain〔全 leaf — jsonEqual の丸ごと比較で leaf 網羅を担保〕・label → prep.label・source → prep.source.file〔**正規化規則: CHARM_SPECS.source = "../" + prep.source.file の恒等で比較** — 相対パス表現差は正規化してから一致判定〕・sourceMode → inputs.sourceMode・toneMode → derived.toneMode〔以上4件も1フィールド1エントリ・既存 assert の対象〕・manualRecipe／logoRange／logoRoi／cropSource／beakRemoval／stoneRoi／stoneLumMax → null〔純ツール定数〕。charmKey → key の一致は CHARM_SPECS の own フィールドではないため projection に含めず既存の独立 identity assert〔prep.charmKey = key = 合成 spec.key〕として扱う）、(i)(ii)(ii)-b・own-property 双方向検査・undefined 禁止は **projection に列挙された field／specPath／prepPath に限定**して適用する〔合成 spec 上の運用項目（key・logoRoiWork など CHARM_SPECS の own フィールドでないもの）は帰属表の対象外 — その検証は §0-F-1 の一致 assert ①〜④と §5-4 検査8 の既存照合が担う〕。(i)・(ii)・(ii)-b の照合では帰属表 projection の各エントリについて合成 spec 側 specPath と prep 側 prepPath〔≠ null のみ〕の own-property 存在を双方向に検査し、projection にあるフィールドが合成 spec に無い／CHARM_SPECS[key] の own フィールドが projection に無い／実測入力層エントリの prep 側対応記録の欠落はいずれも FAIL〔**undefined どうしの一致比較で通過させない**・projection 対象外の運用項目は検査しない〕。swan の prep 相当レコードは §0-D-4 の判定注入機構〔763cf1e の URL 指定〕で detect 固定ベクトル {stitchMode: "detect", threadColor: "light", stitchPathsWork: []}〔rev16 Phase 4 assetCommit e71a1d80 の swan prep.json 実績値〕を注入して他7種と同一の生成経路から作成する **Phase 3 専用の決定的 fixture** とし、fixture の判定値と PASS を prep-selftest.json へ記録する — Phase 4 の実判定を先取り・拘束せず、null は (iii)⑧ の負変異専用・Phase 4 実成果物・過去成果物のファイルにも依存しない〕 (iii) 実効性回帰12件〔①〜⑧・⑫は負変異・⑨〜⑪は追跡変異。**対象チャームは決定性のため固定**〕 = ①prep 相当レコードの inputs.sizeMm.w を +1〔**horse**・§0-F-1 新設①の回帰〕②pipeline.crop.x を +1〔frenchie・新設③〔cropSource 側〕の回帰〕③合成 spec から logoRange を削除〔**horse**・(ii) 照合の検出力回帰〕④合成 spec から stoneRoi を削除〔swan・swan 固有定数の代表〕⑤合成 spec の manualRecipe.id を別文字列へ改変〔**horseshoe**・専用レシピ・ネスト構造照合の代表〕⑥prep 相当レコードの pipeline.logoRoiWork の第1成分を +0.01〔**cat**・新設④〔logoRoi↔logoRoiWork 恒等一致〕の回帰〕⑦pipeline.crop.x を +1〔**dachshund**・cropSource を持たないチャームの新設③〔全面クロップ規範〕の回帰〕⑧合成 spec の swan stitchMode を CHARM_SPECS リテラル初期値（null）へ改変〔(ii)-b 照合の回帰・実効値が prep 記録から構成されることの検証〕がいずれも FAIL として検出されること（不一致を検出できなければ selftest 不合格）、⑨prep 相当レコードの inputs.calibration.reason へ固定 suffix「(selftest-mutation)」を追加〔osanpo・manual-line 契約の代表・record 検証を通過する摂動〕⑩inputs.grain.calf.grainBase を +1〔**toypoodle**・grain leaf の代表・検査F-2 の範囲検証を通過〕⑪inputs.stitchPathsWork へ固定ダミーパス1本 {points: [[100, 100], [200, 200]], widthMm: 1.0, dashMm: 2.0, gapMm: 2.0} を追加〔**horse**・頂点2点・作業座標・workSize 内で record 検証を通過・CHARM_SPECS 初期値 [] と確実に異なる値〕。**⑨〜⑪は追跡変異であり合成 spec の当該フィールドが変異後の prep 記録値と jsonEqual であることを合格条件とする**〔合成器が CHARM_SPECS の静的 seed／default 値を返す実装なら不一致になり FAIL・実測入力層が prep 由来で構成されることの実証〕⑫prep 相当レコードの horseshoe inputs.threadColor を "light" から "dark" へ改変〔負変異・CHARM_SPECS 不変のまま新設②〔threadColor・stitchMode の swan を除く7種一致 assert〕が FAIL することの回帰・通常生成では prep 値と CHARM_SPECS 値が一致するためこの変異が prep 側からの誤採用の検出力を実証する〕〕の1項目を追加** — 計画書 §0-D-2/3・§0-E-1〜9・§0-F）を確認し、`tools/charm-prep.html` だけを toolCommit へ固定。prep-selftest.json＋phase-3-handoff.json を phase-3 evidence commit へ。**レシピの定数（osanpo v2・6種の COLORLESS_CLOSED_WHITE・horseshoe・swan の各定数）は prep.json へ記録しない**（ツール定数 — 再現性は manual-*.png の SHA 経由で検査8 が機械検証。範囲・定数の変更が必要になったら還流票 → 再改訂・再ロック必須〔無断調整禁止〕。計画書 §0-D-3・§0-E-1/4/5/6）
- **チェックポイント①（Phase 4 冒頭・必須停止）**: canonicalSource 8点の SHA 照合結果（§4-1c マニフェストとの完全一致）を報告してユーザーの確認を待つ
- **Phase 4（全8種再生成）**: パイプライン①〜⑩（**① frenchie は cropSource 適用**・⑦' material 別グレイン焼き込み・⑧' 二段階アスペクト補正〔**swan は confirmed=true・preDistortion>3% のため適用対象**〕・straight RGBA 書き出し）で全8種を生成。検査1〜7・M・F（独立 F oracle＋**mutation 6種**込み〔**改訂17 で ⑥`shadeMin` 適用範囲誤りを追加** — 計画書 §5-4 の完全定義に従う〕）→ 書き出し → 検査8（三者照合・最終 PNG 独立デコード）。§7 チェックリスト・81配置3条件・fixedVisualCaseCount 確定・**swan の stitchMode 確定**（前回バッチ実測は detect 成立 — B所有 1,106・差分 1,106）・**osanpo はレシピ生成 manual-alpha / manual-owner で detect 成立を確認**（B>0・差分>0・threadColor="light"。生成画素数・成分数・検査5 実測値を prep-inspection-1-8.json へ記録 — 計画書 §0-D-3）・**frenchie は crop 後の §7-4 再判定（前景長辺 ≥600px）を記録**。**改訂17 追加: ①6種は `COLORLESS_CLOSED_WHITE` で革面が前景化されることを確認**（革面成分数〔swan は 3〕・成分別画素数・革面 bbox・吊り穴・微小成分が全 range 内。§0-E-1）・**②全8種の外周取り残し率を §0-E-5 の定義（評価 RGB = 作業座標の canonicalSource・評価 α = **取り込み適用後の alphaProbe〔horseshoe は取り込み手順①〜⑤適用後・他7種は manual-alpha 完了後 — §0-E-5 の alphaAfterCapture〕**・outside と boundary は外側背景に限定〔boundary は dilate4(outerBg, iterations = 1)〕・base.png では評価しない）で計測し ≤ 10.0% を合格条件とする（**horseshoe のみ固有閾値 ≤ 1.0% を併用**・§0-E-5 の実効性検証）。結果は prep.json の `inspections17.outerLeftover` と prep-inspection-1-8.json の双方へ記録（§5-4）**・**③全8種の `logoPxCountWork` と成分数（作業座標）を prep.json の `inspections17.logoInspection` と prep-inspection-1-8.json の双方へ記録（§5-4）し `logoRange` 照合（照合は作業座標のみ・アセット座標の `derived.logoPxCount` は記録のみ・§0-E-8）**。**採用 threshold は `detectLogo` を用いる6種のみ実数値を記録し、horseshoe / osanpo は `logoThreshold: null`**（非該当。実数値が入っていたら生成エラー）。**`keepPxCountWork`（作業座標）と `keepPxCount`（アセット座標）の両方を記録**し、**範囲照合は `keepPxCountWork` に対してのみ行う**（swan は くちばし 3,629 ＋ ストーン 1,515 = 5,144px・range 4,630〜5,658。§0-E-8・§0-E-6）・**④swan は `BEAK_OUTLINE_REMOVAL` 適用後にくちばしの完全保持・革面不変・前景1成分を確認し、`BEAK_EXPECTED_DIFF` の**5合格条件**（作業座標3件＋アセット座標2件・§0-E-6）の判定結果（真偽・XOR 不一致画素数〔合格時 0〕・部分集合違反画素数〔合格時 0〕）と、**作業座標版・アセット座標版（`BEAK_EXPECTED_DIFF_ASSET`）の両方**の画素数・SHA-256・width・height を **prep-inspection-1-8.json**（Phase 4 の許可成果物）へ記録**（§0-E-6。shape-approval.json はアセット座標版を Phase 5 で転記照合する）・**⑤グレイン構成記録**（grainSourceInset・effectiveWidth／effectiveHeight・effectiveRoiSha256・shadeMin・**革面の grain 比 局所偏差（§0-E-2 の定義。行方向 rowDev と列方向 colDev の両方を prep.json の `inspections17.grainLocalDeviation` と prep-inspection-1-8.json の双方へ記録〔§5-4〕し max(rowDev, colDev) ≤ 0.15 を合格条件とする）**。§0-E-2/3）・**⑥全8種の混合所有画素数 = 0 を確認し prep.json の `inspections17.mixedOwnershipPxCount` と prep-inspection-1-8.json の双方へ記録（§5-4）**（§0-E-3）。**上記②⑤の閾値超過・③の range 外・⑥の > 0 はいずれも生成エラー（fail-fast）**。素材検証レポートに計画書 §7「Phase 4 の成果物」④の**改訂16 追加分（osanpo レシピ記録・frenchie crop 記録・swan 補正記録）＋改訂17 追加分（上記すべて）**を含める。成果物だけを assetCommit へ固定（tools・handoff を混在させない）
- **チェックポイント②（Phase 4 完了時・必須停止）**: 素材検証レポート（8種構成 — 計画書 §7 の「Phase 4 の成果物」④）を報告し、**再撮影要否・例外承認の確認を得るまで Phase 5 の承認依頼に進まない**。「免除不可」項目の不合格は例外承認で通過できない
- **Phase 5（形状・意匠QA）**: 各チャームの4面比較 — ①pipelineSource ②approvalReference ③新アセットの **calf:ivory（`#ece7dd`）表示** ④新アセットの **calf:black（`#41403d`）固定表示**（計画書 §8 override map — chevre 適用チャーム0のため同期計画 §9-1 の「シェーブル代表色」面を置換。material 切替不変性検査は現行 N/A）。**swan ロゴの立体陰影の見た目もここで最終判断**（AE-20260718-07）。**改訂17 追加①: rev16 の shape-approval は全8種で失効しており、変更理由の有無にかかわらず全8種を再承認する**（計画書 §0-E-5/§0-E-11）。個別の変更理由は — horseshoe: シルエットが最大 12px 拡張／horse・frenchie・dachshund・toypoodle・cat・swan: 革面が新たに前景化／osanpo: レシピ v2 でステッチが点線・細身へ／swan: くちばしのガイドライン除去とくちばし・ストーンの keep 化／全8種共通: `grainSourceInset` と `shadeMin` により ⑦' の出力画素値そのものが変わる**改訂17 追加②: swan は `BEAK_EXPECTED_DIFF_ASSET`（アセット座標版・Phase 4 で機械判定済み・§0-E-6）の画素数と SHA-256 を shape-approval.json へ転記し、prep-inspection-1-8.json の記録値との一致を機械照合する。4面比較ではこのマスクを重ねて表示し「くちばし外周の変化は意図した差分」であることを承認者へ明示する。マスク外の形状変化が目視で見つかった場合は Phase 4 へ差し戻す**（§0-E-6）。全8種の形状承認まで charms.html 完成実装へ進まない
- **Phase 6（charms.html 完成）**: 代表版を8種＋なしへ拡張。LAYOUT・BitmapLRU・renderGen・cacheKey（`charmKey|variantKey|materialKey|logo|stitch`）・flatLeatherTone・capture renderJob・ImageBitmap.close・debug harness は既存実装パターンをコピーする（同期計画 §10-2）。§9 受け入れ基準 0〜13 をセルフチェック。**完了条件（改訂18 — 計画書 §0-E-11 の5条件を改訂17から不変のまま継承〔§0-F-4〕。判定対象は Phase 4 で確定した `CHARM_SPECS[key].stitchMode` から動的に決める）: ①全8種で革リカラーの出力差分画素数 > 0（24 calf variant の切替）②全8種でロゴ金／銀切替の出力差分画素数 > 0 ③`stitchMode ∈ { "detect", "fallback" }` の全チャームでステッチ白／黒切替の出力差分画素数 > 0 ④`stitchMode = "none"` の全チャームは**単独表示（もう一方は【なし】）で**ステッチ切替 UI が `disabled` かつ仮に切替しても**当該チャーム単位のビットマップ**の出力差分画素数 = 0（混在表示のトグル有効／無効は §4-2 のグローバル規則が正 — その状態でも none チャーム側のビットマップ差分 0 は同一に要求・計画書 §0-E-11 条件4） ⑤③と④の対象チャームの和が8種ちょうどで、`stitchMode` が3値のいずれでもない（null を含む）チャームが存在しないこと**。rev16 の Phase 6 は代表 horseshoe のみで停止していたため、8種すべてで上記を機械確認して報告する
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
| **独立 F oracle**（Float64・独立 mirror・外dy内dx逐次加算・非革画素不変全列挙）＋ **mutation 6種全検出**〔**改訂17 で ⑥`shadeMin` 適用範囲誤りを追加** — 計画書 §5-4 の完全定義を転記〕 | §5-4 |
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

**改訂14以降は canvas 経由のデコードを使わない**。書き出し済み PNG を**独立 PNG パーサ（parsePngRgba / decodeStraightPngBlob — 39fe815 実装済み）**で straight RGBA へデコードし、`crypto.subtle.digest("SHA-256", rgba)` を prep.json の期待出力ハッシュと照合する。straight marker（toolVersion 付き）・CRC・IHDR・IDAT 順序・filter・寸法・展開量の検査を含む。三者照合（入力照合〔マニフェスト込み〕・出力照合・再生成照合）は計画書 §5-4 検査8 の定義どおり。**③再生成照合の検証 spec は計画書 §0-F-1 の再生成spec合成契約（prep.json の実測入力層 × CHARM_SPECS のツール定数層・一致 assert 4点〔重複一致①〜③＋logoRoi↔logoRoiWork 恒等一致④〕）で構成する（改訂18）**。**8種すべて合格するまで派生アセットをコミットしない**。

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

- `plans/charm-combo-display-plan.md`（revision 18） — **技術仕様の正（最初に全文精読）**
- `plans/charm-combo-codex-claude-sync-plan.md` — 進行・commit chain・handoff・還流票の正本
- `plans/charm-approved-source-freeze.json` / `plans/charm-approved-color-freeze.json` — 素材・色の凍結正本
- `reports/charm-combo/rev14/phase-1-approvals.md` — 承認台帳（AE-20260718-01〜14）
- コミット 39fe815 の `tools/charm-prep.html` — straight RGBA・F oracle・mutation・Worker 世代管理の実装済み参照（`git show 39fe815:tools/charm-prep.html`）
- コミット 3cf9f1c の `charms.html` — 代表チャーム版（Phase 6 の拡張元）
- `index.html` — `metalTone()`・`extractKeyRingAsset()`（丸カン透過PNG生成に1回だけ使用）・CSS変数・スウォッチUI・`copySummaryText()`。**参照のみ、変更禁止（protectedFiles の唯一の対象・§1）**
- `bags.html`（**worktree には存在しない** — ローカルブランチ `ミニバッグ実装テスト-opus` のコミット `b29274f` を `git show b29274f:bags.html` で読む・同一リポジトリ内オブジェクト） — グループ選択UI・サマリー＋コピー・レスポンシブの流用元。**読み取り専用のコミット固定参照であり protectedFiles 対象外**（改訂15・計画書 §0-C-2。phase3StartHead の系統に bags.html は存在しないため byte 不変証明の対象にならない）

以上。丁寧に実装してください。各チェックポイントでの停止と日本語報告、Phase ごとの commit chain 契約（同期計画 §2-4）の遵守を忘れずに。
