# HackNow
2025年　ハッカソン夏の陣（チーム開発 / 期間：2か月 / メンバー：3人）

Djangoベースでタスク管理アプリを**AWS上にデプロイ**。
**Web3層**構成で本番運用を想定したインフラを設計・構築しました。
ローカル環境では Docker compose により同様の構成を再現可能です。

---

## 🚀使用技術
- **フロントエンド**：HTML, CSS, JavaScript
- **バックエンド**：Python（Django）, MySQL
- **インフラ**：AWS, Docker
- **Webサーバー**：Nginx＋Gunicorn
- **開発管理**：GitHub

---
## 👤自分の担当
**環境構築**
- Dockerfile / docker-compose.ymlを作成
- `.env`を利用して環境変数を管理

**AWS環境構築**
- VPC, サブネット, セキュリティグループの設計
- EC2（Amazon Linux）上にアプリをデプロイ
- RDS（MySQL）の構築と接続設定

**アプリケーションサーバー**
- GunicornでDjangoアプリをデプロイ
- Nginxをリバースプロキシとして設定

---
## 🗂️ 構成概要
### AWS本番環境
- EC2 (アプリケーションサーバー)  
  - Nginx + Gunicorn + Flask
- RDS (MySQL)
- VPC / サブネット / セキュリティグループ
- CloudWatch (ログ・監視)

### ローカル開発環境
- Docker Compose により同様の構成を再現可能  
  - `web` (Django + Gunicorn)  
  - `db` (MySQL)  
  - `nginx` (リバースプロキシ)

---
## 📂ディレクトリ構造（開発前段階）
```
HackNow/  
|--- app/
|    |--- apps/
|    |--- config
|    |    |--- asgi.py
|    |    |--- settings.py
|    |    |--- urls.py
|    |    |--- wsgi.py
|    |--- manage.py
|    |--- templates/
|    |--- static/
|    |    |--- css/
|    |    |--- js/
|    |    |--- images/
|    |--- sql/
|--- docker/  
|    |--- Dockerfile
|--- .env  
|--- .env.sample #環境変数のテンプレート  
|--- docker-compose.yml  
|--- requirements.txt  
|___ README.md
```
---
## ▶️ 実行方法（ローカル）

```bash
# 環境変数ファイルの作成
cp .env.sample .env

# 🔑SECRET_KEY生成方法
python -c "import secrets; print(secrets.token_urlsafe(50))"

# .envの中身は以下のように設定してください(サンプル):
# ======= ✅ チームで共通にする設定 =======
MYSQL_DATABASE=django_db             　　　# 開発環境で使うデータベース名（チームで共通・固定）
MYSQL_USER=dev_user                  　　　# 開発用のデータベースユーザ名（チーム共通）
DJANGO_TIME_ZONE=Asia/Tokyo          　　　# タイムゾーン設定(全員共通)
DJANGO_LANGUAGE_CODE=ja              　　　# 言語コード設定(全員共通)
DJANGO_ALLOWED_HOSTS=localhost,127.0.0.1  # 許可するホスト。必要に応じて各自で追加可能

# ======= 🔧 各自で変更する設定 =======
MYSQL_PASSWORD=your_own_password     # 各自がローカル環境で設定するDBユーザーパスワード(環境開発で各自設定、非公開)
MYSQL_ROOT_PASSWORD=your_root_pw     # MYSQLのrootパスワード(開発環境で各自設定、非公開)
DJANGO_SECRET_KEY=your_secret_key    # Djangoのセキュリティキー(必ず各自で生成すること)
DJANGO_DEBUG=True                    # デバッグモード設定。開発中はTrue、本番や検証環境はFalse推奨

# 開発環境立ち上げ
docker compose up --build

# データベースマイグレーション & 初期データ投入（過去ハッカソン情報）：
make run

# アクセス方法
Webアプリ： http://localhost:8000
```

---

## 📸起動イメージ
<img width="1723" height="901" alt="image" src="https://github.com/user-attachments/assets/779706b7-eea3-40e7-b11f-9ef3044a60d9" />

---
## ☁️ AWS デプロイ手順（概要）
1. VPC / サブネット / セキュリティグループを作成
2. RDS(MySQL)を構築し、初期データを投入
3. EC2(Amazon Linux 2023)にDockerを導入
4. DjangoアプリをGunicornで起動
5. Nginxをリバースプロキシとして設定（80->8000転送)
6. CloudWatchでログ・監視を設定

---
## インフラ構成図
<img width="1099" height="744" alt="image" src="https://github.com/user-attachments/assets/887488c0-e335-4a22-bc1a-69a0add022b6" />


