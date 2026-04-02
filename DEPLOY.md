# Voice OS Hackathon - Vercelデプロイ手順

## 🚀 デプロイ方法（2つの選択肢）

### 方法1: Vercel CLI を使う（最速）

#### 1. Vercel CLI をインストール
```bash
npm install -g vercel
```

#### 2. プロジェクトディレクトリに移動
```bash
cd /Users/shoutamatsu/slides/hackathon
```

#### 3. デプロイコマンドを実行
```bash
vercel
```

初回実行時：
- Vercelアカウントへのログインを求められます
- プロジェクト名を聞かれます（デフォルトでOK）
- 設定を聞かれますが、全てEnterでOK

#### 4. 本番デプロイ
```bash
vercel --prod
```

これでデプロイ完了！URLが表示されます。

---

### 方法2: GitHubと連携する（推奨）

#### 1. GitHubにリポジトリを作成
1. https://github.com/new にアクセス
2. リポジトリ名を入力（例: `voice-os-hackathon-slides`）
3. "Create repository" をクリック

#### 2. ローカルからGitHubにプッシュ
```bash
cd /Users/shoutamatsu/slides/hackathon
git remote add origin https://github.com/YOUR_USERNAME/REPO_NAME.git
git branch -M main
git push -u origin main
```

#### 3. Vercelと連携
1. https://vercel.com/new にアクセス
2. "Import Git Repository" を選択
3. 作成したリポジトリを選択
4. "Deploy" をクリック

**メリット**: 以降、`git push`するだけで自動デプロイされます！

---

## 📝 現在の準備状況

✅ Gitリポジトリ初期化済み
✅ .gitignore 作成済み
✅ vercel.json 作成済み（ルートURLでスライド表示）
✅ 初回コミット完了

---

## 🎯 デプロイ後のURL

- スライド: `https://YOUR_PROJECT.vercel.app/`
- 既存スライド: `https://YOUR_PROJECT.vercel.app/slides/index.html`

---

## 💡 おすすめの流れ

1. まず**方法1（Vercel CLI）**で試して、すぐに確認
2. 後で**方法2（GitHub連携）**に切り替えて、自動デプロイ環境を構築

---

**準備は完了しています。上記のコマンドを実行してください！**
