# iris - AI画像高画質化システム

**Image Resolution Improvement System**

irisは、AI技術（Real-ESRGAN）を活用して画像を高画質化するWebアプリケーションです。GitHubとGitHub Actionsを利用し、ブラウザから簡単に画像のアップスケーリング処理を実行できます。

![iris](iris-logo.png)

## 主な機能

- 🖼️ **AIによる高画質化**: Real-ESRGANを使用した最大8倍までの画像アップスケーリング
- 🌐 **ブラウザベースのUI**: HTMLとJavaScriptで構築された直感的なインターフェース
- ⚙️ **GitHub Actions統合**: サーバーレスでAI処理を実行
- 📦 **自動ファイル管理**: GitHubリポジトリを使用した入出力ファイルの管理
- 🔄 **柔軟な出力形式**: JPEG（圧縮）またはPNG（無劣化）を選択可能
- 📱 **レスポンシブデザイン**: モバイルデバイスにも対応

## 仕組み

1. ユーザーが`index.html`から画像をアップロード
2. 画像はGitHubリポジトリの`my-upscaler/input/`に保存
3. GitHub Actionsのワークフロー（`upscale.yml`）を手動トリガー
4. Real-ESRGANがUbuntu環境で画像を処理
5. 処理済み画像は`my-upscaler/output/`に保存され、ブラウザからダウンロード可能

## 必要なもの

- GitHubアカウント
- GitHubパーソナルアクセストークン（PAT）
  - 必要な権限: `repo`（リポジトリへのフルアクセス）
- モダンなWebブラウザ

## セットアップ

### 1. リポジトリのフォーク

このリポジトリを自分のGitHubアカウントにフォークします。

### 2. パーソナルアクセストークンの作成

1. GitHubの設定 > Developer settings > Personal access tokens > Tokens (classic)
2. 「Generate new token」をクリック
3. `repo`スコープを選択
4. トークンを生成し、安全な場所に保存

### 3. ディレクトリ構造の準備

リポジトリ内に以下のディレクトリを作成（初回アップロード時に自動作成されます）:

```
my-upscaler/
├── input/
│   └── processed/
└── output/
```

### 4. GitHub Actionsの有効化

リポジトリの「Actions」タブでGitHub Actionsが有効になっていることを確認します。

## 使用方法

### Webインターフェース

1. ブラウザで`index.html`を開く（またはGitHub Pagesで公開）
2. GitHub設定を入力:
   - ユーザー名
   - リポジトリ名
   - パーソナルアクセストークン
3. 処理設定を選択:
   - 倍率（2倍〜8倍）
   - 出力形式（JPEGまたはPNG）
4. 画像をアップロード
5. 対象ファイルを選択
6. 「AI処理を開始」ボタンをクリック
7. GitHub Actionsで処理が完了するまで待機（数分）
8. 「生成済みファイル」リストからダウンロード

### コマンドラインからの実行

GitHub CLIを使用して直接ワークフローをトリガーすることも可能です:

```bash
gh workflow run upscale.yml -f filename=your_image_x4.jpg -f output_format=jpg
```

## 技術仕様

### 使用技術

- **フロントエンド**: HTML5, CSS3, JavaScript (Vanilla JS)
- **AI処理**: Real-ESRGAN (ncnn-vulkan版 v0.2.5.0)
- **CI/CD**: GitHub Actions
- **ストレージ**: GitHubリポジトリ

### サポートされる倍率

- 2倍、3倍、4倍（標準）、5倍、6倍、7倍、8倍（最大）

### 出力形式

- **JPEG**: 高品質圧縮（quality 95）、GitHubの100MB制限に対応
- **PNG**: 無劣化、大容量ファイルに注意

### 制限事項

- GitHubのファイルサイズ制限: 100MB/ファイル
- 処理時間: 画像サイズと倍率により数分かかる場合があります
- GPU: GitHub Actionsではソフトウェアレンダリング（llvmpipe）を使用

## ファイル構成

```
iris/
├── index.html              # メインのWebインターフェース
├── README.md              # このファイル
├── iris-logo.png          # ロゴ画像
├── iris-bg.jpg            # 背景画像
├── .github/
│   └── workflows/
│       └── upscale.yml    # GitHub Actionsワークフロー
└── my-upscaler/
    ├── input/             # 入力画像フォルダ
    │   └── processed/     # 処理済み画像の移動先
    └── output/            # 出力画像フォルダ
```

## クレジット

- **Real-ESRGAN**: [xinntao/Real-ESRGAN](https://github.com/xinntao/Real-ESRGAN)
- フォント: Google Fonts (Inter, Outfit)

## ライセンス

このプロジェクトはオープンソースです。Real-ESRGANのライセンスに従います。

## トラブルシューティング

### アップロードに失敗する

- PATの権限を確認してください
- ユーザー名とリポジトリ名が正しいか確認してください

### ワークフローが起動しない

- GitHub Actionsが有効になっているか確認してください
- リポジトリの「Actions」タブで手動実行を試してください

### 処理が完了しない

- 「Actions」タブでログを確認してください
- 画像サイズが大きすぎる場合は、倍率を下げてください

---

**Powered by Real-ESRGAN & GitHub Actions**
