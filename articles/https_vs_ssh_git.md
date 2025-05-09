## はじめに

GitHub を VSCode 上で利用していた際、GitHub のウェブサイトからコピーした HTTPS のリポジトリ URL を使ってリモートを登録したところ、`repository not found` のエラーが出て上手くいきませんでした。URL 自体に間違いはなかったため、原因を調べる中で、**HTTPS と SSH の違い**が影響していることが分かりました。

本記事は、同じようなエラーでつまずく Git 初心者の方に向けて、HTTPS と SSH の違い、選び方、設定方法をわかりやすくまとめたものです。

## 前提知識の補足

* **Git（ギット）**：バージョン管理システムの一種。ソースコードの履歴を管理できる。
* **GitHub**：Git を使ってコードをホスティング（公開・共有）できるウェブサービス。
* **VSCode**：Microsoft 製のコードエディタ。Git 操作も統合されており、GUI で管理できる。
* **リモートリポジトリ**：クラウド上の Git リポジトリ。GitHub にあるコードのこと。
* **クローン（clone）**：リモートリポジトリを自分のPCにコピーする操作。
* **ターミナル／CLI**：コマンドを入力して操作する画面。VSCode 内にも搭載されている。
* **Personal Access Token（PAT）**：パスワードの代わりに使う、GitHub 専用の認証トークン。
* **SSH 鍵**：認証のために使う鍵ペア（公開鍵と秘密鍵）で、安全な通信を行うための仕組み。

## Git リモートの接続方法：HTTPS と SSH の違い

### 1. HTTPS とは？

HTTPS は、認証と通信を暗号化したプロトコルで、以下のように使います：

```bash
git clone https://github.com/ユーザー名/リポジトリ名.git
```

#### 特徴

* GitHub サイト上からすぐにコピーして使える
* 通信は暗号化されていて安全
* 認証にユーザー名とパスワード、または Personal Access Token（PAT）が必要

#### 注意点

* プッシュやフェッチのたびに認証が必要な場合がある
* GitHub がパスワード認証を廃止したため、PAT を使う必要がある

### 2. SSH とは？

SSH（Secure Shell）は、公開鍵と秘密鍵の仕組みに基づく認証方式で、以下のように使います：

```bash
git clone git@github.com:ユーザー名/リポジトリ名.git
```

#### 特徴

* 最初に SSH 鍵を設定すれば、それ以降の認証が不要
* 認証が高速でスムーズ
* CI/CD や自動化にも向いている

#### 注意点

* 最初に `ssh-keygen` で鍵を作成し、GitHub に登録する必要がある

## 手順：どちらを使うかで変わる設定

### HTTPS の使い方（基本）

1. GitHub で対象リポジトリのページを開く
2. 緑の「Code」ボタンから HTTPS の URL をコピー
3. VSCode ターミナルや CLI で次を実行：

```bash
git clone https://github.com/lucentrains/zenn_library.git
```

認証を求められたら、GitHub の PAT（Personal Access Token）を入力

### SSH の使い方（推奨）

1. SSH 鍵を生成：

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

2. 公開鍵 (`~/.ssh/id_ed25519.pub`) を GitHub に登録
3. SSH URL を使って clone：

```bash
git clone git@github.com:lucentrains/zenn_library.git
```

VSCode から Git 操作をする場合も、これで毎回認証不要になります

## ベンチマーク比較（HTTPS vs SSH）

簡易的な速度比較では、SSH のほうが高速なことが多いです。

```bash
# HTTPS
$ time git clone https://github.com/example/repo.git

# SSH
$ time git clone git@github.com:example/repo.git
```

特に毎日 Git 操作をするなら、SSH が快適です。

## まとめ：どちらを使うべき？

| 項目      | HTTPS           | SSH            |
| ------- | --------------- | -------------- |
| 設定の簡単さ  | ◎（すぐ使える）        | △（初期設定が必要）     |
| 認証の手軽さ  | △（毎回トークン入力の可能性） | ◎（一度設定すれば認証不要） |
| 自動化との相性 | △               | ◎              |
| セキュリティ  | ◎（暗号化）          | ◎（公開鍵方式）       |

## 結論

* 一時的に使うなら HTTPS でも十分ですが、
* 継続的に使うなら **SSH の利用を強くおすすめ**します。

初学者でも、最初に SSH をしっかり設定しておくことで、あとから悩むことが減ります。GitHub に慣れてきたら、早めに SSH での接続方法に切り替えてみましょう！
