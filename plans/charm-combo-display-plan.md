# チャーム組み合わせ表示システム 実装計画書

- 作成日: 2026-07-12（**改訂14: Phase 1 凍結素材への全面切替・material-aware 27 leatherVariant・192有限値・Codex実装還流（2026-07-19）** — 変更点の完全一覧は §0-B・数量契約は §0-A。正本入力は sourceFreezeCommit `8b3bdb41bd3205287f2612ca9fb071b4d53e8376` の plans/charm-approved-source-freeze.json / plans/charm-approved-color-freeze.json。〔注記: 以下の改訂13以前の履歴に現れる「受領待ち」「photo 6種」「再生成不要」「COLORS / DISPLAY_COLORS / referenceIvory」「216」等の状態・契約はすべて過去の経緯の記録であり、現行契約は改訂14の本文（§0-A・§0-B・§4-1c・§5-2・§5-4・§5-6）を正とする〕。改訂13: **無着色素材モード（sourceMode="colorless" / toneMode="flat"）の導入・horseshoe 生成入力の切替・swan の正式対象化（2026-07-15）** — ①horseshoe の生成入力を旧黒革写真から**無着色デザイン素材** `charm一覧/horseshoe色透過.png`〔2020×2385px・RGB・αなし・SHA-256 は §4-1c に記録〕へ切替（Git管理名 `source-photos/horseshoe-colorless.png`・旧 horseshoe.jpg は参考保管のみ＝生成入力への使用禁止）。②ユーザー承認済みの表示方式（2026-07-15）を契約化: 質感.jpg は**色を捨て局所的な凹凸のみ**使用〔グレイン焼き込み⑦'・§5-2〕／**アイボリー= frenchie 実測 referenceIvory・他23色= COLORS 基準**の表示革色 DISPLAY_COLORS〔§5-6〕／**Lab 空間で質感を合成し sRGB 色域外は明度・色相を保ち彩度のみ必要最小限圧縮**〔flatLeatherTone・§5-6〕／**旧写真の色・明暗は一切使用しない**／上下配置は**丸カン＋チャーム前景のコンテンツ縦センタリング（チャームごとの余白均等化）**へ変更〔§5-5〕。③**swan を「保管のみ」から正式な表示・選択対象へ**（チャーム8種＋なし＝左右各9択・§4-1）。swan の無着色素材 `swan色透過.png` は**受領待ち**であり、解像度・入力ハッシュ・所有領域・校正値・baseLum 等は**受領後にのみ確定**（仮確定禁止・§4-1c）。**くちばしの所有（革連動 or 保持）は §11-13 のユーザー確認事項**（実画素確認でも一意に決まらない場合はフェーズ0停止）。④件数依存の更新: 派生アセット・CHARMS・prep.json 一式 7→8種／有限値検査 168→**192組**〔§9-13〕／配置検査 64→**81配置**〔クリップ＋重なり＋負余白の3条件・§5-5〕／§9-3 固定検証セットは swan の stitchMode 確定後に 40 または 44 ケース。⑤prep.json **schemaVersion 4**〔sourceMode・grain（軸別 grainScaleX/Y・pxPerMmWorkW/H・flatWhite 含む）・referenceIvory・displayColors・calibration.reason を追加。photo 6種もフィールド追記のうえ v4 へ更新〕・書き出し前**検査M〔全チャーム共通 — §4-1c 正規入力マニフェスト照合（旧 horseshoe.jpg / swan.jpg は禁止入力）＋sourceMode⇔toneMode 対応〕・検査F〔colorless 専用〕**新設・CHARMS 転記照合 **10項目**化〔toneMode 追加〕。⑧' アスペクト補正は colorless のみ base.RGB=nearest（無彩色性の維持）とし、photo の bilinear 等の座標系契約は不変。改訂12: **⑧'アスペクト補正の二段階化〔⑧'-1 幾何変換＋⑧'-2 再defringe〕（2026-07-14）** — 改訂10/11 取り込み後のフェーズ0生成で、⑧'幾何補正と検査1・2・3・5・6・7 は全7種合格したが、**補正適用5種が検査4〔エッジ帯 defringe〕に不合格**（d>32 画素率: horseshoe 9.43% / frenchie 0.726% / dachshund 19.23% / toy poodle 10.86% / cat 14.13%・基準 0.5% 未満）。原因は base.RGB の bilinear が α 境界を跨いで RGB を混ぜ、④の defringe 性が補正後画像で破れるため（最近傍算出の厳密化では解消しないことを実装報告で確認済み）。対処: **⑧' を「⑧'-1 暫定 bilinear／nearest 変換 → ⑧'-2 補正後α基準の再defringe〔④と同一規則・同一実装〕」の二段階として §5-2 に契約化**し、優先関係（α=255 画素は bilinear 値が最終・α<255 画素は再defringe 値が最終）・処理順（⑧'-2 完了後に派生値再算出→検査1〜7）・検査4 の判定対象（最終画像・0.5% 基準は不変）・検査8 と期待出力ハッシュの対象（最終画像）・schemaVersion 増分（生成規則の変更も増分対象）を明記。数値・prep.json フィールド構成・UI仕様への影響なし。改訂11: **horse / osanpo の実測寸法を受領（2026-07-13・horse W95×H70 / osanpo W92×H40）— §4-1 の確定値とmm単位で完全一致し、全7種の実寸が実測で確定**。sizeMmMeasured.confirmed を全7種 true に更新（§5-2 発動述語・prep.json 入力値）。数値変更なしのため校正・配置・受け入れ基準への影響なし。horse は実測比 0.3〜0.5% 歪みのため補正発動なし見込み・osanpo は manual-line のため shouldApply 対象外のまま（超過時は停止 → 個別設計）。改訂10: **アスペクト補正〔非等方・縮小のみ〕の導入（2026-07-13）** — フェーズ0の pxPerMm 歪み検査で実測確定済み5種が全てゲート3%超過〔horseshoe 8.7% / frenchie 4.0% / dachshund 6.1% / toy poodle 10.4% / cat 3.4%。Codex 計測＋メインセッション独立検算で一致確認〕。同環境の horse は 0.3〜0.5% で合格・革端マスクは目視で正確なため、撮影系の系統誤差ではなく置き方・革の柔らかさ由来の投影乖離と判断し、**実測寸法（改訂9で5種完全一致）を正とする矯正を §5-2 に契約化**（発動述語 shouldApply・縮小のみの補正式・離散変換の一意定義・作業/アセット座標系の命名・prep.json 記録・検査3/8 連動・§7-9 対処順路の更新・§5-5/§7-11/§9-2 の再検証経路）。sizeMm・ステージ寸法・配置式・受け入れ基準の数値は不変。horse / osanpo は実測依頼中〔2026-07-13〕。改訂9: **実装エージェント（Codex）フェーズ0報告の実測寸法〔2026-07-13 受領・5種〕を照合・記録** — horseshoe 46×54 / frenchie 60×75 / dachshund 90×60 / toy poodle 80×60 / cat 68×65 はいずれも §4-1 の確定値と**mm単位で完全一致（数値変更なし）**。horse / osanpo の2種は実測未受領のためストア記載ベースを維持（§4-1）。数値が不変のため、寸法連動の校正（§5-2）・配置式とステージ寸法（§5-5）・受け入れ基準（§9-1/9-2）はいずれも影響なし — 出典の裏付け記録のみの改訂で設計契約に変更なし。改訂8: §11-4/5/11 の回答〔2026-07-13〕反映 — 選択UI=プルダウン・初期表示=左horse×右horseshoe・UI表示名=画像ファイル名と同じ英語表記。改訂7までの設計は Codexレビュー改訂5=5回・改訂6=4回・改訂7=4回でいずれも `ok: true` 収束済み）
- 対象: クライアント要望①「チャーム組み合わせ」（優先度: 最高）
- 方式: **実物写真リカラー方式 × 派生アセット事前生成**（2026-07-12 ユーザー決定＋レビュー反映）
- 成果物の流れ: **本計画書のレビュー → 修正反映 → 実装指示書作成 → Opus 4.8 / Codex へ実装委任**（Fable 5 では実装しない・2026-07-12 決定の体制を踏襲）

---

## 0-A. 数量契約（改訂14 正本値 — revision lock manifest の counts と一致必須）

本節の表は改訂14の数量契約の正本であり、実装指示書 v2（plans/impl-instructions-charm-combo-display-v2.md）の同名テーブル・plans/charm-combo-revision-lock.json の `counts` と機械照合される（同期計画 §6-5 検証手順8〜9）。

<!-- COUNTS-CONTRACT-BEGIN -->
| countKey | 値 |
|---|---:|
| planRevision | 14 |
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
6. **Codex 実装（コミット 39fe815）からの技術還流を契約化**（§5-4）: straight RGBA PNG エンコーダー＋独立最終デコード検査／独立 F oracle＋mutation 試験5種／horseshoe 6ストーン位相検査（検査6）／production・oracle Worker の世代管理（中断・timeout・cleanup）。改訂12/13 の二段階アスペクト補正（⑧'-1/⑧'-2）と再defringe は §5-2 の本文どおり新版へ引き継ぐ
7. **schemaVersion 4 → 5・toolVersion 0.5.0 → 0.6.0**（全8種 colorless 化・material-aware 化・straight PNG 契約化による生成規則変更のため増分）
8. **cache key へ variant/material を追加**: 完成ビットマップの cache key は `charmKey|variantKey|materialKey|logo|stitch`（丸カン金具は従来どおり含めない・§5-7）
9. **革種→色の2段選択 UI**: 各サイドの state は variantKey を持ち、material を選ぶとその material に属する色だけを表示する。**選択可能 material が2種以上のときのみ革種選択 UI を表示**（現行8チャームは calf のみ＝革種 UI 非表示でカーフ24色のみ表示・AE-20260718-10・§6-1）
10. **実装フェーズの正本を同期計画へ接続**: 実装は同期計画（plans/charm-combo-codex-claude-sync-plan.md）の Phase 3〜7 に従う（§8）。計画外変更は Codex 変更還流票（同期計画 §12）で停止・還流し、再改訂・再ロック（同 §12-1・§12-2）を経てから再開する
11. **実装指示書の失効と v2 化**: 旧 plans/impl-instructions-charm-combo-display.md は SUPERSEDED（DO NOT EXECUTE）とし、plans/impl-instructions-charm-combo-display-v2.md を新正本とする。数量契約は本計画書と v2 指示書の2正本だけから抽出する（同期計画 §2-1）

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
| 実寸の出典 | **ストア商品ページ（canvasart-k.stores.jp）記載のサイズで確定（2026-07-12 全種を照合済み・§4-1）**。**実測値（2026-07-13 受領）とのmm単位の完全一致による二重裏付けは7種**（swan はストア記載のみ・実測未受領 — §4-1 注記）。丸カンの外径42mmも swan 商品ページの「リング部分:外径42mm、内径32mm」記載で一次ソース確認済み |
| 実物の吊り仕様 | 商品ページ確認: 標準は**革紐（長さ約15cm・牛革）**、**金具（開閉式リング＋しずく型コネクタ）は +500円のカスタムオプション**。プレビューは既存 `index.html` と同じ**丸カン（42mm）表現＝金具カスタム版相当**を基準にする（§5-5・§11-8） |
| ステッチの実物仕様 | 商品ページ確認: **白ステッチ標準（黒へ変更は+500円カスタム）が明記されているのは horseshoe と osanpo のみ**。horse / frenchie / dachshund / toy poodle / cat の5種は商品説明にステッチ記載がなく、受領写真でも縁ステッチは見えない＝**ステッチなしが実物仕様**。→ stitchMode に `none` を設け、「全種で白黒トグル保証」はしない（§5-6）。**swan の stitchMode は既存構成を流用せず、無着色素材の実画素と商品仕様からフェーズ0で detect / none / fallback の完全列挙条件（§5-2）により判定する**（§4-1c） |
| 処理方式 | チャームは**固定8種**であるため、背景除去・マスク生成は利用者の端末では行わず、**準備工程で派生アセット（検証済み画像＋マスク）を事前生成してリポジトリに配置**する（§5-1）。実行時はロードとリカラー・合成のみ |
| 実装体制 | 計画書承認後に実装指示書を作成し、Opus 4.8 / Codex に委任。作業ブランチ分離・停止チェックポイント等はミニバッグ引き継ぎ指示書（`plans/impl-instructions-minibag-photo-recolor.md`）の流儀を踏襲 |
| スワン | 正式対象（8種目・改訂13）。**改訂14で生成入力の正本が確定**: `swan-colorless.png`（嘴カラーあり版フチ画像・1701×2560・SHA は §4-1c・AE-20260718-09）。所有権分類も確定 — **くちばし＝アクセント色として保持**（素材の着色ブラウンを表示・AE-20260718-08）・**ロゴ「&.」あり＝立体シルバー表現のまま採用**（G 所有・metalTone 連動・立体陰影の最終判断は Phase 5 QA・AE-20260718-07）・**金具鋲＝保持**（Phase 4 の所有権分類で機械確定）。旧素材（swan.jpg・swanフチ画像.png・swanフチ画像２.png・swan背景透過.png）は supersededSources（生成入力禁止・§4-1c） |

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

### 4-1. チャーム一覧（8種 + なし）— 実寸はストア商品ページ記載で確定（2026-07-12 照合・うち7種は実測照合済み 2026-07-13。swan はストア記載のみ＝実測未受領・下記注記）

| # | クライアント表記 | UI表示名 | HP記載サイズ | 実寸 W×H (mm) | CHARMSキー | canonicalSource（Phase 1 凍結・source-photos/。SHA と寸法は §4-1c 正規入力マニフェストが正本） |
|---|---|---|---|---|---|---|
| 1 | horse | horse | 約H7×W9.5cm | 95×70 | `horse` | `horse-colorless.png`（horseフチ画像由来・2158×2233） |
| 2 | horseshoe | horseshoe | 約H5.4×W4.6cm | 46×54 | `horseshoe` | `horseshoe-colorless.png`（horseshoeフチ画像由来・2020×2385。改訂13の horseshoe色透過.png と同一 byte） |
| 3 | frenchie | frenchie | 約H7.5×W6cm | 60×75 | `frenchie` | `frenchie-colorless.png`（frenchieフチ画像由来・1808×2664） |
| 4 | dachshund | dachshund | 約H6×W9cm | 90×60 | `dachshund` | `dachshund-colorless.png`（dachshundフチ画像由来・2195×2195） |
| 5 | toy poodle | toy poodle | 約H6×W8cm | 80×60 | `toypoodle` | `toy-poodle-colorless.png`（toy poodleフチ画像由来・2176×2176・**スペース除去**） |
| 6 | osanpo | osanpo（骨型ネームタグ） | 約H4×W9.2cm | 92×40 | `osanpo` | `osanpo-colorless.png`（osanpoフチ画像由来・2304×2304） |
| 7 | cat | cat | 約H6.5×W6.8cm | 68×65 | `cat` | `cat-colorless.png`（catフチ画像由来・2195×2195） |
| 8 | swan | swan | チャーム部H9×W9cm | 90×90（**ストア記載のみ・実測未受領**） | `swan` | `swan-colorless.png`（swanフチ画像　嘴カラーあり.png 由来・1701×2560・AE-20260718-09） |
| 9 | — | 【なし】 | — | — | `"none"` | （画像なし） |

- **改訂14: 全8種の生成入力は Phase 1（sourceFreezeCommit `8b3bdb41bd3205287f2612ca9fb071b4d53e8376`）で凍結済みの canonicalSource（上表）であり、全種 sourceMode="colorless" / toneMode="flat"**（§4-1c・§5-2）。承認見本は canonicalReference（`reference-visuals/charms/{key}-approved.png`・フチ画像兼用のため canonicalSource と同一 byte・AE-20260718-02）。旧受領写真 8枚（JPEG）・swan 旧素材2版・背景透過8枚は supersededSources（§4-1c）

- 実寸の出典: **ストア商品ページ（canvasart-k.stores.jp）の記載サイズ（2026-07-12 確認）**。旧計画書の値と全種一致。さらに**実測寸法（2026-07-13 受領: horseshoe W46×H54 / frenchie W60×H75 / dachshund W90×H60 / toy poodle W80×H60 / cat W68×H65〔実装エージェント（Codex）フェーズ0報告〕・horse W95×H70 / osanpo W92×H40〔クライアント実測・同日受領〕）が上表の値と7種すべてmm単位で完全一致**し、確定値として二重に裏付け済み
- **swan（8種目・改訂13）の実寸は現状ストア記載（チャーム部 H9×W9cm = 90×90mm）のみで、実測値は存在しない（実測未受領）**。prep.json の `sizeMmMeasured.confirmed = false` で開始し、フェーズ0の歪み検査で **preDistortion > 3% が出た場合はアスペクト補正を発動せず（§5-2 発動述語が confirmed=true を要求するため機械的に不成立）実測受領まで停止**（§7-9 対処①）してユーザーへ実寸確認を依頼する。歪み 3% 以内なら実測なしで進行可。実測が届き次第 confirmed=true・受領日を記録して本計画書 §4-1 を更新する（W/H の対応も出典と併せて確認する）
- W×H の対応: swan のストア記載「チャーム部H9×W9cm」は正方形のため W/H の取り違えリスクはないが、pxPerMm の W/H 各計測（§5-2）で新素材の縦横比と照合する
- **horse / osanpo も実測受領（2026-07-13・horse W95×H70 / osanpo W92×H40）— 上表と完全一致し、swan を除く7種の実寸が実測で確定**（sizeMmMeasured.confirmed = 7種 true・swan false・§5-2）。これにより horse は preDistortion > 3% の場合にアスペクト補正が自動発動する条件が揃い（実測比 0.3〜0.5% のため発動しない見込み）、osanpo は manual-line のため引き続き shouldApply 対象外（超過時は停止 → 個別設計・§7-9 対処③）。万一将来の再実測で相違が判明した場合の修正経路は §10「実寸データ・校正の誤り」のとおり（準備ツール入力・prep.json 側修正 → 派生アセット再生成＋検査1〜8再実行 → 転記照合10項目の再転記 → **§7-11 の81配置・§9-2 の実寸比率の再検証**〔§5-5〕。**CHARMS 単独修正は禁止**）。なお §7-9 の pxPerMm 歪み検査（≤3%）は幅高比の整合を機械検査するため、sizeMm の縦横比誤りはフェーズ0で自動検出される（等方的な絶対値誤差は検出範囲外 — 7種の実測一致により HP記載の精度は裏付け済み）
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
2. **ラインストーン（horseshoe の6個・swan の金具鋲等 — §4-1c）**: 革でも糸でもない装飾。**保持（リカラー対象外・masks.α=0）とし、素材の色をそのまま表示**する（§11-10 確定・2026-07-13。§5-2 の保持チャンネル）。表示品質は Phase 4 の目視検査（§7-8 — horseshoe 8ケース）で確認し、§9-3 の固定検証セットで回帰確認。horseshoe の6ストーンは**位相検査（検査6・§5-4 改訂14）**で成分数・面積・位置の機械検査も行う
3. **吊り穴**: 革の内側に空いた小穴（背景色が透ける）。前景マット生成時に**穴として抜く（α=0）**。穴周囲のフェザー帯も所有権恒等式（§5-2）の対象

### 4-1c. Phase 1 凍結素材の実態（改訂14・2026-07-19 — sourceFreezeCommit `8b3bdb41bd3205287f2612ca9fb071b4d53e8376`）

全8種の生成入力は **Phase 1（同期計画 §5）で凍結された canonicalSource＝フチ画像由来の colorless PNG**。フチ画像は**革面が未着色（白）のイラスト調デザイン素材**（写真ではなく意匠データ由来・白背景 254 単色・αなし）であり、全8種を無着色素材モード（sourceMode="colorless"・§5-2）で処理する。承認台帳は reports/charm-combo/rev14/phase-1-approvals.md（AE-20260718-01〜14）、freeze の正本は plans/charm-approved-source-freeze.json / plans/charm-approved-color-freeze.json。

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

- 実装セッションは Git 管理外パスを参照しない。**入力の同定は `git show <sourceFreezeCommit>:<canonicalPath>` と上記マニフェストの SHA-256 照合のみで行う**（不一致＝別版混入として停止）。フチ画像 8点の背景はコーナー 100×100 が輝度 254 単色であることを機械確認済み（phase-1-approvals.md 実測記録）
- **horseshoe 素材の目視実態（2026-07-15 確認・改訂14でも同一 byte のため有効）**: 革面＝**純白・無地（質感なし）**／白のロープ調ステッチ2周（外周・内周）／黒系の縁取り線（最外周・内側U字・吊り穴周囲）／金色台座のラインストーン6個／金色ロゴ「&.」（右下）／背景＝白（αなしのため背景除去は Phase 4 のパイプライン②〜④で実施）。吊り穴（右上）の中央と U字中央の空間は背景と同じ白
- **horseshoe の所有権分類（ユーザー指定・2026-07-15・改訂14でも有効）**:
  - **革（R）**: 中央革面／ステッチ外側の革帯／最外周および内側U字の外枠線／右上の吊り穴周囲線／**アンパサンド内部の上下2つの穴**（「&」のカウンター空間 — 透明にしない）
  - **ロゴ（G）**: アンパサンドの金色文字本体のみ
  - **ステッチ（B）**: 白／黒切替対象の縫い目（ロープ調2周）
  - **保持（masks.α<128）**: 6個のラインストーンと台座（金色リム含む）
  - **透明（base.α=0）**: 画像背景／U字中央の空間／右上吊り穴の中央
- **swan の確定事項（改訂14 — AE-20260718-07/08/09 により「受領後確定」条項を解消）**:
  - **pipelineSource = swan-colorless.png**（嘴カラーあり版・1701×2560・RGB・αなし。四隅白 254 単色✓・くちばしブラウン系 17,051px〔平均 RGB 149,87,44・革シボ付き〕を機械確認済み）
  - **くちばし = アクセント色として保持（masks.α<128）**: 素材に着色済みのブラウンをそのまま表示する（AE-20260718-08。革連動にはしない）
  - **ロゴ「&.」= あり（右下・金具鋲の左）**: シルバーの立体表現のまま**ロゴ所有（G）**とし metalTone でロゴカラー（金/銀）に連動させる。立体陰影の見た目の最終判断は Phase 5（形状 QA）で行う（AE-20260718-07。ロゴなし時の UI 条件改訂〔旧 §11-13③ 停止ゲート〕は不要となった）
  - **金具鋲・ラインストーン等の装飾 = 保持（masks.α<128）見込み**: 厳密な画素分類は Phase 4 の所有権分類（manual-keep / manual-owner）で機械確定する
  - **stitchMode は Phase 4 で確定**: 素材実画素から §5-2 の完全列挙条件（detect / none / fallback）で判定する（白ステッチが素材に存在するため detect 見込みだが仮確定しない）。確定後に固定目視回帰セットのケース数（§9-3）と UI 注記（§6-1）を連動更新する
  - **実寸は引き続きストア記載 90×90 のみ（実測未受領）**: sizeMmMeasured.confirmed=false で開始し、歪み>3% ならアスペクト補正を発動せず実測受領まで停止する契約（§4-1・§5-2・§7-9①）は不変

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
    `gray = round(clamp(grainBase × shade(x,y) × grain(x,y), 0, 255))`・`base.RGB ← (gray, gray, gray)`
    - `shade(x,y) = clamp(lum(素材RGB) / flatWhite, 0, 1)` — 素材自体の明暗（縁取り線・陰影）を革色の暗部として保存する係数。`flatWhite` ＝ 革コア画素（base.α=255 ∧ masks.α≥128 ∧ masks.R=255）の lum の**下位中央値（baseLum と同じ 0始まり添字 floor((n−1)/2) 規則）**を⑦'-1 実行直前の画像から算出。**コア画素0件、または flatWhite が正の有限値でない（≤0・NaN・Infinity）場合は生成エラー**（派生アセット不成立 — 0/0 等の NaN が画素バッファへ暗黙変換されて検査をすり抜ける事故を排除）。flatWhite は prep.json（grain 一式）に記録し、検査F-2 が再計算一致を照合する
    - **ミラー添字関数（共通定義 — 比率マップの blur 境界とタイル参照の双方でこの同一関数を使用する）**: `mirrorIndex(k, n) = （t = ((k mod 2n) + 2n) mod 2n として） t < n ? t : 2n − 1 − t`（符号付き整数 k に対して定義 — JavaScript の負剰余を二重 mod で補正。端画素を重複させる対称反転: …2,1,0,0,1,2,…）
    - `grain(x,y)` — **グレイン比率マップ**（適用 profile の grain 入力側で1回生成）を**ミラータイル＋最近傍**で参照した値。離散規則を一意に固定する（決定的 — 検査8の再生成一致の前提）:
      - **比率マップ生成**（適用 profile の grain 入力 W₀×H₀ 上 — calf = 1011×676・chevre = 824×383）: `g0[i,j]` = グレースケール値（§5-2 輝度式と同一係数・浮動小数のまま保持）→ `blur[i,j] = (Σ_{dj=−R..R} Σ_{di=−R..R} g0[mirrorIndex(i+di, W₀), mirrorIndex(j+dj, H₀)]) / (2R+1)²`（R = grainRadiusPx。**境界は上記 mirrorIndex による対称拡張**・加算は di 昇順→dj 昇順の二重ループの逐次浮動小数加算を正とし、分離型等の最適化は結果が完全一致する場合のみ許容）→ **blur=0 の画素が1つでもあれば生成エラー**（不正な質感画像＝派生アセット不成立・ゼロ除算を排除）→ `ratio0[i,j] = g0[i,j] / blur[i,j]` → `grainMap[i,j] = clamp(ratio0, 1−grainAmp, 1+grainAmp)`（＝局所凹凸のみ。大域的な明暗勾配と色を機械的に除去 —「色を捨て局所的な凹凸だけ」の実装定義）
      - **参照**（チャーム作業座標の画素 (x,y)・軸別尺度）: `u = floor((x + 0.5) / grainScaleX)`・`v = floor((y + 0.5) / grainScaleY)`（**最近傍・補間なし**）→ `i = mirrorIndex(u, W₀)`・`j = mirrorIndex(v, H₀)` → `grain(x,y) = grainMap[i,j]`。タイル原点は質感画像原点 (0,0) 固定
      - 浮動小数の途中丸めは行わず、8bit 量子化は ⑦'-1 の最終 `round(clamp(...))` の1回のみ
    - **タイルの拡縮率（軸別 — 非等方投影の素材でも両軸とも実寸 mm 基準になる）**: `grainScaleX = grainScaleMm × pxPerMmWorkW`・`grainScaleY = grainScaleMm × pxPerMmWorkH`。**`pxPerMmWorkW / pxPerMmWorkH`（グレイン尺度の基準・作業座標系）**＝ **⑦' 開始時点（⑦完了後）の画像**に対し calibration 規則（§5-2 — bbox は bboxPx.w / bboxPx.h・manual-line は幅／高さ計測線）で幅・高さ計測値を算出し `幅計測値 ÷ sizeMm.w`／`高さ計測値 ÷ sizeMm.h`（いずれも prep.json に記録）。**⑧が最終画像から再算出する正本 pxPerMm とは別値**であり、正本側の決定規則・転記契約は不変（⑦' が⑧の値を参照する循環を排除）
    - **`grainScaleMm`（grain 入力1pxに対応させる mm）は全チャーム・全 material で共通の値**として Phase 4 で目視確定する — 軸別尺度により**⑦' 時点で全チャームの両軸ともシボ実寸スケールが揃う**。⑧' アスペクト補正は実測寸法を正とする矯正（§5-2）のため、適用チャームでは**補正後にグレイン尺度もほぼ等方へ収束する**（適用軸の尺度〔W軸適用なら pxPerMmWorkW・H軸適用なら pxPerMmWorkH〕× effectiveScale が非適用軸側の尺度へ近づく — 残留乖離は離散丸めと計測差の程度）。事前補償は行わず、**§7-8 の24色回帰目視の合格を許容条件とする**（グレインは表示品質の素材であり幾何契約〔寸法・配置・§9-2〕の対象外）
    - パラメータ（grainBase・grainAmp・grainRadiusPx・grainScaleMm）はフェーズ0で目視確定し prep.json に記録（初期値の目安: grainBase=200・grainAmp=0.25・grainRadiusPx=24）。**制約 `grainBase × (1 + grainAmp) ≤ 255`**（クリップによる質感の潰れ防止 — 検査F-2）
    - 焼き込み結果の革主所有画素は**構成的に R=G=B（無彩色グレー）**となり、素材の革元色は混入しない（検査F-1 で機械確認）
  - **⑦'-2 再defringe**: ⑦'-1 の出力に対し、base.α 基準で α<255 の画素の RGB を**④と共通の最近傍選択規則・同一実装〔同一関数〕**（§5-2 base.png 契約・⑧'-2 と同じ）で再置換する（焼き込みがエッジ帯と近傍 α=255 画素の間に輝度差を作り検査4を破ることを防ぐ — ⑧'-2 と同じ構造の再確立）。base.α・masks は変更しない（RGB のみ）
  - ⑦' の完了後に⑧派生値算出へ進む（baseLum・bboxPx 等は焼き込み後の画像から既存決定規則のまま算出）。アスペクト補正⑧'（shouldApply 成立時）は焼き込み後画像に対して従来契約どおり適用される
- **表示革色 DISPLAY_VARIANTS（approvedHex 正本）・実行時トーン flatLeatherTone**: 決定規則・正本・照合は §5-6 に規定（旧 referenceIvory / DISPLAY_COLORS 契約は改訂14で撤回 — §0-B-4）
- **prep.json への記録（schemaVersion 5・§5-4）**: sourceMode・**materialAssets（materialKey ごとの base 期待出力ハッシュ・byteLength・textureProfileKey・generationMode）**・grain 一式（**material ごとに** file・sha256・grainBase・grainAmp・grainRadiusPx・grainScaleMm・**grainScaleX / grainScaleY・pxPerMmWorkW / pxPerMmWorkH・flatWhite**）

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
  - **発動述語（機械判定・裁量なし・補正前値のみから決定）**: `shouldApply = (calibration.mode = "bbox") ∧ (sizeMmMeasured.confirmed = true) ∧ (preDistortion > 0.03)`。ここで preMeasuredW/H（パイプライン⑧〔§5-4〕時点の幅・高さ計測値）・prePxPerMmW/H・preDistortion（補正前歪み率）は**全チャームで prep.json に記録**する（applied=false でも記録 — 検査3が発動述語を補正前値だけから再計算するため）。`sizeMmMeasured`（実測確認状態 confirmed・受領日）は prep.json の**入力値**とする: **7種 = true〔2026-07-13 受領・§4-1〕・swan = false〔ストア記載のみ・実測未受領 — 改訂13・§4-1〕**。**confirmed=false のチャームで preDistortion > 0.03 が出た場合は実測受領まで停止**（§7-9 対処①。**swan が該当し得る** — 実測が届き次第 confirmed=true に更新して §4-1 へ記録）
  - **補正式（縮小のみ — §7-4「拡大は不可」の思想と整合）**: `pxPerMmW > pxPerMmH` なら W方向へ `scaleX = pxPerMmH / pxPerMmW`、`pxPerMmW < pxPerMmH` なら H方向へ `scaleY = pxPerMmW / pxPerMmH` を適用する（過剰サンプル軸を他軸へ合わせる縮小であり、アップサンプリングは発生しない。§7-4 の前景長辺 ≥600px は補正後の fgBbox で判定 — **補正適用対象の5種**（フェーズ0実績・§4-1）は縮小軸が長辺と直交または縮小後も603px以上のため維持見込み）
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
  - **根拠と品質**: 実測寸法（§4-1・**実測確定済みの7種**がストア記載と完全一致の二重裏付け）を正とする矯正のため、実寸比率の表示精度（§9-2）はむしろ向上する。テクスチャの伸縮は最大10%（toy poodle）で、比較対象が並ばない単体チャーム表示では知覚困難 — §7-8・§9-3 の目視検査で確認する
- bboxPx / fgBboxPx はあくまで**表示・配置・クリップ検査用の軸平行矩形**。斜め素材では `bboxPx ÷ pxPerMm` が公称W/Hと一致しないのは正常（例: osanpo は回転のため bbox が 92×40mm より正方形に近くなる。§9-2 の比率検証も校正線基準で行う）
- **保持画素数 keepPxCount（派生値）**: `masks.α < 128` の画素数。準備ツールが自動算出し prep.json に記録する（保持を使う見込みは horseshoe のラインストーン6個＋台座と、swan のくちばし・金具鋲等の装飾類〔§4-1c — 画素分類は Phase 4 で機械確定〕。意図しない保持画素の混入は §7-8 の目視と検査6で確認）
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

- ステッチ有チャームの detect 成立見込み: **全8種が colorless（白ステッチ×白革面）のため自動検出は不成立見込みだが、manual-owner の手動指定によるマスク化で detect が成立する**（§5-6 — detect の条件はマスクの由来を問わない。39fe815 の horseshoe 実装では seed ベースの決定的レシピで成立済み）。swan は Phase 4 判定（§4-1c）。fallback は Phase 4 でマスク化（自動・手動とも）が不成立だった場合の予備手段としてのみ採用する（採用時は本契約と下記検査が全面適用）
- **描画座標系**: stitchPaths は**アセット座標系**（⑧'適用後の書き出し済み base.png の px・§5-2）の転記値のまま完成ビットマップへ描画し、線幅・dash・gap は `mm値 × pxPerMm` で**アセットpxに換算**する（ステージ倍率はビットマップ全体の拡縮で一括適用・上記合成方針と一致）
- fallback チャームでは、準備工程で元写真の糸画素を①**革所有（R）としてマスク化**し、さらに②**base.RGB を周辺革色で中和**する（手動インペイント。manual-inpaint レイヤに保存）。①だけでは photoLeatherTone が元画素の明暗を保持するため、選択革色の中に元糸が筋として残る。**白・黒どちらを選んでも元糸の残像が見えないこと**をフェーズ0の必須検査とする（§7-7）
- 実行時の描画順: 革・ロゴリカラー（保持画素はそのまま） → stitchPaths を選択ステッチ色で上描き（**革所有領域〔`masks.α≥128` ∧ `R≥128`〕でクリップ** — 共通述語。保持画素の上には描かない）
- 検査（§5-4 書き出し前検査5。**完全列挙型ゲート — 3モードの排他性を機械保証し、下記いずれの条件セットにも合致しない組み合わせ・未知の stitchMode 値は即不合格**。「所有画素数」の集計は全て共通述語 `masks.α ≥ 128` 込み）:
  - `detect`（horseshoe / osanpo）: stitchPaths 空 ∧ `masks.B` 所有画素数 > 0 ∧ 白黒切替の出力差分画素数 > 0 ∧ threadColor ∈ {"dark","light"}
  - `none`（horse / frenchie / dachshund / toypoodle / cat）: stitchPaths 空 ∧ **`masks.B` 所有画素数 = 0** ∧ **白黒切替の出力差分画素数 = 0**（ステッチトグルが出力に影響しないことを機械保証） ∧ threadColor = null
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
- 素材は全種吊り金具なしのため **attachmentMode は全8種 "none"** の見込み（丸カンは非接続の独立表示として全チャーム一律に扱う・§5-5。swan の金具鋲は吊り金具ではなく装飾であり、保持領域として §4-1c で分類する）
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
| 内部ステッチ検出（輝度＋近傍＋enclosed判定） | 既存 `index.html`（ドーナツ実績） | 準備ツールのステッチマスク自動候補生成に流用（自動検出の主対象は osanpo〔白糸×濃色革のため**高輝度側を検出**〕。糸色極性 threadColor に応じた輝度方向の切替機構自体は温存。**無着色素材の horseshoe は白×白で自動検出不成立見込み → manual-owner 手動指定で detect 成立・§5-6**）。実行時には使わない |
| `?debug=1` マスク可視化・グリッド・恒等式検査 | ミニバッグ Opus 実装 | charms.html に移植（§9 検証で使用） |
| シルバー/ゴールドキーリング.jpg | 既存アセット | 丸カン（外径42mm）表示。**42mm はストア swan 商品ページの「リング部分:外径42mm、内径32mm」記載で一次ソース確認済み（2026-07-12）**。銀金の2アセットは**同一の42mm基準枠に正規化**し、切替で位置がずれないことを受け入れ基準に含める（§9） |

### 5-4. 準備ツール `tools/charm-prep.html`

- ブラウザだけで動く自己完結HTML（`python3 -m http.server` 経由で使用。file:// は canvas 汚染のため不可 — ミニバッグ検証と同じ運用）

**決定的パイプライン（この順で毎回再実行できることが契約）**

```
生成入力（canonicalSource・§4-1c） → ①回転・クロップ・縮小（長辺≤1200px） → ②自動背景マット（しきい値） → ③manual-alpha適用
     → ④defringe → ⑤自動所有権候補（革/ロゴ/ステッチ） → ⑥manual-owner適用 → ⑥'manual-keep適用（保持強制）
     → ⑦manual-inpaint適用（base.RGB置換） → ⑦'グレイン焼き込み（sourceMode="colorless" のみ・§5-2。二段階: ⑦'-1 革主所有画素をニュートラルグレー〔grainBase×shade×grain〕へ置換 → ⑦'-2 再defringe〔④と同一規則・同一実装〕）
     → ⑧派生値算出（§5-2決定規則） → ⑧'アスペクト補正（shouldApply 成立時のみ・§5-2。二段階: ⑧'-1 幾何変換〔base.RGB: photo=bilinear・colorless=nearest／α・masks=nearest（共通）〕 → ⑧'-2 補正後α基準の再defringe〔④と同一規則〕→派生値を最終画像から再算出） → ⑨自動検査 → ⑩書き出し
```

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
   - `keepPxCount`（保持画素数）が算出値と一致して prep.json に記録されていること。**保持を使う見込みのないチャーム（horse / frenchie / dachshund / toypoodle / osanpo / cat の6種）で keepPxCount > 0 の場合は警告を出し、意図的かどうかを §7-8 の目視・素材検証レポートで確認**（機械不合格にはしない — 将来の保持利用を妨げないため。horseshoe は保持確定〔ラインストーン6個＋台座 — 6ストーン位相検査は下記〕・swan は保持確定〔くちばし＋金具鋲等・§4-1c〕）
   - **horseshoe 6ストーン位相検査（改訂14 — 39fe815 から還流）**: horseshoe の保持領域（manual-keep 由来）は**作業座標系で 8近傍連結成分がちょうど6個**・**union 面積が 32000〜34000 px**（39fe815 実測レンジ）でなければ不合格。アスペクト補正適用時は、**work 座標の keep bitset を center-nearest 投影（§5-2 の nearest `srcIdx` 式の逆対応）した期待 bitset と最終 masks の保持画素が完全一致**（missing 0・extra 0）し、**期待側・実側の双方が6成分**を保つこと。**asset 座標へ work 座標の固定面積閾値を直接適用してはならない**（補正で面積が変わるため — 位相〔成分数・一致〕で検査する）
7. 転送量: base.png + masks.png 合計 ≤ 900KB

【書き出し前検査M（⑨続き。**全チャーム共通**（sourceMode を問わない）— 1〜7と併せて全通過が⑩の条件。改訂13）】

- **M-1 正規入力マニフェスト照合**: §4-1c の正規入力マニフェスト（CHARMSキー → sourceMode・正規パス・期待SHA-256。準備ツールへ定数として転記し、sourceFreezeCommit の canonical files と一致）に対し、①実入力ファイルの SHA-256、②prep.json の source（file・sha256）と sourceMode、の**すべてが完全一致**すること。**マニフェスト外の入力は即不合格 — 特に supersededSources（旧受領写真 8枚・swan 旧フチ画像2版・背景透過8枚・§4-1c 禁止入力）は、prep.json に自己整合的に記録しても本検査で排除される**（自己申告値どうしの整合だけでは通過できない）。grain は**生成する material に対応する profile の grain 入力**（calf = 質感.jpg／chevre = chevre-grain.png）の SHA-256 を §4-1c マニフェスト記載値と照合する
- **M-2 モード対応（全種）**: `sourceMode="colorless" ⇔ toneMode="flat"`・`sourceMode="photo" ⇔ toneMode="photo"` の1:1対応（§5-2。photo チャームの toneMode 誤転記も本検査で検出する — colorless 専用枠には置かない）

【書き出し前検査F（⑨続き。sourceMode="colorless" のチャーム専用 — 1〜7・Mと併せて全通過が⑩の条件。改訂13）】

- **F-1 焼き込み範囲と元色無混入（二部）**:
  - **(a) ⑦'-1 範囲検査**: ⑦'-1 適用直後、革主所有画素（masks.α≥128 ∧ masks.R≥128）の base.RGB が**全画素 R=G=B（無彩色グレー）**であり、それ以外の画素（ロゴ・ステッチ主所有／保持／base.α=0）の base.RGB が**⑦'-1 の前後で全画素一致**していること（⑦'-2 の再defringe は④と同じ全 α<255 対象のため本比較の対象外。準備ツールは⑦'-1 前後のスナップショット差分を保持し⑨で判定する）
  - **(b) 最終画像の無彩色性**: ⑨時点の最終画像（⑦'-2・⑧' 適用後）で、革主所有 ∧ base.α=255 の画素が**全て R=G=B** であること（⑧'-1 の colorless=nearest 契約〔§5-2 アスペクト補正〕により構成的に成立する — それを機械検査で保証。flatLeatherTone の 256 エントリ LUT 適用範囲〔§5-6〕の前提）
  - **無彩色保証の範囲は「革主所有 ∧ base.α=255」に明示的に限定する**: α<255（エッジ帯）の革所有画素は ⑦'-2／⑧'-2 の再defringe 値＝**最近傍 α=255 画素の実色**（最近傍がロゴ・保持等ならその非グレー値）のままであり、無彩色保証の対象外。これは④と同一のエッジ処理思想（縁は近傍実色で自然に）で、再defringe の最近傍共通規則・検査4 の整合を colorless でも崩さないための設計 — エッジ帯の革所有画素は実行時に flatLeatherTone の lum 式で直接計算され（LUT 対象外・§5-6）、出力αは base.α のためフェザー帯 1〜2px の視覚影響に留まる（§7-8 の24色回帰目視で確認）
  - 素材の**革面の元色**（白面・縁取り線の色）がリカラー入力へ混入しないことの機械保証（§5-2 原則① — 革主所有 α=255 画素はグレーのみ。エッジ帯に残る近傍実色はロゴ・保持等の意匠色であり「革の元色」ではない）
- **F-2 グレイン入力整合**: 適用 profile の grain 入力（calf = 質感.jpg／chevre = chevre-grain.png）の SHA-256 が prep.json 記載値および §4-1c マニフェスト記載値と一致／**grainBase は 1〜255 の整数・grainAmp は 0 < grainAmp ≤ 1 の有限実数・grainRadiusPx は正整数・grainScaleMm は正の有限実数**で、grainScaleMm は**全チャーム・全 material で同一値**／`grainScaleX = grainScaleMm × pxPerMmWorkW`・`grainScaleY = grainScaleMm × pxPerMmWorkH` の**両軸の再計算一致**かつ正の有限値（pxPerMmWorkW / pxPerMmWorkH の再計算一致を含む・§5-2）／**flatWhite の再計算一致かつ正の有限値**（§5-2）／`grainBase × (1 + grainAmp) ≤ 255`／タイル方式が mirror（mirrorIndex 共通定義・§5-2）
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
- **mutation 試験（5種）**: oracle の検出力を自己検証する。production 相当の afterBake を次の変異で生成し、独立 oracle へ通して**不一致が検出されること**を確認する（正常出力は不一致0）: ①Float32 化 ②mirror 誤り ③loop-order 変更（dx 外／dy 内） ④grain map 全1 ⑤非革画素変更
- 検査Fの合格には、oracle 一致（正常系）と mutation 5種の全検出（異常系）の両方を要する

【production／oracle Worker の世代管理（改訂14 — 39fe815 から還流】

- production grain 計算と oracle 検算は**別世代状態**を持つ Worker で実行する
- **新世代開始時に旧 Promise を reject し、旧 Worker を terminate する**
- generation と operation token（beginOperation / assertOperationToken / endOperation）を照合し、旧世代の結果が新世代の状態へ書き込まれないことを保証する
- Worker 不可環境の main-thread fallback も**定期的に中断を確認**する
- **timeout 後は timer・Worker・Blob URL・state 参照をすべて解放**する（リーク検査は selftest に含める）

**prep.json（版付きスキーマ。記録値〔入力値＋派生定義値〕の正本。通常表示では読まない — `?debug=1` の転記照合時のみ読込）**

- `schemaVersion`（**スキーマ拡張または生成規則の変更時に増分する** — フィールド構成が不変でも、同一 schemaVersion に生成規則違いの prep.json が混在すると検査8の再生成一致が破れるため規則変更も増分対象。aspectCorrection・sizeMmMeasured を導入した改訂10、⑧' を二段階化〔再defringe〕した改訂12、無着色素材モードを導入した改訂13、**全8種 colorless 化・material-aware 化〔materialAssets・grain per material〕・straight PNG 契約化の改訂14**でそれぞれ増分 — **改訂14以降の schemaVersion は 5・toolVersion は 0.6.0**。**全8種を v5 の生成規則で再生成し、v4 以前の prep.json・派生アセット・旧 manual レイヤ〔座標系が旧素材由来のもの〕との混在を残さない**〕、ツールバージョン
- 生成入力（canonicalSource）のファイル名と SHA-256、manual-*.png それぞれの参照と SHA-256
- ①の回転・クロップ・縮小値、②の背景しきい値、⑤のロゴ・ステッチ検出しきい値
- 入力値: sizeMm、**sizeMmMeasured（実測確認状態 confirmed・受領日 — §5-2 発動述語に使用。7種 = true〔2026-07-13〕・swan = false〔ストア記載のみ・実測受領で更新 §4-1〕）**、threadColor、stitchMode、**stitchPathsWork（作業座標系の stitchPaths 入力・§5-2）**、**attachmentMode、calibration（mode・計測線座標・reason〔manual-line 採用時は採用理由と計測点の説明を必須記録 — 改訂13〕）**、**sourceMode（"photo" | "colorless"・§5-2）**、**grain（生成 material ごと: materialKey・file・sha256・grainBase・grainAmp・grainRadiusPx・grainScaleMm — §5-2）**
- **派生定義値（正本）**: bboxPx・fgBboxPx・**calibration の計測値**・pxPerMm・baseLum・**keepPxCount（保持画素数）**・**aspectCorrection（applied・axis・scale・effectiveScale・preMeasuredW/H・prePxPerMmW/H・preDistortion — 全チャームで記録。§5-2 改訂10）**・**stitchPathsAsset（アセット座標系の stitchPaths 転記値 — 検査5・CHARMS 転記・`?debug=1` 照合の参照先。§5-2）**・**toneMode（sourceMode と1:1対応 — 検査M-2。CHARMS 転記照合10項目の一つ）**・**materialAssets（materialKey ごとの base 期待出力ハッシュ・byteLength・baseLum・textureProfileKey・generationMode="baked-base"）／grainScaleX・grainScaleY／pxPerMmWorkW・pxPerMmWorkH／flatWhite（sourceMode="colorless"・§5-2・§5-6）**
- **期待出力ハッシュ**: material 別 base 全点 / masks.png の**最終 PNG 独立デコード（straight PNG 契約・§5-4）による RGBA の SHA-256**（PNG エンコード差に依存しない比較のため。base は **⑧'-2〔再defringe・§5-2〕適用後の最終画像**が対象 — 補正非適用チャームは ⑧' 全体をスキップするため④〜⑧の出力がそのまま対象）と byteLength
- 自動検査1〜7・M（sourceMode="colorless" は F 含む）の結果値、コミット前検査8の `verify` 結果
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
- stitchMode は **detect / none / fallback の3値**:
  - `detect`（horseshoe / osanpo 見込み）: 準備工程で素材のステッチ画素をマスク化（自動検出は糸色極性 `threadColor` に応じ、暗糸なら低輝度側・白糸なら高輝度側を検出）。**自動検出が成立しない素材では、準備ツールの手動補正（manual-owner）でステッチ画素を B 所有に指定してマスク化してよい**（無着色素材の horseshoe は白ステッチ×白革面でコントラストがなく自動検出は不成立見込み・§4-1c — マスクの由来〔自動/手動〕は実行時挙動・検査5の条件に影響しない）。実行時は**選択色（白/黒）を問わずマスク画素を明示的に塗り直す**（「白選択時は原画のまま」という元糸色依存の仕様にはしない — 元糸の白と選択白の色を一致させ、黒切替と同一のコードパスを通す）
  - `none`（horse / frenchie / dachshund / toypoodle / cat の5種見込み）: `masks.B` 所有画素数 = 0（共通述語 `masks.α≥128` 込み — `count(masks.α≥128 ∧ masks.B>0) = 0`。保持画素のB値は検査しない・§5-2）。ステッチトグルの影響を受けない（検査5で機械保証）
  - `fallback`: detect が不成立の場合のみの予備手段（stitchPaths 契約 §5-2。現時点で該当チャームなし）
- **swan の stitchMode は上記の見込みに含めない**: Phase 4 で canonicalSource の実画素から detect / none / fallback の完全列挙条件（§5-2・検査5）で判定する（§4-1c）。**detect / fallback と確定した場合はステッチ有チャームに加わり、UI 注記（§6-1）・§9-3 のケース数（40→44）・サマリー表記の対象を更新する**
- ステッチトグルUIの有効条件は §4-2（ステッチ有チャームが表示中のときのみ有効）
- 確定はフェーズ0で素材ごとに判断し、テーブルに記録

**金具（丸カン・銀/金トグル）・ロゴカラー（金/銀トグル）・ラインストーン（保持）— §11-8/9/10 確定（2026-07-13）**

- **金具トグル（銀/金）は丸カン専用**: 準備工程で生成した透過PNG（`ring-silver.png` / `ring-gold.png`・42mm外径の共通基準枠に正規化・中心一致）の差し替え。切替で位置・サイズ不変（§9-8）。実行時の背景除去はしない（§5-3）。チャームビットマップには影響しない（キャッシュキーにも含めない・§5-7）
- **ロゴカラートグル（ゴールド/シルバー・金具トグルと独立）**: 箔押しロゴ「&.」（ロゴマスク G 所有）に `metalTone()` を適用し、選択ロゴカラーで描画（金選択→金箔・銀選択→銀箔）。初期値は**ゴールド**（素材上は金ロゴが多数派〔銀は dachshund の箔と swan のシルバー立体ロゴ〕— masks 化後は選択ロゴカラーで描画されるため素材の箔色は表示に影響しない）。面積が小さいため手動補正（manual-owner）での塗り分けを想定。有効条件はチャーム表示中（1点以上）— チャームなしでは無効化（§4-2。**全8種ロゴあり確定 — swan は AE-20260718-07**）
- **ラインストーン等（horseshoe の6個＋台座、swan のくちばし・金具鋲等〔§4-1c〕）は保持**: masks.α<128（§5-2）でリカラー対象外とし、素材のクリスタル・金具・くちばし着色をそのまま表示。革色・ロゴ・ステッチ・金具のどのトグルにも反応しない。塗り分けは manual-keep レイヤ（§5-4。horseshoe の6ストーンは位相検査 — 検査6・§5-4）
- チャーム側に写る金属・光沢装飾は**「ロゴ（G・ロゴトグル連動）」または「保持（masks.α<128・そのまま）」のどちらかに必ず分類**する。これを Phase 4 の**非免除合格条件**とする（§7-8。素材に吊り金具は写っていないため、対象はロゴ箔・ラインストーン・swan の金具鋲等の装飾〔§4-1c〕。**swan のくちばしはアクセント色保持で確定 — AE-20260718-08**）
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

生成入力8種（全種 canonicalSource・colorless・§4-1c）を、以下の観点で検証してから派生アセット生成に入る。**「免除」列が「不可」の項目は例外承認で通過できない**（§8 Phase 4 ゲート）。Phase 1 の機械確認（背景 = コーナー輝度254単色・寸法・SHA — phase-1-approvals.md 実測記録）により 1・3 は事前確認済みだが、**Phase 4 で準備ツールによる機械検査・最終判定を改めて行う**（**4〔前景解像度〕は背景除去後の fgBbox 計測ではじめて確定するため事前判定に含めない**）。**5・6 は toneMode="photo" 専用要件のため改訂14では適用対象0**（全種 colorless — 行は将来素材用に温存）。

| # | 要件 | 免除 | 満たさない場合の対処 |
|---|---|---|---|
| 1 | チャーム単体で全体が写っている（見切れなし） | 不可 | 再撮影依頼 |
| 2 | 正面・吊り下げ時と同じ上下向き | 可 | 回転補正で吸収可能か個別判断 |
| 3 | 背景が無地でチャームと分離可能（白/ライトグレー推奨） | 不可 | 準備ツールのしきい値調整＋手動ブラシ補正で解決（それでも不可なら再撮影） |
| 4 | チャーム部分の解像度: **背景除去後の前景（fgBbox）の長辺が 600px 以上**（画像全体の寸法では判定しない — 白余白は前景解像度を保証しないため） | 不可 | 拡大は不可。再撮影依頼 |
| 5 | 照明条件が概ね揃っている（極端な色かぶり・強い影がない。**将来 toneMode="photo" となる素材のみ — 改訂14では0種** — colorless は照明ではなくグレイン焼き込みが質感の正のため対象外） | 可 | baseLum 正規化で吸収 → 不可なら再撮影 |
| 6 | 革色が明るめ単色（アイボリー等）だと濃色変換に最有利（**将来 toneMode="photo" となる素材のみ — 改訂14では0種** — colorless は無着色のため対象外） | 可 | 濃色ベース写真は変換品質を個別確認 |
| 7 | **stitchMode の確定**: ステッチ有チャーム（horseshoe / osanpo。**swan は実画素から §5-2 完全列挙で判定・§4-1c**）は糸色を記録し detect のマスク成立（自動検出または manual-owner 手動指定・§5-6）を確認。ステッチ無チャームは `masks.B` 所有画素0（none。共通述語 `masks.α≥128` 込み・§5-2）を確認。fallback 採用時（detect 不成立の場合のみ）は stitchPaths の定義＋**元糸RGBの中和（manual-inpaint）**＋**白・黒どちらの選択でも元糸の残像が見えない**こと | 不可 | detect 不成立は fallback（手動破線パス＋元糸中和）に切替 |
| 8 | **見えている全金属・光沢装飾が「ロゴ（G・ロゴトグル連動）」または「保持（masks.α<128）」に正しく分類されている**（§5-6。素材に吊り金具は写っていない・§4-1c。swan のくちばし = アクセント色保持で確定 — AE-20260718-08）。確認方法: 準備ツールのリカラー試験で **horseshoe = 革{ブラック, アイボリー}×ロゴ{金, 銀}×ステッチ{白, 黒}の8ケース（ラインストーンが全ケースで元色のまま不変であることを含む — 6ストーン位相検査は検査6）、全8種 = ロゴ{金, 銀}の2ケースずつ（swan がステッチ有と確定した場合は horseshoe 同様の8ケースも実施）**を目視し、素材検証レポートに記録（§9-3 の固定検証セットは Phase 6 でのこの回帰再実施）。**全8種はさらに適用可能 variant 全色（現行 calf 24色）の片側表示回帰（variant 色回帰）**を目視し、グレインの継ぎ目・タイル反復の視認・色域外圧縮起因の色調破綻・白面の色抜け・素材元色の混入がないことを確認する（§5-2・§5-6）。**アスペクト補正適用チャーム（§5-2）はこの目視で、補正起因の不自然な伸縮・楕円化・ぼけ・色ハロが輪郭・革テクスチャ・ロゴ・ステッチ・ラインストーンにないことを併せて合格条件とする** | 不可 | 手動ブラシ補正（manual-owner / manual-keep）で解決 → 不可なら素材再提供の依頼（除去は不可・§5-6） |
| 9 | 撮影歪み: pxPerMm 歪み率3%以内（§5-2 決定規則。**アスペクト補正適用チャームは補正後の値で判定**） | 不可 | 超過時は ①実寸を実測で確認（sizeMm 誤りなら §10 経路） → ②実測確定済み（bbox モード）なら**アスペクト補正（§5-2）を適用して再生成**（補正後確定値で §7-11・§9-2 を再通過 — §5-5） → ③補正後も超過・manual-line 素材で補正不能なら再撮影 or 個別設計（計画書改訂） |
| 10 | **§5-4 の書き出し前検査1〜7・M（sourceMode="colorless" は検査F込み）に全通過（通過するまで書き出し不可）＋コミット前検査8（再生成一致）に合格（合格するまでコミット不可）**＋暗色/明色背景での輪郭の目視確認 | 不可 | 準備工程をやり直し |
| 11 | **幾何パラメータ固定（§5-5）のうえ、全81配置の3条件検査（クリップなし・重なりなし・負の余白なし・§5-5）**（確定値による機械計算。対象は2点配置・1点センター配置・0点の各表示チャーム前景と丸カン描画境界。非接続レイアウトのため接続検査はない） | 不可 | **不合格の条件別に修正**: ①クリップ・③負の余白 → ステージ寸法を更新／②左右前景の重なり → gapMm または x 配置式を見直し／丸カンとチャームの間隔不足 → ringGapMm または y 配置式を見直し（ステージ拡大では②は解消しない）。いずれの変更も LAYOUT を再固定 → 81配置全件を最初から再実行（§5-5。フェーズ0完了後の変更ならチェックポイント②も再取得） |

**Phase 4（旧フェーズ0）の成果物**: ①8種の派生アセット一式（material 別 base／masks の straight RGBA PNG＋prep.json〔schemaVersion 5・calibration・keepPxCount・grain・materialAssets 等の記録値一式を含む〕＋manual-*.png）、②丸カン透過PNG（ring-silver/gold）、③チャーム定義テーブル全値（CHARMS 転記照合フィールド10項目 — sizeMm確定値・bboxPx・fgBboxPx・pxPerMm・attachmentMode・threadColor・stitchMode・stitchPaths・baseLum・toneMode。§5-2）＋LEATHER_VARIANTS／MATERIAL_PROFILES 転記値、④素材検証レポート（**8種構成** — チェックリスト判定＋例外事項＋81配置3条件検査結果＋書き出し前検査1〜7・M・F・コミット前検査8＋独立 F oracle／mutation 5種の記録＋**§7-8 のロゴ／ラインストーン目視結果と variant 色回帰結果**＋fixedVisualCaseCount のチャーム別内訳＋swan の所有権分類記録〔§4-1c〕）

## 8. フェーズ計画（改訂14 — 実装フェーズの正本は同期計画の Phase 3〜7）

実装の進行・コミット単位・handoff・開始時検証の正本は**同期計画（plans/charm-combo-codex-claude-sync-plan.md）の Phase 3〜7 と §2-4 の commit chain 契約**とする。本計画書の旧フェーズ0〜4は次の対応で吸収される（技術ゲートの中身 — §7 チェックリスト・§9 受け入れ基準 — は本計画書が引き続き正本）。

**技術値の優先関係（override map — 同期計画に残る旧色契約の再流入防止）**: 同期計画の権限は**進行機構（Phase 順序・commit chain・handoff・専用 worktree・還流票）に限定**し、**技術値（素材・色・検査・数量）は本計画書 revision 14 が優先**する。特に同期計画の次の記述は本計画書の現行契約へ読み替える（同期計画自体は Phase 1 完了時点の文書として不変のまま保存する）:

| 同期計画の記述 | 読み替え（本計画書 rev14） |
|---|---|
| §8-3「frenchie 差し替え時の referenceIvory 再算出・全 flat DISPLAY_COLORS 再導出・F-3/F-4/8 再実施」 | **発生しない**（§5-6 — referenceIvory / DISPLAY_COLORS は撤回済み。DISPLAY_VARIANTS は freeze 定数のため frenchie 再生成に色は連動しない。F-3/F-4 は改訂14の定義〔material profile 整合・DISPLAY_VARIANTS 整合〕で実施） |
| §10-3「既存24色の referenceIvory／旧 DISPLAY_COLORS 互換値」の検証 | **DISPLAY_VARIANTS（27件 approvedHex）の転記照合**（§5-6・検査F-4・`?debug=1`）へ置換 |
| §11-0「referenceIvory と DISPLAY_COLORS の正本値」参照 | **DISPLAY_VARIANTS / MATERIAL_PROFILES の正本値**（§5-6）へ置換 |
| 「216有限値」「8種×27 variant の216組」「approvedCount=216」（§2-1・§5-5 PASS 式・§6・§10・§11・**§14 最終受け入れ条件**の各所） | **192有限値・192組・approvedCount=192**（AE-20260718-10 の適用対象制限 — 同期計画 §5-1 の 8 が自ら規定する `Σ variants.appliesToCharms.length` 再計算分岐の適用結果。§0-A。Phase 7 の submissionReady / productAccepted・§11-4 final acceptance の count 判定もすべて 192 基準） |
| §6-2「DISPLAY_COLORS は…referenceIvory 互換を維持しつつ、最終的には approvedHex を持つ DISPLAY_VARIANTS へ移行」 | **移行完了**（改訂14が「最終」状態 — 互換維持の中間段階は経ない） |
| §9-1「4面比較の4面目 = 新派生アセットの**シェーブル代表色**表示」・§9-2「material 切替で外形・alpha・ロゴ・保持装飾が変わらず…」の material 切替合格条件 | **4面目 = calf:black（`#41403d`）の固定表示へ置換**（chevre 適用チャーム0のため決定的に実行可能な代表暗色を採用 — appliesToCharms が非空の material が2種以上になった再改訂時に本来のシェーブル代表色表示へ戻す）。**material 切替不変性検査は単一 material の現行では N/A**（chevre 適用アイテム追加の再改訂で有効化。§9-5 の比較成果物・shape-approval.json もこの4面定義で記録する） |
| Phase 1 承認台帳（reports/charm-combo/rev14/phase-1-approvals.md）末尾の補足「チャーム表示上の DISPLAY_COLORS では改訂13 §5-6 のとおり referenceIvory（frenchie実物色基準）へ差し替えられる（COLORS 正本値とUI表示の二層構造は変更なし）」 | **失効**（当該補足は改訂13時点の説明を残した履歴記述であり実行契約ではない。台帳は Phase 1 ancestry として凍結済みのため書き換えず、本 override で機械的に失効させる。**後勝ちは AE-20260718-10〜12 と color freeze の approvedHex** — アイボリーの表示も DISPLAY_VARIANTS の approvedHex `#ece7dd` そのもの・二層構造は存在しない） |
| §6-2「prep 確定後に `fixedVisualCaseCount` とチャーム別内訳を検査レポート・**v2指示書へ**記録する」 | **検査レポート（§7 Phase 4 成果物④）と実装最終報告（v2 §9-1）への記録で代替**（v2 指示書は revision lock の documents 3点に含まれ Phase 4 時点で SHA 固定済みのため、事後追記は lock 照合を破壊し実行不能 — locked 文書への追記は行わない） |

| 同期計画 Phase | 旧フェーズ対応 | 内容（進行・handoff 詳細は同期計画の当該節） |
|---|---|---|
| Phase 3 ツール同期 | （新設） | `tools/charm-prep.html` を改訂14契約へ更新: INPUT_MANIFEST 8種（§4-1c）・COLOR_REFERENCE_MANIFEST・LEATHER_VARIANTS 27・MATERIAL_PROFILES 2・schemaVersion 5・toolVersion 0.6.0・swan の CHARM_SPECS 登録。selftest=1 全通過 → toolCommit（同期計画 §7） |
| Phase 4 全8種再生成 | 旧フェーズ0 | canonicalSource 8点から派生アセット一式を再生成（§5-4 検査1〜8・M・F・straight PNG・独立 F oracle・mutation・6ストーン位相・§7 チェックリスト・**幾何パラメータ固定 → 81配置3条件検査〔§7-11〕**・fixedVisualCaseCount の確定）。**§7 の「免除不可」項目は8種すべて合格が必須・swan の stitchMode をここで確定** → assetCommit（同期計画 §8） |
| Phase 5 形状・意匠QA | 旧フェーズ0 承認ゲート | canonicalReference との4面比較・形状/意匠の承認（色承認と分離・**swan ロゴ立体陰影の見た目の最終判断を含む** — AE-20260718-07）。素材検証レポート8種構成の承認 → shapeApprovalCommit（同期計画 §9） |
| Phase 6 charms.html 完成 | 旧フェーズ1〜3 | 代表版 charms.html を8種＋なしへ拡張: variant/material 2段UI・DISPLAY_VARIANTS・cache key（§5-7）・世代管理・サマリー・§9 受け入れ基準 0〜13 → charmsCommit（同期計画 §10）。テイスト調整で LAYOUT・丸カンアセット・N を変更した場合は §7-11 の81配置検査を最初から再通過（§5-5） |
| Phase 7 色クライアント確認 | （新設） | 8種 × 適用可能 variant = **192組**（§0-A）の確認シートを提出（同期計画 §11） |
| （別工程） | 旧フェーズ4 | `index.html` への逆リンク追加はメインセッション担当（実装エージェントは行わない） |

**読み替え規則**: 本計画書の他節に残る「フェーズ0」「フェーズ1」「フェーズ2」「フェーズ3」「フェーズ4」の呼称は上表の対応で同期計画 Phase へ読み替える（フェーズ0 → Phase 4／フェーズ1〜3 → Phase 6／フェーズ4 → 別工程。文中の「フェーズ0停止」「フェーズ0不合格」等のゲート表現は Phase 4 のゲートを指す）。

- 実装委任時は **v2 実装指示書（plans/impl-instructions-charm-combo-display-v2.md）**の規約（ブランチ・停止チェックポイント・日本語報告・同期計画 §2-4 の commit chain）に従う。旧指示書（plans/impl-instructions-charm-combo-display.md）は SUPERSEDED — 実行禁止
- **改訂14に伴う再実施の総括**: 改訂13 §8 の「photo 6種は画像の再生成不要」条項は**撤回**（§0-B-3）。**全8種を schemaVersion 5・straight PNG 契約で再生成**し、旧派生アセット・旧 prep.json・旧 manual レイヤ（旧素材の作業座標系由来）は流用しない。81配置3条件検査・§9-2 実寸比率検証・素材検証レポートも8種構成で新規取得する
- 計画外の変更が必要になった場合は **Codex 変更還流票（同期計画 §12）**を提出して停止し、Fable 5 の再改訂・再ロック（同 §12-1・§12-2 の restartPhase 判定）を経てから再開する

## 9. 受け入れ基準

0. **派生アセット検査**: 8種すべてが §5-4 の書き出し前検査1〜7・M・F＋コミット前検査8（再生成一致・straight PNG 独立デコード・独立 F oracle・mutation 5種）に通過している（Phase 4 成果の再確認）。**CHARMS テーブル値と prep.json の転記一致（§5-2 の10項目の完全列挙）、LEATHER_VARIANTS 転記8項目・MATERIAL_PROFILES 転記6項目と color freeze の一致を `?debug=1` の機械照合で確認**（§0-A）
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
| 保持画素（ラインストーン等）の視覚品質 | 保持は素材の色をそのまま表示するため革色変更時に浮いて見える可能性 → Phase 4 の §7-8 目視（horseshoe 8ケース: 革色を変えてもストーンが自然か。swan のくちばし・金具鋲も同様）で確認し、素材検証レポートに記録 |
| 保持チャンネルの α=0 画素で masks.RGB が PNG デコード時に壊れる | 保持判定は masks.α のみで行い保持画素の RGB は読まない設計（§5-2）。恒等式検査1も保持画素の RGB を不問とし、**baseLum・bboxPx・ステッチ集計・クリップ等の全所有権参照は共通述語 `masks.α≥128` で保持画素を除外**（§5-2 — ラインストーンが革コア・革bbox・ステッチ所有に誤算入されない） |
| 箔押しロゴ「&.」の塗り分け漏れ（小面積） | 準備ツールの拡大表示＋manual-owner 手動補正で確定。漏れると革リカラー時にロゴが革色に染まるため、固定検証セット目視（§9-3）の確認項目に含める |
| 濃色リカラーの白飛び | colorless の flatLeatherTone はハイライト保護なし（白面基準・§5-6）で構成的に回避。photoLeatherTone のハイライト保護は将来 photo 素材用の予備契約 |
| 金箔ロゴ・ラインストーンの彩度が革と近く自動分類しにくい | 準備工程の手動ブラシ補正（manual-owner / manual-keep）で確定（自動分類の精度に依存しない）。それでも塗り分け不能なら素材再提供の依頼（除去はしない・§5-6） |
| 実寸データ・校正の誤り | 実寸はストア商品ページ記載で確定済み・7種は実測値（2026-07-13）とも完全一致（§4-1。swan は実測未受領 — 歪み>3% で停止・§7-9①）。素材側の校正は calibration 契約（斜め素材 osanpo は手動計測線モード必須・§5-2）＋pxPerMm歪み検査。誤りが見つかった場合の修正は**準備ツールの入力（sizeMm・sizeMmMeasured・calibration）と prep.json 側で行い、派生アセット再生成＋検査1〜8 再実行 → CHARMS の転記照合10項目を再転記して `?debug=1` 照合 → §7-11 の81配置・§9-2 の実寸比率を再検証（§5-5）**という経路のみ（**CHARMS 単独修正は禁止** — 正本 prep.json との一致契約を破るため） |
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
| swan の実測寸法が未受領のまま歪み>3% になる | sizeMmMeasured.confirmed=false のためアスペクト補正は機械的に発動せず（§5-2 発動述語）、実測受領まで停止（§4-1・§7-9①） |

## 11. 確認ポイント（レビューのお願い）

**1〜5・7〜11 は回答受領済み（2026-07-13）、12 は承認済み（2026-07-15）、13（swan）は改訂14で解消（Phase 1 承認 AE-20260718-07/08/09 — ⑤stitchMode と④実測値のみ Phase 4 以降へ持ち越し）、14（Phase 1 の素材・色凍結）は 2026-07-18 承認済み**。6（左右同一チャーム可）のみ明示回答なし＝既定案（選択可能）のまま進行（異議があれば実装フェーズ開始前までに）。

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
    - ④実寸の実測値: **未受領のまま**（ストア記載 90×90・sizeMmMeasured.confirmed=false — 歪み>3% ならアスペクト補正は発動せず実測受領まで停止・§4-1・§7-9①）
    - ⑤stitchMode: **Phase 4 で実画素から §5-2 完全列挙で判定**（detect / fallback なら UI 注記・§9-3 ケース数 44 へ更新、none なら 40）
14. **Phase 1 の素材・色凍結**: ✅ **承認済み（2026-07-18・AE-20260718-01〜14）** — pipelineSource=フチ画像×8・approvalReference=フチ画像兼用・カーフ24 approvedHex=チャート抽出値・シェーブル3=P3変換値・シェーブル専用 grain=シェーブル質感.jpg・カーフ grain=質感.jpg 再利用・シェーブル3 variant は現行チャームで選択不可（192有限値）・シエルブルー canonicalLabel。詳細は reports/charm-combo/rev14/phase-1-approvals.md

## 12. 承認後の流れ

1. ~~本計画書を `/codex-review` で反復レビューし収束させる~~ **改訂7まで済（改訂5: 反復5回／改訂6: 反復4回／改訂7: 反復4回 — いずれも `ok: true` 収束・2026-07-12〜13）**。**改訂14は同期計画 §6-5 の revision lock 手続き（arch / diff / cross-check の3フェーズレビュー・全 ok:true・lock manifest）で確定する**
2. ~~素材の凍結~~ **済（Phase 1・2026-07-19 未明・sourceFreezeCommit `8b3bdb4`＋phase-1 evidence `e76f6e8`・§4-1c）**
3. ~~ナミさんの確認・修正反映~~ **§11 の 1〜5・7〜14 は回答受領・反映済み**（残: 6 は既定案進行・13④ 実測値・13⑤ stitchMode は Phase 4 以降）
4. ~~実装指示書の作成~~ **v2 実装指示書（plans/impl-instructions-charm-combo-display-v2.md）を改訂14と同時に作成**。旧指示書は SUPERSEDED（DO NOT EXECUTE）
5. revision lock 成立後、実装委任 → **同期計画 Phase 3（ツール同期）から順に**確認サイクル（§8）
6. **改訂14の取り込み**: 実装エージェント（Codex）は selectedPhase2EvidenceCommit を起点とする専用 worktree（同期計画 §7-0）で作業し、Phase 3 の開始条件（revision lock 照合 PASS・dirty worktree 保全）を満たしてから着手する。v2 指示書と本計画書が矛盾する場合は**本計画書（改訂14）を優先**し、矛盾内容を報告する
