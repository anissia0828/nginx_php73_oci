### 動作環境

Docker Desktop for Windows 2.1.0.5で確認しています
docker 19.03.5
docker-compose 1.24.1

### Oracle instant clientの入手

 [Instant Client for Linux x86-64](https://www.oracle.com/technetwork/jp/topics/linuxx86-64soft-085244-ja.html)からinstantclient-basicとinstantclient-sdkを**zip形式で**ダウンロードして
php7.3のDockerFileと同じディレクトリに置いてください

バージョンは12.2と12.1で確認しました

（例）

./php7.3/instantclient-sdk-linux.x64-12.2.0.1.0.zip

./php7.3/instantclient-basic-linux.x64-12.2.0.1.0.zip

### php7.3/DockerFileの編集

入手したファイル名に合わせて13~25行目を変更してください

### php7.3/tnsnames.oraの編集

ご自分の環境に合わせて記述してください

### 複数プロジェクトで使用する場合

nginx/sitesのproject1.confをコピーして使用してください

（例）*project_hoge.conf*

ファイル内のproject1、project1.local を自分のプロジェクトに合わせて変更してください

もちろんDocument rootも！

##### /etc/hostsを変更する

  Windows10の場合はC:\Windows\System32\drivers\etc\hosts

  管理者モードでメモ帳を起動して設定したホスト名を追記する

```
127.0.0.1	project1.local
::1 project1.local
127.0.0.1	project2.local
::1 project2.local
```



### 起動する
```
docker-compose up -d
```
### おまけ．Laravelのインストール

(1) php-fpmに入る
```
docker-compose exec php-fpm bash
```
(2) composerでインストールする
  プロジェクト名：project1
  バージョン：5.8
    の場合
```
composer create-project --prefer-dist laravel/laravel project1 "5.8.*"
```
  最新版の場合
```
composer create-project --prefer-dist laravel/laravel project1
```
(3) 設定したホスト名でアクセスできれば成功！
  http://project1.local/

(4) yajra/laravel-oci8をインストールする

https://github.com/yajra/laravel-oci8

ドキュメント通りにLaravelのバージョンにあわせてcomposerでインストールする



Laravelのディレクトリに移動して

```
composer require yajra/laravel-oci8:"5.8.*"  <=Laravel5.8

composer require yajra/laravel-oci8:"^6.0"	 <=Laravel6~
```

config/app.phpのproviders内に記載する
```
'providers' => [
    // ...
    Yajra\Oci8\Oci8ServiceProvider::class,
],
```

config/oracle.phpの作成

```
php artisan vendor:publish --tag=oracle
```


.envの設定
```
DB_CONNECTION=oracle
DB_HOST=xxx.xxx.xxx.xxx
DB_PORT=1521
DB_DATABASE=oracleのSID
DB_USERNAME=xxx
DB_PASSWORD=xxx
```

### 蛇足．よく使うコマンド

dockerの停止
```
docker-compose stop
```
dockerの起動
```
docker-compose up -d
または
docker-compose start
```
コンテナとイメージを破棄
```
docker-compose down --rmi all
```