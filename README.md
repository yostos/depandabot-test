# Dependabot Test Project - jQuery

GitHub Dependabot とセキュリティ機能をテストするためのプロジェクトです。意図的に脆弱性を含む jQuery 3.4.1 を使用しています。

## 概要

このプロジェクトは、GitHub の Dependabot がどのように脆弱な依存関係を検出し、自動的にセキュリティアラートとプルリクエストを生成するかをテストするための教育的なプロジェクトです。

### 含まれる脆弱性

- **jQuery 3.4.1**
  - CVE-2020-11022: クロスサイトスクリプティング (XSS) 脆弱性
  - CVE-2020-11023: クロスサイトスクリプティング (XSS) 脆弱性

⚠️ **警告**: このプロジェクトは**テスト目的専用**です。本番環境では使用しないでください。

## 機能

- jQuery を使用した画像のマウスオーバー切り替えデモ
- シンプルな npm セットアップ（バンドラー不使用）
- Dependabot による脆弱性検出のテスト

## セットアップ

### 必要要件

- Node.js と npm
- モダンブラウザ（Chrome、Firefox、Safari、Edge）

### インストール

```bash
# リポジトリをクローン
git clone <repository-url>
cd jquery

# 依存関係をインストール
npm install
```

### 実行

```bash
# ブラウザで index.html を開く
open index.html  # macOS
start index.html # Windows
xdg-open index.html # Linux
```

## プロジェクト構造

```
jquery/
├── index.html              # メインHTMLファイル
├── package.json            # npm パッケージ定義
├── images/                # 画像ファイル
│   ├── 1.png              # デフォルト画像
│   └── 2.png              # ホバー時画像
├── docs/                  # ドキュメント
│   ├── technical-specifications.md
│   ├── architecture.md
│   └── repository-structure.md
└── CLAUDE.md              # Claude Code 用ガイダンス
```

## jQuery 機能デモ

index.html には以下の機能が実装されています：

- **画像のマウスオーバー切り替え**: 画像にマウスを乗せると別の画像に切り替わります
- **jQuery イベントハンドリング**: `mouseenter` と `mouseleave` イベントを使用
- **DOM 操作**: jQuery の `.attr()` と `.data()` メソッドを使用

## Dependabot テスト手順

1. このリポジトリを GitHub にプッシュ
2. リポジトリの Settings > Security & analysis で Dependabot を有効化
3. Dependabot が jQuery 3.4.1 の脆弱性を検出するのを待つ
4. 自動生成されたセキュリティアラートを確認
5. Dependabot が作成する自動プルリクエストを確認

### 期待される動作

- セキュリティアラートが表示される
- jQuery を 3.5.0 以降にアップグレードするプルリクエストが自動生成される

## ドキュメント

詳細なドキュメントは `docs/` ディレクトリにあります：

- [技術仕様書](docs/technical-specifications.md) - プロジェクトの技術詳細
- [アーキテクチャ実現性評価](docs/architecture.md) - アーキテクチャ設計の妥当性評価
- [リポジトリ構造定義書](docs/repository-structure.md) - ファイル構造と管理ポリシー

## 利用可能なツール

### GitHub CLI (gh)

```bash
# リポジトリ情報を表示
gh repo view

# Issue を作成
gh issue create

# Pull Request を作成
gh pr create
```

### Claude Code プラグイン

- `/commit`: Conventional Commits 形式でコミットを作成
- `/release`: CHANGELOG を生成してリリースを作成

## トラブルシューティング

### npm install が失敗する

Node.js と npm が正しくインストールされているか確認してください。

```bash
node --version
npm --version
```

### 画像が表示されない

`npm install` を実行して `node_modules/` ディレクトリが存在することを確認してください。

### Dependabot がアラートを生成しない

リポジトリの Settings > Security & analysis で Dependabot alerts が有効になっているか確認してください。

## ライセンス

このプロジェクトはテスト・教育目的のため、自由に使用できます。

## 参照資料

- [GitHub Dependabot ドキュメント](https://docs.github.com/ja/code-security/dependabot)
- [jQuery 公式サイト](https://jquery.com/)
- [CVE-2020-11022 詳細](https://nvd.nist.gov/vuln/detail/CVE-2020-11022)
- [CVE-2020-11023 詳細](https://nvd.nist.gov/vuln/detail/CVE-2020-11023)

## 貢献

このプロジェクトは教育目的のため、貢献は歓迎しますが、脆弱性を修正するプルリクエストは意図的に受け入れられません。

## 注意事項

- このプロジェクトは意図的に脆弱なバージョンの jQuery を使用しています
- 本番環境では絶対に使用しないでください
- セキュリティのベストプラクティスを学ぶための教材として使用してください
