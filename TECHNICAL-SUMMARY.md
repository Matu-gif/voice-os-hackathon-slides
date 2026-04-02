# 技術的な作業内容まとめ - GitHubとVercelでスライドを公開するまで

## 📋 このドキュメントについて

このドキュメントは、Voice OSハッカソンのスライドをインターネット上に公開するまでに行った技術的な作業を、初心者でも理解できるように説明したものです。

---

## 🎯 やりたかったこと

作成したHTMLスライド（`slides/idea-presentation.html`）をインターネット上に公開して、誰でもURLでアクセスできるようにしたい。

---

## 🛠️ 実施した作業の全体像

```
1. Gitリポジトリの初期化
   ↓
2. GitHubにリポジトリを作成
   ↓
3. コードをGitHubにプッシュ
   ↓
4. Vercel CLIでログイン
   ↓
5. Vercelにデプロイ
   ↓
6. 公開URLが発行される！
```

---

## 1️⃣ Gitリポジトリの初期化

### 何をしたか
```bash
cd /Users/shoutamatsu/slides/hackathon
git init
```

### これは何？
- **Git**: ファイルの変更履歴を管理するツール（バージョン管理システム）
- **`git init`**: 現在のフォルダをGitで管理できるようにするコマンド
  - これを実行すると、隠しフォルダ `.git/` が作成される
  - 以降、このフォルダ内のファイルの変更が記録できるようになる

### なぜ必要？
GitHubにコードをアップロードするには、まずローカル（あなたのPC）でGitの管理下に置く必要があるから。

---

## 2️⃣ .gitignore ファイルの作成

### 何をしたか
`.gitignore` ファイルを作成して、以下の内容を記述：
```
.DS_Store
node_modules/
.vercel
.env
```

### これは何？
- Gitで管理したくないファイルを指定するリスト
- `.DS_Store`: Macが自動生成するシステムファイル（不要）
- `node_modules/`: JavaScriptのライブラリ格納フォルダ（容量が大きいので除外）
- `.vercel`: Vercelの設定ファイル（自動生成されるので除外）
- `.env`: 秘密情報を含む環境変数ファイル（公開してはいけない）

### なぜ必要？
不要なファイルや秘密情報をGitHubに公開しないため。

---

## 3️⃣ vercel.json ファイルの作成

### 何をしたか
`vercel.json` ファイルを作成：
```json
{
  "rewrites": [
    {
      "source": "/",
      "destination": "/slides/idea-presentation.html"
    }
  ]
}
```

### これは何？
- Vercelの設定ファイル
- **rewrites（リライト）**: URLの書き換えルール
  - `"source": "/"`: トップページ（ドメインのルート）にアクセスしたとき
  - `"destination": "/slides/idea-presentation.html"`: このファイルを表示する

### なぜ必要？
`https://your-site.vercel.app/` にアクセスしただけで、スライドが表示されるようにするため。
（設定しないと `https://your-site.vercel.app/slides/idea-presentation.html` と長いURLを入力する必要がある）

---

## 4️⃣ 初回コミット

### 何をしたか
```bash
git add -A
git commit -m "Initial commit: Voice OS idea presentation slides"
```

### これは何？
- **`git add -A`**: すべてのファイルを「ステージング」（次のコミット対象にする）
- **`git commit -m "メッセージ"`**: 現在の状態を「スナップショット」として記録
  - `-m` は「メッセージ」の略
  - この時点でのファイルの状態が履歴として保存される

### なぜ必要？
Gitは「コミット」という単位で履歴を管理する。コミットしないとGitHubにアップロードできない。

---

## 5️⃣ GitHub CLI でログイン

### 何をしたか
```bash
gh auth login
```

その後、インタラクティブな質問に答えた：
1. **GitHub.com を選択**
2. **HTTPS を選択**（通信プロトコル）
3. **Yes** - GitをGitHubの認証情報で使う
4. **Login with a web browser** - ブラウザで認証
5. ブラウザでコード `F280-E3D6` を入力して認証完了

### これは何？
- **GitHub CLI (`gh`)**: GitHubをコマンドラインで操作するツール
- **認証**: あなたのGitHubアカウントにアクセスする権限を得る

### なぜ必要？
GitHubのリポジトリを作成したり、コードをアップロードするには、本人確認が必要だから。

---

## 6️⃣ GitHubリポジトリの作成とプッシュ

### 何をしたか
```bash
gh repo create voice-os-hackathon-slides --public --source=. --remote=origin --push
```

### これは何？
このコマンドは一度に複数のことをやってくれる：

1. **`gh repo create`**: GitHubに新しいリポジトリを作成
2. **`voice-os-hackathon-slides`**: リポジトリの名前
3. **`--public`**: 公開リポジトリ（誰でも見られる）
4. **`--source=.`**: 現在のフォルダをソースコードとして使う
5. **`--remote=origin`**: このリポジトリを `origin` という名前で登録
6. **`--push`**: ローカルのコミットをGitHubにアップロード

### 結果
- GitHubに https://github.com/Matu-gif/voice-os-hackathon-slides が作成された
- ローカルのファイルがGitHubにアップロードされた

### なぜ必要？
コードをGitHubに保存することで：
- バックアップができる
- 他の人と共有できる
- Vercelなどのサービスと連携できる

---

## 7️⃣ Vercel CLI のインストール

### 何をしたか
```bash
npm install -g vercel
```

### これは何？
- **npm**: Node.jsのパッケージマネージャー（ツールをインストールするためのツール）
- **`install -g vercel`**: Vercel CLIをグローバルにインストール
  - `-g` は「global（全体）」の略で、どこからでも使えるようにする

### なぜ必要？
Vercelにデプロイするために、Vercelの専用ツールが必要だから。

---

## 8️⃣ Vercel へのログイン

### 何をしたか
```bash
vercel login
```

その後：
1. ブラウザで https://vercel.com/device を開く
2. コード `SRJR-BNJT` を入力
3. 認証完了

### これは何？
GitHubと同様に、Vercelアカウントにアクセスする権限を得るための認証プロセス。

### なぜ必要？
Vercelにデプロイする権限を得るため。

---

## 9️⃣ Vercel への本番デプロイ

### 何をしたか
```bash
vercel --prod
```

その後、インタラクティブな質問に答えた：
1. **Set up and deploy?** → Yes
2. **Which scope?** → cramreserveform（あなたのアカウント）
3. **Link to existing project?** → No（新規プロジェクト）
4. **Project name?** → voice-os-hackathon-slides
5. **Code directory?** → `./`（現在のフォルダ）
6. **Modify settings?** → No（デフォルト設定でOK）
7. **Connect repository?** → Yes（GitHubリポジトリと連携）

### これは何？
**デプロイ**: ローカルのファイルをインターネット上のサーバーにアップロードして、公開すること

#### Vercelが自動でやってくれること
1. **ファイルのアップロード**: すべてのHTMLやCSSをVercelのサーバーに送信
2. **ビルド**: 必要に応じてファイルを最適化
3. **CDNへの配置**: 世界中のサーバーにファイルをコピー（高速アクセスのため）
4. **URLの発行**: `https://voice-os-hackathon-slides.vercel.app` という公開URLを作成
5. **HTTPS化**: セキュアな通信（鍵マーク付き）を自動設定

### 結果
- **Production URL**: https://voice-os-hackathon-slides.vercel.app
- このURLにアクセスすると、スライドが表示される！

---

## 🔄 今後の更新フロー（自動デプロイ）

GitHubとVercelを連携したので、今後は以下の流れで更新できます：

```bash
# 1. ファイルを編集
# （例: slides/idea-presentation.html を編集）

# 2. Gitでコミット
git add .
git commit -m "スライド更新"

# 3. GitHubにプッシュ
git push

# 4. Vercelが自動で再デプロイ（何もしなくてOK！）
# 数秒後、URLが最新版に更新される
```

---

## 📚 用語集

### Git関連
- **リポジトリ**: プロジェクトのファイルと履歴を保存する場所
- **コミット**: ファイルの変更を記録するスナップショット
- **プッシュ**: ローカルの変更をリモート（GitHub）に送信すること
- **リモート**: インターネット上のリポジトリ（GitHubなど）

### デプロイ関連
- **デプロイ**: ファイルをサーバーにアップロードして公開すること
- **本番環境（Production）**: 実際にユーザーがアクセスする環境
- **CDN**: 世界中に配置されたサーバーネットワーク（高速アクセスのため）
- **HTTPS**: セキュアな通信プロトコル（データが暗号化される）

### Vercel関連
- **Vercel**: Webサイトを簡単にデプロイできるホスティングサービス
- **Rewrites**: URLを別のファイルに転送する機能
- **Scope**: Vercelのアカウントやチームの単位

---

## ✨ まとめ：何が達成されたか

### Before（作業前）
- HTMLファイルは自分のPC内にしかない
- 他の人に見せるには、ファイルを送る必要がある

### After（作業後）
- ✅ GitHubにコードが保存され、バックアップされた
- ✅ Vercelでインターネット上に公開された
- ✅ URLを共有するだけで、誰でもアクセスできる
- ✅ 今後はgit pushするだけで自動更新される

### 技術的な流れ
```
ローカルPC
   ↓ git commit（変更を記録）
   ↓ git push（GitHubに送信）
GitHub（コード保管）
   ↓ Webhook（自動通知）
Vercel（自動デプロイ）
   ↓ ビルド & CDN配置
インターネット上で公開！
```

---

## 🎓 学んだこと

1. **Git**: ファイルの変更履歴を管理する基本的なツール
2. **GitHub**: GitリポジトリをWeb上で管理・共有するサービス
3. **GitHub CLI (`gh`)**: コマンドラインでGitHubを操作できる
4. **Vercel**: 静的サイトを簡単にデプロイできるサービス
5. **CI/CD**: GitHubとVercelの連携で、自動デプロイの仕組みを実現

---

## 📖 参考リンク

- **GitHubリポジトリ**: https://github.com/Matu-gif/voice-os-hackathon-slides
- **公開サイト**: https://voice-os-hackathon-slides.vercel.app
- **Vercelダッシュボード**: https://vercel.com/dashboard

---

**作成日**: 2026年4月2日  
**プロジェクト**: Voice OS Hackathon Slides
