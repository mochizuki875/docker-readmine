# Docker Redmine

## 概要
`sameersbn/docker-redmine`をベースとしてdocker環境にRedmine、Nginx R-Proxy、監視コンポーネントを構築する。
https://github.com/sameersbn/docker-redmine

## クイックスタート

### 監視用NW作成

~~~
docker network create prometheus-network
~~~

### Redmineコンテナビルド

~~~
cd redmine/docker-redmine
docker build -t sameersbn/redmine:4.1.1-7 .
~~~

### コンテナデプロイ

~~~
cd redmine
docker-compose up -d
cd nginx
docker-compose up -d
cd monitor
docker-compose up -d
~~~
