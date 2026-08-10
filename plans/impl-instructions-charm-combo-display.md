<!-- SUPERSEDED-METADATA-BEGIN
status: SUPERSEDED — DO NOT EXECUTE
supersededAt: 2026-07-18
successorPlan: plans/charm-combo-display-plan.md revision 18
successorInstructions: plans/impl-instructions-charm-combo-display-v2.md
lockManifest: plans/charm-combo-revision-lock.json
SUPERSEDED-METADATA-END -->

# チャーム組み合わせ表示 実装指示書（Opus 4.8 / Codex 向け）

> **本指示書は失効済み（status: SUPERSEDED — DO NOT EXECUTE）**。改訂14以降の実装は successorInstructions（plans/impl-instructions-charm-combo-display-v2.md・現行は改訂18対応）を使用すること。以下の本文（改訂8時点・7種/168組/64配置/転記9項目）は監査証跡としてのみ保存し、**いかなる数量・手順も実行契約として解釈しない**（同期計画 §2-1）。

## 0. 依頼概要

あなたは、革小物ブランド「maison &.」カラーシミュレーターの**チャーム組み合わせ表示ページ**の実装を担当します。

- **作るもの**（いずれも新規。既存ページの改修ではない）:
  1. `tools/charm-prep.html` — 元写真から派生アセットを生成・検証する準備ツール（ブラウザ自己完結）
  2. 派生アセット一式 — `source-photos/`（元写真8枚のコピー）・`assets/charms/`（7種の base/masks/prep.json/manual-*）・`assets/rings/`（丸カン透過PNG 銀/金）
  3. `charms.html` — 左右チャーム（7種＋なし）× 革カラー24色 × 金具（丸カン銀/金）× ロゴカラー（金/銀）× ステッチ（白/黒）のプレビューページ
- **技術仕様の正**: `plans/charm-combo-display-plan.md`（529行・改訂8。起点ブランチにコミット済み）
  - この計画書は **Codexレビュー計13反復で収束済み（改訂5=5回・改訂6=4回・改訂7=4回、いずれも最終判定 ok: true）**。記載された契約・数式・検査・文言を**そのまま**実装すること
  - 本指示書は運用ルール（ブランチ・チェックポイント・検証・報告）と、計画書が実装指示書送りにした少数の確定値（本指示書 §5〜7）のみを定める。技術的な内容で本指示書と計画書が矛盾したら**計画書を優先**し、矛盾に気づいたことを報告する
- **実装前に計画書全文を精読すること**（特に計画書 §5-2, §5-4, §5-5, §5-6, §7, §9 は実装の核心）

## 1. ブランチ運用と禁止事項

- 起点ブランチ: `claude/charm-combo-display-plan-444a97`（計画書改訂8 コミット d4f2638 を含む。**ローカルのみ・リモートには存在しない**）
- そこから**自分専用の作業ブランチ**を切って作業する:
  - Opus 4.8 → `feature/charm-combo-opus`
  - Codex → `feature/charm-combo-codex`
- コミットは実装ステップ単位で細かく。メッセージは英語（リポジトリ慣行に合わせる）
- **禁止事項（厳守）**:
  - `index.html`・`bags.html` の変更（1バイトも変えない。`index.html` への逆リンクナビは計画書 §8 フェーズ4＝メインセッション担当）
  - main へのマージ・リモートへの push（ローカルコミットのみ。push はユーザー指示待ち）
  - フレームワーク・ビルドツール・npm の導入（`charms.html`・`tools/charm-prep.html` とも1ファイル自己完結・vanilla JS を維持）
  - 計画書の契約（派生アセット契約・所有権恒等式・共通述語・検査1〜8・calibration・配置式・stitchMode・UI文言）の独自変更。実装不能と判断した場合は**勝手に代替せず、停止して理由を報告**する
  - **`CHARMS` テーブルの単独修正**（計画書 §10: 定義値の誤り修正は必ず「準備ツールの入力・prep.json 側で修正 → 派生アセット再生成＋検査1〜8再実行 → 転記照合9項目を再転記 → `?debug=1` 照合」の経路のみ）
  - 計画書ファイル自体の編集（矛盾・誤記を見つけたら停止・報告。修正はメインセッション側で行う）

## 2. 元写真の所在（最初に確認すること）

元写真8枚は **Git 管理外**の次の絶対パスにある（untracked のネストディレクトリ内。**ブランチを checkout しても worktree を作っても現れない**）:

```
/Users/ah266/nmrn0629-maisonampersand.colorcheckboard/nmrn0629-maisonampersand.colorcheckboard/charm一覧/
```

- 内容: `horse.jpg` / `horseshoe.jpg` / `frenchie.jpg` / `dachshund.jpg` / `toy poodle.jpg` / `osanpo.jpg` / `cat.jpg` / `swan.jpg`（計8枚）
- フェーズ0の最初に、ここから作業ツリーの `source-photos/` へ**コピー・リネーム**して**コミット**する（対応表は計画書 §4-1。`toy poodle.jpg` → `toy-poodle.jpg` の**スペース除去**を忘れない。swan.jpg は保管のみ＝派生アセットは作らない）。以後の工程は Git 管理下の `source-photos/` だけを参照する
- **上記パスが見えない・8枚未満・破損している**場合は、自力で別の場所を探し回らず**停止してユーザーに報告**する

## 3. 実装の進め方（計画書 §8 のフェーズ0〜3を順に実施）

フェーズ4（`index.html` への逆リンク）は実装対象外（メインセッション担当）。以下の停止チェックポイントは**必須**（ユーザーの確認を得るまで次工程に進まない）。

- **チェックポイント①（フェーズ0冒頭・必須停止）**:
  `source-photos/` へのコピー・リネーム・コミットと実寸法の計測（`sips -g pixelWidth -g pixelHeight`）まで行い、**8枚の実測寸法と計画書 §4-1b の解像度表との照合結果**を報告してユーザーの確認を待つ。以降の全派生アセットがこの写真に依存するため、ここで取り違えると全作業が無駄になる（1枚でも不一致があれば停止して指示を仰ぐ）
- **チェックポイント②（フェーズ0完了時・必須停止）**:
  素材検証レポート（計画書 §7 チェックリスト全11項目の判定・書き出し前検査1〜7とコミット前検査8の結果・stitchMode 確定値・64配置クリップなし検査の結果・§7-8 ロゴ／ラインストーン目視結果・例外事項）を報告し、**再撮影要否・例外承認の確認を得るまでフェーズ1に進まない**（計画書 §8 フェーズ0ゲート。「免除不可」項目の不合格は例外承認で通過できない）。**「免除可」項目（計画書 §7 の 2・5・6）で例外が発生した場合は、ユーザー承認を得た後も停止を継続し、メインセッション側で影響する仕様・受け入れ基準が計画書に反映され再レビュー（ok: true）が済んだ連絡を受けるまでフェーズ1に進まない**（計画書 §8 の要求。実装エージェントは計画書を編集しない — 本指示書 §1）
- **チェックポイント③（フェーズ1完了時・必須停止）**:
  代表1チャーム（写真品質が中庸なものを選び、**選定理由を報告**）の一気通貫実装（ロード → リカラー〔革・ロゴ・ステッチ・保持〕→ 丸カン非接続合成）と、`?debug=1` 検査・photoLeatherTone 有限値検査（本指示書 §7・**7種×24色=168組**）の通過を報告し、**見た目のテイストについてユーザーのOKを得てから**フェーズ2へ（計画書 §8 フェーズ1ゲート）。スクリーンショットが取れる環境なら添付する。テイスト調整で LAYOUT・丸カンアセット・描画解像度 N を変更した場合は**64配置検査を再通過してから**報告する（計画書 §5-5）
- フェーズ2完了（計画書 §9-1〜9-5 通過）は報告のみで停止不要。フェーズ3完了時に最終報告（本指示書 §9）を行い、ユーザー最終確認を待つ
- `?debug=1` の自動検査（恒等式・転記照合・有限値・グリッド）は**リカラーの目視チューニングより先に**実装する（調整効率がここで決まる。ミニバッグ実装の実績どおり）

## 4. 計画書で特に落としやすい契約（該当節を必ず読むこと）

| 契約 | 計画書の節 |
|---|---|
| masks.α は**保持フラグ**（読み込み判定 α<128 = 保持）。**保持判定はαのみで行い、保持画素の masks.RGB は読まない** | §5-2 |
| **所有権参照の共通述語 `masks.α≥128`**: baseLum・bboxPx・ステッチ画素集計・クリップ領域など、所有権を参照する派生値・検査の集計は全てこの述語込み（保持画素を誤算入しない） | §5-2 |
| 所有権恒等式は3条件（base.α>0 ∧ masks.α≥128 ⇒ R+G+B=255 ／ base.α>0 ∧ masks.α<128 ⇒ 保持〔RGB不問〕 ／ base.α=0 ⇒ masks.α=255 ∧ R=G=B=0） | §5-2 |
| フェザー帯（0 < base.α < 255）も必ず所有（または保持）を持つ。出力α = base.α のまま（**アルファの二重減衰禁止**） | §5-2 |
| baseLum は**下位中央値**（コア画素の lum 昇順・0始まり添字 `floor((n-1)/2)`）。コア画素0件は書き出しエラー | §5-2 |
| calibration の**正本は prep.json のみ**（CHARMS へ転記するのは採用値 pxPerMm だけ）。**斜め素材 osanpo は manual-line モード必須** | §5-2 |
| 検査5（ステッチ整合）は**完全列挙型ゲート**: detect / none / fallback の条件セット不合致・未知の stitchMode 値は即不合格 | §5-2・§5-4 |
| stitchMode=none は「ステッチトグルが出力に影響しない」ことを**機械保証**（masks.B 所有画素数=0 ∧ 白黒切替の出力差分画素数=0） | §5-2・§5-6 |
| detect のステッチは**選択色（白/黒）を問わず検出画素を明示的に塗り直す**（「白選択時は原画のまま」という元糸色依存の実装にしない） | §5-6 |
| 配置式は軸分離: **y = 前景（fgBboxPx）上端基準・x = 革本体（bboxPx）基準**。丸カンとは非接続（`ringGapMm` の間隔・重ならない） | §5-5 |
| チャーム完成ビットマップは**アセット解像度で生成・キャッシュ**し、ステージへは等倍率 `N / pxPerMm` の drawImage 一発（ステージ座標系でのピクセル操作禁止） | §5-2・§5-5 |
| キャッシュキーは「チャーム × 革色 × ロゴ × ステッチ」。**丸カンの金具トグルは含めない**。LRU 8エントリ・退避時 `ImageBitmap.close()` で明示解放 | §5-7 |
| 描画に**世代トークン**を付与し、素早い連続切替で古い非同期結果が最新状態を上書きしないことを保証 | §5-7・§9-9 |
| UI表示名は**画像ファイル名と同じ英語表記**・サマリー文は §6-3 の形式を**一字一句**反映（ロゴ表記=チャーム1点以上表示中のみ・ステッチ表記=ステッチ有チャーム表示中のみ） | §4-1・§6-3 |
| トグル有効条件と無効時表示（ステッチ=ステッチ有チャーム表示中のみ有効・無効時は「ステッチ対応: horseshoe / osanpo」注記／ロゴ=1点以上表示中のみ有効／【なし】側スウォッチのグレーアウト） | §4-2・§6-1 |
| **転記照合9項目**（sizeMm・bboxPx・fgBboxPx・pxPerMm・attachmentMode・threadColor・stitchMode・stitchPaths・baseLum）を `?debug=1` が prep.json を読み込んで機械照合。**通常表示では prep.json を読まない** | §5-2・§5-4・§9-0 |

## 5. コード例・定義例（計画書が実装指示書送りにした確定値）

### 5-1. photoLeatherTone — b29274f の係数にピン留め（計画書 §5-3）

参照実装（`ミニバッグ実装テスト-opus` ブランチのコミット b29274f・`bags.html`。`git show b29274f:bags.html` で参照可）の係数を**そのまま**使う。チャームでの呼び出しは**全箇所で4引数・shadeFactor=1.0 固定**（第4引数を省略すると `ratio *= undefined` で NaN になる — 計画書 §5-3）:

```js
function clamp(v, min = 0, max = 255) {
  return Math.max(min, Math.min(max, v));
}
// 係数ピン留め（b29274f）: ratioクランプ域 [0.35, 1.60]・ゼロ除算ガード 0.001・
// ハイライト保護 lum≥0.94・ブレンド上限 0.38・遷移幅 0.06
function photoLeatherTone(base, target, baseLum, shadeFactor) {
  const lum = (0.2126*base.r + 0.7152*base.g + 0.0722*base.b) / 255;
  let ratio = Math.max(0.35, Math.min(1.60, lum / (baseLum || 0.001)));
  ratio *= shadeFactor;
  let r = target.r * ratio, g = target.g * ratio, b = target.b * ratio;
  if (lum >= 0.94) {
    const blend = Math.min(0.38, (lum - 0.94) / 0.06 * 0.38);
    r = r*(1-blend) + 255*blend; g = g*(1-blend) + 255*blend; b = b*(1-blend) + 255*blend;
  }
  return { r: clamp(r), g: clamp(g), b: clamp(b) };
}
// チャームでの呼び出し（全箇所共通）:
//   photoLeatherTone(basePixel, targetColor, charm.baseLum, 1.0)
```

- フェーズ1の実写チューニングで係数を変えたい場合は、**変更値と理由を報告してユーザー確認を得てから**変える（計画書 §10「濃色リカラーの白飛び」対策の範囲内。変えた場合は最終報告 §9-3 に最終値を記載）
- `metalTone()` は既存 `index.html` のものを**そのまま流用**（計画書 §5-3。適用先はチャームの箔押しロゴ〔masks.G × ロゴカラートグル〕のみ。丸カンは事前生成PNGの差し替えであり実行時に metalTone は適用しない）

### 5-2. LAYOUT オブジェクト（幾何の正本・計画書 §5-5）

ステージ幾何は次の1オブジェクトに集約する（値は計画書 §5-5 の確定値・仮値）:

```js
const LAYOUT = {
  stageMm:       { w: 220, h: 155 }, // 仮値。フェーズ0の64配置クリップなし検査で確定（はみ出せば更新して再検査）
  pxPerMmStage:  6,                  // 描画解像度 N（初期値6 → 1320×930px。フェーズ1の性能実測で確定）
  ringCenterXMm: 110,                // ステージ幅/2（stageMm.w を更新したら追従させる）
  ringCenterYMm: 31,                 // 上マージン10 + 丸カン半径21（固定値）
  ringOuterMm:   42,                 // 丸カン外径（実寸厳守・ストア一次ソース確認済み）
  gapMm:         8,                  // 革本体間ギャップ（2点表示時）
  ringGapMm:     3,                  // 丸カン下端とチャーム前景上端の間隔（非接続レイアウト）
};
```

- `stageMm`（64配置検査で確定）・`pxPerMmStage`（フェーズ1で確定）以外は確定値。**64配置検査（計画書 §7-11）の実行前に全値を固定**し、以後の変更（フェーズ1のテイスト調整を含む）は検査再通過が条件（計画書 §5-5「幾何パラメータの固定順序」）
- 配置式は計画書 §5-5 の表のとおり（y: 前景上端 = `ringBottomY + ringGapMm`、x: 2点時は革本体端が `cx ∓ gapMm/2`・1点時は bboxPx 中心x = cx。`ringBottomY` = 丸カン中心y + 21mm）

### 5-3. リカラー合成ループの骨格（計画書 §5-2 の合成式の実装形）

```js
// 描画ジョブ: 入力イベントごとに世代を1回だけ進め、全選択状態の不変スナップショット renderJob を
// 捕捉する。キャッシュキーの生成と合成ループは各側の bitmapJob だけを読む（アセット遅延ロード中に
// 選択が変わると、グローバル state を途中で読む実装ではキーと合成内容が食い違ったビットマップが
// キャッシュされ得る — 世代トークンでは防げない）
const gen = ++renderGen;   // 世代トークンは入力イベントごとに1回だけ進める（計画書 §5-7・§9-9。
                           // 左右の bitmapJob で ++ を繰り返すと片側が即座に旧世代になり §9-9 を満たせない）
function makeBitmapJob(side) {
  if (side.charm === "none") return null;
  return {
    gen,
    charmKey: side.charm, charm: CHARMS[side.charm],
    colorHex: side.color.hex, target: hexToRgb(side.color.hex),
    logo: state.logo, stitch: state.stitch,      // state を読むのはこの捕捉時の1回だけ
  };
}
const renderJob = {
  gen,
  metal: state.metal,                            // 丸カンPNGの選択にのみ使用（チャームビットマップには不使用）
  left: makeBitmapJob(state.left), right: makeBitmapJob(state.right),
};
// キャッシュキーは bitmapJob のみから生成（金具は含めない — 計画書 §5-7）
const cacheKey = (job) => `${job.charmKey}|${job.colorHex}|${job.logo}|${job.stitch}`;
// 左右の非同期処理（アセットロード＋リカラー）がすべて完了した時点で renderJob.gen === renderGen を
// 照合し、最新なら renderJob.metal の丸カンPNGとともにステージへ合成する（旧世代なら描画せず破棄）

// 片側の合成ループ（job = 上記 bitmapJob）。base / masks とも同寸の ImageData（アセット解像度）。出力 out も同寸
for (let i = 0; i < base.data.length; i += 4) {
  const bA = base.data[i + 3];
  out.data[i + 3] = bA;                    // 出力α = base.α（再減衰・再適用しない）
  if (bA === 0) continue;                  // 所有なし画素（masks は α=255・RGB=0 の契約）
  if (masks.data[i + 3] < 128) {           // 保持判定はαのみ（masks.RGB は読まない）
    out.data[i]     = base.data[i];        // 保持: base.RGB をそのまま（ラインストーン等）
    out.data[i + 1] = base.data[i + 1];
    out.data[i + 2] = base.data[i + 2];
    continue;
  }
  const wR = masks.data[i] / 255, wG = masks.data[i + 1] / 255, wB = masks.data[i + 2] / 255;
  const b = { r: base.data[i], g: base.data[i + 1], b: base.data[i + 2] };
  const leather = photoLeatherTone(b, job.target, job.charm.baseLum, 1.0);
  const logo    = metalTone(b, job.logo);      // ロゴカラートグル連動（金具トグルではない）
  const stitch  = stitchColor(b, job.stitch);  // 選択色を問わず塗り直す（§5-6）
  out.data[i]     = leather.r*wR + logo.r*wG + stitch.r*wB;  // R+G+B=255 なので係数和=1
  out.data[i + 1] = leather.g*wR + logo.g*wG + stitch.g*wB;
  out.data[i + 2] = leather.b*wR + logo.b*wG + stitch.b*wB;
}
```

```js
// ステッチ色の実装例（白 #f5f5f4 / 黒 #1a1a16 の固定色＋輝度による僅かな陰影・計画書 §5-2）。
// 「白選択時は原画のまま」にはしない（計画書 §5-6）。陰影係数はフェーズ1で微調整可（変更時は §9-3 で報告）
function stitchColor(base, sel) {
  const lum = (0.2126*base.r + 0.7152*base.g + 0.0722*base.b) / 255;
  const c = sel === "white" ? { r: 245, g: 245, b: 244 } : { r: 26, g: 26, b: 22 };
  const k = 0.85 + lum * 0.3;
  return { r: clamp(c.r*k), g: clamp(c.g*k), b: clamp(c.b*k) };
}
```

- 完成ビットマップのキャッシュキーは上記のとおり **job スナップショットのみ**から生成する（**金具は含めない** — 丸カンはチャームビットマップに影響しないため。計画書 §5-7）
- 革カラー24色（COLORS 配列）とグループUIは `bags.html` の定義をそのまま流用する

## 6. 期待出力ハッシュ照合の実施手順（コミット前検査8・計画書 §5-4）

「デコードRGBAの SHA-256」は次の手順で算出する（準備ツール内に実装）:

1. 対象PNG（書き出し済み `*.base.png` / `*.masks.png`）を `createImageBitmap`（または `<img>`）でロード
2. 同寸の canvas に `drawImage` → `getImageData` で全画素 RGBA（`Uint8ClampedArray`）を取得
3. `crypto.subtle.digest("SHA-256", imageData.data)` の結果を hex 文字列化（localhost は secure context のため `crypto.subtle` が使える）
4. prep.json の期待出力ハッシュと比較

運用上の注意（落としやすい）:

- **ハッシュの算出と照合は同一ブラウザ内で完結させる**。canvas のプリマルチプライ往復により α<255 画素の RGB 丸めがブラウザ・OS 間で異なり得るため、異なるブラウザで算出したハッシュ同士を照合してはならない。再現性の担保のため **prep.json に算出ブラウザ名・バージョンを記録**する（検査8を別環境で再実行して不一致になった場合は、まず同一ブラウザでの再実行で切り分ける）
- canvas 書き出し（`canvas.toBlob`）では α=0 画素の RGB が 0 に潰れるが、保持画素・所有なし画素の RGB は「読まない」契約（計画書 §5-2）なので機能上の問題はない。ハッシュは「潰れた後の決定的な値」同士で一致すればよい
- 三者照合（①入力照合: 元写真・manual-*.png のファイル SHA-256 ②出力照合: 書き出し済み base/masks のデコードRGBAハッシュ ③再生成照合: パイプライン①〜⑧の独立再実行結果との完全一致）は計画書 §5-4 検査8の定義どおり。**7種すべて合格するまで派生アセットをコミットしない**
- 入力照合（①）のファイル SHA-256 は `shasum -a 256` などファイルバイト単位でよい（デコードではなくファイル同一性の確認のため。ブラウザ内で行う場合は `File` の `arrayBuffer()` に対する `crypto.subtle.digest`）

## 7. photoLeatherTone 有限値検査（ゲート化・計画書 §9-13）

- `?debug=1` の自動検査項目に組み込む: **7種の baseLum（フェーズ0で確定した CHARMS の値）× 全24色 = 168組**の直積を検査し、不合格が1件でもあれば debug 表示に FAIL と対象組を出す。COLORS 要素は `{name, group, hex}` のため **`hexToRgb` で `{r, g, b}` に変換してから渡す**（本指示書 §5-3 の `job.target` と同じ扱い。未変換のまま渡すと `target.r` 等が undefined になり全組が NaN で必ず FAIL する）:

```js
for (const charm of Object.values(CHARMS)) {
  for (const color of COLORS) {
    const target = hexToRgb(color.hex);
    const o = photoLeatherTone({ r: 128, g: 128, b: 128 }, target, charm.baseLum, 1.0);  // 代表入力
    if (![o.r, o.g, o.b].every(Number.isFinite)) fail(`finite: ${charm.label} × ${color.name}`);
  }
}
```
- **フェーズ1の完了条件（チェックポイント③の報告項目）**: **168組すべて PASS**（計画書 §9-13「フェーズ1の必須単体検査」。CHARMS の7種全定義値はフェーズ0で確定済みのため、実行時処理が代表1チャームのみの段階でも本検査は7種分を実行できる）
- フェーズ3の最終報告で同じ168組の再実行結果を記載する（計画書 §9-13「フェーズ3で再確認」）

## 8. 検証

- ローカルHTTPサーバ（例: `python3 -m http.server 8088`）経由で確認する（`file://` 直開きは canvas が tainted になり不可。`charms.html`・`tools/charm-prep.html` とも同じ）
- `?debug=1` のプログラム検査（所有権恒等式・転記照合9項目・有限値検査・実寸グリッド・マスク可視化）を**全通過**させる
- 計画書 §9 の受け入れ基準 **0〜13** を上から順にセルフチェックする
- ブラウザでの目視確認・性能計測（計画書 §9-4）・モバイル Safari 実機確認（計画書 §5-7・§9-12）が環境的に実施できない場合は、**未実施項目を報告に明記**してユーザーの手動確認に委ねる（できたことにしない）

## 9. 実装報告（日本語で行うこと）

各チェックポイント（本指示書 §3）の報告に加え、最終報告には以下を必ず含める:

1. stitchMode の確定構成（7種それぞれ。見込み〔horseshoe / osanpo = detect・他5種 = none〕どおりか、fallback 採用の有無と理由）
2. 代表チャームの選定理由（フェーズ1）
3. チューニングした係数の最終値（photoLeatherTone 係数・stitchColor 陰影係数・描画解像度 N・LAYOUT 確定値。ピン留め値・初期値から変えた項目はユーザー確認を得た旨とあわせて）
4. 性能計測値（キャッシュ済み切替10回中央値・初回選択 warm cache〔相異なる2チャーム同時ロード〕）と計測環境
5. 計画書 §9 受け入れ基準 0〜13 のチェックリスト結果（未達成・未実施項目は理由つきで）
6. ピークメモリ概算（計画書 §5-7 の定義どおり）
7. 計画書からの逸脱（原則ゼロのはず。あれば事前確認を取った旨とあわせて）

## 10. リポジトリ内の参考ファイル

- `plans/charm-combo-display-plan.md` — **技術仕様の正（最初に全文精読）**
- `index.html` — `metalTone()`（流用可）・`extractKeyRingAsset()`（準備工程で丸カン透過PNG生成に1回だけ使用・計画書 §5-3）・内部ステッチ検出（準備ツールの自動候補生成に流用）・`drawSelectedKeyRing()`（丸カン独立表示の参照元）・CSS変数・スウォッチUI・`copySummaryText()`・`.loading` 表示。**参照のみ、変更禁止**
- `bags.html` — COLORS 24色定義・グループ選択UI・サマリー＋コピー・レスポンシブの流用元。**参照のみ、変更禁止**
- `シルバーキーリング.jpg` / `ゴールドキーリング.jpg` — `assets/rings/ring-silver.png` / `ring-gold.png` の生成元（42mm外径の共通基準枠に正規化・中心一致で書き出す。計画書 §5-3・§5-6）
- `ミニバッグ実装テスト-opus` ブランチ コミット b29274f の `bags.html` — `photoLeatherTone()` 参照実装・`?debug=1` 検査の流儀（`git show b29274f:bags.html`）
- `plans/impl-instructions-minibag-photo-recolor.md` — 本指示書の運用規約の前例（背景理解用）

以上。丁寧に実装してください。各チェックポイントでの停止と日本語報告を忘れずに。
