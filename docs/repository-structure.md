# リポジトリ構造定義書

## 1. 概要

本ドキュメントは、dependabot-test-jquery プロジェクトのリポジトリ構造を定義し、各ファイルとディレクトリの役割を明確にします。

## 2. リポジトリ全体構造

```
jquery/
├── .gitignore                              # Git 除外設定
├── CLAUDE.md                               # Claude Code 用ガイダンス
├── index.html                              # メイン HTML ファイル
├── package.json                            # npm パッケージ定義
├── node_modules/                           # npm 依存関係（自動生成）
│   └── jquery/                             # jQuery パッケージ
│       ├── dist/                           # 配布ファイル
│       │   ├── jquery.js                  # 非圧縮版
│       │   └── jquery.min.js              # 圧縮版
│       ├── src/                            # ソースコード
│       └── package.json                    # jQuery のパッケージ定義
└── docs/                                   # ドキュメント
    ├── technical-specifications.md         # 技術仕様書
    ├── architecture.md                     # アーキテクチャ実現性評価
    └── repository-structure.md             # リポジトリ構造定義書（本文書）
```

## 3. ルートディレクトリファイル

### 3.1 .gitignore

**役割**: Git によるバージョン管理から除外するファイルとディレクトリを指定

**内容**:
```gitignore
node_modules/
```

**重要性**: 🔴 必須
- `node_modules/` をリポジトリから除外
- リポジトリサイズの削減
- 依存関係は `package.json` から再現可能

### 3.2 CLAUDE.md

**役割**: Claude Code（AI アシスタント）がコードベースを理解するためのガイダンス

**対象読者**: Claude Code AI
**言語**: 日本語

**内容**:
- リポジトリの概要
- プロジェクトの目的
- アーキテクチャの説明
- 開発コマンド
- 重要な注意事項

**重要性**: 🟡 推奨
- AI による開発支援の精度向上
- プロジェクト理解の効率化

**更新頻度**: プロジェクトの目的や構造が変わったとき

### 3.3 index.html

**役割**: メインの HTML ファイル

**種類**: プレゼンテーション層
**言語**: HTML5、日本語コンテンツ

**構造**:
```html
<!doctype html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>サンプルページ</title>
    <script src="node_modules/jquery/dist/jquery.js"></script>
  </head>
  <body>
    <!-- コンテンツ -->
  </body>
</html>
```

**特徴**:
- jQuery を `node_modules/` から直接読み込み
- バンドラー不使用
- レスポンシブ対応のメタタグ

**重要性**: 🔴 必須
- アプリケーションのエントリーポイント
- Dependabot テストの動作確認用

**依存関係**:
- `node_modules/jquery/dist/jquery.js`（npm install 後に生成）

### 3.4 package.json

**役割**: npm パッケージの定義と依存関係の管理

**種類**: パッケージ定義
**形式**: JSON

**構造**:
```json
{
  "name": "dependabot-test-jquery",
  "version": "1.0.0",
  "description": "Test project for GitHub Dependabot and security features",
  "private": true,
  "dependencies": {
    "jquery": "3.4.1"
  }
}
```

**重要フィールド**:

| フィールド | 値 | 目的 |
|-----------|-----|------|
| `name` | dependabot-test-jquery | プロジェクト識別子 |
| `version` | 1.0.0 | プロジェクトバージョン |
| `description` | Test project... | プロジェクト説明 |
| `private` | true | npm への公開を防止 |
| `dependencies.jquery` | 3.4.1 | 脆弱なバージョンを明示 |

**重要性**: 🔴 必須
- Dependabot のスキャン対象
- 依存関係の定義
- プロジェクトメタデータ

**セキュリティ注意事項**:
- ⚠️ `"private": true` により npm publish が防止される
- ⚠️ jQuery 3.4.1 は意図的に脆弱なバージョン

## 4. node_modules ディレクトリ

### 4.1 概要

**生成方法**: `npm install` コマンドで自動生成
**バージョン管理**: ❌ Git で管理しない（.gitignore で除外）

### 4.2 構造

```
node_modules/
└── jquery/
    ├── dist/
    │   ├── jquery.js           # 開発用（非圧縮、326KB）
    │   ├── jquery.min.js       # 本番用（圧縮、87KB）
    │   ├── jquery.slim.js      # Slim版（非圧縮）
    │   └── jquery.slim.min.js  # Slim版（圧縮）
    ├── src/                    # jQuery ソースコード
    ├── external/               # 外部依存関係（Sizzle など）
    ├── LICENSE.txt             # ライセンス情報
    ├── package.json            # jQuery のパッケージ定義
    └── README.md               # jQuery の README
```

### 4.3 使用ファイル

**プロジェクトで使用**: `node_modules/jquery/dist/jquery.js`

**選択理由**:
- 非圧縮版でデバッグが容易
- 本番環境では使用しないため圧縮不要
- Dependabot テストに圧縮の有無は無関係

**代替案**:
- `jquery.min.js`: 本番環境用（このプロジェクトには不要）
- `jquery.slim.js`: Ajax/effects を除外した軽量版（機能が限定される）

## 5. docs ディレクトリ

### 5.1 概要

**役割**: プロジェクトのドキュメントを格納
**言語**: 日本語
**形式**: Markdown

### 5.2 ドキュメント一覧

#### 5.2.1 technical-specifications.md

**役割**: プロジェクトの技術仕様を定義

**含まれる内容**:
- プロジェクト概要
- 技術スタック
- システム要件
- インストールと実行方法
- セキュリティ考慮事項
- プロジェクト構成
- テストシナリオ
- トラブルシューティング

**対象読者**:
- 開発者
- QA エンジニア
- DevOps エンジニア

**更新頻度**: 技術スタックや要件が変更されたとき

#### 5.2.2 architecture.md

**役割**: アーキテクチャ設計の実現性と妥当性を評価

**含まれる内容**:
- アーキテクチャ概要
- 設計思想
- 実現性評価
- 設計上の強みと考慮事項
- セキュリティアーキテクチャ
- 拡張性とスケーラビリティ
- 代替アーキテクチャの検討
- 推奨事項

**対象読者**:
- ソフトウェアアーキテクト
- テックリード
- セキュリティエンジニア

**更新頻度**: アーキテクチャに重要な変更があったとき

#### 5.2.3 repository-structure.md

**役割**: リポジトリの構造とファイルの役割を定義（本文書）

**含まれる内容**:
- リポジトリ全体構造
- 各ファイルの役割と重要性
- ディレクトリ構成
- ファイル管理ポリシー
- 命名規則

**対象読者**:
- 新規参加者
- プロジェクトマネージャー
- すべての開発者

**更新頻度**: ファイル構造が変更されたとき

## 6. ファイル管理ポリシー

### 6.1 バージョン管理対象

#### 管理対象（Git にコミット）

| ファイル/ディレクトリ | 理由 |
|---------------------|------|
| `.gitignore` | バージョン管理設定として必須 |
| `CLAUDE.md` | プロジェクト理解に必要 |
| `index.html` | アプリケーションコード |
| `package.json` | 依存関係の定義として必須 |
| `docs/` | ドキュメントは共有資産 |

#### 管理対象外（Git から除外）

| ファイル/ディレクトリ | 理由 |
|---------------------|------|
| `node_modules/` | `package.json` から再現可能 |
| `.DS_Store` | OS 固有ファイル |
| `*.log` | ログファイルは一時的 |

### 6.2 ファイルサイズガイドライン

| ファイルタイプ | 推奨最大サイズ | 現状 |
|--------------|--------------|------|
| HTML | 10 KB | ✅ 〜1 KB |
| JSON | 10 KB | ✅ 〜0.2 KB |
| Markdown | 50 KB | ✅ 各 10-20 KB |

### 6.3 命名規則

#### ファイル名

- **小文字とハイフン**: `technical-specifications.md`
- **拡張子**: 適切な拡張子を使用（`.md`, `.html`, `.json`）
- **説明的**: ファイルの内容が明確にわかる名前

#### ディレクトリ名

- **小文字**: `docs`
- **複数形**: ドキュメント集合は `docs`
- **簡潔**: 一語で表現可能なら一語を使用

## 7. ディレクトリ構成の設計原則

### 7.1 フラット構造の採用

**原則**: 可能な限りフラットな構造を維持

**理由**:
- ✅ ファイル検索が容易
- ✅ パス指定がシンプル
- ✅ 小規模プロジェクトに適している

**適用例**:
```
✅ 推奨（現状）:
jquery/
├── index.html
└── docs/
    └── *.md

❌ 非推奨:
jquery/
├── src/
│   └── pages/
│       └── index.html
└── documentation/
    └── markdown/
        └── *.md
```

### 7.2 目的別ディレクトリ

| ディレクトリ | 目的 | 内容物 |
|------------|------|--------|
| `/` (ルート) | アプリケーションコード | HTML, package.json |
| `docs/` | ドキュメント | Markdown ファイル |
| `node_modules/` | 依存関係 | npm パッケージ |

### 7.3 将来的な拡張

プロジェクトが成長した場合の推奨構造:

```
jquery/
├── src/                    # ソースコード
│   ├── pages/             # HTML ページ
│   ├── scripts/           # JavaScript
│   └── styles/            # CSS
├── tests/                 # テストコード
│   ├── unit/              # ユニットテスト
│   └── integration/       # 統合テスト
├── docs/                  # ドキュメント
├── .github/               # GitHub 設定
│   └── workflows/         # GitHub Actions
└── package.json
```

## 8. 依存関係マップ

### 8.1 ファイル間の依存関係

```
package.json
    │
    ├─► npm install
    │       │
    │       ▼
    │   node_modules/jquery/
    │
    ▼
index.html
    │
    └─► src="node_modules/jquery/dist/jquery.js"
```

### 8.2 ドキュメント間の関連性

```
CLAUDE.md
    │
    ├─► プロジェクト概要を提供
    │
    ▼
docs/technical-specifications.md
    │
    ├─► 技術詳細を参照
    │
    ▼
docs/architecture.md
    │
    ├─► アーキテクチャ評価を提供
    │
    ▼
docs/repository-structure.md（本文書）
```

## 9. アクセス権限とセキュリティ

### 9.1 ファイルアクセス権限

| ファイル | 推奨権限 | 理由 |
|---------|---------|------|
| `.gitignore` | 644 (rw-r--r--) | 読み取り専用で十分 |
| `index.html` | 644 (rw-r--r--) | 実行不要 |
| `package.json` | 644 (rw-r--r--) | 設定ファイル |
| `docs/*.md` | 644 (rw-r--r--) | ドキュメント |

### 9.2 セキュリティ考慮事項

**機密情報の管理**:
- ✅ このプロジェクトには機密情報なし
- ✅ API キーや認証情報を含まない
- ⚠️ 将来的に機密情報を追加する場合は `.env` ファイルを使用し、`.gitignore` に追加

**公開範囲**:
- プロジェクトは教育・テスト目的のため、パブリックリポジトリとして公開可能
- ただし、脆弱性を含むことを README に明記すること

## 10. メンテナンス手順

### 10.1 新規ファイル追加時

1. ✅ 適切なディレクトリを選択（ルート or docs）
2. ✅ 命名規則に従う
3. ✅ 必要に応じて `.gitignore` を更新
4. ✅ 本ドキュメント（repository-structure.md）を更新

### 10.2 ファイル削除時

1. ✅ 依存関係を確認（他のファイルが参照していないか）
2. ✅ Git から削除: `git rm <ファイル名>`
3. ✅ 本ドキュメントを更新

### 10.3 ディレクトリ構造変更時

1. ✅ 影響範囲を分析
2. ✅ 依存関係を更新（`index.html` のパスなど）
3. ✅ ドキュメントを全面的に更新
4. ✅ チーム内で変更を共有

## 11. トラブルシューティング

### 11.1 よくある問題

**問題**: `node_modules/` が見つからない
```
解決策: npm install を実行
$ npm install
```

**問題**: index.html で jQuery が読み込めない
```
原因: node_modules/ が存在しない
解決策: npm install を実行してから index.html を開く
```

**問題**: Git リポジトリが肥大化
```
原因: node_modules/ がコミットされている
解決策:
1. .gitignore に node_modules/ を追加
2. Git キャッシュから削除
   $ git rm -r --cached node_modules/
   $ git commit -m "Remove node_modules from repository"
```

## 12. チェックリスト

### 12.1 新規参加者向けセットアップチェックリスト

- [ ] リポジトリをクローン: `git clone <URL>`
- [ ] 依存関係をインストール: `npm install`
- [ ] ドキュメントを読む:
  - [ ] CLAUDE.md
  - [ ] docs/technical-specifications.md
  - [ ] docs/architecture.md
  - [ ] docs/repository-structure.md（本文書）
- [ ] index.html をブラウザで開いて動作確認

### 12.2 変更前チェックリスト

- [ ] 変更の目的を明確化
- [ ] 影響範囲を確認
- [ ] 依存関係を確認
- [ ] ドキュメント更新の必要性を判断
- [ ] テスト計画を立てる

## 13. 参照資料

### 13.1 関連ドキュメント

- [技術仕様書](./technical-specifications.md)
- [アーキテクチャ実現性評価](./architecture.md)
- [CLAUDE.md](../CLAUDE.md)

### 13.2 外部リソース

- [npm ドキュメント](https://docs.npmjs.com/)
- [Git ドキュメント](https://git-scm.com/doc)
- [Markdown ガイド](https://www.markdownguide.org/)

## 14. 変更履歴

| バージョン | 日付 | 変更者 | 変更内容 |
|-----------|------|--------|---------|
| 1.0 | 2026-01-18 | - | 初版作成 |

---

**次回更新**: ファイル構造に変更があったとき

**管理者**: プロジェクトメンテナー
