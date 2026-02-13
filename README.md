# X.com 検索クエリビルダー

X.com(旧Twitter)の高度な検索クエリを視覚的に組み立てるWebアプリケーション。

## 概要

このツールは、X.comの検索演算子を知らなくても、フォームから直感的に複雑な検索クエリを作成できます。生成されたクエリとURLをコピーして、X.comで検索したり、他の人と共有したりできます。

## 特徴

- 単一のHTMLファイルで動作(サーバー不要)
- リアルタイムでクエリを生成
- レスポンシブデザイン(PC/スマホ対応、大画面では2カラム表示)
- クエリとURLの両方をコピー可能
- ワンクリックでX.comへ遷移
- **プリセット機能**: よく使う検索条件を保存・読み込み
- **6つのデフォルトプリセット**搭載
- **モダンなUIデザイン**: パープルグラデーション + LINE Seed JPフォント
- **入力状態の視覚化**: 値が入力されているフィールドを強調表示
- **カテゴリ別整理**: コンテンツタイプをメディア・投稿タイプ・ユーザー品質・除外に分類

## 使い方

### 基本的な使い方

1. `index.html` をブラウザで開く
2. 検索条件をフォームに入力
3. 生成されたクエリ/URLが自動的に表示される
4. 以下のアクションを実行できる:
   - 「📋 クエリをコピー」: 検索クエリをクリップボードにコピー
   - 「🔗 URLをコピー」: 完全なURLをクリップボードにコピー
   - 「🔍 X.comで検索」: 新しいタブでX.comの検索結果を開く
   - 「🗑️ すべてクリア」: 全ての入力をクリア

### プリセット機能

**プリセットを読み込む:**
1. 「📂 プリセットを読み込む」ボタンをクリック
2. デフォルトプリセット一覧から選択
3. 検索条件が自動的にフォームに反映される

**デフォルトプリセット:**
- フォロー中のユーザー 最新順
- フォロー中のユーザー 画像付き
- リツイート 100 以上
- 今日のニュース(日本語)
- 人気の画像
- オリジナル投稿のみ

**プリセットを保存:**
1. 検索条件を入力
2. 「💾 現在の条件を保存」ボタンをクリック
3. プリセット名と説明(任意)を入力
4. 保存ボタンをクリック

保存したプリセットはブラウザのlocalStorageに保存され、次回以降も使用できます。

## 対応している検索条件

### 基本検索
- **キーワード** - 基本的なキーワード検索
- **完全一致フレーズ** - `"フレーズ"` でフレーズの完全一致
- **いずれかを含む(OR検索)** - `(word1 OR word2 OR word3)`
- **除外ワード** - `-word` で特定のワードを除外
- **ハッシュタグ** - `#hashtag`
- **株式シンボル** - `$SYMBOL`

### ユーザー関連
- **投稿ユーザー(from:)** - `from:username` 特定ユーザーの投稿
- **リプライ先(to:)** - `to:username` 特定ユーザーへのリプライ
- **メンション(@)** - `@username` 特定ユーザーのメンション

### 日付フィルター
- **開始日(since:)** - `since:YYYY-MM-DD` 指定日以降
- **終了日(until:)** - `until:YYYY-MM-DD` 指定日以前

### エンゲージメントフィルター
- **最小リツイート数** - `min_retweets:N`
- **最小いいね数** - `min_faves:N`
- **最小リプライ数** - `min_replies:N`

### コンテンツタイプ
- **filter:links** - リンクを含む投稿
- **filter:images** - 画像を含む投稿
- **filter:videos** - 動画を含む投稿
- **filter:media** - 画像または動画を含む投稿
- **filter:retweets** - リツイートのみ
- **filter:replies** - リプライのみ
- **filter:quote** - 引用ツイートのみ
- **filter:verified** - 認証済みアカウントの投稿
- **filter:follows** - フォロー中のユーザーの投稿
- **filter:safe** - センシティブコンテンツを除外
- **filter:spaces** - Spaces(音声チャット)のみ
- **filter:nativeretweets** - 通常のリツイート(引用なし)

### 除外フィルター
- **リツイートを除外** - `-filter:retweets`
- **リプライを除外** - `-filter:replies`

### ソート順
- **トップ(デフォルト)** - 人気度順
- **最新(live)** - 新着順(`&f=live`)
- **ユーザー(user)** - ユーザー検索(`&f=user`)
- **画像(image)** - 画像タブ(`&f=image`)
- **動画(video)** - 動画タブ(`&f=video`)

### 言語とその他
- **言語** - `lang:ja`, `lang:en` など
- **含むURL** - `url:example.com` 特定のURLを含む

## クエリ例

### 例1: フォロー中のユーザーのAI関連ツイート(画像付き、日本語、最新順)
```
AI filter:follows filter:images lang:ja
```
URL:
```
https://x.com/search?q=AI%20filter%3Afollows%20filter%3Aimages%20lang%3Aja&src=typed_query&f=live
```

### 例2: 特定ユーザーの人気ツイート(リツイート100以上、リプライ除外)
```
from:username min_retweets:100 -filter:replies
```

### 例3: 特定期間のハッシュタグ検索
```
#Python since:2024-01-01 until:2024-12-31 filter:links
```

## X.com検索演算子リファレンス

### 実装済み演算子

| 演算子 | 説明 | 例 |
|--------|------|-----|
| `keyword` | キーワード検索 | `machine learning` |
| `"phrase"` | 完全一致フレーズ | `"hello world"` |
| `word1 OR word2` | いずれかを含む | `cat OR dog` |
| `-word` | 除外 | `python -snake` |
| `#hashtag` | ハッシュタグ | `#AI` |
| `$cashtag` | 株式シンボル | `$AAPL` |
| `from:user` | ユーザーから | `from:jack` |
| `to:user` | ユーザーへ | `to:jack` |
| `@user` | メンション | `@jack` |
| `since:YYYY-MM-DD` | 開始日 | `since:2024-01-01` |
| `until:YYYY-MM-DD` | 終了日 | `until:2024-12-31` |
| `min_retweets:N` | 最小RT数 | `min_retweets:100` |
| `min_faves:N` | 最小いいね数 | `min_faves:500` |
| `min_replies:N` | 最小リプライ数 | `min_replies:10` |
| `filter:links` | リンク含む | `filter:links` |
| `filter:images` | 画像含む | `filter:images` |
| `filter:videos` | 動画含む | `filter:videos` |
| `filter:media` | メディア含む | `filter:media` |
| `filter:retweets` | RTのみ | `filter:retweets` |
| `filter:replies` | リプライのみ | `filter:replies` |
| `filter:quote` | 引用ツイート | `filter:quote` |
| `filter:verified` | 認証済み | `filter:verified` |
| `filter:follows` | フォロー中 | `filter:follows` |
| `filter:safe` | センシティブ除外 | `filter:safe` |
| `filter:spaces` | Spacesのみ | `filter:spaces` |
| `filter:nativeretweets` | 通常のRT | `filter:nativeretweets` |
| `-filter:retweets` | RT除外 | `-filter:retweets` |
| `-filter:replies` | リプライ除外 | `-filter:replies` |
| `lang:code` | 言語 | `lang:ja`, `lang:en` |
| `url:domain` | URL含む | `url:github.com` |

### 未実装の演算子(今後追加可能)

| 演算子 | 説明 | 例 |
|--------|------|-----|
| `near:"location"` | 場所の近く | `near:"Tokyo"` |
| `within:radius` | 距離範囲 | `within:15km` |
| `geocode:lat,lon,radius` | 位置情報 | `geocode:35.6762,139.6503,10km` |
| `max_retweets:N` | 最大RT数 | `max_retweets:10` |
| `max_faves:N` | 最大いいね数 | `max_faves:50` |
| `max_replies:N` | 最大リプライ数 | `max_replies:5` |
| `source:app` | 投稿元アプリ | `source:"Twitter Web App"` |
| `card_name:type` | カードタイプ | `card_name:summary` |
| `list:listid` | リストメンバー | `list:123456789` |
| `has:mentions` | メンション含む | `has:mentions` |
| `has:hashtags` | ハッシュタグ含む | `has:hashtags` |
| `has:links` | リンク含む | `has:links` |

## 実装済み機能

### v2.0.0 (2026-02-10)
- ✅ **プリセット機能**: 検索条件の保存・読み込み
- ✅ **デフォルトプリセット**: 6種類のよく使うプリセットを搭載
- ✅ **モダンUIデザイン**: パープルグラデーション配色
- ✅ **LINE Seed JPフォント**: 日本語に最適化されたフォント
- ✅ **全画面対応**: max-width制限を削除、大画面で2カラム表示
- ✅ **入力状態の視覚化**: 値入力時に自動で強調表示
- ✅ **チェックボックス装飾**: ON時にlabelを強調
- ✅ **カテゴリ分類**: コンテンツタイプを4カテゴリに整理
- ✅ **省スペース化**: padding/margin/font-sizeの最適化

## 今後の拡張案

### 優先度: 高
1. **検索履歴**
   - 最近作成したクエリを自動保存
   - 履歴から過去の検索を復元

2. **位置情報フィルター**
   - `near:"場所名"`
   - `within:距離`
   - `geocode:緯度,経度,半径`

3. **プリセットのインポート/エクスポート**
   - JSON形式でプリセットをエクスポート
   - 他のユーザーが作成したプリセットをインポート

### 優先度: 中
4. **エンゲージメントの最大値**
   - `max_retweets:`, `max_faves:`, `max_replies:`
   - 特定の範囲内の人気度で絞り込み

5. **追加の演算子**
   - `has:mentions`, `has:hashtags`, `has:links`
   - `source:アプリ名`
   - `list:リストID`

6. **URLパラメータからの自動読み込み**
   - URLに条件を含めてシェア
   - `?keywords=AI&lang=ja&filter=images` などで直接読み込み

7. **ダークモード**
   - トグルボタンでライト/ダーク切り替え
   - システム設定に追従

### 優先度: 低
8. **QRコード生成**
   - 生成されたURLをQRコードで表示
   - スマホでスキャンして検索

9. **クエリのインポート/エクスポート**
   - JSON形式で検索条件をエクスポート
   - 他の人が作ったクエリをインポート

10. **バリデーション強化**
    - 矛盾する条件の警告
    - 無効な日付範囲の検出

11. **プレビュー機能**
    - iframe内でX.comの検索結果をプレビュー表示
    - (CORS制限により実装困難の可能性)

## 技術仕様

### 構成
- **言語**: HTML5, CSS3, JavaScript(ES6+)
- **フォント**: LINE Seed JP (Google Fonts)
- **依存関係**: なし(Pure JavaScript、フォントのみCDN)
- **対応ブラウザ**: モダンブラウザ(Chrome, Firefox, Safari, Edge)

### ファイル構成
```
x-search-query/
├── index.html          # メインアプリケーション(全機能を含む単一ファイル)
├── README.md           # このドキュメント
├── CLAUDE.md           # 開発者向けガイド
├── IDEA.md             # 機能拡張アイディア集
└── PRESETS.md          # プリセット設計ドキュメント
```

### 主要機能の実装

#### リアルタイムクエリ生成
- 全ての入力要素に `input` / `change` イベントリスナーを設定
- 入力変更時に `updateQuery()` 関数を呼び出し
- クエリ文字列とURLを動的に生成して表示

#### プリセット機能
- localStorage使用で永続化
- デフォルトプリセット6個を内蔵
- ユーザープリセットの追加・削除
- プリセット読み込みでフォームに自動反映
- モーダルウィンドウでUI実装

#### 入力状態の視覚化
- `.has-value` クラスで管理
- JavaScript で動的に状態更新
- パープルの左ボーダー + 背景色変化
- チェックボックスON時はlabelを太字+色変更

#### クリップボードコピー
- `navigator.clipboard.writeText()` を使用
- 古いブラウザ向けのフォールバック実装あり

#### URL生成
- `encodeURIComponent()` でクエリをエンコード
- ソート順は `&f=` パラメータで指定

### デザイン

#### カラースキーム(パープルグラデーション)
- **プライマリー**: #7c3aed (バイオレット)
- **アクセント**: #a855f7 (パープル)
- **成功カラー**: #ec4899 (ピンク)
- **背景**: #faf5ff (薄紫)
- **ボーダー**: #e9d5ff, #f3e8ff
- **テキスト**: #1f2937 (ダークグレー)

#### レイアウト
- CSS Grid で2カラムレイアウト(1024px以上)
- Flexbox でボタングループ
- レスポンシブ対応(768px以下でモバイルレイアウト)
- max-width: 1400px でコンテンツ幅制限

#### タイポグラフィ
- LINE Seed JP フォント (Google Fonts)
- font-weight: 400(通常), 700(太字)
- letter-spacing: 0.03-0.05em
- line-height: 1.65

#### インタラクション
- ホバーエフェクト、トランジション
- 入力時の装飾アニメーション
- ボタンホバー時の浮き上がり効果
- トースト通知のフェードイン/アウト

## ライセンス

このプロジェクトはMITライセンスの下で公開されています。自由に使用、改変、配布できます。

## 貢献

バグ報告や機能リクエストは、Issueで受け付けています。Pull Requestも歓迎します。

## 参考リンク

- [X.com 高度な検索](https://x.com/search-advanced)
- [X.com 検索演算子ガイド](https://help.twitter.com/ja/using-twitter/twitter-advanced-search)

## 更新履歴

### v2.0.0 (2026-02-10)
- プリセット機能を実装
- デフォルトプリセット6個を追加
- デザインをパープルグラデーションに刷新
- LINE Seed JPフォントを採用
- 全画面対応(max-width削除、2カラムレイアウト)
- 入力状態の視覚化機能
- チェックボックスON時のlabel装飾
- コンテンツタイプをカテゴリ分け
- UIの省スペース化

### v1.0.0 (2026-02-10)
- 初回リリース
- 基本的な検索演算子に対応
- filter:follows を追加
- ソート順機能を追加
- URL表示機能を追加
