# チャーム組み合わせ表示システム 実装計画書

- 作成日: 2026-07-12（**改訂17: Codex変更還流票 923e2094 の受領と、原因調査で判明した派生アセットの構造的欠陥の是正（2026-07-30）** — ①**8種中6種の革面欠落**（manual-alpha レシピ不在のため描線だけが前景・前景充填率 3.0〜5.0%）を `COLORLESS_CLOSED_WHITE` レシピ新設で解消 ②**水平傷の真因はグレイン素材の境界欠陥**（canonicalSource に傷は不在・`質感.jpg` 最終行の飽和率100%）を `grainSourceInset: 2` で解消 ③**horseshoe 外周輪郭の切り落とし**（外周取り残し率 229.79%）を `outlineCaptureRadius: 12` で解消（0.00% へ） ④輪郭線の明度不揃い（チャーム間の p5 レンジ幅 0.46）を ⑦' の `shadeMin: 0.78` で是正（同 0.03 へ収束） ⑤osanpo レシピ v2（ロゴ周辺ステッチの B 所有化・`dilatePasses` 3→0 で点線化・細身化）⑥swan くちばしのガイドライン除去とくちばし・ストーンの keep 化 ⑦horse は `stitchMode "none"` 維持を実測確定 ⑧ロゴ契約を全8種で実測確定（cat は 0px・swan は 4px と破綻していた。swan のロゴは銀の線画で `detectLogo` は keep 除外が必須）⑨§4-1c の背景値記述を実測へ訂正（254 → 255・記述のみで freeze は不変） — 変更点の完全一覧は §0-E。**数量契約・色契約・freeze JSON は改訂16から不変**（planRevision のみ 16 → 17 増分・§0-A）。**`schemaVersion` は 5 → 6 へ増分**（⑦' の出力画素値を変える生成規則の変更＋grain 記録項目の拡張のため・§0-E-9）。検査条件式は §0-E-10 に列挙した7点（①検査F-2 の記録項目追加 ②⑦' の式と検査F-1 の更新 ③**新規検査4種**〔混合所有画素数 0・外周取り残し率 ≤10.0%〈horseshoe 固有 ≤1.0%＋radius 8 対照の実効性回帰〉・grain 比局所偏差 ≤0.15・全8種の logoRange 照合〕 ④Phase 4 の `BEAK_EXPECTED_DIFF` 判定 ⑤**`COLORLESS_CLOSED_WHITE` 新設に伴う6種の生成時 fail-fast 照合一式**〔mainWhiteRange・hangingHoleRange・minorClosedWhiteMaxArea — いずれも新設・§0-E-1〕 ⑥**osanpo `stitchRange` の更新**〔旧 18/34/14000/20000/1/3 → 新 22/34/8200/11500/20/34・§0-E-4〕 ⑦**ロゴ・保持装飾の照合ゲートの内訳**〔6種の logoRange＋採用 threshold・horseshoe logoRange 新設・logoThreshold null 照合・cat/swan logoRoi・swan stoneRoi とストーン選択・keep 除外回帰・keepPxCountWork 照合・§0-E-8〕）のみ変更・追加され、それ以外は不変。再改訂分類は同期計画 §12-2 の「実装指示」（Phase 2）＋「schema/tool・検査実装」（Phase 3）＋「manual レイヤ・base/masks/prep」（Phase 4）の複合 → **restartPhase = Phase 2**（canonicalSource・freeze JSON・sizeMm・grain 素材ファイルはいずれも不変のため「実寸」「canonicalReference」変更〔Phase 1〕には該当しない）。**改訂16: Phase 4 実測の還流 — swan 実寸の実測確定（W90×H90・2026-07-28 受領）・frenchie cropSource 契約・osanpo manual-owner 決定的レシピ（2026-07-28）** — 変更点の完全一覧は §0-D。**sizeMm の数値・数量契約・検査基準は改訂15から不変**（planRevision のみ 15 → 16 増分・§0-A）。再改訂分類は同期計画 §12-2 の「実装指示」（Phase 2）＋「CHARM_SPECS・schema/tool」（Phase 3）の複合 → **restartPhase = Phase 2**（最も早い Phase へ巻き戻し・§0-D 前文。sizeMm 数値不変のため「実寸」変更〔Phase 1〕に該当しない）。改訂15: protectedFiles 契約の実在ファイル限定化 — `index.html` の1点のみ（2026-07-22） — 変更点の完全一覧は §0-C。**技術値（素材・色・検査・数量）は改訂14から不変**（planRevision のみ 14 → 15 増分・§0-A）。再改訂分類は同期計画 §12-2 の「実装指示・受け入れ基準だけの変更」= restartPhase Phase 2。改訂14: **Phase 1 凍結素材への全面切替・material-aware 27 leatherVariant・192有限値・Codex実装還流（2026-07-19）** — 変更点の完全一覧は §0-B・数量契約は §0-A。正本入力は sourceFreezeCommit `8b3bdb41bd3205287f2612ca9fb071b4d53e8376` の plans/charm-approved-source-freeze.json / plans/charm-approved-color-freeze.json。〔注記: 以下の改訂13以前の履歴に現れる「受領待ち」「photo 6種」「再生成不要」「COLORS / DISPLAY_COLORS / referenceIvory」「216」等の状態・契約はすべて過去の経緯の記録であり、現行契約は改訂17の本文（§0-A・§0-B・§0-C・§0-D・§0-E・§4-1c・§5-2・§5-4・§5-6）を正とする〕。改訂13: **無着色素材モード（sourceMode="colorless" / toneMode="flat"）の導入・horseshoe 生成入力の切替・swan の正式対象化（2026-07-15）** — ①horseshoe の生成入力を旧黒革写真から**無着色デザイン素材** `charm一覧/horseshoe色透過.png`〔2020×2385px・RGB・αなし・SHA-256 は §4-1c に記録〕へ切替（Git管理名 `source-photos/horseshoe-colorless.png`・旧 horseshoe.jpg は参考保管のみ＝生成入力への使用禁止）。②ユーザー承認済みの表示方式（2026-07-15）を契約化: 質感.jpg は**色を捨て局所的な凹凸のみ**使用〔グレイン焼き込み⑦'・§5-2〕／**アイボリー= frenchie 実測 referenceIvory・他23色= COLORS 基準**の表示革色 DISPLAY_COLORS〔§5-6〕／**Lab 空間で質感を合成し sRGB 色域外は明度・色相を保ち彩度のみ必要最小限圧縮**〔flatLeatherTone・§5-6〕／**旧写真の色・明暗は一切使用しない**／上下配置は**丸カン＋チャーム前景のコンテンツ縦センタリング（チャームごとの余白均等化）**へ変更〔§5-5〕。③**swan を「保管のみ」から正式な表示・選択対象へ**（チャーム8種＋なし＝左右各9択・§4-1）。swan の無着色素材 `swan色透過.png` は**受領待ち**であり、解像度・入力ハッシュ・所有領域・校正値・baseLum 等は**受領後にのみ確定**（仮確定禁止・§4-1c）。**くちばしの所有（革連動 or 保持）は §11-13 のユーザー確認事項**（実画素確認でも一意に決まらない場合はフェーズ0停止）。④件数依存の更新: 派生アセット・CHARMS・prep.json 一式 7→8種／有限値検査 168→**192組**〔§9-13〕／配置検査 64→**81配置**〔クリップ＋重なり＋負余白の3条件・§5-5〕／§9-3 固定検証セットは swan の stitchMode 確定後に 40 または 44 ケース。⑤prep.json **schemaVersion 4**〔sourceMode・grain（軸別 grainScaleX/Y・pxPerMmWorkW/H・flatWhite 含む）・referenceIvory・displayColors・calibration.reason を追加。photo 6種もフィールド追記のうえ v4 へ更新〕・書き出し前**検査M〔全チャーム共通 — §4-1c 正規入力マニフェスト照合（旧 horseshoe.jpg / swan.jpg は禁止入力）＋sourceMode⇔toneMode 対応〕・検査F〔colorless 専用〕**新設・CHARMS 転記照合 **10項目**化〔toneMode 追加〕。⑧' アスペクト補正は colorless のみ base.RGB=nearest（無彩色性の維持）とし、photo の bilinear 等の座標系契約は不変。改訂12: **⑧'アスペクト補正の二段階化〔⑧'-1 幾何変換＋⑧'-2 再defringe〕（2026-07-14）** — 改訂10/11 取り込み後のフェーズ0生成で、⑧'幾何補正と検査1・2・3・5・6・7 は全7種合格したが、**補正適用5種が検査4〔エッジ帯 defringe〕に不合格**（d>32 画素率: horseshoe 9.43% / frenchie 0.726% / dachshund 19.23% / toy poodle 10.86% / cat 14.13%・基準 0.5% 未満）。原因は base.RGB の bilinear が α 境界を跨いで RGB を混ぜ、④の defringe 性が補正後画像で破れるため（最近傍算出の厳密化では解消しないことを実装報告で確認済み）。対処: **⑧' を「⑧'-1 暫定 bilinear／nearest 変換 → ⑧'-2 補正後α基準の再defringe〔④と同一規則・同一実装〕」の二段階として §5-2 に契約化**し、優先関係（α=255 画素は bilinear 値が最終・α<255 画素は再defringe 値が最終）・処理順（⑧'-2 完了後に派生値再算出→検査1〜7）・検査4 の判定対象（最終画像・0.5% 基準は不変）・検査8 と期待出力ハッシュの対象（最終画像）・schemaVersion 増分（生成規則の変更も増分対象）を明記。数値・prep.json フィールド構成・UI仕様への影響なし。改訂11: **horse / osanpo の実測寸法を受領（2026-07-13・horse W95×H70 / osanpo W92×H40）— §4-1 の確定値とmm単位で完全一致し、全7種の実寸が実測で確定**。sizeMmMeasured.confirmed を全7種 true に更新（§5-2 発動述語・prep.json 入力値）。数値変更なしのため校正・配置・受け入れ基準への影響なし。horse は実測比 0.3〜0.5% 歪みのため補正発動なし見込み・osanpo は manual-line のため shouldApply 対象外のまま（超過時は停止 → 個別設計）。改訂10: **アスペクト補正〔非等方・縮小のみ〕の導入（2026-07-13）** — フェーズ0の pxPerMm 歪み検査で実測確定済み5種が全てゲート3%超過〔horseshoe 8.7% / frenchie 4.0% / dachshund 6.1% / toy poodle 10.4% / cat 3.4%。Codex 計測＋メインセッション独立検算で一致確認〕。同環境の horse は 0.3〜0.5% で合格・革端マスクは目視で正確なため、撮影系の系統誤差ではなく置き方・革の柔らかさ由来の投影乖離と判断し、**実測寸法（改訂9で5種完全一致）を正とする矯正を §5-2 に契約化**（発動述語 shouldApply・縮小のみの補正式・離散変換の一意定義・作業/アセット座標系の命名・prep.json 記録・検査3/8 連動・§7-9 対処順路の更新・§5-5/§7-11/§9-2 の再検証経路）。sizeMm・ステージ寸法・配置式・受け入れ基準の数値は不変。horse / osanpo は実測依頼中〔2026-07-13〕。改訂9: **実装エージェント（Codex）フェーズ0報告の実測寸法〔2026-07-13 受領・5種〕を照合・記録** — horseshoe 46×54 / frenchie 60×75 / dachshund 90×60 / toy poodle 80×60 / cat 68×65 はいずれも §4-1 の確定値と**mm単位で完全一致（数値変更なし）**。horse / osanpo の2種は実測未受領のためストア記載ベースを維持（§4-1）。数値が不変のため、寸法連動の校正（§5-2）・配置式とステージ寸法（§5-5）・受け入れ基準（§9-1/9-2）はいずれも影響なし — 出典の裏付け記録のみの改訂で設計契約に変更なし。改訂8: §11-4/5/11 の回答〔2026-07-13〕反映 — 選択UI=プルダウン・初期表示=左horse×右horseshoe・UI表示名=画像ファイル名と同じ英語表記。改訂7までの設計は Codexレビュー改訂5=5回・改訂6=4回・改訂7=4回でいずれも `ok: true` 収束済み）
- 対象: クライアント要望①「チャーム組み合わせ」（優先度: 最高）
- 方式: **実物写真リカラー方式 × 派生アセット事前生成**（2026-07-12 ユーザー決定＋レビュー反映）
- 成果物の流れ: **本計画書のレビュー → 修正反映 → 実装指示書作成 → Opus 4.8 / Codex へ実装委任**（Fable 5 では実装しない・2026-07-12 決定の体制を踏襲）

---

## 0-A. 数量契約（改訂17 正本値 — revision lock manifest の counts と一致必須）

本節の表は**改訂17の数量契約の正本**であり、実装指示書 v2（plans/impl-instructions-charm-combo-display-v2.md）の同名テーブル・plans/charm-combo-revision-lock.json の `counts` と機械照合される（同期計画 §6-5 検証手順8〜9）。**planRevision 以外の数量は改訂14から不変**（§0-C-3・§0-D-6・§0-E-10）。

<!-- COUNTS-CONTRACT-BEGIN -->
| countKey | 値 |
|---|---:|
| planRevision | 17 |
| charms | 8 |
| materialProfiles | 2 |
| leatherVariants | 27 |
| finiteCases | 192 |
| layouts | 81 |
| charmTransferFields | 10 |
| variantTransferFields | 8 |
| materialProfileTransferFields | 6 |
<!-- COUNTS-CONTRACT-END -->

- **finiteCases = Σ variants.appliesToCharms.length = 24 calf variant × 8チャーム = 192**。chevre 3 variant は `appliesToCharms = []`（AE-20260718-10 — シェーブルが実物で使えるのは実装後に追加される別アイテムのみ）のため、同期計画 §5-1-8 の「適用対象制限」分岐により旧 216（8×27）を **192 へ置換**する。有限値検査（§9-13）・27 variant 色回帰・クライアント確認（Phase 7）の件数もすべて 192 を正とする
- charmTransferFields 10 = CHARMS 転記照合フィールド（§5-2: sizeMm・bboxPx・fgBboxPx・pxPerMm・attachmentMode・threadColor・stitchMode・stitchPaths・baseLum・toneMode）
- variantTransferFields 8 = LEATHER_VARIANTS 転記照合フィールド（§5-6: variantKey・materialKey・colorKey・canonicalLabel・aliases・approvedHex・textureProfileKey・appliesToCharms）
- materialProfileTransferFields 6 = MATERIAL_PROFILES 転記照合フィールド（§5-6: materialKey・canonicalLabel・textureProfileKey・grainPolicy・grainSourceSha256・generationMode）
- layouts 81 = 左右 9択（8種＋なし）× 9択 の全配置（§5-5 の3条件検査）

## 0-B. 改訂14の変更点一覧（2026-07-19）

1. **生成入力を Phase 1 凍結素材へ全面切替**: 全8種の pipelineSource を sourceFreezeCommit `8b3bdb41bd3205287f2612ca9fb071b4d53e8376` で凍結済みの canonicalSource（`source-photos/{key}-colorless.png`・全種フチ画像由来）へ切替（§4-1・§4-1c。正本 = plans/charm-approved-source-freeze.json）。**全8種 sourceMode="colorless" / toneMode="flat"** となり、photo/photo の適用チャームは0（photo 契約は将来素材用の予備契約として温存）。承認見本は canonicalReference（`reference-visuals/charms/{key}-approved.png`・フチ画像兼用）
2. **swan の正本確定**: 「素材受領待ち」状態を解消。swan-colorless.png（1701×2560・SHA `77c8a315d37a0dad23eb3dd564bdd2baf7a26fe2f0583873de1ff2297461c435`）・くちばし＝アクセント色として**保持**（素材の着色を表示・AE-20260718-08）・ロゴ「&.」**あり**（立体シルバー表現のまま採用・G 所有・metalTone 連動・立体陰影の最終判断は Phase 5 QA・AE-20260718-07）・金具鋲＝保持（Phase 4 の所有権分類で機械確定）。旧 swan 素材（swan.jpg・フチ画像旧2版・背景透過）は supersededSources（生成入力禁止）
3. **photo 6種の「画像の再生成不要」条項（改訂13 §8）を撤回**: 全8種を改訂14の生成規則（straight RGBA PNG・schemaVersion 5）で再生成する。旧派生アセット（schemaVersion 4 以前・photo 由来・旧 horseshoe 成果物）と旧 manual レイヤ（座標系が旧素材由来のもの）の流用・混在を禁止する（§5-4・§8）
4. **色正本を material-aware な 27 leatherVariant へ移行**: COLORS 24色 → **LEATHER_VARIANTS 27件**（calf 24 + chevre 3・正本 = plans/charm-approved-color-freeze.json の approvedHex）・**MATERIAL_PROFILES 2件**（calf「カーフ」= 既存 質感.jpg 再利用〔AE-20260718-14〕／chevre「シェーブル」= 専用 chevre-grain.png〔distinct・AE-20260718-13〕）・**DISPLAY_VARIANTS**（approvedHex 正本）が旧 referenceIvory / DISPLAY_COLORS を置換（§5-6。frenchie 実測 referenceIvory 契約は全種 colorless 化により実測根拠を失い撤回）。**「シエルブルー」を canonicalLabel とし「シェルブルー」は alias**（UI 表示も canonicalLabel へ変更）
5. **有限値 192**: chevre 3 variant の appliesToCharms=[]（AE-20260718-10）により 216 → **192**（§0-A）
6. **Codex 実装（コミット 39fe815）からの技術還流を契約化**（§5-4）: straight RGBA PNG エンコーダー＋独立最終デコード検査／独立 F oracle＋mutation 試験5種（**改訂17 で 6種へ拡張 — §5-4**）／horseshoe 6ストーン位相検査（検査6）／production・oracle Worker の世代管理（中断・timeout・cleanup）。改訂12/13 の二段階アスペクト補正（⑧'-1/⑧'-2）と再defringe は §5-2 の本文どおり新版へ引き継ぐ
7. **schemaVersion 4 → 5・toolVersion 0.5.0 → 0.6.0**（全8種 colorless 化・material-aware 化・straight PNG 契約化による生成規則変更のため増分）
8. **cache key へ variant/material を追加**: 完成ビットマップの cache key は `charmKey|variantKey|materialKey|logo|stitch`（丸カン金具は従来どおり含めない・§5-7）
9. **革種→色の2段選択 UI**: 各サイドの state は variantKey を持ち、material を選ぶとその material に属する色だけを表示する。**選択可能 material が2種以上のときのみ革種選択 UI を表示**（現行8チャームは calf のみ＝革種 UI 非表示でカーフ24色のみ表示・AE-20260718-10・§6-1）
10. **実装フェーズの正本を同期計画へ接続**: 実装は同期計画（plans/charm-combo-codex-claude-sync-plan.md）の Phase 3〜7 に従う（§8）。計画外変更は Codex 変更還流票（同期計画 §12）で停止・還流し、再改訂・再ロック（同 §12-1・§12-2）を経てから再開する
11. **実装指示書の失効と v2 化**: 旧 plans/impl-instructions-charm-combo-display.md は SUPERSEDED（DO NOT EXECUTE）とし、plans/impl-instructions-charm-combo-display-v2.md を新正本とする。数量契約は本計画書と v2 指示書の2正本だけから抽出する（同期計画 §2-1）

## 0-C. 改訂15の変更点一覧（2026-07-22）

1. **protectedFiles を実在ファイルへ限定（[`index.html`] の1点のみ）**: 同期計画 §7 の固定配列 `protectedFiles` は `index.html`・`bags.html` の2点だが、`bags.html` は phase3StartHead（= selectedPhase2EvidenceCommit）の系統に**存在しない**（参照対象の Opus 版ミニバッグ実装はローカルブランチ `ミニバッグ実装テスト-opus` のコミット `b29274f` にのみ存在・main 系統へ未マージ。Codex 版ミニバッグは別ブランチ〔68d7011 系〕にあり、Opus 版/Codex 版の採用比較も未決定）ため、存在しないファイルの「無変更の byte SHA 証明」は構造的に実行不能（Codex の Phase 3 開始時検証で検出・2026-07-22）。protectedFiles は起点コミットに実在する `index.html` の1点のみとし、同期計画の該当記述は §8 override map で読み替える（同期計画自体は不変のまま保存）。**ミニバッグ実装が main 系統へマージされた後の再改訂で `bags.html` を protectedFiles へ追加する**
2. **bags.html の UI 流用元参照をコミット固定の読み取り専用参照へ変更**: v2 指示書 §10 の参考ファイル `bags.html` は worktree に存在しないため、`git show b29274f:bags.html`（ローカルブランチ `ミニバッグ実装テスト-opus`・同一リポジトリ内オブジェクト）による**読み取り専用参照**とする（photoLeatherTone 参照実装〔§5-3〕と同一のコミット固定参照形式）
3. **技術値は改訂14から不変**: 数量契約（§0-A）は planRevision の 14 → 15 増分を除き全て不変（charms 8・materialProfiles 2・leatherVariants 27・finiteCases 192・layouts 81・転記 10/8/6 項目）。生成契約・色契約・検査契約・UI 仕様・freeze JSON（source/color）にも変更なし。再改訂の分類は同期計画 §12-2 の「計画権限、数量、実装指示、受け入れ基準だけの変更」= **restartPhase Phase 2**（sourceFreezeCommit `8b3bdb41bd3205287f2612ca9fb071b4d53e8376` は不変・carry-forward）
4. **報告先を reports/charm-combo/rev15/ へ切替**（同期計画 §12-1 手順1）。Phase 1 成果物（承認台帳 phase-1-approvals.md・phase-1-handoff.json 等）は rev14 ディレクトリに凍結済みのまま参照する（Phase 1 ancestry は書き換えない）。Codex 専用 worktree のブランチ名は `feature/charm-combo-codex-rev15`（v2 §1）

## 0-D. 改訂16の変更点一覧（2026-07-28）

**Phase 4 バッチ実測の還流による再改訂**。Codex は rev15 の Phase 4 バッチ（toolCommit 6699b4f＋チェーン外ツール追補 763cf1e で実施・ユーザー報告 2026-07-28）で、horse / horseshoe / dachshund / toy poodle / cat の5種は Phase 4 バッチの検査ステップを PASS（実装報告の実行順番号で「検査1〜9」— §5-4 の正式検査番号〔書き出し前検査1〜7・M・F＋コミット前検査8〕とは別の番号付け。horseshoe は stitchMode=detect 成立・B所有 69,124px・白黒差分 69,124px）、swan / osanpo / frenchie の3種で契約どおり停止した（sizeMm・補正条件・検査基準の無断変更なし・還流はチャット報告 — 還流票なし）。本改訂はこの3件を解消する。restartPhase = **Phase 2**（§12-2: 「実装指示・受け入れ基準」の変更〔Phase 2〕と「CHARM_SPECS・schema/tool」の変更〔Phase 3〕の複合 → 最も早い Phase 2 へ巻き戻し。**sizeMm の数値は全種不変のため「実寸」変更〔Phase 1〕には該当しない** — canonicalSource / freeze JSON への変更なし・sourceFreezeCommit `8b3bdb41bd3205287f2612ca9fb071b4d53e8376` は不変・carry-forward）。

1. **swan 実寸の実測確定（検査3 停止の解消）**: クライアント実測 **W90×H90 を受領（2026-07-28）** — ストア記載（チャーム部 H9×W9cm = 90×90mm）と mm 単位で完全一致し、**全8種の実寸が実測で確定**（sizeMm の数値は全種不変）。`sizeMmMeasured.confirmed` を **8種 true** に更新（swan 受領日 2026-07-28・§4-1・§5-2）。これにより swan の §5-2 発動述語が成立し（calibration.mode="bbox" ∧ confirmed=true ∧ preDistortion 7.081%〔Phase 4 実測値〕> 3%）、**アスペクト補正が自動発動**して素材（フチ画像 1701×2560・前景比率 W:H ≈ 1:1.070）の縦横比を実測 1:1 へ矯正する。検査3 は補正後の再計測値で判定（§5-2 の既存契約のまま — **補正条件・検査基準の変更はない**）
2. **frenchie の cropSource 契約（§7-4〔前景長辺 600px〕停止の解消）**: Phase 4 実測で前景長辺 555px < 600px となった原因は、canonicalSource（1808×2664px）の**大きな白余白を含む全面が ① の長辺≤1200px 縮小の対象になった**こと（前景は 1024×1232px〔x 361–1384・y 888–2119〕で素材上は十分 — 2026-07-28 実測。1232 × 1200/2664 ≈ 555 で Phase 4 報告と一致）。対処は **CHARM_SPECS.frenchie.cropSource = {x:320, y:848, w:1105, h:1312}**（canonicalSource 座標系の固定矩形定数・§5-4）を指定し、①の既存クロップ機能で白余白を除去してから縮小する。**拡大処理は行わず、§7-4 の基準（背景除去後の前景長辺 ≥600px・アスペクト補正適用チャームは補正後 fgBbox で判定）は不変 — 検査免除は追加しない**。機械検算: crop 後長辺 1312px → 縮小 1200/1312 ≈ 0.9146 → 前景長辺 ≈1127px ≥ 600px（アスペクト補正は W軸適用見込みのため長辺 H は不変）。**再素材依頼は不要**（canonicalSource の前景解像度自体は基準を満たすことを機械確認済み）。**cropSource の機械検証**: Phase 3 の selftest に「CHARM_SPECS.frenchie.cropSource が本契約値 {x:320, y:848, w:1105, h:1312} と完全一致」の検証項目を追加する（selftest=1 全通過が toolCommit の条件 — 同期計画 §7。UI 操作等で crop を変えた非正規生成は、検査8 の③再生成照合が CHARM_SPECS.cropSource を crop 入力として①から独立再実行するため prep.json 記録値との不一致で機械検出される — 検査8 の条件式自体は不変）
3. **osanpo の manual-owner / manual-alpha 決定的レシピ（検査5 停止の解消）** 〔**改訂17 注記: 本項のレシピ id `osanpo-colorless-20260728-v1` は改訂17で `osanpo-colorless-20260730-v2` へ失効した（§0-E-4）。alpha 生成の規則は不変、owner 生成のステッチ除外域と `dilatePasses` が改訂されている。本項の本文は監査証跡として変更しない**〕: Phase 4 実測で自動検出（⑤）が B所有 0px と確定（colorless の白革面×薄い破線のコントラスト不足 — §5-2 の「自動検出は不成立見込み」どおり）。**detect 契約・検査5 の条件式は変更せず**、Phase 3 で `tools/charm-prep.html` に **osanpo 専用の seed ベース決定的レシピ `OSANPO_COLORLESS_MANUAL`（horseshoe 39fe815 方式に準拠・同一の補助関数群〔sampleBackground・smoothstep・flood-fill 到達判定・distanceFromTransparent(cap 70)・filterComponents(min 2)・dilateMask・sourceDistance・componentFeatherBands〕で実装）**を追加し、manual-alpha（革面の前景化）と manual-owner（破線ステッチの B 所有・ロゴの G 所有）を機械生成して detect を成立させる（§5-2 — detect の条件はマスクの由来を問わない）。
   - **レシピ定数一式（本改訂の正本 — ツールへ定数として転記し、Phase 3 selftest で完全一致を機械照合する）**: `id: "osanpo-colorless-20260728-v1"` / `backgroundDistance: 14` / `autoAlphaLow: 12` / `autoAlphaHigh: 32` / `featureDistance: 4` / `designPaddingPx: 12` / `mainWhiteRange: { areaMin: 120000, areaMax: 146000, widthMin: 400, widthMax: 435, heightMin: 660, heightMax: 700 }` / `hangingHoleSeed: [.495, .273]` / `hangingHoleRoi: [.465, .245, .525, .305]` / `hangingHoleRange: { areaMin: 850, areaMax: 1150, widthMin: 30, widthMax: 40, heightMin: 30, heightMax: 42 }` / `minorClosedWhiteMaxArea: 500` / `logoRoi: [.53, .27, .68, .43]`（既存 CHARM_SPECS.osanpo.logoRoi と同値） / `logoBoundaryMin: 45` / `logoRange: { countMin: 1000, countMax: 1500, componentMin: 1, componentMax: 2 }` / `stitch: { seedBoundaryMin: 15, seedBoundaryMax: 40, finalBoundaryMin: 12, finalBoundaryMax: 43, dilatePasses: 3, exclusionDilatePasses: 2 }` / `stitchRange: { rawComponentMin: 18, rawComponentMax: 34, finalCountMin: 14000, finalCountMax: 20000, finalComponentMin: 1, finalComponentMax: 3 }`（座標は作業座標 1200×1200 の比率・距離は作業px）
   - **抽出アルゴリズム（一意定義）**: (a) **alpha**: 四隅平均背景色 → passable（背景色距離 ≤ backgroundDistance）→ 画像縁から 4近傍 flood-fill で reachable → **closedWhite = passable ∧ ¬reachable** → 8近傍成分分類〔最大成分 = 革面（mainWhiteRange 照合）・hangingHoleSeed を含む成分 = 吊り穴（hangingHoleRange 照合・α=0・feather 帯は horseshoe と同一の componentFeatherBands）・その他は面積 ≤ minorClosedWhiteMaxArea のみ許容（ロゴ内白等・α=255）・超過成分は生成エラー〕→ manual-alpha へ closedWhite（穴以外）=255・design bbox（革面 bbox + designPaddingPx）内 = smoothstep(autoAlphaLow, autoAlphaHigh) の rawAutoAlpha・外 = 0 を書き込む。 (b) **owner**: 全画素 R=(255,0,0) 初期化 → buildMatte(autoAlphaLow, autoAlphaHigh)・boundaryDistance = distanceFromTransparent(matte.α, cap 70) → 除外域 =（logoRoi 矩形 ∪ hangingHoleRoi 矩形）を exclusionDilatePasses 回 dilate → **rawStitch = matte.α>0 ∧ boundaryDistance∈[seedBoundaryMin, seedBoundaryMax] ∧ ¬除外域 ∧ 背景色距離 > featureDistance** → filterComponents(min 2)・8近傍成分数を rawComponent 範囲と照合 → dilatePasses 回 dilate → **final = ∧ ¬除外域 ∧ boundaryDistance∈[finalBoundaryMin, finalBoundaryMax]** → B=(0,0,255) 指定・finalCount / finalComponent 範囲照合 → **logo = 背景色距離 > featureDistance ∧ boundaryDistance ≥ logoBoundaryMin** → logoRange 照合 → G=(0,255,0) 指定 → B∩G 重複 0 を検査。**いずれかの range 照合に失敗したら生成エラー（fail-fast — horseshoe 方式と同じ）**
   - **期待値範囲の根拠（基準実測 ＝ §0-E-4 が定義する「基準実装値」 — メインセッション独立 Python 実装・PIL bilinear 縮小・2026-07-28）**: 革面 closedWhite 132,865px / bbox 417×680・吊り穴 993px / 35×36・rawStitch 8,324px / 25成分・final stitch 16,758px / 1成分・logo 1,254px / 1成分（bbox 51×52・center (0.606, 0.333)）。**帯域の分離は構造的**（破線は bd[15,40] に集中・輪郭線 bd<10 との間に空隙 bd[10,15)=1px・ロゴは bd≥50 — featureDistance 3〜6・帯域±1 の摂動でも final 16,496〜17,116px / 1成分で安定）。range はツール（canvas 縮小）と基準実装（PIL）の縮小実装差を吸収する保守的マージンとして設定 — **範囲の変更・レシピ定数の変更が必要になった場合は還流票（同期計画 §12）→ 再改訂・再ロックを必須とし、実装時の無断調整を禁止する**
   - **prep.json との境界**: レシピはツール定数（manual レイヤの決定的生成手段）であり prep.json スキーマの外（horseshoe の manualRecipe と同じ扱い — パイプライン①〜⑩は生成済み manual-alpha.png / manual-owner.png を③⑥で適用するだけで、①〜⑩の生成規則・schemaVersion 5 のフィールド構成は不変〔§0-D-5〕。**レシピ定数は prep.json へ記録しない**）。再現性の機械検証経路: 生成された manual-*.png は既存契約どおり prep.json に SHA-256 で記録され（§5-4）、検査8 の入力照合・再生成照合で照合される — レシピが変われば manual-*.png の SHA が変わり検査8 で機械検出される。**Phase 3 selftest はレシピ id・全定数の契約値一致と、canonicalSource からの生成が全 range 照合 PASS になることを検証**し、**Phase 4 は生成画素数・成分数・検査5 結果（B所有 >0・白黒差分 >0・threadColor="light"）の実測値を prep-inspection-1-8.json（Phase 4 evidence の許可 path）と素材検証レポート（§7 Phase 4 成果物④）へ記録**する（素材裏付け: 輪郭内側の破線画素 32,550px〔canonicalSource 2304×2304px・8近傍連結成分 97個・2026-07-28 実測〕）
4. **チェーン外ツール追補 763cf1e の正規取り込み**: phase-3 evidence（e71507d）の後に積まれたコミット 763cf1e（swan の stitchMode Phase 4 判定 UI〔detect / none / fallback の確定 UI・stitchPaths エディタ・手動ブラシ〕・CHARM_SPECS.swan の stitchMode 初期値 null 化・selftest 復元堅牢化）は**チェーン外の WIP として失効**させ、**その内容は新 Phase 3 の toolCommit へ正規に取り込む**（supersession.json に記録。rev15 の eece717 と同じ扱い）
5. **toolVersion 0.6.0 → 0.7.0**（上記 1〜4 のツール変更に伴う増分・straight marker 連動・§5-4）。**schemaVersion 5 は不変**〔**改訂16 当時の記述 — 監査証跡・非規範。改訂17 の現行 schemaVersion は §0-E-9 の 6 が正であり、本項を現行契約として解釈してはならない**〕（①〜⑩のパイプライン生成規則に改訂16 時点では変更がなかった — 変わるのは CHARM_SPECS 定数〔frenchie cropSource・sizeMmMeasured・osanpo manualRecipe・swan stitchMode 初期値〕と prep.json 記録の入力値のみ。クロップ値は「①の回転・クロップ・縮小値」として従来から prep.json 記録対象・§5-4）
6. **数量契約は planRevision の 15 → 16 増分を除き全て不変**（§0-A）。生成契約・色契約・検査契約（検査1〜8・M・F の条件式）・UI 仕様・freeze JSON（source/color）にも変更なし
7. **報告先を reports/charm-combo/rev16/ へ切替**（同期計画 §12-1 手順1）。Codex 専用 worktree のブランチ名は `feature/charm-combo-codex-rev16`（v2 §1）。Phase 4 バッチで PASS した5種の検査結果は**旧 toolCommit 系統の実測記録**として参照し、新 toolCommit での Phase 4 再実行で全8種を再生成・再検査する（§8-1「全8種再生成」の既存契約のとおり）


## 0-E. 改訂17の変更点一覧（2026-07-30）

**Codex変更還流票の受領と、その原因調査で判明した派生アセットの構造的欠陥の是正**。Codex は rev16 の Phase 3〜5 を完走し Phase 6 実装中に、osanpo の①中央上部の水平傷と②ロゴ周辺ステッチが白糸選択でも黒く残る件について**変更還流票 `reports/charm-combo/rev16/change-tickets/20260730T081002Z-osanpo-asset-fix-01.json`（changeTicketCommit `923e2094fe5de1da1f433a3ad49b2a45c4dc743e`・implementationCommit `ac2e9e686faf2cf6c90e5745a66a2be96da1ccdd`）を提出して契約どおり停止**した。Fable 5 は §12 の受領検証（payload SHA 再計算・親子鎖・全 artifact SHA 実測照合・祖先関係）を**28項目 ALL PASS** で完了し受領した。

**受領後の原因調査（レシピの忠実再現ハーネスによる実測）で、還流票の記載を超える欠陥が判明した**。再現ハーネスは §0-D-3 の基準実測値**10項目すべてと完全一致**（mainWhite 132,865px / bbox 417×680・吊り穴 993px / 35×36・rawStitch 8,324px / 25成分・final stitch 16,758px / 1成分・logo 1,254px / 1成分）を確認済みであり、本節の数値はこの検証済みハーネス上の実測（＝ §0-E-4 が定義する**「基準実装値」**。ツール実測値との差については §0-E-4 の測定系表を参照）である。

判明した欠陥は次の3系統である。

- **(i) 8種中6種に革面（leather surface）が存在しない** — manual-alpha レシピを持つ horseshoe / osanpo だけが革面を前景化できており、horse / frenchie / dachshund / toypoodle / cat / swan は**描かれた線だけが前景**で、囲まれた白い革面が透明のまま残っていた（前景充填率 3.0〜5.0% 対 52.0〜56.3%）。リカラーしても輪郭線しか着色されない。**ロゴ検出（§0-E-8）と `flatWhite`（§0-E-3）も同時に破綻**していた。
- **(ii) 水平傷の真因は canonicalSource ではなくグレイン素材の境界欠陥** — 還流票は「canonicalSource から伝播した傷」としていたが、**canonicalSource の該当行は 255 単色で背景と同値であり傷は存在しない**（§0-E-12 の実測）。真因は `質感.jpg`（1011×676）の**最終行の輝度 0.9048**（画像平均 0.5023 の1.8倍）がグレインマップで**上限飽和率 100.00%** となることであり、**目視では osanpo のほか horse / frenchie / cat にも同一の横筋が確認でき（計4種）、測定上は horseshoe を除く7種すべてが閾値超過に該当**していた（還流票 1種／目視 4種／測定 7種の階層定義と全8種の実測は §0-E-2 の表が正本）。A/B 対照実験で因果を確定した（§0-E-2）。
- **(iii) horseshoe の外周輪郭がシルエットの外側に出ており切り落とされていた** — シルエットの外側 8px 以内に素材の描線が **8,284px（外周境界画素数の 229.79% に相当）** 取り残されていた（§0-E-5 の指標）。

**restartPhase = Phase 2**（§12-2: 「実装指示・受け入れ基準」の変更〔Phase 2〕＋「schema/tool・検査実装」の変更〔Phase 3〕＋「manual レイヤ・base/masks/prep」の変更〔Phase 4〕の複合 → 最も早い Phase 2 へ巻き戻し。**canonicalSource・freeze JSON・sizeMm・grain 素材ファイルはいずれも不変**のため「実寸」「canonicalReference」変更〔Phase 1〕には該当しない — sourceFreezeCommit `8b3bdb41bd3205287f2612ca9fb071b4d53e8376` は不変・carry-forward）。

**本節の全定数は改訂17の正本である**。ツールへ定数として転記し Phase 3 selftest で完全一致を機械照合する。**範囲・定数の変更が必要になった場合は還流票（同期計画 §12）→ 再改訂・再ロックを必須とし、実装時の無断調整を禁止する**。

### 0-E-1. 無着色素材の革面抽出レシピ `COLORLESS_CLOSED_WHITE`（新設・6種）

manual-alpha レシピを持たない6種（horse / frenchie / dachshund / toypoodle / cat / swan）へ、horseshoe / osanpo で実証済みの**閉領域 flood-fill（closedWhite）方式**を共通アルゴリズムとして適用する。`tools/charm-prep.html` に `COLORLESS_CLOSED_WHITE` を追加し、CHARM_SPECS の該当6種へ `manualRecipe` として結び付ける。

**レシピ id: `colorless-closedwhite-20260730-v1`**（Phase 3 selftest の照合対象）

**共通定数（6種で同一）**: `backgroundDistance: 14` / `autoAlphaLow: 12` / `autoAlphaHigh: 32` / `designPaddingPx: 12` / **`sealPasses: 2`（新規）** / `mainComponentAreaFraction: 0.05`

**`sealPasses` の必要性と根拠（新規定数）**: swan の外輪郭線には微小な隙間があり、封じ処理なしでは `backgroundDistance` = 14 のときに背景 flood-fill が革面へ漏れ込んで**革面成分が1つ丸ごと失われる**。

**変動率の定義（本節で共通）**: `backgroundDistance` を 12 / 14 / 16 と変えたときの革面総画素の **(max − min) ÷ max**。

| sealPasses | bg=12 | bg=14 | bg=16 | 革面成分数（12/14/16） | 変動率 |
|---:|---:|---:|---:|---|---:|
| 0（封じなし） | 268,202 | **182,339** | 182,577 | **3 / 2 / 2** | **32.01%** |
| 1 | 268,202 | 268,926 | 269,265 | 3 / 3 / 3 | 0.39% |
| **2（採用）** | **268,202** | **268,936** | **269,275** | **3 / 3 / 3** | **0.40%** |
| 3 | 268,202 | 268,947 | 269,286 | 3 / 3 / 3 | 0.40% |

封じなしの bg=14 では 268,202 → 182,339 と **85,863px 減少**し、これは革面成分が 3 → 2 へ減ること（1成分が背景として飲み込まれること）と一致する。`sealPasses` = 1 で既に安定するが、**素材の隙間幅に対する余裕を取って 2 を採用**する（3 でも結果は実質同じ）。**他5種は `sealPasses` 0 → 2 で出力が完全に不変（差 0px・±0.00%）** であることを実測確認済みであり、本定数は swan 以外へ非侵襲である。

**抽出アルゴリズム（一意定義）**:
(a) 四隅 5×5 平均を背景色 `background` とする（§0-D-3 の `sampleBackground` と同一）。
(b) `passable` = 背景色距離 ≤ `backgroundDistance`。
(c) `barrier` = ¬`passable` を **`sealPasses` 回 8近傍 dilate**（＝描線の微小な隙間を封じる）。
(d) `flow` = `passable` ∧ ¬`barrier` とし、画像四辺の `flow` 画素を種として **4近傍 flood-fill** で `reachable` を求める。
(e) `reachable` を `sealPasses` 回 8近傍 dilate し `passable` で AND を取る（封じで過剰に狭めた分を戻す）。
(f) **`closedWhite` = `passable` ∧ ¬`reachable`** を 8近傍で連結成分化する。
(g) **吊り穴の選択（§0-D-3 の osanpo と同一規則へ統一 — `hangingHoleSeed` / `hangingHoleRoi` を実際に用いる）**:

```
ROI の画素化（作業座標 width × height に対して。半開区間 [x0, x1) × [y0, y1)）:
  x0 = floor(roi[0] × width)   y0 = floor(roi[1] × height)
  x1 = ceil (roi[2] × width)   y1 = ceil (roi[3] × height)
seed の画素化: seedX = floor(hangingHoleSeed[0] × width)・seedY = floor(hangingHoleSeed[1] × height)
  （floor は負でない実数に対する切り捨て。0.5 の特別扱いはしない。
    seedX / seedY が [0, width) / [0, height) の外なら生成エラー）

候補 = closedWhite 成分のうち bbox が ROI に完全内包されるもの
      （bbox.x ≥ x0 ∧ bbox.y ≥ y0 ∧ bbox.x + bbox.w ≤ x1 ∧ bbox.y + bbox.h ≤ y1。
        bbox.x / bbox.y は成分の最小座標、bbox.w / bbox.h は画素数。左上は包含・右下は排他）
選択 = 候補のうち minDistance = min over 成分内画素 p of ((p.x − seedX)² + (p.y − seedY)²) が最小のもの。
      同値なら firstIndex（成分内の最小 flat index = y × width + x）が小さいもの。
候補が0件 → 生成エラー（fail-fast）。
```

選択された成分は α = 0 とし、feather 帯は **§0-D-3 と同一の補助関数 `componentFeatherBands`**（8近傍で band1 / band2 を作り、band1 は `clamp(rawAutoAlpha, 32, 127)`、band2 は `clamp(rawAutoAlpha, 128, 223)`。innerCount / outerCount がいずれも ≥1 でなければ生成エラー）を用いる。
(h) **革面成分** = 吊り穴以外で面積 ≥ 最大成分面積 × `mainComponentAreaFraction` を満たす全成分（**swan は 3 成分**、他5種は 1 成分）。α = 255。
(i) **微小成分** = 上記以外。面積 ≤ `minorClosedWhiteMaxArea` のみ許容し、超過は生成エラー。α = 255（ロゴ字面内の白等を革として扱う — §0-D-3 と同契約）。
(j) design bbox（革面最大成分 bbox ± `designPaddingPx`）内は `smoothstep(autoAlphaLow, autoAlphaHigh, 背景色距離)` の rawAutoAlpha、外は 0。**暗い描線は背景色距離が大きいため α = 255 となり、輪郭線はシルエットに含まれる**（§0-E-5 の horseshoe 問題は本方式では構造的に発生しない）。
(k) **fail-fast 照合（すべて必須。1件でも外れたら生成エラー）**: `mainWhiteRange` の `areaMin` / `areaMax`（全革面成分の合計面積）・`widthMin` / `widthMax` / `heightMin` / `heightMax`（革面最大成分の bbox）・**`componentMin` / `componentMax`（革面成分数 — 新規）**、`hangingHoleRange` の area / width / height、`minorClosedWhiteMaxArea`。

**チャーム別定数（本改訂の正本）**:

| key | 作業座標 | mainWhiteRange | hangingHoleSeed | hangingHoleRoi | hangingHoleRange | minorClosedWhiteMaxArea |
|---|---|---|---|---|---|---:|
| horse | 1160×1200 | areaMin 180694 / areaMax 229975 / widthMin 863 / widthMax 871 / heightMin 633 / heightMax 641 / **componentMin 1 / componentMax 1** | [.4858, .4720] | [.4681, .4550, .5043, .4900] | areaMin 225 / areaMax 286 / widthMin 15 / widthMax 21 / heightMin 15 / heightMax 21 | 827 |
| frenchie※ | 1011×1200 | areaMin 439820 / areaMax 559771 / widthMin 913 / widthMax 921 / heightMin 1102 / heightMax 1110 / **componentMin 1 / componentMax 1** | [.4439, .1253] | [.4085, .0950, .4807, .1567] | areaMin 1660 / areaMax 2113 / widthMin 46 / widthMax 52 / heightMin 47 / heightMax 53 | 917 |
| dachshund | 1200×1200 | areaMin 233914 / areaMax 297709 / widthMin 959 / widthMax 967 / heightMin 673 / heightMax 681 / **componentMin 1 / componentMax 1** | [.4682, .5144] | [.4450, .4917, .4917, .5383] | areaMin 705 / areaMax 898 / widthMin 29 / widthMax 35 / heightMin 29 / heightMax 35 | 500 |
| toypoodle | 1200×1200 | areaMin 258169 / areaMax 328578 / widthMin 865 / widthMax 873 / heightMin 695 / heightMax 703 / **componentMin 1 / componentMax 1** | [.5131, .4917] | [.4908, .4692, .5358, .5150] | areaMin 652 / areaMax 831 / widthMin 27 / widthMax 33 / heightMin 28 / heightMax 34 | 500 |
| cat | 1200×1200 | areaMin 236195 / areaMax 300612 / widthMin 759 / widthMax 767 / heightMin 712 / heightMax 720 / **componentMin 1 / componentMax 1** | [.4498, .2917] | [.4275, .2700, .4725, .3142] | areaMin 601 / areaMax 766 / widthMin 27 / widthMax 33 / heightMin 26 / heightMax 32 | 611 |
| **swan** | 797×1200 | areaMin 236663 / areaMax 301208 / widthMin 582 / widthMax 590 / heightMin 729 / heightMax 737 / **componentMin 3 / componentMax 3** | [.3710, .3489] | [.3450, .3317, .3990, .3675] | areaMin 244 / areaMax 311 / widthMin 16 / widthMax 22 / heightMin 16 / heightMax 22 | 500 |

※frenchie は `cropSource` {x:320, y:848, w:1105, h:1312}（§0-D-2）適用後の作業座標。`mainWhiteRange` の width/height は**革面最大成分の bbox** に対する範囲、`areaMin`/`areaMax` は**全革面成分の合計**に対する範囲（swan は 3 成分合計）。座標は作業座標の比率、面積・bbox は作業px。

**基準実測（メインセッション独立 Python 実装・PIL bilinear 縮小・2026-07-30）**:

| key | 革面総px（成分別内訳） | 革面成分数 | 革面最大成分 bbox | 吊り穴 | 微小成分 |
|---|---|---:|---|---|---|
| horse | 205,335 | 1 | 867×637 | 256px / 18×18 | 10個（最大 591） |
| frenchie | 499,796 | 1 | 917×1106 | 1,887px / 49×50 | 5個（最大 655） |
| dachshund | 265,812 | 1 | 963×677 | 802px / 32×32 | 6個（最大 217） |
| toypoodle | 293,374 | 1 | 869×699 | 742px / 30×31 | 4個（最大 355） |
| cat | 268,404 | 1 | 763×716 | 684px / 30×29 | 3個（最大 437） |
| **swan** | **268,936（125,591 + 86,597 + 56,748）** | **3** | 586×733 | 278px / 19×19 | 32個（最大 133） |

swan の 3 革面成分は、本体・翼・胴下部が意匠の内側の線で分割されることによる（意匠どおりで異常ではない）。

**range のマージン規則（一意定義 — 実装者裁量を残さない。すべて整数演算で表現し浮動小数の丸め差を排除する）**: 面積（革面・吊り穴とも）は **`areaMin = floor(実測 × 22 / 25)`（＝ ×0.88）・`areaMax = floor(実測 × 28 / 25)`（＝ ×1.12）**。革面 bbox は実測 **±4px**、吊り穴 bbox は実測 **±3px**。`minorClosedWhiteMaxArea` = **`max(500, floor(微小成分の最大面積 × 7 / 5))`**（＝ ×1.4。horse 591→827・frenchie 655→**917**・cat 437→611・他3種は 1.4 倍が 500 未満のため 500）。革面成分数の `componentMin` / `componentMax` は実測成分数そのもの（5種 1/1・swan 3/3 — 意匠で決まる不変量のためマージンを設けない）。**摂動安定性**: `backgroundDistance` 12/14/16 × `sealPasses` 1/2/3 の全9通りで革面総画素の変動は **0.15〜0.40%**、革面成分数は不変であり、**全6種で `mainWhiteRange` が全9通りを包含する**ことを機械検証済み。range はツール（canvas 縮小）と基準実装（PIL）の縮小実装差を吸収する保守的マージンである。

**修正効果（予測レンダリング検証済み）**: R 所有画素が horse 22,276 → 206,192（9.3倍）／cat 16,847 → 269,068（16.0倍）／swan 19,419 → 269,515（13.9倍）／dachshund 29,980 → 266,436（8.9倍）。革面・グレイン・ロゴ・吊り穴・swan のくちばし（保持装飾）がいずれも正常化する。

**prep.json との境界**: レシピはツール定数であり prep.json スキーマの外（§0-D-3 と同扱い）。**レシピ定数は prep.json へ記録しない**。再現性は生成された manual-*.png の SHA-256 経由で検査8 が機械検証する。

### 0-E-2. グレイン素材の境界欠陥の是正（`grainSourceInset`・新設）

**原因**: グレインマップは `map(x,y) = clamp(g0 / blur, 1-grainAmp, 1+grainAmp)`（blur = 半径 `grainRadiusPx` の box 平均・independent mirror padding）である。`質感.jpg`（1011×676）には**下辺と左辺に明るい境界画素**があり、局所ブラー比が上限に飽和する。

| 位置 | g0 平均 | 上限飽和率 |
|---|---:|---:|
| 最終行（v=675） | **0.9048** | **100.00%** |
| その1つ内側（v=674） | 0.5975 | 16.6% |
| 最左列（u=0） | 0.6627 | 54.3% |
| 内部（行/列とも） | ≈0.5023 | 最大 12.96% |

**影響（改訂17 の革面マスクによる実測）**: 飽和行がミラータイルで写り込むチャームに、革面の行平均輝度が周囲より跳ね上がる**明るい横筋**が出る。**horseshoe を除く7種すべてが該当**し、局所偏差は +0.2177〜+0.2279 の狭い帯に固まる（下表）。horseshoe だけは `grainScaleY` が大きく飽和行に届かないため +0.0811 に留まる。目視で最も目立つのは osanpo y=533 で、horse y=655,656／frenchie y=1065-1067／cat y=793 でも同一の横筋が確認できる。最左列由来の縦筋は全8種で革領域に当たらず実害なし（列方向の偏差は inset なしでも +0.0182〜+0.0511）。

**対象の階層定義（「4種」と「7種」の使い分けの正本 — 本改訂の本文に現れる対象数はすべて本表のいずれかの層を指す）**: 還流票・目視・測定の3層は対象が異なる。**契約上の該当判定は「測定上の閾値超過」（inset なし rowDev > 0.15）であり horseshoe を除く7種**、目視確認はその部分集合の4種、還流票の報告は osanpo 1種である。

| 層 | 対象チャーム | 根拠 |
|---|---|---|
| 還流票の報告（①水平傷） | osanpo（**1種**） | changeTicket `923e2094` |
| 目視で横筋の座標を特定 | osanpo y=533・horse y=655,656・frenchie y=1065-1067・cat y=793（**4種**） | 受領後調査（2026-07-30） |
| 測定上の閾値超過（inset なし rowDev > 0.15） | **horseshoe を除く7種**（swan / cat / toypoodle / osanpo / dachshund / frenchie / horse — +0.2177〜+0.2279） | 本節 A/B 表（正本） |
| `grainSourceInset` の適用範囲 | **grain 素材単位（calf = 2・chevre = 2）→ 全8種へ一律適用**（チャーム個別の選別は存在しない） | 本節 material 表・§5-2 |

目視4種に入らない swan / toypoodle / dachshund も、測定上は同一の +0.21 台の狭い帯に入っており（下の A/B 表）**同一原因の同一欠陥**である（目視4種は受領後調査で横筋の y 座標まで特定できたチャームを列挙したものであり、欠陥の有無の区分ではない）。対処は素材読み込み時の切り出しであるため、測定非該当の horseshoe にも一律に適用されるが**無害**（inset 2 で +0.0811 → +0.0820・正常変動内）であることを下表で実測済みである。

**対処**: グレイン素材を読み込んだ直後に**四辺から `grainSourceInset` px を除去**してからマップを構成する。**`grainSourceInset: 2`** を採用値とする（素材ファイルそのものは不変 — sourceFreezeCommit への影響なし）。

**material 別の宣言（改訂17 — chevre も同一規則で実測して確定）**: `grainSourceInset` は **material 別の宣言値**とし、**この表を正本**とする。**calf = 2・chevre = 2**。

| material（grain source） | 画像全体の飽和率 | 四辺の飽和率（上/下/左/右の各最外行・列） | 欠陥辺 | 採用 inset |
|---|---:|---|---|---:|
| calf（`質感.jpg` 1011×676） | 9.60% | 20.97 / **100.00** / 54.29 / 9.91 % | **あり**（最終行 g0 0.9048・飽和 100%） | **2** |
| chevre（`chevre-grain.png` 824×383） | 21.58% | 17.48 / 21.00 / 15.93 / 15.14 % | **なし**（四辺すべて画像全体の飽和率 21.58% 以下） | **2** |

**chevre にも 2 を適用する理由**: chevre に除去すべき欠陥辺は無い。それでも同値を採る理由は、**ROI 構成規則を material 間で一致させ、実装と selftest を一本化するため**である。無害であることは実測で確認済み — inset 0 → 2 で四辺の最大飽和率は **21.00% → 19.39%** と、画像全体の飽和率 21.58% の範囲内で動くのみで、calf のような 100% 飽和帯の除去のような効果も副作用も生じない。**新規 material を追加する際は、同一の測定（四辺の g0 平均と飽和率・inset 0〜3 の四辺最大飽和率）を Phase 4 の検査レポートへ記録し、飽和率 100% の帯があればそれを除去できる最小値以上を、無ければ既定値 2 を宣言する**。**なお `grainSourceInset` は MATERIAL_PROFILES のフィールドではない**（grain 構成は本節を正本として参照する — 数量契約 `materialProfileTransferFields: 6` は改訂17 でも不変・§0-A）。

**`grainSourceInset` の完全定義（実装差が生じないよう一意に固定する）**:

- **実効 ROI**: 原画像 `W₀×H₀` に対し `ROI = [inset, inset, W₀-inset, H₀-inset]`（右下端は排他）。実効寸法 `W₁ = W₀ - 2×inset`・`H₁ = H₀ - 2×inset`。calf（質感.jpg 1011×676）は **1007×672**、chevre（chevre-grain.png 824×383）は **820×379**。
- **座標写像**: 実効画像の画素 `(u₁, v₁)` は原画像の `(u₁ + inset, v₁ + inset)`。
- **適用順序（固定）**: **inset で切り出した実効画像に対して** independent mirror padding（`radius = grainRadiusPx`）を施して box 平均 `blur` を求め、`grainMap = clamp(g0 / blur, 1-grainAmp, 1+grainAmp)` を計算する。**原画像に対して blur を先に計算してから切り出すことを禁止する**（結果が異なる）。
- **mirror / タイル原点**: タイル参照は実効画像の `(0,0)` を原点とし、`i = mirrorIndex(u, W₁)`・`j = mirrorIndex(v, H₁)`（period は `2×W₁` / `2×H₁`）。**原画像の寸法 W₀ / H₀ は参照しない**。`mirrorIndex` の定義・`blur=0` で生成エラー・浮動小数の加算順は §5-2 の既存契約のまま不変。
- **prep.json 記録**: `grain.<materialKey>` へ **`grainSourceInset`・`effectiveWidth`・`effectiveHeight`・`effectiveRoiSha256`**（実効 ROI の RGBA バイト列の SHA-256）を追加し、**検査 F-2 と検査8 で照合**する。**`effectiveRoiSha256` の入力バイト列の一意定義**: grain 入力を §5-6 の MATERIAL_PROFILES が規定する canonical output（calf = `質感.jpg` / chevre = `chevre-grain.png`。いずれも color freeze の `grainSource` に SHA と寸法が凍結されている）としてデコードし、**EXIF orientation を適用せず**（凍結素材は orientation を持たない — freeze の寸法と一致することで機械確認する）、**色空間変換を行わず**（埋め込みプロファイルを無視して格納値をそのまま sRGB として扱う）、**α = 255 を付与した RGBA 8bit** とする。この RGBA を実効 ROI で切り出し、**左上原点・行優先（row-major）・1画素あたり R,G,B,A の4バイト**を連結した `effectiveWidth × effectiveHeight × 4` バイト列の SHA-256 を取る。**デコード結果が freeze の寸法と一致しない場合は生成エラー**。grain 入力ファイル自体の SHA-256 記録・§4-1c マニフェスト照合は従来どおり不変。

**局所偏差の定義（一意 — 実装差が出ないよう完全に固定する）**:

```
評価座標系 = 作業座標（⑦' 直後・⑧' 補正の前）
対象マスク  = 革所有画素 R = (masks.α ≥ 128 ∧ masks.R ≥ 128)
g(x,y)     = ⑦'-1 の出力 gray(x,y) ÷ 255   （＝ 0〜1 に正規化した焼き込み後の輝度。
             gray は §5-2 の正本式 round(clamp(grainBase × shadeEff × grain, 0, 255))）

行方向:  有効行 = { y : |{x : R(x,y)}| ≥ 40 }   （y 昇順に並べる。40 未満の行は評価しない）
        m[k]  = 有効行の k 番目 y における mean{ g(x,y) : R(x,y) }
        med[k] = m の長さ 25 の移動中央値（窓は k を中心とする 25 点。
                 端は最端値で複製する SciPy median_filter の mode="nearest" と同一。
                 偶数個になることはない）
        rowDev = max_k ( m[k] − med[k] )     ※ 符号つきの最大値。絶対値は取らない
列方向:  上記の x と y を入れ替えて同一に計算し colDev を得る
局所偏差 = max(rowDev, colDev)
有効行または有効列が 25 本未満なら生成エラー
```

符号つきの最大値を用いるのは、本欠陥が「周囲より**明るく**跳ね上がる横筋」だからである。

**A/B 対照実験（canonicalSource を固定し grain 境界処理のみを切り替えた因果確定。**上記定義どおり改訂17 の革面マスク R を対象**とし、⑦' の正本式〔`flatWhite` = 1.0・`shadeMin` = 0.78〕で測定・2026-07-30）**:

| key | R 画素数 | inset なし（行） | inset なし（列） | **inset = 2（行）** | **inset = 2（列）** |
|---|---:|---:|---:|---:|---:|
| swan | 290,582 | **+0.2279** | +0.0385 | +0.0355 | +0.0455 |
| cat | 289,987 | +0.2251 | +0.0182 | +0.0411 | +0.0218 |
| toypoodle | 325,492 | +0.2246 | +0.0438 | +0.0448 | +0.0242 |
| osanpo | 136,358 | +0.2200 | +0.0511 | +0.0377 | +0.0384 |
| dachshund | 303,691 | +0.2198 | +0.0290 | +0.0280 | +0.0477 |
| frenchie | 561,220 | +0.2178 | +0.0235 | +0.0657 | +0.0393 |
| horse | 232,514 | +0.2177 | +0.0248 | +0.0539 | +0.0215 |
| **horseshoe**（非該当 — 正常変動の目安） | 324,153 | +0.0811 | +0.0249 | **+0.0820** | +0.0232 |

※ 革面が欠落している6種の R は、現行 masks ではなく**改訂17 の `COLORLESS_CLOSED_WHITE` レシピが生成する革面（前景 − keep）を独立再構成**して用いた（現行 masks には革面がほぼ存在しないため。§0-E-1）。horseshoe / osanpo は現行 masks の R をそのまま用いた。

**結果の解釈**: `grainSourceInset` なしでは、**該当7種すべてが +0.2177〜+0.2279 の狭い帯**に固まる（グレインタイルの同一飽和行が原因であることの直接証拠）。一方 `grainScaleY` が大きく飽和行に届かない horseshoe だけが +0.0811 に留まる。`grainSourceInset = 2` を適用すると**該当7種は +0.0280〜+0.0657 まで下がり、全体最大は非該当チャーム horseshoe の +0.0820 になる** — すなわち**欠陥が完全に解消し、残るのは正常なテクスチャ変動だけ**である。

**残差の合格閾値（新設）**: **局所偏差 ≤ 0.15**。inset = 2 での全体最大 +0.0820（＝非該当チャームの正常変動）と、inset なしでの最小値 +0.2177 の**中間に位置する**ため、正常な変動を誤検出せず当該欠陥を確実に機械検出できる。**Phase 4 で全8種の rowDev・colDev の両方を計測し、prep.json の `inspections17.grainLocalDeviation`（要約値の正本・§5-4）と prep-inspection-1-8.json（詳細証跡）の双方へ記録、閾値超過は生成エラー**。

他案との比較（全体最大 / osanpo y=533）: inset = 3 は +0.1217 / +0.0499、境界修復方式（下2行・左1列を内側値で置換）は +0.1591 / +0.0458。inset = 2 は素材ファイルを改変せず読み込み時の切り出しだけで済むため採用する。レンダリング検証で osanpo の横筋が視覚的に消失することを確認済み（y=533 の輝度 239.7 → 195.4・周辺 190.7）。

**残存する既知事象（今回は許容）**: グレイン素材はチャームの革面より小さくミラータイルされるため、**折り返し軸の周辺で模様が左右対称になる継ぎ目**が生じる（osanpo では y≈530）。強度は **+0.03**（本改訂で除去した飽和行の約 1/6）で上記の合格閾値 0.15 を大きく下回る。解消には `grainScaleMm` の大幅変更（全チャームの質感の粒の大きさが変わる）またはタイリング方式の再設計が必要であり、**改訂17では対処せず許容する**（ユーザー了承済み 2026-07-30）。Phase 5 形状 QA・Phase 7 色レビューで再評価する。

### 0-E-3. 革面の暗部下限 `shadeMin`（新設）

**現象**: ⑦' グレイン焼き込みは `gray = grainBase × shade × grain`（`shade = clamp(元画像輝度 / flatWhite, 0, 1)`）であるため、意匠の暗い描線は暗いまま焼き込まれ、実行時 `flatLeatherTone` の明度比クランプ下限 0.35 により**革色の非常に暗い版**として描画される。素材の線の濃さがチャームごとに異なるため、輪郭線の見え方が不揃いになる。

**評価指標（一意定義）**: 選択革色をミストブルー `#7aaabf`（輝度 0.6326）とし、**革所有画素（`masks.α ≥ 128 ∧ masks.R ≥ 128`）の描画後輝度を選択革色の輝度で割った比**の分布を用いる。`min` は構成的下限（暗部がどこまで暗くなり得るか）、`p5` は「濃い輪郭線がどの程度濃く見えるか」の代表値であり、**チャーム間の p5 のばらつきが「見え方の不揃い」そのもの**である。

| key | 素材輝度 p5（革所有画素） | min（なし） | **min（0.78）** | p5（なし） | p5（0.67） | **p5（0.78）** | p5（0.88） |
|---|---:|---:|---:|---:|---:|---:|---:|
| horse | 0.3179 | 0.31x | **0.57x** | 0.31x | 0.68x | **0.76x** | 0.80x |
| frenchie | 0.2569 | 0.31x | **0.57x** | 0.31x | 0.68x | **0.75x** | 0.78x |
| dachshund | 0.0793 | 0.31x | **0.57x** | 0.31x | 0.65x | **0.75x** | 0.79x |
| toypoodle | 0.2243 | 0.31x | **0.57x** | 0.31x | 0.69x | **0.76x** | 0.79x |
| cat | 0.7529 | 0.31x | **0.57x** | 0.73x | 0.76x | **0.78x** | 0.80x |
| swan | 0.8913 | 0.31x | **0.57x** | 0.77x | 0.78x | **0.78x** | 0.79x |
| horseshoe | 0.4575 | 0.31x | **0.57x** | 0.42x | 0.73x | **0.75x** | 0.76x |
| osanpo | 0.4756 | 0.31x | **0.57x** | 0.45x | 0.73x | **0.75x** | 0.78x |
| **チャーム間 p5 のレンジ幅**（上表の丸め値から算出） | — | — | — | **0.46**（0.31〜0.77） | 0.13（0.65〜0.78） | **0.03**（0.75〜0.78） | 0.04（0.76〜0.80） |

**対処**: ⑦' の shade に下限を設ける。**`gray = round(clamp(grainBase × max(shade, shadeMin) × grain, 0, 255))`・`shadeMin = 0.78`** を採用値とする（ユーザー承認 2026-07-30）。

**採用根拠**: (1) 構成的下限が全8種一律に 0.31x → **0.57x** へ持ち上がる。内訳: `shadeMin` により ⑦' の最小 gray が `grainBase 200 × 0.78 × (1 − grainAmp) 0.75 = 117` に固定される（`shadeMin` なしでは最小 gray = 0）。**この gray はそのまま描画輝度になるのではなく、実行時に `flatLeatherTone`（Lab 空間・明度比 `gray/255 ÷ baseLum` を [0.35, 1.60] にクランプ）を通る**ため、上表の比（描画後輝度 ÷ 選択革色輝度）は 117/255 = 0.459 ではなく **0.57x** となる。`shadeMin` なしの 0.31x は gray = 0 が明度比クランプ下限 0.35 に張り付いた値であり、**いずれもチャームに依存しない構造的保証**である（gray の下限がチャーム非依存で、`baseLum` は全8種で ⑦' 後のグレー分布から決まるため）。(2) **チャーム間の p5 レンジ幅が 0.46 → 0.03 へ収束し、0.78 が最小**（0.67 では 0.13・0.88 では 0.04）— 「輪郭線の濃さがチャームごとに不揃い」という現象そのものが 0.78 で最小化される。

**適用範囲の限定（`shadeMin` がロゴ・ステッチへ干渉しないことの契約化）**: ⑦' の `shadeMin` 適用対象を **`masks.α ≥ 128 ∧ masks.R = 255 ∧ masks.G = 0 ∧ masks.B = 0`（厳密 one-hot の革所有画素）** に限定する。`shadeMin` を適用しない画素の ⑦' 出力は従来式 `gray = grainBase × shade × grain` のままとする。**実測: 全8種で「`masks.α ≥ 128 ∧ masks.R ≥ 128 ∧ (masks.G > 0 ∨ masks.B > 0)`」の混合所有画素は 0 件**であり one-hot は既に成立しているが、境界画素の混合が将来生じた場合にロゴ（G 所有・`metalTone` 描画）やステッチ（B 所有・`stitchColor` 描画）の色が `shadeMin` によって変わることを構造的に排除するため、契約として明文化する。

**検査の追加**: **Phase 3 selftest と Phase 4 検査へ「混合所有画素数（`masks.α ≥ 128 ∧ masks.R ≥ 128 ∧ (masks.G > 0 ∨ masks.B > 0)`）= 0」を追加**し、実測値を prep.json の `inspections17.mixedOwnershipPxCount`（§5-4）と prep-inspection-1-8.json の双方へ記録する（> 0 なら生成エラー）。独立 F oracle にも混合境界画素の回帰検査を追加する（§5-4）。

**`flatWhite` の破綻も同時に解消される（新規発見）**: `flatWhite` は革コア画素（`base.α = 255 ∧ masks.α ≥ 128 ∧ masks.R = 255`）の輝度の下位中央値であるところ、革面が存在しない6種ではコア画素が暗い描線だけになるため**現行 prep.json の値が破綻**していた（dachshund 0.0655・toypoodle 0.1563・frenchie 0.2078・horse 0.2538・cat 0.3608・swan 0.7230。正しくは horseshoe / osanpo と同じ **1.0000**）。§0-E-1 の革面レシピ適用により全8種で 1.0000 となることを実測確認済み。上表の比はいずれもこの再算出値に基づく。

**既知の相互作用（ユーザー了承済み）**: アイボリー（最も明るい革色 #ece7dd）では輪郭線が革面とほぼ同化して見えにくくなる。中間色（ミストブルー等）では 0.75〜0.78x に揃う。Phase 7 の色レビューで再評価する。

### 0-E-4. osanpo レシピ v2（`osanpo-colorless-20260730-v2`）

§0-D-3 の `OSANPO_COLORLESS_MANUAL`（id `osanpo-colorless-20260728-v1`）を**改訂17で v2 へ失効**させる。**alpha 生成（§0-D-3 の (a)）は完全に不変**とし、owner 生成（(b)）のみを次の2点で改訂する。

**(1) ステッチ除外域の変更（ロゴ周辺ステッチが白糸選択でも黒く残る件の解消）**: v1 は `exclusion` に **logoRoi 矩形全体**を塗ってから dilate していたため、矩形内の破線ステッチが全て除外されていた（実測: logoRoi 内に埋もれたステッチ候補 **1,304px / 5成分**）。除外域を **`dilate(logoMask, logoProtectDilatePasses) ∪ dilate(hangingHoleRoi 矩形, exclusionDilatePasses)`** へ変更する（logoRoi 矩形をステッチ除外へ用いることを廃止。logoRoi は logo 検出 ROI としては不変）。**`logoMask` は (b) の logo 判定（`背景色距離 > featureDistance ∧ boundaryDistance ≥ logoBoundaryMin`）で得た集合であり、ステッチ判定より先に確定させる**（工程順の変更）。

- **`logoProtectDilatePasses: 2`** を採用値とする。
- 実測: rawStitch 8,324px / 25成分 → **9,659px / 28成分**（+1,335px・+3成分）。
- **`B ∩ dilate(logoMask, 2) = 0` が logoProtectDilatePasses = 1, 2, 3, 4, 6 のすべてで成立**する。境界距離帯の構造分離（ステッチ bd∈[15,40] / ロゴ bd ≥ 45）により保護帯の値に依存せず安全であり、出力は k∈[1,6] で完全同一 — **極めて頑健**。
- logo の定義・実測値は v1 のまま不変（**基準実装値 1,254px / 1成分** — 測定系の定義は直下）。v2 の改訂は owner 生成の**除外域**と **`dilatePasses`** のみであり、`logoMask` の定義（`logoRoi` ∩ `背景色距離 > featureDistance` ∩ `boundaryDistance ≥ logoBoundaryMin`）はそのいずれにも依存しないため、**logo 画素は定義・集合とも v1 と同一**である（ハーネス実測で再確認済み）。

**【測定系の明示 — 改訂17 で新設】osanpo の実測値には2系統あり、縮小実装の差により一致しない。**本計画書に現れる osanpo の画素数はいずれか一方であり、**以後は節ごとに測定系を明示する**。

| 測定系 | 縮小実装 | logo | 主閉白 | 吊り穴 | final stitch〔v1 除外域・`dilatePasses` 3〕 |
|---|---|---:|---:|---:|---:|
| **基準実装値** | 独立 Python 実装・PIL bilinear | **1,254 / 1成分** | 132,865 | 993 | 16,758 / 1成分 |
| **ツール実測値** | ブラウザ canvas `drawImage` | **1,221 / 1成分** | 133,890 | 1,014 | 16,136 / 1成分 |

- **§0-D-3 の基準実測10項目・および本節 (1)(2) の掃引表は、すべて「基準実装値」**である（§0-D-3 が測定条件として明記しているとおり）。**§0-E-8 の `logoRange` 表の osanpo 行・horseshoe 行は「ツール実測値（作業座標）」**である。osanpo（補正非適用・作業座標＝アセット座標）は実アセット `osanpo.prep.json` の `derived.logoPxCount` = 1,221 と同値。horseshoe（W軸補正対象）は作業座標の `horseshoe.manual-owner.png` G 実測 4,222 が照合基準であり、アセット座標の `derived.logoPxCount` = 3,957 は補正後の記録値（照合対象外・§0-E-8 の照合座標系規則）。
- **契約上の range 照合対象は「ツール実測値」**である。`logoRange` / `stitchRange` / `mainWhiteRange` / `hangingHoleRange` はいずれも**ツールが生成した画素**を検査するため、Phase 4 で範囲照合されるのはツール実測値の側であり、**基準実装値はレシピの再現性を検証するための参照値**（ハーネス照合用）である。**したがって両値の併記は矛盾ではなく、測定系が異なる2つの実測である。**
- **この差は §0-D-3 が既にマージン設計へ織り込んでいる**（「range はツール〔canvas 縮小〕と基準実装〔PIL〕の縮小実装差を吸収する保守的マージンとして設定」）。実際 osanpo の logo は両系統の値 1,221 と 1,254 がいずれも `logoRange` の `[1000, 1500]` に収まる。ステッチ側も同様で、本節 (2) の `finalCountMin: 8200` / `finalCountMax: 11500` は「縮小実装差で破線が1〜数本分断・融合しても通過する」マージンとして設定してある。
- **この差は作業座標／アセット座標の違いではない**。osanpo は `workSize` = `assetSize` = 1200×1200 かつ `aspectCorrection.applied = false` で座標変換が発生しないため（実アセット `osanpo.prep.json` で確認済み）、差の由来は**縮小実装のみ**である。

**(2) `stitch.dilatePasses` を 3 → 0 へ（ステッチを連続線ではなく実際の点線として、かつ細く表示する）**: v1 は破線（25成分）を dilate×3 で膨張させ**1本の帯に融合**させており、膨張で取り込んだ白い革画素も B 所有となって実行時に糸色で塗られるため、**太く・連続に見える**原因になっていた（`stitchColor` は B 所有画素をすべて糸色で塗る）。

**下表はすべて v2 の除外域（本節 (1)）における `dilatePasses` の掃引である**（v1 の除外域での実測は §0-D-3 の final 16,758px / 1成分〔dilatePasses = 3〕であり、除外域が異なるため下表の 19,389px とは別値。参考: v1 の除外域で dilatePasses = 0 とすると 8,324px / 25成分）。

| dilatePasses | raw（filter 前） | filterComponents(min 2) 後 | final stitch px | 成分数 | 線幅（2×平均距離変換） | 見え方 |
|---:|---|---|---:|---:|---:|---|
| 3 | 9,659px / 28成分 | 9,659px / 28成分 | 19,389 | **1** | 6.26px | 太い連続線 |
| 2 | 9,659px / 28成分 | 9,659px / 28成分 | 16,200 | **1** | 5.42px | やや細い連続線 |
| 1 | 9,659px / 28成分 | 9,659px / 28成分 | 12,954 | 9 | 4.59px | 一部が分離 |
| **0（採用）** | **9,659px / 28成分** | **9,659px / 28成分** | **9,659** | **28** | **3.79px** | **明確な破線** |

- canonicalSource の意匠は**明確な破線**であり、dilatePasses = 0 は素材の破線幾何をそのまま再現する。
- **元糸の残像（ゴースト）は発生しない**: B を 1〜3px 収縮したときに外れる画素の輝度は **0.993（ほぼ白）** で暗画素は 14〜35個のみ。⑦' グレイン焼き込みにより革所有領域は平坦化されるため（B 外周4px の R 画素平均 0.7740 対 遠方 0.7369 ＝ 暗い跡なし）、細身化しても元糸の筋は露出しない。
- **`stitchRange` を実測値に基づき更新する**: `rawComponentMin: 22` / `rawComponentMax: 34` / `finalCountMin: 8200` / `finalCountMax: 11500` / `finalComponentMin: 20` / `finalComponentMax: 34`。実測は raw 9,659px / 28成分・final 9,659px / 28成分。**`dilatePasses = 0` では final = rawStitch が成立する**。(b) の工程は `rawStitch` → `filterComponents(min 2)` → `dilate(0 回)` → `¬除外域 ∧ boundaryDistance ∈ [12, 43]` の再適用であり、3つの段がいずれも恒等になることを**個別に確認済み**である: ①`filterComponents(min 2)` は **v2 の rawStitch に 1px 成分が存在しないため恒等**（実測: 9,659px / 28成分 → 9,659px / 28成分・除去 0px・成分数不変）②`dilate(0 回)` は定義により恒等 ③`rawStitch` は構成上すでに `boundaryDistance ∈ [15, 40] ⊂ [12, 43]` かつ `¬除外域` を満たすため再適用も恒等。したがって **final = rawStitch = 9,659px / 28成分**（実測一致。**基準実装値** — **ツール selftest の合格条件は (a) final ＝ rawStitch の集合恒等性〔画素単位一致〕 (b) raw / final の実測値が更新後 `stitchRange` 内、の2点であり、この画素数・成分数の厳密一致をツールへ要求しない**〔ツール実測値は縮小実装差により別値となる — 上の測定系表・v2 selftest ⑦〕）。**なお `rawComponentMin` / `rawComponentMax` の照合対象は `filterComponents` 適用前の rawStitch の成分数**とする（§0-D-3 (b) の記述順に従う。v2 では両者が一致するが、契約としては適用前を正とする）。**マージンの内訳（検算可能な形で明示する）**: `rawComponentMin: 22` = 実測 28 の −21.4%（摂動 `featureDistance` 3〜6 での成分数変動 25〜31 を包含）／`rawComponentMax: 34` は v1 から据え置き（+21.4%）／`finalCountMin: 8200` = 実測 9,659px の −15.1%・`finalCountMax: 11500` = 同 +19.1%（いずれも 100 単位へ丸めた保守値）／`finalComponentMin: 20` = 実測 28 の −28.6%・`finalComponentMax: 34` = 同 +21.4%。final は raw と同一集合になるため raw 側より広いマージンを与え、縮小実装差で破線が1〜数本分断・融合しても通過するようにしている。
- `stitch` のその他の定数（`seedBoundaryMin: 15` / `seedBoundaryMax: 40` / `finalBoundaryMin: 12` / `finalBoundaryMax: 43` / `exclusionDilatePasses: 2`）および §0-D-3 のその他全定数（`backgroundDistance` 14・`autoAlphaLow` 12・`autoAlphaHigh` 32・`featureDistance` 4・`designPaddingPx` 12・`mainWhiteRange`・`hangingHoleSeed`・`hangingHoleRoi`・`hangingHoleRange`・`minorClosedWhiteMaxArea` 500・`logoRoi`・`logoBoundaryMin` 45・`logoRange`）は**不変**。

**還流票の①水平傷については、本改訂では manual-inpaint を用いない**。§0-E-2 のとおり真因はグレイン素材の境界欠陥であり（§0-E-12 の実測で canonicalSource に傷が無いことを確定）、`grainSourceInset = 2` は grain 素材単位の是正であり（§0-E-2 の階層定義表が正本）、**目視で横筋の座標を特定済みの4種（osanpo・horse・frenchie・cat）にとどまらず、測定上閾値超過に該当する horseshoe を除く7種すべての同一欠陥が解消される**（inset 2 で全体最大 +0.0820 ＝ 測定非該当 horseshoe の正常変動と同水準・§0-E-2 の A/B 表）。**還流票が提案した manual-inpaint は osanpo 単独の対症療法であるため採用しない**（`assets/charms/osanpo.manual-inpaint.png` は新設しない）。

### 0-E-5. horseshoe の外周輪郭の取り込み（`outlineCaptureRadius`・新設）

**原因**: horseshoe の外周輪郭は**素材に存在するが最大8px ぶんシルエット（α）の外側に出ている**。closedWhite（囲まれた革面）の外側にある描線が α に含まれないため、α で切り抜いた時点で輪郭線が部分的に落ちる。線が太い区間は一部が α に入るので暗い縁が残り、細い区間は完全に落ちる — これが「欠けがまだらに見える」理由である。

**指標の定義（一意 — 決定的。法線・補間・浮動小数の探索を用いない）**:

```
評価対象 RGB = 作業座標の canonicalSource（① 回転・クロップ・縮小の直後。
               ⑦' グレイン焼き込みの前・⑧' 補正の前）。
               ※ base.png では α = 0 の画素の RGB が規格上未定義のため評価に用いてはならない
alphaBeforeCapture = outlineCapture を適用する直前の α
               （COLORLESS_CLOSED_WHITE では (j) 直後、HORSESHOE_COLORLESS_MANUAL では
                 取り込み手順 ① の入力となる α）
alphaAfterCapture  = alphaBeforeCapture へ取り込み手順 ①〜⑤（下記「手順の一意定義」—
                 radius 膨張・候補判定・最大成分連結・fill_holes・吊り穴復元）を適用した
                 直後の α。**owner 生成（手順 ⑥）より前**
alphaProbe   = 評価対象 α ＝ **alphaAfterCapture（穴復元後・owner 生成前）**。
               **outlineCapture を持たない7種（horseshoe 以外）では取り込み工程が存在せず**
               **alphaAfterCapture ＝ alphaBeforeCapture ＝ manual-alpha 完了後の α**（swan は
               BEAK_OUTLINE_REMOVAL 適用後・§0-E-6）。canonicalSource 自体は α を持たない
               ため、評価 α は必ずこの生成途中の値を用いる。**外周取り残し率が radius に**
               **依存するのは評価 α が取り込み適用後だからであり**（radius = outlineCaptureRadius）、
               radius 掃引表の「なし（現行）」行のみ取り込み工程なし
               （＝ alphaBeforeCapture）で測定した値、radius 別の各行と対照 selftest
               （radius 8）は当該 radius を手順 ①〜⑤ に適用した alphaAfterCapture で
               測定した値である（本節の実測値はすべてこの定義による基準実装値）
lum(p)       = (0.2126 R + 0.7152 G + 0.0722 B) / 255   （IEEE754 倍精度。§5-2 と同一係数）
outerBg      = ¬(alphaProbe > 0) を 4近傍で連結成分化し、画像の四辺に接する成分の和
               （＝内部の孔〔吊り穴等〕を含まない、外側の背景だけ）
outside      = dilate8(alphaProbe > 0, iterations = outlineProbeRadius) ∧ outerBg
residual     = outside ∧ (lum < outlineLumMax) ∧ ¬BEAK_EXPECTED_DIFF
               （BEAK_EXPECTED_DIFF は意図的に α = 0 にした画素であり取り残しではない。
                 swan 以外では空集合）
boundary     = (alphaProbe > 0) ∧ dilate4(outerBg, iterations = 1)
               （＝外周に接する前景画素。内部孔に接する画素は含まない。
                 dilate4 は 4近傍〔十字〕構造要素・**反復 1 回**・膨張結果が元集合を含む
                 標準の binary dilation。outside の dilate8〔iterations = outlineProbeRadius〕と
                 異なり反復は 1 回に固定する。本節の実測値〔horseshoe boundary 3,611px ほか
                 全8種の外周境界 px〕はこの定義による基準実装値である）
外周取り残し率 = |residual| ÷ |boundary| × 100
```

**本節の定数（改訂17 の正本 — ツールへ転記し Phase 3 selftest で完全一致を機械照合する）**:

| 定数 | 値 | 役割 |
|---|---:|---|
| **`outlineCaptureRadius`** | **12** | α 生成の最終段で描線を取り込む半径（8近傍のチェビシェフ距離。§0-E-5 の対処） |
| **`outlineLumMax`** | **0.75** | 「描線（暗い画素）」と判定する輝度の上限。**取り込み（候補判定）と評価（residual 判定）の双方で同一の値を用いる**。判定は厳密不等号 `lum < outlineLumMax`（`lum` は上記の倍精度の値。`lum = outlineLumMax` は描線に含めない） |
| **`outlineProbeRadius`** | **8** | 評価専用。取り込み半径 `outlineCaptureRadius` とは独立に固定する（両者を混同してはならない） |

`|boundary| = 0` は生成エラー。

**意味**: 「シルエットの外側 8px 以内に、素材の描線（暗い画素）がどれだけ取り残されているか」。**取り込みが完全なら 0 になる**。

**〔測定の訂正 — 本節の数値は改訂17 の作業途中で二度差し替えられている〕**: 当初の測定（欠け率 28.71% → radius 8 で 0.41% 等）は、**輝度判定を `base.png`（アセット）に対して行っていた点で誤り**だった。straight RGBA PNG では `α = 0` の画素の RGB は規格上未定義であり（④ defringe も `α < 255` の画素へ近傍実色を書くだけで `α = 0` の値を保証しない）、その領域の色で判定することはできない。次に作業座標の canonicalSource で測り直したが、そこでは outside / boundary が**内部の孔も含んでいた**ため外周の指標になっていなかった。上記のとおり**外側背景に限定**して測り直したのが下表であり、**採用値は 8 → 10 → 12 と改められている**。旧数値は本改訂では一切採用しない。

**実測（horseshoe・作業座標 1016×1200・メインセッションが `COLORLESS_CLOSED_WHITE` と同一機構で独立再構成した alphaProbe に対して測定・2026-07-30）**:

| outlineCaptureRadius | 取り残し px | 外周境界 px | **外周取り残し率** | 前景 px | 前景増 |
|---:|---:|---:|---:|---:|---:|
| なし（現行） | 8,284 | 3,605 | **229.79%** | 435,071 | — |
| 2 | 6,763 | 3,628 | 186.41% | 437,614 | +0.58% |
| 4 | 4,277 | 3,608 | 118.54% | 440,122 | +1.16% |
| 6 | 1,871 | 3,609 | 51.84% | 442,528 | +1.71% |
| 8 | 161 | 3,611 | 4.46% | 444,238 | +2.11% |
| 10 | 4 | 3,611 | 0.11% | 444,395 | +2.14% |
| **12（採用）** | **0** | **3,611** | **0.00%** | **444,399** | **+2.14%** |
| 14 | 0 | 3,611 | 0.00% | 444,399 | +2.14% |

**radius 12 で取り残しが完全に 0 になる**（14 でも 0・前景も 1px も増えない）。radius 10 では 4px 残るため、**ユーザー決定「シルエットを描線の外縁まで広げる」を文字どおり満たす 12 を採用**する。前景増は 10 と 12 で同一（+2.14%・差 4px）であり、拡張しすぎによる副作用はない。

**全8種の実測（同一指標・採用構成）**:

| key | 取り残し px | 外周境界 px | 外周取り残し率 | 備考 |
|---|---:|---:|---:|---|
| **horseshoe**（取り込み適用） | **0** | 3,611 | **0.00%** | radius 12 適用後 |
| horse | 0 | 3,520 | 0.00% | 取り込み不要 |
| frenchie | 0 | 4,916 | 0.00% | 取り込み不要 |
| dachshund | 0 | 3,469 | 0.00% | 取り込み不要 |
| toypoodle | 0 | 2,990 | 0.00% | 取り込み不要 |
| cat | 0 | 3,034 | 0.00% | 取り込み不要 |
| osanpo | 0 | 1,696 | 0.00% | 取り込み不要 |
| **swan** | **243** | 3,071 | **7.91%** | 下記のとおり許容 |

**`COLORLESS_CLOSED_WHITE` 方式では §0-E-1 (j) により描線が design bbox 内の rawAutoAlpha で α に入るため、構造的に取り残しが生じない**ことが、**同方式5種（horse / frenchie / dachshund / toypoodle / cat）＋同じ閉領域方式の osanpo（`OSANPO_COLORLESS_MANUAL`）の計6種の 0.00%** で裏づけられている（同方式の残る1種 swan のみ 7.91% だが、下記のとおりアンチエイリアス縁であり取り込み不足ではない）。

**swan の 7.91% は取り込み不足ではなくアンチエイリアスの縁である**（実測: 243px が **34 個の微小断片**に分かれ、最大でも 85px。位置は羽・尾の外縁〔x 659–741 / y 523–726〕で、くちばし〔x 92–163〕とは無関係。平均輝度は 0.706〜0.738 で判定閾値 0.75 のすぐ下に集中する）。連続した輪郭線が失われているのではなく、細線の端が薄れて閾値をわずかに下回っているだけであり、**シルエット拡張は行わない**（swan は Phase 5 形状 QA で目視確認する）。

**作業解像度は原因ではない**: WORK_MAX を 1200 → 1600 → 2400 と上げても外周に取り残される描線の量はほぼ変化しない（画素数は 4.0 倍）。**WORK_MAX は不変とする**。

**合格閾値（新設）**: **外周取り残し率 ≤ 10.0%**。取り込みを適用する horseshoe は 0.00%、**取り込み不要の6種（horse / frenchie / dachshund / toypoodle / cat / osanpo）も 0.00%**、唯一の非ゼロである swan の 7.91%（アンチエイリアス縁）を包含し、かつ取り込み前の horseshoe（229.79%）や radius 8 止まり（4.46% → 閾値内だが 0 ではない）との差を保つ値として設定する。Phase 4 で**全8種**を計測し、prep.json の `inspections17.outerLeftover`（residualPx・boundaryPx・ratePercent・thresholdPercent — §5-4）と prep-inspection-1-8.json の双方へ記録、**超過は生成エラー**。**horseshoe のみ固有の強化閾値 ≤ 1.0% を併用する**（直下の実効性検証）。

**horseshoe 固有の実効性検証（改訂17 第12ラウンドレビューで追加）**: 共通閾値 ≤ 10.0% だけでは radius 8 の生成（基準実装値 4.46%）でも通過でき、採用値 12 が実際に α 生成へ適用されたことを機械保証できない。そこで **(a) Phase 3 selftest ⑧ と Phase 4 検査の双方で、horseshoe の外周取り残し率は固有閾値 ≤ 1.0% で判定する**（基準実装値 0.00%・縮小実装差のマージンとして 1.0%） **(b) Phase 3 selftest ⑧ は `outlineCaptureRadius` を 8 に下げた対照生成で取り残し率 > 1.0% となることを回帰検査する**（基準実装値 4.46% — §0-E-2 の grainSourceInset = 0 回帰検査と同型で、取り込み機構の実装漏れ・弱化を機械検出する）。**役割分担**: (a)(b) は取り込み機構が実効であることを、selftest ⑧ 前半の定数転記照合（完全一致）は採用値 12 そのものの転記を保証し、両者の組で「12 が転記され、かつ機構が実効である」ことを機械化する。boundary 3,611px・前景 444,399px（+2.14%）は基準実装値の参照値であり、ツールへの厳密一致要求ではない。

**手順の一意定義（工程順を含む）**:

```
① neighborhood = binary_dilation(α > 0, 8近傍, iterations = outlineCaptureRadius)  … 採用値 12
   （距離計量 = 8近傍のチェビシェフ距離。座標系は作業座標）
② candidate = neighborhood ∧ ¬(α > 0) ∧ 輝度 < outlineLumMax
③ union = (α > 0) ∨ candidate を 8近傍で連結成分化し、最大成分のみを残す
   （同面積なら最小 flat index の成分）
④ binary_fill_holes（4近傍）で内部の孔を埋める
⑤ 吊り穴を復元: 拡張前の α から求めた「fill_holes(α > 0) ∧ ¬(α > 0)」の集合を α = 0 に戻す
⑥ owner（R / G / B）と keep を、拡張後の α に対して再生成する
   （拡張で新たに前景となった画素は革所有 R とし、ステッチ B・ロゴ G・保持 keep の判定は
     拡張前と同一の述語で再評価する）
⑦ 拡張後の α に対して検査4（defringe）・検査1（見切れなし）・§9-2（実寸比）・§5-5（81配置）を再判定する
```

**波及と再検証（必須）**: シルエットが外周方向に最大 12px 拡張されるため、**Phase 5 形状 QA の再承認**（rev16 の shape-approval は失効）、**§7-1（見切れなし）・§9-2（実寸比）・§5-5（81配置3条件）の再検証**が必要である。bboxPx・fgBboxPx・pxPerMm・baseLum・keepPxCount は拡張後の最終画像から再算出する（§5-2 の既存規則のまま）。

### 0-E-6. swan のくちばしガイドラインの除去（`BEAK_OUTLINE_REMOVAL`・新設）

**現象**: swan の茶色い革のくちばし（保持装飾）の外周に、意匠のガイドラインがはみ出して見える。ユーザー説明「ガイドラインの上からくちばし部分のレザーをくり抜いて乗せた」＝ガイドラインは作図上の下書きであり、製品のくちばしは茶革ピースそのものである。

**構造の実測**（くちばし本体 3,629px の外側 `0 < distance(beak) ≤ 6` の**リング 1,765px** を、上から順に判定して**相互排他・網羅的**に分類したもの。合計はリング総数と一致する）:

| 区分（判定順） | 画素数 |
|---|---:|
| 革面（革面成分 closedWhite） | 247 |
| 微小 closedWhite | 7 |
| **前景の描線・暗い**（α > 0 ∧ 非 closedWhite ∧ 輝度 < 0.75） | **683** |
| **前景の描線・明るいアンチエイリアス**（α > 0 ∧ 非 closedWhite ∧ 輝度 ≥ 0.75） | **571** |
| 背景（α = 0） | 257 |
| **合計** | **1,765** |

**くちばしから革面までの最短距離は 2.0px** であり、**くちばしは革面と直接 4連結しておらずガイドライン経由でのみ本体に接続**している。したがってガイドラインは**この領域のシルエット境界そのもの**であり、(a) 線を消して革色で埋めると革がくちばしの外へ回り込み、(b) 近傍の前景をすべて消すとくちばしが前景から孤立して `keepLargestForeground` に落とされる（いずれも実装・レンダリングで確認済み）。

**採用する規則と工程順（`COLORLESS_CLOSED_WHITE` の (j) の直後・owner 生成の前に実行する）**:

```
① beak    = 彩度 > beakSatMin ∧ max(RGB) > beakValueMin の最大8近傍連結成分（茶革の保持装飾）
② beakRoi = beak の bbox ± beakRoiPaddingPx
   target  = beakRoi 内 ∧ distance(beak) ≤ beakOutlineRadius ∧ α > 0
             ∧ ¬beak ∧ ¬革面 ∧ ¬微小closedWhite ∧ distance(革面) > beakBridgeRadius
   → target の α を 0 にする（＝ BEAK_EXPECTED_DIFF。下記）
③ beak 画素を manual-keep レイヤへ書き込む（masks の α を < 128 とし未所有＝保持装飾にする）
④ owner 生成（R / G / B）は keep 画素を対象外にする
```

**定数**: `beakSatMin: 40` / `beakValueMin: 60` / `beakRoiPaddingPx: 12` / `beakOutlineRadius: 12` / `beakBridgeRadius: 3`

**用語の完全定義（実装差を残さないため一意に固定する）**:

```
彩度 sat(p)      = max(R, G, B) − min(R, G, B)   （0〜255 の整数。HSV/HSL 等の別尺度は用いない）
明度 value(p)    = max(R, G, B)                   （0〜255 の整数）
評価対象 RGB     = 作業座標の canonicalSource（§0-E-5 の評価対象と同一）
① beak 候補     = sat(p) > beakSatMin ∧ value(p) > beakValueMin   （厳密不等号）
   beak         = ① の 8近傍連結成分のうち面積最大のもの。
                  同面積なら最小 flat index（= y × width + x）を持つ成分。候補0件は生成エラー
② beakRoi       = beak の bbox を四辺へ beakRoiPaddingPx 拡張した半開区間
                  [x0, x1) × [y0, y1)。x0 = max(0, bbox.x − pad)、y0 = max(0, bbox.y − pad)、
                  x1 = min(width,  bbox.x + bbox.w + pad)、y1 = min(height, bbox.y + bbox.h + pad)
                  （画像端では clamp する）
distance(S)(p)  = 集合 S に属する画素までの**ユークリッド距離変換**（scipy.ndimage の
                  distance_transform_edt と同一。S 自身は 0）。チェビシェフ距離ではない
革面            = §0-E-1 (h) の革面成分（closedWhite の main 群）
微小closedWhite = §0-E-1 (i) の微小成分
```

**採用 `beakOutlineRadius = 12` における target の実測内訳**: target は **1,188px**（BEAK_EXPECTED_DIFF そのもの）で、**輝度 < 0.75 が 668px・輝度 ≥ 0.75 が 520px**（輝度レンジ 0.0467〜0.9728）。bbox は **x 92–163 / y 426–530**。`beakOutlineRadius` を 4〜12 の範囲で変えてもくちばし完全保持・革面不変・前景1成分は成立する（下記の摂動）。**採用値 12 の根拠**: target の画素数は radius とともに緩やかに増え（8 → 1,159px・10 → 1,181px・**12 → 1,188px**・14 → 1,197px・16 → 1,203px）、明確な頭打ちは存在しない。したがって「頭打ち」ではなく**摂動検証済み範囲（4〜12）の上端**として 12 を採用し、期待差分マスクを **1,188px** に固定する（radius 12 における `distance(beak)` の範囲は 1.00〜11.66）。**radius を契約値から変えれば `BEAK_EXPECTED_DIFF` の画素数と SHA-256 が変わり、Phase 4 の合格条件③で機械検出される**。

`distance(革面) > beakBridgeRadius` の条件が**くちばしと本体をつなぐ橋渡し画素および首の輪郭線を保護**する。

**工程 ③ が必須である理由**: α を 0 にするだけでは、くちばし本体は前景のまま残り R 所有（革連動）となって革色でリカラーされ茶色が失われる（現行 swan は `keepPxCount = 0`）。**くちばしを保持装飾（keep）として masks 未所有にする**ことで、horseshoe のクリスタルと同じ扱いになり素材の茶色がそのまま表示される。

**実測**: 除去 1,188px・**くちばし 3,629/3,629px 完全保持**・**革面 268,936px 完全不変**・**前景の連結成分 1（分断なし）**・くちばし外周6px に残る前景 853 → **298px**。安全性検証: くちばし外周8px より外側の線構造は **182成分 → 182成分（首の輪郭線は無傷）**。摂動: `beakBridgeRadius` 3〜5 × `beakOutlineRadius` 4〜12 の全組合せでくちばし完全保持・革面不変・分断なし。

初版で `輝度 < 0.75` の暗い画素のみを対象とした規則では、除去できたのは採用規則の target 1,188px のうち **668px だけ**で、**残る 520px（輝度 0.75〜0.97・うち 237px は 0.78〜0.91）が α = 255 の不透明前景として残存**し革色の細線として見えていた（target 内の輝度レンジは 0.0467〜0.9728 で、閾値 0.75 では上側が切り落とせない）。そのため上記のとおり**輝度条件を用いず「前景かつ革面から `beakBridgeRadius` 超」で判定する**。採用規則の target = 668 + 520 = **1,188px** であり、これが `BEAK_EXPECTED_DIFF` そのものである。

**`BEAK_EXPECTED_DIFF`（期待差分マスク・新設）**: 上記工程 ② で α = 0 にした画素集合そのものを期待差分マスクと定義する。**canonicalReference との生画素比較は行わない**（新アセットはグレイン焼き込みとリカラーを経ており、canonicalReference との画素一致はマスクの内外を問わず成立しないため、そのような判定は定義できない）。代わりに、**同一レシピを工程 ② の有無だけ変えて2回実行した α どうしを比較する**ことで機械検証する。

- **座標系**: 作業座標（⑧' アスペクト補正の**前**。②〜④はいずれも作業座標で実行される）。
- **比較対象**: `α > 0` の二値シルエットのみ（RGB・owner は対象外）。
- **合格条件（Phase 4 の生成時に機械判定・不一致は生成エラー）**:
  ```
  α_without = 工程 ② を実行せずに (j) まで到達した α
  α_with    = 工程 ② を実行した α（＝本番）
  ① (α_without > 0) XOR (α_with > 0) が BEAK_EXPECTED_DIFF と画素単位で完全一致すること
  ② α_with > 0 の画素は α_without > 0 の部分集合であること（除去のみ・追加が無いこと）
  ③ |BEAK_EXPECTED_DIFF| が beakExpectedDiffRange 内であること
  ④ ⑧'-1 直後（⑧'-2 の適用前）のアセット座標において、
     (⑧'-1(α_without) > 0) XOR (⑧'-1(α_with) > 0) が BEAK_EXPECTED_DIFF_ASSET と画素単位で完全一致すること
  ⑤ ④ と同じアセット座標で ⑧'-1(α_with) > 0 が ⑧'-1(α_without) > 0 の部分集合であること
  ```
  **④⑤ が厳密な恒等式である根拠**: ⑧'-1 は出力画素ごとに入力画素を1つ引くだけの最近傍写像（`dst(p) = src(f(p))`・§5-2）であるため、**再標本化と XOR は可換**であり、変換方向・境界処理が正しい限り ④ は誤差なく成立する。したがって **④ の不一致は「アセット座標マスクの導出が誤っている」ことの直接の証拠**であり、Phase 5 の目視まで持ち越さずに Phase 4 の生成時に検出できる。**⑧'-2 は α を変更しない**（フェザー帯の RGB のみを書き換える・§5-2）ため、判定点を ⑧'-1 直後に固定しても最終アセットの α と一致する。**補正が発動しない場合**（`aspectCorrection.applied = false`）は ⑧'-1 が恒等写像となり、④⑤ は ①② と同値になって自動的に成立する。
- **`beakExpectedDiffRange`: { countMin: 1069, countMax: 1307 }**（実測 1,188px の ±10%。`floor(1188 × 9 / 10) = 1069`・`ceil(1188 × 11 / 10) = 1307`）。
- **SHA-256 の直列化規則（一意定義）**: 作業座標の左上原点・行優先（row-major）で、マスク内の画素を `0xFF`・マスク外を `0x00` とする **1画素1バイト**の `width × height` バイト列を作り、その SHA-256 を取る。**`width` と `height`（swan は 797 × 1200）も併記する**（寸法を伴わない SHA は比較不能なため）。
- **アセット座標版マスク `BEAK_EXPECTED_DIFF_ASSET`（Phase 5 の重ね合わせ用）**: swan は ⑧' アスペクト補正の適用対象であるため、作業座標のマスクをそのまま最終画像へ重ねることはできない。**⑧'-1 と完全に同一の幾何変換（colorless の nearest 規則・§5-2）を作業座標マスクへ適用**して得た二値マスクを `BEAK_EXPECTED_DIFF_ASSET` と定義し、その**画素数・SHA-256（同一の直列化規則）・width・height（補正後のアセット寸法）**も記録する。**補正が発動しない場合は作業座標版と同一**とし、その旨も記録する（`aspectCorrection.applied` と整合すること）。
- **記録先**: 作業座標版とアセット座標版の両方について、画素数・SHA-256・width・height を **Phase 4 の `prep-inspection-1-8.json`** へ記録する（同期計画 §2-4 で Phase 4 の許可成果物）。**合格条件 ④⑤ の判定結果（真偽・XOR 不一致画素数〔合格時は 0〕・部分集合違反画素数〔合格時は 0〕）も同ファイルへ記録**する。**Phase 5 の `shape-approval.json` には、重ね合わせに用いた `BEAK_EXPECTED_DIFF_ASSET` の SHA-256 と画素数を転記**し、Phase 4 の記録値との一致を機械照合する（shape-approval.json は Phase 5 の shapeApprovalCommit 成果物であり、生成記録の一次保存先ではない）。
- **Phase 5 の人手 QA での用途**: 4面比較の際、この期待差分マスクを重ねて表示し、「くちばし外周の変化は意図した差分である」ことを承認者へ明示する。**マスク外の形状変化が目視で見つかった場合は、上記合格条件 ① が破れているため Phase 4 へ差し戻す**。

これにより「正しい実装が目視で誤検出される」ことも「恣意的に見逃す」ことも構造的に排除する。

**swan の keep（保持装飾）集合の確定（改訂17）**〔**旧表現の失効** — 改訂13〜16 に現れる「金具鋲・ラインストーン等の装飾」「くちばし・金具鋲等」という表記は当時の暫定描写であり、**改訂17 以降は実行契約ではない**。実行契約は本節の `swanKeep` = beak ∪ stone のみで、§4-1c・§5-6・§7 の検査6・§9 のリスク表はすべて本節を参照する。§4-1b の旧参考写真の行など**履歴・監査目的の記述にのみ旧表記が残る**〕: §0-E-8 の実測により swan 底部にはロゴ（銀の線画）とストーン（horseshoe のクリスタルと同種の保持装飾）が隣接して存在する。**swan の keep は次の2集合の和とし、これ以外は含めない**（改訂13〜14 の「金具鋲等」という暫定表現は、実測でこのストーン1個と特定されたため本項で置換する — §4-1c・§5-6 の当該記述もこの定義で読む）:

```
swanKeep = beak（§0-E-6 ① の最大連結成分・実測 3,629px）
         ∪ stone（§0-E-8 の stoneRoi 内・輝度 < stoneLumMax の最大連結成分・実測 1,515px）
両者は交わらない（実測で重複 0px）。
```

**座標系ごとに値を分けて契約する（`keepPxCount` は §5-2 では ⑧' 後のアセット座標で再算出される派生値であり、swan はアスペクト補正の適用対象のため作業座標の値とは一致しない）**:

| 量 | 座標系 | 期待値 | 照合 |
|---|---|---|---|
| `keepPxCountWork`（**改訂17 新設**） | 作業座標（⑧' の前） | **5,144px** | **`keepRange: { countMin: 4630, countMax: 5658 }`（±10%）で機械照合**（範囲外は生成エラー） |
| `keepPxCount`（§5-2 の既存派生値） | アセット座標（⑧' 後の最終画像） | ⑧'-1 の nearest 変換に依存するため事前に確定しない | **範囲照合は行わず prep.json へ実測値を記録**（検査8 の再生成一致が同値性を担保する） |

範囲照合を作業座標側だけで行うのは、`keepPxCount` に範囲を課すと ⑧'-1 の離散丸めの結果に契約が依存してしまうためである。**両者を prep.json へ別フィールドで記録し、Phase 4 は両方を prep-inspection-1-8.json へ記録する**。ストーンの判定規則は §0-E-8 に定義する。

### 0-E-7. horse の線状要素の仕様確定

canonicalSource の実確認により、**horse の意匠は外輪郭線・吊り穴・金の「&」ロゴのみで、破線ステッチは存在しない**ことを確定した。唯一の線は外周輪郭＝革の縁でありステッチではない。したがって **`stitchMode: "none"` の維持が正しい**（実測根拠付きの仕様確定）。`threadColor: null`・`stitchPathsWork: []` も不変。

### 0-E-8. ロゴ・保持装飾の契約確定と検査追加

`detectLogo` は logoRoi 内の α ≥ 200 の画素から基準色（median）を取るため、革面が存在しないと基準が定まらず検出が破綻する。**現行アセットの logoPxCount は6種すべてで異常値**であった（horse 97 / frenchie 961 / dachshund 183 / toypoodle 92 / **cat 0** / **swan 4**。参考: 正常な osanpo 1,221・horseshoe 3,957 — いずれも現行アセットの `derived.logoPxCount`〔アセット座標〕）。§0-E-1 の革面レシピ適用により解消される。

**`detectLogo` のアルゴリズム正本（改訂17 で明示）**: `detectLogo` は改訂14 で取り込んだ Codex 実装（コミット `39fe815` の `tools/charm-prep.html` 内の `detectLogo` 関数）を**ビット単位の正本**とする。基準色の median 算出・サンプリング stride・ROI の画素化・RGB 距離／輝度差／warm コントラストの式と係数・threshold の単位・α 条件・成分フィルタの接続と上下限は、**すべて当該実装の挙動が契約であり、本計画書はそれを再記述しない**（再記述による二重定義の齟齬を避けるため）。改訂17 が加える変更は**次の1点のみ**である。

**変更点（`detectLogo` を用いる6種〔horse / frenchie / dachshund / toypoodle / cat / swan〕共通の新規則）**: **keep（保持装飾）画素を `detectLogo` の ①基準色（median）サンプリングの母集団と ②候補判定の双方から除外する**。keep 画素の確定は §5-4 の **⑤-0** で `detectLogo` より前に行う。保持装飾（horseshoe のクリスタル・swan のストーン）がロゴに近接する場合、矩形 ROI では分離できないためである。swan では ROI 上でロゴとストーンが x 方向 2px・y 方向とも重なるため本規則が必須である（**horseshoe のクリスタルも keep だが、horseshoe は本節の規定どおり `detectLogo` を用いない〔`manualRecipe` が G 所有画素を直接生成する・`logoThreshold: null`〕ため、本規則の適用対象は上記6種に限られる**）。**Phase 3 の selftest は、keep 除外の有無で swan の検出が 1,221px / 6成分 → 898px / 5成分 と変わることを回帰検査する**（除外が実装されていなければこの差が出ない。数値は修正後 `logoRoi`〔画素化 x[369,446)×y[889,970)〕での実装忠実ハーネス実測 2026-08-02 — 除外なしの 1,221px はストーン画素を含む値であり、旧 logoRoi の 1,244px から x=446 列のストーン画素 23px が ROI 外となった分だけ減少、**keep 除外ありの 898px / 5成分は旧 logoRoi と検出集合が画素単位で完全一致**する。したがって `logoRange`〔718〜1,078 / 3〜8成分〕・`keepPxCountWork` 5,144px はいずれも不変）。

**cat / swan の logoRoi を実測で確定する（改訂17で先送りを撤回）**:

- **cat — 金の塗りロゴ「&.」が実在**（拡大目視で確認）。成分: 2,145px @bbox (599,865) 76×75（平均 RGB 199,167,102）＋ 118px @bbox (681,929) 13×12（「.」）。合成 bbox は x 599–694 / y 865–941。**`logoRoi: [0.4875, 0.7092, 0.5900, 0.7958]`**（合成 bbox ±14px・作業座標 1200×1200 の比率）。
- **swan — 銀の線画ロゴ「&」が実在**。当初「swan にロゴは存在しない」と判定したのは誤りであり、**ロゴを「暖色 ∧ 彩度 > 25」の金色として探したため、彩度 4.7 の銀線画が条件に掛からず、隣接するストーン（彩度 15.7）を拾っていた**ことが原因である（ユーザー指摘により訂正 2026-07-30）。**全8種で同一なのはロゴの所有契約（いずれも G 所有として `metalTone` で金／銀に塗る）**であり、swan 専用の別規則は設けない — swan は cat と同一の `detectLogo`（logoRoi ＋ スコア閾値、keep 除外）機構を用いる。**`detectLogo` の適用対象は horse / frenchie / dachshund / toypoodle / cat / swan の6種に限り、horseshoe / osanpo は `manualRecipe` が G 所有画素を直接生成する例外（`logoThreshold: null`・照合規定は本節後段）である**。見え方が cat（金の塗り）と swan（銀の線画）で異なるのは**素材画像側の描き方の差**にすぎず、実行時はいずれも G 所有として `metalTone` で金／銀に塗られる。

**swan 底部の実測（作業座標 797×1200・ROI x370-500 / y875-995 の非白成分〔輝度 < 0.965〕の8近傍連結成分）**:

| 要素 | 面積 | bbox | 平均RGB | 彩度 | 種別 |
|---|---:|---|---|---:|---|
| **「&」ロゴ** | **1,410px** | (x383–431, y903–955) 49×53 | (208,204,204) | **4.7** | **ロゴ（G 所有）** |
| **ストーン** | **1,515px** | (x430–478, y924–966) 49×43 | (173,163,158) | 15.7 | **保持装飾（keep）** |
| （参考）horseshoe のクリスタル1個 | 2,579px | 56×70 | (158,144,116) | 42.3 | 保持装飾（keep） |

- **`logoRoi: [0.4630, 0.7412, 0.5590, 0.8083]`**（ロゴ bbox ±14px ＝ px(x369–445, y889–969)。**比率は §0-E-1 の ROI 画素化規則〔x0 = floor(r0×W)・y0 = floor(r1×H)・x1 = ceil(r2×W)・y1 = ceil(r3×H)・半開区間〕を作業座標 797×1200 へ適用したとき、この宣言境界〔半開 x[369,446)×y[889,970)〕を厳密に再現する値**として選定した。旧値 [0.4630, 0.7408, 0.5596, 0.8083]〔改訂17 第11ラウンドレビューで失効〕は同規則で x[369,447)×y[888,970) となり宣言境界より x 1列・y 1行広く、候補画素集合が一意に再現できなかった。修正後の各積は 369.011 / 889.44 / 445.523 / 969.96 で**整数境界から最小 0.011 離れており浮動小数の丸めに対して安全**。**Phase 3 selftest は ROI 画素化が {x0: 369, y0: 889, x1: 446, y1: 970}〔半開〕を返すことを機械照合する**）
- **`stoneRoi: [0.5245, 0.7600, 0.6155, 0.8158]`**（ストーン bbox ±12px ＝ px(x418–490, y912–978)。**同上の画素化規則で宣言境界〔半開 x[418,491)×y[912,979)〕を厳密に再現する値**。旧値の x1 = 0.6161〔同レビューで失効〕は ceil(0.6161×797) = 492 となり x 1列広かった。修正後の各積は 418.0265 / 912.0 / 490.5535 / 978.96〔y0 = 0.7600 は意図境界 912 ちょうどであり、IEEE754 倍精度の 0.76×1200 = 912.0000000000000107… の floor が 912 で安定することを実測確認済み〕。**Phase 3 selftest は ROI 画素化が {x0: 418, y0: 912, x1: 491, y1: 979}〔半開〕を返すことを機械照合する**。なお比率修正の前後でストーン選択の画素集合は完全同一（実測 1,515px 不変）である）
- **ストーンの判定規則（一意定義）**: `stoneRoi` 内の `輝度 < stoneLumMax` の画素を8近傍で連結成分化し、**最大成分**をストーン（keep）とする（同面積なら最小 flat index）。**`stoneLumMax: 0.965`**。実測 1,515px。**候補が0件なら生成エラー**。工程順は §0-E-6 の ③ と同じ manual-keep 書き込み段で、くちばしと合わせて keep レイヤへ書き込む。

**`logoRange` の確定値（全8種）**: 6種は `detectLogo` の閾値別実測から、次のマージン規則で確定する（整数演算・一意）。**`countMin = round(実測 count × 0.8)`・`countMax = round(実測 count × 1.2)`**（round は四捨五入 = `floor(x + 0.5)`）。**`componentMin = max(2, 実測成分数 − 2)`・`componentMax = 実測成分数 + 2`**（**swan のみ銀の線画で断片化しやすいため `componentMax = 実測成分数 + 3`**）。`componentMin` の下限 2 は、ロゴ「&.」が最低でも「&」と「.」の2成分から成るという意匠上の不変量による。

| key | logoRoi | 採用 threshold | 実測 count / 成分 | countMin | countMax | componentMin | componentMax |
|---|---|---:|---|---:|---:|---:|---:|
| horse | [.54, .49, .68, .63]（不変） | 18 | 2,474 / 4 | 1,979 | 2,969 | 2 | 6 |
| frenchie | [.50, .55, .76, .82]（不変） | 18 | 6,892 / 3 | 5,514 | 8,270 | 2 | 5 |
| dachshund | [.64, .56, .78, .72]（不変） | 18 | 3,044 / 6 | 2,435 | 3,653 | 4 | 8 |
| toypoodle | [.61, .58, .75, .73]（不変） | 18 | 2,378 / 3 | 1,902 | 2,854 | 2 | 5 |
| **cat** | **[.4875, .7092, .5900, .7958]（新規）** | 18 | 2,086 / 2 | 1,669 | 2,503 | 2 | 4 |
| **swan** | **[.4630, .7412, .5590, .8083]（新規）** | **10** | **898 / 5** | **718** | **1,078** | **3** | **8** |
| horseshoe | — | — | **4,222 / 2**〔ツール実測値・作業座標。アセット 3,957 は記録のみ〕 | 3,378 | 5,066 | 2 | 4 |
| osanpo | [.53, .27, .68, .43]（不変） | — | **1,221 / 1**〔ツール実測値・作業座標＝アセット座標（補正なし）〕 | 1,000 | 1,500 | 1 | 2 |

**上表の6種（horse / frenchie / dachshund / toypoodle / cat / swan）の「実測 count / 成分」は `detectLogo` の閾値別実測 — 測定系・座標系のラベルは「基準実装値（作業座標）」**（PIL bilinear 縮小の作業画像上で `detectLogo` のアルゴリズム〔stride 2 median・下側中央値・filterComponents [3, round(w·h·0.012)]・8近傍〕を忠実再現した実測。§0-E-4 の測定系区分では基準実装値の系譜）。**現行アセットの `logoPxCount` は本節冒頭のとおり6種すべてで異常値**（革面が無く基準色が定まらないため）であるから、**6種にはツール実測値との比較対象が現時点では存在せず、Phase 4 の生成時に初めて確定する**（照合は作業座標の `logoPxCountWork` — range のマージン ±20% は縮小実装差〔基準実装 vs ツール〕を吸収する設計であり、照合が作業座標のため ⑧' の座標変換差は照合に入らない）。ツール実測値と併記できるのは、`manualRecipe` で G 所有画素を直接生成する horseshoe / osanpo の2種のみである（いずれも作業座標の manual レイヤ実測）。

**horseshoe / osanpo は `detectLogo` を用いない**。両者は `manualRecipe`（`HORSESHOE_COLORLESS_MANUAL` / `OSANPO_COLORLESS_MANUAL` v2）が G 所有画素を直接生成するため「採用 threshold」は存在せず（表の「—」）、`logoRange` の照合対象は**生成された G 所有画素の数と8近傍連結成分数**となる。**`logoRange` の照合座標系は全8種とも作業座標（`logoPxCountWork` / `logoComponentCountWork`・⑤ の検出／生成直後の G 所有画素）であり、アセット座標の `derived.logoPxCount`（⑧' 適用後）は記録のみで照合対象外とする**（keepPxCountWork と同じ規則・§0-E-6 の前例。アスペクト補正対象チャームでは両座標の画素数が一致しないため、照合座標系を検出・生成が行われる作業座標へ一意化する — 補正非適用チャームでは同値）。**osanpo の `logoRange` は §0-D-3 の既存契約値（countMin 1000 / countMax 1500 / componentMin 1 / componentMax 2）を据え置く**（**ツール実測値〔作業座標〕** 1,221px / 1成分が範囲内であることを確認済み — osanpo は補正非適用のため作業座標＝アセット座標で、`derived.logoPxCount` とも同値。**基準実装値 1,254px / 1成分も同一 range 内**であり、両測定系のいずれでも通過する。測定系の定義は §0-E-4。改訂17では変更しない）。**horseshoe の `logoRange` は改訂17で新設**し、**ツール実測値〔作業座標〕 4,222px / 2成分**〔実アセット `horseshoe.manual-owner.png`（1016×1200・作業座標）の G 所有画素（α ≥ 128 ∧ G ≥ 128）の実測 2026-08-02。アセット座標の `derived.logoPxCount` = 3,957px / 2成分は W軸補正（955/1016）後の記録値であり照合対象外 — 比 3957/4222 = 0.937 は縮小比 0.940 と整合〕に対し6種と同一のマージン規則を適用した（countMin = round(4222 × 0.8) = 3,378・countMax = round(4222 × 1.2) = 5,066・componentMin = max(2, 2 − 2) = 2・componentMax = 2 + 2 = 4。実測の成分内訳は 4,199px と 23px の2成分で、23px は「&.」の「.」に相当する）。**これで「全8種の `logoRange` 照合」が全種で実行可能になる**。

swan が threshold 10 なのは、銀の線画ロゴは閾値を上げると断片化するため（th14 701px/4成分・th18 565px/7成分・th22 443px/11成分）。keep 除外の効果は成分構造で確認済み（除外なし 1,221px/6成分 → 除外あり 898px/5成分〔検出全体 bbox (383, 903) 49×52〕。**除外なしの6成分 ＝ 除外ありの5成分〔画素単位で同一〕＋ ストーン由来成分 323px / 1成分〔bbox (431, 926) 15×39〕であり、898 + 323 = 1,221 が厳密に成立する**。除外なし側の検出全体 bbox はストーン成分を含むため (383, 903) 63×62 となる。数値はいずれも修正後 `logoRoi` での実装忠実ハーネス実測 2026-08-02。なお threshold 掃引〔th14 / th18 / th22〕は keep 除外ありの値であり、logoRoi 修正の前後で完全同一であることを実測確認済み）。**なお上表の「ロゴ成分の bbox」49×53（y 903–955）と `detectLogo` の検出 bbox 49×52 の1px 差は、`detectLogo` の閾値判定で最下行のアンチエイリアス1行が落ちることによる**（`logoRoi` は成分 bbox 49×53 に ±14px を取っており両者を包含する）。

**検査の追加**: **Phase 3 selftest へ「全8種の logoPxCount と成分数が `logoRange` 内であること」「`detectLogo` を用いる6種の採用 threshold が契約値と一致し、horseshoe / osanpo は `logoThreshold` が `null` であること」を追加**し、**Phase 4 は全8種の logoPxCount・成分数・`logoThreshold`（6種は実数値・2種は null）を prep.json の `inspections17.logoInspection`（§5-4）と prep-inspection-1-8.json の双方へ、`keepPxCountWork`・`keepPxCount` を派生定義値（§5-4）として記録**する。範囲外・非該当種への実数値混入はいずれも生成エラー（fail-fast）。

### 0-E-9. toolVersion / schemaVersion

**`toolVersion` 0.7.0 → 0.8.0**（§0-E-1〜§0-E-8 のツール変更に伴う増分・straight marker 連動・§5-4）。

**`schemaVersion` 5 → 6 へ増分する**。§5-4 の schemaVersion は「**スキーマ拡張または生成規則の変更**時に増分する」と定義されており（⑧' 二段階化の改訂12・無着色素材モードの改訂13・colorless / material-aware 化の改訂14に先例がある）、改訂17は次の両方に該当する。

- **生成規則の変更**: `grainSourceInset`（§0-E-2）と `shadeMin`（§0-E-3）は **⑦' の出力画素値そのものを変える**。`COLORLESS_CLOSED_WHITE`（§0-E-1）・`outlineCaptureRadius`（§0-E-5）・`BEAK_OUTLINE_REMOVAL`（§0-E-6）は α・masks・keep を変える。
- **スキーマ拡張**: `grain.<materialKey>` へ `grainSourceInset` / `effectiveWidth` / `effectiveHeight` / `effectiveRoiSha256` / `shadeMin` を追加し、**改訂17 新規検査の per-charm 結果値 `inspections17`（`mixedOwnershipPxCount`・`outerLeftover`・`grainLocalDeviation`・`logoInspection` — フィールド定義は §5-4）を新設**する（§0-E-2・§0-E-10 の3・§5-2・§5-4）。

したがって**改訂17の prep.json は schemaVersion 6** とし、**v5 以前の prep.json・派生アセット・manual レイヤとの流用・混在を禁止する**（§8-1「全8種再生成」の既存契約のとおり）。prep.json・検査実装・独立 F oracle・Phase 3 selftest・Phase 4 検査・revision lock manifest の参照はすべて v6 を正とする。

### 0-E-10. 数量契約

**planRevision の 16 → 17 増分を除き、§0-A の数量はすべて不変**（charms 8・materialProfiles 2・leatherVariants 27・finiteCases 192・layouts 81・charmTransferFields 10・variantTransferFields 8・materialProfileTransferFields 6）。色契約・UI 仕様・freeze JSON（source/color）にも変更はない。

**検査の条件式のうち改訂17で変更・追加されるもの**（上記以外の検査1〜8・M・F の条件式は不変）:
1. **検査F-2**: grain 記録項目へ `grainSourceInset`・`effectiveWidth`・`effectiveHeight`・`effectiveRoiSha256`・`shadeMin` の照合を追加（§0-E-2・§0-E-3）。
2. **⑦' の式**: `gray = round(clamp(grainBase × max(shade, shadeMin) × grain, 0, 255))`（厳密 one-hot 革所有画素のみ。§0-E-3・§5-2）— 検査F-1 の独立 oracle の期待値式も同式へ更新する。
3. **新規検査（Phase 3 selftest ＋ Phase 4）**: 混合所有画素数 = 0（§0-E-3）・外周取り残し率 ≤ 10.0%〔**horseshoe は固有閾値 ≤ 1.0% を併用し、selftest は radius 8 対照生成 > 1.0% の実効性回帰を追加**・§0-E-5〕・grain 比の局所偏差 ≤ 0.15（§0-E-2）・全8種の `logoRange` 照合（§0-E-8）。**結果値の記録先はフィールド単位で一意とする**: prep.json の `inspections17`（per-charm 要約値の正本・§5-4）＋ prep-inspection-1-8.json（詳細証跡）。selftest 専用の対照生成（radius 8・inset 0）の結果は prep-selftest.json のみに記録する。
4. **Phase 4 の生成時検査**: `BEAK_EXPECTED_DIFF` の**5合格条件**（作業座標の XOR 一致・除去のみ・範囲内＋**アセット座標〔⑧'-1 直後〕の XOR 一致・除去のみ**）を追加（§0-E-6）。**Phase 5 の `shape-approval.json` は Phase 4 記録の SHA-256 と画素数を転記して一致照合する**（新規の比較検査ではなく転記照合。**shape-approval.json が Phase 4 記録から転記するのは swan の `BEAK_EXPECTED_DIFF_ASSET`〔SHA-256・画素数〕のみであり、新規4検査の結果値は転記しない — 正本は prep.json の `inspections17` と prep-inspection-1-8.json〔上記3〕**）。
5. **`COLORLESS_CLOSED_WHITE` 新設に伴う6種の生成時 fail-fast 照合（§0-E-1 (k)）**: horse / frenchie / dachshund / toypoodle / cat / swan の各 `mainWhiteRange`（areaMin / areaMax・widthMin / widthMax・heightMin / heightMax・**componentMin / componentMax**）・`hangingHoleRange`（area / width / height）・`minorClosedWhiteMaxArea`。**6種は改訂16以前に `manualRecipe` を持たず対応する照合ゲートが存在しなかったため、いずれも新設**である（チャーム別の値は §0-E-1 の表が正本）。
6. **osanpo `stitchRange` の更新（§0-E-4 (2)）**: 旧値（§0-D-3・レシピ v1）`rawComponentMin: 18` / `rawComponentMax: 34` / `finalCountMin: 14000` / `finalCountMax: 20000` / `finalComponentMin: 1` / `finalComponentMax: 3` → 新値 `rawComponentMin: 22` / `rawComponentMax: 34`（据え置き）/ `finalCountMin: 8200` / `finalCountMax: 11500` / `finalComponentMin: 20` / `finalComponentMax: 34`（`dilatePasses` 3 → 0 の点線化により final の画素数・成分数の分布が変わるため）。**ツール selftest の判定は final ＝ rawStitch の集合恒等性と本 range 照合の2点**であり、基準実装値 9,659px / 28成分 の厳密一致は要求しない（§0-E-4）。
7. **ロゴ・保持装飾の照合ゲートの内訳（§0-E-8・§0-E-6 — 上記3の「全8種の `logoRange` 照合」を構成する新旧の別）**: `detectLogo` を用いる6種の `logoRange` と採用 threshold（horse / frenchie / dachshund / toypoodle / cat = 18・swan = 10）は**新設**・horseshoe の `logoRange` は**新設**・osanpo の `logoRange` は**据え置き**・horseshoe / osanpo の `logoThreshold: null` 照合は**新設**。cat / swan の `logoRoi` は**新設**（swan は ROI 画素化境界の機械照合を含む）。swan の `stoneRoi`・`stoneLumMax` とストーン選択規則（実測 1,515px）・`detectLogo` の keep 除外の回帰検査（swan 1,221px / 6成分 → 898px / 5成分）・`keepPxCountWork` の範囲照合（swan 5,144px・keepRange 4,630〜5,658）は**新設**。

上記 5〜7 は v2 指示書の Phase 3 selftest（④〜⑫）と Phase 4 の停止チェックポイント・成果物要件に個別列挙済みの内容と同一であり、本節はロック時の受入条件見落としを防ぐため**計画書側の変更インベントリを完全化**するものである（数値の正本は §0-E-1 (k)・§0-E-4 (2)・§0-E-8 の各節）。

### 0-E-11. 報告先・ブランチ・再実施範囲

**報告先を `reports/charm-combo/rev17/` へ切替**（同期計画 §12-1 手順1）。Codex 専用 worktree のブランチ名は **`feature/charm-combo-codex-rev17`**（v2 §1）。

rev16 の Phase 3〜5 成果物（toolCommit `8a363f2`・assetCommit `e71a1d80`・shapeApprovalCommit `8444c18`）および Phase 6 実装 WIP（`ac2e9e6`）は**すべて失効**とし、その内容は新 Phase 3 以降で正規に取り込む（supersession.json に記録）。**Phase 4 は全8種を再生成**し、検査1〜7・M・F → 書き出し → 検査8（三者照合）を再実行する。**Phase 5 形状 QA は §0-E-5 のシルエット拡張と §0-E-1 の革面追加により全8種で再承認が必要**。

**Phase 6 の完了条件（改訂17で確定 — §5-6 の `stitchMode` 実態に合わせて条件を分ける）**:

**判定対象の集合は固定値ではなく、Phase 4 で確定した `CHARM_SPECS[key].stitchMode` から動的に決める**（§5-6 は `detect` / `fallback` / `none` の3値を許容し、swan は Phase 4 判定で確定する — rev16 で初期値 null 化・§0-D-4）。

1. **全8種**: 革リカラーが機能すること（選択した革色で base が着色され、24 calf variant の切替で出力差分画素数 > 0）。
2. **全8種**: ロゴ金／銀切替の出力差分画素数 > 0（G 所有・`metalTone`）。
3. **`stitchMode ∈ { "detect", "fallback" }` の全チャーム**: ステッチ白／黒切替の出力差分画素数 > 0（`fallback` は `stitchPaths` 由来の B 所有に対して同一に判定する）。改訂17 時点の確定分は horseshoe / osanpo の2種、**swan は Phase 4 の判定次第**。
4. **`stitchMode = "none"` の全チャーム**: **UI 無効の判定は単独表示（当該チャームのみ表示・もう一方は【なし】）で行い**、ステッチ切替 UI が無効（`disabled`）であること。かつ**仮に切替しても出力差分画素数 = 0**（B 所有 0px のため構造的に成立する。**差分は当該チャーム単位のビットマップで評価する** — 全体画面のピクセル差分ではない）。**ステッチ有チャームとの混在表示では、トグルの有効／無効は §4-2 のグローバル規則（表示中にステッチ有チャームが1つ以上あれば有効）が正であり本条件の対象外**だが、その状態でトグルを切り替えても none チャーム側のビットマップ差分 = 0 であることは同一に要求される（本条件と §4-2 は評価スコープが異なるため矛盾しない — UI 状態は単独表示・差分はチャーム単位で判定する）。改訂17 時点の確定分は horse / frenchie / dachshund / toypoodle / cat の5種、**swan が `none` と確定した場合は6種**。
5. **網羅性**: 上記 3 と 4 の対象チャームの**和が8種ちょうど**であり、`stitchMode` が3値のいずれでもないチャーム（null を含む）が存在しないことを機械確認する（Phase 4 で `stitchMode` が未確定なら Phase 6 を完了しない）。

rev16 の Phase 6 では代表 horseshoe のみの実装で停止していた。

### 0-E-12. §4-1c の背景値記述の訂正（254 → 255）

§4-1c および §7 の素材前提チェックには「白背景 254 単色」「コーナー 100×100 が輝度 254 単色」という記述があるが、**全8種の canonicalSource の実測は 255 単色**であった（sourceFreezeCommit `8b3bdb41bd3205287f2612ca9fb071b4d53e8376` の記録 SHA と一致することを確認したうえでの実測 — 素材そのものは同一で、Phase 1 の記録側の誤記または輝度計算の丸めと推定される）。

| key | 原寸 | 四隅 5×5 平均 | 左上 100×100 min/max | 255未満の割合 |
|---|---|---:|---|---:|
| horse | 2158×2233 | 255.0 | 255 / 255 | 0.00% |
| horseshoe | 2020×2385 | 255.0 | 255 / 255 | 0.00% |
| frenchie | 1808×2664 | 255.0 | 255 / 255 | 0.00% |
| **dachshund** | **2195×2195** | **255.0** | **255 / 255** | **0.00%** |
| **toypoodle** | **2176×2176** | **255.0** | **255 / 255** | **0.00%** |
| osanpo | 2304×2304 | 255.0 | 255 / 255 | 0.00% |
| cat | 2195×2195 | 255.0 | 255 / 255 | 0.00% |
| swan | 1701×2560 | 255.0 | 255 / 255 | 0.00% |

**改訂17では記述を実測値（255 単色）へ訂正する**。この訂正は §0-E-2 の因果判定に直結する — 背景が 255 単色であることにより、還流票が傷とした行（osanpo 原寸 y≈1023）が**背景と同値の 255 単色**であって「背景より明るい線」は存在し得ないと機械的に結論できる。**freeze JSON・sourceFreezeCommit・入力 SHA-256 マニフェストはいずれも不変**であり、変更は本計画書の記述のみである（Phase 1 へ巻き戻さない根拠）。

## 1. 目的とゴール

maison &. のチャーム商品について、**左右2つのチャームの組み合わせ × 革カラー × 金具（丸カン） × ロゴカラー × ステッチ**を視覚的に確認できるページ `charms.html` を新規追加する。

- チャームは **8種 + 【なし】** から左右それぞれ選択できる（swan は改訂13で「保管のみ」から正式対象へ変更・2026-07-15）
- 【なし】を選んだ場合、そのチャームは表示しない
- 既存 `index.html`（ドーナツ＆キャンディ）には影響を与えない
- ホスティングは現状どおり GitHub Pages（静的・ビルドなし・vanilla JS）

## 2. 前提と経緯

| 項目 | 内容 |
|---|---|
| 描画方式 | 旧計画書（`simulator-expansion-plan.md`）では SVG＋質感合成だったが、ミニバッグの SVG 版が「イラストに寄りすぎ」と判断され**実物写真リカラー方式へ転換**（2026-07-11）。チャームも同方式で行く（2026-07-12 ユーザー決定） |
| 生成素材 | **改訂14（2026-07-19）: 全8種の生成入力を Phase 1 で凍結した canonicalSource（フチ画像由来の colorless PNG）へ切替**（§4-1・§4-1c。正本 = sourceFreezeCommit `8b3bdb4` の plans/charm-approved-source-freeze.json）。旧受領写真（2026-07-12 の JPEG 8枚・§4-1b）・swan 旧素材2版・背景透過8枚はすべて supersededSources（生成入力禁止。背景透過はマスク生成の補助資料としてのみ使用可 — 装飾位置が α で機械的に取れる）。承認見本 = canonicalReference 8点（reference-visuals/charms/{key}-approved.png・フチ画像兼用） |
| 実寸の出典 | **ストア商品ページ（canvasart-k.stores.jp）記載のサイズで確定（2026-07-12 全種を照合済み・§4-1）**。**実測値（7種 2026-07-13・swan 2026-07-28 受領〔§0-D-1〕）とのmm単位の完全一致による二重裏付けは全8種**（§4-1 注記）。丸カンの外径42mmも swan 商品ページの「リング部分:外径42mm、内径32mm」記載で一次ソース確認済み |
| 実物の吊り仕様 | 商品ページ確認: 標準は**革紐（長さ約15cm・牛革）**、**金具（開閉式リング＋しずく型コネクタ）は +500円のカスタムオプション**。プレビューは既存 `index.html` と同じ**丸カン（42mm）表現＝金具カスタム版相当**を基準にする（§5-5・§11-8） |
| ステッチの実物仕様 | 商品ページ確認: **白ステッチ標準（黒へ変更は+500円カスタム）が明記されているのは horseshoe と osanpo のみ**。horse / frenchie / dachshund / toy poodle / cat の5種は商品説明にステッチ記載がなく、受領写真でも縁ステッチは見えない＝**ステッチなしが実物仕様**。→ stitchMode に `none` を設け、「全種で白黒トグル保証」はしない（§5-6）。**swan の stitchMode は既存構成を流用せず、無着色素材の実画素と商品仕様からフェーズ0で detect / none / fallback の完全列挙条件（§5-2）により判定する**（§4-1c） |
| 処理方式 | チャームは**固定8種**であるため、背景除去・マスク生成は利用者の端末では行わず、**準備工程で派生アセット（検証済み画像＋マスク）を事前生成してリポジトリに配置**する（§5-1）。実行時はロードとリカラー・合成のみ |
| 実装体制 | 計画書承認後に実装指示書を作成し、Opus 4.8 / Codex に委任。作業ブランチ分離・停止チェックポイント等はミニバッグ引き継ぎ指示書（`plans/impl-instructions-minibag-photo-recolor.md`）の流儀を踏襲 |
| スワン | 正式対象（8種目・改訂13）。**改訂14で生成入力の正本が確定**: `swan-colorless.png`（嘴カラーあり版フチ画像・1701×2560・SHA は §4-1c・AE-20260718-09）。所有権分類も確定 — **くちばし＝アクセント色として保持**（素材の着色ブラウンを表示・AE-20260718-08）・**ロゴ「&.」あり＝立体シルバー表現のまま採用**（G 所有・metalTone 連動・立体陰影の最終判断は Phase 5 QA・AE-20260718-07）・**stone〔旧「金具鋲」表記〕＝保持**（改訂17 で `swanKeep` = beak ∪ stone の2成分に確定・§0-E-6。**保持はこの2成分ちょうどで、3成分以上は生成エラー**）。旧素材（swan.jpg・swanフチ画像.png・swanフチ画像２.png・swan背景透過.png）は supersededSources（生成入力禁止・§4-1c） |

## 3. スコープ

**やること**

- `charms.html` の新規作成（自己完結1ファイル、既存デザイン言語踏襲）。ページ内に `index.html` へのリンクナビを含む
- **準備ツール `tools/charm-prep.html`** の更新（Phase 1 凍結の canonicalSource 8点から派生アセットを生成・検証するブラウザツール。§5-4。実装済みコミット 39fe815 を正として拡張する — §5-4 の還流契約）
- チャーム8種の派生アセット生成（全8種 colorless モード §5-2。背景除去済み画像＋所有権マスク＋定義値・straight RGBA PNG・§5-4）
- 左右チャーム選択（各9択: 8種＋なし）、**左右独立の leatherVariant 選択（LEATHER_VARIANTS 27件のうち当該チャームの appliesToCharms に含まれる variant のみ。現行8チャームは calf 24色 — §5-6・§6-1）**、金具（銀/金・丸カンに適用）、**ロゴカラー（金/銀・チャームの箔押しロゴに適用・金具と独立 §5-6）**、ステッチ（白/黒・**ステッチ有チャーム〔stitchMode ≠ none。現状 horseshoe / osanpo。swan は Phase 4 判定 — detect / fallback となった場合は対象に加わり UI 注記も更新 §6-1〕にのみ適用**。実物仕様 §2）
- 組み合わせサマリー文＋コピーボタン、カラーチャート参照（canonicalColorChart 5枚・§5-6）、注意書き

**やらないこと（別工程・対象外）**

- **`index.html` への逆リンクナビ追加は実装委任の対象外**とする。実装エージェントは `index.html` に一切触れない（ミニバッグ委任規約の「index.html 変更禁止」をそのまま維持）。逆リンクは `charms.html` の受け入れ完了後、メインセッション側が最小差分で追加する（担当と順序を §8 フェーズ4 に明記）
- 要望②③④（ウォレット/バッグ/ネームタグ）への着手
- チャーム写真の物理シミュレーション（揺れ・傾き）。配置は垂直吊り下げの静止画とする

## 4. チャーム仕様

### 4-1. チャーム一覧（8種 + なし）— 実寸はストア商品ページ記載で確定（2026-07-12 照合・**全8種が実測照合済み**: 7種 2026-07-13・swan 2026-07-28〔§0-D-1〕）

| # | クライアント表記 | UI表示名 | HP記載サイズ | 実寸 W×H (mm) | CHARMSキー | canonicalSource（Phase 1 凍結・source-photos/。SHA と寸法は §4-1c 正規入力マニフェストが正本） |
|---|---|---|---|---|---|---|
| 1 | horse | horse | 約H7×W9.5cm | 95×70 | `horse` | `horse-colorless.png`（horseフチ画像由来・2158×2233） |
| 2 | horseshoe | horseshoe | 約H5.4×W4.6cm | 46×54 | `horseshoe` | `horseshoe-colorless.png`（horseshoeフチ画像由来・2020×2385。改訂13の horseshoe色透過.png と同一 byte） |
| 3 | frenchie | frenchie | 約H7.5×W6cm | 60×75 | `frenchie` | `frenchie-colorless.png`（frenchieフチ画像由来・1808×2664） |
| 4 | dachshund | dachshund | 約H6×W9cm | 90×60 | `dachshund` | `dachshund-colorless.png`（dachshundフチ画像由来・2195×2195） |
| 5 | toy poodle | toy poodle | 約H6×W8cm | 80×60 | `toypoodle` | `toy-poodle-colorless.png`（toy poodleフチ画像由来・2176×2176・**スペース除去**） |
| 6 | osanpo | osanpo（骨型ネームタグ） | 約H4×W9.2cm | 92×40 | `osanpo` | `osanpo-colorless.png`（osanpoフチ画像由来・2304×2304） |
| 7 | cat | cat | 約H6.5×W6.8cm | 68×65 | `cat` | `cat-colorless.png`（catフチ画像由来・2195×2195） |
| 8 | swan | swan | チャーム部H9×W9cm | 90×90（**実測 2026-07-28 受領・ストア記載と完全一致 — §0-D-1**） | `swan` | `swan-colorless.png`（swanフチ画像　嘴カラーあり.png 由来・1701×2560・AE-20260718-09） |
| 9 | — | 【なし】 | — | — | `"none"` | （画像なし） |

- **改訂14: 全8種の生成入力は Phase 1（sourceFreezeCommit `8b3bdb41bd3205287f2612ca9fb071b4d53e8376`）で凍結済みの canonicalSource（上表）であり、全種 sourceMode="colorless" / toneMode="flat"**（§4-1c・§5-2）。承認見本は canonicalReference（`reference-visuals/charms/{key}-approved.png`・フチ画像兼用のため canonicalSource と同一 byte・AE-20260718-02）。旧受領写真 8枚（JPEG）・swan 旧素材2版・背景透過8枚は supersededSources（§4-1c）

- 実寸の出典: **ストア商品ページ（canvasart-k.stores.jp）の記載サイズ（2026-07-12 確認）**。旧計画書の値と全種一致。さらに**実測寸法（2026-07-13 受領: horseshoe W46×H54 / frenchie W60×H75 / dachshund W90×H60 / toy poodle W80×H60 / cat W68×H65〔実装エージェント（Codex）フェーズ0報告〕・horse W95×H70 / osanpo W92×H40〔クライアント実測・同日受領〕・**swan W90×H90〔クライアント実測・2026-07-28 受領 — §0-D-1〕**）が上表の値と**全8種**mm単位で完全一致**し、確定値として二重に裏付け済み
- **swan（8種目・改訂13）の実寸は改訂16で実測確定（W90×H90・2026-07-28 受領 — ストア記載チャーム部 H9×W9cm = 90×90mm と完全一致・§0-D-1）**。改訂15までの「confirmed=false で開始し preDistortion > 3% なら実測受領まで停止（§7-9 対処①）」の契約は、Phase 4 バッチ実測（preDistortion 7.081% → 契約どおり停止）→ 実測受領（2026-07-28）の順で消化済み。prep.json の `sizeMmMeasured.confirmed = true`（受領日 2026-07-28）とし、§5-2 発動述語の成立により**アスペクト補正が自動発動**する（素材フチ画像の前景比率 W:H ≈ 1:1.070 を実測 1:1 へ矯正 — 補正後の再計測値で検査3 を判定）。W/H の対応は正方形のため取り違えリスクなし（§4-1 注記のとおり）
- W×H の対応: swan のストア記載「チャーム部H9×W9cm」は正方形のため W/H の取り違えリスクはないが、pxPerMm の W/H 各計測（§5-2）で新素材の縦横比と照合する
- **horse / osanpo も実測受領（2026-07-13・horse W95×H70 / osanpo W92×H40）— 上表と完全一致。さらに swan の実測受領（2026-07-28・§0-D-1）により全8種の実寸が実測で確定**（sizeMmMeasured.confirmed = **8種 true**・§5-2）。これにより horse は preDistortion > 3% の場合にアスペクト補正が自動発動する条件が揃い（実測比 0.3〜0.5% のため発動しない見込み）、osanpo は manual-line のため引き続き shouldApply 対象外（超過時は停止 → 個別設計・§7-9 対処③）。万一将来の再実測で相違が判明した場合の修正経路は §10「実寸データ・校正の誤り」のとおり（準備ツール入力・prep.json 側修正 → 派生アセット再生成＋検査1〜8再実行 → 転記照合10項目の再転記 → **§7-11 の81配置・§9-2 の実寸比率の再検証**〔§5-5〕。**CHARMS 単独修正は禁止**）。なお §7-9 の pxPerMm 歪み検査（≤3%）は幅高比の整合を機械検査するため、sizeMm の縦横比誤りはフェーズ0で自動検出される（等方的な絶対値誤差は検出範囲外 — 7種の実測一致により HP記載の精度は裏付け済み）
- **フェーズ0歪み検査の実績（2026-07-13）**: 実測確定済み5種の全てが歪みゲート3%を超過（horseshoe 8.7% / frenchie 4.0% / dachshund 6.1% / toy poodle 10.4% / cat 3.4% — Codex 計測・メインセッション独立検算で一致確認）。革端マスクは正確で実測も上表と一致しているため、**写真の投影が実物の縦横比から乖離している**（置き方・革の柔らかさ由来。同環境の horse は実測比（ストア記載と一致・2026-07-13 受領）0.3〜0.5% で合格しており撮影系の系統誤差ではない）と確定 → **実測寸法を正とするアスペクト補正（§5-2・改訂10）で矯正する。sizeMm の数値は変更しない**
- HP値は「約」付きの丸め値であり、写真との整合はフェーズ0の pxPerMm 歪み検査（≤3%・§5-2。歪み超過はアスペクト補正 §5-2 で矯正のうえ補正後値で判定）で機械的に担保する
- 実寸は**「革本体のみ（吊り金具を含まない）」の寸法と定義**する（受領素材は吊り金具なしのため、素材の革本体＝HP記載サイズがそのまま対応する）。前景全体の高さ・張り出しは個別の計測項目を設けず、**`fgBboxPx`（§5-2）÷ `pxPerMm` から導出**する
- **UI表示名は全チャームとも画像ファイル名と同じ英語表記で確定（§11-11・2026-07-13 ユーザー回答。swan は改訂13で同契約に従い追加）**: horse / horseshoe / frenchie / dachshund / toy poodle / osanpo / cat / **swan**（ストア商品名の英語表記とも一致）。osanpo は商品名「ネームタグ｟osanpo｠」の**骨（ボーン）型ネームタグ**（旧計画の「お散歩ネームタグ」という理解を写真で具体化）
- **左右に同一チャーム（色違い含む）を選択できる仕様とする**（実物でも同一チャーム2点付けが可能なため。ステージ寸法§5-5 もこの前提で計算）
- 素材ファイル名は日本語・スペースを避け英数字に統一（GitHub Pages の URL エンコーディング事故防止。既存の日本語名ファイルはそのまま）。**canonicalSource の Git 管理名は全8種 `{CHARMSキー}-colorless.png` で統一**（toy poodle のみ `toy-poodle-colorless.png` — スペース除去。既存の `source-photos/*.jpg`〔参考保管〕を上書きしない別ファイルとして管理・§4-1c）。**Git 管理名と SHA の正本は plans/charm-approved-source-freeze.json**
- 生成入力素材（canonicalSource 8点・grain 2点）・canonicalReference・canonicalColorChart・派生アセットはページが参照し得るため**公開リポジトリ上で公開される**。公開可否も含めて素材提供済みの前提で確定（§11-7・2026-07-13。懸念が出た場合のみ再相談）

### 4-1b. 旧受領写真の実態（2026-07-12 確認 — 改訂14では参考記録）

**改訂14注記: 本節は旧受領写真（2026-07-12 の JPEG 8枚）の観察記録であり、全8種の生成入力は §4-1c の canonicalSource（フチ画像由来・colorless）へ切替済み**。本節の事前判定（§7-1/2/3/5 ○ 等）はいずれの生成入力についても効力を持たず、生成入力の判定は Phase 4（フェーズ0相当）で canonicalSource の実画素検証のみを正とする。装飾（ロゴ・ラインストーン・ステッチ・くちばし等）の存在の参考情報としてのみ保持する。

8枚共通: **白背景・チャーム本体のみ（吊り紐・金具なし・吊り穴のみ開いている）**。箔押しブランドロゴ「&.」は swan を除く7枚で確認済み（swan のロゴは canonicalSource で確認済み — AE-20260718-07・§4-1c）。JPEG（αなし）・長辺 1054〜1554px。

| チャーム | 写真の革色 | 解像度 (px) | 縁ステッチ | 装飾 | 吊り穴の位置 | 向き |
|---|---|---|---|---|---|---|
| horse | ブラック | 1257×1302 | なし | 金ロゴ | 背中上部 | 頭が左 |
| horseshoe | ダークカーキ | 1175×1392 | **白ステッチあり** | 金ロゴ＋**ラインストーン6個** | 右上端 | U字正立（左右ほぼ対称） |
| frenchie | アイボリー | 1054×1554 | なし | 金ロゴ | 右耳 | 頭が左 |
| dachshund | キャメル | 1280×1280 | なし | 銀ロゴ | 背中上部 | 頭が左 |
| toy poodle | キャメルオレンジ | 1198×1366 | なし | 金ロゴ | 背中上部 | 頭が左 |
| osanpo | ブラック | 1290×1268 | **白ステッチあり** | 金ロゴ | 上下2箇所 | 骨が斜め（左下→右上） |
| cat | ブルー | 1279×1280 | なし | 金ロゴ | 頭部 | 頭が左上・尻尾が右 |
| （swan・旧参考写真） | ライトブルーグレー | 1142×1434 | 白ステッチ | 金具鋲＋ラインストーン・くちばし別色革 | 首 | 頭が左（**swan 本体は正式対象〔改訂13〕 — 本行は旧写真の観察記録で、生成入力は §4-1c の無着色素材**） |

この確認により §7 チェックリストの一部は**事前判定済み**となる: 全体写り○（§7-1）・正立○（§7-2）・白背景○（§7-3）・照明概ね均一○（§7-5）。**解像度（§7-4）は画像全体では十分だが前景基準の最終判定はフェーズ0**。pxPerMm歪み（osanpo は手動計測線モード §5-2）とロゴ／保持の塗り分けはフェーズ0で機械検査＋目視（§7-8。吊り金具は写っていないため対象外）。

**改訂14注記**: 全8種の生成入力が §4-1c の canonicalSource（フチ画像由来）へ切り替わったため、**旧写真の色・明暗は生成・表示のどこにも使用しない**（§5-2 無着色素材モード — 全種適用）。

**扱いを定義する要素（§5-6 に規定）**

1. **箔押しロゴ「&.」**: 全8種で存在確認済み（swan は AE-20260718-07 — 右下・立体シルバー表現のまま採用）。**ロゴマスク（G）所有とし、`metalTone()` を「ロゴカラー（ゴールド/シルバー）」の独立トグルに連動**させる（§11-9 確定・2026-07-13。丸カンの金具トグルとは別の選択肢。素材上の箔色の個体差は表示に影響しない — masks 化した時点で素材の箔色ではなく選択ロゴカラーで描画されるため）
2. **ラインストーン（horseshoe の6個・swan の stone 1個〔改訂17 で確定 — §0-E-6 の `swanKeep`〕）**: 革でも糸でもない装飾。**保持（リカラー対象外・masks.α=0）とし、素材の色をそのまま表示**する（§11-10 確定・2026-07-13。§5-2 の保持チャンネル）。表示品質は Phase 4 の目視検査（§7-8 — horseshoe 8ケース）で確認し、§9-3 の固定検証セットで回帰確認。horseshoe の6ストーンは**位相検査（検査6・§5-4 改訂14）**で成分数・面積・位置の機械検査も行う
3. **吊り穴**: 革の内側に空いた小穴（背景色が透ける）。前景マット生成時に**穴として抜く（α=0）**。穴周囲のフェザー帯も所有権恒等式（§5-2）の対象

### 4-1c. Phase 1 凍結素材の実態（改訂14・2026-07-19 — sourceFreezeCommit `8b3bdb41bd3205287f2612ca9fb071b4d53e8376`）

全8種の生成入力は **Phase 1（同期計画 §5）で凍結された canonicalSource＝フチ画像由来の colorless PNG**。フチ画像は**革面が未着色（白）のイラスト調デザイン素材**（写真ではなく意匠データ由来・**白背景 255 単色**〔改訂17で 254 から実測値へ訂正 — §0-E-12〕・αなし）であり、全8種を無着色素材モード（sourceMode="colorless"・§5-2）で処理する。承認台帳は reports/charm-combo/rev14/phase-1-approvals.md（AE-20260718-01〜14）、freeze の正本は plans/charm-approved-source-freeze.json / plans/charm-approved-color-freeze.json。

**正規入力マニフェスト（全8種＋グレイン2種 — freeze JSON からの機械導出転記〔参考〕。値の正本と出典は次のとおりで、本表と不一致の場合は正本が勝つ）**

- **8チャームの key・canonicalPath・sourceMode・寸法・SHA-256 の正本 = sourceFreezeCommit 上の plans/charm-approved-source-freeze.json**（entries[].pipelineSource）。**INPUT_MANIFEST への転記・検査M-1・検査8 の照合値は freeze JSON の値を直接参照して生成**する（本表を経由した手写しを禁止）
- **グレイン2点の byte 同一性（SHA-256）の正本 = plans/charm-approved-color-freeze.json** の materials[].grainSource.**outputSha256**。**Git 管理下の canonicalPath は本計画書が定義**する: calf = **リポジトリ直下の `質感.jpg`**（main 管理下〔bbc1b70〕の既存ファイル — sourceFreezeCommit `8b3bdb4` のツリーに実在し SHA `d0b7b644…` が outputSha256 と一致することを確認済み。freeze の outputPath は Git 管理外の同名原本を指すが同一 byte。**改訂13の「source-photos/leather-grain.jpg へコピー」契約は撤回** — コピーは実施されておらず、実在する管理下ファイルを直接 canonical とする方が freeze ツリーとの `git show` 同定が成立するため）／chevre = `reference-visuals/colors/grains/chevre-grain.png`（freeze の outputPath そのまま・8b3bdb4 に実在）。**path は計画書・byte は freeze が正本**という分離であり、SHA 不一致時は freeze が勝つ

| CHARMSキー | sourceMode | 正規入力 | 寸法 | 期待SHA-256 |
|---|---|---|---|---|
| horse | colorless | source-photos/horse-colorless.png | 2158×2233 | `ec6ef0cc6a2da23c172b0f3d04a06c4c2e91958babe980ee1908683d7955e6f0` |
| horseshoe | colorless | source-photos/horseshoe-colorless.png | 2020×2385 | `111196581550f80d8d1edb6a4f929af9e7bdb3e239c1680bbc6459c53bf9341c` |
| frenchie | colorless | source-photos/frenchie-colorless.png | 1808×2664 | `05c08873322c11f6ed48ef2b6e272f2582bd6f597fd2ced874ef15d84b9d6edf` |
| dachshund | colorless | source-photos/dachshund-colorless.png | 2195×2195 | `e0e228f792b69eae074695ef020c9af1c2ad34306cf05fb735e7b131d17487da` |
| toypoodle | colorless | source-photos/toy-poodle-colorless.png | 2176×2176 | `b4251b764c0ca3f35af6d6f0904b52c909811bc44637dc64b12d8ccdf64bfcba` |
| osanpo | colorless | source-photos/osanpo-colorless.png | 2304×2304 | `f9153732573aac77090914c5abf08fb98c5456e8a345cd098e74d23a48cef28e` |
| cat | colorless | source-photos/cat-colorless.png | 2195×2195 | `563b5c975eaebbafa3fbb25a1720d0c5021022dca23cb2f069d46553be10bce7` |
| swan | colorless | source-photos/swan-colorless.png | 1701×2560 | `77c8a315d37a0dad23eb3dd564bdd2baf7a26fe2f0583873de1ff2297461c435` |
| （グレイン calf） | — | 質感.jpg（リポジトリ直下） | 1011×676 | `d0b7b644bcce2691b42b776f054ad2dd3f6c23313dab72f6a1b83b1292943a2a` |
| （グレイン chevre） | — | reference-visuals/colors/grains/chevre-grain.png | 824×383 | `5d3f86a0f2da40a482be2e4efc6aa98c636b108b05de32b3cdb341ac9bba6774` |

- **canonicalReference 8点**（reference-visuals/charms/{key}-approved.png）は**フチ画像兼用（canonicalSource と同一 byte・AE-20260718-02）**。形状・縁・ロゴ・保持装飾の照合はこの reference を正とする
- **chevre-grain.png は Phase 1 で生成済みの canonical grain**（正本ソース =「シェーブル質感.jpg」844×403・SHA `f0a65c5b841285a1e34f9f39a7d9500ad414a23c11569c5b94c7a42b0492518d`・採用 ROI = 縁10px除外 {x:10, y:10, w:824, h:383}・grainSamplingMethod=`jpeg-roi-luma-straight-v1`・AE-20260718-13）。準備ツールは chevre-grain.png（output）を入力として使用し、再導出はしない（grain の生成証跡は color freeze の grainSource が正本）
- **禁止入力（supersededSources — 正本列挙は plans/charm-approved-source-freeze.json の各 entry）**: 旧受領写真 8枚（horse.jpg `7e3d8444…`・horseshoe.jpg `91ee0d22…`・frenchie.jpg `bdc8ccb6…`・dachshund.jpg `b6a4273e…`・toy poodle.jpg `4ce01fa9…`・osanpo.jpg `2793ad00…`・cat.jpg `e5b94c2e…`・swan.jpg `c24f59cc…`）／swan 旧フチ画像2版（swanフチ画像.png `847e75bb…`=チェッカー柄焼き込みで不適・swanフチ画像２.png `87ad635c…`=くちばし無着色）／**背景透過 8枚**（全て革面まで α=0 の装飾オーバーレイ〔不透過率 0.7〜5.8%〕のため生成入力に使えない — **マスク生成の補助資料（manual-owner / manual-keep の下絵）としてのみ使用可**・AE-20260718-03）。いかなる sourceMode でも生成入力に使用してはならない（prep.json に自己整合的に記録しても検査M-1 のマニフェスト照合で不合格になる・§5-4）

**素材の来歴（Phase 1 で凍結済み — コピー・照合作業は完了しており、実装側の追加コピーは不要）**

| canonicalSource | 元ファイル（Git 管理外 `charm一覧/`） | 凍結状態 |
|---|---|---|
| horse / frenchie / dachshund / toy-poodle / osanpo / cat の各 -colorless.png | 各「{名前}フチ画像.png」 | sourceFreezeCommit へ SHA 同一コピー済み（AE-20260718-01） |
| horseshoe-colorless.png | horseshoeフチ画像.png（改訂13 の horseshoe色透過.png と同一 byte） | 同上 |
| swan-colorless.png | swanフチ画像　嘴カラーあり.png（2026-07-18 20:50 受領） | 同上（AE-20260718-09） |
| 質感.jpg（canonical そのもの） | リポジトリ直下・main 管理下〔bbc1b70〕・1011×676 | 既存 Git 管理ファイル＝8b3bdb4 のツリーに実在（AE-20260718-14・approved-reuse・コピー不要） |
| chevre-grain.png | シェーブル質感.jpg（素材ルート直下・ユーザー提供） | Phase 1 で生成し freeze 済み（AE-20260718-13・distinct） |

- 実装セッションは Git 管理外パスを参照しない。**入力の同定は `git show <sourceFreezeCommit>:<canonicalPath>` と上記マニフェストの SHA-256 照合のみで行う**（不一致＝別版混入として停止）。フチ画像 8点の背景はコーナー 100×100 が**輝度 255 単色**であることを機械確認済み〔改訂17 実測 — 全8種で 100×100 の min/max = 255/255・255未満 0.00%。Phase 1 の phase-1-approvals.md には 254 と記録されているが、freeze の SHA は不変で素材は同一であり、記録側の誤記または輝度計算の丸めと判断して本計画書の記述を実測値へ訂正した。§0-E-12〕
- **horseshoe 素材の目視実態（2026-07-15 確認・改訂14でも同一 byte のため有効）**: 革面＝**純白・無地（質感なし）**／白のロープ調ステッチ2周（外周・内周）／黒系の縁取り線（最外周・内側U字・吊り穴周囲）／金色台座のラインストーン6個／金色ロゴ「&.」（右下）／背景＝白（αなしのため背景除去は Phase 4 のパイプライン②〜④で実施）。吊り穴（右上）の中央と U字中央の空間は背景と同じ白
- **horseshoe の所有権分類（ユーザー指定・2026-07-15・改訂14でも有効）**:
  - **革（R）**: 中央革面／ステッチ外側の革帯／最外周および内側U字の外枠線／右上の吊り穴周囲線／**アンパサンド内部の上下2つの穴**（「&」のカウンター空間 — 透明にしない）
  - **ロゴ（G）**: アンパサンドの金色文字本体のみ
  - **ステッチ（B）**: 白／黒切替対象の縫い目（ロープ調2周）
  - **保持（masks.α<128）**: 6個のラインストーンと台座（金色リム含む）
  - **透明（base.α=0）**: 画像背景／U字中央の空間／右上吊り穴の中央
- **swan の確定事項（改訂14 — AE-20260718-07/08/09 により「受領後確定」条項を解消）**:
  - **pipelineSource = swan-colorless.png**（嘴カラーあり版・1701×2560・RGB・αなし。四隅白 **255 単色**✓〔§0-E-12〕・くちばしブラウン系 17,051px〔平均 RGB 149,87,44・革シボ付き〕を機械確認済み）
  - **くちばし = アクセント色として保持（masks.α<128）**: 素材に着色済みのブラウンをそのまま表示する（AE-20260718-08。革連動にはしない）
  - **ロゴ「&.」= あり（右下・stone〔旧「金具鋲」表記〕の左）**: シルバーの立体表現のまま**ロゴ所有（G）**とし metalTone でロゴカラー（金/銀）に連動させる。立体陰影の見た目の最終判断は Phase 5（形状 QA）で行う（AE-20260718-07。ロゴなし時の UI 条件改訂〔旧 §11-13③ 停止ゲート〕は不要となった）
  - **保持（masks.α<128）は改訂17 で確定**: **`swanKeep` = beak ∪ stone の2集合のみ**（§0-E-6）。〔改訂13〜14 の「金具鋲・ラインストーン等の装飾」という暫定表現は、実測により**このストーン1個**と特定されたため失効し、本項の定義へ置換された。**keep 集合へこの2つ以外の要素を加えてはならない**（検査6 の fail-fast で機械検出する）〕
  - **stitchMode は Phase 4 で確定**: 素材実画素から §5-2 の完全列挙条件（detect / none / fallback）で判定する（白ステッチが素材に存在するため detect 見込みだが仮確定しない）。確定後に固定目視回帰セットのケース数（§9-3）と UI 注記（§6-1）を連動更新する
  - **実寸は実測で確定（改訂16）**: クライアント実測 W90×H90（2026-07-28 受領）がストア記載 90×90 と完全一致（§0-D-1）。sizeMmMeasured.confirmed=true とし、Phase 4 実測 preDistortion 7.081% > 3% のため §5-2 発動述語によりアスペクト補正が自動発動する（改訂15までの「confirmed=false・歪み>3% で停止」契約は実測受領で消化済み — §4-1）

### 4-2. 【なし】の挙動

| 左 | 右 | 表示 | UI状態 |
|---|---|---|---|
| あり | あり | 丸カンの下、中心線から左右に振り分けて2つ表示 | 全コントロール有効（※ステッチトグルは下記条件） |
| あり | なし | 残ったチャームを**丸カン真下センター**に表示 | 【なし】側のカラースウォッチを無効化（グレーアウト） |
| なし | あり | 同上（右チャームをセンターに） | 同上 |
| なし | なし | **丸カンのみ**表示 | 両カラースウォッチ＋ステッチトグル＋**ロゴトグル**も無効化。金具トグルは有効（丸カンに適用） |

- **ステッチトグルの有効条件**: 表示中のチャームに**ステッチ有チャーム（stitchMode ≠ none。現状 horseshoe / osanpo。swan はフェーズ0判定・§4-1c）が1つ以上ある場合のみ有効**。ステッチなしチャームだけの表示では無効化（グレーアウト）する — 実物にないステッチ選択肢を見せない（§5-6）
- **ロゴカラートグルの有効条件**: チャームが1点以上表示中のときのみ有効（**全8種にロゴがあることを確認済み** — swan は AE-20260718-07。チャームなしでは無効化 — 丸カンにロゴはない）
- 切替は即時再描画（アニメーション不要）
- サマリー文も表示状態に連動（§6-3）

## 5. 技術設計

### 5-1. 全体アーキテクチャ — 準備工程と実行時の分離

既存 `index.html` は「1枚の完成写真のピクセルを実行時に分類して色を差し替える」方式だが、実行時の自動分類は撮影条件に強く依存する。今回はチャームが**固定8種**なので、品質が不安定な処理はすべて**準備工程**に寄せ、**人が目視検証してからコミット**する。

```
【準備工程】（tools/charm-prep.html を使い、Phase 4 で1回だけ実施。成果物をコミット）
   生成入力（canonicalSource・全8種 colorless・§4-1c）→ 背景除去（前景マットα生成）→ defringe（背景色かぶり除去）
         → 所有権マスク生成（革/ロゴ/ステッチ＋保持、自動候補＋手動補正）
         → グレイン焼き込み（material 別 base 派生 — 適用 profile の grain 入力の局所凹凸をニュートラルグレーで革画素へ焼き込み ⑦'・§5-2。現行の生成対象は calf のみ）
         → 検証ビュー（暗色背景合成・恒等式検査・マスクオーバーレイ）
         → 派生アセット書き出し（straight RGBA PNG・§5-4）: {charm}.base.png ＋ {charm}.masks.png ＋ 定義値

【実行時】（charms.html。ピクセル走査はリカラーのみ）
   派生アセットをロード → リカラー（革=flatLeatherTone〔toneMode="flat"・全8種・DISPLAY_VARIANTS 入力・§5-6〕 / ロゴ=metalTone〔ロゴカラー連動〕 / ステッチ=白黒塗り / 保持=base.RGBそのまま）
         → チャーム完成ビットマップ（cache key = charmKey|variantKey|materialKey|logo|stitch。丸カンの金具トグルはキーに含めない・§5-7）
         → mm基準ステージに丸カン（独立表示）＋左右チャームを実寸比率で drawImage 合成
```

- 実行時に背景除去・マスク生成を行わないため、端末差・初回遅延・誤検出のリスクを排除できる
- 実行時生成方式（旧案）は採用しない。将来チャームが大幅に増えて事前生成が回らなくなった場合にのみ再検討する

### 5-2. 派生アセットとチャーム定義テーブルの契約

**ファイル構成**

```
assets/charms/
  horse.base.png             … 表示用前景画像（RGBA、背景除去＋defringe済み、長辺≤1200pxに正規化）
  horse.masks.png            … 所有権マスク（RGB: R=革 / G=ロゴ / B=ステッチ、α: 保持フラグ。baseと同寸）
  horse.prep.json            … 準備工程の再現データ（§5-4。通常表示では読まない — `?debug=1` の転記照合時のみ読込）
  horse.manual-alpha.png     … 手動補正レイヤ①: 前景マット強制（作業座標系と同寸・§5-2。実行時は読まない）
  horse.manual-owner.png     … 手動補正レイヤ②: 所有権強制（作業座標系と同寸・§5-2。実行時は読まない）
  horse.manual-keep.png      … 手動補正レイヤ②': 保持強制（ラインストーン等。作業座標系と同寸・§5-2。実行時は読まない）
  horse.manual-inpaint.png   … 手動補正レイヤ③: base.RGB置換（fallback元糸中和等。作業座標系と同寸・§5-2。実行時は読まない）
  …（8種 × 一式。手動補正が無いレイヤのファイルは省略可）
assets/rings/
  ring-silver.png / ring-gold.png … 丸カン透過PNG（準備工程で生成。42mm外径の共通基準枠に正規化・中心一致）
source-photos/               … 生成入力と参考素材（§4-1・§4-1c）: canonicalSource 8種（{key}-colorless.png — 全種フチ画像由来・Phase 1 凍結・生成入力）
                               ＋ 旧受領写真 8枚（*.jpg — supersededSources・形状参考の保管のみ・生成入力への使用禁止 §4-1c）
質感.jpg（リポジトリ直下）    … calf のグレイン入力（main 管理下・8b3bdb4 に実在・§4-1c・§5-2）
reference-visuals/colors/grains/chevre-grain.png … chevre のグレイン入力（Phase 1 生成済み・§4-1c）
reference-visuals/charms/    … canonicalReference 8点（承認見本・§4-1c）
tools/charm-prep.html        … 派生アセット生成・検証ツール
```

**base.png（前景マット）の契約**

- α = 前景マット（0–255）。エッジは1–2pxのフェザー
- **defringe 必須**: α<255 の画素の RGB は最近傍の不透明画素色で置換し、背景（白系）の色かぶりを除去する（写真は JPEG 白背景・無着色素材は白背景 — §4-1b・§4-1c）
- **最近傍選択の共通規則（④ defringe・⑧'-2 再defringe〔§5-2 アスペクト補正〕・検査4〔§5-4〕で同一規則・同一実装〔同一関数〕を共有する — 改訂12）**: 対象画素ごとに `α=255` の画素のうち**空間の二乗ユークリッド距離が最小**のものを選び、同距離候補が複数ある場合は**線形画素インデックス `y×width+x` が最小**のものを選ぶ（y 優先→x）。`α=255` の画素が画像内に0件なら生成エラー（そのチャームは派生アセット不成立＝フェーズ0不合格）。実装アルゴリズムは自由だが、結果はこの規則との完全一致を要する（決定的 — 検査8の再生成一致・期待出力ハッシュの前提）
- 検証: 暗色背景（#333）と明色背景（#eee）の両方に合成し、輪郭に白/黒のハロが出ないこと（準備ツールの検証ビューで目視＋エッジ帯の彩度異常の自動チェック）

**masks.png（部位所有権）の契約 — 前景マットと所有権は別概念**

- **RGBチャンネル = 所有権**: **R = 革（photoLeatherTone・革カラートグル連動） / G = ロゴ〔金属箔「&.」〕（metalTone・ロゴカラートグル連動 — 金具トグルではない・§5-6） / B = ステッチ（白黒トグル連動）**。受領素材にチャーム側の吊り金具は写っていないため「金具」所有はチャーム側に存在しない（丸カンは別アセット。将来 attachmentMode="photo" の金具付き写真を扱う場合は所有権設計の再検討が必要 — その時点で計画書改訂）
- **αチャンネル = 保持フラグ（2026-07-13 ユーザー回答 §11-10 により導入）**: 書き出しは二値（255 = 通常所有 / 0 = **保持**〔リカラー対象外・base.RGB をそのまま表示。horseshoe のラインストーン等〕）、読み込み判定は `masks.α < 128` を保持とする。**保持判定はαのみで行い、保持画素の masks.RGB は読まない**（PNG デコード時のプリマルチプライで α=0 画素の RGB が壊れても影響しない設計）
- 所有権恒等式は **base.α を基準**に判定する:
  - `base.α > 0` かつ `masks.α ≥ 128` の画素: R+G+B=255（1画素1所有者の排他。境界はソフト値可、ただし合計は常に255）
  - `base.α > 0` かつ `masks.α < 128` の画素: **保持**（RGB 値は不問 — 読まないため）
  - `base.α = 0` の画素: masks.α=255 かつ R=G=B=0（所有なし。保持と誤読させない）
- **フェザー帯（0 < base.α < 255）の画素も必ずいずれかの所有（または保持）を持つ**。これにより輪郭画素も選択色へ変換され、「アイボリー写真をブラックに変換したら輪郭だけ元色の明色ハロが残る」事故を防ぐ（出力αは base.α のままなのでエッジの柔らかさは維持される）
- **所有権参照の共通述語**: 本計画書で「革所有（R≥128）」「ロゴ所有」「ステッチ所有（masks.B）」等と書くときは、**常に `masks.α ≥ 128` の画素のみ**を対象とする（保持画素は所有権を持たず、その masks.RGB は不定値のため参照してはならない）。baseLum・bboxPx・ステッチ画素数・クリップ領域などの派生値・検査の集計はすべてこの述語込みで行う
- 恒等式検査は準備ツール（書き出し前・必須）と `?debug=1`（実行時・任意）の双方で機械検査

**リカラー実行時の合成式（アルファの二重減衰を防ぐ）**

```
出力α = base.α                                   // そのまま透過。再減衰・再適用しない
if (masks.α < 128)  出力RGB = base.RGB           // 保持（ラインストーン等）: リカラーしない
else                出力RGB = 革変換×(R/255) + ロゴ変換×(G/255) + ステッチ変換×(B/255)
                                                  // R+G+B=255 なので係数和は常に1
  革     : toneMode="flat"  → flatLeatherTone(base.RGB, DISPLAY_VARIANTS[選択variantKey].approvedHex, 対応baseLum)  // 全8種（無着色素材モード・§5-6。base と baseLum は選択 variant の materialKey に対応する materialAssets のもの — 改訂14）
           toneMode="photo" → photoLeatherTone(base.RGB, 選択革色, baseLum, 1.0)   // 将来の photo 素材用の予備契約（適用チャーム0・第4引数 shadeFactor=1.0 固定・§5-3）
  ロゴ   : metalTone(base.RGB, 選択ロゴカラー)    // ロゴカラートグル（金/銀）に連動。丸カンの金具トグルとは独立（§5-6）
  ステッチ: 選択ステッチ色（白 #f5f5f4 / 黒 #1a1a16）の固定色＋輝度による僅かな陰影
```

- 完成チャームビットマップは**アセット解像度（base.png と同寸）で生成・キャッシュ**し、ステージへは全体を等倍率 `N / pxPerMm` で拡縮して `drawImage` する（ステージ座標系でピクセル操作はしない）

**無着色素材モード（sourceMode="colorless" / toneMode="flat"）の契約（改訂13・2026-07-15 ユーザー承認方式）**

- **適用対象**: 生成入力が無着色デザイン素材（革面が未着色・§4-1c）のチャーム。**改訂14で全8種が該当**（Phase 1 凍結の canonicalSource は全種フチ画像由来の colorless）。**特定チャームへのハードコードはせず**、prep.json の入力値 `sourceMode`（"photo" | "colorless"）と CHARMS の `toneMode`（"photo" | "flat"）で駆動する**共通モード**として実装する（photo/photo の契約は将来素材用の予備契約として温存 — 適用チャームは現在0）
- **対応の機械保証**: `sourceMode="colorless" ⇔ toneMode="flat"`・`sourceMode="photo" ⇔ toneMode="photo"` の1:1対応を書き出し前検査M-2（**全チャーム共通** — photo 側の誤転記も検出・§5-4）で機械検査する。生成入力そのものの正しさは正規入力マニフェスト照合（§4-1c・検査M-1）で機械保証する
- **原則（ユーザー承認 2026-07-15・改訂14で③②を更新）**: ①素材の革の元色（および旧写真の色・明暗）をリカラー結果へ**一切混入させない** ②革の質感は**適用 material の grain 入力**（calf = リポジトリ直下の `質感.jpg`／chevre = `chevre-grain.png`・§4-1c）から**色を捨てて局所的な凹凸・明暗情報だけ**を取得する ③表示革色は **LEATHER_VARIANTS の approvedHex（カラーチャート抽出値・Phase 1 凍結・AE-20260718-11/12）を基準**とする（DISPLAY_VARIANTS・§5-6 — 改訂14で旧 referenceIvory / COLORS 基準を置換） ④質感の合成は **Lab 色空間**で行い、**sRGB 色域外は明度と色相を保ちながら彩度だけを必要最小限圧縮**する（flatLeatherTone・§5-6） ⑤黒い元写真を前提とする photoLeatherTone 方式は flat チャームに適用しない
- **material 別 base 派生方式（改訂14 — 同期計画 §6-2 の二択〔material別base派生／runtime material合成〕から一意選択）**:
  - 派生 base は **material ごとにアセット生成時に焼き込む**（runtime material 合成方式は不採用 — neutral base＋実行時合成は F oracle・検査8の再設計が大きく、「ステージ座標系でピクセル操作をしない」契約と両立しにくいため）
  - **命名規則**: 既定 material（calf）は従来どおり `{key}.base.png`。追加 material は `{key}.{materialKey}.base.png`（例: `horse.chevre.base.png`）
  - **生成対象は「当該チャームの appliesToCharms に含まれる variant が属する material」のみ**: 現行は chevre 3 variant が appliesToCharms=[]（AE-20260718-10）のため、**生成するのは全8種 × calf の 8 base のみ**（ファイル構成は従来と同一）。chevre 適用アイテムが追加された時点で、この命名規則に従い当該 material の base を追加生成する
  - **masks / manual-* / prep.json の派生値は material 間で共有**する（同一 canonicalSource・同一パイプラインから生成され、material 差は ⑦' の grain 入力だけのため、寸法・alpha・所有権は構成的に一致する。共有は寸法・alpha・所有権の完全一致が機械検証で確認された場合だけ許可 — 同期計画 §8-1）
  - prep.json へ `materialAssets`（materialKey ごとの base の期待出力ハッシュ・byteLength・**baseLum**〔material 別派生値 — 下記「派生値の決定規則」〕）・`textureProfileKey`・grain source SHA・生成方式（generationMode="baked-base"）を記録する（§5-4）
- **グレイン焼き込み（パイプライン⑦'・sourceMode="colorless" のみ・二段階・material-aware〔改訂14〕）**:
  - **grain 入力は生成する base の materialKey に対応する MATERIAL_PROFILES（§5-6）の grain source を使う**: calf = リポジトリ直下の `質感.jpg`（1011×676・AE-20260718-14）／chevre = `reference-visuals/colors/grains/chevre-grain.png`（824×383・AE-20260718-13）。以下の式・規則は全 material 共通で、W₀×H₀・SHA だけが profile により異なる
  - **⑦'-1 焼き込み**: 対象は**革主所有画素（masks.α≥128 ∧ masks.R≥128）**のみ。base.RGB を次のニュートラルグレーに置換する:
    `gray = round(clamp(grainBase × shadeEff(x,y) × grain(x,y), 0, 255))`・`base.RGB ← (gray, gray, gray)`
    - **`shadeEff(x,y)`（改訂17 — 暗部下限 `shadeMin`・§0-E-3）**: **厳密 one-hot の革所有画素（`masks.α ≥ 128 ∧ masks.R = 255 ∧ masks.G = 0 ∧ masks.B = 0`）では `shadeEff = max(shade(x,y), shadeMin)`、それ以外の革主所有画素では `shadeEff = shade(x,y)`**（従来式）。**`shadeMin = 0.78`**（採用値・§0-E-3）。この限定により、混合所有の境界画素が生じてもロゴ（G 所有・`metalTone`）とステッチ（B 所有・`stitchColor`）の色が `shadeMin` の影響を受けないことを構造的に保証する（実測では全8種で混合所有画素 0 件 — Phase 3 selftest・Phase 4 検査で「混合所有画素数 = 0」を機械照合する）。`shadeMin` は prep.json（grain 一式）へ記録し、検査F-1 の独立 oracle は本式で期待値を算出する
    - `shade(x,y) = clamp(lum(素材RGB) / flatWhite, 0, 1)` — 素材自体の明暗（縁取り線・陰影）を革色の暗部として保存する係数。`flatWhite` ＝ 革コア画素（base.α=255 ∧ masks.α≥128 ∧ masks.R=255）の lum の**下位中央値（baseLum と同じ 0始まり添字 floor((n−1)/2) 規則）**を⑦'-1 実行直前の画像から算出。**コア画素0件、または flatWhite が正の有限値でない（≤0・NaN・Infinity）場合は生成エラー**（派生アセット不成立 — 0/0 等の NaN が画素バッファへ暗黙変換されて検査をすり抜ける事故を排除）。flatWhite は prep.json（grain 一式）に記録し、検査F-2 が再計算一致を照合する
    - **ミラー添字関数（共通定義 — 比率マップの blur 境界とタイル参照の双方でこの同一関数を使用する）**: `mirrorIndex(k, n) = （t = ((k mod 2n) + 2n) mod 2n として） t < n ? t : 2n − 1 − t`（符号付き整数 k に対して定義 — JavaScript の負剰余を二重 mod で補正。端画素を重複させる対称反転: …2,1,0,0,1,2,…）
    - `grain(x,y)` — **グレイン比率マップ**（適用 profile の grain 入力側で1回生成）を**ミラータイル＋最近傍**で参照した値。離散規則を一意に固定する（決定的 — 検査8の再生成一致の前提）:
      - **⑦'-0 実効 ROI の切り出し（改訂17 新設 — `grainSourceInset`・§0-E-2）**: 比率マップを構成する前に、grain 入力を四辺から `grainSourceInset` px 除去した**実効画像**へ切り出す。`ROI = [inset, inset, W₀-inset, H₀-inset]`（右下端は排他）・実効寸法 `W₁ = W₀ − 2×inset`・`H₁ = H₀ − 2×inset`・実効画素 `(u₁, v₁)` は原画像の `(u₁+inset, v₁+inset)`。**`grainSourceInset = 2`**（採用値）により calf は **1007×672**、chevre は **820×379**。**以降の blur・比率・タイル参照はすべて実効画像 W₁×H₁ に対して行い、原画像の W₀ / H₀ は参照しない**（＝ inset → mirror padding → blur → 比 の順。原画像で blur を先に計算してから切り出すことを禁止する）。実効 ROI の RGBA バイト列の SHA-256（`effectiveRoiSha256`）・`effectiveWidth`・`effectiveHeight`・`grainSourceInset` を prep.json（grain 一式）へ記録し、検査F-2 と検査8 が照合する
      - **比率マップ生成**（**実効画像 W₁×H₁ 上**。改訂17では原画像寸法 W₀ / H₀ は⑦'-0 の切り出しにのみ用い、以降の式には一切現れない）: `g0[i,j]` = 実効画像のグレースケール値（§5-2 輝度式と同一係数・浮動小数のまま保持）→ `blur[i,j] = (Σ_{dj=−R..R} Σ_{di=−R..R} g0[mirrorIndex(i+di, W₁), mirrorIndex(j+dj, H₁)]) / (2R+1)²`（R = grainRadiusPx。**境界は上記 mirrorIndex による対称拡張**・加算は di 昇順→dj 昇順の二重ループの逐次浮動小数加算を正とし、分離型等の最適化は結果が完全一致する場合のみ許容）→ **blur=0 の画素が1つでもあれば生成エラー**（不正な質感画像＝派生アセット不成立・ゼロ除算を排除）→ `ratio0[i,j] = g0[i,j] / blur[i,j]` → `grainMap[i,j] = clamp(ratio0, 1−grainAmp, 1+grainAmp)`（＝局所凹凸のみ。大域的な明暗勾配と色を機械的に除去 —「色を捨て局所的な凹凸だけ」の実装定義）
      - **参照**（チャーム作業座標の画素 (x,y)・軸別尺度）: `u = floor((x + 0.5) / grainScaleX)`・`v = floor((y + 0.5) / grainScaleY)`（**最近傍・補間なし**）→ `i = mirrorIndex(u, W₁)`・`j = mirrorIndex(v, H₁)` → `grain(x,y) = grainMap[i,j]`。**タイル原点は実効画像の原点 (0,0) 固定**（＝原画像の (grainSourceInset, grainSourceInset)。改訂17・§0-E-2）
      - 浮動小数の途中丸めは行わず、8bit 量子化は ⑦'-1 の最終 `round(clamp(...))` の1回のみ
    - **タイルの拡縮率（軸別 — 非等方投影の素材でも両軸とも実寸 mm 基準になる）**: `grainScaleX = grainScaleMm × pxPerMmWorkW`・`grainScaleY = grainScaleMm × pxPerMmWorkH`。**`pxPerMmWorkW / pxPerMmWorkH`（グレイン尺度の基準・作業座標系）**＝ **⑦' 開始時点（⑦完了後）の画像**に対し calibration 規則（§5-2 — bbox は bboxPx.w / bboxPx.h・manual-line は幅／高さ計測線）で幅・高さ計測値を算出し `幅計測値 ÷ sizeMm.w`／`高さ計測値 ÷ sizeMm.h`（いずれも prep.json に記録）。**⑧が最終画像から再算出する正本 pxPerMm とは別値**であり、正本側の決定規則・転記契約は不変（⑦' が⑧の値を参照する循環を排除）
    - **`grainScaleMm`（grain 入力1pxに対応させる mm）は全チャーム・全 material で共通の値**として Phase 4 で目視確定する — 軸別尺度により**⑦' 時点で全チャームの両軸ともシボ実寸スケールが揃う**。⑧' アスペクト補正は実測寸法を正とする矯正（§5-2）のため、適用チャームでは**補正後にグレイン尺度もほぼ等方へ収束する**（適用軸の尺度〔W軸適用なら pxPerMmWorkW・H軸適用なら pxPerMmWorkH〕× effectiveScale が非適用軸側の尺度へ近づく — 残留乖離は離散丸めと計測差の程度）。事前補償は行わず、**§7-8 の24色回帰目視の合格を許容条件とする**（グレインは表示品質の素材であり幾何契約〔寸法・配置・§9-2〕の対象外）
    - パラメータ（grainBase・grainAmp・grainRadiusPx・grainScaleMm）はフェーズ0で目視確定し prep.json に記録（初期値の目安: grainBase=200・grainAmp=0.25・grainRadiusPx=24）。**制約 `grainBase × (1 + grainAmp) ≤ 255`**（クリップによる質感の潰れ防止 — 検査F-2）
    - 焼き込み結果の革主所有画素は**構成的に R=G=B（無彩色グレー）**となり、素材の革元色は混入しない（検査F-1 で機械確認）
  - **⑦'-2 再defringe**: ⑦'-1 の出力に対し、base.α 基準で α<255 の画素の RGB を**④と共通の最近傍選択規則・同一実装〔同一関数〕**（§5-2 base.png 契約・⑧'-2 と同じ）で再置換する（焼き込みがエッジ帯と近傍 α=255 画素の間に輝度差を作り検査4を破ることを防ぐ — ⑧'-2 と同じ構造の再確立）。base.α・masks は変更しない（RGB のみ）
  - ⑦' の完了後に⑧派生値算出へ進む（baseLum・bboxPx 等は焼き込み後の画像から既存決定規則のまま算出）。アスペクト補正⑧'（shouldApply 成立時）は焼き込み後画像に対して従来契約どおり適用される
- **表示革色 DISPLAY_VARIANTS（approvedHex 正本）・実行時トーン flatLeatherTone**: 決定規則・正本・照合は §5-6 に規定（旧 referenceIvory / DISPLAY_COLORS 契約は改訂14で撤回 — §0-B-4）
- **prep.json への記録（schemaVersion 6・§5-4）**: sourceMode・**materialAssets（materialKey ごとの base 期待出力ハッシュ・byteLength・textureProfileKey・generationMode）**・grain 一式（**material ごとに** file・sha256・grainBase・grainAmp・grainRadiusPx・grainScaleMm・**grainScaleX / grainScaleY・pxPerMmWorkW / pxPerMmWorkH・flatWhite**・**改訂17 追加: grainSourceInset・effectiveWidth・effectiveHeight・effectiveRoiSha256・shadeMin**〔§0-E-2・§0-E-3〕）

**派生値の決定規則（準備ツールが自動算出。実装者裁量を残さない）**

- 輝度式: `lum = (0.2126·R + 0.7152·G + 0.0722·B) / 255`（既存 `index.html` と同一係数）
- **baseLum（改訂14: material 別派生値）**: **対象 material の base（⑦' で当該 material の grain を焼き込んだ後の最終画像）ごとに**、コア画素（`base.α = 255` ∧ `masks.α ≥ 128` ∧ `masks.R = 255`）の lum を昇順ソートし、0始まり添字 **`floor((n-1)/2)`** 番目の値（下位中央値。例: n=1→添字0、n=2→添字0、n=3→添字1、n=4→添字1）。**コア画素が0件なら書き出しエラー**（そのチャームは派生アセット不成立＝Phase 4 不合格）。**material ごとの値は prep.json の materialAssets[materialKey].baseLum へ記録**する（grain 入力が material で異なるため焼き込み後の値も material で異なり得る）。**CHARMS 転記10項目の baseLum = 既定 material（calf）の baseLum**（現行の生成対象は calf のみのため唯一の値と一致）。実行時は選択 variant の materialKey に対応する materialAssets の baseLum を base とともに捕捉する（§5-6・キャッシュキーに variantKey/materialKey が含まれるため取り違えは構成的に起きない）。非 calf material の適用アイテムが追加された時点で、`?debug=1` の照合は materialAssets の全 material 分の baseLum を対象に拡張する
- **bboxPx**（革本体の外接矩形）: `masks.α ≥ 128` ∧ `masks.R ≥ 128` の画素集合の最小外接矩形（保持画素は含めない — 共通述語）
- **fgBboxPx**（前景全体の外接矩形。素材に吊り金具が写る場合は金具込み — 今回の8種は吊り金具なし〔§4-1c〕のため bboxPx とほぼ一致する見込み）: `base.α > 0` の画素集合の最小外接矩形
- **実寸校正 `calibration`（表示用bboxと校正座標系の分離）**: 公称 sizeMm に対応する写真上の計測は校正モードで行う:
  - `mode: "bbox"`（正立素材の既定。horse / horseshoe / frenchie / dachshund / toypoodle / cat）: 幅計測値 = `bboxPx.w`、高さ計測値 = `bboxPx.h`
  - `mode: "manual-line"`（**斜め素材用。osanpo は必須**）: 準備ツール上で**幅計測線・高さ計測線**（各2点・base.png px座標。実物の W=92mm / H=40mm に対応する区間を物体のローカル軸に沿って指定）を手動指定し、線分長（ユークリッド距離）を計測値とする。**非正方形の物体が写真内で回転していると軸平行bboxのW/Hは公称W/Hと乖離するため、bboxモードでは撮影歪みがなくても歪み検査に落ちるか誤校正になる**（レビュー指摘・2026-07-12）
  - calibration（mode・計測線座標・計測値）の**正本は prep.json のみ**（CHARMS へは転記しない — CHARMS に入るのは採用値 pxPerMm だけ）。妥当性は書き出し前検査3（判別ゲート）で、再現性はコミット前検査8（再生成一致）で検証する
- **pxPerMm**: `pxPerMmW = 幅計測値 / sizeMm.w`、`pxPerMmH = 高さ計測値 / sizeMm.h`。歪み率 `|pxPerMmW − pxPerMmH| / pxPerMmW ≤ 0.03` を検査。**採用値は pxPerMmW**（丸めず浮動小数のまま記録）
- **アスペクト補正（非等方・縮小のみ。改訂10・2026-07-13）**: 発動述語 `shouldApply` が真のチャームは、**実測寸法を正として写真の縦横比を矯正**してから書き出す（フェーズ0実績では bbox モード5種が該当 — §4-1）:
  - **発動述語（機械判定・裁量なし・補正前値のみから決定）**: `shouldApply = (calibration.mode = "bbox") ∧ (sizeMmMeasured.confirmed = true) ∧ (preDistortion > 0.03)`。ここで preMeasuredW/H（パイプライン⑧〔§5-4〕時点の幅・高さ計測値）・prePxPerMmW/H・preDistortion（補正前歪み率）は**全チャームで prep.json に記録**する（applied=false でも記録 — 検査3が発動述語を補正前値だけから再計算するため）。`sizeMmMeasured`（実測確認状態 confirmed・受領日）は prep.json の**入力値**とする: **全8種 = true〔7種 2026-07-13・swan 2026-07-28 受領 — 改訂16・§0-D-1・§4-1〕**。**confirmed=false のチャームで preDistortion > 0.03 が出た場合は実測受領まで停止**（§7-9 対処①。現行8種は全て confirmed=true のため該当チャームなし — 将来素材用の契約として温存。swan は Phase 4 実測 preDistortion 7.081% > 3% のため本補正の適用対象となった — §0-D-1）
  - **補正式（縮小のみ — §7-4「拡大は不可」の思想と整合）**: `pxPerMmW > pxPerMmH` なら W方向へ `scaleX = pxPerMmH / pxPerMmW`、`pxPerMmW < pxPerMmH` なら H方向へ `scaleY = pxPerMmW / pxPerMmH` を適用する（過剰サンプル軸を他軸へ合わせる縮小であり、アップサンプリングは発生しない。§7-4 の前景長辺 ≥600px は補正後の fgBbox で判定 — **補正適用対象は旧フェーズ0実績の5種〔horseshoe / frenchie / dachshund / toypoodle / cat〕＋改訂16で加わる swan の6種**（§4-1・§0-D-1）。旧5種は縮小軸が長辺と直交または縮小後も603px以上のため維持見込み・**frenchie は cropSource 適用後の前景長辺 ≈1127px で維持（W軸縮小・長辺 H 不変 — §0-D-2）・swan は作業画像の前景 ≈692×741px に H軸縮小 ≈0.934 を適用しても長辺 ≈692px ≥ 600px で維持見込み**（Phase 4 の検査3 で補正後実測値により機械判定）
  - **座標系の命名**: ①〜⑧の中間結果・manual-*.png・stitchPaths 入力・校正計測線の座標系を**作業座標系**、⑧'適用後（書き出される base/masks と bboxPx・fgBboxPx・stitchPaths 転記値等の派生値）の座標系を**アセット座標系**と呼ぶ。applied=false のチャームでは両者は一致する。manual-*.png は**作業座標系の入力**であり③〜⑦で適用済みのため⑧'では変換しない（ファイルも書き換えない。§5-2 ファイル構成・§5-4 手動補正レイヤの「同寸」は作業座標系基準 — 補正適用チャームでは書き出し済み base.png と寸法が異なる）
  - **離散変換の一意定義（決定的・ブラウザ非依存）**: 適用軸の出力寸法は `dst = round(src × scale)`（round は JS `Math.round` — 同値は正の無限大方向へ丸める規則で固定。他軸は `dst = src`）。変換原点は画像原点 (0,0) 固定・余白の追加や切り詰めはしない（キャンバス全体を変換）。**実効スケール = dst / src**（丸め後の実比率）とし、以降の座標変換（stitchPaths 等）はこの実効スケールを用いる。画素対応は `srcIdx = clamp(floor((dstIdx + 0.5) × src / dst), 0, src − 1)`（**この式は base.α と masks の nearest 専用** — base.RGB の bilinear は下記の補間定義を正本とする。中心座標規約 `dstIdx + 0.5` のみ全レイヤ共通）
  - **補間は準備ツール内の自前実装で固定**（canvas `drawImage` の品質ヒントは使わない — アルゴリズムがブラウザ実装依存で、プリマルチプライにより α=0 画素の defringe 済み RGB が失われ得るため）:
    - **base.RGB（sourceMode="photo"）** = α を無視した RGB 平面への適用軸1次元 bilinear。式を固定する: `u = (dstIdx + 0.5) × src / dst − 0.5`、`t = u − floor(u)`、`i0 = clamp(floor(u), 0, src − 1)`、`i1 = clamp(floor(u) + 1, 0, src − 1)`、チャンネル値 `v = (1 − t) × V[i0] + t × V[i1]` を `Math.round` で 8bit 量子化し [0, 255] へ clamp する（端点は i0 = i1 となるエッジクランプ）
    - **base.RGB（sourceMode="colorless"・改訂13）** = **base.α・masks と同一の中心規約の nearest**（下記 `srcIdx` 式）。bilinear は α 境界や所有境界を跨いで異色画素（ロゴ・保持等）の RGB を革主所有の α=255 画素へ混入させ、⑦' が確立した無彩色性（R=G=B・検査F-1(b)）と flatLeatherTone の LUT 完全一致（§5-6）を破るため、colorless では RGB も nearest とする（photo チャームの bilinear 契約は不変。nearest による硬さはグレインのランダムテクスチャ上では知覚困難 — §7-8 目視で確認）
    - **base.α と masks の全チャンネル** = 上記中心規約の nearest（`srcIdx` 式は nearest 専用 — bilinear の式とは区別する）。α と masks が同一画素ソースから移送されるため、**所有権恒等式3条件・保持αの二値性が構成的に保たれる**（補間で base.α>0 と masks の対応がずれて検査1に落ちる事故を排除）
  - **⑧' は二段階で構成する（改訂12・2026-07-14）**: **⑧'-1 暫定変換** = 上記の sourceMode 分岐の補間（base.RGB: photo=bilinear・colorless=nearest〔改訂13〕）／nearest（base.α・masks — 全 sourceMode 共通）による幾何変換 → **⑧'-2 再defringe** = ⑧'-1 の出力（暫定画像）に対し、**補正後の base.α を基準に、α<255 の画素の RGB を「同画像内の最近傍 α=255 画素の暫定RGB」で置換**する。最近傍の選択は**④と共通の最近傍選択規則（§5-2 base.png 契約 — 二乗ユークリッド距離最小・同距離は線形インデックス `y×width+x` 最小・α=255 が0件なら生成エラー）**に従い、④の defringe と**同一規則・同一実装〔同一関数〕**をアセット座標系で再適用する（規則の二重実装を禁止 — ④と検査4の整合を補正後画像へそのまま引き継ぐ。α=0 の画素も④と同様に置換対象）。**置換ソースの RGB は ⑧'-1 完了時点の暫定画像の値**とする（ソースは α=255 画素のみで、⑧'-2 は α=255 画素を変更しないため、スナップショット参照と in-place 実装は結果同値）。base.α・masks は ⑧'-2 で変更しない（RGB のみ書き換え）ため、⑧'-1 が構成的に保った所有権恒等式3条件・保持αの二値性はそのまま維持される。⑧' 全体が shouldApply 成立時のみ実施される点は不変 — applied=false のチャームは ⑧'-1・⑧'-2 とも実施せず、**⑧直後の画像（④ defringe・⑦ manual-inpaint 反映済み）を無変換のまま最終画像とする**
  - **⑧'-1 補間値と再defringe の優先関係（sourceMode 分岐で統一 — 改訂13）**: 上記の補間式（photo=bilinear・colorless=nearest）は**⑧'-1 の暫定値の定義**である。最終 base.RGB は、**α=255 の画素 = ⑧'-1 の補間値（photo は bilinear 値・colorless は nearest 値。⑧'-2 は変更しない）・α<255 の画素 = ⑧'-2 の再defringe 値（⑧'-1 の暫定値を上書き）**。根拠（photo で bilinear 後の再defringe が必要な理由）: bilinear は α 境界を跨いで不透明画素とエッジ・透明画素の RGB を混ぜるため、④で成立していた defringe 性が補正後画像で破れ、検査4（エッジ帯 d>32 率 0.5% 未満）に不合格となる — フェーズ0実績（2026-07-14・改訂10/11 取り込み後の生成報告）: 補正適用5種が horseshoe 9.43% / frenchie 0.726% / dachshund 19.23% / toy poodle 10.86% / cat 14.13% で不合格（検査1・2・3・5・6・7 は全7種合格）。最近傍算出の厳密化〔2D ユークリッド化〕では解消しないことも確認済みのため、再defringe を契約とする（colorless の nearest でも α<255 画素の暫定値は元画像のエッジ実色のままであり、⑧'-2 の再適用契約は全 sourceMode 共通）
  - **⑧' 内の処理順**: ⑧'-2（再defringe）の完了後に派生値の再算出（下記）→⑨検査1〜7 を実行する。検査4 を含む⑨の全検査の対象は ⑧'-2 適用後の**最終画像**（検査4 の 0.5% 基準は補正の有無によらず不変 — 改訂12 で変更しない）
  - **派生値の再算出**: ⑧' 二段階の完了後に bboxPx・fgBboxPx・calibration 計測値・pxPerMm・baseLum・keepPxCount を**最終画像から全て再算出**する（補正前の値は採用しない。bboxPx・fgBboxPx・baseLum の参照画素は base.α・masks・α=255 コア画素のみで決まるため ⑧'-2 の影響は受けないが、算出時点は ⑧'-2 後に統一する）。歪み率も再計測し、検査3は補正後値で判定する（離散丸めにより概ね 0.5% 未満へ収束する見込み）
  - **manual-line 素材（osanpo）は対象外**（mode 条件により shouldApply から機械的に除外）: 軸平行の縮小では回転した物体ローカル軸の比率を矯正できない。osanpo の校正線計測で実測確定済みかつ preDistortion > 0.03 が判明した場合は、**補正を適用せず（applied=false のまま）検査3不合格としてフェーズ0を停止**し、個別に方式を設計して本計画書を改訂する（§7-9 対処③）
  - **stitchPaths の座標変換と格納先（fallback 採用チャームが補正対象になった場合）**: points は作業座標系で入力し（prep.json の入力値 **`stitchPathsWork`**）、⑧'で適用軸の座標へ実効スケールを乗算した**アセット座標系の値を派生値 `stitchPathsAsset` として prep.json に記録し、CHARMS へは `stitchPathsAsset` の値のみ転記**する（転記照合10項目の stitchPaths = stitchPathsAsset）。**再生成は stitchPathsWork を入力とし、検査5・CHARMS 転記・`?debug=1` 照合は stitchPathsAsset を参照**する。applied=false のチャームは両者が完全一致すること（検査3で機械検証）
  - **記録**: prep.json に `aspectCorrection: { applied, axis（"W"|"H"|null）, scale, effectiveScale（= dst/src）, preMeasuredW, preMeasuredH, prePxPerMmW, prePxPerMmH, preDistortion }` を**全チャームで記録**する（未適用は applied=false・axis=null・scale=effectiveScale=1。pre系は常に記録）。**CHARMS へは転記しない**（calibration と同じ prep.json 内部値 — 転記照合10項目の対象外）
  - **根拠と品質**: 実測寸法（§4-1・**実測確定済みの全8種**〔7種 2026-07-13・swan 2026-07-28 — §0-D-1〕がストア記載と完全一致の二重裏付け）を正とする矯正のため、実寸比率の表示精度（§9-2）はむしろ向上する。テクスチャの伸縮は最大10%（toy poodle）で、比較対象が並ばない単体チャーム表示では知覚困難 — §7-8・§9-3 の目視検査で確認する
- bboxPx / fgBboxPx はあくまで**表示・配置・クリップ検査用の軸平行矩形**。斜め素材では `bboxPx ÷ pxPerMm` が公称W/Hと一致しないのは正常（例: osanpo は回転のため bbox が 92×40mm より正方形に近くなる。§9-2 の比率検証も校正線基準で行う）
- **保持画素数 keepPxCount（派生値）**: `masks.α < 128` の画素数。準備ツールが自動算出し prep.json に記録する（保持を使う見込みは horseshoe のラインストーン6個＋台座と、**swan のくちばし＋ストーンの2集合**〔改訂17 で確定・§0-E-6。改訂13〜14 の「金具鋲等」という暫定表現を置換する〕。**`keepPxCount` はアセット座標（⑧' 後の最終画像）の値であり、改訂17 で新設した作業座標の `keepPxCountWork` とは別値である**（swan はアスペクト補正の適用対象のため一致しない。範囲照合は `keepPxCountWork` 側でのみ行う — §0-E-6）。意図しない保持画素の混入は §7-8 の目視と検査6で確認）
- ※接続アンカー（attachAnchorPx）は、連結表現が「非接続・独立表示」で確定（§11-8・2026-07-13）したため**定義しない**（将来接続表現を導入する場合に §5-5 の検討経緯とともに再設計する）

**stitchPaths の契約（stitchMode=fallback のチャームのみ必須。※現時点で該当チャームなし — 予備契約として温存）**

```js
stitchPaths: [
  { points: [[x,y], …],      // 折れ線（相異なる2点以上）。入力は stitchPathsWork（作業座標系）・CHARMS 転記は stitchPathsAsset（§5-2 アスペクト補正）
    widthMm: 0.8,            // 線幅（mm指定・正の有限値）
    dashMm: 1.8, gapMm: 1.2  // 破線パターン（mm指定・正の有限値）
  }, …
]
```

- ステッチ有チャームの detect 成立見込み: **全8種が colorless（白ステッチ×白革面）のため自動検出は不成立見込みだが、manual-owner の手動指定によるマスク化で detect が成立する**（§5-6 — detect の条件はマスクの由来を問わない。39fe815 の horseshoe 実装では seed ベースの決定的レシピで成立済み）。**osanpo は Phase 4 バッチ実測で自動検出の不成立（B所有 0px）が確定したため、Phase 3 で osanpo 専用の seed ベース決定的レシピ（horseshoe 方式に準拠）をツールへ追加し、manual-owner の機械生成で detect を成立させる（§0-D-3 — detect 契約・検査5 の条件式は不変）**。swan は Phase 4 判定（§4-1c。Phase 4 バッチ実測では detect 成立 — B所有 1,106px・白黒差分 1,106px。最終確定は新 toolCommit での Phase 4 再実行時）。fallback は Phase 4 でマスク化（自動・手動とも）が不成立だった場合の予備手段としてのみ採用する（採用時は本契約と下記検査が全面適用）
- **描画座標系**: stitchPaths は**アセット座標系**（⑧'適用後の書き出し済み base.png の px・§5-2）の転記値のまま完成ビットマップへ描画し、線幅・dash・gap は `mm値 × pxPerMm` で**アセットpxに換算**する（ステージ倍率はビットマップ全体の拡縮で一括適用・上記合成方針と一致）
- fallback チャームでは、準備工程で元写真の糸画素を①**革所有（R）としてマスク化**し、さらに②**base.RGB を周辺革色で中和**する（手動インペイント。manual-inpaint レイヤに保存）。①だけでは photoLeatherTone が元画素の明暗を保持するため、選択革色の中に元糸が筋として残る。**白・黒どちらを選んでも元糸の残像が見えないこと**をフェーズ0の必須検査とする（§7-7）
- 実行時の描画順: 革・ロゴリカラー（保持画素はそのまま） → stitchPaths を選択ステッチ色で上描き（**革所有領域〔`masks.α≥128` ∧ `R≥128`〕でクリップ** — 共通述語。保持画素の上には描かない）
- 検査（§5-4 書き出し前検査5。**完全列挙型ゲート — 3モードの排他性を機械保証し、下記いずれの条件セットにも合致しない組み合わせ・未知の stitchMode 値は即不合格**。**各条件セットは per-charm の `stitchMode` 値そのものに対する述語であり、下記括弧内のチャーム名は現行の割当見込み〔swan は Phase 4 判定・§4-1c〕の例示にすぎない — 検査の分岐は Phase 4 で確定した `CHARM_SPECS[key].stitchMode` の値で行う（§0-E-11 の Phase 6 完了条件と同じ動的判定）**。「所有画素数」の集計は全て共通述語 `masks.α ≥ 128` 込み）:
  - `detect`（見込み: horseshoe / osanpo）: stitchPaths 空 ∧ `masks.B` 所有画素数 > 0 ∧ 白黒切替の出力差分画素数 > 0 ∧ threadColor ∈ {"dark","light"}
  - `none`（見込み: horse / frenchie / dachshund / toypoodle / cat）: stitchPaths 空 ∧ **`masks.B` 所有画素数 = 0** ∧ **白黒切替の出力差分画素数 = 0**（ステッチトグルが出力に影響しないことを機械保証） ∧ threadColor = null
  - `fallback`（採用時のみ）: stitchPaths（検査の参照先は **stitchPathsAsset**・§5-2 アスペクト補正）非空・全パス点が革所有領域（共通述語込み・アセット座標系で判定）内・各パスが相異なる2点以上・幅/dash/gap が正の有限値・総延長 > 0・クリップ後の描画差分画素 > 0 ∧ **`masks.B` 所有画素数 = 0**（元糸は革所有へ移す契約のため。マスク由来とパス由来の糸の混在を禁止） ∧ **manual-inpaint.png の参照必須（置換画素数 ≥ 1）** ∧ threadColor ∈ {"dark","light"} ∧ 白黒切替の出力差分画素数 > 0

**チャーム定義テーブル（フェーズ0の成果として全値を確定）**

```js
const CHARMS = {
  horse: {
    label: "horse",              // UI表示名 = 画像ファイル名と同じ英語表記（§4-1・§11-11 確定）
    base:  "assets/charms/horse.base.png",
    masks: "assets/charms/horse.masks.png",
    sizeMm:   { w: 95, h: 70 },   // 革本体のみ。ストア商品ページ記載で確定（§4-1）
    bboxPx:   { x, y, w, h },     // 革本体の外接矩形（決定規則は上記）
    fgBboxPx: { x, y, w, h },     // 前景全体の外接矩形（配置とクリップ検査に使用）
    pxPerMm:  6.32,               // 決定規則は上記（幅基準・calibration 由来）
    threadColor: null,            // 生成入力の糸色: "dark" | "light" | null（stitchMode=none は null）
    stitchMode:  "none",          // "detect" | "none" | "fallback"（§5-6。実物仕様に基づきフェーズ0で確定）
    stitchPaths: [],              // fallback時のみ非空（契約は上記）
    attachmentMode: "none",       // 素材は全種吊り金具なし＝全8種 "none" の見込み（丸カンは非接続の独立表示・§5-5）。"photo" は将来の金具付き素材用に温存
    toneMode: "flat",             // 改訂14: 全8種 "flat"（無着色素材モード・§5-2・§5-6。"photo" は将来素材用の予備値）
    baseLum:  0.78,               // 決定規則は上記（colorless は白面基準のグレイン焼き込み後のため高い値になる見込み）
  },
  // … 8種
};
```

- calibration（校正モード・計測線・計測値）は **prep.json 側にのみ保存**し、CHARMS へは採用値 pxPerMm を転記する
- **CHARMS 転記照合のフィールド一覧（`?debug=1` が prep.json と機械照合する完全列挙。§9-0）**: `sizeMm`・`bboxPx`・`fgBboxPx`・`pxPerMm`・`attachmentMode`・`threadColor`・`stitchMode`・`stitchPaths`・`baseLum`・**`toneMode`**（この**10フィールド**以外は照合対象外。calibration・keepPxCount・aspectCorrection・sourceMode・grain 系・materialAssets は prep.json 内部値のため対象外 — 検査3・6・8・Fで検証）。**加えて `?debug=1` は LEATHER_VARIANTS（27件）の転記8項目と MATERIAL_PROFILES（2件）の転記6項目を color freeze の値と機械照合する**（§0-A・§5-6）

- **stitchMode の見込み値（Phase 4 で確定）**: horseshoe / osanpo = `detect`（threadColor: "light"）、horse / frenchie / dachshund / toypoodle / cat の5種 = `none`（threadColor: null）。**swan は既存構成を流用せず、canonicalSource の実画素から §5-2 の完全列挙条件（detect / none / fallback）で判定する**（§4-1c。素材に白ステッチが存在するため detect 見込みだが仮確定しない）
- 素材は全種吊り金具なしのため **attachmentMode は全8種 "none"** の見込み（丸カンは非接続の独立表示として全チャーム一律に扱う・§5-5。swan の stone（旧「金具鋲」— §0-E-6 で `swanKeep` へ確定）は吊り金具ではなく装飾であり、保持領域として §4-1c で分類する）
- **fgBboxPx ≒ bboxPx となる見込み**（吊り金具が写っていないため）だが、配置式の軸分離（§5-5）・検査の定義は維持する（将来の金具付き写真にもそのまま耐える）
- 将来のチャーム追加は「素材1枚（写真または無着色素材）→ 準備ツールで派生アセット生成 → テーブルに1エントリ追加」で完結する（無着色素材モードは共通モードのため追加チャームにもそのまま適用可・§5-2）

### 5-3. 流用する設計資産と適用条件

| 資産 | 出典 | 適用条件・読み替え |
|---|---|---|
| 二層座標契約（BASE_DIMS / WORK_DIMS） | ミニバッグ計画書（Codexレビュー済み） | 派生アセットを長辺≤1200pxに正規化する時点で WORK 相当に統一。実行時は「アセットpx座標」と「ステージmm座標」の2層（§5-5） |
| 所有権方式の優先合成マスク | 同上 | 準備工程に移動。恒等式は §5-2 のα条件付きに読み替え |
| `photoLeatherTone()`（ハイライト保護 lum≥0.94・上限0.38） | ミニバッグ Opus 実装（`ミニバッグ実装テスト-opus` b29274f） | **参照実装は4引数で第4引数 `shadeFactor` が必須**（省略すると `ratio *= undefined` で NaN）。チャームの関数契約は **`photoLeatherTone(base, target, baseLum, 1.0)` の4引数・shadeFactor=1.0 固定**とする。係数（ハイライト保護等）は実装指示書で b29274f の値にピン留めし、出力RGBが有限値であることを単体テストで確認する。**改訂14: 適用チャームは0（全8種 flat）— 将来の photo 素材用の予備契約として温存**（flatLeatherTone・§5-6） |
| 質感画像 `質感.jpg`（main 管理下 bbc1b70・リポジトリ直下） | 既存アセット | **calf** のグレイン入力（**色は捨て局所的な凹凸のみ**使用・§5-2・AE-20260718-14）。**そのまま canonical**（コピー・リネームしない — 8b3bdb4 のツリーで `git show` 同定可能・SHA は §4-1c マニフェスト）。**chevre のグレイン入力は chevre-grain.png**（Phase 1 生成済み・AE-20260718-13・§4-1c） |
| `metalTone()`（銀/金） | 既存 `index.html` | そのまま流用 |
| `extractKeyRingAsset()` | 既存 `index.html` | **準備工程で丸カン透過PNG（ring-silver/gold.png）の生成に1回だけ使用**し、42mm外径の共通基準枠へ正規化して書き出す。**実行時の背景除去は行わない**（既存 index.html の実行時抽出は流用しない）。チャーム写真側の背景除去初期実装としても流用し、defringe を追加 |
| 内部ステッチ検出（輝度＋近傍＋enclosed判定） | 既存 `index.html`（ドーナツ実績） | 準備ツールのステッチマスク自動候補生成に流用（糸色極性 threadColor に応じた輝度方向の切替機構自体は温存 — 旧 photo 素材時代の自動検出の主対象は osanpo〔白糸×濃色革・高輝度側を検出〕だった。**無着色素材では horseshoe / osanpo とも白×白で自動検出不成立 — osanpo は Phase 4 バッチで B所有 0px を実測確定〔§0-D-3〕。いずれも seed ベース決定的レシピによる manual-owner 生成で detect 成立・§5-6**）。実行時には使わない |
| `?debug=1` マスク可視化・グリッド・恒等式検査 | ミニバッグ Opus 実装 | charms.html に移植（§9 検証で使用） |
| シルバー/ゴールドキーリング.jpg | 既存アセット | 丸カン（外径42mm）表示。**42mm はストア swan 商品ページの「リング部分:外径42mm、内径32mm」記載で一次ソース確認済み（2026-07-12）**。銀金の2アセットは**同一の42mm基準枠に正規化**し、切替で位置がずれないことを受け入れ基準に含める（§9） |

### 5-4. 準備ツール `tools/charm-prep.html`

- ブラウザだけで動く自己完結HTML（`python3 -m http.server` 経由で使用。file:// は canvas 汚染のため不可 — ミニバッグ検証と同じ運用）

**決定的パイプライン（この順で毎回再実行できることが契約）**

```
生成入力（canonicalSource・§4-1c） → ①回転・クロップ・縮小（長辺≤1200px） → ②自動背景マット（しきい値） → ③manual-alpha適用
     → ④defringe → **⑤-0 keepMask 確定〔改訂17 新設・§0-E-6/§0-E-8〕** → ⑤自動所有権候補（革/ロゴ/ステッチ。**keep 画素を除外して算出**）
     → ⑥manual-owner適用 → ⑥'manual-keep適用（保持強制。⑤-0 の keepMask と同一集合であることを検査）
     → ⑦manual-inpaint適用（base.RGB置換） → ⑦'グレイン焼き込み（sourceMode="colorless" のみ・§5-2。二段階: ⑦'-1 革主所有画素をニュートラルグレー〔`gray = round(clamp(grainBase × shadeEff × grain, 0, 255))`・`shadeEff` は厳密 one-hot 革所有画素なら `max(shade, shadeMin)`・それ以外は `shade`〕へ置換 → ⑦'-2 再defringe〔④と同一規則・同一実装〕）
     → ⑧派生値算出（§5-2決定規則） → ⑧'アスペクト補正（shouldApply 成立時のみ・§5-2。二段階: ⑧'-1 幾何変換〔base.RGB: photo=bilinear・colorless=nearest／α・masks=nearest（共通）〕 → ⑧'-2 補正後α基準の再defringe〔④と同一規則〕→派生値を最終画像から再算出） → ⑨自動検査 → ⑩書き出し
```


**⑤-0 keepMask の確定（改訂17 新設 — 工程順の一意化）**: 改訂16 までは manual-keep の適用（⑥'）が自動所有権候補の算出（⑤）より**後**だったため、`detectLogo` が保持装飾（horseshoe のクリスタル・swan のストーン）を基準色サンプリングや候補判定に取り込み得た。改訂17 では **keepMask を ⑤ より前に確定**し、⑤ の全候補生成（革・ロゴ・ステッチ）から keep 画素を除外する。

```
⑤-0 keepMask = manual-keep.png の α ≥ 128 の画素
              ∪ チャーム別の keep 判定規則で得た画素
                （swan: BEAK_OUTLINE_REMOVAL の beak ＋ stoneRoi のストーン — §0-E-6・§0-E-8。
                  他チャームは現時点では規則なし＝manual-keep.png のみ）
⑤   自動所有権候補は keepMask を除外して算出する:
     - detectLogo の基準色（median）サンプリング母集団から keep 画素を除外する
     - detectLogo の候補判定から keep 画素を除外する
     - 革・ステッチの候補判定からも keep 画素を除外する
⑥'  manual-keep 適用の結果が ⑤-0 の keepMask と画素単位で完全一致することを検査する
     （不一致は生成エラー。⑥ の manual-owner が keep 画素を所有化していないことの機械保証）
```

**この工程順の変更は生成規則の変更であり、`schemaVersion` 5 → 6 の増分理由の一つである**（§0-E-9）。

**①のクロップ（cropSource 契約 — 改訂16・§0-D-2）**: ①のクロップは CHARM_SPECS の `cropSource`（canonicalSource 座標系の固定矩形定数 {x, y, w, h}）で指定する。未指定のチャームは全面（クロップなし）が既定。**改訂16では frenchie のみ `cropSource = {x:320, y:848, w:1105, h:1312}` を指定**する（canonicalSource 1808×2664px の前景 bbox〔x 361–1384・y 888–2119・1024×1232px・2026-07-28 実測〕を余白マージン ≥40px で完全包含する矩形。白余白を除去してから長辺≤1200px 縮小を適用することで、**拡大処理なしに** §7-4〔背景除去後の前景長辺 ≥600px〕を満たす — 機械検算は §0-D-2）。クロップ値は「①の回転・クロップ・縮小値」として prep.json に記録し（既存契約・本節下記）、検査8 の再生成一致で再現性を機械検証する（③再生成照合は CHARM_SPECS.cropSource を crop 入力として①から独立再実行するため、prep.json 記録値と CHARM_SPECS 定数の一致は構成的に検証される）。**Phase 3 の selftest には「CHARM_SPECS.frenchie.cropSource = 契約値 {x:320, y:848, w:1105, h:1312} の完全一致」検証を含める**（§0-D-2）。クロップが前景を見切った場合は §7-1（見切れなし）で不合格となる（マージンはこの事故の防波堤）。**§7-4 の判定基準は不変であり、クロップは検査免除ではない**（クロップ後も前景長辺 <600px なら §7-4 のとおり再撮影・再素材依頼）

**手動補正4レイヤ（いずれも作業座標系＝補正前の base と同寸のPNG〔§5-2 — アスペクト補正適用チャームでは書き出し済み base.png と寸法が異なる〕。α=0は「無指定」、α=255は「指定あり」の画素）**

| レイヤ | 意味 | 適用 |
|---|---|---|
| manual-alpha.png | R値＝強制する base.α（0–255） | ③で自動マットを画素単位に上書き |
| manual-owner.png | RGB＝強制する所有度（合計255） | ⑥で自動候補を画素単位に上書き |
| manual-keep.png | 指定画素を**保持**（masks.α=0）に強制 | ⑥'で適用（ラインストーン等。所有指定より優先） |
| manual-inpaint.png | RGB＝置換色 | ⑦で base.RGB を置換（fallback元糸の中和等） |

**自動検査（二段階。循環しない構成）**

【書き出し前検査（⑨。1〜7・M — sourceMode="colorless" のチャームは検査F込み — の全通過が⑩書き出しの条件。結果は prep.json に記録）】

1. 所有権恒等式（§5-2 の3条件: base.α>0 ∧ masks.α≥128 ⇒ R+G+B=255 ／ base.α>0 ∧ masks.α<128 ⇒ 保持〔RGB不問〕 ／ base.α=0 ⇒ masks.α=255 ∧ R=G=B=0。違反画素数0）
2. baseLum コア画素 ≥ 1件
3. **実寸校正（判別型ゲート — calibration の妥当性と歪み率を両方検証）**:
   - `calibration.mode ∈ {"bbox", "manual-line"}`（それ以外は即不合格）
   - **斜め素材（現状 osanpo）は `manual-line` でなければ不合格**（対象チャームは §5-2 で契約・将来追加時は素材検証で指定）
   - `manual-line` 時: 幅計測線・高さ計測線がそれぞれ**画像内の相異なる有限2点**で、記録された計測値が線分長の再計算値と一致する正の有限値
   - `bbox` 時: 計測値が `bboxPx.w` / `bboxPx.h` と一致する正の有限値
   - そのうえで pxPerMm 歪み率 ≤ 3%（§5-2 決定規則の式。**アスペクト補正〔§5-2〕適用チャームは補正後の再計測値で判定** — manual-line 素材が実測確定済みで補正前歪み率>3%の場合は補正が適用されないため本項で不合格となり、フェーズ0を停止して個別設計へ送る・§7-9 対処③）
   - **アスペクト補正の整合（判定は補正前値のみから再計算 — 補正後値と混同しない）**: prep.json の preMeasuredW/H・prePxPerMmW/H・preDistortion が sizeMm からの相互再計算で一致し、`aspectCorrection.applied` が発動述語 `shouldApply`（§5-2 — calibration.mode="bbox" ∧ sizeMmMeasured.confirmed ∧ preDistortion>0.03）の再評価と一致すること。適用時は axis が過剰サンプル軸・scale が prePxPerMmW/H からの再計算値・effectiveScale が dst/src と一致すること（規則外の軸・値は即不合格）。stitchPathsAsset は applied=true なら stitchPathsWork へ実効スケールを乗算した再計算値と、applied=false なら stitchPathsWork と完全一致すること（§5-2）。**補正適用チャームは fgBbox 長辺 ≥600px（§7-4）を補正後の値で再判定**する
4. **エッジ帯検査**: **前提条件としてエッジ帯（0 < α < 255）画素数 ≥ 1件**（0件はフェザー契約違反として不合格・分母0を排除）。各エッジ帯画素について、**④・⑧'-2 と共通の最近傍選択規則（§5-2 base.png 契約 — 二乗ユークリッド距離最小・同距離は線形インデックス `y×width+x` 最小）で対応づけた** `α=255` 画素とのRGBユークリッド距離 d を算出し、**d > 32 の画素がエッジ帯全体の 0.5% 未満**（defringe の機械的合否。加えて暗色 #333 / 明色 #eee 背景での目視確認を必須検査として併置 — §7-10）。**アスペクト補正適用チャームは ⑧'-2〔再defringe・§5-2〕適用後の最終画像で判定し、0.5% 基準は補正の有無で変えない**（⑧'-2 の契約により補正後画像にも④と同一の defringe 性が成立する — 改訂12）
5. ステッチ整合性（**完全列挙型**）: stitchMode ごとに §5-2 の条件セットを全て満たすこと（detect: paths空・B>0・差分>0・threadColor有効 ／ none: paths空・B=0・差分=0・threadColor=null ／ fallback: paths有効一式・**B=0**・manual-inpaint参照必須・threadColor有効・差分>0。B の集計は共通述語 `masks.α≥128` 込み・§5-2）。**未知の stitchMode・条件セット不合致は即不合格**
6. **定義値・保持整合検査**:
   - `attachmentMode ∈ {"photo", "none"}`（それ以外は即不合格）。**今回の8種は全て `"none"` でなければ不合格**（§4-1c の確定の機械化。吊り金具付き素材が判明した場合は所有権設計とともに計画書改訂）
   - `keepPxCount`（保持画素数・アセット座標）と **`keepPxCountWork`（作業座標・改訂17 新設・§0-E-6）** がそれぞれ算出値と一致して prep.json に記録されていること。**swan は `keepPxCountWork` が `keepRange` 4,630〜5,658 内であり、かつ keep 集合が `swanKeep` = beak ∪ stone の2成分ちょうど（beak 実測 3,629px・stone 実測 1,515px・重複 0px）であることを機械照合する — 3成分以上、または beak / stone のいずれかを欠く場合は生成エラー**（§0-E-6）。**保持を使う見込みのないチャーム（horse / frenchie / dachshund / toypoodle / osanpo / cat の6種）で keepPxCount > 0 の場合は警告を出し、意図的かどうかを §7-8 の目視・素材検証レポートで確認**（機械不合格にはしない — 将来の保持利用を妨げないため。horseshoe は保持確定〔ラインストーン6個＋台座 — 6ストーン位相検査は下記〕・swan は保持確定〔**`swanKeep` = beak ∪ stone の2成分ちょうど**・§0-E-6〕）
   - **horseshoe 6ストーン位相検査（改訂14 — 39fe815 から還流）**: horseshoe の保持領域（manual-keep 由来）は**作業座標系で 8近傍連結成分がちょうど6個**・**union 面積が 32000〜34000 px**（39fe815 実測レンジ）でなければ不合格。アスペクト補正適用時は、**work 座標の keep bitset を center-nearest 投影（§5-2 の nearest `srcIdx` 式の逆対応）した期待 bitset と最終 masks の保持画素が完全一致**（missing 0・extra 0）し、**期待側・実側の双方が6成分**を保つこと。**asset 座標へ work 座標の固定面積閾値を直接適用してはならない**（補正で面積が変わるため — 位相〔成分数・一致〕で検査する）
7. 転送量: base.png + masks.png 合計 ≤ 900KB

【書き出し前検査M（⑨続き。**全チャーム共通**（sourceMode を問わない）— 1〜7と併せて全通過が⑩の条件。改訂13）】

- **M-1 正規入力マニフェスト照合**: §4-1c の正規入力マニフェスト（CHARMSキー → sourceMode・正規パス・期待SHA-256。準備ツールへ定数として転記し、sourceFreezeCommit の canonical files と一致）に対し、①実入力ファイルの SHA-256、②prep.json の source（file・sha256）と sourceMode、の**すべてが完全一致**すること。**マニフェスト外の入力は即不合格 — 特に supersededSources（旧受領写真 8枚・swan 旧フチ画像2版・背景透過8枚・§4-1c 禁止入力）は、prep.json に自己整合的に記録しても本検査で排除される**（自己申告値どうしの整合だけでは通過できない）。grain は**生成する material に対応する profile の grain 入力**（calf = 質感.jpg／chevre = chevre-grain.png）の SHA-256 を §4-1c マニフェスト記載値と照合する
- **M-2 モード対応（全種）**: `sourceMode="colorless" ⇔ toneMode="flat"`・`sourceMode="photo" ⇔ toneMode="photo"` の1:1対応（§5-2。photo チャームの toneMode 誤転記も本検査で検出する — colorless 専用枠には置かない）

【書き出し前検査F（⑨続き。sourceMode="colorless" のチャーム専用 — 1〜7・Mと併せて全通過が⑩の条件。改訂13）】

- **F-1 焼き込み範囲と元色無混入（三部・改訂17で (c) を追加）**:
  - **(c) 所有権の one-hot 性と `shadeMin` 適用範囲（改訂17 新設・§0-E-3）**: **混合所有画素（`masks.α ≥ 128 ∧ masks.R ≥ 128 ∧ (masks.G > 0 ∨ masks.B > 0)`）の画素数が 0 であること**（実測: 全8種で 0 件。> 0 なら生成エラー）。加えて独立 F oracle は ⑦'-1 の期待画素値を **`gray = round(clamp(grainBase × shadeEff × grain, 0, 255))`・`shadeEff = 厳密 one-hot 革所有画素なら max(shade, shadeMin)・それ以外は shade`**（§5-2）で算出して全画素一致を照合し、**混合境界画素を人工的に注入した mutation 試験で「`shadeMin` がロゴ・ステッチ主所有画素の値を変えない」ことを回帰検査**する（§5-4 の mutation 試験へ1種追加）
  - **(a) ⑦'-1 範囲検査**: ⑦'-1 適用直後、革主所有画素（masks.α≥128 ∧ masks.R≥128）の base.RGB が**全画素 R=G=B（無彩色グレー）**であり、それ以外の画素（ロゴ・ステッチ主所有／保持／base.α=0）の base.RGB が**⑦'-1 の前後で全画素一致**していること（⑦'-2 の再defringe は④と同じ全 α<255 対象のため本比較の対象外。準備ツールは⑦'-1 前後のスナップショット差分を保持し⑨で判定する）
  - **(b) 最終画像の無彩色性**: ⑨時点の最終画像（⑦'-2・⑧' 適用後）で、革主所有 ∧ base.α=255 の画素が**全て R=G=B** であること（⑧'-1 の colorless=nearest 契約〔§5-2 アスペクト補正〕により構成的に成立する — それを機械検査で保証。flatLeatherTone の 256 エントリ LUT 適用範囲〔§5-6〕の前提）
  - **無彩色保証の範囲は「革主所有 ∧ base.α=255」に明示的に限定する**: α<255（エッジ帯）の革所有画素は ⑦'-2／⑧'-2 の再defringe 値＝**最近傍 α=255 画素の実色**（最近傍がロゴ・保持等ならその非グレー値）のままであり、無彩色保証の対象外。これは④と同一のエッジ処理思想（縁は近傍実色で自然に）で、再defringe の最近傍共通規則・検査4 の整合を colorless でも崩さないための設計 — エッジ帯の革所有画素は実行時に flatLeatherTone の lum 式で直接計算され（LUT 対象外・§5-6）、出力αは base.α のためフェザー帯 1〜2px の視覚影響に留まる（§7-8 の24色回帰目視で確認）
  - 素材の**革面の元色**（白面・縁取り線の色）がリカラー入力へ混入しないことの機械保証（§5-2 原則① — 革主所有 α=255 画素はグレーのみ。エッジ帯に残る近傍実色はロゴ・保持等の意匠色であり「革の元色」ではない）
- **F-2 グレイン入力整合**: 適用 profile の grain 入力（calf = 質感.jpg／chevre = chevre-grain.png）の SHA-256 が prep.json 記載値および §4-1c マニフェスト記載値と一致／**grainBase は 1〜255 の整数・grainAmp は 0 < grainAmp ≤ 1 の有限実数・grainRadiusPx は正整数・grainScaleMm は正の有限実数**で、grainScaleMm は**全チャーム・全 material で同一値**／`grainScaleX = grainScaleMm × pxPerMmWorkW`・`grainScaleY = grainScaleMm × pxPerMmWorkH` の**両軸の再計算一致**かつ正の有限値（pxPerMmWorkW / pxPerMmWorkH の再計算一致を含む・§5-2）／**flatWhite の再計算一致かつ正の有限値**（§5-2）／`grainBase × (1 + grainAmp) ≤ 255`／タイル方式が mirror（mirrorIndex 共通定義・§5-2）／**改訂17 追加（§0-E-2・§0-E-3）: `grainSourceInset` が契約値 2 と一致する非負整数／`effectiveWidth = W₀ − 2×grainSourceInset`・`effectiveHeight = H₀ − 2×grainSourceInset` の再計算一致（calf 1007×672・chevre 820×379）かつ正／`effectiveRoiSha256` が実効 ROI の RGBA バイト列から再計算して一致／`shadeMin` が契約値 0.78 と一致する 0 ≤ shadeMin ≤ 1 の有限実数／比率マップとタイル参照が実効寸法 W₁×H₁ を用いていること（原寸 W₀×H₀ を用いた実装は effectiveRoiSha256 と grainMap の不一致で検出される）／革面の行平均および列平均の grain 比の局所偏差（移動中央値 size 25 からの差）の最大値 ≤ 0.15**
- **F-3 material profile 整合（改訂14 — 旧「referenceIvory 整合」を置換）**: prep.json の materialAssets / grain 記録（materialKey・textureProfileKey・grain の file/sha256・generationMode）が MATERIAL_PROFILES の転記6項目＋非転記メタデータ（grain 原本 SHA/ROI・canonical output の path/寸法/SHA・許可 mode pair — §5-6 の転記合成規則）および color freeze の materials（grainPolicy・grainSource の input/output SHA・roi）と完全一致すること。生成した material 別 base ごとに、適用した grain 入力（canonical output ファイル）の SHA・寸法（calf 1011×676／chevre 824×383）の再照合と、当該 material の許可 mode pair（colorless/flat）への sourceMode/toneMode 適合を含む
- **F-4 DISPLAY_VARIANTS 整合（改訂14 — 旧「DISPLAY_COLORS 整合」を置換）**: DISPLAY_VARIANTS（= LEATHER_VARIANTS の approvedHex・27件・§5-6）が color freeze の variants と完全一致すること（lowercase hex 文字列一致・27件の variantKey に欠落/重複なし・appliesToCharms 一致〔calf 24 = 8キー全件・chevre 3 = 空〕）。approvedHex は freeze 済み定数の**転記のみ**で導出計算は存在しない（転記照合は `?debug=1` でも再実施・§5-6。sRGB 色域外の圧縮が発生し得るのは flatLeatherTone 実行時のみで、その検証は §9-13 の境界試験による）

【コミット前検査（8。書き出し済みファイルに対する独立ゲート。合格するまでコミット不可）】

8. **再生成一致（三者照合）**: 準備ツールの再検証モードで、書き出し済みの canonicalSource＋prep.json＋manual-*.png を読み込み、次の3照合すべてに合格すること。①**入力照合**: 生成入力（canonicalSource）・manual-*.png の SHA-256 が prep.json 記載値と一致し、**生成入力・sourceMode は §4-1c の正規入力マニフェストとも一致**すること（検査M-1 と同一の照合 — prep.json の自己申告値どうしの整合だけでは通過できない。**material ごとの grain 入力 SHA〔マニフェスト照合込み〕を含む** — §5-2・§5-6） ②**出力照合**: 書き出し済み base（**material 別 base 全点**）/masks の**最終 PNG 独立デコード**（下記 straight PNG 契約）による RGBA ハッシュが prep.json の期待出力ハッシュと一致 ③**再生成照合**: パイプライン①〜⑧'（**⑦' の二段階〔⑦'-1 グレイン焼き込み＋⑦'-2 再defringe。colorless のみ〕**と⑧' の二段階〔⑧'-1 幾何変換＋⑧'-2 再defringe〕を含む・§5-2）を独立再実行した結果のデコードRGBA・派生定義値（calibration・keepPxCount・aspectCorrection・**materialAssets・grainScaleX / grainScaleY・pxPerMmWorkW / pxPerMmWorkH・flatWhite〔colorless — §5-2 の入力から再計算し記録値と完全一致させる〕**を含む prep.json 記録値一式）が、書き出し済みファイル・prep.json 記載値と完全一致（比較対象は常に「書き出し済み一式」なので初回生成でも循環しない）。合否は prep.json の `verify` フィールドに追記する

【straight RGBA PNG と独立最終デコード（改訂14 — 39fe815 から還流・全書き出し PNG に適用】

- base・masks・manual 派生 PNG は **straight RGBA エンコーダー**（準備ツール内実装・8-bit RGBA・non-interlace）で生成する。**Canvas `toBlob` を base/masks/manual 派生 PNG の正規エンコーダーに使わない**（プリマルチプライ往復で α<255 画素の RGB が壊れるため）
- 書き出した PNG には **toolVersion 付き straight marker**（`charm-prep-straight-rgba\0` + toolVersion）を記録する
- 検査: CRC・IHDR・IDAT 順序・filter・寸法・展開量（zlib 伸長サイズ）を独立パーサで検査する
- **最終 PNG を独立デコード**（canvas を経由しない自前 PNG パーサ — parsePngRgba / decodeStraightPngBlob 相当）し、生成時 ImageData と**全画素完全一致**させる
- **派生値・検査1〜7・M/F・期待出力ハッシュ・byteLength は最終デコード画素を正とする**（生成中間バッファではなく、書き出し済みファイルを独立デコードした画素で再判定する）

【独立 F oracle と mutation 試験（改訂14 — 39fe815 から還流・colorless の検査F を補強】

- **独立 F oracle**: production 実装（⑦' パイプライン）と独立に、grain 焼き込みの期待値を検算する oracle を準備ツール内へ実装する。要件: ①raw grain bytes の SHA を再照合してからデコードする ②production helper（比率マップ生成関数等）を再利用せず独立デコード・独立実装とする ③数値は Float64 で扱う ④mirrorIndex を独立実装する ⑤blur の加算は**外側 dy・内側 dx の順の逐次加算**とする ⑥shade・u・v・scale から**全革画素の期待 gray を算出**して production 出力と完全一致させる ⑦**非革画素が不変であることも全列挙で確認**する
- **mutation 試験（改訂17 で 5種 → 6種）**: oracle の検出力を自己検証する。production 相当の afterBake を次の変異で生成し、独立 oracle へ通して**不一致が検出されること**を確認する（正常出力は不一致0）: ①Float32 化 ②mirror 誤り ③loop-order 変更（dx 外／dy 内） ④grain map 全1 ⑤非革画素変更 **⑥`shadeMin` 適用範囲誤り（改訂17 新設・§0-E-3）**
- **mutation ⑥ の完全定義（入力・期待値・検出対象）**:
  - **入力**: 厳密 one-hot 革所有画素**以外**（ロゴ主所有 `masks.G ≥ 128` ／ ステッチ主所有 `masks.B ≥ 128` ／ 混合所有）にも `shadeEff = max(shade, shadeMin)` を適用した afterBake を生成する。**該当画素が1つも無いチャーム**では、**革所有画素のうち線形インデックス `y×width+x` が最小の1画素の masks.G を 255 にした変異 masks** を併用して混合所有画素を1件だけ作り、その画素へ `shadeEff` を適用する（変異対象を一意に固定するための規則）。
  - **期待値**: 独立 oracle は `shadeEff` を**厳密 one-hot 革所有画素にのみ**適用するため、**不一致 ≥ 1画素**を報告すること（不一致 0 なら mutation 試験の不合格 ＝ 検査Fの不合格）。
  - **検出対象**: `shadeEff` の適用条件から one-hot 判定が抜け落ち、`shadeMin` がロゴ・ステッチ・混合所有画素の値まで変えてしまう実装。**検査 F-1 (c) の回帰検査**にあたる。
- 検査Fの合格には、oracle 一致（正常系）と **mutation 6種**の全検出（異常系）の両方を要する（改訂17）

【production／oracle Worker の世代管理（改訂14 — 39fe815 から還流】

- production grain 計算と oracle 検算は**別世代状態**を持つ Worker で実行する
- **新世代開始時に旧 Promise を reject し、旧 Worker を terminate する**
- generation と operation token（beginOperation / assertOperationToken / endOperation）を照合し、旧世代の結果が新世代の状態へ書き込まれないことを保証する
- Worker 不可環境の main-thread fallback も**定期的に中断を確認**する
- **timeout 後は timer・Worker・Blob URL・state 参照をすべて解放**する（リーク検査は selftest に含める）

**prep.json（版付きスキーマ。記録値〔入力値＋派生定義値〕の正本。通常表示では読まない — `?debug=1` の転記照合時のみ読込）**

- `schemaVersion`（**スキーマ拡張または生成規則の変更時に増分する** — フィールド構成が不変でも、同一 schemaVersion に生成規則違いの prep.json が混在すると検査8の再生成一致が破れるため規則変更も増分対象。aspectCorrection・sizeMmMeasured を導入した改訂10、⑧' を二段階化〔再defringe〕した改訂12、無着色素材モードを導入した改訂13、**全8種 colorless 化・material-aware 化〔materialAssets・grain per material〕・straight PNG 契約化の改訂14**、**⑦' の出力画素値を変える生成規則の変更〔grainSourceInset・shadeMin〕と α／masks／keep を変える生成規則の変更〔COLORLESS_CLOSED_WHITE・outlineCaptureRadius・BEAK_OUTLINE_REMOVAL〕、および grain 記録項目の拡張〔grainSourceInset・effectiveWidth・effectiveHeight・effectiveRoiSha256・shadeMin〕を行った改訂17**でそれぞれ増分 — **改訂14〜16 の schemaVersion は 5（改訂16では不変 — §0-D-5）、改訂17で 5 → 6 へ増分（§0-E-9）・toolVersion は改訂16で 0.6.0 → 0.7.0、改訂17で 0.7.0 → 0.8.0**（straight marker の toolVersion も連動）。**全8種を v6 の生成規則で再生成し、v5 以前の prep.json・派生アセット・旧 manual レイヤ〔座標系が旧素材由来のもの／旧生成規則由来のもの〕との混在を残さない**〕、ツールバージョン
- 生成入力（canonicalSource）のファイル名と SHA-256、manual-*.png それぞれの参照と SHA-256
- ①の回転・クロップ・縮小値、②の背景しきい値、⑤のロゴ・ステッチ検出しきい値
- 入力値: sizeMm、**sizeMmMeasured（実測確認状態 confirmed・受領日 — §5-2 発動述語に使用。全8種 = true〔7種 2026-07-13・swan 2026-07-28 — §0-D-1・§4-1〕）**、threadColor、stitchMode、**stitchPathsWork（作業座標系の stitchPaths 入力・§5-2）**、**attachmentMode、calibration（mode・計測線座標・reason〔manual-line 採用時は採用理由と計測点の説明を必須記録 — 改訂13〕）**、**sourceMode（"photo" | "colorless"・§5-2）**、**grain（生成 material ごと: materialKey・file・sha256・grainBase・grainAmp・grainRadiusPx・grainScaleMm — §5-2）**
- **派生定義値（正本）**: bboxPx・fgBboxPx・**calibration の計測値**・pxPerMm・baseLum・**keepPxCount（保持画素数・アセット座標）**・**keepPxCountWork（保持画素数・作業座標〔⑧' の前〕。改訂17 新設 — 範囲照合はこちらに対して行う・§0-E-6。keep 規則を持たないチャームでは keepPxCount と同値になる）**・**logoPxCountWork / logoComponentCountWork（G 所有画素数と8近傍連結成分数・作業座標〔⑧' の前・⑤ の検出／manualRecipe 生成直後〕。改訂17 新設 — `logoRange` の範囲照合はこちらに対してのみ行い〔keepPxCountWork と同じ規則・§0-E-8〕、アセット座標の logoPxCount〔`derived.logoPxCount`・⑧' 適用後〕は記録のみで照合対象外。補正非適用チャームでは両座標の値が一致する）**・**aspectCorrection（applied・axis・scale・effectiveScale・preMeasuredW/H・prePxPerMmW/H・preDistortion — 全チャームで記録。§5-2 改訂10）**・**stitchPathsAsset（アセット座標系の stitchPaths 転記値 — 検査5・CHARMS 転記・`?debug=1` 照合の参照先。§5-2）**・**toneMode（sourceMode と1:1対応 — 検査M-2。CHARMS 転記照合10項目の一つ）**・**materialAssets（materialKey ごとの base 期待出力ハッシュ・byteLength・baseLum・textureProfileKey・generationMode="baked-base"）／grainScaleX・grainScaleY／pxPerMmWorkW・pxPerMmWorkH／flatWhite（sourceMode="colorless"・§5-2・§5-6）**
- **grain 記録（material ごと・改訂17 で拡張。§5-2・§0-E-2・§0-E-3）**: `file`・`sha256`・`grainBase`・`grainAmp`・`grainRadiusPx`・`grainScaleMm`・`grainScaleX`・`grainScaleY`・`pxPerMmWorkW`・`pxPerMmWorkH`・`flatWhite`、および**改訂17 で追加した5項目 `grainSourceInset`（非負整数）・`effectiveWidth`（正整数）・`effectiveHeight`（正整数）・`effectiveRoiSha256`（64桁の16進文字列）・`shadeMin`（0 ≤ x ≤ 1 の有限実数）**。**この5項目の追加が `schemaVersion` 5 → 6 の増分理由の一つである**（§0-E-9）。検査F-2 が全項目を照合する
- **期待出力ハッシュ**: material 別 base 全点 / masks.png の**最終 PNG 独立デコード（straight PNG 契約・§5-4）による RGBA の SHA-256**（PNG エンコード差に依存しない比較のため。base は **⑧'-2〔再defringe・§5-2〕適用後の最終画像**が対象 — 補正非適用チャームは ⑧' 全体をスキップするため④〜⑧の出力がそのまま対象）と byteLength
- 自動検査1〜7・M（sourceMode="colorless" は F 含む）の結果値、コミット前検査8の `verify` 結果、および**改訂17 新規検査の per-charm 結果値 `inspections17`（§0-E-10 の3。記録値はすべてツール実測値〔測定系 §0-E-4〕。座標系は個別に明記 — ①は最終 masks〔アセット座標。nearest 変換で one-hot 性は座標系間で保存される・§5-2〕・②③④は作業座標）**: ①`mixedOwnershipPxCount`（非負整数・合格 = 0・§0-E-3）②`outerLeftover` = `{ residualPx, boundaryPx, ratePercent, thresholdPercent }`（thresholdPercent は horseshoe = 1.0・他7種 = 10.0。評価 α ＝ alphaAfterCapture・boundary ＝ dilate4(outerBg, iterations = 1)・§0-E-5）③`grainLocalDeviation` = 生成 material ごとの `{ rowDev, colDev, max, threshold }`（threshold = 0.15・§0-E-2）④`logoInspection` = `{ logoPxCountWork, logoComponentCountWork, logoThreshold, rangeCheck }`（logoThreshold は detectLogo 6種 = 実数値・horseshoe / osanpo = null。**logoPxCountWork は同名の派生定義値と同値であることを検査F-2 が照合し、`logoRange` の照合は作業座標のこの値に対してのみ行う** — アセット座標の `derived.logoPxCount` は記録のみ〔補正非適用チャームでは同値・§0-E-8〕）。**この `inspections17` の新設も schemaVersion 6 の増分理由に含まれる（§0-E-9）**。**selftest 専用の対照生成（`outlineCaptureRadius` 8・`grainSourceInset` 0）の結果は prep-selftest.json（Phase 3 evidence）が正本であり prep.json には記録しない**（prep.json は採用構成の記録のみ）。**詳細証跡（掃引・分布・対照値）の正本は prep-inspection-1-8.json（Phase 4 evidence・§7）であり、`inspections17` は合否判定に必要な要約値のみを持つ**（prep-inspection-1-8.json は prep.json より後に確定するため、prep.json から prep-inspection への SHA 参照は生成順の循環となり持たない — 逆方向〔prep-inspection が各 prep.json を参照・照合する〕は §7 の既存契約のとおり）
- **CHARMS テーブルへの転記一致**: `charms.html` の CHARMS 値が prep.json の**転記照合10項目（§5-2 の完全列挙 — sizeMm・bboxPx・fgBboxPx・pxPerMm・attachmentMode・threadColor・stitchMode・stitchPaths・baseLum・toneMode。入力値・派生定義値をまたぐ）**と一致すること、および **LEATHER_VARIANTS 転記8項目・MATERIAL_PROFILES 転記6項目が color freeze の値と一致すること（§0-A・§5-6）**を `?debug=1` が prep.json / 転記定数を読み込んで機械照合する（§9-0 の受け入れに含む。手転記のミスを検出）

- その他のUI機能: stitchPaths エディタ（頂点打ち＋破線プレビュー）、検証ビュー（暗/明背景合成、マスクオーバーレイ、実寸グリッド、ブラック/アイボリー適用のリカラー試験 — **各チャームは適用可能 variant 全色〔現行 calf 24色〕の回帰試験**〔§7-8〕）
- **手動補正を前提とする**ことで、写真品質のバラつき・自動分類の限界（明色革×白糸、金箔ロゴやラインストーンの彩度など）を人の目で確実に解決する

### 5-5. 合成ステージ（mm 基準座標系）

**実寸校正規則（フェーズ0で8種すべて記録）**

- 実寸 `sizeMm` は革本体のみ（金具除く）。写真側の対応計測は **calibration**（§5-2。正立素材 = bbox モード／斜め素材 = 手動計測線モード。osanpo は後者必須）
- 歪み検査・採用値は §5-2 の決定規則のとおり（幅基準・3%以内。歪み超過チャームは準備工程のアスペクト補正 §5-2 で矯正済み — 補正後は W/H 基準がほぼ一致する）
- 拡縮は等方倍率のみ（縦横別スケールはしない。**非等方の縦横比矯正はフェーズ0準備工程のアスペクト補正〔§5-2〕で完結**しており、実行時アセットは補正済み — 実行時に縦横別スケールを行わない契約は不変）

**配置式（軸ごとに基準を分離して一意化）**

記号: ステージ中心線 `cx`、コンテンツ上端 `contentTopY`（下記）、丸カン下端 `ringBottomY`、革本体間ギャップ `gapMm = 8`、丸カンとチャームの間隔 `ringGapMm = 3`（**非接続レイアウト** — 丸カンとチャームは重ねず間隔を空けて独立表示。§11-8 で確定・2026-07-13。全チャーム共通の定数。初期値3mm・**フェーズ0の81配置検査前に固定**、以後の変更は§7-11再通過が条件 — 下記「幾何パラメータの固定順序」）

**縦方向はコンテンツ縦センタリング（チャームごとの余白均等化。改訂13・2026-07-15 ユーザー承認）**: 丸カンとチャーム前景を含む**表示コンテンツ全体**の上余白と下余白がステージ内で均等になるよう配置する。

```
maxFgHeightMm  = max(表示中チャームの fgBboxPx.h ÷ 当該チャームの pxPerMm)   // 0点表示では項なし
contentHeightMm = ringOuterMm(42) ＋（表示チャーム1点以上なら ringGapMm ＋ maxFgHeightMm、0点なら 0）
contentTopY    = (stageMm.h − contentHeightMm) / 2      // 上余白 = 下余白。選択が変わるたび再計算
丸カン中心y     = contentTopY ＋ ringOuterMm / 2          // 選択に応じて動的（改訂12までの固定 31mm は撤廃）
ringBottomY    = contentTopY ＋ ringOuterMm
```

| 軸 | 規則（mm換算はすべて当該チャームの pxPerMm を使用） |
|---|---|
| y（1点/2点共通） | **前景上端**（`fgBboxPx` 上端）が `ringBottomY ＋ ringGapMm` に一致（丸カンの下に間隔を空けてチャームが並ぶ関係は従来どおり。2点表示で左右の前景高が異なる場合も上端は共通で、コンテンツ高は maxFgHeightMm 基準 — 上下余白の均等化はコンテンツ全体に対して行う） |
| x（2点表示） | 左チャーム: **革本体右端**（`bboxPx` 右端）が `cx − gapMm/2` ／ 右チャーム: **革本体左端**が `cx + gapMm/2`。x配置に fgBbox は使わない |
| x（1点表示） | 革本体（`bboxPx`）の中心xが `cx` に一致（§11-1 センター寄せ・確定） |

- y は前景基準・x は革本体基準、と**軸ごとに基準を分けることで一意に決まる**（丸カンとチャームは接続表現を持たない独立パーツとして表示する。物理接続の再現はスコープ外 — 組み合わせ・配色の確認という目的を優先し、ユーザーが既存ページと同じ表現でよいと確定済み・§11-8）
- 0点表示（丸カンのみ）もコンテンツ縦センタリングの同式で配置する（contentHeightMm = 42 → 丸カンがステージ縦中央）
- チャームの向きは**素材のまま（動物系は頭が左向き）を左右共通**とする。左右反転はしない（革製品は表裏があるため反転表示は実物と乖離する。§11-2）。canonicalSource は全種正立・動物系は頭が左向きで統一（Phase 1 の形状承認済み reference と同一 byte・§4-1c — Phase 4 の §7-2 機械・目視判定で再確認）
- 接地影: 不要（吊り下げ表示のため）。輪郭の自然さは defringe とフェザーで担保

**LAYOUT 定義（幾何の正本）と固定順序（81配置検査を無効化させない）**

- ステージ幾何は単一の `LAYOUT` 定義（コード上の1オブジェクト）に集約して記録する: ステージ寸法（mm）、描画解像度 N、丸カン中心x（**`ringCenterXMm = ステージ幅/2`**）、`ringOuterMm = 42`、`gapMm`、`ringGapMm`。**丸カン中心y は固定値を持たず、コンテンツ縦センタリング式（上記）から選択状態ごとに導出する**（改訂13 — 旧 `ringCenterYMm = 31mm` 固定は撤廃）
- 写真金具の有無はチャーム定義の **`attachmentMode`** として記録する。**素材は全種吊り金具なしのため全8種 `"none"` で統一の見込み**（§4-1c。`"photo"` の分岐は将来の金具付き素材用に定義だけ温存 — その場合は所有権設計 §5-2 とともに要再検討）
- **連結表現は「非接続・独立表示」で確定（§11-8・2026-07-13 ユーザー回答）**: 丸カンは既存 `index.html` と同様にステージ中央上部へ独立して描画し、チャームとは重ねず `ringGapMm` の間隔を空ける。接続パーツ（しずくコネクタ等）・重なり表現・接続検査はいずれも**不採用**（検討経緯: 旧・案A=2mm重なり直結／案B=しずくコネクタ描画は 2026-07-12 レビューで接続保証の契約まで詰めたが、ユーザーが非接続の独立表示を選択したため全て不要となった）
- 以下は**81配置検査（§7-11）を実行する前に確定**する: LAYOUT の全値、チャームの向き、1点表示の配置（§11-1・11-2・11-8 は回答済み・2026-07-13）
- **81配置検査（左右 9択×9択 — 8種＋なし。改訂13）の判定は3条件**とする: ①**クリップなし**（チャーム前景 fgBbox と丸カン描画境界がステージ内に収まる） ②**重なりなし**（丸カンとチャーム前景の間隔が ringGapMm どおりであること、および2点表示の左右チャーム前景 fgBbox 同士が交差しないこと） ③**負の余白なし**（`contentHeightMm ≤ stageMm.h` — 上下余白 ≥ 0）。事前計算と実描画の境界を一致させる（非接続のため接続検査は不要）
- **確定後に LAYOUT のいずれか・丸カンアセット・描画解像度 N を変更した場合**（フェーズ1のテイスト確認による調整を含む）**は、ステージ寸法と全81配置を再計算し、フェーズ0ゲート（§7-11）を最初から再通過してから先へ進む**。**チャーム側の確定値（sizeMm・calibration・aspectCorrection・bboxPx・fgBboxPx・pxPerMm）のいずれかが変わった場合**（swan の実測受領による更新・アスペクト補正の再適用を含む）**も同様に、§7-11 の81配置検査と §9-2 の実寸比率検証を再通過する**

**ステージ寸法（仮値 → フェーズ0で機械検証）**

- 仮値: 幅は革寸法ベースの最大組み合わせ **horse×horse = 95 + 8 + 95 = 198mm** ＋左右マージン各11mm → **幅220mm**（swan 90×90 を加えても幅の最大は horse×horse のまま）。高さはコンテンツ最大 = 丸カン42 + 間隔3（ringGapMm） + 前景最大高（仮: 革最大 **90〔swan・ストア記載。改訂13〕**＋予備10）= 145mm に上下余白（各 ≥7.5mm）を加え → **160mm**（縦センタリングでは固定マージンではなく余白 = (stageMm.h − contentHeightMm)/2 が上下へ均等配分される — 上記配置式）。※改訂12まで の仮値は 155mm（革最大75〔frenchie〕基準）・実装ブランチの現行 LAYOUT（swan 追加前・旧上端揃え式）は 220×160
- **フェーズ0の確定値（fgBboxPx / bboxPx / pxPerMm）を用い、全81配置について上記配置式で前景・丸カン境界を機械計算し、3条件（クリップ・重なり・負余白なし）を検査**する。仮値で満たせなければステージ寸法を更新し、§9-1 の受け入れと連動させる（前景の張り出しと丸カン描画境界を含めた検証であり、革寸法だけの机上計算では確定しない）
- 描画解像度: `1mm = N px` の単一定数（初期値 N=6 → 1320×960px。フェーズ1の性能実測で確定）
- 丸カン: ステージ横センター固定（外径42mm厳守）。縦位置はコンテンツ縦センタリング式で動的（上記）

### 5-6. リカラー・ステッチ・金具（丸カン）・ロゴ・保持

**色の正本 — LEATHER_VARIANTS・MATERIAL_PROFILES・DISPLAY_VARIANTS（改訂14）**

- **正本は plans/charm-approved-color-freeze.json**（sourceFreezeCommit `8b3bdb4` で凍結。5 canonicalColorChart・2 materials・27 variants・approvedHex・panelRect/samplingRect・grainSource — Phase 1 承認 AE-20260718-04〜06・10〜14）。本計画書・実装定数への転記が freeze と食い違う場合は **freeze が勝ち**、訂正は転記側の修正で行う（freeze 自体の変更は同期計画 §12-2 の Phase 1 巻き戻し事項）
- **LEATHER_VARIANTS（27件）**: 色選択の正本。旧 `COLORS`（24色配列）を**置換**する。variantKey = `materialKey + ":" + colorKey` で一意化し、**無効な material × color の直積を生成しない**（27件だけを列挙）。各 entry は転記8項目 — **variantKey・materialKey・colorKey・canonicalLabel・aliases・approvedHex・textureProfileKey・appliesToCharms**。**項目別の由来（転記合成規則）**: textureProfileKey 以外の7項目 = color freeze の variants から直接転記／**textureProfileKey = variant.materialKey で color freeze の materials を引いた textureProfileKey**（freeze の variants 自体はこのフィールドを持たないため join で合成する）。`?debug=1` 機械照合・検査F-4 も**この同じ合成規則**で照合する（§0-A）
- **MATERIAL_PROFILES（2件）**: material の正本。転記6項目 — **materialKey・canonicalLabel・textureProfileKey・grainPolicy・grainSourceSha256・generationMode**。**項目別の由来（転記合成規則）**: materialKey・canonicalLabel・textureProfileKey・grainPolicy の4項目 = color freeze の materials から直接転記／**grainSourceSha256 = materials[].grainSource.outputSha256**（実際に焼き込みへ使う canonical grain ファイルの byte SHA: calf=`d0b7b644…`／chevre=`5d3f86a0…`）／**generationMode = 本計画書 §5-2 の確定値 `"baked-base"`**（color freeze には存在しないフィールド — 正本は本計画書）。`?debug=1`・検査F-3 も**この同じ合成規則**で照合する
- **MATERIAL_PROFILES の非転記メタデータ（転記6項目とは別に profile 定数へ持たせ、検査F-3・selftest が照合する — 同期計画 §6-2 の「grain source SHA/ROI・生成方式・適用可能な sourceMode/toneMode」要件）**: ①grain 原本来歴 = grainSource.inputSha256（calf=`d0b7b644…`〔input=output・full-frame 再利用〕／chevre=`f0a65c5b841285a1e34f9f39a7d9500ad414a23c11569c5b94c7a42b0492518d`〔シェーブル質感.jpg〕）と採用 ROI（calf = full-frame `{x:0,y:0,w:1011,h:676}`／chevre = 縁10px除外 `{x:10,y:10,w:824,h:383}`） ②canonical grain output の path・寸法・SHA（§4-1c マニフェストの grain 2行と同値） ③**許可 sourceMode/toneMode pair = 両 material とも `colorless/flat` のみ**（⑦' グレイン焼き込みは colorless 専用 — photo/photo への material 適用は定義されない）。**準備ツールは canonical output（path 記載のファイル全体）を grain 入力として使用し、原本（input）からの再導出はしない**（生成証跡の正本は color freeze の grainSource — AE-20260718-13/14）
- **DISPLAY_VARIANTS**: 表示・変換入力に使う hex の正本 = **LEATHER_VARIANTS の approvedHex そのもの**（旧 referenceIvory / DISPLAY_COLORS を置換 — 全種 colorless 化により frenchie 実測アイボリー基準は存在しなくなったため撤回。アイボリーは calf:ivory の approvedHex `#ece7dd`）。**導出計算は存在しない**（freeze 済み定数の転記のみ。Lab 変換・色域圧縮が発生するのは flatLeatherTone 実行時のみ）
- **canonicalLabel「シエルブルー」・alias「シェルブルー」（AE-20260718-04）**: UI 表示・サマリー文は canonicalLabel を使う。**旧 COLORS の 24 hex（シェルブルー #6cb9cf 等）は変換・表示のどこにも使わない**（旧値との比較記録は Phase 1 の color-sheet にのみ残る）

| materialKey | canonicalLabel | textureProfileKey | grainPolicy | grainSourceSha256（=output） | generationMode |
|---|---|---|---|---|---|
| calf | カーフ | calf-embossed-v1 | approved-reuse | `d0b7b644bcce2691b42b776f054ad2dd3f6c23313dab72f6a1b83b1292943a2a` | baked-base |
| chevre | シェーブル | chevre-grained-v1 | distinct | `5d3f86a0f2da40a482be2e4efc6aa98c636b108b05de32b3cdb341ac9bba6774` | baked-base |

**LEATHER_VARIANTS 27件（参考転記 — 正本は color freeze。不一致時は freeze が勝つ）**

| variantKey | canonicalLabel | approvedHex | appliesToCharms |
|---|---|---|---|
| calf:black | ブラック | `#41403d` | 8種全部 |
| calf:brick | ブリック | `#885854` | 8種全部 |
| calf:brown | ブラウン | `#935a45` | 8種全部 |
| calf:charcoal-gray | チャコールグレー | `#616062` | 8種全部 |
| calf:ciel-blue | シエルブルー（alias: シェルブルー） | `#6cbacf` | 8種全部 |
| calf:coral-pink | コーラルピンク | `#ef9f7e` | 8種全部 |
| calf:etoupe | エトープ | `#a48e73` | 8種全部 |
| calf:gold | ゴールド | `#c8925f` | 8種全部 |
| calf:greige | グレージュ | `#82756b` | 8種全部 |
| calf:ice-gray | アイスグレー | `#cbcdcc` | 8種全部 |
| calf:ivory | アイボリー | `#ece7dd` | 8種全部 |
| calf:light-blue | ライトブルー | `#b0ece8` | 8種全部 |
| calf:lime-yellow | ライムイエロー | `#f0e15c` | 8種全部 |
| calf:mist-blue | ミストブルー | `#7aaabf` | 8種全部 |
| calf:navy | ネイビー | `#3c4d61` | 8種全部 |
| calf:orange | オレンジ | `#e59134` | 8種全部 |
| calf:pale-pink | ペールピンク | `#e7cbda` | 8種全部 |
| calf:pale-yellow | ペールイエロー | `#f4ebb8` | 8種全部 |
| calf:pink | ピンク | `#f78192` | 8種全部 |
| calf:pistachio | ピスタチオ | `#94ad79` | 8種全部 |
| calf:purple | パープル | `#a855a2` | 8種全部 |
| calf:red | レッド | `#e45c61` | 8種全部 |
| calf:rose-pink | ローズピンク | `#a93f74` | 8種全部 |
| calf:royal-blue | ロイヤルブルー | `#445fc1` | 8種全部 |
| chevre:green | グリーン | `#00894c` | **空（選択不可）** |
| chevre:lavender | ラベンダー | `#8089e3` | **空（選択不可）** |
| chevre:pale-greige | ペールグレージュ | `#9e8e89` | **空（選択不可）** |

- **appliesToCharms**: calf 24 variant = 8キー全件／**chevre 3 variant = 空（AE-20260718-10 — 現行8チャームでは選択不可。27 variant の定義は将来アイテム用の正本として凍結済み）**。有限値・色回帰・クライアント確認の件数は §0-A（finiteCases=192）
- **色名・hex・grain・material UI のいずれかを変更したら**、color freeze（Phase 1 巻き戻し）・MATERIAL_PROFILES・DISPLAY_VARIANTS・debug 照合・192有限値・variant 色回帰・クライアント確認を**連動更新**する（同期計画 §6-3・§12-2）

**革リカラー（toneMode で分岐・§5-2）**

- `toneMode="flat"`（**全8種**・無着色素材モード・§5-2）: `flatLeatherTone(base.RGB, DISPLAY_VARIANTS[選択variantKey].approvedHex, materialAssets[variant.materialKey].baseLum)` を革所有画素に適用（**base と baseLum は選択 variant の materialKey に対応する materialAssets のものを対で捕捉**する — 現行はすべて calf のため CHARMS.baseLum と同値・§5-2）
- `toneMode="photo"`（**適用チャーム0** — 将来の photo 素材用の予備契約）: `photoLeatherTone(base.RGB, 選択色, baseLum, 1.0)`（**全呼び出し箇所で4引数・shadeFactor=1.0 固定**・§5-3）
- 左右チャームで独立した variant 選択（material 絞り込み UI・§6-1）。**有限値検査は 8種 × 適用可能 variant = 192組（§0-A）**で、トーン関数の出力RGBが有限値であることを検査項目とする（§9-13）

**flatLeatherTone（Lab 空間の質感合成・改訂13）**

```
flatLeatherTone(base, displayColor, baseLum):
  lum   = (0.2126·R + 0.7152·G + 0.0722·B) / 255        // §5-2 と同一係数。常にこの加重輝度式で計算する — R=G=B（lum = R/255）が成り立つのは革主所有 ∧ base.α=255 の画素のみ（検査F-1(b)）で、α<255 のエッジ帯等では R 単独への簡略化を禁止
  ratio = clamp(lum / max(baseLum, 0.001), 0.35, 1.60)   // クランプ幅・ゼロ除算ガードは photoLeatherTone と同値（§5-3）
  Lab   = srgbToLab(displayColor)                         // sRGB(IEC 61966-2-1) → 線形化 → XYZ(D65: Xn=95.047, Yn=100, Zn=108.883) → CIELAB 標準式
  L'    = clamp(Lab.L × ratio, 0, 100)                    // 質感（明暗変動）を L* に乗算合成。a*・b* は不変 = 色相・彩度を保つ
  rgb   = labToSrgb(L', Lab.a, Lab.b)
  if rgb のいずれかの成分が [0,255] 範囲外（= sRGB 色域外）:
    L' と色相角 h（LCh）を固定し、彩度 C を 0〜元値の二分探索（固定20回・最終値は色域内側）で必要最小限まで圧縮して再変換
  return Math.round で 8bit 量子化した RGB
```

- **ハイライト保護（photoLeatherTone の lum≥0.94 ブレンド）は適用しない**: 焼き込みベースは白面基準（baseLum が高い）のため、保護を掛けると大半の革画素が白へ希釈され選択色が出なくなる
- **色域外の写像は「明度と色相を保ち彩度のみ必要最小限圧縮」が契約**（2026-07-15 ユーザー承認）— 成分クリップ等の他の写像は不可。C=0（無彩色軸）は L'∈[0,100] で常に sRGB 内のため二分探索は必ず収束する
- **性能注記（LUT の適用範囲を限定）**: **革主所有 ∧ base.α=255 の画素**は R=G=B（⑦'・⑧'-1 nearest・検査F-1(b) の機械保証）のため lum = R/255 は 256 通り — この範囲の画素は選択色ごとの 256 エントリ LUT で上式と**完全一致のまま**高速化できる。**エッジ帯（base.α<255）の革所有画素は再defringe 由来で RGB が等しくないことがあり LUT 対象外 — 上式を直接計算する**（どちらの経路も本式が正。式と結果が一致しない近似は不可）

**旧 referenceIvory / DISPLAY_COLORS 契約の撤回（改訂14）**

- 改訂13の「アイボリー = frenchie 実測 referenceIvory・他23色 = COLORS 基準」の契約は、全8種 colorless 化により**撤回**した（写真由来の実測アイボリーがどこにも存在しなくなったため）。表示・変換入力は本節冒頭の **DISPLAY_VARIANTS（approvedHex）に一本化**する。charms.html へは定数として**転記**し、実行時に再計算しない（`?debug=1` が転記照合 — 転記の一致は検査F-4 でも機械検証・§5-4）
- 改訂13 §8 の「photo / flat の左右比較差」テイスト確認項目も対象消滅により削除（全チャームが同一の flat 方式のため方式間差は発生しない）。variant 間・material 間の見え方は §7-8 の色回帰と Phase 7 のクライアント確認で確認する

**ステッチ（白/黒トグル・実物仕様に整合）**

- 実物仕様（§2）: 白ステッチ標準・黒変更カスタム可が明記されているのは **horseshoe / osanpo の2種のみ**。他5種はステッチなし。プレビューもこれに合わせる
- stitchMode は **detect / none / fallback の3値**（**割当は Phase 4 で per-charm に確定する — 検査5〔§5-4〕と §0-E-11 の Phase 6 完了条件は確定した stitchMode 値に対する述語で判定し、下記の括弧内割当は見込みの例示**）:
  - `detect`（horseshoe / osanpo 見込み）: 準備工程で素材のステッチ画素をマスク化（自動検出は糸色極性 `threadColor` に応じ、暗糸なら低輝度側・白糸なら高輝度側を検出）。**自動検出が成立しない素材では、準備ツールの手動補正（manual-owner）でステッチ画素を B 所有に指定してマスク化してよい**（無着色素材の horseshoe は白ステッチ×白革面でコントラストがなく自動検出は不成立見込み・§4-1c — マスクの由来〔自動/手動〕は実行時挙動・検査5の条件に影響しない）。実行時は**選択色（白/黒）を問わずマスク画素を明示的に塗り直す**（「白選択時は原画のまま」という元糸色依存の仕様にはしない — 元糸の白と選択白の色を一致させ、黒切替と同一のコードパスを通す）
  - `none`（horse / frenchie / dachshund / toypoodle / cat の5種見込み）: `masks.B` 所有画素数 = 0（共通述語 `masks.α≥128` 込み — `count(masks.α≥128 ∧ masks.B>0) = 0`。保持画素のB値は検査しない・§5-2）。ステッチトグルの影響を受けない（検査5で機械保証）
  - `fallback`: detect が不成立の場合のみの予備手段（stitchPaths 契約 §5-2。現時点で該当チャームなし）
- **swan の stitchMode は上記の見込みに含めない**: Phase 4 で canonicalSource の実画素から detect / none / fallback の完全列挙条件（§5-2・検査5）で判定する（§4-1c。Phase 4 バッチ実測では detect 成立 — B所有 1,106px・白黒差分 1,106px。最終確定は新 toolCommit での Phase 4 再実行時・§0-D-7）。**detect / fallback と確定した場合はステッチ有チャームに加わり、UI 注記（§6-1）・§9-3 のケース数（40→44）・サマリー表記の対象を更新する**
- ステッチトグルUIの有効条件は §4-2（ステッチ有チャームが表示中のときのみ有効）
- 確定はフェーズ0で素材ごとに判断し、テーブルに記録

**金具（丸カン・銀/金トグル）・ロゴカラー（金/銀トグル）・ラインストーン（保持）— §11-8/9/10 確定（2026-07-13）**

- **金具トグル（銀/金）は丸カン専用**: 準備工程で生成した透過PNG（`ring-silver.png` / `ring-gold.png`・42mm外径の共通基準枠に正規化・中心一致）の差し替え。切替で位置・サイズ不変（§9-8）。実行時の背景除去はしない（§5-3）。チャームビットマップには影響しない（キャッシュキーにも含めない・§5-7）
- **ロゴカラートグル（ゴールド/シルバー・金具トグルと独立）**: 箔押しロゴ「&.」（ロゴマスク G 所有）に `metalTone()` を適用し、選択ロゴカラーで描画（金選択→金箔・銀選択→銀箔）。初期値は**ゴールド**（素材上は金ロゴが多数派〔銀は dachshund の箔と swan のシルバー立体ロゴ〕— masks 化後は選択ロゴカラーで描画されるため素材の箔色は表示に影響しない）。面積が小さいため手動補正（manual-owner）での塗り分けを想定。有効条件はチャーム表示中（1点以上）— チャームなしでは無効化（§4-2。**全8種ロゴあり確定 — swan は AE-20260718-07**）
- **ラインストーン等（horseshoe の6個＋台座、swan は `swanKeep` = beak ∪ stone の2成分ちょうど〔改訂17で確定・§0-E-6〕）は保持**: masks.α<128（§5-2）でリカラー対象外とし、素材のクリスタル・台座・swan のくちばし着色とストーンをそのまま表示。革色・ロゴ・ステッチ・金具のどのトグルにも反応しない。塗り分けは manual-keep レイヤ（§5-4。horseshoe の6ストーンは位相検査 — 検査6・§5-4）
- チャーム側に写る金属・光沢装飾は**「ロゴ（G・ロゴトグル連動）」または「保持（masks.α<128・そのまま）」のどちらかに必ず分類**する。これを Phase 4 の**非免除合格条件**とする（§7-8。素材に吊り金具は写っていないため、対象はロゴ箔・ラインストーン・swan の stone〔旧「金具鋲」表記。改訂17 で `swanKeep` = beak ∪ stone に確定・§0-E-6〕。**swan のくちばしはアクセント色保持で確定 — AE-20260718-08**）
- 手動ブラシ補正（§5-4）を使っても塗り分けられない素材は**再撮影（写真）または素材の再提供（無着色素材）の依頼**とする。「画像から装飾を除去する」案は**採用しない**（単純なα消去は跡が穴になる。インペイント修復は準備ツールのスコープ外）
- 実物の吊り仕様（革紐標準・金具カスタム）とプレビュー表現（丸カン独立表示）の関係は §2・§5-5・§11-8 を参照

### 5-7. 性能・メモリ設計

- 実行時のピクセル走査はリカラー変換のみ（背景除去・マスク生成は準備工程で完了済み）
- 派生アセットは選択時に遅延ロード。**転送量上限: base+masks 合計 ≤ 900KB/チャーム**（§5-4 書き出し前検査7としてフェーズ0で機械検査。超過時はPNG最適化または解像度の見直し）
- リカラー結果は **`charmKey|variantKey|materialKey|logo|stitch` キー**でキャッシュ（LRU・上限8エントリ。丸カンの金具トグルはチャームビットマップに影響しないためキーに含めない — 改訂14: variantKey / materialKey を含めることで material 別 base の取り違えを構成的に排除）。**LRU退避時は `ImageBitmap.close()` 等で明示解放し、9件目生成時の退避→解放→再生成の動作を受け入れ試験で確認**する（§9-12）。**material 切替を含む素早い連続切替時は、古い非同期結果（旧 material の base ロード等）を世代トークンで破棄**する（§9-9）
- **ピークメモリの概算対象**を「デコード済み派生アセット（全8種保持で概算92MB: 1200²×4B×2枚×8種）＋LRU完成ビットマップ8エントリ＋作業ImageData＋ステージcanvas」と定義し、フェーズ3で概算値を報告する（swan を含む）。モバイルSafariでのメモリ挙動（タブ再読込の発生有無）を実機確認する
- ステージ再描画は `drawImage` 合成のみ
- **性能基準と計測方法**:
  - キャッシュ済み切替: 入力イベント発火→描画完了（最終 `drawImage` 直後）を `performance.now()` で計測し、**連続10回の中央値 ≤ 300ms**。対象: macOS Chrome 最新・iOS Safari（実機1機種以上）
  - 初回選択（アセットロード込み）: **HTTP warm cache（2回目以降のアクセス相当）で、相異なる2チャームを同時ロードする条件に固定し ≤ 1500ms**、その間は既存の「読み込み中」表示。cold cache（初回訪問）はローディング表示があれば基準対象外と明記
- **非同期の追い越し対策**: 描画に世代トークンを付与し、素早い連続切替時に古い非同期結果（画像ロード完了等）が最新状態を上書きしないことを保証する（§9-9）

## 6. UI仕様（charms.html）

### 6-1. 画面構成

- 左パネル: プレビューキャンバス（既存 `index.html` と同配置・スマホは縦積み＋2/3幅ルール踏襲）
- 右パネル(コントロール):
  1. **左チャーム選択**: 9択（8種＋なし）
  2. **左チャーム革選択（material → 色の2段選択・改訂14）**: 各サイドの選択は variantKey（§5-6）。**選択可能 material が2種以上のときのみ革種セレクタを表示**し、material を選ぶとその material に属する色だけをスウォッチ表示する（AE-20260718-10。**現行8チャームは calf のみのため革種セレクタは非表示 — カーフ24色スウォッチのみの従来相当の見た目**。グループ選択UIは既存流用・色の表示名は canonicalLabel〔シエルブルー等・§5-6〕）
  3. **右チャーム選択**: 9択
  4. **右チャーム革選択**: 同上（左と独立）
  5. **金具カラー**: シルバー/ゴールド 2択ボタン（丸カンに適用）
  6. **ロゴカラー**: ゴールド/シルバー 2択ボタン（チャームの箔押しロゴに適用・金具カラーと独立。有効条件は §4-2 — チャーム1点以上表示中のみ）
  7. **ステッチカラー**: ブラック/ホワイト 2択ボタン（有効条件は §4-2 — ステッチ有チャーム表示中のみ。無効時はグレーアウト＋ステッチ対応チャームの注記表示。注記の内容は stitchMode 確定値に連動 — 現状「ステッチ対応: horseshoe / osanpo」・**swan が detect / fallback と確定した場合は swan を注記へ追加する**〔§5-6〕）
  8. サマリー＋コピーボタン
  9. カラーチャート参照（canonicalColorChart・reference-visuals/colors/ — 現行は calf 4枚を表示。chevre チャートは chevre variant が選択可能になった時点で追加）＋注意書き
- チャーム選択UIの形式は**プルダウンで確定**（§11-4・2026-07-13。選択肢の表示名は §4-1 の英語表記）
- 無効化ルールは §4-2 の表のとおり（【なし】側スウォッチの無効化、ステッチトグルの有効条件 = **ステッチ有チャーム表示中のみ**、ロゴトグルの有効条件 = **チャーム1点以上表示中のみ**）

### 6-2. 状態モデル

```js
const state = {
  left:  { charm: "horse",     variantKey: <VARIANT_KEY> },   // charm: 8種のキー or "none"。variantKey は LEATHER_VARIANTS のキー（§5-6・改訂14 — 当該チャームの appliesToCharms に含まれる variant のみ有効）
  right: { charm: "horseshoe", variantKey: <VARIANT_KEY> },   // 初期表示 = 左 horse × 右 horseshoe（§11-5 確定・2026-07-13。ステッチ有チャームを含み全トグルが初期状態で有効）
  metal: "silver",   // 丸カン（§5-6）
  logo:  "gold",     // チャームの箔押しロゴ。初期値ゴールド（§5-6。全8種ロゴあり — AE-20260718-07）
  stitch: "white",   // 初期値ホワイトで確定（ステッチ有2種の実物標準仕様に一致・§2。§11-3）
};
```

### 6-3. サマリー文仕様（表示状態と完全一致）

- **色名は canonicalLabel（§5-6 — シエルブルー等）**で表記する。**材質表記は、選択可能 material が2種以上のチャームが表示中の場合のみ**色名の前に material の canonicalLabel を付す（例: `「シェーブル グリーン」`）。**現行（calf のみ）は付さない — 下記例文の従来表記と互換**
- **ロゴ表記は、チャームが1点以上表示中の場合のみ**含める（全チャームにロゴがあるため常に適用値・§4-2 のトグル有効条件と一致）
- **ステッチ表記は、表示中にステッチ有チャーム（stitchMode ≠ none）が1つ以上ある場合のみ**含める（トグルの有効条件 §4-2 と一致）
- 2つ表示（ステッチ有を含む）: `現在の組み合わせ: 左 horse「ネイビー」 / 右 horseshoe「レッド」 / 金具「シルバー」 / ロゴ「ゴールド」 / ステッチ「ホワイト」。`
- 2つ表示（ステッチ有なし）: `現在の組み合わせ: 左 horse「ネイビー」 / 右 cat「レッド」 / 金具「シルバー」 / ロゴ「ゴールド」。`
- 1つ表示: `現在の組み合わせ: osanpo「ブラック」（1点） / 金具「シルバー」 / ロゴ「ゴールド」 / ステッチ「ホワイト」。`（ステッチ無チャームなら同様にステッチ表記なし）
- 0個: `現在の組み合わせ: チャームなし / 金具「シルバー」。`（ロゴ・ステッチ表記なし・トグルも無効化済み）
- コピーボタンは表示中の文面と完全一致でコピー（既存実装流用）

### 6-4. ナビゲーション

- `charms.html` 上部に `index.html` へのテキストリンクを置く（`配色プレビュー（ドーナツ＆キャンディ） | チャーム組み合わせ`）
- `index.html` 側への逆リンクは§3のとおり**別工程**（フェーズ4・メインセッション担当）

## 7. 写真素材の受け入れ要件（フェーズ0チェックリスト）

生成入力8種（全種 canonicalSource・colorless・§4-1c）を、以下の観点で検証してから派生アセット生成に入る。**「免除」列が「不可」の項目は例外承認で通過できない**（§8 Phase 4 ゲート）。Phase 1 の機械確認（背景 = コーナー**輝度 255 単色**〔改訂17 訂正・§0-E-12〕・寸法・SHA — phase-1-approvals.md 実測記録）により 1・3 は事前確認済みだが、**Phase 4 で準備ツールによる機械検査・最終判定を改めて行う**（**4〔前景解像度〕は背景除去後の fgBbox 計測ではじめて確定するため事前判定に含めない**）。**5・6 は toneMode="photo" 専用要件のため改訂14では適用対象0**（全種 colorless — 行は将来素材用に温存）。

| # | 要件 | 免除 | 満たさない場合の対処 |
|---|---|---|---|
| 1 | チャーム単体で全体が写っている（見切れなし） | 不可 | 再撮影依頼 |
| 2 | 正面・吊り下げ時と同じ上下向き | 可 | 回転補正で吸収可能か個別判断 |
| 3 | 背景が無地でチャームと分離可能（白/ライトグレー推奨） | 不可 | 準備ツールのしきい値調整＋手動ブラシ補正で解決（それでも不可なら再撮影） |
| 4 | チャーム部分の解像度: **背景除去後の前景（fgBbox）の長辺が 600px 以上**（画像全体の寸法では判定しない — 白余白は前景解像度を保証しないため） | 不可 | 拡大は不可。**canonicalSource の前景解像度自体は十分（≥600px）で、①の全面縮小（長辺≤1200px）が白余白ごと前景を縮めたことが原因の場合は、cropSource 指定（§5-4・§0-D-2）で余白を除去してから再判定する（改訂16: frenchie が該当 — canonicalSource 前景長辺 1232px を機械確認済み・判定基準は不変）**。素材自体の前景が不足する場合は再撮影・再素材依頼 |
| 5 | 照明条件が概ね揃っている（極端な色かぶり・強い影がない。**将来 toneMode="photo" となる素材のみ — 改訂14では0種** — colorless は照明ではなくグレイン焼き込みが質感の正のため対象外） | 可 | baseLum 正規化で吸収 → 不可なら再撮影 |
| 6 | 革色が明るめ単色（アイボリー等）だと濃色変換に最有利（**将来 toneMode="photo" となる素材のみ — 改訂14では0種** — colorless は無着色のため対象外） | 可 | 濃色ベース写真は変換品質を個別確認 |
| 7 | **stitchMode の確定**: ステッチ有チャーム（horseshoe / osanpo。**swan は実画素から §5-2 完全列挙で判定・§4-1c**）は糸色を記録し detect のマスク成立（自動検出または manual-owner 手動指定・§5-6。**osanpo は自動検出不成立が Phase 4 バッチで確定 — Phase 3 追加の決定的レシピによる manual-owner 機械生成で成立させる・§0-D-3**）を確認。ステッチ無チャームは `masks.B` 所有画素0（none。共通述語 `masks.α≥128` 込み・§5-2）を確認。fallback 採用時（detect 不成立の場合のみ）は stitchPaths の定義＋**元糸RGBの中和（manual-inpaint）**＋**白・黒どちらの選択でも元糸の残像が見えない**こと | 不可 | detect 不成立は fallback（手動破線パス＋元糸中和）に切替 |
| 8 | **見えている全金属・光沢装飾が「ロゴ（G・ロゴトグル連動）」または「保持（masks.α<128）」に正しく分類されている**（§5-6。素材に吊り金具は写っていない・§4-1c。swan のくちばし = アクセント色保持で確定 — AE-20260718-08）。確認方法: 準備ツールのリカラー試験で **horseshoe = 革{ブラック, アイボリー}×ロゴ{金, 銀}×ステッチ{白, 黒}の8ケース（ラインストーンが全ケースで元色のまま不変であることを含む — 6ストーン位相検査は検査6）、全8種 = ロゴ{金, 銀}の2ケースずつ（swan がステッチ有と確定した場合は horseshoe 同様の8ケースも実施）**を目視し、素材検証レポートに記録（§9-3 の固定検証セットは Phase 6 でのこの回帰再実施）。**全8種はさらに適用可能 variant 全色（現行 calf 24色）の片側表示回帰（variant 色回帰）**を目視し、グレインの継ぎ目・タイル反復の視認・色域外圧縮起因の色調破綻・白面の色抜け・素材元色の混入がないことを確認する（§5-2・§5-6）。**アスペクト補正適用チャーム（§5-2）はこの目視で、補正起因の不自然な伸縮・楕円化・ぼけ・色ハロが輪郭・革テクスチャ・ロゴ・ステッチ・ラインストーンにないことを併せて合格条件とする** | 不可 | 手動ブラシ補正（manual-owner / manual-keep）で解決 → 不可なら素材再提供の依頼（除去は不可・§5-6） |
| 9 | 撮影歪み: pxPerMm 歪み率3%以内（§5-2 決定規則。**アスペクト補正適用チャームは補正後の値で判定**） | 不可 | 超過時は ①実寸を実測で確認（sizeMm 誤りなら §10 経路） → ②実測確定済み（bbox モード）なら**アスペクト補正（§5-2）を適用して再生成**（補正後確定値で §7-11・§9-2 を再通過 — §5-5） → ③補正後も超過・manual-line 素材で補正不能なら再撮影 or 個別設計（計画書改訂） |
| 10 | **§5-4 の書き出し前検査1〜7・M（sourceMode="colorless" は検査F込み）に全通過（通過するまで書き出し不可）＋コミット前検査8（再生成一致）に合格（合格するまでコミット不可）**＋暗色/明色背景での輪郭の目視確認 | 不可 | 準備工程をやり直し |
| 11 | **幾何パラメータ固定（§5-5）のうえ、全81配置の3条件検査（クリップなし・重なりなし・負の余白なし・§5-5）**（確定値による機械計算。対象は2点配置・1点センター配置・0点の各表示チャーム前景と丸カン描画境界。非接続レイアウトのため接続検査はない） | 不可 | **不合格の条件別に修正**: ①クリップ・③負の余白 → ステージ寸法を更新／②左右前景の重なり → gapMm または x 配置式を見直し／丸カンとチャームの間隔不足 → ringGapMm または y 配置式を見直し（ステージ拡大では②は解消しない）。いずれの変更も LAYOUT を再固定 → 81配置全件を最初から再実行（§5-5。フェーズ0完了後の変更ならチェックポイント②も再取得） |

**Phase 4（旧フェーズ0）の成果物**: ①8種の派生アセット一式（material 別 base／masks の straight RGBA PNG＋prep.json〔**schemaVersion 6**・calibration・keepPxCount・grain・materialAssets 等の記録値一式を含む〕＋manual-*.png）、②丸カン透過PNG（ring-silver/gold）、③チャーム定義テーブル全値（CHARMS 転記照合フィールド10項目 — sizeMm確定値・bboxPx・fgBboxPx・pxPerMm・attachmentMode・threadColor・stitchMode・stitchPaths・baseLum・toneMode。§5-2）＋LEATHER_VARIANTS／MATERIAL_PROFILES 転記値、④素材検証レポート（**8種構成** — チェックリスト判定＋例外事項＋81配置3条件検査結果＋書き出し前検査1〜7・M・F・コミット前検査8＋独立 F oracle／**mutation 6種**の記録〔改訂17 — **⑥`shadeMin` 適用範囲誤りの検出結果を必ず含む**・§5-4〕＋**§7-8 のロゴ／ラインストーン目視結果と variant 色回帰結果**＋fixedVisualCaseCount のチャーム別内訳＋swan の所有権分類記録〔§4-1c〕＋**改訂16 追加分〔**改訂17 注記: 本項のうち osanpo のレシピ生成記録は id `osanpo-colorless-20260730-v2` に対して行う。旧 id `osanpo-colorless-20260728-v1` は §0-E-4 で失効しており、v1 に対する生成記録は現行の成果物要件ではない（rev16 の記録は監査証跡として rev16 ディレクトリに残す）**〕: osanpo のレシピ生成記録〔レシピ id・生成画素数と成分数（mainWhite・吊り穴・rawStitch・final stitch・logo）・全 range 照合結果・検査5 detect 成立値（B所有・白黒差分・threadColor）〕・frenchie の cropSource 値と §7-4 再判定結果〔crop 後の fgBbox 長辺〕・swan のアスペクト補正発動記録〔preDistortion・適用軸・scale・補正後歪み〕（§0-D-1/2/3）。**改訂17 追加分（§0-E-1/2/3/4/5/6/8）**: ①6種の `COLORLESS_CLOSED_WHITE` 生成記録〔レシピ id "colorless-closedwhite-20260730-v1"・革面成分数と成分別画素数・革面 bbox・吊り穴 画素数と bbox と選択した seed 距離・微小成分数と最大面積・全 range 照合結果〕 ②**全8種の外周取り残し率**〔§0-E-5 の定義・合格閾値 ≤ 10.0%〕 ③**全8種の logoPxCount と成分数**、および**採用 threshold**〔`detectLogo` を用いる6種のみ実数値。**`manualRecipe` が G 所有を直接生成する horseshoe / osanpo は `logoThreshold: null` を正本スキーマとする**（非該当の表現を null に固定し、実数値が入っていたら生成エラー）〕、**`keepPxCountWork` と `keepPxCount`**〔両座標系。swan は `keepPxCountWork` に対して keepRange 4,630〜5,658 の照合結果を含む〕（§0-E-8・§0-E-6） ④osanpo レシピ v2 の生成記録〔レシピ id "osanpo-colorless-20260730-v2"・rawStitch 画素数と成分数・final stitch 画素数と成分数・logo 画素数と成分数・全 range 照合結果・検査5 detect 成立値〕 ⑤swan の `BEAK_OUTLINE_REMOVAL` 記録〔beak 画素数・**`BEAK_EXPECTED_DIFF`（作業座標）と `BEAK_EXPECTED_DIFF_ASSET`（アセット座標・⑧'-1 直後）の両方**について**画素数・SHA-256・width・height**（§0-E-6 の直列化規則）・**合格条件①〜⑤の判定結果**〔真偽に加え、④の XOR 不一致画素数と⑤の部分集合違反画素数（いずれも合格時 0）〕・`aspectCorrection.applied` との整合・くちばし保持画素数・ストーン保持画素数・革面画素数・前景連結成分数〕 ⑥グレイン構成記録〔grainSourceInset・effectiveWidth／effectiveHeight・effectiveRoiSha256・shadeMin・**革面の行平均および列平均の grain 比の局所偏差の最大値**（合格閾値 ≤ 0.15）〕 ⑦**全8種の混合所有画素数**〔`masks.α≥128 ∧ masks.R≥128 ∧ (masks.G>0 ∨ masks.B>0)` — 期待値 0〕**）

## 8. フェーズ計画（改訂17 — 実装フェーズの正本は同期計画の Phase 3〜7）

実装の進行・コミット単位・handoff・開始時検証の正本は**同期計画（plans/charm-combo-codex-claude-sync-plan.md）の Phase 3〜7 と §2-4 の commit chain 契約**とする。本計画書の旧フェーズ0〜4は次の対応で吸収される（技術ゲートの中身 — §7 チェックリスト・§9 受け入れ基準 — は本計画書が引き続き正本）。

**技術値の優先関係（override map — 同期計画に残る旧色契約の再流入防止）**: 同期計画の権限は**進行機構（Phase 順序・commit chain・handoff・専用 worktree・還流票）に限定**し、**技術値（素材・色・検査・数量）は本計画書 revision 17 が優先**する。特に同期計画の次の記述は本計画書の現行契約へ読み替える（同期計画自体は Phase 1 完了時点の文書として不変のまま保存する）:

| 同期計画の記述 | 読み替え（本計画書 rev17） |
|---|---|
| §6-5 の revision lock 検証アルゴリズム（15手順）および §2-4 の成果物 path 契約が固定している `reports/charm-combo/rev14/…`（plan-review-{arch,diff,cross-check}.json・plan-review.json・revision-lock-verification.json・phase-2-handoff.json 等）と、同 §6-5 の `counts.finiteCases = 216` | **`reports/charm-combo/rev17/…` と `counts.finiteCases = 192` へ読み替える**（§12-1 手順1 が「報告先を `reports/charm-combo/revNN/` へ切り替える」と規定しており、rev14 は改訂14 当時の具体値。**path の revNN 部分と finiteCases 以外の検証手順・条件式・親子鎖の要件は一切変更しない**）。改訂17 の具体値は revNN = **rev17**・finiteCases = **192**・planRevision = **17** |
| §6-5 の manifest 必須フィールド表「planRevision \| 14」・検証アルゴリズム step 8 の抽出対象「改訂14計画とv2指示書の2文書から、改訂14、…、216有限値、…を独立抽出する」・step 10 の旧指示書失効メタデータ期待値（rev14 当時の successorPlan 等）・§2-1 の「planRevision は作成予定の14を記録する」 | **planRevision の固定値はすべて現行 17 へ読み替える**。step 8 は「**改訂17 計画と v2 指示書〔改訂17 対応〕の2文書から、改訂17・8種・2 material profile・27 leatherVariant・192有限値・81配置・CHARMS 10項目・variant 8項目・material 6項目を独立抽出する**」へ、step 10 の期待値は「**documentCommit に固定された旧指示書の現行失効メタデータ（status: superseded・successorPlan ＝ 本計画書 revision 17・successorInstructions ＝ v2 指示書〔改訂17 対応〕・lockManifest ＝ plans/charm-combo-revision-lock.json・supersededAt は記録値）との完全一致**」へ読み替える（§12-1 手順1「planRevision を必ず1増分して報告先を revNN へ切り替える」の帰結を検証手順の期待値まで貫徹するもの。**rev17 の revision lock manifest は planRevision ＝ 17・counts.finiteCases ＝ 192〔§0-A〕であり、lock 検証・revision-lock-verification・phase-2-handoff はこの読み替え後の期待値で生成・照合する** — rev15 lock〔planRevision 15〕・rev16 lock〔5cc72dd・planRevision 16〕も同一の読み替え規則で検証済みの前例） |
| §7「Phase 3で無変更を保証する固定配列`protectedFiles`は次の2点とする — index.html・bags.html」・§7-3「`protectedFiles`がphase3StartHeadとbyte単位で一致する」・§10-4「`index.html`と`bags.html`が比較基準commitからbyte不変」・§10-5「index.htmlまたはbags.htmlに差分」・§14「index.html、bags.htmlは無変更」 | **protectedFiles = [`index.html`] の1点のみ**（改訂15・§0-C-1）。`bags.html` は phase3StartHead（= selectedPhase2EvidenceCommit）の系統に存在しないため、無変更の byte SHA 証明は構造的に実行不能 — 同期計画の bags.html への言及はすべて **index.html 単独**へ読み替える（`git diff --exit-code <phase3StartHead> -- index.html` と byte SHA-256 の両方で無変更を証明）。bags.html の UI 流用は `git show b29274f:bags.html` の読み取り専用参照（§0-C-2）。**ミニバッグ実装の main 系統マージ後の再改訂で protectedFiles へ追加**する |
| 同期計画 §7〜§11 の Phase 4〜7 開始時検証・成果物要件が参照する「plans/charm-combo-display-plan.md revision 14」「revision 14計画とv2指示書」（同期計画 769・884・961・1049 行ほか各所） | **「本計画書 revision 17 と v2 指示書〔改訂17 対応〕」へ読み替える**（§12-1 手順1 の帰結 — 各 Phase の開始時検証・数量照合・成果物要件はすべて rev17 の正本 documents 3点〔lock の documents と同一 SHA〕を参照する。**例外 = Phase 1 ancestry の rev14 参照〔reports/charm-combo/rev14/ の Phase 1 証跡・phase-1-approvals.md・sourceFreezeCommit 8b3bdb4 の凍結成果物・phase-1-handoff〕は歴史的事実であり読み替えない** — Phase 1 は carry-forward・§0-E 前文） |
| §8-3「frenchie 差し替え時の referenceIvory 再算出・全 flat DISPLAY_COLORS 再導出・F-3/F-4/8 再実施」 | **発生しない**（§5-6 — referenceIvory / DISPLAY_COLORS は撤回済み。DISPLAY_VARIANTS は freeze 定数のため frenchie 再生成に色は連動しない。F-3/F-4 は改訂14の定義〔material profile 整合・DISPLAY_VARIANTS 整合〕で実施） |
| §10-3「既存24色の referenceIvory／旧 DISPLAY_COLORS 互換値」の検証 | **DISPLAY_VARIANTS（27件 approvedHex）の転記照合**（§5-6・検査F-4・`?debug=1`）へ置換 |
| §11-0「referenceIvory と DISPLAY_COLORS の正本値」参照 | **DISPLAY_VARIANTS / MATERIAL_PROFILES の正本値**（§5-6）へ置換 |
| 「216有限値」「8種×27 variant の216組」「approvedCount=216」（§2-1・§5-5 PASS 式・§6・§10・§11・**§14 最終受け入れ条件**の各所） | **192有限値・192組・approvedCount=192**（AE-20260718-10 の適用対象制限 — 同期計画 §5-1 の 8 が自ら規定する `Σ variants.appliesToCharms.length` 再計算分岐の適用結果。§0-A。Phase 7 の submissionReady / productAccepted・§11-4 final acceptance の count 判定もすべて 192 基準） |
| §5-5 色 freeze 機械 PASS 式の述語「∧ 全variantのappliesToCharmsが8key全件と一致」および同式から自動導出される `canStartPhase2`・phase-1-handoff の同項目 | **「calf 24 variant の appliesToCharms ＝ 8キー全件 ∧ chevre 3 variant の appliesToCharms ＝ 空 ∧ Σ variants.appliesToCharms.length ＝ 192」へ読み替える**（AE-20260718-10 の適用対象制限。件数の読み替え〔216 → 192・上記行〕と**同時に述語も更新する** — §5-5 自身が規定する「適用制限を採用した改訂では、件数式とPASS式を同時に更新する」の適用結果。**Phase 2 の再開ゲート・§6-5 lock 検証・phase-1-handoff 照合の canStartPhase2 判定はすべてこの読み替え後の PASS 式で評価する** — 現行 color freeze は calf 24件 ＝ 8キー全件・chevre 3件 ＝ 空であり読み替え後の式を満たすことを確認済み） |
| §6-2「mutation試験」が列挙する**5種**（Float32化・mirror誤り・loop-order変更・grain map全1・非革画素変更） | **6種へ拡張**（改訂17 — `shadeMin` 適用範囲誤りを追加。完全定義は本計画書 §5-4・根拠は §0-E-3）。同期計画側の列挙は5種のまま残るが、**技術値の正本は本計画書 rev17** であり、Phase 4 は6種すべての検出をもって検査F合格とする |
| §6-2「DISPLAY_COLORS は…referenceIvory 互換を維持しつつ、最終的には approvedHex を持つ DISPLAY_VARIANTS へ移行」 | **移行完了**（改訂14が「最終」状態 — 互換維持の中間段階は経ない） |
| §9-1「4面比較の4面目 = 新派生アセットの**シェーブル代表色**表示」・§9-2「material 切替で外形・alpha・ロゴ・保持装飾が変わらず…」の material 切替合格条件 | **4面目 = calf:black（`#41403d`）の固定表示へ置換**（chevre 適用チャーム0のため決定的に実行可能な代表暗色を採用 — appliesToCharms が非空の material が2種以上になった再改訂時に本来のシェーブル代表色表示へ戻す）。**material 切替不変性検査は単一 material の現行では N/A**（chevre 適用アイテム追加の再改訂で有効化。§9-5 の比較成果物・shape-approval.json もこの4面定義で記録する） |
| Phase 1 承認台帳（reports/charm-combo/rev14/phase-1-approvals.md）末尾の補足「チャーム表示上の DISPLAY_COLORS では改訂13 §5-6 のとおり referenceIvory（frenchie実物色基準）へ差し替えられる（COLORS 正本値とUI表示の二層構造は変更なし）」 | **失効**（当該補足は改訂13時点の説明を残した履歴記述であり実行契約ではない。台帳は Phase 1 ancestry として凍結済みのため書き換えず、本 override で機械的に失効させる。**後勝ちは AE-20260718-10〜12 と color freeze の approvedHex** — アイボリーの表示も DISPLAY_VARIANTS の approvedHex `#ece7dd` そのもの・二層構造は存在しない） |
| §6-2「prep 確定後に `fixedVisualCaseCount` とチャーム別内訳を検査レポート・**v2指示書へ**記録する」 | **検査レポート（§7 Phase 4 成果物④）と実装最終報告（v2 §9-1）への記録で代替**（v2 指示書は revision lock の documents 3点に含まれ Phase 4 時点で SHA 固定済みのため、事後追記は lock 照合を破壊し実行不能 — locked 文書への追記は行わない） |

| 同期計画 Phase | 旧フェーズ対応 | 内容（進行・handoff 詳細は同期計画の当該節） |
|---|---|---|
| Phase 3 ツール同期 | （新設） | `tools/charm-prep.html` を改訂17契約へ更新: INPUT_MANIFEST 8種（§4-1c）・COLOR_REFERENCE_MANIFEST・LEATHER_VARIANTS 27・MATERIAL_PROFILES 2・**schemaVersion 6**〔§0-E-9〕・**toolVersion 0.8.0**・swan の CHARM_SPECS 登録・改訂16分（frenchie cropSource 定数〔§0-D-2〕・osanpo レシピ・sizeMmMeasured 8種 true〔§0-D-1〕・763cf1e の swan ステッチ判定 UI／selftest 復元堅牢化）に加え、**改訂17分 = `COLORLESS_CLOSED_WHITE` レシピ新設と6種への適用〔§0-E-1〕・`grainSourceInset: 2`〔§0-E-2〕・⑦' の `shadeMin: 0.78`〔§0-E-3〕・osanpo レシピ v2〔§0-E-4〕・horseshoe の `outlineCaptureRadius: 12`／`outlineLumMax: 0.75`〔§0-E-5〕・swan の `BEAK_OUTLINE_REMOVAL` と `BEAK_EXPECTED_DIFF`〔§0-E-6〕・cat／swan の `logoRoi` と全8種の `logoRange`・`detectLogo` の keep 除外〔§0-E-8〕・新規検査4種〔混合所有画素数 0・外周取り残し率 ≤10.0%・grain 比局所偏差 ≤0.15・全8種の logoRange 照合〕**。selftest=1 全通過 → toolCommit（同期計画 §7） |
| Phase 4 全8種再生成 | 旧フェーズ0 | canonicalSource 8点から派生アセット一式を再生成（§5-4 検査1〜8・M・F・straight PNG・独立 F oracle・mutation・6ストーン位相・§7 チェックリスト・**幾何パラメータ固定 → 81配置3条件検査〔§7-11〕**・fixedVisualCaseCount の確定）。**§7 の「免除不可」項目は8種すべて合格が必須・swan の stitchMode をここで確定** → assetCommit（同期計画 §8） |
| Phase 5 形状・意匠QA | 旧フェーズ0 承認ゲート | canonicalReference との4面比較・形状/意匠の承認（色承認と分離・**swan ロゴ立体陰影の見た目の最終判断を含む** — AE-20260718-07）。素材検証レポート8種構成の承認 → shapeApprovalCommit（同期計画 §9） |
| Phase 6 charms.html 完成 | 旧フェーズ1〜3 | 代表版 charms.html を8種＋なしへ拡張: variant/material 2段UI・DISPLAY_VARIANTS・cache key（§5-7）・世代管理・サマリー・§9 受け入れ基準 0〜13 → charmsCommit（同期計画 §10）。**完了条件は §0-E-11 の5条件を機械確認すること — 判定対象は Phase 4 で確定した `CHARM_SPECS[key].stitchMode` から動的に決める: ①全8種の革リカラー差分 > 0 ②全8種のロゴ金銀差分 > 0 ③`stitchMode ∈ {"detect", "fallback"}` の全チャームでステッチ白黒差分 > 0 ④`stitchMode = "none"` の全チャームは UI が `disabled` かつ差分 = 0 ⑤③と④の対象チャームの和が8種ちょうどで、`stitchMode` が3値のいずれでもない（null を含む）チャームが存在しないこと**。テイスト調整で LAYOUT・丸カンアセット・N を変更した場合は §7-11 の81配置検査を最初から再通過（§5-5） |
| Phase 7 色クライアント確認 | （新設） | 8種 × 適用可能 variant = **192組**（§0-A）の確認シートを提出（同期計画 §11） |
| （別工程） | 旧フェーズ4 | `index.html` への逆リンク追加はメインセッション担当（実装エージェントは行わない） |

**読み替え規則**: 本計画書の他節に残る「フェーズ0」「フェーズ1」「フェーズ2」「フェーズ3」「フェーズ4」の呼称は上表の対応で同期計画 Phase へ読み替える（フェーズ0 → Phase 4／フェーズ1〜3 → Phase 6／フェーズ4 → 別工程。文中の「フェーズ0停止」「フェーズ0不合格」等のゲート表現は Phase 4 のゲートを指す）。

- 実装委任時は **v2 実装指示書（plans/impl-instructions-charm-combo-display-v2.md）**の規約（ブランチ・停止チェックポイント・日本語報告・同期計画 §2-4 の commit chain）に従う。旧指示書（plans/impl-instructions-charm-combo-display.md）は SUPERSEDED — 実行禁止
- **改訂14に伴う再実施の総括〔改訂14 当時の非規範的な履歴 — 本項の「schemaVersion 5」を現行指示として解釈してはならない。現行の schemaVersion は §0-E-9 の 6 が正〕**: 改訂13 §8 の「photo 6種は画像の再生成不要」条項は**撤回**（§0-B-3）。全8種を schemaVersion 5〔当時の値〕・straight PNG 契約で再生成し、旧派生アセット・旧 prep.json・旧 manual レイヤ（旧素材の作業座標系由来）は流用しない。81配置3条件検査・§9-2 実寸比率検証・素材検証レポートも8種構成で新規取得する〔**再実施の要求自体は改訂17 の §0-E-11「Phase 4 は全8種を再生成」（schemaVersion 6・§0-E-9）が現行正本として引き継いでいる**〕
- 計画外の変更が必要になった場合は **Codex 変更還流票（同期計画 §12）**を提出して停止し、Fable 5 の再改訂・再ロック（同 §12-1・§12-2 の restartPhase 判定）を経てから再開する

## 9. 受け入れ基準

0. **派生アセット検査**: 8種すべてが §5-4 の書き出し前検査1〜7・M・F＋コミット前検査8（再生成一致・straight PNG 独立デコード・独立 F oracle・**mutation 6種**〔改訂17 — ⑥`shadeMin` 適用範囲誤りを追加・§5-4〕）に通過している（Phase 4 成果の再確認）。**CHARMS テーブル値と prep.json の転記一致（§5-2 の10項目の完全列挙）、LEATHER_VARIANTS 転記8項目・MATERIAL_PROFILES 転記6項目と color freeze の一致を `?debug=1` の機械照合で確認**（§0-A）
1. **81通り配置スモーク**: 左右 9×9 全組み合わせ（同一チャーム同士・なし×なし含む）がエラーなく表示され、**3条件（クリップなし・重なりなし・負の余白なし・§5-5）を満たす**。horse×horse（革幅ベース最大198mm）と **swan×swan（前景高最大・ストア記載90mm）** を必須確認
2. **実寸比率**: チャーム同士の大きさ比率が実寸どおり（例: horse幅:horseshoe幅 ≒ 95:46。debug グリッドで検証）。**斜め素材（osanpo）は軸平行bboxではなく校正線基準で照合**（§5-2 calibration）
3. **固定目視回帰セット（片側表示・ケース数は swan の stitchMode 確定で決まる — Phase 4 で `fixedVisualCaseCount` とチャーム別内訳を検査レポートへ記録）**: ベースラインは既存 material（calf）の2 variant = **calf:black と calf:ivory**。各チャームのケース数は次式で機械算出する: `2 baselineVariants × (hasLogo ? 2 logoColors : 1) × (stitchMode === "none" ? 1 : 2 stitchColors)`。全8種ロゴあり確定（AE-20260718-07）のため、既知7種 = ロゴ有・stitch なし5種×4件＋ロゴ有・stitch 有2種×8件 = **36件**。swan は **stitch なしなら合計40・stitch 有なら合計44**（stitchMode は Phase 4 判定・§4-1c）。**ロゴ有無または stitchMode が未確定なら Phase 4 を完了しない**。マスク品質・白飛び・ステッチ視認性（fallback 採用時は**元糸の残像なし**を含む）・**ロゴのロゴカラー連動・ラインストーン等の保持画素が全ケースで元色のまま不変であること**（§4-1c・§5-6）、**アスペクト補正適用チャームに補正起因の不自然な伸縮・楕円化・ぼけ・色ハロがないこと**（§5-2・§7-8）、**グレインの継ぎ目・タイル反復・色域外圧縮起因の色調破綻・素材元色の混入がないこと（§7-8 の variant 色回帰の Phase 6 再実施を含む・§5-2・§5-6）**を目視確認（丸カンの銀金は §9-8 で別途確認 — チャームビットマップに影響しないため本セットの次元に含めない）
4. **応答性能**: §5-7 の計測方法で中央値 ≤300ms（キャッシュ済み切替）、初回選択（warm cache・相異なる2チャーム同時ロード）≤1500ms＋ローディング表示
5. **サマリー一致**: 左右独立カラー・【なし】各状態でサマリー文・コピー文面が表示と完全一致。検証マトリクスは**ステッチ有無 × 表示数の5形態**（2点ステッチ有含む／2点ステッチ有なし／1点ステッチ有／1点ステッチ無／0点。§6-3）。ロゴ表記の有無（1点以上で表示・0点で非表示）も各形態で確認
6. **【なし】挙動とトグル有効条件**: §4-2 の表のとおり（センター寄せ・丸カンのみ・スウォッチ無効化）。**ステッチトグルが「ステッチ有チャーム表示中のみ有効」、ロゴトグルが「チャーム1点以上表示中のみ有効」の条件どおり切り替わる**こと
7. **輪郭品質**: 暗色革（calf:black `#41403d` — approvedHex・§5-6）選択時・暗色背景 debug 表示で、輪郭に白ハロ・フチ欠けがない
8. **丸カン切替と金具／ロゴの非干渉**: 銀⇔金の切替で丸カンの位置・サイズが変わらない。さらに代表チャーム（horseshoe 推奨 — ロゴ・ラインストーン・ステッチを全て含む）で**金具{銀, 金} × ロゴ{金, 銀}の4組**を検証し、①金具変更時は**丸カンのみ**が変化しチャームビットマップは再利用される（キャッシュキー不変・再リカラーなし・ロゴ不変） ②ロゴ変更時は**チャームのロゴのみ**が変化し丸カンは不変 ③各組でサマリー・コピー文面が表示と一致、を確認する
9. **連続操作**: 素早い連続切替で描画結果が最新の選択状態と一致する（世代トークン。切替対象には variant 切替・【なし】を含める）。アセットロード失敗時は既存 `index.html` 同様のエラーメッセージを表示する
10. **レスポンシブ**: スマホ（375px幅）で操作・表示が崩れない
11. **既存無影響**: `index.html` に変更がない（フェーズ4のナビ追加時は追加リンク以外の差分ゼロ）
12. **メモリ挙動**: 完成ビットマップの9件目生成時に LRU 退避→`ImageBitmap.close()` 実行→退避済みエントリの再選択で正しく再生成されること（swan を含む相異なる9キー以上で確認）。モバイルSafari 実機でタブ再読込が発生しないこと
13. **革トーン有限値検査（192有限値・§0-A）**: **8種 × 当該チャームへ適用可能な variant（appliesToCharms 基準・現行 calf 24）= 192組**の全列挙で、flatLeatherTone（**DISPLAY_VARIANTS の approvedHex を入力**・各 variant の正しい material profile の base/baseLum で評価）の出力RGBの全成分が有限値（NaN/Infinity なし）であること。material profile 分岐・色域外圧縮・texture 適用も**各 variant の正しい profile で検査**する。**さらに flatLeatherTone は実チャーム設定に依存しない合成入力の分岐強制単体試験を行う**（実データの baseLum では ratio が上限に届かないことがある — 例: lum=255 でも ratio = 1/baseLum は baseLum > 0.625 なら 1.60 未満）: ①**ratio 上限** — base=(255,255,255)・baseLum=0.5（合成値）で ratio=2.0 → 1.60 クランプが実行される ②**ratio 下限** — base=(0,0,0)・baseLum=0.5 で ratio → 0.35 クランプが実行される ③**色域外圧縮** — 27 variant 中の最大彩度色（chevre:green `#00894c` を含む）× ratio=1.60 相当の合成入力で labToSrgb が [0,255] 外となり、**彩度Cの二分探索（固定20回・§5-6）が実際に実行される**こと（分岐実行の確認は実行カウンタ等の決定的手段による）。①〜③すべてで出力が有限かつ [0,255] の整数であること（Phase 6 の必須単体検査として実施し、最終報告で再確認。photoLeatherTone の有限値単体試験は予備契約として維持 — 適用チャーム0）

## 10. リスクと対策

| リスク | 対策 |
|---|---|
| 写真の撮影条件バラつき（スケール・照明・背景） | スケールは実寸mm基準の等方正規化＋3%歪み検査（§5-2。超過時は実測寸法を正とするアスペクト補正で矯正）。照明は baseLum 正規化。背景・マスク品質は**準備工程の手動補正で人が確定**してからコミット（実行時リスクなし） |
| 輪郭の白ハロ（白背景の色かぶり・フェザー帯の元色残り） | defringe＋「フェザー帯も所有を持つ」恒等式（§5-2）を契約化し、暗色背景合成の検証を準備ツールと受け入れ基準（§9-7）の両方に置く |
| ステッチのコントラスト不足でマスク化不能 | 全8種 colorless（白ステッチ×白革面）のため自動検出は不成立見込みだが、manual-owner の手動マスク化で detect 成立（§5-6。39fe815 の horseshoe では seed ベースの決定的レシピで成立済み）。swan は Phase 4 判定。手動でもマスク化不能なら stitchMode=fallback（stitchPaths契約・§5-2）に切替 |
| 保持画素（ラインストーン等）の視覚品質 | 保持は素材の色をそのまま表示するため革色変更時に浮いて見える可能性 → Phase 4 の §7-8 目視（horseshoe 8ケース: 革色を変えてもストーンが自然か。swan は beak・stone の2成分〔§0-E-6〕も同様）で確認し、素材検証レポートに記録 |
| 保持チャンネルの α=0 画素で masks.RGB が PNG デコード時に壊れる | 保持判定は masks.α のみで行い保持画素の RGB は読まない設計（§5-2）。恒等式検査1も保持画素の RGB を不問とし、**baseLum・bboxPx・ステッチ集計・クリップ等の全所有権参照は共通述語 `masks.α≥128` で保持画素を除外**（§5-2 — ラインストーンが革コア・革bbox・ステッチ所有に誤算入されない） |
| 箔押しロゴ「&.」の塗り分け漏れ（小面積） | 準備ツールの拡大表示＋manual-owner 手動補正で確定。漏れると革リカラー時にロゴが革色に染まるため、固定検証セット目視（§9-3）の確認項目に含める |
| 濃色リカラーの白飛び | colorless の flatLeatherTone はハイライト保護なし（白面基準・§5-6）で構成的に回避。photoLeatherTone のハイライト保護は将来 photo 素材用の予備契約 |
| 金箔ロゴ・ラインストーンの彩度が革と近く自動分類しにくい | 準備工程の手動ブラシ補正（manual-owner / manual-keep）で確定（自動分類の精度に依存しない）。それでも塗り分け不能なら素材再提供の依頼（除去はしない・§5-6） |
| 実寸データ・校正の誤り | 実寸はストア商品ページ記載で確定済み・全8種が実測値（7種 2026-07-13・swan 2026-07-28〔§0-D-1〕）とも完全一致（§4-1）。素材側の校正は calibration 契約（斜め素材 osanpo は手動計測線モード必須・§5-2）＋pxPerMm歪み検査。誤りが見つかった場合の修正は**準備ツールの入力（sizeMm・sizeMmMeasured・calibration）と prep.json 側で行い、派生アセット再生成＋検査1〜8 再実行 → CHARMS の転記照合10項目を再転記して `?debug=1` 照合 → §7-11 の81配置・§9-2 の実寸比率を再検証（§5-5）**という経路のみ（**CHARMS 単独修正は禁止** — 正本 prep.json との一致契約を破るため） |
| 写真の投影が実物の縦横比と乖離（置き方・革の柔らかさ由来。フェーズ0で実測確定済み5種が歪みゲート超過 — §4-1） | **実測寸法を正とするアスペクト補正（非等方・縮小のみ・§5-2 改訂10）**で矯正し、補正後の歪み再計測（検査3）で機械確認。テクスチャ伸縮は最大10%で単体表示では知覚困難（§7-8・§9-3 の目視で確認）。manual-line 素材（osanpo）で超過が出た場合は個別設計（§7-9 対処③） |
| アスペクト補正の bilinear が α 境界を跨いで RGB を混ぜ、④の defringe 性が補正後画像で破れる（フェーズ0実績・2026-07-14: 補正適用5種が検査4不合格 0.73〜19.2% — 基準0.5%） | **⑧' の二段階契約（⑧'-1 幾何変換 → ⑧'-2 補正後α基準の再defringe・④と同一規則〔§5-2 改訂12〕）**で defringe 性を補正後画像に再確立し、検査4（0.5% 基準不変・最終画像で判定）で機械確認。base.α・masks は ⑧'-2 で不変のため恒等式・保持二値性への影響なし |
| 吊り金具の写り込みが素材間で不統一 | **解消済み**: canonicalSource は全種吊り金具なしで統一（§4-1c）。丸カンは非接続の独立表示（§5-5・§11-8）で全チャーム一律 |
| 前景の張り出しでステージからはみ出す | 前景（fgBbox）と丸カン描画境界ベースの81配置3条件検査（§5-5・§7-11）で機械的に検出し、ステージ寸法を更新 |
| 派生アセットの再生成不能（手動補正の属人化） | prep.json＋manual-*.png の版付きスキーマと決定的パイプライン（§5-4）で全状態を保存し、準備ツールの読込・SHA照合・再実行機能＋再生成一致検査（コミット前検査8）で機械的に担保 |
| fallback元糸の残像（革リカラー後に糸の筋が残る） | 準備工程の manual-inpaint による元糸RGB中和＋白黒双方の残像なし必須検査（§5-2・§7-7） |
| 準備ツールの実装コスト | 既存 `extractKeyRingAsset()`・ステッチ検出の流用で初期実装を短縮。ツール自体は使い捨てでなく将来のチャーム追加にも使う資産になる |
| グレイン品質（タイル継ぎ目・反復パターンの視認・シボの不自然さ） | ミラータイル＋局所平均除算のハイパスで大域ムラ・色を機械的に排除し、grainScaleMm を全チャーム・全 material 共通にして質感の実寸スケールを統一（§5-2）。§7-8 の variant 色回帰と §9-3 で目視確認し、パラメータは prep.json 記録で決定化（検査F-2・検査8）。焼き込みの数値正しさは独立 F oracle＋mutation 試験（§5-4）で機械担保 |
| 白革面×白ステッチで自動ステッチ検出が不成立 | detect は手動マスク化（manual-owner による B 所有指定）を許容（§5-6 — マスクの由来は実行時挙動・検査5の条件に影響しない） |
| approvedHex と実物の乖離 | approvedHex は実物カラーチャートからの抽出値（AE-20260718-11/12・Phase 1 で比較シート承認済み）。最終確認は Phase 7 のクライアント確認（192組）で行い、修正依頼は同期計画 §12 の還流票・restartPhase 判定を通す |
| material 間の質感差（calf / chevre）が将来アイテムで不自然になる | grain 比較シート（Phase 1 grain-sheet・AE-20260718-13）で同一条件比較を承認済み。chevre 適用アイテム追加時は §9-3 の固定目視回帰セットと variant 色回帰を chevre 構成で再実施 |
| swan の実測寸法が未受領のまま歪み>3% になる | **解消済み（改訂16・§0-D-1）**: Phase 4 バッチで本リスクが顕在化（preDistortion 7.081% → 契約どおり停止）し、実測 W90×H90（2026-07-28 受領・ストア記載と完全一致）で解消。confirmed=true によりアスペクト補正が自動発動し矯正する。〔停止契約自体は将来素材用に §5-2・§7-9① へ温存〕 |

## 11. 確認ポイント（レビューのお願い）

**1〜5・7〜11 は回答受領済み（2026-07-13）、12 は承認済み（2026-07-15）、13（swan）は改訂14で解消（Phase 1 承認 AE-20260718-07/08/09。④実測値は改訂16で受領・確定〔2026-07-28・§0-D-1〕 — ⑤stitchMode のみ Phase 4 へ持ち越し）、14（Phase 1 の素材・色凍結）は 2026-07-18 承認済み**。6（左右同一チャーム可）のみ明示回答なし＝既定案（選択可能）のまま進行（異議があれば実装フェーズ開始前までに）。

1. **【なし】1点時のセンター寄せ**: ✅ **確定（2026-07-13）** — 真下センターへ移動する仕様（§4-2）
2. **チャームの向き**: ✅ **確定（2026-07-13）** — 左右とも写真のまま（動物系は頭が左向き・horseshoe は正立U字・osanpo は斜め配置。§4-1b。反転しない）
3. **ステッチ初期値**: ✅ **ホワイトで確定**（ステッチ有2種〔horseshoe / osanpo〕の商品ページに「全カラー白ステッチ」標準と明記・§2）。既存ドーナツ版はブラック初期のためページ間で初期値が異なる（承知の上）
4. **チャーム選択UI**: ✅ **プルダウンで確定（2026-07-13）**
5. **初期表示の組み合わせ**: ✅ **「左 horse × 右 horseshoe」で確定（2026-07-13）**（ステッチ有チャームを含むため、初期状態で全トグルが有効になりデモとしても好都合）
6. **左右同一チャーム**: 「選択可能」を仕様とした（§4-1。ステージ寸法も同一チャーム前提で計算済み）。明示回答なし＝既定のまま進行。不可に変える場合もステージ設計はそのまま成立する
7. **生成素材**: ✅ **全8種の canonicalSource が Phase 1 で凍結済み（sourceFreezeCommit `8b3bdb4`・§4-1c・AE-20260718-01/09）**。公開リポジトリ上での公開可否も含めて素材提供済みの前提で進行（grain 2点・canonicalReference・canonicalColorChart を含む。懸念が出た場合のみ再相談）
8. **丸カンとチャームの連結表現**: ✅ **確定（2026-07-13）** — **「既存 `index.html` と同様に、丸カンは中央上部に独立して表示」（非接続レイアウト）**。丸カンとチャームは重ねず、接続パーツも描かない。接続検査は不要（§5-5）。旧・案A（2mm重なり）/案B（しずくコネクタ）は不採用
9. **箔押しロゴ「&.」の扱い**: ✅ **確定（2026-07-13）** — 金具トグル連動ではなく、**ロゴカラー（ゴールド/シルバー）の独立トグル**を新設して切替（§5-6。masks.G をロゴ専用とし metalTone をロゴカラーに連動）
10. **ラインストーン（horseshoe）の扱い**: ✅ **確定（2026-07-13）** — **リカラー対象外（保持）**。素材の色をそのまま表示する保持チャンネルを導入（§5-2・§5-6。改訂13: 無着色素材でも同じ扱い — 6個のラインストーンと台座を保持・§4-1c）
11. **UI表示名**: ✅ **全チャームとも画像ファイル名と同じ英語表記で確定（2026-07-13）** — horse / horseshoe / frenchie / dachshund / toy poodle / osanpo / cat に **swan を追加（改訂13・既存契約に従い英語表記 `swan`）**（§4-1）
12. **horseshoe の無着色素材への切替と表示方式**: ✅ **承認済み（2026-07-15）** — 生成入力を `horseshoe色透過.png` へ切替（§4-1c）。表示方式: 質感.jpg は色を捨て局所的な凹凸だけを利用／Lab 空間で質感合成／sRGB 色域外は明度と色相を保ち彩度だけを必要最小限圧縮／旧ブラック写真の色・明暗は一切使用しない／上下配置はチャームごとに余白を均等化（コンテンツ縦センタリング・§5-5）。〔当時の色基準「アイボリー = frenchie 実物色・他色 = COLORS」は改訂14で DISPLAY_VARIANTS（approvedHex）へ置換 — §5-6・§0-B-4〕
13. **swan の正式対象化と未確定事項**: ✅ **改訂14でほぼ解消**（対象化は 2026-07-15 指示受領・§4-1）:
    - ①素材: ✅ **swan-colorless.png（嘴カラーあり版）で正本確定（AE-20260718-09・§4-1c）**
    - ②くちばし: ✅ **アクセント色として保持で確定（AE-20260718-08）**
    - ③ロゴ「&.」: ✅ **あり・立体シルバー表現のまま採用で確定（AE-20260718-07 — ロゴなし時の停止ゲートは不要となった。立体陰影の見た目の最終判断のみ Phase 5 QA）**
    - ④実寸の実測値: ✅ **受領・確定（改訂16・2026-07-28）** — クライアント実測 W90×H90 がストア記載 90×90 と完全一致（§0-D-1・§4-1。confirmed=true → Phase 4 実測 preDistortion 7.081% に対しアスペクト補正が自動発動）
    - ⑤stitchMode: **Phase 4 で実画素から §5-2 完全列挙で判定**（detect / fallback なら UI 注記・§9-3 ケース数 44 へ更新、none なら 40。Phase 4 バッチ実測では detect 成立 — B所有 1,106px・白黒差分 1,106px。最終確定は新 toolCommit での Phase 4 再実行時・§0-D-7）
14. **Phase 1 の素材・色凍結**: ✅ **承認済み（2026-07-18・AE-20260718-01〜14）** — pipelineSource=フチ画像×8・approvalReference=フチ画像兼用・カーフ24 approvedHex=チャート抽出値・シェーブル3=P3変換値・シェーブル専用 grain=シェーブル質感.jpg・カーフ grain=質感.jpg 再利用・シェーブル3 variant は現行チャームで選択不可（192有限値）・シエルブルー canonicalLabel。詳細は reports/charm-combo/rev14/phase-1-approvals.md

## 12. 承認後の流れ

1. ~~本計画書を `/codex-review` で反復レビューし収束させる~~ **改訂7まで済（改訂5: 反復5回／改訂6: 反復4回／改訂7: 反復4回 — いずれも `ok: true` 収束・2026-07-12〜13）**。**改訂14は同期計画 §6-5 の revision lock 手続き（arch / diff / cross-check の3フェーズレビュー・全 ok:true・lock manifest）で確定済（2026-07-19・selectedLockCommit `2dc9420`）。改訂15は同 §12-1 の再改訂・再ロック手順で確定済（2026-07-22・selectedLockCommit `3fda5bf`・supersedesLockCommit=`2dc9420`）。改訂16は同 §12-1 の再改訂・再ロック手順（同じ3フェーズレビュー・全 ok:true・新 lock manifest・supersedesLockCommit=`3fda5bf`・restartAnchorCommit=`e71507d`〔rev15 phase-3 evidence〕）で確定済（2026-07-28・selectedLockCommit `5cc72dd`）**。
   **改訂17は同 §12-1 の再改訂・再ロック手順で確定する** — restartAnchorCommit = `923e2094fe5de1da1f433a3ad49b2a45c4dc743e`（Codex 変更還流票 20260730T081002Z-osanpo-asset-fix-01 の changeTicketCommit・受領検証 28項目 ALL PASS）・supersedesLockCommit = `5cc72dd`・restartPhase = Phase 2・sourceFreezeCommit `8b3bdb4` は carry-forward で不変（§0-E 前文・§12-2 の複合分類）
2. ~~素材の凍結~~ **済（Phase 1・2026-07-19 未明・sourceFreezeCommit `8b3bdb4`＋phase-1 evidence `e76f6e8`・§4-1c）**
3. ~~ナミさんの確認・修正反映~~ **§11 の 1〜5・7〜14 は回答受領・反映済み**（残: 6 は既定案進行・13⑤ stitchMode は Phase 4 以降。13④ 実測値は改訂16で受領済み・§0-D-1）
4. ~~実装指示書の作成~~ **v2 実装指示書（plans/impl-instructions-charm-combo-display-v2.md）を改訂14と同時に作成**。旧指示書は SUPERSEDED（DO NOT EXECUTE）
5. revision lock 成立後、実装委任 → **同期計画 Phase 3（ツール同期）から順に**確認サイクル（§8）
6. **改訂17の取り込み**: 実装エージェント（Codex）は selectedPhase2EvidenceCommit を起点とする専用 worktree（同期計画 §7-0・ブランチ `feature/charm-combo-codex-rev17`）で作業し、Phase 3 の開始条件（revision lock 照合 PASS・dirty worktree 保全・同期計画 §12-3 の再開ゲート6条件）を満たしてから着手する。v2 指示書と本計画書が矛盾する場合は**本計画書（改訂17）を優先**し、矛盾内容を報告する
