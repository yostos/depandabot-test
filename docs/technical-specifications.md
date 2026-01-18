# 技術仕様書

## プロジェクト概要

**プロジェクト名**: dependabot-test-jquery
**バージョン**: 1.0.0
**目的**: GitHub Dependabot とセキュリティ機能のテストプロジェクト

## 1. プロジェクトの目的と背景

このプロジェクトは、GitHub の Dependabot とセキュリティスキャン機能をテストするために作成されました。意図的に脆弱性を含む jQuery 3.4.1 を使用することで、以下の検証を行います：

- Dependabot による脆弱な依存関係の検出
- セキュリティアラートの自動生成
- 自動化されたプルリクエストの作成

## 2. 技術スタック

### 2.1 フロントエンド

| 技術 | バージョン | 用途 |
|------|-----------|------|
| HTML5 | - | マークアップ |
| jQuery | 3.4.1 | JavaScript ライブラリ（テスト用） |

### 2.2 パッケージ管理

- **npm**: 依存関係の管理
- **package.json**: 依存関係の定義

### 2.3 既知の脆弱性

jQuery 3.4.1 には以下の既知の脆弱性が存在します：

- **CVE-2020-11022**: クロスサイトスクリプティング (XSS) 脆弱性
- **CVE-2020-11023**: クロスサイトスクリプティング (XSS) 脆弱性

これらの脆弱性は意図的に含まれており、セキュリティスキャンのテスト目的です。

## 3. システム要件

### 3.1 開発環境

- **Node.js**: npm を実行するために必要
- **モダンブラウザ**: HTML ファイルを表示するために必要
  - Google Chrome (推奨)
  - Firefox
  - Safari
  - Edge

### 3.2 必要なツール

```bash
# Node.js と npm がインストールされていることを確認
node --version
npm --version
```

## 4. インストールと実行

### 4.1 依存関係のインストール

```bash
npm install
```

このコマンドにより、`package.json` に定義された jQuery 3.4.1 が `node_modules/` ディレクトリにインストールされます。

### 4.2 アプリケーションの実行

```bash
# ブラウザで index.html を開く
open index.html  # macOS
start index.html # Windows
xdg-open index.html # Linux
```

## 5. セキュリティ考慮事項

### 5.1 脆弱性の詳細

**CVE-2020-11022 および CVE-2020-11023**:
- jQuery の HTML パース機能における XSS 脆弱性
- 悪意のある HTML コンテンツの処理時に任意のスクリプトが実行される可能性

### 5.2 本番環境での使用禁止

⚠️ **警告**: このプロジェクトは**テスト目的専用**です。本番環境では使用しないでください。

### 5.3 推奨される対応

Dependabot が脆弱性を検出した場合：
1. 自動生成されたプルリクエストを確認
2. jQuery を安全なバージョン（3.5.0 以降）にアップグレード
3. アプリケーションの動作をテスト
4. プルリクエストをマージ

## 6. プロジェクト構成

```
jquery/
├── index.html              # メインHTMLファイル
├── package.json            # npm パッケージ定義
├── CLAUDE.md              # Claude Code 用ガイダンス
├── .gitignore             # Git 除外設定
├── images/                # 画像ファイル
│   ├── 1.svg              # デフォルト画像（青）
│   └── 2.svg              # ホバー時画像（赤）
├── node_modules/          # npm 依存関係（自動生成）
│   └── jquery/
│       └── dist/
│           └── jquery.js
└── docs/                  # ドキュメント
    ├── technical-specifications.md
    ├── architecture.md
    └── repository-structure.md
```

### 6.1 jQuery 機能デモ

index.html には jQuery を使用した画像切り替え機能が実装されています：

**機能**:
- マウスオーバーで画像が切り替わる
- `mouseenter` イベントで 2.svg に切り替え
- `mouseleave` イベントで 1.svg に戻る

**実装方法**:
```javascript
$('.hover-image').on('mouseenter', function() {
  const hoverSrc = $(this).data('hover');
  $(this).attr('src', hoverSrc);
});

$('.hover-image').on('mouseleave', function() {
  const originalSrc = $(this).data('original');
  $(this).attr('src', originalSrc);
});
```

**画像ファイル**:
- `images/1.svg`: 青色の背景に「画像 1」と表示
- `images/2.svg`: 赤色の背景に「画像 2」と表示

**カスタマイズ**:
独自の PNG 画像を使用する場合は、`images/` ディレクトリに `1.png` と `2.png` を配置し、index.html の画像パスを更新してください：
```html
<img
  src="images/1.png"
  data-original="images/1.png"
  data-hover="images/2.png"
/>
```

## 7. テストシナリオ

### 7.1 Dependabot テスト

1. リポジトリを GitHub にプッシュ
2. Dependabot がスキャンを実行するのを待つ
3. セキュリティアラートが生成されることを確認
4. 自動プルリクエストが作成されることを確認

### 7.2 期待される動作

- **セキュリティアラート**: jQuery 3.4.1 の脆弱性に関するアラートが表示される
- **自動PR**: jQuery を安全なバージョンにアップグレードするプルリクエストが自動生成される

## 8. トラブルシューティング

### 8.1 よくある問題

**問題**: `npm install` が失敗する
- **解決策**: Node.js と npm が正しくインストールされているか確認

**問題**: index.html で jQuery が読み込まれない
- **解決策**: `npm install` を実行して `node_modules/` ディレクトリが存在することを確認

**問題**: Dependabot がアラートを生成しない
- **解決策**: リポジトリの Settings > Security & analysis で Dependabot が有効になっているか確認

## 9. 今後の拡張性

このプロジェクトは以下の目的で拡張可能です：

- 他の脆弱な依存関係のテスト
- GitHub Advanced Security 機能のテスト
- CodeQL によるコードスキャンのテスト
- Secret scanning のテスト

## 10. 参照資料

- [jQuery 公式サイト](https://jquery.com/)
- [GitHub Dependabot ドキュメント](https://docs.github.com/ja/code-security/dependabot)
- [CVE-2020-11022 詳細](https://nvd.nist.gov/vuln/detail/CVE-2020-11022)
- [CVE-2020-11023 詳細](https://nvd.nist.gov/vuln/detail/CVE-2020-11023)

## 11. 変更履歴

| バージョン | 日付 | 変更内容 |
|-----------|------|---------|
| 1.0.0 | 2026-01-18 | 初版作成 |
