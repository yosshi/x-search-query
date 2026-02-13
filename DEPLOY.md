# デプロイ手順

## 初回セットアップ

### 1. 依存関係のインストール

```bash
npm install
```

### 2. Netlify CLIにログイン

```bash
npx netlify login
```

ブラウザが開くので、Netlifyアカウントで認証してください。

### 3. 既存のNetlifyサイトとリンク

既にNetlify Dropでサイトを作成している場合は、以下のコマンドでリンクします:

```bash
npx netlify link
```

プロンプトが表示されたら:
1. "Use current git remote origin" を選択
2. または "Choose from a list of your Netlify sites" を選択して、既存のサイトを選ぶ

**サイトIDがわかる場合:**

```bash
npx netlify link --id YOUR_SITE_ID
```

サイトIDはNetlifyのダッシュボードで確認できます(Site settings > General > Site information > Site ID)。

### 4. 新しいNetlifyサイトを作成する場合

```bash
npx netlify init
```

プロンプトに従って、新しいサイトを作成します。

## 日常的なデプロイ

### 本番環境へのデプロイ

```bash
npm run deploy
```

このコマンドは以下を実行します:
1. distディレクトリを作成してindex.htmlをコピー(ビルド)
2. distディレクトリの内容をNetlifyにアップロード
3. 本番環境に直接デプロイ

**デプロイされるファイル:** index.htmlのみ

### ドラフト環境へのデプロイ(プレビュー)

```bash
npm run deploy:draft
```

本番環境に影響を与えずに、プレビューURLでテストできます。

## トラブルシューティング

### サイトがリンクされていない場合

```bash
npx netlify status
```

このコマンドで現在の接続状態を確認できます。
"Not linked to a Netlify site" と表示された場合は、`netlify link` を実行してください。

### 認証エラーが発生した場合

```bash
npx netlify logout
npx netlify login
```

再度ログインしてください。

### デプロイログの確認

デプロイ後、以下のコマンドでログを確認できます:

```bash
npx netlify watch
```

または、Netlifyのダッシュボードでデプロイログを確認してください。

## ビルドとクリーンアップ

### ビルドのみ実行

```bash
npm run build
```

distディレクトリにindex.htmlをコピーします。

### ビルド成果物をクリーンアップ

```bash
npm run clean
```

distディレクトリを削除します。

## ワークフロー例

通常の開発フローは以下の通りです:

```bash
# 1. コードを編集
vim index.html

# 2. ドラフト環境でテスト(オプション)
npm run deploy:draft
# プレビューURLで動作確認

# 3. Gitコミット
git add index.html
git commit -m "機能を追加した"

# 4. 本番環境にデプロイ
npm run deploy

# 5. Gitにpush(オプション)
git push origin main
```

**注意:** distディレクトリは自動生成されるため、Gitにコミットする必要はありません。

## 自動デプロイへの移行(将来的な選択肢)

手動デプロイが面倒になったら、GitHubリポジトリとNetlifyを連携させて自動デプロイに切り替えることもできます:

1. Netlifyのダッシュボードを開く
2. Site settings > Build & deploy > Continuous Deployment
3. "Link repository" からGitHubリポジトリを接続
4. mainブランチへのpushで自動デプロイされるようになります

この場合、`npm run deploy` は不要になります。
