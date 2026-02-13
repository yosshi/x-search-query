# X.com 検索クエリビルダー - 開発ガイド

## プロジェクト概要

X.com(旧Twitter)の高度な検索クエリを視覚的に組み立てるWebアプリケーション。
単一のHTMLファイルで動作し、サーバー不要で使用可能。

## 技術スタック

- Pure JavaScript (ES6+)
- HTML5
- CSS3
- LINE Seed JP (Google Fonts)
- localStorage (プリセット保存)
- 外部依存: フォントのみCDN使用

## アーキテクチャ原則

### 単一ファイル構成の維持

このプロジェクトは意図的に単一のHTMLファイル(index.html)で構成されている。
以下の理由で、ファイルを分割してはいけない:

- サーバー不要で、ローカルでブラウザから直接開ける
- デプロイが簡単(1ファイルをホスティングするだけ)
- 配布が簡単(メールで送ったり、USBメモリにコピーできる)

### 外部依存の最小化

React、Vue、jQueryなどのライブラリは使用しない。
Pure JavaScriptのみを使用すること。

**例外:**
- フォント: LINE Seed JP (Google Fonts CDN)
  - 日本語の可読性向上のため許可
  - フォールバックフォントを設定

## コーディング規約

### JavaScript

- ES6+の機能を積極的に使用する(const/let, arrow function, template literals等)
- グローバル変数を最小限にする
- 関数は小さく、単一責任の原則に従う

### CSS

- モダンなCSS機能を使用する(Flexbox, Grid等)
- レスポンシブデザインを維持する
  - 768px以下: モバイルレイアウト
  - 1024px以上: 2カラムグリッドレイアウト
- ホバーエフェクトやトランジションで滑らかなUXを提供する
- カラースキーム: パープルグラデーション
  - プライマリー: #7c3aed
  - アクセント: #a855f7
  - 成功: #ec4899
- フォント: LINE Seed JP
  - font-weight: 400 (通常), 700 (太字のみ)
  - letter-spacing: 0.03-0.05em

### HTML

- セマンティックなHTML要素を使用する
- アクセシビリティを考慮する(label要素、aria属性等)

## 機能追加時の注意点

### 新しい検索演算子を追加する場合

1. HTMLに入力フォームを追加
2. updateQuery()関数内でクエリ生成ロジックを追加
3. clearAll()関数内でクリア処理を追加(不要な場合あり)
4. updateInputState()が自動的に状態を管理
5. generateQueryFromValues()にもロジックを追加(プリセット用)
6. README.mdの対応演算子表を更新

### プリセット機能について

**データ構造:**
```javascript
{
  id: 'unique-id',
  name: 'プリセット名',
  description: '説明',
  isDefault: false,  // デフォルトプリセットはtrue
  formValues: {
    'input-id': 'value',
    'checkbox-id': true,
    ...
  }
}
```

**保存場所:**
- デフォルトプリセット: `defaultPresets` 配列(コード内)
- ユーザープリセット: localStorage (`x-search-presets`)

**関数:**
- `loadPresets()`: 全プリセットを読み込み
- `savePresetsToStorage()`: ユーザープリセットを保存
- `getFormValues()`: 現在のフォーム値を取得
- `setFormValues()`: フォームに値を設定
- `generateQueryFromValues()`: 値からクエリを生成

### ストレージ機能を追加する場合

- localStorage を使用する
- エラーハンドリングを忘れずに(プライベートブラウジングモードでは無効)
- 保存するデータはJSON形式にする

### 外部API連携を追加する場合

- CORS制限に注意する
- エラーハンドリングを適切に行う
- ローディング状態を表示する

## X.com検索仕様の注意点

### クエリのエンコーディング

- URLに含める際は`encodeURIComponent()`を使用する
- スペースは`%20`にエンコードされる

### ソート順パラメータ

- `&f=live`: 最新順
- `&f=user`: ユーザー検索
- `&f=image`: 画像タブ
- `&f=video`: 動画タブ
- パラメータなし: トップ(人気度順)

### 演算子の組み合わせ

- filter:とhas:は同じ機能のものがある(filter:linksとhas:links等)
- 矛盾する条件(filter:retweetsと-filter:retweets)は避ける
- 日付の範囲はsince <= until である必要がある

## テスト方法

### 手動テスト項目

**基本機能:**
1. 各入力フィールドが正しくクエリに反映されるか
2. クエリコピー機能が動作するか
3. URLコピー機能が動作するか
4. X.comで検索ボタンが正しいURLで遷移するか
5. クリアボタンが全ての入力をリセットするか
6. クリア時に入力状態の装飾が解除されるか

**プリセット機能:**
7. デフォルトプリセットが正しく読み込まれるか
8. プリセット保存が動作するか
9. 保存したプリセットがlocalStorageに保存されているか
10. プリセット削除が動作するか
11. プリセット読み込み後、フォームに値が反映されるか

**UI/装飾:**
12. 入力時にフィールドが強調表示されるか
13. チェックボックスON時にlabelが装飾されるか
14. モーダルが正しく開閉するか
15. モーダル外クリックで閉じるか

**レスポンシブ:**
16. モバイル表示が崩れていないか(768px以下)
17. タブレット表示が崩れていないか(769-1023px)
18. デスクトップで2カラム表示されるか(1024px以上)

### 対応ブラウザでの動作確認

- Chrome(最新版)
- Firefox(最新版)
- Safari(最新版)
- Edge(最新版)

## デバッグのヒント

### よくある問題

1. **クエリが生成されない**
   - ブラウザのコンソールでエラーを確認
   - updateQuery()関数の各条件分岐をログ出力で追跡

2. **コピー機能が動作しない**
   - HTTPS環境でないとnavigator.clipboard APIが動作しない
   - フォールバック実装を使用しているか確認

3. **X.comで検索結果がおかしい**
   - 生成されたURLをデコードして内容を確認
   - X.comの演算子仕様が変更されている可能性

4. **プリセットが保存されない**
   - localStorageが有効か確認(プライベートブラウジングでは無効)
   - ブラウザのストレージ容量を確認
   - コンソールでlocalStorage.getItem('x-search-presets')を確認

5. **入力状態の装飾が解除されない**
   - updateInputState()が呼ばれているか確認
   - clearAll()の最後でupdateInputState()を実行

6. **チェックボックスのlabel装飾が効かない**
   - CSSセレクタの順序を確認: `input:checked + label`
   - HTMLでinputとlabelが隣接しているか確認

## 今後の開発方針

README.mdの「今後の拡張案」セクションを参照。
優先度の高いものから実装していく。

**実装済み:**
- ✅ プリセット保存機能(localStorage使用)
- ✅ デフォルトプリセット6個
- ✅ パープルグラデーションデザイン
- ✅ LINE Seed JPフォント
- ✅ 全画面対応・2カラムレイアウト
- ✅ 入力状態の視覚化
- ✅ コンテンツタイプのカテゴリ分け

**次に実装する価値が高い機能:**
1. 検索履歴機能(localStorage使用)
2. 位置情報フィルター(near:, within:, geocode:)
3. プリセットのインポート/エクスポート(JSON)
4. URLパラメータからの自動読み込み

## デプロイ方法

### GitHub Pages

1. リポジトリをGitHubにプッシュ
2. Settings > Pages > Source を `main` ブランチに設定
3. index.htmlが自動的に公開される

### その他の静的ホスティング

- Netlify: リポジトリを接続するだけで自動デプロイ
- Vercel: 同上
- Cloudflare Pages: 同上

いずれもビルド不要(静的ファイルなので)。

## 参考資料

- [X.com 高度な検索](https://x.com/search-advanced)
- [X.com 検索演算子ガイド](https://help.twitter.com/ja/using-twitter/twitter-advanced-search)
- [MDN Web Docs](https://developer.mozilla.org/)
