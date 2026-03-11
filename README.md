# みんなの経理

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![GitHub Pages](https://img.shields.io/badge/使ってみる-GitHub%20Pages-brightgreen)](https://choreia.github.io/minnano-keiri/)

**個人から中小法人まで使える完全無料の会計ソフト**

サーバー不要・インストール不要。ブラウザだけで税金計算から請求書作成まで、経理業務をまるごとカバーします。データはすべてご自身のGoogle Sheetsに保存されるため、プライバシーも安心です。

**[使ってみる](https://choreia.github.io/minnano-keiri/)**

## 特徴

- **税金計算** — 所得税・復興特別所得税・住民税・個人事業税・法人税をワンクリックで算出
- **仕訳帳（Google Sheets連携）** — Googleアカウントでログインし、仕訳データをSpreadsheetに自動保存・読み込み
- **損益計算書** — 仕訳データから損益計算書を自動生成
- **請求書作成・PDF出力** — ブラウザ上で請求書を作成し、PDFとしてダウンロード
- **年度対応** — 令和6年（2024年）・令和7年（2025年）の税制に対応
- **完全無料** — オープンソース（MIT）、広告なし、利用制限なし

## アーキテクチャ

```
┌─────────────┐     ┌──────────────┐     ┌───────────────┐
│  ブラウザ    │────▶│  WebAssembly  │────▶│  kaikei-rs    │
│  (HTML/JS)  │◀────│  (Rust→Wasm) │◀────│  (税金計算)   │
└─────┬───────┘     └──────────────┘     └───────────────┘
      │
      ▼
┌─────────────┐
│Google Sheets │
│ (データ保存) │
└─────────────┘
```

- **Rust + WebAssembly** — 税金計算ロジックはRustで実装し、wasm-packでWebAssemblyにコンパイル
- **サーバーレス** — GitHub Pagesで静的ホスティング。バックエンドサーバー不要
- **データ保存** — ユーザーのGoogleアカウントのGoogle Sheetsにデータを保存。サーバーにデータを送信しません
- **税金計算エンジン** — [kaikei-rs](https://github.com/Ryujiyasu/kaikei-rs) ライブラリを使用

## 必要な環境

- [Rust](https://rustup.rs/)（最新stable）
- [wasm-pack](https://rustwasm.github.io/wasm-pack/installer/)
- ローカルHTTPサーバー（`python3 -m http.server` など）

## 開発環境のセットアップ

```bash
# リポジトリのクローン
git clone https://github.com/Choreia/minnano-keiri.git
cd minnano-keiri

# Rustとwasm-packのインストール（未インストールの場合）
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
cargo install wasm-pack

# WASMビルド
wasm-pack build --target web

# pkgディレクトリをwebにコピー
cp -r pkg web/pkg

# ローカルサーバーで起動
cd web
python3 -m http.server 8080
```

ブラウザで http://localhost:8080 にアクセスしてください。

## プロジェクト構成

```
minnano-keiri/
├── src/
│   └── lib.rs          # WASMバインディング（kaikei-rsのラッパー）
├── web/
│   └── index.html      # フロントエンド（HTML/CSS/JS）
├── pkg/                # wasm-packビルド出力
├── .github/
│   └── workflows/      # GitHub Pages自動デプロイ
├── Cargo.toml
└── LICENSE
```

## 使用ライブラリ

| ライブラリ | 用途 |
|---|---|
| [kaikei-rs](https://github.com/Ryujiyasu/kaikei-rs) | 日本の税金計算エンジン |
| [wasm-bindgen](https://github.com/rustwasm/wasm-bindgen) | Rust↔JavaScript連携 |
| [serde_json](https://github.com/serde-rs/json) | JSON変換 |

## ロードマップ

日々の経理から確定申告・法人税申告まで、一気通貫で完結する会計ソフトを目指しています。

### Phase 0: 個人（家計簿）

| 機能 | 状況 |
|---|---|
| 収支記録・カテゴリ管理 | 予定 |
| 月次・年次レポート | 予定 |

### Phase 1: 個人事業主

| 機能 | 状況 |
|---|---|
| 税金シミュレーション（所得税・住民税・事業税） | 完了 |
| 仕訳帳（Google Sheets連携） | 完了 |
| 損益計算書 | 完了 |
| 請求書作成・PDF出力 | 完了 |
| 貸借対照表 | 開発中 |
| 家事按分 | 開発中 |
| 源泉徴収管理 | 開発中 |
| 確定申告書の数値集計 | 予定 |

### Phase 2: 一人法人

| 機能 | 状況 |
|---|---|
| 法人決算書（P/L・B/S） | 予定 |
| 役員報酬・社会保険の最適化シミュレーション | 予定 |
| 法人税申告書の数値集計 | 予定 |

### Phase 3: 中小法人

| 機能 | 状況 |
|---|---|
| 複数従業員の給与計算 | 予定 |
| 固定資産管理・減価償却 | 予定 |
| 証憑管理（電子帳簿保存法対応） | 予定 |

## コントリビュート

コントリビュート大歓迎です！ 詳しくは [CONTRIBUTING.md](CONTRIBUTING.md) をご覧ください。

## ライセンス

[MIT](LICENSE)
