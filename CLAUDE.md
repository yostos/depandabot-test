# CLAUDE.md

このファイルは、Claude Code (claude.ai/code) がこのリポジトリのコードを扱う際のガイダンスを提供します。

## リポジトリ概要

これは GitHub Dependabot とセキュリティ機能のテストプロジェクトです。意図的に jQuery 3.4.1 を使用しており、既知の脆弱性（CVE-2020-11022、CVE-2020-11023）を含んでいます。これは GitHub のセキュリティスキャンとアラート機能をテストするためです。

## プロジェクトの目的

- GitHub Dependabot が脆弱な依存関係を検出する能力をテストする
- GitHub セキュリティアラートと自動プルリクエストをテストする
- jQuery は CDN ではなく npm 経由でインストールされ、`package.json` に記載されるため Dependabot が検出できる

## アーキテクチャ

**シンプルな npm セットアップ**: jQuery は `package.json` で依存関係として宣言され、バンドラーを使わずに `node_modules/` から直接読み込まれます。HTML ファイル（`index.html`）は相対パスを使用して `node_modules/jquery/dist/jquery.js` を参照します。

**jQuery 機能デモ**: index.html にはマウスオーバーで画像が切り替わる機能が実装されており、jQuery の基本的なイベントハンドリングと DOM 操作を示しています。

## 開発コマンド

```bash
# 依存関係をインストール（脆弱な jQuery 3.4.1 を含む）
npm install

# ブラウザでページを開く
# npm install 実行後、単純にブラウザで index.html を開く
```

## 利用可能なツール

### GitHub CLI (gh)

このプロジェクトでは GitHub CLI が使用可能です。以下のような操作が可能です：

```bash
# リポジトリ情報を表示
gh repo view

# Issue を作成
gh issue create

# Pull Request を作成
gh pr create

# セキュリティアラートを確認
gh api repos/{owner}/{repo}/dependabot/alerts
```

### Claude Code プラグイン

**simple-commit**: Conventional Commits 形式でのコミットとリリース管理

- `/commit`: 変更を自動的に解析して Conventional Commits 形式でコミット
- `/release`: CHANGELOG を自動生成してリリースを作成

使用例：
```bash
# ステージングした変更を自動的にコミット
/commit

# 新しいリリースを作成（タグ付けとCHANGELOG生成）
/release
```

## 重要な注意事項

- このプロジェクトはテスト目的で意図的に脆弱なバージョンの jQuery を使用しています
- 本番環境でこのセットアップを使用しないでください
- Dependabot が脆弱性を検出すると、jQuery を安全なバージョンに更新するプルリクエストを作成するはずです
